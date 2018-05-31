---
title: Apache 搭載の Linux で ASP.NET Core をホストする
description: CentOS 上にリバース プロキシ サーバーとして Apache をセットアップし、Kestrel 上で実行されている ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。
author: spboyer
manager: wpickett
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 473585f1be180645395c14a154c9c017ca50edab
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "33962818"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="1d46c-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="1d46c-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="1d46c-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="1d46c-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="1d46c-105">このガイドでは、[CentOS 7](https://www.centos.org/) 上にリバース プロキシ サーバーとして [Apache](https://httpd.apache.org/) をセットアップし、[Kestrel](xref:fundamentals/servers/kestrel) 上で実行されている ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="1d46c-106">[mod_proxy 拡張機能](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)および関連するモジュールは、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d46c-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="1d46c-107">Prerequisites</span></span>

1. <span data-ttu-id="1d46c-108">CentOS 7 を実行しているサーバーと、sudo 特権を持つ標準ユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="1d46c-108">Server running CentOS 7 with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="1d46c-109">ASP.NET Core アプリ</span><span class="sxs-lookup"><span data-stu-id="1d46c-109">ASP.NET Core app</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="1d46c-110">アプリの発行</span><span class="sxs-lookup"><span data-stu-id="1d46c-110">Publish the app</span></span>

<span data-ttu-id="1d46c-111">リリース構成の[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)として CentOS 7 ランタイム (`centos.7-x64`) にアプリを発行します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-111">Publish the app as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) in Release configuration for the CentOS 7 runtime (`centos.7-x64`).</span></span> <span data-ttu-id="1d46c-112">*bin/Release/netcoreapp2.0/centos.7-x64/publish* フォルダーの内容を、SCP、FTP、またはその他のファイル転送方法を使ってサーバーにコピーします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-112">Copy the contents of the *bin/Release/netcoreapp2.0/centos.7-x64/publish* folder to the server using SCP, FTP, or other file transfer method.</span></span>

> [!NOTE]
> <span data-ttu-id="1d46c-113">運用展開シナリオの場合、継続的インテグレーション ワークフローが、アプリの発行処理とサーバーへの資産のコピーを行います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-113">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span> 

## <a name="configure-a-proxy-server"></a><span data-ttu-id="1d46c-114">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="1d46c-114">Configure a proxy server</span></span>

<span data-ttu-id="1d46c-115">リバース プロキシは、動的 Web アプリを提供するための一般的な仕組みです。</span><span class="sxs-lookup"><span data-stu-id="1d46c-115">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="1d46c-116">リバース プロキシは HTTP 要求を終了させ、ASP.NET アプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-116">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="1d46c-117">プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="1d46c-117">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="1d46c-118">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-118">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="1d46c-119">このガイドでは、Kestrel が ASP.NET Core アプリを提供しているものと同じサーバー上で実行されるリバース プロキシとして Apache を構成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-119">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="1d46c-120">要求はリバース プロキシによって転送されます。そのため、[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) パッケージの Forwarded Headers Middleware を使用します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-120">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="1d46c-121">リダイレクト URI とその他のセキュリティ ポリシーを正しく機能させるために、このミドルウェアは、`X-Forwarded-Proto` ヘッダーを利用して、`Request.Scheme` を更新します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-121">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="1d46c-122">任意の認証ミドルウェアを使用する場合は、Forwarded Headers Middleware を最初に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-122">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="1d46c-123">この順序により、認証ミドルウェアはヘッダーの値を利用して、正しいリダイレクト URI を生成できます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-123">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1d46c-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1d46c-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1d46c-125">[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) や同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-125">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1d46c-126">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-126">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1d46c-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1d46c-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1d46c-128">[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) や [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)、または同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-128">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="1d46c-129">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-129">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="1d46c-130">ミドルウェアに対して [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が指定されていない場合、転送される既定のヘッダーは `None` です。</span><span class="sxs-lookup"><span data-stu-id="1d46c-130">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="1d46c-131">プロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-131">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="1d46c-132">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d46c-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="1d46c-133">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="1d46c-133">Install Apache</span></span>

<span data-ttu-id="1d46c-134">CentOS パッケージを最新の安定したバージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-134">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="1d46c-135">1 つの `yum` コマンドで、CentOS に Apache Web サーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-135">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="1d46c-136">コマンド実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="1d46c-136">Sample output after running the command:</span></span>

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
> <span data-ttu-id="1d46c-137">この例では、CentOS 7 のバージョンが 64 ビットなので、出力は httpd.86_64 を反映しています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-137">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="1d46c-138">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-138">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache-for-reverse-proxy"></a><span data-ttu-id="1d46c-139">Apache をリバース プロキシとして構成する</span><span class="sxs-lookup"><span data-stu-id="1d46c-139">Configure Apache for reverse proxy</span></span>

<span data-ttu-id="1d46c-140">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-140">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="1d46c-141">`/etc/httpd/conf.modules.d/` 内のモジュール構成ファイルに加え、拡張子が *.conf* のファイルがアルファベット順で処理されます。このディレクトリには、モジュールの読み込みに必要な構成ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-141">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="1d46c-142">アプリ用に *hellomvc.conf* という名前の構成ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-142">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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

<span data-ttu-id="1d46c-143">`VirtualHost` ブロックは、サーバー上の 1 つまたは複数のファイルに複数回出現することができます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-143">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="1d46c-144">上記の構成ファイルでは、Apache はポート 80 でパブリック トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-144">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="1d46c-145">ドメイン `www.example.com` が提供されており、別名 `*.example.com` は同じ Web サイトに解決されます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-145">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="1d46c-146">詳しくは、「[名前ベースのバーチャル ホスト](https://httpd.apache.org/docs/current/vhosts/name-based.html)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1d46c-146">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="1d46c-147">要求は、ルートにおいて、127.0.0.1 にあるサーバーのポート 5000 にプロキシされます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-147">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="1d46c-148">双方向通信の場合は、`ProxyPass` と `ProxyPassReverse` が必要です。</span><span class="sxs-lookup"><span data-stu-id="1d46c-148">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span>

> [!WARNING]
> <span data-ttu-id="1d46c-149">**VirtualHost** ブロックで適切な [ServerName ディレクティブ](https://httpd.apache.org/docs/current/mod/core.html#servername)を指定しないと、アプリにセキュリティ上の脆弱性が生じます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-149">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="1d46c-150">親ドメイン全体を制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.example.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="1d46c-150">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="1d46c-151">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d46c-151">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="1d46c-152">`ErrorLog` および `CustomLog` ディレクティブを使って、`VirtualHost` ごとにログを構成できます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-152">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="1d46c-153">`ErrorLog` は、サーバーがエラーをログに記録する場所です。`CustomLog` には、ログ ファイルのファイル名と形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-153">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="1d46c-154">この例では、要求の情報がログに記録される場所です。</span><span class="sxs-lookup"><span data-stu-id="1d46c-154">In this case, this is where request information is logged.</span></span> <span data-ttu-id="1d46c-155">1 つの要求につき 1 行が記録されます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-155">There's one line for each request.</span></span>

<span data-ttu-id="1d46c-156">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-156">Save the file and test the configuration.</span></span> <span data-ttu-id="1d46c-157">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-157">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="1d46c-158">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-158">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="1d46c-159">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="1d46c-159">Monitoring the app</span></span>

<span data-ttu-id="1d46c-160">これで、`http://localhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリ (`http://127.0.0.1:5000`) に転送するように Apache が設定されました。</span><span class="sxs-lookup"><span data-stu-id="1d46c-160">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="1d46c-161">ただし、Apache は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="1d46c-161">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="1d46c-162">*systemd* を使って、基礎 Web アプリを起動して監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-162">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="1d46c-163">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-163">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 


### <a name="create-the-service-file"></a><span data-ttu-id="1d46c-164">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="1d46c-164">Create the service file</span></span>

<span data-ttu-id="1d46c-165">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-165">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="1d46c-166">アプリのサービス ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-166">An example service file for the app:</span></span>

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
> <span data-ttu-id="1d46c-167">**ユーザー** &mdash; *apache* が構成で使用されない場合、ユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-167">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="1d46c-168">構成プロバイダーが環境変数を読み取れるようにするために、一部の値 (たとえば SQL の接続文字列) をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-168">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="1d46c-169">次のコマンドを使用して、構成ファイルで使用するために適切にエスケープされた値を生成します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-169">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="1d46c-170">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-170">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="1d46c-171">サービスを起動し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-171">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="1d46c-172">リバース プロキシが構成され、Kestrel は *systemd* 経由で管理されます。これで Web アプリは完全に構成され、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-172">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="1d46c-173">応答ヘッダーを調べると、**Server** ヘッダーでは ASP.NET Core アプリが Kestrel によって提供されていることが示されています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-173">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="1d46c-174">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="1d46c-174">Viewing logs</span></span>

<span data-ttu-id="1d46c-175">Kestrel を使う Web アプリは *systemd* を使って管理されるため、イベントとプロセスは中央のジャーナルに記録されます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-175">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="1d46c-176">ただし、このジャーナルには、*systemd* によって管理されるすべてのサービスとプロセスのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-176">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="1d46c-177">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-177">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="1d46c-178">時間フィルタリングの場合は、コマンドで時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-178">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="1d46c-179">たとえば、現在の日付でフィルター処理するには `--since today` を使い、前の 1 時間のエントリを参照するには `--until 1 hour ago` を使います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-179">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="1d46c-180">詳しくは、[journalctl の man ページ](https://www.unix.com/man-page/centos/1/journalctl/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1d46c-180">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="1d46c-181">アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="1d46c-181">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="1d46c-182">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="1d46c-182">Configure firewall</span></span>

<span data-ttu-id="1d46c-183">*Firewalld* は、ネットワーク ゾーンをサポートするファイアウォールを管理するための動的デーモンです。</span><span class="sxs-lookup"><span data-stu-id="1d46c-183">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="1d46c-184">ポートとパケットのフィルター処理は、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-184">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="1d46c-185">*Firewalld* は既定でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-185">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="1d46c-186">`yum` を使ってパッケージをインストールしたり、インストールされていることを確認したりできます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-186">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="1d46c-187">アプリに必要なポートのみを開くには、`firewalld` を使います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-187">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="1d46c-188">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-188">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="1d46c-189">次のコマンドは、ポート 80 と 443 が永続的に開かれるように設定します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-189">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="1d46c-190">ファイアウォールの設定を再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-190">Reload the firewall settings.</span></span> <span data-ttu-id="1d46c-191">既定のゾーンで使用できるサービスとポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-191">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="1d46c-192">オプションは `firewall-cmd -h` を調べることで使用できます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-192">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="1d46c-193">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="1d46c-193">SSL configuration</span></span>

<span data-ttu-id="1d46c-194">SSL 用に Apache を構成するには、*mod_ssl* モジュールを使います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-194">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="1d46c-195">*httpd* モジュールがインストールされていると、*mod_ssl* モジュールもインストールされています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-195">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="1d46c-196">インストールされていない場合は、`yum` を使って構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-196">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```
<span data-ttu-id="1d46c-197">SSL を強制するには、`mod_rewrite` モジュールをインストールして URL の書き換えを有効にします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-197">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="1d46c-198">*hellomvc.conf* ファイルを変更して、URL の書き換えを有効にし、ポート 443 での通信をセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-198">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="1d46c-199">この例では、ローカルで生成された証明書を使います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-199">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="1d46c-200">**SSLCertificateFile** は、ドメイン名のプライマリ証明書ファイルです。</span><span class="sxs-lookup"><span data-stu-id="1d46c-200">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="1d46c-201">**SSLCertificateKeyFile** は、CSR の作成時に生成されるキー ファイルです。</span><span class="sxs-lookup"><span data-stu-id="1d46c-201">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="1d46c-202">**SSLCertificateChainFile** は、証明機関から提供された中間証明書ファイル (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="1d46c-202">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="1d46c-203">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-203">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="1d46c-204">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-204">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="1d46c-205">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="1d46c-205">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="1d46c-206">その他のヘッダー</span><span class="sxs-lookup"><span data-stu-id="1d46c-206">Additional headers</span></span>

<span data-ttu-id="1d46c-207">悪意のある攻撃から防御するために、変更または追加する必要があるヘッダーがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="1d46c-207">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="1d46c-208">必ず `mod_headers` モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-208">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="1d46c-209">Apache をクリックジャッキング攻撃から保護する</span><span class="sxs-lookup"><span data-stu-id="1d46c-209">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="1d46c-210">[クリックジャッキング](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)は "*UI 着せ替え攻撃*" とも呼ばれ、Web サイトの訪問者を騙して現在訪れているものとは異なるページのリンクやボタンをクリックさせる悪意のある攻撃です。</span><span class="sxs-lookup"><span data-stu-id="1d46c-210">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="1d46c-211">サイトをセキュリティで保護するには、`X-FRAME-OPTIONS` を使います。</span><span class="sxs-lookup"><span data-stu-id="1d46c-211">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="1d46c-212">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-212">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="1d46c-213">行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-213">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="1d46c-214">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-214">Save the file.</span></span> <span data-ttu-id="1d46c-215">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-215">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="1d46c-216">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="1d46c-216">MIME-type sniffing</span></span>

<span data-ttu-id="1d46c-217">`X-Content-Type-Options` ヘッダーは、Internet Explorer を "*MIME スニッフィング*" から防ぎます (ファイルの内容からファイルの `Content-Type` を判断します)。</span><span class="sxs-lookup"><span data-stu-id="1d46c-217">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="1d46c-218">サーバーが `nosniff` オプションを指定して `Content-Type` ヘッダーを `text/html` に設定すると、Internet Explorer はファイルの内容に関係なく `text/html` として内容をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-218">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="1d46c-219">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-219">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="1d46c-220">行 `Header set X-Content-Type-Options "nosniff"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-220">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="1d46c-221">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-221">Save the file.</span></span> <span data-ttu-id="1d46c-222">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-222">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="1d46c-223">負荷分散</span><span class="sxs-lookup"><span data-stu-id="1d46c-223">Load Balancing</span></span> 

<span data-ttu-id="1d46c-224">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d46c-224">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="1d46c-225">単一障害点を持たないように、*mod_proxy_balancer* を使い、**VirtualHost** を変更することで、Apache プロキシ サーバーの背後で複数インスタンスの Web アプを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="1d46c-225">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="1d46c-226">次に示す構成ファイルでは、ポート 5001 上で実行するように `hellomvc` アプリの追加インスタンスを設定しています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-226">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="1d46c-227">*Proxy* セクションは、2 メンバーのバランサー構成を使って *byrequests* を負荷分散するように設定されています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-227">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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

### <a name="rate-limits"></a><span data-ttu-id="1d46c-228">速度の制限</span><span class="sxs-lookup"><span data-stu-id="1d46c-228">Rate Limits</span></span>
<span data-ttu-id="1d46c-229">*httpd* モジュールに含まれる *mod_ratelimit* を使って、クライアントの帯域幅を制限できます。</span><span class="sxs-lookup"><span data-stu-id="1d46c-229">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="1d46c-230">次のファイルの例では、ルートの場所の下での帯域幅を 600 KB/秒に制限しています。</span><span class="sxs-lookup"><span data-stu-id="1d46c-230">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```
