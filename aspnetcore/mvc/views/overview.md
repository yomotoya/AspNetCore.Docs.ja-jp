---
title: "ASP.NET Core MVC ビュー"
author: ardalis
description: "ビューが、アプリのデータの表示と ASP.NET Core MVC でのユーザーとの対話を処理する方法について説明します。"
keywords: "ASP.NET Core、表示、MVC、razor、viewmodel、viewdata、viewbag"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: d3fbdecaed87b3432f0532748a0833c833c65129
ms.sourcegitcommit: a60a99104fe7a29e271667cead6a06b6d8258d03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/03/2017
---
# <a name="views-in-aspnet-core-mvc"></a>ASP.NET Core MVC ビュー

によって[Steve Smith](https://ardalis.com/)と[Luke Latham](https://github.com/guardrex)

**M**odel -**V**ビュー -**C**ontroller (MVC) パターン、*ビュー*アプリのデータのプレゼンテーションとユーザーの操作を処理します。 ビューは、HTML テンプレートが埋め込まれて[Razor マークアップ](xref:mvc/views/razor)です。 Razor のマークアップは、クライアントに送信される web ページを生成するために HTML マークアップが対話するコードです。

ASP.NET Core MVC ビューは*.cshtml*ファイルを使用する、 [c# プログラミング言語](/dotnet/csharp/)Razor マークアップでします。 通常、ファイルの表示はフォルダーの各アプリの名前にグループ化[コント ローラー](xref:mvc/controllers/actions)です。 フォルダーが格納されているので、*ビュー*アプリのルートにあるフォルダー。

![Visual Studio のソリューション エクスプ ローラーで、views フォルダーは About.cshtml、Contact.cshtml、および Index.cshtml ファイルを表示するのには開放の [ホーム] フォルダーで開く](overview/_static/views_solution_explorer.png)

*ホーム*コント ローラーがによって表される、*ホーム*内のフォルダー、*ビュー*フォルダーです。 *ホーム*フォルダーには用のビューが含まれています、*に関する*、*連絡先*、および*インデックス*(ホーム ページ) の web ページ。 ユーザーがこれらの 3 つ web サイト、コント ローラー アクションのいずれかを要求するときに、*ホーム*コント ローラーは、3 つのビューのどちらを使用してビルドし、ユーザーに web ページを返すを決定します。

使用して[レイアウト](xref:mvc/views/layout)を一貫性のある web ページのセクションを提供し、コードの繰り返しを削減します。 多くの場合、レイアウトには、ヘッダー、ナビゲーションとメニューの要素、およびフッターが含まれています。 ヘッダーとフッター多くのメタデータ要素とスクリプトとスタイルの資産へのリンクの定型的なマークアップを通常含まれます。 レイアウトでは、ビューでは、この定型的なマークアップを回避できます。

[部分ビュー](xref:mvc/views/partial)ビューの再利用可能な部分を管理することにより、コードの重複を削減します。 たとえば、部分ビューは、いくつかのビューに表示されているブログ web サイトで、作成者略歴に役立ちます。 作成者略歴は、通常のビューのコンテンツを必要し、しない web ページのコンテンツを生成するために実行するコードです。 作成者略歴コンテンツは単独で、モデル バインディングによってビューに表示する部分ビューを使用して、この種類のコンテンツは、最適です。

[コンポーネントの表示](xref:mvc/views/view-components)部分のようなビューは、コードの繰り返しを削減することができる点で、web ページを表示するために、サーバーで実行するコードを必要とするコンテンツの表示に適していますがします。 ビューのコンポーネントは、描画された内容は、ショッピング カートの web サイトのように、データベースとの対話を必要とする場合に便利です。 コンポーネントの表示は web ページの出力を生成するためにモデル バインディングに制限されます。

## <a name="benefits-of-using-views"></a>ビューを使用する利点

ビューを確立するために、 [ **S**eparation **o**f **C**oncerns (SoC) デザイン](http://deviq.com/separation-of-concerns/)からユーザー インターフェイスのマークアップを分離することにより、MVC アプリケーション内でアプリの他の部分です。 次の SoC 設計により、アプリ モジュール、いくつかの利点を提供します。

* アプリより適切に編成されていますのでを維持するために簡単です。 ビューは、アプリの機能によって一般にグループ化されます。 これにより、機能を使用する場合は、関連するビューを見つけやすくします。
* アプリの一部は疎結合します。 構築し、ビジネス ロジックとデータ アクセス コンポーネントから個別に、アプリのビューを更新できます。 必ずしも、アプリの他の部分を更新することがなく、アプリのビューを変更できます。
* 個別の単位は、ビューがあるために、ユーザー インターフェイスがアプリの一部をテストする簡単です。
* 効率的な整理、により可能性は低くなりますユーザー インターフェイスの繰り返しセクションでは誤ってを学習します。

## <a name="creating-a-view"></a>ビューを作成します。

コント ローラーに固有のビューを作成するのには*ビュー/[ControllerName]*フォルダーです。 コント ローラー間で共有されるビューについてに、 *Views/shared*フォルダーです。 ビューを作成するには、新しいファイルを追加し、関連付けられたコント ローラー アクションのと同じ名前を付けます、 *.cshtml*ファイル拡張子。 対応するビューを作成する、*に関する*アクションで、*ホーム*コント ローラーで、作成、 *About.cshtml*ファイルで、*ビュー/ホーム*フォルダー。

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor*マークアップが始まり、`@`シンボル。 内のコードを配置する (C#) で実行 (C#) ステートメント[Razor コードのブロック](xref:mvc/views/razor#razor-code-blocks)中かっこで off に設定 (`{ ... }`)。 たとえばに"About"の割り当てを参照してください`ViewData["Title"]`上に示したです。 HTML 内の値を表示するには単に値が参照することによって、`@`シンボル。 内容を参照してください、`<h2>`と`<h3>`要素上です。

前に示したコンテンツの表示は、ユーザーに表示される web ページ全体の一部のみです。 ページのレイアウトの残りの部分とビューの他の一般的な側面は、他のビュー ファイルに指定されます。 詳細については、次を参照してください。、[レイアウト トピック](xref:mvc/views/layout)です。

## <a name="how-controllers-specify-views"></a>コント ローラーがビューを指定する方法

ビューは通常の操作から返される、 [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult)の型である[ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult)です。 アクション メソッドが作成して返すことができます、`ViewResult`を直接一般的に実行されていないことができます。 ほとんどのコント ローラーが継承ため[コント ローラー](/aspnet/core/api/microsoft.aspnetcore.mvc.controller)、単に使用する、`View`を返すヘルパー メソッド、 `ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

この操作から制御が戻るとき、 *About.cshtml*最後のセクションに表示されているビューは、次の web ページとして表示されます。

![Edge ブラウザーでレンダリングされるページについて](overview/_static/about-page.png)

`View`ヘルパー メソッドが複数のオーバー ロードします。 必要に応じて指定できます。

* 返される明示的なビュー。

  ```csharp
  return View("Orders");
  ```
* A[モデル](xref:mvc/models/model-binding)に渡す、ビュー。

  ```csharp
  return View(Orders);
  ```
* ビューとモデルの両方で:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>ビューの検出

アクションは、ビューを返します、ときに、プロセスを呼び出す*ビュー検出*行われます。 このプロセスでは、ビュー名に基づいて、どのファイルの表示が使用されるを決定します。 

既定の動作、`View`メソッド (`return View();`) を呼び出し元のアクション メソッドと同じ名前のビューを返すことです。 たとえば、*に関する*`ActionResult`という名前のビュー ファイルを検索するコント ローラーのメソッド名が使用される*About.cshtml*です。 ランタイムが最初に、検索、*ビュー/[ControllerName]*ビューのフォルダーです。 検索に一致するビューがありますが見つからない場合は、 *Shared*ビューのフォルダーです。

暗黙的に返す場合にかかわらず、`ViewResult`で`return View();`にビューの名前を明示的に渡すことも、`View`メソッドを`return View("<ViewName>");`です。 どちらの場合は、ビューの検出は、この順序で一致するビュー ファイルを検索します。

   1. *ビュー/\[ControllerName]\[ViewName] .cshtml*
   1. *ビュー/共有/\[ViewName] .cshtml*

ビュー名の代わりに、ビューのファイル パスを指定できます。 アプリのルートから始まる絶対パスを使用した場合 (必要に応じて開始され、「/」または"~/") では、 *.cshtml*拡張機能を指定する必要があります。

```csharp
return View("Views/Home/About.cshtml");
```

相対パスを使用することがなく異なるディレクトリにビューを指定する、 *.cshtml*拡張機能です。 内部、 `HomeController`、返すことができます、*インデックス*の表示、*管理*相対パスを持つビュー。

```csharp
return View("../Manage/Index");
```

同様に、現在のコント ローラーに固有のディレクトリでを指定することができます、"./"プレフィックス。

```csharp
return View("./About");
```

[部分ビュー](xref:mvc/views/partial)と[コンポーネントを表示](xref:mvc/views/view-components)似ています (ただしと一致しない) の検出メカニズムを使用します。

どのビューは、アプリ内にあるを使用して、カスタムの既定の規則をカスタマイズする[IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander)です。

ビューの検出は、ファイル名でファイルの表示を検出することに依存しています。 基になるファイル システムが大文字と小文字の場合は、ビューの名前はおそらく大文字小文字を区別です。 オペレーティング システム間で互換性のため、コント ローラーとアクション名および関連するビューのフォルダーとファイル名の間のケースに一致します。 ファイルの表示を区別するファイル システムの操作中に検出できないエラーが発生した場合は、要求されたビュー ファイルと実際のビューのファイル名の大文字と小文字が一致していることを確認します。

コント ローラー、アクション、および保守容易性とわかりやすくするためのビューの間の関係を反映するように、ビューのファイル構造を整理するためのベスト プラクティスに従ってください。

## <a name="passing-data-to-views"></a>ビューにデータを渡す

いくつかのアプローチを使用して、ビューにデータを渡すことができます。 最も堅牢な方法は、指定する、[モデル](xref:mvc/models/model-binding)ビュー内の型。 このモデルと呼ばれる一般的な*viewmodel*です。 Viewmodel 型のインスタンスは、アクションからビューに渡します。

により、利用するためにビューをビューにデータを渡して、viewmodel を使用して*強力な*型チェックします。 *厳密な型指定*(または*厳密に型指定された*) すべての変数および定数では、明示的に定義された型 (たとえば、 `string`、 `int`、または`DateTime`)。 ビューで使用される型の妥当性は、コンパイル時にチェックされます。

[Visual Studio](https://www.visualstudio.com/vs/)と[Visual Studio Code](https://code.visualstudio.com/)と呼ばれる機能を使用して、厳密に型指定されたクラス メンバーを一覧表示[IntelliSense](/visualstudio/ide/using-intellisense)です。 Viewmodel のプロパティを表示する場合は、入力変数名のピリオドが続く viewmodel (`.`)。 これにより、エラーの少ないコードをより早く作成できます。

使用してモデルを指定して、`@model`ディレクティブです。 使用してモデルを使用して`@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

ビューにモデルを提供するには、コント ローラーは、それをパラメーターとして渡します。

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

ビューを提供できるモデルの種類に制限はありません。 使用することをお勧め**P**lain **O**%ld **C**LR **O**オブジェクト (POCO) viewmodels ほとんどまたはまったく動作 (メソッド) を定義します。 通常、viewmodel クラスは、いずれかに格納されて、*モデル*フォルダーまたは個別の*ViewModels*アプリのルートにあるフォルダーです。 *アドレス*viewmodel 上記の例で使用される、という名前のファイルに格納されている POCO viewmodel *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

> [!NOTE]
> Viewmodel 型やビジネス モデルの種類の両方を同じクラスを使用して何もできません。 ただし、別のモデルを使用してでは、ビューとは異なるいない個別にビジネス ロジックとデータ、アプリのアクセスの部分です。 モデルと viewmodels の分離はセキュリティ上の利点をモデルで使用する場合にも[モデル バインディング](xref:mvc/models/model-binding)と[検証](xref:mvc/models/validation)ユーザーがアプリに送信されるデータにします。

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>弱い型指定のデータ (ViewData と ViewBag)

ビューへのアクセスのある、厳密に型指定されたビューだけでなく、*弱い型指定*(とも呼ばれる*弱い型指定*) データのコレクション。 厳密な型とは異なり*弱い型*(または*型が失われる*) 使用するデータの型を明示的に宣言しないことを意味します。 少量のコント ローラーとビューとの間のデータを渡すため、厳密に型指定されたデータのコレクションを使用できます。

| 間でデータを渡すことをしています.                        | 例                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| コント ローラーとビュー                             | ドロップダウン リストをデータを作成します。                                          |
| ビューと[[レイアウト] ビュー](xref:mvc/views/layout)   | 設定、 **\<タイトル >**ビュー ファイルから、レイアウト ビューの要素の内容。  |
| [部分ビュー](xref:mvc/views/partial)とビュー | ユーザーが要求した web ページに基づいてデータを表示するウィジェット。      |

このコレクションは、いずれかで参照できる、`ViewData`または`ViewBag`コント ローラーとビューのプロパティです。 `ViewData`プロパティは、弱い型指定のオブジェクトのディクショナリ。 `ViewBag`プロパティはラッパー `ViewData` 、基になるの動的なプロパティを提供する`ViewData`コレクション。

`ViewData`および`ViewBag`は実行時に動的に解決されます。 コンパイル時の型チェック見て、どちらも通常よりエラーを起こしやすい、viewmodel を使用するよりもです。 そのため、一部の開発者が最小限に抑えるまたはまったく使用しないたい`ViewData`と`ViewBag`です。

**ViewData**

`ViewData`[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)を介してアクセスするオブジェクト`string`キー。 その他のキャストする必要がありますが、文字列データを格納し、キャストの必要性なしで直接使用`ViewData`オブジェクトの値を特定の型に抽出するときにします。 使用することができます`ViewData`およびなどのビュー内でビューをビューには、コント ローラーからデータを渡す[部分ビュー](xref:mvc/views/partial)と[レイアウト](xref:mvc/views/layout)です。

するとあいさつ文とアドレスを使用して、値を設定する例を次に示します`ViewData`アクションで。

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

ビューでデータを操作します。

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

**ViewBag**

`ViewBag`[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)に格納されているオブジェクトへの動的アクセスを提供するオブジェクト`ViewData`です。 `ViewBag`簡単にキャストする必要がないため、操作を指定できます。 次の例は、使用する方法を示しています。`ViewBag`を使用するのと同じ結果に`ViewData`上。

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**ViewBag、ViewData を同時に使用します。**

`ViewData`と`ViewBag`同じ基になるを参照してください`ViewData`コレクション、両方を使用することができます`ViewData`と`ViewBag`を混在させるし、値を読み書きするときにそれらの間に一致します。

設定を使用して、タイトル`ViewBag`を使用して、および`ViewData`の上部にある、 *About.cshtml*ビュー。

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

プロパティを読み取るがの使用を反転`ViewData`と`ViewBag`です。 *_Layout.cshtml*ファイルを使用して、タイトルを入手してください`ViewData`しを使用して、取得する`ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

文字列でのキャストを必要としないことに注意してください`ViewData`です。 使用することができます`@ViewData["Title"]`キャストなし。

両方を使用して`ViewData`と`ViewBag`に混在させると、読み取りと書き込みのプロパティに一致すると、同じ時間動作します。 次のマークアップがレンダリングされます。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**ViewBag、ViewData の違いの概要**

* `ViewData`
  * 派生した[ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)など、役に立ちます辞書プロパティがあるため、 `ContainsKey`、 `Add`、 `Remove`、および`Clear`です。
  * ディクショナリ内のキーは、空白が許可されているために、文字列、です。 例 : `ViewData["Some Key With Whitespace"]`
  * 任意の型以外の場合、`string`を使用するビューでキャストする必要があります`ViewData`です。
* `ViewBag`
  * 派生した[DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata)が、ドット表記を使用して動的なプロパティの作成を許可するため、(`@ViewBag.SomeKey = <value or object>`)、キャストは必要ありません。 構文`ViewBag`コント ローラーとビューを追加する方が手軽になります。
  * Null 値をチェックする方が簡単です。 例 : `@ViewBag.Person?.Name`

**ViewBag、ViewData、またはを使用する場合**

両方`ViewData`と`ViewBag`少量のコント ローラーとビューの間でデータを渡すための有効なアプローチでは同じようにします。 うち (またはその両方) を使用する 1 つはまたは個人用の設定は、組織のユーザー設定を選択します。 混在させるし、一致することができますも`ViewData`と`ViewBag`オブジェクトの場合、コードは読み取りおよび 1 つだけ選択し、一貫して使用する場合、維持を容易にします。 両方がので実行時に動的に解決されるとランタイム エラーの原因が発生しやすく、慎重に使用します。 開発者によって完全回避します。

### <a name="dynamic-views"></a>動的ビュー

使用して、モデルは宣言しないでビュー型`@model`に渡されたモデルのインスタンスがあるが (たとえば、 `return View(Address);`) 動的にインスタンスのプロパティを参照できます。

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

この機能は、柔軟性を提供していますが、コンパイルの保護や IntelliSense は提供していません。 プロパティが存在しない場合は、実行時に web ページの生成が失敗します。

## <a name="more-view-features"></a>詳細ビュー

[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)容易に既存の HTML タグにサーバー側の動作を追加します。 タグ ヘルパーを使用するには、カスタム コードや、ビューの中でヘルパーを記述する必要が回避できます。 タグ ヘルパーは、HTML 要素に属性として適用され、処理できないエディターでは無視されます。 これにより、さまざまなツールのビューのマークアップを表示して編集できます。

カスタムの HTML マークアップを生成するは、多くの組み込み HTML ヘルパーを実現できます。 複雑なユーザー インターフェイス ロジックで処理できる[ビュー コンポーネント](xref:mvc/views/view-components)です。 ビューのコンポーネントは、同じ SoC そのコント ローラーを提供し、ビューを提供します。 アクションおよび一般的なユーザー インターフェイス要素で使用されるデータを処理するビューの必要をなくしことができます。

ASP.NET Core の他の多くの側面と同様にサポートをビュー[依存性の注入](xref:fundamentals/dependency-injection)、するサービスを許可する[ビューに挿入される](xref:mvc/views/dependency-injection)です。
