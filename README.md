Setting up environments for testing with OP-TEE
=====================

We use Docker to facilitate testing. Learn more about Docker? https://www.docker.com/what-docker

If you don't have Docker installed, install Docker by following this url:
https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description 

We use Docker image created by Joakim Bech from OP-TEE team.

```bash
git clone https://github.com/jbech-linaro/docker_optee/
cd docker_optee
sudo docker build -t cpi-optee-test .

```
It will take some time.  When it's done, create an instance (container) of the docker image by :

```
test_project_path=<path/to/cpi_test>
sudo docker run --name cpi-test -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $test_project_path:/home/optee/cpi_test optee

# docker run will create a new container and run /bin/bash in an interactive mode:

# apply patches so that optee build include the our test prooject
cd /home/optee/
git clone https://github.com/peeratham/cpi-test-optee-patch.git
patch -p1 ~/qemu-optee/build/common.mk ~/cpi-test-optee-patch/common.mk.patch
patch -p1 ~/qemu-optee/build/qemu.mk ~/cpi-test-optee-patch/qemu.mk.patch

# make test scripts executable
cd /home/optee/cpi-test-optee-patch
chmod +x build.sh test-qemu.sh

# build op-tee
cd /home/optee/qemu-optee/build/
make all

# When build finishes, press Ctrl+D to stop container
```
The container is now setup for running the test and we don't need to be in this interactive mode to run the test.
Docker allows us to send commands to the container to be executed. 

```
# If not already running, start the container first before docker exec ... commands by :
sudo docker start cpi-test

# test compilation
docker exec -it cpi-test /bin/bash -c 'cd /home/optee/cpi-test-optee-patch; ./build.sh'

# test running on qemu
docker exec -it cpi-test /bin/bash -c 'cd /home/optee/cpi-test-optee-patch; ./test-qemu.sh'

# when done stop container
sudo docker stop cpi-test

```
Of course if you want to get back to interactive mode e.g. to debug; re-attach to the running container (start it first) by 
```
sudo docker attach cpi-test
```
