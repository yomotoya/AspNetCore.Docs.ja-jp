---
title: "ASP.NET Core MVC の概要"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 67394b066c18a149a97b957d6478ba8301ea8147
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC の概要

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC は、web アプリを構築するための豊富なフレームワークと、モデル ビュー コント ローラーを使用して Api デザイン パターンです。

## <a name="what-is-the-mvc-pattern"></a>MVC パターンとは何ですか。

モデル ビュー コント ローラー (MVC) アーキテクチャ パターンでは、アプリケーション コンポーネントの次の 3 つの主要グループに: モデル、ビュー、およびコント ローラー。 このパターンを実現するために役立つ[関心の分離](http://deviq.com/separation-of-concerns/)です。 このパターンを使用して、ユーザーの要求は、担当するユーザーの操作の実行やクエリの結果を取得するモデルはコント ローラーにルーティングされます。 コント ローラーは、ユーザーに表示するビューを選択し、必要なすべてのモデル データを提供します。

次の図は、次の 3 つの主要なコンポーネントと、どれが、他のユーザーを参照します。

![MVC パターン](overview/_static/mvc.png)

この概念の責任のでは、コード、デバッグ、およびテスト (モデル、ビュー、またはコント ローラー) が持っているもの 1 つのジョブを簡単になっているために、複雑さの観点からアプリケーションを拡張できます (およびに従って、[単一責任の原則。](http://deviq.com/single-responsibility-principle/)). 更新プログラム、テスト、および分散これら 3 つの領域の 2 つ以上の依存関係のあるコードをデバッグすることが難しくなります。 たとえば、ユーザー インターフェイス ロジックは、ビジネス ロジックよりも頻繁に変更する傾向があります。 プレゼンテーション コードとビジネス ロジックを 1 つのオブジェクトに結合し場合、は、ユーザー インターフェイスを変更するたびに、ビジネス ロジックを格納するオブジェクトの変更が必要です。 これは、エラーが発生して、すべてのビジネス ロジックの再テストごとの最小ユーザー インターフェイスを変更した後を必要とする可能性があります。

> [!NOTE]
> ビューとコント ローラーの両方は、モデルによって異なります。 ただし、モデルは、ビューでも、コント ローラーに依存します。 これは、分離の主要な利点の 1 つです。 この分離により、モデルをビルドおよびテストできる、ビジュアル化に依存しません。

### <a name="model-responsibilities"></a>モデルの責任

MVC アプリケーション内のモデルでは、アプリケーションやビジネス ロジックによって実行される操作の状態を表します。 モデルでは、アプリケーションの状態を保存するための実装ロジックとビジネス ロジックをカプセル化する必要があります。 厳密に型指定されたビューはされる通常具体的には、データを含むように設計 ViewModel 型を使用してそのビューに表示するにはコント ローラーが作成され、モデルからこれらの ViewModel インスタンスを作成します。

> [!NOTE]
> MVC アーキテクチャ パターンを使用するアプリでモデルを整理する方法があります。 詳細については、いくつか[さまざまな種類の種類のモデルの](http://deviq.com/kinds-of-models/)します。

### <a name="view-responsibilities"></a>ビューの責任

ビューは、ユーザー インターフェイスからコンテンツの表示を担当します。 使用する、 [Razor ビュー エンジン](#razor-view-engine)HTML マークアップに .NET コードを埋め込む。 ビューの中で最低限のロジックが存在する必要があり、それらで任意のロジックは、コンテンツを表示することに関連する必要があります。 複雑なモデルからデータを表示するためにファイルのビューに大量のロジックを実行する必要を検索する場合は、使用を検討して、[ビュー コンポーネント](views/view-components.md)ViewModel、または表示を簡略化のテンプレートの表示。

### <a name="controller-responsibilities"></a>コント ローラーの機能

コント ローラーは、ユーザーとの対話を処理し、モデルでは、作業、最終的にレンダリングするビューを選択するコンポーネントです。 MVC アプリケーションでは、ビューのみ情報が表示されます。コント ローラーは、処理し、ユーザー入力との対話に応答します。 MVC パターンでは、コント ローラー、初期のエントリ ポイントは、どのモデルの種類を使用して表示するには、どのビューの選択を担当する (そのため、名前の制御、アプリが特定の要求に応答する方法)。

> [!NOTE]
> 多数の責任でコント ローラーが過度に複雑ないない必要があります。 コント ローラーのロジックが過度に複雑になることを防止するには、[単一責任の原則](http://deviq.com/single-responsibility-principle/)プッシュ ビジネス ロジック、コント ローラー アウトし、ドメイン モデルにします。

>[!TIP]
> コント ローラーのアクションは、同じ種類のアクションを頻繁に実行する場合は、行うことができる、[繰り返さない自分で原則](http://deviq.com/don-t-repeat-yourself/)にこれらの一般的なアクションを移動することによって[フィルター](#filters)です。

## <a name="what-is-aspnet-core-mvc"></a>ASP.NET Core MVC は、新機能

ASP.NET Core MVC フレームワークは、ライトウェイト、オープン ソース、ASP.NET Core で使用するために最適化された高テスト可能なプレゼンテーション フレームワークです。

ASP.NET Core MVC は、問題を明確に分離できるようにする動的な web サイトを構築するパターンに基づく方法を提供します。 マークアップを完全に制御を規格をサポートしては最新の web 標準を使用します。

## <a name="features"></a>フィーチャー

ASP.NET Core MVC、次のとおりです。

* [ルーティング](#routing)
* [モデル バインディング](#model-binding)
* [モデル検証](#model-validation)
* [依存関係の挿入](../fundamentals/dependency-injection.md)
* [フィルター](#filters)
* [領域](#areas)
* [Web Api](#web-apis)
* [テストの容易性](#testability)
* [Razor ビュー エンジン](#razor-view-engine)
* [厳密に型指定されたビュー](#strongly-typed-views)
* [タグ ヘルパー](#tag-helpers)
* [コンポーネントの表示](#view-components)

### <a name="routing"></a>ルーティング

ASP.NET Core MVC がの上に構築された[ASP.NET Core のルーティング](../fundamentals/routing.md)、できる強力な URL マッピング コンポーネントと、わかりやすく検索可能な Url を持つアプリケーションをビルドします。 これにより、web サーバー上のファイルの整理方法に関係なく、リンクの生成と検索エンジン最適化 (SEO) にとっても、アプリケーションの URL の名前付けパターンを定義することができます。 ルート値の制約、既定値および省略可能な値をサポートする便利なルート テンプレートの構文を使用して、ルートを定義することができます。

*ルーティング規約に基づく*形式の URL をグローバルに定義することにより、アプリケーションを受け入れるし、特定のコント ローラー上の特定のアクション メソッドにマップそれらのすべてがどのように書式設定します。 着信要求を受信すると、ルーティング エンジンは、URL を解析および定義済みの URL の形式の 1 つに一致する、関連付けられているコント ローラーのアクション メソッドを呼び出します。

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*属性のルーティング*コント ローラーとアクションは、アプリケーションのルートを定義する属性で修飾することによって、ルーティング情報を指定することができます。 これは、コント ローラーと関連付けられているアクションの横にある、route 定義を配置していることを意味します。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [1, 4]}} -->

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>モデル バインディング

ASP.NET Core MVC[モデル バインディング](models/model-binding.md)クライアント要求データ (フォームの値、ルート データ、クエリ文字列パラメーター、HTTP ヘッダー)、コント ローラーを処理できるオブジェクトを変換します。 その結果、コント ローラーのロジックを受信した要求データを見つけ出しの作業を行うにはありません。単に、そのアクション メソッドにパラメーターとデータを持ちます。

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>モデルの検証

ASP.NET Core MVC をサポートしている[検証](models/validation.md)データ注釈検証属性を持つモデル オブジェクトを修飾します。 検証属性の値は、サーバーに送信される前に、クライアント側で確認されますだけでなくコント ローラー アクションの前に、サーバー上と呼びます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 8, 9]}} -->

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

コント ローラーのアクション:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [3]}} -->

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

フレームワークでは、クライアントとサーバーの両方の要求データの検証を処理します。 モデルの種類に対して指定された検証ロジックは控えめな注釈としてレンダリングされるビューに追加され、と共にブラウザーに適用される[jQuery 検証](https://jqueryvalidation.org/)です。

### <a name="dependency-injection"></a>依存関係の挿入

ASP.NET Core はの組み込みサポート[依存性の注入 (DI)](../fundamentals/dependency-injection.md)です。 MVC では ASP.NET Core、[コント ローラー](controllers/dependency-injection.md)に従うように、コンス トラクターからサービスを必要な要求、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。

アプリケーションで使用できるも[ファイル ビュー内の依存性の注入](views/dependency-injection.md)を使用して、`@inject`ディレクティブ。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@inject SomeService ServiceName
<!DOCTYPE html>
<html>
<head>
  <title>@ServiceName.GetTitle</title>
</head>
<body>
  <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>フィルター

[フィルター](controllers/filters.md)例外処理や承認などの横断的関心事をカプセル化する開発者を支援します。 フィルターは、アクション メソッドの実行中のカスタムの前処理および後処理ロジックを有効にして、特定の要求の実行パイプライン内の特定の時点で実行するように構成できます。 フィルターは属性としてアクションまたはコント ローラーに適用できます (または、グローバルに実行することができます)。 いくつかのフィルター (など`Authorize`) フレームワークに含まれています。


```csharp
[Authorize]
   public class AccountController : Controller
   {

```

### <a name="areas"></a>区分

[領域](controllers/areas.md)大規模な ASP.NET Core MVC Web アプリケーションを小さい機能グループに分割する方法を提供します。 領域は、事実上、アプリケーション内部 MVC 構造です。 MVC プロジェクトでは、モデル、コント ローラー、およびビューなどの論理コンポーネントが、他のフォルダーに保持され、MVC では、名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。 大規模なアプリの機能の個別の高レベル領域に、アプリを分割すると便利な場合があります。 たとえば、電子商取引を使用したアプリなど、チェック アウト、請求、および検索などの複数の事業単位です。各事業ユニットは、独自の論理コンポーネント ビュー、コント ローラー、およびモデルがあります。

### <a name="web-apis"></a>Web Api

Web サイトを構築するための優れたプラットフォームだけでなくは、ASP.NET Core MVC は、Web Api を構築するための優れたサポートがします。 さまざまなブラウザーやモバイル デバイスを含む、クライアントに到達可能なサービスをビルドすることができます。

フレームワークの組み込みサポートを備えた HTTP コンテンツ ネゴシエーションをサポートしています[データの書式設定](models/formatting.md)JSON または XML として。 書き込む[カスタム フォーマッタ](advanced/custom-formatters.md)ユーザー独自の形式のサポートを追加します。

Hypermedia のサポートを有効にするのにには、リンクの生成を使用します。 サポートを容易にできる[クロス オリジン リソース共有 (CORS)](http://www.w3.org/TR/cors/)に Web Api は、複数の Web アプリケーションで共有できるようにします。

### <a name="testability"></a>テストの容易性

インターフェイスと依存関係の挿入のフレームワークの使用に適してを単体テスト、行い、フレームワークには、構成する (Entity Framework の考えます、InMemory プロバイダー) のような機能が含まれています[統合テスト](../testing/integration-testing.md)クイック。簡単にもします。 詳細については[テスト コント ローラー ロジック](controllers/testing.md)です。

### <a name="razor-view-engine"></a>Razor ビュー エンジン

[ASP.NET Core MVC ビュー](views/overview.md)を使用して、 [Razor ビュー エンジン](views/razor.md)ビューをレンダリングします。 Razor は、埋め込みの c# コードを使用して、ビューを定義するためのコンパクトな表現力が豊かおよび滑らかなテンプレート マークアップ言語です。 Razor を使用して、サーバー上の web コンテンツを動的に生成します。 サーバー コード クライアント側のコンテンツおよびコードを混在しているクリーンにことができます。

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

定義することができます、Razor ビュー エンジンを使用して[レイアウト](views/layout.md)、[部分ビュー](views/partial.md)と置き換え可能なセクションです。

### <a name="strongly-typed-views"></a>厳密に型指定されたビュー

MVC の razor ビューできる厳密に型指定、モデルに基づく。 コント ローラーは、型チェックと IntelliSense をサポートして、ビューを有効にするビューを厳密に型指定されたモデルを渡すことができます。

たとえば、次のビューが型のモデルを定義`IEnumerable<Product>`:

```html
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>タグ ヘルパー

[タグ ヘルパー](views/tag-helpers/intro.md)を作成して、Razor ファイルでの HTML 要素のレンダリングに参加させるサーバー側コードを有効にします。 カスタム タグを定義するタグ ヘルパーを使用することができます (たとえば、 `<environment>`) または既存のタグの動作を変更する (たとえば、 `<label>`)。 タグ ヘルパーは、要素名とその属性に基づいて特定の要素にバインドします。 HTML 編集エクスペリエンスを維持したまま、サーバー側のレンダリングの利点を提供します。

これは、フォーム、リンク、読み込みの資産および詳細 - およびさらに高いで利用できるパブリックの GitHub リポジトリとして NuGet パッケージの作成などの一般的なタスクの多くの組み込みタグ ヘルパー。 タグ ヘルパーは、C# の場合は、作成し、要素名、属性名、または親タグに基づく HTML 要素を対象にします。 たとえば、あらかじめ登録された LinkTagHelper をへのリンクの作成に使用することができます、`Login`のアクション、 `AccountsController`:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper`開発、ステージング、運用環境など、ランタイム環境に基づきます (たとえば、生または縮小された) ビュー内の別のスクリプトを含めるために使用できます。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 3, 4, 9]}} -->

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

タグ ヘルパーは、HTML に適した開発エクスペリエンスと HTML および Razor マークアップを作成するための豊富な IntelliSense 環境を提供します。 組み込みタグ ヘルパーのほとんどは、既存の HTML 要素をターゲットし、要素のサーバー側の属性を指定します。

### <a name="view-components"></a>コンポーネントの表示

[コンポーネントの表示](views/view-components.md)レンダリング ロジックをパッケージ化し、アプリケーション全体で再利用することを許可します。 似て[部分ビュー](views/partial.md)が関連付けられているロジックを使用します。
