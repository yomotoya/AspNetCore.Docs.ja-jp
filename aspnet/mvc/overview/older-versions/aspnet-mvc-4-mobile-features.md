---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: "ASP.NET MVC 4 のモバイル機能 |Microsoft ドキュメント"
author: Rick-Anderson
description: "ASP.NET MVC 5 モバイル Web アプリケーションでは、Azure Web サイトの展開でのコード サンプルでは、このチュートリアルの MVC 5 のバージョンがようになりました。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: e660595d66d81069fa47b77387509e73b1ec834e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 モバイル機能
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでのコード サンプルでの MVC 5 のバージョンがないようになりました[ASP.NET MVC 5 モバイル Web アプリケーションでは、Azure Web サイトを展開](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)です。


このチュートリアルでは、ASP.NET MVC 4 Web アプリケーションでのモバイル機能を使用する方法の基本を説明します。 このチュートリアルで行うこともできます[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express)または Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer または VWD&quot;)。 既にある場合は、プロフェッショナルなバージョンの Visual Studio を使用できます。

開始する前に、以下に示す前提条件がインストールされていることを確認してください。

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express) (推奨) または Visual Studio Web Developer Express SP1。 Visual Studio 2012 には、ASP.NET MVC 4 が含まれています。 Visual Web Developer 2010 を使用している場合は、インストールする必要あります[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)です。

モバイル ブラウザー エミュレーターも必要になります。 次のいずれか、動作します。

- [Windows 7 Phone エミュレーター](https://msdn.microsoft.com/en-us/library/ff402563(VS.92).aspx)です。 (これは、このチュートリアルでは、スクリーン ショットのほとんどで使用されているエミュレーターです)。
- IPhone をエミュレートするためにユーザー エージェント文字列を変更します。 参照してください[この](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)ブログ エントリです。
- [元の opera Mobile エミュレーター](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/)ユーザー エージェント iPhone に設定します。 Safari で、ユーザー エージェントを"iPhone"に設定する方法については、次を参照してください。 [IE が見かけ上 Safari させる方法について](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison ブログ。

C# ソース コードの visual Studio プロジェクトは、このトピックを使用できます。

- [スタート プロジェクトのダウンロード](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [プロジェクトのダウンロードが完了しました](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>新機能のビルドします。

このチュートリアルでは、モバイルの機能を追加会議一覧の簡単なアプリケーションで提供される、[スタート プロジェクト](https://go.microsoft.com/fwlink/?LinkId=228307)です。 次のスクリーン ショットに示すように、完成したアプリケーションの [タグ] ページを示しています、 [Windows 7 Phone エミュレーター](https://msdn.microsoft.com/en-us/library/ff402563(VS.92).aspx)です。 参照してください[キーボード マッピングの Windows Phone エミュレーター](https://msdn.microsoft.com/en-us/library/ff754352(v=vs.92).aspx)をキーボード入力を簡略化します。

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Internet Explorer バージョン 9、または 10 に設定して、モバイル アプリケーションを開発するには、FireFox または Chrome を使用することができます、[ユーザー エージェント文字列](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)です。 次の図は、iPhone をエミュレートする Internet Explorer を使用して、完了したチュートリアルを示します。 Internet Explorer F-12 開発者ツールを使用することができます、 [Fiddler ツール](http://www.fiddler2.com/fiddler2/)アプリケーションをデバッグする際にします。

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>スキルの学習

学習する内容を次に示します。

- ASP.NET MVC 4 のテンプレートが HTML5 に使用して`viewport`属性とアダプティブ レンダリング向上させるためにモバイル デバイスに表示します。
- Mobile に固有のビューを作成する方法。
- ビュー スイッチャーにそのによりユーザーを切り替えるモバイル ビューと、アプリケーションのデスクトップ ビューを作成する方法。

### <a name="getting-started"></a>作業の開始

会議リスト アプリケーションをダウンロードして、次のリンクを使用して初期プロジェクト:[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=228307)です。 Windows エクスプ ローラーで右クリック、 *MvcMobile.zip*ファイルして選択**プロパティ**です。 **MvcMobile.zip プロパティ** ダイアログ ボックスで、選択、**ブロックを解除する**ボタンをクリックします。 (ブロック解除により、セキュリティの警告を使用しようとしたときに発生する、 *.zip* web からダウンロードしたファイルです)。

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

右クリックし、 *MvcMobile.zip*ファイルおよび選択した**すべて展開**ファイルを解凍します。 Visual Studio で開く、 *MvcMobile.sln*ファイル。

CTRL + f5 キーを押してデスクトップ ブラウザーで表示されるアプリケーションを実行します。 モバイル ブラウザー エミュレーターを起動し、エミュレーターに会議アプリケーションの URL をコピーして、をクリックして、**タグによってブラウズ**リンクします。 Windows Phone エミュレーターを使用している場合の URL バーをクリックし、一時停止キーを押してキーボード アクセスを取得します。 下の例のイメージ、 *AllTags*ビュー (選択**タグによってブラウズ**)。

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

表示は、モバイル デバイスで非常に読み取り可能です。 ASP.NET は、リンクを選択します。

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET タグ表示は非常に混雑です。 たとえば、**日付**列は読み取りを非常に困難です。 このチュートリアルで後ほどのバージョンを作成します、 *AllTags*モバイル ブラウザーについて具体的には、読み取り可能な表示を構成するはビューです。

注: バグに現在存在するモバイル キャッシュ エンジン。 実稼働アプリケーションのインストールする必要があります、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes)注目パッケージです。 参照してください[ASP.NET MVC 4 Mobile キャッシュ バグ固定](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。

## <a name="css-media-queries"></a>CSS のメディア クエリ

[CSS のメディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)メディアの種類の CSS を拡張します。 これらによって、特定のブラウザー (ユーザー エージェント) の既定の CSS 規則を上書きするルールを作成できます。 モバイル ブラウザーを対象とする CSS の一般的なルールでは、画面の最大サイズを定義します。 *Content\Site.css*新しい ASP.NET MVC 4 インターネット プロジェクトを作成するときに作成されるファイルには、次のメディア クエリが含まれています。

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

ブラウザー ウィンドウが 850 ピクセル以下の場合は、このメディア ブロック内で CSS ルールが使用されます。 このような CSS メディア クエリを使用するよう設計されている既定の CSS 規則よりも (モバイル ブラウザーなど) の小さなブラウザーに HTML コンテンツをよりわかりやすく表示をデスクトップ ブラウザーの幅の表示を提供します。

## <a name="the-viewport-meta-tag"></a>ビューポートのメタ タグ

ほとんどのモバイル ブラウザー仮想ブラウザー ウィンドウの幅を定義する (、*ビューポート*) は、モバイル デバイスの実際の幅よりも大きくします。 これにより、仮想の表示内の web ページ全体に合わせてモバイル ブラウザー。 ユーザーは、興味深い内容でにズームできます。 ただし、実際のデバイスの幅をビューポートの幅を設定する場合は、ズームは必要ありませんモバイル ブラウザーで、コンテンツを収めるためです。

ビューポート`<meta>`ASP.NET MVC 4 レイアウト ファイル内のタグは、デバイスの幅に、ビューポートを設定します。 次の行は、ビューポートを示しています。 `<meta>` ASP.NET MVC 4 レイアウト ファイル内のタグ。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>CSS のメディア クエリと、ビューポートのメタ タグの影響を確認します。

開く、 *\shared\\_Layout.cshtml*をコメント アウト、ビューポート、エディターでファイル`<meta>`タグ。 次のマークアップは、コメント アウトされた行を示しています。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

開く、 *MvcMobile\Content\Site.css*エディターでファイルを開き 0 ピクセルにメディア クエリで最大の幅を変更します。 これは、ため、CSS 規則はモバイル ブラウザーで使用されているできなくなります。 次の行は、変更されたメディア クエリを示しています。

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

変更を保存し、モバイル ブラウザー エミュレーターで会議アプリケーションを参照します。 ビューポートを削除するは、次の図の小さなテキスト`<meta>`タグ。 ありませんビューポートと`<meta>`タグ、ブラウザーがズーム アウトする既定のビューポートの幅 (850 ピクセルのほとんどのモバイル ブラウザーで広くしたりします)。

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

変更を取り消して、-、ビューポートのコメントを解除`<meta>`レイアウト ファイルにタグを付けるし、850 ピクセルにメディア クエリを復元、 *Site.css*ファイル。 変更内容を保存し、モバイル対応の表示が復元されていることを確認するモバイル ブラウザーを更新します。

ビューポート`<meta>`タグと CSS のメディア クエリは ASP.NET MVC 4 に固有ではないと、任意の web アプリケーションこれらの機能のメリットを利用することができます。 それらは、新しい ASP.NET MVC 4 プロジェクトを作成するときに生成されるファイルに組み込まれてようになりました。

ビューポートの詳細については`<meta>`タグは、「 [A tale 2 つのビューポートの-パート 2](http://www.quirksmode.org/mobile/viewports2.html)です。

次のセクションでは、mobile ブラウザーの特定のビューを提供する方法が表示されます。

## <a name="overriding-views-layouts-and-partial-views"></a>ビュー、レイアウト、および部分ビューをオーバーライドします。

ASP.NET MVC 4 において重要な新しい機能は、オーバーライドできます (レイアウトや部分ビューを含む) の任意のビューの個々 のモバイル ブラウザーでの一般に、または、特定のブラウザーのモバイル ブラウザーでを単純なメカニズムです。 Mobile に固有のビューを提供するには、ビュー ファイルをコピーし、追加することができます*です。Mobile*ファイル名にします。 たとえば、モバイルを作成する*インデックス*ビューで、コピー *Views\Home\Index.cshtml*に*Views\Home\Index.Mobile.cshtml*です。

このセクションでは、mobile に固有のレイアウト ファイルを作成します。

を開始するコピー *\shared\\_Layout.cshtml*に*\shared\\_Layout.Mobile.cshtml*です。 開いている *\_Layout.Mobile.cshtml*からタイトルを変更**MVC4 会議**に**会議 (モバイル)**です。

各`Html.ActionLink`呼び出し、それぞれのリンクに「によって参照」を削除*ActionLink*です。 次のコードは、モバイルのレイアウト ファイルの完了の body セクションを示しています。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

コピー、 *Views\Home\AllTags.cshtml*ファイルの名前を*Views\Home\AllTags.Mobile.cshtml*です。 新しいファイルを開き、変更、 `<h2>` "Tags"からの要素に"タグ (M)"。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

デスクトップのブラウザーを使用し、モバイル ブラウザーのエミュレーターを使用して、タグ ページを参照します。 モバイル ブラウザー エミュレーターは、次の 2 つの変更を示しています。

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

これに対し、デスクトップの表示は変更されていません。

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>ブラウザー固有のビュー

モバイルおよびデスクトップに固有のビューだけでなく個々 のブラウザーのビューを作成できます。 たとえば、iPhone ブラウザーのための特別なビューを作成できます。 ここでの iPhone ブラウザーと iPhone のバージョンのレイアウトを作成、 *AllTags*ビュー。

開く、 *Global.asax*ファイルし、次のコードを追加、`Application_Start`メソッドです。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

このコードでは、各入力方向の要求と一致する"iPhone"という名前の新しい表示モードを定義します。 受信要求には、(ユーザー エージェントには、文字列"iPhone"が含まれている) 場合は、定義した条件が一致すると、ASP.NET MVC ビューの名前持つにはには、"iPhone"サフィックスが含まれています。 検索されます。

コードを右クリックして`DefaultDisplayMode`、選択**解決**を選択し`using System.Web.WebPages;`です。 参照を追加、`System.Web.WebPages`が名前空間、`DisplayModes`と`DefaultDisplayMode`型が定義されています。

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

次の行を手動で追加する代わりに、`using`ファイルのセクションです。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

完全な内容、 *Global.asax*ファイルを次に示します。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

変更を保存します。 コピー、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルの名前を*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*です。 新しいファイルを開き、変更、`h1`から見出し`Conference (Mobile)`に`Conference (iPhone)`です。

コピー、 *MvcMobile\Views\Home\AllTags.Mobile.cshtml*ファイルの名前を*MvcMobile\Views\Home\AllTags.iPhone.cshtml*です。 新しいファイルで、変更、`<h2>`要素から"タグ (M)"を「タグ (iPhone)」です。

アプリケーションを実行します。 モバイル ブラウザー エミュレーターの実行でそのユーザー エージェントが"iPhone"に設定されていることを確認しを参照、 *AllTags*ビュー。 次のスクリーン ショットでは、 *AllTags*でビューが表示される、 [Safari](http://www.apple.com/safari/download/)ブラウザー。 Safari for Windows をダウンロードする[ここ](https://support.apple.com/kb/DL1531)です。

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

このセクションのレイアウトや iPhone などの特定のデバイス用のビューを作成する方法と、モバイルのレイアウトやビューを作成する方法を説明しました。 次のセクションでは、説得力のある他のモバイル ビューの jQuery Mobile を活用する方法が表示されます。

## <a name="using-jquery-mobile"></a>JQuery Mobile を使用します。

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)ライブラリには、すべての主要なモバイル ブラウザーでも機能するユーザー インターフェイスのフレームワークが用意されています。 jQuery Mobile 適用*段階的な強化*CSS および JavaScript をサポートするモバイル ブラウザー。 段階的な強化より強力なブラウザーおよびデバイスが豊富な表示にしながら、web ページの基本的な内容を表示するすべてのブラウザーを許可します。 JavaScript および CSS ファイル jQuery Mobile に含まれているモバイル ブラウザーをマークアップ変更を加えずに適合するさまざまな要素のスタイルを設定します。

このセクションでをインストールし、 *jQuery.Mobile.MVC* NuGet パッケージは、jQuery Mobile、およびビュー スイッチャー ウィジェットをインストールします。

開始するには、削除、 *Shared\\_Layout.Mobile.cshtml*と*Shared\\_Layout.iPhone.cshtml*先ほど作成したファイルです。

名前を変更*Views\Home\AllTags.Mobile.cshtml*と*Views\Home\AllTags.iPhone.cshtml*ファイルを*Views\Home\AllTags.iPhone.cshtml.hide*と*Views\Home\AllTags.Mobile.cshtml.hide*です。 ファイルが必要がなくなるので、 *.cshtml*拡張機能、表示するために、ASP.NET MVC ランタイムによって使用されない、 *AllTags*ビュー。

インストール、 *jQuery.Mobile.MVC*これにより、NuGet パッケージ。

1. **ツール**メニューの **ライブラリ パッケージ マネージャー**、し、 **Package Manager Console**です。

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. **Package Manager Console**を入力してください`Install-Package jQuery.Mobile.MVC -version 1.0.0`

次の図は、追加および NuGet jQuery.Mobile.MVC パッケージによって MvcMobile プロジェクトに変更したファイルを示しています。 ファイル名の後に追加された [追加] が追加されるファイルです。 イメージは、GIF を表示しないおよび PNG ファイルに追加、 *Content\images*フォルダーです。

![](aspnet-mvc-4-mobile-features/_static/image21.png)

次に、jQuery.Mobile.MVC NuGet パッケージがインストールされます。

- *アプリ\_Start\BundleMobileConfig.cs*追加 jQuery JavaScript および CSS ファイルを参照するために必要なファイルです。 次の手順に従ってし、このファイルで定義されているモバイルのバンドルを参照する必要があります。
- jQuery Mobile CSS ファイル。
- A`ViewSwitcher`ウィジェットのコント ローラー (*Controllers\ViewSwitcherController.cs*)。
- jQuery モバイルの JavaScript ファイル。
- JQuery Mobile スタイル レイアウト ファイル (*\shared\\_Layout.Mobile.cshtml*)。
- ビュー スイッチャー部分ビュー *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) モバイル ビューおよびその逆のデスクトップの表示からを切り替えるには、各ページの上部にあるリンクを提供します。
- いくつか*.png*と*.gif*イメージ ファイル、 *Content\images*フォルダーです。

開く、 *Global.asax*ファイルし、の最後の行として次のコードを追加、`Application_Start`メソッドです。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

次のコードは、完全な*Global.asax*ファイル。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Internet Explorer 9 を使用しているし、表示されない場合、`BundleMobileConfig`黄色のハイライト上線をクリックして、[互換表示 ボタン](https://windows.microsoft.com/en-US/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")アウトラインから変更アイコン IE で![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")を純色![(on) 互換表示 ボタンの画像](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 互換表示 ボタンの画像")します。 また FireFox または Chrome で、このチュートリアルを参照できます。


開く、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルし、次のマークアップを直接後の追加、`Html.Partial`呼び出し。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

完全な*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルを次に示します。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

アプリケーションをビルドし、モバイル ブラウザー エミュレーターを参照、 *AllTags*ビュー。 次を参照してください。

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> モバイルの特定のコードをデバッグする[ユーザー エージェント文字列を設定](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE または iPhone および F-12 開発者ツールを使用するには Chrome です。 モバイル ブラウザーで表示されない場合は、**ホーム**、**スピーカー**、**タグ**、および**日付**ボタンとしてリンク、jQuery Mobile への参照スクリプトおよび CSS ファイルが正しくない可能性があります。


表示スタイルの変更だけでなく**モバイル ビューを表示する**およびモバイル ビューからデスクトップ ビューに切り替えることができますをリンクします。 選択、**デスクトップ ビュー**リンク、およびデスクトップ ビューが表示されます。

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

デスクトップのビューには、モバイル ビューに直接移動する方法を提供しません。 修正するようになりました。 開く、 *\shared\\_Layout.cshtml*ファイル。 ページのすぐ下`body`要素ビュー スイッチャー ウィジェットを表示する次のコードを追加します。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

更新、 *AllTags*モバイル ブラウザーで表示します。 デスクトップおよびモバイル ビュー間を移動するようになりました。

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> 注のデバッグ: \shared の末尾に次のコードを追加することができます\\_ViewSwitcher.cshtml ブラウザーのユーザー エージェント文字列を使用してモバイル デバイスに設定すると、ビューをデバッグする際にします。
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  次の見出しを追加して、 *\shared\\_Layout.cshtml*ファイル。  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


参照、 *AllTags*デスクトップ ブラウザーでページ。 モバイルのレイアウト ページにのみ追加されるため、ビュー スイッチャー ウィジェットはデスクトップ ブラウザーで表示されません。 このチュートリアルで後ほど、デスクトップ ビューに、ビュー スイッチャー ウィジェットを追加する方法が表示されます。

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>スピーカーのリストを向上させる

モバイル ブラウザーで、選択、**スピーカー**リンクします。 モバイル ビューがないため (*AllSpeakers.Mobile.cshtml*)、既定のスピーカーの表示 (*AllSpeakers.cshtml*)、モバイルのレイアウト ビューを使用してレンダリング ( *\_Layout.Mobile.cshtml*)。

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

設定してモバイル レイアウト内の表示から既定の (非モバイル) ビューを無効にすることができますグローバル`RequireConsistentDisplayMode`に`true`で、*ビュー\\は _viewstart.vbhtml*次のように、ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

ときに`RequireConsistentDisplayMode`に設定されている`true`、モバイルのレイアウト (*\_Layout.Mobile.cshtml*) はモバイル ビューに対してのみ使用します。 (そのファイルの表示は、フォームの ***ViewName**です。Mobile.cshtml*)。設定することができます`RequireConsistentDisplayMode`に`true`場合は、非モバイル ビューに対して、モバイルのレイアウトは動作しません。 次のスクリーン ショット方法、*スピーカー*ページが表示されるときに`RequireConsistentDisplayMode`に設定されている`true`です。

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

設定して、ビューの一貫した表示モードを無効にすることができます`RequireConsistentDisplayMode`に`false`ファイルの表示にします。 次のマークアップ、 *Views\Home\AllSpeakers.cshtml*ファイルのセットを`RequireConsistentDisplayMode`に`false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>スピーカーがモバイル ビューを作成します。

説明したとおり、*スピーカー*ビューは読み取り可能なですが、リンクは、小さな、モバイル デバイスをタップするが困難です。 このセクションでモバイル固有の仕様を作成します*スピーカー*ビューを最新のモバイル アプリケーションのようになります: 大規模なが表示されます、tap 簡単なリンクしスピーカーをすばやく検索する検索ボックスが含まれています。

コピー *AllSpeakers.cshtml*に*AllSpeakers.Mobile.cshtml*です。 開く、 *AllSpeakers.Mobile.cshtml*ファイルし、削除、`<h2>`見出し要素。

`<ul>`タグ、追加、`data-role`属性し、その値に設定`listview`です。 などの他の[`data-*`属性](http://html5doctor.com/html5-custom-data-attributes/)、`data-role="listview"`タップする大きなリスト項目を簡単には、します。 これは、完全なマークアップはのようになります。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

モバイル ブラウザーを更新します。 表示を更新は、次のようになります。

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

モバイル ビューが改善はスピーカーの長いリストを移動するが困難です。 これを修正する、`<ul>`タグ、追加、`data-filter`属性に設定し、`true`です。 次のコード、`ul`マークアップ。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

次の図は、検索フィルター ボックスに起因するページの上部にある、`data-filter`属性。

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

検索ボックスに各文字を入力すると jQuery Mobile は、次の図に示すように、表示される一覧をフィルターします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>[タグ] ボックスの向上

既定値と同様に*スピーカー*ビュー、*タグ*ビューが読み取り可能では、リンクは小さく、モバイル デバイスをタップするが困難です。 修正、このセクションで、*タグ*修正したのと同じ方法を表示、*スピーカー*ビュー。

削除、&quot;を非表示に&quot;にサフィックスが付いて、 *Views\Home\AllTags.Mobile.cshtml.hide*ファイルの名前は*Views\Home\AllTags.Mobile.cshtml*です。 名前が変更されたファイルを開き、削除、`<h2>`要素。

追加、`data-role`と`data-filter`属性を`<ul>`タグを次のように。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

次の図は、アルファベットのフィルタ リング タグ ページを示しています`J`です。

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>日付のリストを向上させる

向上できます、*日付*表示を改善する、*スピーカー*と*タグ*ビュー、モバイル デバイスを使用する方が簡単にできるようにします。

コピー、 *Views\Home\AllDates.cshtml*ファイルの名前を*Views\Home\AllDates.Mobile.cshtml*です。 新しいファイルを開き、削除、`<h2>`要素。

追加`data-role="listview"`を`<ul>`タグは、次のようにします。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

次の図が動作を示しています、**日付**ページのように、`data-role`インプレース属性。

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)の内容を置き換える、 *Views\Home\AllDates.Mobile.cshtml*を次のコード ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

このコードでは、指定した日数をすべてのセッションがグループ化します。 新しい日付ごとに、リストの区分線を作成し、1 日に区分線の下のすべてのセッションが一覧表示されます。 このコードを実行すると外観を次に示します。

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>SessionsTable ビューの向上

このセクションでは、セッションの mobile に固有のビューを作成します。 変更を行う場合は、他のビューを作成したよりもより広範なされます。

モバイル ブラウザーで、タップ、**スピーカー**ボタンをクリックし、入力`Sc`検索ボックスにします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

タップして、 **Scott Hanselman**リンクします。

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

ご覧のように、表示はモバイル ブラウザーで読みにくいです。 日付の列が読みにくくなると、タグ列は、表示します。 この問題を解決するにはコピー *Views\Home\SessionsTable.cshtml*に*Views\Home\SessionsTable.Mobile.cshtml*、ファイルの内容を次のコードに置き換えます。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

コードが、ルームを削除し、タグ列、およびすべての情報をモバイル ブラウザーで読み取ることができるように、タイトル、スピーカー、および日の垂直方向に書式します。 次の図には、コードの変更が反映されます。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>SessionByCode ビューの向上

最後のモバイルに固有のビューを作成、 *SessionByCode*ビュー。 モバイル ブラウザーで、タップ、**スピーカー**ボタンをクリックし、入力`Sc`検索ボックスにします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

タップして、 **Scott Hanselman**リンクします。 Scott Hanselman のセッションが表示されます。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

選択、 **of Love の MS Web スタック概要**リンクします。

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

既定のデスクトップ表示ですが、それを向上させることができます。

コピー、 *Views\Home\SessionByCode.cshtml*に*Views\Home\SessionByCode.Mobile.cshtml*の内容を置き換えると、 *Views\Home\SessionByCode.Mobile.cshtml*次のマークアップ ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

新しいマークアップを使用して、`data-role`属性ビューのレイアウトを改善します。

モバイル ブラウザーを更新します。 次の図は、先ほど作成したコードの変更を反映しています。

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>(コース完了) とレビュー

このチュートリアルには、ASP.NET MVC 4 Developer Preview の新しいモバイル機能が導入されました。 モバイルの機能は次のとおりです。

- グローバルと個々 のビューの両方に、レイアウト、ビュー、および部分ビューを上書きする権限です。
- レイアウトと部分的な上書きの適用を使用して制御、`RequireConsistentDisplayMode`プロパティです。
- モバイル ビュー スイッチャー ウィジェット デスクトップ ビューにも表示されるよりもを表示します。
- IPhone ブラウザーなどの特定のブラウザーをサポートするためのサポート。

## <a name="see-also"></a>関連項目

- [jQuery Mobile](http://jquerymobile.com)サイトです。
- [jQuery Mobile の概要](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C Recommendation モバイル Web アプリケーションのベスト プラクティス](http://www.w3.org/TR/mwabp/)
- [W3C Candidate Recommendation メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)
