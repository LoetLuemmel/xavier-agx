# xavier-agx
Working with the Nvidia Xavier AGX

# Starting the docking container:

```bash
git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
cd jetson-inference
docker/run.sh
# (press Ctrl+D to exit the container)
```
# Docker starten und ein gemeinsames Verzeichnis mounten:

```bash
docker/run.sh --volume /home/pit/Pictures:/jetson-inference/build/aarch64/bin/images/test
docker/run.sh --volume /home/pit/Pictures:/jetson-inference/build/aarch64/bin/images/test --volume /home/pit/jetson-inference/build/aarch64/bin/networks:/jetson-inference/build/aarch64/bin/networks


```

# Running the Live Camera Recognition Demo

The [`imagenet.cpp`](../examples/imagenet/imagenet.cpp) / [`imagenet.py`](../python/examples/imagenet.py) samples that we used previously can also be used for realtime camera streaming.  The types of supported cameras include:

- MIPI CSI cameras (`csi://0`)
- V4L2 cameras (`/dev/video0`)
- RTP/RTSP streams (`rtsp://username:password@ip:port`)

For more information about video streams and protocols, please see the [Camera Streaming and Multimedia](https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-streaming.md) page.

Below are some typical scenarios for launching the program on a camera feed (run `--help` for more options):

#### C++

``` bash
$ ./imagenet csi://0                    # MIPI CSI camera
$ ./imagenet /dev/video0                # V4L2 camera
$ ./imagenet /dev/video0 output.mp4     # save to video file
```

#### Python

``` bash
$ ./imagenet.py csi://0                 # MIPI CSI camera
$ ./imagenet.py /dev/video0             # V4L2 camera
$ ./imagenet.py /dev/video0 output.mp4  # save to video file

Dustin from Nvidia has his prebuild docker containers on [DockerHub](https://hub.docker.com/r/dustynv/jetson-inference/tags): https://hub.docker.com/r/dustynv/jetson-inference/tags
Alternatively, you can [Build the Project ](building-repo-2.md) from source.   

Dustin has more details on his [Gihub](https://github.com/dusty-nv/jetson-inference/blob/master/docs/aux-docker.md).

# Modelle wiederherstellen

```$ cd jetson-inference/data/networks/
$ wget https://raw.githubusercontent.com/BVLC/caffe/master/models/bvlc_googlenet/deploy.prototxt -O googlenet.prototxt
$ wget http://dl.caffe.berkeleyvision.org/bvlc_googlenet.caffemodel -O bvlc_googlenet.caffemodel
Then check “ls -ll” again - your bvlc_googlenet.caffemodel should be 53533754 bytes and googlenet.prototxt should be 35861 bytes. If sizes match, try re-running imagenet-console again.

To re-download all the models, clear your build directory and re-run cmake:

$ cd jetson-inference
$ rm -r -f build
$ mkdir build
$ cd build
$ cmake ../
$ make
$ sudo make install
```

[Dustin's Tipp komplett](https://forums.developer.nvidia.com/t/can-not-initialize-imagenet/75716)

# Working with the I2C bus

Output of i2c devices on bus #8

```
pit@pit-desktop:~$ i2cdetect -y -r 8
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- 76 --
```

Char value of `bus 8`, `I2C-Address 0x76` in `register 0x0d`

```
pit@pit-desktop:~$ sudo i2cget 8 0x76 0xd0
WARNING! This program can confuse your I2C bus, cause data loss and worse!
I will read from device file /dev/i2c-8, chip address 0x76, data address
0xd0, using read byte data.
Continue? [Y/n] y
0x58
```








