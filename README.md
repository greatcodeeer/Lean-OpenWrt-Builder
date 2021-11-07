# Lean-OpenWrt-Builder
基于Docker编译lean/openwrt

a. 获取
```
$ sudo docker pull codeeer/env-lean-openwrt
```
b. 创建实例并进入容器
```
$ docker run -it --name lean-openwrt codeeer/env-lean-openwrt
```
#### 编译操作不能在root用户下进行，实例中已切换至用户admin，密码admin
1. 克隆lede源码
```
$ git clone https://github.com/coolsnowwolf/lede
```
2. 进入lede目录
```
cd lede
```
3. 添加科学上网：
在 feeds.conf.default 文件尾部添加：
```
src-git lienol https://github.com/fw876/helloworld
```
4. 安装/更新源，并进入菜单
```
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make menuconfig
```
5. 在【LuCI -> Application】中添加/删除插件（按Y勾选，N取消勾选）
6. 首次编译需要使用单线程，首次编译会久一些
```
make -j1 V=s
```

c. 从容器复制编译好的固件
```
// 找到运行此实例的CONTAINER ID
$ docker ps
// 从容器中复制固件到本地
$ sudo docker cp <CONTAINER ID>:/home/admin/lede/bin/targets/ .
```
d. 再次编译（用于更新源）
```
// 运行容器并进入lede目录
$ git pull // 更新feeds
$ ./scripts/feeds update -a && ./scripts/feeds install -a // 更新feeds
$ rm -rf ./tmp && rm -rf .config // 清除编译配置和缓存
$ make -j<N> V=s // 多线程编译（N=线程数+1，例如4线程的i5填-j5）
```
