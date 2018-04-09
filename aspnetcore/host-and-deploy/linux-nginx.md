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
# <a name="host-aspnet-core-on-linux-with-nginx"></a><span data-ttu-id="2006c-103">Nginx 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="2006c-103">Host ASP.NET Core on Linux with Nginx</span></span>

<span data-ttu-id="2006c-104">[Sourabh Shirhatti](https://twitter.com/sshirhatti) による投稿</span><span class="sxs-lookup"><span data-stu-id="2006c-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="2006c-105">このガイドでは、Ubuntu 16.04 サーバーで本稼働対応の ASP.NET Core 環境をセットアップする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2006c-105">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

> [!NOTE]
> <span data-ttu-id="2006c-106">Ubuntu 14.04 の*supervisord* Kestrel プロセスを監視するためのソリューションとしてはお勧めします。</span><span class="sxs-lookup"><span data-stu-id="2006c-106">For Ubuntu 14.04, *supervisord* is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="2006c-107">*systemd* Ubuntu 14.04 では使用できません。</span><span class="sxs-lookup"><span data-stu-id="2006c-107">*systemd* isn't available on Ubuntu 14.04.</span></span> <span data-ttu-id="2006c-108">[このドキュメントの以前のバージョンを参照してください](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="2006c-108">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span></span>

<span data-ttu-id="2006c-109">このガイドでは:</span><span class="sxs-lookup"><span data-stu-id="2006c-109">This guide:</span></span>

* <span data-ttu-id="2006c-110">リバース プロキシ サーバーの背後にある既存の ASP.NET Core アプリケーションを配置します。</span><span class="sxs-lookup"><span data-stu-id="2006c-110">Places an existing ASP.NET Core app behind a reverse proxy server.</span></span>
* <span data-ttu-id="2006c-111">リバース プロキシ サーバーを設定すると、Kestrel web サーバーに要求を転送します。</span><span class="sxs-lookup"><span data-stu-id="2006c-111">Sets up the reverse proxy server to forward requests to the Kestrel web server.</span></span>
* <span data-ttu-id="2006c-112">デーモンとしての起動時に web アプリが実行されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2006c-112">Ensures the web app runs on startup as a daemon.</span></span>
* <span data-ttu-id="2006c-113">Web アプリを再起動するプロセスの管理ツールを構成します。</span><span class="sxs-lookup"><span data-stu-id="2006c-113">Configures a process management tool to help restart the web app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2006c-114">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2006c-114">Prerequisites</span></span>

1. <span data-ttu-id="2006c-115">Ubuntu 16.04 サーバーへのアクセスと sudo 特権が与えられた標準ユーザー アカウント</span><span class="sxs-lookup"><span data-stu-id="2006c-115">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
1. <span data-ttu-id="2006c-116">既存の ASP.NET Core アプリケーション</span><span class="sxs-lookup"><span data-stu-id="2006c-116">An existing ASP.NET Core app</span></span>

## <a name="copy-over-the-app"></a><span data-ttu-id="2006c-117">アプリ経由でコピーします。</span><span class="sxs-lookup"><span data-stu-id="2006c-117">Copy over the app</span></span>

<span data-ttu-id="2006c-118">実行[dotnet 発行](/dotnet/core/tools/dotnet-publish)サーバーで実行できる自己完結型のディレクトリに、アプリのパッケージを開発環境からです。</span><span class="sxs-lookup"><span data-stu-id="2006c-118">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="2006c-119">どのようなツールを使用してサーバーに ASP.NET Core アプリケーションのコピーは、組織のワークフロー (たとえば、SCP、FTP など) に統合します。</span><span class="sxs-lookup"><span data-stu-id="2006c-119">Copy the ASP.NET Core app to the server using whatever tool integrates into the organization's workflow (for example, SCP, FTP).</span></span> <span data-ttu-id="2006c-120">次のようにアプリをテストします。</span><span class="sxs-lookup"><span data-stu-id="2006c-120">Test the app, for example:</span></span>

* <span data-ttu-id="2006c-121">コマンドラインから実行`dotnet <app_assembly>.dll`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-121">From the command line, run `dotnet <app_assembly>.dll`.</span></span>
* <span data-ttu-id="2006c-122">ブラウザーで、`http://<serveraddress>:<port>` に移動し、アプリが Linux で動作することを検証します。</span><span class="sxs-lookup"><span data-stu-id="2006c-122">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="2006c-123">リバース プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="2006c-123">Configure a reverse proxy server</span></span>

<span data-ttu-id="2006c-124">リバース プロキシは、動的な web アプリの送信用の共通のセットアップです。</span><span class="sxs-lookup"><span data-stu-id="2006c-124">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="2006c-125">リバース プロキシは、HTTP 要求を終了し、ASP.NET Core アプリケーションに転送します。</span><span class="sxs-lookup"><span data-stu-id="2006c-125">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core app.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="2006c-126">リバース プロキシ サーバーを利用する理由</span><span class="sxs-lookup"><span data-stu-id="2006c-126">Why use a reverse proxy server?</span></span>

<span data-ttu-id="2006c-127">Kestrel は、ASP.NET Core から動的なコンテンツを提供しているに便利です。</span><span class="sxs-lookup"><span data-stu-id="2006c-127">Kestrel is great for serving dynamic content from ASP.NET Core.</span></span> <span data-ttu-id="2006c-128">ただし、web サービス機能は、IIS、Apache、Nginx などのサーバーと豊富な機能としてはありません。</span><span class="sxs-lookup"><span data-stu-id="2006c-128">However, the web serving capabilities aren't as feature rich as servers such as IIS, Apache, or Nginx.</span></span> <span data-ttu-id="2006c-129">リバース プロキシ サーバーは、静的なコンテンツ、要求をキャッシュ、要求、および HTTP サーバーからの SSL ターミネーションを圧縮するなどの作業をオフロードできます。</span><span class="sxs-lookup"><span data-stu-id="2006c-129">A reverse proxy server can offload work such as serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="2006c-130">リバース プロキシ サーバーは専用コンピューター上に置かれることもあれば、HTTP サーバーと並んで展開されることもあります。</span><span class="sxs-lookup"><span data-stu-id="2006c-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="2006c-131">このガイドの目的のために、単一インスタンスの Nginx が使用されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="2006c-132">HTTP サーバーと並んで、同じサーバー上で実行されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="2006c-133">要件に基づき、異なる設定が選択すること。</span><span class="sxs-lookup"><span data-stu-id="2006c-133">Based on requirements, a different setup may be chosen.</span></span>

<span data-ttu-id="2006c-134">要求は、リバース プロキシによって転送される、ためから転送されるヘッダー ミドルウェアを使用して、 [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/)パッケージです。</span><span class="sxs-lookup"><span data-stu-id="2006c-134">Because requests are forwarded by reverse proxy, use the Forwarded Headers Middleware from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="2006c-135">ミドルウェアの更新プログラム、`Request.Scheme`を使用して、`X-Forwarded-Proto`ヘッダー、そのリダイレクト Uri とその他のセキュリティ ポリシーが正常に動作するようにします。</span><span class="sxs-lookup"><span data-stu-id="2006c-135">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="2006c-136">認証ミドルウェアの任意の型を使用する場合、転送ヘッダー ミドルウェアが最初に実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2006c-136">When using any type of authentication middleware, the Forwarded Headers Middleware must run first.</span></span> <span data-ttu-id="2006c-137">この順序により、認証ミドルウェアがヘッダーの値を使用して正しくリダイレクト Uri を生成することができます。</span><span class="sxs-lookup"><span data-stu-id="2006c-137">This ordering ensures that the authentication middleware can consume the header values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2006c-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2006c-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2006c-139">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication)または類似の認証スキームのミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="2006c-139">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2006c-140">転送するミドルウェアを構成、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="2006c-140">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2006c-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2006c-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2006c-142">呼び出す、 [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders)メソッド`Startup.Configure`呼び出す前に[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity)と[UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)または同様の認証スキームミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="2006c-142">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="2006c-143">転送するミドルウェアを構成、`X-Forwarded-For`と`X-Forwarded-Proto`ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="2006c-143">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="2006c-144">ない場合は[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions)は指定した、ミドルウェアに転送する既定のヘッダーが`None`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-144">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="2006c-145">追加の構成は、プロキシ サーバーとロード バランサーの背後にホストされているアプリの必要があります。</span><span class="sxs-lookup"><span data-stu-id="2006c-145">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="2006c-146">詳細については、次を参照してください。[にプロキシ サーバーを操作すると、ロード バランサーの ASP.NET Core の構成](xref:host-and-deploy/proxy-load-balancer)です。</span><span class="sxs-lookup"><span data-stu-id="2006c-146">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-nginx"></a><span data-ttu-id="2006c-147">Nginx をインストールする</span><span class="sxs-lookup"><span data-stu-id="2006c-147">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="2006c-148">省略可能な Nginx モジュールをインストールする場合ソースから Nginx を構築する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2006c-148">If optional Nginx modules will be installed, building Nginx from source might be required.</span></span>

<span data-ttu-id="2006c-149">`apt-get` を利用し、Nginx をインストールします。</span><span class="sxs-lookup"><span data-stu-id="2006c-149">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="2006c-150">インストーラーにより System V init スクリプトが作成されます。このスクリプトがシステム起動時に Nginx をデーモンとして実行します。</span><span class="sxs-lookup"><span data-stu-id="2006c-150">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="2006c-151">Nginx は初めてのインストールとなるので、次を実行して明示的に起動します。</span><span class="sxs-lookup"><span data-stu-id="2006c-151">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="2006c-152">ブラウザーで Nginx の既定のランディング ページが表示されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2006c-152">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="2006c-153">Nginx を構成する</span><span class="sxs-lookup"><span data-stu-id="2006c-153">Configure Nginx</span></span>

<span data-ttu-id="2006c-154">ASP.NET Core アプリケーションに要求を転送するリバース プロキシとして Nginx を構成するには、変更*/etc/nginx/sites-available/default*です。</span><span class="sxs-lookup"><span data-stu-id="2006c-154">To configure Nginx as a reverse proxy to forward requests to your ASP.NET Core app, modify */etc/nginx/sites-available/default*.</span></span> <span data-ttu-id="2006c-155">テキスト エディターで開き、中身を次のものに変更します。</span><span class="sxs-lookup"><span data-stu-id="2006c-155">Open it in a text editor, and replace the contents with the following:</span></span>

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

<span data-ttu-id="2006c-156">ない場合`server_name`Nginx の一致が既定のサーバーを使用します。</span><span class="sxs-lookup"><span data-stu-id="2006c-156">When no `server_name` matches, Nginx uses the default server.</span></span> <span data-ttu-id="2006c-157">既定のサーバーが定義されていない場合、最初のサーバー構成ファイルでは、既定のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="2006c-157">If no default server is defined, the first server in the configuration file is the default server.</span></span> <span data-ttu-id="2006c-158">ベスト プラクティスとして、構成ファイルで 444 のステータス コードを返す特定の既定のサーバーを追加します。</span><span class="sxs-lookup"><span data-stu-id="2006c-158">As a best practice, add a specific default server which returns a status code of 444 in your configuration file.</span></span> <span data-ttu-id="2006c-159">既定のサーバーの構成例を示します。</span><span class="sxs-lookup"><span data-stu-id="2006c-159">A default server configuration example is:</span></span>

```nginx
server {
    listen   80 default_server;
    # listen [::]:80 default_server deferred;
    return   444;
}
```

<span data-ttu-id="2006c-160">上記の構成ファイルと、既定のサーバーと Nginx はホスト ヘッダーを持つポート 80 でパブリック トラフィックを受け付ける`example.com`または`*.example.com`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-160">With the preceding configuration file and default server, Nginx accepts public traffic on port 80 with host header `example.com` or `*.example.com`.</span></span> <span data-ttu-id="2006c-161">これらのホストと一致しない要求 Kestrel に転送されません。</span><span class="sxs-lookup"><span data-stu-id="2006c-161">Requests not matching these hosts won't get forwarded to Kestrel.</span></span> <span data-ttu-id="2006c-162">Nginx で Kestrel に一致する要求を転送する`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-162">Nginx forwards the matching requests to Kestrel at `http://localhost:5000`.</span></span> <span data-ttu-id="2006c-163">参照してください[nginx が要求をどのように処理するか](https://nginx.org/docs/http/request_processing.html)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="2006c-163">See [How nginx processes a request](https://nginx.org/docs/http/request_processing.html) for more information.</span></span>

> [!WARNING]
> <span data-ttu-id="2006c-164">適切なを指定する[server_name ディレクティブ](https://nginx.org/docs/http/server_names.html)セキュリティの脆弱性にアプリを公開します。</span><span class="sxs-lookup"><span data-stu-id="2006c-164">Failure to specify a proper [server_name directive](https://nginx.org/docs/http/server_names.html) exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="2006c-165">サブドメイン ワイルドカード バインド (たとえば、 `*.example.com`) 全体の親ドメインを制御する場合、このセキュリティ上のリスクは発生しません (to `*.com`、に対して脆弱である)。</span><span class="sxs-lookup"><span data-stu-id="2006c-165">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="2006c-166">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="2006c-166">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="2006c-167">Nginx 構成が確立されると、実行`sudo nginx -t`構成ファイルの構文を確認します。</span><span class="sxs-lookup"><span data-stu-id="2006c-167">Once the Nginx configuration is established, run `sudo nginx -t` to verify the syntax of the configuration files.</span></span> <span data-ttu-id="2006c-168">構成ファイルのテストが成功した場合は、強制的に実行して、変更を取得する Nginx`sudo nginx -s reload`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-168">If the configuration file test is successful, force Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-the-app"></a><span data-ttu-id="2006c-169">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="2006c-169">Monitoring the app</span></span>

<span data-ttu-id="2006c-170">サーバーがセットアップへの要求を転送する`http://<serveraddress>:80`で Kestrel で実行されている ASP.NET Core アプリケーションにログオン`http://127.0.0.1:5000`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-170">The server is setup to forward requests made to `http://<serveraddress>:80` on to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="2006c-171">ただし、Nginx は Kestrel プロセスを管理するセットアップをされていません。</span><span class="sxs-lookup"><span data-stu-id="2006c-171">However, Nginx isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="2006c-172">*systemd*を起動し、基になる web アプリを監視するサービス ファイルを作成するために使用できます。</span><span class="sxs-lookup"><span data-stu-id="2006c-172">*systemd* can be used to create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="2006c-173">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="2006c-173">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="2006c-174">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="2006c-174">Create the service file</span></span>

<span data-ttu-id="2006c-175">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="2006c-175">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="2006c-176">アプリのサービス ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2006c-176">The following is an example service file for the app:</span></span>

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

<span data-ttu-id="2006c-177">**注:**場合、ユーザー *www データ*使用されていない、構成によって、ここで定義したユーザー必要がありますで最初に作成されファイルの適切な所有権を指定します。</span><span class="sxs-lookup"><span data-stu-id="2006c-177">**Note:** If the user *www-data* isn't used by the configuration, the user defined here must be created first and given proper ownership for files.</span></span>
<span data-ttu-id="2006c-178">**注:** Linux には、区別するファイル システム。</span><span class="sxs-lookup"><span data-stu-id="2006c-178">**Note:** Linux has a case-sensitive file system.</span></span> <span data-ttu-id="2006c-179">設定する ASPNETCORE_ENVIRONMENT"Production"、構成ファイルの検索中に*appsettings です。Production.json*ではなく、 *appsettings.production.json*です。</span><span class="sxs-lookup"><span data-stu-id="2006c-179">Setting ASPNETCORE_ENVIRONMENT to "Production" results in searching for the configuration file *appsettings.Production.json*, not *appsettings.production.json*.</span></span>

<span data-ttu-id="2006c-180">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="2006c-180">Save the file and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="2006c-181">サービスを開始して、実行されていることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="2006c-181">Start the service and verify that it's running.</span></span>

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

<span data-ttu-id="2006c-182">リバース プロキシの構成および Kestrel systemd を介して管理されている場合、web アプリが完全に構成されているし、にローカル コンピューター上のブラウザーからアクセスできる`http://localhost`です。</span><span class="sxs-lookup"><span data-stu-id="2006c-182">With the reverse proxy configured and Kestrel managed through systemd, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="2006c-183">妨げている可能性があるすべてのファイアウォールがなければ、リモート コンピューターからアクセスできることもです。</span><span class="sxs-lookup"><span data-stu-id="2006c-183">It's also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="2006c-184">応答ヘッダーの検査、`Server`ヘッダー Kestrel によって処理される ASP.NET Core アプリケーションを示しています。</span><span class="sxs-lookup"><span data-stu-id="2006c-184">Inspecting the response headers, the `Server` header shows the ASP.NET Core app being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="2006c-185">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="2006c-185">Viewing logs</span></span>

<span data-ttu-id="2006c-186">Web アプリから Kestrel を使用して、使用して管理`systemd`、すべてのイベントとプロセスは、一元的な履歴に記録されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-186">Since the web app using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="2006c-187">ただし、このジャーナルには、`systemd` が管理するすべてのサービスとプロセスのすべてのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2006c-187">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="2006c-188">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="2006c-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="2006c-189">さらに絞り込むには、時間オプションとして `--since today`、`--until 1 hour ago`、あるいはこの 2 つの組み合わせを利用すれば、返されるエントリの数を減らすことができます。</span><span class="sxs-lookup"><span data-stu-id="2006c-189">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="2006c-190">アプリをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="2006c-190">Securing the app</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="2006c-191">AppArmor を有効にする</span><span class="sxs-lookup"><span data-stu-id="2006c-191">Enable AppArmor</span></span>

<span data-ttu-id="2006c-192">Linux セキュリティ モジュール (LSM) は、Linux 2.6 以降では Linux カーネルの一部であるフレームワークです。</span><span class="sxs-lookup"><span data-stu-id="2006c-192">Linux Security Modules (LSM) is a framework that's part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="2006c-193">LSM は、セキュリティ モジュールのさまざまな実装に対応しています。</span><span class="sxs-lookup"><span data-stu-id="2006c-193">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="2006c-194">[AppArmor](https://wiki.ubuntu.com/AppArmor) は Mandatory Access Control システムを実装する LSM です。このシステムは、プログラムのリソース範囲を限定できます。</span><span class="sxs-lookup"><span data-stu-id="2006c-194">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="2006c-195">AppArmor が有効であり、正しく構成されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="2006c-195">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-the-firewall"></a><span data-ttu-id="2006c-196">ファイアウォールを構成します。</span><span class="sxs-lookup"><span data-stu-id="2006c-196">Configuring the firewall</span></span>

<span data-ttu-id="2006c-197">使用されていないすべての外部ポートを閉じます。</span><span class="sxs-lookup"><span data-stu-id="2006c-197">Close off all external ports that are not in use.</span></span> <span data-ttu-id="2006c-198">ufw (uncomplicated firewall/複雑ではないファイアウォール) は `iptables` のフロント エンドとなり、ファイアウォールを構成するためのコマンド ライン インターフェイスを提供します。</span><span class="sxs-lookup"><span data-stu-id="2006c-198">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="2006c-199">いることを確認`ufw`必要なポートのトラフィックを許可するように構成します。</span><span class="sxs-lookup"><span data-stu-id="2006c-199">Verify that `ufw` is configured to allow traffic on any ports needed.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="2006c-200">Nginx のセキュリティを強化する</span><span class="sxs-lookup"><span data-stu-id="2006c-200">Securing Nginx</span></span>

<span data-ttu-id="2006c-201">Nginx の既定のディストリビューションでは SSL が有効になっていません。</span><span class="sxs-lookup"><span data-stu-id="2006c-201">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="2006c-202">追加のセキュリティ機能を有効にするには、ソースからビルドします。</span><span class="sxs-lookup"><span data-stu-id="2006c-202">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="2006c-203">ソースをダウンロードし、ビルド依存関係をインストールする</span><span class="sxs-lookup"><span data-stu-id="2006c-203">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download Nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="2006c-204">Nginx 応答名を変更する</span><span class="sxs-lookup"><span data-stu-id="2006c-204">Change the Nginx response name</span></span>

<span data-ttu-id="2006c-205">*src/http/ngx_http_header_filter_module.c* を編集します。</span><span class="sxs-lookup"><span data-stu-id="2006c-205">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="2006c-206">オプションを構成し、ビルドする</span><span class="sxs-lookup"><span data-stu-id="2006c-206">Configure the options and build</span></span>

<span data-ttu-id="2006c-207">正規表現には PCRE ライブラリが必要です。</span><span class="sxs-lookup"><span data-stu-id="2006c-207">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="2006c-208">正規表現は、ngx_http_rewrite_module の場所ディレクティブで使用されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-208">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="2006c-209">http_ssl_module により HTTPS プロトコル サポートが追加されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-209">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="2006c-210">ように web アプリケーション ファイアウォールの使用を検討*ModSecurity*にアプリを強化します。</span><span class="sxs-lookup"><span data-stu-id="2006c-210">Consider using a web app firewall like *ModSecurity* to harden the app.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="2006c-211">SSL を構成する</span><span class="sxs-lookup"><span data-stu-id="2006c-211">Configure SSL</span></span>

* <span data-ttu-id="2006c-212">ポートで HTTPS トラフィックをリッスンするようにサーバーを構成する`443`を信頼された証明書機関 (CA) によって発行された有効な証明書を指定します。</span><span class="sxs-lookup"><span data-stu-id="2006c-212">Configure the server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="2006c-213">次のプラクティスを採用することによって、セキュリティを強化する*/etc/nginx/nginx.conf*ファイル。</span><span class="sxs-lookup"><span data-stu-id="2006c-213">Harden the security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="2006c-214">たとえば、強力な暗号を選択したり、HTTP 経由のすべてのトラフィックを HTTPS にリダイレクトしたりします。</span><span class="sxs-lookup"><span data-stu-id="2006c-214">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="2006c-215">`HTTP Strict-Transport-Security` (HSTS) ヘッダーを追加すると、クライアントが行う後続のすべての要求が HTTPS 経由のみになります。</span><span class="sxs-lookup"><span data-stu-id="2006c-215">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="2006c-216">Strict トランスポート セキュリティ ヘッダーを追加しないが、適切な選択または`max-age`場合は SSL は、今後は無効にします。</span><span class="sxs-lookup"><span data-stu-id="2006c-216">Don't add the Strict-Transport-Security header or chose an appropriate `max-age` if SSL will be disabled in the future.</span></span>

<span data-ttu-id="2006c-217">*/etc/nginx/proxy.conf* 構成ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="2006c-217">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[](linux-nginx/proxy.conf)]

<span data-ttu-id="2006c-218">*/etc/nginx/nginx.conf* 構成ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="2006c-218">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="2006c-219">この例では、1 つの構成ファイルに `http` セクションと `server` セクションの両方が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2006c-219">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[](linux-nginx/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="2006c-220">Nginx をクリックジャッキングから守る</span><span class="sxs-lookup"><span data-stu-id="2006c-220">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="2006c-221">クリックジャッキングは、感染したユーザーのクリックを集めるという悪意のある手法です。</span><span class="sxs-lookup"><span data-stu-id="2006c-221">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="2006c-222">クリックジャッキングは被害者 (訪問者) をだまし、感染したサイトでクリックさせます。</span><span class="sxs-lookup"><span data-stu-id="2006c-222">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="2006c-223">X-フレームのオプションを使用、サイトをセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="2006c-223">Use X-FRAME-OPTIONS to secure the site.</span></span>

<span data-ttu-id="2006c-224">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="2006c-224">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="2006c-225">行 `add_header X-Frame-Options "SAMEORIGIN";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2006c-225">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="2006c-226">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="2006c-226">MIME-type sniffing</span></span>

<span data-ttu-id="2006c-227">このヘッダーは応答コンテンツの種類をオーバーライドしないようにブラウザーに指示するので、ほとんどのブラウザーで MIME スニッフィングが阻止されます。</span><span class="sxs-lookup"><span data-stu-id="2006c-227">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="2006c-228">`nosniff` オプションを指定すると、サーバーがコンテンツを "text/html" と読むとき、ブラウザーはそれを "text/html" として表示します。</span><span class="sxs-lookup"><span data-stu-id="2006c-228">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="2006c-229">*nginx.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="2006c-229">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="2006c-230">行 `add_header X-Content-Type-Options "nosniff";` を追加し、ファイルを保存し、Nginx を再起動します。</span><span class="sxs-lookup"><span data-stu-id="2006c-230">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
