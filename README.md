## これは何

SNI（Server Name Indication）を元にルーティングしてくれる以下の各 Proxy ソフトウェアで SNI でのルーティングの動作確認を行う docker（docker-compose）の設定。

- [tcpproxy/cmd/tlsrouter](https://github.com/google/tcpproxy/tree/master/cmd/tlsrouter)
- [haproxy](http://www.haproxy.org/)
- [nginx](https://nginx.org/)

また Proxy した場合、backend（この構成では Web サーバ） では client（この構成では goss） の IP アドレスがわからないのだが、[proxy protocol](https://www.haproxy.com/blog/haproxy/proxy-protocol/) を利用して backend でも client の IP アドレスを取得できる設定も合わせて確認している。

## 必要なもの

- [docker](https://docs.docker.com/install/)
- [docker-compose](https://docs.docker.com/compose/install/)

## 使い方

```console
$ docker-compose build
$ docker-compose up -d
$ # 実行テスト
$ docker-compose exec goss goss v --format tap
```

## 実行結果例

### テストの実行結果

```
$ docker-compose exec goss goss v --format tap                                                                                                                 [22/2994]
1..28
ok 1 - HTTP: https://noproxy.foo.example.com: status: matches expectation: [200]
ok 2 - HTTP: https://noproxy.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
ok 3 - HTTP: https://haproxy-proxy.foo.example.com: status: matches expectation: [200]
ok 4 - HTTP: https://haproxy-proxy.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
ok 5 - HTTP: https://nginx-proxy.bar.example.com:8443: status: matches expectation: [200]
ok 6 - HTTP: https://nginx-proxy.bar.example.com:8443: Body: all expectations found: [server: bar, protocol: https]
ok 7 - HTTP: https://haproxy-proxy.bar.example.com: status: matches expectation: [200]
ok 8 - HTTP: https://haproxy-proxy.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 9 - HTTP: https://noproxy.bar.example.com: status: matches expectation: [200]
ok 10 - HTTP: https://noproxy.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 11 - HTTP: https://haproxy.bar.example.com: status: matches expectation: [200]
ok 12 - HTTP: https://haproxy.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 13 - HTTP: https://tlsrouter.foo.example.com: status: matches expectation: [200]
ok 14 - HTTP: https://tlsrouter.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
ok 15 - HTTP: https://nginx.foo.example.com: status: matches expectation: [200]
ok 16 - HTTP: https://nginx.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
ok 17 - HTTP: https://nginx-proxy.foo.example.com:8443: status: matches expectation: [200]
ok 18 - HTTP: https://nginx-proxy.foo.example.com:8443: Body: all expectations found: [server: foo, protocol: https]
ok 19 - HTTP: https://nginx.bar.example.com: status: matches expectation: [200]
ok 20 - HTTP: https://nginx.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 21 - HTTP: https://tlsrouter-proxy.foo.example.com: status: matches expectation: [200]
ok 22 - HTTP: https://tlsrouter-proxy.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
ok 23 - HTTP: https://tlsrouter-proxy.bar.example.com: status: matches expectation: [200]
ok 24 - HTTP: https://tlsrouter-proxy.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 25 - HTTP: https://tlsrouter.bar.example.com: status: matches expectation: [200]
ok 26 - HTTP: https://tlsrouter.bar.example.com: Body: all expectations found: [server: bar, protocol: https]
ok 27 - HTTP: https://haproxy.foo.example.com: status: matches expectation: [200]
ok 28 - HTTP: https://haproxy.foo.example.com: Body: all expectations found: [server: foo, protocol: https]
```

### Web サーバのログ

```console
$ docker-compose logs web_bar
Attaching to compare_sni_proxy_web_bar_1_4b2303ae5a6a
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:noproxy.bar.example.com    remote_addr:172.21.0.3  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:haproxy.bar.example.com    remote_addr:172.21.0.2  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:nginx-proxy.bar.example.com        remote_addr:172.21.0.6  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:172.21.0.3
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:nginx.bar.example.com      remote_addr:172.21.0.6  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:haproxy-proxy.bar.example.com      remote_addr:172.21.0.2  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:172.21.0.3
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:tlsrouter.bar.example.com  remote_addr:172.21.0.5  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:
web_bar_1_4b2303ae5a6a | time:2019-02-05T04:49:27+00:00 host:tlsrouter-proxy.bar.example.com    remote_addr:172.21.0.5  request_method:GET      request_uri:/   https:on        status:200      forwardedfor:-  proxy_protocol_addr:172.21.0.3
```

## 構成

FIXME
