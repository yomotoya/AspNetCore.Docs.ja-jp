---
title: "レイアウト"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/layout
ms.openlocfilehash: f225e2a93edfc552961f9f16294bc0ace6eb0002
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="layout"></a>レイアウト

によって[Steve Smith](https://ardalis.com/)

ビューは、ビジュアル化およびプログラムの要素を頻繁に共有します。 この記事では、一般的なレイアウトを使用して、共有するディレクティブ、および ASP.NET アプリのビューのレンダリングの前に共通のコードを実行する方法を学習します。

## <a name="what-is-a-layout"></a>レイアウトします。

ほとんどの web アプリでは、ページ間を移動するのに合わせて一貫した使用環境をユーザーに提供する一般的なレイアウトがあります。 レイアウトには、通常、アプリのヘッダー、ナビゲーションまたはメニュー要素、およびフッターなどの一般的なユーザー インターフェイス要素が含まれます。

![ページ レイアウトの例](layout/_static/page-layout.png)

スクリプトとスタイル シートなどの共通の HTML 構造体は、アプリ内の多数のページでも頻繁に使用されます。 これらの共有要素のすべての環境で定義されている可能性があります、*レイアウト*ファイルで、アプリ内で使用される任意のビューで参照できます。 レイアウト削減重複コード ビュー、支援をフォロー、[しない繰り返します自分で (ドライ) 原則](http://deviq.com/don-t-repeat-yourself/)です。

慣例により、ASP.NET アプリケーションの既定のレイアウトが名前付き`_Layout.cshtml`します。 Visual Studio ASP.NET Core の MVC プロジェクト テンプレートには、このレイアウト ファイルが含まれています、`Views/Shared`フォルダー。

![ソリューション エクスプ ローラー ビュー フォルダー](layout/_static/web-project-views.png)

このレイアウトは、アプリでのビューの最上位レベルのテンプレートを定義します。 アプリには、レイアウトは不要、アプリは、さまざまなレイアウトを指定するさまざまなビューで複数のレイアウトを定義できます。

たとえば`_Layout.cshtml`:

[!code-html[Main](../../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=42,66)]

## <a name="specifying-a-layout"></a>レイアウトの指定

Razor ビューが、`Layout`プロパティです。 個々 のビューは、このプロパティを設定して、レイアウトを指定します。

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml?highlight=2)]

指定したレイアウトは、完全なパスを使用できます (例: `/Views/Shared/_Layout.cshtml`) または部分的な名前 (例: `_Layout`)。 部分的な名前を指定すると、Razor ビュー エンジンは、標準の探索プロセスを使用して、レイアウト ファイルで検索します。 コント ローラーに関連付けられているフォルダーが最初に検索後に、`Shared`フォルダーです。 この検出プロセスが検出に使用したものと同じ[部分ビュー](partial.md)です。

既定では、すべてのレイアウトを呼び出す必要があります`RenderBody`です。 任意の場所への呼び出し`RenderBody`が配置されると、ビューの内容が表示されます。

<a name="layout-sections-label"></a>

### <a name="sections"></a>セクション

レイアウトは、必要に応じて 1 つまたは複数を参照できます*セクション*、呼び出すことによって`RenderSection`です。 セクションでは、特定のページ要素の配置場所を整理することを示します。 各呼び出し`RenderSection`そのセクションが必須またはオプションかどうかを指定できます。 必要なセクションが見つからない場合、例外がスローされます。 個々 のビューを使用して、セクション内に表示するコンテンツを指定する、 `@section` Razor 構文です。 ビューには、セクションが定義されている場合、表示する必要があります (または、エラーが発生)。

たとえば`@section`ビューで定義します。

```html
@section Scripts {
     <script type="text/javascript" src="/scripts/main.js"></script>
   }
   ```

上記のコードに検証スクリプトを追加、`scripts`セクションのフォームを含むビュー。 同じアプリケーション内の他のビューは、追加のスクリプトは必要ありませんがおよびためスクリプトのセクションを定義する必要はありません。

ビューで定義されたセクションは、イミディ エイト レイアウト ページでのみ使用できます。 パーシャル、コンポーネントの表示、またはシステムの表示の他の部分からは参照できません。

### <a name="ignoring-sections"></a>セクションでは無視されます。

既定では、本体およびコンテンツ ページのすべてのセクション必要がありますすべてによってレンダリングされるページをレイアウトします。 Razor ビュー エンジンは、本文と各セクションで表示されているかどうかを追跡することによってこれを適用します。

セクションでは、本文を無視する、ビュー エンジンを指示する、`IgnoreBody`と`IgnoreSection`メソッドです。

本文と Razor ページで、すべてのセクション必要があるレンダリングまたは無視します。

<a name="viewimports"></a>

## <a name="importing-shared-directives"></a>共有ディレクティブのインポート

ビューは、Razor ディレクティブを使用して名前空間をインポートまたはを実行するなど、多くの作業を行う[依存性の注入](dependency-injection.md)です。 共通の多くのビューで共有されるディレクティブを指定することがあります`_ViewImports.cshtml`ファイル。 `_ViewImports`ファイルは、次のディレクティブをサポートします。

* `@addTagHelper`

* `@removeTagHelper`

* `@tagHelperPrefix`

* `@using`

* `@model`

* `@inherits`

* `@inject`

ファイルは、関数とセクションの定義などの Razor 機能をサポートしません。

サンプル`_ViewImports.cshtml`ファイル。

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewImports.cshtml)]

`_ViewImports.cshtml` ASP.NET Core MVC アプリは、一般に配置のファイル、`Views`フォルダーです。 A`_ViewImports.cshtml`場合にのみ適用されますが、そのフォルダーとそのサブフォルダー内ビューを任意のフォルダー内でファイルを配置することができます。 `_ViewImports`ファイルのルート レベルでは、起動処理がおり、しに至るまでの各フォルダーについて自体には、ビューの場所のため、ルート レベルの設定を指定オーバーライド フォルダー レベルでします。

たとえば、ルート レベル`_ViewImports.cshtml`ファイルを指定`@model`と`@addTagHelper`、別および`_ViewImports.cshtml`ビューのコント ローラーに関連付けられているフォルダー内のファイルが、さまざまなを指定します`@model`し、別の追加`@addTagHelper`、ビュー両方のタグ ヘルパーにはアクセスおよび後者を使用して、`@model`です。

複数`_ViewImports.cshtml`ファイル ビューの実行に含まれるディレクティブの動作を組み合わせて、`ViewImports.cshtml`ファイルに次のようになります。

* `@addTagHelper`、 `@removeTagHelper`: 順序で、すべての実行

* `@tagHelperPrefix`: ビューに最も近いものでは、その他のものよりも優先

* `@model`: ビューに最も近いものでは、その他のものよりも優先

* `@inherits`: ビューに最も近いものでは、その他のものよりも優先

* `@using`: すべてが含まれます。重複部分は無視されます。

* `@inject`: 各プロパティのビューに最も近いものよりも優先、それ以外の、同じプロパティ名

<a name="viewstart"></a>

## <a name="running-code-before-each-view"></a>各ビューの前にコードを実行しています。

すべてのビューの前に実行する必要があります、これは、コードがある場合、`_ViewStart.cshtml`ファイル。 慣例により、`_ViewStart.cshtml`ファイルにある、`Views`フォルダーです。 表示されているステートメント`_ViewStart.cshtml`すべて完全のビュー (いないレイアウト、およびいない部分ビュー) の前に実行されます。 同様に[ViewImports.cshtml](xref:mvc/views/layout#viewimports)、`_ViewStart.cshtml`は階層構造です。 場合、`_ViewStart.cshtml`コント ローラーに関連付けられている表示フォルダーで定義されたファイルは、これは、1 つのルートで定義されている後に実行されます、`Views`フォルダー (存在する場合)。

サンプル`_ViewStart.cshtml`ファイル。

[!code-html[Main](../../common/samples/WebApplication1/Views/_ViewStart.cshtml)]

上記のファイルは、すべてのビューが使用することを指定します、`_Layout.cshtml`レイアウトです。

> [!NOTE]
> どちらも`_ViewStart.cshtml`も`_ViewImports.cshtml`では、通常、`/Views/Shared`フォルダーです。 これらのファイルのアプリケーション レベルのバージョンに直接配置する必要があります、`/Views`フォルダーです。
