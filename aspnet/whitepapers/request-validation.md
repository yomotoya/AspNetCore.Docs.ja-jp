---
uid: whitepapers/request-validation
title: 要求の検証 - スクリプト攻撃の防止 |Microsoft Docs
author: rick-anderson
description: このホワイト ペーパーでは、ここで、既定では、アプリケーションはできませんがエンコードされていない HTML コンテンツして処理する ASP.NET の要求の検証機能について説明しています.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809746"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="109bf-103">要求の検証 - スクリプト攻撃を防ぐ</span><span class="sxs-lookup"><span data-stu-id="109bf-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="109bf-104">このホワイト ペーパーでは、ここで、既定では、アプリケーションが回避エンコードされていない HTML コンテンツがサーバーに送信を処理する ASP.NET の要求の検証機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="109bf-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="109bf-105">アプリケーションは HTML のデータを安全に処理するように設計されている場合は、この要求の検証機能を無効にできます。</span><span class="sxs-lookup"><span data-stu-id="109bf-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="109bf-106">ASP.NET 1.1 および ASP.NET 2.0 に適用されます。</span><span class="sxs-lookup"><span data-stu-id="109bf-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="109bf-107">要求の検証は ASP.NET のバージョン 1.1 では、以降の機能は、サーバーがコンテナーのコンテンツのエンコードされていない HTML を受け入れることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="109bf-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="109bf-108">この機能は、クライアント スクリプト コードや HTML 知らないうちに、サーバーに送信された、保存、およびできる他のユーザーに提示されますいくつかのスクリプト インジェクション攻撃を防ぐために設計されています。</span><span class="sxs-lookup"><span data-stu-id="109bf-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="109bf-109">すべての入力データを検証して、HTML エンコード時に適切なことをまだ強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="109bf-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="109bf-110">たとえば、ユーザーの電子メール アドレスと、ストア データベースでの電子メール アドレスを要求する Web ページを作成します。</span><span class="sxs-lookup"><span data-stu-id="109bf-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="109bf-111">ユーザーが入力した場合&lt;スクリプト&gt;アラート (「こんにちは from script」)&lt;/script&gt; 、有効な電子メール アドレスの代わりにそのデータが表示されたら、このスクリプトを実行する、コンテンツが正しくエンコードされていません。</span><span class="sxs-lookup"><span data-stu-id="109bf-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="109bf-112">ASP.NET の要求の検証機能が行われてからこれを防止します。</span><span class="sxs-lookup"><span data-stu-id="109bf-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="109bf-113">この機能が便利な理由</span><span class="sxs-lookup"><span data-stu-id="109bf-113">Why this feature is useful</span></span>

<span data-ttu-id="109bf-114">多くのサイトは、単純なスクリプト インジェクション攻撃に対して開いていることに対応していません。</span><span class="sxs-lookup"><span data-stu-id="109bf-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="109bf-115">これらの攻撃の目的は、HTML を表示することで、サイトの内容を改ざんしたり、可能性のあるハッカーのサイトにユーザーをリダイレクトするクライアント スクリプトを実行するには、スクリプト インジェクション攻撃、Web 開発者に取り組む必要がある問題です。</span><span class="sxs-lookup"><span data-stu-id="109bf-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="109bf-116">スクリプト インジェクション攻撃は、ASP.NET、ASP、またはその他の web 開発テクノロジを使用しているかどうか、すべての web 開発者の問題です。</span><span class="sxs-lookup"><span data-stu-id="109bf-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="109bf-117">ASP.NET 要求の検証機能は、事前にそのコンテンツを許可する、開発者が決定しない限り、サーバーによって処理されるエンコードされていない HTML コンテンツを許可しないことによってこれらの攻撃を防ぎます。</span><span class="sxs-lookup"><span data-stu-id="109bf-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="109bf-118">予想される結果: エラー ページ</span><span class="sxs-lookup"><span data-stu-id="109bf-118">What to expect: Error Page</span></span>

<span data-ttu-id="109bf-119">以下のスクリーン ショットは、いくつかのサンプル ASP.NET コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="109bf-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="109bf-120">このコードの結果を実行して、テキスト ボックスにテキストを入力できるようにする単純なページで、ボタンをクリックし、ラベル コントロールでテキストを表示します。</span><span class="sxs-lookup"><span data-stu-id="109bf-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="109bf-121">ただし、JavaScript をなどが`<script>alert("hello!")</script>`入力し、例外が発生しましたが送信します。</span><span class="sxs-lookup"><span data-stu-id="109bf-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="109bf-122">エラー メッセージの状態を '可能性のある危険な Request.Form 値が検出された' し、詳細は何が発生したと動作を変更する方法を正確に説明を提供します。</span><span class="sxs-lookup"><span data-stu-id="109bf-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="109bf-123">例えば:</span><span class="sxs-lookup"><span data-stu-id="109bf-123">For example:</span></span>

<span data-ttu-id="109bf-124">要求の検証には、危険性のあるクライアントの入力値が検出され、要求の処理は中止されました。</span><span class="sxs-lookup"><span data-stu-id="109bf-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="109bf-125">この値は、クロスサイト スクリプティング攻撃などのアプリケーションのセキュリティを侵害する試行を示す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="109bf-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="109bf-126">要求の検証を無効に設定できます`validateRequest=false`Page ディレクティブまたは構成セクション。</span><span class="sxs-lookup"><span data-stu-id="109bf-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="109bf-127">ただし、アプリケーションに明示的にチェックするすべての入力ケースでこのを強くお勧めします。</span><span class="sxs-lookup"><span data-stu-id="109bf-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="109bf-128">ページの要求の検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="109bf-128">Disabling request validation on a page</span></span>

<span data-ttu-id="109bf-129">設定する必要があります ページの要求の検証を無効にする、`validateRequest`する Page ディレクティブの属性`false`:</span><span class="sxs-lookup"><span data-stu-id="109bf-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="109bf-130">ページにコンテンツを送信できる要求の検証を無効にすると、そのコンテンツを確認するページの開発者の責任が正しくエンコードまたは処理をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="109bf-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="109bf-131">アプリケーションの要求の検証を無効にします。</span><span class="sxs-lookup"><span data-stu-id="109bf-131">Disabling request validation for your application</span></span>

<span data-ttu-id="109bf-132">アプリケーションの要求の検証を無効にする必要があります変更またはアプリケーションの Web.config ファイルを作成しての validateRequest 属性を設定、`<pages />`セクション`false`:</span><span class="sxs-lookup"><span data-stu-id="109bf-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="109bf-133">サーバー上のすべてのアプリケーションの要求の検証を無効にする場合は、Machine.config ファイルには、この変更を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="109bf-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="109bf-134">コンテンツをアプリケーションに送信できる要求の検証を無効にすると、そのコンテンツを確実にアプリケーション開発者の責任が正しくエンコードまたは処理をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="109bf-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="109bf-135">次のコードは、要求の検証を無効に変更されます。</span><span class="sxs-lookup"><span data-stu-id="109bf-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="109bf-136">次の JavaScript は、テキスト ボックスに入力した場合は、今すぐ`<script>alert("hello!")</script>`結果になります。</span><span class="sxs-lookup"><span data-stu-id="109bf-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="109bf-137">これをになっている要求の検証を防ぐには、html コンテンツでエンコードします必要があります。</span><span class="sxs-lookup"><span data-stu-id="109bf-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="109bf-138">Html コンテンツをエンコード方法</span><span class="sxs-lookup"><span data-stu-id="109bf-138">How to HTML encode content</span></span>

<span data-ttu-id="109bf-139">要求の検証を無効にした場合は、将来使用するために格納されるコンテンツの HTML エンコードすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="109bf-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="109bf-140">HTML エンコーディングが自動的に置き換え、'&lt;'または'&gt;' (とその他のいくつかの記号) で、対応する HTML でエンコードされた表現。</span><span class="sxs-lookup"><span data-stu-id="109bf-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="109bf-141">たとえば、'&lt;'に置換されます'&amp;lt;' と '&gt;'に置換されます'&amp;gt;'。</span><span class="sxs-lookup"><span data-stu-id="109bf-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="109bf-142">ブラウザーでは、これらの特殊なコードを使用して、表示を '&lt;'または'&gt;' ブラウザーにします。</span><span class="sxs-lookup"><span data-stu-id="109bf-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="109bf-143">コンテンツを使用してサーバーに簡単に HTML エンコードすることができます、 `Server.HtmlEncode(string)` API。</span><span class="sxs-lookup"><span data-stu-id="109bf-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="109bf-144">コンテンツできますも簡単に HTML でデコードされては、標準の HTML を使用してに戻す、`Server.HtmlDecode(string)`メソッド。</span><span class="sxs-lookup"><span data-stu-id="109bf-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="109bf-145">その結果。</span><span class="sxs-lookup"><span data-stu-id="109bf-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
