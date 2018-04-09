---
title: Nginx 搭載の Linux で ASP.NET Core をホストする
author: rick-anderson
description: Ubuntu 16.04 Kestrel で実行されている ASP.NET Core web アプリへの HTTP トラフィックを転送にリバース プロキシとして Nginx をセットアップする方法について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-nginx
ms.openlocfilehash: 64093b9fcfa9047145de8f8b142f72fa1515f248
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2018
---
# <a name="host-aspnet-core-on-linux-with-nginx"></a>Nginx 搭載の Linux で ASP.NET Core をホストする

[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿

このガイドでは、Ubuntu 16.04 サーバーで本稼働対応の ASP.NET Core 環境をセットアップする方法について説明します。

> [!NOTE]
> Ubuntu 14.04 の*supervisord* Kestrel プロセスを監視するためのソリューションとしてはお勧めします。 *systemd* Ubuntu 14.04 では使用できません。 [このドキュメントの以前のバージョンを参照してください](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)です。

このガイドでは:

* リバース プロキシ サーバーの背後にある既存の ASP.NET Core アプリケーションを配置します。
* リバース プロキシ サーバーを設定すると、Kestrel web サーバーに要求を転送します。
* デーモンとしての起動時に web アプリが実行されることを確認します。
* Web アプリを再起動するプロセスの管理ツールを構成します。

## <a name="prerequisites"></a>必須コンポーネント

1. Ubuntu 16.04 サーバーへのアクセスと sudo 特権が与えられた標準ユーザー アカウント
1. 既存の ASP.NET Core アプリケーション

## <a name="copy-over-the-app"></a>アプリ経由でコピーします。

実行[dotnet 発行](/dotnet/core/tools/dotnet-publish)サーバーで実行できる自己完結型のディレクトリに、アプリのパッケージを開発環境からです。

どのようなツールを使用してサーバーに ASP.NET Core アプリケーションのコピーは、組織のワークフロー (たとえば、SCP、FTP など) に統合します。 次のようにアプリをテストします。

* コマンドラインから実行`dotnet <app_assembly>.dll`です。
* ブラウザーで、`http://<serveraddress>:<port>` に移動し、アプリが Linux で動作することを検証します。 
 
## <a name="configure-a-reverse-proxy-server"></a>リバース プロキシ サーバーを構成する

リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。 リバース プロキシは、HTTP 要求を終了し、ASP.NET Core アプリケーションに転送します。

### <a name="why-use-a-reverse-proxy-server"></a>リバース プロキシ サーバーを利用する理由

Kestrel は、ASP.NET Core から動的なコンテンツを提供しているに便利です。 ただし、web サービス機能は、IIS、Apache、Nginx などのサーバーと豊富な機能としてはありません。 リバース プロキシ サーバーは、静的なコンテンツ、要求をキャッシュ、要求、および HTTP サーバーからの SSL ターミネーションを圧縮するなどの作業をオフロードできます。 リバース プロキシ サーバーは専用コンピューター上に置かれることもあれば、HTTP サーバーと並んで展開されることもあります。

このガイドの目的のために、単一インスタンスの Nginx が使用されます。 HTTP サーバーと並んで、同じサーバー上で実行されます。 要件に基づき、異なる設定が選択すること。

要求は、リバース プロキシによって転送される、ためから転送されるヘッダー ミドルウェアを使用して、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージです。 ミドルウェアの更新プログラム、`Request.Scheme`を使用して、`X-Forwarded-Proto`ヘッダー、そのリダイレクト Uri とその他のセキュリティ ポリシーが正常に動作するようにします。

認証ミドルウェアの任意の型を使用する場合、転送ヘッダー ミドルウェアが最初に実行する必要があります。 この順序により、認証ミドルウェアがヘッダーの値を使用して正しくリダイレクト Uri を生成することができます。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)または類似の認証スキームのミドルウェア。 転送するミドルウェアを構成、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダー。

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)と[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)または同様の認証スキームミドルウェア。 転送するミドルウェアを構成、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダー。

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

ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)は指定した、ミドルウェアに転送する既定のヘッダーが`None`です。

追加の構成は、プロキシ サーバーとロード バランサーの背後にホストされているアプリの必要があります。 詳細については、次を参照してください。[にプロキシ サーバーを操作すると、ロード バランサーの ASP.NET Core の構成](xref:host-and-deploy/proxy-load-balancer)です。

### <a name="install-nginx"></a>Nginx をインストールする

```bash
sudo apt-get install nginx
```

> [!NOTE]
> 省略可能な Nginx モジュールをインストールする場合ソースから Nginx を構築する必要があります。

`apt-get` を利用し、Nginx をインストールします。 インストーラーにより System V init スクリプトが作成されます。このスクリプトがシステム起動時に Nginx をデーモンとして実行します。 Nginx は初めてのインストールとなるので、次を実行して明示的に起動します。

```bash
sudo service nginx start
```

ブラウザーで Nginx の既定のランディング ページが表示されることを確認します。

### <a name="configure-nginx"></a>Nginx を構成する

ASP.NET Core アプリケーションに要求を転送するリバース プロキシとして Nginx を構成するには、変更*/etc/nginx/sites-available/default*です。 テキスト エディターで開き、中身を次のものに変更します。

```nginx
server {
    listen        80;
    server_name   example.com *.example.com;
    location / {
        proxy_pass         http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

ない場合`server_name`Nginx の一致が既定のサーバーを使用します。 既定のサーバーが定義されていない場合、最初のサーバー構成ファイルでは、既定のサーバーです。 ベスト プラクティスとして、構成ファイルで 444 のステータス コードを返す特定の既定のサーバーを追加します。 既定のサーバーの構成例を示します。

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

上記の構成ファイルと、既定のサーバーと Nginx はホスト ヘッダーを持つポート 80 でパブリック トラフィックを受け付ける`example.com`または`*.example.com`です。 これらのホストと一致しない要求 Kestrel に転送されません。 Nginx で Kestrel に一致する要求を転送する`http://localhost:5000`です。 参照してください[nginx が要求をどのように処理するか](https://nginx.org/docs/http/request_processing.html)詳細についてはします。

> [!WARNING]
> 適切なを指定する[server_name ディレクティブ](https://nginx.org/docs/http/server_names.html)セキュリティの脆弱性にアプリを公開します。 サブドメイン ワイルドカード バインド (たとえば、 `*.example.com`) 全体の親ドメインを制御する場合、このセキュリティ上のリスクは発生しません (to `*.com`、に対して脆弱である)。 詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。

Nginx 構成が確立されると、実行`sudo nginx -t`構成ファイルの構文を確認します。 構成ファイルのテストが成功した場合は、強制的に実行して、変更を取得する Nginx`sudo nginx -s reload`です。

## <a name="monitoring-the-app"></a>アプリの監視

サーバーがセットアップへの要求を転送する`http://<serveraddress>:80`で Kestrel で実行されている ASP.NET Core アプリケーションにログオン`http://127.0.0.1:5000`です。 ただし、Nginx は Kestrel プロセスを管理するセットアップをされていません。 *systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成するために使用できます。 *systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。 

### <a name="create-the-service-file"></a>サービス ファイルを作成する

次のように、サービス定義ファイルを作成します。

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

アプリのサービス ファイルの例を次に示します。

```ini
[Unit]
Description=Example .NET Web API App running on Ubuntu

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

**注:**場合、ユーザー *www データ*使用されていない、構成によって、ここで定義したユーザー必要がありますで最初に作成されファイルの適切な所有権を指定します。
**注:** Linux には、区別するファイル システム。 設定する ASPNETCORE_ENVIRONMENT"Production"、構成ファイルの検索中に*appsettings です。Production.json*ではなく、 *appsettings.production.json*です。

ファイルを保存し、サービスを有効にします。

```bash
systemctl enable kestrel-hellomvc.service
```

サービスを開始して、実行されていることを確認してください。

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

リバース プロキシの構成および Kestrel systemd を介して管理されている場合、web アプリが完全に構成されているし、にローカル コンピューター上のブラウザーからアクセスできる`http://localhost`です。 妨げている可能性があるすべてのファイアウォールがなければ、リモート コンピューターからアクセスできることもです。 応答ヘッダーの検査、`Server`ヘッダー Kestrel によって処理される ASP.NET Core アプリケーションを示しています。

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>ログを表示する

Web アプリから Kestrel を使用して、使用して管理`systemd`、すべてのイベントとプロセスは、一元的な履歴に記録されます。 ただし、このジャーナルには、`systemd` が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。 `kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>アプリをセキュリティで保護します。

### <a name="enable-apparmor"></a>AppArmor を有効にする

Linux セキュリティ モジュール (LSM) は、Linux 2.6 以降では Linux カーネルの一部であるフレームワークです。 LSM は、セキュリティ モジュールのさまざまな実装に対応しています。 [AppArmor](https://wiki.ubuntu.com/AppArmor) は Mandatory Access Control システムを実装する LSM です。このシステムは、プログラムのリソース範囲を限定できます。 AppArmor が有効であり、正しく構成されていることを確認します。

### <a name="configuring-the-firewall"></a>ファイアウォールを構成します。

使用されていないすべての外部ポートを閉じます。 ufw (uncomplicated firewall/複雑ではないファイアウォール) は `iptables` のフロント エンドとなり、ファイアウォールを構成するためのコマンド ライン インターフェイスを提供します。 いることを確認`ufw`必要なポートのトラフィックを許可するように構成します。

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

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a>Nginx 応答名を変更する

*src/http/ngx_http_header_filter_module.c* を編集します。

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a>オプションを構成し、ビルドする

正規表現には PCRE ライブラリが必要です。 正規表現は、ngx_http_rewrite_module の場所ディレクティブで使用されます。 http_ssl_module により HTTPS プロトコル サポートが追加されます。

ように web アプリケーション ファイアウォールの使用を検討*ModSecurity*にアプリを強化します。

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a>SSL を構成する

* ポートで HTTPS トラフィックをリッスンするようにサーバーを構成する`443`を信頼された証明書機関 (CA) によって発行された有効な証明書を指定します。

* 次のプラクティスを採用することによって、セキュリティを強化する*/etc/nginx/nginx.conf*ファイル。 たとえば、強力な暗号を選択したり、HTTP 経由のすべてのトラフィックを HTTPS にリダイレクトしたりします。

* `HTTP Strict-Transport-Security` (HSTS) ヘッダーを追加すると、クライアントが行う後続のすべての要求が HTTPS 経由のみになります。

* Strict トランスポート セキュリティ ヘッダーを追加しないが、適切な選択または`max-age`場合は SSL は、今後は無効にします。

*/etc/nginx/proxy.conf* 構成ファイルを追加します。

[!code-nginx[](linux-nginx/proxy.conf)]

*/etc/nginx/nginx.conf* 構成ファイルを編集します。 この例では、1 つの構成ファイルに `http` セクションと `server` セクションの両方が含まれています。

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a>Nginx をクリックジャッキングから守る
クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。 クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。 X-フレームのオプションを使用、サイトをセキュリティで保護します。

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
