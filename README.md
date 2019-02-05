## これは何

SNI(Server Name Indication) を元にルーティングしてくれる以下の各 Proxy で SNI でのルーティングの動作確認を行う docker（docker-compose） の設定。

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

## 構成

FIXME
