---
title: ASP.NET Core でのレイアウト
author: ardalis
description: 共通レイアウトの使用方法、ディレクティブの共有方法、および ASP.NET Core アプリでビューをレンダリングする前に共通コードを実行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/views/layout
ms.openlocfilehash: a99b239a0aeeb14492b1eee962dc1149f056f0eb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274119"
---
# <a name="layout-in-aspnet-core"></a>ASP.NET Core でのレイアウト

作成者: [Steve Smith](https://ardalis.com/)

ビューは多くの場合、ビジュアルおよびプログラムの要素を共有します。 この記事では、共通レイアウトの使用方法、ディレクティブの共有方法、および ASP.NET アプリでビューをレンダリングする前に共通コードを実行する方法について説明します。

## <a name="what-is-a-layout"></a>レイアウトとは

ほとんどの Web アプリには、ユーザーがページ間を移動する際に一貫性のあるエクスペリエンスを提供する共通レイアウトがあります。 通常、このレイアウトには、アプリのヘッダー、ナビゲーションまたはメニュー要素、フッターなどの共通のユーザー インターフェイス要素が含まれます。

![ページ レイアウトの例](layout/_static/page-layout.png)

スクリプトやスタイルシートなどの共通の HTML 構造体も、アプリ内の多くのページでよく使用されます。 これらの共有要素をすべて *layout* ファイルで定義することで、アプリ内で使用する任意のビューで参照できるようになります。 レイアウトは、[DRY (Don't Repeat Yourself) 原則](http://deviq.com/don-t-repeat-yourself/)に従って、ビュー内の重複するコードを削減します。

規則により、ASP.NET アプリの既定のレイアウトには `_Layout.cshtml` という名前が付けられます。 Visual Studio ASP.NET Core MVC プロジェクト テンプレートには、このレイアウト ファイルが `Views/Shared` フォルダーに含まれています。

![ソリューション エクスプローラーの Views フォルダー](layout/_static/web-project-views.png)

このレイアウトは、アプリのビューの最上位のテンプレートを定義します。 アプリはレイアウトを必要とせず、それぞれ異なるレイアウトを指定するさまざまなビューを使用して、複数のレイアウトを定義できます。

`_Layout.cshtml` の例:

[!code-html[](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>レイアウトの指定

Razor ビューには `Layout` プロパティがあります。 個々のビューは、このプロパティを設定することでレイアウトを指定します。

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定されたレイアウトは、完全なパス (例: `/Views/Shared/_Layout.cshtml`) または部分的な名前 (例: `_Layout`) を使用できます。 部分的な名前を指定すると、Razor ビュー エンジンが標準の検出プロセスを使用して、レイアウト ファイルを検索します。 コントローラーに関連付けられているフォルダーが最初に検索され、次に `Shared` フォルダーが検索されます。 この検出プロセスは、[部分ビュー](partial.md)の検出に使用されるのと同じプロセスです。

既定では、すべてのレイアウトで `RenderBody` を呼び出す必要があります。 `RenderBody` への呼び出しが配置されると、ビューのコンテンツがレンダリングされます。

<a name="layout-sections-label"></a>

### <a name="sections"></a>セクション

レイアウトは、必要に応じて `RenderSection` を呼び出すことで、1 つ以上の*セクション*を参照することができます。 セクションは、特定のページ要素の配置場所を整理する方法を提供します。 `RenderSection` の呼び出しごとに、そのセクションを必須またはオプションにするかどうかを指定できます。 必須のセクションが見つからない場合、例外がスローされます。 個々のビューは、`@section` Razor 構文を使用して、セクション内にレンダリングされるコンテンツを指定します。 ビューでセクションを定義する場合は、レンダリングされる必要があります (そうしないと、エラーが発生します)。

ビューでの `@section` 定義の例:

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

上記のコードでは、フォームを含むビューの `scripts` セクションに検証スクリプトが追加されます。 同じアプリケーション内の他のビューに追加のスクリプトが必要ない場合は、スクリプト セクションを定義する必要はありません。

ビューで定義されたセクションは、そのイミディエイト レイアウト ページでのみ使用できます。 これらは、部分、ビュー コンポーネント、またはビュー システムの他の部分からは参照できません。

### <a name="ignoring-sections"></a>セクションの無視

既定では、コンテンツ ページの本文とすべてのセクションがレイアウト ページですべてレンダリングされる必要があります。 Razor ビュー エンジンは、本文と各セクションがレンダリングされているかどうかを追跡することによってこれを実行します。

本文またはセクションを無視するようにビュー エンジンに指示するには、`IgnoreBody` メソッドと `IgnoreSection` メソッドを呼び出します。

Razor ページ内の本文とすべてのセクションは、レンダリングされるか無視される必要があります。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>共有ディレクティブのインポート

ビューは、Razor ディレクティブを使用して、名前空間のインポートや[依存性の注入](dependency-injection.md)など、多くのことを行うことができます。 多くのビューで共有されるディレクティブは、共通の `_ViewImports.cshtml` ファイルで指定できます。 `_ViewImports` ファイルは、次のディレクティブをサポートします。

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

このファイルは、関数やセクションの定義などの Razor 機能をサポートしていません。

`_ViewImports.cshtml` ファイルのサンプル:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

ASP.NET Core MVC アプリの `_ViewImports.cshtml` ファイルは、通常、`Views` フォルダーに配置されます。 `_ViewImports.cshtml` ファイルは、任意のフォルダー内に配置できますが、その場合は、そのフォルダーとそのサブフォルダー内にあるビューにのみ適用されます。 `_ViewImports` ファイルは、最初にルート レベルで処理され、その後ビュー自体の場所までフォルダーごとに処理されるため、ルート レベルで指定された設定がフォルダー レベルでオーバーライドされる可能性があります。

たとえば、ルート レベルの `_ViewImports.cshtml` ファイルで `@model` と `@addTagHelper` を指定し、ビューのコントローラーに関連付けられているフォルダー内の別の `_ViewImports.cshtml` ファイルで異なる `@model` を指定し、別の `@addTagHelper` を追加すると、ビューは両方のタグ ヘルパーのアクセス権を持ち、後者の `@model` を使用するようになります。

1 つのビューに対して複数の `_ViewImports.cshtml` ファイルを実行すると、`ViewImports.cshtml` ファイルに含まれるディレクティブの動作の組み合わせは次のようになります。

* `@addTagHelper`、`@removeTagHelper`: 順番どおりにすべて実行

* `@tagHelperPrefix`: ビューに最も近いものが、他のものをすべてをオーバーライドする

* `@model`: ビューに最も近いものが、他のものをすべてをオーバーライドする

* `@inherits`: ビューに最も近いものが、他のものをすべてをオーバーライドする

* `@using`: すべてが含まれ、重複は無視される

* `@inject`: プロパティごとに、ビューに最も近いものが、同じプロパティ名を持つ他のものすべてをオーバーライドする

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>各ビューの前にコードを実行する

すべてのビューの前に実行する必要があるコードがある場合は、このコードを `_ViewStart.cshtml` ファイル内に配置する必要があります。 規則によって、`_ViewStart.cshtml` ファイルは `Views` フォルダー内に配置されます。 `_ViewStart.cshtml` に一覧表示されているステートメントは、すべての (レイアウトでもなく、部分ビューでもない) 完全なビューより前に実行されます。 [ViewImports.cshtml](xref:mvc/views/layout#viewimports) と同様に、`_ViewStart.cshtml` は階層構造です。 `_ViewStart.cshtml` ファイルがコントローラーに関連付けられているビュー フォルダーで定義されている場合、`Views` フォルダーのルートで定義されているファイル (ある場合) の後に実行されます。

`_ViewStart.cshtml` ファイルのサンプル:

[!code-html[](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上記のファイルは、すべてのビューで `_Layout.cshtml` レイアウトを使用することを指定します。

> [!NOTE]
> 通常、`_ViewStart.cshtml` または `_ViewImports.cshtml` も `/Views/Shared` フォルダーには配置されません。 これらのファイルのアプリ レベルのバージョンは、`/Views` フォルダーに直接配置する必要があります。
