# seata-docker

关联项目:

https://github.com/seata/seata

https://github.com/seata/seata-samples/tree/docker/springboot-dubbo-fescar

## 本地构建(体验)
```sh
git clone https://github.com/seata/seata-docker.git
cd seata-docker
docker build -t seata:0.5.1 .\build\
```

## 案例使用帮助
由于一些原因, seata docker 镜像使用暂不提供容器外部调用 ,那么需要案例相关项目也在容器内部 和 seata 镜像保持link模式

```sh
## 创建一个单独的网络
docker network create app_net
## 启动 env (nacos,seata,mysql)
docker-compose -f example/env.yaml up
## 启动 example (samples-account,samples-storage)
docker-compose -f example/example.yaml up
## 启动 order(samples-order)
docker-compose -f example/order.yaml up
## 启动 business (samples-dubbo-business-call)
docker-compose -f example/business.yaml up
```

### 浏览器 打开 nacos 控制台 http://localhost:8848/nacos/ 看看所有实例是否注册成功
### 连接到mysql 导入表结构
### 测试
```sh
# 账户服务  扣费
curl  -H "Content-Type: application/json" -X POST --data "{\"id\":1,\"userId\":\"1\",\"amount\":100}"   127.0.0.1:8102/account/dec_account
# 库存服务 扣库存
curl  -H "Content-Type: application/json" -X POST --data "{\"commodityCode\":\"C201901140001\",\"count\":100}"   127.0.0.1:8100/storage/dec_storage
# 订单服务 添加订单 扣费
curl  -H "Content-Type: application/json" -X POST --data "{\"userId\":\"1\",\"commodityCode\":\"C201901140001\",\"orderCount\":10,\"orderAmount\":100}"   127.0.0.1:8101/order/create_order
# 业务服务 客户端seata版本太低
curl  -H "Content-Type: application/json" -X POST --data "{\"userId\":\"1\",\"commodityCode\":\"C201901140001\",\"count\":10,\"amount\":100}"   127.0.0.1:8104/business/dubbo/buy
 ```

