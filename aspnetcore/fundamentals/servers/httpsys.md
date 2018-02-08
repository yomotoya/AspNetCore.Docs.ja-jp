---
title: "ASP.NET Core での HTTP.sys Web サーバーの実装"
author: rick-anderson
description: "Windows 上の ASP.NET Core 用 Web サーバーである HTTP.sys について紹介します。 HTTP.sys は、Http.Sys カーネル モード ドライバーに基づいて構築され、IIS なしでインターネットに直接接続するために使用できる Kestrel の代替製品です。"
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="d98b8-104">ASP.NET Core での HTTP.sys Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="d98b8-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="d98b8-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="d98b8-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="d98b8-106">このトピックは、ASP.NET Core 2.0 以降にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="d98b8-107">以前のバージョンの ASP.NET Core では、HTTP.sys は [WebListener](xref:fundamentals/servers/weblistener) という名前です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="d98b8-108">HTTP.sys は Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="d98b8-109">[Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づいて構築されています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="d98b8-110">HTTP.sys は、Kestel では提供されないいくつかの機能を提供する [Kestrel](kestrel.md) の代替製品です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="d98b8-111">**HTTP.sys は [ASP.NET Core モジュール](aspnet-core-module.md)と互換性がないため、IIS や IIS Express で使用することはできません。**</span><span class="sxs-lookup"><span data-stu-id="d98b8-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="d98b8-112">HTTP.sys は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="d98b8-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="d98b8-113">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="d98b8-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="d98b8-114">ポート共有</span><span class="sxs-lookup"><span data-stu-id="d98b8-114">Port sharing</span></span>
- <span data-ttu-id="d98b8-115">SNI を使用する HTTPS</span><span class="sxs-lookup"><span data-stu-id="d98b8-115">HTTPS with SNI</span></span>
- <span data-ttu-id="d98b8-116">HTTP/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="d98b8-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="d98b8-117">直接ファイル伝送</span><span class="sxs-lookup"><span data-stu-id="d98b8-117">Direct file transmission</span></span>
- <span data-ttu-id="d98b8-118">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="d98b8-118">Response caching</span></span>
- <span data-ttu-id="d98b8-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="d98b8-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="d98b8-120">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="d98b8-120">Supported Windows versions:</span></span>

- <span data-ttu-id="d98b8-121">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="d98b8-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="d98b8-122">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="d98b8-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="d98b8-123">HTTP.sys を使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="d98b8-123">When to use HTTP.sys</span></span>

<span data-ttu-id="d98b8-124">HTTP.sys は、IIS を使用せずにインターネットにサーバーを直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="d98b8-126">HTTP.sys は Http.Sys に基づいて構築されているため、攻撃から保護するためのリバース プロキシ サーバーは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d98b8-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="d98b8-127">Http.Sys とは、さまざまな種類の攻撃から保護し、完全な機能を備えた Web サーバーの堅牢性、セキュリティ、およびスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="d98b8-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="d98b8-128">IIS 自体が Http.Sys 上で HTTP リスナーとして実行されています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="d98b8-129">HTTP.sys は、Windows 認証など、Kestrel で使用できない機能が必要な場合の内部展開に適しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="d98b8-131">HTTP.sys の使用方法</span><span class="sxs-lookup"><span data-stu-id="d98b8-131">How to use HTTP.sys</span></span>

<span data-ttu-id="d98b8-132">ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要について説明します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="d98b8-133">Windows Server を構成する</span><span class="sxs-lookup"><span data-stu-id="d98b8-133">Configure Windows Server</span></span>

* <span data-ttu-id="d98b8-134">[.NET Core](https://www.microsoft.com/net/download/core) や [.NET Framework](https://www.microsoft.com/net/download/framework) など、アプリケーションに必要な .NET のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="d98b8-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="d98b8-135">HTTP.sys にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します</span><span class="sxs-lookup"><span data-stu-id="d98b8-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="d98b8-136">Windows で URL プレフィックスを事前登録していない場合は、管理者特権でアプリケーションを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="d98b8-137">唯一の例外は、1024 より大きいポート番号で (HTTPS ではなく) HTTP を使用して localhost にバインドする場合です。この場合、管理者権限は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="d98b8-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="d98b8-138">詳細については、この記事で後述する「[URL プレフィックスの事前登録と SSL の構成](#preregister-url-prefixes-and-configure-ssl)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d98b8-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="d98b8-139">トラフィックが HTTP.sys に到達できるようにファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="d98b8-140">*netsh.exe* または [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)を使用できます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="d98b8-141">[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="d98b8-142">HTTP.sys を使用するように ASP.NET Core アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="d98b8-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="d98b8-143">[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) メタパッケージを使用する場合は、パッケージのインストールは必要はありません。</span><span class="sxs-lookup"><span data-stu-id="d98b8-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="d98b8-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) パッケージはメタパッケージに含まれています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="d98b8-145">次の例のように、`Main` メソッド内の `WebHostBuilder` で `UseHttpSys` 拡張メソッドを呼び出して、必要な [HTTP.sys オプション](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)を指定します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="d98b8-146">HTTP.sys オプションを構成する</span><span class="sxs-lookup"><span data-stu-id="d98b8-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="d98b8-147">構成できる HTTP.sys の設定と制限をいくつか以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="d98b8-148">**クライアントの最大接続数**</span><span class="sxs-lookup"><span data-stu-id="d98b8-148">**Maximum client connections**</span></span>

<span data-ttu-id="d98b8-149">*Program.cs* で次のコードを使用することで、アプリケーション全体に対して同時に開かれる TCP 接続の最大数を設定できます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="d98b8-150">接続の最大数は既定では無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="d98b8-151">**要求本文の最大サイズ**</span><span class="sxs-lookup"><span data-stu-id="d98b8-151">**Maximum request body size**</span></span>

<span data-ttu-id="d98b8-152">既定の要求本文の最大サイズは、30,000,000 バイトです。これは約 28.6 MB になります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="d98b8-153">ASP.NET Core MVC アプリでの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) 属性を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="d98b8-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="d98b8-154">次の例では、アプリケーション全体を対象にしてすべての要求に対する制約を構成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="d98b8-155">*Startup.cs* の特定の要求に関する設定をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="d98b8-156">アプリケーションで要求の読み取りが開始された後、要求に対する制限を構成しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="d98b8-157">`MaxRequestBodySize` プロパティが読み取り専用状態にある (制限を構成するには遅すぎる) かどうかを知らせる `IsReadOnly` プロパティがあります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="d98b8-158">その他の HTTP.sys オプションについては、「[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="d98b8-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="d98b8-159">リッスンする URL とポートを構成する</span><span class="sxs-lookup"><span data-stu-id="d98b8-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="d98b8-160">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="d98b8-161">URL プレフィックスとポートを構成する場合、`UseUrls` 拡張メソッド、`urls` コマンドライン引数、ASPNETCORE_URLS 環境引数、または「[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)」の `UrlPrefixes` プロパティを使用できます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="d98b8-162">`UrlPrefixes` を使用するコード例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="d98b8-163">`UrlPrefixes` の利点は、正しくフォーマットされていないプレフィックスを追加しようとした場合にすぐにエラー メッセージが表示されることです。</span><span class="sxs-lookup"><span data-stu-id="d98b8-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="d98b8-164">`UseUrls` (`urls` と ASPNETCORE_URLS で共有) の利点は、Kestrel と HTTP.sys をより簡単に切り替えられることです。</span><span class="sxs-lookup"><span data-stu-id="d98b8-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="d98b8-165">`UseUrls` (または `urls` あるいは ASPNETCORE_URLS) と `UrlPrefixes` を両方使用する場合、`UrlPrefixes` の設定で `UseUrls` の設定がオーバーライドされます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="d98b8-166">詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d98b8-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="d98b8-167">HTTP.sys では、[HTTP サーバー API の UrlPrefix 文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)が使用されます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d98b8-168">サーバーで事前登録する `UseUrls` または `UrlPrefixes` に同じプレフィックス文字列を指定してください。</span><span class="sxs-lookup"><span data-stu-id="d98b8-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="d98b8-169">IIS を使用しない</span><span class="sxs-lookup"><span data-stu-id="d98b8-169">Don't use IIS</span></span>

<span data-ttu-id="d98b8-170">アプリケーションが IIS または IIS Express を実行するように構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="d98b8-171">Visual Studio では、既定の起動プロファイルは IIS Express 用です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="d98b8-172">プロジェクトをコンソール アプリケーションとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![コンソール アプリのプロファイルを選択する](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="d98b8-174">URL プレフィックスの事前登録と SSL の構成</span><span class="sxs-lookup"><span data-stu-id="d98b8-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="d98b8-175">IIS と HTTP.sys はいずれも、要求をリッスンして初期処理を行うために、基になる Http.Sys カーネル モード ドライバーに依存しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="d98b8-176">IIS では、管理 UI を使用すると、すべての構成を比較的簡単に実行できます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="d98b8-177">ただし、Http.Sys を自分で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="d98b8-178">この処理に使用する組み込みツールは *netsh.exe* です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="d98b8-179">*netsh.exe* を使用して、URL プレフィックスを予約し、SSL 証明書を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="d98b8-180">このツールには管理特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="d98b8-181">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小限の値を示しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="d98b8-182">次の例は、SSL 証明書を割り当てる方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="d98b8-183">以下は、*netsh.exe* のリファレンス ドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="d98b8-183">Here is the reference documentation for *netsh.exe*:</span></span>

* <span data-ttu-id="d98b8-184">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)</span><span class="sxs-lookup"><span data-stu-id="d98b8-184">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
* <span data-ttu-id="d98b8-185">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)</span><span class="sxs-lookup"><span data-stu-id="d98b8-185">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

<span data-ttu-id="d98b8-186">次のリソースでは、いくつかのシナリオの詳細な手順を説明しています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="d98b8-187">HttpListener を参照する記事は、両方とも Http.Sys に基づいているため、HTTP.sys に同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="d98b8-188">方法 : SSL 証明書を使用してポートを構成する</span><span class="sxs-lookup"><span data-stu-id="d98b8-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="d98b8-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通信 - HttpListener ベースのホスティングとクライアントの認証): サードパーティのブログであり、かなり古い投稿ですが、有用な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="d98b8-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (方法: SSL Simple Server として HttpListener または Http Server アンマネージ コード (C++) を使用するチュートリアル): これも古いブログですが、有用な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="d98b8-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="d98b8-191">*netsh.exe* コマンド ラインよりも使いやすいサードパーティ製ツールをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="d98b8-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="d98b8-192">これらのツールは Microsoft 製ではなく、Microsoft が推奨するものでもありません。</span><span class="sxs-lookup"><span data-stu-id="d98b8-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="d98b8-193">*netsh.exe* 自体に管理者特権が必要なので、これらのツールは既定で管理者として実行されます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="d98b8-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) には、SSL 証明書とオプション、プレフィックス予約、および証明書信頼リストを一覧表示および構成するための UI があります。</span><span class="sxs-lookup"><span data-stu-id="d98b8-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="d98b8-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) では、SSL 証明書と URL プレフィックスを一覧表示したり、設定することができます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="d98b8-196">UI は http.sys Manager より洗練されており、公開されている構成オプションも少し多くなっていますが、その他の機能は同様です。</span><span class="sxs-lookup"><span data-stu-id="d98b8-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="d98b8-197">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のものを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="d98b8-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="d98b8-198">次の手順</span><span class="sxs-lookup"><span data-stu-id="d98b8-198">Next steps</span></span>

<span data-ttu-id="d98b8-199">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="d98b8-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="d98b8-200">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="d98b8-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="d98b8-201">HTTP.sys のソース コード</span><span class="sxs-lookup"><span data-stu-id="d98b8-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="d98b8-202">ホスティング</span><span class="sxs-lookup"><span data-stu-id="d98b8-202">Hosting</span></span>](xref:fundamentals/hosting)
