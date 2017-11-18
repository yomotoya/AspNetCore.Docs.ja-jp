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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-apache-and-deploy-to-it"></a><span data-ttu-id="3daab-104">Apache 搭載の Linux で ASP.NET Core をホストするための環境をセットアップし、その環境に展開する</span><span class="sxs-lookup"><span data-stu-id="3daab-104">Set up a hosting environment for ASP.NET Core on Linux with Apache, and deploy to it</span></span>

<span data-ttu-id="3daab-105">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="3daab-105">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="3daab-106">Apache は非常に一般的な HTTP サーバーです。nginx と同様に、プロキシとして構成して HTTP トラフィックをリダイレクトすることができます。</span><span class="sxs-lookup"><span data-stu-id="3daab-106">Apache is a very popular HTTP server and can be configured as a proxy to redirect HTTP traffic similar to nginx.</span></span> <span data-ttu-id="3daab-107">このガイドでは、CentOS 7 上で Apache をセットアップしてリバース プロキシとして使用し、着信接続を受け入れ、Kestrel 上で実行されている ASP.NET Core にリダイレクトする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3daab-107">In this guide, we will learn how to set up Apache on CentOS 7 and use it as a reverse proxy to welcome incoming connections and redirect them to the ASP.NET Core application running on Kestrel.</span></span> <span data-ttu-id="3daab-108">そのために、*mod_proxy* 拡張機能などの関連する Apache モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="3daab-108">For this purpose, we will use the *mod_proxy* extension and other related Apache modules.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3daab-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3daab-109">Prerequisites</span></span>

1. <span data-ttu-id="3daab-110">CentOS 7 を実行しているサーバーと、sudo 特権が与えられた標準ユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="3daab-110">A server running CentOS 7, with a standard user account with sudo privilege.</span></span>
2. <span data-ttu-id="3daab-111">既存の ASP.NET Core アプリケーション。</span><span class="sxs-lookup"><span data-stu-id="3daab-111">An existing ASP.NET Core application.</span></span> 

## <a name="publish-your-application"></a><span data-ttu-id="3daab-112">アプリケーションを公開する</span><span class="sxs-lookup"><span data-stu-id="3daab-112">Publish your application</span></span>

<span data-ttu-id="3daab-113">開発環境から `dotnet publish -c Release` を実行し、サーバー上で実行できる内蔵ディレクトリにアプリケーションをパッケージ化します。</span><span class="sxs-lookup"><span data-stu-id="3daab-113">Run `dotnet publish -c Release` from your development environment to package your application into a self-contained directory that can run on your server.</span></span> <span data-ttu-id="3daab-114">発行したアプリケーションは、SCP、FTP などのファイル転送方法を使用してサーバーにコピーする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3daab-114">The published application must then be copied to the server using SCP, FTP or other file transfer method.</span></span> 

> [!NOTE]
> <span data-ttu-id="3daab-115">実稼働環境に展開するシナリオの場合、継続的な統合ワークフローで、アプリケーションの発行処理とアセットのサーバーへのコピー処理を行います。</span><span class="sxs-lookup"><span data-stu-id="3daab-115">Under a production deployment scenario, a continuous integration workflow does the work of publishing the application and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="3daab-116">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="3daab-116">Configure a proxy server</span></span>

<span data-ttu-id="3daab-117">リバース プロキシは、動的 Web アプリケーションにサービスを提供するための一般的な仕組みです。</span><span class="sxs-lookup"><span data-stu-id="3daab-117">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="3daab-118">リバース プロキシは HTTP 要求を終了させ、ASP.NET アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="3daab-118">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET application.</span></span>

<span data-ttu-id="3daab-119">プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="3daab-119">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="3daab-120">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="3daab-120">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="3daab-121">このガイドでは、Kestrel が ASP.NET Core アプリケーションを提供しているものと同じサーバー上で実行されるリバース プロキシとして Apache を構成します。</span><span class="sxs-lookup"><span data-stu-id="3daab-121">In this guide, Apache is being configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core application.</span></span> 

<span data-ttu-id="3daab-122">アプリケーションの各部分は、別の物理コンピューター、Docker コンテナー、またはアーキテクチャのニーズや制限に応じた組み合わせに配置できます。</span><span class="sxs-lookup"><span data-stu-id="3daab-122">Each piece of the application can exist on separate physical machines, Docker containers, or a combination of configurations depending on your architectural needs or restrictions.</span></span>

### <a name="install-apache"></a><span data-ttu-id="3daab-123">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="3daab-123">Install Apache</span></span>

<span data-ttu-id="3daab-124">CentOS に Apache Web サーバーをインストールするには、1 つのコマンドで実行できますが、まずパッケージを更新してみましょう。</span><span class="sxs-lookup"><span data-stu-id="3daab-124">Installing the Apache web server on CentOS is a single command, but first let's update our packages.</span></span>

```bash
    sudo yum update -y
```

<span data-ttu-id="3daab-125">この手順で、インストールされているすべてのパッケージを最新バージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="3daab-125">This ensures that all of the installed packages are updated to their latest version.</span></span> <span data-ttu-id="3daab-126">`yum` を使用して Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="3daab-126">Install Apache using `yum`</span></span>

```bash
    sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="3daab-127">次のような内容が出力されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-127">The output should reflect something similar to the following.</span></span>

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
> <span data-ttu-id="3daab-128">この例では、CentOS 7 のバージョンが 64 ビットなので、出力は httpd.86_64 を反映しています。</span><span class="sxs-lookup"><span data-stu-id="3daab-128">In this example the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="3daab-129">お使いのサーバーによっては出力が異なる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3daab-129">The output may be different for your server.</span></span> <span data-ttu-id="3daab-130">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="3daab-130">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="3daab-131">Apache をリバース プロキシとして構成する</span><span class="sxs-lookup"><span data-stu-id="3daab-131">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="3daab-132">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="3daab-132">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="3daab-133">`/etc/httpd/conf.modules.d/` 内のモジュール構成ファイルに加え、拡張子が **.conf** のファイルがアルファベット順で処理されます。このディレクトリには、モジュールの読み込みに必要な構成ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3daab-133">Any file with the **.conf** extension will be processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="3daab-134">アプリケーションの構成ファイル (この例では `hellomvc.conf`) を作成します。</span><span class="sxs-lookup"><span data-stu-id="3daab-134">Create a configuration file for your app, for this example we'll call it `hellomvc.conf`</span></span>

```text
    <VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/
        ErrorLog /var/log/httpd/hellomvc-error.log
        CustomLog /var/log/httpd/hellomvc-access.log common
    </VirtualHost>
```

<span data-ttu-id="3daab-135">*VirtualHost* ノードは、1 つのファイル内、またはサーバー上の多数のファイル内に存在する可能性があります。このノードは、ポート 80 を使用する任意の IP アドレスでリッスンできるように設定されています。</span><span class="sxs-lookup"><span data-stu-id="3daab-135">The *VirtualHost* node, of which there can be multiple in a file or on a server in many files, is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="3daab-136">次の 2 行は、ルートで受信したすべての要求を逆順でコンピューター 127.0.0.1 のポート 5000 に渡すように設定されています。</span><span class="sxs-lookup"><span data-stu-id="3daab-136">The next two lines are set to pass all requests received at the root to the machine 127.0.0.1 port 5000 and in reverse.</span></span> <span data-ttu-id="3daab-137">双方向通信にするには、*ProxyPass* と *ProxyPassReverse* の両方の設定が必要です。</span><span class="sxs-lookup"><span data-stu-id="3daab-137">For there to be bi-directional communication, both settings *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="3daab-138">VirtualHost ごとにログを構成するには、*ErrorLog* および *CustomLog* ディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="3daab-138">Logging can be configured per VirtualHost using *ErrorLog* and *CustomLog* directives.</span></span> <span data-ttu-id="3daab-139">*ErrorLog* は、サーバーがエラーをログに記録する場所です。*CustomLog* には、ログ ファイルのファイル名と形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="3daab-139">*ErrorLog* is the location where the server will log errors and *CustomLog* sets the filename and format of log file.</span></span> <span data-ttu-id="3daab-140">この例では、要求の情報がログに記録される場所です。</span><span class="sxs-lookup"><span data-stu-id="3daab-140">In our case this is where request information will be logged.</span></span> <span data-ttu-id="3daab-141">1 つの要求につき 1 行が記録されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-141">There will be one line for each request.</span></span>

<span data-ttu-id="3daab-142">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="3daab-142">Save the file, and test the configuration.</span></span> <span data-ttu-id="3daab-143">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="3daab-143">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="3daab-144">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3daab-144">Restart Apache.</span></span>

```text
    sudo systemctl restart httpd
    sudo systemctl enable httpd
```

## <a name="monitoring-our-application"></a><span data-ttu-id="3daab-145">アプリケーションの監視</span><span class="sxs-lookup"><span data-stu-id="3daab-145">Monitoring our application</span></span>

<span data-ttu-id="3daab-146">これで、`http://localhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリケーション (`http://127.0.0.1:5000`) に転送するように Apache が設定されました。</span><span class="sxs-lookup"><span data-stu-id="3daab-146">Apache is now setup to forward requests made to `http://localhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="3daab-147">ただし、Apache は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="3daab-147">However, Apache is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="3daab-148">ここでは、*systemd* を利用し、基礎 Web アプリを起動し、監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3daab-148">We will use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="3daab-149">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="3daab-149">*systemd* is an init system that provides many powerful features for starting, stopping and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="3daab-150">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="3daab-150">Create the service file</span></span>

<span data-ttu-id="3daab-151">次のように、サービス定義ファイルを作成します</span><span class="sxs-lookup"><span data-stu-id="3daab-151">Create the service definition file</span></span> 

```bash
    sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="3daab-152">アプリケーションのサービス ファイルの例です。</span><span class="sxs-lookup"><span data-stu-id="3daab-152">An example service file for our application.</span></span>

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
> <span data-ttu-id="3daab-153">**ユーザー** -- ユーザー *apache* が構成で利用されない場合、ここで定義するユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="3daab-153">**User** -- If the user *apache* is not used by your configuration, the user defined here must be created first and given proper ownership for files</span></span>

<span data-ttu-id="3daab-154">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3daab-154">Save the file and enable the service.</span></span>

```bash
    systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="3daab-155">サービスを起動し、動作していることを確認します。</span><span class="sxs-lookup"><span data-stu-id="3daab-155">Start the service and verify that it is running.</span></span>

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

<span data-ttu-id="3daab-156">リバース プロキシを構成し、Kestrel を systemd で管理している状態で、Web アプリケーションは完全に構成されたことになり、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3daab-156">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="3daab-157">応答ヘッダーを調べるときにも、Kestrel がサービスを提供している ASP.NET Core アプリケーションが**サーバー**に表示されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-157">Inspecting the response headers, the **Server** still shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
    HTTP/1.1 200 OK
    Date: Tue, 11 Oct 2016 16:22:23 GMT
    Server: Kestrel
    Keep-Alive: timeout=5, max=98
    Connection: Keep-Alive
    Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="3daab-158">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="3daab-158">Viewing logs</span></span>

<span data-ttu-id="3daab-159">Kestrel を利用する Web アプリケーションは systemd で管理されるため、すべてのイベントとプロセスが記録され、中心的ジャーナルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-159">Since the web application using Kestrel is managed using systemd, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="3daab-160">ただし、このジャーナルには、systemd が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3daab-160">However, this journal includes all entries for all services and processes managed by systemd.</span></span> <span data-ttu-id="3daab-161">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3daab-161">To view the `kestrel-hellomvc.service` specific items, use the following command.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="3daab-162">さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="3daab-162">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
    sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="3daab-163">アプリケーションのセキュリティ強化</span><span class="sxs-lookup"><span data-stu-id="3daab-163">Securing our application</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="3daab-164">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="3daab-164">Configure firewall</span></span>

<span data-ttu-id="3daab-165">*Firewalld* は、ネットワーク ゾーンのサポートによってファイアウォールを管理する動的デーモンですが、iptables を使用してポートとパケット フィルターを管理することもできます。</span><span class="sxs-lookup"><span data-stu-id="3daab-165">*Firewalld* is a dynamic daemon to manage firewall with support for network zones, although you can still use iptables to manage ports and packet filtering.</span></span> <span data-ttu-id="3daab-166">既定で Firewalld がインストールされているはずですが、`yum` を使用してパッケージをインストールするか確認することができます。</span><span class="sxs-lookup"><span data-stu-id="3daab-166">Firewalld should be installed by default, `yum` can be used to install the package or verify.</span></span>

```bash
    sudo yum install firewalld -y
```

<span data-ttu-id="3daab-167">`firewalld` を使用して、アプリケーションに必要なポートのみを開くことができます。</span><span class="sxs-lookup"><span data-stu-id="3daab-167">Using `firewalld` you can open only the ports needed for the application.</span></span> <span data-ttu-id="3daab-168">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="3daab-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="3daab-169">次のコマンドを実行すると、これらのポートを開くように永続的に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-169">The following commands permanently sets these to open.</span></span>

```bash
    sudo firewall-cmd --add-port=80/tcp --permanent
    sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="3daab-170">ファイアウォール設定を再読み込みし、既定のゾーンで使用できるサービスとポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="3daab-170">Reload the firewall settings, and check the available services and ports in the default zone.</span></span> <span data-ttu-id="3daab-171">`firewall-cmd -h` を確認すると、使用できるサービスとポートを取得できます。</span><span class="sxs-lookup"><span data-stu-id="3daab-171">Options are available by inspecting `firewall-cmd -h`</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="3daab-172">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="3daab-172">SSL configuration</span></span>

<span data-ttu-id="3daab-173">SSL 用に Apache を構成するには、mod_ssl モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="3daab-173">To configure Apache for SSL, the mod_ssl module is used.</span></span>  <span data-ttu-id="3daab-174">これは、`httpd` モジュールをインストールしたときに最初にインストールされたモジュールです。</span><span class="sxs-lookup"><span data-stu-id="3daab-174">This was installed initially when we installed the `httpd` module.</span></span> <span data-ttu-id="3daab-175">見つからないかインストールされていなかった場合は、yum を使用して構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="3daab-175">If it was missed or not installed, use yum to add it to your configuration.</span></span>

```bash
    sudo yum install mod_ssl
```
<span data-ttu-id="3daab-176">SSL を強制するには、`mod_rewrite` をインストールします。</span><span class="sxs-lookup"><span data-stu-id="3daab-176">To enforce SSL, install `mod_rewrite`</span></span>

```bash
    sudo yum install mod_rewrite
```

<span data-ttu-id="3daab-177">この例で作成された `hellomvc.conf` ファイルは、再書き込みを有効にするだけでなく、HTTPS のために新しい **VirtualHost** セクションを追加できるように変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3daab-177">The `hellomvc.conf` file that was created for this example needs to be modified to enable the rewrite as well as adding the new **VirtualHost** section for HTTPS.</span></span>

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
> <span data-ttu-id="3daab-178">この例では、ローカルで生成された証明書を使用しています。</span><span class="sxs-lookup"><span data-stu-id="3daab-178">This example is using a locally generated certificate.</span></span> <span data-ttu-id="3daab-179">**SSLCertificateFile** は、ドメイン名のプライマリ証明書ファイルです。</span><span class="sxs-lookup"><span data-stu-id="3daab-179">**SSLCertificateFile** should be your primary certificate file for your domain name.</span></span> <span data-ttu-id="3daab-180">**SSLCertificateKeyFile** は、CSR を作成したときに生成されるキー ファイルです。</span><span class="sxs-lookup"><span data-stu-id="3daab-180">**SSLCertificateKeyFile** should be the key file generated when you created the CSR.</span></span> <span data-ttu-id="3daab-181">**SSLCertificateChainFile** は、証明機関から提供された中間証明書ファイル (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="3daab-181">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by your certificate authority</span></span>

<span data-ttu-id="3daab-182">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="3daab-182">Save the file, and test the configuration.</span></span>

```bash
    sudo service httpd configtest
```

<span data-ttu-id="3daab-183">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3daab-183">Restart Apache.</span></span>

```bash
    sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="3daab-184">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="3daab-184">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="3daab-185">その他のヘッダー</span><span class="sxs-lookup"><span data-stu-id="3daab-185">Additional Headers</span></span> 
<span data-ttu-id="3daab-186">悪意のある攻撃から防御するために、変更または追加する必要があるヘッダーがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="3daab-186">In order to secure against malicious attacks there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="3daab-187">必ず `mod_headers` モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3daab-187">Ensure that the `mod_headers` module is installed.</span></span>

```bash
    sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking"></a><span data-ttu-id="3daab-188">Apache をクリックジャッキングから保護する</span><span class="sxs-lookup"><span data-stu-id="3daab-188">Secure Apache from clickjacking</span></span>
<span data-ttu-id="3daab-189">クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。</span><span class="sxs-lookup"><span data-stu-id="3daab-189">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="3daab-190">クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。</span><span class="sxs-lookup"><span data-stu-id="3daab-190">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="3daab-191">X-FRAME-OPTIONS を利用し、サイトのセキュリティを強化します。</span><span class="sxs-lookup"><span data-stu-id="3daab-191">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="3daab-192">httpd.conf ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="3daab-192">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="3daab-193">行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` を追加し、ファイルを保存し、Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3daab-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"` and save the file, then restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="3daab-194">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="3daab-194">MIME-type sniffing</span></span>

<span data-ttu-id="3daab-195">このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、Internet Explorer で MIME スニッフィングが阻止されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-195">This header prevents Internet Explorer from MIME-sniffing a response away from the declared content-type as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="3daab-196">nosniff オプションを指定すると、サーバーがコンテンツを text/html と指定すれば、ブラウザーで text/html として表示されます。</span><span class="sxs-lookup"><span data-stu-id="3daab-196">With the nosniff option, if the server says the content is text/html, the browser will render it as text/html.</span></span>

<span data-ttu-id="3daab-197">httpd.conf ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="3daab-197">Edit the httpd.conf file.</span></span>

```bash
    sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="3daab-198">行 `Header set X-Content-Type-Options "nosniff"` を追加し、ファイルを保存し、Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3daab-198">Add the line `Header set X-Content-Type-Options "nosniff"` and save the file, then restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="3daab-199">負荷分散</span><span class="sxs-lookup"><span data-stu-id="3daab-199">Load Balancing</span></span> 

<span data-ttu-id="3daab-200">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3daab-200">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span>  <span data-ttu-id="3daab-201">ただし、単一障害点を持たないように、*mod_proxy_balancer* を使用し、VirtualHost を変更することで、Apache プロキシ サーバーの背後で複数インスタンスの Web アプリケーションを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3daab-201">However, in order to not have a single point of failure; using *mod_proxy_balancer* and modifying the VirtualHost would allow for managing mutliple instances of the web applications behind the Apache proxy server.</span></span>

```bash
    sudo yum install mod_proxy_balancer
```

<span data-ttu-id="3daab-202">構成ファイルで、追加インスタンスの `hellomvc` アプリケーションをセットアップしてポート 5001 で実行するようにしました。また、*byrequests* の負荷を分散するメンバーが 2 つのバランサー構成を使用して *Proxy* セクションを設定しました。</span><span class="sxs-lookup"><span data-stu-id="3daab-202">In the configuration file, an additional instance of the `hellomvc` app has been setup to run on port 5001 and the *Proxy* section has been set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="3daab-203">速度の制限</span><span class="sxs-lookup"><span data-stu-id="3daab-203">Rate Limits</span></span>
<span data-ttu-id="3daab-204">`htttpd` モジュールに含まれている `mod_ratelimit` を使用すると、クライアントの帯域幅を制限できます。</span><span class="sxs-lookup"><span data-stu-id="3daab-204">Using `mod_ratelimit`, which is included in the `htttpd` module you can limit the amount of bandwidth of clients.</span></span> 

```bash
    sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="3daab-205">次のファイル例では、ルート ディレクトリ以下の帯域幅を 600 KB/秒に制限しています。</span><span class="sxs-lookup"><span data-stu-id="3daab-205">The example file limits bandwidth as 600 KB/sec under the root location.</span></span>

```text
    <IfModule mod_ratelimit.c>
        <Location />
            SetOutputFilter RATE_LIMIT
            SetEnv rate-limit 600
        </Location>
    </IfModule>
```
