# amqp命令概览

## Connection

| 名称                | 包含内容体  | 客户端方法              | 简要描述    |
| ---                | ---       | ---                    | ---        |
|Connection.Start    | ❌        |	factory.newConnection |	建立连接相关 |
|Connection.Start-Ok |			
|Connection.Tune	 |		
|Connection.Tune-Ok	 |		
|Connection.Open	 |		
|Connection.Open-Ok	 |		
|Connection.Close	 | ❌        |	connection.close      | 关闭连接    |
|Connection.Close-Ok |

## Channel
| 名称                | 包含内容体  | 客户端方法              | 简要描述    |
| ---                | ---       | ---                    | ---        |
| Channel.Open	     | ❌	     | connection.openChannel |	开启信道    |
| Channel.Open-Ok	 |		
| Channel.Close	     | ❌	     | channel.close          |	关闭信道    |
| Channel.Close-Ok   |


## Exchange
| 名称                | 包含内容体  | 客户端方法               | 简要描述        |
| ---                | ---       | ---                     | ---            |
| Exchange.Declare	 | ❌	     | channel.exchangeDeclare | 声明交换器       |
| Exchange.Declare-Ok|			
| Exchange.Delete	 | ❌	     | channel.exchangeDelete  | 删除交换器       |
| Exchange.Delete-OK |			
| Exchange.Bind	     | ❌	     | channel.exchangeBind    | 交换器与交换器绑定 |
| Exchange.Bind-Ok	 |		
| Exchange.Unbind	 | ❌	     | channel.exchangeUnbind |	交换器与交换器解绑  |
| Exchange.Unbind-Ok |


## Queue
| 名称                | 包含内容体  | 客户端方法              | 简要描述       |
| ---                | ---       | ---                    | ---           |
| Queue.Declare      | 	❌	     | channel.queueDeclare   | 声明队列       |
| Queue.Declare-Ok   |
| Queue.Bind	     | ❌	     | channel.queueBind      | 队列与交换机绑定 | 
| Queue.Bind-Ok	     |
| Queue.Purge	     | ❌	     | channel.queuePurge     | 清楚队列中内容   | 
| Queue.Purge-Ok     |
| Queue.Delete       | 	❌       | 	channel.queueDelete   | 删除队列        | 
| Queue.Delete-OK    |
| Queue.Unbind	     | ❌	     | channel.queueUnbind	 | 队列与交换器解绑  | 
| Queue.Unbind-Ok    |


## Basic
| 名称                | 包含内容体  | 客户端方法             | 简要描述                |
| ---                | ---       | ---                   | ---                    |
| Basic.Qos          | 	❌       | channel.basicQos      | 	设置未被确认消费的个数   | 
| Basic.Qos-Ok		 |
| Basic.Consume *	 | ❌	     | channel.basicConsume	 | 消费消息（推）           | 
| Basic.Consume-Ok   |
| Basic.Cancel       | 	❌	     | channel.basicCancel   | 取消                   | 
| Basic.Cancel-Ok	 |
| Basic.Publish *    | ✅	     | channel.basicPublish  | 发送消息                | 
| Basic.Return	     | ✅        | 	🈚️	                 | 未能成功路由的消息返回    | 
| Basic.Deliver	     | ✅	     | 🈚️	                 | broker推送消息          | 
| Basic.Get *	     | ❌	     | channel.basicGet      | 	消费消息（拉）          | 
| Basic.Get-Ok	     | ✅		
| Basic.Ack	         | ❌	     | channel.basicAck      | 	确认                   | 
| Basic.Reject	     | ❌	     | channel.basicReject	 | 拒绝（单条）             | 
| Basic.Recover	     | ❌	     | channel.basicRecover	 | 请求broker重发未ack的消息 | 
| Basic.Recover-Ok   |
| Basic.Nack	     | ❌	     | channel.basicNack	 | 拒绝（可批量）            | 


## 其他
| 名称                | 包含内容体  | 客户端方法              | 简要描述         |
| ---                | ---       | ---                    | ---             |
| Tx.Select        	 | ❌	     | channel.txSelect	      | 开启事务         | 
| Tx.Select-Ok		 |
| Tx.Commit	         | ❌     	 | channel.txCommit       | 事务提交         |
| Tx.Commit-Ok	     |
| Tx.Rollback	     | ❌        | 	channel.txRollback    | 事务回滚         | 
| Tx.Rollback-Ok	 |
| Confirm.Select	 | ❌        | 	channel.confirmSelect | 开启发送端确认模式 | 
| Confirm.Select-Ok	 | 	

