---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: ロールの割り当てをユーザー (c#) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを作成します。 最初のページには、何を表示する機能が含まれます.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: ada08e48da2f7b1513e1347e18fb7944c66d5a75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836853"
---
<a name="assigning-roles-to-users-c"></a>(C#) のユーザーにロールの割り当てください。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを作成します。 最初のページがどのようなユーザーが特定のロールに属しているかを確認する機能が含まれますが属する特定のユーザー ロールと、割り当てまたは特定のロールから、特定のユーザーを削除する機能。 2 番目のページではの強化点 CreateUserWizard コントロールを新しく作成したユーザーが属しているロールを指定するステップが含まれるようにします。 これは、管理者が新しいユーザー アカウントを作成することがあるシナリオで役立ちます。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a>[前のチュートリアル](creating-and-managing-roles-cs.md)ロール フレームワークを調べて、 `SqlRoleProvider`; を使用する方法を説明しました、`Roles`クラスを作成、取得、およびロールを削除します。 作成し、ロールの削除、割り当てまたはロールからユーザーを削除することができる必要があります。 残念ながら、ASP.NET では、どのようなユーザー ロールに属しているかを管理するためのすべての Web コントロールが付属していません。 代わりに、これらの関連付けを管理する独自の ASP.NET ページを作成する必要があります。 良い知らせは、追加して、ロールにユーザーを削除することは非常に簡単です。 `Roles`クラスには、いくつかの 1 つまたは複数のロールを 1 つまたは複数のユーザーを追加するためのメソッドが含まれています。

このチュートリアルでは、どのようなユーザー ロールに属しているを管理するために 2 つの ASP.NET ページを作成します。 最初のページがどのようなユーザーが特定のロールに属しているかを確認する機能が含まれますが属する特定のユーザー ロールと、割り当てまたは特定のロールから、特定のユーザーを削除する機能。 2 番目のページではの強化点 CreateUserWizard コントロールを新しく作成したユーザーが属しているロールを指定するステップが含まれるようにします。 これは、管理者が新しいユーザー アカウントを作成することがあるシナリオで役立ちます。

それでは、始めましょう!

## <a name="listing-what-users-belong-to-what-roles"></a>どのようなロールに属しているユーザーの一覧を表示します。

このチュートリアルでは、最初の注文のビジネスでは、元のユーザーをロールに割り当てることができます、web ページを作成します。 前に、ユーザー ロールを割り当てる方法を自分たちを考慮しましたみましょう前半でどのようなユーザー ロールに属しているかを特定する方法。 この情報を表示する 2 つの方法: ロール"によって"または"."のユーザーによって ロールを選択し、表示をすべての ("role"で表示)、ロールに属しているユーザーに訪問者によりまたはユーザーを選択し、表示し、そのユーザー ("user"で表示) に割り当てられているロールにユーザー入力をできます。

"ロール"でビューは、訪問者が、特定のロールに属しているユーザーのセットを知りたいとした場所の状況で役に立ちます"ユーザー"によってビューは、訪問者は、特定のユーザー ロールを把握する必要がある場合に最適です。 みましょうロール"によって"と"でユーザー"の両方を含める、ページがあるインターフェイスです。

最初に、"、"ユーザー インターフェイスを作成します。 このインターフェイスは、ドロップダウン リストとチェック ボックスの一覧で構成されます。 システムのユーザーのセットで、ドロップダウン リストが入力されます。ロールのチェック ボックスが列挙されます。 ドロップダウン リストからユーザーを選択すると、ユーザーが所属するこれらのロールを確認します。 ユーザーがページにアクセスできますし、確認するかを追加または削除、選択したユーザー、対応するロールに、チェック ボックスをオフにします。

> [!NOTE]
> ドロップダウン リストを使用してユーザー アカウントでない web サイトの最適な選択肢が何百ものユーザー アカウントがある可能性があります。 ドロップダウン リストは、ユーザーが比較的短い形式の一覧からオプションの 1 つの項目を選択できるように設計されています。 すばやく手に負えなくリスト項目の数が増える。 可能性のある多数のユーザー アカウントがある web サイトを構築する場合たい場合があります、代替のユーザー インターフェイスの使用を検討してページング可能な GridView またはフィルター可能なインターフェイスを一覧表示する、文字を選択するビジターを求めるなどしのみこれらのユーザーが選択されている文字で始まるがユーザー名を示します。


## <a name="step-1-building-the-by-user-user-interface"></a>手順 1:"User"でユーザー インターフェイスを構築します。

開く、`UsersAndRoles.aspx`ページ。 ページの上部にある、という名前のラベルの Web コントロールを追加`ActionStatus`クリアとその`Text`プロパティ。 私たちはのようなメッセージを表示する、実行される操作に関するフィードバックを提供するこのラベルを使用して、「ユーザー Tito が追加された、管理者ロールに」または「ユーザー Jisun はスーパーバイザー ロールから削除されましたが」。 これらを行うためにメッセージが目立つ設定で、ラベルの`CssClass`プロパティを"Important"にします。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

次の CSS クラス定義を次に、追加、`Styles.css`スタイル シート。

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

この CSS 定義では、ブラウザーで、大規模な赤いフォントを使用してラベルを表示するように指示します。 図 1 は、Visual Studio デザイナーでは、この効果を示します。


[![ラベルの CssClass プロパティは、大規模な赤いフォントで結果します。](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**図 1**:、ラベルの`CssClass`大規模、赤いフォントでプロパティの結果 ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image3.png))。


次に、設定ページに、DropDownList を追加、`ID`プロパティを`UserList`、設定とその`AutoPostBack`プロパティを True にします。 システム内のすべてのユーザーの一覧を表示するのにこの DropDownList を使用します。 この DropDownList MembershipUser オブジェクトのコレクションにバインドされます。 DropDownList MembershipUser オブジェクトの UserName プロパティの表示 (および、リスト項目の値として使用) するため、設定の DropDownList の`DataTextField`と`DataValueField`プロパティが"UserName"にします。

という名前の Repeater を追加、DropDownList の下にある`UsersRoleList`します。 この Repeater として一覧表示すべての役割、システムで、一連のチェック ボックスをオンします。 Repeater の定義`ItemTemplate`次の宣言型マークアップを使用します。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

`ItemTemplate`マークアップには、という名前の 1 つのチェック ボックスを Web コントロールが含まれています。`RoleCheckBox`します。 チェック ボックスをオンの`AutoPostBack`プロパティが True に設定し、`Text`プロパティにバインドする`Container.DataItem`します。 理由のデータ バインド構文は単純に`Container.DataItem`はロール フレームワークは、文字列の配列としてロール名の一覧を返しますがこの文字列の配列は、Repeater にバインドするためです。 データ Web コントロールにバインドされている配列の内容を表示する次の構文を使用する理由の詳細な説明では、このチュートリアルの範囲外です。 この問題の詳細についてを参照してください[データ Web コントロールにスカラーの配列をバインド](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)します。

この時点で、"ユーザー"によってインターフェイスの宣言型マークアップは、次のようになります。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

これで、DropDownList にユーザー アカウントのセットと Repeater をロールのセットをバインドするコードを記述する準備が整いました。 ページの分離コード クラスには、という名前のメソッドを追加`BindUsersToUserList`という 2 つと`BindRolesList`、次のコードを使用します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

`BindUsersToUserList`メソッドはによってシステム内のすべてのユーザー アカウントを取得、 [ `Membership.GetAllUsers`メソッド](https://msdn.microsoft.com/library/dy8swhya.aspx)します。 返されます、 [ `MembershipUserCollection`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)のコレクションである[`MembershipUser`インスタンス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)します。 このコレクションにバインドし、 `UserList` DropDownList します。 `MembershipUser`インスタンス構成のコレクションのようなさまざまなプロパティに含まれるその`UserName`、 `Email`、 `CreationDate`、および`IsOnline`します。 DropDownList の値を表示するように指示するために、`UserName`プロパティ、いることを確認、 `UserList` DropDownList の`DataTextField`と`DataValueField`プロパティは、"UserName"に設定されています。

> [!NOTE]
> `Membership.GetAllUsers`メソッドに 2 つのオーバー ロードがあります。 もう 1 つの入力パラメーターを受け取らずし、ユーザーのすべてを返しますを受け取り、ページのインデックスと、ページ サイズの整数値で指定されたユーザーのサブセットのみを返します。 大量のページング可能なユーザー インターフェイス要素に表示されているユーザー アカウントがある場合に、それらのすべてではなくユーザー アカウントの正確なサブセットだけを返すため 2 番目のオーバー ロードはより効率的にユーザーを使用してページを使用できます。


`BindRolesToList`メソッドを呼び出すことによって開始、`Roles`クラスの[`GetAllRoles`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)システムの役割を格納する文字列配列が返されます。 この文字列の配列は、Repeater にバインドします。

最後に、ページが最初に読み込まれたときに、これら 2 つのメソッドを呼び出す必要があります。 `Page_Load` イベント ハンドラーに次のコードを追加します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

このコードでする少し; ブラウザーを使用してページを参照してください。画面は、図 2 のようになります。 すべてのユーザー アカウントが入力されますドロップダウン リストで、その下に、各ロールは、チェック ボックスとして表示されます。 設定するため、`AutoPostBack`プロパティ DropDownList およびチェック ボックスの true の場合、チェックまたは役割をオフにする、選択したユーザーの変更がポストバックを発生します。 アクションは実行されません、ただし、これらのアクションを処理するコードを記述があるまだあるので。 次の 2 つのセクションでは、これらのタスクに取り組むします。


[![ページは、ユーザーおよびロールが表示されます。](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**図 2**: ページは、ユーザーおよびロールが表示されます ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image6.png))。


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>選択したユーザーが所属するロールのチェック

ページが最初に読み込まれたときに、またはドロップダウン リストから、訪問者が新しいユーザーを選択するたびに更新する必要があります、`UsersRoleList`のチェック ボックス、選択したユーザーがそのロールに属している場合にのみ、特定のロールのチェック ボックスにチェックします。 これを実現するという名前のメソッドを作成`CheckRolesForSelectedUser`を次のコード。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

上記のコードは、選択したユーザーが決定することで開始します。 ロールのクラスを使用して、 [ `GetRolesForUser(userName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)文字列の配列としての役割の指定したユーザーのセットを取得します。 次に、Repeater のアイテムが列挙し、各項目の`RoleCheckBox`チェック ボックスがプログラムで参照されています。 対応するロールに含まれる場合にのみチェック ボックスがオン、`selectedUsersRoles`文字列配列。

> [!NOTE]
> `selectedUserRoles.Contains<string>(...)`構文は、ASP.NET version 2.0 を使用している場合はコンパイルされません。 `Contains<string>`メソッドの一部である、 [LINQ ライブラリ](http://en.wikipedia.org/wiki/Language_Integrated_Query)、これは新しい ASP.NET 3.5 にします。 ASP.NET version 2.0 を使用している場合は、使用、 [ `Array.IndexOf<string>`メソッド](https://msdn.microsoft.com/library/eha9t187.aspx)代わりにします。


`CheckRolesForSelectedUser`メソッドは、2 つのケースで呼び出される必要があります: およびページが最初に読み込まれたときに、 `UserList` DropDownList の選択されたインデックスを変更します。 したがってからこのメソッドを呼び出す、`Page_Load`イベント ハンドラー (呼び出し後に`BindUsersToUserList`と`BindRolesToList`)。 また、イベント ハンドラーを作成の DropDownList の`SelectedIndexChanged`イベントそこからこのメソッドを呼び出すとします。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

このコードでは、ブラウザーを使用してページをテストできます。 ただし、以降、`UsersAndRoles.aspx`ページに現在のロールにユーザーを割り当てる機能がない、ユーザーにはロールはありません。 インターフェイス、単語には、このコードは動作を実行し、後はそのことを確認するためのすぐにロールにユーザーの割り当てを作成またはにレコードを挿入することでユーザーをロールに手動で追加することができます、`aspnet_UsersInRoles`この functi をテストするにはテーブルonality ようになりました。

### <a name="assigning-and-removing-users-from-roles"></a>割り当てとロールからユーザーを削除します。

訪問者がチェックや内のチェック ボックスをオフに、 `UsersRoleList` Repeater を追加または対応するロールから、選択したユーザーを削除する必要があります。 チェック ボックスをオンの`AutoPostBack`プロパティは現在でも、Repeater 内のチェック ボックスが checked または unchecked ポストバックが発生する True に設定します。 つまり、CheckBox のイベント ハンドラーを作成する必要があります`CheckChanged`イベント。 チェック ボックスは、Repeater コントロールであるため、イベント ハンドラーのプラミングを手動で追加する必要があります。 イベント ハンドラーと分離コード クラスを追加して開始、`protected`メソッドでは、次のように。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

すぐにこのイベント ハンドラーのコードを記述する返されます。 まずは最後に、イベントのプラミングを処理します。 Repeater の内でチェック ボックスをオンから`ItemTemplate`、追加`OnCheckedChanged="RoleCheckBox_CheckChanged"`します。 この構文の接続、`RoleCheckBox_CheckChanged`イベント ハンドラーを`RoleCheckBox`の`CheckedChanged`イベント。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

最後のタスクが完了するには、`RoleCheckBox_CheckChanged`イベント ハンドラー。 このチェック ボックスのインスタンスと、"どのような役割が checked または unchecked 経由で"わかりますのでイベントを発生させた CheckBox コントロールを参照することで開始する必要があります。 その`Text`と`Checked`プロパティ。 選択したユーザーのユーザー名と共にこの情報を使用して、私たちを追加またはを介してロールからユーザーを削除、`Roles`クラスの[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)または[`RemoveUserFromRole`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

上記のコードをプログラムでチェック ボックスを利用すると、イベントを発生させたを参照して開始、`sender`入力パラメーター。 チェック ボックスをオンにした場合、指定したロールに、それ以外の場合、ロールから削除された、選択したユーザーが追加されます。 どちらの場合、`ActionStatus`ラベルにだけ実行されるアクションを要約するメッセージが表示されます。

このページで、ブラウザーでテストする時間がかかります。 Tito のユーザーを選択し、Tito を管理者と管理者の両方のロールに追加します。


[![Tito が管理者と管理者ロールに追加されました](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**図 3**: Tito が管理者と管理者ロールに追加されました ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image9.png))。


次に、ドロップダウン リストからユーザー Bruce を選択します。 ポストバックがあるし、Repeater のチェック ボックスが更新を使用して、`CheckRolesForSelectedUser`します。 任意のロールにも Bruce がまだ属していないため、2 つのチェック ボックスはチェックされません。 次に、Bruce を管理者ロールに追加します。


[![Bruce が管理者ロールに追加されました](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**図 4**: Bruce が管理者ロールに追加されました ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image12.png))。


さらの機能を検証する、`CheckRolesForSelectedUser`メソッド、Tito または Bruce 以外のユーザーを選択します。 チェック ボックスが自動的にチェックする方法を確認して、任意のロールに属していないことを示します。 Tito に戻ります。 管理者と管理者の両方のチェック ボックスをチェックする必要があります。

## <a name="step-2-building-the-by-roles-user-interface"></a>手順 2:"ロール"でユーザー インターフェイスの構築

この時点で"によって"のユーザー インターフェイスが完了し、"ロール"でインターフェイスへの取り組みを開始する準備が整いました。 "ロール"でインターフェイスでは、ドロップダウン リストからロールを選択するよう求めるされ、GridView では、そのロールに属しているユーザーのセットが表示されます。

もう 1 つの DropDownList コントロールを追加、`UsersAndRoles.aspx`ページ。 Repeater コントロールの下に 1 つ配置、名前を付けます`RoleList`、設定とその`AutoPostBack`プロパティを True にします。 その下に、GridView を追加し、名前`RolesUserList`します。 この GridView には、選択したロールに属しているユーザーが表示されます。 GridView の設定`AutoGenerateColumns`プロパティを False には、グリッドの TemplateField を追加`Columns`コレクション、およびセットの`HeaderText`プロパティを「ユーザー」にします。 定義 TemplateField の`ItemTemplate`データ バインド式の値を表示するよう`Container.DataItem`で、`Text`という名前のラベルのプロパティ`UserNameLabel`します。

追加し、GridView を構成したら、"role"でインターフェイスの宣言型マークアップは、次のようなになります。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

設定する必要があります、 `RoleList` DropDownList 一連のシステムの役割。 これを行うには、更新、`BindRolesToList`によってメソッドのバインドがあるため、文字列の配列が返される、`Roles.GetAllRoles`メソッドを`RolesList`DropDownList (だけでなく`UsersRoleList`Repeater)。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

最後の 2 行、`BindRolesToList`への役割のセットをバインドするメソッドが追加されて、 `RoleList` DropDownList コントロール。 図 5 は、ブラウザーから、システムの役割を含むドロップダウン リストで表示した場合は、最終結果を示します。


[![ロールは RoleList DropDownList に表示されます。](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**図 5**:、ロールが表示されます、 `RoleList` DropDownList ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image15.png))。


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>選択したロールに属しているユーザーを表示します。

ページが最初に読み込ままたはからの新しいロールを選択すると、 `RoleList` DropDownList、GridView では、そのロールに属しているユーザーの一覧を表示する必要があります。 という名前のメソッドを作成する`DisplayUsersBelongingToRole`次のコードを使用します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

このメソッドの開始から、選択したロールを取得することによって、 `RoleList` DropDownList します。 次を使用して、 [ `Roles.GetUsersInRole(roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)そのロールに属しているユーザーのユーザー名の文字列の配列を取得します。 この配列にバインドし、 `RolesUserList` GridView。

このメソッドは、2 つの状況で呼び出される必要があります。 ページが最初に読み込まれるとき、および、選択したロールで、 `RoleList` DropDownList の変更。 このため、更新、`Page_Load`イベント ハンドラーへの呼び出し後にこのメソッドが呼び出されるように`CheckRolesForSelectedUser`します。 次に、イベント ハンドラーを作成、`RoleList`の`SelectedIndexChanged`イベント、すぎる、そこから、このメソッドを呼び出します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

インプレースでは、次のコードで、 `RolesUserList` GridView は選択したロールに属しているこれらのユーザーを表示する必要があります。 図 6 に示すよう 2 つのメンバー、監督者ロールで構成されます: Bruce と Tito します。


[![GridView には、選択したロールに属しているこれらのユーザーが一覧表示します。](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**図 6**:、GridView を一覧表示、ユーザーに属している、選択したロール ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image18.png))。


### <a name="removing-users-from-the-selected-role"></a>選択したロールからユーザーを削除します。

拡張しましょう、 `RolesUserList` GridView の列が含まれるため、「削除」ボタンします。 特定のユーザーの「削除」ボタンをクリックしては、そのロールから削除されます。

GridView に Delete ボタン フィールドを追加することで開始します。 最もフィールドの左側として表示され、変更は、このフィールドの`DeleteText`プロパティを「削除」(既定値) から"Remove"にします。


[![追加します](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**図 7**: GridView を「削除」ボタンを追加 ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image21.png))。


ポストバックに陥ります「削除」ボタンがクリックされたときに、GridView の`RowDeleting`イベントが発生します。 このイベントのイベント ハンドラーを作成し、選択したロールからユーザーを削除するコードを記述する必要があります。 イベント ハンドラーを作成し、次のコードを追加します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

選択したロールの名前を決定することにより、コードを開始します。 そのプログラムで参照、`UserNameLabel`を削除するユーザーのユーザー名を判断するための「削除」ボタンがクリックしてされた行からのコントロール。 ユーザーがへの呼び出しを使用してロールから削除し、`Roles.RemoveUserFromRole`メソッド。 `RolesUserList` GridView が更新され、経由でメッセージが表示されます、`ActionStatus`コントロールのラベルします。

> [!NOTE]
> 「削除」ボタンには、あらゆる種類のロールからユーザーを削除する前に、ユーザーから送信される確認は不要です。 ユーザーの確認のいくつかのレベルを追加することをお勧めします。 アクションを確認する最も簡単な方法の 1 つは、クライアント側の確認 ダイアログ ボックスからです。 この手法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-cs.aspx)します。


図 8 は、ユーザー Tito が管理者グループから削除された後に、ページを示します。


[![悲しいかな、Tito が不要になった、監督者です。](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**図 8**: 悲しいかな、Tito が不要になった、監督者 ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image24.png))。


### <a name="adding-new-users-to-the-selected-role"></a>選択したロールに新しいユーザーの追加

選択したロールからユーザーを削除、と共にこのページに訪問者の選択したロールにユーザーを追加することも場合があります。 選択したロールにユーザーを追加するための最適なインターフェイスは、ユーザーのアカウントは、予想される数に依存します。 Web サイトは、わずか数十個のユーザー アカウントを格納する以下の場合、ここで DropDownList を使用できます。 何千ものユーザー アカウントである可能性がありますは場合は、アカウント、特定のアカウントの検索 ページまたはその他のなんらかの方法でユーザー アカウントをフィルター処理するビジターを許可するユーザー インターフェイスが含まれます。

このページで、システムでユーザー アカウントの数に関係なく動作する非常に単純なインターフェイスを使用しましょう。 つまり、選択したロールに追加したいので、ユーザーのユーザー名を入力するユーザーの入力を求めるテキスト ボックスを使用します。 その名前を持つユーザーが存在しないか、ユーザーが既にロールのメンバーである場合にメッセージを表示します`ActionStatus`ラベル。 ただし、ユーザーが存在するロールのメンバーでない場合に、ロールに追加し、グリッドを更新します。

GridView の下にあるテキスト ボックスに追加します。 設定、テキスト ボックスの`ID`に`UserNameToAddToRole`ボタンの設定と`ID`と`Text`プロパティ`AddUserToRoleButton`および"ユーザー ロールに追加、それぞれします。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

次に、作成、`Click`のイベント ハンドラー、`AddUserToRoleButton`し、次のコードを追加します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

内のコードの大部分、`Click`イベント ハンドラーは、さまざまな検証チェックを実行します。 訪問者のユーザー名が指定されているようになります、 `UserNameToAddToRole`  ボックスに、システムでは、ユーザーが存在して、選択したロールに既に属していないこと。 適切なメッセージが表示される場合、これらのいずれかの確認が失敗、`ActionStatus`イベント ハンドラーが終了しました。 使用してロールにユーザーを追加、すべてのチェックを渡す場合、`Roles.AddUserToRole`メソッド。 次に、テキスト ボックスの`Text`プロパティが取り除か、GridView が更新されると、および`ActionStatus`ラベルには、指定したユーザーが、選択したロールを正常に追加されたことを示すメッセージが表示されます。

> [!NOTE]
> 指定したユーザーは既にに属していないこと、選択したロールを確認するには使用、 [ `Roles.IsUserInRole(userName, roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)を示すブール値を返すかどうか*userName* のメンバーである*roleName*します。 内でもう一度このメソッドは使用して、 <a id="_msoanchor_2"> </a>[次のチュートリアル](role-based-authorization-cs.md)ロールベースの承認に注目するとします。


ブラウザーでページにアクセスしからスーパーバイザー ロールを選択、 `RoleList` DropDownList します。 無効なユーザー名を入力してください: ユーザーがシステムに存在しないことを説明するメッセージが表示されます。


[![ロールに存在しないユーザーを追加することはできません。](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**図 9**: ロールに存在しないユーザーを追加することはできません ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image27.png))。


有効なユーザーを追加してみましょう。 管理者ロールに Tito を再度追加してください。


[![Tito はスーパーバイザーではもう一度です。](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**図 10**: Tito はスーパーバイザーでもう一度です。  ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image30.png))。


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>手順 3: クロス更新、ユーザー"によって"と"ロール"によってインターフェイス

`UsersAndRoles.aspx`ページには、ユーザーとロールを管理するための 2 つの異なるインターフェイスが用意されています。 現時点では、これら 2 つのインターフェイスは、できるようになりますが、1 つのインターフェイスに行われた変更はすぐに反映されません、他の考えられるはそれぞれ独立して機能します。 たとえば、ページに訪問者の監督者ロールを選択すること、 `RoleList` DropDownList で、そのメンバーとして、Bruce と Tito を一覧表示されます。 次に、訪問者が Tito からを選択、 `UserList` DropDownList で、管理者と管理者のチェック ボックスをチェックイン、 `UsersRoleList` Repeater します。 訪問者には、Repeater からスーパーバイザー ロールがオフにし、Tito は、監督者のロールから削除されますが、"ロール"でインターフェイスにこの変更は反映されません。 GridView は、管理者ロールのメンバーとして Tito を引き続き表示されます。

ロールとは、checked または unchecked のたびに、GridView を更新する必要があります。 これを解決する、 `UsersRoleList` Repeater します。 同様に、ユーザーが削除されるか、"role"でインターフェイスからロールに追加されるたびに、Repeater を更新する必要があります。

"ユーザー"がインターフェイスで Repeater が呼び出すことによって更新される、`CheckRolesForSelectedUser`メソッド。 "Role"でインターフェイスを変更できる、 `RolesUserList` GridView の`RowDeleting`イベント ハンドラーと`AddUserToRoleButton`ボタンの`Click`イベント ハンドラー。 そのためを呼び出す必要があります、`CheckRolesForSelectedUser`からこれらの各メソッドのメソッド。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

"ロール"によってインターフェイスで GridView を呼び出すことによって更新同様に、`DisplayUsersBelongingToRole`メソッドと、"ユーザー"によってインターフェイスを通じて変更が、`RoleCheckBox_CheckChanged`イベント ハンドラー。 そのためを呼び出す必要があります、`DisplayUsersBelongingToRole`このイベント ハンドラーからメソッド。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

これらのコードの軽微な変更により、今すぐインターフェイス"でユーザー"と"ロール"で正しく更新プログラムの間。 これを確認するブラウザーを使用してページを参照してくださいし、Tito とから管理者を選択して、`UserList`と`RoleList`Dropdownlist、それぞれします。 "ユーザー"がインターフェイスで Repeater から Tito の監督者の役割をオフにするとその Tito が自動的に"ロール"でインターフェイスの GridView から削除することに注意してください。 Tito スーパーバイザー ロールに"ロール"によってインターフェイスから自動的に再チェックの追加"によって"のユーザー インターフェイスでスーパーバイザー チェック ボックスをオンします。

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>手順 4: ロールを指定する」の手順を含める CreateUserWizard のカスタマイズ

<a id="_msoanchor_3"> </a> [*ユーザー アカウントを作成する*](../membership/creating-user-accounts-cs.md)チュートリアル CreateUserWizard Web コントロールを使用して、新しいユーザー アカウントを作成するインターフェイスを提供する方法を説明しました。 CreateUserWizard コントロールは、2 つの方法のいずれかで使用できます。

- 訪問者、サイトで自分のユーザー アカウントを作成するための手段として、
- 管理者は新しいアカウントを作成するための手段として

最初のユース ケースでは、訪問者はに関しては、サイトし、サイトに登録するために情報の入力、CreateUserWizard に入力します。 2 番目のケースでは、管理者は、別のユーザーに新しいアカウントを作成します。

ほかの人の管理者アカウントの作成には、新しいユーザー アカウントに属しているロールを指定できるように役立つ場合があります。 <a id="_msoanchor_4"> </a> [*格納**追加のユーザー情報*](../membership/storing-additional-user-information-cs.md)チュートリアルを追加することで、CreateUserWizard をカスタマイズする方法を説明しました`WizardSteps`. CreateUserWizard に新しいユーザーのロールを指定するには、追加のステップを追加する方法を見てみましょう。

開く、`CreateUserWizardWithRoles.aspx`ページし、という名前の CreateUserWizard コントロールを追加`RegisterUserWithRoles`します。 コントロールの設定`ContinueDestinationPageUrl`プロパティを"~/Default.aspx"。 この考え方は、新しいユーザー アカウントを作成するこの CreateUserWizard コントロールが、管理者を使用する、ことであるためにの設定、コントロールの`LoginCreatedUser`プロパティを False にします。 これは、`LoginCreatedUser`プロパティは、訪問者が自動的に作成したばかりのユーザーとしてログオンし、既定値は True かどうかを指定します。 設定しました False ため、管理者は、新しいアカウントを作成するときに、彼自身のアカウントでサインインを維持するように指定します。

次に、選択、"追加/削除`WizardSteps`..."CreateUserWizard のスマート タグからオプションを選択し、新しい追加`WizardStep`設定、その`ID`に`SpecifyRolesStep`します。 移動、 `SpecifyRolesStep WizardStep` 「記号を新しいアカウントの」手順の後が「完了」手順の前になるようにします。 設定、`WizardStep`の`Title`プロパティを"Roles"を指定、その`StepType`プロパティを`Step`、およびその`AllowReturn`プロパティを False にします。


[![追加します](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**図 11**:「を指定のロール」を追加`WizardStep`CreateUserWizard に ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image33.png))。


この変更後、次のよう CreateUserWizard の宣言型マークアップになります。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

指定のロール"で`WizardStep`、CheckBoxList という名前の追加`RoleList`します。 この CheckBoxList を新しく作成したユーザーが属しているロールを確認する人、ページにアクセスを有効にすると、使用可能なロールが表示されます。

2 つのコーディング作業が残ること: 最初に設定する必要があります、 `RoleList` CheckBoxList システムの役割を持つ第 2 に、ユーザーが [ロールの指定] に、"Complete"の手順に移動すると、選択したロールに、作成したユーザーを追加する必要があります。 最初のタスクのためには、`Page_Load`イベント ハンドラー。 次のコード プログラムで参照、`RoleList`最初のチェック ボックスのページを参照してくださいし、そこにシステムの役割をバインドします。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

上記のコードは、使い慣れたになります。 <a id="_msoanchor_5"> </a> [*格納**追加のユーザー情報*](../membership/storing-additional-user-information-cs.md) 2 つを使用したチュートリアル`FindControl`Web コントロールを参照するステートメントカスタム内`WizardStep`します。 このチュートリアルで前に、CheckBoxList にロールをバインドするコードがから取得されました。

2 番目のプログラミング タスクを実行するためには、「指定のロール」手順が完了したときを把握する必要があります。 CreateUserWizard はことを思い出してください、`ActiveStepChanged`イベントで、訪問者が 1 つの手順から別に移動するたびに発生します。 ここで、ユーザーが"Complete"ステップに達したかどうかを判断できます。そうである場合は、選択したロールにユーザーを追加する必要があります。

イベント ハンドラーを作成、`ActiveStepChanged`イベントと、次のコードを追加します。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

イベント ハンドラーがの項目を列挙するユーザーが「完了」手順に達すると同様、 `RoleList` CheckBoxList と、作成したばかりのユーザーは、選択したロールに割り当てられています。

ブラウザーからこのページを参照してください。 CreateUserWizard の最初の手順は、標準の「記号を新しいアカウントの」の手順は、新しいユーザーのユーザー名、パスワード、電子メール、およびその他の重要な情報の入力を求めるです。 Wanda をという名前の新しいユーザーを作成する情報を入力します。


[![Wanda をという名前の新しいユーザーを作成します。](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**図 12**: 新しいユーザーという Wanda の作成 ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image36.png))。


「ユーザーの作成」ボタンをクリックします。 CreateUserWizard は内部的に呼び出し、`Membership.CreateUser`方法、新しいユーザー アカウントと、進行状況に応じて次の手順に作成する「を指定するロール」。 ここでは、システムの役割が一覧表示されます。 監督者のチェック ボックスを確認し、[次へ] をクリックします。


[![Wanda スーパーバイザー ロールのメンバーにします。](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**図 13**: Wanda スーパーバイザー ロールのメンバーにする ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image39.png))。


[次へ] をクリックすると、ポストバックと更新プログラム、 `ActiveStep` "Complete"の手順にします。 `ActiveStepChanged`イベント ハンドラーでは、最近作成されたユーザー アカウントが管理者ロールに割り当てられています。 これを確認するに戻り、`UsersAndRoles.aspx`ページし、上司からの選択、 `RoleList` DropDownList します。 スーパーバイザーが 3 人のユーザーのに対して可能図 14 に示す: Bruce、Tito、および Wanda します。


[![Bruce、Tito、および Wanda は、すべての管理者](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**図 14**: Bruce、Tito、Wanda はすべてスーパーバイザー ([フルサイズの画像を表示する をクリックします](assigning-roles-to-users-cs/_static/image42.png))。


## <a name="summary"></a>まとめ

ロールのフレームワークでは、特定のユーザー ロールと指定したロールに属しているユーザーを決定するためのメソッドに関する情報を取得するためのメソッドを提供します。 さらに、いくつかのメソッドの追加や削除を 1 つまたは複数のロールに 1 つまたは複数のユーザーがあります。 このチュートリアルではこれらのメソッドの 2 つだけに重点を置きました。`AddUserToRole`と`RemoveUserFromRole`します。 これには 1 つのロールに複数のユーザーを追加して、1 人のユーザーに複数のロールを割り当てるように設計追加のバリエーションがあります。

このチュートリアルには、拡張に含める CreateUserWizard コントロールを見ても含まれている、`WizardStep`を新しく作成したユーザーのロールを指定します。 このような手順は、管理者は新しいユーザーのユーザー アカウントを作成するプロセスを効率化できます。

この時点で作成し、ロールを削除する方法と追加およびロールからユーザーを削除する方法を行ってきた。 まだロールベースの承認を適用することを確認します。 <a id="_msoanchor_6"> </a>[次のチュートリアル](role-based-authorization-cs.md)注目するは、ロールを役割ごとに URL 承認規則を定義するのにも、現在ログインしているユーザーのロールに基づいて、ページ レベルの機能を制限する方法。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET Web サイト管理ツールの概要](https://msdn.microsoft.com/library/ms228053.aspx)
- [ASP を調べています。NET のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](creating-and-managing-roles-cs.md)
> [次へ](role-based-authorization-cs.md)
