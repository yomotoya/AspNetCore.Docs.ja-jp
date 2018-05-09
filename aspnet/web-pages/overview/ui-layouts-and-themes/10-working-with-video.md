---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: ページ (Razor) サイトの ASP.NET Web でのビデオの表示 |Microsoft ドキュメント
author: tfitzmac
description: この章では、ビデオ、ASP.NET Web Pages で Razor 構文のページを表示する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 4e7dc50fb60546d1e1f10a16ed863c0b812ec82b
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトにビデオを表示します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトにビデオ (メディア プレーヤーを使用して、サイトに保存されているビデオを表示できるようにする方法について説明します。 ASP.NET Web ページ Razor 構文を使用すると、Flash を再生することができます (*.swf*)、Media Player (*.wmv*)、および Silverlight (*.xap*) ビデオです。
> 
> 学習する内容。
> 
> - ビデオ プレーヤーを選択する方法です。
> - Web ページにビデオを追加する方法です。
> - ビデオ プレーヤーの属性を設定する方法。
> 
> これらは、ASP.NET Razor ページのアーティクルで導入された機能。
> 
> - `Video`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> このチュートリアルは、WebMatrix 3 でも動作します。


## <a name="introduction"></a>はじめに

サイトのビデオを表示する場合があります。 1 つの方法では、YouTube と同様に、ビデオは既にサイトへのリンクです。 独自のページに直接これらのサイトからビデオを埋め込む場合は、通常、サイトからの HTML マークアップを取得でき、ページにコピーできます。 たとえば、次の例を示しています、YouTube を埋め込む方法ビデオ。

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

(パブリックのビデオ共有サイト) ではなく、独自の web サイト上にあるビデオを再生する場合は、直接、次のように埋め込みのマークアップを使用してリンクすることはできません。 使用して、サイトからビデオを再生するただし、`Video`ヘルパーに渡し、ページに直接メディア プレーヤーをレンダリングします。

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>ビデオ プレーヤーを選択します。

ビデオ ファイルの形式のたくさん、それぞれの形式は、他のプレーヤーと、player を構成するさまざまな方法に通常必要とします。 ASP.NET Razor ページで、web ページを使用して、ビデオを再生することができます、`Video`ヘルパー。 `Video`ヘルパーが自動的に生成されるため、web ページでビデオを埋め込みのプロセスを簡略化、`object`と`embed`ビデオ ページを追加する、通常使用される HTML 要素です。

`Video`ヘルパーは、次のメディア プレーヤーをサポートしています。

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash`のプレーヤー、`Video`ヘルパーを使用して、フラッシュのビデオを再生できます (*.swf*ファイル)、web ページにします。 少なくとも、ビデオ ファイルへのパスを指定する必要があります。 パスだけを指定する場合、player はフラッシュの現在のバージョンで設定されている既定値を使用します。 一般的な既定の設定は次のとおりです。

- 既定の幅と高さを使用してビデオを表示および背景色がないです。
- ビデオは、ページが読み込まれるときに自動的に再生されます。
- ビデオは、明示的に停止されるまでに継続的にループ処理します。
- ビデオは、特定のサイズに合わせてビデオがトリミングされる点ではなく、ビデオのすべてを表示するスケールします。
- ウィンドウでビデオを再生します。

### <a name="the-mediaplayer-player"></a>Media Player プレーヤー

`MediaPlayer`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*web ページではファイル)。 を再生するメディア ファイルのパスを含める必要がありますその他のすべてのパラメーターは省略できます。 パスのみを指定する場合、player はなどの設定、media Player の現在のバージョンで既定の設定を使用します。

- 既定の幅と高さを使用して、ビデオが表示されます。
- ビデオは、ページが読み込まれるときに自動的に再生されます。
- ビデオが 1 回再生 (そのループしません)。
- Player は、ユーザー インターフェイスのコントロールの完全なセットを表示します。
- ウィンドウでビデオを再生します。

### <a name="the-silverlight-player"></a>Silverlight プレーヤー

`Silverlight`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media Audio (*.wma*ファイル)、および MP3 (*.mp3*ファイル)。 Silverlight ベースのアプリケーション パッケージを指すよう path パラメーターを設定する必要があります (*.xap*ファイル)。 また、幅と高さのパラメーターを設定することも必要があります。 その他のすべてのパラメーターは省略できます。 必須パラメーターのみを設定した場合、ビデオ、Silverlight プレーヤーを使用するときに、Silverlight プレーヤーには、背景色がないビデオが表示されます。

> [!NOTE]
> Silverlight がわからない場合に備えて、: *.xap*ファイルは、レイアウトの指示を含む圧縮ファイル、 *.xaml*ファイル、アセンブリ、および省略可能なリソースのマネージ コード。 作成することができます、 *.xap* Silverlight アプリケーション プロジェクトとして Visual Studio でのファイルです。


`Silverlight`ビデオ プレーヤー、プレーヤーに指定される設定とに用意されている設定の両方を使用して、 *.xap*ファイル。

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME の種類
> 
> ブラウザーがファイルをダウンロードするとき、ブラウザーは、ファイルの種類が表示されるドキュメントに対して指定されている MIME の種類と一致していることを確認します。 MIME の種類は、ファイルのコンテンツの種類またはメディアの種類です。 `Video`ヘルパーは、次の MIME の種類を使用します。
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>フラッシュ (.swf) のビデオの再生

この手順は、名前、フラッシュ ビデオを再生する方法を示します*sample.swf*です。 手順では、という名前のフォルダーをしたものと*メディア*ことと、サイトで、 *.swf*ファイルがそのフォルダーにします。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. Web サイトでページを追加し、名前を付けます*FlashVideo.cshtml*です。
3. ページに、次のマークアップを追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. ブラウザーでページを実行します。 (ページが選択されて、必ず、**ファイル**ワークスペースを実行する前にします)。ページが表示され、ビデオを自動的に再生します。 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

設定することができます、`quality`フラッシュ ビデオへのパラメーター `low`、 `autolow`、 `autohigh`、 `medium`、 `high`、および`best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

フラッシュ ビデオを使用して特定のサイズで再生を変更することができます、`scale`パラメーターで、次を設定することができます。

- `showall`。 これにより、元の縦横比を維持しながらビデオ全体を表示にします。 ただし、それぞれの側の枠で囲まれた終了する可能性があります。
- `noorder`。 元の縦横比を維持しながら、ビデオ拡大または縮小がトリミングされる可能性があります。
- `exactfit`。 これにより、ビデオを全体表示されている元の縦横比を維持せずがゆがみが発生する可能性があります。

指定しない場合は、`scale`パラメーター、ビデオ全体が表示されます、元の縦横比がトリミングことがなく継続されます。 次の例を使用する方法を示しています、`scale`パラメーター。

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player が名前付きの設定のビデオ モードをサポートしている`windowMode`です。 これを設定することができます`window`、 `opaque`、および`transparent`です。 既定では、`windowMode`に設定されている`window`、web ページ上の別のウィンドウで、ビデオが表示されます。 `opaque`設定では、web ページ上のビデオの背後にすべて非表示にします。 `transparent`設定は、ビデオの任意の部分は透過的と仮定した場合、ビデオを表示する web ページの背景をことができます。

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Media Player の再生 (*.wmv*) のビデオ

次の手順は、という名前のウィンドウ Media ビデオを再生する方法を示します*sample.wmv*内にある、*メディア*フォルダーです。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*MediaPlayerVideo.cshtml*です。
3. ページに、次のマークアップを追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. ブラウザーでページを実行します。 ビデオを読み込んで、自動的に再生します。 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

設定することができます`playCount`を自動的にビデオを再生する回数を示す整数。

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode`パラメーターを使用して、ユーザー インターフェイスに表示するコントロールを指定できます。 設定することができます`uiMode`に`invisible`、 `none`、 `mini`、または`full`です。 指定しない場合、`uiMode`パラメーター、ビデオがでシーク、ステータス ウィンドウが表示されます、バー、ボタン、およびビデオのウィンドウに加え、ボリューム コントロールを制御します。 これらのコントロールが、オーディオ ファイルを再生するプレーヤーを使用する場合にも表示されます。 使用する方法の例をここでは、`uiMode`パラメーター。

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

既定では、オーディオは、ビデオの再生時にです。 設定して、オーディオをミュートすることができます、`mute`パラメーターを true にします。

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

設定して、media Player ビデオのオーディオ レベルを制御することができます、`volume`パラメーターを 0 ~ 100 の範囲の値。 既定値は 50 です。 次に例を示します。

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Silverlight のビデオの再生

この手順は、Silverlight に含まれているビデオを再生する方法を示します *.xap*という名前のフォルダーには、ページ*メディア*です。

1. 説明に従って、ASP.NET Web Helpers Library を web サイトに追加[ASP.NET Web Pages サイトをインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*SilverlightVideo.cshtml*です。
3. ページに、次のマークアップを追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. ブラウザーでページを実行します。 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Silverlight の概要](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[タグ属性をフラッシュのオブジェクトと埋め込み](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM タグします。](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
