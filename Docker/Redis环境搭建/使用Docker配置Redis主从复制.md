## 使用 Docker 配置 Redis 主从复制

这篇文章主要介绍如何使用 Docker 在本机搭建一个带有主从复制功能的 Redis 环境，内容包括涉及的目录结构、`docker-compose.yml` 的编写，以及结果的验证。

### 目录结构

项目的目录结构如下：

```shell
.
├── README.md                       # 说明文档
├── docker-compose-example.yml      # docker-compose 示例文件，由 docker-compose.yml 复制
├── docker-compose.yml              # docker-compose 启动文件
├── env.example                     # 配置文件示例
├── redis-master                    # redis master 节点目录
│   ├── conf                        # 配置文件目录
│   │   ├── redis.conf              # redis 配置文件
│   │   └── start.sh                # 容器启动后执行脚本
│   ├── data                        # redis 数据集
│   │   └── dump.rdb
│   └── log                         # redis 日志
│       └── redis.log
├── redis-slave-1                   # redis slave 1 节点目录
│   ├── conf
│   │   ├── redis.conf
│   │   └── start.sh
│   ├── data
│   │   └── dump.rdb
│   └── log
│       └── redis.log
└── redis-slave-2                   # redis slave 2 节点目录
    ├── conf
    │   ├── redis.conf
    │   └── start.sh
    ├── data
    │   └── dump.rdb
    └── log
        └── redis.log
```



### 配置 Redis 节点

#### 配置 master 节点

编辑 `redis-master/conf/redis.conf`，修改以下配置：

```shell
# 设置日志文件
logfile "/var/log/redis/redis.log"
```

#### 配置 slave 节点

编辑 `redis-slave-1/conf/redis.conf` 和 `redis-slave-2/conf/redis.conf`，修改以下配置：

```shell
# 设置日志文件
logfile "/var/log/redis/redis.log"
# 配置 master 节点信息
# 格式：replicaof <masterip> <masterport>
# 此处 masterip 所指定的 redis-master 是运行 master 节点的容器名
# Docker容器间可以使用容器名代替实际的IP地址来通信
replicaof redis-master 6379
```



### 配置及启动容器

#### 编写 `docker-compose.yml`

```yaml
version: "3"
services:
  # 主节点容器
  redis-master:
    image: redis:${REDIS_VERSION}
    container_name: redis-master
    ports:
      - ${REDIS_HOST_PORT}:6379
    # 映射目录
    volumes:
      - ${REDIS_CONF_DIR}:/etc/redis
      - ${REDIS_DATA_DIR}:/data
      - ${REDIS_LOG_DIR}:/var/log/redis
    restart: always
    # 指定网络和ip
    networks:
      default:
        ipv4_address: 172.20.0.7
    # 指定时区
    environment:
      TZ: ${TIMEZONE}
    privileged: true
    # 容器启动后执行脚本
    command: ["/bin/bash","/etc/redis/start.sh"]
  # 从节点1的容器
  redis-slave-1:
    image: redis:${REDIS_VERSION}
    container_name: redis-slave-1
    ports:
      - ${REDIS_SLAVE_1_HOST_PORT}:6379
    volumes:
      - ${REDIS_SLAVE_1_CONF_DIR}:/etc/redis
      - ${REDIS_SLAVE_1_DATA_DIR}:/data
      - ${REDIS_SLAVE_1_LOG_DIR}:/var/log/redis
    restart: always
    depends_on:
      - redis-master
    networks:
      default:
        ipv4_address: 172.20.0.8
    environment:
      TZ: ${TIMEZONE}
    privileged: true
    command: ["/bin/bash","/etc/redis/start.sh"]
  # 从节点2的容器
  redis-slave-2:
    image: redis:${REDIS_VERSION}
    container_name: redis-slave-2
    ports:
      - ${REDIS_SLAVE_2_HOST_PORT}:6379
    volumes:
      - ${REDIS_SLAVE_2_CONF_DIR}:/etc/redis
      - ${REDIS_SLAVE_2_DATA_DIR}:/data
      - ${REDIS_SLAVE_2_LOG_DIR}:/var/log/redis
    restart: always
    depends_on:
      - redis-master
    networks:
      default:
        ipv4_address: 172.20.0.9
    environment:
      TZ: ${TIMEZONE}
    privileged: true
    command: ["/bin/bash","/etc/redis/start.sh"]
networks:
  default:
    driver: bridge
    ipam:
      driver: default
      config:
        - subnet: 172.20.0.0/24
```

#### 启动容器

在 `docker-compose.yml` 所在目录执行 `docker-compose up -d` 即可启动上述三个容器，如下图所示：

![image-20200404135646017](https://tva1.sinaimg.cn/large/00831rSTgy1gdhp0cnrgnj31ib0u0wrq.jpg)

容器启动成功后，进入容器，可以使用 `redis-cli` 连接 Redis 服务器。连接成功后，执行 `info replication` 命令检查该 Redis 服务器相关信息。

![image-20200404140307245](https://tva1.sinaimg.cn/large/00831rSTgy1gdhp6wzi7aj318a0nygw3.jpg)

其中 `role:master` 说明该节点为主节点，`connected_slaves:2` 说明当前有 2 个从节点，`slave0` 和 `slave1` 的内容是两个从节点的信息，包括它们的 ip、端口号和状态。



### 测试

光是启动成功还不够，还需要测试一下从节点是否能同步主节点的数据。

首先连接主节点，新增一个 set：

![image-20200404140725172](https://tva1.sinaimg.cn/large/00831rSTgy1gdhpbe8nvej312w09sgo1.jpg)

在主节点成功添加了一条数据，此时连接到 `slave-1`，看一下数据有没有同步过去：

![image-20200404141021187](https://tva1.sinaimg.cn/large/00831rSTgy1gdhpeg5uvmj312w0dmn2a.jpg)

可以发现数据已经成功同步到 `slave-1` 节点，并且该节点是一个只读的节点。接下来看 `slave-2`：

![image-20200404141151188](https://tva1.sinaimg.cn/large/00831rSTgy1gdhpfzhm8yj312w0cqgqq.jpg)

`slave-2` 节点也已经成功从 `master` 节点同步了数据，并且正在作为一个只读节点运行着。



### 项目地址

- [docker-redis](https://github.com/ballooninmyhand/docker-redis)

