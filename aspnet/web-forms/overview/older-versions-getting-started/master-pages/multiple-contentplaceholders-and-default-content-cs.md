---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: 複数の contentplaceholders と既定のコンテンツ (c#) |Microsoft ドキュメント
author: rick-anderson
description: マスター ページに複数のコンテンツのプレース ホルダーを追加する方法とコンテンツのプレース ホルダーで既定のコンテンツを指定する方法を調べます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: b60017c21b4cf45081893af08e68186009475fd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889057"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>複数の contentplaceholders と既定のコンテンツ (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> マスター ページに複数のコンテンツのプレース ホルダーを追加する方法とコンテンツのプレース ホルダーで既定のコンテンツを指定する方法を調べます。


## <a name="introduction"></a>はじめに

マスター ページの有効化検査を前のチュートリアルでは ASP.NET 開発者は一貫性のあるサイト全体のレイアウトを作成します。 マスター ページでは、すべてのコンテンツ ページに共通するマークアップとページ単位ごとにカスタマイズ可能な領域の両方を定義します。 前のチュートリアルでは、単純なマスター ページを作成しました (`Site.master`) と 2 つのコンテンツ ページ (`Default.aspx`と`About.aspx`)。 マスター ページは、2 つの contentplaceholders にという名前で構成されている`head`と`MainContent`、内に配置されていますが、`<head>`要素および Web フォームでは、それぞれします。 対応する 1 つのマークアップだけ指定したコンテンツ ページには、次の 2 つのコンテンツ コントロールがでしたが、`MainContent`です。

2 つのプレース ホルダー コントロールが示すよう`Site.master`、マスター ページは複数 contentplaceholders に含めることができます。 さらに、マスター ページは、プレース ホルダー コントロールの既定のマークアップを指定することができます。 コンテンツ ページでは、次に、必要に応じて独自のマークアップを指定したり、既定のマークアップを使用できます。 このチュートリアルでは、マスター ページの複数のコンテンツ コントロールを使用して拝見し、ContentPlaceHolder コントロールの既定のマークアップを定義する方法を参照してください。

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>手順 1: マスター ページへの追加 ContentPlaceHolder のコントロールの追加

多くの web サイトの設計には、ページ単位ごとにカスタマイズされた画面上のいくつかの領域が含まれます。 `Site.master`、前のチュートリアルで作成したマスター ページには、という名前の Web フォーム内で 1 つのプレース ホルダーが含まれています。`MainContent`です。 具体的には、この ContentPlaceHolder が内にある、 `mainContent` `<div>`要素。

図 1 は`Default.aspx`ブラウザーで表示したときにします。 赤い円で囲まれた領域に対応するページ固有のマークアップは、`MainContent`です。


[![丸地域領域を示しています、現在カスタマイズ可能なページの単位で](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**図 01**: ページの単位、領域現在カスタマイズ可能な丸領域で表示 ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))


図 1 に示すように、領域だけでなくも必要がある項目を追加するページに固有のレッスンやニュースの下の左の列を想像してみてくださいのセクションでします。 これを実現するには、マスター ページに ContentPlaceHolder の別のコントロールを追加します。 どうしたら、開く、`Site.master`マスター Visual Web Developer でページと、ツールボックスからデザイナーにニュース セクションの後に ContentPlaceHolder コントロールをドラッグします。 設定 ContentPlaceHolder の`ID`に`LeftColumnContent`です。


[![ContentPlaceHolder コントロール、マスター ページの左の列を追加します。](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**図 02**: ContentPlaceHolder コントロール、マスター ページの左の列を追加する ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))


追加すると、`LeftColumnContent`マスター ページ プレース ホルダーは、定義できますこの領域の内容をページ単位ごとに、コンテンツを含めることによって、ページ内の制御が`ContentPlaceHolderID`に設定されている`LeftColumnContent`です。 手順 2 では、このプロセスを説明します。

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>手順 2: コンテンツのページで、新しい ContentPlaceHolder のコンテンツを定義します。

に、web サイトを新しいコンテンツ ページを追加すると Visual Web Developer がコンテンツを自動的に作成、選択したマスター ページで各 ContentPlaceHolder のページにコントロールできます。 追加した、 `LeftColumnContent` ContentPlaceHolder 手順 1 で、新しい ASP.NET ページは、マスター ページに次の 3 つのコンテンツ コントロールがあります。

これを示すためには、という名前のルート ディレクトリに新しいコンテンツ ページを追加`MultipleContentPlaceHolders.aspx`にバインドされている、`Site.master`マスター ページ。 Visual Web Developer では、次の宣言型マークアップでこのページを作成します。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

参照するコンテンツ コントロールにコンテンツの一部を入力、 `MainContent` contentplaceholders に (`Content2`)。 次に、次に示すマークアップを追加、`Content3`コンテンツ コントロール (参照、 `LeftColumnContent` ContentPlaceHolder)。

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

このマークアップを追加した後は、ブラウザーでページを参照してください。 図 3 に表示される、マークアップに配置、 `Content3` (丸赤で囲まれた)、[ニュース] セクションの下にある左の列にコンテンツ コントロールが表示されます。 マークアップの配置で`Content2`(青色の丸囲み) ページの右側の部分に表示されます。


[![左の列が含まれています、ニュース セクションの下にあるページ固有のコンテンツには](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**図 03**:、左列ようになりましたが含まれていますページ固有のコンテンツの下に [ニュース] セクション ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>既存のコンテンツ ページでコンテンツを定義します。

新しいコンテンツ ページを自動的に作成するには、手順 1. で追加したお ContentPlaceHolder コントロールが組み込まれています。 次の 2 つ既存コンテンツ ページ -`About.aspx`と`Default.aspx`-のコンテンツ コントロールがない、 `LeftColumnContent` ContentPlaceHolder です。 これら 2 つの既存のページでこの ContentPlaceHolder のコンテンツを指定するには、社内でコンテンツ コントロールを追加する必要があります。

ほとんどの ASP.NET Web コントロールとは異なり、Visual Web Developer ツールボックスにコンテンツ コントロールの項目は含まれません。 手動で、ソース ビューにコンテンツ コントロールの宣言型マークアップで入力おできますが、簡単かつ迅速なアプローチは、デザイン ビューを使用します。 開く、`About.aspx`ページし、デザイン ビューに切り替えます。 図 4 に示すように、 `LeftColumnContent` ContentPlaceHolder がデザイン ビューに表示されます表示されるタイトルを読み取ります上にマウスを置く場合:"LeftColumnContent (マスター)。"。 タイトルに「マスター」を含めることは、この ContentPlaceHolder のページで定義されているコンテンツ コントロールがないことを示します。 場合と同様に、ContentPlaceHolder のコンテンツ コントロールが存在する場合`MainContent`、タイトルが読み取られます"*ContentPlaceHolderID* (カスタム) です。"。

コンテンツ コントロールを追加する、`LeftColumnContent`する ContentPlaceHolder `About.aspx`ContentPlaceHolder のスマート タグを展開し、カスタム コンテンツの作成 リンクをクリックします。


[![About.aspx のデザイン ビューは、LeftColumnContent ContentPlaceHolder を示しています。](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**図 04**: [デザイン] ビューの`About.aspx`を示しています、 `LeftColumnContent` ContentPlaceHolder ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))


必要なカスタム コンテンツの作成 リンクをクリックすると生成されますコンテンツ コントロールをページにその`ContentPlaceHolderID`ContentPlaceHolder のプロパティ`ID`です。 たとえば、リンクをクリックして、カスタム コンテンツの作成の`LeftColumnContent`内の領域`About.aspx`ページに、次の宣言型マークアップを追加します。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>コンテンツ コントロールを省略すること

ASP.NET では、すべてのコンテンツ ページがマスター ページで定義されている各 ContentPlaceHolder のコンテンツ コントロールを含めることは必要ありません。 コンテンツ コントロールを省略すると、ASP.NET エンジンは、マスター ページ プレース ホルダー内で定義されているマークアップを使用します。 このマークアップ呼びます ContentPlaceHolder の*既定のコンテンツ*とコンテンツ一部の地域が共通のページ大部分のでは、する必要がある少数のページ用にカスタマイズするシナリオに便利です。 手順 3 では、マスター ページで指定する既定のコンテンツについて説明します。

現在、`Default.aspx`の 2 つのコンテンツ コントロールが含まれています、`head`と`MainContent`contentplaceholders に; のコンテンツ コントロールがない`LeftColumnContent`です。 その結果、`Default.aspx`が表示される、 `LeftColumnContent` ContentPlaceHolder の既定のコンテンツを使用します。 まだこの ContentPlaceHolder の任意の既定のコンテンツを定義する、実質的な影響はこの領域のマークアップが出力されないことです。 この動作を確認するを参照してください。`Default.aspx`ブラウザーを使用します。 図 5 に示す、ニュース セクションの下に左側にあるマークアップは出力されません。


[![LeftColumnContent ContentPlaceHolder のコンテンツが表示されることはありません。](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**図 05**: No Content が表示されるため、 `LeftColumnContent` ContentPlaceHolder ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>手順 3: マスター ページの既定のコンテンツを指定します。

一部の web サイトのデザインには、その内容は、1 つまたは 2 つの例外を除き、サイト内のすべてのページの同じ領域が含まれます。 ユーザー アカウントをサポートする web サイトを検討してください。 このようなサイトでは、ログイン ページを訪問者が、サイトにサインインする資格情報を入力する場所が必要です。 プロセスには、サインインの時間を短縮するには、web サイト デザイナーは明示的にログイン ページにアクセスしなくてもサインインできるようにするには、各ページの左上隅にユーザー名とパスワードのテキスト ボックスを含める可能性があります。 これらのユーザー名とパスワード テキスト ボックスは、ほとんどのページに役立ちますが重複しているログイン ページで、ユーザーの資格情報のテキスト ボックスが既に存在します。

この設計を実装するのには、マスター ページの左上隅で ContentPlaceHolder コントロールを作成できます。 そのウィンドウの左上隅のユーザー名とパスワードのテキスト ボックスを表示するために必要な各ページは、この ContentPlaceHolder のコンテンツ コントロールを作成し、必要なインターフェイスを追加します。 その一方で、ログイン ページはこの ContentPlaceHolder のコンテンツ コントロールの追加を省略するか、またはコンテンツを作成で定義されたマークアップのコントロールです。 この方法の欠点は、すべてのページ ([ログイン] ページ) を除くサイトに追加するユーザー名とパスワードのテキスト ボックスを追加することを覚えてあります。 これは、問題を求めています。 私たちは、ページまたは 2 にこれらのテキスト ボックスを追加することを忘れがち可能性の高い、さらに、お可能性がありますいないインターフェイスを実装したり、正しく (2 ではなく 1 つだけの textbox の追加など)。

ContentPlaceHolder の既定のコンテンツとしてユーザー名とパスワードのテキスト ボックスを定義することをお勧めします。 これにより、のみいただくためにそれらのユーザー名とパスワードのテキスト ボックスが表示されない少数のページでこの既定のコンテンツを上書き (ログイン ページのインスタンス)。 ContentPlaceHolder コントロールの既定のコンテンツを指定することを示すためで説明したシナリオを実装してみましょう。

> [!NOTE]
> このチュートリアルの残りの部分では、すべてのページが、ログイン ページの左の列にログイン インターフェイスを含める当社の web サイトを更新します。 ただし、このチュートリアルでは、ユーザー アカウントをサポートするために、web サイトを構成する方法については検査しません。 このトピックの詳細についてを参照してください [フォーム認証、承認、ユーザー アカウントおよびロール](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)チュートリアルです。


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>プレース ホルダーを追加して、既定の内容を指定します。

開く、`Site.master`マスター ページとの間で左の列に、次のマークアップを追加、`DateDisplay`ラベルとレッスンについてのセクション。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

このマークアップを追加した後、マスター ページの [デザイン] ビューは図 6 のようになります。


[![マスター ページには、ログイン コントロールが含まれています。](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**図 06**: マスター ページには、ログイン コントロールが含まれています ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))


この ContentPlaceHolder`QuickLoginUI`既定の内容としてログイン Web コントロールが関連付けられています。 ログイン コントロールでは、ユーザー、ユーザー名とパスワードと共に、[ログイン] ボタンを要求するユーザー インターフェイスを表示します。 ログイン ボタンをクリックすると、ログイン コントロールは内部的には、メンバーシップ API に対するユーザーの資格情報を検証します。 次に、実際にはこのログイン コントロールを使用するには、メンバーシップを使用するようにサイトを構成する必要があります。 このトピックでは、このチュートリアルの対象外参照してください [フォーム認証、承認、ユーザー アカウントおよびロール](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)ユーザー アカウントをサポートする web アプリケーションを構築する方法についてのチュートリアルです。

自由にログイン コントロールの動作と外観をカスタマイズできます。 2 つのプロパティを設定している:`TitleText`と`FailureAction`です。 `TitleText`コントロールのユーザー インターフェイスの上部にある既定の「ログに」に、プロパティ値が表示されます。 [サインイン] テキストとして表示されるため、このプロパティを設定している、`<h3>`要素。 `FailureAction`プロパティには、ユーザーの資格情報が有効でない場合の対処方法を示します。 既定値は、値は`Refresh`、同じページにそのユーザーとログイン コントロール内のエラー メッセージが表示されます。 変更した`RedirectToLoginPage`ユーザーに無効な資格情報が発生した場合、ログイン ページを送信します。 しようとすると、ユーザーから他のいくつかのページが失敗し、ログイン、ログイン ページは、追加の指示と左の列に簡単に収まらないオプションを含めることができますので、ユーザーをログイン ページに送信する (_n)。 たとえば、ログイン ページには、忘れたパスワードを取得するか、新しいアカウントを作成するオプションがあります。

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>ログイン ページを作成し、既定のコンテンツをオーバーライドします。

完全なマスター ページには、次の手順は、ログイン ページを作成します。 という名前のサイトのルート ディレクトリを ASP.NET ページを追加`Login.aspx`、バインドにするには、`Site.master`マスター ページ。 これはページを作成する 4 つのコンテンツ コントロールを含むで定義されている、contentplaceholders にごとに 1 つ`Site.master`です。

ログイン コントロールを追加、`MainContent`コンテンツ コントロールです。 同様に、自由に任意のコンテンツを追加、`LeftColumnContent`領域。 ただし、確認用のコンテンツ コントロールのままにして、 `QuickLoginUI` ContentPlaceHolder 空です。 こうと、ログイン コントロールが、ログイン ページの左の列に表示されません。

コンテンツを定義した後、`MainContent`と`LeftColumnContent`領域の場合、ログイン ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

図 7 は、ブラウザーで表示したときに、このページを示します。 このページのコンテンツ コントロールを指定するため、 `QuickLoginUI` ContentPlaceHolder、マスター ページで指定された既定のコンテンツを上書きします。 実質的な影響は、ログイン コントロールがマスター ページのデザイン ビュー (図 6 を参照してください) はレンダリングされませんこのページに表示されることです。


[![ログイン ページ Represses QuickLoginUI ContentPlaceHolder の既定のコンテンツ](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**図 07**: ログイン ページ Represses、 `QuickLoginUI` ContentPlaceHolder の既定のコンテンツ ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>新しいページで既定のコンテンツの使用

ログイン ページを除くすべてのページの左の列にログイン コントロールを表示することができます。 これを実現する、ログイン ページを除くすべてのコンテンツ ページのコンテンツ コントロールを削除してください、 `QuickLoginUI` ContentPlaceHolder です。 コンテンツ コントロールを省くことで、ContentPlaceHolder の既定のコンテンツを代わりに使用されます。

既存のコンテンツ ページ - `Default.aspx`、 `About.aspx`、および`MultipleContentPlaceHolders.aspx`-のコンテンツ コントロールを含めないでください`QuickLoginUI`ContentPlaceHolder を制御するマスター ページに追加する前に作成されたためです。 したがって、これらの既存のページを更新する必要はありません。 ただし、web サイトに追加された新しいページがのコンテンツ コントロールを含める、`QuickLoginUI`既定では、プレース ホルダーです。 これらを削除することを覚えてがあるため、コンテンツ コントロール (私たちは、ログイン ページの場合は、ContentPlaceHolder の既定のコンテンツを無効にする) を除き、新しいコンテンツ ページに追加するたびにします。

コンテンツ コントロールを削除するに手動での宣言型マークアップをソース ビューから削除するか、デザイン ビューで、マスターのコンテンツへのリンクを既定の選択のスマート タグからです。 コンテンツ コントロールを削除する方法のいずれかのページと生成から同じ実質的にします。

図 8 は`Default.aspx`ブラウザーで表示したときにします。 注意してください`Default.aspx`のみ宣言型マークアップ - のいずれかで指定された 2 つのコンテンツ コントロールを持つ`head`と 1 つずつ`MainContent`です。 その結果、既定値に対し、コンテンツ、`LeftColumnContent`と`QuickLoginUI`contentplaceholders が表示されます。


[![LeftColumnContent および QuickLoginUI contentplaceholders に既定のコンテンツが表示されます。](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**図 08**: 既定のコンテンツを`LeftColumnContent`と`QuickLoginUI`contentplaceholders が表示されます ([フルサイズのイメージを表示するをクリックして](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))


## <a name="summary"></a>まとめ

ASP.NET マスター ページのモデルは、マスター ページの任意の数の contentplaceholders にできます。 Contentplaceholders がない対応する場合に生成されますが、既定のコンテンツを含める詳細は、コンテンツのページにコントロールのコンテンツします。 このチュートリアルでは、新規および既存の ASP.NET ページで新しいこれら contentplaceholders にコンテンツ コントロールを定義する方法と、マスター ページに追加のプレース ホルダー コントロールを含める方法を説明しました。 についても説明しました既定値を指定する ContentPlaceHolder のコンテンツは、それをカスタマイズするページ、必要な少数のみが特定の領域内でコンテンツを標準化する場合に便利です。

次のチュートリアルで取り上げる、`head`さらに詳しく ContentPlaceHolder の宣言とプログラムでページごとに、タイトル、メタ タグ、およびその他の HTML ヘッダーを定義する方法を表示します。

満足プログラミング!

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Suchi Banerjee しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

> [!div class="step-by-step"]
> [前へ](creating-a-site-wide-layout-using-master-pages-cs.md)
> [次へ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
