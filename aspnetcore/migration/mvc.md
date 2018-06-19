---
title: ASP.NET MVC から ASP.NET Core MVC への移行します。
author: ardalis
description: ASP.NET Core MVC に ASP.NET MVC プロジェクトを移行する作業を開始する方法を説明します。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851028"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>ASP.NET MVC から ASP.NET Core MVC への移行します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)、 [Steve Smith](https://ardalis.com/)、および[Scott Addie](https://scottaddie.com)

この記事に ASP.NET MVC プロジェクトを移行する作業を開始する方法を示しています。 [ASP.NET Core MVC](../mvc/overview.md)です。 プロセスでは、ASP.NET MVC から変更されたことの多くを強調表示します。 複数段階のプロセスは、ASP.NET MVC を移行して、この記事は、初期セットアップ、基本的なコント ローラーとビュー、静的なコンテンツ、およびクライアント側の依存関係について説明します。 その他の記事では、構成を移行して多くの ASP.NET MVC プロジェクトで見つかった id コードについて説明します。

> [!NOTE]
> サンプルのバージョン番号が最新でない可能性があります。 したがって、プロジェクトを更新する必要があります。

## <a name="create-the-starter-aspnet-mvc-project"></a>スターター ASP.NET MVC プロジェクトを作成します。

アップグレードを示すためには、ASP.NET MVC アプリケーションを作成することで始めます。 名前を使用して作成*WebApp1*ので、名前空間を作成する、次の手順に ASP.NET Core プロジェクトに一致します。

![Visual Studio の新しいプロジェクト ダイアログ ボックス](mvc/_static/new-project.png)

![新しい Web アプリケーション ダイアログ: MVC プロジェクト テンプレートがテンプレートの ASP.NET のパネルで選択されています。](mvc/_static/new-project-select-mvc-template.png)

*省略可能:* からソリューションの名前を変更*WebApp1*に*Mvc5*です。 Visual Studio には、新しいソリューション名が表示されます (*Mvc5*)、簡単に次のプロジェクトからこのプロジェクトに指示します。

## <a name="create-the-aspnet-core-project"></a>ASP.NET Core プロジェクトを作成します。

新規作成*空*前のプロジェクトと同じ名前での ASP.NET Core web アプリ (*WebApp1*) ため、2 つのプロジェクトの名前空間と一致します。 同じ名前空間を持つと、2 つのプロジェクト間でコードをコピーしやすきます。 同じ名前を使用する前のプロジェクトとは異なるディレクトリでこのプロジェクトを作成する必要があります。

![[新しいプロジェクト] ダイアログ](mvc/_static/new_core.png)

![新しい ASP.NET Web アプリケーション ダイアログ ボックス: ASP.NET Core テンプレート パネルで選択した空のプロジェクト テンプレート](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *省略可能:* 新しい ASP.NET Core アプリケーションを使用して、作成、 *Web アプリケーション*プロジェクト テンプレート。 プロジェクトに名前を*WebApp1*、認証オプションを選択して**個々 のユーザー アカウント**です。 このアプリの名前を変更*FullAspNetCore*です。 変換では、このプロジェクト、時間の節約を作成します。 最終的な結果を表示する、または変換プロジェクトにコードをコピーするテンプレートによって生成されたコードを調べることができます。 テンプレートによって生成されたプロジェクトと比較するへの変換ステップに行き詰まった場合にも便利です。

## <a name="configure-the-site-to-use-mvc"></a>MVC を使用するサイトを構成します。

* .NET Core をターゲットとすると、ASP.NET Core metapackage と呼ばれる、プロジェクトに追加`Microsoft.AspNetCore.All`既定です。 このパッケージを含むようにパッケージ`Microsoft.AspNetCore.Mvc`と`Microsoft.AspNetCore.StaticFiles`です。 .NET Framework を対象にする場合、パッケージの参照は *.csproj ファイルに個別に一覧表示する必要があります。

`Microsoft.AspNetCore.Mvc` ASP.NET Core MVC フレームワークです。 `Microsoft.AspNetCore.StaticFiles` 静的ファイル ハンドラーです。 ASP.NET Core ランタイムがモジュール、および静的なファイルが機能するように明示的にオプトインする必要があります (を参照してください[静的ファイル](xref:fundamentals/static-files))。

* 開く、 *Startup.cs*ファイルを開き、次の一致するようにコードを変更します。

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

`UseStaticFiles`拡張メソッドが静的ファイル ハンドラーを追加します。 前述のように、ASP.NET ランタイムは、モジュール、および静的なファイルが機能するように明示的にオプトインする必要があります。 `UseMvc`ルーティング拡張メソッドを追加します。 詳細については、次を参照してください。[アプリケーションの起動](xref:fundamentals/startup)と[ルーティング](xref:fundamentals/routing)です。

## <a name="add-a-controller-and-view"></a>コント ローラーとビューを追加します。

このセクションでは、最小限のコント ローラーと ASP.NET MVC コント ローラーのプレース ホルダーとして機能するようにビューとビューの次のセクションで移行しますを追加します。

* 追加、*コント ローラー*フォルダーです。

* 追加、**コント ローラー クラス**という*HomeController.cs*を*コント ローラー*フォルダーです。

![[新しい項目の追加] ダイアログ](mvc/_static/add_mvc_ctl.png)

* 追加、*ビュー*フォルダーです。

* 追加、*ビュー/ホーム*フォルダーです。

* 追加、 **Razor ビュー**という*Index.cshtml*を*ビュー/ホーム*フォルダーです。

![[新しい項目の追加] ダイアログ](mvc/_static/view.png)

プロジェクトの構造を次に示します。

![ソリューション エクスプ ローラー WebApp1 のファイルとフォルダーの表示](mvc/_static/project-structure-controller-view.png)

内容を置き換える、 *Views/Home/Index.cshtml*を次のファイル。

```html
<h1>Hello world!</h1>
```

アプリを実行します。

![Web アプリを Microsoft Edge で開いた状態](mvc/_static/hello-world.png)

参照してください[コント ローラー](xref:mvc/controllers/actions)と[ビュー](xref:mvc/views/overview)詳細についてはします。

最小限の作業用 ASP.NET Core プロジェクトができました、ASP.NET MVC プロジェクトから機能の移行が始めることができます。 次に移動する必要があります。

* クライアント側のコンテンツ (CSS、フォント、およびスクリプト)

* コントローラー

* ビュー

* モデル

* バンドル化

* フィルター

* ログの/out、Id (これは、次のチュートリアルでします。)

## <a name="controllers-and-views"></a>コント ローラーとビュー

* ASP.NET MVC のコピーの各メソッド`HomeController`を新しい`HomeController`です。 ASP.NET MVC では、組み込みのテンプレートのコント ローラー アクション メソッドの戻り値型は[ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx)以外の場合は ASP.NET Core MVC、アクション メソッドの戻り値の`IActionResult`代わりにします。 `ActionResult` 実装する`IActionResult`ので、アクション メソッドの戻り値の型を変更する必要はありません。

* コピー、 *About.cshtml*、 *Contact.cshtml*、および*Index.cshtml* Razor ビューのファイルを ASP.NET MVC プロジェクトから ASP.NET Core プロジェクト。

* ASP.NET Core アプリケーションを実行し、各メソッドをテストします。 いない移行ファイルのレイアウトやスタイルまだ、レンダリングされるビューには、ファイルの表示内のコンテンツのみが含まれるようにします。 レイアウトに生成されたファイルへのリンクする必要はありません、`About`と`Contact`ビュー、ブラウザーからそれらを起動する必要がありますので (交換**4492**プロジェクトで使用されるポート番号を持つ)。

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![連絡先のページ](mvc/_static/contact-page.png)

スタイル設定およびメニュー項目がないことに注意してください。 次のセクションでこれを修正します。

## <a name="static-content"></a>静的コンテンツ

ASP.NET MVC の以前のバージョンでは、静的コンテンツは、web プロジェクトのルートからホストされているし、サーバー側のファイルが混合されました。 ASP.NET Core での静的なコンテンツをホスト、 *wwwroot*フォルダーです。 古い ASP.NET MVC のアプリから静的なコンテンツをコピーしておく、 *wwwroot* ASP.NET Core プロジェクトのフォルダーにします。 このサンプルの変換。

* コピー、 *favicon.ico*に古い MVC プロジェクトのファイル、 *wwwroot* ASP.NET Core プロジェクト フォルダーにします。

古い ASP.NET MVC プロジェクトの使用[ブートス トラップ](https://getbootstrap.com/)にファイルをブートス トラップのスタイルとストアの*コンテンツ*と*スクリプト*フォルダーです。 以前の ASP.NET MVC プロジェクトを生成するには、テンプレートを参照して、レイアウト ファイル ブートス トラップ (*Views/Shared/_Layout.cshtml*)。 コピーすることで、 *bootstrap.js*と*bootstrap.css*ファイルから ASP.NET MVC プロジェクトを*wwwroot*新しいプロジェクトのフォルダーです。 代わりに、次のセクションで Cdn を使用してブートス トラップのサポート (およびその他のクライアント側ライブラリ) を追加します。

## <a name="migrate-the-layout-file"></a>レイアウト ファイルを移行します。

* コピー、*は _viewstart.vbhtml*古い ASP.NET MVC プロジェクトのファイルの*ビュー* ASP.NET Core プロジェクトのフォルダー*ビュー*フォルダーです。 *は _viewstart.vbhtml* ASP.NET Core MVC でファイルが変更されていません。

* 作成、 *Views/shared*フォルダーです。

* *省略可能:* コピー *_ViewImports.cshtml*から、 *FullAspNetCore* MVC プロジェクトの*ビュー* ASP.NET Core プロジェクトのフォルダー *ビュー*フォルダーです。 任意の名前空間宣言を削除、 *_ViewImports.cshtml*ファイル。 *_ViewImports.cshtml*ファイル ビューのすべてのファイルの名前空間を提供しでによってもたらさ[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)です。 タグ ヘルパーは、新しいレイアウト ファイルで使用されます。 *_ViewImports.cshtml*ファイルが ASP.NET Core の新機能です。

* コピー、 *_Layout.cshtml*古い ASP.NET MVC プロジェクトのファイルの*Views/shared* ASP.NET Core プロジェクトのフォルダー *Views/shared*フォルダーです。

開いている *_Layout.cshtml*ファイルし、次の変更 (完了したコードは、以下のとおりです)。

* 置き換える`@Styles.Render("~/Content/css")`で、`<link>`読み込み要素*bootstrap.css* (下記参照)。

* `@Scripts.Render("~/bundles/modernizr")` を削除します。

* コメント アウト、`@Html.Partial("_LoginPartial")`行 (では、行を囲む`@*...*@`)。 今後のチュートリアルでを返します。

* 置き換える`@Scripts.Render("~/bundles/jquery")`で、`<script>`要素 (下記参照)。

* 置き換える`@Scripts.Render("~/bundles/bootstrap")`で、`<script>`要素 (下記参照)。

ブートス トラップ CSS 含める置換マークアップ:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

交換用のマークアップ jQuery とブートス トラップの JavaScript の包含を:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

更新された *_Layout.cshtml*ファイルを次に示します。

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

ブラウザーで、サイトを表示します。 これを場所に必要なスタイルを使用して正しく読み込むようになりました必要があります。

* *省略可能:* 新しいレイアウト ファイルを使用して再試行することができます。 このプロジェクトからレイアウト ファイルをコピーすることができます、 *FullAspNetCore*プロジェクト。 新しいレイアウト ファイルを使用して[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)あり、その他の改善。

## <a name="configure-bundling-and-minification"></a>バンドルと縮小を構成します。

バンドルと縮小を構成する方法については、次を参照してください。 [Bundling and Minification](../client-side/bundling-and-minification.md)です。

## <a name="solve-http-500-errors"></a>HTTP 500 エラーを解決します。

多くの問題 HTTP 500 エラー メッセージを引き起こす可能性のある問題の原因に関する情報が含まれていないことがあります。 たとえば場合、 *Views/_ViewImports.cshtml*ファイルがプロジェクトに存在しない名前空間を含む、HTTP 500 エラーが表示されます。 ASP.NET Core アプリの場合は、既定では、`UseDeveloperExceptionPage`に拡張子を追加、`IApplicationBuilder`構成時に実行および*開発*です。 これは、次のコードの詳細を示します。

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core は、web アプリで未処理の例外を HTTP 500 エラー応答に変換します。 通常、エラーの詳細は、サーバーの機密性の高い情報の漏えいを防ぐためにこれらの応答に含まれません。 参照してください**開発者例外ページを使用して**で[エラーの処理](../fundamentals/error-handling.md)詳細についてはします。

## <a name="additional-resources"></a>その他の技術情報

* [クライアント側の開発](xref:client-side/index)
* [タグ ヘルパー](xref:mvc/views/tag-helpers/intro)
