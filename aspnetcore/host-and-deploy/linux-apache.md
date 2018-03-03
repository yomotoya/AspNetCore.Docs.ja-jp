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
ms.openlocfilehash: b11bc811b6aefce22b60a28afd72c2a2d0b26955
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="49799-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="49799-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="49799-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="49799-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="49799-105">このガイドを使用して設定する方法を学習[Apache](https://httpd.apache.org/)にリバース プロキシ サーバーとして[CentOS 7](https://www.centos.org/)で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="49799-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="49799-106">[Mod_proxy 拡張子](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)関連モジュールが、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="49799-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49799-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="49799-107">Prerequisites</span></span>

1. <span data-ttu-id="49799-108">Sudo 権限を持つ標準ユーザー アカウントを使用して CentOS 7 を実行しているサーバー</span><span class="sxs-lookup"><span data-stu-id="49799-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="49799-109">ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="49799-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="49799-110">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="49799-110">Publish the app</span></span>

<span data-ttu-id="49799-111">アプリを発行すると、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 ランタイムのリリースの構成で (`centos.7-x64`)。</span><span class="sxs-lookup"><span data-stu-id="49799-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="49799-112">内容をコピー、 *bin/Release/netcoreapp2.0/centos.7-x64/publish* SCP、FTP、またはその他のファイルの転送方法を使用してサーバーのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="49799-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="49799-113">運用展開シナリオでは、継続的インテグレーション ワークフローは、アプリを発行し、サーバーへの資産のコピーの作業を行います。</span><span class="sxs-lookup"><span data-stu-id="49799-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="49799-114">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="49799-114">Configure a proxy server</span></span>

<span data-ttu-id="49799-115">リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。</span><span class="sxs-lookup"><span data-stu-id="49799-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="49799-116">リバース プロキシは、HTTP 要求を終了し、ASP.NET アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="49799-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="49799-117">プロキシ サーバーは、要求を満たせません自体ではなく別のサーバーにクライアント要求を転送する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="49799-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="49799-118">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="49799-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="49799-119">このガイドでは、Apache は Kestrel ASP.NET Core アプリケーションが機能していること、同じサーバーで実行されているリバース プロキシとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="49799-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="49799-120">要求は、リバース プロキシによって転送される、ためから転送されるヘッダー ミドルウェアを使用して、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="49799-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="49799-121">ミドルウェアの更新プログラム、`Request.Scheme`を使用して、`X-Forwarded-Proto`ヘッダー、そのリダイレクト Uri とその他のセキュリティ ポリシーが正常に動作するようにします。</span><span class="sxs-lookup"><span data-stu-id="49799-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="49799-122">認証ミドルウェアの任意の型を使用する場合、転送ヘッダー ミドルウェアが最初に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="49799-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="49799-123">この順序により、認証ミドルウェアがヘッダーの値を使用して正しくリダイレクト Uri を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="49799-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="49799-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="49799-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="49799-125">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッドで`Startup.Configure`呼び出す前に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)または類似の認証スキームのミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="49799-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="49799-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="49799-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="49799-127">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)と[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)または同様の認証スキームミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="49799-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="49799-128">ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)は指定した、ミドルウェアに転送する既定のヘッダーが`None`です。</span><span class="sxs-lookup"><span data-stu-id="49799-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="49799-129">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="49799-129">Install Apache</span></span>

<span data-ttu-id="49799-130">CentOS パッケージを最新の安定したバージョンに更新プログラム:</span><span class="sxs-lookup"><span data-stu-id="49799-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="49799-131">1 つの CentOS に Apache web サーバーをインストール`yum`コマンド。</span><span class="sxs-lookup"><span data-stu-id="49799-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="49799-132">コマンドの実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="49799-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="49799-133">この例では、出力は、CentOS 7 バージョンが 64 ビットであるために httpd.86_64 を反映します。</span><span class="sxs-lookup"><span data-stu-id="49799-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="49799-134">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="49799-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span> 

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="49799-135">Apache をリバース プロキシとして構成する</span><span class="sxs-lookup"><span data-stu-id="49799-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="49799-136">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="49799-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="49799-137">ファイルのいずれか、 *.conf*拡張機能が、モジュール構成ファイルでだけでなくアルファベット順に処理される`/etc/httpd/conf.modules.d/`、ファイルのモジュールを読み込むために必要な構成が含まれています。</span><span class="sxs-lookup"><span data-stu-id="49799-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="49799-138">という名前のアプリの構成ファイルを作成`hellomvc.conf`:</span><span class="sxs-lookup"><span data-stu-id="49799-138">Create a configuration file for the app named `hellomvc.conf`:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="49799-139">**VirtualHost**ノードは、サーバー上の 1 つまたは複数のファイルに複数回を使用できます。</span><span class="sxs-lookup"><span data-stu-id="49799-139">The **VirtualHost** node can appear multiple times in one or more files on a server.</span></span> <span data-ttu-id="49799-140">**VirtualHost**がポート 80 を使用して任意の IP アドレスでリッスンするように設定します。</span><span class="sxs-lookup"><span data-stu-id="49799-140">**VirtualHost** is set to listen on any IP address using port 80.</span></span> <span data-ttu-id="49799-141">次の 2 行は、5000 のポートで 127.0.0.1 のサーバーにルートにプロキシ要求に設定されます。</span><span class="sxs-lookup"><span data-stu-id="49799-141">The next two lines are set to proxy requests at the root to the server at 127.0.0.1 on port 5000.</span></span> <span data-ttu-id="49799-142">双方向通信の*ProxyPass*と*ProxyPassReverse*が必要です。</span><span class="sxs-lookup"><span data-stu-id="49799-142">For bi-directional communication, *ProxyPass* and *ProxyPassReverse* are required.</span></span>

<span data-ttu-id="49799-143">ごとにログ記録を構成できる**VirtualHost**を使用して**ErrorLog**と**CustomLog**ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="49799-143">Logging can be configured per **VirtualHost** using **ErrorLog** and **CustomLog** directives.</span></span> <span data-ttu-id="49799-144">**ErrorLog** 、サーバーが、エラーをログの場所と**CustomLog**ファイル名とログ ファイルの形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="49799-144">**ErrorLog** is the location where the server logs errors, and **CustomLog** sets the filename and format of log file.</span></span> <span data-ttu-id="49799-145">この場合、要求の情報が記録される場所はします。</span><span class="sxs-lookup"><span data-stu-id="49799-145">In this case, this is where request information is logged.</span></span> <span data-ttu-id="49799-146">要求ごとに行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="49799-146">There's one line for each request.</span></span>

<span data-ttu-id="49799-147">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="49799-147">Save the file and test the configuration.</span></span> <span data-ttu-id="49799-148">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="49799-148">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="49799-149">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="49799-149">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="49799-150">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="49799-150">Monitoring the app</span></span>

<span data-ttu-id="49799-151">Apache がへの要求を転送するセットアップでは今すぐ`http://localhost:80`で Kestrel で実行されている ASP.NET Core アプリケーションに`http://127.0.0.1:5000`です。</span><span class="sxs-lookup"><span data-stu-id="49799-151">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="49799-152">ただし、Apache は Kestrel プロセスを管理するセットアップをされていません。</span><span class="sxs-lookup"><span data-stu-id="49799-152">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="49799-153">使用して*systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="49799-153">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="49799-154">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="49799-154">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="49799-155">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="49799-155">Create the service file</span></span>

<span data-ttu-id="49799-156">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="49799-156">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="49799-157">アプリのサービス ファイルの例:</span><span class="sxs-lookup"><span data-stu-id="49799-157">An example service file for the app:</span></span>

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
> <span data-ttu-id="49799-158">**ユーザー** &mdash;場合、ユーザー *apache*使用されていない、構成によってユーザー必要があります最初に作成してファイルの適切な所有権を指定します。</span><span class="sxs-lookup"><span data-stu-id="49799-158">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="49799-159">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="49799-159">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="49799-160">サービスを開始し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="49799-160">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="49799-161">リバース プロキシが構成されているとを通じて管理 Kestrel *systemd*、web アプリが完全に構成されているし、ローカルのコンピューター上のブラウザーからアクセスできる`http://localhost`です。</span><span class="sxs-lookup"><span data-stu-id="49799-161">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="49799-162">応答ヘッダーの検査、**サーバー**ヘッダーは、ASP.NET Core アプリケーションが Kestrel によって処理されることを示します。</span><span class="sxs-lookup"><span data-stu-id="49799-162">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="49799-163">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="49799-163">Viewing logs</span></span>

<span data-ttu-id="49799-164">Web アプリから Kestrel を使用して管理を使用して*systemd*イベントとプロセスは、一元的な履歴に記録されます。</span><span class="sxs-lookup"><span data-stu-id="49799-164">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="49799-165">ただし、このジャーナルには、サービスとプロセスによって管理されるすべてのエントリが含まれます*systemd*です。</span><span class="sxs-lookup"><span data-stu-id="49799-165">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="49799-166">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="49799-166">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="49799-167">時間のフィルタ リング、コマンドを使用して時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="49799-167">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="49799-168">たとえば、使用して`--since today`を現在の日付のフィルター処理または`--until 1 hour ago`前の 1 時間のエントリを参照します。</span><span class="sxs-lookup"><span data-stu-id="49799-168">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="49799-169">詳細については、次を参照してください。、 [journalctl についてマニュアル](https://www.unix.com/man-page/centos/1/journalctl/)です。</span><span class="sxs-lookup"><span data-stu-id="49799-169">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="49799-170">アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="49799-170">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="49799-171">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="49799-171">Configure firewall</span></span>

<span data-ttu-id="49799-172">*Firewalld*動的デーモンでネットワーク ゾーンのサポートされたファイアウォールを管理します。</span><span class="sxs-lookup"><span data-stu-id="49799-172">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="49799-173">ポートとパケット フィルタ リングは、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="49799-173">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="49799-174">*Firewalld*既定でインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="49799-174">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="49799-175">`yum` パッケージをインストールまたはがインストールされていることを確認するために使用します。</span><span class="sxs-lookup"><span data-stu-id="49799-175">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="49799-176">使用して`firewalld`を開くには、アプリに必要なポートのみです。</span><span class="sxs-lookup"><span data-stu-id="49799-176">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="49799-177">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="49799-177">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="49799-178">次のコマンドでは、ポート 80 と 443 を開くには完全に設定します。</span><span class="sxs-lookup"><span data-stu-id="49799-178">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="49799-179">ファイアウォールの設定を再読み込みされます。</span><span class="sxs-lookup"><span data-stu-id="49799-179">Reload the firewall settings.</span></span> <span data-ttu-id="49799-180">使用可能なサービスと、既定のゾーン内のポートを確認してください。</span><span class="sxs-lookup"><span data-stu-id="49799-180">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="49799-181">検査することによってオプションがある`firewall-cmd -h`です。</span><span class="sxs-lookup"><span data-stu-id="49799-181">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="49799-182">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="49799-182">SSL configuration</span></span>

<span data-ttu-id="49799-183">Ssl は、Apache を構成する、 *mod_ssl*モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="49799-183">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="49799-184">ときに、 *httpd*モジュールがインストールされている、 *mod_ssl*モジュールもインストールするとします。</span><span class="sxs-lookup"><span data-stu-id="49799-184">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="49799-185">インストールされている場合を使用して`yum`構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="49799-185">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="49799-186">SSL を強制するのには、インストール、 `mod_rewrite` URL 書き換えを有効にするモジュール。</span><span class="sxs-lookup"><span data-stu-id="49799-186">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="49799-187">変更、 *hellomvc.conf* URL 書き換えを有効にして、ポート 443 での通信をセキュリティで保護するファイル。</span><span class="sxs-lookup"><span data-stu-id="49799-187">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="49799-188">この例は、ローカルに生成された証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="49799-188">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="49799-189">**SSLCertificateFile**ドメイン名のプライマリ証明書ファイルでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="49799-189">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="49799-190">**SSLCertificateKeyFile**生成するか、キー ファイル CSR を作成します。</span><span class="sxs-lookup"><span data-stu-id="49799-190">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="49799-191">**SSLCertificateChainFile** (存在する場合)、中間証明書ファイルをする必要があります証明機関から提供されています。</span><span class="sxs-lookup"><span data-stu-id="49799-191">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="49799-192">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="49799-192">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="49799-193">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="49799-193">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="49799-194">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="49799-194">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="49799-195">追加のヘッダー</span><span class="sxs-lookup"><span data-stu-id="49799-195">Additional headers</span></span>

<span data-ttu-id="49799-196">悪意のある攻撃からセキュリティで保護するためには、いくつかのヘッダーをする必要がありますか、変更または追加できます。</span><span class="sxs-lookup"><span data-stu-id="49799-196">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="49799-197">いることを確認、`mod_headers`モジュールがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="49799-197">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="49799-198">Clickjacking の攻撃からセキュリティで保護された Apache</span><span class="sxs-lookup"><span data-stu-id="49799-198">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="49799-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)とも呼ばれる、 *UI redress 攻撃*、悪意のある攻撃は、ここで web サイトの訪問者は、騙されてクリックしてリンクやボタンは別のページをしている現在のページです。</span><span class="sxs-lookup"><span data-stu-id="49799-199">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="49799-200">使用して`X-FRAME-OPTIONS`サイトをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="49799-200">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="49799-201">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="49799-201">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="49799-202">行を追加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`です。</span><span class="sxs-lookup"><span data-stu-id="49799-202">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="49799-203">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="49799-203">Save the file.</span></span> <span data-ttu-id="49799-204">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="49799-204">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="49799-205">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="49799-205">MIME-type sniffing</span></span>

<span data-ttu-id="49799-206">`X-Content-Type-Options`ヘッダーにより、Internet Explorer から*MIME スニッフィング*(ファイルの決定`Content-Type`ファイルの内容から)。</span><span class="sxs-lookup"><span data-stu-id="49799-206">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="49799-207">サーバーを設定する場合、`Content-Type`ヘッダーを`text/html`で、 `nosniff` Internet Explorer、オプションのセットとして内容を表示する`text/html`ファイルの内容に関係なく。</span><span class="sxs-lookup"><span data-stu-id="49799-207">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="49799-208">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="49799-208">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="49799-209">行を追加`Header set X-Content-Type-Options "nosniff"`です。</span><span class="sxs-lookup"><span data-stu-id="49799-209">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="49799-210">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="49799-210">Save the file.</span></span> <span data-ttu-id="49799-211">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="49799-211">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="49799-212">負荷分散</span><span class="sxs-lookup"><span data-stu-id="49799-212">Load Balancing</span></span> 

<span data-ttu-id="49799-213">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="49799-213">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="49799-214">ためにないの単一障害点です。使用して*mod_proxy_balancer*を変更して、 **VirtualHost**なります Apache プロキシ サーバーの背後に web アプリの複数のインスタンスを管理するためです。</span><span class="sxs-lookup"><span data-stu-id="49799-214">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="49799-215">追加のインスタンスの次に示す構成ファイルで、`hellomvc`アプリは 5001 ポート上で実行するセットアップです。</span><span class="sxs-lookup"><span data-stu-id="49799-215">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="49799-216">*プロキシ*セクションは、負荷を分散する 2 つのメンバー バランサー構成で設定が*byrequests*です。</span><span class="sxs-lookup"><span data-stu-id="49799-216">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="49799-217">速度の制限</span><span class="sxs-lookup"><span data-stu-id="49799-217">Rate Limits</span></span>
<span data-ttu-id="49799-218">使用して*mod_ratelimit*に含まれている、 *httpd*モジュールのクライアントの帯域幅は制限されることができます。</span><span class="sxs-lookup"><span data-stu-id="49799-218">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="49799-219">ファイルの例では、600 KB/秒 ルートの場所として帯域幅を制限します。</span><span class="sxs-lookup"><span data-stu-id="49799-219">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
