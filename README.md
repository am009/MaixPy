
# [Maixpy](https://github.com/sipeed/MaixPy) port for Widora AIRV r3

learning...

## feature:
- 1.14 inch screen supported!

## TODO:

- try using git tag and github release
- 放上blog文章链接

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