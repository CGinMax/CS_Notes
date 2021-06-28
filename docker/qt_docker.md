# 问题


sudo docker run -itd --name ubuntu -e DISPLAY=$DISPLAY -v ~/ubuntu_docker:/opt/data -v /tmp/.X11-unix:/tmp/.X11-unix --workdir=/opt/data ubuntu:16.04 /bin/bash

在docker上运行qt出现
```
no protocol specified 
qt.qpa.xcb could not connect to display
```
在主机运行 `xhost +local:docker`

如果出现 `non-network local connections being added to access control list`即可运行
