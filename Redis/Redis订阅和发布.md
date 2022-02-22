# Redis订阅和发布

- 发布/订阅(pub/sub)是一种消息通信模式

## 基本命令

1. `subscribe <channel>` ：订阅一个频道，接收该频道的消息
2. `publish <channel> <message>` : 对一个频道发送消息