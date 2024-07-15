---
title: citus
date: 2024-07-11 15:47:58
tags:
---

# Citus安装

1. 添加Citus仓库
``` bash
# Add Citus repository for package manager
curl https://install.citusdata.com/community/rpm.sh | sudo bash
```

2. 安装Citus
``` bash
# install PostgreSQL with Citus extension
sudo yum install -y citus113_15
# initialize system database (using RHEL 6 vs 7 method as necessary)
sudo service postgresql-15 initdb || sudo /usr/pgsql-15/bin/postgresql-15-setup initdb
# preload citus extension
echo "shared_preload_libraries = 'citus'" | sudo tee -a /var/lib/pgsql/15/data/postgresql.conf
```

3. 初始化配置
``` bash
sudo vi /var/lib/pgsql/15/data/postgresql.conf
```
``` text
# Uncomment listen_addresses for the changes to take effect
listen_addresses = '*'
```
``` bash
sudo vi /var/lib/pgsql/15/data/pg_hba.conf
```
``` text
# Allow unrestricted access to nodes in the local network. The following ranges
# correspond to 24, 20, and 16-bit blocks in Private IPv4 address spaces.
host    all             all             10.0.0.0/8              trust

# Also allow the host unrestricted access to connect to itself
host    all             all             127.0.0.1/32            trust
host    all             all             ::1/128                 trust
```
``` bash
# start the db server
sudo service postgresql-15 restart
# and make it start automatically when computer does
sudo chkconfig postgresql-15 on
# Enable citus extention
sudo -i -u postgres psql -c "CREATE EXTENSION citus;"
```

1. master节点配置
``` bash
# Register the hostname that future workers will use to connect
# to the coordinator node.
#
# You'll need to change the example, 'coord.example.com',
# to match the actual hostname or ip address

sudo -i -u postgres psql -c \
  "SELECT citus_set_coordinator_host('coord.example.com', 5432);"

# Add the worker nodes.
#
# Similarly, you'll need to change 'worker-101' and 'worker-102' to the
# actual hostnames

sudo -i -u postgres psql -c "SELECT * from citus_add_node('worker-101', 5432);"
sudo -i -u postgres psql -c "SELECT * from citus_add_node('worker-102', 5432);"
```

1. 验证是否安装成功
``` bash
sudo -i -u postgres psql -c "SELECT * FROM citus_get_active_worker_nodes();"
```

# Citus相关概念
# 分布式表
分布式表在物理存储方面进行了水平分表，不同的行被分配到了不同的node中进行存储。
# 引用表
1. 引用表修改策略（二阶段提交协议-2PC）
二阶段提交协议（`Two-phase Commit`，即2PC）是常用的分布式事务解决方案，它可以保证在分布式事务中，要么所有参与进程都提交事务，要么都取消事务，即实现 ACID 的原子性（A）。在数据一致性中，它的含义是：要么所有副本（备份数据）同时修改某个数值，要么都不更改，以此来保证数据的强一致性。
    - 准备阶段第一阶段:投票；该阶段的主要目的是在于打探数据库集群中的各个参与者是否能够正常执行事务，具体步骤如下:
        - 协调者向所有的参与者发送事务执行请求，并等待参与者反馈事务执行结果
        - 事务参与者收到请求后，执行事务但是不提交，并记录事务日志
        - 参与者将自己事务执行情况反馈给协调者，同时阻塞等待协调者的后续指令
    - 经过第一阶段的协调者询问之后，各个参与者回复自己事务的执行情况，存在三种可能:
        - 所有参与者都回复能正常执行事务
        - 一个或者多个参与者回复事务执行失败
        - 协调者等待超时
    - 2PC的缺陷
        - 单点问题：协调者在整个两阶段提交过程中扮演着举足轻重的作用，一旦协调者所在服务器宕机，就会影响整个数据库集群的正常运行。比如在第二阶段中，如果协调者因为故障不能正常发送事务提交或回滚通知，那么参与者们将一直处于阻塞状态，整个数据库集群将无法提供服务。
        - 同步阻塞：两阶段提交执行过程中，所有的参与者都需要听从协调者的统一调度，期间处于阻塞状态而不能从事其他操作，这样效率极其低下。
        - 数据不一致性：两阶段提交协议虽然是分布式数据强一致性所设计，但仍然存在数据不一致性的可能性。比如在第二阶段中，假设协调者发出了事务 commit 通知，但是因为网络问题该通知仅被一部分参与者所收到并执行了 commit 操作，其余的参与者则因为没有收到通知一直处于阻塞状态，这时候就产生了数据的不一致性。
2. 拓展：3PC。三阶段提交协议通过引入一个`can_commit` 阶段，以及超时策略来减少整个集群的阻塞时间，提升系统性能。

# Citus创建分布式表示例
1. 创建表
``` sql
create table test_schema.test_table
(
    id          serial,
    tenant_id   integer,
    description varchar(255)
);
```
2. 将创建的表设置为分布式表
``` sql
SELECT create_distributed_table('test_schema.test_table', 'tenant_id');
```
# Citus创建引用表示例
1. 创建表
``` sql
create table test_schema.test_table
(
    id          serial,
    tenant_id   integer,
    description varchar(255)
);
```
2. 将创建的表设置为引用表
``` sql
SELECT create_reference_table('test_schema.test_table', 'tenant_id');
```