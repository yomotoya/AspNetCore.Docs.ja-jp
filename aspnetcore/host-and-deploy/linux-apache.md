---
title: Apache 搭載の Linux で ASP.NET Core をホストする
description: CentOS 上にリバース プロキシ サーバーとして Apache をセットアップし、Kestrel 上で実行されている ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。
author: spboyer
ms.author: spboyer
ms.custom: mvc
ms.date: 03/13/2018
uid: host-and-deploy/linux-apache
ms.openlocfilehash: d02fbd82be37e6d67214a9a0bf5851662b577cb9
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37433975"
---
# <a name="host-aspnet-core-on-linux-with-apache"></a><span data-ttu-id="858e8-103">Apache 搭載の Linux で ASP.NET Core をホストする</span><span class="sxs-lookup"><span data-stu-id="858e8-103">Host ASP.NET Core on Linux with Apache</span></span>

<span data-ttu-id="858e8-104">作成者: [Shayne Boyer](https://github.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="858e8-104">By [Shayne Boyer](https://github.com/spboyer)</span></span>

<span data-ttu-id="858e8-105">このガイドでは、[CentOS 7](https://www.centos.org/) 上にリバース プロキシ サーバーとして [Apache](https://httpd.apache.org/) をセットアップし、[Kestrel](xref:fundamentals/servers/kestrel) 上で実行されている ASP.NET Core Web アプリに HTTP トラフィックを転送する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="858e8-105">Using this guide, learn how to set up [Apache](https://httpd.apache.org/) as a reverse proxy server on [CentOS 7](https://www.centos.org/) to redirect HTTP traffic to an ASP.NET Core web app running on [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="858e8-106">[mod_proxy 拡張機能](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html)および関連するモジュールは、サーバーのリバース プロキシを作成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-106">The [mod_proxy extension](http://httpd.apache.org/docs/2.4/mod/mod_proxy.html) and related modules create the server's reverse proxy.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="858e8-107">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="858e8-107">Prerequisites</span></span>

1. <span data-ttu-id="858e8-108">CentOS 7 を実行しているサーバーと、sudo 特権を持つ標準ユーザー アカウント。</span><span class="sxs-lookup"><span data-stu-id="858e8-108">Server running CentOS 7 with a standard user account with sudo privilege.</span></span>
1. <span data-ttu-id="858e8-109">サーバーへの .NET Core ランタイムのインストール。</span><span class="sxs-lookup"><span data-stu-id="858e8-109">Install the .NET Core runtime on the server.</span></span>
   1. <span data-ttu-id="858e8-110">[.NET の「All Downloads」 (すべてのダウンロード) ページ](https://www.microsoft.com/net/download/all)に移動します。</span><span class="sxs-lookup"><span data-stu-id="858e8-110">Visit the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>
   1. <span data-ttu-id="858e8-111">プレビューではない最新のランタイムを、**Runtime** (ランタイム) の一覧から選択します。</span><span class="sxs-lookup"><span data-stu-id="858e8-111">Select the latest non-preview runtime from the list under **Runtime**.</span></span>
   1. <span data-ttu-id="858e8-112">CentOS/Oracle の手順を選択し、それに従います。</span><span class="sxs-lookup"><span data-stu-id="858e8-112">Select and follow the instructions for CentOS/Oracle.</span></span>
1. <span data-ttu-id="858e8-113">既存の ASP.NET Core アプリ。</span><span class="sxs-lookup"><span data-stu-id="858e8-113">An existing ASP.NET Core app.</span></span>

## <a name="publish-and-copy-over-the-app"></a><span data-ttu-id="858e8-114">アプリを介して発行およびコピーする</span><span class="sxs-lookup"><span data-stu-id="858e8-114">Publish and copy over the app</span></span>

<span data-ttu-id="858e8-115">[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)用にアプリを構成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-115">Configure the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd).</span></span>

<span data-ttu-id="858e8-116">開発環境から [dotnet publish](/dotnet/core/tools/dotnet-publish) を実行し、サーバー上で実行できるディレクトリ (たとえば、*bin/Release/&lt;target_framework_moniker&gt;/publish*) にアプリをパッケージします。</span><span class="sxs-lookup"><span data-stu-id="858e8-116">Run [dotnet publish](/dotnet/core/tools/dotnet-publish) from the development environment to package an app into a directory (for example, *bin/Release/&lt;target_framework_moniker&gt;/publish*) that can run on the server:</span></span>

```console
dotnet publish --configuration Release
```

<span data-ttu-id="858e8-117">サーバーで .NET Core ランタイムを管理しない場合、アプリは[独立した展開](/dotnet/core/deploying/#self-contained-deployments-scd)として発行することもできます。</span><span class="sxs-lookup"><span data-stu-id="858e8-117">The app can also be published as a [self-contained deployment](/dotnet/core/deploying/#self-contained-deployments-scd) if you prefer not to maintain the .NET Core runtime on the server.</span></span>

<span data-ttu-id="858e8-118">組織のワークフローに統合されているツール (SCP や SFTP など) を使用して、サーバーに ASP.NET Core アプリをコピーします。</span><span class="sxs-lookup"><span data-stu-id="858e8-118">Copy the ASP.NET Core app to the server using a tool that integrates into the organization's workflow (for example, SCP, SFTP).</span></span> <span data-ttu-id="858e8-119">Web アプリは一般的に *var* ディレクトリの下に配置されます (たとえば、*var/aspnetcore/hellomvc*)。</span><span class="sxs-lookup"><span data-stu-id="858e8-119">It's common to locate web apps under the *var* directory (for example, *var/aspnetcore/hellomvc*).</span></span>

> [!NOTE]
> <span data-ttu-id="858e8-120">運用展開シナリオの場合、継続的インテグレーション ワークフローが、アプリの発行処理とサーバーへの資産のコピーを行います。</span><span class="sxs-lookup"><span data-stu-id="858e8-120">Under a production deployment scenario, a continuous integration workflow does the work of publishing the app and copying the assets to the server.</span></span>

## <a name="configure-a-proxy-server"></a><span data-ttu-id="858e8-121">プロキシ サーバーを構成する</span><span class="sxs-lookup"><span data-stu-id="858e8-121">Configure a proxy server</span></span>

<span data-ttu-id="858e8-122">リバース プロキシは、動的 Web アプリを提供するための一般的な仕組みです。</span><span class="sxs-lookup"><span data-stu-id="858e8-122">A reverse proxy is a common setup for serving dynamic web apps.</span></span> <span data-ttu-id="858e8-123">リバース プロキシは HTTP 要求を終了させ、ASP.NET アプリに転送します。</span><span class="sxs-lookup"><span data-stu-id="858e8-123">The reverse proxy terminates the HTTP request and forwards it to the ASP.NET app.</span></span>

<span data-ttu-id="858e8-124">プロキシ サーバーは、クライアント要求を処理せずに他のサーバーに転送するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="858e8-124">A proxy server is one which forwards client requests to another server instead of fulfilling requests itself.</span></span> <span data-ttu-id="858e8-125">リバース プロキシは、一般的に任意のクライアントに代わって固定の送信先に転送します。</span><span class="sxs-lookup"><span data-stu-id="858e8-125">A reverse proxy forwards to a fixed destination, typically on behalf of arbitrary clients.</span></span> <span data-ttu-id="858e8-126">このガイドでは、Kestrel が ASP.NET Core アプリを提供しているものと同じサーバー上で実行されるリバース プロキシとして Apache を構成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-126">In this guide, Apache is configured as the reverse proxy running on the same server that Kestrel is serving the ASP.NET Core app.</span></span>

<span data-ttu-id="858e8-127">要求はリバース プロキシによって転送されます。そのため、[Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) パッケージの [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) を使用します。</span><span class="sxs-lookup"><span data-stu-id="858e8-127">Because requests are forwarded by reverse proxy, use the [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package.</span></span> <span data-ttu-id="858e8-128">リダイレクト URI とその他のセキュリティ ポリシーを正しく機能させるために、このミドルウェアは、`X-Forwarded-Proto` ヘッダーを利用して、`Request.Scheme` を更新します。</span><span class="sxs-lookup"><span data-stu-id="858e8-128">The middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="858e8-129">認証、リンクの生成、リダイレクト、および地理的位置情報など、スキームに依存するすべてのコンポーネントは、Forwarded Headers Middleware の呼び出し後に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-129">Any component that depends on the scheme, such as authentication, link generation, redirects, and geolocation, must be placed after invoking the Forwarded Headers Middleware.</span></span> <span data-ttu-id="858e8-130">一般的な規則として、Forwarded Headers Middleware は、診断およびエラー処理ミドルウェアを除くその他のミドルウェアより前に実行される必要があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-130">As a general rule, Forwarded Headers Middleware should run before other middleware except diagnostics and error handling middleware.</span></span> <span data-ttu-id="858e8-131">この順序により、転送されるヘッダー情報に依存するミドルウェアが処理にヘッダー値を使用できます。</span><span class="sxs-lookup"><span data-stu-id="858e8-131">This ordering ensures that the middleware relying on forwarded headers information can consume the header values for processing.</span></span>

::: moniker range=">= aspnetcore-2.0"
> [!NOTE]
> <span data-ttu-id="858e8-132">リバース プロキシ サーバーの有無に関わらず、いずれの構成も有効で、ASP.NET Core 2.0 またはそれ以降のアプリ用にホスティング構成がサポートされている必要があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-132">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="858e8-133">詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="858e8-133">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>
::: moniker-end

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="858e8-134">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="858e8-134">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="858e8-135">[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) や同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="858e8-135">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="858e8-136">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="858e8-136">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="858e8-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="858e8-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="858e8-138">[UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) や [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication)、または同様の認証スキームのミドルウェアを呼び出す前に、`Startup.Configure` で [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="858e8-138">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling [UseIdentity](/dotnet/api/microsoft.aspnetcore.builder.builderextensions.useidentity) and [UseFacebookAuthentication](/dotnet/api/microsoft.aspnetcore.builder.facebookappbuilderextensions.usefacebookauthentication) or similar authentication scheme middleware.</span></span> <span data-ttu-id="858e8-139">ミドルウェアを構成して、`X-Forwarded-For` および `X-Forwarded-Proto` ヘッダーを転送します。</span><span class="sxs-lookup"><span data-stu-id="858e8-139">Configure the middleware to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers:</span></span>

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

<span data-ttu-id="858e8-140">ミドルウェアに対して [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) が指定されていない場合、転送される既定のヘッダーは `None` です。</span><span class="sxs-lookup"><span data-stu-id="858e8-140">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default headers to forward are `None`.</span></span>

<span data-ttu-id="858e8-141">プロキシ サーバーとロード バランサーの背後でホストされているアプリでは、追加の構成が必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-141">Additional configuration might be required for apps hosted behind proxy servers and load balancers.</span></span> <span data-ttu-id="858e8-142">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="858e8-142">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

### <a name="install-apache"></a><span data-ttu-id="858e8-143">Apache をインストールする</span><span class="sxs-lookup"><span data-stu-id="858e8-143">Install Apache</span></span>

<span data-ttu-id="858e8-144">CentOS パッケージを最新の安定したバージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="858e8-144">Update CentOS packages to their latest stable versions:</span></span>

```bash
sudo yum update -y
```

<span data-ttu-id="858e8-145">1 つの `yum` コマンドで、CentOS に Apache Web サーバーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="858e8-145">Install the Apache web server on CentOS with a single `yum` command:</span></span>

```bash
sudo yum -y install httpd mod_ssl
```

<span data-ttu-id="858e8-146">コマンド実行後の出力例:</span><span class="sxs-lookup"><span data-stu-id="858e8-146">Sample output after running the command:</span></span>

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
> <span data-ttu-id="858e8-147">この例では、CentOS 7 のバージョンが 64 ビットなので、出力は httpd.86_64 を反映しています。</span><span class="sxs-lookup"><span data-stu-id="858e8-147">In this example, the output reflects httpd.86_64 since the CentOS 7 version is 64 bit.</span></span> <span data-ttu-id="858e8-148">Apache がインストールされている場所を確認するには、コマンド プロンプトから `whereis httpd` を実行します。</span><span class="sxs-lookup"><span data-stu-id="858e8-148">To verify where Apache is installed, run `whereis httpd` from a command prompt.</span></span>

### <a name="configure-apache"></a><span data-ttu-id="858e8-149">Apache を構成する</span><span class="sxs-lookup"><span data-stu-id="858e8-149">Configure Apache</span></span>

<span data-ttu-id="858e8-150">Apache の構成ファイルは、`/etc/httpd/conf.d/` ディレクトリ内にあります。</span><span class="sxs-lookup"><span data-stu-id="858e8-150">Configuration files for Apache are located within the `/etc/httpd/conf.d/` directory.</span></span> <span data-ttu-id="858e8-151">`/etc/httpd/conf.modules.d/` 内のモジュール構成ファイルに加え、拡張子が *.conf* のファイルがアルファベット順で処理されます。このディレクトリには、モジュールの読み込みに必要な構成ファイルが含まれています。</span><span class="sxs-lookup"><span data-stu-id="858e8-151">Any file with the *.conf* extension is processed in alphabetical order in addition to the module configuration files in `/etc/httpd/conf.modules.d/`, which contains any configuration files necessary to load modules.</span></span>

<span data-ttu-id="858e8-152">アプリ用に *hellomvc.conf* という名前の構成ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-152">Create a configuration file, named *hellomvc.conf*, for the app:</span></span>

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
    ErrorLog ${APACHE_LOG_DIR}hellomvc-error.log
    CustomLog ${APACHE_LOG_DIR}hellomvc-access.log common
</VirtualHost>
```

<span data-ttu-id="858e8-153">`VirtualHost` ブロックは、サーバー上の 1 つまたは複数のファイルに複数回出現することができます。</span><span class="sxs-lookup"><span data-stu-id="858e8-153">The `VirtualHost` block can appear multiple times, in one or more files on a server.</span></span> <span data-ttu-id="858e8-154">上記の構成ファイルでは、Apache はポート 80 でパブリック トラフィックを受け入れます。</span><span class="sxs-lookup"><span data-stu-id="858e8-154">In the preceding configuration file, Apache accepts public traffic on port 80.</span></span> <span data-ttu-id="858e8-155">ドメイン `www.example.com` が提供されており、別名 `*.example.com` は同じ Web サイトに解決されます。</span><span class="sxs-lookup"><span data-stu-id="858e8-155">The domain `www.example.com` is being served, and the `*.example.com` alias resolves to the same website.</span></span> <span data-ttu-id="858e8-156">詳しくは、「[名前ベースのバーチャル ホスト](https://httpd.apache.org/docs/current/vhosts/name-based.html)」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="858e8-156">See [Name-based virtual host support](https://httpd.apache.org/docs/current/vhosts/name-based.html) for more information.</span></span> <span data-ttu-id="858e8-157">要求は、ルートにおいて、127.0.0.1 にあるサーバーのポート 5000 にプロキシされます。</span><span class="sxs-lookup"><span data-stu-id="858e8-157">Requests are proxied at the root to port 5000 of the server at 127.0.0.1.</span></span> <span data-ttu-id="858e8-158">双方向通信の場合は、`ProxyPass` と `ProxyPassReverse` が必要です。</span><span class="sxs-lookup"><span data-stu-id="858e8-158">For bi-directional communication, `ProxyPass` and `ProxyPassReverse` are required.</span></span> <span data-ttu-id="858e8-159">Kestrel の IP/ポートを変更する場合は、[Kestrel のエンドポイントの構成](xref:fundamentals/servers/kestrel#endpoint-configuration)に関するセクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="858e8-159">To change Kestrel's IP/port, see [Kestrel: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!WARNING]
> <span data-ttu-id="858e8-160">**VirtualHost** ブロックで適切な [ServerName ディレクティブ](https://httpd.apache.org/docs/current/mod/core.html#servername)を指定しないと、アプリにセキュリティ上の脆弱性が生じます。</span><span class="sxs-lookup"><span data-stu-id="858e8-160">Failure to specify a proper [ServerName directive](https://httpd.apache.org/docs/current/mod/core.html#servername) in the **VirtualHost** block exposes your app to security vulnerabilities.</span></span> <span data-ttu-id="858e8-161">親ドメイン全体を制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.example.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="858e8-161">Subdomain wildcard binding (for example, `*.example.com`) doesn't pose this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="858e8-162">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="858e8-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

<span data-ttu-id="858e8-163">`ErrorLog` および `CustomLog` ディレクティブを使って、`VirtualHost` ごとにログを構成できます。</span><span class="sxs-lookup"><span data-stu-id="858e8-163">Logging can be configured per `VirtualHost` using `ErrorLog` and `CustomLog` directives.</span></span> <span data-ttu-id="858e8-164">`ErrorLog` は、サーバーがエラーをログに記録する場所です。`CustomLog` には、ログ ファイルのファイル名と形式を設定します。</span><span class="sxs-lookup"><span data-stu-id="858e8-164">`ErrorLog` is the location where the server logs errors, and `CustomLog` sets the filename and format of log file.</span></span> <span data-ttu-id="858e8-165">この例では、要求の情報がログに記録される場所です。</span><span class="sxs-lookup"><span data-stu-id="858e8-165">In this case, this is where request information is logged.</span></span> <span data-ttu-id="858e8-166">1 つの要求につき 1 行が記録されます。</span><span class="sxs-lookup"><span data-stu-id="858e8-166">There's one line for each request.</span></span>

<span data-ttu-id="858e8-167">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="858e8-167">Save the file and test the configuration.</span></span> <span data-ttu-id="858e8-168">すべてに合格すると、応答は `Syntax [OK]` になります。</span><span class="sxs-lookup"><span data-stu-id="858e8-168">If everything passes, the response should be `Syntax [OK]`.</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="858e8-169">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="858e8-169">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
sudo systemctl enable httpd
```

## <a name="monitoring-the-app"></a><span data-ttu-id="858e8-170">アプリの監視</span><span class="sxs-lookup"><span data-stu-id="858e8-170">Monitoring the app</span></span>

<span data-ttu-id="858e8-171">これで、`http://localhost:80` に対して行われた要求を Kestrel で実行されている ASP.NET Core アプリ (`http://127.0.0.1:5000`) に転送するように Apache が設定されました。</span><span class="sxs-lookup"><span data-stu-id="858e8-171">Apache is now setup to forward requests made to `http://localhost:80` to the ASP.NET Core app running on Kestrel at `http://127.0.0.1:5000`.</span></span>  <span data-ttu-id="858e8-172">ただし、Apache は Kestrel プロセスを管理するようには設定されていません。</span><span class="sxs-lookup"><span data-stu-id="858e8-172">However, Apache isn't set up to manage the Kestrel process.</span></span> <span data-ttu-id="858e8-173">*systemd* を使って、基礎 Web アプリを起動して監視するサービス ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-173">Use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="858e8-174">*systemd* は init システムであり、プロセスを起動、停止、管理するためのさまざまな高性能機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="858e8-174">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="858e8-175">サービス ファイルを作成する</span><span class="sxs-lookup"><span data-stu-id="858e8-175">Create the service file</span></span>

<span data-ttu-id="858e8-176">次のように、サービス定義ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-176">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="858e8-177">アプリのサービス ファイルの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="858e8-177">An example service file for the app:</span></span>

```
[Unit]
Description=Example .NET Web API App running on CentOS 7

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
# Restart service after 10 seconds if the dotnet service crashes:
RestartSec=10
SyslogIdentifier=dotnet-example
User=apache
Environment=ASPNETCORE_ENVIRONMENT=Production 

[Install]
WantedBy=multi-user.target
```

> [!NOTE]
> <span data-ttu-id="858e8-178">**ユーザー** &mdash; *apache* が構成で使用されない場合、ユーザーを先に作成し、ファイルの適切な所有権を与える必要があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-178">**User** &mdash; If the user *apache* isn't used by the configuration, the user must be created first and given proper ownership for files.</span></span>

> [!NOTE]
> <span data-ttu-id="858e8-179">構成プロバイダーが環境変数を読み取れるようにするために、一部の値 (たとえば SQL の接続文字列) をエスケープする必要があります。</span><span class="sxs-lookup"><span data-stu-id="858e8-179">Some values (for example, SQL connection strings) must be escaped for the configuration providers to read the environment variables.</span></span> <span data-ttu-id="858e8-180">次のコマンドを使用して、構成ファイルで使用するために適切にエスケープされた値を生成します。</span><span class="sxs-lookup"><span data-stu-id="858e8-180">Use the following command to generate a properly escaped value for use in the configuration file:</span></span>
>
> ```console
> systemd-escape "<value-to-escape>"
> ```

<span data-ttu-id="858e8-181">ファイルを保存し、サービスを有効にします。</span><span class="sxs-lookup"><span data-stu-id="858e8-181">Save the file and enable the service:</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="858e8-182">サービスを起動し、動作を確認します。</span><span class="sxs-lookup"><span data-stu-id="858e8-182">Start the service and verify that it's running:</span></span>

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

<span data-ttu-id="858e8-183">リバース プロキシが構成され、Kestrel は *systemd* 経由で管理されます。これで Web アプリは完全に構成され、`http://localhost` でローカル コンピューター上のブラウザーからアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="858e8-183">With the reverse proxy configured and Kestrel managed through *systemd*, the web app is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="858e8-184">応答ヘッダーを調べると、**Server** ヘッダーでは ASP.NET Core アプリが Kestrel によって提供されていることが示されています。</span><span class="sxs-lookup"><span data-stu-id="858e8-184">Inspecting the response headers, the **Server** header indicates that the ASP.NET Core app is served by Kestrel:</span></span>

```
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="858e8-185">ログを表示する</span><span class="sxs-lookup"><span data-stu-id="858e8-185">Viewing logs</span></span>

<span data-ttu-id="858e8-186">Kestrel を使う Web アプリは *systemd* を使って管理されるため、イベントとプロセスは中央のジャーナルに記録されます。</span><span class="sxs-lookup"><span data-stu-id="858e8-186">Since the web app using Kestrel is managed using *systemd*, events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="858e8-187">ただし、このジャーナルには、*systemd* によって管理されるすべてのサービスとプロセスのエントリが含まれます。</span><span class="sxs-lookup"><span data-stu-id="858e8-187">However, this journal includes entries for all of the services and processes managed by *systemd*.</span></span> <span data-ttu-id="858e8-188">`kestrel-hellomvc.service` 固有の項目を表示するには、次のコマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="858e8-188">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="858e8-189">時間フィルタリングの場合は、コマンドで時間のオプションを指定します。</span><span class="sxs-lookup"><span data-stu-id="858e8-189">For time filtering, specify time options with the command.</span></span> <span data-ttu-id="858e8-190">たとえば、現在の日付でフィルター処理するには `--since today` を使い、前の 1 時間のエントリを参照するには `--until 1 hour ago` を使います。</span><span class="sxs-lookup"><span data-stu-id="858e8-190">For example, use `--since today` to filter for the current day or `--until 1 hour ago` to see the previous hour's entries.</span></span> <span data-ttu-id="858e8-191">詳しくは、[journalctl の man ページ](https://www.unix.com/man-page/centos/1/journalctl/)をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="858e8-191">For more information, see the [man page for journalctl](https://www.unix.com/man-page/centos/1/journalctl/).</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-the-app"></a><span data-ttu-id="858e8-192">アプリのセキュリティ保護</span><span class="sxs-lookup"><span data-stu-id="858e8-192">Securing the app</span></span>

### <a name="configure-firewall"></a><span data-ttu-id="858e8-193">ファイアウォールを構成する</span><span class="sxs-lookup"><span data-stu-id="858e8-193">Configure firewall</span></span>

<span data-ttu-id="858e8-194">*Firewalld* は、ネットワーク ゾーンをサポートするファイアウォールを管理するための動的デーモンです。</span><span class="sxs-lookup"><span data-stu-id="858e8-194">*Firewalld* is a dynamic daemon to manage the firewall with support for network zones.</span></span> <span data-ttu-id="858e8-195">ポートとパケットのフィルター処理は、iptables によって引き続き管理できます。</span><span class="sxs-lookup"><span data-stu-id="858e8-195">Ports and packet filtering can still be managed by iptables.</span></span> <span data-ttu-id="858e8-196">*Firewalld* は既定でインストールされます。</span><span class="sxs-lookup"><span data-stu-id="858e8-196">*Firewalld* should be installed by default.</span></span> <span data-ttu-id="858e8-197">`yum` を使ってパッケージをインストールしたり、インストールされていることを確認したりできます。</span><span class="sxs-lookup"><span data-stu-id="858e8-197">`yum` can be used to install the package or verify it's installed.</span></span>

```bash
sudo yum install firewalld -y
```

<span data-ttu-id="858e8-198">アプリに必要なポートのみを開くには、`firewalld` を使います。</span><span class="sxs-lookup"><span data-stu-id="858e8-198">Use `firewalld` to open only the ports needed for the app.</span></span> <span data-ttu-id="858e8-199">この場合、ポート 80 と 443 が使用されています。</span><span class="sxs-lookup"><span data-stu-id="858e8-199">In this case, port 80 and 443 are used.</span></span> <span data-ttu-id="858e8-200">次のコマンドは、ポート 80 と 443 が永続的に開かれるように設定します。</span><span class="sxs-lookup"><span data-stu-id="858e8-200">The following commands permanently set ports 80 and 443 to open:</span></span>

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
```

<span data-ttu-id="858e8-201">ファイアウォールの設定を再度読み込みます。</span><span class="sxs-lookup"><span data-stu-id="858e8-201">Reload the firewall settings.</span></span> <span data-ttu-id="858e8-202">既定のゾーンで使用できるサービスとポートを確認します。</span><span class="sxs-lookup"><span data-stu-id="858e8-202">Check the available services and ports in the default zone.</span></span> <span data-ttu-id="858e8-203">オプションは `firewall-cmd -h` を調べることで使用できます。</span><span class="sxs-lookup"><span data-stu-id="858e8-203">Options are available by inspecting `firewall-cmd -h`.</span></span>

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

### <a name="ssl-configuration"></a><span data-ttu-id="858e8-204">SSL の構成</span><span class="sxs-lookup"><span data-stu-id="858e8-204">SSL configuration</span></span>

<span data-ttu-id="858e8-205">SSL 用に Apache を構成するには、*mod_ssl* モジュールを使います。</span><span class="sxs-lookup"><span data-stu-id="858e8-205">To configure Apache for SSL, the *mod_ssl* module is used.</span></span> <span data-ttu-id="858e8-206">*httpd* モジュールがインストールされていると、*mod_ssl* モジュールもインストールされています。</span><span class="sxs-lookup"><span data-stu-id="858e8-206">When the *httpd* module was installed, the *mod_ssl* module was also installed.</span></span> <span data-ttu-id="858e8-207">インストールされていない場合は、`yum` を使って構成に追加します。</span><span class="sxs-lookup"><span data-stu-id="858e8-207">If it wasn't installed, use `yum` to add it to the configuration.</span></span>

```bash
sudo yum install mod_ssl
```

<span data-ttu-id="858e8-208">SSL を強制するには、`mod_rewrite` モジュールをインストールして URL の書き換えを有効にします。</span><span class="sxs-lookup"><span data-stu-id="858e8-208">To enforce SSL, install the `mod_rewrite` module to enable URL rewriting:</span></span>

```bash
sudo yum install mod_rewrite
```

<span data-ttu-id="858e8-209">*hellomvc.conf* ファイルを変更して、URL の書き換えを有効にし、ポート 443 での通信をセキュリティで保護します。</span><span class="sxs-lookup"><span data-stu-id="858e8-209">Modify the *hellomvc.conf* file to enable URL rewriting and secure communication on port 443:</span></span>

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
> <span data-ttu-id="858e8-210">この例では、ローカルで生成された証明書を使います。</span><span class="sxs-lookup"><span data-stu-id="858e8-210">This example is using a locally-generated certificate.</span></span> <span data-ttu-id="858e8-211">**SSLCertificateFile** は、ドメイン名のプライマリ証明書ファイルです。</span><span class="sxs-lookup"><span data-stu-id="858e8-211">**SSLCertificateFile** should be the primary certificate file for the domain name.</span></span> <span data-ttu-id="858e8-212">**SSLCertificateKeyFile** は、CSR の作成時に生成されるキー ファイルです。</span><span class="sxs-lookup"><span data-stu-id="858e8-212">**SSLCertificateKeyFile** should be the key file generated when CSR is created.</span></span> <span data-ttu-id="858e8-213">**SSLCertificateChainFile** は、証明機関から提供された中間証明書ファイル (存在する場合) です。</span><span class="sxs-lookup"><span data-stu-id="858e8-213">**SSLCertificateChainFile** should be the intermediate certificate file (if any) that was supplied by the certificate authority.</span></span>

<span data-ttu-id="858e8-214">ファイルを保存し、構成をテストします。</span><span class="sxs-lookup"><span data-stu-id="858e8-214">Save the file and test the configuration:</span></span>

```bash
sudo service httpd configtest
```

<span data-ttu-id="858e8-215">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="858e8-215">Restart Apache:</span></span>

```bash
sudo systemctl restart httpd
```

## <a name="additional-apache-suggestions"></a><span data-ttu-id="858e8-216">Apache に関するその他の推奨事項</span><span class="sxs-lookup"><span data-stu-id="858e8-216">Additional Apache suggestions</span></span>

### <a name="additional-headers"></a><span data-ttu-id="858e8-217">その他のヘッダー</span><span class="sxs-lookup"><span data-stu-id="858e8-217">Additional headers</span></span>

<span data-ttu-id="858e8-218">悪意のある攻撃から防御するために、変更または追加する必要があるヘッダーがいくつかあります。</span><span class="sxs-lookup"><span data-stu-id="858e8-218">In order to secure against malicious attacks, there are a few headers that should either be modified or added.</span></span> <span data-ttu-id="858e8-219">必ず `mod_headers` モジュールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="858e8-219">Ensure that the `mod_headers` module is installed:</span></span>

```bash
sudo yum install mod_headers
```

#### <a name="secure-apache-from-clickjacking-attacks"></a><span data-ttu-id="858e8-220">Apache をクリックジャッキング攻撃から保護する</span><span class="sxs-lookup"><span data-stu-id="858e8-220">Secure Apache from clickjacking attacks</span></span>

<span data-ttu-id="858e8-221">[クリックジャッキング](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger)は "*UI 着せ替え攻撃*" とも呼ばれ、Web サイトの訪問者を騙して現在訪れているものとは異なるページのリンクやボタンをクリックさせる悪意のある攻撃です。</span><span class="sxs-lookup"><span data-stu-id="858e8-221">[Clickjacking](https://blog.qualys.com/securitylabs/2015/10/20/clickjacking-a-common-implementation-mistake-that-can-put-your-websites-in-danger), also known as a *UI redress attack*, is a malicious attack where a website visitor is tricked into clicking a link or button on a different page than they're currently visiting.</span></span> <span data-ttu-id="858e8-222">サイトをセキュリティで保護するには、`X-FRAME-OPTIONS` を使います。</span><span class="sxs-lookup"><span data-stu-id="858e8-222">Use `X-FRAME-OPTIONS` to secure the site.</span></span>

<span data-ttu-id="858e8-223">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="858e8-223">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="858e8-224">行 `Header append X-FRAME-OPTIONS "SAMEORIGIN"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="858e8-224">Add the line `Header append X-FRAME-OPTIONS "SAMEORIGIN"`.</span></span> <span data-ttu-id="858e8-225">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="858e8-225">Save the file.</span></span> <span data-ttu-id="858e8-226">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="858e8-226">Restart Apache.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="858e8-227">MIME タイプ スニッフィング</span><span class="sxs-lookup"><span data-stu-id="858e8-227">MIME-type sniffing</span></span>

<span data-ttu-id="858e8-228">`X-Content-Type-Options` ヘッダーは、Internet Explorer を "*MIME スニッフィング*" から防ぎます (ファイルの内容からファイルの `Content-Type` を判断します)。</span><span class="sxs-lookup"><span data-stu-id="858e8-228">The `X-Content-Type-Options` header prevents Internet Explorer from *MIME-sniffing* (determining a file's `Content-Type` from the file's content).</span></span> <span data-ttu-id="858e8-229">サーバーが `nosniff` オプションを指定して `Content-Type` ヘッダーを `text/html` に設定すると、Internet Explorer はファイルの内容に関係なく `text/html` として内容をレンダリングします。</span><span class="sxs-lookup"><span data-stu-id="858e8-229">If the server sets the `Content-Type` header to `text/html` with the `nosniff` option set, Internet Explorer renders the content as `text/html` regardless of the file's content.</span></span>

<span data-ttu-id="858e8-230">*httpd.conf* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="858e8-230">Edit the *httpd.conf* file:</span></span>

```bash
sudo nano /etc/httpd/conf/httpd.conf
```

<span data-ttu-id="858e8-231">行 `Header set X-Content-Type-Options "nosniff"` を追加します。</span><span class="sxs-lookup"><span data-stu-id="858e8-231">Add the line `Header set X-Content-Type-Options "nosniff"`.</span></span> <span data-ttu-id="858e8-232">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="858e8-232">Save the file.</span></span> <span data-ttu-id="858e8-233">Apache を再起動します。</span><span class="sxs-lookup"><span data-stu-id="858e8-233">Restart Apache.</span></span>

### <a name="load-balancing"></a><span data-ttu-id="858e8-234">負荷分散</span><span class="sxs-lookup"><span data-stu-id="858e8-234">Load Balancing</span></span>

<span data-ttu-id="858e8-235">この例では、同じインスタンス コンピューターに CentOS 7 と Kestrel をインストールし、Apache をセットアップおよび構成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="858e8-235">This example shows how to setup and configure Apache on CentOS 7 and Kestrel on the same instance machine.</span></span> <span data-ttu-id="858e8-236">単一障害点を持たないように、*mod_proxy_balancer* を使い、**VirtualHost** を変更することで、Apache プロキシ サーバーの背後で複数インスタンスの Web アプを管理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="858e8-236">In order to not have a single point of failure; using *mod_proxy_balancer* and modifying the **VirtualHost** would allow for managing multiple instances of the web apps behind the Apache proxy server.</span></span>

```bash
sudo yum install mod_proxy_balancer
```

<span data-ttu-id="858e8-237">次に示す構成ファイルでは、ポート 5001 上で実行するように `hellomvc` アプリの追加インスタンスを設定しています。</span><span class="sxs-lookup"><span data-stu-id="858e8-237">In the configuration file shown below, an additional instance of the `hellomvc` app is setup to run on port 5001.</span></span> <span data-ttu-id="858e8-238">*Proxy* セクションは、2 メンバーのバランサー構成を使って *byrequests* を負荷分散するように設定されています。</span><span class="sxs-lookup"><span data-stu-id="858e8-238">The *Proxy* section is set with a balancer configuration with two members to load balance *byrequests*.</span></span>

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
    ErrorLog /var/log/httpd/hellomvc-error.log
    CustomLog /var/log/httpd/hellomvc-access.log common
    SSLEngine on
    SSLProtocol all -SSLv2
    SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:!RC4+RSA:+HIGH:+MEDIUM:!LOW:!RC4
    SSLCertificateFile /etc/pki/tls/certs/localhost.crt
    SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
</VirtualHost>
```

### <a name="rate-limits"></a><span data-ttu-id="858e8-239">速度の制限</span><span class="sxs-lookup"><span data-stu-id="858e8-239">Rate Limits</span></span>

<span data-ttu-id="858e8-240">*httpd* モジュールに含まれる *mod_ratelimit* を使って、クライアントの帯域幅を制限できます。</span><span class="sxs-lookup"><span data-stu-id="858e8-240">Using *mod_ratelimit*, which is included in the *httpd* module, the bandwidth of clients can be limited:</span></span>

```bash
sudo nano /etc/httpd/conf.d/ratelimit.conf
```
<span data-ttu-id="858e8-241">次のファイルの例では、ルートの場所の下での帯域幅を 600 KB/秒に制限しています。</span><span class="sxs-lookup"><span data-stu-id="858e8-241">The example file limits bandwidth as 600 KB/sec under the root location:</span></span>

```
<IfModule mod_ratelimit.c>
    <Location />
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 600
    </Location>
</IfModule>
```

## <a name="additional-resources"></a><span data-ttu-id="858e8-242">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="858e8-242">Additional resources</span></span>

* [<span data-ttu-id="858e8-243">プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する</span><span class="sxs-lookup"><span data-stu-id="858e8-243">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
