---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: (VB) のユーザー ロールの割り当て |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを構築します。 最初のページは、何を表示する機能が含まれます.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 959a73f53d4fdb114f222fe8bc830876b76c9d9e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892011"
---
<a name="assigning-roles-to-users-vb"></a>(VB) のユーザー ロールの割り当てください。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを構築します。 最初のページには、特定のロールに属しているユーザーを表示する機能が含まれては特定のユーザーが属するロールと権限を割り当てたり、特定のロールから、特定のユーザーを削除します。 2 番目のページには、新しく作成されたユーザーが所属するロールを指定するステップが含まれるように CreateUserWizard コントロールを補強おがされます。 これは、管理者が新しいユーザー アカウントを作成することであるシナリオで役に立ちます。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a>[前のチュートリアル](creating-and-managing-roles-vb.md)ロール フレームワークを調べると、 `SqlRoleProvider`; を使用する方法を説明しました、`Roles`クラスを作成、取得、およびロールを削除します。 を作成し、ロールを削除するだけでなく割り当てまたはロールからユーザーを削除できる必要があります。 残念ながら、ASP.NET では、どのようなユーザー ロールに属しているを管理するためのすべての Web コントロールが付属していません。 代わりに、これらの関連付けを管理する独自の ASP.NET ページを作成する必要があります。 良いニュースは、追加して、ロールへのユーザーを削除することは非常に簡単です。 `Roles`クラスには、1 つまたは複数のロールに 1 つまたは複数のユーザーを追加するためのメソッドの数が含まれています。

このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを構築します。 最初のページには、特定のロールに属しているユーザーを表示する機能が含まれては特定のユーザーが属するロールと権限を割り当てたり、特定のロールから、特定のユーザーを削除します。 2 番目のページには、新しく作成されたユーザーが所属するロールを指定するステップが含まれるように CreateUserWizard コントロールを補強おがされます。 これは、管理者が新しいユーザー アカウントを作成することであるシナリオで役に立ちます。

開始しましょう!

## <a name="listing-what-users-belong-to-what-roles"></a>ロールに属しているどのようなユーザーを表示します。

このチュートリアルの最初の注文のビジネスでは、元のユーザーをロールに割り当てることができます、web ページを作成します。 前に、自分をロールにユーザーを割り当てる方法と関係おみましょう最初に集中どのようなユーザー ロールに属しているを確認する方法。 この情報を表示する 2 つの方法があります:「ロール"、」ユーザーです"。 により、ロールを選択し、表示をユーザー ("ロール"で表示)、ロールに所属するすべてのビジターまたはにユーザーを選択し、そのユーザー (""のユーザーによって表示) に割り当てられているロールのユーザー入力を可能性があります。

"ロール"でビューは、ここで、訪問者が特定のロールに属しているユーザーのセットを知りたい状況で役に立ちます"ユーザー"によってビューは、訪問者を特定のユーザー ロールを知る必要がある場合に最適です。 みましょう両方「ロール"または」ユーザー"を含む、ページがあるインターフェイスです。

"によって"のユーザー インターフェイスの作成を開始します。 このインターフェイスは、ドロップダウン リストと対応するチェック ボックスの一覧で構成されます。 システムの一連のユーザー ドロップダウン リストは表示されます。対応するチェック ボックスは、ロールを列挙します。 ドロップダウン リストからユーザーを選択すると、ユーザーが所属するこれらの役割を確認します。 ページにアクセスするユーザーは、確認し、またはを追加または削除、選択したユーザー、対応するロールにあるチェック ボックスをオフにできます。

> [!NOTE]
> ドロップダウン リストを使用して、ユーザー アカウントは、web サイトに適してが数百のユーザー アカウントがある可能性があります。 ドロップダウン リストは、ユーザーが比較的短い形式の一覧からオプションの 1 つの項目を選択できるように設計されています。 これは、迅速に手に負えなくリスト項目の数の増大に応じて拡張します。 可能性のある多数のユーザー アカウントを持っている web サイトを作成している可能性がある場合、別のユーザー インターフェイスの使用を検討してページング可能な GridView やを一覧表示するフィルターを適用できるインターフェイスは、文字を選択するビジターを求めるなどしのみユーザー名が、選択した文字で始まるそれらのユーザーを示します。


## <a name="step-1-building-the-by-user-user-interface"></a>手順 1:""のユーザーによってユーザー インターフェイスの構築

開く、`UsersAndRoles.aspx`ページ。 ページの上部にある、という名前のラベルの Web コントロールを追加`ActionStatus`をクリアし、`Text`プロパティです。 このラベルのようにメッセージを表示する、行われた操作に関するフィードバックを提供するを使用する、"ユーザー Tito がロールに追加されて、管理者、"か「ユーザー Jisun はスーパーバイザー ロールから削除されています」。 これらを行うためにメッセージが目立つ設定で、ラベルの`CssClass`プロパティを"Important"です。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

次の CSS クラス定義を次に、追加、`Styles.css`スタイル シート。

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

この CSS 定義では、ブラウザーで、大規模な赤いフォントを使用してラベルを表示するように指示します。 図 1 は、Visual Studio デザイナーでこの特殊効果を示します。


[![ラベルの CssClass プロパティは、大規模な赤いフォントで結果します。](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**図 1**:、ラベルの`CssClass`大、赤いフォントでプロパティの結果 ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image3.png))


次に、設定、DropDownList ページを追加、その`ID`プロパティを`UserList`、設定とその`AutoPostBack`プロパティを True にします。 システム内のすべてのユーザーを一覧表示するのにこの DropDownList を使用します。 この DropDownList MembershipUser オブジェクトのコレクションにバインドされます。 DropDownList を設定するための MembershipUser オブジェクトのユーザー名プロパティを表示 (およびリスト項目の値として使用) に DropDownList、`DataTextField`と`DataValueField`"UserName"をプロパティです。

DropDownList を下にある追加というリピータ`UsersRoleList`です。 このリピータはすべて一覧表示の役割のシステムを一連のチェック ボックスをオンします。 リピータの定義`ItemTemplate`次の宣言型マークアップを使用します。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate`マークアップには、1 つのチェック ボックスを Web コントロールという名前が含まれています。`RoleCheckBox`です。 チェック ボックスの`AutoPostBack`プロパティが True に設定され、`Text`プロパティにバインド`Container.DataItem`です。 理由のデータ バインド構文は単に`Container.DataItem`は、ロール フレームワークには、文字列の配列としてロール名の一覧が返されます。 このリピータにバインドしている文字列配列であるためです。 データの Web コントロールにバインドされている配列の内容を表示する次の構文を使用する理由の詳細な説明についてはこのチュートリアルの範囲外です。 この問題の詳細についてを参照してください[データ Web コントロールへのスカラー配列のバインド](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)です。

この時点で、"によって"のユーザー インターフェイスの宣言型マークアップは、次のようになります。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

これで、DropDownList するユーザー アカウントのセットとリピータへのロールのセットをバインドするコードを記述する準備が整いました。 ページの分離コード クラスには、という名前のメソッドを追加`BindUsersToUserList`というもう`BindRolesList`、次のコードを使用します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList`メソッドはによってシステム内のすべてのユーザー アカウントを取得、 [ `Membership.GetAllUsers`メソッド](https://msdn.microsoft.com/library/dy8swhya.aspx)です。 これを返します、 [ `MembershipUserCollection`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)の集合である[`MembershipUser`インスタンス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)です。 このコレクションにバインドし、 `UserList` DropDownList です。 `MembershipUser`インスタンスのコレクションにはと同様に、さまざまなプロパティが含まれている構成`UserName`、 `Email`、 `CreationDate`、および`IsOnline`です。 DropDownList の値を表示するように指示するために、`UserName`プロパティ、いることを確認、 `UserList` DropDownList の`DataTextField`と`DataValueField`プロパティは、"UserName"に設定されています。

> [!NOTE]
> `Membership.GetAllUsers`メソッドに次の 2 つのオーバー ロードがあります: 入力パラメーターを受け取らずにあり、ユーザーのすべてを返します、もう 1 つで、ページのインデックスと、ページ サイズの整数値を受け取り、指定したユーザーのサブセットのみを返します。 大量のページング可能なユーザー インターフェイス要素に表示されているユーザー アカウントが存在する場合、それらのすべてではなくユーザー アカウントの正確なサブセットだけを返すので、2 番目のオーバー ロードはより効率的にユーザーを使用してページを使用できます。


`BindRolesToList`メソッドを呼び出すことによって開始、`Roles`クラスの[`GetAllRoles`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)システムの役割を含む文字列配列が返されます。 この文字列の配列はリピータにし、バインドされています。

最後に、ページが最初に読み込まれたときに、これら 2 つのメソッドを呼び出す必要があります。 `Page_Load` イベント ハンドラーに次のコードを追加します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

このコードですぐを; ブラウザーでページを参照してください。画面は、図 2 のようになります。 すべてのユーザー アカウントが設定されますドロップダウン リストで、その下に、各ロールは、チェック ボックスとして表示されます。 設定するため、`AutoPostBack`プロパティ DropDownList と対応するチェック ボックスの true の場合、選択したユーザーを変更するか、オンまたはオフにして、ロール ポストバックが発生します。 アクションは実行されません、ただし、まだこれらのアクションを処理するコードを記述しているためです。 これらのタスクを次の 2 つのセクションで取り上げるです。


[![ページは、ユーザーおよびロールが表示されます。](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**図 2**: ページは、ユーザーおよびロールが表示されます ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>選択したユーザーが所属するロールを確認しています

ページが最初に読み込まれたとき、または、訪問者がドロップダウン リストから、新しいユーザーを選択するたびに、更新する必要があります、`UsersRoleList`のチェック ボックス、選択したユーザーがそのロールに属している場合にのみ特定のロール checkbox がオンにできるようにします。 これを実現する、という名前のメソッドを作成する`CheckRolesForSelectedUser`を次のコード。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

選択したユーザーを決定することにより、上記のコードを開始します。 ロールのクラスを使用して、 [ `GetRolesForUser(userName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)文字列の配列としての役割の指定したユーザーのセットを取得します。 リピータのアイテムが列挙された次に、および各項目の`RoleCheckBox` チェック ボックスがプログラムによって参照されています。 チェック ボックスをオンに対応して、ロールに含まれる場合にのみ、`selectedUsersRoles`文字列配列。

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)`構文は、ASP.NET 2.0 を使用している場合はコンパイルされません。 `Contains(Of String)`メソッドの一部である、 [LINQ ライブラリ](http://en.wikipedia.org/wiki/Language_Integrated_Query)、ASP.NET 3.5 に新機能です。 ASP.NET version 2.0 を使用して引き続き場合を使用して、 [ `Array.IndexOf(Of String)`メソッド](https://msdn.microsoft.com/library/eha9t187.aspx)代わりにします。


`CheckRolesForSelectedUser`メソッドは、2 つのケースで呼び出される必要があります: およびページが最初に読み込まれたときに、 `UserList` DropDownList の選択されたインデックスを変更します。 したがってからこのメソッドを呼び出す、`Page_Load`イベント ハンドラー (の呼び出し後に`BindUsersToUserList`と`BindRolesToList`)。 またの DropDownList のイベント ハンドラーを作成`SelectedIndexChanged`イベントからこのメソッドを呼び出すとします。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

配置でこのコードでは、ブラウザーでページをテストできます。 ただし、以降、`UsersAndRoles.aspx`ページに現在のユーザーをロールに割り当てる機能がない、ユーザーにはロールはありません。 いずれかを取得してこのコードが動作する言葉、これは、後で、ことを確認するために、すぐにロールにユーザーを割り当てるためのインターフェイスを作成、またはレコードを挿入することでユーザーをロールに手動で追加することができます、`aspnet_UsersInRoles`この functi をテストするためにテーブルonality ようになりました。

### <a name="assigning-and-removing-users-from-roles"></a>割り当てると、ユーザーの役割の削除

訪問者がチェックや内のチェック ボックスをオフに、 `UsersRoleList` Repeater を追加または対応するロールから、選択したユーザーを削除する必要があります。 チェック ボックスの`AutoPostBack`プロパティは現在でもリピータ内のチェック ボックスがオンまたはオフ ポストバックが発生する True に設定します。 つまり、チェック ボックスのイベント ハンドラーを作成する必要があります`CheckChanged`イベント。 チェック ボックスがリピータ コントロール内にあるため、イベント ハンドラーの組み込みを手動で追加する必要があります。 分離コード クラスをイベント ハンドラーを追加して、開始、`Protected`メソッド、次のようにします。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

すぐにこのイベント ハンドラーのコードを記述することが戻ります。 まずは、イベント処理の組み込みを完了してみましょう。 リピータので、このチェック ボックスから`ItemTemplate`、追加`OnCheckedChanged="RoleCheckBox_CheckChanged"`です。 この構文の接続、`RoleCheckBox_CheckChanged`イベント ハンドラーを`RoleCheckBox`の`CheckedChanged`イベント。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

最後のタスクが完了するには、`RoleCheckBox_CheckChanged`イベント ハンドラー。 イベントが発生したため、このチェック ボックスのインスタンスによるどのような役割が checked または unchecked 経由で CheckBox コントロールを参照することで開始する必要があります、`Text`と`Checked`プロパティです。 この情報は、選択したユーザーのユーザー名と共に使用して、おを追加または削除ユーザー ロールを介して、`Roles`クラスの[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)または[`RemoveUserFromRole`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)です。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

プログラムを利用すると、イベントを発生させた チェック ボックスを参照して、上記のコードを開始、`sender`入力パラメーターです。 チェック ボックスをオンにした場合、指定したロールに、それ以外の場合、ロールから削除された、選択したユーザーが追加されます。 どちらの場合、`ActionStatus`ラベルだけ実行されるアクションの概要を作成するメッセージが表示されます。

すぐをブラウザーからこのページをテストします。 Tito のユーザーを選択し、管理者と管理者の両方のロールに Tito を追加します。


[![Tito が管理者と管理者ロールに追加されました](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**図 3**: Tito が管理者と管理者ロールに追加されました ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image9.png))


次に、ドロップダウン リストからユーザー ブルースを選択します。 ポストバックがあるし、更新されたリピータのチェック ボックス、`CheckRolesForSelectedUser`です。 ブルースまだに属さないすべての役割、ので、2 つのチェック ボックスはチェックされません。 次に、管理者ロールにブルースを追加します。


[![ブルースが管理者ロールに追加されました](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**図 4**: ブルースが管理者ロールに追加されました ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image12.png))


さらの機能を検証する、`CheckRolesForSelectedUser`メソッド、Tito またはブルース以外のユーザーを選択します。 対応するチェック ボックスが自動的にチェックする方法に注意してください、役割に属していないことを示すです。 Tito に戻ります。 管理者と管理者の両方のチェック ボックスをチェックする必要があります。

## <a name="step-2-building-the-by-roles-user-interface"></a>手順 2:"ロール"でユーザー インターフェイスの構築

この時点で"によって"のユーザー インターフェイスが完了し、"ロール"でインターフェイスへの取り組みを開始する準備が整いました。 "ロール"でインターフェイスでは、ドロップダウン リストからロールを選択するように求めるし、GridView でそのロールに属しているユーザーのセットを表示します。

別の DropDownList コントロールを追加して、`UsersAndRoles.aspx page`です。 リピータ コントロールの下に 1 つ配置、名前を付けます`RoleList`、設定とその`AutoPostBack`プロパティを True にします。 その下に、GridView を追加し、名前`RolesUserList`です。 この GridView には、選択したロールに属しているユーザーが表示されます。 GridView の設定`AutoGenerateColumns`プロパティを False、TemplateField をグリッドに追加の`Columns`コレクション、およびセットその`HeaderText`"Users"をプロパティです。 定義 TemplateField の`ItemTemplate`データ バインド式の値を表示するよう`Container.DataItem`で、`Text`という名前のラベルのプロパティ`UserNameLabel`です。

追加し、GridView を構成したら、"ロール"でインターフェイスの宣言型マークアップを次のようになります。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

設定する必要があります、 `RoleList` DropDownList 一連のシステムの役割です。 これを実現する、更新、`BindRolesToList`メソッドがバインドによって文字列の配列が返される、`Roles.GetAllRoles`メソッドを`RolesList`DropDownList (だけでなく`UsersRoleList`リピータ)。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

最後の 2 つの行、`BindRolesToList`メソッドへのロールのセットをバインドに追加された、 `RoleList` DropDownList コントロール。 図 5 は、ドロップダウン リストが、システムの役割を持つが表示されます: ブラウザーを使って表示したときに、最終的な結果を示します。


[![RoleList DropDownList で、ロールが表示されます。](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**図 5**:、ロールが表示されます、 `RoleList` DropDownList ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>選択したロールに属しているユーザーを表示します。

ページが最初に読み込まれるときに、またはからの新しいロールを選択すると、 `RoleList` DropDownList、GridView でそのロールに属しているユーザーの一覧を表示する必要があります。 という名前のメソッドを作成する`DisplayUsersBelongingToRole`次のコードを使用します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

このメソッドは、選択したロールから取得することによって開始、 `RoleList` DropDownList です。 次を使用して、 [ `Roles.GetUsersInRole(roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)そのロールに属しているユーザーのユーザー名の文字列の配列を取得します。 この配列にバインドし、 `RolesUserList` GridView です。

このメソッドは、2 つの状況で呼び出される必要があります。 ページが最初に読み込まれるとき、およびで選択された役割、 `RoleList` DropDownList 変更します。 このため、更新、`Page_Load`イベント ハンドラー呼び出しの後にこのメソッドが呼び出されるように`CheckRolesForSelectedUser`です。 イベント ハンドラーを次に、作成、`RoleList`の`SelectedIndexChanged`イベント、しすぎるそこから、このメソッドを呼び出します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

代わりに、このコードで、 `RolesUserList` GridView で選択したロールに属しているユーザーを表示する必要があります。 スーパーバイザー ロールが 2 つのメンバーで構成図 6 に示す: ブルースと Tito です。


[![GridView は、選択したロールに属しているユーザーを一覧表示します。](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**図 6**: GridView を一覧表示、ユーザーに属している、選択したロール ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>選択したロールからユーザーを削除します。

みましょう補強、 `RolesUserList` GridView の列が含まれるように [削除] ボタンをクリックします。 特定のユーザーの「削除」 をクリックしては、そのロールから削除されます。

GridView に削除ボタン フィールドを追加することで開始します。 このフィールドを左側の最もフィールドとして表示され、変更を行うその`DeleteText`プロパティを"Delete"(既定値) から「削除」にします。


[![追加します](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**図 7**: GridView に、「削除」ボタンを追加 ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image21.png))


[削除] ボタンをクリックするとポストバックに陥りますおよび GridView の`RowDeleting`イベントが発生します。 このイベントのイベント ハンドラーを作成し、選択したロールからユーザーを削除するコードを記述する必要があります。 イベント ハンドラーを作成し、次のコードを追加します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

選択したロール名を決定することにより、コードを開始します。 次にプログラムで参照、`UserNameLabel`が [削除] ボタンがクリックされたを削除するユーザーのユーザー名を判断するための行からコントロール。 呼び出しを使用して、ロールからユーザーを削除、`Roles.RemoveUserFromRole`メソッドです。 `RolesUserList` GridView は、更新および経由でメッセージが表示されます、`ActionStatus`ラベル コントロール。

> [!NOTE]
> 「削除」ボタンには、あらゆる種類のロールからユーザーを削除する前に、ユーザーから送信される確認は不要です。 すればを招待するユーザーの確認のいくつかのレベルを追加します。 クライアント側の確認 ダイアログ ボックスで、操作を確認する最も簡単な方法の 1 つです。 この方法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-vb.aspx)です。


図 8 は、ユーザー Tito が管理者グループから削除された後に、ページを示します。


[![残念ながら、Tito が不要になったスーパーバイザーです。](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**図 8**: 残念ながら、Tito が不要になったスーパーバイザー ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>選択したロールに新しいユーザーを追加します。

場合、選択したロールからユーザーを削除すると共に、このページに訪問者のユーザーを選択したロールに追加することも場合があります。 選択したロールにユーザーを追加するための最適なインターフェイスは、ユーザー アカウントが予想される数に依存します。 Web サイトは、わずか 1 ダースのユーザー アカウントを格納する以下の場合、DropDownList をここで使用する可能性があります。 何千ものユーザー アカウントである可能性がありますはする場合、検索、特定のアカウントのアカウントを複数のページまたはその他の何らかの方法でユーザー アカウントをフィルター処理するビジターを許可するユーザー インターフェイスが含まれます。

このページのユーザー アカウントの数に関係なく、システムで動作する非常に単純なインターフェイスを利用してみましょう。 つまり、選択したロールに追加するユーザーのユーザー名を入力するビジターのメッセージを表示、テキスト ボックスを使用します。 その名前を持つユーザーが存在しない、または、ユーザーが既にロールのメンバーである場合にメッセージを表示します`ActionStatus`ラベル。 ユーザーが存在し、ロールのメンバーではない場合は、ロールに追加し、グリッドを更新おします。

テキスト ボックスと GridView の下にあるボタンを追加します。 テキスト ボックスの設定`ID`に`UserNameToAddToRole`ボタンの設定と`ID`と`Text`プロパティ`AddUserToRoleButton`と「ユーザー ロールを追加」、それぞれします。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

次に、作成、`Click`のイベント ハンドラー、`AddUserToRoleButton`し、次のコードを追加します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

内のコードの大部分、`Click`イベント ハンドラーは、さまざまな検証チェックを実行します。 訪問者が内のユーザー名を提供することを確認、 `UserNameToAddToRole`  ボックスに、ユーザーは、システムに存在して、選択したロールに既に属しているしないことです。 適切なメッセージが表示される場合、これらのいずれかの確認が失敗した、`ActionStatus`し、イベント ハンドラーが終了しました。 ユーザーが経由でロールを追加、すべてのチェックに合格した場合、`Roles.AddUserToRole`メソッドです。 次に、テキスト ボックスの`Text`プロパティがオフ、GridView が更新されると、および`ActionStatus`ラベルには、指定されたユーザーが選択した役割を正常に追加されたことを示すメッセージが表示されます。

> [!NOTE]
> 指定されたユーザーは既にに属していないこと、選択したロールには、使用して、 [ `Roles.IsUserInRole(userName, roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)を示すブール値を返すことをするかどうか*ユーザー名*のメンバーである*roleName*です。 内で再度このメソッドを使用しては、 <a id="_msoanchor_2"> </a>[次のチュートリアル](role-based-authorization-vb.md)ロールベースの承認を見てとき。


ブラウザーでページを参照してくださいからスーパーバイザー ロールをクリックし、 `RoleList` DropDownList です。 無効なユーザー名を入力してください: ユーザーがシステムに存在しないことを示すメッセージが表示されます。


[![ロールに存在しないユーザーを追加することはできません。](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**図 9**: 存在しないユーザーをロールに追加することはできません ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image27.png))


有効なユーザーを追加してみましょう。 スーパーバイザー ロールに Tito を再度追加してください。


[![Tito は、もう一度スーパーバイザーです!](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**図 10**: Tito はスーパーバイザー、もう一度!  ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>手順 3: クロス更新、「ユーザー"と」ロール"インターフェイス

`UsersAndRoles.aspx`ページには、ユーザーおよびロールを管理するための 2 つの異なるインターフェイスが用意されています。 現時点では、ため、1 つのインターフェイスで行われた変更はすぐに反映されません、他のことは、これら 2 つのインターフェイスは互いに動作します。 たとえば、ページに訪問者からスーパーバイザー ロールを選択すること、 `RoleList` DropDownList で、そのメンバーとしてブルースと Tito の一覧を示します。 次に、訪問者が Tito からを選択、`UserList`管理者と管理者のチェック ボックスがチェックインの DropDownList、`UsersRoleList`リピータします。 訪問者には、リピータからスーパーバイザー ロールがオフにし場合、は、Tito はスーパーバイザー ロールから削除されますが、"ロール"でインターフェイスにこの変更は反映されません。 GridView は、管理者ロールのメンバーとして Tito を引き続き表示されます。

ロールが checked または unchecked から GridView を更新する必要がありますこれを解決する、`UsersRoleList`リピータします。 同様に、ユーザーが削除されるか、"ロール"でインターフェイスからロールに追加されるたびにリピータを更新する必要があります。

"によって"のユーザー インターフェイスにリピータが呼び出すことによって更新される、`CheckRolesForSelectedUser`メソッドです。 "ロール"でインターフェイスを変更できる、 `RolesUserList` GridView の`RowDeleting`イベント ハンドラーと`AddUserToRoleButton`ボタンの`Click`イベント ハンドラー。 したがってを呼び出す必要があります、`CheckRolesForSelectedUser`からこれらの各メソッドのメソッドです。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

同様に、"ロール"でインターフェイスの GridView が呼び出すことによってに更新される、`DisplayUsersBelongingToRole`メソッドと、"によって"のユーザー インターフェイスを通じて変更が、`RoleCheckBox_CheckChanged`イベント ハンドラー。 したがってを呼び出す必要があります、`DisplayUsersBelongingToRole`このイベント ハンドラーからメソッドです。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

これらのコードの軽微な変更により、今すぐインターフェイス"でユーザー"と"ロール"で正しく更新プログラムの間です。 これを確認するには、ブラウザーでページを参照してください Tito およびからスーパーバイザーをクリックし、`UserList`と`RoleList`DropDownLists、それぞれします。 "によって"のユーザー インターフェイスにリピータから Tito の監督者の役割をオフにするとその Tito が自動的に"role"でインターフェイスの GridView から削除することに注意してください。 Tito スーパーバイザー ロールに"ロール"でインターフェイスから自動的に再チェックの追加"によって"のユーザー インターフェイスで [監督者] チェック ボックスです。

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>手順 4:「の役割を指定する」手順を含める CreateUserWizard をカスタマイズします。

<a id="_msoanchor_3"> </a> [*ユーザー アカウントを作成する*](../membership/creating-user-accounts-vb.md)チュートリアル CreateUserWizard Web コントロールを使用して、新しいユーザー アカウントを作成するためのインターフェイスを提供する方法を説明しました。 CreateUserWizard コントロールは、2 つの方法のいずれかで使用できます。

- 訪問者をサイトで、自分のユーザー アカウントを作成するための手段として、
- 管理者は新しいアカウントを作成するための手段として

最初のユース ケースでは、訪問者は、サイトの訪問し、サイトに登録するために、それらの情報を入力する、CreateUserWizard に入力します。 2 番目のケースでは、管理者は、他のユーザーの新しいアカウントを作成します。

ほかの人の管理者アカウントが作成されている、ときに、管理者が新しいユーザー アカウントに属しているロールを指定できるようにすると役立つ場合があります。 <a id="_msoanchor_4"> </a> [*格納**追加のユーザー情報*](../membership/storing-additional-user-information-vb.md)チュートリアル、CreateUserWizard を追加してカスタマイズする方法を説明しました`WizardSteps`. 新しいユーザーのロールを指定するために、CreateUserWizard に、追加の手順を追加する方法を見てみましょう。

開く、`CreateUserWizardWithRoles.aspx`ページおよびという名前の CreateUserWizard コントロールを追加`RegisterUserWithRoles`です。 コントロールの設定`ContinueDestinationPageUrl`プロパティを"~/Default.aspx"です。 この考え方がいる管理者を使用するこの CreateUserWizard コントロールを新しいユーザー アカウントを作成するための設定コントロールの`LoginCreatedUser`プロパティを False にします。 これは、`LoginCreatedUser`プロパティ訪問者が自動的にだけが作成したユーザーとしてログオンし、既定値は True かどうかを指定します。 お False に設定管理者が新しいアカウントを作成するには、彼の自分自身のアカウントでサインインしてください。

次に、選択、"追加/削除`WizardSteps`..." CreateUserWizard のスマート タグからオプションを追加、新しい`WizardStep`、設定、`ID`に`SpecifyRolesStep`です。 移動、 `SpecifyRolesStep WizardStep` 「記号に、新しいアカウント」手順の後に、「完了」手順の前になるようにします。 設定、`WizardStep`の`Title`プロパティを"Roles"を指定して、その`StepType`プロパティを`Step`、およびその`AllowReturn`プロパティを False にします。


[![追加します](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**図 11**: ロールを追加する"を指定する" `WizardStep` 、CreateUserWizard に ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image33.png))


この変更後に CreateUserWizard の宣言型マークアップは、次のようになります。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

「を指定のロール」の`WizardStep`、CheckBoxList という名前の追加`RoleList.`この CheckBoxList に利用可能な役割は一覧表示、新しく作成されたユーザーが所属するロールを確認する人、ページにアクセスを有効にします。

2 つのコーディング作業が残さお: おを設定する必要があります最初、`RoleList`システムの役割を持つ CheckBoxList 第二に、ユーザーが指定「ロール」手順「完了」手順に移動すると、選択した役割に、作成したユーザーを追加する必要があります。 最初のタスクを行うには、`Page_Load`イベント ハンドラー。 次のコードの参照でプログラムによって、`RoleList`を最初にチェック ボックス ページにアクセスしてし、をシステムの役割をバインドします。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

上記のコードは、使い慣れたはずです。 <a id="_msoanchor_5"> </a> [*格納**追加のユーザー情報*](../membership/storing-additional-user-information-vb.md) 2 つを使用したチュートリアル`FindControl`Web コントロールを参照するステートメントカスタム内`WizardStep`です。 および CheckBoxList にロールをバインドするコードは、このチュートリアルで先ほどから取得されました。

2 番目のプログラミング タスクを実行するためには、「ロールの指定」手順が完了したときに知っている必要があります。 CreateUserWizard が取り消し、`ActiveStepChanged`訪問者が 1 つのステップから別に移動するたびに起動するイベントです。 ここで、ユーザーが「完了」手順; に達したかどうかを判断できます。場合は、選択したロールにユーザーを追加する必要があります。

イベント ハンドラーを作成、`ActiveStepChanged`イベントし、次のコードを追加します。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

イベント ハンドラーがの項目を列挙する場合は、ユーザーは、「完了」手順と同様に達しました、 `RoleList` CheckBoxList とだけで作成されたユーザーが選択した役割に割り当てられます。

ブラウザーからこのページを参照してください。 CreateUserWizard の最初の手順は、標準の「記号に、新しいアカウント」の手順は、新しいユーザーのユーザー名、パスワード、電子メール、およびその他の重要な情報の入力を求めるです。 Wanda をという名前の新しいユーザーを作成する情報を入力します。


[![Wanda をという名前の新しいユーザーを作成します。](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**図 12**: 新しいユーザーという Wanda の作成 ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image36.png))


"Create User"ボタンをクリックします。 CreateUserWizard を内部的に呼び出して、`Membership.CreateUser`方法、新しいユーザー アカウントとし、進行状況に応じて次の手順に作成する「を指定するロールです」。 ここでは、システムの役割が一覧表示されます。 監督者のチェック ボックスを確認し、[次へ] をクリックします。


[![Wanda スーパーバイザー ロールのメンバーにします。](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**図 13**: Wanda スーパーバイザー ロールのメンバーにする ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image39.png))


ポストバックと更新プログラム [次へ] をクリックすると、 `ActiveStep` 「完了」手順にします。 `ActiveStepChanged` 、イベント ハンドラー、最近作成されたユーザー アカウントは、管理者ロールに割り当てられます。 これを確認するに戻り、`UsersAndRoles.aspx`ページからスーパーバイザーをクリックし、 `RoleList` DropDownList です。 図 14 に示す、監督者が可能になります 3 人のユーザーの: ブルース、Tito、および Wanda です。


[![ブルース、Tito、および Wanda は、すべての管理者](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**図 14**: ブルース、Tito、および Wanda は、すべての監督者 ([フルサイズのイメージを表示するをクリックして](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>まとめ

ロール フレームワークには、特定のユーザー ロールと、指定されたロールに属しているユーザーを決定するためのメソッドに関する情報を取得するための方法が用意されています。 さらに、いくつかの追加および 1 つまたは複数の役割を 1 つまたは複数のユーザーを削除するメソッドがあります。 このチュートリアルでに重点を置きましたこれらのメソッドを 2 つのみ:`AddUserToRole`と`RemoveUserFromRole`です。 1 つのロールに複数のユーザーを追加して、1 人のユーザーに複数のロールを割り当てるように設計する追加のバリアントがあります。

このチュートリアルには、含める CreateUserWizard コントロールの拡張を見ても含まれている、`WizardStep`を新しく作成したユーザーのロールを指定します。 このような手順、管理者が新しいユーザーのユーザー アカウントを作成するプロセスを効率化できる場合があります。

この時点で作成し、ロールを削除する方法および追加し、ロールからユーザーを削除する方法をおきました。 おまだロールベースの承認を適用することを確認します。 <a id="_msoanchor_6"> </a>[次のチュートリアル](role-based-authorization-vb.md)を見てみましょうロール別のロールごとに URL 承認規則を定義すると、現在ログインしているユーザーのロールに基づいて、ページ レベルの機能を制限する方法です。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET Web サイト管理ツールの概要](https://msdn.microsoft.com/library/ms228053.aspx)
- [ASP を検査中です。NET のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Teresa マーフィーしました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-and-managing-roles-vb.md)
> [次へ](role-based-authorization-vb.md)
