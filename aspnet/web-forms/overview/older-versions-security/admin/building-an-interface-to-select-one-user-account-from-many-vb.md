---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: 多数 (VB) から 1 つのユーザー アカウントを選択するインターフェイスを作成 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、ページング、フィルターを適用できるグリッドによるユーザー インターフェイスを構築します。 具体的には、ユーザー インターフェイスは、一連のあるので構成されます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 56f4d72993bfcb9629d6b4cd08efe0da6dea2486
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>多数 (VB) から 1 つのユーザー アカウントを選択するインターフェイスの構築
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> このチュートリアルでは、ページング、フィルターを適用できるグリッドによるユーザー インターフェイスを構築します。 具体的には、ユーザー インターフェイスではある一連のユーザー名と一致するユーザーに表示する GridView コントロールの開始文字に基づく結果をフィルター処理します。 まず、GridView のユーザー アカウントのすべてを一覧表示します。 次に、手順 3 であるフィルターを追加します。 手順 4 は、フィルター選択された結果をページングします。 4 までの手順 2 で構築されたインターフェイスは、特定のユーザー アカウントの管理タスクを実行すると、以降のチュートリアルで使用されてはします。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ロールをユーザーに割り当てる*](../roles/assigning-roles-to-users-vb.md)チュートリアルでは、管理者ユーザーを選択し、自分のロールを管理するための基本的なインターフェイスを作成しました。 具体的には、インターフェイスでは、すべてのユーザーのドロップダウン リストで、管理者が表示されます。 このようなインターフェイスがありますが、ユーザーの数が 12、アカウントしますが、数百または数千のアカウントのサイト用に複雑になる場合に適しています。 ページング、フィルターを適用できるグリッドとは、多数のユーザー ベースの web サイトの適切なユーザー インターフェイスです。

このチュートリアルでは、このようなユーザー インターフェイスを構築します。 具体的には、ユーザー インターフェイスではある一連のユーザー名と一致するユーザーに表示する GridView コントロールの開始文字に基づく結果をフィルター処理します。 まず、GridView のユーザー アカウントのすべてを一覧表示します。 次に、手順 3 であるフィルターを追加します。 手順 4 は、フィルター選択された結果をページングします。 4 までの手順 2 で構築されたインターフェイスは、特定のユーザー アカウントの管理タスクを実行すると、以降のチュートリアルで使用されてはします。

開始しましょう!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新規の ASP.NET ページの追加

このチュートリアルで次の 2 つお検査さまざまな管理に関連する関数と機能します。 一連のトピックでは、これらのチュートリアル全体で検証を実装する ASP.NET ページが必要ですが。 これらのページを作成し、サイト マップを更新してみましょう。

という名前のプロジェクトに新しいフォルダーを作成して開始`Administration`です。 次に、リンクの各ページで、フォルダーを新しい 2 つの ASP.NET ページを追加、`Site.master`マスター ページ。 ページの名前を付けます。

- `ManageUsers.aspx`
- `UserInformation.aspx`

また、web サイトのルート ディレクトリに 2 つのページを追加:`ChangePassword.aspx`と`RecoverPassword.aspx`です。

これら 4 つのページ、この時点では、マスター ページの contentplaceholders にごとに 1 つ、2 つのコンテンツ コントロール:`MainContent`と`LoginContent`です。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

マスター ページの既定のマークアップを表示すること、 `LoginContent` ContentPlaceHolder これらのページです。 このため、削除の宣言型マークアップ、`Content2`コンテンツ コントロールです。 その後、ページのマークアップは、1 つだけのコンテンツ コントロールを含める必要があります。

ASP.NET ページで、`Administration`管理ユーザーにのみフォルダーが目的としています。 システムを管理者ロールに追加しました、 <a id="_msoanchor_2"> </a> [*の作成とロールの管理*](../roles/creating-and-managing-roles-vb.md)チュートリアルです。 このロールにこれら 2 つのページへのアクセスを制限します。 これを実現するには追加、`Web.config`ファイルの名前を`Administration`フォルダーを構成して、`<authorization>`管理者ロールのユーザーを許可し、その他のすべてを拒否する要素。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

この時点で、プロジェクトのソリューション エクスプ ローラーはのスクリーン ショット、図 1 に示すようになります。


[![次の 4 つの新しいページと、Web.config ファイルが、web サイトに追加されました](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**図 1**: 次の 4 つの新しいページおよび`Web.config`ファイルは、web サイトに追加されている ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))


最後に、サイト マップを更新 (`Web.sitemap`) に項目を追加し、`ManageUsers.aspx`ページ。 後に次の XML を追加、`<siteMapNode>`チュートリアルでは、ロールを追加しました。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

更新されたサイト マップには、ブラウザーを使用してサイトを参照してください。 図 2 では、左側のナビゲーションには、管理のチュートリアルのアイテムが含まれます。


[![サイト マップには、ユーザーの管理をという名前のノードが含まれています。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**図 2**: サイト マップにはノードという名前のユーザー管理が含まれています ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>手順 2: GridView 内のすべてのユーザー アカウントを一覧表示します。

このチュートリアルの最後の目標は、管理者が管理するユーザー アカウントを選択するページング、フィルターを適用できるグリッドを作成します。 一覧から始めましょう*すべて*GridView 内のユーザーです。 この処理が完了した後、フィルター処理とページング インターフェイスと機能を追加します。

開く、 `ManageUsers.aspx`  ページで、`Administration`フォルダー、GridView の追加の設定とその`ID`に`UserAccounts`GridView を使用して、ユーザー アカウントのセットにバインドするコードの記述は、すぐに、`Membership`クラスの`GetAllUsers`メソッドです。 前のチュートリアルで説明したように、`GetAllUsers`メソッドを返します、`MembershipUserCollection`コレクションであるオブジェクトの`MembershipUser`オブジェクト。 各`MembershipUser`コレクション内のようなプロパティが含まれています。 `UserName`、 `Email`、`IsApproved`のようにします。

GridView で目的のユーザー アカウント情報を表示するために設定、GridView の`AutoGenerateColumns`プロパティを False の BoundFields を追加し、 `UserName`、 `Email`、および`Comment`プロパティとの CheckBoxFields、 `IsApproved`、`IsLockedOut`、および`IsOnline`プロパティです。 コントロールの宣言型マークアップ、または [フィールド] ダイアログ ボックスを使用して、この構成を適用できます。 図 3 は、フィールドの自動生成チェック ボックスがオフになっていると、BoundFields と CheckBoxFields が追加して構成した後に、フィールドのスクリーン ショットのダイアログ ボックスを示します。


[![次の 3 つ BoundFields と 3 つ CheckBoxFields GridView を追加します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**図 3**: 次の 3 つ BoundFields の追加と GridView に 3 つの CheckBoxFields ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))


構成した後、GridView に、宣言型マークアップが、次のようになっていること確認します。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

次に、ユーザー アカウントを GridView にバインドするコードを記述する必要があります。 という名前のメソッドを作成する`BindUserAccounts`してこのタスクを実行しからを呼び出して、`Page_Load`最初のページ アクセス時にイベント ハンドラー。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

ブラウザーでページをテストするのにはしばらくを実行します。 図 4 に示す、 `UserAccounts` GridView がシステムにユーザー名、電子メール アドレス、およびすべてのユーザーには、その他の関連するアカウント情報を一覧表示されます。


[![ユーザー アカウントが GridView で一覧表示します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**図 4**: GridView に、ユーザー アカウントが表示されます ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>手順 3: ユーザー名の最初の文字が結果のフィルター処理

現在、 `UserAccounts` GridView を示しています*すべて*ユーザー アカウントです。 数百または数千のユーザー アカウントと web サイトの場合は、すぐに表示されているアカウントを縮小できるユーザーであるが不可欠です。 これは、あるページにフィルターを追加することで実現できます。 ページにある 27 を追加してみましょう。: アルファベットの各文字の 1 つの LinkButton と共にすべてという 1 つです。 GridView の訪問者には、すべての LinkButton がクリックすると、すべてのユーザーが表示されます。 特定の文字をクリックした場合は、ユーザー名が、選択した文字で開始しているユーザーのみが表示されます。

最初のタスクでは、27 LinkButton コントロールを追加します。 27 あるを宣言して、作成する 1 つのオプションを一度に 1 つです。 柔軟なアプローチを持つリピータ コントロールを使用する、 `ItemTemplate` LinkButton を表示し、としてリピータにフィルター処理オプションをバインドする、`String`配列。

リピータ コントロール上のページを追加するスタート、 `UserAccounts` GridView です。 リピータの設定`ID`プロパティを`FilteringUI`リピータのテンプレートを構成するようにその`ItemTemplate`LinkButton をレンダリングが`Text`と`CommandName`プロパティ、現在の配列要素にバインドされます。 示したように、 <a id="_msoanchor_3"> </a> [*ロールをユーザーに割り当てる*](../roles/assigning-roles-to-users-vb.md)チュートリアルでは、これを使用して、 `Container.DataItem` databinding 構文です。 リピータを使用して`SeparatorTemplate`を各リンクの間で縦線を表示します。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

このリピータ必要なフィルタ リング オプションを設定するには、という名前のメソッドを作成`BindFilteringUI`です。 このメソッドを呼び出すことを確認する、`Page_Load`イベント ハンドラーに、最初のページ読み込み。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

このメソッドでは、フィルター処理オプションを指定の要素として、`String`配列`filterOptions`リピータがの LinkButton を表示、配列内の各要素に対してその`Text`と`CommandName`配列の値に割り当てられているプロパティ要素。

図 5 は、`ManageUsers.aspx`ページをブラウザーで表示する場合。


[![リピータ 27 フィルターのあるを一覧表示します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**図 5**:「Repeater を一覧表示 27 フィルタ リングある ([フルサイズのイメージを表示するには、をクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))


> [!NOTE]
> ユーザー名を数字や句読点などの任意の文字で開始可能性があります。 これらのアカウントを表示するのには、管理者は、すべて LinkButton オプションを使用する必要があります。 また、数字で始まるすべてのユーザー アカウントを返す LinkButton を追加できます。 私のままに練習として、リーダーの。


フィルターのあるをクリックするとポストバックが発生させ、リピータの`ItemCommand`イベント、きたところにあるために、グリッド内の変更はありませんが、結果をフィルター処理するためのコードを記述します。 `Membership`クラスが含まれています、 [ `FindUsersByName`メソッド](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)それらのユーザー アカウントを指定した検索パターンに一致するユーザー名を返します。 このメソッドを使用しているユーザー名がで指定された文字で始めるには、これらのユーザー アカウントを取得できます、`CommandName`のフィルター選択された LinkButton がクリックされたことです。

更新することで開始、`ManageUser.aspx`ページの分離コード クラスという名前のプロパティが含まれるように`UsernameToMatch`このプロパティは、ポストバック間でユーザー名のフィルター文字列を永続化します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

`UsernameToMatch`プロパティに割り当てられている値を格納する、 `ViewState` UsernameToMatch キーを使用してコレクション。 このプロパティの値が読み取られると、確認に値が存在するかどうか、`ViewState`コレクションです。 ない場合、既定値、空の文字列を返します。 `UsernameToMatch`プロパティは、つまり、プロパティへの変更がポストバック間で永続化できるように状態を表示する値を保持する、一般的なパターンを示します。 このパターンの詳細については、「[について ASP.NET ビューステート](https://msdn.microsoftn-us/library/ms972976.aspx)です。

次に、更新、`BindUserAccounts`呼び出し元の代わりにメソッド`Membership.GetAllUsers`、呼び出し`Membership.FindUsersByName`の値を渡して、 `UsernameToMatch` SQL ワイルドカード文字が付いたプロパティ % です。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

ユーザー名は、文字 A で始まるには、これらのユーザーを表示するには、次のように設定します、`UsernameToMatch`プロパティ A をまず`BindUserAccounts`への呼び出しでこの結果`Membership.FindUsersByName("A%")`、A. 同様に、戻りますで始まるすべてのユーザー名を返しますが。*すべて*、ユーザーの割り当てに空の文字列、`UsernameToMatch`プロパティできるように、`BindUserAccounts`メソッドを呼び出す`Membership.FindUsersByName("%")`、それによってすべてのユーザー アカウントを取得します。

リピータのイベント ハンドラーを作成する`ItemCommand`イベント。 このイベントは、あるフィルターのいずれかをクリックするとします。クリックされた LinkButton の渡された`CommandName`を通じて値、`RepeaterCommandEventArgs`オブジェクト。 適切な値を割り当てる必要があります、`UsernameToMatch`プロパティと、呼び出し、`BindUserAccounts`メソッドです。 場合、`CommandName`はすべてに空の文字列を割り当てる`UsernameToMatch`すべてのユーザー アカウントが表示されるようにします。 それ以外の場合、割り当て、`CommandName`値です。 `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

このコードでフィルタ リングの機能をテストします。 ページが最初にアクセスされると、すべてのユーザー アカウントが表示されます (戻るは図 5 を参照してください)。 A LinkButton がクリックすると、ポストバックを発生させるし、A で始まるユーザー アカウントのみを表示する、結果をフィルター処理します。


[![フィルターのあるを使用して、ユーザー名が特定の英字で始まるそれらのユーザーを表示](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**図 6**: フィルターのあるを使用して、それらのユーザーをユーザー名から始まり、ある文字を表示 ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>手順 4: ページングを使用して GridView の更新

図 5 と 6 に示すように GridView には、すべてから返されるレコードの一覧表示、`FindUsersByName`メソッドです。 数百または数千のユーザー アカウントがある場合につながります情報の過負荷 (同様にすべての LinkButton をクリックしたとき、または最初に、ページにアクセスしたとき) に、すべてのアカウントを表示するときにします。 管理しやすいチャンクでユーザー アカウントを表示するには、一度に 10 個のユーザー アカウントを表示する GridView を構成してみましょう。

GridView コントロールには、ページングの 2 つの種類が用意されています。

- **既定のページング**- 簡単に実装しますが、非効率的です。 簡単に言うと、既定値が GridView でページングが必要ですが*すべて*そのデータ ソースからレコードのです。 表示のみレコードの適切なページです。
- **カスタム ページング**-より多くの作業を実装する必要がありますが、既定のデータのページング カスタム ソースが表示するレコードの正確なセットだけを返すためにのページングよりも効率的です。

数千のレコードをページングするとき、既定およびカスタム ページングのパフォーマンスの差を大幅に指定できます。 構築おため、仮定し、このインターフェイスが数百または数千のユーザー アカウントを更新した後、カスタム ページングを使用してみましょう可能性があります。

> [!NOTE]
> 既定とカスタム ページングを実装するカスタム ページングに関連する課題間の相違点の詳細についてを参照してください[効率的にから大規模な量のデータのページング](https://asp.net/learn/data-access/tutorial-25-vb.aspx)です。 既定およびカスタム ページングのパフォーマンスの差の分析、次を参照してください。 [ASP.NET での SQL Server 2005 でカスタム ページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)です。


カスタム ページングを実装するには、GridView によって表示されているレコードの正確なサブセットを取得するには、その機構まず必要あります。 良いニュースを`Membership`クラスの`FindUsersByName`メソッドにより、ページのインデックスと、ページ サイズを指定するオーバー ロードがあり、そのレコードの範囲内にあるユーザー アカウントのみを返します。

特に、このオーバー ロードには、次の署名: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx)です。

*PageIndex*パラメーターは、返されるユーザー アカウントのページを指定します。*pageSize*ページごとに表示するレコードの数を示します。 *TotalRecords*パラメーターは、`ByRef`ユーザー ストアに合計ユーザー アカウントの数を返すパラメーターです。

> [!NOTE]
> によって返されるデータ`FindUsersByName`%username; に並べ替えて、並べ替えの条件をカスタマイズすることはできません。


GridView は、のみ、ObjectDataSource コントロールにバインドすると、カスタム ページングを利用を構成できます。 カスタム ページングを実装する、ObjectDataSource コントロールの 2 つの方法が必要ですいずれかの開始行インデックスおよび表示するにはレコードの最大数に渡されます; スパン内にあるレコードの正確なサブセットを返しますと。およびを介してページングされるレコードの合計数を返すメソッド。 `FindUsersByName`オーバー ロードが、ページのインデックスと、ページ サイズを受け取り、を通じてレコードの合計数を返します、`ByRef`パラメーター。 したがって、インターフェイスの不一致があります。

1 つのオプションは、ObjectDataSource が必要ですが、してから内部的にインターフェイスを公開するプロキシ クラスを作成すること、`FindUsersByName`メソッドです。 別のオプション - と、この記事の内容を使用します - 独自ページング インターフェイスを作成して、GridView の組み込みのページング インターフェイスの代わりに使用するがします。

### <a name="creating-a-first-previous-next-last-paging-interface"></a>まず、作成する前に、ページング インターフェイスを次に、前回

最初、Previous、次に、最後にあると、ページング インターフェイスを構築してみましょう。 最初の LinkButton がクリックするは前戻ります彼前のページに対し、データの最初のページにユーザーになります。 同様に、次へと最後はユーザーへの移動次と最後のページで、それぞれします。 下にある 4 つの LinkButton コントロールを追加、 `UserAccounts` GridView です。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

LinkButton の各イベント ハンドラーを次に、作成`Click`イベント。

図 7 は、Visual Web Developer のデザイン ビューで表示したときに、次の 4 つあるを示します。


[![最初、直前の動作を次に、追加し、最後の GridView の下にあります。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**図 7**: 最初の追加、Previous、次へ] および [GridView 下にある最後のある ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>現在のページ インデックスの追跡

ユーザーが最初にアクセスすると、`ManageUsers.aspx`ボタン、フィルター選択のいずれかのページまたはクリック数、GridView でデータの最初のページを表示したいです。 ただし、ユーザーでは、あるナビゲーションの 1 つクリックすると、ときに、ページのインデックスを更新する必要があります。 ページのインデックスと 1 ページに表示するレコードの数を維持するには、ページの分離コード クラスに次の 2 つのプロパティを追加します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

同様に、 `UsernameToMatch` 、プロパティ、`PageIndex`プロパティの状態を表示するのには、その値が引き続き発生します。 読み取り専用`PageSize`10、ハード コーディングされた値を返します。 招待するには、関心のリーダーと同様のパターンを使用するには、このプロパティを更新する`PageIndex`、およびその強化するために、`ManageUsers.aspx`ページごとに表示するユーザー アカウントの数のページをページにアクセスするユーザーが指定できるようにします。

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>現在のページのレコードだけを取得する、ページ インデックスを更新して有効化、およびページング インターフェイスあるを無効にします。

場所でのページング インターフェイスおよび`PageIndex`と`PageSize`プロパティの追加、更新する準備が整いました、`BindUserAccounts`メソッドを使用する適切なので`FindUsersByName`オーバー ロードします。 さらに、して、このメソッドは、有効にするにまたはによってどのようなページが表示されているページング インターフェイスを無効にする必要があります。 データの最初のページを表示すると、最初と前のリンクを無効にする必要があります。最後のページを表示するときに、次に、前回を無効にする必要があります。

`BindUserAccounts` メソッドを次のコードで更新します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

最後のパラメーターによってを介してページングされるレコードの合計数が決定されるに注意してください、`FindUsersByName`メソッドです。 ユーザー アカウントの指定したページが返された後、4 つあるか、有効または無効に、データの最初と最後のページが表示されているかどうかによって異なります。

最後の手順では 4 つあるのに対して、コードを記述する`Click`イベント ハンドラー。 これらのイベント ハンドラーを更新する必要があります、`PageIndex`プロパティへの呼び出しを使用して GridView にデータをバインドし、`BindUserAccounts`イベント ハンドラーは、最初、Previous、および 次へは非常に単純です。 `Click`イベント ハンドラーの最後の LinkButton ただしはもう少し複雑なため、最後のページ インデックスを特定するために表示されるレコードの数を決定する必要があります。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

図 8 と 9 は、アクションで、カスタム ページング インターフェイスを表示します。 図 8 は、`ManageUsers.aspx`ページのすべてのユーザー アカウントのデータの最初のページを表示するときにします。 13 アカウント 10 行だけが表示されることに注意してください。 更新プログラム、ポストバックを発生させる次へ または最後のリンクをクリックして、 `PageIndex` 1、およびバインド グリッドへのアカウントのユーザーの 2 ページ目に (図 9 を参照してください)。


[![最初の 10 ユーザー アカウントが表示されます。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**図 8**: の最初の 10 ユーザー アカウントが表示されます ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))


[![ユーザー アカウントの 2 ページ目を表示する次のリンクをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**図 9**: 次のリンクをクリックすると、2 番目のページのユーザー アカウントが表示されます ([フルサイズのイメージを表示するをクリックして](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))


## <a name="summary"></a>まとめ

多くの場合、管理者は、ユーザー アカウントの一覧から選択する必要があります。 前のチュートリアルで、ユーザーが、設定のドロップダウン リストを使用してきましたが、このアプローチのスケーラビリティが得られない。 このチュートリアルではため、探索がより優れた代替: ページングされた GridView にある結果が表示されます、フィルターを適用できるインターフェイス。 このユーザー インターフェイスを持つ管理者できます迅速かつ効率的に見つけて何千もの間で 1 つのユーザー アカウントを選択します。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET での SQL Server 2005 でのカスタムのページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [効率的に大量のデータのページング](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Alicja Maziarz しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ

> [!div class="step-by-step"]
> [前へ](unlocking-and-approving-user-accounts-cs.md)
> [次へ](recovering-and-changing-passwords-vb.md)
