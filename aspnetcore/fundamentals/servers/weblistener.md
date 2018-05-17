---
title: ASP.NET Core への WebListener Web サーバーの実装
author: rick-anderson
description: IIS なしでインターネットに直接接続するために使用できる Windows 上の ASP.NET Core の Web サーバーである WebListener について説明します。
manager: wpickett
ms.author: riande
ms.date: 03/13/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/weblistener
ms.openlocfilehash: d40243454632550147a7d42ab26a8f1d2d100db2
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="95a55-103">ASP.NET Core への WebListener Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="95a55-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="95a55-104">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="95a55-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="95a55-105">このトピックは、ASP.NET Core 1.x にのみ適用されます。</span><span class="sxs-lookup"><span data-stu-id="95a55-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="95a55-106">ASP.NET Core 2.0 では、WebListener は [HTTP.sys](httpsys.md) と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="95a55-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="95a55-107">WebListener は Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](index.md)です。</span><span class="sxs-lookup"><span data-stu-id="95a55-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="95a55-108">[Http.Sys カーネル モード ドライバー](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)に基づいて構築されています。</span><span class="sxs-lookup"><span data-stu-id="95a55-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="95a55-109">WebListener は、IIS に依存せずにリバース プロキシ サーバーとしてインターネットに直接接続するために使用できる [Kestrel](kestrel.md) の代替製品です。</span><span class="sxs-lookup"><span data-stu-id="95a55-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="95a55-110">実際、**WebListener は [ASP.NET Core モジュール](aspnet-core-module.md)と互換性がないため、IIS または IIS Express で使用することはできません。**</span><span class="sxs-lookup"><span data-stu-id="95a55-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="95a55-111">WebListener は ASP.NET Core 用に開発されましたが、[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージを介して任意の .NET Core または.NET Framework アプリケーションで直接使用できます。</span><span class="sxs-lookup"><span data-stu-id="95a55-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="95a55-112">WebListener は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="95a55-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="95a55-113">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="95a55-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="95a55-114">ポート共有</span><span class="sxs-lookup"><span data-stu-id="95a55-114">Port sharing</span></span>
- <span data-ttu-id="95a55-115">SNI を使用する HTTPS</span><span class="sxs-lookup"><span data-stu-id="95a55-115">HTTPS with SNI</span></span>
- <span data-ttu-id="95a55-116">HTTP/2 over TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="95a55-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="95a55-117">直接ファイル伝送</span><span class="sxs-lookup"><span data-stu-id="95a55-117">Direct file transmission</span></span>
- <span data-ttu-id="95a55-118">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="95a55-118">Response caching</span></span>
- <span data-ttu-id="95a55-119">WebSocket (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="95a55-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="95a55-120">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="95a55-120">Supported Windows versions:</span></span>

- <span data-ttu-id="95a55-121">Windows 7 および Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="95a55-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="95a55-122">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="95a55-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="95a55-123">WebListener を使用する場合</span><span class="sxs-lookup"><span data-stu-id="95a55-123">When to use WebListener</span></span>

<span data-ttu-id="95a55-124">WebListener は、IIS を使用せずにインターネットにサーバーを直接公開する必要がある展開に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="95a55-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![インターネットと直接通信する Weblistener](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="95a55-126">WebListener は Http.Sys に基づいて構築されているため、攻撃から保護するためのリバース プロキシ サーバーは必要ありません。</span><span class="sxs-lookup"><span data-stu-id="95a55-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="95a55-127">Http.Sys とは、さまざまな種類の攻撃から保護し、完全な機能を備えた Web サーバーの堅牢性、セキュリティ、およびスケーラビリティを提供する成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="95a55-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="95a55-128">IIS 自体が Http.Sys 上で HTTP リスナーとして実行されています。</span><span class="sxs-lookup"><span data-stu-id="95a55-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="95a55-129">WebListener は、Kestrel を使用して取得できない機能のいずれかが必要な場合の内部展開にも適しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![内部ネットワークと直接通信する Weblistener](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="95a55-131">WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="95a55-131">How to use WebListener</span></span>

<span data-ttu-id="95a55-132">ホスト OS と ASP.NET Core アプリケーションのセットアップ タスクの概要について説明します。</span><span class="sxs-lookup"><span data-stu-id="95a55-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="95a55-133">Windows Server を構成する</span><span class="sxs-lookup"><span data-stu-id="95a55-133">Configure Windows Server</span></span>

* <span data-ttu-id="95a55-134">[.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe)、.NET Framework 4.5.1 など、アプリケーションに必要な .NET のバージョンをインストールします。</span><span class="sxs-lookup"><span data-stu-id="95a55-134">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="95a55-135">WebListener にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します</span><span class="sxs-lookup"><span data-stu-id="95a55-135">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="95a55-136">Windows で URL プレフィックスを事前登録していない場合は、管理者特権でアプリケーションを実行する必要があります。</span><span class="sxs-lookup"><span data-stu-id="95a55-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="95a55-137">唯一の例外は、1024 より大きいポート番号で (HTTPS ではなく) HTTP を使用して localhost にバインドする場合です。この場合、管理者権限は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="95a55-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="95a55-138">詳細については、この記事で後述する「[URL プレフィックスの事前登録と SSL の構成](#preregister-url-prefixes-and-configure-ssl)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="95a55-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="95a55-139">トラフィックが WebListener に到達できるようにファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="95a55-139">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="95a55-140">netsh.exe または [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)を使用できます。</span><span class="sxs-lookup"><span data-stu-id="95a55-140">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="95a55-141">[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。</span><span class="sxs-lookup"><span data-stu-id="95a55-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="95a55-142">ASP.NET Core アプリケーションを構成する</span><span class="sxs-lookup"><span data-stu-id="95a55-142">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="95a55-143">NuGet パッケージ [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="95a55-143">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="95a55-144">これで、依存関係として [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) もインストールされます。</span><span class="sxs-lookup"><span data-stu-id="95a55-144">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="95a55-145">次の例で示すように、必要な WebListener [オプション](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs)と[設定](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs)を指定して、`Main` メソッド内で [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に対して `UseWebListener` 拡張メソッドを呼び出します。</span><span class="sxs-lookup"><span data-stu-id="95a55-145">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="95a55-146">リッスンする URL とポートを構成する</span><span class="sxs-lookup"><span data-stu-id="95a55-146">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="95a55-147">既定では ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="95a55-147">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="95a55-148">URL プレフィックスとポートを構成するには、`UseURLs` 拡張メソッド、`urls` コマンド ライン引数、または ASP.NET Core 構成システムを使用できます。</span><span class="sxs-lookup"><span data-stu-id="95a55-148">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="95a55-149">詳細については、[ホスティング](../../fundamentals/hosting.md)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="95a55-149">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="95a55-150">Web Listener は、[Http.Sys プレフィックス文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)を使用します。</span><span class="sxs-lookup"><span data-stu-id="95a55-150">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="95a55-151">WebListener に固有のプレフィックス文字列形式の要件はありません。</span><span class="sxs-lookup"><span data-stu-id="95a55-151">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="95a55-152">最上位のワイルドカードのバインド (`http://*:80/` と `http://+:80`) は使用しては**いけません**。</span><span class="sxs-lookup"><span data-stu-id="95a55-152">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="95a55-153">最上位のワイルドカードのバインドは、セキュリティの脆弱性に対してアプリを切り開くことができます。</span><span class="sxs-lookup"><span data-stu-id="95a55-153">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="95a55-154">これは、強力と脆弱の両方のワイルドカードに適用されます。</span><span class="sxs-lookup"><span data-stu-id="95a55-154">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="95a55-155">ワイルドカードではなく、明示的なホスト名を使用します。</span><span class="sxs-lookup"><span data-stu-id="95a55-155">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="95a55-156">全体の親ドメインを制御する場合、サブドメイン ワイルドカード バインド (たとえば、`*.mysub.com`) にこのセキュリティ リスクはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="95a55-156">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="95a55-157">詳細については、[rfc7230 セクション-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="95a55-157">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="95a55-158">サーバーで事前登録する `UseUrls` に同じプレフィックス文字列を指定してください。</span><span class="sxs-lookup"><span data-stu-id="95a55-158">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="95a55-159">アプリケーションが IIS または IIS Express を実行するように構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="95a55-159">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="95a55-160">Visual Studio では、既定の起動プロファイルは IIS Express 用です。</span><span class="sxs-lookup"><span data-stu-id="95a55-160">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="95a55-161">プロジェクトをコンソール アプリケーションとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="95a55-161">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![コンソール アプリのプロファイルを選択する](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="95a55-163">ASP.NET Core の外部で WebListener を使用する方法</span><span class="sxs-lookup"><span data-stu-id="95a55-163">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="95a55-164">[Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="95a55-164">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="95a55-165">ASP.NET Core で使用する場合と同様に、[WebListener にバインドする URL プレフィックスを事前登録し、SSL 証明書を設定します](#preregister-url-prefixes-and-configure-ssl)。</span><span class="sxs-lookup"><span data-stu-id="95a55-165">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="95a55-166">[Http.Sys のレジストリ設定](https://support.microsoft.com/kb/820129)もあります。</span><span class="sxs-lookup"><span data-stu-id="95a55-166">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="95a55-167">次のコード サンプルは、ASP.NET Core の外部で WebListener を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-167">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="95a55-168">URL プレフィックスの事前登録と SSL の構成</span><span class="sxs-lookup"><span data-stu-id="95a55-168">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="95a55-169">IIS と WebListener はいずれも、要求をリッスンして初期処理を行うために、基になる Http.Sys カーネル モード ドライバーに依存しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-169">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="95a55-170">IIS では、管理 UI を使用すると、すべての構成を比較的簡単に実行できます。</span><span class="sxs-lookup"><span data-stu-id="95a55-170">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="95a55-171">ただし、WebListener を使用している場合は、Http.Sys を自分で構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="95a55-171">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="95a55-172">この処理に使用する組み込みツールは netsh.exe です。</span><span class="sxs-lookup"><span data-stu-id="95a55-172">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="95a55-173">netsh.exe を使用するために必要な最も一般的なタスクは、URL プレフィックスの予約と SSL 証明書の割り当てです。</span><span class="sxs-lookup"><span data-stu-id="95a55-173">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="95a55-174">NetSh.exe は、初心者向けの簡単なツールではありません。</span><span class="sxs-lookup"><span data-stu-id="95a55-174">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="95a55-175">次の例は、ポート 80 と 443 の URL プレフィックスを予約するために必要な最小限の値を示しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-175">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="95a55-176">次の例は、SSL 証明書を割り当てる方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-176">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="95a55-177">公式のリファレンス ドキュメントは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="95a55-177">Here is the official reference documentation:</span></span>

* <span data-ttu-id="95a55-178">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)</span><span class="sxs-lookup"><span data-stu-id="95a55-178">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
* <span data-ttu-id="95a55-179">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)</span><span class="sxs-lookup"><span data-stu-id="95a55-179">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

<span data-ttu-id="95a55-180">次のリソースでは、いくつかのシナリオの詳細な手順を説明しています。</span><span class="sxs-lookup"><span data-stu-id="95a55-180">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="95a55-181">`HttpListener` を参照する記事は、両方とも Http.Sys に基づいているため、`WebListener` に同様に適用されます。</span><span class="sxs-lookup"><span data-stu-id="95a55-181">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="95a55-182">方法 : SSL 証明書を使用してポートを構成する</span><span class="sxs-lookup"><span data-stu-id="95a55-182">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="95a55-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) (HTTPS 通信 - HttpListener ベースのホスティングとクライアントの認証): サードパーティのブログであり、かなり古い投稿ですが、有用な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="95a55-183">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="95a55-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) (方法: SSL Simple Server として HttpListener または Http Server アンマネージ コード (C++) を使用するチュートリアル): これも古いブログですが、有用な情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="95a55-184">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* <span data-ttu-id="95a55-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/) (SSL を使用して.NET Core WebListener を設定する方法)</span><span class="sxs-lookup"><span data-stu-id="95a55-185">[How Do I Set Up A .NET Core WebListener With SSL?](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)</span></span>

<span data-ttu-id="95a55-186">netsh.exe コマンド ラインよりも使いやすいサードパーティ製ツールをいくつか紹介します。</span><span class="sxs-lookup"><span data-stu-id="95a55-186">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="95a55-187">これらのツールは Microsoft 製ではなく、Microsoft が推奨するものでもありません。</span><span class="sxs-lookup"><span data-stu-id="95a55-187">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="95a55-188">netsh.exe 自体に管理者特権が必要なので、これらのツールは既定で管理者として実行されます。</span><span class="sxs-lookup"><span data-stu-id="95a55-188">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="95a55-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) には、SSL 証明書とオプション、プレフィックス予約、および証明書信頼リストを一覧表示および設定するための UI があります。</span><span class="sxs-lookup"><span data-stu-id="95a55-189">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="95a55-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) では、SSL 証明書と URL プレフィックスを一覧表示したり、設定することができます。</span><span class="sxs-lookup"><span data-stu-id="95a55-190">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="95a55-191">UI は http.sys Manager より洗練されており、公開されている構成オプションも少し多くなっていますが、その他の機能は同様です。</span><span class="sxs-lookup"><span data-stu-id="95a55-191">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="95a55-192">新しい証明書信頼リスト (CTL) を作成することはできませんが、既存の証明書信頼リストを割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="95a55-192">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="95a55-193">自己署名 SSL 証明書を生成する場合は、Microsoft が提供するコマンド ラインツールである [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) と PowerShell コマンドレットの [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate) を利用できます。</span><span class="sxs-lookup"><span data-stu-id="95a55-193">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="95a55-194">また、自己署名 SSL 証明書を簡単に生成できるサードパーティ製 UI ツールもあります。</span><span class="sxs-lookup"><span data-stu-id="95a55-194">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="95a55-195">SelfCert</span><span class="sxs-lookup"><span data-stu-id="95a55-195">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="95a55-196">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="95a55-196">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="95a55-197">次の手順</span><span class="sxs-lookup"><span data-stu-id="95a55-197">Next steps</span></span>

<span data-ttu-id="95a55-198">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="95a55-198">For more information, see the following resources:</span></span>

* [<span data-ttu-id="95a55-199">この記事のサンプル アプリ</span><span class="sxs-lookup"><span data-stu-id="95a55-199">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="95a55-200">WebListener ソース コード</span><span class="sxs-lookup"><span data-stu-id="95a55-200">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="95a55-201">ホスティング</span><span class="sxs-lookup"><span data-stu-id="95a55-201">Hosting</span></span>](../hosting.md)
