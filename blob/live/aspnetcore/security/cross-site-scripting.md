---
title: "クロスサイト スクリプティングの防止"
author: rick-anderson
description: "このドキュメントでは、クロスサイト スクリプト (XSS) と ASP.NET Core アプリケーションでこの脆弱性に対処するための手法を紹介します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: af73a86aa6bcde084ecbe1a3fb5711c7da55871c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-cross-site-scripting"></a><span data-ttu-id="90e52-103">クロスサイト スクリプティングの防止</span><span class="sxs-lookup"><span data-stu-id="90e52-103">Preventing Cross-Site Scripting</span></span>

<span data-ttu-id="90e52-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="90e52-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="90e52-105">クロス サイト スクリプト (XSS) は、セキュリティの脆弱性に攻撃者がクライアント側スクリプト (JavaScript では通常) を web ページに配置することができます。</span><span class="sxs-lookup"><span data-stu-id="90e52-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="90e52-106">他のユーザーは、該当するページが、攻撃者がスクリプト実行を読み込み、ときに、攻撃者が cookie やセッション トークンを盗んだりを有効にする DOM を介した web ページの内容を変更または別のページにブラウザーをリダイレクトします。</span><span class="sxs-lookup"><span data-stu-id="90e52-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="90e52-107">XSS の脆弱性は、通常、アプリケーション ユーザーの入力を受け取りを検証する、エンコードまたはエスケープすることなしは、ページの出力時に行われます。</span><span class="sxs-lookup"><span data-stu-id="90e52-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="90e52-108">XSS に対してアプリケーションを保護します。</span><span class="sxs-lookup"><span data-stu-id="90e52-108">Protecting your application against XSS</span></span>

<span data-ttu-id="90e52-109">挿入するアプリケーションを聞き出すによって at 基本的なレベル XSS 機能、`<script>`タグにレンダリングされるページ、または挿入することで、`On*`要素内にイベント。</span><span class="sxs-lookup"><span data-stu-id="90e52-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="90e52-110">開発者は、そのアプリケーションに XSS を導入しないように、次の予防手順を使ってください。</span><span class="sxs-lookup"><span data-stu-id="90e52-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="90e52-111">しないで、次の手順の残りの部分に従わない場合、HTML 入力に信頼されていないデータを配置します。</span><span class="sxs-lookup"><span data-stu-id="90e52-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="90e52-112">信頼されていないデータは、攻撃者が、HTML フォームの入力、クエリ文字列、HTTP ヘッダーは、攻撃者は、アプリケーションを侵害することはできない場合でも、データベースを侵害できる場合があります、データベースからソース データでさえによって制御されている可能性がありますすべてのデータです。</span><span class="sxs-lookup"><span data-stu-id="90e52-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="90e52-113">HTML 要素内で信頼されていないデータを配置する前に HTML エンコードであることを確認します。</span><span class="sxs-lookup"><span data-stu-id="90e52-113">Before putting untrusted data inside an HTML element ensure it is HTML encoded.</span></span> <span data-ttu-id="90e52-114">などの文字は、HTML エンコード&lt;のように安全な形式に変更および&amp;lt;</span><span class="sxs-lookup"><span data-stu-id="90e52-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="90e52-115">信頼されていないデータを HTML 属性に配置する前にエンコードされた HTML 属性を確認します。</span><span class="sxs-lookup"><span data-stu-id="90e52-115">Before putting untrusted data into an HTML attribute ensure it is HTML attribute encoded.</span></span> <span data-ttu-id="90e52-116">HTML 属性エンコード HTML エンコーディングのスーパー セットでありなど、追加の文字をエンコード"と ' です。</span><span class="sxs-lookup"><span data-stu-id="90e52-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="90e52-117">JavaScript に信頼されていないデータを配置する前に、実行時に取得する内容を HTML 要素にデータを配置します。</span><span class="sxs-lookup"><span data-stu-id="90e52-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="90e52-118">このことはできません、データを JavaScript エンコードされています。</span><span class="sxs-lookup"><span data-stu-id="90e52-118">If this is not possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="90e52-119">JavaScript エンコーディング JavaScript の危険性のある文字を取得しに置き換え、16 進数には、たとえば&lt;としてエンコードされる`\u003C`です。</span><span class="sxs-lookup"><span data-stu-id="90e52-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="90e52-120">URL クエリ文字列に信頼されていないデータを配置する前に URL エンコードされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="90e52-120">Before putting untrusted data into a URL query string ensure it is URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="90e52-121">Razor を使用して HTML エンコード</span><span class="sxs-lookup"><span data-stu-id="90e52-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="90e52-122">MVC で自動的に使用される、Razor エンジン エンコードすべて苦労して本当にそのようにしない限り、変数が出力に基づいています。</span><span class="sxs-lookup"><span data-stu-id="90e52-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="90e52-123">ルールのエンコードを使用するときに HTML 属性を使用して、  *@* ディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="90e52-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="90e52-124">HTML 属性エンコードは HTML エンコード HTML エンコーディングまたは HTML 属性エンコードを使用するかどうかを意識する必要はありませんつまりのスーパー セットです。</span><span class="sxs-lookup"><span data-stu-id="90e52-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="90e52-125">使用することのみコンテキストでは、HTML、JavaScript に直接信頼されていない入力を挿入しようとしています。 ときではなくを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90e52-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="90e52-126">タグ ヘルパーは、タグのパラメーターで使用する入力もエンコードされます。</span><span class="sxs-lookup"><span data-stu-id="90e52-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="90e52-127">次の Razor ビュー; の実行します。</span><span class="sxs-lookup"><span data-stu-id="90e52-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="90e52-128">このビューの内容を出力する、 *untrustedInput*変数。</span><span class="sxs-lookup"><span data-stu-id="90e52-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="90e52-129">この変数には、つまり XSS 攻撃に使用されるいくつかの文字が含まれています。 &lt;、"と&gt;です。</span><span class="sxs-lookup"><span data-stu-id="90e52-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="90e52-130">ソースを調べるには、エンコードとして表示される出力は示しています。</span><span class="sxs-lookup"><span data-stu-id="90e52-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="90e52-131">ASP.NET Core MVC の提供、`HtmlString`クラスは出力時に自動的にエンコードされていません。</span><span class="sxs-lookup"><span data-stu-id="90e52-131">ASP.NET Core MVC provides an `HtmlString` class which is not automatically encoded upon output.</span></span> <span data-ttu-id="90e52-132">これは、必要がありますで使用しないで信頼関係のない入力と組み合わせて XSS 脆弱性にさらされるこのされます。</span><span class="sxs-lookup"><span data-stu-id="90e52-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="90e52-133">Razor を使用して Javascript のエンコード</span><span class="sxs-lookup"><span data-stu-id="90e52-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="90e52-134">ビューで処理する JavaScript に値を挿入する回数である可能性があります。</span><span class="sxs-lookup"><span data-stu-id="90e52-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="90e52-135">これには、2 つの方法があります。</span><span class="sxs-lookup"><span data-stu-id="90e52-135">There are two ways to do this.</span></span> <span data-ttu-id="90e52-136">単純な値を挿入する最も安全な方法は、タグのデータの属性に値を設定し、JavaScript で取得します。</span><span class="sxs-lookup"><span data-stu-id="90e52-136">The safest way to insert simple values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="90e52-137">例:</span><span class="sxs-lookup"><span data-stu-id="90e52-137">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="90e52-138">次の HTML が生成されます。</span><span class="sxs-lookup"><span data-stu-id="90e52-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="90e52-139">実行時に表示されます以下です。</span><span class="sxs-lookup"><span data-stu-id="90e52-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="90e52-140">JavaScript のエンコーダーを直接呼び出すことができますも</span><span class="sxs-lookup"><span data-stu-id="90e52-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="90e52-141">これによって、ブラウザーで次のようにします。</span><span class="sxs-lookup"><span data-stu-id="90e52-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="90e52-142">DOM 要素を作成する JavaScript で信頼されていない入力は連結しないでください。</span><span class="sxs-lookup"><span data-stu-id="90e52-142">Do not concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="90e52-143">使用する必要があります`createElement()`プロパティの値を次のように適切に割り当てると`node.TextContent=`、使用または`element.SetAttribute()` / `element[attribute]=`それ以外の場合に公開する自分で DOM ベース XSS です。</span><span class="sxs-lookup"><span data-stu-id="90e52-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="90e52-144">コード内のエンコーダーへのアクセス</span><span class="sxs-lookup"><span data-stu-id="90e52-144">Accessing encoders in code</span></span>

<span data-ttu-id="90e52-145">HTML、JavaScript、および URL のエンコーダーでは、2 つの方法でコードに使用できる、経由でそれらを挿入する[依存性の注入](../fundamentals/dependency-injection.md#fundamentals-dependency-injection)に含まれている既定のエンコーダーを使用することも、`System.Text.Encodings.Web`名前空間。</span><span class="sxs-lookup"><span data-stu-id="90e52-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="90e52-146">文字の範囲に適用するいずれかの既定のエンコーダーを使用する場合を安全なコントロールとして扱う場合に反映されません - 既定のエンコーダーが可能な最も安全なエンコードの規則を使用します。</span><span class="sxs-lookup"><span data-stu-id="90e52-146">If you use the default encoders then any  you applied to character ranges to be treated as safe will not take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="90e52-147">DI、コンス トラクターを受け取る必要がありますを使用して構成可能なエンコーダーを使用する、 *HtmlEncoder*、 *JavaScriptEncoder*と*UrlEncoder*として適切なパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="90e52-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="90e52-148">例を示します。</span><span class="sxs-lookup"><span data-stu-id="90e52-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="90e52-149">エンコードの URL パラメーター</span><span class="sxs-lookup"><span data-stu-id="90e52-149">Encoding URL Parameters</span></span>

<span data-ttu-id="90e52-150">として値を使用して信頼されていない入力を持つ URL クエリ文字列を作成する場合、`UrlEncoder`値をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="90e52-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="90e52-151">たとえば、オブジェクトに適用された</span><span class="sxs-lookup"><span data-stu-id="90e52-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="90e52-152">変数を含む、encodedValue のエンコード後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`です。</span><span class="sxs-lookup"><span data-stu-id="90e52-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="90e52-153">スペース、引用符、句読点、およびその他の安全でない文字パーセント エンコードされる 16 進数の値には、たとえば、空白文字になる %20。</span><span class="sxs-lookup"><span data-stu-id="90e52-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="90e52-154">URL パスの一部として信頼されていない入力を使用しません。</span><span class="sxs-lookup"><span data-stu-id="90e52-154">Do not use untrusted input as part of a URL path.</span></span> <span data-ttu-id="90e52-155">常に信頼されていない入力を渡すクエリ文字列値として。</span><span class="sxs-lookup"><span data-stu-id="90e52-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="90e52-156">エンコーダーのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="90e52-156">Customizing the Encoders</span></span>

<span data-ttu-id="90e52-157">既定では、エンコーダーは、基本的なラテン文字の Unicode の範囲に制限された安全なリストを使用され、同等の文字コードとしてその範囲外のすべての文字をエンコードします。</span><span class="sxs-lookup"><span data-stu-id="90e52-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="90e52-158">この動作では、エンコーダーを使用して、文字列を出力として Razor TagHelper、HtmlHelper のレンダリングも影響します。</span><span class="sxs-lookup"><span data-stu-id="90e52-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="90e52-159">この理由は、不明または将来のブラウザーのバグ (以前のブラウザー バグが英語以外の文字の処理に基づく解析をトリップした) を防ぐためにです。</span><span class="sxs-lookup"><span data-stu-id="90e52-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="90e52-160">場合は、web サイトは、ラテン文字以外、中国語などの頻繁に使用します (キリル) またはその他のこれは可能性があります動作です。</span><span class="sxs-lookup"><span data-stu-id="90e52-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="90e52-161">Unicode でスタートアップ中に、アプリケーションに適切な範囲を含めるエンコーダー セーフ リストをカスタマイズする`ConfigureServices()`です。</span><span class="sxs-lookup"><span data-stu-id="90e52-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="90e52-162">たとえば、Razor HtmlHelper を使用する既定の構成を使用して次のようにします。</span><span class="sxs-lookup"><span data-stu-id="90e52-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="90e52-163">Web ページのソースを表示するときにエンコードされている中国語のテキストで、次のように表示された表示されます。</span><span class="sxs-lookup"><span data-stu-id="90e52-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="90e52-164">扱われる文字を拡大するために、エンコーダーで安全な挿入するには、次の行、`ConfigureServices()`メソッド`startup.cs`です。</span><span class="sxs-lookup"><span data-stu-id="90e52-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="90e52-165">この例では、セーフ リストに含める Unicode 範囲 CjkUnifiedIdeographs 拡大変換されます。</span><span class="sxs-lookup"><span data-stu-id="90e52-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="90e52-166">表示される出力になるようになりました</span><span class="sxs-lookup"><span data-stu-id="90e52-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="90e52-167">セーフ リストの範囲は、Unicode コード グラフ、いない言語として指定されます。</span><span class="sxs-lookup"><span data-stu-id="90e52-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="90e52-168">[Unicode 標準](http://unicode.org/)のリストを持つ[グラフをコード](http://www.unicode.org/charts/index.html)文字を含むグラフの検索を行うこともできます。</span><span class="sxs-lookup"><span data-stu-id="90e52-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="90e52-169">各エンコーダーは、Html、JavaScript および Url を個別に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="90e52-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="90e52-170">セーフ リストのカスタマイズには、エンコーダー DI を介してソースのみに影響します。</span><span class="sxs-lookup"><span data-stu-id="90e52-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="90e52-171">エンコーダーを使用してに直接アクセスする場合`System.Text.Encodings.Web.*Encoder.Default`し、既定値、基本的なラテン セーフリストのみが使用されます。</span><span class="sxs-lookup"><span data-stu-id="90e52-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="90e52-172">エンコードの実行の配置場所必要があります。</span><span class="sxs-lookup"><span data-stu-id="90e52-172">Where should encoding take place?</span></span>

<span data-ttu-id="90e52-173">[全般] では、実際には、エンコードが行われる出力の時点でされ、エンコード値は、データベースに保存することはありませんか受け入れられます。</span><span class="sxs-lookup"><span data-stu-id="90e52-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="90e52-174">出力の時点でのエンコードには、クエリ文字列の値を HTML からのデータなどの使用を変更することができます。</span><span class="sxs-lookup"><span data-stu-id="90e52-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="90e52-175">検索する前に値をエンコードすることがなく、データを簡単に検索することができ、エンコーダーに加えられたバグの修正や変更を活用することができます。</span><span class="sxs-lookup"><span data-stu-id="90e52-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="90e52-176">XSS 防止手法として検証</span><span class="sxs-lookup"><span data-stu-id="90e52-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="90e52-177">検証は、XSS 攻撃を制限することで便利なツールを指定できます。</span><span class="sxs-lookup"><span data-stu-id="90e52-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="90e52-178">たとえば、文字 0 ~ 9 のみを含む単純な数値文字列では、XSS 攻撃はトリガーされません。</span><span class="sxs-lookup"><span data-stu-id="90e52-178">For example, a simple numeric string containing only the characters 0-9 will not trigger an XSS attack.</span></span> <span data-ttu-id="90e52-179">検証より複雑な - ユーザー入力に HTML をそのまま使用する場合は HTML 入力を解析できない場合は困難になります。</span><span class="sxs-lookup"><span data-stu-id="90e52-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="90e52-180">マークダウンとその他のテキスト形式では、豊富な入力をより安全なオプションはなります。</span><span class="sxs-lookup"><span data-stu-id="90e52-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="90e52-181">単独での検証に依存しないようにします。</span><span class="sxs-lookup"><span data-stu-id="90e52-181">You should never rely on validation alone.</span></span> <span data-ttu-id="90e52-182">どのような検証を実行しているに関係なく、出力する前に信頼されていない入力は常にエンコードします。</span><span class="sxs-lookup"><span data-stu-id="90e52-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
