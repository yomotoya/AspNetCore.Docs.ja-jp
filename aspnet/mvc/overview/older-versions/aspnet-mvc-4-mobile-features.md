---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: ASP.NET MVC 4 モバイル機能 |Microsoft Docs
author: Rick-Anderson
description: 今すぐ展開 ASP.NET MVC 5 モバイル Web アプリケーションを Azure Websites でのコード サンプルで、このチュートリアルの MVC 5 バージョンがないです。
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: c852f4a853d14badb6c9a1c2c1ddb7b069bc3441
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806588"
---
<a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 モバイル機能
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> コード サンプルで、このチュートリアルの MVC 5 バージョンがないようになりました[ASP.NET MVC 5 モバイル Web アプリケーションを Azure Websites をデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/)します。


このチュートリアルでは、ASP.NET MVC 4 Web アプリケーションでモバイル機能を使用する方法の基本を説明します。 このチュートリアルで使用することができます[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)または Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer または VWD&quot;)。 ですが既にある場合は、Visual Studio の professional バージョンを使用できます。

始める前に、以下の前提条件がインストールされていることを確認します。

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (推奨) または Visual Studio Web Developer Express SP1。 Visual Studio 2012 には、ASP.NET MVC 4 が含まれています。 インストールする必要があります Visual Web Developer 2010 を使用している場合[ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)します。

モバイル ブラウザー エミュレーターも必要になります。 次のいずれでも動作します。

- [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)します。 (これは、このチュートリアルでは、スクリーン ショットの大半で使用されているエミュレーターです)。
- IPhone をエミュレートするためにユーザー エージェント文字列を変更します。 参照してください[この](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)ブログ エントリ。
- [Opera Mobile Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/)ユーザー エージェントを iPhone に設定します。 Safari でのユーザー エージェントを"iPhone"に設定する方法については、次を参照してください。 [Safari pretend IE ができるようにする方法](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html)David Alison のブログ。

C# ソース コードを visual Studio プロジェクトをこのトピックと共に使用できます。

- [スタート プロジェクトのダウンロード](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [完成したプロジェクトのダウンロード](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>構築します

このチュートリアルで提供されている単純な会議一覧アプリケーションにモバイル機能を追加します、[スターター プロジェクト](https://go.microsoft.com/fwlink/?LinkId=228307)します。 次のスクリーン ショットに示すように、完成したアプリケーションの [タグ] ページを示しています、 [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx)します。 参照してください[キーボード マッピングの Windows Phone エミュレーター](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx)をキーボード入力を簡略化します。

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Internet Explorer バージョン 9 または 10 を設定して、モバイル アプリケーションを開発するには、FireFox または Chrome を使用することができます、[ユーザー エージェント文字列](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)します。 次の図は、iPhone をエミュレートする Internet Explorer を使用して、完了したチュートリアルを示します。 Internet Explorer F-12 開発者ツールを使用することができます、 [Fiddler ツール](http://www.fiddler2.com/fiddler2/)アプリケーションのデバッグに役立ちます。

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>学習内容

学習内容を次に示します。

- ASP.NET MVC 4 テンプレートでの HTML5 の使用方法`viewport`属性とアダプティブ レンダリング向上させるためには、モバイル デバイスで表示します。
- モバイル専用ビューを作成する方法。
- ビュー スイッチャー モバイル ビューと、アプリケーションのデスクトップ ビューの間そのによりユーザーの切り替えを作成する方法。

### <a name="getting-started"></a>作業の開始

次のリンクを使用して、スターター プロジェクトの会議一覧アプリケーションのダウンロード:[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=228307)します。 Windows エクスプ ローラーで右クリック、 *MvcMobile.zip*ファイル**プロパティ**します。 **MvcMobile.zip プロパティ** ダイアログ ボックスで、選択、**ブロック解除**ボタンをクリックします。 (セキュリティの警告を使用しようとするときに発生するブロックを解除できないように、 *.zip* web からダウンロードしたファイルです)。

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

右クリックし、 *MvcMobile.zip*ファイルおよび選択**すべて展開**ファイルを解凍します。 Visual Studio で開く、 *MvcMobile.sln*ファイル。

デスクトップ ブラウザーで表示すると、アプリケーションを実行するには、CTRL + F5 キーを押します。 モバイル ブラウザー エミュレーターを起動、会議アプリケーションの URL をエミュレーターにコピーおよび をクリックし、**タグによってブラウズ**リンク。 Windows Phone エミュレーターを使用している場合は、URL バーをクリックし、キーボード アクセスするために一時停止キーを押します。 次のイメージ、 *AllTags*ビュー (選択から**タグによってブラウズ**)。

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

表示は、モバイル デバイス上でも読みやすいです。 ASP.NET のリンクを選択します。

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

ASP.NET タグ ビューは、非常に煩雑です。 たとえば、**日付**列が読み取りにくくします。 チュートリアルの後半では、バージョンを作成します、 *AllTags*モバイル ブラウザー専用の読み取り可能な表示を構成するが表示されます。

注: バグに現在存在モバイル キャッシュ エンジン。 実稼働アプリケーションをインストールする必要があります、[固定 DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget パッケージ。 参照してください[ASP.NET MVC 4 モバイル キャッシュ バグ フィックス](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx)詳細については、修正します。

## <a name="css-media-queries"></a>CSS メディア クエリ

[CSS メディア クエリ](http://www.w3.org/TR/css3-mediaqueries/)は CSS メディアの種類の拡張機能。 特定のブラウザー (ユーザー エージェント) の既定の CSS ルールを上書きするルールを作成できます。 モバイル ブラウザーを対象とする CSS の一般的なルールでは、画面の最大サイズを定義します。 *Content\Site.css*新しい ASP.NET MVC 4 のインターネット プロジェクトを作成するときに作成されるファイルには、次のメディア クエリが含まれています。

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

ブラウザー ウィンドウが 850 ピクセル以下の場合は、このメディア ブロック内での CSS 規則が使用されます。 このような CSS メディア クエリを使用すると、デスクトップ ブラウザーの幅の表示のように設計された既定の CSS 規則よりも (モバイル ブラウザー) などの小さなブラウザーに HTML コンテンツの表示をわかりやすくを提供します。

## <a name="the-viewport-meta-tag"></a>ビューポートのメタ タグ

ほとんどのモバイルのブラウザー定義仮想ブラウザー ウィンドウの幅 (、*ビューポート*) は、モバイル デバイスの実際の幅よりも大きくします。 これにより、モバイル ブラウザーに合わせて仮想表示内で web ページ全体ができます。 ユーザーは、興味を引くコンテンツ上ズームできます。 ただし、ビューポートの幅を実際のデバイスの幅に設定するとズームは必要ありません、モバイル ブラウザーで、コンテンツを収めるためです。

ビューポート`<meta>`ASP.NET MVC 4 レイアウト ファイルのタグは、デバイスの幅にビューポートを設定します。 次の行では、ビューポートを示しています。 `<meta>` ASP.NET MVC 4 レイアウト ファイルのタグ。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>CSS メディア クエリと、ビューポートの Meta タグの効果の確認

開く、 *views \shared\\_Layout.cshtml*をコメント アウト、ビューポート、エディターでファイル`<meta>`タグ。 次のマークアップは、コメント アウトされた行を示しています。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

開く、 *MvcMobile\Content\Site.css*エディターでファイルを開き、メディア クエリで最大の幅を 0 ピクセルに変更します。 これにより、モバイル ブラウザーで使用される CSS 規則を防ぎます。 次の行は、変更後のメディア クエリを示しています。

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

変更を保存し、モバイル ブラウザー エミュレーターでアプリケーションを会議に移動します。 ビューポートを削除するは、次の図の小さなテキスト`<meta>`タグ。 ないビューポートで`<meta>`タグ、ブラウザーが、ズームアウトしてビューポートの既定の幅を (850 ピクセルまたはほとんどのモバイル ブラウザーの幅)。

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

変更を元に戻す、ビューポートをコメント解除します`<meta>`レイアウト ファイルにタグを付けるし、850 のピクセルに、メディア クエリを復元、 *Site.css*ファイル。 変更を保存し、モバイル対応画面が復元されたことを確認するモバイル ブラウザーを更新します。

ビューポート`<meta>`タグと CSS メディア クエリは ASP.NET MVC 4 に固有ではないと、任意の web アプリケーションでこれらの機能を行うことができます。 新しい ASP.NET MVC 4 プロジェクトを作成するときに生成されるファイルに組み込まれているいるようになりました。

ビューポートの詳細については`<meta>`タグは、「 [2 つのビューポートのお話-第 2 部](http://www.quirksmode.org/mobile/viewports2.html)します。

次のセクションでは、モバイル ブラウザー専用ビューを提供する方法を確認します。

## <a name="overriding-views-layouts-and-partial-views"></a>ビュー、レイアウト、および部分ビューをオーバーライドします。

ASP.NET MVC 4 の重要な新機能は、モバイル ブラウザー全般、個々 のモバイル ブラウザー、または特定のブラウザー (レイアウトと部分ビューを含む) 任意のビューをオーバーライドすることができます、シンプルなメカニズムです。 モバイル専用ビューを提供するビュー ファイルのコピーし、追加*します。Mobile*ファイル名にします。 たとえば、モバイルを作成する*インデックス*ビューで、コピー *Views\Home\Index.cshtml*に*Views\Home\Index.Mobile.cshtml*します。

このセクションでは、モバイル専用のレイアウト ファイルを作成します。

開始するには、コピー *views \shared\\_Layout.cshtml*に*views \shared\\_Layout.Mobile.cshtml*します。 開いている *\_Layout.Mobile.cshtml*からタイトルを変更**MVC4 Conference**に**Conference (Mobile)** します。

各`Html.ActionLink`呼び出し、それぞれのリンクに「によって参照」を削除*ActionLink*します。 次のコードでは、モバイル レイアウト ファイルの完成した body セクションを示します。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

コピー、 *Views\Home\AllTags.cshtml*ファイルを*Views\Home\AllTags.Mobile.cshtml*します。 新しいファイルを開き、変更、`<h2>`要素から"Tags"を"Tags (M)"。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

デスクトップ ブラウザーを使用して、およびモバイル ブラウザー エミュレーターを使用してタグ ページに移動します。 モバイル ブラウザー エミュレーターでは、2 つの変更を示します。

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

これに対し、デスクトップでの表示は変更されていません。

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>ブラウザー専用のビュー

モバイルおよびデスクトップに固有のビューだけでなく、個々 のブラウザー ビューを作成できます。 たとえば、iPhone ブラウザー専用のビューを作成できます。 このセクションでは、iPhone ブラウザーと iPhone バージョンのレイアウトを作成します、 *AllTags*ビュー。

開く、 *Global.asax*ファイルを開き、次のコードを追加、`Application_Start`メソッド。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

このコードは、各受信要求と一致する"iPhone"という名前の新しい表示モードを定義します。 受信要求には、(ユーザー エージェントには、"iPhone"という文字列が含まれている) 場合は、定義した条件が一致すると、ASP.NET MVC ビューの名前持つにはには、"iPhone"サフィックスが含まれています。 検索されます。

コードを右クリックし`DefaultDisplayMode`、選択**解決**を選び、 `using System.Web.WebPages;`。 参照を追加、`System.Web.WebPages`されている名前空間、`DisplayModes`と`DefaultDisplayMode`型が定義されます。

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

また、次の行を追加だけ手動で、`using`ファイルのセクション。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

すべての内容、 *Global.asax*ファイルを次に示します。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

変更を保存します。 コピー、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルを*MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*します。 新しいファイルを開き、変更、`h1`から見出し`Conference (Mobile)`に`Conference (iPhone)`します。

コピー、 *MvcMobile\Views\Home\AllTags.Mobile.cshtml*ファイルを*MvcMobile\Views\Home\AllTags.iPhone.cshtml*します。 新規のファイルで、変更、`<h2>`要素から"Tags (M)""Tags (iPhone)"にします。

アプリケーションを実行します。 モバイル ブラウザー エミュレーターを実行、そのユーザー エージェントは、"iPhone"に設定されているかどうかを確認しを参照、 *AllTags*ビュー。 次のスクリーン ショット、 *AllTags*でビューが表示される、 [Safari](http://www.apple.com/safari/download/)ブラウザー。 Safari の Windows をダウンロードする[ここ](https://support.apple.com/kb/DL1531)します。

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

このセクションではモバイル レイアウトとビューを作成する方法とレイアウト、および iPhone などの特定のデバイス用のビューを作成する方法を説明しました。 次のセクションでは、さらに魅力的なモバイル ビューの jQuery Mobile を活用する方法を確認します。

## <a name="using-jquery-mobile"></a>JQuery Mobile を使用します。

[の jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html)ライブラリには、すべての主要なモバイル ブラウザーでも機能するユーザー インターフェイス フレームワークが用意されています。 jQuery Mobile 適用*プログレッシブ エンハンスメント*CSS および JavaScript をサポートするモバイル ブラウザーにします。 プログレッシブ エンハンスメントつつより強力なブラウザーと高度な表示するデバイスの web ページでは、基本的なコンテンツを表示するすべてのブラウザーを許可します。 JQuery Mobile に含まれている JavaScript と CSS ファイルは、モバイル ブラウザーに合わせてマークアップ変更を加えずに多くの要素をスタイル設定します。

このセクションでをインストール、 *jQuery.Mobile.MVC* NuGet パッケージは、jQuery Mobile とビュー切り替えウィジェットをインストールします。

開始するには、削除、 *Shared\\_Layout.Mobile.cshtml*と*Shared\\_Layout.iPhone.cshtml*先ほど作成したファイル。

名前を変更*Views\Home\AllTags.Mobile.cshtml*と*Views\Home\AllTags.iPhone.cshtml*ファイル*Views\Home\AllTags.iPhone.cshtml.hide*と*Views\Home\AllTags.Mobile.cshtml.hide*します。 ファイルが必要がなくなるので、 *.cshtml*拡張機能、表示するために、ASP.NET MVC ランタイムによって使用されない、 *AllTags*ビュー。

インストール、 *jQuery.Mobile.MVC*これにより、NuGet パッケージ。

1. **ツール**メニューの **ライブラリ パッケージ マネージャー**、し、**パッケージ マネージャー コンソール**します。

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. **パッケージ マネージャー コンソール**を入力します `Install-Package jQuery.Mobile.MVC -version 1.0.0`

次の図は、ファイルが追加され、jQuery.Mobile.MVC nuget MvcMobile プロジェクトに変更します。 ファイル名の後に追加された [追加] が追加されるファイル。 イメージでは、GIF は表示されず、PNG ファイルに追加、 *content \images*フォルダー。

![](aspnet-mvc-4-mobile-features/_static/image21.png)

次に、jQuery.Mobile.MVC NuGet パッケージがインストールされます。

- *アプリ\_Start\BundleMobileConfig.cs*ファイルで、追加された jQuery JavaScript と CSS ファイルを参照するが必要です。 以下の手順に従うし、このファイルで定義されているモバイル バンドルを参照する必要があります。
- jQuery Mobile CSS ファイル。
- A`ViewSwitcher`コント ローラー ウィジェット (*controllers \viewswitchercontroller.cs*)。
- jQuery Mobile JavaScript ファイル。
- JQuery Mobile スタイル レイアウト ファイル (*views \shared\\_Layout.Mobile.cshtml*)。
- ビュー切り替え部分ビュー *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) からデスクトップ ビューとモバイル ビューに切り替えるには、各ページの上部にあるリンクを提供します。
- いくつか<em>.png</em>と<em>.gif</em>イメージ ファイル、 <em>content \images</em>フォルダー。

開く、 *Global.asax*ファイルを開き、次のコードを追加の最後の行として、`Application_Start`メソッド。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

次のコードは、完全な*Global.asax*ファイル。

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Internet Explorer 9 を使用しているし、表示されない場合、`BundleMobileConfig`黄色の枠線の上をクリックして、[互換表示ボタン](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")アウトラインから変更アイコン IE で![(オフ) 互換表示 ボタンの画像](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "(オフ) 互換表示 ボタンの画像")を純色![(on) 互換表示 ボタンの画像](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "(on) 互換表示 ボタンの画像")します。 または FireFox または Chrome では、このチュートリアルを表示できます。


開く、 *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルを開き、次のマークアップを追加、`Html.Partial`呼び出し。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

完全な*MvcMobile\Views\Shared\\_Layout.Mobile.cshtml*ファイルを次に示します。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

アプリケーションをビルドし、モバイル ブラウザー エミュレーターを参照、 *AllTags*ビュー。 次を参照してください。

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> によって、モバイル固有のコードをデバッグすることができます[ユーザー エージェント文字列を設定](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/)IE または Chrome iPhone、し F-12 developer tools を使用します。 モバイル ブラウザーに表示されない場合は、**ホーム**、**スピーカー**、**タグ**、および**日付**リンクがボタンとして、jQuery Mobile への参照スクリプトおよび CSS ファイルが正しくない可能性があります。


表示スタイルの変更だけでなく**モバイル ビューを表示する**リンクがモバイル ビューからデスクトップ ビューに切り替えることができます。 選択、**デスクトップ ビュー**リンク、およびデスクトップ ビューが表示されます。

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

デスクトップ ビューには、モバイル ビューに直接移動する方法を提供しません。 今すぐを修正します。 開く、 *views \shared\\_Layout.cshtml*ファイル。 ページのすぐ下`body`要素、ビュー切り替えウィジェットを描画する次のコードを追加します。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

更新、 *AllTags*モバイル ブラウザーで表示します。 デスクトップとモバイル ビュー間を移動できるようになりました。

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> デバッグに注意してください: 次のコードを追加するには、views \shared の末尾に\\_ViewSwitcher.cshtml ブラウザー ユーザー エージェント文字列を使用してモバイル デバイスに設定されている場合は、ビューをデバッグする際にします。
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  次の見出しを追加して、 *views \shared\\_Layout.cshtml*ファイル。  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


参照、 *AllTags*デスクトップ ブラウザーでページ。 ビュー切り替えウィジェットは、モバイル レイアウト ページにのみ追加されるため、デスクトップ ブラウザーでは表示されません。 チュートリアルの後半でデスクトップ ビューにビュー切り替えウィジェットを追加する方法がわかります。

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>スピーカー一覧を向上させる

モバイル ブラウザーで、選択、**スピーカー**リンク。 モバイル ビューがないため (*AllSpeakers.Mobile.cshtml*)、既定のスピーカー表示 (*AllSpeakers.cshtml*) モバイル レイアウト ビューを使用して描画されます ( *\_Layout.Mobile.cshtml*)。

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

設定して、モバイル レイアウト内の既定の (非モバイル) ビューを無効にすることができますグローバル一意識`RequireConsistentDisplayMode`に`true`で、*ビュー\\_ViewStart.cshtml*次のように、ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

ときに`RequireConsistentDisplayMode`に設定されている`true`、モバイル レイアウト (<em>\_Layout.Mobile.cshtml</em>) モバイル ビューにのみ使用されます。 (これは、ビュー ファイルの形式がある<em>* * ViewName</em><em>します。*.Mobile.cshtml</em>)。設定することがあります`RequireConsistentDisplayMode`に`true`モバイル レイアウトが非モバイル ビューでうまくいかない場合。 以下のスクリーン ショット、<em>スピーカー</em>ページが表示される`RequireConsistentDisplayMode`に設定されている`true`します。

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

設定して、ビューの一貫した表示モードを無効にできます`RequireConsistentDisplayMode`に`false`ファイルの表示にします。 次のマークアップは、 *Views\Home\AllSpeakers.cshtml*ファイルのセット`RequireConsistentDisplayMode`に`false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>モバイル スピーカー ビューを作成します。

いま見たように、*スピーカー*ビューは読み取れますでが、リンクが小さく、モバイル デバイスでタップが困難です。 このセクションでは、モバイル専用を作成します*スピーカー*現代的なモバイル アプリケーションのようなビュー: 大規模なが表示されます、タップ簡単なリンクしスピーカーをすばやく見つける検索ボックスが含まれています。

コピー *AllSpeakers.cshtml*に*AllSpeakers.Mobile.cshtml*します。 開く、 *AllSpeakers.Mobile.cshtml*ファイルし、削除、`<h2>`見出し要素。

`<ul>`タグを追加、`data-role`属性し、その値に設定`listview`します。 などの他の[`data-*`属性](http://html5doctor.com/html5-custom-data-attributes/)、`data-role="listview"`大きい一覧項目を簡単をタップします。 完成したマークアップのようになります。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

モバイル ブラウザーを更新します。 更新されたビューのようになります。

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

モバイル ビューが改善、ことが難しいスピーカーの長い一覧を移動します。 これを修正する、`<ul>`タグを追加、`data-filter`属性およびに設定`true`します。 次のコード、`ul`マークアップ。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

次の図は、検索フィルター ボックスに起因するページの上部にある、`data-filter`属性。

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

検索ボックスにそれぞれの文字を入力するの jQuery Mobile は、次の図に示すように、表示される一覧をフィルターします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>[タグ] ボックスの向上

などの既定の*スピーカー*ビュー、*タグ*ビューは読み取れますが、リンクが小さく、モバイル デバイスのタップが困難です。 このセクションで修正します、*タグ*と同じ方法を表示、*スピーカー*ビュー。

削除、&quot;を非表示に&quot;サフィックスを*Views\Home\AllTags.Mobile.cshtml.hide*ファイルの名前は*Views\Home\AllTags.Mobile.cshtml*します。 名前が変更されたファイルを開き、削除、`<h2>`要素。

追加、`data-role`と`data-filter`属性を`<ul>`タグを次に示します。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

次の図は、文字で絞り込んだタグ ページ`J`します。

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>日付一覧を向上させる

向上することができます、*日付*表示を改善する、*スピーカー*と*タグ*ビュー、モバイル デバイスで使いやすいようにします。

コピー、 *Views\Home\AllDates.cshtml*ファイルを*Views\Home\AllDates.Mobile.cshtml*します。 新しいファイルを開き、削除、`<h2>`要素。

追加`data-role="listview"`を`<ul>`タグは、次のようにします。

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

次の図はどのような**日付**ページ、`data-role`インプレース属性。

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png)の内容を置き換える、 *Views\Home\AllDates.Mobile.cshtml*を次のコード ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

このコードは、日数をすべてのセッションをグループ化します。 、毎日新しいリストの区切り線を作成し、毎日の区分線の下のすべてのセッションが一覧表示されます。 このコードが実行されるときの表示内容を次に示します。

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>SessionsTable ビューを向上させる

このセクションでは、セッションのモバイル専用ビューを作成します。 変更箇所は、作成した他のビューよりもより広範なになります。

モバイルのブラウザーでは、タップ、**スピーカー** 」と入力し、このボタンをクリックすると、`Sc`検索ボックスにします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

タップして、 **Scott Hanselman**リンク。

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

ご覧のように、表示はモバイル ブラウザーで読みにくいです。 日付列が読み取りにくくなると、タグ列がビューからは。 この問題を解決するにはコピー *Views\Home\SessionsTable.cshtml*に*Views\Home\SessionsTable.Mobile.cshtml*、ファイルの内容を次のコードに置き換えます。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

コードでは、タグ列とし、このすべての情報をモバイル ブラウザーで読み取れるように、タイトル、スピーカー、および日付を縦方向にフォーマット ルームを削除します。 次の図では、コードの変更が反映されます。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>SessionByCode ビューを向上させる

最後のモバイル専用ビューを作成します、 *SessionByCode*ビュー。 モバイルのブラウザーでは、タップ、**スピーカー** 」と入力し、このボタンをクリックすると、`Sc`検索ボックスにします。

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

タップして、 **Scott Hanselman**リンク。 Scott Hanselman のセッションが表示されます。

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

選択、 **An Overview of、MS Web Stack of Love**リンク。

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

既定のデスクトップ ビューには問題ありません、向上させることができます。

コピー、 *Views\Home\SessionByCode.cshtml*に*Views\Home\SessionByCode.Mobile.cshtml*の内容を置き換えると、 *Views\Home\SessionByCode.Mobile.cshtml*を次のマークアップ ファイル。

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

新しいマークアップでは、`data-role`属性ビューのレイアウトを改善します。

モバイル ブラウザーを更新します。 次の図は、直前に行ったコードの変更を反映しています。

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>(コース完了) とレビュー

このチュートリアルには、ASP.NET MVC 4 Developer Preview の新しいモバイル機能が導入されています。 モバイルの機能は次のとおりです。

- グローバルと個別のビュー レイアウト、ビュー、および部分ビューをオーバーライドする機能。
- レイアウトと部分の上書きの強制を使用して制御、`RequireConsistentDisplayMode`プロパティ。
- モバイル ビュー切り替えウィジェット ビューよりも、デスクトップ ビューにも表示されることができます。
- IPhone ブラウザーなどの特定のブラウザーをサポートするためのサポート。

## <a name="see-also"></a>関連項目

- [jQuery Mobile](http://jquerymobile.com)サイト。
- [jQuery モバイルの概要](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3c 勧告: モバイル Web アプリケーションのベスト プラクティス](http://www.w3.org/TR/mwabp/)
- [W3C のメディア クエリに関する勧告候補](http://www.w3.org/TR/css3-mediaqueries/)
