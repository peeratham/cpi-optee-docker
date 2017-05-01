Dockerfile for CPI-OP-TEE test
=====================

If you don't have Docker installed, install Docker by following this url:
https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description 

```bash
git clone https://github.com/jbech-linaro/docker_optee/
cd docker_optee
sudo docker build -t cpi-optee-test .

```
It will take some time.  When it's done, create an instance (container) of the docker image by :

```bash
test_project_path=<path/to/cpi_test>
sudo docker run --name cpi-test -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $test_project_path:/home/optee/cpi_test cpi-optee-test
```
The container will be in an interactive mode. Ctrl+D to exit

Later use of the container:
```bash
#If not already running, you need to start the container.
$ docker start cpi-test

#Send command to compile project under cpi_test directory
docker exec -it cpi-test cd /home/optee/qemu_optee/cpi_test && make clean && make

#Re-attach to the running container for interactive mode
$ docker attach cpi-test
```
