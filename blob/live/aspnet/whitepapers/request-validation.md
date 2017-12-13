---
uid: whitepapers/request-validation
title: "要求の検証 - スクリプト攻撃の防止 |Microsoft ドキュメント"
author: rick-anderson
description: "このペーパーでは、ここで、既定では、アプリケーションが回避エンコードされていない HTML コンテンツして処理から ASP.NET の要求の検証機能について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="88926-103">要求の検証 - スクリプト攻撃の防止</span><span class="sxs-lookup"><span data-stu-id="88926-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="88926-104">このホワイト ペーパーでは、ここで、既定では、アプリケーションが回避サーバーに送信されたエンコードされていない HTML コンテンツを処理から ASP.NET の要求の検証機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="88926-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="88926-105">HTML のデータを安全に処理するアプリケーションは設計されて 場合は、この要求の検証機能を無効にすることができます。</span><span class="sxs-lookup"><span data-stu-id="88926-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="88926-106">ASP.NET 1.1 および ASP.NET 2.0 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="88926-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="88926-107">要求の検証、ASP.NET のバージョン 1.1 では、以降の機能は、サーバーがコンテンツを含むエンコードされていない HTML を受け入れることを防止します。</span><span class="sxs-lookup"><span data-stu-id="88926-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="88926-108">この機能は、クライアント スクリプト コードまたは HTML 知らずにサーバーに送信、格納、し、他のユーザーに提示し、いくつかのスクリプト インジェクション攻撃を防ぐために設計されています。</span><span class="sxs-lookup"><span data-stu-id="88926-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="88926-109">すべての入力データを検証することと、適切なときに HTML エンコードをまだ強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88926-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="88926-110">たとえば、ユーザーの電子メール アドレスを要求し、データベースにその電子メール アドレスを格納する Web ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="88926-110">For example, you create a Web page that requests a user's e-mail address and then stores that e-mail address in a database.</span></span> <span data-ttu-id="88926-111">ユーザーが入力した場合&lt;スクリプト&gt;アラート (「こんにちはスクリプトから」)&lt;/script&gt;有効な電子メール アドレスの代わりにそのデータが表示されたら、このスクリプトを実行できる場合は、コンテンツが正しくエンコードされていません。</span><span class="sxs-lookup"><span data-stu-id="88926-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid e-mail address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="88926-112">ASP.NET の要求の検証機能は、問題が起こらないこれを防止します。</span><span class="sxs-lookup"><span data-stu-id="88926-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="88926-113">この機能は役立ちます理由</span><span class="sxs-lookup"><span data-stu-id="88926-113">Why this feature is useful</span></span>

<span data-ttu-id="88926-114">多くのサイトは、単純なスクリプト インジェクション攻撃に対して開いていることに対応していません。</span><span class="sxs-lookup"><span data-stu-id="88926-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="88926-115">このような攻撃の目的は、HTML を表示することによって、サイトの内容を改ざんまたは可能性のあるハッカーのサイトにユーザーをリダイレクトするクライアント スクリプトを実行するには、スクリプト インジェクション攻撃、Web 開発者必要がありますどうしは競合している問題です。</span><span class="sxs-lookup"><span data-stu-id="88926-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="88926-116">スクリプト インジェクション攻撃は、すべての web 開発者の ASP.NET、ASP、またはその他の web 開発テクノロジを使用しているかどうかです。</span><span class="sxs-lookup"><span data-stu-id="88926-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="88926-117">事前対応的に、ASP.NET 要求の検証機能は、開発者は、そのコンテンツを許可することにした場合を除き、サーバーによって処理される、エンコードされていない HTML コンテンツを許可しないことによってこれらの攻撃を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="88926-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="88926-118">予想される結果: エラー ページ</span><span class="sxs-lookup"><span data-stu-id="88926-118">What to expect: Error Page</span></span>

<span data-ttu-id="88926-119">以下のスクリーン ショットは、いくつかのサンプルの ASP.NET コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="88926-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="88926-120">これにより、コードを実行して、テキスト ボックスにいくつかのテキストを入力することができる、単純なページで、ボタンをクリックし、ラベル コントロールでテキストを表示します。</span><span class="sxs-lookup"><span data-stu-id="88926-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="88926-121">ただし、JavaScript をなどが`<script>alert("hello!")</script>`を入力して送信された例外になります。</span><span class="sxs-lookup"><span data-stu-id="88926-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="88926-122">エラー メッセージの状態を '危険性のある Request.Form 値が検出された' し、詳細は何が発生したと動作を変更する方法を正確に説明を提供します。</span><span class="sxs-lookup"><span data-stu-id="88926-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="88926-123">例:</span><span class="sxs-lookup"><span data-stu-id="88926-123">For example:</span></span>

<span data-ttu-id="88926-124">要求の検証が危険性のあるクライアントの入力値を検出し、要求の処理は中止されました。</span><span class="sxs-lookup"><span data-stu-id="88926-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="88926-125">この値は、クロスサイト スクリプティング攻撃など、アプリケーションのセキュリティを侵害しようである可能性があります。</span><span class="sxs-lookup"><span data-stu-id="88926-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="88926-126">要求の検証を無効に設定できます`validateRequest=false`ページ ディレクティブまたは構成セクション。</span><span class="sxs-lookup"><span data-stu-id="88926-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="88926-127">ただし、アプリケーション明示的にチェックするすべての入力ケースでこのを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88926-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="88926-128">ページの要求の検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="88926-128">Disabling request validation on a page</span></span>

<span data-ttu-id="88926-129">設定する必要があります ページで要求の検証を無効にする、`validateRequest`ページ ディレクティブの属性`false`:</span><span class="sxs-lookup"><span data-stu-id="88926-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="88926-130">コンテンツは、ページに送信する要求の検証を無効にすると、そのコンテンツを確実にページの開発者の責任においてが正しくエンコードまたは処理をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88926-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="88926-131">アプリケーションの要求の検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="88926-131">Disabling request validation for your application</span></span>

<span data-ttu-id="88926-132">アプリケーションの要求の検証を無効にする必要がありますを変更または、アプリケーションの Web.config ファイルを作成しのクロス属性を設定、`<pages />`セクション`false`:</span><span class="sxs-lookup"><span data-stu-id="88926-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="88926-133">サーバー上のすべてのアプリケーションの要求の検証を無効にする場合は、Machine.config ファイルには、この変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="88926-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="88926-134">コンテンツがアプリケーションに送信する要求の検証を無効にすると、アプリケーション開発者がそのコンテンツを確認する必要が正しくエンコードまたは処理をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88926-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="88926-135">次のコードは、要求の検証をオフに変更されます。</span><span class="sxs-lookup"><span data-stu-id="88926-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="88926-136">ここで、次の JavaScript は、テキスト ボックスに入力された`<script>alert("hello!")</script>`結果になります。</span><span class="sxs-lookup"><span data-stu-id="88926-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="88926-137">これが発生しないように、要求の検証を無効になっていると、お必要がありますを HTML エンコード、コンテンツ。</span><span class="sxs-lookup"><span data-stu-id="88926-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="88926-138">方法を HTML コンテンツをエンコード</span><span class="sxs-lookup"><span data-stu-id="88926-138">How to HTML encode content</span></span>

<span data-ttu-id="88926-139">要求の検証を無効にした場合は、将来使用するために格納されるコンテンツの HTML エンコードすることお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88926-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="88926-140">HTML エンコードは自動的に置き換える '&lt;'または'&gt;' (およびその他のいくつかの記号) と、対応する HTML でエンコードされた表現です。</span><span class="sxs-lookup"><span data-stu-id="88926-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="88926-141">たとえば、'&lt;'は置き換え'&amp;lt;' と '&gt;'は置き換え'&amp;gt;'。</span><span class="sxs-lookup"><span data-stu-id="88926-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="88926-142">ブラウザーでは、これらの特別なコードを使用して、表示を '&lt;'または'&gt;' ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="88926-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="88926-143">コンテンツを使用して、サーバーに簡単に HTML エンコードする、 `Server.HtmlEncode(string)` API です。</span><span class="sxs-lookup"><span data-stu-id="88926-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="88926-144">コンテンツすることもできます簡単に HTML デコード、つまり、標準を使用して HTML に戻す、`Server.HtmlDecode(string)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="88926-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="88926-145">その結果。</span><span class="sxs-lookup"><span data-stu-id="88926-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
