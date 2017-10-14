---
title: "クロスサイト スクリプティングの防止"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 95790927-2bfe-445e-b1fd-429c2c7030ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: d0880fda4ee726bd30a48cce0907a3887f2a4545
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/13/2017
---
# <a name="preventing-cross-site-scripting"></a>クロスサイト スクリプティングの防止

<a name="security-cross-site-scripting"></a>

クロス サイト スクリプト (XSS) は、セキュリティの脆弱性に攻撃者がクライアント側スクリプト (JavaScript では通常) を web ページに配置することができます。 他のユーザーは、該当するページが、攻撃者がスクリプト実行を読み込み、ときに、攻撃者が cookie やセッション トークンを盗んだりを有効にする DOM を介した web ページの内容を変更または別のページにブラウザーをリダイレクトします。 XSS の脆弱性は、通常、アプリケーション ユーザーの入力を受け取りを検証する、エンコードまたはエスケープすることなしは、ページの出力時に行われます。

## <a name="protecting-your-application-against-xss"></a>XSS に対してアプリケーションを保護します。

挿入するアプリケーションを聞き出すによって at 基本的なレベル XSS 機能、`<script>`タグにレンダリングされるページ、または挿入することで、`On*`要素内にイベント。 開発者は、そのアプリケーションに XSS を導入しないように、次の予防手順を使ってください。

1. しないで、次の手順の残りの部分に従わない場合、HTML 入力に信頼されていないデータを配置します。 信頼されていないデータは、攻撃者が、HTML フォームの入力、クエリ文字列、HTTP ヘッダーは、攻撃者は、アプリケーションを侵害することはできない場合でも、データベースを侵害できる場合があります、データベースからソース データでさえによって制御されている可能性がありますすべてのデータです。

2. HTML 要素内で信頼されていないデータを配置する前に HTML エンコードであることを確認します。 などの文字は、HTML エンコード&lt;のように安全な形式に変更および&amp;lt;

3. 信頼されていないデータを HTML 属性に配置する前にエンコードされた HTML 属性を確認します。 HTML 属性エンコード HTML エンコーディングのスーパー セットでありなど、追加の文字をエンコード"と ' です。

4. JavaScript に信頼されていないデータを配置する前に、実行時に取得する内容を HTML 要素にデータを配置します。 このことはできません、データを JavaScript エンコードされています。 JavaScript エンコーディング JavaScript の危険性のある文字を取得しに置き換え、16 進数には、たとえば&lt;としてエンコードされる`\u003C`です。

5. URL クエリ文字列に信頼されていないデータを配置する前に URL エンコードされていることを確認します。

## <a name="html-encoding-using-razor"></a>Razor を使用して HTML エンコード

MVC で自動的に使用される、Razor エンジン エンコードすべて苦労して本当にそのようにしない限り、変数が出力に基づいています。 ルールのエンコードを使用するときに HTML 属性を使用して、  *@* ディレクティブです。 HTML 属性エンコードは HTML エンコード HTML エンコーディングまたは HTML 属性エンコードを使用するかどうかを意識する必要はありませんつまりのスーパー セットです。 使用することのみコンテキストでは、HTML、JavaScript に直接信頼されていない入力を挿入しようとしています。 ときではなくを確認する必要があります。 タグ ヘルパーは、タグのパラメーターで使用する入力もエンコードされます。

次の Razor ビュー; の実行します。

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

このビューの内容を出力する、 *untrustedInput*変数。 この変数には、つまり XSS 攻撃に使用されるいくつかの文字が含まれています。 &lt;、"と&gt;です。 ソースを調べるには、エンコードとして表示される出力は示しています。

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC の提供、`HtmlString`クラスは出力時に自動的にエンコードされていません。 これは、必要がありますで使用しないで信頼関係のない入力と組み合わせて XSS 脆弱性にさらされるこのされます。

## <a name="javascript-encoding-using-razor"></a>Razor を使用して Javascript のエンコード

ビューで処理する JavaScript に値を挿入する回数である可能性があります。 これには、2 つの方法があります。 単純な値を挿入する最も安全な方法は、タグのデータの属性に値を設定し、JavaScript で取得します。 例:

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

次の HTML が生成されます。

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

実行時に表示されます以下です。

```none
<"123">
   <"123">
   ```

JavaScript のエンコーダーを直接呼び出すことができますも

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

これによって、ブラウザーで次のようにします。

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> DOM 要素を作成する JavaScript で信頼されていない入力は連結しないでください。 使用する必要があります`createElement()`プロパティの値を次のように適切に割り当てると`node.TextContent=`、使用または`element.SetAttribute()` / `element[attribute]=`それ以外の場合に公開する自分で DOM ベース XSS です。

## <a name="accessing-encoders-in-code"></a>コード内のエンコーダーへのアクセス

HTML、JavaScript、および URL のエンコーダーでは、2 つの方法でコードに使用できる、経由でそれらを挿入する[依存性の注入](../fundamentals/dependency-injection.md#fundamentals-dependency-injection)に含まれている既定のエンコーダーを使用することも、`System.Text.Encodings.Web`名前空間。 文字の範囲に適用するいずれかの既定のエンコーダーを使用する場合を安全なコントロールとして扱う場合に反映されません - 既定のエンコーダーが可能な最も安全なエンコードの規則を使用します。

DI、コンス トラクターを受け取る必要がありますを使用して構成可能なエンコーダーを使用する、 *HtmlEncoder*、 *JavaScriptEncoder*と*UrlEncoder*として適切なパラメーターです。 例を示します。

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

## <a name="encoding-url-parameters"></a>エンコードの URL パラメーター

として値を使用して信頼されていない入力を持つ URL クエリ文字列を作成する場合、`UrlEncoder`値をエンコードします。 次に例を示します。

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

変数を含む、encodedValue のエンコード後`%22Quoted%20Value%20with%20spaces%20and%20%26%22`です。 スペース、引用符、句読点、およびその他の安全でない文字パーセント エンコードされる 16 進数の値には、たとえば、空白文字になる %20。

>[!WARNING]
> URL パスの一部として信頼されていない入力を使用しません。 常に信頼されていない入力を渡すクエリ文字列値として。

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>エンコーダーのカスタマイズ

既定では、エンコーダーは、基本的なラテン文字の Unicode の範囲に制限された安全なリストを使用され、同等の文字コードとしてその範囲外のすべての文字をエンコードします。 この動作では、エンコーダーを使用して、文字列を出力として Razor TagHelper、HtmlHelper のレンダリングも影響します。

この理由は、不明または将来のブラウザーのバグ (以前のブラウザー バグが英語以外の文字の処理に基づく解析をトリップした) を防ぐためにです。 場合は、web サイトは、ラテン文字以外、中国語などの頻繁に使用します (キリル) またはその他のこれは可能性があります動作です。

Unicode でスタートアップ中に、アプリケーションに適切な範囲を含めるエンコーダー セーフ リストをカスタマイズする`ConfigureServices()`です。

たとえば、Razor HtmlHelper を使用する既定の構成を使用して次のようにします。

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Web ページのソースを表示するときにエンコードされている中国語のテキストで、次のように表示された表示されます。

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

扱われる文字を拡大するために、エンコーダーで安全な挿入するには、次の行、`ConfigureServices()`メソッド`startup.cs`です。

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

この例では、セーフ リストに含める Unicode 範囲 CjkUnifiedIdeographs 拡大変換されます。 表示される出力になるようになりました

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

セーフ リストの範囲は、Unicode コード グラフ、いない言語として指定されます。 [Unicode 標準](http://unicode.org/)のリストを持つ[グラフをコード](http://www.unicode.org/charts/index.html)文字を含むグラフの検索を行うこともできます。 各エンコーダーは、Html、JavaScript および Url を個別に構成する必要があります。

> [!NOTE]
> セーフ リストのカスタマイズには、エンコーダー DI を介してソースのみに影響します。 エンコーダーを使用してに直接アクセスする場合`System.Text.Encodings.Web.*Encoder.Default`し、既定値、基本的なラテン セーフリストのみが使用されます。

## <a name="where-should-encoding-take-place"></a>エンコードの実行の配置場所必要があります。

[全般] では、実際には、エンコードが行われる出力の時点でされ、エンコード値は、データベースに保存することはありませんか受け入れられます。 出力の時点でのエンコードには、クエリ文字列の値を HTML からのデータなどの使用を変更することができます。 検索する前に値をエンコードすることがなく、データを簡単に検索することができ、エンコーダーに加えられたバグの修正や変更を活用することができます。

## <a name="validation-as-an-xss-prevention-technique"></a>XSS 防止手法として検証

検証は、XSS 攻撃を制限することで便利なツールを指定できます。 たとえば、文字 0 ~ 9 のみを含む単純な数値文字列では、XSS 攻撃はトリガーされません。 検証より複雑な - ユーザー入力に HTML をそのまま使用する場合は HTML 入力を解析できない場合は困難になります。 マークダウンとその他のテキスト形式では、豊富な入力をより安全なオプションはなります。 単独での検証に依存しないようにします。 どのような検証を実行しているに関係なく、出力する前に信頼されていない入力は常にエンコードします。
