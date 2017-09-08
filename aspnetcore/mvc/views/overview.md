---
title: "ビューの概要"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 668c320d-c050-45e3-8161-2f460dc93b2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: a93ee8165be52e33c2e7da4d3fee2c8225864db9
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="rendering-html-with-views-in-aspnet-core-mvc"></a>ASP.NET Core MVC でビューを使用して HTML をレンダリング

によって[Steve Smith](http://ardalis.com)

ASP.NET Core MVC コント ローラーを使用して書式設定された結果を返すことができます*ビュー*です。

## <a name="what-are-views"></a>ビューとは

モデル ビュー コント ローラー (MVC) パターンでは、*ビュー*アプリでのユーザーの操作のプレゼンテーションの詳細情報をカプセル化します。 ビューは、埋め込みコードをクライアントに送信するコンテンツを生成する HTML テンプレートです。 ビューを使用して[Razor 構文](razor.md)、これにより、最小限のコードまたは手続きに HTML と対話するコードです。

ASP.NET Core MVC ビューは*.cshtml*既定で格納されているファイル、*ビュー*アプリケーション内のフォルダーです。 通常、各コント ローラーは、特定のコント ローラー アクションのビューは、独自のフォルダーがあります。

![ソリューション エクスプ ローラー ビュー フォルダー](overview/_static/views_solution_explorer.png)

アクションに固有のビューだけでなく[部分ビュー](partial.md)、[レイアウト、およびその他の特殊なビュー ファイル](layout.md)繰り返しが削減され、アプリのビューの中で再利用できるようにするために使用できます。

## <a name="benefits-of-using-views"></a>ビューを使用する利点

ビューを提供[関心の分離](http://deviq.com/separation-of-concerns/)MVC アプリでユーザー インターフェイスのビジネス ロジックから個別にレベル マークアップをカプセル化します。 ASP.NET MVC ビューを使用して[Razor 構文](razor.md)HTML マークアップとサーバー側ロジック簡単に切り替えることです。 使用してビューの間でのアプリのユーザー インターフェイスの一般的な反復的な側面を簡単に再利用する[レイアウトと共有ディレクティブ](layout.md)または[部分ビュー](partial.md)です。

## <a name="creating-a-view"></a>ビューを作成します。

コント ローラーに固有のビューを作成するのには*ビュー/[ControllerName]*フォルダーです。 コント ローラー間で共有されるビューについてに、 */ビュー/共有*フォルダーです。 ファイルの表示を関連付けられたコント ローラー アクションの場合と同じ名前し、追加、 *.cshtml*ファイル拡張子。 例については、ビューを作成する、*に関する*アクションを*ホーム*コント ローラーを作成、 *About.cshtml*ファイルで、   */ビュー/ホーム*フォルダーです。

サンプル ファイルの表示 (*About.cshtml*)。

[!code-html[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor*でコードが示される、`@`シンボル。 (C#) ステートメントが Razor コード ブロックは中かっこで off に設定内で実行されます (`{` `}`)、"About"の割り当てなどに、`ViewData["Title"]`上に示す要素。 Razor を使用してで値を単に参照によって HTML 内の値を表示すること、 `@` symbol, 内で示すように、`<h2>`と`<h3>`要素上です。

このビューは、担当するは、出力の部分だけについて説明します。 ページのレイアウトの残りの部分と、ビューの他の一般的な側面は、他の場所で指定されます。 詳細については[レイアウトと共有のビュー ロジック](layout.md)です。

## <a name="how-do-controllers-specify-views"></a>コント ローラーを指定してビューを操作する方法

ビューは通常の操作から返される、`ViewResult`です。 アクション メソッドが作成して返すことができます、`ViewResult`を直接がより一般的なコント ローラーから継承している場合`Controller`、単に使用する、`View`ヘルパー メソッドを次の例として示します。

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

`View`ヘルパー メソッドが返すビュー容易にするためアプリ開発者にとっていくつかのオーバー ロードします。 ビューに渡すモデル オブジェクトと同様に戻るには、ビューを指定することができます。

この操作から制御が戻るとき、 *About.cshtml*上に表示されるビューが表示されます。

![ページについて](overview/_static/about-page.png)

### <a name="view-discovery"></a>ビューの検出

アクションは、ビューを返します、ときに、プロセスを呼び出す*ビュー検出*行われます。 このプロセスでは、どのビュー ファイルを使用してを決定します。 ランタイムはコント ローラーに固有のビューはまず、し、検索に一致するビューの名前の検索、特定のビュー ファイルが指定されていない限り、 *Shared*フォルダーです。

アクションが返されるときに、`View`メソッドでは、次のように`return View();`アクション名は、ビュー名として使用します。 たとえば、これは、"Index"という名前のアクション メソッドから呼び出された、なります"Index"の表示名を渡すことに相当します。 メソッドに、ビュー名を明示的に渡すことができます (`return View("SomeView");`)。 このような場合の両方で一致するビューでのファイルの検索を参照してください。

   1. ビュー/<ControllerName>/<ViewName>.cshtml

   2. ビュー/共有/<ViewName>.cshtml

>[!TIP]
> 次の規則だけを返すことをお勧め`View()`可能であればより柔軟で簡単にコードをリファクタリングのためのアクションからです。

ビュー名の代わりに、ビューのファイル パスを指定できます。 ここで、 *.cshtml*拡張機能は、ファイルのパスの一部として指定する必要があります。 アプリケーション ルートに対する相対パスでなければなりません (先頭に使用できます必要に応じて「/」または"~/") です。 例: `return View("Views/Home/About.cshtml");`

> [!NOTE]
> [部分ビュー](partial.md)と[コンポーネントを表示](view-components.md)似ています (ただしと一致しない) の検出メカニズムを使用します。

> [!NOTE]
> ビューが配置されているアプリ内でカスタムを使用してに関する既定の規則をカスタマイズする`IViewLocationExpander`です。

>[!TIP]
> ビューの名前は、基になるファイル システムによって大文字小文字を区別可能性があります。 オペレーティング システム間で互換性のため、常に大文字小文字をコント ローラーとアクション名と関連するビューのフォルダーとファイル名の間です。

## <a name="passing-data-to-views"></a>ビューにデータを渡す

いくつかのメカニズムを使用して、ビューにデータを渡すことができます。 最も堅牢な方法は、指定する、*モデル*ビュー内の型 (一般に呼ば、 *viewmodel*、ビジネス ドメイン モデルの種類を区別するために)、ビューにこの型のインスタンスを渡すとアクション。 モデルまたはモデルの表示を使用してデータをビューに渡すことをお勧めします。 これにより、厳密な型チェックを活用するために表示できます。 使用してビューのモデルを指定することができます、`@model`ディレクティブ。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@model WebApplication1.ViewModels.Address
   <h2>Contact</h2>
   <address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

ビューに送信されるインスタンスを使用して、厳密に型指定された方法でアクセスできるビューのモデルを指定すると、`@Model`上記のようにします。 ビューにモデルの種類のインスタンスを提供するには、コント ローラーは、それをパラメーターとして渡します。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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

ビュー モデルとしてを指定できる型に制限はありません。 ビジネス ロジックは、アプリで他の場所でカプセル化できますように POCO Plain Old CLR Object () でほとんどまたはまったく動作では、モデルの表示を渡すことをお勧めします。 このアプローチの例は、*アドレス*viewmodel 上記の例で使用します。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [13]}} -->

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
> 同じクラスを使用して、ビジネス モデルの種類と、表示の種類のモデルを何もできません。 ただし、分離しておくことに、ドメインまたは永続化モデルと無関係に変更するためにビューは、によってセキュリティ上の利点も提供できます (モデルを使用して、アプリにユーザーを送信する[モデル バインディング](../models/model-binding.md))。

### <a name="loosely-typed-data"></a>弱く型指定されたデータ

厳密に型指定されたビューは、以外のすべてのビューにデータの弱い型指定されたコレクションへのアクセスがあります。 この同じコレクションは、いずれかで参照できる、`ViewData`または`ViewBag`コント ローラーとビューのプロパティです。 `ViewBag`プロパティはラッパー`ViewData`コレクションを動的ビューを提供します。 別のコレクションではありません。

`ViewData`ディクショナリ オブジェクトを通じてアクセス`string`キー。 格納し、内のオブジェクトを取得でき、それらを抽出する場合は、特定の型にキャストする必要があります。 使用することができます`ViewData`ビューに、ビュー (部分ビュー、およびレイアウト) 内や、コント ローラーからデータを渡す。 文字列データの格納され、キャストがなくても、直接使用されることができます。

いくつかの値を設定`ViewData`アクションで。

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

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3, 6]}} -->

```html
@{
       // Requires cast
       var address = ViewData["Address"] as Address;
   }

   @ViewData["Greeting"] World!

   <address>
       @address.Name<br />
       @address.Street<br />
       @address.City, @address.State @address.PostalCode
   </address>
   ```

`ViewBag`オブジェクトに格納されているオブジェクトへの動的アクセスを提供する`ViewData`です。 簡単にキャストする必要がないため、操作を指定できます。 使用して、上記と同じ例`ViewBag`厳密に型指定ではなく`address`ビュー内のインスタンス。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 4, 5, 6]}} -->

```html
@ViewBag.Greeting World!

   <address>
       @ViewBag.Address.Name<br />
       @ViewBag.Address.Street<br />
       @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
   </address>
   ```

> [!NOTE]
> いずれも、同じ基になるを参照するので`ViewData`コレクションすることができますを混在させるし、の間で一致`ViewData`と`ViewBag`便利な場合、値を読み書きするときにします。

### <a name="dynamic-views"></a>動的ビュー

ビューがモデルの型を宣言しませんが、渡されたモデルのインスタンスは、このインスタンスを動的に参照できます。 場合のインスタンスなど、`Address`宣言していないので、ビューに渡される、`@model`ビューが示すように動的にインスタンスのプロパティを参照してもよい。

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [13, 16, 17, 18]}} -->

```html
<address>
       @Model.Street<br />
       @Model.City, @Model.State @Model.PostalCode<br />
       <abbr title="Phone">P:</abbr>
       425.555.0100
   </address>
   ```

この機能は、いくつかの柔軟性を提供できますが、IntelliSense でもコンパイル保護は提供しません。 プロパティが存在しない場合、ページは実行時に失敗します。

## <a name="more-view-features"></a>詳細ビュー

[タグ ヘルパー](tag-helpers/intro.md)簡単にカスタム コードまたはビューの中でヘルパーを使用する必要がある、既存の HTML タグをサーバー側の動作を追加します。 タグ ヘルパーは、編集して、さまざまなツールで表示するビューのマークアップを許可するのには、馴染みがないエディターでは無視されますが、HTML 要素に属性として適用されます。 タグ ヘルパーは、多くの用途し、具体的には行うことができます[フォームを使用する](working-with-forms.md)はるかに簡単です。

カスタムの HTML マークアップを生成する行うには多くの組み込み HTML ヘルパーおよびで (場合によっては、独自のデータ要件) のより複雑な UI ロジックをカプセル化できます[ビュー コンポーネント](view-components.md)です。 ビュー コンポーネントは、同じコント ローラーとビューを提供して、関心の分離を提供され、共通の UI 要素で使用されるデータを扱うアクションおよびビューは、する必要があります。

ASP.NET Core の他の多くの側面と同様にサポートをビュー[依存性の注入](../../fundamentals/dependency-injection.md)、するサービスを許可する[ビューに挿入される](dependency-injection.md)です。
