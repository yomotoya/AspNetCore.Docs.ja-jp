---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: 多くの (VB) から 1 つのユーザー アカウントを選択するインターフェイスの構築 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ページング、フィルター可能の grid でのユーザー インターフェイスを作成します。 具体的には、ユーザー インターフェイスは、一連の Linkbutton ので構成されます.
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: bb30c5d3ce6e04f60d8192e8ed0404b89031b4b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832146"
---
<a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>インターフェイスを構築する多くの (VB) から 1 つのユーザー アカウントを選択するには
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> このチュートリアルでは、ページング、フィルター可能の grid でのユーザー インターフェイスを作成します。 具体的には、ユーザー インターフェイスは、ユーザー名と一致するユーザーを表示する GridView コントロールの開始文字に基づく結果をフィルター処理するためのある一連の構成されます。 まず、GridView でユーザー アカウントのすべてを一覧表示します。 次に、手順 3 であるフィルターを追加します。 手順 4 がフィルター処理された結果をページングします。 特定のユーザー アカウントの管理タスクを実行するは、後のチュートリアルから 4 までの手順 2 で構築されたインターフェイスを使用するされます。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ユーザー ロールの割り当て*](../roles/assigning-roles-to-users-vb.md)チュートリアルでは、管理者がユーザーを選択し、自分のロールを管理する基本的なインターフェイスを作成しました。 具体的には、インターフェイスでは、すべてのユーザーのドロップダウン リストで、管理者が表示されます。 このようなインターフェイスはありますが、数十個ものユーザー アカウントします、ははるかに数百または数千のアカウントを持つサイトの場合に適しています。 ページング、フィルター可能グリッドとは、大規模なユーザー ベースの web サイトの方が適切なユーザー インターフェイスです。

このチュートリアルでは、このようなユーザー インターフェイスを作成します。 具体的には、ユーザー インターフェイスは、ユーザー名と一致するユーザーを表示する GridView コントロールの開始文字に基づく結果をフィルター処理するためのある一連の構成されます。 まず、GridView でユーザー アカウントのすべてを一覧表示します。 次に、手順 3 であるフィルターを追加します。 手順 4 がフィルター処理された結果をページングします。 特定のユーザー アカウントの管理タスクを実行するは、後のチュートリアルから 4 までの手順 2 で構築されたインターフェイスを使用するされます。

それでは、始めましょう!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新しい ASP.NET ページの追加

このチュートリアルで次の 2 つ検査さまざまな管理関連の機能と機能します。 一連のトピックでは、このチュートリアルで調査を実装するために ASP.NET ページを必要になります。 これらのページを作成して、サイト マップを更新してみましょう。

という名前のプロジェクトで新しいフォルダーを作成して開始`Administration`します。 次に、2 つの新しい ASP.NET ページを使用する各ページのリンク、フォルダーに追加、`Site.master`マスター ページ。 ページの名前を付けます。

- `ManageUsers.aspx`
- `UserInformation.aspx`

2 つのページを web サイトのルート ディレクトリに追加することも:`ChangePassword.aspx`と`RecoverPassword.aspx`します。

これら 4 つのページが、マスター ページの ContentPlaceHolders ごとに 1 つ、2 つのコンテンツ コントロールがある必要があります、この時点では、:`MainContent`と`LoginContent`します。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

マスター ページの既定のマークアップを表示する、`LoginContent`これらのページのプレース ホルダーです。 そのため、宣言型マークアップを削除する、`Content2`コンテンツ コントロール。 その後、ページのマークアップは、1 つだけのコンテンツ コントロールを含める必要があります。

ASP.NET ページで、`Administration`管理者のユーザー専用のフォルダーが対象としています。 システム管理者の役割を追加しました、 <a id="_msoanchor_2"> </a> [*ロールの管理の作成と*](../roles/creating-and-managing-roles-vb.md)チュートリアル; このロールにこれら 2 つのページへのアクセスを制限します。 これを行うには、追加、`Web.config`ファイルを`Administration`フォルダーを構成して、`<authorization>`管理者ロールのユーザーを正直に言うと、他のすべてを拒否する要素。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

この時点で、プロジェクトのソリューション エクスプ ローラーのスクリーン ショット、図 1 に示すようなはずです。


[![4 つの新しいページと Web.config ファイルが web サイトに追加されました](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**図 1**: 4 つの新しいページと`Web.config`ファイルは、web サイトに追加されている ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))。


最後に、サイト マップを更新 (`Web.sitemap`) にエントリを含める、`ManageUsers.aspx`ページ。 追加した後、次の XML、`<siteMapNode>`チュートリアルでは、ロールを追加しました。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

更新サイト マップでは、ブラウザーを使用してサイトを参照してください。 図 2 に示す、左側のナビゲーションには、管理のチュートリアルのアイテムが含まれます。


[![サイト マップには、「ユーザーの管理ノードが含まれていますいます。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**図 2**: サイト マップにはノードという名前のユーザー管理が含まれています ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))。


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>手順 2: GridView を内のすべてのユーザー アカウントを一覧表示します。

このチュートリアルでは、最終目標では、管理者が管理するユーザー アカウントを選択、ページング、フィルター可能グリッドを作成します。 一覧から始めましょう*すべて*GridView でのユーザー。 これが完了すると、フィルター処理とページング インターフェイスと機能を追加します。

開く、`ManageUsers.aspx`ページで、`Administration`フォルダー、GridView では、追加の設定とその`ID`に`UserAccounts`、すぐには、GridView を使用して、一連のユーザー アカウントにバインドするコードを記述しました、`Membership`クラスの`GetAllUsers`メソッド。 前のチュートリアルで説明したように、`GetAllUsers`メソッドを返します。 を`MembershipUserCollection`コレクションであるオブジェクトの`MembershipUser`オブジェクト。 各`MembershipUser`コレクションにはなどのプロパティが含まれています。 `UserName`、 `Email`、`IsApproved`など。

GridView で目的のユーザー アカウントの情報を表示するために設定、GridView の`AutoGenerateColumns`プロパティを False の BoundFields を追加し、 `UserName`、 `Email`、および`Comment`プロパティとの CheckBoxFields、 `IsApproved`、`IsLockedOut`、および`IsOnline`プロパティ。 コントロールの宣言型マークアップまたはフィールド ダイアログ ボックスを使用して、この構成を適用できます。 図 3 は、自動生成フィールド チェック ボックスがオフになっていると、BoundFields と CheckBoxFields が追加して構成した後に、フィールドのスクリーン ショットのダイアログ ボックスを示します。


[![GridView に 3 つ BoundFields と 3 つ CheckBoxFields を追加します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**図 3**: 次の 3 つ BoundFields の追加と GridView に 3 つの CheckBoxFields ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))。


GridView を構成した後、その宣言型マークアップが次のようなことを確認します。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

次に、ユーザー アカウントを GridView にバインドするコードを記述する必要があります。 という名前のメソッドを作成する`BindUserAccounts`このタスクを実行してから呼び出して、`Page_Load`最初のページ アクセス時にイベント ハンドラー。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

ブラウザーでページをテストする時間がかかります。 図 4 に示すよう、 `UserAccounts` GridView がシステムにユーザー名、電子メール アドレス、およびすべてのユーザーの場合は、その他の関連するアカウント情報を一覧表示されます。


[![ユーザーのアカウントは、gridview 一覧します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**図 4**: GridView で、ユーザー アカウントが表示されている ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))。


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>手順 3: ユーザー名の最初の文字で結果をフィルター処理

現在、 `UserAccounts` GridView 表示*すべて*のユーザー アカウント。 数百または何千ものユーザー アカウントの web サイトでは、そのユーザーが表示されているアカウントをすばやく縮小することが不可欠です。 これは、Linkbutton をページにフィルターを追加することで実現できます。 27 Linkbutton をページに追加してみましょう。 アルファベットの各文字の 1 つの LinkButton と共にすべてという 1 つ。 訪問者には、すべての LinkButton がクリックすると、GridView はすべてのユーザーに表示されます。 特定の文字をクリックすると、選択した文字から始まりますユーザー名を持つユーザーのみが表示されます。

最初のタスクでは、27 LinkButton コントロールを追加します。 ここでは、27 Linkbutton を作成する 1 つのオプションを一度に 1 つ。 柔軟性の高いアプローチは、使用して、Repeater コントロールを使用する、 `ItemTemplate` linkbutton コントロールをレンダリングし、として Repeater にフィルター オプションをバインドする、`String`配列。

スタート ページの上部に、Repeater コントロールを追加することで、 `UserAccounts` GridView。 Repeater の設定`ID`プロパティを`FilteringUI`Repeater のテンプレートを構成するようにその`ItemTemplate`linkbutton コントロールをレンダリングが`Text`と`CommandName`プロパティ、現在の配列要素にバインドされます。 説明したように、 <a id="_msoanchor_3"> </a> [*ユーザー ロールの割り当て*](../roles/assigning-roles-to-users-vb.md)チュートリアルでは、これを使用して、`Container.DataItem`データ バインド構文。 使用して、Repeater の`SeparatorTemplate`各リンクの間の垂直線を表示します。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

この Repeater、目的のフィルター処理オプションを設定するには、という名前のメソッドを作成`BindFilteringUI`です。 このメソッドを呼び出すことを確認する、`Page_Load`最初のページの読み込み時のイベント ハンドラー。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

このメソッド内の要素としてフィルター処理オプションを指定します、`String`配列`filterOptions`リピータが LinkButton を表示、配列内の各要素に対してその`Text`と`CommandName`配列の値に割り当てられたプロパティ要素。

図 5 は、`ManageUsers.aspx`ページをブラウザーで表示する場合。


[![リピータが 27 のフィルタ リング Linkbutton を一覧表示します。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**図 5**:、Repeater を一覧表示 27 フィルター Linkbutton ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))。


> [!NOTE]
> ユーザー名は、数字や句読点などの任意の文字で開始できます。 これらのアカウントを表示するには、管理者はすべて LinkButton オプションを使用して、必要があります。 または、LinkButton を返す数値で始まるすべてのユーザー アカウントを追加できます。 ままを演習として、リーダーの。


ポストバックが発生するフィルターの Linkbutton をクリックすると、Repeater のさせ`ItemCommand`イベント、まだために、グリッド内の変更はありませんが、結果をフィルター処理するコードを記述します。 `Membership`クラスが含まれています、 [ `FindUsersByName`メソッド](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)ユーザー名を持つ指定した検索パターンに一致するユーザー アカウントを返します。 このメソッドを使用しているユーザー名がで指定された文字で始まるには、これらのユーザー アカウントを取得することができます、`CommandName`フィルター選択された LinkButton がクリックされるのです。

更新することで開始、`ManageUser.aspx`ページの分離コード クラスという名前のプロパティが含まれるように`UsernameToMatch`このプロパティは、ポストバック間でユーザー名のフィルター文字列を永続化します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

`UsernameToMatch`プロパティに割り当てられている値が格納、 `ViewState` UsernameToMatch キーを使用してコレクション。 このプロパティの値は読み取り専用に値が存在するかどうかにチェック、`ViewState`はない場合、既定値、空の文字列を返します。 `UsernameToMatch`プロパティ namely プロパティに変更がポストバック間で永続化するために状態を表示する値を保持する、一般的なパターンが発生します。 このパターンの詳細については、読み取る[について ASP.NET ビュー ステート](https://msdn.microsoftn-us/library/ms972976.aspx)します。

次に、更新、`BindUserAccounts`メソッドを呼び出し元の代わりに`Membership.GetAllUsers`、呼び出します`Membership.FindUsersByName`の値で渡し、`UsernameToMatch`プロパティは、SQL ワイルドカード文字が付加されます %。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

文字 A で始まるがユーザー名には、これらのユーザーを表示するには、設定、 `UsernameToMatch` a プロパティを呼び出して`BindUserAccounts`への呼び出しでこの結果は`Membership.FindUsersByName("A%")`、A. 同様に、返すで始まるユーザー名を持つすべてのユーザーが返されます*すべて*、ユーザーの割り当てに空の文字列、`UsernameToMatch`プロパティように、`BindUserAccounts`メソッドを呼び出す`Membership.FindUsersByName("%")`のため、すべてのユーザー アカウントを取得します。

Repeater のイベント ハンドラーを作成`ItemCommand`イベント。 フィルター Linkbutton のいずれかがクリックされたときに、このイベントが発生しますクリックされた LinkButton の渡された`CommandName`値を通じて、`RepeaterCommandEventArgs`オブジェクト。 適切な値を代入する必要があります、`UsernameToMatch`プロパティと、呼び出し、`BindUserAccounts`メソッド。 場合、`CommandName`はすべてに空の文字列を割り当てる`UsernameToMatch`すべてのユーザー アカウントが表示されるようにします。 それ以外の場合、割り当て、`CommandName`値です。 `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

このコードでフィルター処理の機能をテストします。 ページが初めてアクセスした場合、すべてのユーザー アカウントが表示されます (バックアップは図 5 を参照してください)。 A LinkButton をクリックすると、ポストバックが発生して、A で始まるユーザー アカウントのみを表示する、結果をフィルター処理します。


[![フィルター処理の Linkbutton を使用して、ユーザー名は、特定の文字で始まるユーザーを表示するには](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**図 6**: フィルター Linkbutton を使用して、それらのユーザーをで始まるユーザー名、特定の文字を表示する ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))。


## <a name="step-4-updating-the-gridview-to-use-paging"></a>手順 4: ページングを使用して、GridView を更新しています

図 5 および 6 に示すように、GridView では、すべてから返されるレコードの一覧表示、`FindUsersByName`メソッド。 数百または何千ものユーザー アカウントがある場合、(この場合はすべての LinkButton をクリックすると、または最初に、ページにアクセスしたとき) と同様に、すべてのアカウントを表示するときにオーバー ロードにリードこのことができます。 管理しやすいチャンク単位でユーザー アカウントを表示するには、一度に 10 個のユーザー アカウントを表示する GridView を構成しましょう。

GridView コントロールには、ページングの 2 つの種類があります。

- **既定のページング**- 簡単に実装しますが、非効率的です。 簡単に言うと、既定値は、GridView のページングが必要ですが*すべて*のデータ ソースからレコード。 のみが表示されます、適切なページのレコード。
- **カスタム ページング**-より多くの作業を実装する必要がありますが、カスタムのデータのページングとソースが表示するレコードの正確なセットのみを返すので、既定のページングよりも効率的です。

数千のレコードをページングするときに、既定およびカスタム ページングのパフォーマンスの違いは大幅にできます。 構築されたためがいると仮定して、このインターフェイスが数百または何千ものユーザー アカウントを更新した後、カスタム ページングを使用してみましょう可能性があります。

> [!NOTE]
> 既定とカスタム ページング、さらにカスタム ページングを実装に伴う課題の違いの詳細についてを参照してください[効率的にページングを大規模な量のデータ](https://asp.net/learn/data-access/tutorial-25-vb.aspx)します。 既定およびカスタム ページングのパフォーマンスの違いのいくつかの分析では、次を参照してください。 [SQL Server 2005 での ASP.NET でカスタム ページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)します。


カスタム ページングを実装するために、GridView により表示されるレコードの正確なサブセットを取得するには、いくつかメカニズムまず必要あります。 良い知らせは、`Membership`クラスの`FindUsersByName`メソッドがページのインデックスと、ページ サイズを指定することができるようにするオーバー ロードと、そのレコードの範囲内にあるユーザー アカウントのみを返します。

具体的には、このオーバー ロードが、次のシグネチャを持つ: [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx)します。

*PageIndex*パラメーターは、返されるユーザー アカウントのページを指定します。*pageSize*ページごとに表示するレコードの数を示します。 *TotalRecords*パラメーターは、`ByRef`ユーザー ストアに合計ユーザー アカウントの数を返すパラメーターです。

> [!NOTE]
> によって返されるデータ`FindUsersByName`; ユーザー名で並べ替えられますが、並べ替えの条件をカスタマイズすることはできません。


GridView は、カスタムのページングを使用するが、ObjectDataSource コントロールにバインドした場合のみを構成できます。 カスタム ページングを実装する ObjectDataSource コントロール、2 つの方法が必要ですが開始行インデックスおよび表示するにはレコードの最大数に渡される 1 つと正確な; スパン内に含まれるレコードのサブセットを返します。でレコードの合計数を返すメソッドのポケットベル通知します。 `FindUsersByName`オーバー ロードは、ページのインデックスと、ページ サイズを受け入れるし、使用してレコードの合計数を返します、`ByRef`パラメーター。 したがって、インターフェイスの不一致があります。

1 つのオプションが、ObjectDataSource が必要ですと内部的に呼び出すインターフェイスを公開するプロキシ クラスを作成すると、`FindUsersByName`メソッド。 別のオプション - および使用してこの記事のいずれかで独自のページング インターフェイスを作成し、GridView の組み込みのページング インターフェイスではなくを使用することです。

### <a name="creating-a-first-previous-next-last-paging-interface"></a>A まず、作成する前に、次に、最後のページング インターフェイス

最初、Previous、次に、最後の Linkbutton とページング インターフェイスを構築してみましょう。 以前は前のページに彼の連絡先を返す一方、最初の LinkButton がクリックされるは、データの最初のページにユーザーになります。 同様に、次へと最終がユーザーへの移動 次へ と最後のページで、それぞれします。 下にある 4 つの LinkButton コントロールを追加、 `UserAccounts` GridView。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

次に、LinkButton の各イベント ハンドラーを作成`Click`イベント。

図 7 では、Visual Web Developer のデザイン ビューで表示した場合は、次の 4 つ Linkbutton を示します。


[![次に、まず、前を追加して、最後の GridView の下にあります。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**図 7**: 最初の追加、前、[次へと最終 Linkbutton の下に、GridView ([フルサイズの画像を表示する] をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))。


### <a name="keeping-track-of-the-current-page-index"></a>現在のページ インデックスの管理

ユーザーが最初にアクセスすると、`ManageUsers.aspx`ボタン、フィルター選択のいずれかのページまたは数回のクリック、GridView でデータの最初のページを表示するようにします。 ただし、ユーザーには、ナビゲーション Linkbutton の 1 つクリックすると、ときに、ページのインデックスを更新する必要があります。 ページのインデックスと 1 ページに表示するレコードの数を維持するには、ページの分離コード クラスに次の 2 つのプロパティを追加します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

ように、`UsernameToMatch`プロパティ、`PageIndex`プロパティは、状態を表示するには、その値を永続化します。 読み取り専用`PageSize`10、ハード コーディングされた値を返します。 招待興味を持った読者と同様のパターンを使用するには、このプロパティを更新するには`PageIndex`、しを増やすために、`ManageUsers.aspx`ページごとに表示するユーザー アカウントの数のページ、ページにアクセスするユーザーを指定できるようにします。

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>現在のページのレコードだけを取得する、ページのインデックスを更新および有効化とページング インターフェイスの Linkbutton を無効にします。

配置でのページング インターフェイスと`PageIndex`と`PageSize`プロパティの追加、更新する準備ができました、`BindUserAccounts`メソッドを使用する適切なので`FindUsersByName`オーバー ロードします。 さらに、このメソッドを有効にするかによって、どのようなページが表示されているページング インターフェイスを無効にする必要があります。 データの最初のページを表示すると、最初と前のリンクを無効にする必要があります。最後のページを表示するときに、次に、最後を無効にする必要があります。

`BindUserAccounts` メソッドを次のコードで更新します。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

ポケットベル通知でレコードの合計数がの最後のパラメーターによって決定されるに注意してください、`FindUsersByName`メソッド。 ユーザー アカウントの指定したページが返された後、4 つあるか、有効または無効に、データの最初と最後のページが表示されているかどうかに応じて。

最後の手順では 4 つの Linkbutton のコードを記述する`Click`イベント ハンドラー。 これらのイベント ハンドラーを更新する必要があります、`PageIndex`プロパティへの呼び出しを使用して GridView にデータをバインドし、`BindUserAccounts`最初、Previous、および次のイベント ハンドラーは非常にシンプルです。 `Click`最後の LinkButton のイベント ハンドラーが複雑になるため、最後のページ インデックスを決定するために表示されるレコードの数を決定する必要があります。

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

図 8 と 9 は、アクションでカスタム ページング インターフェイスを表示します。 図 8 は、`ManageUsers.aspx`ページすべてのユーザー アカウントのデータの最初のページを表示する場合。 13 のアカウントの 10 個しかが表示されることに注意してください。 [次へ] または最後のリンクをクリックすると、ポストバックで更新プログラム、 `PageIndex` 1、およびバインド グリッドへのアカウントのユーザーの 2 ページ目に (図 9 参照)。


[![最初の 10 のユーザー アカウントが表示されます。](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**図 8**:、最初の 10 のユーザー アカウントが表示されます ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))。


[![次のリンクをクリックするとユーザー アカウントの 2 ページ目を表示します](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**図 9**: ユーザー アカウントの 2 番目のページの次のリンクをクリックすると表示されます ([フルサイズの画像を表示する をクリックします](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))。


## <a name="summary"></a>まとめ

多くの場合、管理者は、アカウントの一覧からユーザーを選択する必要があります。 前のチュートリアルでは、ユーザーを含むドロップダウン リストを使用してしましたが、このアプローチが適切にスケーリングされません。 このチュートリアルでは代替検討しました: ページングされた GridView で結果が表示されます、フィルター可能なインターフェイスです。 このユーザー インターフェイスでは、管理者はことができますを検索し、迅速かつ効率的に何千もの間で 1 つのユーザー アカウントを選択します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [SQL Server 2005 での ASP.NET でカスタム ページング](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [大量のデータを効率的にページング](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Alicja Maziarz でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に

> [!div class="step-by-step"]
> [前へ](unlocking-and-approving-user-accounts-cs.md)
> [次へ](recovering-and-changing-passwords-vb.md)
