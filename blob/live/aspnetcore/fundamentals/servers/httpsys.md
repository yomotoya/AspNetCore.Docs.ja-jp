---
title: "ASP.NET Core の HTTP.sys web サーバーの実装"
author: rick-anderson
description: "HTTP.sys は、Windows 上の ASP.NET core web サーバーが導入されています。 Http.Sys のカーネル モード ドライバーでビルドする HTTP.sys は、代わりに IIS なしでインターネットに直接接続に使用することができます Kestrel です。"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="1d310-104">ASP.NET Core の HTTP.sys web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="1d310-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="1d310-105">によって[Tom Dykstra](https://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="1d310-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="1d310-106">このトピックには、のみを ASP.NET Core 2.0 以降が適用されます。</span><span class="sxs-lookup"><span data-stu-id="1d310-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="1d310-107">以前のバージョンの ASP.NET Core、HTTP.sys が名前付き[WebListener](xref:fundamentals/servers/weblistener)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="1d310-108">HTTP.sys は、 [ASP.NET core web server](index.md) Windows だけで実行されています。</span><span class="sxs-lookup"><span data-stu-id="1d310-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="1d310-109">に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="1d310-110">HTTP.sys は、代わりに[Kestrel](kestrel.md) Kestel しない一部の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d310-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="1d310-111">**HTTP.sys と互換性がありません、IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**</span><span class="sxs-lookup"><span data-stu-id="1d310-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="1d310-112">HTTP.sys は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="1d310-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="1d310-113">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="1d310-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="1d310-114">ポート共有</span><span class="sxs-lookup"><span data-stu-id="1d310-114">Port sharing</span></span>
- <span data-ttu-id="1d310-115">SNI を HTTPS</span><span class="sxs-lookup"><span data-stu-id="1d310-115">HTTPS with SNI</span></span>
- <span data-ttu-id="1d310-116">Http/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="1d310-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="1d310-117">ファイルを直接転送</span><span class="sxs-lookup"><span data-stu-id="1d310-117">Direct file transmission</span></span>
- <span data-ttu-id="1d310-118">応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="1d310-118">Response caching</span></span>
- <span data-ttu-id="1d310-119">Websocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="1d310-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="1d310-120">サポートされている Windows のバージョン:</span><span class="sxs-lookup"><span data-stu-id="1d310-120">Supported Windows versions:</span></span>

- <span data-ttu-id="1d310-121">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="1d310-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="1d310-122">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="1d310-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="1d310-123">HTTP.sys を使用する場合</span><span class="sxs-lookup"><span data-stu-id="1d310-123">When to use HTTP.sys</span></span>

<span data-ttu-id="1d310-124">HTTP.sys は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="1d310-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="1d310-126">Http.Sys で用意されているので HTTP.sys は、攻撃に対する保護のため、リバース プロキシ サーバーを必要としません。</span><span class="sxs-lookup"><span data-stu-id="1d310-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="1d310-127">Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="1d310-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="1d310-128">Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="1d310-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="1d310-129">HTTP.sys をお勧めの内部の展開では使用できない Kestrel、Windows 認証などの機能を必要とするときにします。</span><span class="sxs-lookup"><span data-stu-id="1d310-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="1d310-131">HTTP.sys を使用する方法</span><span class="sxs-lookup"><span data-stu-id="1d310-131">How to use HTTP.sys</span></span>

<span data-ttu-id="1d310-132">次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="1d310-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="1d310-133">Windows Server を構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-133">Configure Windows Server</span></span>

* <span data-ttu-id="1d310-134">など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://www.microsoft.com/net/download/core)または[.NET Framework](https://www.microsoft.com/net/download/framework)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="1d310-135">HTTP.sys は、バインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。</span><span class="sxs-lookup"><span data-stu-id="1d310-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="1d310-136">Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。</span><span class="sxs-lookup"><span data-stu-id="1d310-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="1d310-137">唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="1d310-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="1d310-138">詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="1d310-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="1d310-139">HTTP.sys に到達するトラフィックを許可するファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="1d310-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="1d310-140">使用することができます*netsh.exe*または[PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="1d310-141">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="1d310-142">HTTP.sys を使用する ASP.NET Core アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="1d310-143">使用する場合は、パッケージのインストールは必要ありません、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage です。</span><span class="sxs-lookup"><span data-stu-id="1d310-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="1d310-144">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) metapackage でパッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="1d310-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="1d310-145">呼び出す、`UseHttpSys`拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[HTTP.sys オプション](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)する必要がある、次の例で示すように。</span><span class="sxs-lookup"><span data-stu-id="1d310-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="1d310-146">HTTP.sys のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="1d310-147">HTTP.sys の設定および構成できる制限のいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="1d310-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="1d310-148">**最大クライアント接続**</span><span class="sxs-lookup"><span data-stu-id="1d310-148">**Maximum client connections**</span></span>

<span data-ttu-id="1d310-149">次のコードを含むアプリケーション全体の同時実行の開いている TCP 接続の最大数を設定することができます*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d310-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="1d310-150">接続の最大数は、既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="1d310-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="1d310-151">**最大要求本文のサイズ**</span><span class="sxs-lookup"><span data-stu-id="1d310-151">**Maximum request body size**</span></span>

<span data-ttu-id="1d310-152">既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。</span><span class="sxs-lookup"><span data-stu-id="1d310-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="1d310-153">ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="1d310-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="1d310-154">全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="1d310-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="1d310-155">内の特定の要求に設定を上書きできます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d310-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="1d310-156">アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="1d310-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="1d310-157">`IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。</span><span class="sxs-lookup"><span data-stu-id="1d310-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="1d310-158">その他の HTTP.sys オプションについては、次を参照してください。 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="1d310-159">Url とポートでリッスンするように構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="1d310-160">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="1d310-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="1d310-161">URL プレフィックスとポートを構成するのに使用することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数では、ASPNETCORE_URLS 環境変数、または`UrlPrefixes`プロパティ[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="1d310-162">次のコード例では`UrlPrefixes`します。</span><span class="sxs-lookup"><span data-stu-id="1d310-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="1d310-163">利点`UrlPrefixes`が正しくフォーマットされているプレフィックスを追加しようとする場合にすぐに、エラー メッセージを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="1d310-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="1d310-164">利点`UseUrls`(とは共有`urls`と ASPNETCORE_URLS) は Kestrel と HTTP.sys の間でより簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="1d310-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="1d310-165">両方を使用する場合`UseUrls`(または`urls`または ASPNETCORE_URLS) と`UrlPrefixes`、設定`UrlPrefixes`内のオーバーライド`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="1d310-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="1d310-166">詳細については、[ホスティング](xref:fundamentals/hosting)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d310-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="1d310-167">HTTP.sys を使用して、[文字列形式の HTTP サーバー API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="1d310-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="1d310-168">内の同じプレフィックス文字列を指定することを確認してください`UseUrls`または`UrlPrefixes`サーバーに事前登録します。</span><span class="sxs-lookup"><span data-stu-id="1d310-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="1d310-169">IIS を使用しません。</span><span class="sxs-lookup"><span data-stu-id="1d310-169">Don't use IIS</span></span>

<span data-ttu-id="1d310-170">IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="1d310-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="1d310-171">Visual Studio では、既定の起動プロファイルは、IIS express はします。</span><span class="sxs-lookup"><span data-stu-id="1d310-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="1d310-172">コンソール アプリケーションとしてプロジェクトを実行するには、手動で変更、選択したプロファイルの次のスクリーン ショットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="1d310-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![コンソール アプリのプロファイルを選択します。](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="1d310-174">URL プレフィックスを事前登録し、SSL を構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="1d310-175">IIS と HTTP.sys のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。</span><span class="sxs-lookup"><span data-stu-id="1d310-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="1d310-176">IIS では、management UI では、すべての構成に比較的簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="1d310-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="1d310-177">ただし、Http.Sys を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1d310-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="1d310-178">つまりを行うための組み込みツール*netsh.exe*です。</span><span class="sxs-lookup"><span data-stu-id="1d310-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="1d310-179">*Netsh.exe* URL プレフィックスを予約し、SSL 証明書を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="1d310-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="1d310-180">このツールには、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="1d310-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="1d310-181">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小値を示しています。</span><span class="sxs-lookup"><span data-stu-id="1d310-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="1d310-182">次の例では、SSL 証明書を割り当てる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1d310-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="1d310-183">ここでは、リファレンス ドキュメントを*netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="1d310-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="1d310-184">ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)</span><span class="sxs-lookup"><span data-stu-id="1d310-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="1d310-185">UrlPrefix 文字列</span><span class="sxs-lookup"><span data-stu-id="1d310-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="1d310-186">次のリソースは、いくつかのシナリオの詳細な手順を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d310-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="1d310-187">HttpListener を参照している記事は、Http.Sys に基づいている両方に、HTTP.sys に同じように適用します。</span><span class="sxs-lookup"><span data-stu-id="1d310-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="1d310-188">方法 : SSL 証明書を使用してポートを構成する</span><span class="sxs-lookup"><span data-stu-id="1d310-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="1d310-189">[HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。</span><span class="sxs-lookup"><span data-stu-id="1d310-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="1d310-190">[方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)有用な情報で以前のブログをすぎますがこれです。</span><span class="sxs-lookup"><span data-stu-id="1d310-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="1d310-191">ここより簡単に使用できる一部のサード パーティ製ツール、 *netsh.exe*コマンドライン。</span><span class="sxs-lookup"><span data-stu-id="1d310-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="1d310-192">によって提供されるか、Microsoft によって承認されているこれらがありません。</span><span class="sxs-lookup"><span data-stu-id="1d310-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="1d310-193">ツールは、実行管理者として既定では、ので*netsh.exe*自体には、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="1d310-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="1d310-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="1d310-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="1d310-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="1d310-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="1d310-196">UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="1d310-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="1d310-197">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="1d310-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="1d310-198">次の手順</span><span class="sxs-lookup"><span data-stu-id="1d310-198">Next steps</span></span>

<span data-ttu-id="1d310-199">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="1d310-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="1d310-200">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="1d310-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="1d310-201">HTTP.sys のソース コード</span><span class="sxs-lookup"><span data-stu-id="1d310-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="1d310-202">ホスティング</span><span class="sxs-lookup"><span data-stu-id="1d310-202">Hosting</span></span>](xref:fundamentals/hosting)
