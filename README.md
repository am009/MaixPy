
# [Maixpy](https://github.com/sipeed/MaixPy) port for Widora AIRV r3

learning...

## feature:
- 1.14 inch screen supported!
- microphone useable
- ov2640 camera supported!

## TODO:
I only purchased single board with on board 1.14 inch screen, with a single ov2640 camera, so support for any other hardware is not planned.

- learn audio recognition
- frequently rebase and follow up maixpy code.

## Build

1. set up environment. [build doc](https://github.com/sipeed/MaixPy/blob/master/build.md)

basically:

git clone and the widora-airv-r3 branch of this repo and:
```
git submodule update --recursive --init
sudo apt update
sudo apt install python3 python3-pip build-essential cmake
sudo pip3 install -r requirements.txt
wget http://dl.cdn.sipeed.com/kendryte-toolchain-ubuntu-amd64-8.2.0-20190409.tar.xz
sudo tar -Jxvf kendryte-toolchain-ubuntu-amd64-8.2.0-20190409.tar.xz -C /opt
```

2. change directory to projects/maixpy_airvr3 and execute `python3 ./project.py build`

## camera
camera is fine now. support OV2640

## microphone
This is the example with microphone config. now default configuration is changed for airvr3, so some parameter in rx.channel_config can be emitted.
```
from Maix import GPIO, I2S, FFT
import image
import lcd
import math
import time
import gc
from board import board_info
from fpioa_manager import fm
import audio

fm.register(8,  fm.fpioa.GPIO0)
wifi_en = GPIO(GPIO.GPIO0, GPIO.OUT)
wifi_en.value(0)

sample_rate = 22050
sample_points = 4096

fm.register(32,fm.fpioa.I2S0_IN_D0)
fm.register(31,fm.fpioa.I2S0_SCLK)
fm.register(30,fm.fpioa.I2S0_WS)

rx = I2S(I2S.DEVICE_0)
rx.channel_config(rx.CHANNEL_0, rx.RECEIVER, resolution=rx.RESOLUTION_24_BIT, cycles=rx.SCLK_CYCLES_32, align_mode=I2S.LEFT_JUSTIFYING_MODE)
rx.set_sample_rate(sample_rate)
print(rx)

player = audio.Audio(path="/sd/record_1.wav", is_create=True, samplerate=22050)

queue = []

for i in range(400):
    tmp = rx.record(sample_points)
    if len(queue) > 0:
        ret = player.record(queue[0])
        queue.pop(0)
    rx.wait_record()
    queue.append(tmp)
    print(time.ticks())

player.finish()
```

## 维护日志
目标: 经常rebase代码, 和Maixpy保持一致, 并且把自己的改动commits放在rebase之后.
每次先更新一下自己本地的sipeed的master分支, 然后使用rebase -i master直接把所有commit都rebase过去
然后再加入新的代码

遇到git submodule出问题的情况, 就执行git submodule update --remote --recursive 会把每个子模块都pull, 然后执行git submodule update --recursive --init会切换好对应的commit
遇到修改了project/maixpy_k210的情况, 先stash, 再使用rsync -a /maixpy_k210/ /maixpy_airvr3/, 再看git里面的changes, 把改了的地方commit.
