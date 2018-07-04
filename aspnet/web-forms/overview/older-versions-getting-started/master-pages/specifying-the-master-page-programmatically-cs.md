---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: プログラムでマスター ページを指定する (c#) |Microsoft Docs
author: rick-anderson
description: PreInit イベント ハンドラーを使用してプログラムでマスター ページのコンテンツ ページの設定を見ます。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b3fbd95a8088e20382c426fcdcc6a4b6e67d44b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370391"
---
<a name="specifying-the-master-page-programmatically-c"></a>プログラムでマスター ページを指定する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> PreInit イベント ハンドラーを使用してプログラムでマスター ページのコンテンツ ページの設定を見ます。


## <a name="introduction"></a>はじめに

以降の例では「 [ *、サイト全体レイアウトを使用してマスター ページを作成する*](creating-a-site-wide-layout-using-master-pages-cs.md)すべてのコンテンツ ページ宣言で使用して、マスター ページを参照している`MasterPageFile`属性`@Page`ディレクティブ。 たとえば、次`@Page`ディレクティブは、マスター ページに [コンテンツ] ページをリンク`Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

[ `Page`クラス](https://msdn.microsoft.com/library/system.web.ui.page.aspx)で、`System.Web.UI`名前空間が含まれています、 [ `MasterPageFile`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx)コンテンツ ページのマスター ページに、パスを返すは、この、によって設定されるプロパティです`@Page`ディレクティブ。 このプロパティは、プログラムによってコンテンツ ページのマスター ページを指定することも使用できます。 この方法は、ページにアクセスするユーザーなどの外部要因に基づくマスター ページを動的に割り当てる場合に便利です。

このチュートリアルでは、web サイトに 2 つ目のマスター ページの追加を実行時に使用するマスター ページを動的に決定します。

## <a name="step-1-a-look-at-the-page-lifecycle"></a>手順 1: を見て、ページ ライフ サイクル

ページのコンテンツ ページの ASP.NET ページ用の web サーバーに要求が到着すると、ときに、ASP.NET エンジンする必要があります fuse のマスター ページにコントロールのプレース ホルダー コントロールに対応するコンテンツ。 この fusion は、一般的なページ ライフ サイクルの手順を実行できる 1 つのコントロール階層を作成します。

図 1 は、この fusion を示しています。 図 1 の手順 1 では、初期コンテンツとマスター ページのコントロール階層を示します。 PreInit ステージのコンテンツの末尾には、ページ内のコントロールは、マスター ページ (ステップ 2) に対応する ContentPlaceHolders に追加されます。 この fusion の場合後、は、マスター ページが融合型のコントロール階層のルートとして機能します。 コントロール組み合わされ、この階層は、完成したコントロールの階層 (手順 3) を生成するために、ページに追加されます。 最終的な結果は、ページのコントロール階層に融合型のコントロール階層が含まれています。


[![PreInit ステージ中にマスター ページとコンテンツ ページのコントロール階層が一緒に組み合わされ、](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**図 01**: PreInit ステージ中に、マスター ページとコンテンツのページのコントロール階層が組み合わされ、まとめては ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image3.png))。


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>手順 2: 設定、`MasterPageFile`コードからプロパティ

値に依存マスター ページは、この fusion で partakes、`Page`オブジェクトの`MasterPageFile`プロパティ。 設定、`MasterPageFile`属性、`@Page`ディレクティブが割り当てることの実質的な効果、`Page`の`MasterPageFile`プロパティ ページのライフ サイクルの最初の段階である初期化ステージ時にします。 またはこのプロパティをプログラムで設定できます。 ただし、図 1 に fusion が発生する前にこのプロパティを設定することが不可欠です。

PreInit ステージの開始時、`Page`オブジェクトその[`PreInit`イベント](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx)呼び出しとその[`OnPreInit`メソッド](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx)します。 マスター ページをプログラムで設定するし作成するかのイベント ハンドラー、`PreInit`イベントまたはオーバーライド、`OnPreInit`メソッド。 どちらの方法を見てみましょう。

開いて開始`Default.aspx.cs`、私たちのサイトのホーム ページの分離コード クラス ファイル。 ページのイベント ハンドラーを追加`PreInit`次のコードを入力してイベント。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

ここで設定できる、`MasterPageFile`プロパティ。 値を代入するために、コードを更新"~/Site.master"に、`MasterPageFile`プロパティ。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

場合、ブレークポイントを設定して、デバッグの開始が表示されますたびに、`Default.aspx`ページにアクセスまたはこのページへのポストバックが存在する場合は、`Page_PreInit`イベント ハンドラーが実行されると、`MasterPageFile`にプロパティが割り当てられている"~/Site.master"。

また、オーバーライド、`Page`クラスの`OnPreInit`メソッドとセット、`MasterPageFile`プロパティがあります。 この例では、みましょう設定されていない、特定のページでマスター ページからのものではなく`BasePage`します。 ベース ページのカスタム クラスが作成されたことを思い出してください (`BasePage`) に戻り、 [*マスター ページのタイトル、メタ タグ、およびその他の HTML ヘッダーを指定する*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)チュートリアル。 現在`BasePage`オーバーライド、`Page`クラスの`OnLoadComplete`メソッド、ページの設定`Title`プロパティは、サイト マップ データに基づきます。 更新`BasePage`もオーバーライドする、`OnPreInit`メソッドをプログラムでマスター ページを指定します。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

すべてのコンテンツ ページが派生するため`BasePage`、それらのすべてのようになりましたがあるプログラムによって割り当てられている、マスター ページ。 この時点で、`PreInit`内のイベント ハンドラー`Default.aspx.cs`余分なは、自由に削除します。

### <a name="what-about-thepagedirective"></a>では、`@Page`ディレクティブでしょうか。

どのような少し混乱を招く可能性がありますはコンテンツ ページの`MasterPageFile`プロパティが 2 つの場所で指定されているようになりました: でプログラムによって、`BasePage`クラスの`OnPreInit`メソッドによっても、`MasterPageFile`各コンテンツ ページ内の属性`@Page`ディレクティブ。

ページのライフ サイクルの最初のステージは、初期化段階です。 このステージでは、`Page`オブジェクトの`MasterPageFile`プロパティには、値が割り当てられている、`MasterPageFile`属性、`@Page`ディレクティブ (提供されている) 場合。 初期化ステージでの次の PreInit 段階とはプログラムで設定をここで、`Page`オブジェクトの`MasterPageFile`から割り当てられた値を上書きして、プロパティ、`@Page`ディレクティブ。 設定するため、`Page`オブジェクトの`MasterPageFile`プロパティをプログラムで削除することでした、`MasterPageFile`属性を`@Page`エンドユーザーのエクスペリエンスに影響を与えずにディレクティブ。 自分でこのことをさあ、削除、`MasterPageFile`属性を`@Page`ディレクティブで`Default.aspx`し、ブラウザーを使用してページにアクセスします。 次のものを予想どおり、出力は、同じ属性が削除される前に。

かどうか、`MasterPageFile`プロパティを使用して、`@Page`ディレクティブ プログラムでは、エンドユーザーのエクスペリエンスに派生的損害またはします。 ただし、`MasterPageFile`属性、 `@Page` WYSIWYG を生成するためにデザイン時に、ディレクティブが Visual Studio によって使用される、デザイナーで表示します。 戻る場合`Default.aspx`Visual Studio で、移動、デザイナーには、メッセージが表示されます"マスター ページのエラー: ページがマスター ページの参照を必要とするコントロールが指定されていない"(図 2 参照)。

つまりのままにする必要があります、`MasterPageFile`属性、 `@Page` Visual Studio でデザイン時の豊富なエクスペリエンスをご利用いただくにディレクティブ。


[![Visual Studio の使用、@Pageデザイン ビューをレンダリングするディレクティブの MasterPageFile 属性](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**図 02**: Visual Studio を使用して、`@Page`ディレクティブの`MasterPageFile`デザイン ビューを表示する属性 ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image6.png))。


## <a name="step-3-creating-an-alternative-master-page"></a>手順 3: 代わりにマスター ページの作成

マスター ページのコンテンツ ページの実行時にプログラムで設定するためには、外部条件に基づいて、特定のマスター ページを動的に読み込むこともできますが。 この機能は、場所、サイトのレイアウトに基づいて必要がありますを変更するため、ユーザーの状況で役立ちます。 たとえば、ブログ エンジンの web アプリケーションのユーザーに許可できます、ブログのレイアウトを選択各レイアウトが別のマスター ページに関連付けられています。 、実行時に、訪問者が、ユーザーのブログを表示するときに、web アプリケーションはブログのレイアウトを決定をコンテンツ ページと対応するマスター ページを動的に関連付ける必要。

外部の条件に基づいて実行時にマスター ページを動的に読み込む方法を見てみましょう。 Web サイトには現在 1 つのマスター ページが含まれています (`Site.master`)。 別のマスター ページを示す実行時にマスター ページを選択する必要があります。 この手順は、作成して、新しいマスター ページの構成について説明します。 手順 4 では、実行時に使用するマスター ページを判断するために検索します。

という名前のルート フォルダーに新しいマスター ページを作成`Alternate.master`です。 新しいスタイル シートをという名前の web サイトに追加することも`AlternateStyles.css`します。


[![もう 1 つ追加マスター ページおよび CSS ファイル、web サイトを](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**図 03**: 別のマスター ページを追加して、web サイトに CSS ファイル ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image9.png))。


設計した、`Alternate.master`マスター ページとネイビー カラーの背景に中央揃え、ページの上部に表示されるタイトルにします。 左の列の管理されているして下にあるそのコンテンツを移動、`MainContent`プレース ホルダー コントロールは、ページの幅全体にまたがるようになりました。 さらに、順序なしのレッスンのリストを nixed し、上記の水平方向のリストに置き換え`MainContent`します。 フォントおよびマスター ページで、拡張機能、そのコンテンツのページで) を使用する色も更新しました。 図 4 は`Default.aspx`を使用する場合、`Alternate.master`マスター ページ。

> [!NOTE]
> ASP.NET には、定義する機能が含まれています。*テーマ*します。 テーマは、一連のイメージ、CSS ファイル、およびスタイルに関連する Web コントロール プロパティの設定時にページに適用できます。 テーマは、サイトのレイアウトには、表示される画像でのみ、し、CSS 規則が異なる場合に移動する方法です。 など、別の Web コントロールを使用したり、根本的に異なるレイアウトを大幅により、レイアウトが異なる場合は、別のマスター ページを使用する必要があります。 テーマの詳細については、このチュートリアルの最後に、関連項目」セクションを参照してください。


[![このコンテンツ ページは新しいルック アンド フィールを使えるようになりました](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**図 04**: コンテンツ ページは新しいルック アンド フィールを使えるようになりました ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image12.png))。


マスターおよびコンテンツのページのマークアップは組み合わされ、ときに、`MasterPage`クラスのすべてのコンテンツをことを確認するコンテンツ ページのコントロールは、マスター ページで ContentPlaceHolder を参照します。 存在しない ContentPlaceHolder を参照するコンテンツ コントロールが見つかった場合は、例外がスローされます。 つまり、命令型のコンテンツ ページに割り当てられているマスター ページにあるごとに、プレース ホルダーは、コンテンツ ページでコントロールのコンテンツします。

`Site.master`マスター ページには、次の 4 つのプレース ホルダー コントロールが含まれています。

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

1 つか 2 つのコンテンツ コントロールを含めるいくつかの web サイトのコンテンツ ページ他のユーザーには、使用可能な ContentPlaceHolders の各コンテンツ コントロールが含まれます。 場合、新しいマスター ページ (`Alternate.master`) これまでの ContentPlaceHolders のすべてのコンテンツ コントロールを与えているコンテンツ ページを割り当てることができます`Site.master`ことが重要ですしを`Alternate.master`として同じレイアウトを得ることがあります`Site.master`.

取得する、`Alternate.master`マスター ページ (図 4 参照) をマイニングは、マスター ページのスタイルを定義することによって開始のように、`AlternateStyles.css`スタイル シート。 次の規則を追加`AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

次に示す宣言型マークアップを次に、追加`Alternate.master`します。 ご覧のとおり、`Alternate.master`と同じ 4 つのプレース ホルダー コントロールが含まれています`ID`値内のプレース ホルダー コントロールとして`Site.master`します。 さらに、これらのページ、web サイトで ASP.NET AJAX フレームワークを使用するために必要なである ScriptManager コントロールが含まれています。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>新しいマスター ページのテスト

この新しいマスター ページの更新プログラムをテストする、`BasePage`クラスの`OnPreInit`メソッドように、`MasterPageFile`プロパティの値が割り当てられている"~/Alternate.master"し、web サイトにアクセスします。 すべてのページが 2 つを除くエラーなし機能する必要があります:`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`します。 DetailsView に製品を追加する`~/Admin/AddProduct.aspx`で結果を`NullReferenceException`、マスター ページを設定しようとするコード行から`GridMessageText`プロパティ。 アクセスすると`~/Admin/Products.aspx`、`InvalidCastException`メッセージ ページの読み込み時にスローされる:"型のオブジェクトをキャストできません 'ASP.alternate\_マスター' 型に ' ASP.site\_マスター '。"。

これらのエラーが発生する、`Site.master`分離コード クラスには、パブリック イベント、プロパティ、およびで定義されていないメソッドが含まれています。`Alternate.master`します。 これら 2 つのページのマークアップの部分である、`@MasterType`を参照するディレクティブ、`Site.master`マスター ページ。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

また、DetailsView の`ItemInserted`内のイベント ハンドラー `~/Admin/AddProduct.aspx` 、厳密に型をキャストするコードが含まれる`Page.Master`プロパティ型のオブジェクトを`Site`します。 `@MasterType`ディレクティブ (この方法を使用) と、キャスト、`ItemInserted`イベント ハンドラーを密に結合、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページを`Site.master`マスター ページ。

あってもこの密結合を中断する`Site.master`と`Alternate.master`パブリック メンバーの定義を含む共通の基本クラスから派生します。 次に、今後更新できる、`@MasterType`この共通の基本型を参照するディレクティブ。

### <a name="creating-a-custom-base-master-page-class"></a>カスタム ベースのマスター ページ クラスを作成します。

新しいクラス ファイルを追加、`App_Code`という名前のフォルダー`BaseMasterPage.cs`から派生すること`System.Web.UI.MasterPage`します。 定義する必要があります、`RefreshRecentProductsGrid`メソッドと`GridMessageText`プロパティ`BaseMasterPage`からそこに移動できませんだけですが、`Site.master`これらのメンバーに固有の Web コントロールを操作するため、`Site.master`マスター ページ (、 `RecentProducts`GridView と`GridMessage`ラベル)。

構成を行う必要がありますが`BaseMasterPage`これらのメンバーは、定義されているが、によって実際に実装される方法で`BaseMasterPage`クラスの派生 (`Site.master`と`Alternate.master`)。 クラスとそのメンバーとしてマークすることでこの種類の継承が可能な`abstract`します。 つまり、追加、`abstract`これら 2 つのメンバーにキーワードを発表する`BaseMasterPage`実装いない`RefreshRecentProductsGrid`と`GridMessageText`いるものの、その派生クラスは、します。

定義する必要があります、`PricesDoubled`でイベント`BaseMasterPage`イベントを発生させる、派生クラスによって手段を提供します。 この動作を容易にするために、.NET Framework で使用されるパターンは、基本クラスのパブリック イベントを作成し、保護を追加する`virtual`という名前のメソッド`OnEventName`します。 派生クラスでは、イベントを発生させるには、このメソッドを呼び出すことができますし、またはイベントが発生した後、または直前にコードを実行するようにオーバーライドできます。

更新プログラム、`BaseMasterPage`クラスに次のコードが含まれています。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

次に移動、`Site.master`コード分離し、クラスから派生`BaseMasterPage`します。 `BaseMasterPage`は`abstract`をオーバーライドする必要があります`abstract`でここにメンバー`Site.master`します。 追加、`override`メソッドおよびプロパティ定義のキーワード。 発生させるコードを更新しても、`PricesDoubled`内のイベント、`DoublePrice`ボタンの`Click`イベント ハンドラーを基本クラスの呼び出しで`OnPricesDoubled`メソッド。

これらの変更後、`Site.master`分離コード クラスは、次のコードを含める必要があります。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

更新する必要もあります`Alternate.master`の分離コード クラスから派生する`BaseMasterPage`、2 つのオーバーライドと`abstract`メンバー。 しかし`Alternate.master`リストが最新の製品や新製品の後にメッセージを表示するラベルは、データベースに追加されて、これらのメソッドは必要がないこと何もする GridView が含まれていません。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>ベースのマスター ページのクラスを参照します。

これで完了しました、`BaseMasterPage`クラスおよび拡張すること、2 つのマスター ページ、最後の手順が更新するには、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`この一般的な型を参照するページ。 変更することで開始、`@MasterType`から両方のページ ディレクティブ。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

移動先:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

ファイルのパスを参照するのではなく、`@MasterType`プロパティでは、基本データ型を参照 (`BaseMasterPage`)。 その結果、厳密に型指定された`Master`両方のページの分離コード クラスで使用されるプロパティは、現在の型の`BaseMasterPage`(型ではなく`Site`)。 この場所の変更を再検討`~/Admin/Products.aspx`します。 以前は、このエラーが発生した、キャスト、ページを使用して構成されているため、`Alternate.master`マスター ページですが、`@MasterType`参照ディレクティブ、`Site.master`ファイル。 エラーが発生せず、ページをレンダリングするようになりました。 これは、ため、`Alternate.master`マスター ページは、型のオブジェクトにキャストできます`BaseMasterPage`(からの拡張と)。

作成する必要がある 1 つの小さな変更がある`~/Admin/AddProduct.aspx`します。 DetailsView コントロールの`ItemInserted`両方、厳密に型指定されたイベント ハンドラーを使用して`Master`プロパティと、厳密に型`Page.Master`プロパティ。 更新されたときに、厳密に型指定された参照を修正しました、`@MasterType`ディレクティブが疎に型指定された参照を更新する必要がまだします。 次のコード行に置き換えます。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

キャストが、次のよう`Page.Master`基本型にします。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>手順 4: コンテンツのページにバインドするには、どのようなマスター ページを決定します。

この`BasePage`クラスは、すべてのコンテンツ ページを現在設定`MasterPageFile`プロパティ ページのライフ サイクルの PreInit ステージでハード コーディングされた値にします。 私たちは、外部要因でマスター ページを基に、このコードを更新できます。 おそらくマスター ページを読み込むには、現在ログオンしているユーザーの基本設定に依存します。 その場合は、コードを記述する必要は、`OnPreInit`メソッド`BasePage`現在アクセスしたユーザーのマスター ページの基本設定を検索します。

ユーザーが使用するマスター ページを選択できる web ページを作成しましょう`Site.master`または`Alternate.master`-セッション変数にこの選択を保存します。 という名前のルート ディレクトリで新しい web ページを作成して開始`ChooseMasterPage.aspx`します。 このページ (またはその他のコンテンツ ページ以下) を作成するときに、マスター ページは、プログラムで設定されているため、マスター ページにバインドする必要はありません`BasePage`します。 ただし、マスター ページに新しいページをバインドしない場合、新しいページの既定の宣言型マークアップが含まれています、Web フォームとマスター ページで指定されたその他のコンテンツ。 手動でこのマークアップを適切なコンテンツ コントロールに置き換える必要があります。 そのため、方が簡単に新しい ASP.NET ページをマスター ページにバインドします。

> [!NOTE]
> `Site.master`と`Alternate.master`が同じ設定のプレース ホルダー コントロールのどのようなマスター ページを新しいコンテンツ ページを作成するときに選択するかは関係ありません。 一貫性を保つのためにそちらを使用して`Site.master`します。


[![新しいコンテンツ ページ、web サイトを追加します。](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**図 05**: web サイトに新しいコンテンツ ページを追加 ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image15.png))。


更新プログラム、`Web.sitemap`このレッスンのエントリを追加するファイル。 下に次のマークアップを追加、`<siteMapNode>`マスター ページと ASP.NET AJAX のレッスン。


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

任意のコンテンツを追加する前に、`ChooseMasterPage.aspx`ページから派生するように、ページの分離コード クラスを更新する少し`BasePage`(なく`System.Web.UI.Page`)。 DropDownList コントロールをページに追加、設定を次に、その`ID`プロパティを`MasterPageChoice`で 2 つのリスト項目を追加し、`Text`の値"~/Site.master"と"~/Alternate.master"。

ボタンの Web コントロールをページに追加し、設定、`ID`と`Text`プロパティを`SaveLayout`"保存レイアウト Choice"、およびそれぞれします。 この時点で、ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

ページが初めてアクセスしたときに、ユーザーの現在選択されているマスター ページの選択を表示する必要があります。 作成、`Page_Load`イベント ハンドラーを次のコードを追加。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

上記のコードでは、最初のページ アクセスでのみ (および以降のポストバックではなく) を実行します。 かどうかをまずセッション変数`MyMasterPage`存在します。 場合に一致する ListItem を探そうと、 `MasterPageChoice` DropDownList します。 一致する ListItem が見つかった場合、`Selected`プロパティに設定されて`true`します。

ユーザーの選択を保存するコードも必要があります、`MyMasterPage`セッション変数。 イベント ハンドラーを作成、`SaveLayout`ボタンの`Click`イベントと、次のコードを追加します。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> 時間によって、`Click`ポストバック時にイベント ハンドラーが実行される、マスター ページは既に選択されています。 そのため、ユーザーの一覧から選択有効になりませんまで、次のページを参照してください。 `Response.Redirect`強制的に再要求するブラウザー`ChooseMasterPage.aspx`します。


`ChooseMasterPage.aspx`が完全なページで、最後のタスクは`BasePage`割り当てる、`MasterPageFile`プロパティの値に基づいて、`MyMasterPage`セッション変数。 セッション変数が設定されていない場合がある`BasePage`既定`Site.master`します。


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> 代入するコードを移動、`Page`オブジェクトの`MasterPageFile`プロパティのうち、`OnPreInit`イベント ハンドラーと 2 つの異なるメソッドにします。 この最初のメソッドで`SetMasterPageFile`、代入、`MasterPageFile`プロパティを 2 番目のメソッドによって返される値に`GetMasterPageFileFromSession`します。 作成した、`SetMasterPageFile`メソッド`virtual`今後のクラスを拡張するように`BasePage`必要に応じて、カスタム ロジックを実装するために、必要に応じてオーバーライドできます。 オーバーライドする例を見て`BasePage`の`SetMasterPageFile`プロパティで、次のチュートリアル。


このコードを参照してください。、`ChooseMasterPage.aspx`ページ。 最初に、`Site.master`マスター ページが選択されていると (を図 6 参照) が、ユーザーがドロップダウン リストから別のマスター ページを選択できます。


[![Site.master マスター ページを使用してコンテンツ ページが表示されます。](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**図 06**: コンテンツ ページは表示を使用して、`Site.master`マスター ページ ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image18.png))。


[![コンテンツ ページが Alternate.master マスター ページを使用して表示されます。](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**図 07**: コンテンツ ページは表示を使用して今すぐ、`Alternate.master`マスター ページ ([フルサイズの画像を表示する をクリックします](specifying-the-master-page-programmatically-cs/_static/image21.png))。


## <a name="summary"></a>まとめ

コンテンツ ページがアクセスしたときに、コンテンツ コントロールはそのマスター ページの ContentPlaceHolder コントロールと組み合わされします。 コンテンツ ページのマスター ページがで示される、`Page`クラスの`MasterPageFile`プロパティに割り当てられる、`@Page`ディレクティブの`MasterPageFile`初期化段階での属性。 このチュートリアルでは示しました、値を割り当てることができますと、 `MasterPageFile` PreInit ステージの終了前に、これを行っている限りプロパティ。 プログラムでマスター ページを指定することには、外部要因に基づく、マスター ページにコンテンツ ページを動的にバインドするなどの高度なシナリオのドアが表示されます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET ページ ライフ サイクルの図](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [ASP.NET ページ ライフ サイクルの概要](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET のテーマおよびスキンの概要](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [マスター ページ: ヒント、テクニック、およびトラップ](http://www.odetocode.com/articles/450.aspx)
- [ASP.NET におけるテーマ](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者が、Suchi 著。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](master-pages-and-asp-net-ajax-cs.md)
> [次へ](nested-master-pages-cs.md)
