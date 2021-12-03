# EMQ

## 安装

docker-compose up

## 持久化

服务运行起来后，先拷贝文件

```bash
docker cp emqx:/opt/emqx/etc ./data/etc
docker cp emqx:/opt/emqx/lib ./data/lib
docker cp emqx:/opt/emqx/data ./data/data
docker cp emqx:/opt/emqx/log ./data/log
```

修改主题订阅权限

 emqx/etc/acl.conf

```diff

{allow, {user, "dashboard"}, subscribe, ["$SYS/#"]}.

- {allow, {ipaddr, "127.0.0.1"}, pubsub, ["$SYS/#", "#"]}.
+ %%{allow, {ipaddr, "127.0.0.1"}, pubsub, ["$SYS/#", "#"]}.

+ {allow, all, subscribe, ["$SYS/brokers/+/clients/#"]}.

- {deny, all, subscribe, ["$SYS/#", {eq, "#"}]}.
+ %%{deny, all, subscribe, ["$SYS/#", {eq, "#"}]}.

 {allow, all}.

```

修改文件权限

```bash
chown -R 1000:1000 ./data
chmod -R 775 ./data
```

配置 volumes

```yaml
volumes:
    - ./data/etc:/opt/emqx/etc
    - ./data/lib:/opt/emqx/lib
    - ./data/data:/opt/emqx/data
    - ./data/log:/opt/emqx/log
```

## 重新启动服务

docker-compose down

docker-compose up

## 兼容性问题

CGW 中使用的是 EMQX v3的接口 （/connetctions），与 v4 不兼容，详见 CGW 模块

详见 `com.oneiotworld.qj.cgw.utils.MqttApiUtils` 和 `application.yaml` 配置文档

cat com.oneiotworld.qj.cgw.utils.MqttApiUtils.java

```java
    public static boolean clientIsOnline(String clientId) {
        String apiurl = url + "/connections/" + clientId;
        log.debug("url: {}", url);
        log.debug("appid:{}", appid);
        log.debug("secret:{}", secret);
        try {
```


 `application-dev.yaml` 配置文档

```yaml
mqtt:
  api:
    url: http://{ip}:18083/api/v3
```

## 账号配置

1. 登录 emqx-dashboard, 如 http://{ip}:18083, 默认账号密码 admin/public
2. 修改 admin 账号密码
3. 在用户管理，为 CGW 模块配置一个账号密码, 并配置 application-dev.yaml
注意，因 3.7 版本添加 App 无效，这里改用添加账号

```diff
mqtt:
  api:
    url: http://{ip}:18083/api/v3
+   appid: appid
+   appsecret: secret
```
