---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: 複数の ContentPlaceHolders と既定のコンテンツ (c#) |Microsoft Docs
author: rick-anderson
description: マスター ページに複数のコンテンツのプレース ホルダーを追加する方法とコンテンツのプレース ホルダーに既定のコンテンツを指定する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: 19c9d03d9aacdf842fb12bd16c83859ec4299d68
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390090"
---
<a name="multiple-contentplaceholders-and-default-content-c"></a>複数の ContentPlaceHolders と既定のコンテンツ (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> マスター ページに複数のコンテンツのプレース ホルダーを追加する方法とコンテンツのプレース ホルダーに既定のコンテンツを指定する方法について説明します。


## <a name="introduction"></a>はじめに

マスター ページの有効化調査を前のチュートリアルでは ASP.NET 開発者は一貫性のあるサイト全体レイアウトを作成します。 マスター ページは、すべてのコンテンツ ページに共通するマークアップとページの単位でカスタマイズ可能なリージョンの両方を定義します。 前のチュートリアルで単純なマスター ページを作成しました (`Site.master`) と 2 つのコンテンツ ページ (`Default.aspx`と`About.aspx`)。 マスター ページという名前の 2 つの ContentPlaceHolders を行いました`head`と`MainContent`、内に配置されていますが、`<head>`要素と Web フォームでは、それぞれします。 対応する 1 つのマークアップだけを指定したコンテンツ ページでは、2 つのコンテンツ コントロールでしたが、`MainContent`します。

これで 2 つのプレース ホルダー コントロールによって`Site.master`、マスター ページは、複数の ContentPlaceHolders を含めることができます。 さらに、マスター ページは、プレース ホルダー コントロールの既定のマークアップを指定することができます。 コンテンツ ページでは、必要に応じて、独自のマークアップを指定したり、既定のマークアップを使用できます。 このチュートリアルでは、マスター ページで複数のコンテンツ コントロールの使い方を見てをプレース ホルダー コントロールで既定のマークアップを定義する方法を参照してください。

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>手順 1: マスター ページに追加のプレース ホルダー コントロールを追加します。

多くの web サイトのデザインには、ページごとにカスタマイズされた画面上の複数の領域が含まれます。 `Site.master`、前のチュートリアルで作成したマスター ページには、という名前の Web フォーム内で 1 つのプレース ホルダーが含まれています。`MainContent`します。 具体的には、このプレース ホルダーが内にある、 `mainContent` `<div>`要素。

図 1 は`Default.aspx`とき、ブラウザーで表示します。 赤い丸で地域に対応するページ固有のマークアップは、`MainContent`します。


[![領域を囲んだ領域を示しています、現在カスタマイズ可能なページの単位で](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**図 01**: ページの単位、領域現在カスタマイズ可能な丸領域で表示 ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))。


図 1 に示すように、リージョンだけでなくも必要がある」のレッスンとニュースの下にある左の列にページ固有の項目を追加するセクション。 これを行うには、マスター ページに ContentPlaceHolder の別のコントロールを追加します。 作業を進めるには、開く、 `Site.master` Visual Web Developer でページをマスターし、"ニュース"セクションの後に、ツールボックスからデザイナーにプレース ホルダー コントロールをドラッグします。 ContentPlaceHolder の設定`ID`に`LeftColumnContent`します。


[![ContentPlaceHolder コントロール、マスター ページの左側の列を追加します。](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**図 02**: マスター ページの左の列に [ContentPlaceHolder のコントロールを追加 ([フルサイズの画像を表示する] をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))。


追加され、`LeftColumnContent`マスター ページにプレース ホルダーを定義できますこの領域の内容をページごとに、コンテンツを含めることによっているページにコントロール`ContentPlaceHolderID`に設定されている`LeftColumnContent`します。 手順 2 では、このプロセスを説明します。

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>手順 2: コンテンツのページで、新しい ContentPlaceHolder のコンテンツを定義します。

Visual Web Developer で、コンテンツが自動的に作成、web サイトを新しいコンテンツ ページを追加するときに、選択したマスター ページの各プレース ホルダーのページにコントロール。 追加した後は、`LeftColumnContent`マスター ページで、手順 1. 新しい ASP.NET ページを今すぐに得ることがある 3 つのコンテンツ コントロール。

これを示すためには、新しいコンテンツ ページをという名前のルート ディレクトリに追加`MultipleContentPlaceHolders.aspx`にバインドされている、`Site.master`マスター ページ。 Visual Web Developer では、次の宣言型マークアップでこのページを作成します。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

コンテンツ コントロールを参照することに、一部のコンテンツを入力、 `MainContent` ContentPlaceHolders (`Content2`)。 次に示すマークアップを次に、追加、`Content3`コンテンツ コントロール (参照、 `LeftColumnContent` ContentPlaceHolder)。

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

このマークアップを追加した後、ブラウザーを使用してページを参照してください。 図 3 に示すように、マークアップ配置、 `Content3` (赤の丸囲み)"ニュース"セクションの下にある左の列のコンテンツ コントロールが表示されます。 マークアップに配置`Content2`(青色の円で囲まれた) ページの右側の部分に表示されます。


[![左側の列が"ニュース"セクションの下にあるページに固有な内容が含まれています](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**図 03**: The 左列ようになりましたが含まれていますページ固有のコンテンツの下に、"ニュース"セクション ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))。


### <a name="defining-content-in-existing-content-pages"></a>既存のコンテンツ ページのコンテンツを定義します。

新しいコンテンツ ページを自動的に作成するには、手順 1. で追加されたプレース ホルダー コントロールが組み込まれています。 2 つ既存コンテンツ ページ -`About.aspx`と`Default.aspx`-のコンテンツ コントロールがない、`LeftColumnContent`プレース ホルダーです。 これら 2 つの既存のページで、このプレース ホルダーのコンテンツを指定するには、自分たちのコンテンツ コントロールを追加する必要があります。

ほとんどの ASP.NET Web コントロールとは異なり、Visual Web Developer ツールボックスにコンテンツ コントロールの項目は含まれません。 手動で、ソース ビューにコンテンツ コントロールの宣言型マークアップで入力できますが、簡単かつ迅速なアプローチは、デザイン ビューを使用します。 開く、`About.aspx`ページし、デザイン ビューに切り替えます。 図 4 に示すように、`LeftColumnContent`デザイン ビューにプレース ホルダーが表示されます上にマウスを置いた場合に表示されるタイトルを読み取ります:"LeftColumnContent (マスター)"。 タイトルに"Master"を含めることは、このプレース ホルダーのページで定義されているコンテンツ コントロールがないことを示します。 場合と同様、ContentPlaceHolder のコンテンツ コントロールが存在する場合`MainContent`、タイトルが読み取られます"*ContentPlaceHolderID* (カスタム)。"。

コンテンツ コントロールを追加、`LeftColumnContent`に ContentPlaceHolder`About.aspx`し、[ContentPlaceHolder のスマート タグを展開し、カスタム コンテンツの作成] リンクをクリックします。


[![About.aspx のデザイン ビューは、LeftColumnContent プレース ホルダーを示しています。](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**図 04**: [デザイン] ビューの`About.aspx`を示しています、 `LeftColumnContent` ContentPlaceHolder ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))。


カスタム コンテンツの作成] リンクをクリックすると、必要なが生成されますコンテンツ コントロールをページにその`ContentPlaceHolderID`プロパティを [ContentPlaceHolder の`ID`します。 たとえば、カスタムのコンテンツの作成リンクをクリックして`LeftColumnContent`でリージョン`About.aspx`ページに次の宣言型マークアップを追加します。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>コンテンツ コントロールを省略します。

ASP.NET では、すべてのコンテンツ ページがマスター ページで定義されている各 ContentPlaceHolder のコンテンツ コントロールを含めることは必要ありません。 コンテンツ コントロールを省略した場合、ASP.NET エンジンは、マスター ページで ContentPlaceHolder 内で定義されたマークアップを使用します。 このマークアップは、ContentPlaceHolder のと呼ばれます*既定のコンテンツ*コンテンツいくつかのリージョンは、ページの大半の間で共通がする必要があるページの数が少ないのカスタマイズのシナリオで便利です。 手順 3 では、マスター ページで指定する既定のコンテンツについて説明します。

現時点では、`Default.aspx`の 2 つのコンテンツ コントロールが含まれています、`head`と`MainContent`ContentPlaceHolders; のコンテンツ コントロールがない`LeftColumnContent`します。 その結果、`Default.aspx`が表示される、 `LeftColumnContent` ContentPlaceHolder の既定のコンテンツを使用します。 まだこのプレース ホルダーを既定のコンテンツを定義するがある、あるので、実際の効果はこのリージョンのマークアップは出力されませんするには。 この動作を確認するを参照してください。`Default.aspx`ブラウザーを使用します。 図 5 に示す、"ニュース"セクションの下にある左の列にマークアップは出力されません。


[![LeftColumnContent ContentPlaceHolder のコンテンツは表示されません。](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**図 05**: No Content に表示される、 `LeftColumnContent` ContentPlaceHolder ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))。


## <a name="step-3-specifying-default-content-in-the-master-page"></a>手順 3: マスター ページで既定のコンテンツを指定します。

一部の web サイトのデザインには、コンテンツがインストールされている 1 つまたは 2 つの例外を除くサイト内のすべてのページのリージョンが含まれます。 ユーザー アカウントをサポートする web サイトを検討してください。 このようなサイトでは、ログイン ページがサイトに、訪問者が署名資格情報を入力する場所が必要です。 サインイン プロセスを迅速に web サイト デザイナーは、ユーザーがログイン ページを明示的にアクセスすることがなくサインインできるようにするには、各ページの左上隅でユーザー名とパスワードのテキスト ボックスをなどがあります。 これらのユーザー名とパスワード テキスト ボックスは、ほとんどのページで便利ですが、ログイン ページで、テキスト ボックスに、ユーザーの資格情報が格納されている冗長です。

この設計を実装するには、マスター ページの左上隅でプレース ホルダー コントロールを作成できます。 各ページの左上隅で、ユーザー名とパスワードのテキスト ボックスを表示するために必要は、このプレース ホルダーのコンテンツ コントロールを作成し、必要なインターフェイスを追加します。 その一方で、ログイン ページがこの ContentPlaceHolder のコンテンツ コントロールを追加するか、省略または、コンテンツの作成は定義されていないマークアップを持つコントロール。 このアプローチの欠点は、すべてのページ (ログイン ページ) を除くサイトに追加するユーザー名とパスワードのテキスト ボックスを追加するのにあります。 これは、問題を求めています。 ページまたは 2 つに、これらのテキスト ボックスを追加することを忘れおそれがあります、またはさらに悪いことで可能性がありますインターフェイスが実装されて正しく (2 つではなくテキスト ボックスに 1 つだけを追加するなど)。

ContentPlaceHolder の既定のコンテンツとして、ユーザー名とパスワードのテキスト ボックスを定義することをお勧めします。 これにより、のみ必要があります、いくつかのページのユーザー名とパスワードのテキスト ボックスを表示しないこの既定のコンテンツを上書きする (ログイン ページのインスタンス)。 プレース ホルダー コントロールの既定のコンテンツを指定することを示すためで説明したシナリオを実装してみましょう。

> [!NOTE]
> このチュートリアルの残りの部分では、すべてのページが、ログイン ページの左の列にログイン インターフェイスを含める web サイトを更新します。 ただし、このチュートリアルでは、ユーザー アカウントをサポートするために、web サイトを構成する方法を検査しません。 このトピックの詳細についてを参照してください、[フォーム認証、承認、ユーザー アカウントとロール](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)チュートリアル。


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>プレース ホルダーを追加して、その既定のコンテンツを指定します。

開く、`Site.master`マスター ページとの間で、左側の列に、次のマークアップを追加、`DateDisplay`ラベルとレッスン セクション。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

このマークアップを追加した後、マスター ページの [デザイン] ビューは図 6 のようになります。


[![マスター ページには、Login コントロールが含まれます。](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**図 06**: マスター ページには、Login コントロールが含まれています ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))。


このプレース ホルダー、 `QuickLoginUI`、その既定のコンテンツとしてログイン Web コントロールがあります。 Login コントロールは、ユーザー名とパスワードと共に Log In ボタンをユーザーに求めるユーザー インターフェイスを表示します。 Log In ボタンをクリックすると、Login コントロールは内部的にメンバーシップ API に対するユーザーの資格情報を検証します。 次に、実際にはこのログイン コントロールを使用するには、メンバーシップを使用するサイトを構成する必要があります。 このトピックは、このチュートリアルの対象外参照してください、[フォーム認証、承認、ユーザー アカウントとロール](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)ユーザー アカウントをサポートする web アプリケーションの構築の詳細についてはチュートリアル。

自由にログイン コントロールの動作や外観をカスタマイズします。 2 つのプロパティを設定しました:`TitleText`と`FailureAction`します。 `TitleText`コントロールのユーザー インターフェイスの上部にある既定値は"Log In"に、プロパティの値が表示されます。 「サインイン」テキストとして表示するよう、このプロパティを設定しましたが、`<h3>`要素。 `FailureAction`プロパティには、ユーザーの資格情報が有効でない場合の対処方法を示します。 値を既定で`Refresh`、同じページにそのユーザーとログイン コントロール内のエラー メッセージが表示されます。 これを変更した`RedirectToLoginPage`、無効な資格情報が発生した場合、ログイン ページにユーザーを送信します。 しようとした場合、ユーザーから他のページが失敗し、ログイン、ログイン ページは、追加の手順と、左側の列に簡単にも適合しないオプションに含めることができますので、ユーザーをログイン ページに送信するしたいと考えています。 たとえば、ログイン ページには、忘れたパスワードを取得するか新しいアカウントを作成するオプションがあります。

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>ログイン ページを作成して、既定のコンテンツのオーバーライド

完全なマスター ページには、次の手順は、ログイン ページを作成します。 ASP.NET ページをという名前のサイトのルート ディレクトリに追加`Login.aspx`、バインドすることを`Site.master`マスター ページ。 4 つのコンテンツ コントロールを含むページが作成されそうで定義されている、ContentPlaceHolders ごとに 1 つ`Site.master`します。

ログイン コントロールを追加、`MainContent`コンテンツ コントロール。 同様に、お気軽に任意のコンテンツを追加、`LeftColumnContent`リージョン。 ただし、コンテンツ コントロールでのままにしてください、 `QuickLoginUI` ContentPlaceHolder 空です。 これにより、ログイン コントロールは、ログイン ページの左の列に表示されません。

コンテンツを定義した後、`MainContent`と`LeftColumnContent`リージョン、ログイン ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

図 7 では、ブラウザーで表示した場合は、このページを示します。 このページのコンテンツ コントロールを指定するため、 `QuickLoginUI` ContentPlaceHolder、マスター ページで指定された既定のコンテンツを上書きします。 実質的な効果は、このページでビューがないレンダリングされます (図 6 参照) は、マスター ページのデザインで、Login コントロールが表示されることです。


[![ログイン ページ Represses QuickLoginUI ContentPlaceHolder の既定のコンテンツ](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**図 07**: ログイン ページ Represses、 `QuickLoginUI` ContentPlaceHolder の既定のコンテンツ ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))。


### <a name="using-the-default-content-in-new-pages"></a>新しいページで、既定のコンテンツの使用

ログイン ページを除くすべてのページの左の列にログイン コントロールを表示します。 これを実現する、ログイン ページを除くすべてのコンテンツ ページがのコンテンツ コントロールを省略する必要があります、`QuickLoginUI`プレース ホルダーです。 コンテンツ コントロールを省略すると、ContentPlaceHolder の既定のコンテンツを代わりに使用されます。

既存のコンテンツ ページ - `Default.aspx`、`About.aspx`と`MultipleContentPlaceHolders.aspx`-のコンテンツ コントロールが含まれていません`QuickLoginUI`プレース ホルダーを制御するマスター ページに追加する前に作成されたためです。 そのため、これらの既存のページを更新する必要はありません。 ただし、コンテンツ コントロールでは、web サイトに追加された新しいページ、`QuickLoginUI`既定では、プレース ホルダーです。 これらを削除することがあるため、コンテンツ コントロール (ContentPlaceHolder の既定のコンテンツを無効にすることには、「ログイン ページの場合と同様) を除き、新しいコンテンツ ページに追加するたびにします。

コンテンツ コントロールを削除するには、手動でソース ビューからその宣言型マークアップを削除するか、デザイン ビューで、マスターのコンテンツへのリンクを既定の選択のスマート タグから。 どちらの方法は、コンテンツ コントロールを削除します。 影響を net 同じページから生成します。

図 8 は`Default.aspx`とき、ブラウザーで表示します。 いることを思い出してください`Default.aspx`しか宣言型マークアップ - のいずれかで指定された 2 つのコンテンツ コントロール`head`とに 1 つずつ`MainContent`します。 その結果、既定のコンテンツ、`LeftColumnContent`と`QuickLoginUI`ContentPlaceHolders が表示されます。


[![LeftColumnContent と QuickLoginUI ContentPlaceHolders の既定のコンテンツが表示されます。](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**図 08**: 既定のコンテンツを`LeftColumnContent`と`QuickLoginUI`ContentPlaceHolders が表示されます ([フルサイズの画像を表示する をクリックします](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))。


## <a name="summary"></a>まとめ

ASP.NET マスター ページのモデルでは、ContentPlaceHolders のマスター ページ内の任意の数。 詳細については、ContentPlaceHolders 含める既定のコンテンツがない対応する場合は、出力はコンテンツ ページのコントロールのコンテンツします。 このチュートリアルでは、マスター ページに追加のプレース ホルダー コントロールを含める方法およびこれらの新しい ContentPlaceHolders 新規および既存の ASP.NET ページ内のコンテンツ コントロールを定義する方法を説明しました。 見てきました既定値を指定することで、プレース ホルダーは、コンテンツは少数のページをそれをカスタマイズする必要がありますが、特定の地域内のコンテンツを標準のシナリオで役立ちます。

次のチュートリアルについて説明します、`head`宣言とプログラミングでページごとに、タイトル、メタ タグ、およびその他の HTML ヘッダーを定義する方法が表示される ContentPlaceHolder の詳細については、します。

満足のプログラミングです。

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Suchi 著。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)します。

> [!div class="step-by-step"]
> [前へ](creating-a-site-wide-layout-using-master-pages-cs.md)
> [次へ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
