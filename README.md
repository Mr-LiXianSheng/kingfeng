# KingFeng

技术栈 `vue 2` `asp.net core` `docker` 

## 说明

KingFeng 仅供学习参考使用，请于下载后的 24 小时内删除，本人不对使用过程中出现的任何问题负责，包括但不限于 `数据丢失` `数据泄露`。

KingFeng 仅支持 qinglong 2.9+

不提供 `技术上的任何帮助`

[TG 频道]()

## 功能

- [x] 添加/更新wskey 自动执行wskey转换
- [x] 添加备注
- [x] 管理员登录 执行任务/任务日志查看

- [ ] 扫码推送卡片
- [ ] 环境变量导出/恢复
- [ ] 登录页面自定义公告
- [ ] 各种助力脚本执行
- [ ] 自建推送日志数据库
- [ ] 查看每个用户添加/修改wskey的日志并发送通知

### 配置文件

配置文件第一次部署后端会自动生成
配置文件所有项必填 如不填(无法预知的后果)

`backend` config.yaml
```yaml
#青龙地址 默认为空 必须以/结尾
QL_URL: http://localhost:5700/
#青龙OpenAPI Client_ID
QL_Client_ID: 
#青龙OpenAPI Client_Secret
QL_Client_Secret: 
#管理员密钥 第一次启动会自动生成
SecretKey: 
#wskey转cookie脚本全名称 如有重复 默认执行第一个
WsKeyTaskFullName: wskey转换
# 首页的抓取wskey教程链接 默认 http://www.baidu.com
Course: http://www.baidu.com
```
`frontend/dist` .env
```bash
# KingFeng 运行端口
PORT=5710
```
<!-- ### 推送卡片

自定义推送二维码：将 `push.jpg` 文件添加到 `/ql/kingfeng/static/` 目录下刷新网页即可。 -->

### wskey转换库
[Zy143L](https://github.com/Zy143L/wskey)

请按照使用文档正确拉取 wskey转换库

## 项目指南
```bash
docker run -dit \
   -p 5000:80 \
   --name kingfeng \
   --hostname kingfeng \
   ranqi03/kingfeng:latest


```
<!-- ## 注意事项 -->

<!-- ## 常见问题 -->