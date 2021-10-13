# kafka

## 配置

编辑 .env

将 DOCKER_HOST_IP=设置为本机地址（局域网地址）

## 启动

docker-compose up

## 相关命令

### 生产者

kafka-console-producer --bootstrap-server 10.1.32.212:9092 --topic qjiang_gw_to_cbs_queue

### 消费者

kafka-console-consumer --bootstrap-server 10.1.32.212:9092 --topic qjiang_gw_to_cbs_queue --from-beginning

### 查看主题列表

## 参考

- [kafka-stack-docker-compose](https://github.com/simplesteph/kafka-stack-docker-compose)