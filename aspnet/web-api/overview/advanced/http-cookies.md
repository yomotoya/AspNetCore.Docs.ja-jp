---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API の HTTP Cookie |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/17/2012
ms.topic: article
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 5885586df1d0f67d4e7e04ad88bc4fd1af71dc80
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368406"
---
<a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="b853b-102">ASP.NET Web API の HTTP Cookie</span><span class="sxs-lookup"><span data-stu-id="b853b-102">HTTP Cookies in ASP.NET Web API</span></span>
====================
<span data-ttu-id="b853b-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b853b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b853b-104">このトピックでは、Web API で HTTP クッキーを送受信する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="b853b-104">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="b853b-105">HTTP クッキーの背景</span><span class="sxs-lookup"><span data-stu-id="b853b-105">Background on HTTP Cookies</span></span>

<span data-ttu-id="b853b-106">ここでは、HTTP レベルで cookie を実装する方法について概説します。</span><span class="sxs-lookup"><span data-stu-id="b853b-106">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="b853b-107">詳細についてを参照してください[RFC 6265](http://tools.ietf.org/html/rfc6265)します。</span><span class="sxs-lookup"><span data-stu-id="b853b-107">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="b853b-108">Cookie とは、サーバーが HTTP 応答で送信するデータの一部です。</span><span class="sxs-lookup"><span data-stu-id="b853b-108">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="b853b-109">(省略可能)、クライアントはクッキーを格納し、subsequet 要求で返します。</span><span class="sxs-lookup"><span data-stu-id="b853b-109">The client (optionally) stores the cookie and returns it on subsequet requests.</span></span> <span data-ttu-id="b853b-110">これにより、クライアントとサーバー状態を共有できます。</span><span class="sxs-lookup"><span data-stu-id="b853b-110">This allows the client and server to share state.</span></span> <span data-ttu-id="b853b-111">サーバーには cookie を設定するには、応答 Set-cookie ヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b853b-111">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="b853b-112">Cookie の形式は、省略可能な属性を持つ、名前と値のペアです。</span><span class="sxs-lookup"><span data-stu-id="b853b-112">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="b853b-113">例えば:</span><span class="sxs-lookup"><span data-stu-id="b853b-113">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="b853b-114">属性を持つ例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="b853b-114">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="b853b-115">クライアントにはサーバーに返す cookie は、以降の要求で Cookie ヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="b853b-115">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="b853b-116">HTTP 応答には、複数 Set-cookie ヘッダーを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="b853b-116">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="b853b-117">クッキー ヘッダーを 1 つを使用して複数の cookie をクライアントに返します。</span><span class="sxs-lookup"><span data-stu-id="b853b-117">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="b853b-118">Set-cookie ヘッダーで次の属性では、スコープと cookie の期間が制御されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-118">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="b853b-119">**ドメイン**: どのドメインに cookie を受け取るクライアントに通知します。</span><span class="sxs-lookup"><span data-stu-id="b853b-119">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="b853b-120">たとえば、ドメインが"example.com"の場合は、クライアントは、example.com のすべてのサブドメインに cookie を返します。</span><span class="sxs-lookup"><span data-stu-id="b853b-120">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="b853b-121">指定しない場合、ドメイン、配信元サーバーです。</span><span class="sxs-lookup"><span data-stu-id="b853b-121">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="b853b-122">**パス**: cookie をドメイン内で指定されたパスに制限します。</span><span class="sxs-lookup"><span data-stu-id="b853b-122">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="b853b-123">指定されていない場合は、要求 URI のパスが使用されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-123">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="b853b-124">**有効期限が切れる**: cookie の有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="b853b-124">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="b853b-125">クライアントは、有効期限が切れた cookie を削除します。</span><span class="sxs-lookup"><span data-stu-id="b853b-125">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="b853b-126">**Max Age**: cookie の最大有効期間を設定します。</span><span class="sxs-lookup"><span data-stu-id="b853b-126">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="b853b-127">クライアントは、最大有効期間に達すると、クッキーを削除します。</span><span class="sxs-lookup"><span data-stu-id="b853b-127">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="b853b-128">両方`Expires`と`Max-Age`が設定されている`Max-Age`が優先されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-128">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="b853b-129">どちらも設定されている場合、クライアントは、現在のセッションの終了時に cookie を削除します。</span><span class="sxs-lookup"><span data-stu-id="b853b-129">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="b853b-130">(「セッション」の厳密な意味は、ユーザー エージェントによって決まります。)</span><span class="sxs-lookup"><span data-stu-id="b853b-130">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="b853b-131">ただし、クライアントがクッキーを無視することこともあります。</span><span class="sxs-lookup"><span data-stu-id="b853b-131">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="b853b-132">たとえば、ユーザーには、プライバシー保護のための cookie が無効にする可能性があります。</span><span class="sxs-lookup"><span data-stu-id="b853b-132">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="b853b-133">クライアントは、有効期限、または格納されているクッキーの数を制限する前に、cookie を削除できます。</span><span class="sxs-lookup"><span data-stu-id="b853b-133">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="b853b-134">プライバシー上の理由から、クライアントは多くの場合、ドメインが配信元サーバーと一致しません、「サードパーティ」の cookie を拒否します。</span><span class="sxs-lookup"><span data-stu-id="b853b-134">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="b853b-135">簡単に言えば、これを設定する cookie を取得するのには、サーバーが依存する必要があります。</span><span class="sxs-lookup"><span data-stu-id="b853b-135">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="b853b-136">Web API の cookie</span><span class="sxs-lookup"><span data-stu-id="b853b-136">Cookies in Web API</span></span>

<span data-ttu-id="b853b-137">クッキーを HTTP 応答に追加するには、作成、 **CookieHeaderValue**クッキーを表すインスタンス。</span><span class="sxs-lookup"><span data-stu-id="b853b-137">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="b853b-138">呼び出して、 **AddCookies**で定義されている拡張メソッド、 **System.Net.Http します。HttpResponseHeadersExtensions**クラスに、cookie を追加します。</span><span class="sxs-lookup"><span data-stu-id="b853b-138">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="b853b-139">たとえば、次のコードでは、コント ローラー アクション内のクッキーが追加されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-139">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="b853b-140">注意**AddCookies**の配列を受け取る**CookieHeaderValue**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="b853b-140">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="b853b-141">クライアント要求から cookie を抽出する呼び出し、 **GetCookies**メソッド。</span><span class="sxs-lookup"><span data-stu-id="b853b-141">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="b853b-142">A **CookieHeaderValue**のコレクションを含む**CookieState**インスタンス。</span><span class="sxs-lookup"><span data-stu-id="b853b-142">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="b853b-143">各**CookieState** cookie の 1 つを表します。</span><span class="sxs-lookup"><span data-stu-id="b853b-143">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="b853b-144">取得するインデクサー メソッドを使用して、 **CookieState**名前で示すようにします。</span><span class="sxs-lookup"><span data-stu-id="b853b-144">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="b853b-145">構造化された Cookie のデータ</span><span class="sxs-lookup"><span data-stu-id="b853b-145">Structured Cookie Data</span></span>

<span data-ttu-id="b853b-146">多くのブラウザーの制限数の cookie が格納されます&#8212;合計の数と、ドメインごとの数。</span><span class="sxs-lookup"><span data-stu-id="b853b-146">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="b853b-147">したがって、複数の cookie を設定する代わりに、1 つの cookie に構造化データを格納するのに便利ですができます。</span><span class="sxs-lookup"><span data-stu-id="b853b-147">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="b853b-148">RFC 6265 は、cookie のデータの構造を定義しません。</span><span class="sxs-lookup"><span data-stu-id="b853b-148">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="b853b-149">使用して、 **CookieHeaderValue**クラス、cookie のデータの名前と値のペアの一覧を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="b853b-149">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="b853b-150">これらの名前と値のペアは、Set-cookie ヘッダーの URL でエンコードされたフォーム データとしてエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="b853b-150">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="b853b-151">前のコードでは、次の Set-cookie ヘッダーが生成されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-151">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="b853b-152">**CookieState**クラス インデクサー要求メッセージ内の cookie からサブの値を読み取るメソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="b853b-152">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="b853b-153">例: 設定し、メッセージ ハンドラーで Cookie を取得します。</span><span class="sxs-lookup"><span data-stu-id="b853b-153">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="b853b-154">前の例では、Web API コント ローラー内から cookie を使用する方法を示しました。</span><span class="sxs-lookup"><span data-stu-id="b853b-154">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="b853b-155">別のオプションは、使用する[メッセージ ハンドラー](http-message-handlers.md)します。</span><span class="sxs-lookup"><span data-stu-id="b853b-155">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="b853b-156">コント ローラーとパイプラインの初期には、メッセージのハンドラーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-156">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="b853b-157">メッセージ ハンドラーは、コント ローラーに到達する前に、cookie を要求から読み取りまたはコント ローラーが応答を生成した後、応答に cookie を追加できます。</span><span class="sxs-lookup"><span data-stu-id="b853b-157">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="b853b-158">次のコードでは、セッション Id を作成するためのメッセージ ハンドラーを示します。</span><span class="sxs-lookup"><span data-stu-id="b853b-158">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="b853b-159">セッション ID は、cookie に格納されます。</span><span class="sxs-lookup"><span data-stu-id="b853b-159">The session ID is stored in a cookie.</span></span> <span data-ttu-id="b853b-160">ハンドラーは、セッション cookie の要求を確認します。</span><span class="sxs-lookup"><span data-stu-id="b853b-160">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="b853b-161">ハンドラーが新しいセッション ID を生成する要求にクッキーが含まれていない場合</span><span class="sxs-lookup"><span data-stu-id="b853b-161">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="b853b-162">どちらの場合、ハンドラーが内のセッション ID を格納、 **HttpRequestMessage.Properties**プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="b853b-162">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="b853b-163">また、セッション クッキーを HTTP 応答に追加します。</span><span class="sxs-lookup"><span data-stu-id="b853b-163">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="b853b-164">この実装では、クライアントからのセッション ID がサーバーによって実際に発行されたことを検証しません。</span><span class="sxs-lookup"><span data-stu-id="b853b-164">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="b853b-165">認証の形式として使用しないでください。</span><span class="sxs-lookup"><span data-stu-id="b853b-165">Don't use it as a form of authentication!</span></span> <span data-ttu-id="b853b-166">この例のポイントでは、HTTP クッキー管理を説明します。</span><span class="sxs-lookup"><span data-stu-id="b853b-166">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="b853b-167">コント ローラーからのセッション ID を取得できます、 **HttpRequestMessage.Properties**プロパティ バッグ。</span><span class="sxs-lookup"><span data-stu-id="b853b-167">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
