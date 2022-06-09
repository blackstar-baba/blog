#### description



```shell
docker run -it \
-p 9030:9030 \
-p 30002:30002 \
-p 30003:30003 \
-p 9060:9060
-p 8040:8040
-p 9050:9050
-v /Users/blackstar/.m2:/root/.m2 \
-v /Users/blackstar/github/fork/starrocks:/root/starrocks \
--ip 172.17.0.2 \
--name sr-build \
-d starrocks/dev-env:main

docker exec -it sr-build /bin/bash

cd /root/starrocks

./build.sh



```



#### link

- https://www.jetbrains.com/help/clion/remote-projects-support.html#remote-toolchain