---
title: "Apache 搭載の Linux で ASP.NET Core をホストする"
description: "Kestrel で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする CentOS にリバース プロキシ サーバーとして Apache を設定する方法を説明します。"
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 10/19/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: aa55ecd6dc8169e0e77b3899389ec924b1e1ae4a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a>Apache 搭載の Linux で ASP.NET Core をホストする

作成者: [Shayne Boyer](https://github.com/spboyer)

このガイドを使用して設定する方法を学習[Apache](https://httpd.apache.org/)にリバース プロキシ サーバーとして[CentOS 7](https://www.centos.org/)で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする[Kestrel](xref:fundamentals/servers/kestrel)です。 [Mod_proxy 拡張子](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)関連モジュールが、サーバーのリバース プロキシを作成します。

## <a name="prerequisites"></a>必須コンポーネント

1. Sudo 権限を持つ標準ユーザー アカウントを使用して CentOS 7 を実行しているサーバー
2. ASP.NET Core アプリケーション

## <a name="publish-the-app"></a>アプリの発行

アプリを発行すると、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 ランタイムのリリースの構成で (`centos.7-x64`)。 内容をコピー、 *bin/Release/netcoreapp2.0/centos.7-x64/publish* SCP、FTP、またはその他のファイルの転送方法を使用してサーバーのフォルダーです。

> [!NOTE]
> 運用展開シナリオでは、継続的インテグレーション ワークフローは、アプリを発行し、サーバーへの資産のコピーの作業を行います。 

## <a name="configure-a-proxy-server"></a>プロキシ サーバーを構成する

リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。 リバース プロキシは、HTTP 要求を終了し、ASP.NET アプリケーションに転送します。

プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。 リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。 このガイドでは、Apache は Kestrel ASP.NET Core アプリケーションが機能していること、同じサーバーで実行されているリバース プロキシとして構成されます。

### <a name="install-apache"></a>Apache をインストールする

CentOS パッケージを最新の安定したバージョンに更新プログラム:

```bash
sudo yum update -y
```

1 つの CentOS に Apache web サーバーをインストール`yum`コマンド。

```bash
sudo yum -y install httpd mod_ssl
```

コマンドの実行後の出力例:

```bash
Downloading packages:
httpd-2.4.6-40.el7.centos.4.x86_64.rpm               | 2.7 MB  00:00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 
Verifying  : httpd-2.4.6-40.el7.centos.4.x86_64      1/1 

Installed:
httpd.x86_64 0:2.4.6-40.el7.centos.4

Complete!
```

> [!NOTE]
> この例では、出力は、CentOS 7 バージョンが 64 ビットであるために httpd.86_64 を反映します。 Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。 

### <a name="configure-apache-for-reverse-proxy"></a>Apache をリバース プロキシとして構成する

Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。 ファイルのいずれか、 *.conf*拡張機能が、モジュール構成ファイルでだけでなくアルファベット順に処理される`/etc/httpd/conf.modules.d/`、ファイルのモジュールを読み込むために必要な構成が含まれています。

という名前のアプリの構成ファイルを作成`hellomvc.conf`:

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

**VirtualHost**ノードは、サーバー上の 1 つまたは複数のファイルに複数回を使用できます。 **VirtualHost**がポート 80 を使用して任意の IP アドレスでリッスンするように設定します。 次の 2 行は、5000 のポートで 127.0.0.1 のサーバーにルートにプロキシ要求に設定されます。 双方向通信の*ProxyPass*と*ProxyPassReverse*が必要です。

ごとにログ記録を構成できる**VirtualHost**を使用して**ErrorLog**と**CustomLog**ディレクティブです。 **ErrorLog** 、サーバーが、エラーをログの場所と**CustomLog**ファイル名とログ ファイルの形式を設定します。 この場合、要求の情報が記録される場所はします。 要求ごとに行が表示されます。

ファイルを保存し、構成をテストします。 すべてに合格すると、応答は `Syntax [OK]` になります。

```bash
sudo service httpd configtest
```

Apache を再起動します。

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a>アプリの監視

Apache がへの要求を転送するセットアップでは今すぐ`http://localhost:80`で Kestrel で実行されている ASP.NET Core アプリケーションに`http://127.0.0.1:5000`です。  ただし、Apache は Kestrel プロセスを管理するセットアップをされていません。 使用して*systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成します。 *systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。 


### <a name="create-the-service-file"></a>サービス ファイルを作成する

次のように、サービス定義ファイルを作成します。

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

アプリのサービス ファイルの例:

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if dotnet service crashes
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> **ユーザー** &mdash;場合、ユーザー *apache*使用されていない、構成によってユーザー必要があります最初に作成してファイルの適切な所有権を指定します。

ファイルを保存し、サービスを有効にします。

```bash
systemctl enable kestrel-hellomvc.service
```

サービスを開始し、実行されていることを確認します。

```bash
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

リバース プロキシが構成されているとを通じて管理 Kestrel *systemd*、web アプリが完全に構成されているし、ローカルのコンピューター上のブラウザーからアクセスできる`http://localhost`です。 応答ヘッダーの検査、**サーバー**ヘッダーは、ASP.NET Core アプリケーションが Kestrel によって処理されることを示します。

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>ログを表示する

Web アプリから Kestrel を使用して管理を使用して*systemd*イベントとプロセスは、一元的な履歴に記録されます。 ただし、このジャーナルには、サービスとプロセスによって管理されるすべてのエントリが含まれます*systemd*です。 `kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

時間のフィルタ リング、コマンドを使用して時間のオプションを指定します。 たとえば、使用して`--since today`を現在の日付のフィルター処理または`--until 1 hour ago`前の 1 時間のエントリを参照します。 詳細については、次を参照してください。、 [journalctl についてマニュアル](https://www.unix.com/man-page/centos/1/journalctl/)です。

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a>アプリをセキュリティで保護します。

### <a name="configure-firewall"></a>ファイアウォールを構成する

*Firewalld*動的デーモンでネットワーク ゾーンのサポートされたファイアウォールを管理します。 ポートとパケット フィルタ リングは、iptables によって引き続き管理できます。 *Firewalld*既定でインストールする必要があります。 `yum`パッケージをインストールまたはがインストールされていることを確認するために使用します。

```bash
sudo yum install firewalld -y
```

使用して`firewalld`を開くには、アプリに必要なポートのみです。 この場合、ポート 80 と 443 が使用されています。 次のコマンドでは、ポート 80 と 443 を開くには完全に設定します。

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

ファイアウォールの設定を再読み込みされます。 使用可能なサービスと、既定のゾーン内のポートを確認してください。 検査することによってオプションがある`firewall-cmd -h`です。

```bash 
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

```bash
public (default, active)
interfaces: eth0
sources: 
services: dhcpv6-client
ports: 443/tcp 80/tcp
masquerade: no
forward-ports: 
icmp-blocks: 
rich rules: 
```

### <a name="ssl-configuration"></a>SSL の構成

Ssl は、Apache を構成する、 *mod_ssl*モジュールを使用します。 ときに、 *httpd*モジュールがインストールされている、 *mod_ssl*モジュールもインストールするとします。 インストールされている場合を使用して`yum`構成に追加します。

```bash
sudo yum install mod_ssl
```
SSL を強制するのには、インストール、 `mod_rewrite` URL 書き換えを有効にするモジュール。

```bash
sudo yum install mod_rewrite
```

変更、 *hellomvc.conf* URL 書き換えを有効にして、ポート 443 での通信をセキュリティで保護するファイル。

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> この例は、ローカルに生成された証明書を使用します。 **SSLCertificateFile**ドメイン名のプライマリ証明書ファイルでなければなりません。 **SSLCertificateKeyFile**生成するか、キー ファイル CSR を作成します。 **SSLCertificateChainFile** (存在する場合)、中間証明書ファイルをする必要があります証明機関から提供されています。

ファイルを保存し、構成をテストします。

```bash
sudo service httpd configtest
```

Apache を再起動します。

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Apache に関するその他の推奨事項

### <a name="additional-headers"></a>追加のヘッダー

悪意のある攻撃からセキュリティで保護するためには、いくつかのヘッダーをする必要がありますか、変更または追加できます。 いることを確認、`mod_headers`モジュールがインストールされています。

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a>Clickjacking の攻撃からセキュリティで保護された Apache

[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)とも呼ばれる、 *UI redress 攻撃*、悪意のある攻撃は、ここで web サイトの訪問者は、騙されてクリックしてリンクやボタンは別のページをしている現在のページです。 使用して`X-FRAME-OPTIONS`サイトをセキュリティで保護します。

編集、 *httpd.conf*ファイル。

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

行を追加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`です。 ファイルを保存します。 Apache を再起動します。

#### <a name="mime-type-sniffing"></a>MIME タイプ スニッフィング

`X-Content-Type-Options`ヘッダーにより、Internet Explorer から*MIME スニッフィング*(ファイルの決定`Content-Type`ファイルの内容から)。 サーバーを設定する場合、`Content-Type`ヘッダーを`text/html`で、 `nosniff` Internet Explorer、オプションのセットとして内容を表示する`text/html`ファイルの内容に関係なく。

編集、 *httpd.conf*ファイル。

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

行を追加`Header set X-Content-Type-Options "nosniff"`です。 ファイルを保存します。 Apache を再起動します。

### <a name="load-balancing"></a>負荷分散 

この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。 ためにないの単一障害点です。使用して*mod_proxy_balancer*を変更して、 **VirtualHost**なります Apache プロキシ サーバーの背後に web アプリの複数のインスタンスを管理するためです。

```bash
sudo yum install mod_proxy_balancer
```

追加のインスタンスの次に示す構成ファイルで、`hellomvc`アプリは 5001 ポート上で実行するセットアップです。 *プロキシ*セクションは、負荷を分散する 2 つのメンバー バランサー構成で設定が*byrequests*です。

```
<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/ [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPass / balancer://mycluster/ 

    ProxyPassReverse / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5001/

    <Proxy balancer://mycluster>
        BalancerMember http://127.0.0.1:5000
        BalancerMember http://127.0.0.1:5001 
        ProxySet lbmethod=byrequests
    </Proxy>

    <Location />
        SetHandler balancer
    </Location>
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a>速度の制限
使用して*mod_ratelimit*に含まれている、 *httpd*モジュールのクライアントの帯域幅は制限されることができます。

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
ファイルの例では、600 KB/秒 ルートの場所として帯域幅を制限します。

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
