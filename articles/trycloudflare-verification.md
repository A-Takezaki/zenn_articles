---
title: "trycloudflareを使って簡単にローカルサーバーを公開する"
emoji: "🦎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Cloudflare, Security]
published: true
published_at: 2024-12-28 09:00 # 未来の日時を指定する
---
## はじめに

筆者は普段はサイバーセキュリティ分野や脅威インテリジェンス（Cyber Threat Intelligence）に関わる業務を行っている。

RAT ツールが trycloudflare を使って使い捨てのホスティングをしているらしいので、どれくらい楽にできるのかを実際に試してみることにした。

https://iototsecnews.jp/2024/08/01/hackers-abuse-free-trycloudflare-to-deliver-remote-access-malware/

https://www.proofpoint.com/jp/blog/threat-insight/threat-actor-abuses-cloudflare-tunnels-deliver-rats

好奇心の起点はリサーチャー視点だけど、開発者視点でも Cloudflare を使っており、便利なら使えるようにしておこうという考えもある。

結果から言うと、とても簡単にローカル環境を公開できるので便利だということがわかった。

## 本記事を読んで嬉しい人

- trycloudflare を使ったサーバー公開の簡単さを体験したい人
    - セキュリティリサーチャーで、RAT ツール配信の実際の挙動を知りたい人
    - 開発者で、簡易的な一時公開環境が欲しい人
    - 自宅にサーバーがあって、とりあえずインターネット公開を試してみたい人

### 話さないこと

- 具体的な RAT ツール配信の挙動
- サイバーセキュリティっぽい観点はこの記事には含みません

## 準備

- 適当に自宅 Proxmox 上に VM を用意する
    - 別になんでもいいのだが、筆者の自宅に立っているのが Proxmox なのでこれを使った。
    - 試すだけなら WSL とかでもいい気がする。
- 適当に公開するサーバーを作成しておく
    
    ```python
    # Python 3.x
    python3 -m http.server 8080
    ```
    

## やること

- **`cloudflared`** を install する。[^1]
    
    ```bash
    sudo mkdir -p --mode=0755 /usr/share/keyrings
    curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null
    echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
    sudo apt-get update && sudo apt-get install cloudflared
    ```
    
- 公開するためのコマンドを発行する。
    
    ```bash
    cloudflared tunnel --url http://localhost:8080/
    ```

以上！わーお手軽！

Cloudflare Tunnel を本格的に使うには Cloudflare のクレデンシャルでログインして設定が必要だけど、trycloudflare を使えばクレデンシャルもドメインも不要で公開できる。すごい。

[^1]:[https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-local-tunnel/](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-local-tunnel/)

## 公開したあとの話と気をつけること

前節の公開するためのコマンドを発行したあとはこんな感じ。

```bash
hlpdsk@vm-dockerhost-helpdesk-dev-prxmx-01:~$ cloudflared tunnel --url http://localhost:8080
2024-12-27T17:33:12Z INF Thank you for trying Cloudflare Tunnel. Doing so, without a Cloudflare account, is a quick way to experiment and try it out. However, be aware that these account-less Tunnels have no uptime guarantee, are subject to the Cloudflare Online Services Terms of Use (https://www.cloudflare.com/website-terms/), and Cloudflare reserves the right to investigate your use of Tunnels for violations of such terms. If you intend to use Tunnels in production you should use a pre-created named tunnel by following: https://developers.cloudflare.com/cloudflare-one/connections/connect-apps
2024-12-27T17:33:12Z INF Requesting new quick Tunnel on trycloudflare.com...
2024-12-27T17:33:16Z INF +--------------------------------------------------------------------------------------------+
2024-12-27T17:33:16Z INF |  Your quick Tunnel has been created! Visit it at (it may take some time to be reachable):  |
2024-12-27T17:33:16Z INF |  https://venice-progress-constitutional-classifieds.trycloudflare.com                      |
2024-12-27T17:33:16Z INF +--------------------------------------------------------------------------------------------+
2024-12-27T17:33:16Z INF Cannot determine default configuration path. No file [config.yml config.yaml] in [~/.cloudflared ~/.cloudflare-warp ~/cloudflare-warp /etc/cloudflared /usr/local/etc/cloudflared]
2024-12-27T17:33:16Z INF Version 2024.11.1 (Checksum 55d789465955ccfffcd61ba72807a2a4495002f7d9b7cc5eadcaa1f93c279d25)
2024-12-27T17:33:16Z INF GOOS: linux, GOVersion: go1.22.5, GoArch: amd64
2024-12-27T17:33:16Z INF Settings: map[ha-connections:1 protocol:quic url:http://localhost:8080]
2024-12-27T17:33:16Z INF cloudflared will not automatically update if installed by a package manager.
2024-12-27T17:33:16Z INF Generated Connector ID: ac87a428-92f1-4bd3-8490-d26da339ff46
2024-12-27T17:33:16Z INF Initial protocol quic
2024-12-27T17:33:16Z INF ICMP proxy will use 10.1.1.117 as source for IPv4
2024-12-27T17:33:16Z INF ICMP proxy will use fe80::be24:11ff:fe51:7aac in zone ens18 as source for IPv6
2024-12-27T17:33:17Z INF Starting metrics server on 127.0.0.1:36993/metrics
2024/12/27 17:33:17 failed to sufficiently increase receive buffer size (was: 208 kiB, wanted: 7168 kiB, got: 416 kiB). See https://github.com/quic-go/quic-go/wiki/UDP-Buffer-Sizes for details.
2024-12-27T17:33:17Z INF Registered tunnel connection connIndex=0 connection=61e462d7-4511-41ec-9f6b-1ab1fe452ffd event=0 ip=198.41.192.37 location=kix04 protocol=quic

```

枠で囲まれている部分が外部からアクセスできる URL になる。

この URL はコマンドを実行するたびに変わるので、雑に公開してもコマンドを停止すれば外部からアクセスできなくなる。

つまり、用が終わればコマンドを切るだけで想定外の人からアクセスされなくなる。便利。

ただし、SSH 越しにコマンドを発行して放置していると、SSH のセッションが切れていつの間にか公開できなくなっているので注意（一敗）

## まとめ

- よく悪用されていると噂の trycloudflare を試してみた。
- 思った以上にとても簡単に公開できてしまった。
- そりゃ悪い人も使うわと思うとともに、開発者視点だと手軽にローカル環境を公開できてとても便利
- 開発者のみんなはぜひ使って欲しい。