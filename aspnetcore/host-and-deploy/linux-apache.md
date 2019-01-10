---
title: Apache 搭載の Linux で ASP.NET Core をホストする
description: CentOS 上にリバース プロキシ サーバーとして Apache をセットアップし、Kestrel 上で実行されている ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 12/20/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: 8c590743328885336498ca2446c618b13a7d2ce2
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997228"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="3cebd-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="3cebd-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="3cebd-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="3cebd-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="3cebd-105">このガイドでは、[CentOS 7](https://www.centos.org/) 上にリバース プロキシ サーバーとして [Apache](https://httpd.apache.org/) をセットアップし、[Kestrel](xref:fundamentals/servers/kestrel) サーバー上で実行されている ASP.NET Core Web アプリに HTTP トラフィックをリダイレクトする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="3cebd-106">[mod_proxy 拡張機能](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)および関連するモジュールは、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cebd-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3cebd-107">Prerequisites</span></span>

* <span data-ttu-id="3cebd-108">CentOS 7 を実行しているサーバーと、sudo 特権を持つ標準ユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="3cebd-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
* <span data-ttu-id="3cebd-109">サーバーへの .NET Core ランタイムのインストール。</span><span class="sxs-lookup"><span data-stu-id="3cebd-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="3cebd-110">[.NET の「All Downloads」 (すべてのダウンロード) ページ](https://www.microsoft.com/net/download/all)に移動します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="3cebd-111">プレビューではない最新のランタイムを、**Runtime** (ランタイム) の一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="3cebd-112">CentOS/Oracle の手順を選択し、それに従います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-112">Select and follow the instructions for CentOS/Oracle.</span></span>
* <span data-ttu-id="3cebd-113">既存の ASP.NET Core アプリ。</span><span class="sxs-lookup"><span data-stu-id="3cebd-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="3cebd-114">アプリを介して発行およびコピーする</span><span class="sxs-lookup"><span data-stu-id="3cebd-114">Publish and copy over the app</span></span>

<span data-ttu-id="3cebd-115">[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="3cebd-116">開発環境から [dotnet publish](/dotnet/core/tools/dotnet-publish) を実行し、サーバー上で実行できるディレクトリ (たとえば、*bin/Release/&lt;target_framework_moniker&gt;/publish*) にアプリをパッケージします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="3cebd-117">サーバーで .NET Core ランタイムを管理しない場合、アプリは[独立した展開](/dotnet/core/deploying/#self-contained-deployments-scd)として発行することもできます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="3cebd-118">組織のワークフローに統合されているツール (SCP や SFTP など) を使用して、サーバーに ASP.NET Core アプリをコピーします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="3cebd-119">Web アプリは一般的に *var* ディレクトリの下に配置されます (たとえば、*var/www/helloapp*)。</span><span class="sxs-lookup"><span data-stu-id="3cebd-119">It's common to locate web apps under the *var* directory (for example, *var/www/helloapp*).</span></span>

> [!NOTE]
> <span data-ttu-id="3cebd-120">運用展開シナリオの場合、継続的インテグレーション ワークフローが、アプリの発行処理とサーバーへの資産のコピーを行います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="3cebd-121">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="3cebd-121">Configure a proxy server</span></span>

<span data-ttu-id="3cebd-122">リバース プロキシは、動的 Web アプリを提供するための一般的な仕組みです。</span><span class="sxs-lookup"><span data-stu-id="3cebd-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="3cebd-123">リバース プロキシは HTTP 要求を終了させ、ASP.NET アプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="3cebd-124">プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="3cebd-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="3cebd-125">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="3cebd-126">このガイドでは、Kestrel が ASP.NET Core アプリを提供しているものと同じサーバー上で実行されるリバース プロキシとして Apache を構成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="3cebd-127">要求はリバース プロキシによって転送されます。そのため、[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) パッケージの [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) を使用します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="3cebd-128">リダイレクト URI とその他のセキュリティ ポリシーを正しく機能させるために、このミドルウェアは、`X-Forwarded-Proto` ヘッダーを利用して、`Request.Scheme` を更新します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="3cebd-129">認証、リンクの生成、リダイレクト、および地理的位置情報など、スキームに依存するすべてのコンポーネントは、Forwarded Headers Middleware の呼び出し後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="3cebd-130">一般的な規則として、Forwarded Headers Middleware は、診断およびエラー処理ミドルウェアを除くその他のミドルウェアより前に実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="3cebd-131">この順序により、転送されるヘッダー情報に依存するミドルウェアが処理にヘッダー値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="3cebd-132">[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) や同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-132">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3cebd-133">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-133">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="3cebd-134">[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) や [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)、または同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-134">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="3cebd-135">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-135">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

::: moniker-end

<span data-ttu-id="3cebd-136">ミドルウェアに対して [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が指定されていない場合、転送される既定のヘッダーは `None` です。</span><span class="sxs-lookup"><span data-stu-id="3cebd-136">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="3cebd-137">localhost (127.0.0.1, [::1]) で実行されるプロキシのみが既定で信頼されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-137">Only proxies running on localhost (127.0.0.1, [::1]) are trusted by default.</span></span> <span data-ttu-id="3cebd-138">組織内のその他の信頼されているプロキシまたはネットワークによってインターネットと Web サーバーの間の要求が処理される場合は、それらを、<xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions> を使用して <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> または <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> のリストに追加します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-138">If other trusted proxies or networks within the organization handle requests between the Internet and the web server, add them to the list of <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownProxies*> or <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.KnownNetworks*> with <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions>.</span></span> <span data-ttu-id="3cebd-139">次の例では、IP アドレス 10.0.0.100 にある信頼されているプロキシ サーバーが `Startup.ConfigureServices` 内の Forwarded Headers Middleware `KnownProxies` に追加されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-139">The following example adds a trusted proxy server at IP address 10.0.0.100 to the Forwarded Headers Middleware `KnownProxies` in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.KnownProxies.Add(IPAddress.Parse("10.0.0.100"));
});
```

<span data-ttu-id="3cebd-140">詳細については、「<xref:host-and-deploy/proxy-load-balancer>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-140">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

### <a name="install-apache"></a><span data-ttu-id="3cebd-141">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="3cebd-141">Install Apache</span></span>

<span data-ttu-id="3cebd-142">CentOS パッケージを最新の安定したバージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-142">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="3cebd-143">1 つの `yum` コマンドで、CentOS に Apache Web サーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-143">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="3cebd-144">コマンド実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="3cebd-144">Sample output after running the command:</span></span>

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
> <span data-ttu-id="3cebd-145">この例では、CentOS 7 のバージョンが 64 ビットなので、出力は httpd.86_64 を反映しています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-145">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="3cebd-146">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-146">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="3cebd-147">Apache を構成する</span><span class="sxs-lookup"><span data-stu-id="3cebd-147">Configure Apache</span></span>

<span data-ttu-id="3cebd-148">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-148">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="3cebd-149">`/etc/httpd/conf.modules.d/` 内のモジュール構成ファイルに加え、拡張子が *.conf* のファイルがアルファベット順で処理されます。このディレクトリには、モジュールの読み込みに必要な構成ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-149">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="3cebd-150">アプリ用に *helloapp.conf* という名前の構成ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-150">Create a configuration file, named *helloapp.conf*, for the app:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ServerName www.example.com
    ServerAlias *.example.com
    ErrorLog ${APACHE_LOG_DIR}helloapp-error.log
    CustomLog ${APACHE_LOG_DIR}helloapp-access.log common
</VirtualHost>
```

<span data-ttu-id="3cebd-151">`VirtualHost` ブロックは、サーバー上の 1 つまたは複数のファイルに複数回出現することができます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-151">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="3cebd-152">上記の構成ファイルでは、Apache はポート 80 でパブリック トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-152">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="3cebd-153">ドメイン `www.example.com` が提供されており、別名 `*.example.com` は同じ Web サイトに解決されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-153">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="3cebd-154">詳しくは、「[名前ベースのバーチャル ホスト](https://httpd.apache.org/docs/current/vhosts/name-based.html)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-154">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="3cebd-155">要求は、ルートにおいて、127.0.0.1 にあるサーバーのポート 5000 にプロキシされます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-155">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="3cebd-156">双方向通信の場合は、`ProxyPass` と `ProxyPassReverse` が必要です。</span><span class="sxs-lookup"><span data-stu-id="3cebd-156">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="3cebd-157">Kestrel の IP/ポートを変更するには、[Kestrel のエンドポイントの構成](xref:fundamentals/servers/kestrel#endpoint-configuration)に関するセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-157">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="3cebd-158">**VirtualHost** ブロックで適切な [ServerName ディレクティブ](https://httpd.apache.org/docs/current/mod/core.html#servername)を指定しないと、アプリにセキュリティ上の脆弱性が生じます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-158">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="3cebd-159">親ドメイン全体を制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.example.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="3cebd-159">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="3cebd-160">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-160">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="3cebd-161">`ErrorLog` および `CustomLog` ディレクティブを使って、`VirtualHost` ごとにログを構成できます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-161">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="3cebd-162">`ErrorLog` は、サーバーがエラーをログに記録する場所です。`CustomLog` には、ログ ファイルのファイル名と形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-162">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="3cebd-163">この例では、要求の情報がログに記録される場所です。</span><span class="sxs-lookup"><span data-stu-id="3cebd-163">In this case, this is where request information is logged.</span></span> <span data-ttu-id="3cebd-164">1 つの要求につき 1 行が記録されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-164">There's one line for each request.</span></span>

<span data-ttu-id="3cebd-165">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-165">Save the file and test the configuration.</span></span> <span data-ttu-id="3cebd-166">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-166">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="3cebd-167">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-167">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitor-the-app"></a><span data-ttu-id="3cebd-168">アプリを監視する</span><span class="sxs-lookup"><span data-stu-id="3cebd-168">Monitor the app</span></span>

<span data-ttu-id="3cebd-169">これで、`http://localhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリ (`http://127.0.0.1:5000`) に転送するように Apache が設定されました。</span><span class="sxs-lookup"><span data-stu-id="3cebd-169">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="3cebd-170">ただし、Apache は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="3cebd-170">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="3cebd-171">*systemd* を使って、基礎 Web アプリを起動して監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-171">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="3cebd-172">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-172">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span>

### <a name="create-the-service-file"></a><span data-ttu-id="3cebd-173">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="3cebd-173">Create the service file</span></span>

<span data-ttu-id="3cebd-174">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-174">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-helloapp.service
```

<span data-ttu-id="3cebd-175">アプリのサービス ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-175">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/www/helloapp
ExecStart=/usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="3cebd-176">ユーザー *apache* が構成で使用されない場合、ユーザーを先に作成し、そのユーザーにファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-176">If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership of files.</span></span>

<span data-ttu-id="3cebd-177">アプリが最初の割り込み信号を受信してからシャットダウンするのを待機する期間を構成するには、`TimeoutStopSec` を使用します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-177">Use `TimeoutStopSec` to configure the duration of time to wait for the app to shut down after it receives the initial interrupt signal.</span></span> <span data-ttu-id="3cebd-178">この期間内にアプリがシャットダウンしない場合は、SIGKILL を発行してアプリを終了します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-178">If the app doesn't shut down in this period, SIGKILL is issued to terminate the app.</span></span> <span data-ttu-id="3cebd-179">タイムアウトを無効にするには、値として、単位なしの秒数 (`150` など)、期間の値 (`2min 30s` など)、または `infinity` を指定します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-179">Provide the value as unitless seconds (for example, `150`), a time span value (for example, `2min 30s`), or `infinity` to disable the timeout.</span></span> <span data-ttu-id="3cebd-180">`TimeoutStopSec` は、既定ではマネージャー構成ファイル (*systemd-system.conf*、*system.conf.d*、*systemd-user.conf*、*user.conf.d*) 内の `DefaultTimeoutStopSec` の値に設定されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-180">`TimeoutStopSec` defaults to the value of `DefaultTimeoutStopSec` in the manager configuration file (*systemd-system.conf*, *system.conf.d*, *systemd-user.conf*, *user.conf.d*).</span></span> <span data-ttu-id="3cebd-181">ほとんどのディストリビューションにおいて、タイムアウトの既定値は 90 秒となります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-181">The default timeout for most distributions is 90 seconds.</span></span>

```
# The default value is 90 seconds for most distributions.
TimeoutStopSec=90
```

<span data-ttu-id="3cebd-182">構成プロバイダーが環境変数を読み取れるようにするために、一部の値 (たとえば SQL の接続文字列) をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-182">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="3cebd-183">次のコマンドを使用して、構成ファイルで使用するために適切にエスケープされた値を生成します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-183">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>

```console
systemd-escape "<value-to-escape>"
```

<span data-ttu-id="3cebd-184">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-184">Save the file and enable the service:</span></span>

```bash
sudo systemctl enable kestrel-helloapp.service
```

<span data-ttu-id="3cebd-185">サービスを起動し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-185">Start the service and verify that it's running:</span></span>

```bash
sudo systemctl start kestrel-helloapp.service
sudo systemctl status kestrel-helloapp.service

● kestrel-helloapp.service - Example .NET Web API App running on CentOS 7
    Loaded: loaded (/etc/systemd/system/kestrel-helloapp.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-helloapp.service
            └─9021 /usr/local/bin/dotnet /var/www/helloapp/helloapp.dll
```

<span data-ttu-id="3cebd-186">リバース プロキシが構成され、Kestrel は *systemd* 経由で管理されます。これで Web アプリは完全に構成され、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-186">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="3cebd-187">応答ヘッダーを調べると、**Server** ヘッダーでは ASP.NET Core アプリが Kestrel によって提供されていることが示されています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-187">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="view-logs"></a><span data-ttu-id="3cebd-188">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="3cebd-188">View logs</span></span>

<span data-ttu-id="3cebd-189">Kestrel を使う Web アプリは *systemd* を使って管理されるため、イベントとプロセスは中央のジャーナルに記録されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-189">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="3cebd-190">ただし、このジャーナルには、*systemd* によって管理されるすべてのサービスとプロセスのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-190">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="3cebd-191">`kestrel-helloapp.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-191">To view the `kestrel-helloapp.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service
```

<span data-ttu-id="3cebd-192">時間フィルタリングの場合は、コマンドで時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-192">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="3cebd-193">たとえば、現在の日付でフィルター処理するには `--since today` を使い、前の 1 時間のエントリを参照するには `--until 1 hour ago` を使います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-193">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="3cebd-194">詳しくは、[journalctl の man ページ](https://www.unix.com/man-page/centos/1/journalctl/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-194">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-helloapp.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="data-protection"></a><span data-ttu-id="3cebd-195">データの保護</span><span class="sxs-lookup"><span data-stu-id="3cebd-195">Data protection</span></span>

<span data-ttu-id="3cebd-196">[ASP.NET Core データ保護スタック](xref:security/data-protection/introduction)は、認証ミドルウェア (Cookie ミドルウェアなど) やクロスサイト リクエスト フォージェリ (CSRF) 保護を含む、いくつかの ASP.NET Core [ ミドルウェア](xref:fundamentals/middleware/index)で使用されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-196">The [ASP.NET Core Data Protection stack](xref:security/data-protection/introduction) is used by several ASP.NET Core [middlewares](xref:fundamentals/middleware/index), including authentication middleware (for example, cookie middleware) and cross-site request forgery (CSRF) protections.</span></span> <span data-ttu-id="3cebd-197">データ保護 API がユーザーのコードから呼び出されない場合でも、永続的な暗号化[キー ストア](xref:security/data-protection/implementation/key-management)を作成するようにデータ保護を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-197">Even if Data Protection APIs aren't called by user code, data protection should be configured to create a persistent cryptographic [key store](xref:security/data-protection/implementation/key-management).</span></span> <span data-ttu-id="3cebd-198">データ保護を構成しない場合、既定でキーはメモリ内に保持され、アプリが再起動すると破棄されます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-198">If data protection isn't configured, the keys are held in memory and discarded when the app restarts.</span></span>

<span data-ttu-id="3cebd-199">キーリングがメモリに格納されている場合、アプリを再起動すると次のことが行われます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-199">If the key ring is stored in memory when the app restarts:</span></span>

* <span data-ttu-id="3cebd-200">すべての cookie ベースの認証トークンは無効になります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-200">All cookie-based authentication tokens are invalidated.</span></span>
* <span data-ttu-id="3cebd-201">ユーザーは、次回の要求時に再度サインインする必要があります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-201">Users are required to sign in again on their next request.</span></span>
* <span data-ttu-id="3cebd-202">キーリングで保護されているデータは、いずれも復号化できなくなります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-202">Any data protected with the key ring can no longer be decrypted.</span></span> <span data-ttu-id="3cebd-203">これには、[CSRF トークン](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)と [ASP.NET Core MVC TempData cookie](xref:fundamentals/app-state#tempdata) が含まれます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-203">This may include [CSRF tokens](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration) and [ASP.NET Core MVC TempData cookies](xref:fundamentals/app-state#tempdata).</span></span>

<span data-ttu-id="3cebd-204">キー リングを永続化して暗号化するようにデータ保護を構成する場合は、次を参照してください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-204">To configure data protection to persist and encrypt the key ring, see:</span></span>

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>

## <a name="secure-the-app"></a><span data-ttu-id="3cebd-205">アプリをセキュリティで保護する</span><span class="sxs-lookup"><span data-stu-id="3cebd-205">Secure the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="3cebd-206">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="3cebd-206">Configure firewall</span></span>

<span data-ttu-id="3cebd-207">*Firewalld* は、ネットワーク ゾーンをサポートするファイアウォールを管理するための動的デーモンです。</span><span class="sxs-lookup"><span data-stu-id="3cebd-207">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="3cebd-208">ポートとパケットのフィルター処理は、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-208">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="3cebd-209">*Firewalld* は既定でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-209">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="3cebd-210">`yum` を使ってパッケージをインストールしたり、インストールされていることを確認したりできます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-210">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="3cebd-211">アプリに必要なポートのみを開くには、`firewalld` を使います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-211">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="3cebd-212">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-212">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="3cebd-213">次のコマンドは、ポート 80 と 443 が永続的に開かれるように設定します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-213">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="3cebd-214">ファイアウォールの設定を再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-214">Reload the firewall settings.</span></span> <span data-ttu-id="3cebd-215">既定のゾーンで使用できるサービスとポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-215">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="3cebd-216">オプションは `firewall-cmd -h` を調べることで使用できます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-216">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="3cebd-217">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="3cebd-217">SSL configuration</span></span>

<span data-ttu-id="3cebd-218">SSL 用に Apache を構成するには、*mod_ssl* モジュールを使います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-218">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="3cebd-219">*httpd* モジュールがインストールされていると、*mod_ssl* モジュールもインストールされています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-219">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="3cebd-220">インストールされていない場合は、`yum` を使って構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-220">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="3cebd-221">SSL を強制するには、`mod_rewrite` モジュールをインストールして URL の書き換えを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-221">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="3cebd-222">*helloapp.conf* ファイルを変更して、URL の書き換えを有効にし、ポート 443 での通信をセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-222">Modify the *helloapp.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
</VirtualHost>

<VirtualHost *:443>
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5000/
    ProxyPassReverse / http://127.0.0.1:5000/
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

> [!NOTE]
> <span data-ttu-id="3cebd-223">この例では、ローカルで生成された証明書を使います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-223">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="3cebd-224">**SSLCertificateFile** は、ドメイン名のプライマリ証明書ファイルです。</span><span class="sxs-lookup"><span data-stu-id="3cebd-224">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="3cebd-225">**SSLCertificateKeyFile** は、CSR の作成時に生成されるキー ファイルです。</span><span class="sxs-lookup"><span data-stu-id="3cebd-225">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="3cebd-226">**SSLCertificateChainFile** は、証明機関から提供された中間証明書ファイル (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="3cebd-226">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="3cebd-227">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-227">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="3cebd-228">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-228">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="3cebd-229">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="3cebd-229">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="3cebd-230">その他のヘッダー</span><span class="sxs-lookup"><span data-stu-id="3cebd-230">Additional headers</span></span>

<span data-ttu-id="3cebd-231">悪意のある攻撃から防御するために、変更または追加する必要があるヘッダーがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-231">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="3cebd-232">必ず `mod_headers` モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-232">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="3cebd-233">Apache をクリックジャッキング攻撃から保護する</span><span class="sxs-lookup"><span data-stu-id="3cebd-233">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="3cebd-234">[クリックジャッキング](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)は "*UI 着せ替え攻撃*" とも呼ばれ、Web サイトの訪問者を騙して現在訪れているものとは異なるページのリンクやボタンをクリックさせる悪意のある攻撃です。</span><span class="sxs-lookup"><span data-stu-id="3cebd-234">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="3cebd-235">サイトをセキュリティで保護するには、`X-FRAME-OPTIONS` を使います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-235">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="3cebd-236">クリックジャッキング攻撃を軽減するには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="3cebd-236">To mitigate clickjacking attacks:</span></span>

1. <span data-ttu-id="3cebd-237">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-237">Edit the *httpd.conf* file:</span></span>

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

   <span data-ttu-id="3cebd-238">行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-238">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span>
1. <span data-ttu-id="3cebd-239">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-239">Save the file.</span></span>
1. <span data-ttu-id="3cebd-240">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-240">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="3cebd-241">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="3cebd-241">MIME-type sniffing</span></span>

<span data-ttu-id="3cebd-242">`X-Content-Type-Options` ヘッダーは、Internet Explorer を "*MIME スニッフィング*" から防ぎます (ファイルの内容からファイルの `Content-Type` を判断します)。</span><span class="sxs-lookup"><span data-stu-id="3cebd-242">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="3cebd-243">サーバーが `nosniff` オプションを指定して `Content-Type` ヘッダーを `text/html` に設定すると、Internet Explorer はファイルの内容に関係なく `text/html` として内容をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-243">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="3cebd-244">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-244">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="3cebd-245">行 `Header set X-Content-Type-Options "nosniff"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-245">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="3cebd-246">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-246">Save the file.</span></span> <span data-ttu-id="3cebd-247">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-247">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="3cebd-248">負荷分散</span><span class="sxs-lookup"><span data-stu-id="3cebd-248">Load Balancing</span></span>

<span data-ttu-id="3cebd-249">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-249">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="3cebd-250">単一障害点を持たないように、*mod_proxy_balancer* を使い、**VirtualHost** を変更することで、Apache プロキシ サーバーの背後で複数インスタンスの Web アプを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="3cebd-250">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="3cebd-251">次に示す構成ファイルでは、ポート 5001 上で実行するように `helloapp` の追加インスタンスを設定しています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-251">In the configuration file shown below, an additional instance of the `helloapp` is set up to run on port 5001.</span></span> <span data-ttu-id="3cebd-252">*Proxy* セクションは、2 メンバーのバランサー構成を使って *byrequests* を負荷分散するように設定されています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-252">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:80>
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L]
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
    ErrorLog /var/log/httpd/helloapp-error.log
    CustomLog /var/log/httpd/helloapp-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="3cebd-253">速度の制限</span><span class="sxs-lookup"><span data-stu-id="3cebd-253">Rate Limits</span></span>

<span data-ttu-id="3cebd-254">*httpd* モジュールに含まれる *mod_ratelimit* を使って、クライアントの帯域幅を制限できます。</span><span class="sxs-lookup"><span data-stu-id="3cebd-254">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```

<span data-ttu-id="3cebd-255">次のファイルの例では、ルートの場所の下での帯域幅を 600 KB/秒に制限しています。</span><span class="sxs-lookup"><span data-stu-id="3cebd-255">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

### <a name="long-request-header-fields"></a><span data-ttu-id="3cebd-256">要求ヘッダー フィールドが長すぎます</span><span class="sxs-lookup"><span data-stu-id="3cebd-256">Long request header fields</span></span>

<span data-ttu-id="3cebd-257">アプリにプロキシ サーバーの既定の設定によって許可されている (通常、8,190 バイト) よりも長い要求ヘッダー フィールドが必要な場合、[LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) ディレクティブの値を調整します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-257">If the app requires request header fields longer than permitted by the proxy server's default setting (typically 8,190 bytes), adjust the value of the [LimitRequestFieldSize](https://httpd.apache.org/docs/2.4/mod/core.html#LimitRequestFieldSize) directive.</span></span> <span data-ttu-id="3cebd-258">適用する値は、シナリオによって異なります。</span><span class="sxs-lookup"><span data-stu-id="3cebd-258">The value to apply is scenario-dependent.</span></span> <span data-ttu-id="3cebd-259">詳細については、ご利用のサーバーのドキュメントを参照してください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-259">For more information, see your server's documentation.</span></span>

> [!WARNING]
> <span data-ttu-id="3cebd-260">必要な場合を除いて、`LimitRequestFieldSize` の既定値を増やさないでください。</span><span class="sxs-lookup"><span data-stu-id="3cebd-260">Don't increase the default value of `LimitRequestFieldSize` unless necessary.</span></span> <span data-ttu-id="3cebd-261">値を増やすと、悪意のあるユーザーによるバッファー オーバーラン (オーバーフロー) とサービス拒否 (DoS) の攻撃のリスクが増加します。</span><span class="sxs-lookup"><span data-stu-id="3cebd-261">Increasing the value increases the risk of buffer overrun (overflow) and Denial of Service (DoS) attacks by malicious users.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3cebd-262">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="3cebd-262">Additional resources</span></span>

* [<span data-ttu-id="3cebd-263">Linux における .NET Core の前提条件</span><span class="sxs-lookup"><span data-stu-id="3cebd-263">Prerequisites for .NET Core on Linux</span></span>](/dotnet/core/linux-prerequisites)
* <xref:test/troubleshoot>
* <xref:host-and-deploy/proxy-load-balancer>
