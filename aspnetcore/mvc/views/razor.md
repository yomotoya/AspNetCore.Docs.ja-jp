---
title: "ASP.NET Core の razor 構文のリファレンス"
author: rick-anderson
description: "Razor 構文を詳細します。"
keywords: "ASP.NET Core、Razor"
ms.author: riande
manager: wpickett
ms.date: 07/4/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 7648bc2ac7b9efd1653725cda749d6cd271bae77
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>ASP.NET Core の razor 構文

によって[Taylor Mullen](https://twitter.com/ntaylormullen)と[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>Razor とは何ですか。

Razor とは、サーバー ベースのコードを埋め込む web ページのマークアップ構文です。 Razor 構文は、c#、HTML、Razor マークアップで構成されます。 通常、Razor を含むファイルが、 *.cshtml*ファイル拡張子。

## <a name="rendering-html"></a>HTML を表示

Razor の既定の言語は、HTML です。 Razor から HTML をレンダリングは、HTML ファイルと同じです。 次のマークアップ Razor ファイル:

```html
<p>Hello World</p>
   ```

変更されることが表示される`<p>Hello World</p>`サーバーでします。

## <a name="razor-syntax"></a>Razor 構文

Razor (C#) をサポートしを使用して、 `@` HTML を c# から移行する記号。 Razor では、c# 式を評価し、それらを HTML 出力でレンダリングします。 Razor は、c# や Razor 固有のマークアップに HTML から移行できます。 ときに、`@`シンボルが続く、 [Razor 予約キーワード](#razor-reserved-keywords)Razor 固有のマークアップに遷移、遷移プレーンな C# の場合にそれ以外の場合。

<a name=escape-at-label></a>

HTML を含む`@`シンボルは、1 秒あたりにエスケープする必要があります`@`シンボル。 例:

```html
<p>@@Username</p>
   ```

次の HTML を表示します。

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

HTML 属性と電子メール アドレスを含むコンテンツが処理されない、`@`遷移文字として記号。

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Razor の暗黙的な式

Razor の暗黙的な式が始まる`@`c# コードを続けています。 例:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

ただし、c#`await`キーワードの暗黙的な式はスペースを含めることはできません。 たとえば、(C#) ステートメントがあるオフの末尾にある限りは、スペースを intermingle できます。

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Razor の明示的な式

Razor の明示的な式から成る、@ バランスの取れたかっこ記号。 たとえば、先週の時間を表示するためにします。

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

内のコンテンツ、@ () かっこが評価され、出力に表示されます。

暗黙的な式は一般に、スペースを含めることはできません。 たとえば、次のコードに 1 週間はから差し引かれません、現在の時刻。

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

これは、次の HTML を表示します。

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

Expression の結果の文字列を連結するのに明示的な式を使用することができます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

明示的の式がない`<p>Age@joe.Age</p>`電子メール アドレスとして扱われてと`<p>Age@joe.Age</p>`レンダリングされます。 明示的な式として書き込まれるときに`<p>Age33</p>`が表示されます。

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>式のエンコード

文字列に評価される c# 式は、HTML エンコードです。 C# 式の評価が`IHtmlContent`経由で直接レンダリング*IHtmlContent.WriteTo*です。 C# 式の評価がない*IHtmlContent*を文字列に変換 (によって*ToString*) し、レンダリングされる前にエンコードします。 たとえば、次の Razor マークアップ。

```html
@("<span>Hello World</span>")
   ```

これに HTML を表示します。

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

ブラウザーは、としてレンダリングします。

`<span>Hello World</span>`

`HtmlHelper``Raw`出力ではなくでエンコードされた、HTML マークアップとしてレンダリングされます。

>[!WARNING]
> 使用して`HtmlHelper.Raw`unsanitized ユーザー入力は、セキュリティ上のリスクです。 ユーザー入力は、悪意のある JavaScript または他の攻撃に含まれる可能性があります。 ユーザー入力をサニタイズしが困難なを使用しないでください`HtmlHelper.Raw`ユーザーの入力します。

次の Razor マークアップ。

```html
@Html.Raw("<span>Hello World</span>")
   ```

これに HTML を表示します。

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Razor コードのブロック

Razor コード ブロックが始まる`@`で囲まれたと`{}`です。 式とは異なりコード ブロック内の c# コードはレンダリングされません。 同じスコープを共有し、順序でも定義コード ブロックおよび Razor ページ内の式 (、コード ブロックで宣言されると後のコード ブロックと式のスコープ内)。

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

描画されます。

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>暗黙の切り替え

既定の言語コード ブロックでは、C# の場合が、HTML に移行することができます。 コード ブロック内の HTML は HTML 表示に戻す移行します。

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>明示的な区切り記号付き遷移

HTML を描画するコード ブロックのサブ セクションを定義するのには、Razor で表示する文字を囲みます`<text>`タグ。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

一般に、この方法は、HTML タグで囲まれていない HTML をレンダリングするときに使用します。 HTML または Razor のタグのない Razor 実行時エラーが表示されます。

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>明示的な行の移行`@:`

コード ブロックの内部 HTML として残りの行全体を表示を使用して、`@:`構文。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

なし、`@:`上記のコードでは、実行時エラー Razor を取得します。

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>制御構造

制御構造体は、コード ブロックの拡張機能です。 コード ブロック (マークアップ、インライン c# への遷移) ものすべての側面は、次の構造体に適用されます。

### <a name="conditionals-if-else-if-else-and-switch"></a>条件付き`@if`、 `else if`、`else`と`@switch`

`@if`ファミリ制御コードが実行されます。

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`および`else if`を必要としない、`@`シンボル。

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

次のようにスイッチ ステートメントを使用することができます。

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>ループ`@for`、 `@foreach`、 `@while`、および`@do while`

コントロール ステートメントのループで template 宣言された HTML をレンダリングすることができます。 たとえば、ユーザーの一覧を表示するためにします。

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

次のループ ステートメントのいずれかの操作を行うこともできます。

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
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

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>複合`@using`

C# を使用して、オブジェクトが破棄されることを確認するステートメントを使用します。 Razor で追加のコンテンツを含む HTML ヘルパーの作成をこれと同じメカニズムを使用できます。 インスタンスとフォーム タグをレンダリングする HTML ヘルパーを利用できます、`@using`ステートメント。

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

上記のようなスコープ レベルのアクションを実行することもできます。[タグ ヘルパー](tag-helpers/index.md)です。

### <a name="try-catch-finally"></a>`@try`、`catch`、`finally`

例外処理は、C# の場合に似ています。

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor では、クリティカル セクション lock ステートメントを保護する機能があります。

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>コメント

Razor では、c# と HTML のコメントをサポートします。 次のマークアップ。

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

サーバーによってレンダリングされます。

```none
<!-- HTML comment -->
```

Razor コメントは、ページが表示される前に、サーバーによって削除されます。 Razor を使用して`@*  *@`コメントを区切るためにします。 次のコードがコメント アウト、ため、サーバーにすべてのマークアップは表示されません。

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a>ディレクティブ

Razor ディレクティブが次の予約済みキーワードからの暗黙的な式で表される、`@`シンボル。 ディレクティブは通常、ページの解析方法を変更するか、Razor ページ内の別の機能を有効にします。

Razor がビューのコードを生成する方法を理解することが容易にできるようにディレクティブの動作を理解します。 Razor ページを使用して、c# ファイルを生成します。 たとえば、Razor ページ:

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

次のようなクラスが生成されます。

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

[ビューに対して生成された Razor c# クラスを表示する](#razor-customcompilationservice-label)この生成されたクラスを表示する方法について説明します。

### `@using`

`@using`ディレクティブを追加すると、c#`using`ディレクティブを生成された razor ページ。

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

`@model`ディレクティブは、Razor ページに渡されたモデルの種類を指定します。 このツールでは、次の構文が使用されます。

```none
@model TypeNameOfModel
   ```

たとえば、個々 のユーザー アカウントを持つ ASP.NET Core MVC アプリを作成する場合、 *Views/Account/Login.cshtml* Razor ビューには、次のモデルの宣言が含まれています。

```csharp
@model LoginViewModel
   ```

クラスの例で生成されるクラスを継承から`RazorPage<dynamic>`です。 追加することによって、`@model`新機能は、継承を制御します。 次に例を示します。

```csharp
@model LoginViewModel
   ```

次のクラスが生成されます。

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

Razor ページを公開、`Model`モデルにアクセスするためのプロパティ ページに渡されます。

```html
<div>The Login Email: @Model.Email</div>
   ```

`@model`ディレクティブは、このプロパティの型を指定 (指定することによって、`T`で`RazorPage<T>`からページに対して生成されたクラスが派生する)。 指定しない場合は、`@model`ディレクティブ、`Model`型プロパティである`dynamic`です。 モデルの値は、コント ローラーからビューに渡されます。 参照してください[モデルを厳密に型指定と@modelキーワード](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label)詳細についてはします。

### `@inherits`

`@inherits`ディレクティブは、Razor ページを継承するクラスを完全に制御を提供します。

```none
@inherits TypeNameOfClassToInheritFrom
   ```

インスタンスが発生しました。 次のカスタム Razor ページの種類とします。

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

次の Razor 生成`<div>Custom text: Hello World</div>`です。

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

使用することはできません`@model`と`@inherits`同じページにします。 した`@inherits`で、 *_ViewImports.cshtml* Razor ページをインポートするファイル。 たとえば、Razor ビューは、次をインポート*_ViewImports.cshtml*ファイル。

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

次の厳密に型指定された Razor ページ

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

この HTML マークアップを生成します。

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

渡されたときに"[Rick@contoso.com](mailto:Rick@contoso.com)"モデル。

   参照してください[レイアウト](layout.md)詳細についてはします。

### `@inject`

`@inject`ディレクティブを使用すると、サービスからの挿入、[サービス コンテナー](../../fundamentals/dependency-injection.md) Razor ページの使用にします。 参照してください[ビューに依存性の注入](dependency-injection.md)です。

<a name="functions"></a>

### `@functions`

`@functions`ディレクティブでは、Razor ページに関数レベルのコンテンツを追加することができます。 構文は次のとおりです。

```none
@functions { // C# Code }
   ```

例:

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

次の HTML マークアップを生成します。

```none
<div>From method: Hello</div>
   ```

生成された Razor c# のようになります。

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

`@section`ディレクティブを組み合わせて使用、[レイアウト ページ](layout.md)を表示する HTML ページのさまざまな部分にコンテンツをレンダリングするビューを有効にします。 参照してください[セクション](layout.md#layout-sections-label)詳細についてはします。

## <a name="tag-helpers"></a>タグ ヘルパー

次[タグ ヘルパー](tag-helpers/index.md)ディレクティブが記載されているリンクで詳しく説明します。

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Razor の予約済みキーワード

### <a name="razor-keywords"></a>Razor キーワード

* (ASP.NET Core 2.0 以降が必要) ページ
* 関数
* 継承
* モデル
* section
* ヘルパー (サポートしていません ASP.NET Core。)

Razor キーワードでエスケープできます`@(Razor Keyword)`、たとえば`@(functions)`します。 以下の完全なサンプルを参照してください。

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

C# Razor キーワードする必要がある二重でエスケープ`@(@C# Razor Keyword)`、たとえば`@(@case)`します。 最初の`@`Razor パーサーをエスケープする 2 番目`@`c# パーサーをエスケープします。 以下の完全なサンプルを参照してください。

### <a name="reserved-keywords-not-used-by-razor"></a>Razor で使用されていない予約済みキーワード

* namespace
* class

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>ビューに対して生成された Razor c# クラスを表示します。

ASP.NET Core MVC プロジェクトに次のクラスを追加します。

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

上書き、 `ICompilationService` MVC によって上記のクラスに追加

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

ブレークポイントを設定、`Compile`メソッドの`CustomCompilationService`とビュー`compilationContent`です。

![CompilationContent のテキスト ビジュアライザーのビュー](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>ビューの参照や大文字と小文字の区別

Razor ビュー エンジンは、ビューを区別する検索を実行します。 ただし、実際の参照は、基になるソースによって決定されます。

* ファイル ベースのソース: 

    * (Windows) などの大文字と小文字のファイル システムでのオペレーティング システムで物理ファイルのプロバイダーの参照は大文字小文字を区別します。 たとえば`return View("Test")`になる`/Views/Home/Test.cshtml`、`/Views/home/test.cshtml`し、その他のすべての大文字と小文字のバリアントは発見します。
    * Os X、Linux を含むシステムでは大文字小文字を区別ファイル、および`EmbeddedFileProvider`検索は大文字小文字を区別します。 たとえば、`return View("Test")`を具体的には検索`/Views/Home/Test.cshtml`です。
        
* プリコンパイル済みのビュー:

   * ASP.Net Core 2.0 以降はすべてのオペレーティング システムで大文字と小文字がプリコンパイル済みのビューを検索します。 動作は、Windows 上の物理ファイルのプロバイダーの動作と同じです。 
   注: プリコンパイル済みの 2 つのビューが大文字と小文字が異なる場合、参照の結果は非決定的です。

開発者は、領域、コント ローラーとアクション名の大文字小文字の区別するファイルとディレクトリ名の大文字と小文字が一致することをお勧めします。 これにより、デプロイの基になるファイル システムの柔軟なままを確認します。
