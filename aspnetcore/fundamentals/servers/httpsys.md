---
title: "ASP.NET Core の HTTP.sys web サーバーの実装"
author: rick-anderson
description: "HTTP.sys は、Windows 上の ASP.NET core web サーバーが導入されています。 Http.Sys のカーネル モード ドライバーでビルドする HTTP.sys は、代わりに IIS なしでインターネットに直接接続に使用することができます Kestrel です。"
keywords: "ASP.NET Core、HttpSys、HTTP.sys、HttpListener、SSL url プレフィックス"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: cff6f171432febac5ec3e7adf9cf77953e0ece2d
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2017
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="28dd4-105">ASP.NET Core の HTTP.sys web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="28dd4-105">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="28dd4-106">によって[Tom Dykstra](http://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="28dd4-106">By [Tom Dykstra](http://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="28dd4-107">このトピックには、のみを ASP.NET Core 2.0 以降が適用されます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-107">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="28dd4-108">以前のバージョンの ASP.NET Core、HTTP.sys が名前付き[WebListener](xref:fundamentals/servers/weblistener)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-108">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="28dd4-109">HTTP.sys は、 [ASP.NET core web server](index.md) Windows だけで実行されています。</span><span class="sxs-lookup"><span data-stu-id="28dd4-109">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="28dd4-110">に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="28dd4-111">HTTP.sys は、代わりに[Kestrel](kestrel.md) Kestel しない一部の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-111">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="28dd4-112">**HTTP.sys と互換性がないと IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**</span><span class="sxs-lookup"><span data-stu-id="28dd4-112">**HTTP.sys can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="28dd4-113">HTTP.sys は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="28dd4-113">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="28dd4-114">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="28dd4-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="28dd4-115">ポート共有</span><span class="sxs-lookup"><span data-stu-id="28dd4-115">Port sharing</span></span>
- <span data-ttu-id="28dd4-116">SNI を HTTPS</span><span class="sxs-lookup"><span data-stu-id="28dd4-116">HTTPS with SNI</span></span>
- <span data-ttu-id="28dd4-117">Http/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="28dd4-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="28dd4-118">ファイルを直接転送</span><span class="sxs-lookup"><span data-stu-id="28dd4-118">Direct file transmission</span></span>
- <span data-ttu-id="28dd4-119">応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="28dd4-119">Response caching</span></span>
- <span data-ttu-id="28dd4-120">Websocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="28dd4-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="28dd4-121">サポートされている Windows のバージョン:</span><span class="sxs-lookup"><span data-stu-id="28dd4-121">Supported Windows versions:</span></span>

- <span data-ttu-id="28dd4-122">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="28dd4-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="28dd4-123">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="28dd4-123">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)

## <a name="when-to-use-httpsys"></a><span data-ttu-id="28dd4-124">HTTP.sys を使用する場合</span><span class="sxs-lookup"><span data-stu-id="28dd4-124">When to use HTTP.sys</span></span>

<span data-ttu-id="28dd4-125">HTTP.sys は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-125">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys がインターネットに直接通信します。](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="28dd4-127">Http.Sys で用意されているので HTTP.sys は、攻撃に対する保護のため、リバース プロキシ サーバーを必要としません。</span><span class="sxs-lookup"><span data-stu-id="28dd4-127">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="28dd4-128">Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="28dd4-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="28dd4-129">Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="28dd4-130">HTTP.sys をお勧めの内部の展開では使用できない Kestrel、Windows 認証などの機能を必要とするときにします。</span><span class="sxs-lookup"><span data-stu-id="28dd4-130">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys が内部ネットワークに直接通信します。](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="28dd4-132">HTTP.sys を使用する方法</span><span class="sxs-lookup"><span data-stu-id="28dd4-132">How to use HTTP.sys</span></span>

<span data-ttu-id="28dd4-133">次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="28dd4-134">Windows Server を構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-134">Configure Windows Server</span></span>

* <span data-ttu-id="28dd4-135">など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://www.microsoft.com/net/download/core)または[.NET Framework](https://www.microsoft.com/net/download/framework)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-135">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="28dd4-136">HTTP.sys は、バインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-136">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="28dd4-137">Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="28dd4-138">唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="28dd4-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="28dd4-139">詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="28dd4-140">HTTP.sys に到達するトラフィックを許可するファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-140">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="28dd4-141">使用することができます*netsh.exe*または[PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-141">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="28dd4-142">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="28dd4-143">HTTP.sys を使用する ASP.NET Core アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-143">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="28dd4-144">使用する場合は、パッケージのインストールは必要ありません、 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-144">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="28dd4-145">[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) metapackage でパッケージが含まれています。</span><span class="sxs-lookup"><span data-stu-id="28dd4-145">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="28dd4-146">呼び出す、`UseHttpSys`拡張メソッドを`WebHostBuilder`で、`Main`いずれかを指定してメソッド[HTTP.sys オプション](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)する必要がある、次の例で示すように。</span><span class="sxs-lookup"><span data-stu-id="28dd4-146">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="28dd4-147">HTTP.sys のオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-147">Configure HTTP.sys options</span></span>

<span data-ttu-id="28dd4-148">HTTP.sys の設定および構成できる制限のいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-148">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="28dd4-149">**最大クライアント接続**</span><span class="sxs-lookup"><span data-stu-id="28dd4-149">**Maximum client connections**</span></span>

<span data-ttu-id="28dd4-150">次のコードを含むアプリケーション全体の同時実行の開いている TCP 接続の最大数を設定することができます*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="28dd4-150">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="28dd4-151">接続の最大数は、既定で無制限 (null) です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-151">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="28dd4-152">**最大要求本文のサイズ**</span><span class="sxs-lookup"><span data-stu-id="28dd4-152">**Maximum request body size**</span></span>

<span data-ttu-id="28dd4-153">既定の最大要求本文のサイズは、約 28.6 MB である 30,000,000 (バイト単位) です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-153">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="28dd4-154">ASP.NET Core MVC アプリの制限を上書きすることをお勧めを使用して、 [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)アクション メソッドの属性。</span><span class="sxs-lookup"><span data-stu-id="28dd4-154">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="28dd4-155">全体のアプリケーションでは、すべての要求の制約を構成する方法を示す例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-155">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="28dd4-156">内の特定の要求に設定を上書きできます*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="28dd4-156">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="28dd4-157">アプリケーションの要求の読み取りが開始した後、要求の制限を構成しようとする場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-157">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="28dd4-158">`IsReadOnly`プロパティを示す場合、`MaxRequestBodySize`プロパティは読み取り専用状態で制限を構成するには遅すぎますを意味します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-158">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="28dd4-159">その他の HTTP.sys オプションについては、次を参照してください。 [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-159">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="28dd4-160">Url とポートでリッスンするように構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-160">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="28dd4-161">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-161">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="28dd4-162">URL プレフィックスとポートを構成するのに使用することができます、`UseUrls`の拡張メソッドで、`urls`コマンドライン引数では、ASPNETCORE_URLS 環境変数、または`UrlPrefixes`プロパティ[HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-162">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="28dd4-163">次のコード例では`UrlPrefixes`します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-163">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="28dd4-164">利点`UrlPrefixes`が正しくフォーマットされているプレフィックスを追加しようとする場合にすぐに、エラー メッセージを取得することができます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-164">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="28dd4-165">利点`UseUrls`(とは共有`urls`と ASPNETCORE_URLS) は Kestrel と HTTP.sys の間でより簡単に切り替えることができます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-165">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="28dd4-166">両方を使用する場合`UseUrls`(または`urls`または ASPNETCORE_URLS) と`UrlPrefixes`、設定`UrlPrefixes`内のオーバーライド`UseUrls`です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-166">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="28dd4-167">詳細については、次を参照してください。[ホスティング](xref:fundamentals/hosting)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-167">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="28dd4-168">HTTP.sys を使用して、[文字列形式の HTTP サーバー API UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-168">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="28dd4-169">内の同じプレフィックス文字列を指定することを確認してください`UseUrls`または`UrlPrefixes`サーバーに事前登録します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-169">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="28dd4-170">IIS を使用しません。</span><span class="sxs-lookup"><span data-stu-id="28dd4-170">Don't use IIS</span></span>

<span data-ttu-id="28dd4-171">IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="28dd4-171">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="28dd4-172">Visual Studio では、既定の起動プロファイルは、IIS express はします。</span><span class="sxs-lookup"><span data-stu-id="28dd4-172">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="28dd4-173">コンソール アプリケーションとしてプロジェクトを実行するには、手動で変更、選択したプロファイルの次のスクリーン ショットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="28dd4-173">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![コンソール アプリのプロファイルを選択します。](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="28dd4-175">URL プレフィックスを事前登録し、SSL を構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-175">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="28dd4-176">IIS と HTTP.sys のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-176">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="28dd4-177">IIS では、management UI では、すべての構成に比較的簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-177">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="28dd4-178">ただし、Http.Sys を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="28dd4-178">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="28dd4-179">つまりを行うための組み込みツール*netsh.exe*です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-179">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="28dd4-180">*Netsh.exe* URL プレフィックスを予約し、SSL 証明書を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-180">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="28dd4-181">このツールには、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-181">The tool requires administrative privileges.</span></span>

<span data-ttu-id="28dd4-182">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小値を示しています。</span><span class="sxs-lookup"><span data-stu-id="28dd4-182">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="28dd4-183">次の例では、SSL 証明書を割り当てる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-183">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="28dd4-184">ここでは、リファレンス ドキュメントを*netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="28dd4-184">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="28dd4-185">ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)</span><span class="sxs-lookup"><span data-stu-id="28dd4-185">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](http://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="28dd4-186">UrlPrefix 文字列</span><span class="sxs-lookup"><span data-stu-id="28dd4-186">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="28dd4-187">次のリソースは、いくつかのシナリオの詳細な手順を提供します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-187">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="28dd4-188">HttpListener を参照している記事は、Http.Sys に基づいている両方に、HTTP.sys に同じように適用します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-188">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="28dd4-189">方法: SSL 証明書でポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-189">How to: Configure a Port with an SSL Certificate</span></span>](http://msdn.microsoft.com/library/ms733791.aspx)
* <span data-ttu-id="28dd4-190">[HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-190">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="28dd4-191">[方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx)有用な情報で以前のブログをすぎますがこれです。</span><span class="sxs-lookup"><span data-stu-id="28dd4-191">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx) This too is an older blog with useful information.</span></span>

<span data-ttu-id="28dd4-192">ここより簡単に使用できる一部のサード パーティ製ツール、 *netsh.exe*コマンドライン。</span><span class="sxs-lookup"><span data-stu-id="28dd4-192">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="28dd4-193">によって提供されるか、Microsoft によって承認されているこれらがありません。</span><span class="sxs-lookup"><span data-stu-id="28dd4-193">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="28dd4-194">ツールは、実行管理者として既定では、ので*netsh.exe*自体には、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="28dd4-194">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="28dd4-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-195">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="28dd4-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-196">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="28dd4-197">UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="28dd4-197">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="28dd4-198">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="28dd4-198">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="28dd4-199">次のステップ</span><span class="sxs-lookup"><span data-stu-id="28dd4-199">Next steps</span></span>

<span data-ttu-id="28dd4-200">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="28dd4-200">For more information, see the following resources:</span></span>

* [<span data-ttu-id="28dd4-201">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="28dd4-201">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/HttpSys/sample)
* [<span data-ttu-id="28dd4-202">HTTP.sys のソース コード</span><span class="sxs-lookup"><span data-stu-id="28dd4-202">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="28dd4-203">ホスティング</span><span class="sxs-lookup"><span data-stu-id="28dd4-203">Hosting</span></span>](xref:fundamentals/hosting)
