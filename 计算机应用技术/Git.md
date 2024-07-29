


















### git config
> 查看系统级配置
git config --system --list

> 查看全局用户配置
git config --global --list

> 查看当前仓库的配置信息
git config --local --list

> 取消设置
git  config --global --unset
##### 设置认证存储
设置缓存存储
git config --global credential.helper cache
设置磁盘存储
git config --glboal credential.helper store 

git config --glboal credential.helpe 'cache-timeout =3600‘

##### 设置代理
> http代理
git config --global http.proxy {URL}

> https代理
git config --global https.proxy {URL}