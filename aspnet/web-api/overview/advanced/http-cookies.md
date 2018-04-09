---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API で HTTP Cookie |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 363ca975cf75b635b766a53eeda87cf957eed60c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="2e1d7-102">ASP.NET Web API で HTTP Cookie</span><span class="sxs-lookup"><span data-stu-id="2e1d7-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="2e1d7-103">作成者 [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2e1d7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2e1d7-104">このトピックでは、Web API で HTTP クッキーを送受信する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="2e1d7-105">HTTP クッキーの背景</span><span class="sxs-lookup"><span data-stu-id="2e1d7-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="2e1d7-106">このセクションでは、HTTP レベルでは cookie を実装する方法の概要を説明します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="2e1d7-107">詳細についてを参照してください[RFC 6265](http://tools.ietf.org/html/rfc6265)です。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="2e1d7-108">Cookie とは、サーバーは、HTTP 応答で送信されるデータの一部です。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="2e1d7-109">(省略可能) クライアントは、cookie を格納し、subsequet 要求に返します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="2e1d7-110">これにより、クライアントとサーバー状態を共有できます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-110">This allows the client and server to share state.</span></span> <span data-ttu-id="2e1d7-111">Cookie を設定するには、サーバーは、応答に Set-cookie ヘッダーを含めます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="2e1d7-112">Cookie の形式は、省略可能な属性の名前と値ペアです。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="2e1d7-113">例えば:</span><span class="sxs-lookup"><span data-stu-id="2e1d7-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="2e1d7-114">属性の使用例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="2e1d7-115">戻るには、cookie をサーバーに、クライアントには、以降の要求で、[Cookie] ヘッダーが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="2e1d7-116">HTTP 応答には、複数の Set-cookie ヘッダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="2e1d7-117">クライアントでは、Cookie のヘッダーを 1 つを使用して複数の cookie を返します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="2e1d7-118">Set-cookie ヘッダーで次の属性によって、スコープと cookie の期間が制御されます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="2e1d7-119">**ドメイン**: のどのドメインは、cookie を受信する必要がありますがクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="2e1d7-120">たとえば、ドメインが"example.com"の場合は、クライアントは、example.com のすべてのサブドメインに cookie を返します。指定しない場合、ドメインは、元のサーバーです。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com. If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="2e1d7-121">**パス**: ドメイン内で指定されたパスに cookie を制限します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-121">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="2e1d7-122">指定されていない場合、要求 URI のパスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-122">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="2e1d7-123">**有効期限が切れる**: cookie の有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-123">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="2e1d7-124">クライアントは、有効期限が切れるときに、cookie を削除します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-124">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="2e1d7-125">**Max-age**: cookie の最大有効期間を設定します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-125">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="2e1d7-126">クライアントは、最大有効期間に達したときに、cookie を削除します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-126">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="2e1d7-127">両方`Expires`と`Max-Age`設定されている`Max-Age`が優先されます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-127">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="2e1d7-128">どちらも設定されている場合、クライアントは、現在のセッションの終了時に、cookie を削除します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-128">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="2e1d7-129">(「セッション」の意味は、ユーザー エージェントによって決まります。)</span><span class="sxs-lookup"><span data-stu-id="2e1d7-129">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="2e1d7-130">ただし、クライアントがクッキーを無視することがある注意してください。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-130">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="2e1d7-131">たとえば、ユーザーには、プライバシー保護のための cookie が無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-131">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="2e1d7-132">クライアントは、有効期限、または保存されている cookie の数を制限する前に、cookie を削除できます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-132">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="2e1d7-133">プライバシー上の理由から、クライアントは多くの場合、ドメインが、元のサーバーと一致しません、「サードパーティ」の cookie を拒否します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-133">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="2e1d7-134">つまり、サーバーでは、上に設定する cookie を取得する保証はありません。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-134">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="2e1d7-135">Web API の cookie</span><span class="sxs-lookup"><span data-stu-id="2e1d7-135">Cookies in Web API</span></span>

<span data-ttu-id="2e1d7-136">HTTP 応答に cookie を追加するには、作成、 **CookieHeaderValue** cookie を表すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-136">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="2e1d7-137">まず、 **AddCookies**で定義されている拡張メソッド、 **System.Net.Http です。HttpResponseHeadersExtensions**クラスに、cookie を追加します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-137">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="2e1d7-138">たとえば、次のコードは、コント ローラーのアクション内の cookie を追加します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-138">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="2e1d7-139">注意して**AddCookies**の配列を受け取る**CookieHeaderValue**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-139">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="2e1d7-140">クライアントの要求から cookie を抽出する呼び出し、 **GetCookies**メソッド。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-140">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="2e1d7-141">A **CookieHeaderValue**のコレクションを格納**CookieState**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-141">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="2e1d7-142">各**CookieState** 1 つの cookie を表します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-142">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="2e1d7-143">取得するインデクサー メソッドを使用して、 **CookieState**名前で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-143">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="2e1d7-144">構造化された Cookie のデータ</span><span class="sxs-lookup"><span data-stu-id="2e1d7-144">Structured Cookie Data</span></span>

<span data-ttu-id="2e1d7-145">ブラウザーの多くの制限数の cookie が格納されます&#8212;総数、およびドメインあたりの数の両方です。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-145">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="2e1d7-146">したがって、複数の cookie を設定する代わりに、1 つの cookie に構造化データを格納すると便利ですができます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-146">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="2e1d7-147">RFC 6265 は、cookie のデータの構造を定義しません。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-147">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="2e1d7-148">使用して、 **CookieHeaderValue**クラス、cookie のデータの名前と値のペアの一覧を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-148">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="2e1d7-149">これらの名前と値のペアが Set-cookie ヘッダーの URL でエンコードされたフォーム データとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-149">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="2e1d7-150">前のコードは、次の Set-cookie ヘッダーを生成します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-150">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="2e1d7-151">**CookieState**クラス値を読み取る、サブ要求メッセージ内の cookie からインデクサー メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-151">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="2e1d7-152">例: 設定し、メッセージのハンドラーで Cookie を取得</span><span class="sxs-lookup"><span data-stu-id="2e1d7-152">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="2e1d7-153">前の例では、Web API コント ローラー内から cookie を使用する方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-153">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="2e1d7-154">別のオプションは、使用する[メッセージ ハンドラー](http-message-handlers.md)です。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-154">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="2e1d7-155">コント ローラーよりも、パイプラインの初期には、メッセージのハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-155">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="2e1d7-156">メッセージ ハンドラーでは、コント ローラーに到達する前に、要求の cookie を読み取るしたり、コント ローラーが応答を生成した後、応答に cookie を追加することができます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-156">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="2e1d7-157">次のコードでは、セッション Id を作成するためのメッセージ ハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-157">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="2e1d7-158">セッション ID は、cookie に格納されます。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-158">The session ID is stored in a cookie.</span></span> <span data-ttu-id="2e1d7-159">ハンドラーでは、セッションの cookie の要求を確認します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-159">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="2e1d7-160">ハンドラーが新しいセッション ID を生成する要求にクッキーが含まれていない場合</span><span class="sxs-lookup"><span data-stu-id="2e1d7-160">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="2e1d7-161">どちらの場合、ハンドラーが内のセッション ID を格納、 **HttpRequestMessage.Properties**プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-161">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="2e1d7-162">また、セッション クッキーを HTTP 応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-162">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="2e1d7-163">この実装では、クライアントからのセッション ID がサーバーによって実際に発行されたことを検証しません。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-163">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="2e1d7-164">認証の形式として使用しません。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-164">Don't use it as a form of authentication!</span></span> <span data-ttu-id="2e1d7-165">例のポイントでは、HTTP クッキー管理を説明します。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-165">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="2e1d7-166">コント ローラーからセッション ID を取得できます、 **HttpRequestMessage.Properties**プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="2e1d7-166">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
