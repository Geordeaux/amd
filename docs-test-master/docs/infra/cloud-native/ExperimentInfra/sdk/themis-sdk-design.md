# SDK Design Principle
app 刚刚安装 SDK 后：注册用户，并开始从服务端拉取最新 AB 配置，缓存到本地，下次冷启动时生效。

app 冷启动：SDK 将缓存的 AB 配置生效（提供给业务方）

app 后台进入前台，或者 app 在前台时：SDK 每隔 5 分钟（默认 5 分钟间隔，间隔时长可以受到服务端控制）拉取最新 AB 配置，并缓存到本地。

SDK 需要缓存一份 AB 配置的原因：

app 启动后，会从 SDK 获取一份当前 AB 的配置。当服务端有新的 AB 配置的时候，SDK 获取到新配置，此时如果直接应用到 app，可能会导致这次启动打点数据混乱。前一部分打点数据来自前一份 AB 配置，后一份 AB 配置来自新的 AB 配置，从而导致实验数据污染。

## When get the flow result
SDK 遍历一遍 flows 数组，将所有的 k-v 读取出来保存到本地，拼成一个大的字典，提供给业务方使用。
conditions 条件类型的实验信息 payload 中，存的是user 自定義的 k-v map 字典
