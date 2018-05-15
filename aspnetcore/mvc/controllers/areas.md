---
title: ASP.NET Core の区分
author: rick-anderson
description: 区分は ASP.NET MVC の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 61527eb350b5aba9cb37b1de5acdeae1287bf073
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="areas-in-aspnet-core"></a>ASP.NET Core の区分

作成者: [Dhananjay Kumar](https://twitter.com/debug_mode) および [Rick Anderson](https://twitter.com/RickAndMSFT)

区分は ASP.NET MVC の機能であり、関連する機能を別の名前空間 (ルーティングの場合) およびフォルダー構造 (ビューの場合) としてグループにまとめるために使用されます。 ルーティングを行うために、区分を使用して、別のルート パラメーター `area` を `controller` と `action` に追加して階層を作成します。

区分は、大きな ASP.NET Core MVC Web アプリを小さい機能グループに分割するための方法を提供します。 区分は、実質的にはアプリケーション内の MVC 構造体となります。 MVC プロジェクトでは、モデル、コント ローラー、ビューなどの論理コンポーネントが異なるフォルダーに保持され、MVC では名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。 大きなアプリでは、アプリを機能の個別の高レベル区分に分割すると便利な場合があります。 たとえば、チェックアウト、請求、検索などの複数のビジネス ユニットがある e コマース アプリの場合です。これらのユニットにはそれぞれ独自の論理コンポーネント ビュー、コントローラー、およびモデルがあります。 このシナリオでは、区分を使用して、同じプロジェクト内のビジネス コンポーネントを物理的に分割できます。

区分は、独自のコントローラー、ビュー、およびモデルのセットがある、ASP.NET Core MVC プロジェクトのより小さい機能ユニットとして定義できます。

次のような場合は、MVC プロジェクトで区分を使用することを検討してください。

* アプリケーションが、論理的に区切る必要がある複数の高レベル機能コンポーネントで構成されている。

* 各機能区分を個別に使用できるように、MVC プロジェクトを分割したい。

区分の機能:

* ASP.NET Core MVC アプリは任意の数の区分を持つことができます。

* 各区分には独自のコントローラー、モデル、およびビューがあります。

* 大きな MVC プロジェクトを、個別に使用できる複数の高レベル コンポーネントにまとめることができます。

* 同じ名前の複数のコントローラーをサポートします (ただし、*区分* が異なる場合)。

区分の作成および使用方法を示す例を見てましょう。 たとえば、ストア アプリに製品とサービスという、コントローラーとビューの 2 つの異なるグループがあるとします。 MVC 区分を使用する場合の一般的なフォルダー構造は次のようになります。

* プロジェクト名

  * 区分

    * 製品

      * Controllers

        * HomeController.cs

        * ManageController.cs

      * ビュー

        * Home

          * Index.cshtml

        * 管理

          * Index.cshtml

    * サービス

      * Controllers

        * HomeController.cs

      * ビュー

        * Home

          * Index.cshtml

MVC は区分でのビューのレンダリングを試みる場合、既定では、次の場所で検索を試みます。

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

これらは、`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions` の `AreaViewLocationFormats` を使用して変更できる、既定の場所です。

たとえば、以下のコードでは、'Areas' というフォルダー名が、'Categories' に変更されています。

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

注目すべき 1 つの点は、ここでは *Views* フォルダーの構造のみが重要と見なされ、*Controllers* や *Models* などの他のフォルダーは重要では**ない**ということです。 たとえば、*Controllers* や *Models* フォルダーはまったく必要ありません。 *Controllers* や *Models* のコンテンツはコードのみであり、.dll にコンパイルされますが、*Views* のコンテンツはそのビューに対する要求が実行されるまでコンパイルされないためです。

フォルダー階層を定義したら、各コントローラーが区分に関連付けられていることを MVC に知らせる必要があります。 コントローラー名を `[Area]` 属性で修飾することでこれを行います。

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

新しく作成した区分を操作するルート定義を設定します。 従来のルートと属性ルートの使用を含む、ルート定義の作成方法の詳細については、「[コントローラー アクションへのルーティング](routing.md)」記事を参照してください。 この例では、従来のルートを使用します。 そのためには、*Startup.cs* ファイルを開き、以下の `areaRoute` というルート定義を追加して変更します。

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

`http://<yourApp>/products` を参照することで、`Products` 区分内の `HomeController` の `Index` アクション メソッドが呼び出されます。

## <a name="link-generation"></a>リンクの生成

* 区分ベースのコントローラー内のアクションから、同じコントローラー内の別のアクションへのリンクの生成。

  たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。

  HtmlHelper 構文: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper 構文: `<a asp-action="Index">Go to Product's Home Page</a>`

  'area' と 'controller' の値は現在の要求のコンテキストで既に使用可能であるため、ここでは指定する必要がないことに注意してください。 このような値は `ambient` 値といいます。

* 区分ベースのコントローラー内のアクションから、異なるコントローラー内の別のアクションへのリンクの生成

  たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。

  HtmlHelper 構文: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper 構文: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  ここでは 'area' のアンビエント値が使用されていますが、'controller' 値は上記では明示的に指定されていることに注意してください。

* 区分ベースのコントローラー内のアクションから、異なるコントローラーと異なる区分の別のアクションへのリンクの生成。

  たとえば、現在の要求のパスが `/Products/Home/Create` であるとします。

  HtmlHelper 構文: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper 構文: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  ここではアンビエント値が使用されないことに注意してください。

* 区分ベースのコントローラー内のアクションから、異なるコントローラーと、区分には**ない**別のアクションへのリンクの生成。

  HtmlHelper 構文: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper 構文: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  非区分ベースのコントローラー アクションへのリンクを生成する必要があるため、ここでは 'area' のアンビエント値は空にします。

## <a name="publishing-areas"></a>区分の発行

`<Project Sdk="Microsoft.NET.Sdk.Web">` が *.csproj* ファイルに含まれている場合、すべての `*.cshtml` および `wwwroot/**` ファイルが発行され、出力されます。
