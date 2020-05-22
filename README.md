# pull

sudo docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/momodocker:raspbian-buster_armv7

sudo docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/momodocker:ubuntu-16.04_x86_64_ros	

sudo docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/momodocker:ubuntu-18.04_armv8_jetson_nano

sudo docker pull registry.cn-shenzhen.aliyuncs.com/neoneone/momodocker:ubuntu-18.04_x86_64


# use

DOCKER_IMAGE="registry.cn-shenzhen.aliyuncs.com/neoneone/momodocker:$PACKAGE"

../script/docker_run.sh `pwd` `pwd`/.. $DOCKER_MOUNT_TYPE $PACKAGE $DOCKER_IMAGE $MOMO_COMMIT
