---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: ページ (Razor) サイトを ASP.NET web ビデオの表示 |Microsoft Docs
author: tfitzmac
description: この章では、ビデオ、ASP.NET Web Pages での Razor 構文のページを表示する方法について説明します。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 28884d8c4998ea5b00a60bf094f41b915b565bd8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828059"
---
<a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトにビデオを表示します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事では、ASP.NET Web Pages (Razor) の web サイトに (メディア) のビデオ プレーヤーを使用して、サイトに保存されているビデオを表示できるようにする方法について説明します。 Razor 構文を使用して ASP.NET Web ページでは、Flash を再生することができます (*.swf*)、Media Player (*.wmv*)、および Silverlight (*.xap*) ビデオ。
> 
> 学習内容。
> 
> - ビデオ プレーヤーを選択する方法。
> - Web ページにビデオを追加する方法。
> - ビデオ プレーヤー属性を設定する方法。
> 
> これらは、ASP.NET Razor ページの記事で導入された機能。
> 
> - `Video`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> このチュートリアルは、WebMatrix 3 でも機能します。


## <a name="introduction"></a>はじめに

サイトのビデオを表示する場合があります。 1 つの簡単な方法では、YouTube のように、ビデオが既にサイトにリンクします。 独自のページに直接これらのサイトからビデオを埋め込む場合は、通常は、サイトからの HTML マークアップを取得し、ページにコピーします。 たとえば、次の例では、ビデオを示しています、YouTube を埋め込む方法。

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

(パブリック ビデオ共有サイト) ではなく、独自の web サイト上にあるビデオを再生する場合は、直接を次のように埋め込みのマークアップを使用してリンクすることはできません。 使用して、サイトからビデオを再生するただし、`Video`ヘルパーで、ページに直接メディア プレーヤーを表示します。

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>ビデオ プレーヤーを選択します。

大量のビデオのファイルの形式があり、さまざまなプレーヤー、プレーヤーを構成するさまざまな方法に通常各形式が必要です。 ASP.NET Razor ページでは、web ページを使用してビデオを再生できます、`Video`ヘルパー。 `Video`ヘルパーが自動的に生成されるため、web ページで動画の埋め込みのプロセスを簡略化、`object`と`embed`ページにビデオを追加することに通常使用される HTML 要素。

`Video`ヘルパーは、次のメディア プレーヤーをサポートしています。

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>Flash Player

`Flash`のプレーヤー、`Video`ヘルパーを使用して、フラッシュのビデオを再生できます (*.swf*ファイル) で web ページ。 少なくともは、ビデオ ファイルへのパスを指定する必要あります。 何が、パスを指定する場合、player は Flash の現在のバージョンで設定されている既定値を使用します。 一般的な既定の設定は次のとおりです。

- ビデオは、既定の幅と高さを使用して表示されると、背景色を使用しない場合。
- ビデオは、ページの読み込み時に自動的に再生されます。
- ビデオは、明示的に停止されるまでに継続的にループ処理します。
- ビデオをスケーリングして、特定のサイズに合わせてビデオをトリミングするのではなく、ビデオをすべて表示します。
- ウィンドウで、ビデオを再生します。

### <a name="the-mediaplayer-player"></a>MediaPlayer プレーヤー

`MediaPlayer`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*web ページ内のファイル)。 を再生するメディア ファイルのパスを含める必要がありますその他のすべてのパラメーターは省略可能です。 パスだけを指定する場合、player は既定の設定など、media Player の現在のバージョンで設定を使用します。

- 既定の幅と高さを使用して、ビデオが表示されます。
- ビデオは、ページの読み込み時に自動的に再生されます。
- ビデオが 1 回再生 (にループしません)。
- ユーザー インターフェイスのコントロールの完全なセットが表示されます。
- ウィンドウで、ビデオを再生します。

### <a name="the-silverlight-player"></a>Silverlight プレーヤー

`Silverlight`のプレーヤー、`Video`ヘルパーでは、Windows Media ビデオを再生することができます (*.wmv*ファイル)、Windows Media オーディオ (*.wma*ファイル)、および MP3 (*.mp3*ファイルの場合)。 Silverlight ベースのアプリケーション パッケージを指すパス パラメーターを設定する必要があります (*.xap*ファイル)。 また、幅と高さのパラメーターを設定する必要があります。 その他のすべてのパラメーターは省略できます。 必要なパラメーターのみを設定した場合にビデオについては、Silverlight プレーヤーを使用する Silverlight プレーヤーの背景色のないビデオが表示されます。

> [!NOTE]
> Silverlight がまだわからない場合: *.xap*ファイルは、圧縮されたファイルのレイアウトの指示を含む、 *.xaml*ファイル、アセンブリ、および省略可能なリソースのマネージ コード。 作成することができます、 *.xap* Silverlight アプリケーション プロジェクトとして Visual Studio でのファイル。


`Silverlight`ビデオ プレーヤーをプレーヤーに対して指定した設定とに用意されている設定の両方を使用して、 *.xap*ファイル。

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>MIME の種類
> 
> ブラウザー ファイルをダウンロードするとき、ブラウザーは、ファイルの種類が表示されるドキュメントが指定されている MIME の種類と一致していることを確認します。 MIME の種類は、ファイルのコンテンツの種類またはメディアの種類です。 `Video`ヘルパーは、次の MIME の種類を使用します。
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`


<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>フラッシュ (.swf) のビデオの再生

この手順は、という名前のフラッシュ ビデオを再生する方法を示します*sample.swf*します。 という名前のフォルダーが得られたら、手順の前提条件*メディア*自分のサイトにある、 *.swf*ファイルがそのフォルダーにします。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)、既に追加していない場合。
2. Web サイトでページを追加し、名前*FlashVideo.cshtml*します。
3. 次のマークアップをページに追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. ブラウザーでページを実行します。 (内でページが選択されていることを確認、**ファイル**ワークスペースを実行する前にします)。ページが表示され、ビデオを自動的に再生します。 

    ![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")

設定することができます、`quality`フラッシュ ビデオのパラメーター `low`、 `autolow`、 `autohigh`、 `medium`、 `high`、および`best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

フラッシュ ビデオを再生を使用して特定のサイズを変更する、`scale`パラメーターで、次を設定することができます。

- `showall`。 これにより、元の縦横比を維持しながらビデオ全体を表示にします。 ただし、それぞれの側に境界線で入ることがあります。
- `noorder`。 元の縦横比を維持しながら、ビデオが拡大または縮小しますが、それがトリミングされる可能性があります。
- `exactfit`。 元の縦横比を維持せずビデオ全体を表示によりこれが、ゆがみが発生する可能性があります。

指定しない場合、`scale`パラメーター、ビデオ全体が表示されます、トリミングせずに元の縦横比は維持されます。 次の例は、使用する方法を示します、`scale`パラメーター。

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

Flash player がビデオ モードという名前の設定をサポートしている`windowMode`します。 これを設定して`window`、 `opaque`、および`transparent`します。 既定で、`windowMode`に設定されている`window`、web ページ上の別のウィンドウで、ビデオが表示されます。 `opaque`設定では、web ページ上のビデオの背後にあるすべて非表示にします。 `transparent`設定により、ビデオを表示する web ページのバック グラウンド ビデオの任意の部分が透明であると仮定します。

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Media Player の再生 (*.wmv*) のビデオ

次の手順は、名前付きウィンドウ Media ビデオを再生する方法を示します*sample.wmv*内にある、*メディア*フォルダー。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*MediaPlayerVideo.cshtml*します。
3. 次のマークアップをページに追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. ブラウザーでページを実行します。 ビデオでは、読み込みを自動的に再生します。 

    ![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")

設定することができます`playCount`に自動的にビデオを再生する回数を示す整数です。

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

`uiMode`パラメーターを使用して、ユーザー インターフェイスに表示するコントロールを指定できます。 設定できる`uiMode`に`invisible`、 `none`、 `mini`、または`full`します。 指定しない場合、`uiMode`パラメーター、ビデオになりますステータス ウィンドウに表示される、シーク バー ボタン、およびビデオのウィンドウのほかに、ボリューム コントロールを制御します。 これらのコントロールが、オーディオ ファイルを再生するプレーヤーを使用する場合にも表示されます。 使用する方法の例を次に示します、`uiMode`パラメーター。

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

既定では、オーディオは、ビデオの再生時に。 設定して、オーディオをミュートすることができます、`mute`パラメーターを true にします。

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

MediaPlayer のビデオのオーディオ レベルを設定して制御することができます、`volume`パラメーターが 0 から 100 までの値。 既定値は 50 です。 次に例を示します。

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Silverlight のビデオの再生

この手順は、Silverlight に含まれているビデオを再生する方法を示します *.xap*という名前のフォルダーには、ページ*メディア*します。

1. 」の説明に従って、web サイトに、ASP.NET Web Helpers Library を追加[ASP.NET Web ページ サイトでインストールするヘルパー](https://go.microsoft.com/fwlink/?LinkId=252372)まだ行っていない場合は、します。
2. という名前の新しいページを作成する*SilverlightVideo.cshtml*します。
3. 次のマークアップをページに追加します。 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. ブラウザーでページを実行します。 

    ![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


[Silverlight の概要](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Flash のオブジェクトおよび埋め込みのタグの属性](http://kb2.adobe.com/cps/127/tn_12701.html)

[Windows Media Player 11 SDK PARAM タグします。](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
