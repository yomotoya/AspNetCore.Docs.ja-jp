---
title: "ASP.NET Core の razor 構文のリファレンス"
author: guardrex
description: "サーバー ベースのコードを埋め込む web ページの Razor マークアップの構文について説明します。"
keywords: "ASP.NET Core、Razor、Razor ディレクティブ"
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>ASP.NET Core の razor 構文

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Luke Latham](https://github.com/guardrex)、および[Taylor Mullen](https://twitter.com/ntaylormullen)

Razor とは、サーバー ベースのコードを埋め込む web ページのマークアップ構文です。 Razor 構文は、Razor マークアップ、c#、および HTML で構成されます。 通常、Razor を含むファイルが、 *.cshtml*ファイル拡張子。

## <a name="rendering-html"></a>HTML を表示

Razor の既定の言語は、HTML です。 Razor マークアップからレンダリング HTML は、HTML ファイルから HTML をレンダリングと違いはありません。 HTML マークアップを配置する場合、 *.cshtml*レンダリング変更されていないサーバーで Razor ファイル。

## <a name="razor-syntax"></a>Razor 構文

Razor (C#) をサポートしを使用して、 `@` HTML を c# から移行する記号。 Razor では、c# 式を評価し、それらを HTML 出力でレンダリングします。

ときに、`@`シンボルが続く、 [Razor 予約キーワード](#razor-reserved-keywords)、Razor 固有のマークアップに移行します。 それ以外の場合、一般的な C# の場合に移行します。

エスケープする、`@`シンボル Razor マークアップを 1 秒間を使用して`@`シンボル。

```cshtml
<p>@@Username</p>
```

コードが、1 つの HTML でレンダリング`@`シンボル。

```html
<p>@Username</p>
```

HTML 属性と電子メール アドレスを含むコンテンツが処理されない、`@`遷移文字として記号。 Razor 解析では、次の例では電子メール アドレスがそのまま残ります。

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Razor の暗黙的な式

Razor の暗黙的な式が始まる`@`c# コードを続けて。

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

C# の場合を除く`await`キーワードで暗黙的な式はスペースを含めることはできません。 C# ステートメントにクリア終了がある場合は、スペースを intermingle できます。

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Razor の明示的な式

Razor の明示的な式から成る、`@`バランスの取れたかっこ記号。 過去 1 週間の時間を表示するために、次の Razor マークアップを使用します。

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

内のコンテンツ、`@()`かっこが評価され、出力に表示されます。

一般に、前のセクションで説明されている、暗黙的な式は、スペースを含めることはできません。 次のコードでは、1 週間は現在の時間から減算されていません。

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

コードは、次の HTML を表示します。

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Expression の結果の文字列を連結するのに明示的な式を使用することができます。

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

明示的の式がない`<p>Age@joe.Age</p>`、電子メール アドレスとして扱われるおよび`<p>Age@joe.Age</p>`が表示されます。 明示的な式として書き込まれるときに`<p>Age33</p>`が表示されます。

## <a name="expression-encoding"></a>式のエンコード

文字列に評価される c# 式は、HTML エンコードです。 C# 式の評価が`IHtmlContent`経由で直接レンダリング`IHtmlContent.WriteTo`です。 C# 式の評価がない`IHtmlContent`によって文字列に変換`ToString`描画する前にエンコードします。

```cshtml
@("<span>Hello World</span>")
```

コードは、次の HTML を表示します。

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

としてブラウザーで HTML が表示されます。

```
<span>Hello World</span>
```

`HtmlHelper.Raw`出力では、エンコードされたが、HTML マークアップとしてレンダリングされます。

> [!WARNING]
> 使用して`HtmlHelper.Raw`unsanitized ユーザー入力は、セキュリティ上のリスクです。 ユーザー入力は、悪意のある JavaScript または他の攻撃に含まれる可能性があります。 ユーザー入力をサニタイズしすることは困難です。 使用しないでください`HtmlHelper.Raw`ユーザー入力とします。

```cshtml
@Html.Raw("<span>Hello World</span>")
```

コードは、次の HTML を表示します。

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Razor コードのブロック

Razor コード ブロックが始まる`@`で囲まれたと`{}`です。 式とは異なり、コード ブロック内の c# コードは表示されません。 コード ブロックと、ビューの式は、同じスコープの共有の順序で定義されます。

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

コードは、次の HTML を表示します。

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>暗黙の切り替え

既定の言語コード ブロックでは、C# の場合が、HTML に移行することができます。

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>明示的な区切り記号付き遷移

HTML を描画するコード ブロックのサブ セクションを定義するのには、Razor で表示するための文字を囲む**\<テキスト >**タグ。

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

HTML タグで囲まれていない HTML をレンダリングするときは、この方法を使用します。 HTML または Razor のタグのない Razor 実行時エラーが表示されます。

**\<テキスト >**タグもコンテンツを表示するときに空白を制御する便利です。 間のコンテンツのみ、 **\<テキスト >**のタグをレンダリングすると、しの前後に空白、 **\<テキスト >**タグは HTML 出力で表示されます。

### <a name="explicit-line-transition-with-"></a>明示的な行の遷移と @。

コード ブロックの内部 HTML として残りの行全体を表示を使用して、`@:`構文。

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

なし、 `@:` Razor ランタイム エラーが発生するコードでは、します。

## <a name="control-structures"></a>制御構造

制御構造体は、コード ブロックの拡張機能です。 コード ブロック (マークアップ、インライン c# への遷移) ものすべての側面は、次の構造体に適用されます。

### <a name="conditionals-if-else-if-else-and-switch"></a>条件付き@if、一致しない場合、#else、および@switch

`@if`コードの実行時にコントロール:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`および`else if`を必要としない、`@`シンボル。

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

次のようにスイッチ ステートメントを使用することができます。

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>ループ@for、 @foreach、 @while、および@do中

コントロール ステートメントのループで template 宣言された HTML をレンダリングすることができます。 人のユーザーの一覧を表示するには。

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

次のループ ステートメントのいずれかの操作を行うこともできます。

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>複合@using

C# で、`using`オブジェクトが破棄されることを確認するステートメントを使用します。 Razor では、同じメカニズムを使用してを追加のコンテンツを含む HTML ヘルパーを作成します。 インスタンスを含む form タグを表示するために HTML ヘルパーを利用できる、`@using`ステートメント。

```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

スコープ レベルのアクションを実行することもできます。[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。

### <a name="try-catch-finally"></a>@try、catch、finally

例外処理は、C# の場合に似ています。

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor では、クリティカル セクション lock ステートメントを保護する機能があります。

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>コメント

Razor には、c# と HTML のコメントがサポートされています。

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

コードは、次の HTML を表示します。

```html
<!-- HTML comment -->
```

Razor コメントは、web ページが表示される前に、サーバーによって削除されます。 Razor を使用して`@*  *@`コメントを区切るためにします。 次のコードはコメント アウトされて、サーバーは、マークアップを表示しないようにします。

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>ディレクティブ

Razor ディレクティブが次の予約済みキーワードからの暗黙的な式で表される、`@`シンボル。 ディレクティブは、通常ビューは解析または別の機能を有効にする方法を変更します。

Razor がビューのコードを生成する方法を理解しやすくディレクティブの動作を理解します。

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

コードでは、次のようなクラスが生成されます。

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

この記事のセクションで後述[ビューに対して生成された Razor c# クラスを表示する](#viewing-the-razor-c-class-generated-for-a-view)この生成されたクラスを表示する方法について説明します。

### <a name="using"></a>@using

`@using`ディレクティブを追加、c#`using`ディレクティブを生成されたビューに。

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

`@model`ディレクティブがビューに渡されたモデルの種類を指定します。

```cshtml
@model TypeNameOfModel
```

個々 のユーザー アカウントを持つ ASP.NET Core MVC アプリを作成する場合、 *Views/Account/Login.cshtml*ビューには、次のモデルの宣言が含まれています。

```cshtml
@model LoginViewModel
```

生成されたクラスから継承`RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor の公開、`Model`ビューに渡されるモデルにアクセスするためのプロパティ。

```cshtml
<div>The Login Email: @Model.Email</div>
```

`@model`ディレクティブは、このプロパティの種類を指定します。 ディレクティブを指定します、`T`で`RazorPage<T>`を生成するクラスから派生して、表示します。 指定しない場合は、`@model`ディレクティブ、`Model`プロパティの型は`dynamic`します。 モデルの値は、コント ローラーからビューに渡されます。 参照してください[モデルを厳密に型指定と@modelキーワード](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label)詳細についてはします。

### <a name="inherits"></a>@inherits

`@inherits`ディレクティブには、ビューが継承するクラスのフル コントロールが提供します。

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

カスタム Razor ページの種類を次に示します。

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

`CustomText`ビューに表示されます。

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

コードは、次の HTML を表示します。

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

使用することはできません`@model`と`@inherits`同じビューにします。 した`@inherits`で、 *_ViewImports.cshtml*ビューをインポートするファイル。

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

厳密に型指定されたビューの例を次に示します。

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

場合"rick@contoso.com"渡されるモデルでは、ビューには、次の HTML マークアップが生成されます。

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

`@inject`ディレクティブを使用すると、サービスからの挿入、[サービス コンテナー](xref:fundamentals/dependency-injection)ビュー内にします。 参照してください[ビューに依存性の注入](xref:mvc/views/dependency-injection)詳細についてはします。

### <a name="functions"></a>@functions

`@functions`ディレクティブでは、関数レベルのコンテンツ ビューを追加することができます。

```cshtml
@functions { // C# Code }
```

例:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

コードでは、次の HTML マークアップが生成されます。

```html
<div>From method: Hello</div>
```

次のコードでは、生成された Razor c# クラスを示します。

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

`@section`ディレクティブを組み合わせて使用、[レイアウト](xref:mvc/views/layout)HTML ページのさまざまな部分にコンテンツをレンダリングするビューを有効にします。 参照してください[セクション](xref:mvc/views/layout#layout-sections-label)詳細についてはします。

## <a name="tag-helpers"></a>タグ ヘルパー

関連する次の 3 つのディレクティブがある[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。

| ディレクティブ | 関数 |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | タグ ヘルパーを可能にすると表示されます。 |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | 以前に追加したビューからタグ ヘルパーを削除します。 |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | タグ ヘルパーのサポートを有効にして、タグ ヘルパーの使用法を明確にするのには、タグ プレフィックスを指定します。 |

## <a name="razor-reserved-keywords"></a>Razor の予約済みキーワード

### <a name="razor-keywords"></a>Razor キーワード

* (ASP.NET Core 2.0 以降が必要) ページ
* 関数
* 継承
* モデル
* section
* ヘルパー (ASP.NET Core ではサポートされていない)

Razor のキーワードでエスケープ`@(Razor Keyword)`(たとえば、 `@(functions)`)。

### <a name="c-razor-keywords"></a>C# Razor キーワード

* case
* do
* default
* for
* foreach
* if
* else
* lock
* switch
* try
* catch
* finally
* 使用
* while

C# Razor キーワードでダブル エスケープする必要があります`@(@C# Razor Keyword)`(たとえば、 `@(@case)`)。 最初の`@`Razor パーサーをエスケープします。 2 番目`@`c# パーサーをエスケープします。

### <a name="reserved-keywords-not-used-by-razor"></a>Razor で使用されていない予約済みキーワード

* namespace
* class

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>ビューに対して生成された Razor c# クラスを表示します。

ASP.NET Core MVC プロジェクトに次のクラスを追加します。

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

上書き、`RazorTemplateEngine`での MVC によって追加された、`CustomTemplateEngine`クラス。

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

ブレークポイントの設定、`return csharpDocument`ステートメントの`CustomTemplateEngine`します。 プログラムの実行がブレークポイントで停止したときの値を表示`generatedCode`です。

![GeneratedCode のテキスト ビジュアライザーのビュー](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>ビューの参照や大文字と小文字の区別

Razor ビュー エンジンは、ビューを区別する検索を実行します。 ただし、実際の参照は、基になるファイル システムによって決定されます。

* ファイル ベースのソース: 
  * 大文字と小文字のファイル システム (Windows など) でのオペレーティング システムで物理ファイルのプロバイダーの参照は大文字小文字を区別します。 たとえば、`return View("Test")`に一致する結果*/Views/Home/Test.cshtml*、 */Views/home/test.cshtml*、およびその他の文字種バリアント。
  * 大文字と小文字のファイル システムで (たとえば、Linux、os X を使用して`EmbeddedFileProvider`)、検索は大文字小文字を区別します。 たとえば、`return View("Test")`具体的には一致*/Views/Home/Test.cshtml*です。
* ビューをプリコンパイル: ASP.Net Core 2.0 以降ではすべてのオペレーティング システムで大文字と小文字はプリコンパイル済みのビューを検索します。 動作は、Windows 上の物理ファイルのプロバイダーの動作と同じです。 プリコンパイル済みの 2 つのビューが大文字と小文字が異なる場合、参照の結果は非決定的です。

開発者は、領域、コント ローラー、およびアクションの名前の大文字と小文字をファイルおよびディレクトリ名の大文字と小文字が一致することをお勧めします。 これにより、デプロイは、基になるファイル システムに関係なく、ビューを検索します。
