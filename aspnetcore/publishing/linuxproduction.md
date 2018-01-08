---
title: "Nginx 搭載の Linux で ASP.NET Core をホストする"
description: "Ubuntu 16.04 でリバース プロキシとして Nginx をセットアップし、Kestrel で実行している ASP.NET Core Web アプリケーションに HTTP トラフィックを転送する方法について説明します。"
keywords: "ASP.NET Core、Linux、nginx、Ubuntu、リバース プロキシ"
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a>Nginx 搭載の Linux で ASP.NET Core をホストするための環境をセットアップし、その環境に展開する

[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿

このガイドでは、Ubuntu 16.04 サーバーで本稼働対応の ASP.NET Core 環境をセットアップする方法について説明します。

**注:** Ubuntu 14.04 の場合、Kestrel プロセスを監視するためのソリューションとして supervisord が推奨されます。 systemd は Ubuntu 14.04 ではご利用いただけません。 [この文書の前のバージョンをご覧ください](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)

このガイドでは:

* リバース プロキシ サーバーの後ろに既存の ASP.NET Core アプリケーションを配置します
* Kestrel Web サーバーに要求を転送するようにリバース プロキシ サーバーを設定します
* Web アプリケーションを起動時にデーモンとして実行します
* Web アプリケーションの再起動を支援するようにプロセス管理ツールを構成します

## <a name="prerequisites"></a>必須コンポーネント

1. Ubuntu 16.04 サーバーへのアクセスと sudo 特権が与えられた標準ユーザー アカウント
2. 既存の ASP.NET Core アプリケーション

## <a name="copy-over-your-app"></a>アプリをコピーする

開発環境から `dotnet publish` を実行し、サーバーで実行可能な自己完結型ディレクトリにアプリをパッケージ化します。

ワークフローに統合されているツール (SCP や FTP など) を利用し、サーバーに ASP.NET Core アプリをコピーします。 次のようにアプリをテストします。
 - コマンド ラインから `dotnet yourapp.dll` を実行します。
 - ブラウザーで、`http://<serveraddress>:<port>` に移動し、アプリが Linux で動作することを検証します。 
 
## <a name="configure-a-reverse-proxy-server"></a>リバース プロキシ サーバーを構成する

リバース プロキシは、動的 Web アプリケーションにサービスを提供するための一般的なしくみです。 リバース プロキシは HTTP 要求を終了させ、ASP.NET Core アプリケーションに転送します。

### <a name="why-use-a-reverse-proxy-server"></a>リバース プロキシ サーバーを利用する理由

ASP.NET Core から動的コンテンツにサービスを提供することに関して、Kestrel は優れた Web ツールです。しかしながら、IIS、Apache、Nginx などのサーバーのように機能が豊富ではありません。 リバース プロキシ サーバーは、静的コンテンツ サービス、要求のキャッシュ、要求の圧縮、HTTP サーバーからの SSL 終了などの作業の負荷を軽減します。 リバース プロキシ サーバーは専用コンピューター上に置かれることもあれば、HTTP サーバーと並んで展開されることもあります。

このガイドの目的のために、単一インスタンスの Nginx が使用されます。 HTTP サーバーと並んで、同じサーバー上で実行されます。 要件によっては、別のセットアップを選択することになります。

要求はリバース プロキシによって転送されるため、`Microsoft.AspNetCore.HttpOverrides` パッケージの `ForwardedHeaders` ミドルウェアを使用します。 このミドルウェアは `X-Forwarded-Proto` ヘッダーを利用して `Request.Scheme` を更新します。リダイレクト URI とその他のセキュリティ ポリシーが正しく機能します。

リバース プロキシ サーバーを設定するとき、認証ミドルウェアで `UseForwardedHeaders` を先に実行する必要があります。 この順序により、認証ミドルウェアは影響を受けた値を利用し、正しいリダイレクト URI を生成できます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`UseAuthentication` や同様の認証スキーム ミドルウェアを呼び出す前に、`UseForwardedHeaders` メソッドを呼び出します (*Startup.cs* の `Configure` メソッド)。

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

`UseIdentity`、`UseFacebookAuthentication` や同様の認証スキーム ミドルウェアを呼び出す前に、`UseForwardedHeaders` メソッドを呼び出します (*Startup.cs* の `Configure` メソッド)。

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a>Nginx をインストールする

```bash
sudo apt-get install nginx
```

> [!NOTE]
> オプションの Nginx モジュールをインストールする場合、ソースから Nginx をビルドしなければならないことがあります。

`apt-get` を利用し、Nginx をインストールします。 インストーラーにより System V init スクリプトが作成されます。このスクリプトがシステム起動時に Nginx をデーモンとして実行します。 Nginx は初めてのインストールとなるので、次を実行して明示的に起動します。

```bash
sudo service nginx start
```

ブラウザーで Nginx の既定のランディング ページが表示されることを確認します。

### <a name="configure-nginx"></a>Nginx を構成する

Nginx をリバース プロキシとして構成し、ASP.NET Core アプリケーションに要求を転送するには、`/etc/nginx/sites-available/default` を変更します。 テキスト エディターで開き、中身を次のものに変更します。

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

この Nginx 構成ファイルは、ポート `80` から入ってくるパブリック トラフィックをポート `5000` に転送します。

Nginx 構成の変更が終わったら、`sudo nginx -t` を実行し、構成ファイルの構文を検証します。 構成ファイル テストが合格の場合、`sudo nginx -s reload` を実行し、変更を反映するように Nginx に要求できます。

## <a name="monitoring-our-application"></a>アプリケーションの監視

これで、`http://yourhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリケーション (`http://127.0.0.1:5000`) に転送するように Nginx が設定されました。 ただし、Nginx は Kestrel プロセスを管理するようには設定されていません。 *systemd* を利用し、基礎 Web アプリを起動し、監視するサービス ファイルを作成できます。 *systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。 

### <a name="create-the-service-file"></a>サービス ファイルを作成する

次のように、サービス定義ファイルを作成します。

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

次はアプリケーションのサービス ファイルの例です。

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

**注:** ユーザー *www-data* が構成で利用されない場合、ここで定義するユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。

ファイルを保存し、サービスを有効にします。

```bash
systemctl enable kestrel-hellomvc.service
```

サービスを起動し、動作していることを確認します。

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

リバース プロキシを構成し、Kestrel を systemd で管理している状態で、Web アプリケーションは完全に構成されたことになり、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。 妨げとなるファイアウォールを禁止すれば、リモート コンピューターからもアクセスできます。 応答ヘッダーを調べるとき、Kestrel がサービスを提供している ASP.NET Core アプリケーションが `Server` ヘッダーに表示されます。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>ログの表示する

Kestrel を利用する Web アプリケーションは `systemd` で管理されるため、すべてのイベントとプロセスが記録され、中心的ジャーナルが生成されます。 ただし、このジャーナルには、`systemd` が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。 `kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>アプリケーションのセキュリティ強化

### <a name="enable-apparmor"></a>AppArmor を有効にする

Linux Security Modules (LSM) は、Linux 2.6 以降の Linux カーネルに含まれるフレームワークです。 LSM は、セキュリティ モジュールのさまざまな実装に対応しています。 [AppArmor](https://wiki.ubuntu.com/AppArmor) は Mandatory Access Control システムを実装する LSM です。このシステムは、プログラムのリソース範囲を限定できます。 AppArmor が有効であり、正しく構成されていることを確認します。

### <a name="configuring-our-firewall"></a>ファイアウォールの構成

使用されていないすべての外部ポートを閉じます。 ufw (uncomplicated firewall/複雑ではないファイアウォール) は `iptables` のフロント エンドとなり、ファイアウォールを構成するためのコマンド ライン インターフェイスを提供します。 必要なあらゆるポートでトラフィックを許可するように `ufw` が設定されていることを確認します。

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a>Nginx のセキュリティを強化する

Nginx の既定のディストリビューションでは SSL が有効になっていません。 追加のセキュリティ機能を有効にするには、ソースからビルドします。

#### <a name="download-the-source-and-install-the-build-dependencies"></a>ソースをダウンロードし、ビルド依存関係をインストールする

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Nginx 応答名を変更する

*src/http/ngx_http_header_filter_module.c* を編集します。

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>オプションを構成し、ビルドする

正規表現には PCRE ライブラリが必要です。 正規表現は、ngx_http_rewrite_module の場所ディレクティブで使用されます。 http_ssl_module により HTTPS プロトコル サポートが追加されます。

*ModSecurity* など、Web アプリケーション ファイアウォールを利用してアプリケーションの防護を検討してください。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>SSL を構成する

* ポート `443` で HTTPS トラフィックを待ち受けるようにサーバーを構成します。信頼されている証明書機関 (CA) が発行した有効な証明書を指定します。

* 次の */etc/nginx/nginx.conf* ファイルにある方法のいくつかを利用し、セキュリティを強化します。 たとえば、強力な暗号を選択したり、HTTP 経由のすべてのトラフィックを HTTPS にリダイレクトしたりします。

* `HTTP Strict-Transport-Security` (HSTS) ヘッダーを追加すると、クライアントが行う後続のすべての要求が HTTPS 経由のみになります。

* Strict-Transport-Security ヘッダーは追加しないでください。あるいは、今後、SSL を無効にすることがあれば、適切な `max-age` を選択してください。

*/etc/nginx/proxy.conf* 構成ファイルを追加します。

[!code-nginx[Main](linuxproduction/proxy.conf)]

*/etc/nginx/nginx.conf* 構成ファイルを編集します。 この例では、1 つの構成ファイルに `http` セクションと `server` セクションの両方が含まれています。

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Nginx をクリックジャッキングから守る
クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。 クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。 X-FRAME-OPTIONS を利用し、サイトのセキュリティを強化します。

*nginx.conf* ファイルを編集します。

```bash
sudo nano /etc/nginx/nginx.conf
```

行 `add_header X-Frame-Options "SAMEORIGIN";` を追加し、ファイルを保存し、Nginx を再起動します。

#### <a name="mime-type-sniffing"></a>MIME タイプ スニッフィング

このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、ほとんどのブラウザーで MIME スニッフィングが阻止されます。 `nosniff` オプションを指定すると、サーバーがコンテンツを "text/html" と読むとき、ブラウザーはそれを "text/html" として表示します。

*nginx.conf* ファイルを編集します。

```bash
sudo nano /etc/nginx/nginx.conf
```

行 `add_header X-Content-Type-Options "nosniff";` を追加し、ファイルを保存し、Nginx を再起動します。
