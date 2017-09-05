---
title: "ASP.NET Core の WebListener web サーバーの実装"
author: rick-anderson
description: "WebListener、Windows 上の ASP.NET core web サーバーが導入されています。 Http.Sys のカーネル モード ドライバーで WebListener は、IIS なしでインターネットに直接接続に使用することができます Kestrel する代わりにします。"
keywords: "ASP.NET Core、WebListener、HttpListener、SSL url プレフィックス"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: 0a7286e4-6428-424e-b5c4-5c98815cf61c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: bcd225875cfe2a544581c331231c704094780ea3
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="65c31-105">ASP.NET Core の WebListener web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="65c31-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="65c31-106">によって[Tom Dykstra](http://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="65c31-106">By [Tom Dykstra](http://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="65c31-107">このトピックは ASP.NET Core にのみ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="65c31-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="65c31-108">ASP.NET Core 2.0 では、WebListener の名前[HTTP.sys](httpsys.md)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="65c31-109">WebListener は、 [ASP.NET core web server](index.md) Windows だけで実行されています。</span><span class="sxs-lookup"><span data-stu-id="65c31-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="65c31-110">に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="65c31-111">WebListener が代わりに[Kestrel](kestrel.md)リバース プロキシ サーバーとして IIS に依存せず、インターネットに直接接続に使用できます。</span><span class="sxs-lookup"><span data-stu-id="65c31-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="65c31-112">実際には、 **WebListener と互換性がないと IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**</span><span class="sxs-lookup"><span data-stu-id="65c31-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="65c31-113">経由で任意の .NET Core または .NET Framework アプリケーションで直接使用できますが、WebListener は、ASP.NET Core 用に開発されて、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="65c31-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="65c31-114">WebListener には、次の機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="65c31-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="65c31-115">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="65c31-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="65c31-116">ポート共有</span><span class="sxs-lookup"><span data-stu-id="65c31-116">Port sharing</span></span>
- <span data-ttu-id="65c31-117">SNI を HTTPS</span><span class="sxs-lookup"><span data-stu-id="65c31-117">HTTPS with SNI</span></span>
- <span data-ttu-id="65c31-118">Http/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="65c31-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="65c31-119">ファイルを直接転送</span><span class="sxs-lookup"><span data-stu-id="65c31-119">Direct file transmission</span></span>
- <span data-ttu-id="65c31-120">応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="65c31-120">Response caching</span></span>
- <span data-ttu-id="65c31-121">Websocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="65c31-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="65c31-122">サポートされている Windows のバージョン:</span><span class="sxs-lookup"><span data-stu-id="65c31-122">Supported Windows versions:</span></span>

- <span data-ttu-id="65c31-123">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="65c31-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

[<span data-ttu-id="65c31-124">サンプル コードを表示またはダウンロードする</span><span class="sxs-lookup"><span data-stu-id="65c31-124">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)

## <a name="when-to-use-weblistener"></a><span data-ttu-id="65c31-125">WebListener を使用する場合</span><span class="sxs-lookup"><span data-stu-id="65c31-125">When to use WebListener</span></span>

<span data-ttu-id="65c31-126">WebListener は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="65c31-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![Weblistener がインターネットに直接通信します。](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="65c31-128">Http.Sys で用意されているので WebListener は攻撃から保護するため、リバース プロキシ サーバーを必要としません。</span><span class="sxs-lookup"><span data-stu-id="65c31-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="65c31-129">Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="65c31-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="65c31-130">Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="65c31-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="65c31-131">WebListener も内部環境に適して Kestrel を使用して取得できない場合、提供される機能のいずれかの操作を必要なとき。</span><span class="sxs-lookup"><span data-stu-id="65c31-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![Weblistener が内部ネットワークに直接通信します。](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="65c31-133">WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="65c31-133">How to use WebListener</span></span>

<span data-ttu-id="65c31-134">次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="65c31-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="65c31-135">Windows Server を構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-135">Configure Windows Server</span></span>

* <span data-ttu-id="65c31-136">など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://go.microsoft.com/fwlink/?LinkID=827524)または .NET Framework 4.5.1。</span><span class="sxs-lookup"><span data-stu-id="65c31-136">Install the version of .NET that your application requires, such as [.NET Core](https://go.microsoft.com/fwlink/?LinkID=827524) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="65c31-137">WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。</span><span class="sxs-lookup"><span data-stu-id="65c31-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="65c31-138">Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。</span><span class="sxs-lookup"><span data-stu-id="65c31-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="65c31-139">唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="65c31-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="65c31-140">詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="65c31-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="65c31-141">WebListener に到達するトラフィックを許可するファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="65c31-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="65c31-142">Netsh.exe を使用するか、 [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="65c31-143">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="65c31-144">ASP.NET Core アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="65c31-145">NuGet パッケージのインストール[Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="65c31-146">これもインストール[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/)依存関係として。</span><span class="sxs-lookup"><span data-stu-id="65c31-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="65c31-147">呼び出す、 [ `UseWebListener` ](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilderKestrelExtensions/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilderWebListenerExtensions.UseWebListener.md)拡張メソッドを[WebHostBuilder](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilder/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilder.md)で、`Main`任意 WebListener を指定して、メソッド[オプション](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)と[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)する必要がある、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="65c31-147">Call the [`UseWebListener`](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilderKestrelExtensions/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilderWebListenerExtensions.UseWebListener.md) extension method on [WebHostBuilder](http://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Hosting/WebHostBuilder/index.html#Microsoft.AspNetCore.Hosting.WebHostBuilder.md) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="65c31-148">Url とポートでリッスンするように構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="65c31-149">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="65c31-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="65c31-150">URL プレフィックスとポートを構成するのに使用することができます、`UseURLs`の拡張メソッドで、`urls`コマンドライン引数または ASP.NET Core の構成システムです。</span><span class="sxs-lookup"><span data-stu-id="65c31-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="65c31-151">詳細については、次を参照してください。[ホスティング](../../fundamentals/hosting.md)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="65c31-152">リスナーは web、 [Http.Sys プレフィックス文字列書式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="65c31-153">WebListener に固有のプレフィックス文字列形式の要件はありません。</span><span class="sxs-lookup"><span data-stu-id="65c31-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="65c31-154">内の同じプレフィックス文字列を指定することを確認してください`UseUrls`サーバーに事前登録します。</span><span class="sxs-lookup"><span data-stu-id="65c31-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="65c31-155">IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="65c31-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="65c31-156">Visual Studio では、既定の起動プロファイルは、IIS express はします。</span><span class="sxs-lookup"><span data-stu-id="65c31-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="65c31-157">コンソール アプリケーションとして、プロジェクトを実行するには、必要がある、選択したプロファイルを手動で変更する次のスクリーン ショットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="65c31-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![コンソール アプリのプロファイルを選択します。](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="65c31-159">ASP.NET Core の外部で WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="65c31-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="65c31-160">インストール、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="65c31-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="65c31-161">[WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録](#preregister-url-prefixes-and-configure-ssl)ASP.NET Core で使用するのと同様です。</span><span class="sxs-lookup"><span data-stu-id="65c31-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="65c31-162">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="65c31-163">ASP.NET Core の外部で WebListener の使用方法を示すコード サンプルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="65c31-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="65c31-164">URL プレフィックスを事前登録し、SSL を構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="65c31-165">IIS と WebListener のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。</span><span class="sxs-lookup"><span data-stu-id="65c31-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="65c31-166">IIS では、management UI では、すべての構成に比較的簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="65c31-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="65c31-167">ただし、WebListener を使用している場合は、Http.Sys を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="65c31-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="65c31-168">これを行うための組み込みツールは、netsh.exe です。</span><span class="sxs-lookup"><span data-stu-id="65c31-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="65c31-169">Netsh.exe を使用する必要があります。 最も一般的なタスクは URL プレフィックスを予約して、SSL 証明書を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="65c31-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="65c31-170">NetSh.exe は、初心者向けの使いやすいツールではありません。</span><span class="sxs-lookup"><span data-stu-id="65c31-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="65c31-171">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最低限のものを示しています。</span><span class="sxs-lookup"><span data-stu-id="65c31-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="65c31-172">次の例では、SSL 証明書を割り当てる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="65c31-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="65c31-173">公式のリファレンス ドキュメントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="65c31-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="65c31-174">ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)</span><span class="sxs-lookup"><span data-stu-id="65c31-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](http://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="65c31-175">UrlPrefix 文字列</span><span class="sxs-lookup"><span data-stu-id="65c31-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="65c31-176">次のリソースは、いくつかのシナリオの詳細な手順を提供します。</span><span class="sxs-lookup"><span data-stu-id="65c31-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="65c31-177">参照している記事`HttpListener`に均等に適用`WebListener`Http.Sys に基づいて、両方は、します。</span><span class="sxs-lookup"><span data-stu-id="65c31-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="65c31-178">方法: SSL 証明書でポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-178">How to: Configure a Port with an SSL Certificate</span></span>](http://msdn.microsoft.com/library/ms733791.aspx)
* <span data-ttu-id="65c31-179">[HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。</span><span class="sxs-lookup"><span data-stu-id="65c31-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="65c31-180">[方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx)有用な情報で以前のブログをすぎますがこれです。</span><span class="sxs-lookup"><span data-stu-id="65c31-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](http://blogs.msdn.com/b/jpsanders/archive/2009/09/29/walkthrough-using-httplistener-as-an-ssl-simple-server.aspx) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="65c31-181">SSL を使用して .NET Core WebListener を設定する方法は?</span><span class="sxs-lookup"><span data-stu-id="65c31-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="65c31-182">Netsh.exe コマンド ラインよりも簡単に使用できる一部のサード パーティ製ツールを次に示します。</span><span class="sxs-lookup"><span data-stu-id="65c31-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="65c31-183">によって提供されるか、Microsoft によって承認されているこれらがありません。</span><span class="sxs-lookup"><span data-stu-id="65c31-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="65c31-184">ツールは既定では、管理者として実行ため netsh.exe 自体には、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="65c31-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="65c31-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="65c31-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="65c31-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="65c31-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="65c31-187">UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="65c31-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="65c31-188">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="65c31-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="65c31-189">自己署名 SSL 証明書を生成するのには、マイクロソフトは、コマンド ライン ツールを提供しています: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968)し、PowerShell コマンドレット[New-selfsignedcertificate](https://technet.microsoft.com/library/hh848633)です。</span><span class="sxs-lookup"><span data-stu-id="65c31-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633).</span></span> <span data-ttu-id="65c31-190">自己署名 SSL 証明書を生成するための簡略化するサード パーティの UI ツールもあります。</span><span class="sxs-lookup"><span data-stu-id="65c31-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="65c31-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="65c31-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="65c31-192">Makecert の UI</span><span class="sxs-lookup"><span data-stu-id="65c31-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="65c31-193">次のステップ</span><span class="sxs-lookup"><span data-stu-id="65c31-193">Next steps</span></span>

<span data-ttu-id="65c31-194">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="65c31-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="65c31-195">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="65c31-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="65c31-196">WebListener ソース コード</span><span class="sxs-lookup"><span data-stu-id="65c31-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="65c31-197">ホスティング</span><span class="sxs-lookup"><span data-stu-id="65c31-197">Hosting</span></span>](../hosting.md)
