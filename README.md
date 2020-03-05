# 	DCDB编译及部署

DCDB的核心引擎是百度开源baikaldb，编译、部署围绕baikaldb展开。

## Download

下载BaikalDB源码，下载地址https://github.com/baidu/BaikalDB.git

``` shell
git clone https://github.com/baidu/BaikalDB.git
```

## Compile（centos）

### 安装工具

1. 安装bazel（centos，v0.18.1及更早）

```shell
wget https://github.com/bazelbuild/bazel/releases/download/0.18.1/bazel-0.18.1-installer-linux-x86_64.sh
sh bazel-0.18.1-installer-linux-x86_64.sh
```

2. 安装依赖

```shell
sudo yum install flex bison patch openssl-devel
sudo yum install gcc-c++ (v4.8.2 or later)
```

### 编译

```shell
bazel build //:all
```

> 注意： 
>
> 1. WORKSPACE中的以下配置：
>
> ```shell
> git_repository(
>  name = "com_github_brpc_brpc",
>  remote= "https://github.com/apache/incubator-brpc.git",
>  tag = "0.9.7-rc01",
> )
> ```
> 2. 确认已经安装过automake，如果没有，通过下面的指令安装：
>
> ```shell
> yum install libtool -y
> yum install automake.noarch
> ```
## 部署

### 启动顺序

meta->store->db，也就说首先启动所有的meta，然后启动所有的store，最后启动所有db。

### 准备脚本及目录

为了方便部署，我们提前把需要运行的指令做成脚本，并把组件的目录结构组织在一起存放的BaikalDB.tar.gz文件中，直接解压缩到将要部署的目录。下面的假设要部署到/home/work/目录下进行讲解。

### Meta部署

1. 将编译生成的baikalMeta拷贝到/home/work/BaikalDB/meta/bin路径下，替换原有的baikalMeta文件。

2. 修改/home/work/BaikalDB/meta/conf/gflags.conf配置文件：

   ![image-20200304182906088](/Users/liuyong/Desktop/123/3.png)

> 根据集群的具体场景可以修改的配置参数
>
> meta_replica_numer：meta集群副本数
>
> meta_port：meta的端口号（建议默认）
>
> meta_server_bns：meta集群中每个meta的ip:meta_port

3. 启动meta	
- 首相要启动所有meta节点中的baikalMeta

   ```shell
   cd /home/work/BaikalDB/meta/
   sh restart.sh --init
   ```
- 修改/home/work/BaikalDB/leader.sh中的地址为meta集群中的任一meta的地址(图中假设meta集群找那个有一个meta部署在10.152.22.38机器上，使用默认端口8010)。
  
   ![企业微信截图_3494c0ef-1595-427d-acd6-d820fb317acb](/Users/liuyong/Desktop/123/4.png)
   
- 启动meta集群中的一个节点的初始化脚本即可（此节点中的leader.sh必须按照上述修改过）

   ```shell
   cd /home/work/BaikalDB/
   sh init_meta.sh
   ```

### Store部署

1. 将编译生成的baikalStore拷贝到/home/work/BaikalDB/store/bin路径下，替换原有的baikalStore文件。
2. 修改/home/work/BaikalDB/store/conf/gflags.conf配置文件：

![image-20200305152309709](/Users/liuyong/Library/Application Support/typora-user-images/image-20200305152309709.png)

> 根据集群的具体场景可以修改的配置参数：
>
> meta_server_bns：meta集群的ip:meta_port（根据meta的配置）
>
> store_port：store节点的端口号（建议默认）
>
> resource_tag：必须与建表语句中的resource_tag一致

3. 启动所有store节点的baikalStore

```shell
cd /home/work/BaikalDB/store/
sh restart.sh --init
```

### db部署

1. 将编译生成的baikaldb拷贝到/home/work/BaikalDB/db/bin路径下，替换原有的baikaldb文件。
2. 修改/home/work/BaikalDB/db/conf/gflags.conf配置文件：

![image-20200304190503667](/Users/liuyong/Desktop/123/6.png)

> 根据集群的具体场景可以修改的配置参数：
>
> meta_server_bns：meta集群的ip:meta_port（根据meta的配置）
>
> baikal_port：访问DCDB的端口号(建议默认)。

3. 启动db

- 启动db集群中的一个节点上的初始化脚本

```shell
cd /home/work/Baikal/
sh init_db.sh
```

- 启动所有db节点的baikaldb

```shell
cd /home/work/Baikal/store/
sh restart.sh --init
```


