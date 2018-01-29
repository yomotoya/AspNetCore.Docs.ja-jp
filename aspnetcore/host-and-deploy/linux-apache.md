---
title: "Apache 搭載の Linux で ASP.NET Core をホストする"
description: "Kestrel で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする CentOS にリバース プロキシ サーバーとして Apache を設定する方法を説明します。"
author: spboyer
ms.author: spboyer
manager: wpickett
ms.custom: mvc
ms.date: 10/19/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/linux-apache
ms.openlocfilehash: caa8cce53b3982815f1eab75792a10ec390f3257
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="6c3ba-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="6c3ba-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="6c3ba-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="6c3ba-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="6c3ba-105">このガイドを使用して設定する方法を学習[Apache](https://httpd.apache.org/)にリバース プロキシ サーバーとして[CentOS 7](https://www.centos.org/)で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="6c3ba-106">[Mod_proxy 拡張子](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)関連モジュールが、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6c3ba-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="6c3ba-107">Prerequisites</span></span>

1. <span data-ttu-id="6c3ba-108">Sudo 権限を持つ標準ユーザー アカウントを使用して CentOS 7 を実行しているサーバー</span><span class="sxs-lookup"><span data-stu-id="6c3ba-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="6c3ba-109">ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="6c3ba-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="6c3ba-110">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="6c3ba-110">Publish the app</span></span>

<span data-ttu-id="6c3ba-111">アプリを発行すると、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 ランタイムのリリースの構成で (`centos.7-x64`)。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="6c3ba-112">内容をコピー、 *bin/Release/netcoreapp2.0/centos.7-x64/publish* SCP、FTP、またはその他のファイルの転送方法を使用してサーバーのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="6c3ba-113">運用展開シナリオでは、継続的インテグレーション ワークフローは、アプリを発行し、サーバーへの資産のコピーの作業を行います。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="6c3ba-114">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="6c3ba-114">Configure a proxy server</span></span>

<span data-ttu-id="6c3ba-115">リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="6c3ba-116">リバース プロキシは、HTTP 要求を終了し、ASP.NET アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="6c3ba-117">プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-117">A proxy server is one which forwards client requests to another server instead of fulfilling them itself.</span></span> <span data-ttu-id="6c3ba-118">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="6c3ba-119">このガイドでは、Apache は Kestrel ASP.NET Core アプリケーションが機能していること、同じサーバーで実行されているリバース プロキシとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

### <a name="install-apache"></a><span data-ttu-id="6c3ba-120">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="6c3ba-120">Install Apache</span></span>

<span data-ttu-id="6c3ba-121">CentOS パッケージを最新の安定したバージョンに更新プログラム:</span><span class="sxs-lookup"><span data-stu-id="6c3ba-121">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="6c3ba-122">1 つの CentOS に Apache web サーバーをインストール`yum`コマンド。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-122">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="6c3ba-123">コマンドの実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="6c3ba-123">Sample output after running the command:</span></span>

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
> <span data-ttu-id="6c3ba-124">この例では、出力は、CentOS 7 バージョンが 64 ビットであるために httpd.86_64 を反映します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-124">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="6c3ba-125">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-125">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="6c3ba-126">Apache をリバース プロキシとして構成する</span><span class="sxs-lookup"><span data-stu-id="6c3ba-126">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="6c3ba-127">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-127">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="6c3ba-128">ファイルのいずれか、 *.conf*拡張機能が、モジュール構成ファイルでだけでなくアルファベット順に処理される`/etc/httpd/conf.modules.d/`、ファイルのモジュールを読み込むために必要な構成が含まれています。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-128">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="6c3ba-129">という名前のアプリの構成ファイルを作成`hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="6c3ba-129">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="6c3ba-130">**VirtualHost**ノードは、サーバー上の 1 つまたは複数のファイルに複数回を使用できます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-130">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="6c3ba-131">**VirtualHost**がポート 80 を使用して任意の IP アドレスでリッスンするように設定します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-131">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="6c3ba-132">次の 2 行は、5000 のポートで 127.0.0.1 のサーバーにルートにプロキシ要求に設定されます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-132">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="6c3ba-133">双方向通信の*ProxyPass*と*ProxyPassReverse*が必要です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-133">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="6c3ba-134">ごとにログ記録を構成できる**VirtualHost**を使用して**ErrorLog**と**CustomLog**ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-134">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="6c3ba-135">**ErrorLog** 、サーバーが、エラーをログの場所と**CustomLog**ファイル名とログ ファイルの形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-135">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="6c3ba-136">この場合、要求の情報が記録される場所はします。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-136">In this case, this is where request information is logged.</span></span> <span data-ttu-id="6c3ba-137">要求ごとに行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-137">There's one line for each request.</span></span>

<span data-ttu-id="6c3ba-138">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-138">Save the file and test the configuration.</span></span> <span data-ttu-id="6c3ba-139">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-139">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="6c3ba-140">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-140">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="6c3ba-141">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="6c3ba-141">Monitoring the app</span></span>

<span data-ttu-id="6c3ba-142">Apache がへの要求を転送するセットアップでは今すぐ`http://localhost:80`で Kestrel で実行されている ASP.NET Core アプリケーションに`http://127.0.0.1:5000`です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-142">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="6c3ba-143">ただし、Apache は Kestrel プロセスを管理するセットアップをされていません。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-143">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="6c3ba-144">使用して*systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-144">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="6c3ba-145">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-145">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="6c3ba-146">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="6c3ba-146">Create the service file</span></span>

<span data-ttu-id="6c3ba-147">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-147">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="6c3ba-148">アプリのサービス ファイルの例:</span><span class="sxs-lookup"><span data-stu-id="6c3ba-148">An example service file for the app:</span></span>

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
> <span data-ttu-id="6c3ba-149">**ユーザー** &mdash;場合、ユーザー *apache*使用されていない、構成によってユーザー必要があります最初に作成してファイルの適切な所有権を指定します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-149">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="6c3ba-150">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-150">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="6c3ba-151">サービスを開始し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-151">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="6c3ba-152">リバース プロキシが構成されているとを通じて管理 Kestrel *systemd*、web アプリが完全に構成されているし、ローカルのコンピューター上のブラウザーからアクセスできる`http://localhost`です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-152">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="6c3ba-153">応答ヘッダーの検査、**サーバー**ヘッダーは、ASP.NET Core アプリケーションが Kestrel によって処理されることを示します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-153">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="6c3ba-154">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="6c3ba-154">Viewing logs</span></span>

<span data-ttu-id="6c3ba-155">Web アプリから Kestrel を使用して管理を使用して*systemd*イベントとプロセスは、一元的な履歴に記録されます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-155">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="6c3ba-156">ただし、このジャーナルには、サービスとプロセスによって管理されるすべてのエントリが含まれます*systemd*です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-156">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="6c3ba-157">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-157">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="6c3ba-158">時間のフィルタ リング、コマンドを使用して時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-158">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="6c3ba-159">たとえば、使用して`--since today`を現在の日付のフィルター処理または`--until 1 hour ago`前の 1 時間のエントリを参照します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-159">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="6c3ba-160">詳細については、次を参照してください。、 [journalctl についてマニュアル](https://www.unix.com/man-page/centos/1/journalctl/)です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-160">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="6c3ba-161">アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-161">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="6c3ba-162">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="6c3ba-162">Configure firewall</span></span>

<span data-ttu-id="6c3ba-163">*Firewalld*動的デーモンでネットワーク ゾーンのサポートされたファイアウォールを管理します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-163">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="6c3ba-164">ポートとパケット フィルタ リングは、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-164">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="6c3ba-165">*Firewalld*既定でインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-165">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="6c3ba-166">`yum`パッケージをインストールまたはがインストールされていることを確認するために使用します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-166">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="6c3ba-167">使用して`firewalld`を開くには、アプリに必要なポートのみです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-167">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="6c3ba-168">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-168">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="6c3ba-169">次のコマンドでは、ポート 80 と 443 を開くには完全に設定します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-169">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="6c3ba-170">ファイアウォールの設定を再読み込みされます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-170">Reload the firewall settings.</span></span> <span data-ttu-id="6c3ba-171">使用可能なサービスと、既定のゾーン内のポートを確認してください。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-171">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="6c3ba-172">検査することによってオプションがある`firewall-cmd -h`です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-172">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="6c3ba-173">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="6c3ba-173">SSL configuration</span></span>

<span data-ttu-id="6c3ba-174">Ssl は、Apache を構成する、 *mod_ssl*モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-174">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="6c3ba-175">ときに、 *httpd*モジュールがインストールされている、 *mod_ssl*モジュールもインストールするとします。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-175">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="6c3ba-176">インストールされている場合を使用して`yum`構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-176">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="6c3ba-177">SSL を強制するのには、インストール、 `mod_rewrite` URL 書き換えを有効にするモジュール。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-177">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="6c3ba-178">変更、 *hellomvc.conf* URL 書き換えを有効にして、ポート 443 での通信をセキュリティで保護するファイル。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-178">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="6c3ba-179">この例は、ローカルに生成された証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-179">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="6c3ba-180">**SSLCertificateFile**ドメイン名のプライマリ証明書ファイルでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-180">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="6c3ba-181">**SSLCertificateKeyFile**生成するか、キー ファイル CSR を作成します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-181">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="6c3ba-182">**SSLCertificateChainFile** (存在する場合)、中間証明書ファイルをする必要があります証明機関から提供されています。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-182">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="6c3ba-183">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-183">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="6c3ba-184">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-184">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="6c3ba-185">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="6c3ba-185">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="6c3ba-186">追加のヘッダー</span><span class="sxs-lookup"><span data-stu-id="6c3ba-186">Additional headers</span></span>

<span data-ttu-id="6c3ba-187">悪意のある攻撃からセキュリティで保護するためには、いくつかのヘッダーをする必要がありますか、変更または追加できます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-187">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="6c3ba-188">いることを確認、`mod_headers`モジュールがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-188">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="6c3ba-189">Clickjacking の攻撃からセキュリティで保護された Apache</span><span class="sxs-lookup"><span data-stu-id="6c3ba-189">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="6c3ba-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)とも呼ばれる、 *UI redress 攻撃*、悪意のある攻撃は、ここで web サイトの訪問者は、騙されてクリックしてリンクやボタンは別のページをしている現在のページです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-190">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="6c3ba-191">使用して`X-FRAME-OPTIONS`サイトをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-191">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="6c3ba-192">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-192">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="6c3ba-193">行を追加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-193">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="6c3ba-194">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-194">Save the file.</span></span> <span data-ttu-id="6c3ba-195">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-195">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="6c3ba-196">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="6c3ba-196">MIME-type sniffing</span></span>

<span data-ttu-id="6c3ba-197">`X-Content-Type-Options`ヘッダーにより、Internet Explorer から*MIME スニッフィング*(ファイルの決定`Content-Type`ファイルの内容から)。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-197">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determing a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="6c3ba-198">サーバーを設定する場合、`Content-Type`ヘッダーを`text/html`で、 `nosniff` Internet Explorer、オプションのセットとして内容を表示する`text/html`ファイルの内容に関係なく。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-198">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="6c3ba-199">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-199">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="6c3ba-200">行を追加`Header set X-Content-Type-Options "nosniff"`です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-200">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="6c3ba-201">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-201">Save the file.</span></span> <span data-ttu-id="6c3ba-202">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-202">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="6c3ba-203">負荷分散</span><span class="sxs-lookup"><span data-stu-id="6c3ba-203">Load Balancing</span></span> 

<span data-ttu-id="6c3ba-204">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-204">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="6c3ba-205">ためにないの単一障害点です。使用して*mod_proxy_balancer*を変更して、 **VirtualHost**なります Apache プロキシ サーバーの背後に web アプリの複数のインスタンスを管理するためです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-205">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing mutliple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="6c3ba-206">追加のインスタンスの次に示す構成ファイルで、`hellomvc`アプリは 5001 ポート上で実行するセットアップです。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-206">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="6c3ba-207">*プロキシ*セクションは、負荷を分散する 2 つのメンバー バランサー構成で設定が*byrequests*です。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-207">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="6c3ba-208">速度の制限</span><span class="sxs-lookup"><span data-stu-id="6c3ba-208">Rate Limits</span></span>
<span data-ttu-id="6c3ba-209">使用して*mod_ratelimit*に含まれている、 *httpd*モジュールのクライアントの帯域幅は制限されることができます。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-209">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="6c3ba-210">ファイルの例では、600 KB/秒 ルートの場所として帯域幅を制限します。</span><span class="sxs-lookup"><span data-stu-id="6c3ba-210">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
