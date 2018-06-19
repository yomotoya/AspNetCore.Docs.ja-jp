---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: 入れ子になったマスター ページ (VB) |Microsoft ドキュメント
author: rick-anderson
description: 他の中で 1 つのマスター ページを入れ子にする方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 8c0123c12bb653a7f680154e2155eae0eb129428
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891820"
---
<a name="nested-master-pages-vb"></a>入れ子になったマスター ページ (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> 他の中で 1 つのマスター ページを入れ子にする方法を示します。


## <a name="introduction"></a>はじめに

過去の 9 つのチュートリアルの過程でマスター ページをサイト全体のレイアウトを実装する方法を説明しました。 簡単に言うと、マスター ページにより、開発者は、マスター ページ コンテンツのページ コンテンツ ページごとにカスタマイズ可能な特定の地域とで一般的なマークアップを定義するページ。 マスター ページ内のプレース ホルダー コントロールのカスタマイズ可能な領域を示します。ContentPlaceHolder のコントロールのカスタマイズされたマークアップは、コンテンツ コントロールを使用してコンテンツ ページで定義されます。

これまでに調査したマスター ページ手法は、サイト全体にわたって使用される単一のレイアウトがある場合は、優れたです。 ただし、多くの大規模な web サイトでは、さまざまなセクション全体にカスタマイズされているサイトのレイアウトがあります。 たとえば、病院スタッフ患者の情報、アクティビティ、および課金を管理するために使用する医療アプリケーションがあるとします。 このアプリケーションの web ページの次の 3 つの型である可能性があります。

- スタッフのメンバーが、可用性を更新できますスタッフ メンバーに固有のページでは、スケジュール、表示されます。 または、休暇時間を要求します。
- 患者固有のページ、スタッフのメンバーの表示または特定の患者の情報を編集します。
- 課金固有のページで経理が現在確認は、表示される状態および財務レポートを要求します。

すべてのページには、上部と下部にある頻繁に使用されるリンクの系列の間でメニューなどの一般的なレイアウトを共有可能性があります。 スタッフ、患者情報、および請求固有のページは、この一般的なレイアウトをカスタマイズする必要があります。 たとえば、スタッフ固有のページなどは、予定表、タスクが一覧に現在ログオンしているユーザーの可用性と毎日のスケジュールを含める必要があります。 おそらくの患者情報に固有のすべてのページは、名前、アドレス、および、患者の情報を含む、編集中の保険情報を表示する必要があります。

使用してこのようなカスタマイズされたレイアウトを作成することは*入れ子になったマスター ページ*です。 上記のシナリオを実装するのには、contentplaceholders にカスタマイズ可能な領域を定義すると、サイト全体のレイアウト、メニューとページ フッターのコンテンツを定義するマスター ページを作成して開始します。 次の 3 つ入れ子になったマスター ページ、web ページの種類ごとに 1 つは作成します。 入れ子になった各マスター ページには、マスター ページを使用するコンテンツ ページの種類間でコンテンツを定義します。 つまり、マークアップと編集されている患者に関する情報を表示するプログラム ロジックの患者情報に固有のコンテンツ ページの入れ子になったマスター ページが含まれます。 新しい患者固有のページを作成するときに、この入れ子になったマスター ページにバインドおとします。

このチュートリアルは、入れ子になったマスター ページの利点を強調表示して起動します。 作成し、入れ子になったマスター ページを使用する方法を示しています。

> [!NOTE]
> .NET Framework のバージョン 2.0 以降可能であった入れ子になったマスター ページ。 ただし、Visual Studio 2005 では、入れ子になったマスター ページのデザイン時サポートが含まれませんでした。 良いニュースは、Visual Studio 2008 が入れ子になったマスター ページのデザイン時の機能豊富なエクスペリエンスを提供します。 入れ子になったマスター ページを使用して関心のある Visual Studio 2005 を引き続き使用している場合は、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ「 [VS 2005 のデザイン時入れ子になったマスター ページのヒント](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)です。


## <a name="the-benefits-of-nested-master-pages"></a>入れ子になったマスター ページの利点

多くの web サイトがある、包括的なサイトの設計とカスタマイズされたデザイン複数ページの特定の種類に固有です。 たとえば、web アプリケーションではデモ作成しました基本的な管理セクション (内のページ、`~/Admin`フォルダー)。 現在、web ページで、`~/Admin`フォルダーは、[管理] セクションではなく、これらのページとして同じマスター ページを使用して (つまり、`Site.master`または`Alternate.master`ユーザーの選択に応じて、)。

> [!NOTE]
> ここには、マイクロソフトのサイトが 1 つのマスター ページとして扱わ`Site.master`です。 このチュートリアルの後半で 2 つ (以上) のマスター ページを使用して、「を使用して、入れ子になったマスター ページの 管理セクション」で始まるを入れ子になったマスター ページを使用してお対処します。


追加情報や、それ以外の場合はありません、サイトの他のページ内に存在するリンクを含める [管理] ページのレイアウトをカスタマイズするように求められたことを想像してください。 この要件を実装する次の 4 つの方法があります。

1. すべてのコンテンツ ページに、管理に固有の情報とリンクを手動で追加、`~/Admin`フォルダーです。
2. 更新プログラム、`Site.master`管理セクションに固有の情報とリンクを含めるし、これらのセクションでは、非表示のマスター ページにコードを追加するマスター ページで、[管理] ページのいずれかが参照されているかどうかに基づいて。
3. 新しいマスター ページを作成する具体的には、[管理] セクションをコピーからのマークアップを`Site.master`管理セクションに固有の情報とリンクについては、追加、およびコンテンツのページを更新、`~/Admin`新しいマスターが使用するフォルダーページです。
4. 入れ子になったマスター ページにバインドを作成して`Site.master`内でコンテンツ ページを持つ、`~/Admin`入れ子になったマスター ページをこの新しいフォルダー使用します。 この入れ子になったマスター ページの追加情報と管理ページに特定のリンクだけが含まれますで既に定義されているマークアップを繰り返す必要はありません`Site.master`です。

最初のオプションは、適切です。 マスター ページを使用して、あらゆる部分を手動でコピーして、新規の ASP.NET ページに共通のマークアップを貼り付けることから離れた場所に移動を開始します。 2 番目のオプションは、許容ですがときどきのみが表示され、開発者マスター ページの編集このマークアップを回避してとにする必要があるマークアップを使用してマスター ページをレイアウトやバルクことと保守性が低下するため、アプリケーション正確に特定マークアップは、非表示にされてときと表示されます。 この方法は、この単一のマスター ページでは対応に必要な web ページの詳細と詳細の種類のカスタマイズと小さい tenable があります。

3 番目のオプションが煩雑さを削除し、2 番目のオプションと、複雑性の問題、提示します。 ただし、3 つのオプションの主な欠点は、コピーしてから、一般的なレイアウトを貼り付けることが必要なこと`Site.master`新しい管理セクションに固有のマスター ページにします。 場合後でサイト全体のレイアウトを変更するお必要がありますに 2 つの場所を変更することに注意してください。

4 番目のオプションは、マスター ページを入れ子にされた、2 番目と 3 番目のオプションの最適な意見をお持ちです。 特定の地域に固有のコンテンツが別のファイルに分割中に、1 つのファイルの最上位レベルのマスター ページ - サイト全体のレイアウト情報が保持されます。

このチュートリアルは、作成して、単純な入れ子になったマスター ページを使用してを見てを開始します。 まったく新しいトップ レベルのマスター ページ、2 つの入れ子になったマスター ページ、および 2 つのコンテンツ ページを作成します。 「を使用して、入れ子になったマスター ページの 管理セクション」から始めて、入れ子になったマスター ページを使用するように、既存のマスター ページ アーキテクチャの更新について説明します。 具体的には、入れ子になったマスター ページを作成し、コンテンツ ページの追加のカスタム コンテンツを追加することを使用して、`~/Admin`フォルダーです。

## <a name="step-1-creating-a-simple-top-level-master-page"></a>手順 1: 単純な最上位レベルのマスター ページを作成します。

入れ子になったマスターを作成するのいずれかに基づいて、既存のマスター ページと、既存のコンテンツ ページが既に特定予定ために、多少複雑なをオーバーヘッドが最上位レベルのマスター ページではなく、この新しい入れ子になったマスター ページを使用する既存のコンテンツ ページを更新ContentPlaceHolder の最上位レベルのマスター ページで定義されているコントロール。 したがって、入れ子になったマスター ページには、同じ名前を持つ同じ ContentPlaceHolder コントロールおく必要があります。 さらに、特定のデモ アプリケーションに含まれる 2 つマスター ページ (`Site.master`と`Alternate.master`) このような複雑さをさらに追加するユーザーの設定に基づくコンテンツ ページに動的に割り当てられています。 このチュートリアルの後半で入れ子になったマスター ページを使用する既存のアプリケーションの更新に見ていきますが、マスター ページの例が、単純なに焦点を最初に入れ子になったみましょう。

という名前の新しいフォルダーを作成する`NestedMasterPages`し、そのフォルダーに新しいマスター ページ ファイルを追加`Simple.master`です。 (図 1 の参照、ソリューション エクスプ ローラーのスクリーン ショットこのフォルダーとファイルを追加した後です。)ドラッグ、`AlternateStyles.css`ソリューション エクスプ ローラー、デザイナーからのスタイル シート ファイル。 これを追加、`<link>`要素のスタイル シート ファイルを`<head>`要素の後に、マスター ページの`<head>`要素のマークアップのようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

次に、追加の Web フォーム内で次のマークアップ`Simple.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

このマークアップでは、ネイビー カラーのバック グラウンドで大きな白いフォント内のページの上部にある「入れ子になったマスター ページ (単純)」というリンクが表示されます。 下には、 `MainContent` ContentPlaceHolder です。 図 1 は、 `Simple.master` Visual Studio デザイナーで読み込まれるときに、マスター ページ。


[![入れ子になったマスター ページ、[管理] セクション内のページに特定のコンテンツを定義します。](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**図 01**:、入れ子になったマスター ページを定義コンテンツ特定 [管理] セクション内のページ ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>手順 2: 単純な入れ子になったマスター ページを作成します。

`Simple.master` ContentPlaceHolder の 2 つのコントロールが含まれています。 `MainContent` ContentPlaceHolder と共に Web フォーム内で追加されました、`head`で ContentPlaceHolder、`<head>`要素。 コンテンツ ページを作成しにバインドした場合`Simple.master`コンテンツ ページで次の 2 つの contentplaceholders に参照する 2 つのコンテンツ コントロールが必要があります。 同様に、入れ子になったマスター ページを作成し、バインドにかどうか`Simple.master`の場合、入れ子になったマスター ページは次の 2 つのコンテンツ コントロールです。

新しい入れ子になったマスター ページを追加してみましょう。、`NestedMasterPages`という名前のフォルダー`SimpleNested.master`です。 右クリックし、`NestedMasterPages`フォルダーと、新しい項目の追加を選択します。 図 2 に示すように、新しい項目の追加 ダイアログ ボックスが表示されます。 マスター ページ テンプレートの種類を選択して、新しいマスター ページの名前を入力します。 指定すること、新しいマスター ページは、入れ子になったマスター ページ、するには、「マスター ページを選択」チェック ボックスを確認します。

次に、[追加] ボタンをクリックします。 同じ Select マスター ページに、コンテンツ ページをバインドするときを参照してください、マスター ページ ダイアログ ボックスが表示されます (図 3 を参照してください)。 選択、`Simple.master`マスター ページで、`NestedMasterPages`フォルダー OK をクリックします。

> [!NOTE]
> Web サイト プロジェクトのモデルではなく、Web アプリケーション プロジェクト モデルを使用して、ASP.NET web サイトを作成する場合は図 2 に示すように新しい項目の追加 ダイアログ ボックスで"Select マスター ページ"チェック ボックスは表示されません。 Web アプリケーション プロジェクトのモデルを使用するときに、入れ子になったマスター ページを作成するには、(テンプレートの代わりに、マスター ページ) 入れ子になったマスター ページ テンプレートを選択する必要があります。 マスター ページの入れ子になったテンプレートを選択すると、追加をクリックして、後に同じは、図 3 に示すようにダイアログ ボックスが表示されますマスター ページを選択します。


[![チェック、&quot;マスター ページを選択&quot;入れ子になったマスター ページを追加する チェック ボックス](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**図 02**: 入れ子になったマスター ページを追加する「マスター ページの選択」チェック ボックスをオン ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image6.png))


[![入れ子になったマスター ページを Simple.master マスター ページにバインドします。](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**図 03**: 入れ子になったマスター ページへのバインド、`Simple.master`マスター ページ ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image9.png))


入れ子になったマスター ページの宣言型マークアップ次に示すにはには、2 つのコンテンツ コントロールがマスター ページの最上位レベルの 2 つのプレース ホルダー コントロールの参照が含まれています。


[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

を除き、`<%@ Master %>`ディレクティブを入れ子になったマスター ページの初期宣言型マークアップは、コンテンツ ページを同じ最上位レベルのマスター ページにバインドするときに最初に生成されたマークアップと同じです。 などのコンテンツ ページの`<%@ Page %>`ディレクティブ、`<%@ Master %>`ディレクティブは、ここに含まれています、`MasterPageFile`入れ子になったマスター ページの親のマスター ページを指定する属性。 入れ子になったマスター ページと同じ最上位レベルのマスター ページにバインドされているコンテンツ ページの主な違いは、入れ子になったマスター ページが ContentPlaceHolder コントロールを含めることができます。 入れ子になったマスター ページの ContentPlaceHolder コントロールは、コンテンツ ページがマークアップをカスタマイズできる領域を定義します。

テキスト「こんにちは, from SimpleNested!」を表示するように、この入れ子になったマスター ページを更新します。 対応するコンテンツ コントロールに、 `MainContent` ContentPlaceHolder コントロール。


[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

この追加した場合、入れ子になったマスター ページを保存しを新しいコンテンツ ページを追加、`NestedMasterPages`という名前のフォルダー`Default.aspx`にバインドし、`SimpleNested.master`マスター ページ。 このページを追加するのには、驚くことが含まれていない (図 4 を参照してください) のコンテンツ コントロールを表示する場合があります! コンテンツ ページにのみアクセスできますその*親*マスター ページの contentplaceholders にします。 `SimpleNested.master` ContentPlaceHolder コントロール; が含まれていません。そのため、このマスター ページにバインドされている任意のコンテンツ ページでは、すべてのコンテンツ コントロールを含めることはできません。


[![新しいコンテンツ ページにコンテンツ コントロールが含まれていません。](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**図 04**:、新しいコンテンツ ページを含むいいえコンテンツ コントロール ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image12.png))


入れ子になったマスター ページの更新が行う必要があります (`SimpleNested.master`) プレース ホルダー コントロールを追加します。 通常、各 ContentPlaceHolder の親のマスター ページで定義できるため、その子マスター ページまたはマスター ページの最上位の ContentPlaceHolder のいずれかを使用するコンテンツ ページのプレース ホルダーを追加する場合は、入れ子になったマスター ページを場合もあります。制御します。

更新プログラム、`SimpleNested.master`マスター ページ、ContentPlaceHolder は、その 2 つのコンテンツ コントロールにします。 ContentPlaceHolder のコントロールのコンテンツ コントロールを指す ContentPlaceHolder コントロールと同じ名前を付けます。 つまり、という名前のプレース ホルダー コントロールを追加`MainContent`コンテンツを制御するのに`SimpleNested.master`を参照する、`MainContent`で ContentPlaceHolder`Simple.master`です。 参照するコンテンツ コントロールに同じ処理を行う、 `head` ContentPlaceHolder です。

> [!NOTE]
> 入れ子になったマスター ページ内のプレース ホルダー コントロールをトップレベルのマスター ページで contentplaceholders と同じ名前付けをお勧め、中にこの名前付けの対称性は必要ありません。 任意の名前で、入れ子になったマスター ページ プレース ホルダー コントロールを付与できます。 ただし、ほうが簡単に対応して contentplaceholders に注意してください 最上位レベルのマスター ページや入れ子になったマスター ページは、同じ名前を使用する場合、ページのどの領域です。


これらの追加を行った後、`SimpleNested.master`マスター ページの宣言型マークアップは、次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

削除、`Default.aspx`コンテンツ ページを作成したばかりにし、再度追加して、バインドにするには、`SimpleNested.master`マスター ページ。 今度は Visual Studio 2 つ追加のコンテンツ コントロールを`Default.aspx`、内で定義され、contentplaceholders に参照する`SimpleNested.master`(図 6 を参照してください)。 テキスト「こんにちは, from Default.aspx!」を追加します。 コンテンツの制御参照`MainContent`です。

図 5 は、ここでは、関与する 3 つのエンティティ`Simple.master`、 `SimpleNested.master`、および`Default.aspx`- と相互に関連付ける方法。 図に示す入れ子になったマスター ページは、その親の ContentPlaceHolder のコンテンツ コントロールを実装します。 これらの領域は、コンテンツ ページにアクセスできるようにする必要がある場合、入れ子になったマスター ページは、コンテンツ コントロールに、独自の contentplaceholders を追加する必要があります。


[![最上位レベルと入れ子になったマスター ページ コンテンツ ページのレイアウトを決定します。](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**図 05**: マスター ページの最上位レベルと入れ子になったディクテーション コンテンツ ページのレイアウト ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image15.png))


この動作は、どのコンテンツ ページまたはマスター ページはその親のマスター ページの認識を示しています。 この動作は、Visual Studio デザイナーによっても示されます。 図 6 は、Designer for`Default.aspx`です。 デザイナーは、どのような領域がコンテンツ ページの編集可能であり、特定の部分がないを明確に示しています、中はあいまいさを解消編集不可能な領域が、入れ子になったマスター ページからは、領域は、最上位レベルのマスター ページからです。


[![コンテンツ ページここには、入れ子になったマスター ページの contentplaceholders ににはコンテンツ コントロールが含まれています](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**図 06**:、コンテンツ ページようになりましたが含まれていますコンテンツ コントロールの入れ子になったマスター ページの contentplaceholders に ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>手順 3: 単純な入れ子になったマスターのページは、2 番目の追加

入れ子になったマスター ページの利点は、複数の入れ子になったマスター ページがある場合に顕著です。 この利点を示すためには、別の入れ子になったマスター ページを作成、`NestedMasterPages`フォルダー以外の名前をこの新しい入れ子になったマスター ページ`SimpleNestedAlternate.master`にバインドし、`Simple.master`マスター ページ。 手順 2 で行ったように、2 つのコンテンツ コントロールを入れ子になったマスター ページの ContentPlaceHolder コントロールを追加します。 また、テキストを「こんにちは, from SimpleNestedAlternate!」を追加します。 マスター ページの最上位レベルに対応するコンテンツ コントロールに`MainContent`ContentPlaceHolder です。 これらの変更を行った後、新しい入れ子になったマスター ページの宣言型マークアップを次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

という名前のコンテンツ ページを作成する`Alternate.aspx`で、`NestedMasterPages`フォルダーにそれをバインドし、`SimpleNestedAlternate.master`入れ子になったマスター ページ。 テキスト「こんにちは, from 代替!」を追加します。 対応するコンテンツ コントロールに`MainContent`です。 図 7 は`Alternate.aspx`Visual Studio デザイナーで表示したときにします。


[![Alternate.aspx が SimpleNestedAlternate.master マスター ページにバインドされています。](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**図 07**:`Alternate.aspx`にバインドされて、`SimpleNestedAlternate.master`マスター ページ ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image21.png))


図 6 でデザイナーに図 7 に、デザイナーを比較します。 両方のコンテンツ ページは、最上位レベルのマスター ページで定義されている同じレイアウトを共有 (`Simple.master`)、つまり"入れ子になったマスター ページ Tutorial (単純)"タイトルです。 個別のコンテンツがその親のマスター ページ - で定義されているテキスト「こんにちは, from SimpleNested!」があるどちらもまだ 図 6 で「こんにちは, from SimpleNestedAlternate!」 図 7 です。 確かに、これらの相違をここでは、単純なが、意味のある相違点を含めるには、この例を拡張できます。 インスタンス、`SimpleNested.master`一方、ページはそのコンテンツ ページに固有のオプションのメニューを含めることが`SimpleNestedAlternate.master`にバインドするコンテンツのページに関連する情報があります。

ここで、サイトの包括的なレイアウトを変更する必要がありましたを想像してください。 たとえばを想像すべてのコンテンツ ページに共通のリンクの一覧を追加します。 最上位レベルのマスター ページを更新これを実現する`Simple.master`です。 その入れ子になったマスター ページで、拡張機能、そのコンテンツ ページで、すべての変更がありますがすぐに反映されます。

簡単に使用するは、サイトの包括的なレイアウトを変更できますを示すためには、開く、`Simple.master`マスター ページとの間で、次のマークアップを追加、`topContent`と`mainContent``<div>`要素。


[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

これにバインドする各ページの上部に 2 つのリンクを追加`Simple.master`、 `SimpleNested.master`、または`SimpleNestedAlternate.master`; 入れ子になったすべてのマスター ページとそのコンテンツ ページに即座にこれらの変更が適用されます。 図 8 は`Alternate.aspx`ブラウザーで表示したときにします。 (図 7 と比べると)、ページの上部にあるリンクの追加に注意してください。


[![その入れ子になったマスター ページとそのコンテンツ ページですぐに反映されますが、最上位のマスター ページに変更](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**図 08**: その入れ子になったマスター ページとそのコンテンツ ページですぐに反映されますが、最上位のマスター ページに変更 ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>[管理] セクションの入れ子になったマスター ページを使用します。

この時点で入れ子になったマスターの利点にページが検査しを作成し、ASP.NET アプリケーションで使用する方法について説明しました。 手順 1、2、および 3 の例では、新しい最上位レベルのマスター ページ、新しい入れ子になったマスター ページ、および新しいコンテンツ ページを作成するただし、関係しています。 では、新しい入れ子になったマスター ページの web サイトに最上位レベルの既存のマスター ページで、コンテンツ ページを追加しますか。

入れ子になったマスター ページを既存の web サイトに統合し、既存のコンテンツ ページに関連付けるには、最初から開始するよりも少し多くの労力が必要です。 手順 4、5、6、7 がという名前の新しい入れ子になったマスター ページを含めるデモ アプリケーションを補強およう、これらの課題を探索`AdminNested.master`内の ASP.NET ページによって使用され、管理者の手順を説明する、`~/Admin`フォルダーです。

デモ アプリケーションに入れ子になったマスター ページを統合するには、次の難関が導入されています。

- 既存のコンテンツのページ、`~/Admin`フォルダーがある、マスター ページから特定の期待します。 まず、存在する特定のプレース ホルダー コントロールを享受できます。 さらに、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`ページは、マスター ページの公開を呼び出す`RefreshRecentProductsGrid`方法、設定、`GridMessageText`プロパティのイベント ハンドラーを設定またはその`PricesDoubled`イベント。 その結果、入れ子になったマスター ページでは、同じ contentplaceholders とパブリック メンバーを指定する必要があります。
- 前のチュートリアルでは強化されて、`BasePage`を動的に設定するクラス、`Page`オブジェクトの`MasterPageFile`プロパティ セッション変数に基づきます。 方法をサポート動的なマスター ページ入れ子になったマスター ページを使用する場合ですか。

入れ子になったマスター ページを作成し、既存のコンテンツ ページから使用すると、これら 2 つの課題が現れます。 調査うまくし、発生すると、これらの問題を克服します。

## <a name="step-4-creating-the-nested-master-page"></a>手順 4: 入れ子になったマスター ページの作成

最初のタスクでは、[管理] ページで使用する場合は、入れ子になったマスター ページを作成します。 マスター ページの追加、新しい入れ子になったときに手順 2 で示したように、入れ子になったマスター ページの親のマスター ページを指定する必要があります。 2 つの最上位レベルのマスター ページがお:`Site.master`と`Alternate.master`です。 作成した再呼び出し`Alternate.master`前のチュートリアルでコードを記述し、`BasePage`クラスのセットを`Page`オブジェクトの`MasterPageFile`するか、実行時にプロパティ`Site.master`または`Alternate.master`の値に応じて`MyMasterPage`セッション変数です。

操作方法構成する、入れ子になったマスター ページ、適切な最上位レベルのマスター ページを使用するようにしますか。 2 つのオプションがあります。

- 2 つの入れ子になったマスター ページを作成する`AdminNestedSite.master`と`AdminNestedAlternate.master`、し、最上位レベルのマスター ページにバインド`Site.master`と`Alternate.master`、それぞれします。 `BasePage`を設定し、`Page`オブジェクトの`MasterPageFile`適切な入れ子になったマスター ページ。
- 1 つの入れ子になったマスター ページを作成し、この特定のマスター ページを使用してコンテンツのページがあります。 次に、実行時に、ような入れ子になったマスター ページの設定`MasterPageFile`実行時に適切な最上位レベルのマスター ページのプロパティです。 (可能性がありますが理解ここまでで、マスター ページもある、`MasterPageFile`プロパティです)。

2 番目のオプションを使用してみましょう。 1 つ入れ子になったマスター ページでファイルを作成、`~/Admin`という名前のフォルダー`AdminNested.master`です。 両方`Site.master`と`Alternate.master`ContentPlaceHolder コントロールのセットが同じ、関係ありませんマスター ページをバインドするにバインドすることをお勧めは`Site.master`整合性のためです。


[![~/Admin フォルダーに入れ子になったマスター ページを追加します。](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**図 09**: する入れ子になったマスター ページの追加、`~/Admin`フォルダーです。 ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image27.png))


Visual Studio は 4 つに追加して、入れ子になったマスター ページが 4 つのプレース ホルダー コントロールでのマスター ページにバインドされているため、新しい入れ子になったマスター ページファイルの初期のマークアップにコントロールのコンテンツします。 同様上位レベルのマスター ページ プレース ホルダー コントロールと同じ名前を与える手順 2. および 3.、各コンテンツ コントロールに ContentPlaceHolder コントロールを追加します。 対応するコンテンツ コントロールに、次のマークアップを追加しても、 `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

次に、定義、 `instructions` CSS クラスの`Styles.css`と`AlternateStyles.css`CSS ファイル。 次の CSS 規則とスタイルの HTML 要素が発生する、`instructions`明るい黄色の背景色と黒い実線の境界線を表示するクラス。


[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

入れ子になったマスター ページには、このマークアップが追加された、この入れ子になったマスター ページ (つまり、[管理] ページ) を使用してこれらのページにのみ表示されます。

入れ子になったマスター ページへの追加を行った後の宣言型マークアップを次のようになります。


[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

各コンテンツ コントロールは ContentPlaceHolder 制御し、ContentPlaceHolder のコントロールの`ID`プロパティには、最上位レベルのマスター ページ内の対応する ContentPlaceHolder コントロールと同じ値が割り当てられます。 管理セクション固有のマークアップがさらに、表示、 `MainContent` ContentPlaceHolder です。

図 10 を示しています、 `AdminNested.master` Visual Studio のデザイナーを表示したときにマスター ページを入れ子になった。 黄色のボックスの上部にある手順を参照してできます、`MainContent`コンテンツ コントロールです。


[![入れ子になったマスター ページでは、管理者の手順を付属する上位レベルのマスター ページを拡張します。](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**図 10**: 入れ子になったマスター ページは、管理者の手順を付属する上位レベルのマスター ページを拡張します。 ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>手順 5: 新しい入れ子になったマスター ページを使用する既存のコンテンツ ページを更新します。

バインドする必要があります [管理] セクションに新しいコンテンツ ページを追加お常、`AdminNested.master`マスター ページを作成したばかりです。 しかし、ページのコンテンツについて、既存のでしょうか。 現時点から派生して、サイト内のすべてのコンテンツ ページ、`BasePage`クラスは、プログラムによって実行時に、コンテンツ ページのマスター ページを設定します。 [管理] セクションのコンテンツ ページの想定どおりの動作ではありません。 これらのコンテンツ ページを常に使用する代わりに、`AdminNested.master`ページ。 実行時に右最上位レベル コンテンツ ページを選択する場合は、入れ子になったマスター ページの責任があります。

最善の方法をこの目的の動作はという名前の新しいカスタムの基本ページ クラスを作成する`AdminBasePage`自身を拡張する、`BasePage`クラスです。 `AdminBasePage` オーバーライドし、`SetMasterPageFile`設定と、`Page`オブジェクトの`MasterPageFile`ハード コーディングされた値に"~/Admin/AdminNested.master"です。 この方法で任意のページから派生した`AdminBasePage`が使用されます`AdminNested.master`から派生した任意のページに対して`BasePage`がその`MasterPageFile`いずれかに動的にプロパティが設定"~/Site.master"または"~/Alternate.master"の値に基づいて、`MyMasterPage`セッション変数。

新しいクラス ファイルを追加することで開始、`App_Code`という名前のフォルダー`AdminBasePage.vb`です。 `AdminBasePage`拡張`BasePage`をオーバーライドし、`SetMasterPageFile`メソッドです。 そのメソッドで割り当てる、`MasterPageFile`値"~/Admin/AdminNested.master"です。 クラスのこれらの変更を行った後ファイルを次のようになります。


[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

派生してセクションの管理で、既存のコンテンツ ページに今すぐ必要があります`AdminBasePage`の代わりに`BasePage`です。 コンテンツ ページごとに分離コード クラス ファイルに移動して、`~/Admin`フォルダーこの変更を加えます。 たとえば、`~/Admin/Default.aspx`から分離コード クラスの宣言を変更するとします。


[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

移動先:


[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

図 11 を示して 方法、最上位レベルのマスター ページ (`Site.master`または`Alternate.master`)、入れ子になったマスター ページ (`AdminNested.master`)、し、管理セクションのコンテンツ ページを相互に関連付けます。


[![入れ子になったマスター ページ、[管理] セクション内のページに特定のコンテンツを定義します。](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**図 11**:、入れ子になったマスター ページを定義コンテンツ特定 [管理] セクション内のページ ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>手順 6: ミラーリングのマスター ページのパブリック メソッドとプロパティ

注意してください、`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`マスター ページとプログラム ページの操作:`~/Admin/AddProduct.aspx`呼び出しマスター ページのパブリック`RefreshRecentProductsGrid`メソッドとセットその`GridMessageText`プロパティです。`~/Admin/Products.aspx`のイベント ハンドラーを持つ、`PricesDoubled`イベント。 前のチュートリアルで作成しました、 `MustInherit` `BaseMasterPage`クラスが、これらのパブリック メンバーを定義します。

`~/Admin/AddProduct.aspx`と`~/Admin/Products.aspx`がマスター ページから派生しているページがあると、`BaseMasterPage`クラスです。 `AdminNested.master`  ページで、ただし、現在は拡張、`System.Web.UI.MasterPage`クラスです。 アクセスすると、結果として`~/Admin/Products.aspx`、`InvalidCastException`メッセージと共にスローされます:"型のオブジェクトをキャストできません 'ASP.admin\_adminnested\_マスター' を 'BaseMasterPage' を入力します"。

必要ですがこれを解決する、`AdminNested.master`分離コード クラスを拡張`BaseMasterPage`です。 入れ子になったマスター ページの分離コード クラスの宣言を更新します。


[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

移動先:


[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

まだ完了していません。 としてマークされたメンバーをオーバーライドする必要があります`MustOverride`、つまり`RefreshRecentProductsGrid`と`GridMessageText`です。 これらのメンバーは、そのユーザー インターフェイスを更新する最上位レベルのマスター ページで使用されます。 (実際には、専用の`Site.master`両方最上位レベルのマスター ページは、両方を拡張するためにこれらのメソッドを実装がマスター ページは、これらのメソッドを使用して`BaseMasterPage`)。

これらのメンバーを実装する必要があります、`AdminNested.master`を行うには、これらの実装が必要なは、入れ子になったマスター ページで使用される最上位レベルのマスター ページで、同じメンバーを呼び出すだけです。 たとえば、[管理] セクションのコンテンツのページが、入れ子になったマスター ページを呼び出すときに`RefreshRecentProductsGrid`メソッド、入れ子になったマスター ページのために必要なすべてが、さらに、呼び出す`Site.master`または`Alternate.master`の`RefreshRecentProductsGrid`メソッドです。

これを実現する、次を追加することによって開始`@MasterType`の最上位にディレクティブ`AdminNested.master`:


[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

注意してください、`@MasterType`ディレクティブは、という名前の分離コード クラスを厳密に型指定されたプロパティを追加`Master`です。 オーバーライドして、`RefreshRecentProductsGrid`と`GridMessageText`メンバーへの呼び出しを単純に委任し、`Master`メソッドの対応します。


[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

このコードにアクセスし、[管理] セクションのコンテンツ ページを使用することができます。 図 12 を示しています、`~/Admin/Products.aspx`ページをブラウザーで表示する場合。 ご覧のように、ページには、管理手順ボックスで、入れ子になったマスター ページで定義されているが含まれています。


[![コンテンツ管理 セクションの内容を含む各ページの上部にある手順](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**図 12**: 各ページの上部にある管理セクションを含める手順では、コンテンツ ページ ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>手順 7: 実行時に、適切な最上位レベルのマスター ページを使用します。

すべて同じ最上位レベルのマスター ページを使用しのユーザーによって選択されたマスター ページを無視して、[管理] セクションのすべてのコンテンツ ページは完全に機能が、`ChooseMasterPage.aspx`です。 この動作は、入れ子になったマスター ページがあるという事実のというその`MasterPageFile`プロパティに静的に設定`Site.master`でその`<%@ Master %>`ディレクティブです。

設定する必要があります、エンドユーザーが選択した最上位レベルのマスター ページを使用する、`AdminNested.master`の`MasterPageFile`プロパティの値を`MyMasterPage`セッション変数。 コンテンツ ページの設定するため`MasterPageFile`プロパティ`BasePage`、こと、入れ子になったマスター ページの設定と思われる場合があります`MasterPageFile`プロパティに`BaseMasterPage`または、`AdminNested.master`の分離コード クラスです。 これは、ために、機能しません、ただし、設定する必要があります、`MasterPageFile`プロパティ PreInit ステージが終了します。 マスター ページからページのライフ サイクルにプログラムでタップお最初の時刻は、(PreInit 段階の後に発生します) を初期化段階です。

そのため、入れ子になったのマスター ページを設定する必要があります`MasterPageFile`コンテンツ ページのプロパティです。 だけのコンテンツ ページを使用する、`AdminNested.master`から派生してマスター ページ`AdminBasePage`です。 そのため、このロジックをある配置おことができます。 私たちが手順 5. で、`SetMasterPageFile`メソッドを設定するページ オブジェクトの`MasterPageFile`プロパティを"~/Admin/AdminNested.master"です。 更新`SetMasterPageFile`も、マスター ページを設定する`MasterPageFile`セッションに格納されている結果のプロパティ。


[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

`GetMasterPageFileFromSession`に追加するメソッド、`BasePage`クラスには、前のチュートリアルでは、セッション変数の値に基づいて、適切なマスター ページファイルのパスを返します。

配置でこの変更により、ユーザーのマスター ページの選択は、[管理] セクションに引き継がです。 図 13 図 12 ですが、ユーザーが、マスター ページの選択範囲を変更した後、同じページを示しています`Alternate.master`です。


[![入れ子になった管理ページが、ユーザーが選択した最上位レベルのマスター ページを使用します。](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**図 13**: トップレベル マスター ページで選択されて、ユーザーは、入れ子になった管理ページを使用して ([フルサイズのイメージを表示するをクリックして](nested-master-pages-vb/_static/image39.png))


## <a name="summary"></a>まとめ

Like 程度コンテンツ ページは、マスター ページにバインドできる、可能であれば入れ子になったを作成する子のマスター ページ、マスター ページが親のマスター ページにバインドします。 子マスター ページは、親の contentplaceholders に; の各コンテンツ コントロールを定義することがあります。これらのコンテンツ コントロールを独自のプレース ホルダー コントロール (およびその他のマークアップ) し、追加できます。 入れ子になったマスター ページは、大規模な web アプリケーションをすべてのページが、包括的なルック アンド フィールを共有しながら特定のセクションでは、サイトの一意のカスタマイズが必要で非常に便利です。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [入れ子になった ASP.NET マスター ページ](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [入れ子になったマスター ページと VS 2005 のデザイン時のヒント](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 には、マスター ページのサポートが入れ子になった](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](specifying-the-master-page-programmatically-vb.md)
