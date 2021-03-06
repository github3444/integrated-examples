介绍：

此配置包括 v2ray、naiveproxy(caddy2) 应用。用 haproxy 或 nginx 为 v2ray（vless+tcp+tls与trojan+tcp+tls）、naiveproxy(caddy2) 进行 SNI 分流（四层转发），实现共用443端口。另 caddy2 还同时为v2ray（vless+tcp+tls 与 trojan+tcp+tls）提供回落服务。v2ray包括应用如下：

1、vless+tcp+tls（回落/分流配置。）

2、vless+ws+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加vmess+ws+tls、SS+v2ray-plugin+tls、trojan+ws+tls应用。）

3、SS+v2ray-plugin+tls（tls由vless+tcp+tls提供及处理，不需配置；另可改成或添加vless+ws+tls、vmess+ws+tls、trojan+ws+tls应用。）

4、vless+h2c+tls（tls由caddy2提供及处理，，不需配置；另可改成或添加vmess+h2c+tls应用。）

5、vmess+kcp+seed（可改成vless+kcp+seed，或添加它。）

6、trojan+tcp+tls（回落配置。）

v2ray vless+tcp 类应用直连，v2ray ws 类应用分流一次；v2ray trojan+tcp 直连；naiveproxy 直连，v2ray h2 类应用分流（反代）一次。

注意：

1、caddy2 目前只能 json 配置才能开启 h2c server，故要实现回落 h2 就不能采用 Caddyfile 配置；另外 caddy2 版本不能低于 v2.1.0 ，否则不支持 h2c server。

2、caddy2 支持 http/1.1 server 与 h2c server 共用一个端口。

3、caddy2 发行版不支持 PROXY protocol，如要支持 PROXY protocol 需选 caddy2-proxyprotocol 插件定制编译或选使用本人 github 文件即可。

4、v2ray v4.31.0 版本及以后才支持 trojan 及完整回落。

5、用haproxy 或 nginx 为 v2ray vless+tcp、v2ray trojan+tcp、naiveproxy(caddy2) 进行 SNI 分流（四层转发），实现共用443端口。

6、nginx 预编译程序包不带支持 SNI 分流协议的模块。如要使用此项协议应用，需加 stream_ssl_preread_module 模块构建自定义模板，再进行源代码编译和安装。

7、配置1：没有启用 PROXY protocol，仅端口回落。配置2：启用了 PROXY protocol，且端口回落。
