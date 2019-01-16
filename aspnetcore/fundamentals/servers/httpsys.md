---
title: ASP.NET Core での HTTP.sys Web サーバーの実装
author: guardrex
description: Windows 上の ASP.NET Core 用 Web サーバーである HTTP.sys について説明します。 HTTP.sys は、Http.sys カーネル モード ドライバーに基づいて構築された、IIS なしで直接インターネットに接続するために使用できる Kestrel の代替製品です。
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/03/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 46538d256ae2c5f3b7e6c725fa8f29092759f69f
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098855"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="025b6-104">ASP.NET Core での HTTP.sys Web サーバーの実装</span><span class="sxs-lookup"><span data-stu-id="025b6-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="025b6-105">作成者: [Tom Dykstra](https://github.com/tdykstra)、[Chris Ross](https://github.com/Tratcher)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="025b6-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="025b6-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) は、Windows 上でのみ動作する [ASP.NET Core 用 Web サーバー](xref:fundamentals/servers/index)です。</span><span class="sxs-lookup"><span data-stu-id="025b6-106">[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) is a [web server for ASP.NET Core](xref:fundamentals/servers/index) that only runs on Windows.</span></span> <span data-ttu-id="025b6-107">HTTP.sys は [Kestrel](xref:fundamentals/servers/kestrel) サーバーの代替製品であり、Kestrel では提供されていない機能がいくつか用意されています。</span><span class="sxs-lookup"><span data-stu-id="025b6-107">HTTP.sys is an alternative to [Kestrel](xref:fundamentals/servers/kestrel) server and offers some features that Kestrel doesn't provide.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="025b6-108">HTTP.sys は [ASP.NET Core モジュール](xref:host-and-deploy/aspnet-core-module)と互換性がなく、IIS や IIS Express で使用することはできません。</span><span class="sxs-lookup"><span data-stu-id="025b6-108">HTTP.sys isn't compatible with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) and can't be used with IIS or IIS Express.</span></span>

<span data-ttu-id="025b6-109">HTTP.sys は、次の機能をサポートします。</span><span class="sxs-lookup"><span data-stu-id="025b6-109">HTTP.sys supports the following features:</span></span>

* [<span data-ttu-id="025b6-110">Windows 認証</span><span class="sxs-lookup"><span data-stu-id="025b6-110">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
* <span data-ttu-id="025b6-111">ポート共有</span><span class="sxs-lookup"><span data-stu-id="025b6-111">Port sharing</span></span>
* <span data-ttu-id="025b6-112">SNI を使用する HTTPS</span><span class="sxs-lookup"><span data-stu-id="025b6-112">HTTPS with SNI</span></span>
* <span data-ttu-id="025b6-113">HTTP/2 over TLS (Windows 10 以降)</span><span class="sxs-lookup"><span data-stu-id="025b6-113">HTTP/2 over TLS (Windows 10 or later)</span></span>
* <span data-ttu-id="025b6-114">直接ファイル伝送</span><span class="sxs-lookup"><span data-stu-id="025b6-114">Direct file transmission</span></span>
* <span data-ttu-id="025b6-115">応答キャッシュ</span><span class="sxs-lookup"><span data-stu-id="025b6-115">Response caching</span></span>
* <span data-ttu-id="025b6-116">WebSocket (Windows 8 以降)</span><span class="sxs-lookup"><span data-stu-id="025b6-116">WebSockets (Windows 8 or later)</span></span>

<span data-ttu-id="025b6-117">サポートされている Windows バージョン:</span><span class="sxs-lookup"><span data-stu-id="025b6-117">Supported Windows versions:</span></span>

* <span data-ttu-id="025b6-118">Windows 7 以降</span><span class="sxs-lookup"><span data-stu-id="025b6-118">Windows 7 or later</span></span>
* <span data-ttu-id="025b6-119">Windows Server 2008 R2 以降</span><span class="sxs-lookup"><span data-stu-id="025b6-119">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="025b6-120">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="025b6-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="025b6-121">HTTP.sys を使用するタイミング</span><span class="sxs-lookup"><span data-stu-id="025b6-121">When to use HTTP.sys</span></span>

<span data-ttu-id="025b6-122">HTTP.sys は、次のような展開に適しています。</span><span class="sxs-lookup"><span data-stu-id="025b6-122">HTTP.sys is useful for deployments where:</span></span>

* <span data-ttu-id="025b6-123">IIS を使用せず、インターネットに直接サーバーを公開する必要がある。</span><span class="sxs-lookup"><span data-stu-id="025b6-123">There's a need to expose the server directly to the Internet without using IIS.</span></span>

  ![インターネットと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internet.png)

* <span data-ttu-id="025b6-125">内部の展開で、[Windows 認証](xref:security/authentication/windowsauth)などの、Kestrel では使用できない機能が要求されている。</span><span class="sxs-lookup"><span data-stu-id="025b6-125">An internal deployment requires a feature not available in Kestrel, such as [Windows Authentication](xref:security/authentication/windowsauth).</span></span>

  ![内部ネットワークと直接通信する HTTP.sys](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="025b6-127">HTTP.sys は、さまざまな種類の攻撃を防ぎ、フル機能の Web サーバーとして堅牢性、セキュリティ、スケーラビリティを提供する、成熟したテクノロジです。</span><span class="sxs-lookup"><span data-stu-id="025b6-127">HTTP.sys is mature technology that protects against many types of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="025b6-128">IIS 自体が、HTTP.sys 上で HTTP リスナーとして実行されています。</span><span class="sxs-lookup"><span data-stu-id="025b6-128">IIS itself runs as an HTTP listener on top of HTTP.sys.</span></span>

## <a name="http2-support"></a><span data-ttu-id="025b6-129">HTTP/2 のサポート</span><span class="sxs-lookup"><span data-stu-id="025b6-129">HTTP/2 support</span></span>

<span data-ttu-id="025b6-130">[Http/2](https://httpwg.org/specs/rfc7540.html) は、次の基本要件が満たされている場合に、ASP.NET Core アプリに対して有効になります。</span><span class="sxs-lookup"><span data-stu-id="025b6-130">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is enabled for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="025b6-131">Windows Server 2016/Windows 10 以降</span><span class="sxs-lookup"><span data-stu-id="025b6-131">Windows Server 2016/Windows 10 or later</span></span>
* <span data-ttu-id="025b6-132">[アプリケーション レイヤー プロトコル ネゴシエーション (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) 接続</span><span class="sxs-lookup"><span data-stu-id="025b6-132">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="025b6-133">TLS 1.2 以降の接続</span><span class="sxs-lookup"><span data-stu-id="025b6-133">TLS 1.2 or later connection</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="025b6-134">Http/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) が `HTTP/2` を報告します。</span><span class="sxs-lookup"><span data-stu-id="025b6-134">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="025b6-135">Http/2 接続が確立されると、[HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) が `HTTP/1.1` を報告します。</span><span class="sxs-lookup"><span data-stu-id="025b6-135">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/1.1`.</span></span>

::: moniker-end

<span data-ttu-id="025b6-136">HTTP/2 は既定で有効になっています。</span><span class="sxs-lookup"><span data-stu-id="025b6-136">HTTP/2 is enabled by default.</span></span> <span data-ttu-id="025b6-137">Http/2 接続が確立されない場合、接続は http/1.1 にフォールバックします。</span><span class="sxs-lookup"><span data-stu-id="025b6-137">If an HTTP/2 connection isn't established, the connection falls back to HTTP/1.1.</span></span> <span data-ttu-id="025b6-138">Windows の今後のリリースで、HTTP.sys で HTTP/2 を無効にする機能を含む HTTP/2 構成フラグが使用可能になる予定です。</span><span class="sxs-lookup"><span data-stu-id="025b6-138">In a future release of Windows, HTTP/2 configuration flags will be available, including the ability to disable HTTP/2 with HTTP.sys.</span></span>

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="025b6-139">Kerberos を使用したカーネル モード認証</span><span class="sxs-lookup"><span data-stu-id="025b6-139">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="025b6-140">HTTP.sys では、Kerberos 認証プロトコルを使用したカーネル モード認証に処理が委任されます。</span><span class="sxs-lookup"><span data-stu-id="025b6-140">HTTP.sys delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="025b6-141">Kerberos および HTTP.sys ではユーザー モード認証がサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="025b6-141">User mode authentication isn't supported with Kerberos and HTTP.sys.</span></span> <span data-ttu-id="025b6-142">Active Directory から取得され、クライアントによって、ユーザーを認証するサーバーに転送される Kerberos トークン/チケットを暗号化解除するには、コンピューター アカウントを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="025b6-142">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="025b6-143">アプリのユーザーではなく、ホストのサービス プリンシパル名 (SPN) を登録します。</span><span class="sxs-lookup"><span data-stu-id="025b6-143">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-httpsys"></a><span data-ttu-id="025b6-144">HTTP.sys の使用方法</span><span class="sxs-lookup"><span data-stu-id="025b6-144">How to use HTTP.sys</span></span>

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a><span data-ttu-id="025b6-145">HTTP.sys を使用するように ASP.NET Core アプリを構成する</span><span class="sxs-lookup"><span data-stu-id="025b6-145">Configure the ASP.NET Core app to use HTTP.sys</span></span>

1. <span data-ttu-id="025b6-146">[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 以降) を使用する場合は、プロジェクト ファイルのパッケージ参照は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="025b6-146">A package reference in the project file isn't required when using the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="025b6-147">`Microsoft.AspNetCore.App` メタパッケージを使用しない場合は、[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) にパッケージ参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="025b6-147">When not using the `Microsoft.AspNetCore.App` metapackage, add a package reference to [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).</span></span>

2. <span data-ttu-id="025b6-148">Web ホストを構築するときに [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) 拡張メソッドを呼び出し、必要な [HTTP.sys オプション](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions)を指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-148">Call the [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extension method when building the web host, specifying any required [HTTP.sys options](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions):</span></span>

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   <span data-ttu-id="025b6-149">HTTP.sys の追加の構成は、[レジストリ設定](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows)を通じて処理されます。</span><span class="sxs-lookup"><span data-stu-id="025b6-149">Additional HTTP.sys configuration is handled through [registry settings](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).</span></span>

   <span data-ttu-id="025b6-150">**HTTP.sys オプション**</span><span class="sxs-lookup"><span data-stu-id="025b6-150">**HTTP.sys options**</span></span>

   | <span data-ttu-id="025b6-151">プロパティ</span><span class="sxs-lookup"><span data-stu-id="025b6-151">Property</span></span> | <span data-ttu-id="025b6-152">説明</span><span class="sxs-lookup"><span data-stu-id="025b6-152">Description</span></span> | <span data-ttu-id="025b6-153">既定値</span><span class="sxs-lookup"><span data-stu-id="025b6-153">Default</span></span> |
   | -------- | ----------- | :-----: |
   | [<span data-ttu-id="025b6-154">AllowSynchronousIO</span><span class="sxs-lookup"><span data-stu-id="025b6-154">AllowSynchronousIO</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | <span data-ttu-id="025b6-155">`HttpContext.Request.Body` および `HttpContext.Response.Body` に対して、入力/出力の同期を許可するかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="025b6-155">Control whether synchronous input/output is allowed for the `HttpContext.Request.Body` and `HttpContext.Response.Body`.</span></span> | `true` |
   | [<span data-ttu-id="025b6-156">Authentication.AllowAnonymous</span><span class="sxs-lookup"><span data-stu-id="025b6-156">Authentication.AllowAnonymous</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | <span data-ttu-id="025b6-157">匿名要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="025b6-157">Allow anonymous requests.</span></span> | `true` |
   | [<span data-ttu-id="025b6-158">Authentication.Schemes</span><span class="sxs-lookup"><span data-stu-id="025b6-158">Authentication.Schemes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | <span data-ttu-id="025b6-159">許可される認証方式を指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-159">Specify the allowed authentication schemes.</span></span> <span data-ttu-id="025b6-160">リスナーを破棄する前ならいつでも変更できます。</span><span class="sxs-lookup"><span data-stu-id="025b6-160">May be modified at any time prior to disposing the listener.</span></span> <span data-ttu-id="025b6-161">値は [AuthenticationSchemes 列挙型](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes) (`Basic`、`Kerberos`、`Negotiate`、`None`、および `NTLM`) によって指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-161">Values are provided by the [AuthenticationSchemes enum](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None`, and `NTLM`.</span></span> | `None` |
   | [<span data-ttu-id="025b6-162">EnableResponseCaching</span><span class="sxs-lookup"><span data-stu-id="025b6-162">EnableResponseCaching</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | <span data-ttu-id="025b6-163">対象となるヘッダーを持つ応答に対して、[カーネル モード](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode)のキャッシュを試行します。</span><span class="sxs-lookup"><span data-stu-id="025b6-163">Attempt [kernel-mode](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) caching for responses with eligible headers.</span></span> <span data-ttu-id="025b6-164">`Set-Cookie`、`Vary`、または `Pragma` ヘッダーを含む応答は対象外です。</span><span class="sxs-lookup"><span data-stu-id="025b6-164">The response may not include `Set-Cookie`, `Vary`, or `Pragma` headers.</span></span> <span data-ttu-id="025b6-165">応答は、`public` である `Cache-Control` ヘッダーと `shared-max-age` または `max-age` の値のいずれかを含むか、または `Expires` ヘッダーを含む必要があります。</span><span class="sxs-lookup"><span data-stu-id="025b6-165">It must include a `Cache-Control` header that's `public` and either a `shared-max-age` or `max-age` value, or an `Expires` header.</span></span> | `true` |
   | [<span data-ttu-id="025b6-166">MaxAccepts</span><span class="sxs-lookup"><span data-stu-id="025b6-166">MaxAccepts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | <span data-ttu-id="025b6-167">同時受け入れの最大数です。</span><span class="sxs-lookup"><span data-stu-id="025b6-167">The maximum number of concurrent accepts.</span></span> | <span data-ttu-id="025b6-168">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span><span class="sxs-lookup"><span data-stu-id="025b6-168">5 &times; [Environment.<br>ProcessorCount](/dotnet/api/system.environment.processorcount)</span></span> |
   | [<span data-ttu-id="025b6-169">MaxConnections</span><span class="sxs-lookup"><span data-stu-id="025b6-169">MaxConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | <span data-ttu-id="025b6-170">受け入れるコンカレント接続の最大数です。</span><span class="sxs-lookup"><span data-stu-id="025b6-170">The maximum number of concurrent connections to accept.</span></span> <span data-ttu-id="025b6-171">無限にするには、`-1` を使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-171">Use `-1` for infinite.</span></span> <span data-ttu-id="025b6-172">コンピューター全体のレジストリ設定を使用するには、`null` を使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-172">Use `null` to use the registry's machine-wide setting.</span></span> | `null`<br><span data-ttu-id="025b6-173">(無制限)</span><span class="sxs-lookup"><span data-stu-id="025b6-173">(unlimited)</span></span> |
   | [<span data-ttu-id="025b6-174">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="025b6-174">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | <span data-ttu-id="025b6-175">「<a href="#maxrequestbodysize">MaxRequestBodySize</a>」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-175">See the <a href="#maxrequestbodysize">MaxRequestBodySize</a> section.</span></span> | <span data-ttu-id="025b6-176">30000000 バイト</span><span class="sxs-lookup"><span data-stu-id="025b6-176">30000000 bytes</span></span><br><span data-ttu-id="025b6-177">(~28.6 MB)</span><span class="sxs-lookup"><span data-stu-id="025b6-177">(~28.6 MB)</span></span> |
   | [<span data-ttu-id="025b6-178">RequestQueueLimit</span><span class="sxs-lookup"><span data-stu-id="025b6-178">RequestQueueLimit</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | <span data-ttu-id="025b6-179">キューに置くことができる要求の最大数。</span><span class="sxs-lookup"><span data-stu-id="025b6-179">The maximum number of requests that can be queued.</span></span> | <span data-ttu-id="025b6-180">1000</span><span class="sxs-lookup"><span data-stu-id="025b6-180">1000</span></span> |
   | [<span data-ttu-id="025b6-181">ThrowWriteExceptions</span><span class="sxs-lookup"><span data-stu-id="025b6-181">ThrowWriteExceptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | <span data-ttu-id="025b6-182">応答本文の書き込みがクライアントの接続の切断によって失敗した場合、例外をスローするか、または正常に完了するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-182">Indicate if response body writes that fail due to client disconnects should throw exceptions or complete normally.</span></span> | `false`<br><span data-ttu-id="025b6-183">(正常に完了する)</span><span class="sxs-lookup"><span data-stu-id="025b6-183">(complete normally)</span></span> |
   | [<span data-ttu-id="025b6-184">Timeouts</span><span class="sxs-lookup"><span data-stu-id="025b6-184">Timeouts</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | <span data-ttu-id="025b6-185">HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) 構成を公開します。レジストリでも構成することができます。</span><span class="sxs-lookup"><span data-stu-id="025b6-185">Expose the HTTP.sys [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) configuration, which may also be configured in the registry.</span></span> <span data-ttu-id="025b6-186">各設定に関する既定値などの詳細については、API のリンクを参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-186">Follow the API links to learn more about each setting, including default values:</span></span><ul><li><span data-ttu-id="025b6-187">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; HTTP サーバー API が Keep-Alive 接続でエンティティ本体をドレインするまでに許容される時間です。</span><span class="sxs-lookup"><span data-stu-id="025b6-187">[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; Time allowed for the HTTP Server API to drain the entity body on a Keep-Alive connection.</span></span></li><li><span data-ttu-id="025b6-188">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; 要求のエンティティ本体が到着するまでに許容される時間です。</span><span class="sxs-lookup"><span data-stu-id="025b6-188">[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; Time allowed for the request entity body to arrive.</span></span></li><li><span data-ttu-id="025b6-189">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; HTTP サーバー API が要求ヘッダーを解析するまでに許容される時間です。</span><span class="sxs-lookup"><span data-stu-id="025b6-189">[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; Time allowed for the HTTP Server API to parse the request header.</span></span></li><li><span data-ttu-id="025b6-190">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; 接続で許容されるアイドル時間です。</span><span class="sxs-lookup"><span data-stu-id="025b6-190">[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; Time allowed for an idle connection.</span></span></li><li><span data-ttu-id="025b6-191">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; 応答の最小の送信率です。</span><span class="sxs-lookup"><span data-stu-id="025b6-191">[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; The minimum send rate for the response.</span></span></li><li><span data-ttu-id="025b6-192">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; 要求が、アプリにピック アップされるまでに要求キューの中に留まっていられる時間です。</span><span class="sxs-lookup"><span data-stu-id="025b6-192">[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; Time allowed for the request to remain in the request queue before the app picks it up.</span></span></li></ul> |  |
   | [<span data-ttu-id="025b6-193">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="025b6-193">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | <span data-ttu-id="025b6-194">HTTP.sys に登録する [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) を指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-194">Specify the [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) to register with HTTP.sys.</span></span> <span data-ttu-id="025b6-195">最も便利なのは [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add) です。これを使用して、コレクションにプレフィックスを追加できます。</span><span class="sxs-lookup"><span data-stu-id="025b6-195">The most useful is [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), which is used to add a prefix to the collection.</span></span> <span data-ttu-id="025b6-196">これらは、リスナーを破棄する前ならいつでも変更できます。</span><span class="sxs-lookup"><span data-stu-id="025b6-196">These may be modified at any time prior to disposing the listener.</span></span> |  |

   <a name="maxrequestbodysize"></a>
   <span data-ttu-id="025b6-197">**MaxRequestBodySize**</span><span class="sxs-lookup"><span data-stu-id="025b6-197">**MaxRequestBodySize**</span></span>

   <span data-ttu-id="025b6-198">要求本文の最大許容サイズ (バイト単位) です。</span><span class="sxs-lookup"><span data-stu-id="025b6-198">The maximum allowed size of any request body in bytes.</span></span> <span data-ttu-id="025b6-199">`null` に設定する場合、要求本文の最大サイズは制限されません。</span><span class="sxs-lookup"><span data-stu-id="025b6-199">When set to `null`, the maximum request body size is unlimited.</span></span> <span data-ttu-id="025b6-200">この制限は、アップグレード済みの接続 (常に無制限) には影響しません。</span><span class="sxs-lookup"><span data-stu-id="025b6-200">This limit has no effect on upgraded connections, which are always unlimited.</span></span>

   <span data-ttu-id="025b6-201">1 つの `IActionResult` に対する ASP.NET Core MVC アプリの制限をオーバーライドする方法としては、次のように、アクション メソッドに対して [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) 属性を使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="025b6-201">The recommended method to override the limit in an ASP.NET Core MVC app for a single `IActionResult` is to use the [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   <span data-ttu-id="025b6-202">アプリが要求の読み取りを開始した後に、アプリが要求に対する制限を構成しようとすると、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="025b6-202">An exception is thrown if the app attempts to configure the limit on a request after the app has started reading the request.</span></span> <span data-ttu-id="025b6-203">`IsReadOnly` プロパティを使用して、`MaxRequestBodySize` プロパティが読み取り専用状態にあるかどうか、つまり制限を構成するには遅すぎるかどうかを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="025b6-203">An `IsReadOnly` property can be used to indicate if the `MaxRequestBodySize` property is in a read-only state, meaning it's too late to configure the limit.</span></span>

   <span data-ttu-id="025b6-204">アプリで要求ごとに [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) をオーバーライドする必要がある場合は、次のように、[IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature) を使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-204">If the app should override [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) per-request, use the [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):</span></span>

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. <span data-ttu-id="025b6-205">Visual Studio を使用する場合は、アプリが IIS または IIS Express を実行するように構成されていないことを確認します。</span><span class="sxs-lookup"><span data-stu-id="025b6-205">If using Visual Studio, make sure the app isn't configured to run IIS or IIS Express.</span></span>

   <span data-ttu-id="025b6-206">Visual Studio では、既定の起動プロファイルは IIS Express 用です。</span><span class="sxs-lookup"><span data-stu-id="025b6-206">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="025b6-207">プロジェクトをコンソール アプリとして実行するには、次のスクリーン ショットに示すように、選択したプロファイルを手動で変更します。</span><span class="sxs-lookup"><span data-stu-id="025b6-207">To run the project as a console app, manually change the selected profile, as shown in the following screen shot:</span></span>

   ![コンソール アプリのプロファイルを選択する](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a><span data-ttu-id="025b6-209">Windows Server を構成する</span><span class="sxs-lookup"><span data-stu-id="025b6-209">Configure Windows Server</span></span>

1. <span data-ttu-id="025b6-210">アプリに対して開くポートを決めたら、Windows ファイアウォールか [PowerShell コマンドレット](https://technet.microsoft.com/library/jj554906)を使用して、トラフィックが HTTP.sys に到達できるようにファイアウォールのポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="025b6-210">Determine the ports to open for the app and use Windows Firewall or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906) to open firewall ports to allow traffic to reach HTTP.sys.</span></span> <span data-ttu-id="025b6-211">Azure VM に展開する場合は、[ネットワーク セキュリティ グループ](/azure/virtual-network/security-overview)内でポートを開きます。</span><span class="sxs-lookup"><span data-stu-id="025b6-211">When deploying to an Azure VM, open the ports in the [Network Security Group](/azure/virtual-network/security-overview).</span></span> <span data-ttu-id="025b6-212">次のコマンドとアプリの構成では、ポート 443 を使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-212">In the following commands and app configuration, port 443 is used.</span></span>

1. <span data-ttu-id="025b6-213">必要に応じて、X.509 証明書を取得してインストールします。</span><span class="sxs-lookup"><span data-stu-id="025b6-213">Obtain and install X.509 certificates, if required.</span></span>

   <span data-ttu-id="025b6-214">Windows の場合は、[New-SelfSignedCertificate PowerShell コマンドレット](/powershell/module/pkiclient/new-selfsignedcertificate)を使用して自己署名証明書を作成します。</span><span class="sxs-lookup"><span data-stu-id="025b6-214">On Windows, create self-signed certificates using the [New-SelfSignedCertificate PowerShell cmdlet](/powershell/module/pkiclient/new-selfsignedcertificate).</span></span> <span data-ttu-id="025b6-215">サポート対象外の例については、[UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1) を参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-215">For an unsupported example, see [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).</span></span>

   <span data-ttu-id="025b6-216">自己署名証明書か CA 署名証明書のいずれかをサーバーの **Local Machine** > **Personal** ストアにインストールします。</span><span class="sxs-lookup"><span data-stu-id="025b6-216">Install either self-signed or CA-signed certificates in the server's **Local Machine** > **Personal** store.</span></span>

1. <span data-ttu-id="025b6-217">アプリが[フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd)である場合は、.NET Core、.NET Framework、またはその両方 (アプリが .NET Framework をターゲットとする .NET Core アプリである場合) をインストールします。</span><span class="sxs-lookup"><span data-stu-id="025b6-217">If the app is a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd), install .NET Core, .NET Framework, or both (if the app is a .NET Core app targeting the .NET Framework).</span></span>

   * <span data-ttu-id="025b6-218">**.NET Core** &ndash; アプリで .NET Core が必要な場合は、[.NET Core のダウンロード](https://dotnet.microsoft.com/download) ページから **.NET Core Runtime** インストーラーを取得して実行します。</span><span class="sxs-lookup"><span data-stu-id="025b6-218">**.NET Core** &ndash; If the app requires .NET Core, obtain and run the **.NET Core Runtime** installer from [.NET Core Downloads](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="025b6-219">サーバーに SDK 全体をインストールしないでください。</span><span class="sxs-lookup"><span data-stu-id="025b6-219">Don't install the full SDK on the server.</span></span>
   * <span data-ttu-id="025b6-220">**.NET Framework** &ndash; アプリで .NET Framework が必要な場合は、[.NET Framework のインストール ガイド](/dotnet/framework/install/)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-220">**.NET Framework** &ndash; If the app requires .NET Framework, see the [.NET Framework installation guide](/dotnet/framework/install/).</span></span> <span data-ttu-id="025b6-221">必要な .NET Framework をインストールします。</span><span class="sxs-lookup"><span data-stu-id="025b6-221">Install the required .NET Framework.</span></span> <span data-ttu-id="025b6-222">最新の .NET Framework のインストーラーは [.NET Core のダウンロード](https://dotnet.microsoft.com/download) ページから入手できます。</span><span class="sxs-lookup"><span data-stu-id="025b6-222">The installer for the latest .NET Framework is available from from the [.NET Core Downloads](https://dotnet.microsoft.com/download) page.</span></span>

   <span data-ttu-id="025b6-223">アプリが[自己完結型の展開](/dotnet/core/deploying/#framework-dependent-deployments-scd)の場合、アプリの展開内にランタイムが含まれています。</span><span class="sxs-lookup"><span data-stu-id="025b6-223">If the app is a [self-contained deployment](/dotnet/core/deploying/#framework-dependent-deployments-scd), the app includes the runtime in its deployment.</span></span> <span data-ttu-id="025b6-224">サーバーにフレームワークをインストールする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="025b6-224">No framework installation is required on the server.</span></span>

1. <span data-ttu-id="025b6-225">アプリに URL とポートを構成します。</span><span class="sxs-lookup"><span data-stu-id="025b6-225">Configure URLs and ports in the app.</span></span>

   <span data-ttu-id="025b6-226">既定では、ASP.NET Core は `http://localhost:5000` にバインドされます。</span><span class="sxs-lookup"><span data-stu-id="025b6-226">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="025b6-227">URL プレフィックスとポートを構成するには、次のオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="025b6-227">To configure URL prefixes and ports, options include:</span></span>

   * [<span data-ttu-id="025b6-228">UseUrls</span><span class="sxs-lookup"><span data-stu-id="025b6-228">UseUrls</span></span>](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * <span data-ttu-id="025b6-229">`urls` コマンド ライン引数</span><span class="sxs-lookup"><span data-stu-id="025b6-229">`urls` command-line argument</span></span>
   * <span data-ttu-id="025b6-230">`ASPNETCORE_URLS` 環境変数</span><span class="sxs-lookup"><span data-stu-id="025b6-230">`ASPNETCORE_URLS` environment variable</span></span>
   * [<span data-ttu-id="025b6-231">UrlPrefixes</span><span class="sxs-lookup"><span data-stu-id="025b6-231">UrlPrefixes</span></span>](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   <span data-ttu-id="025b6-232">次のコード例は、[UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) でサーバーのローカル IP アドレス `10.0.0.4` とポート 443 を使用する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="025b6-232">The following code example shows how to use [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) with the server's local IP address `10.0.0.4` on port 443:</span></span>

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   <span data-ttu-id="025b6-233">`UrlPrefixes` の利点は、プレフィックスの形式が正しくなかった場合、すぐにエラー メッセージが生成されることです。</span><span class="sxs-lookup"><span data-stu-id="025b6-233">An advantage of `UrlPrefixes` is that an error message is generated immediately for improperly formatted prefixes.</span></span>

   <span data-ttu-id="025b6-234">`UrlPrefixes` の設定は `UseUrls`/`urls`/`ASPNETCORE_URLS` の設定をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="025b6-234">The settings in `UrlPrefixes` override `UseUrls`/`urls`/`ASPNETCORE_URLS` settings.</span></span> <span data-ttu-id="025b6-235">したがって、`UseUrls`、`urls`、および `ASPNETCORE_URLS` 環境変数の利点は、Kestrel と HTTP.sys を簡単に切り替えられることです。</span><span class="sxs-lookup"><span data-stu-id="025b6-235">Therefore, an advantage of `UseUrls`, `urls`, and the `ASPNETCORE_URLS` environment variable is that it's easier to switch between Kestrel and HTTP.sys.</span></span> <span data-ttu-id="025b6-236">詳細については、「<xref:fundamentals/host/web-host>」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-236">For more information, see <xref:fundamentals/host/web-host>.</span></span>

   <span data-ttu-id="025b6-237">HTTP.sys では、[HTTP サーバー API の UrlPrefix 文字列形式](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)が使用されます。</span><span class="sxs-lookup"><span data-stu-id="025b6-237">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

   > [!WARNING]
   > <span data-ttu-id="025b6-238">最上位のワイルドカードのバインド ( `http://*:80/` と `http://+:80` ) は使用しては **いけません** 。</span><span class="sxs-lookup"><span data-stu-id="025b6-238">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="025b6-239">最上位のワイルドカードのバインドを使用すると、アプリにセキュリティの脆弱性が生じます。</span><span class="sxs-lookup"><span data-stu-id="025b6-239">Top-level wildcard bindings create app security vulnerabilities.</span></span> <span data-ttu-id="025b6-240">これは、強力と脆弱の両方のワイルドカードに適用されます。</span><span class="sxs-lookup"><span data-stu-id="025b6-240">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="025b6-241">ワイルドカードではなく、明示的なホスト名か IP アドレスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-241">Use explicit host names or IP addresses rather than wildcards.</span></span> <span data-ttu-id="025b6-242">親ドメイン全体を制御する場合、サブドメインのワイルドカードのバインド (たとえば、`*.mysub.com`) がセキュリティ リスクになることはありません (脆弱である `*.com` とは対照的)。</span><span class="sxs-lookup"><span data-stu-id="025b6-242">Subdomain wildcard binding (for example, `*.mysub.com`) isn't a security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="025b6-243">詳細については、[RFC 7230:セクション 5.4:ホスト](https://tools.ietf.org/html/rfc7230#section-5.4)に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-243">For more information, see [RFC 7230: Section 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).</span></span>

1. <span data-ttu-id="025b6-244">サーバーで URL プレフィックスを事前登録します。</span><span class="sxs-lookup"><span data-stu-id="025b6-244">Preregister URL prefixes on the server.</span></span>

   <span data-ttu-id="025b6-245">HTTP.sys を構成するための組み込みツールは、*netsh.exe* です。</span><span class="sxs-lookup"><span data-stu-id="025b6-245">The built-in tool for configuring HTTP.sys is *netsh.exe*.</span></span> <span data-ttu-id="025b6-246">*netsh.exe* を使用して、URL プレフィックスを予約し、X.509 証明書を割り当てることができます。</span><span class="sxs-lookup"><span data-stu-id="025b6-246">*netsh.exe* is used to reserve URL prefixes and assign X.509 certificates.</span></span> <span data-ttu-id="025b6-247">ツールを使用するには管理者特権が必要です。</span><span class="sxs-lookup"><span data-stu-id="025b6-247">The tool requires administrator privileges.</span></span>

   <span data-ttu-id="025b6-248">*netsh.exe* ツールを使用して、アプリ用に URL を登録します。</span><span class="sxs-lookup"><span data-stu-id="025b6-248">Use the *netsh.exe* tool to register URLs for the app:</span></span>

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * <span data-ttu-id="025b6-249">`<URL>` &ndash; 完全修飾 URL (Uniform Resource Locator)。</span><span class="sxs-lookup"><span data-stu-id="025b6-249">`<URL>` &ndash; The fully qualified Uniform Resource Locator (URL).</span></span> <span data-ttu-id="025b6-250">ワイルドカードのバインドは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="025b6-250">Don't use a wildcard binding.</span></span> <span data-ttu-id="025b6-251">有効なホスト名かローカル IP アドレスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-251">Use a valid hostname or local IP address.</span></span> <span data-ttu-id="025b6-252">"*URL の末尾にはスラッシュが必要です。*"</span><span class="sxs-lookup"><span data-stu-id="025b6-252">*The URL must include a trailing slash.*</span></span>
   * <span data-ttu-id="025b6-253">`<USER>` &ndash; ユーザーまたはユーザー グループの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-253">`<USER>` &ndash; Specifies the user or user-group name.</span></span>

   <span data-ttu-id="025b6-254">次の例では、サーバーのローカル IP アドレスは `10.0.0.4` です。</span><span class="sxs-lookup"><span data-stu-id="025b6-254">In the following example, the local IP address of the server is `10.0.0.4`:</span></span>

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   <span data-ttu-id="025b6-255">URL が登録されると、ツールから `URL reservation successfully added` という応答があります。</span><span class="sxs-lookup"><span data-stu-id="025b6-255">When a URL is registered, the tool responds with `URL reservation successfully added`.</span></span>

   <span data-ttu-id="025b6-256">登録済みの URL を削除するには、`delete urlacl` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-256">To delete a registered URL, use the `delete urlacl` command:</span></span>

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. <span data-ttu-id="025b6-257">サーバーで X.509 証明書を登録します。</span><span class="sxs-lookup"><span data-stu-id="025b6-257">Register X.509 certificates on the server.</span></span>

   <span data-ttu-id="025b6-258">*netsh.exe* ツールを使用して、アプリ用の証明書を登録します。</span><span class="sxs-lookup"><span data-stu-id="025b6-258">Use the *netsh.exe* tool to register certificates for the app:</span></span>

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * <span data-ttu-id="025b6-259">`<IP>` &ndash; バインド用のローカル IP アドレスを指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-259">`<IP>` &ndash; Specifies the local IP address for the binding.</span></span> <span data-ttu-id="025b6-260">ワイルドカードのバインドは使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="025b6-260">Don't use a wildcard binding.</span></span> <span data-ttu-id="025b6-261">有効な IP アドレスを使用してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-261">Use a valid IP address.</span></span>
   * <span data-ttu-id="025b6-262">`<PORT>` &ndash; バインド用のポートを指定します。</span><span class="sxs-lookup"><span data-stu-id="025b6-262">`<PORT>` &ndash; Specifies the port for the binding.</span></span>
   * <span data-ttu-id="025b6-263">`<THUMBPRINT>` &ndash; X.509 証明書の拇印です。</span><span class="sxs-lookup"><span data-stu-id="025b6-263">`<THUMBPRINT>` &ndash; The X.509 certificate thumbprint.</span></span>
   * <span data-ttu-id="025b6-264">`<GUID>` &ndash; 情報提供を目的として開発者によって生成された、アプリを表す GUID です。</span><span class="sxs-lookup"><span data-stu-id="025b6-264">`<GUID>` &ndash; A developer-generated GUID to represent the app for informational purposes.</span></span>

   <span data-ttu-id="025b6-265">参照用に、この GUID をパッケージ タグとしてアプリに格納します。</span><span class="sxs-lookup"><span data-stu-id="025b6-265">For reference purposes, store the GUID in the app as a package tag:</span></span>

   * <span data-ttu-id="025b6-266">Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="025b6-266">In Visual Studio:</span></span>
     * <span data-ttu-id="025b6-267">**ソリューション エクスプローラー**内でアプリを右クリックし、**[プロパティ]** をクリックして、アプリのプロジェクト プロパティを開きます。</span><span class="sxs-lookup"><span data-stu-id="025b6-267">Open the app's project properties by right-clicking on the app in **Solution Explorer** and selecting **Properties**.</span></span>
     * <span data-ttu-id="025b6-268">**[パッケージ]** タブを選択します。</span><span class="sxs-lookup"><span data-stu-id="025b6-268">Select the **Package** tab.</span></span>
     * <span data-ttu-id="025b6-269">作成した GUID を **[タグ]** フィールドに入力します。</span><span class="sxs-lookup"><span data-stu-id="025b6-269">Enter the GUID that you created in the **Tags** field.</span></span>
   * <span data-ttu-id="025b6-270">Visual Studio を使用しない場合:</span><span class="sxs-lookup"><span data-stu-id="025b6-270">When not using Visual Studio:</span></span>
     * <span data-ttu-id="025b6-271">アプリのプロジェクト ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="025b6-271">Open the app's project file.</span></span>
     * <span data-ttu-id="025b6-272">作成した GUID を指定した `<PackageTags>` プロパティを、新規または既存の `<PropertyGroup>` に追加します。</span><span class="sxs-lookup"><span data-stu-id="025b6-272">Add a `<PackageTags>` property to a new or existing `<PropertyGroup>` with the GUID that you created:</span></span>

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   <span data-ttu-id="025b6-273">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="025b6-273">In the following example:</span></span>

   * <span data-ttu-id="025b6-274">サーバーのローカル IP アドレスは `10.0.0.4` です。</span><span class="sxs-lookup"><span data-stu-id="025b6-274">The local IP address of the server is `10.0.0.4`.</span></span>
   * <span data-ttu-id="025b6-275">オンラインのランダム GUID ジェネレーターによって、`appid` の値が提供されます。</span><span class="sxs-lookup"><span data-stu-id="025b6-275">An online random GUID generator provides the `appid` value.</span></span>

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   <span data-ttu-id="025b6-276">証明書が登録されると、ツールから `SSL Certificate successfully added` という応答があります。</span><span class="sxs-lookup"><span data-stu-id="025b6-276">When a certificate is registered, the tool responds with `SSL Certificate successfully added`.</span></span>

   <span data-ttu-id="025b6-277">証明書の登録を削除するには、`delete sslcert` コマンドを使用します。</span><span class="sxs-lookup"><span data-stu-id="025b6-277">To delete a certificate registration, use the `delete sslcert` command:</span></span>

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   <span data-ttu-id="025b6-278">以下は、*netsh.exe* のリファレンス ドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="025b6-278">Reference documentation for *netsh.exe*:</span></span>

   * <span data-ttu-id="025b6-279">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx) (ハイパーテキスト転送プロトコル (HTTP) 用の Netsh コマンド)</span><span class="sxs-lookup"><span data-stu-id="025b6-279">[Netsh Commands for Hypertext Transfer Protocol (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)</span></span>
   * <span data-ttu-id="025b6-280">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx) (UrlPrefix 文字列)</span><span class="sxs-lookup"><span data-stu-id="025b6-280">[UrlPrefix Strings](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)</span></span>

1. <span data-ttu-id="025b6-281">アプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="025b6-281">Run the app.</span></span>

   <span data-ttu-id="025b6-282">1024 より大きいポート番号で (HTTPS ではなく) HTTP を使用して localhost にバインドする場合、アプリの実行に管理者権限は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="025b6-282">Administrator privileges aren't required to run the app when binding to localhost using HTTP (not HTTPS) with a port number greater than 1024.</span></span> <span data-ttu-id="025b6-283">その他の構成の場合 (たとえば、ローカル IP アドレスを使用する場合やポート 443 にバインドする場合)、管理者権限でアプリを実行します。</span><span class="sxs-lookup"><span data-stu-id="025b6-283">For other configurations (for example, using a local IP address or binding to port 443), run the app with administrator privileges.</span></span>

   <span data-ttu-id="025b6-284">サーバーのパブリック IP アドレスでアプリが応答します。</span><span class="sxs-lookup"><span data-stu-id="025b6-284">The app responds at the server's public IP address.</span></span> <span data-ttu-id="025b6-285">この例では、サーバーは自身のパブリック IP アドレス `104.214.79.47` でインターネットからアクセスされます。</span><span class="sxs-lookup"><span data-stu-id="025b6-285">In this example, the server is reached from the Internet at its public IP address of `104.214.79.47`.</span></span>

   <span data-ttu-id="025b6-286">この例では開発証明書が使用されています。</span><span class="sxs-lookup"><span data-stu-id="025b6-286">A development certificate is used in this example.</span></span> <span data-ttu-id="025b6-287">証明書が信頼できないというブラウザーの警告がバイパスされた後に、ページが安全に読み込まれます。</span><span class="sxs-lookup"><span data-stu-id="025b6-287">The page loads securely after bypassing the browser's untrusted certificate warning.</span></span>

   ![読み込まれたアプリのインデックス ページを表示するブラウザー ウィンドウ](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="025b6-289">プロキシ サーバーとロード バランサーのシナリオ</span><span class="sxs-lookup"><span data-stu-id="025b6-289">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="025b6-290">インターネットや企業ネットワークからの要求とやりとりする HTTP.sys でホストされるアプリの場合、プロキシ サーバーやロード バランサーの背後でホストするとき、追加の構成が必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="025b6-290">For apps hosted by HTTP.sys that interact with requests from the Internet or a corporate network, additional configuration might be required when hosting behind proxy servers and load balancers.</span></span> <span data-ttu-id="025b6-291">詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="025b6-291">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="025b6-292">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="025b6-292">Additional resources</span></span>

* [<span data-ttu-id="025b6-293">HTTP.sys を使用して Windows 認証を有効にする</span><span class="sxs-lookup"><span data-stu-id="025b6-293">Enable Windows Authentication with HTTP.sys</span></span>](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [<span data-ttu-id="025b6-294">HTTP サーバー API</span><span class="sxs-lookup"><span data-stu-id="025b6-294">HTTP Server API</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [<span data-ttu-id="025b6-295">aspnet/HttpSysServer GitHub リポジトリ (ソース コード)</span><span class="sxs-lookup"><span data-stu-id="025b6-295">aspnet/HttpSysServer GitHub repository (source code)</span></span>](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
