- 单机ES和kibana。
- 一定要给./es目录 777 权限，其实是logs目录需要写权限
```sh
$ chmod -R 777 ./es
```

1. 启动服务：
```sh
$ docker-compose -p es-node up -d
``

2. ES设置密码。(忽略)

```sh
# 进入容器
docker exec -it es /bin/bash

# 设置密码-随机生成密码
elasticsearch-setup-passwords auto

# 设置密码-手动设置密码
elasticsearch-setup-passwords interactive

# 访问
curl 127.0.0.1:9200 -u elastic:123456
```

2. 修改ES密码。(忽略)
```txt
# 修改elastic密码为123456 (注：执行命令时会让认证之前账号密码)
curl -H "Content-Type:application/json" -XPOST -u elastic 'http://127.0.0.1:9200/_xpack/security/user/elastic/_password' -d '{ "password" : "123456" }'

```

3. 由于kibana不可用elastic这个用户，新建账户并授予权限。
```sh
# bin/elasticsearch-users
# 创建新账户
elasticsearch-users useradd username

# 给账户授权
elasticsearch-users roles -a superuser username
elasticsearch-users roles -a kibana_system username

# 重启kibana
```

4. 验证：

```txt
http://localhost:9200/

http://localhost:5601/
```