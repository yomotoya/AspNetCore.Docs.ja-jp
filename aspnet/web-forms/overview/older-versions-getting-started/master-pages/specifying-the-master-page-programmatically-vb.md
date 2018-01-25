---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: "プログラムによるマスター ページの指定 (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "PreInit イベント ハンドラーを使用してプログラムからのコンテンツ ページのマスター ページの設定を見ます。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 191de7546e2ba913fda0c8c8a8bfd3531b53336e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="specifying-the-master-page-programmatically-vb"></a>プログラムによるマスター ページの指定 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> PreInit イベント ハンドラーを使用してプログラムからのコンテンツ ページのマスター ページの設定を見ます。


## <a name="introduction"></a>はじめに

以降の例ではスタートとなった[ *、サイト全体のレイアウトを使用してマスター ページを作成する*](creating-a-site-wide-layout-using-master-pages-vb.md)、ページは、宣言を使用して、マスター ページを参照しているすべてのコンテンツ、 `MasterPageFile` 属性`@Page`ディレクティブです。 たとえば、次`@Page`ディレクティブがコンテンツ ページをマスター ページにリンク`Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

[ `Page`クラス](https://msdn.microsoft.com/library/system.web.ui.page.aspx)で、`System.Web.UI`名前空間が含まれています、 [ `MasterPageFile`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx)コンテンツ ページのマスター ページへのパスを返す; はこの、によって設定されるプロパティ`@Page`ディレクティブです。 このプロパティは、コンテンツ ページのマスター ページ、プログラムで指定するも使用できます。 この方法は、ページにアクセスしたユーザーなどの外部要因に基づくマスター ページを動的に割り当てる場合に便利です。

このチュートリアルでは 2 つ目のマスター ページ、web サイトに追加を実行時に使用するマスター ページを動的に決定します。

## <a name="step-1-a-look-at-the-page-lifecycle"></a>手順 1: を見て、ページのライフ サイクル

ASP.NET エンジンが、ページを組み込む必要があります要求がコンテンツ ページの ASP.NET ページ用の web サーバーに到着すると、マスター ページにコントロールのプレース ホルダー コントロールに対応するコンテンツ。 この fusion は、一般的なページのライフ サイクルを続行し、1 つのコントロール階層を作成します。

図 1 は、この fusion を示しています。 図 1 の手順 1 では、初期コンテンツおよびマスター ページ コントロール階層を示します。 PreInit ステージ コンテンツの末尾には、ページのコントロールがマスター ページ (手順 2) に対応する contentplaceholders に追加されます。 この fusion 後は、マスター ページは、余裕が生まれますコントロールの階層のルートとして機能します。 コントロールを定着この階層は完成したコントロールの階層 (手順 3) を生成するために、ページに追加されます。 最終的な結果は、ページのコントロールの階層に余裕が生まれますコントロール階層が含まれることです。


[![PreInit ステージ中に、マスター ページとコンテンツ ページのコントロールの階層が一緒に定着](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**図 01**: PreInit ステージ中に、マスター ページとページのコンテンツのコントロール階層は、一緒に定着 ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>手順 2: 設定、`MasterPageFile`コードからプロパティ

値に依存この fusion でどのようなマスター ページが partakes、`Page`オブジェクトの`MasterPageFile`プロパティです。 設定、`MasterPageFile`属性、`@Page`ディレクティブが割り当てることの実際の影響、`Page`の`MasterPageFile`段階で、初期化、ページのライフ サイクルの最初のステージであるプロパティ。 代わりにこのプロパティをプログラムで設定できます。 ただし、これが図 1 に fusion 行われる前にこのプロパティを設定することが重要です。

PreInit ステージの開始時、`Page`オブジェクト、 [ `PreInit`イベント](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx)呼び出しとその[`OnPreInit`メソッド](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)です。 マスター ページ、プログラムで設定を次に、生み出すことができますか、イベント ハンドラーを`PreInit`イベントまたは上書き、`OnPreInit`メソッドです。 両方の方法を見てみましょう。

開いて開始`Default.aspx.vb`、このサイトのホーム ページの分離コード クラス ファイルです。 ページのイベント ハンドラーを追加`PreInit`次のコードを入力してイベント。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

ここで設定することが、`MasterPageFile`プロパティです。 値を代入するため、コードを更新"~/Site.master"に、`MasterPageFile`プロパティです。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

ブレークポイントを設定して、デバッグの開始に表示される場合されるたびに、`Default.aspx`ページにアクセスするか、このページへのポストバックが存在する場合は、`Page_PreInit`イベント ハンドラーが実行されると、`MasterPageFile`にプロパティが割り当てられた"~/Site.master"です。

また、オーバーライド、`Page`クラスの`OnPreInit`メソッドと set、`MasterPageFile`プロパティがあります。 たとえば、みましょう設定されていない特定 ページでは、マスター ページからわけではなく`BasePage`です。 ベース ページのカスタム クラスを作成したということに注意してください (`BasePage`) に戻り、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)チュートリアルです。 現在`BasePage`よりも優先、`Page`クラスの`OnLoadComplete`メソッド、ページの設定`Title`プロパティのサイト マップ データに基づきます。 みましょう更新`BasePage`も上書きする、`OnPreInit`メソッドをプログラムで、マスター ページを指定します。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

派生して、すべてのコンテンツ ページのため`BasePage`、今すぐに割り当てられたプログラムで、マスター ページを持つし、すべてのです。 この時点で、`PreInit`内のイベント ハンドラー`Default.aspx.vb`は不要です。 自由にそれを削除します。

### <a name="what-about-thepagedirective"></a>では、`@Page`ディレクティブですか?

少しわかりにくいものがありますは、コンテンツ ページの`MasterPageFile`2 つの場所のプロパティが指定されているようになりました: でプログラムによって、`BasePage`クラスの`OnPreInit`メソッドからも、`MasterPageFile`ページごとにコンテンツの属性`@Page`ディレクティブです。

ページのライフ サイクルの最初のステージは、初期化段階です。 この段階で、`Page`オブジェクトの`MasterPageFile`プロパティには、値が割り当てられた、`MasterPageFile`属性、`@Page`ディレクティブ (提供されている) 場合。 初期化段階の次の PreInit 段階であり、ここでプログラムで設定されている、`Page`オブジェクトの`MasterPageFile`から割り当てられた値を上書きして、プロパティ、`@Page`ディレクティブです。 設定するため、`Page`オブジェクトの`MasterPageFile`プロパティ、プログラムで削除することでした、`MasterPageFile`属性から、`@Page`ディレクティブをエンドユーザーのエクスペリエンスに影響を与えずにします。 このたどりにさあ、削除、`MasterPageFile`属性から、`@Page`ディレクティブで`Default.aspx`し、ブラウザーでページにアクセスします。 予想できるように、出力は、同じとして属性が削除される前にします。

かどうか、`MasterPageFile`経由でプロパティが設定されて、`@Page`ディレクティブまたはプログラムによってエンドユーザーのエクスペリエンスに重要ではありません。 ただし、`MasterPageFile`属性、`@Page`ディレクティブを使用して Visual Studio デザイン時に、WYSIWYG を生成するために、デザイナーで表示します。 戻る場合`Default.aspx`Visual Studio で、移動、デザイナーには、メッセージが表示されます"マスター ページのエラー: ページ、マスター ページの参照が必要なコントロールが指定されていない"(図 2 を参照してください)。

つまりのままにする必要があります、`MasterPageFile`属性、`@Page`ディレクティブを Visual Studio での豊富なデザイン時機能をご利用いただけます。


[![Visual Studio で使用、@Pageデザイン ビューを表示するために、ディレクティブの MasterPageFile 属性](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**図 02**: Visual Studio を使用して、`@Page`ディレクティブの`MasterPageFile`デザイン ビューを表示する属性 ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>手順 3: 代替のマスター ページを作成します。

実行時に、コンテンツ ページのマスター ページをプログラムで設定できるため、外部条件に基づいた特定のマスター ページを動的に読み込むことです。 この機能は、サイトのレイアウトをする必要がある異なるユーザーに基づく場合に便利です。 たとえば、ブログ エンジン web アプリケーションのユーザーに許可できます、ブログのレイアウトを選択各レイアウトは別のマスター ページに関連付けられてます。 実行時に、訪問者が、ユーザーのブログを表示するときに web アプリケーション必要があります、ブログのレイアウトを確認し、コンテンツ ページに、対応するマスター ページを動的に関連付けます。

外部の条件に基づいて実行時に、マスター ページを動的に読み込む方法を調べてみましょう。 当社の web サイトには現在 1 つのマスター ページが含まれています (`Site.master`)。 実行時にマスター ページの選択を説明するために別のマスター ページが必要です。 この手順は、作成して、新しいマスター ページの構成について説明します。 手順 4 では、実行時に使用するどのようなマスター ページを判断するために検索します。

という名前のルート フォルダーに新しいマスター ページを作成`Alternate.master`です。 新しいスタイル シートをという名前の web サイトにも追加`AlternateStyles.css`です。


[![もう 1 つ追加の web サイトにマスター ページおよび CSS ファイル](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**図 03**: 別のマスター ページの追加と、web サイトに CSS ファイル ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image9.png))


設計すれば、`Alternate.master`マスター ページにあるネイビー カラーのバック グラウンドで中央揃え、ページの上部に表示されるタイトルです。 左の列の割り当てを下にあるそのコンテンツを移動したすれば、 `MainContent` ContentPlaceHolder コントロールは、ページの幅全体にまたがるようになりました。 さらに、順序なしのレッスンのリストを nixed して上記の水平方向の一覧に置き換え`MainContent`です。 フォントおよびマスター ページで (および、そのコンテンツ ページの拡張機能) を使用する色を更新しました。 図 4 は`Default.aspx`を使用する場合、`Alternate.master`マスター ページ。

> [!NOTE]
> ASP.NET には、定義する機能が含まれています。*テーマ*です。 テーマは、イメージ、CSS ファイル、およびスタイルに関連する Web コントロール プロパティの設定時にページに適用できるのコレクションです。 テーマは、および、CSS 規則によって表示される画像にのみ、サイトのレイアウトが異なる場合に移動する方法です。 別の Web コントロールを使用して、根本的に異なるレイアウトを持つなど、実質的に複数のレイアウトが異なる場合は、個別のマスター ページを使用する必要があります。 テーマの詳細については、このチュートリアルの最後には、関連項目」セクションを参照してください。


[![コンテンツ ページ、使用できるように新しいルック アンド フィール](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**図 04**: コンテンツのページは、使用できるように新しいルック アンド フィール ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image12.png))


マスターとコンテンツのページのマークアップを定着すると、ときに、`MasterPage`クラスのすべてのコンテンツをことを確認するためのチェック コンテンツ ページにコントロールがマスター ページ プレース ホルダーを参照します。 存在しない ContentPlaceHolder を参照するコンテンツ コントロールが見つかった場合は、例外がスローされます。 つまり、コンテンツ ページに割り当てられているマスター ページにある各、ContentPlaceHolder 命令型は、コンテンツ ページ内のコントロールのコンテンツします。

`Site.master`マスター ページには、4 つのプレース ホルダー コントロールが含まれています。

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

当社の web サイトのコンテンツ ページのものが 1 つか 2 つのコンテンツ コントロールです。他のユーザーには、使用可能な contentplaceholders の各コンテンツ コントロールが含まれます。 場合、新しいマスター ページ (`Alternate.master`) これまでに contentplaceholders には、すべてのコンテンツ コントロールがあるこれらのコンテンツ ページを割り当てることができます`Site.master`を`Alternate.master`として同じレイアウトを得ることがあります`Site.master`.

取得する、`Alternate.master`マスター ページ (図 4 を参照) が広くで、マスター ページのスタイルを定義することによって開始するように、`AlternateStyles.css`スタイル シートです。 次の規則を追加`AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

次に示す宣言型マークアップを次に、追加`Alternate.master`です。 ご覧のとおり、`Alternate.master`を同じ 4 つのプレース ホルダー コントロールが含まれています`ID`ContentPlaceHolder のコントロールと値`Site.master`です。 さらに、これは、これらのページ、web サイトで ASP.NET AJAX framework を使用するために必要な ScriptManager コントロールが含まれています。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>新しいマスター ページのテスト

この新しいマスター ページの更新プログラムをテストする、`BasePage`クラスの`OnPreInit`メソッドできるように、`MasterPageFile`プロパティの値が割り当てられた`"~/Alternate.maser"`し、web サイトにアクセスします。 すべてのページが 2 つを除くエラーなし機能する必要があります:`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`です。 DetailsView で製品を追加`~/Admin/AddProduct.aspx`結果、`NullReferenceException`マスター ページの設定を試みるコードの行から`GridMessageText`プロパティです。 アクセスすると`~/Admin/Products.aspx`、`InvalidCastException`メッセージ ページの読み込みでスローされます:"型のオブジェクトはキャストできません 'ASP.alternate\_マスター' 型に ' ASP.site\_マスター '。"。

これらのエラーが発生する、`Site.master`分離コード クラスには、パブリック イベント、プロパティ、およびで定義されていないメソッドが含まれています。`Alternate.master`です。 これら 2 つのページのマークアップの部分である、`@MasterType`を参照するディレクティブ、`Site.master`マスター ページ。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

また、DetailsView の`ItemInserted`内のイベント ハンドラー `~/Admin/AddProduct.aspx` 、厳密に型をキャストするコードが含まれています`Page.Master`プロパティ型のオブジェクトを`Site`です。 `@MasterType`ディレクティブ (この方法を使用) とのキャスト、`ItemInserted`イベント ハンドラーを密に結合、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページを`Site.master`マスター ページ。

あってもこの密結合を中断する`Site.master`と`Alternate.master`のパブリック メンバーの定義が含まれる共通の基本クラスから派生します。 次に、更新することが、`@MasterType`この共通の基本型を参照するディレクティブ。

### <a name="creating-a-custom-base-master-page-class"></a>カスタム ベースのマスター ページ クラスを作成します。

新しいクラス ファイルを追加、`App_Code`という名前のフォルダー`BaseMasterPage.vb`をから派生して`System.Web.UI.MasterPage`です。 定義する必要があります、`RefreshRecentProductsGrid`メソッドおよび`GridMessageText`プロパティ`BaseMasterPage`、そこからを単に移動できませんが、`Site.master`これらのメンバーに固有の Web コントロールを操作するため、`Site.master`マスター ページ (、 `RecentProducts`GridView と`GridMessage`ラベル)。

構成を行う必要があります`BaseMasterPage`これらのメンバーが、定義されますが、によって実際に実装されているように`BaseMasterPage`派生クラス (`Site.master`と`Alternate.master`)。 この継承の種類がクラスとしてマークすることによって可能な`MustInherit`とそのメンバーとして`MustOverride`です。 つまり、クラスとその 2 つのメンバーにこれらのキーワードを追加することを発表を`BaseMasterPage`実装いない`RefreshRecentProductsGrid`と`GridMessageText`が、その派生クラスはします。

定義する必要があります、`PricesDoubled`でイベント`BaseMasterPage`し、イベントを発生させる、派生クラスによって手段を提供します。 この動作を容易にするために、.NET Framework で使用されるパターンでは、基本クラスにパブリック イベントを作成し、という名前の保護されている、オーバーライド可能なメソッドを追加`OnEventName`です。 派生クラスでは、イベントを発生させるには、このメソッドを呼び出すことができますし、またはイベントが発生した後、または直前にコードを実行するようにオーバーライドできます。

更新プログラム、`BaseMasterPage`クラスの次のコードが含まれるようにします。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

次に移動、`Site.master`分離コード クラスをから派生して`BaseMasterPage`です。 `BaseMasterPage`マークされたメンバーを含む`MustOverride`をここでのメンバーをオーバーライドする必要があります`Site.master`です。 追加、`Overrides`キーワード、メソッドとプロパティの定義をします。 発生させるコードを更新しても、`PricesDoubled`内のイベント、`DoublePrice`ボタンの`Click`を呼び出して、基底クラスのイベント ハンドラー`OnPricesDoubled`メソッドです。

これらの変更後、`Site.master`分離コード クラスには、次のコードが含まれている必要があります。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

更新する必要があります`Alternate.master`の分離コード クラスから派生する`BaseMasterPage`、2 つのオーバーライドと`MustOverride`メンバー。 `Alternate.master`リストは、データベースに最新の製品や新製品の後にメッセージを表示するラベルを追加、これらのメソッドは必要がないことは何も操作する GridView が含まれていません。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>ベースのマスター ページ クラスを参照します。

これで完了しました、`BaseMasterPage`クラスし拡張すること、2 つのマスター ページ、最後の手順が更新するには、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`この共通の型を参照するページ。 変更することで開始、`@MasterType`ディレクティブから両方のページで。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

移動先:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

ファイルのパスを参照するのではなく、`@MasterType`プロパティは、基本データ型を参照するよう (`BaseMasterPage`)。 その結果、厳密に型指定された`Master`両方のページの分離コード クラスで使用されるプロパティは、現在の型の`BaseMasterPage`(型ではなく`Site`)。 この場所の変更と再表示`~/Admin/Products.aspx`です。 以前は、このエラーが発生しました、キャスト、ページを使用して構成されているため、`Alternate.master`マスター ページですが、`@MasterType`参照ディレクティブ、`Site.master`ファイル。 エラーが発生せず、ページが表示されるようになりました。 これは、ため、`Alternate.master`マスター ページは、型のオブジェクトにキャストできる`BaseMasterPage`(これは、それを拡張) してからです。

実行する必要がある 1 つの小さな変更がある`~/Admin/AddProduct.aspx`です。 DetailsView コントロールの`ItemInserted`両方、厳密に型指定されたイベント ハンドラーを使用して`Master`プロパティと、厳密に型`Page.Master`プロパティです。 更新されたときに、厳密に型指定された参照を修正しました、`@MasterType`ディレクティブ、ですが厳密に型参照を更新する必要があります。 次のコード行に置き換えます。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

キャストすると、次のように`Page.Master`基本型にします。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>手順 4: コンテンツのページへのバインドには、どのようなマスター ページを決定します。

当社`BasePage`クラスは、すべてのコンテンツ ページを現在設定`MasterPageFile`プロパティ ページのライフ サイクルの PreInit 段階ではハード コーディングされた値にします。 このコードの外部要素をマスター ページを基に更新することができます。 おそらく、マスター ページの読み込みには、現在ログオンしているユーザーの基本設定に依存します。 コードを記述する必要があります、その場合は、`OnPreInit`メソッド`BasePage`現在アクセスしたユーザーのマスター ページの基本設定を検索します。

使用するマスター ページを選択できる web ページを作成しましょう`Site.master`または`Alternate.master`-セッション変数にこの選択を保存します。 まず、という名前のルート ディレクトリに新しい web ページを作成して`ChooseMasterPage.aspx`です。 このページ (またはその他のコンテンツ ページ以降) を作成するときにないマスター ページをプログラムで設定されているため、マスター ページにバインドする必要があります。`BasePage`です。 ただし、マスター ページに新しいページをバインドしない場合、新しいページの既定の宣言型マークアップが含まれています、Web フォームおよびマスター ページで指定されたその他のコンテンツ。 適切なコンテンツ コントロールをこのマークアップを手動で交換する必要があります。 そのため、ほうが簡単にマスター ページに新しい ASP.NET ページをバインドします。

> [!NOTE]
> `Site.master`と`Alternate.master`同じ設定した ContentPlaceHolder コントロールの新しいコンテンツ ページを作成するときに選択するどのようなマスター ページは重要でありません。 一貫性のことをお勧めを使用して`Site.master`です。


[![Web サイトに新しいコンテンツ ページを追加します。](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**図 05**: web サイトに新しいコンテンツ ページを追加 ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image15.png))


更新プログラム、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`マスター ページや ASP.NET AJAX のレッスン。


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

コンテンツを追加する前に、`ChooseMasterPage.aspx`から派生するように、ページの分離コード クラスを更新する ページですぐ`BasePage`(なく`System.Web.UI.Page`)。 DropDownList コントロール、ページを追加、設定を次に、その`ID`プロパティを`MasterPageChoice`で 2 つのリスト項目を追加し、`Text`の値"~/Site.master"と"~/Alternate.master"です。

Button Web コントロールをページに追加し、設定、`ID`と`Text`プロパティ`SaveLayout`と「保存レイアウトの選択」、それぞれします。 この時点で、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

ページが初めてアクセスしたときに、ユーザーの現在選択されているマスター ページの選択を表示する必要があります。 作成、`Page_Load`イベント ハンドラーを次のコードを追加します。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

上記のコードは、最初のページ アクセスでのみ (および以降のポストバックではなく) を実行します。 かどうかをまずセッション変数`MyMasterPage`が存在します。 一致するリスト アイテムを検索する場合はその、 `MasterPageChoice` DropDownList です。 一致する ListItem が見つかった場合、その`Selected`プロパティに設定されている`True`です。

コードにユーザーの選択を保存する必要があります、`MyMasterPage`セッション変数。 イベント ハンドラーを作成、`SaveLayout`ボタンの`Click`イベントし、次のコードを追加します。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> 時間によって、`Click`ポストバック時にイベント ハンドラーが実行されると、マスター ページは既に選択されています。 そのため、ユーザーのドロップダウン リストの選択はありませんが有効にまで、次のページを参照してください。 `Response.Redirect`強制的に再要求するブラウザー`ChooseMasterPage.aspx`です。


`ChooseMasterPage.aspx`ページが完了したら、最後のタスクは、こと`BasePage`を割り当てる、`MasterPageFile`プロパティの値に基づきます、`MyMasterPage`セッション変数。 セッション変数が設定されていない場合は`BasePage`既定`Site.master`です。


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> 割り当てるコードを移動したら、`Page`オブジェクトの`MasterPageFile`プロパティのうち、`OnPreInit`イベント ハンドラーと 2 つの異なるメソッドにします。 この最初のメソッドで`SetMasterPageFile`、代入、`MasterPageFile`プロパティを 2 番目のメソッドによって返される値に`GetMasterPageFileFromSession`です。 マークは、`SetMasterPageFile`メソッド`Overridable`今後のクラスを拡張するように`BasePage`必要に応じて、カスタム ロジックを実装するように必要に応じてオーバーライドできます。 オーバーライドする例を見てみましょう`BasePage`の`SetMasterPageFile`次のチュートリアルではプロパティです。


配置でこのコードでは、次を参照してください。、`ChooseMasterPage.aspx`ページ。 最初に、`Site.master`マスター ページ選択 (図 6 参照)、ですが、ユーザーがドロップダウン リストから別のマスター ページを選択できます。


[![Site.master マスター ページを使用してコンテンツ ページが表示されます。](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**図 06**: コンテンツのページは、表示を使用して、`Site.master`マスター ページ ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![コンテンツ ページを表示 Alternate.master マスター ページを使用するようになりました](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**図 07**: コンテンツのページは、表示を使用するようになりました、`Alternate.master`マスター ページ ([フルサイズのイメージを表示するをクリックして](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>まとめ

コンテンツ ページをアクセスすると、そのコンテンツ コントロールは、マスター ページ プレース ホルダー コントロールと: fused です。 コンテンツ ページのマスター ページがで示される、`Page`クラスの`MasterPageFile`プロパティに割り当てられる、`@Page`ディレクティブの`MasterPageFile`初期化段階での属性です。 このチュートリアルに値を割り当てることで見たとして、 `MasterPageFile` PreInit ステージの終了前に行うように限りプロパティです。 マスター ページ、プログラムで指定できることには、外部要因に基づくマスター ページに、コンテンツ ページの動的な連結などの高度なシナリオのドアが表示されます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET ページ ライフ サイクル ダイアグラム](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET ページ ライフ サイクルの概要](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET のテーマとスキンの概要](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [ASP.NET のテーマ](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Suchi Banerjee しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](master-pages-and-asp-net-ajax-vb.md)
[次へ](nested-master-pages-vb.md)
