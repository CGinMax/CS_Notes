# Docker
========================

## 基本命令

`docker pull image-name[:version]` : 拉取镜像仓库。版本默认为latest的最新版本。

`docker images` : 查看docker镜像。

`docker run image/container`:
1. 客户端输入 `docker run` 命令。
2. Docker daemon 在本地查找不到镜像。
3. 从 **Docker Hub** 下载镜像。
4. 保存镜像到本地。
5. Docker daemon 启动容器。

`run option` :
+ **-i** :后台运行，不自动退出
+ **-t** :创建shell终端，并进入内部shell操作

`docker start|restart|stop|rm <container-name/container-id>` :启动|重启|停止|删除容器

`docker image rm <image-name>` : 删除镜像实例，也可以用 `docker rmi <image name>`

`docker build -t <repo-name/image-name:TAG> <path>` : 从Dockerfile中构建docker仓库。

`docker commit -a "author" -m "commit" <container-name/container-id> <hub-user>/<repo-name>[:<TAG>]` : 类似git的commit，提交容器。

`docker inspect <container-name/id>` : 查看容器详细配置信息。

`docker logs <container-name/id>` : 查看容器日志。

`docker port <container-name/id>` : 查看容器的端口映射。

## Dockerfile常用指令
Dockerfile由指令组合而成， 指令格式 `INSTRUCTION argument`
1. **FORM**

    `FORM image-name` :从基础镜像继承下来。
2. **MAINTAINER**

    `MAINTAINER email` :维护人信息。
    
3. **COPY**

    `COPY src dest` :复制host的文件或文件夹到容器。
    
4. **ADD**

    `ADD src/url dest` :跟 `COPY` 类似但可从网络中将文件下载后拷到 `dest` ，并将文件权限设为600
    
5. **WORKDIR**

    `WORKDIR path` :设置容器的工作路径。
    
6. **RUN**

    `RUN cmd arg...` :运行命令，一般linux命令。
    
7. **EXPOSE**

    `EXPOSE port` :暴露端口。
    
8. **VOLUME**

    `VOLUME path1 path2` :为容器添加匿名卷。
    
9. **ENV**

    `ENV (key value/key=value)` :设置环境变量。
    
10. **ENTRYPOINT**

    `ENTRYPOINT ["executable", "param1", "param2"]` :让你的容器表现得像一个可执行程序一样。
    
11. **USER**

    `USER <username>[:<group>]`