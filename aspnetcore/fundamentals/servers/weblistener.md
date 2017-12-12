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
ms.openlocfilehash: f1abb3558546cd907c78b44d9353d9c9f1f5aff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c03f0-105">ASP.NET Core の WebListener web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="c03f0-105">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c03f0-106">によって[Tom Dykstra](https://github.com/tdykstra)と[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c03f0-106">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="c03f0-107">このトピックは ASP.NET Core にのみ 1.x です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-107">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="c03f0-108">ASP.NET Core 2.0 では、WebListener の名前[HTTP.sys](httpsys.md)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-108">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="c03f0-109">WebListener は、 [ASP.NET core web server](index.md) Windows だけで実行されています。</span><span class="sxs-lookup"><span data-stu-id="c03f0-109">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="c03f0-110">に基づいて構築されて、 [Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-110">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c03f0-111">WebListener が代わりに[Kestrel](kestrel.md)リバース プロキシ サーバーとして IIS に依存せず、インターネットに直接接続に使用できます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-111">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="c03f0-112">実際には、 **WebListener と互換性がないと IIS または IIS Express では使用できません、 [ASP.NET Core モジュール](aspnet-core-module.md)です。**</span><span class="sxs-lookup"><span data-stu-id="c03f0-112">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="c03f0-113">経由で任意の .NET Core または .NET Framework アプリケーションで直接使用できますが、WebListener は、ASP.NET Core 用に開発されて、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="c03f0-113">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="c03f0-114">WebListener には、次の機能がサポートされています。</span><span class="sxs-lookup"><span data-stu-id="c03f0-114">WebListener supports the following features:</span></span>

- [<span data-ttu-id="c03f0-115">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="c03f0-115">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="c03f0-116">ポート共有</span><span class="sxs-lookup"><span data-stu-id="c03f0-116">Port sharing</span></span>
- <span data-ttu-id="c03f0-117">SNI を HTTPS</span><span class="sxs-lookup"><span data-stu-id="c03f0-117">HTTPS with SNI</span></span>
- <span data-ttu-id="c03f0-118">Http/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="c03f0-118">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="c03f0-119">ファイルを直接転送</span><span class="sxs-lookup"><span data-stu-id="c03f0-119">Direct file transmission</span></span>
- <span data-ttu-id="c03f0-120">応答のキャッシュ</span><span class="sxs-lookup"><span data-stu-id="c03f0-120">Response caching</span></span>
- <span data-ttu-id="c03f0-121">Websocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="c03f0-121">WebSockets (Windows 8)</span></span>

<span data-ttu-id="c03f0-122">サポートされている Windows のバージョン:</span><span class="sxs-lookup"><span data-stu-id="c03f0-122">Supported Windows versions:</span></span>

- <span data-ttu-id="c03f0-123">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="c03f0-123">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="c03f0-124">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="c03f0-124">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="c03f0-125">WebListener を使用する場合</span><span class="sxs-lookup"><span data-stu-id="c03f0-125">When to use WebListener</span></span>

<span data-ttu-id="c03f0-126">WebListener は、IIS を使用して、サーバーをインターネットに直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-126">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="c03f0-128">Http.Sys で用意されているので WebListener は攻撃から保護するため、リバース プロキシ サーバーを必要としません。</span><span class="sxs-lookup"><span data-stu-id="c03f0-128">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="c03f0-129">Http.Sys は、さまざまな種類の攻撃から保護し、堅牢性、セキュリティ、および多機能な web サーバーのスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="c03f0-129">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c03f0-130">Http.Sys の上部に HTTP リスナーとして IIS 自体が実行されます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-130">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="c03f0-131">WebListener も内部環境に適して Kestrel を使用して取得できない場合、提供される機能のいずれかの操作を必要なとき。</span><span class="sxs-lookup"><span data-stu-id="c03f0-131">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="c03f0-133">WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="c03f0-133">How to use WebListener</span></span>

<span data-ttu-id="c03f0-134">次に、ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要を示します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-134">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="c03f0-135">Windows Server を構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-135">Configure Windows Server</span></span>

* <span data-ttu-id="c03f0-136">など、アプリケーションが必要な .NET のバージョンをインストール[.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)または .NET Framework 4.5.1。</span><span class="sxs-lookup"><span data-stu-id="c03f0-136">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="c03f0-137">WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-137">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="c03f0-138">Windows での URL プレフィックスを事前登録しない場合は、管理者特権を持つ、アプリケーションを実行する必要です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-138">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="c03f0-139">唯一の例外は、ポート番号です。 1024 よりも大きい HTTP (HTTPS ではなく) を使用してローカル ホストにバインドするかどうかその場合は、管理者特権は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="c03f0-139">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="c03f0-140">詳細については、「[プレフィックスを事前登録し SSL を構成する方法](#preregister-url-prefixes-and-configure-ssl)この記事で後述します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-140">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="c03f0-141">WebListener に到達するトラフィックを許可するファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-141">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="c03f0-142">Netsh.exe を使用するか、 [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-142">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="c03f0-143">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-143">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="c03f0-144">ASP.NET Core アプリケーションを構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-144">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="c03f0-145">NuGet パッケージのインストール[Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-145">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="c03f0-146">これもインストール[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/)依存関係として。</span><span class="sxs-lookup"><span data-stu-id="c03f0-146">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="c03f0-147">呼び出す、`UseWebListener`拡張メソッドを[WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder)で、`Main`任意 WebListener を指定して、メソッド[オプション](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)と[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)必要があります。、次の例で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c03f0-147">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="c03f0-148">Url とポートでリッスンするように構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-148">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="c03f0-149">既定では ASP.NET Core にバインド`http://localhost:5000`です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-149">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c03f0-150">URL プレフィックスとポートを構成するのに使用することができます、`UseURLs`の拡張メソッドで、`urls`コマンドライン引数または ASP.NET Core の構成システムです。</span><span class="sxs-lookup"><span data-stu-id="c03f0-150">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="c03f0-151">詳細については、[ホスティング](../../fundamentals/hosting.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c03f0-151">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="c03f0-152">リスナーは web、 [Http.Sys プレフィックス文字列書式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-152">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="c03f0-153">WebListener に固有のプレフィックス文字列形式の要件はありません。</span><span class="sxs-lookup"><span data-stu-id="c03f0-153">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c03f0-154">内の同じプレフィックス文字列を指定することを確認してください`UseUrls`サーバーに事前登録します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-154">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="c03f0-155">IIS または IIS Express を実行するアプリケーションが構成されていないことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="c03f0-155">Make sure your application is not configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="c03f0-156">Visual Studio では、既定の起動プロファイルは、IIS express はします。</span><span class="sxs-lookup"><span data-stu-id="c03f0-156">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="c03f0-157">コンソール アプリケーションとして、プロジェクトを実行するには、必要がある、選択したプロファイルを手動で変更する次のスクリーン ショットに示すようにします。</span><span class="sxs-lookup"><span data-stu-id="c03f0-157">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![コンソール アプリのプロファイルを選択します。](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="c03f0-159">ASP.NET Core の外部で WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="c03f0-159">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="c03f0-160">インストール、 [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージです。</span><span class="sxs-lookup"><span data-stu-id="c03f0-160">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="c03f0-161">[WebListener へのバインドし、SSL 証明書を設定する URL プレフィックスを事前登録](#preregister-url-prefixes-and-configure-ssl)ASP.NET Core で使用するのと同様です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-161">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="c03f0-162">[Http.Sys レジストリ設定](https://support.microsoft.com/kb/820129)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-162">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="c03f0-163">ASP.NET Core の外部で WebListener の使用方法を示すコード サンプルを次に示します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-163">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="c03f0-164">URL プレフィックスを事前登録し、SSL を構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-164">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="c03f0-165">IIS と WebListener のどちらも、要求のリッスンを基になる Http.Sys カーネル モード ドライバーに依存し、処理の初期実行します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-165">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="c03f0-166">IIS では、management UI では、すべての構成に比較的簡単な方法です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-166">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="c03f0-167">ただし、WebListener を使用している場合は、Http.Sys を構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c03f0-167">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="c03f0-168">これを行うための組み込みツールは、netsh.exe です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-168">The built-in tool for doing that is netsh.exe.</span></span> 

<span data-ttu-id="c03f0-169">Netsh.exe を使用する必要があります。 最も一般的なタスクは URL プレフィックスを予約して、SSL 証明書を割り当てます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-169">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="c03f0-170">NetSh.exe は、初心者向けの使いやすいツールではありません。</span><span class="sxs-lookup"><span data-stu-id="c03f0-170">NetSh.exe is not an easy tool to use for beginners.</span></span> <span data-ttu-id="c03f0-171">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最低限のものを示しています。</span><span class="sxs-lookup"><span data-stu-id="c03f0-171">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="c03f0-172">次の例では、SSL 証明書を割り当てる方法を示します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-172">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="c03f0-173">公式のリファレンス ドキュメントを次に示します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-173">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="c03f0-174">ハイパー テキスト用の Netsh コマンドは転送プロトコル (HTTP)</span><span class="sxs-lookup"><span data-stu-id="c03f0-174">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="c03f0-175">UrlPrefix 文字列</span><span class="sxs-lookup"><span data-stu-id="c03f0-175">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="c03f0-176">次のリソースは、いくつかのシナリオの詳細な手順を提供します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-176">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="c03f0-177">参照している記事`HttpListener`に均等に適用`WebListener`Http.Sys に基づいて、両方は、します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-177">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="c03f0-178">方法: SSL 証明書でポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-178">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="c03f0-179">[HTTPS 通信 - HttpListener ベースのホストとクライアント証明書を](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html)これは、サード パーティ製のブログとがかなり古いいてもが有用な情報です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-179">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="c03f0-180">[方法: チュートリアルを使用して HttpListener または Http サーバー アンマネージ コード (C++) SSL 単純なサーバーとして](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/)有用な情報で以前のブログをすぎますがこれです。</span><span class="sxs-lookup"><span data-stu-id="c03f0-180">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="c03f0-181">SSL を使用して .NET Core WebListener を設定する方法は?</span><span class="sxs-lookup"><span data-stu-id="c03f0-181">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="c03f0-182">Netsh.exe コマンド ラインよりも簡単に使用できる一部のサード パーティ製ツールを次に示します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-182">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="c03f0-183">によって提供されるか、Microsoft によって承認されているこれらがありません。</span><span class="sxs-lookup"><span data-stu-id="c03f0-183">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="c03f0-184">ツールは既定では、管理者として実行ため netsh.exe 自体には、管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-184">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="c03f0-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) UI の一覧を提供し、予約をプレフィックスおよび証明書信頼リストの SSL 証明書とオプションを構成します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-185">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="c03f0-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx)一覧または SSL 証明書と URL プレフィックスを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-186">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="c03f0-187">UI http.sys Manager よりもより洗練されたは、他のいくつかの構成オプションを公開するが、それ以外の場合と同様の機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="c03f0-187">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="c03f0-188">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存のテーブルを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="c03f0-188">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="c03f0-189">自己署名 SSL 証明書を生成するのには、マイクロソフトは、コマンド ライン ツールを提供しています: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968)し、PowerShell コマンドレット[New-selfsignedcertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate)です。</span><span class="sxs-lookup"><span data-stu-id="c03f0-189">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="c03f0-190">自己署名 SSL 証明書を生成するための簡略化するサード パーティの UI ツールもあります。</span><span class="sxs-lookup"><span data-stu-id="c03f0-190">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="c03f0-191">SelfCert</span><span class="sxs-lookup"><span data-stu-id="c03f0-191">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="c03f0-192">Makecert の UI</span><span class="sxs-lookup"><span data-stu-id="c03f0-192">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="c03f0-193">次のステップ</span><span class="sxs-lookup"><span data-stu-id="c03f0-193">Next steps</span></span>

<span data-ttu-id="c03f0-194">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c03f0-194">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c03f0-195">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="c03f0-195">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="c03f0-196">WebListener ソース コード</span><span class="sxs-lookup"><span data-stu-id="c03f0-196">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c03f0-197">ホスティング</span><span class="sxs-lookup"><span data-stu-id="c03f0-197">Hosting</span></span>](../hosting.md)
