---
title: ASP.NET MVC から ASP.NET Core MVC への移行します。
author: ardalis
description: ASP.NET Core mvc、ASP.NET MVC プロジェクトの移行を開始する方法について説明します。
ms.author: riande
ms.date: 02/13/2019
uid: migration/mvc
ms.openlocfilehash: 2ca51a145243444722ad8081fd8cdbb65d72b53a
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248044"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC から ASP.NET Core MVC への移行します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)

この記事では、ASP.NET MVC プロジェクトの移行を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)します。 プロセスで ASP.NET MVC から変更されたことの多くハイライトされます。 複数手順のプロセスは、ASP.NET MVC から移行して、この記事では、初期セットアップ、基本的なコント ローラーとビュー、静的コンテンツ、およびクライアント側の依存関係について説明します。 その他の記事では、移行の構成と多くの ASP.NET MVC プロジェクトで検出された id コードについて説明します。

> [!NOTE]
> サンプルのバージョン番号が最新でない可能性があります。 それに応じて、プロジェクトを更新する必要があります。

## <a name="create-the-starter-aspnet-mvc-project"></a>Starter の ASP.NET MVC プロジェクトを作成します。

アップグレードを示すためには、ASP.NET MVC アプリの作成から始めます。 名前を使用して作成*WebApp1*ので、名前空間の一致、ASP.NET Core プロジェクトの次の手順で作成します。

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ:ASP.NET テンプレート パネルで選択した MVC プロジェクト テンプレート](mvc/_static/new-project-select-mvc-template.png)

*省略可能。* ソリューションからの名前を変更*WebApp1*に*Mvc5*します。 Visual Studio は、新しいソリューション名を表示します (*Mvc5*)、ことが容易に、次のプロジェクトからこのプロジェクトに指示します。

## <a name="create-the-aspnet-core-project"></a>ASP.NET Core プロジェクトを作成します。

新規作成*空*前のプロジェクトと同じ名前の ASP.NET Core web アプリ (*WebApp1*) 2 つのプロジェクトの名前空間が一致するようにします。 同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。 同じ名前を使用して、前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ:ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *省略可能。* 新しい ASP.NET Core アプリを作成、 *Web アプリケーション*プロジェクト テンプレート。 プロジェクトに名前を*WebApp1*の認証オプションを選択します**個々 のユーザー アカウント**します。 このアプリの名前を変更*FullAspNetCore*します。 変換には、このプロジェクトを保存する時間を作成します。 最終的な結果を参照してください。 または、変換のプロジェクトにコードをコピーするテンプレートによって生成されたコードを確認することができます。 テンプレートによって生成されたプロジェクトと比較する変換ステップを行き詰まった場合にも便利です。

## <a name="configure-the-site-to-use-mvc"></a>MVC を使用するサイトを構成します。

::: moniker range=">= aspnetcore-2.1"

* .NET Core を対象とする場合、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)既定で参照します。 このパッケージには、MVC アプリでよく使用されるパッケージのパッケージが含まれています。 .NET Framework を対象とする場合、プロジェクト ファイルでのパッケージ参照を個別に追加する必要があります。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* .NET Core を対象とする場合、 [Microsoft.AspNetCore.All メタパッケージ](xref:fundamentals/metapackage)既定で参照します。 このパッケージには、MVC アプリでよく使用されるパッケージのパッケージが含まれています。 .NET Framework を対象とする場合、プロジェクト ファイルでのパッケージ参照を個別に追加する必要があります。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* .NET Core または .NET Framework を対象とするときに MVC アプリでよく使用されるパッケージのパッケージがプロジェクト ファイルに個別に表示されます。

::: moniker-end

`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC フレームワークです。 `Microsoft.AspNetCore.StaticFiles` 静的ファイル ハンドラーです。 ASP.NET Core のランタイムがモジュール化、および静的ファイルを処理するために明示的にオプトインする必要があります (を参照してください[静的ファイル](xref:fundamentals/static-files))。

* 開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。 前述のように、ASP.NET ランタイムは、モジュール式と、静的ファイルを処理するために明示的にオプトインする必要があります。 `UseMvc`拡張メソッドがルーティングを追加します。 詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)と[ルーティング](xref:fundamentals/routing)します。

## <a name="add-a-controller-and-view"></a>コント ローラーとビューを追加します。

このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するビューとビューの次のセクションで移行されますを追加します。

* 追加、*コント ローラー*フォルダー。

* 追加、**コント ローラー クラス**という*HomeController.cs*を*コント ローラー*フォルダー。

![[新しい項目の追加] ダイアログ](mvc/_static/add_mvc_ctl.png)

* 追加、*ビュー*フォルダー。

* 追加、*ビュー/ホーム*フォルダー。

* 追加、 **Razor ビュー**という*Index.cshtml*を*ビュー/ホーム*フォルダー。

![[新しい項目の追加] ダイアログ](mvc/_static/view.png)

プロジェクトの構造は、以下に示します。

![WebApp1 のファイルとフォルダーを表示したソリューション エクスプ ローラー](mvc/_static/project-structure-controller-view.png)

内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。

```html
<h1>Hello world!</h1>
```

アプリを実行します。

![Microsoft Edge で開いている web アプリ](mvc/_static/hello-world.png)

参照してください[コント ローラー](xref:mvc/controllers/actions)と[ビュー](xref:mvc/views/overview)詳細についてはします。

最小限の作業用 ASP.NET Core プロジェクトが作成できた、ASP.NET MVC プロジェクトから機能の移行が始めることができます。 次に移動する必要があります。

* クライアント側のコンテンツ (CSS、フォント、およびスクリプト)

* コントローラー

* ビュー

* モデル

* バンドル

* フィルター

* 入力/出力のログ、Identity (これは次のチュートリアルです。)

## <a name="controllers-and-views"></a>コント ローラーとビュー

* ASP.NET MVC からの各メソッドをコピー`HomeController`を新しい`HomeController`します。 ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); ASP.NET Core MVC、アクション メソッドの戻り値で`IActionResult`代わりにします。 `ActionResult` 実装`IActionResult`では、アクション メソッドの戻り値の型を変更する必要はありません。

* コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。

* ASP.NET Core アプリを実行し、各メソッドをテストします。 いない移行ファイルのレイアウトやスタイル、まだ、レンダリングされるビューには、ビュー ファイルのコンテンツのみが含まれるようにします。 リンクをレイアウト ファイルを生成する必要はありません、`About`と`Contact`ビュー、ブラウザーから呼び出すことがあるありますので (置換**4492**プロジェクトで使用されるポート番号を使用)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

スタイルとメニュー項目がないことに注意してください。 次のセクションでこれを修正します。

## <a name="static-content"></a>静的コンテンツ

ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが個です。 ASP.NET Core で静的コンテンツがホストされている、 *wwwroot*フォルダー。 以前の ASP.NET MVC アプリから静的コンテンツをコピーする必要あります、 *wwwroot* ASP.NET Core プロジェクトのフォルダー。 このサンプルの変換。

* コピー、*追加すると*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクトのフォルダー。

ASP.NET MVC、古いプロジェクトは[ブートス トラップ](https://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダー。 テンプレートで、以前の ASP.NET MVC プロジェクトを生成するには、これを参照して、レイアウト ファイルでブートス トラップ (*Views/Shared/_Layout.cshtml*)。 コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*新しいプロジェクトのフォルダー。 代わりに、次のセクションで Cdn を使用して、ブートス トラップのサポート (およびその他のクライアント側ライブラリ) を追加します。

## <a name="migrate-the-layout-file"></a>レイアウト ファイルを移行します。

* コピー、 *_ViewStart.cshtml*ファイルから以前の ASP.NET MVC プロジェクトの*ビュー*に ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダー。 *_ViewStart.cshtml* ASP.NET Core MVC でファイルが変更されていません。

* 作成、 *Views/shared*フォルダー。

* *省略可能。* コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー*に ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダー。 任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。 *_ViewImports.cshtml*ファイルは、すべてのビュー ファイルの名前空間を提供しに移行する[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)します。 タグ ヘルパーは、新しいレイアウト ファイルで使用されます。 *_ViewImports.cshtml*ファイルは ASP.NET Core の新機能です。

* コピー、 *_Layout.cshtml*ファイルから以前の ASP.NET MVC プロジェクトの*Views/shared*に ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダー。

開いている *_Layout.cshtml*ファイルを開き、次の変更 (完成したコードは下記参照)。

* 置換`@Styles.Render("~/Content/css")`で、`<link>`要素を読み込む*bootstrap.css* (下記参照)。

* `@Scripts.Render("~/bundles/modernizr")` を削除します。

* コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。 詳細については、次を参照してください[認証の移行と ASP.NET core Id。](xref:migration/identity)

* 置換`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。

* 置換`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照)。

ブートス トラップ CSS 含める置換マークアップ:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

JQuery と JavaScript のブートス トラップの包含の置換マークアップ:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

更新された *_Layout.cshtml*ファイルを次に示します。

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

ブラウザーでサイトを表示します。 それを場所に必要なスタイルを使用して正しく読み込むようになりましたする必要があります。

* *省略可能。* 新しいレイアウト ファイルを使用してお試しくださいする可能性があります。 このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。 新しいレイアウト ファイルを使用して[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)あり、その他の改善。

## <a name="configure-bundling-and-minification"></a>バンドルと縮小を構成します。

バンドルと縮小を構成する方法については、次を参照してください。[バンドルと縮小](../client-side/bundling-and-minification.md)します。

## <a name="solve-http-500-errors"></a>HTTP 500 エラーを解決します。

多くの問題、HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報を含まないがあります。 たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。 既定では、ASP.NET Core アプリで、`UseDeveloperExceptionPage`に拡張子を追加、`IApplicationBuilder`構成が実行されると*開発*します。 これは、次のコードの詳細を示します。

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core は、web アプリで未処理の例外を HTTP 500 エラー応答に変換します。 通常、エラーの詳細は、サーバーに関する機密情報の漏えいを防ぐためにこれらの応答に含まれません。 参照してください**開発者例外ページを使用して**で[エラー処理](../fundamentals/error-handling.md)詳細についてはします。

## <a name="additional-resources"></a>その他の技術情報

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
