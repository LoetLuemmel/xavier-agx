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
```

# Running the Live Camera Recognition Demo

The [`imagenet.cpp`](../examples/imagenet/imagenet.cpp) / [`imagenet.py`](../python/examples/imagenet.py) samples that we used previously can also be used for realtime camera streaming.  The types of supported cameras include:

- MIPI CSI cameras (`csi://0`)
- V4L2 cameras (`/dev/video0`)
- RTP/RTSP streams (`rtsp://username:password@ip:port`)

For more information about video streams and protocols, please see the [Camera Streaming and Multimedia](aux-streaming.md) page.

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
