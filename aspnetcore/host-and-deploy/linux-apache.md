---
title: "Apache 搭載の Linux で ASP.NET Core をホストする"
description: "Kestrel で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする CentOS にリバース プロキシ サーバーとして Apache を設定する方法を説明します。"
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 033adddc586b60c9f7453df5434617aa838737f8
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="00589-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="00589-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="00589-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="00589-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="00589-105">このガイドを使用して設定する方法を学習[Apache](https://httpd.apache.org/)にリバース プロキシ サーバーとして[CentOS 7](https://www.centos.org/)で実行されている ASP.NET Core web アプリへの HTTP トラフィックをリダイレクトする[Kestrel](xref:fundamentals/servers/kestrel)です。</span><span class="sxs-lookup"><span data-stu-id="00589-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="00589-106">[Mod_proxy 拡張子](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)関連モジュールが、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="00589-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00589-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="00589-107">Prerequisites</span></span>

1. <span data-ttu-id="00589-108">Sudo 権限を持つ標準ユーザー アカウントを使用して CentOS 7 を実行しているサーバー</span><span class="sxs-lookup"><span data-stu-id="00589-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="00589-109">ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="00589-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="00589-110">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="00589-110">Publish the app</span></span>

<span data-ttu-id="00589-111">アプリを発行すると、[自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd)CentOS 7 ランタイムのリリースの構成で (`centos.7-x64`)。</span><span class="sxs-lookup"><span data-stu-id="00589-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="00589-112">内容をコピー、 *bin/Release/netcoreapp2.0/centos.7-x64/publish* SCP、FTP、またはその他のファイルの転送方法を使用してサーバーのフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="00589-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="00589-113">運用展開シナリオでは、継続的インテグレーション ワークフローは、アプリを発行し、サーバーへの資産のコピーの作業を行います。</span><span class="sxs-lookup"><span data-stu-id="00589-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="00589-114">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="00589-114">Configure a proxy server</span></span>

<span data-ttu-id="00589-115">リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。</span><span class="sxs-lookup"><span data-stu-id="00589-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="00589-116">リバース プロキシは、HTTP 要求を終了し、ASP.NET アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="00589-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="00589-117">プロキシ サーバーは、要求を満たせません自体ではなく別のサーバーにクライアント要求を転送する 1 つです。</span><span class="sxs-lookup"><span data-stu-id="00589-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="00589-118">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="00589-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="00589-119">このガイドでは、Apache は Kestrel ASP.NET Core アプリケーションが機能していること、同じサーバーで実行されているリバース プロキシとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="00589-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="00589-120">要求は、リバース プロキシによって転送される、ためから転送されるヘッダー ミドルウェアを使用して、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="00589-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="00589-121">ミドルウェアの更新プログラム、`Request.Scheme`を使用して、`X-Forwarded-Proto`ヘッダー、そのリダイレクト Uri とその他のセキュリティ ポリシーが正常に動作するようにします。</span><span class="sxs-lookup"><span data-stu-id="00589-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="00589-122">認証ミドルウェアの任意の型を使用する場合、転送ヘッダー ミドルウェアが最初に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="00589-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="00589-123">この順序により、認証ミドルウェアがヘッダーの値を使用して正しくリダイレクト Uri を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="00589-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="00589-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="00589-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="00589-125">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッドで`Startup.Configure`呼び出す前に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)または類似の認証スキームのミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="00589-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="00589-126">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="00589-126">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="00589-127">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)と[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)または同様の認証スキームミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="00589-127">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware:</span></span>

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

<span data-ttu-id="00589-128">ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)は指定した、ミドルウェアに転送する既定のヘッダーが`None`です。</span><span class="sxs-lookup"><span data-stu-id="00589-128">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

### <a name="install-apache"></a><span data-ttu-id="00589-129">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="00589-129">Install Apache</span></span>

<span data-ttu-id="00589-130">CentOS パッケージを最新の安定したバージョンに更新プログラム:</span><span class="sxs-lookup"><span data-stu-id="00589-130">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="00589-131">1 つの CentOS に Apache web サーバーをインストール`yum`コマンド。</span><span class="sxs-lookup"><span data-stu-id="00589-131">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="00589-132">コマンドの実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="00589-132">Sample output after running the command:</span></span>

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
> <span data-ttu-id="00589-133">この例では、出力は、CentOS 7 バージョンが 64 ビットであるために httpd.86_64 を反映します。</span><span class="sxs-lookup"><span data-stu-id="00589-133">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="00589-134">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="00589-134">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="00589-135">Apache をリバース プロキシとして構成する</span><span class="sxs-lookup"><span data-stu-id="00589-135">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="00589-136">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="00589-136">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="00589-137">ファイルのいずれか、 *.conf*拡張機能が、モジュール構成ファイルでだけでなくアルファベット順に処理される`/etc/httpd/conf.modules.d/`、ファイルのモジュールを読み込むために必要な構成が含まれています。</span><span class="sxs-lookup"><span data-stu-id="00589-137">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="00589-138">という名前の構成ファイルを作成する*hellomvc.conf*アプリの。</span><span class="sxs-lookup"><span data-stu-id="00589-138">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

```
<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="00589-139">`VirtualHost`ブロックは、サーバー上の 1 つまたは複数のファイルに複数回を表示することができます。</span><span class="sxs-lookup"><span data-stu-id="00589-139">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="00589-140">上記の構成ファイルでは、Apache は、ポート 80 でパブリック トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="00589-140">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="00589-141">ドメイン`www.example.com`が提供される、および`*.example.com`エイリアスが同じ web サイトに解決されます。</span><span class="sxs-lookup"><span data-stu-id="00589-141">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="00589-142">参照してください[仮想ホストの名前に基づくサポート](https://httpd.apache.org/docs/current/vhosts/name-based.html)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="00589-142">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="00589-143">要求は、ポート 5000 127.0.0.1 のサーバーのルートにあるプロキシ。</span><span class="sxs-lookup"><span data-stu-id="00589-143">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="00589-144">双方向通信の`ProxyPass`と`ProxyPassReverse`が必要です。</span><span class="sxs-lookup"><span data-stu-id="00589-144">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="00589-145">適切なを指定する[ServerName ディレクティブ](https://httpd.apache.org/docs/current/mod/core.html#servername)で、 **VirtualHost**ブロックはセキュリティの脆弱性にアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="00589-145">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="00589-146">サブドメイン ワイルドカード バインド (たとえば、 `*.example.com`) 全体の親ドメインを制御する場合、このセキュリティ上のリスクは発生しません (to `*.com`、に対して脆弱である)。</span><span class="sxs-lookup"><span data-stu-id="00589-146">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="00589-147">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="00589-147">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="00589-148">ごとにログ記録を構成できる`VirtualHost`を使用して`ErrorLog`と`CustomLog`ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="00589-148">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="00589-149">`ErrorLog` サーバーが、エラーをログの場所と`CustomLog`ファイル名とログ ファイルの形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="00589-149">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="00589-150">この場合、要求の情報が記録される場所はします。</span><span class="sxs-lookup"><span data-stu-id="00589-150">In this case, this is where request information is logged.</span></span> <span data-ttu-id="00589-151">要求ごとに行が表示されます。</span><span class="sxs-lookup"><span data-stu-id="00589-151">There's one line for each request.</span></span>

<span data-ttu-id="00589-152">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="00589-152">Save the file and test the configuration.</span></span> <span data-ttu-id="00589-153">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="00589-153">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="00589-154">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="00589-154">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="00589-155">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="00589-155">Monitoring the app</span></span>

<span data-ttu-id="00589-156">Apache がへの要求を転送するセットアップでは今すぐ`http://localhost:80`で Kestrel で実行されている ASP.NET Core アプリケーションに`http://127.0.0.1:5000`です。</span><span class="sxs-lookup"><span data-stu-id="00589-156">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="00589-157">ただし、Apache は Kestrel プロセスを管理するセットアップをされていません。</span><span class="sxs-lookup"><span data-stu-id="00589-157">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="00589-158">使用して*systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="00589-158">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="00589-159">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="00589-159">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="00589-160">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="00589-160">Create the service file</span></span>

<span data-ttu-id="00589-161">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="00589-161">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="00589-162">アプリのサービス ファイルの例:</span><span class="sxs-lookup"><span data-stu-id="00589-162">An example service file for the app:</span></span>

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
> <span data-ttu-id="00589-163">**ユーザー** &mdash;場合、ユーザー *apache*使用されていない、構成によってユーザー必要があります最初に作成してファイルの適切な所有権を指定します。</span><span class="sxs-lookup"><span data-stu-id="00589-163">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="00589-164">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="00589-164">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="00589-165">サービスを開始し、実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="00589-165">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="00589-166">リバース プロキシが構成されているとを通じて管理 Kestrel *systemd*、web アプリが完全に構成されているし、ローカルのコンピューター上のブラウザーからアクセスできる`http://localhost`です。</span><span class="sxs-lookup"><span data-stu-id="00589-166">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="00589-167">応答ヘッダーの検査、**サーバー**ヘッダーは、ASP.NET Core アプリケーションが Kestrel によって処理されることを示します。</span><span class="sxs-lookup"><span data-stu-id="00589-167">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="00589-168">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="00589-168">Viewing logs</span></span>

<span data-ttu-id="00589-169">Web アプリから Kestrel を使用して管理を使用して*systemd*イベントとプロセスは、一元的な履歴に記録されます。</span><span class="sxs-lookup"><span data-stu-id="00589-169">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="00589-170">ただし、このジャーナルには、サービスとプロセスによって管理されるすべてのエントリが含まれます*systemd*です。</span><span class="sxs-lookup"><span data-stu-id="00589-170">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="00589-171">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="00589-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="00589-172">時間のフィルタ リング、コマンドを使用して時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="00589-172">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="00589-173">たとえば、使用して`--since today`を現在の日付のフィルター処理または`--until 1 hour ago`前の 1 時間のエントリを参照します。</span><span class="sxs-lookup"><span data-stu-id="00589-173">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="00589-174">詳細については、次を参照してください。、 [journalctl についてマニュアル](https://www.unix.com/man-page/centos/1/journalctl/)です。</span><span class="sxs-lookup"><span data-stu-id="00589-174">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="00589-175">アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="00589-175">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="00589-176">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="00589-176">Configure firewall</span></span>

<span data-ttu-id="00589-177">*Firewalld*動的デーモンでネットワーク ゾーンのサポートされたファイアウォールを管理します。</span><span class="sxs-lookup"><span data-stu-id="00589-177">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="00589-178">ポートとパケット フィルタ リングは、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="00589-178">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="00589-179">*Firewalld*既定でインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="00589-179">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="00589-180">`yum` パッケージをインストールまたはがインストールされていることを確認するために使用します。</span><span class="sxs-lookup"><span data-stu-id="00589-180">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="00589-181">使用して`firewalld`を開くには、アプリに必要なポートのみです。</span><span class="sxs-lookup"><span data-stu-id="00589-181">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="00589-182">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="00589-182">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="00589-183">次のコマンドでは、ポート 80 と 443 を開くには完全に設定します。</span><span class="sxs-lookup"><span data-stu-id="00589-183">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="00589-184">ファイアウォールの設定を再読み込みされます。</span><span class="sxs-lookup"><span data-stu-id="00589-184">Reload the firewall settings.</span></span> <span data-ttu-id="00589-185">使用可能なサービスと、既定のゾーン内のポートを確認してください。</span><span class="sxs-lookup"><span data-stu-id="00589-185">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="00589-186">検査することによってオプションがある`firewall-cmd -h`です。</span><span class="sxs-lookup"><span data-stu-id="00589-186">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="00589-187">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="00589-187">SSL configuration</span></span>

<span data-ttu-id="00589-188">Ssl は、Apache を構成する、 *mod_ssl*モジュールを使用します。</span><span class="sxs-lookup"><span data-stu-id="00589-188">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="00589-189">ときに、 *httpd*モジュールがインストールされている、 *mod_ssl*モジュールもインストールするとします。</span><span class="sxs-lookup"><span data-stu-id="00589-189">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="00589-190">インストールされている場合を使用して`yum`構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="00589-190">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="00589-191">SSL を強制するのには、インストール、 `mod_rewrite` URL 書き換えを有効にするモジュール。</span><span class="sxs-lookup"><span data-stu-id="00589-191">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="00589-192">変更、 *hellomvc.conf* URL 書き換えを有効にして、ポート 443 での通信をセキュリティで保護するファイル。</span><span class="sxs-lookup"><span data-stu-id="00589-192">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="00589-193">この例は、ローカルに生成された証明書を使用します。</span><span class="sxs-lookup"><span data-stu-id="00589-193">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="00589-194">**SSLCertificateFile**ドメイン名のプライマリ証明書ファイルでなければなりません。</span><span class="sxs-lookup"><span data-stu-id="00589-194">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="00589-195">**SSLCertificateKeyFile**生成するか、キー ファイル CSR を作成します。</span><span class="sxs-lookup"><span data-stu-id="00589-195">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="00589-196">**SSLCertificateChainFile** (存在する場合)、中間証明書ファイルをする必要があります証明機関から提供されています。</span><span class="sxs-lookup"><span data-stu-id="00589-196">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="00589-197">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="00589-197">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="00589-198">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="00589-198">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="00589-199">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="00589-199">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="00589-200">追加のヘッダー</span><span class="sxs-lookup"><span data-stu-id="00589-200">Additional headers</span></span>

<span data-ttu-id="00589-201">悪意のある攻撃からセキュリティで保護するためには、いくつかのヘッダーをする必要がありますか、変更または追加できます。</span><span class="sxs-lookup"><span data-stu-id="00589-201">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="00589-202">いることを確認、`mod_headers`モジュールがインストールされています。</span><span class="sxs-lookup"><span data-stu-id="00589-202">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="00589-203">Clickjacking の攻撃からセキュリティで保護された Apache</span><span class="sxs-lookup"><span data-stu-id="00589-203">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="00589-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)とも呼ばれる、 *UI redress 攻撃*、悪意のある攻撃は、ここで web サイトの訪問者は、騙されてクリックしてリンクやボタンは別のページをしている現在のページです。</span><span class="sxs-lookup"><span data-stu-id="00589-204">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="00589-205">使用して`X-FRAME-OPTIONS`サイトをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="00589-205">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="00589-206">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00589-206">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="00589-207">行を追加`Header append X-FRAME-OPTIONS "SAMEORIGIN"`です。</span><span class="sxs-lookup"><span data-stu-id="00589-207">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="00589-208">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="00589-208">Save the file.</span></span> <span data-ttu-id="00589-209">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="00589-209">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="00589-210">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="00589-210">MIME-type sniffing</span></span>

<span data-ttu-id="00589-211">`X-Content-Type-Options`ヘッダーにより、Internet Explorer から*MIME スニッフィング*(ファイルの決定`Content-Type`ファイルの内容から)。</span><span class="sxs-lookup"><span data-stu-id="00589-211">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="00589-212">サーバーを設定する場合、`Content-Type`ヘッダーを`text/html`で、 `nosniff` Internet Explorer、オプションのセットとして内容を表示する`text/html`ファイルの内容に関係なく。</span><span class="sxs-lookup"><span data-stu-id="00589-212">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="00589-213">編集、 *httpd.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="00589-213">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="00589-214">行を追加`Header set X-Content-Type-Options "nosniff"`です。</span><span class="sxs-lookup"><span data-stu-id="00589-214">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="00589-215">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="00589-215">Save the file.</span></span> <span data-ttu-id="00589-216">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="00589-216">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="00589-217">負荷分散</span><span class="sxs-lookup"><span data-stu-id="00589-217">Load Balancing</span></span> 

<span data-ttu-id="00589-218">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="00589-218">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="00589-219">ためにないの単一障害点です。使用して*mod_proxy_balancer*を変更して、 **VirtualHost**なります Apache プロキシ サーバーの背後に web アプリの複数のインスタンスを管理するためです。</span><span class="sxs-lookup"><span data-stu-id="00589-219">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="00589-220">追加のインスタンスの次に示す構成ファイルで、`hellomvc`アプリは 5001 ポート上で実行するセットアップです。</span><span class="sxs-lookup"><span data-stu-id="00589-220">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="00589-221">*プロキシ*セクションは、負荷を分散する 2 つのメンバー バランサー構成で設定が*byrequests*です。</span><span class="sxs-lookup"><span data-stu-id="00589-221">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="00589-222">速度の制限</span><span class="sxs-lookup"><span data-stu-id="00589-222">Rate Limits</span></span>
<span data-ttu-id="00589-223">使用して*mod_ratelimit*に含まれている、 *httpd*モジュールのクライアントの帯域幅は制限されることができます。</span><span class="sxs-lookup"><span data-stu-id="00589-223">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="00589-224">ファイルの例では、600 KB/秒 ルートの場所として帯域幅を制限します。</span><span class="sxs-lookup"><span data-stu-id="00589-224">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
