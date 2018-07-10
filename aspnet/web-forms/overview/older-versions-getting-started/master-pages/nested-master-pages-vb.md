---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: 入れ子になったマスター ページ (VB) |Microsoft Docs
author: rick-anderson
description: 別の 1 つのマスター ページを入れ子にする方法を示します。
ms.author: aspnetcontent
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e58eef87afce7d1b7e29445bdbf48baf5f65d3e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836263"
---
<a name="nested-master-pages-vb"></a>入れ子になったマスター ページ (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> 別の 1 つのマスター ページを入れ子にする方法を示します。


## <a name="introduction"></a>はじめに

過去の 9 つのチュートリアルの過程でマスター ページをサイト全体レイアウトを実装する方法を学習しました。 簡単に言うと、マスター ページを使用する、ページ開発者は、マスター ページ コンテンツのコンテンツ ページのページごとにカスタマイズ可能な特定のリージョンとの共通のマークアップを定義します。 マスター ページ内のプレース ホルダー コントロールが、カスタマイズ可能な領域を示しますContentPlaceHolder のコントロールのカスタマイズのマークアップは、コンテンツ コントロールを使用して、コンテンツ ページで定義されます。

ここまで紹介しましたマスター ページ テクニックは、サイト全体にわたって使用される 1 つのレイアウトがある場合に最適です。 ただし、多くの大規模な web サイトでは、さまざまなセクション間でカスタマイズされたサイト レイアウトがあります。 たとえば、病院スタッフ患者の情報、アクティビティ、および課金を管理するために使用する医療アプリケーションを検討してください。 このアプリケーションの web ページの 3 つの型である可能性があります。

- スタッフ メンバーが、可用性を更新できますスタッフ メンバー固有のページでは、スケジュールを表示または休暇時間を要求します。
- 患者固有のページ、スタッフのメンバの表示または特定の患者の情報を編集します。
- 経理担当者が現在確認して課金固有のページ要求のステータスと財務報告します。

すべてのページには、上部と下部に頻繁に使用されるリンクの一連の間でのメニューなどの一般的なレイアウトを共有可能性があります。 ただし、スタッフ、患者、および課金固有のページは、この汎用的なレイアウトをカスタマイズする必要があります。 たとえば、おそらくすべてスタッフに固有のページでは、予定表、タスクの一覧が、現在ログオンしているユーザーの可用性と毎日のスケジュールを示すを含める必要があります。 おそらく患者に固有のすべてのページでは、名前、アドレス、および情報の編集中は、患者の保険情報を表示する必要があります。

使用してこのようなカスタマイズされたレイアウトを作成することは*入れ子になったマスター ページ*します。 上記のシナリオを実装するためにサイト全体レイアウト、メニューとページ フッターのコンテンツをカスタマイズ可能な領域を定義する ContentPlaceHolders で定義されているマスター ページを作成して始めたいです。 次の 3 つ入れ子になったマスター ページ、web ページの種類ごとに 1 つは作成します。 入れ子になった各マスター ページには、マスター ページを使用するコンテンツ ページの種類の間でコンテンツを定義します。 つまり、患者に固有のコンテンツ ページの入れ子になったマスター ページは、マークアップと編集されている患者についての情報を表示するプログラム ロジックが含まれます。 新しい患者に固有のページを作成するときに、この入れ子になったマスター ページにバインドしますが。

このチュートリアルでは、入れ子になったマスター ページのメリットを強調表示して起動します。 これは、後作成し、入れ子になったマスター ページを使用する方法を示します。

> [!NOTE]
> 入れ子になったマスター ページは、.NET Framework のバージョン 2.0 以降、可能なされています。 ただし、Visual Studio 2005 では、入れ子になったマスター ページのデザイン時サポートが含まれませんでした。 良い知らせは、Visual Studio 2008 が入れ子になったマスター ページのデザイン時の豊富なエクスペリエンスを提供します。 入れ子になったマスター ページを使用して興味のある Visual Studio 2005 を使用している場合は、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ「 [VS 2005 のデザイン時入れ子になったマスター ページのヒント](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)します。


## <a name="the-benefits-of-nested-master-pages"></a>入れ子になったマスター ページの利点があります。

多くの web サイトがある、包括的なサイトのデザインとカスタマイズされたデザイン複数ページの特定の種類に固有です。 たとえば、デモ web アプリケーションで作成しました、初歩的な管理 セクション (ページ、`~/Admin`フォルダー)。 現在の web ページ、`~/Admin`フォルダーでは、同じマスター ページを使用して、[管理] セクションではなく、それらのページとして (つまり、`Site.master`または`Alternate.master`ユーザーの選択に応じて、)。

> [!NOTE]
> ここでは、私たちのサイトに 1 つのマスター ページに振る舞う`Site.master`します。 このチュートリアルの後半で 2 つ (以上) のマスター ページを使用して、「を使用して、入れ子になったマスター ページの [管理] セクション」で始まるの入れ子になったマスター ページを使用して対処します。


追加情報やリンクされていない場合は、サイトの他のページ内に存在する管理ページのレイアウトをカスタマイズするように依頼されたことを想像してください。 この要件を実装するために 4 つの手法があります。

1. すべてのコンテンツ ページに、管理に固有の情報とリンクを手動で追加、`~/Admin`フォルダー。
2. 更新プログラム、`Site.master`管理セクションに固有の情報とリンクを含めるし、これらのセクションを非表示のマスター ページにコードを追加するマスター ページで、[管理] ページのいずれかが参照されているかどうかに基づいて。
3. 新しいマスター ページを作成する専用の管理 セクションのコピーからマークアップ`Site.master`管理セクションに固有の情報とリンクを追加し、内のコンテンツ ページを更新し、`~/Admin`新しいマスターが使用するフォルダーページです。
4. バインドする入れ子になったマスター ページを作成`Site.master`にコンテンツ ページがあると、`~/Admin`入れ子にマスター ページをこの新しいフォルダーを使用します。 この入れ子になったマスター ページの追加情報と管理ページに固有のリンクだけが含まれますで既に定義されているマークアップを繰り返す必要はありません`Site.master`します。

最初のオプションは、少なくとも快適です。 マスター ページを使用しての全体のポイントは、手動でコピーして、新しい ASP.NET ページに共通のマークアップを貼り付けますから脱却するためです。 2 番目のオプションは、十分ですが、マークアップがごくまれにしか表示され、開発者をマスター ページの編集を覚えるときに、必要に、このマークアップを回避する必要がありますをマスター ページ、レイアウトやバルクと保守性が低下するため、アプリケーション正確には、特定のマークアップは、非表示になってときと表示されます。 このアプローチは、この単一のマスター ページでは対応に必要な web ページの詳細と詳細型のカスタマイズとして小さい tenable があります。

3 番目のオプションは、煩雑さを削除し、2 番目のオプションと、複雑さの問題、提示します。 ただし、3 つのオプションの主な欠点は、必要があることをコピーしてから、一般的なレイアウトを貼り付けること`Site.master`新しい管理セクションに固有のマスター ページにします。 後で 2 つの場所で変更する必要があるサイト全体レイアウトを変更する場合。

入れ子になったマスター ページを 4 番目のオプションの 2 番目と 3 番目のオプションの長所をお送りします。 特定のリージョンに固有のコンテンツが別のファイルに分割中に、1 つのファイルの最上位マスター ページ - サイト全体レイアウト情報が維持されます。

このチュートリアルを作成すると、単純な入れ子になったマスター ページを使用してから始まります。 新しい最上位マスター ページ、2 つの入れ子になったマスター ページ、および 2 つのコンテンツ ページを作成します。 以降「を使用して、入れ子になったマスター ページの [管理] セクション」では、入れ子になったマスター ページを使用するように、既存のマスター ページ アーキテクチャの更新について説明します。 具体的には、入れ子になったマスター ページを作成し、コンテンツ ページの追加のカスタム コンテンツを追加することを使用して、`~/Admin`フォルダー。

## <a name="step-1-creating-a-simple-top-level-master-page"></a>手順 1: 単純な最上位マスター ページの作成

入れ子になったマスターを作成するのいずれかに基づいて、既存のマスター ページと、既存のコンテンツ ページが既に特定予定ため複雑化する必要がありますし、最上位マスター ページではなく、この新しい入れ子になったマスター ページを使用する既存のコンテンツ ページを更新ContentPlaceHolder の最上位マスター ページで定義されているコントロール。 そのため、入れ子になったマスター ページは同じ名前を持つ同じレイアウトを得ることも含める必要があります。 特定のデモ アプリケーションの 2 つのマスター ページがさらに、(`Site.master`と`Alternate.master`) このような複雑さをさらに増加するユーザーの設定に基づいてコンテンツ ページに動的に割り当てられています。 このチュートリアルで後で入れ子になったマスター ページを使用する既存のアプリケーションを更新して見ていきますが、簡単な最初のフォーカスがマスター ページの例を入れ子にしてみましょう。

という名前の新しいフォルダーを作成する`NestedMasterPages`という名前のフォルダーに新しいマスター ページファイルを追加および`Simple.master`します。 (図 1 参照ソリューション エクスプ ローラーのスクリーン ショットの後、このフォルダーとファイルが追加されました。)ドラッグ、`AlternateStyles.css`デザイナーの ソリューション エクスプ ローラーからスタイル シート ファイル。 これを追加、`<link>`要素のスタイル シート ファイルを`<head>`要素の後に、マスター ページの`<head>`要素のマークアップのようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Web フォーム内で次のマークアップを次に、追加`Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

このマークアップでは、ネイビー カラーの背景に白のフォントを大規模にページの上部にある「入れ子になったマスター ページ (単純)」というリンクが表示されます。 下には、`MainContent`プレース ホルダーです。 図 1 は、 `Simple.master` Visual Studio デザイナーで読み込まれるときに、マスター ページ。


[![入れ子になったマスター ページ、[管理] ページに特定のコンテンツを定義します](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**図 01**:、入れ子になったマスター ページを定義コンテンツに固有の管理セクションのページ ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image3.png))。


## <a name="step-2-creating-a-simple-nested-master-page"></a>手順 2: 単純な入れ子になったマスター ページを作成します。

`Simple.master` 2 つのプレース ホルダー コントロールが含まれています。 `MainContent` ContentPlaceHolder と共に Web フォーム内で追加されました、`head`でプレース ホルダー、`<head>`要素。 コンテンツ ページを作成しにバインドする場合`Simple.master`コンテンツ ページで、2 つの ContentPlaceHolders を参照する 2 つのコンテンツ コントロールが必要があります。 同様に、入れ子になったマスター ページを作成し、バインドするかどうかは`Simple.master`コンテンツ コントロールを 2 つ持つ入れ子になったマスター ページになります。

追加する新しい入れ子になったマスター ページ、`NestedMasterPages`という名前のフォルダー`SimpleNested.master`します。 右クリックし、`NestedMasterPages`フォルダーと新しい項目の追加を選択します。 図 2 に示すように新しい項目の追加 ダイアログ ボックスが表示されます。 マスター ページ テンプレートの種類を選択して、新しいマスター ページの名前を入力します。 新しいマスター ページで、入れ子になったマスター ページがあることを示すには、「マスター ページの選択」チェック ボックスをオンします。

次に、[追加] ボタンをクリックします。 同じ Select マスター ページにコンテンツ ページをバインドする場合を参照してください、マスター ページ ダイアログ ボックスが表示されます (図 3 を参照してください)。 選択、`Simple.master`でマスター ページ、`NestedMasterPages`フォルダー [ok] をクリックします。

> [!NOTE]
> Web サイト プロジェクト モデルではなく、Web アプリケーション プロジェクト モデルを使用して ASP.NET web サイトを作成した場合は、図 2 に示すように新しい項目の追加 ダイアログ ボックスの"マスター ページの選択 チェック ボックスは表示されません。 Web アプリケーション プロジェクト モデルを使用する場合は、入れ子になったマスター ページを作成するには、(マスター ページ テンプレート) ではなく、入れ子になったマスター ページ テンプレートを選択する必要があります。 入れ子になったマスター ページ テンプレートを選択し、追加をクリックすると、図 3 に示すダイアログ ボックスが表示されますマスター ページの選択と同じです。


[![チェック、&quot;マスター ページを選択&quot;入れ子になったマスター ページを追加するチェック ボックス](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**図 02**: 入れ子になったマスター ページを追加する [マスター ページを選択する] チェック ボックスをオン ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image6.png))。


[![入れ子になったマスター ページを Simple.master マスター ページにバインドします。](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**図 03**: 入れ子になったマスター ページへのバインド、`Simple.master`マスター ページ ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image9.png))。


入れ子になったマスター ページの宣言型マークアップ次に示すにはには、マスター ページの最上位レベルの 2 つのプレース ホルダー コントロールを参照する 2 つのコンテンツ コントロールが含まれています。


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

を除き、`<%@ Master %>`入れ子になったマスター ページの最初の宣言型マークアップは、同じ最上位マスター ページにコンテンツ ページをバインドするときに最初に生成されたマークアップと同じですディレクティブ。 などのコンテンツ ページの`<%@ Page %>`ディレクティブ、`<%@ Master %>`ここディレクティブが含まれています、`MasterPageFile`入れ子になったマスター ページの親マスター ページを指定する属性。 入れ子になったマスター ページと同じ最上位マスター ページにバインドされたコンテンツ ページの主な違いは、入れ子になったマスター ページが ContentPlaceHolder のコントロールを含めることができます。 入れ子になったマスター ページのレイアウトを得ることは、コンテンツ ページがマークアップをカスタマイズできる領域を定義します。

テキスト「こんにちは, SimpleNested から!」を表示するように、この入れ子になったマスター ページを更新します。 対応するコンテンツ コントロールで、`MainContent`プレース ホルダー コントロール。


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

この追加した場合、入れ子になったマスター ページを保存しする場合は、新しいコンテンツ ページを追加し、`NestedMasterPages`という名前のフォルダー`Default.aspx`にバインドし、`SimpleNested.master`マスター ページ。 このページを追加すると驚かれるかもしれませんがないこと (図 4 参照) のコンテンツ コントロールを表示します。 コンテンツ ページにアクセスできるだけその*親*マスター ページの ContentPlaceHolders します。 `SimpleNested.master` ContentPlaceHolder コントロール; が含まれていません。そのため、このマスター ページにバインドされている任意のコンテンツ ページには、任意のコンテンツ コントロールを含めることはできません。


[![新しいコンテンツ ページにコンテンツ コントロールが含まれていません](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**図 04**:、新しいコンテンツ ページが含まれていますいいえコンテンツ コントロール ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image12.png))。


入れ子になったマスター ページの更新は行う必要があります (`SimpleNested.master`) プレース ホルダー コントロールを追加します。 通常、入れ子になったマスター ページにできるため、その子のマスター ページまたはコンテンツ ページ、最上位マスター ページの ContentPlaceHolder のいずれかを使用するようにその親のマスター ページで定義されている各の ContentPlaceHolder、プレース ホルダーを含める必要があります。コントロール。

更新プログラム、`SimpleNested.master`マスター ページにその 2 つのコンテンツ コントロールで、プレース ホルダーが含まれます。 プレース ホルダー コントロールのコンテンツ コントロールは、プレース ホルダー コントロールとして同じ名前を付けます。 つまりという名前のプレース ホルダー コントロールを追加`MainContent`コンテンツに制御`SimpleNested.master`を参照する、`MainContent`で ContentPlaceHolder`Simple.master`します。 参照するコンテンツ コントロールで同じ処理を行う、`head`プレース ホルダーです。

> [!NOTE]
> 入れ子になったマスター ページ内のプレース ホルダー コントロールの最上位マスター ページの ContentPlaceHolders と同じ名前付けお勧めしますが、この名前付けの対称性は必要ありません。 任意の名前、入れ子になったマスター ページで ContentPlaceHolder のコントロールを付与できます。 ただし、方が簡単に対応する ContentPlaceHolders を記憶する場合は、最上位マスター ページと入れ子になったマスター ページと同じ名前を使用して、ページのどのリージョンです。


これらの追加を行った後、`SimpleNested.master`マスター ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

削除、`Default.aspx`コンテンツ ページを先ほど作成したし、再度追加し、バインドすることを`SimpleNested.master`マスター ページ。 今度は Visual Studio 2 つ追加のコンテンツ コントロールを`Default.aspx`、ContentPlaceHolders を参照するようになりましたで定義されている`SimpleNested.master`(図 6 参照)。 テキスト「こんにちは, Default.aspx から!」を追加します。 コンテンツの制御参照`MainContent`します。

図 5 は、関連する 3 つのエンティティ`Simple.master`、 `SimpleNested.master`、および`Default.aspx`- とを相互に関連付ける方法。 図が示すように入れ子になったマスター ページは、その親のプレース ホルダーのコンテンツ コントロールを実装します。 これらのリージョンでは、コンテンツ ページにアクセスできるようにする必要があります、入れ子になったマスター ページはコンテンツ コントロールに独自の ContentPlaceHolders を追加する必要があります。


[![最上位レベルと入れ子になったマスター ページ コンテンツ ページのレイアウトを決定します。](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**図 05**: マスター ページの最上位と入れ子になったコンテンツ ページのレイアウトを音声入力 ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image15.png))。


この動作は、方法 [コンテンツ] ページまたはマスター ページはその親マスター ページを理解しているかを示しています。 この動作は、Visual Studio デザイナーによっても示されます。 図 6 に示すのデザイナー`Default.aspx`します。 明確のデザイナーに表示されますコンテンツ ページの編集可能なリージョンと、特定の部分ではありませんが、入れ子になったマスター ページからは何が編集可能なリージョンとリージョンが最上位マスター ページからを明確にしません。


[![コンテンツ ページの現在には入れ子になったマスター ページの ContentPlaceHolders のコンテンツ コントロールが含まれています](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**図 06**: [コンテンツ ページようになりましたが含まれていますコンテンツ コントロールの入れ子になったマスター ページの ContentPlaceHolders ([フルサイズの画像を表示する] をクリックします](nested-master-pages-vb/_static/image18.png))。


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>手順 3: 2 つ目単純入れ子になったマスター ページを追加します。

入れ子になったマスター ページの利点は、複数の入れ子になったマスター ページがある場合に顕著です。 この特典を示すためには、別の入れ子になったマスター ページを作成、`NestedMasterPages`フォルダー; 名前の新しい入れ子になったマスター ページ`SimpleNestedAlternate.master`にバインドし、`Simple.master`マスター ページ。 手順 2 で行ったように、入れ子になったマスター ページのコンテンツ コントロールを 2 つのプレース ホルダー コントロールを追加します。 また、テキスト「こんにちは, SimpleNestedAlternate から!」を追加します。 マスター ページの最上位レベルに対応するコンテンツ コントロールで`MainContent`プレース ホルダーです。 これらの変更を行った後、新しい入れ子になったマスター ページの宣言型マークアップは次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

という名前のコンテンツ ページを作成する`Alternate.aspx`で、`NestedMasterPages`フォルダーにバインドし、`SimpleNestedAlternate.master`入れ子になったマスター ページ。 テキスト「こんにちは, 代替から!」を追加します。 対応するコンテンツ コントロールで`MainContent`します。 図 7 は`Alternate.aspx`Visual Studio デザイナーで表示した場合。


[![Alternate.aspx が SimpleNestedAlternate.master マスター ページにバインドされています。](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**図 07**:`Alternate.aspx`にバインドされて、`SimpleNestedAlternate.master`マスター ページ ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image21.png))。


図 7 図 6 デザイナーにデザイナーを比較します。 両方のコンテンツ ページは、最上位マスター ページで定義されている同じレイアウトを共有 (`Simple.master`)、つまり「入れ子になったマスター ページのチュートリアル (単純)」タイトル。 テキスト「こんにちは, SimpleNested から!」の親マスター ページ - で定義された個別の内容をまだ両方があります。 図 6 で「こんにちは, SimpleNestedAlternate から!」 図 7。 確かに、これらの相違をここしますですが、意味のある違いを含めるには、この例を拡張できます。 たとえば、`SimpleNested.master`ページはそのコンテンツのページに固有のオプションのメニューを含めることができますが、`SimpleNestedAlternate.master`をバインドするコンテンツ ページに関連する情報があります。

ここで、包括的なサイトのレイアウトを変更する必要があったことを想像してください。 たとえば、すべてのコンテンツ ページに一般的なリンクのリストを追加したいとします。 最上位レベルのマスター ページを更新してこれを実現する`Simple.master`します。 その入れ子になったマスター ページで、拡張機能、そのコンテンツのページで、変更がありますがすぐに反映されます。

包括的なサイトのレイアウトを変更しました容易さを示すためには、開く、`Simple.master`マスター ページとの間で、次のマークアップを追加、`topContent`と`mainContent``<div>`要素。


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

バインドされる各ページの上部に 2 つのリンクが追加されます`Simple.master`、 `SimpleNested.master`、または`SimpleNestedAlternate.master`; 入れ子になったすべてのマスター ページとそのコンテンツのページにすぐにこれらの変更が適用されます。 図 8 は`Alternate.aspx`とき、ブラウザーで表示します。 (図 7 と比較して) ページの上部にあるリンクの追加に注意してください。


[![その入れ子になったマスター ページとそのコンテンツのページにすぐに反映されますが、最上位マスター ページに変更されました](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**図 08**: 入れ子になったマスター ページとそのコンテンツのページですぐに反映されますが、最上位マスター ページに変更 ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image24.png))。


## <a name="using-a-nested-master-page-for-the-administration-section"></a>[管理] セクションに入れ子になったマスター ページを使用します。

この時点でページの入れ子になったマスター利点きましたし、作成し、ASP.NET アプリケーションで使用する方法を見てきました。 ただし、例 1、2、および 3 の手順は、新しい最上位マスター ページ、新しい入れ子になったマスター ページ、および新しいコンテンツ ページの作成に関与します。 について新しい入れ子になったマスター ページ、web サイトを既存の最上位マスター ページで、コンテンツ ページを追加しますか。

入れ子になったマスター ページを既存の web サイトに統合し、既存のコンテンツ ページに関連付けるには、最初から開始するよりも少しの労力が必要です。 手順 4、5、6、7 はという名前の新しい入れ子になったマスター ページに、デモ アプリケーションの強化点として、これらの課題を探索`AdminNested.master`を管理者の指示が含まれています、内の ASP.NET ページによって使用されます、`~/Admin`フォルダー。

デモ アプリケーションに入れ子になったマスター ページの統合には、次の問題点が導入されています。

- 既存のコンテンツのページ、`~/Admin`フォルダーがそのマスター ページから特定の期待があります。 まず、ある特定のプレース ホルダー コントロールに期待されます。 さらに、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページは、マスター ページの公開を呼び出す`RefreshRecentProductsGrid`方法、設定、`GridMessageText`プロパティのイベント ハンドラーを設定またはその`PricesDoubled`イベント。 その結果、同じ ContentPlaceHolders とパブリック メンバー、入れ子になったマスター ページを提供する必要があります。
- 機能強化し、前のチュートリアルでは、`BasePage`を動的に設定するにはクラス、`Page`オブジェクトの`MasterPageFile`セッション変数に基づいてプロパティ。 サポートする方法を動的なマスター ページ入れ子になったマスター ページを使用する場合ですか。

入れ子になったマスター ページを作成し、既存のコンテンツ ページから使用すると、これら 2 つの課題を提示します。 調査がされが発生すると、これらの問題を克服します。

## <a name="step-4-creating-the-nested-master-page"></a>手順 4: 入れ子になったマスター ページを作成します。

最初のタスクでは、[管理] ページで使用する入れ子になったマスター ページを作成します。 入れ子になったマスター ページを新しいを追加するときに手順 2. で説明したように、入れ子になったマスター ページの親マスター ページを指定する必要があります。 2 つの最上位マスター ページが、:`Site.master`と`Alternate.master`します。 作成したことを思い出してください`Alternate.master`前のチュートリアルでコードを記述しました、`BasePage`クラスのセットを`Page`オブジェクトの`MasterPageFile`するか、実行時にプロパティ`Site.master`または`Alternate.master`の値に応じて`MyMasterPage`セッション変数です。

適切な最上位マスター ページを使用するように、入れ子になったマスター ページを構成方法しますか。 2 つのオプションがあります。

- 2 つの入れ子になったマスター ページを作成する`AdminNestedSite.master`と`AdminNestedAlternate.master`、およびそれらの最上位マスター ページにバインド`Site.master`と`Alternate.master`、それぞれします。 `BasePage`、設定し、`Page`オブジェクトの`MasterPageFile`適切な入れ子になったマスター ページにします。
- 1 つの入れ子になったマスター ページを作成し、この特定のマスター ページを使用して、コンテンツ ページがあります。 次に、実行時に必要に入れ子になったマスター ページの設定`MasterPageFile`実行時に適切な最上位マスター ページのプロパティ。 (マスター ページもある可能性がありますがおわかりのようここまでで、`MasterPageFile`プロパティです)。

2 番目のオプションを使用してみましょう。 単一ファイルを作成入れ子になったマスター ページで、`~/Admin`という名前のフォルダー`AdminNested.master`します。 両方`Site.master`と`Alternate.master`プレース ホルダー コントロールのセットが同じ、ともかくマスター ページをバインドするにバインドすることをお勧めしましたが`Site.master`整合性のためです。


[![~/Admin フォルダーには、入れ子になったマスター ページを追加します。](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**図 09**: する入れ子になったマスター ページを追加、`~/Admin`フォルダー。 ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image27.png))。


Visual Studio 4 が加算されますので、入れ子になったマスター ページは 4 つのプレース ホルダー コントロールでマスター ページにバインドするコンテンツ コントロールを新しい入れ子になったマスター ページ ファイルの初期のマークアップ。 同様、最上位マスター ページの ContentPlaceHolder のコントロールと同じ名前を付けます手順 2. および 3.、各コンテンツ コントロールでは、プレース ホルダー コントロールを追加します。 次のマークアップに対応するコンテンツ コントロールに追加することも、`MainContent`プレース ホルダー。


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

次に、定義、 `instructions` CSS クラス、`Styles.css`と`AlternateStyles.css`CSS ファイル。 次の CSS ルールがスタイルを使用する HTML 要素を原因と、`instructions`明るい黄色の背景の色を黒い実線の境界線を表示するクラス。


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

このマークアップは、入れ子になったマスター ページに追加されて、この入れ子になったマスター ページ (具体的には、[管理] ページ) を使用してこれらのページにのみ表示されます。

入れ子になったマスター ページへの追加を行った後、宣言型マークアップは次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

各コンテンツ コントロールは、プレース ホルダー コントロールと ContentPlaceHolder のコントロールの`ID`プロパティには、最上位マスター ページの対応するプレース ホルダー コントロールと同じ値が割り当てられます。 管理セクションに固有のマークアップがさらに表示されます、`MainContent`プレース ホルダーです。

図 10 に示します、 `AdminNested.master` Visual Studio のデザイナーで表示した場合の入れ子になったマスター ページ。 上部にある黄色のボックスの指示を確認できます、`MainContent`コンテンツ コントロール。


[![入れ子になったマスター ページは、管理者の方法を紹介の最上位マスター ページを拡張します。](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**図 10**: 入れ子になったマスター ページに含める手順については、管理者の最上位マスター ページを拡張します。 ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image30.png))。


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>手順 5: 新しい入れ子になったマスター ページを使用する既存のコンテンツ ページを更新しています

新しいコンテンツ ページにバインドする必要があります。 [管理] セクションに追加するたび、`AdminNested.master`マスター ページを作成しました。 しかし、コンテンツ ページで、既存のものでしょうか。 現時点から派生して、サイト内のすべてのコンテンツ ページ、`BasePage`クラスは、プログラムによって実行時にコンテンツ ページのマスター ページを設定します。 これは、[管理] セクションのコンテンツ ページの動作ではありません。 代わりに、これらのコンテンツ ページを常に使用する、`AdminNested.master`ページ。 実行時に右最上位レベルのコンテンツ ページを選択する入れ子になったマスター ページの責任になります。

動作がという名前の新しいベース ページのカスタム クラスを作成するにはこの目的を達成する最善の方法を`AdminBasePage`まで、`BasePage`クラス。 `AdminBasePage` オーバーライドし、`SetMasterPageFile`設定と、`Page`オブジェクトの`MasterPageFile`ハード コーディングされた値に"~/Admin/AdminNested.master"。 これにより、任意のページから派生した`AdminBasePage`が使用されます`AdminNested.master`から派生した任意のページは、`BasePage`がその`MasterPageFile`いずれかに動的にプロパティが設定"~/Site.master"または"~/Alternate.master"の値に基づいて、`MyMasterPage`セッション変数。

新しいクラス ファイルを追加して、開始、`App_Code`という名前のフォルダー`AdminBasePage.vb`します。 `AdminBasePage`拡張`BasePage`をオーバーライドし、`SetMasterPageFile`メソッド。 そのメソッドで割り当てる、`MasterPageFile`値"~/Admin/AdminNested.master"。 クラスのこれらの変更を行った後、次のようなファイルにする必要がありますなります。


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

派生してセクションの管理で、既存のコンテンツ ページに今すぐ必要があります`AdminBasePage`の代わりに`BasePage`します。 内の各コンテンツ ページの分離コード クラス ファイルに移動して、`~/Admin`フォルダーこの変更を加えます。 たとえば、`~/Admin/Default.aspx`から分離コード クラスの宣言を変更する場合します。


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

移動先:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

図 11 を示して 方法最上位マスター ページ (`Site.master`または`Alternate.master`)、入れ子になったマスター ページ (`AdminNested.master`)、および管理セクションのコンテンツ ページが互いに関連します。


[![入れ子になったマスター ページ、[管理] ページに特定のコンテンツを定義します](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**図 11**:、入れ子になったマスター ページを定義コンテンツに固有の管理セクションのページ ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image33.png))。


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>手順 6: マスター ページのパブリック メソッドとプロパティのミラーリング

いることを思い出してください、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページは、マスター ページをプログラムで操作:`~/Admin/AddProduct.aspx`呼び出しマスター ページの公開`RefreshRecentProductsGrid`メソッドとセットの`GridMessageText`プロパティです。`~/Admin/Products.aspx`のイベント ハンドラーを持って、`PricesDoubled`イベント。 前のチュートリアルで作成した、 `MustInherit` `BaseMasterPage`クラスが、これらのパブリック メンバーを定義します。

`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページ、マスター ページから派生したことを想定しています、`BaseMasterPage`クラス。 `AdminNested.master`  ページで、ただし、現在拡張、`System.Web.UI.MasterPage`クラス。 アクセスすると、結果として`~/Admin/Products.aspx`、`InvalidCastException`がスローされ、メッセージ:"型のオブジェクトをキャストできません 'ASP.admin\_adminnested\_マスター' 'BaseMasterPage' を入力します"。

させる必要があります。 これを解決する、`AdminNested.master`分離コード クラスを拡張`BaseMasterPage`します。 入れ子になったマスター ページの分離コード クラスの宣言を更新します。


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

移動先:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

まだ終わっていません。 としてマークされているメンバーをオーバーライドする必要があります`MustOverride`、つまり`RefreshRecentProductsGrid`と`GridMessageText`します。 これらのメンバーは、そのユーザー インターフェイスを更新する最上位マスター ページで使用されます。 (実際には、専用、`Site.master`マスター ページには、これらのメソッドが使用されますが、両方の最上位マスター ページは、両方を拡張するために、これらのメソッドを実装`BaseMasterPage`)。

これらのメンバーを実装する必要があります、 `AdminNested.master`、これらの実装を行う必要があるは入れ子になったマスター ページで使用される最上位マスター ページで、同じメンバーを呼び出すだけです。 たとえば、[管理] セクションのコンテンツ ページが、入れ子になったマスター ページを呼び出すときに`RefreshRecentProductsGrid`メソッド、入れ子になったマスター ページを行う必要があるすべてが、さらに、呼び出す`Site.master`または`Alternate.master`の`RefreshRecentProductsGrid`メソッド。

これを実現する、次を追加することで開始`@MasterType`の先頭にディレクティブ`AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

いることを思い出してください、`@MasterType`ディレクティブでは、厳密に型指定されたプロパティをという名前の分離コード クラスに追加`Master`します。 オーバーライドして、`RefreshRecentProductsGrid`と`GridMessageText`メンバーへの呼び出しを委任して、`Master`メソッドの対応します。


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

このコードでアクセスし、[管理] セクションのコンテンツ ページを使用できる必要があります。 図 12 は、`~/Admin/Products.aspx`ページをブラウザーで表示する場合。 ご覧のように、ページには、管理手順ボックスで、入れ子になったマスター ページで定義されているが含まれます。


[![[管理] セクションのコンテンツ ページが各ページの上部にある手順を含める](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**図 12**: 各ページの上部にある管理セクションを含める手順では、コンテンツのページ ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image36.png))。


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>手順 7: 実行時に適切な最上位マスター ページを使用します。

すべてが同じ最上位マスター ページを使用しでユーザーが選択したマスター ページを無視の管理セクションのすべてのコンテンツ ページは完全に機能が、`ChooseMasterPage.aspx`します。 この動作は、入れ子になったマスター ページがあるという事実のその`MasterPageFile`プロパティが静的に設定`Site.master`でその`<%@ Master %>`ディレクティブ。

設定する必要があります、エンドユーザーが選択した最上位マスター ページを使用する、`AdminNested.master`の`MasterPageFile`プロパティの値を`MyMasterPage`セッション変数。 コンテンツ ページの設定するため`MasterPageFile`プロパティ`BasePage`、入れ子になったマスター ページの設定はことが考えられます`MasterPageFile`プロパティ`BaseMasterPage`または、`AdminNested.master`の分離コード クラス。 これは、ために、機能しません、ただし、設定する必要があります、 `MasterPageFile` PreInit ステージの終わりまでのプロパティ。 マスター ページからページのライフ サイクルにをタップできますプログラムを使用した最初の時刻は、Init ステージ (これは、PreInit 段階の後に発生します)。

そのため、入れ子になったのマスター ページを設定する必要があります`MasterPageFile`コンテンツ ページのプロパティ。 唯一のコンテンツ ページを使用する、`AdminNested.master`からマスター ページを派生`AdminBasePage`します。 そのためがこのロジックを格納することができます。 手順 5 でオーバーライドされました、`SetMasterPageFile`メソッドを設定するページ オブジェクトの`MasterPageFile`プロパティを"~/Admin/AdminNested.master"。 Update`SetMasterPageFile`も、マスター ページを設定する`MasterPageFile`プロパティをセッションに保存された結果。


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession`メソッドを追加しました、`BasePage`クラスでは、前のチュートリアルでは、セッション変数の値に基づいて、適切なマスター ページのファイル パスを返します。

この変更、ユーザーのマスター ページの選択は、[管理] セクションに持ち越します。 図 13 では図 12、形式が、ユーザーがそのマスター ページの選択範囲を変更した後、同じページ`Alternate.master`します。


[![入れ子になった管理ページが、ユーザーが選択した最上位マスター ページを使用します。](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**図 13**: 入れ子になったの管理 ページは、最上位マスター ページで選択されて、ユーザーを使用して ([フルサイズの画像を表示する をクリックします](nested-master-pages-vb/_static/image39.png))。


## <a name="summary"></a>まとめ

Like 程度コンテンツ ページは、マスター ページにバインドできますが作成することは入れ子になった子マスター ページ、マスター ページが親マスター ページにバインドします。 子のマスター ページは、親の ContentPlaceHolders; の各コンテンツ コントロールを定義することがあります。これらのコンテンツ コントロールに独自のプレース ホルダー コントロール (およびその他のマークアップ) し、追加できます。 入れ子になったマスター ページでは、大規模な web アプリケーションをすべてのページが、包括的なルック アンド フィールを共有しながら特定のセクションでは、サイトの一意のカスタマイズが必要では、非常に便利です。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [入れ子になった ASP.NET マスター ページ](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [入れ子になったマスター ページと VS 2005 のデザイン時のためのヒント](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 には、マスター ページのサポートが入れ子になった](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)作成者の複数受け取りますブックと 4GuysFromRolla.com の創設者で、携わって Microsoft Web テクノロジ 1998 年からです。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 3.5 in 24 時間*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)します。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](specifying-the-master-page-programmatically-vb.md)
