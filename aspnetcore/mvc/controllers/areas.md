---
title: "区分"
author: rick-anderson
description: "領域を使用する方法を示します。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 666be2da6b38ffb538ae3888ea879a4104c8fd12
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="areas"></a>区分

によって[Dhananjay Kumar](https://twitter.com/debug_mode)と[Rick Anderson](https://twitter.com/RickAndMSFT)

領域は、別の名前空間 (ルーティング) と (views) 用のフォルダー構造をグループに関連する機能を整理するため、ASP.NET MVC 機能です。 別のルート パラメーターを追加することでルーティングするために階層を作成する領域を使用して`area`を`controller`と`action`です。

領域は、大規模な ASP.NET Core MVC Web アプリケーションを小さい機能グループに分割する方法を提供します。 領域は、事実上、アプリケーション内部 MVC 構造です。 MVC プロジェクトでは、モデル、コント ローラー、およびビューなどの論理コンポーネントが、他のフォルダーに保持され、MVC では、名前付け規則を使用して、これらのコンポーネント間のリレーションシップを作成します。 大規模なアプリの機能の個別の高レベル領域に、アプリを分割すると便利な場合があります。 たとえば、電子商取引を使用したアプリなど、チェック アウト、請求、および検索などの複数の事業単位です。各事業ユニットは、独自の論理コンポーネント ビュー、コント ローラー、およびモデルがあります。 このシナリオでは、同じプロジェクトでは、ビジネス コンポーネントの物理的にパーティション分割に領域を使用できます。

領域は、コント ローラー、ビュー、およびモデルの独自セットと ASP.NET Core MVC プロジェクトでより小さな機能ユニットとして定義することができます。

MVC の領域の使用を検討時にプロジェクトします。

* 論理的に区切る必要があります複数の高度な機能コンポーネントのアプリケーションが行われます

* 各機能領域に作業できるいない個別にできるように、MVC プロジェクトのパーティション分割します。

領域の機能:

* ASP.NET Core MVC アプリの領域の任意の数を持つことができます。

* 各領域には、独自のコント ローラー、モデル、およびビュー

* 作業できるいない個別に複数の高度なコンポーネントに大規模な MVC プロジェクトを整理することができます。

* 異なるを持っていれば、同じ名前の複数のコント ローラーをサポートしている*領域*

領域の作成方法および使用方法を説明する例を見てをみましょう。 コント ローラーとビューの 2 つの異なるグループのあるストア アプリがあるとしましょう。 製品およびサービス。 一般的なフォルダーは、ある MVC 領域を使用して次のように下の構造化します。

* プロジェクト名

  * 区分

    * 製品

      * コント ローラー

        * HomeController.cs

        * ManageController.cs

      * ビュー

        * Home

          * Index.cshtml

        * 管理

          * Index.cshtml

    * サービス

      * コント ローラー

        * HomeController.cs

      * ビュー

        * Home

          * Index.cshtml

MVC は、既定では、領域内のビューをレンダリングするときは、次の場所で検索を試みます。

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

これらは、既定の場所を使用して変更することができます、`AreaViewLocationFormats`上、`Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`です。

たとえばで、以下のコード '領域' として、フォルダー名ではなく、変更されている '分類' をします。

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

1 つに注目することですの構造、*ビュー*フォルダーと考えられる重要ここで 1 つのみでありなどの他のフォルダーのコンテンツ*コント ローラー*と*モデル*は**いない**は重要です。 たとえば、する必要はありません、*コント ローラー*と*モデル*まったくフォルダーです。 これは機能するためのコンテンツ*コント ローラー*と*モデル*のコンテンツの場所としてコンパイルして .dll に取得される単なるコード、*ビュー*に要求されるまでビューが加えられました。

フォルダー階層を定義したら、各コント ローラーが領域に関連付けられている MVC に指示する必要があります。 コント ローラー名を修飾すれば、`[Area]`属性。

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

新しく作成された分野で動作するルート定義を設定します。 [コント ローラー アクションへのルーティング](routing.md)属性ルートと従来のルートを使用するなどの route 定義を作成する方法について詳細にアーティクルが入ります。 この例では、従来のルートを使用します。 開く、 *Startup.cs*ファイルし、追加することによって変更、`areaRoute`下記の route 定義をという名前です。

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

参照して`http://<yourApp>/products`、`Index`のアクション メソッド、`HomeController`で、`Products`領域が呼び出されます。

## <a name="link-generation"></a>リンクの生成

* 領域内でのアクションからのリンクを生成すると、同じコント ローラー内の別のアクションのコント ローラーが基づいています。

  たとえば、現在の要求のパスはのように、`/Products/Home/Create`

  HtmlHelper 構文:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper 構文:`<a asp-action="Index">Go to Product's Home Page</a>`

  '区分' と 'controller' の値を指定しない必要があるおことに注意してくださいここは既に現在の要求のコンテキストで使用できる、です。 このような値と呼ばれる`ambient`値。

* 別のコント ローラー上の別のアクションのコント ローラーに基づく領域内でのアクションからのリンクを生成します。

  たとえば、現在の要求のパスはのように、`/Products/Home/Create`

  HtmlHelper 構文:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  TagHelper 構文:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  アンビエント '領域' の値を使用するここでは、上で 'controller' 値が明示的に指定したことに注意してください。

* 領域内でのアクションからのリンクを生成するコント ローラーは別のアクションを別のコント ローラーとに基づいて別の領域。

  たとえば、現在の要求のパスはのように、`/Products/Home/Create`

  HtmlHelper 構文:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper 構文:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  なおここアンビエント値は使用されません。

* 領域のコント ローラー内のアクションから別のコント ローラー上の別のアクション リンクを生成して**いない**領域。

  HtmlHelper 構文:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  TagHelper 構文:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  生成するため非領域へのリンク ベースのコント ローラー アクション、空のお '領域' ここでのアンビエント値。

## <a name="publishing-areas"></a>発行の領域

すべて`*.cshtml`と`wwwroot/**`ファイルに出力するときにパブリッシュされます`<Project Sdk="Microsoft.NET.Sdk.Web">`に含まれる、 *.csproj*ファイル。
