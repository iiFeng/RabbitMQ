# 使用消息队列
一种异步处理服务间的通信方式。生成数据的是生产者，调用数据的为消费者。

消息队列处于两者中间，当生产者发送过多数据，消费者无法一下进行处理时，使用消息队列将数据存储在队列中，在慢慢发送数据给消费者进行处理。消息队列的数据分为持久化和非持久化，如果是非持久化，当 mq 服务挂了者之前未处理的数据则丢失，反之亦然。

生产者发送的消息需要消息队列有进行 confirm 确认机制，如果消费者在处理该数据时挂了则需要 mq 确认已处理完，防止数据未被处理。

## 测试 mq ：

测试生产者，模拟消费者接收消息，并验证发出的消息的正确性

测试消费者，模拟生产者去发送消息，然后验证被测应该（exchange）能收到
## 性能测试
使用 perf-test 模拟发布者和一些消费者个数

```
docker run -it --rm --network perf-test --name rabbitmq -p 15672:15672 rabbitmq:3.8.2-management
docker pull ifeng1-docker.pkg.coding.net/coding-demo/qu/perf-test
docker run -it --rm --network perf-test pivotalrabbitmq/perf-test:latest --uri amqp://rabbitmq
//示例：-x 100个生产者；-y 100个消费者；-u queue名称为testque；bingding为kk01;
docker run -it --rm --network perf-test pivotalrabbitmq/perf-test:latest -x 100 -y 100 -u "q_sms" -a --id "test 1" --uri amqp://rabbitmq
```

![性能测试](https://github.com/iiFeng/RabbitMQ/blob/master/src/main/resources/mq.png?raw=true)