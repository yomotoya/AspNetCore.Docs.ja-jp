---
title: "ASP.NET MVC から ASP.NET Core MVC への移行"
author: ardalis
description: "ASP.NET Core MVC に ASP.NET MVC プロジェクトを移行する作業を開始する方法を説明します。"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: c9c9f63cd635f364d9b2e081dc051a46a44d3e4f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC から ASP.NET Core MVC への移行

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)

この記事に ASP.NET MVC プロジェクトを移行する作業を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)です。 プロセスでは、ASP.NET MVC から変更されたことの多くを強調表示します。 複数段階のプロセスは、ASP.NET MVC を移行して、この記事は、初期セットアップ、基本的なコント ローラーとビュー、静的なコンテンツ、およびクライアント側の依存関係について説明します。 その他の記事では、構成を移行して多くの ASP.NET MVC プロジェクトで見つかった id コードについて説明します。

> [!NOTE]
> サンプルのバージョン番号が最新でない可能性があります。 したがって、プロジェクトを更新する必要があります。

## <a name="create-the-starter-aspnet-mvc-project"></a>スターター ASP.NET MVC プロジェクトを作成します。

アップグレードを示すためには、ASP.NET MVC アプリケーションを作成することで始めます。 名前を使用して作成*WebApp1* ASP.NET Core プロジェクトを作成すると、次の手順には、名前空間に一致するようにします。

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ: MVC プロジェクト テンプレートがテンプレートの ASP.NET のパネルで選択されています。](mvc/_static/new-project-select-mvc-template.png)

*省略可能:*からソリューションの名前を変更*WebApp1*に*Mvc5*です。 Visual Studio 新しいソリューション名が表示されます (*Mvc5*)、するやすく次回のプロジェクトからこのプロジェクトに指示します。

## <a name="create-the-aspnet-core-project"></a>ASP.NET Core プロジェクトを作成します。

新規作成*空*前のプロジェクトと同じ名前での ASP.NET Core web アプリ (*WebApp1*) ため、2 つのプロジェクトの名前空間と一致します。 同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。 同じ名前を使用する前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ ボックス: ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *省略可能:*新しい ASP.NET Core アプリケーションを使用して、作成、 *Web アプリケーション*プロジェクト テンプレート。 プロジェクトに名前を*WebApp1*、認証オプションを選択して**個々 のユーザー アカウント**です。 このアプリの名前を変更*FullAspNetCore*です。 変換では、このプロジェクトは、時間を節約を作成します。 最終的な結果を表示する、または変換プロジェクトにコードをコピーするテンプレートによって生成されたコードを調べることができます。 テンプレートによって生成されたプロジェクトと比較するへの変換ステップに行き詰まった場合にも便利です。

## <a name="configure-the-site-to-use-mvc"></a>MVC を使用するサイトを構成します。

* インストール、`Microsoft.AspNetCore.Mvc`と`Microsoft.AspNetCore.StaticFiles`NuGet パッケージの管理。

  `Microsoft.AspNetCore.Mvc` ASP.NET Core MVC フレームワークです。 `Microsoft.AspNetCore.StaticFiles` 静的ファイル ハンドラーです。 ASP.NET ランタイムがモジュール、および静的なファイルが機能するように明示的にオプトインする必要があります (を参照してください[静的ファイルで作業](../fundamentals/static-files.md))。

* 開く、 *.csproj*ファイル (でプロジェクトを右クリックして**ソリューション エクスプ ローラー**を選択し、**編集 WebApp1.csproj**) を追加し、`PrepareForPublish`ターゲット。

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  `PrepareForPublish`ターゲットは、Bower を使用してクライアント側のライブラリを取得するために必要です。 後でについてを説明します。

* 開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  `UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。 前述のように、ASP.NET ランタイムは、モジュール、および静的なファイルが機能するように明示的にオプトインする必要があります。 `UseMvc`ルーティング拡張メソッドを追加します。 詳細については、次を参照してください。[アプリケーションの起動](../fundamentals/startup.md)と[ルーティング](../fundamentals/routing.md)です。

## <a name="add-a-controller-and-view"></a>コント ローラーとビューを追加します。

このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するようにビューとビューの次のセクションで移行しますを追加します。

* 追加、*コント ローラー*フォルダーです。

* 追加、 **MVC コント ローラー クラス**名前を持つ*HomeController.cs*を*コント ローラー*フォルダーです。

![[新しい項目の追加] ダイアログ](mvc/_static/add_mvc_ctl.png)

* 追加、*ビュー*フォルダーです。

* 追加、*ビュー/ホーム*フォルダーです。

* 追加、 *Index.cshtml* MVC ビュー ページを*ビュー/ホーム*フォルダーです。

![[新しい項目の追加] ダイアログ](mvc/_static/view.png)

プロジェクトの構造を次に示します。

![ソリューション エクスプ ローラー WebApp1 のファイルとフォルダーの表示](mvc/_static/project-structure-controller-view.png)

内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。

```html
<h1>Hello world!</h1>
```

アプリを実行します。

![Microsoft Edge で開いている web アプリケーション](mvc/_static/hello-world.png)

参照してください[コント ローラー](xref:mvc/controllers/actions)と[ビュー](xref:mvc/views/overview)詳細についてはします。

最小限の作業用 ASP.NET Core プロジェクトができました、ASP.NET MVC プロジェクトから機能の移行が始めることができます。 次に移動する必要があります。

* クライアント側のコンテンツ (CSS、フォント、およびスクリプト)

* コントローラー

* ビュー

* モデル

* バンドル化

* フィルター

* ログの/out、id (はこれで次のチュートリアルです。)

## <a name="controllers-and-views"></a>コント ローラーとビュー

* ASP.NET MVC のコピーの各メソッド`HomeController`を新しい`HomeController`です。 ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り値型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx)以外の場合は ASP.NET Core MVC、アクション メソッドの戻り値の`IActionResult`代わりにします。 `ActionResult` 実装する`IActionResult`ので、アクション メソッドの戻り値の型を変更する必要はありません。

* コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。

* ASP.NET Core アプリケーションを実行し、各メソッドをテストします。 いない移行ファイルのレイアウトやスタイルまだ、レンダリングされるビューはビュー ファイル内のコンテンツのみを含めるようにします。 レイアウトに生成されたファイルへのリンクする必要はありません、`About`と`Contact`ビュー、ブラウザーからそれらを起動する必要がありますので (交換**4492**プロジェクトで使用されるポート番号を持つ)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

スタイル設定およびメニュー項目がないことに注意してください。 次のセクションでこれを修正します。

## <a name="static-content"></a>静的コンテンツ

ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが混合されました。 ASP.NET Core での静的なコンテンツをホスト、 *wwwroot*フォルダーです。 古い ASP.NET MVC のアプリから静的なコンテンツをコピーしておく、 *wwwroot* ASP.NET Core プロジェクトのフォルダーにします。 このサンプルの変換。

* コピー、 *favicon.ico*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクト フォルダーにします。

古い ASP.NET MVC プロジェクトの使用[ブートス トラップ](http://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダーです。 以前の ASP.NET MVC プロジェクトを生成するには、テンプレートを参照して、レイアウト ファイル ブートス トラップ (*Views/Shared/_Layout.cshtml*)。 コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*フォルダーに、新しいプロジェクトが、その方法を使用しない、ASP.NET Core でのクライアント側の依存関係を管理するための強化メカニズムです。

新しいプロジェクトを使用してブートス トラップ (およびその他のクライアント側ライブラリ) サポートを追加します[Bower](https://bower.io/):

* 追加、 [Bower](https://bower.io/)という名前の構成ファイル*bower.json*プロジェクトのルートに (、プロジェクトを右クリックし、**追加 > 新しい項目の追加 > Bower 構成ファイル**)。 追加[ブートス トラップ](http://getbootstrap.com/)と[jQuery](https://jquery.com/)ファイル (以下の強調表示された行を参照してください)。

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

ファイルを保存するときに Bower は自動的にダウンロードする依存関係、 *wwwroot/lib*フォルダーです。 使用することができます、**ソリューション エクスプ ローラーの検索**資産のパスを検索するボックス。

![ソリューション エクスプ ローラー検索結果に示される jquery 資産](mvc/_static/search.png)

参照してください[Bower でクライアント側のパッケージを管理](../client-side/bower.md)詳細についてはします。

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>レイアウト ファイルを移行します。

* コピー、*は _viewstart.vbhtml*古い ASP.NET MVC プロジェクトのファイルの*ビュー* ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダーです。 *は _viewstart.vbhtml* ASP.NET Core MVC でファイルが変更されていません。

* 作成、 *Views/shared*フォルダーです。

* *省略可能:*コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー* ASP.NET Core プロジェクトのフォルダー *ビュー*フォルダーです。 任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。 *_ViewImports.cshtml*ファイル ビューのすべてのファイルの名前空間を提供しでによってもたらさ[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。 タグ ヘルパーは、新しいレイアウト ファイルで使用されます。 *_ViewImports.cshtml*ファイルが ASP.NET Core の新機能です。

* コピー、 *_Layout.cshtml*古い ASP.NET MVC プロジェクトのファイルの*Views/shared* ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダーです。

開いている*_Layout.cshtml*ファイルし、次の変更 (完了したコードは、以下のとおりです)。

   * 置き換える`@Styles.Render("~/Content/css")`で、`<link>`読み込み要素*bootstrap.css* (下記参照)。

   * `@Scripts.Render("~/bundles/modernizr")` を削除します。

   * コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。 今後のチュートリアルでを返します。

   * 置き換える`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。

   * 置き換える`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照).

置換する CSS リンク:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

交換用のスクリプト タグ:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

更新された*_Layout.cshtml*ファイルを次に示します。

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

ブラウザーで、サイトを表示します。 これを場所に必要なスタイルを使用して正しく読み込むようになりました必要があります。

* *省略可能:*新しいレイアウト ファイルを使用して再試行することができます。 このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。 新しいレイアウト ファイルを使用して[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)あり、その他の改善。

## <a name="configure-bundling-and-minification"></a>バンドルと縮小を構成します。

バンドルと縮小を構成する方法については、次を参照してください。 [Bundling and Minification](../client-side/bundling-and-minification.md)です。

## <a name="solving-http-500-errors"></a>HTTP 500 のエラーを解決します。

多くの問題 HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報が含まれていないことがあります。 たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。 詳細なエラー メッセージを取得するには、次のコードを追加します。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

参照してください**開発者例外ページを使用して**で[Error Handling](../fundamentals/error-handling.md)詳細についてはします。

## <a name="additional-resources"></a>その他の技術情報

* [クライアント側の開発](xref:client-side/index)
* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
