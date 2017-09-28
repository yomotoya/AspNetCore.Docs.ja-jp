---
title: "Apache 搭載の Linux で ASP.NET Core をホストする"
description: "CentOS 上にリバース プロキシ サーバーとして Apache をセットアップし、Kestrel 上で実行されている ASP.NET Core Web アプリケーションに HTTP トラフィックを転送する方法について説明します。"
keywords: "ASP.NET Core, Apache, CentOS, リバース プロキシ,Linux, mod_proxy, httpd, ホスティング"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.date: 10/19/2016
ms.topic: article
ms.assetid: fa9b0cb7-afb3-4361-9e7e-33afffeaca0c
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/apache-proxy
ms.openlocfilehash: 34ede2fdebe0e9516f62e39618e7adba3c8c89db
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a>Apache 搭載の Linux で ASP.NET Core をホストするための環境をセットアップし、その環境に展開する

作成者: [Shayne Boyer](https://github.com/spboyer)

Apache は非常に一般的な HTTP サーバーです。nginx と同様に、プロキシとして構成して HTTP トラフィックをリダイレクトすることができます。 このガイドでは、CentOS 7 上で Apache をセットアップしてリバース プロキシとして使用し、着信接続を受け入れ、Kestrel 上で実行されている ASP.NET Core にリダイレクトする方法について説明します。 そのために、*mod_proxy* 拡張機能などの関連する Apache モジュールを使用します。

## <a name="prerequisites"></a>必須コンポーネント

1. CentOS 7 を実行しているサーバーと、sudo 特権が与えられた標準ユーザー アカウント。
2. 既存の ASP.NET Core アプリケーション。 

## <a name="publish-your-application"></a>アプリケーションを公開する

開発環境から `dotnet publish -c Release` を実行し、サーバー上で実行できる内蔵ディレクトリにアプリケーションをパッケージ化します。 発行したアプリケーションは、SCP、FTP などのファイル転送方法を使用してサーバーにコピーする必要があります。 

> [!NOTE]
> 実稼働環境に展開するシナリオの場合、継続的な統合ワークフローで、アプリケーションの発行処理とアセットのサーバーへのコピー処理を行います。 

## <a name="configure-a-proxy-server"></a>プロキシ サーバーを構成する

リバース プロキシは、動的 Web アプリケーションにサービスを提供するための一般的な仕組みです。 リバース プロキシは HTTP 要求を終了させ、ASP.NET アプリケーションに転送します。

プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。 リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。 このガイドでは、Kestrel が ASP.NET Core アプリケーションを提供しているものと同じサーバー上で実行されるリバース プロキシとして Apache を構成します。 

アプリケーションの各部分は、別の物理コンピューター、Docker コンテナー、またはアーキテクチャのニーズや制限に応じた組み合わせに配置できます。

### <a name="install-apache"></a>Apache をインストールする

CentOS に Apache Web サーバーをインストールするには、1 つのコマンドで実行できますが、まずパッケージを更新してみましょう。

```bash
    sudo yum update -y
```

この手順で、インストールされているすべてのパッケージを最新バージョンに更新します。 `yum` を使用して Apache をインストールする

```bash
    sudo yum -y install httpd mod_ssl
```

次のような内容が出力されます。

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
> この例では、CentOS 7 のバージョンが 64 ビットなので、出力は httpd.86_64 を反映しています。 お使いのサーバーによっては出力が異なる可能性があります。 Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。 

### <a name="configure-apache-for-reverse-proxy"></a>Apache をリバース プロキシとして構成する

Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。 `/etc/httpd/conf.modules.d/` 内のモジュール構成ファイルに加え、拡張子が **.conf** のファイルがアルファベット順で処理されます。このディレクトリには、モジュールの読み込みに必要な構成ファイルが含まれています。

アプリケーションの構成ファイル (この例では `hellomvc.conf`) を作成します。

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

*VirtualHost* ノードは、1 つのファイル内、またはサーバー上の多数のファイル内に存在する可能性があります。このノードは、ポート 80 を使用する任意の IP アドレスでリッスンできるように設定されています。 次の 2 行は、ルートで受信したすべての要求を逆順でコンピューター 127.0.0.1 のポート 5000 に渡すように設定されています。 双方向通信にするには、*ProxyPass* と *ProxyPassReverse* の両方の設定が必要です。

VirtualHost ごとにログを構成するには、*ErrorLog* および *CustomLog* ディレクティブを使用します。 *ErrorLog* は、サーバーがエラーをログに記録する場所です。*CustomLog* には、ログ ファイルのファイル名と形式を設定します。 この例では、要求の情報がログに記録される場所です。 1 つの要求につき 1 行が記録されます。

ファイルを保存し、構成をテストします。 すべてに合格すると、応答は `Syntax [OK]` になります。

```bash
    sudo service httpd configtest
```

Apache を再起動します。

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a>アプリケーションの監視

これで、`http://localhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリケーション (`http://127.0.0.1:5000`) に転送するように Apache が設定されました。  ただし、Apache は Kestrel プロセスを管理するようには設定されていません。 ここでは、*systemd* を利用し、基礎 Web アプリを起動し、監視するサービス ファイルを作成します。 *systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。 


### <a name="create-the-service-file"></a>サービス ファイルを作成する

次のように、サービス定義ファイルを作成します 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

アプリケーションのサービス ファイルの例です。

```text
[Unit]
    Description=Example .NET Web API Application running on CentOS 7

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
> **ユーザー** -- ユーザー *apache* が構成で利用されない場合、ここで定義するユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。

ファイルを保存し、サービスを有効にします。

```bash
    systemctl enable kestrel-hellomvc.service
```

サービスを起動し、動作していることを確認します。

```
    systemctl start kestrel-hellomvc.service
    systemctl status kestrel-hellomvc.service

    ● kestrel-hellomvc.service - Example .NET Web API Application running on CentOS 7
        Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
        Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
    Main PID: 9021 (dotnet)
        CGroup: /system.slice/kestrel-hellomvc.service
                └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

リバース プロキシを構成し、Kestrel を systemd で管理している状態で、Web アプリケーションは完全に構成されたことになり、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。 応答ヘッダーを調べるときにも、Kestrel がサービスを提供している ASP.NET Core アプリケーションが**サーバー**に表示されます。

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a>ログを表示する

Kestrel を利用する Web アプリケーションは systemd で管理されるため、すべてのイベントとプロセスが記録され、中心的ジャーナルが生成されます。 ただし、このジャーナルには、systemd が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。 `kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a>アプリケーションのセキュリティ強化

### <a name="configure-firewall"></a>ファイアウォールを構成する

*Firewalld* は、ネットワーク ゾーンのサポートによってファイアウォールを管理する動的デーモンですが、iptables を使用してポートとパケット フィルターを管理することもできます。 既定で Firewalld がインストールされているはずですが、`yum` を使用してパッケージをインストールするか確認することができます。

```bash
    sudo yum install firewalld -y
```

`firewalld` を使用して、アプリケーションに必要なポートのみを開くことができます。 この場合、ポート 80 と 443 が使用されています。 次のコマンドを実行すると、これらのポートを開くように永続的に設定されます。

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

ファイアウォール設定を再読み込みし、既定のゾーンで使用できるサービスとポートを確認します。 `firewall-cmd -h` を確認すると、使用できるサービスとポートを取得できます。

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

SSL 用に Apache を構成するには、mod_ssl モジュールを使用します。  これは、`httpd` モジュールをインストールしたときに最初にインストールされたモジュールです。 見つからないかインストールされていなかった場合は、yum を使用して構成に追加します。

```bash
    sudo yum install mod_ssl
```
SSL を強制するには、`mod_rewrite` をインストールします。

```bash
    sudo yum install mod_rewrite
```

この例で作成された `hellomvc.conf` ファイルは、再書き込みを有効にするだけでなく、HTTPS のために新しい **VirtualHost** セクションを追加できるように変更する必要があります。

```text
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
> この例では、ローカルで生成された証明書を使用しています。 **SSLCertificateFile** は、ドメイン名のプライマリ証明書ファイルです。 **SSLCertificateKeyFile** は、CSR を作成したときに生成されるキー ファイルです。 **SSLCertificateChainFile** は、証明機関から提供された中間証明書ファイル (存在する場合) です。

ファイルを保存し、構成をテストします。

```bash
    sudo service httpd configtest
```

Apache を再起動します。

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a>Apache に関するその他の推奨事項

### <a name="additional-headers"></a>その他のヘッダー 
悪意のある攻撃から防御するために、変更または追加する必要があるヘッダーがいくつかあります。 必ず `mod_headers` モジュールをインストールします。

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a>Apache をクリックジャッキングから保護する
クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。 クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。 X-FRAME-OPTIONS を利用し、サイトのセキュリティを強化します。

httpd.conf ファイルを編集します。

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` を追加し、ファイルを保存し、Apache を再起動します。

#### <a name="mime-type-sniffing"></a>MIME タイプ スニッフィング

このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、Internet Explorer で MIME スニッフィングが阻止されます。 nosniff オプションを指定すると、サーバーがコンテンツを text/html と指定すれば、ブラウザーで text/html として表示されます。

httpd.conf ファイルを編集します。

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

行 `Header set X-Content-Type-Options "nosniff"` を追加し、ファイルを保存し、Apache を再起動します。

### <a name="load-balancing"></a>負荷分散 

この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。  ただし、単一障害点を持たないように、*mod_proxy_balancer* を使用し、VirtualHost を変更することで、Apache プロキシ サーバーの背後で複数インスタンスの Web アプリケーションを管理できるようにします。

```bash
    sudo yum install mod_proxy_balancer
```

構成ファイルで、追加インスタンスの `hellomvc` アプリケーションをセットアップしてポート 5001 で実行するようにしました。また、*byrequests* の負荷を分散するメンバーが 2 つのバランサー構成を使用して *Proxy* セクションを設定しました。

```text
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
`htttpd` モジュールに含まれている `mod_ratelimit` を使用すると、クライアントの帯域幅を制限できます。 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
次のファイル例では、ルート ディレクトリ以下の帯域幅を 600 KB/秒に制限しています。

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
