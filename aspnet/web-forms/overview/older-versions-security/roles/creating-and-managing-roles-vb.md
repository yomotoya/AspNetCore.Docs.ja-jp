---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: "作成して、(VB) の役割の管理 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成し、ロールの削除を構築します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: a67065a33bb38388ab941c785b7dcc268645ceca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-managing-roles-vb"></a>ロール (VB) 作成および管理
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成し、ロールの削除を構築します。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル URL 承認を使用して、ページのセットから特定のユーザーを制限するには検索し、宣言的に調査し、アクセスしたユーザーに基づいて ASP.NET ページの機能を調整するためのプログラムによるテクニック ページへのアクセスやユーザー単位のユーザーごとの機能にアクセス権を付与するには、ただし、なることのシナリオでのメンテナンス大変な作業が多くのユーザー アカウントがある場合、またはユーザーの特権を頻繁に変更します。 ユーザーを取得したり、特定のタスクを実行するための承認を失ったり、いつでも、管理者は、適切な URL 承認規則、宣言型のマークアップとコードを更新する必要があります。

通常では、ユーザーをグループに分類または*ロール*とそのアクセス許可をロール別のロールごとに適用するためにします。 たとえば、ほとんどの web アプリケーションでは、特定の一連のページまたは管理ユーザーに対してのみに予約されているタスクがあります。 学習技法を使用して、*ユーザー ベースの承認*チュートリアルでは、私たちは、適切な URL 承認規則、宣言型のマークアップと追加管理タスクを実行する指定されたユーザー アカウントを許可するためのコード。 新しい管理者を追加した場合、または既存の管理者が失効して自分の管理権限を持っている必要な場合は、返すし、構成ファイルと web ページを更新する必要があります。 ただし、ロールで管理者と呼ばれるロールを作成し、管理者ロールにこれらの信頼されたユーザーを割り当てるでしたおです。 次に、おは、適切な URL 承認規則、宣言型のマークアップとさまざまな管理タスクを実行する管理者ロールを許可するコードを追加します。 このインフラストラクチャを導入するには、サイトに新しい管理者を追加または既存のテーブルを削除するなど、または管理者ロールからユーザーを削除するだけでです。 構成、宣言型マークアップ、またはコードの変更は必要ありません。

ASP.NET には、ロールの定義とユーザー アカウントに関連付けるロール フレームワークが用意されています。 ロールを作成してロールを削除、フレームワークでは、または削除するユーザー ロールから特定のロールに属しているし、ユーザーが特定のロールに属しているかどうかを通知するユーザーのセットを決定するユーザーを追加します。 ロール フレームワークを構成すると、お URL 承認規則経由での役割の役割ごとにページへのアクセスを制限し、表示と非表示追加情報や、現在ログオンしているユーザーの役割に基づいた ページで機能します。

このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成し、ロールの削除を構築します。 <a id="_msoanchor_2"> </a> [*ロールをユーザーに割り当てる*](assigning-roles-to-users-vb.md)チュートリアルを追加およびロールからユーザーを削除する方法を紹介します。 および、 <a id="_msoanchor_3"> </a> [*ロールベース承認*](role-based-authorization-vb.md)チュートリアル ページ機能によってを調整する方法と共にの役割の役割ごとにページへのアクセスを制限する方法が表示されますアクセスしたユーザーのロール。 開始しましょう!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新規の ASP.NET ページの追加

このチュートリアルで次の 2 つお検査さまざまなロール関連の機能と機能します。 一連のトピックでは、これらのチュートリアル全体で検証を実装する ASP.NET ページが必要ですが。 これらのページを作成し、サイト マップを更新してみましょう。

という名前のプロジェクトに新しいフォルダーを作成して開始`Roles`です。 次に、追加する 4 つの新しい ASP.NET ページ、`Roles`フォルダーで、各ページのリンク、`Site.master`マスター ページ。 ページの名前を付けます。

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

この時点で、プロジェクトのソリューション エクスプ ローラーはのスクリーン ショット、図 1 に示すようになります。


[![[ロール] フォルダーに 4 つの新しいページが追加されました](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**図 1**: 次の 4 つ新しいページに追加されている、`Roles`フォルダー ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image3.png))


各ページ、この時点では 2 つのコンテンツ コントロールは、マスター ページの contentplaceholders にごとに 1 つ:`MainContent`と`LoginContent`です。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

注意してください、 `LoginContent` ContentPlaceHolder の既定のマークアップにログオンするか、いったんログオフして、ユーザーが認証されるかどうかに応じて、サイトからへのリンクが表示されます。 存在、`Content2`コンテンツ、ASP.NET ページ内のコントロールがただし、マスター ページの既定のマークアップをオーバーライドします。 説明したよう<a id="_msoanchor_4"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)チュートリアルでは、既定のマークアップをオーバーライドする、場所たくないを表示するログインに関連するページに便利です左の列にオプションです。

これら 4 つのページ、ただし、表示することのマスター ページの既定のマークアップ、 `LoginContent` ContentPlaceHolder です。 このため、削除の宣言型マークアップ、`Content2`コンテンツ コントロールです。 その後、1 つだけのコンテンツ コントロールを含めるそれぞれ 4 つのページのマークアップの必要があります。

最後に、サイト マップを更新してみましょう (`Web.sitemap`) にこれらの新しい web ページが含まれます。 後に次の XML を追加、`<siteMapNode>`メンバーシップ チュートリアルについては、追加しました。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

更新されたサイト マップには、ブラウザーを使用してサイトを参照してください。 図 2 では、左側のナビゲーションには、ロール チュートリアルについては、アイテムが含まれます。


[![[ロール] フォルダーに 4 つの新しいページが追加されました](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**図 2**: 次の 4 つ新しいページに追加されている、`Roles`フォルダー ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>手順 2: を指定して、ロール Framework プロバイダーの構成

メンバーシップ フレームワークと同様にロール フレームワークはプロバイダー モデルの上に構築されます。 説明したように、 <a id="_msoanchor_5"> </a> [*セキュリティの基礎と ASP.NET サポート*](../introduction/security-basics-and-asp-net-support-vb.md)チュートリアルでは、次の 3 つの組み込みのロール プロバイダーが付属して .NET Framework: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.authorizationstoreroleprovider.aspx)、 [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.windowstokenroleprovider.aspx)、および[ `SqlRoleProvider`](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx)です。 このチュートリアルのシリーズについて重点的に、 `SqlRoleProvider`、ロール ストアとして、Microsoft SQL Server データベースを使用します。

背後のロール フレームワークと`SqlRoleProvider`メンバーシップ framework と同じように動作し、`SqlMembershipProvider`です。 .NET Framework には、`Roles`ロール フレームワーク API として機能するクラス。 `Roles`などのメソッドがクラスに共有`CreateRole`、 `DeleteRole`、 `GetAllRoles`、 `AddUserToRole`、`IsUserInRole`などのようにします。 これらのメソッドのいずれかが呼び出されると、`Roles`クラスが構成されているプロバイダーへの呼び出しを代行します。 `SqlRoleProvider`役割に固有のテーブルで機能 (`aspnet_Roles`と`aspnet_UsersInRoles`) で応答します。

使用するために、`SqlRoleProvider`プロバイダー アプリケーションでは、ストアとして使用するデータベースの内容を指定する必要があります。 `SqlRoleProvider`特定のデータベース テーブル、ビュー、およびストアド プロシージャに指定されたロール ストアが必要です。 使用してこれらの必要なデータベース オブジェクトを追加することができます、 [ `aspnet_regsql.exe`ツール](https://msdn.microsoft.com/en-us/library/ms229862.aspx)です。 この時点で既にデータベースがあるために必要なスキーマを持つ、`SqlRoleProvider`です。 戻り、 <a id="_msoanchor_6"> </a> [*メンバーシップ スキーマを作成する SQL Server で*](../membership/creating-the-membership-schema-in-sql-server-vb.md)という名前のデータベースを作成したチュートリアル`SecurityTutorials.mdf`使用`aspnet_regsql.exe`アプリケーションを追加サービスに必要なデータベース オブジェクトが含まれている、`SqlRoleProvider`です。 したがってだけいただくために使用するロールのサポートを有効にして、ロール フレームワーク、`SqlRoleProvider`で、`SecurityTutorials.mdf`ロール ストアとしてのデータベースです。

使用して役割、フレームワークが構成されている、`<roleManager>`アプリケーションの要素`Web.config`ファイル。 既定では、ロールのサポートは無効です。 有効にするに設定する必要があります、 [ `<roleManager>` ](https://msdn.microsoft.com/en-us/library/ms164660.aspx)要素の`enabled`属性を`true`次のようにします。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

既定では、すべての web アプリケーションがあるという名前のロール プロバイダー`AspNetSqlRoleProvider`型の`SqlRoleProvider`します。 この既定のプロバイダーが登録されている`machine.config`(にある`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`)。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

プロバイダーの`connectionStringName`属性が使用されるロール ストアを指定します。 `AspNetSqlRoleProvider`プロバイダーでは、この属性を設定`LocalSqlServer`でも定義されている`machine.config`と既定では、SQL Server 2005 Express Edition のデータベースに、ポイント、`App_Data`という名前のフォルダー`aspnet.mdf`です。

その結果、おだけを有効にするロール framework アプリケーションでの任意のプロバイダー情報を指定しない場合`Web.config`ファイル、アプリケーションが登録されている既定のロール プロバイダーを使用して`AspNetSqlRoleProvider`です。 場合、`~/App_Data/aspnet.mdf`データベースが存在しないか、ASP.NET ランタイムが自動的に作成し、アプリケーションのサービス スキーマを追加します。 ただし、ここを使用しない、`aspnet.mdf`データベース; を使用する代わりに、`SecurityTutorials.mdf`データベースを既に作成して、アプリケーションのサービス スキーマを追加しました。 この変更は、2 つの方法のいずれかで実行できます。

- **値を指定、* * *`LocalSqlServer`* * * 接続文字列名に * * *`Web.config`* * *。** 上書きすることによって、`LocalSqlServer`の接続文字列名値`Web.config`は登録されている既定のロール プロバイダーを使用することができます (`AspNetSqlRoleProvider`) で正しく動作させることが、`SecurityTutorials.mdf`データベース。 この方法の詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[を使用して SQL Server 2000 または SQL Server 2005 に ASP.NET 2.0 アプリケーション サービスを構成する](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)です。
- **型 * * * の場合は、新しい登録済みプロバイダーの追加`SqlRoleProvider`* * * を構成して、* * *`connectionStringName`* * * をポイントする設定、* * *`SecurityTutorials.mdf`* * * データベース。** これは、ことをお勧めしてで使用される方法は、 <a id="_msoanchor_7"> </a> [*メンバーシップ スキーマを作成する SQL Server で*](../membership/creating-the-membership-schema-in-sql-server-vb.md)チュートリアルでは、これはもこのチュートリアルで使用するアプローチです。

次の役割の構成のマークアップを追加、`Web.config`ファイル。 このマークアップという名前の新しいプロバイダーを登録します。`SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

上記のマークアップを定義、`SecurityTutorialsSqlRoleProvider`既定のプロバイダーとして (を使用して、`defaultProvider`属性、`<roleManager>`要素)。 また、設定、`SecurityTutorialsSqlRoleProvider`の`applicationName`設定`SecurityTutorials`、これは同じ`applicationName`メンバーシップ プロバイダーによって使用される設定 (`SecurityTutorialsSqlMembershipProvider`)。 ここでは、表示されません中に、 [ `<add>`要素](https://msdn.microsoft.com/en-us/library/ms164662.aspx)の`SqlRoleProvider`含まれることも、`commandTimeout`データベース タイムアウト期間を秒単位で指定する属性。 既定値は 30 です。

この構成には、インプレースのマークアップが、アプリケーション内でロール機能の使用を開始する準備が整いました。

> [!NOTE]
> 使用して上記の構成のマークアップを示しています、`<roleManager>`要素の`enabled`と`defaultProvider`属性。 ロール framework がユーザーによってごとにロール情報を関連付ける方法に影響するその他の属性の数があります。 これらの設定で見ていきます、 <a id="_msoanchor_8"> </a> [*ロールベース承認*](role-based-authorization-vb.md)チュートリアルです。


## <a name="step-3-examining-the-roles-api"></a>手順 3: API の役割を確認します。

ロール フレームワークの機能がを介して公開される、 [ `Roles`クラス](https://msdn.microsoft.com/en-us/library/system.web.security.roles.aspx)、ロール ベースの操作を実行するための 13 個の共有メソッドが含まれています。 ときに作成および手順 4. でロールを削除する見るし、6 を使用、 [ `CreateRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.createrole.aspx)と[ `DeleteRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.deleterole.aspx)メソッドを追加またはロールをシステムから削除します。

システム内のすべての役割の一覧を取得する、 [ `GetAllRoles`メソッド](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getallroles.aspx)(手順 5. を参照してください)。 [ `RoleExists`メソッド](https://msdn.microsoft.com/en-us/library/system.web.security.roles.roleexists.aspx)指定されたロールが存在するかどうかを示すブール値を返します。

次のチュートリアルでは、ユーザー ロールに関連付ける方法について調べます。 `Roles`クラスの[ `AddUserToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertorole.aspx)、 [ `AddUserToRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.addusertoroles.aspx)、 [ `AddUsersToRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.adduserstorole.aspx)、および[ `AddUsersToRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.adduserstoroles.aspx)メソッドは、1 つまたは複数のユーザーを 1 つまたは複数のロールに追加します。 ユーザー ロールから削除するを使用して、 [ `RemoveUserFromRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromrole.aspx)、 [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeuserfromroles.aspx)、 [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeusersfromrole.aspx)、または[ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.removeusersfromroles.aspx)メソッド。

<a id="_msoanchor_9"> </a> [*ロールベース承認*](role-based-authorization-vb.md)チュートリアル プログラムで非表示機能、現在ログインしているユーザーの役割に基づいた方法を紹介します。 ロール クラスを使用してこれを実現する[ `FindUsersInRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.findusersinrole.aspx)、 [ `GetRolesForUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getrolesforuser.aspx)、 [ `GetUsersInRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.getusersinrole.aspx)、または[ `IsUserInRole` ](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx)メソッドです。

> [!NOTE]
> これらのメソッドのいずれかが呼び出されることに注意してください、`Roles`クラスが構成されているプロバイダーへの呼び出しを代行します。 ここではつまり、呼び出しに送信されている、`SqlRoleProvider`です。 `SqlRoleProvider`呼び出されたメソッドに基づく適切なデータベース操作を実行します。 たとえば、コード`Roles.CreateRole("Administrators")`結果、`SqlRoleProvider`を実行する、`aspnet_Roles_CreateRole`に新しいレコードを挿入するプロシージャが格納されている、`aspnet_Roles`管理者という名前のテーブルです。


このチュートリアルの残りの部分参照を使用して、`Roles`クラスの`CreateRole`、 `GetAllRoles`、および`DeleteRole`システムの役割を管理する方法です。

## <a name="step-4-creating-new-roles"></a>手順 4: 新しいロールの作成

ロールを任意のグループのユーザーを使用して、承認規則を適用する方が便利な方法をこのグループ化を使用する最も一般的です。 最初に、アプリケーション内にあるロールを定義する必要がありますの認証メカニズムとしての役割を使用するためにします。 残念ながら、ASP.NET では、CreateRoleWizard コントロールは含まれません。 新しいロールを追加するのには、適切なユーザー インターフェイスを作成し、社内でロール API を呼び出す必要があります。 良いニュースは、これは、非常に簡単に実行します。

> [!NOTE]
> CreateRoleWizard Web コントロールはありませんが、 [ASP.NET Web サイト管理ツール](https://msdn.microsoft.com/en-us/library/ms228053.aspx)、これは、ローカルの ASP.NET アプリケーションを表示して、web アプリケーションの構成の管理を支援するように設計します。 ただし、私はいない 2 つの理由の大好きな ASP.NET Web サイト管理ツールです。 最初に、少しバグであるし、ユーザー エクスペリエンスが必要なことはたくさんのままにします。 次に、ASP.NET Web サイト管理ツールは設計されていますローカルでのみ動作する実際のサイトの役割をリモートで管理する必要がある場合、独自のロールの管理 web ページを構築する必要があることを意味します。 これら 2 つの理由から、このチュートリアルと、次へは焦点 ASP.NET Web サイト管理ツールで証明書利用者のではなく、必要なロール、web ページで、管理ツールを構築します。


開く、 `ManageRoles.aspx`  ページで、`Roles`フォルダーと、ページにテキスト ボックスとボタンの Web コントロールを追加します。 TextBox コントロールの設定`ID`プロパティを`RoleName`とボタンの`ID`と`Text`プロパティ`CreateRoleButton`とロールの作成、それぞれします。 この時点では、ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

次をダブルクリックして、`CreateRoleButton`ボタン コントロールを作成するデザイナーで、`Click`イベント ハンドラーし、次のコードを追加します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

上記のコードを開始で入力したトリミングされたロール名を割り当てることによって、`RoleName`するテキスト ボックス、`newRoleName`変数。 次に、`Roles`クラスの`RoleExists`かどうかをメソッドが呼び出されたロール`newRoleName`システムに既に存在します。 ロールが存在しない場合への呼び出しを使用して作成、`CreateRole`メソッドです。 場合、`CreateRole`メソッドは、システムに既に存在するロール名が渡される、`ProviderException`例外がスローされます。 これは、ため、コードは、ロールが存在しないように既に呼び出す前に、システムでまず`CreateRole`です。 `Click`をクリアするイベント ハンドラーの終了、`RoleName`テキスト ボックスの`Text`プロパティです。

> [!NOTE]
> 思うかもしれません何が起こるかユーザーに任意の値を入力しない場合、 `RoleName`  テキスト ボックス。 値が渡された場合、`CreateRole`メソッドは`Nothing`空の文字列、例外が発生します。 同様に、ロール名にコンマが含まれている場合、例外が発生します。 そのため、ページには、ユーザーは、ロールの入力をコンマが含まれていないことを確認する検証コントロールを含める必要があります。 私は、リーダーの課題としてままにします。


管理者をという名前のロールを作成してみましょう。 参照してください、`ManageRoles.aspx`ブラウザーを使って ページで、管理者に、テキスト ボックスに入力 (図 3 を参照)、ロールの作成ボタンをクリックします。


[![管理者ロールを作成します。](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**図 3**: 管理者の役割を作成 ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image9.png))


どうなりますか。 ポストバックが発生するがないを視覚的にロールがされて実際には、システムに追加します。 このページを視覚的なフィードバックを含める手順 5 で更新されます。 ここでは、ただし、ことを確認するに移動して、ロールが作成されたこと、`SecurityTutorials.mdf`データベースとデータを表示する、`aspnet_Roles`テーブル。 図 4 に示す、`aspnet_Roles`テーブルには、単に追加された管理者の役割のレコードが含まれています。


[![Aspnet_Roles テーブルは、管理者は、行を含む](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**図 4**:`aspnet_Roles`テーブルには、管理者用の行 ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>手順 5: システムの役割を表示します。

みましょう補強、`ManageRoles.aspx`システムの現在のロールの一覧を挿入するページ。 これを実現する GridView コントロールをページに追加し、設定、`ID`プロパティを`RoleList`です。 という名前のページの分離コード クラスにメソッドを次に、追加`DisplayRolesInGrid`次のコードを使用します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

`Roles`クラスの`GetAllRoles`メソッドのすべての役割のシステムとして返します文字列の配列。 この文字列の配列が GridView にバインドし、されています。 ページが最初に読み込まれたときに、ロールの一覧を GridView にバインドする必要がありますを呼び出して、`DisplayRolesInGrid`メソッドから、ページの`Page_Load`イベント ハンドラー。 次のコードは、ページが初めてアクセスしたときに、以降のポストバックではなく、このメソッドを呼び出します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

配置でこのコードでは、ブラウザーでページを参照してください。 図 5 に示す項目ラベルの付いた 1 つの列をグリッドが表示されます。 グリッドには、手順 4. で追加して、管理者ロールの行が含まれています。


[![GridView は、1 つの列で、ロールを表示します。](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**図 5**: GridView はの役割を 1 つの列に表示されます ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image15.png))


GridView ために、ラベルの付いた項目内の単一行の列を表示する GridView の`AutoGenerateColumns`プロパティが True (既定)、これにより、内の各プロパティの列を自動的に作成する GridView にその`DataSource`です。 配列には、GridView で 1 つの列のため、配列内の要素を表す 1 つのプロパティがあります。

GridView にデータを表示するときに (_n) GridView によって暗黙的に生成されることがあるのではなく、明示的に列を定義します。 によって、データの書式をはるかに簡単である列を明示的に定義するには、列を並べ替えるにし、その他の一般的なタスクを実行します。 このため、みましょう更新 GridView の宣言型マークアップの列が明示的に定義できるようにします。

開始するには、GridView の`AutoGenerateColumns`プロパティを False にします。 次に、設定、TemplateField をグリッドに追加の`HeaderText`、ロールにプロパティを構成して、`ItemTemplate`配列の内容を表示するようです。 これを実現する、という名前のラベルの Web コントロールを追加`RoleNameLabel`を`ItemTemplate`バインドとその`Text`プロパティ`Container.DataItem.`

これらのプロパティと`ItemTemplate`の宣言またはにより GridView のフィールド] ダイアログ ボックスと [テンプレートの編集インターフェイス コンテンツを設定できます。 フィールド ダイアログ ボックスが、接続するには、GridView のスマート タグの列の編集リンクをクリックします。 次に、自動生成を設定するフィールド のチェック ボックスをオフに、`AutoGenerateColumns`プロパティを False に設定、GridView に TemplateField を追加し、その`HeaderText`ロールにプロパティです。 定義する、`ItemTemplate`の内容が GridView のスマート タグからテンプレートの編集オプションを選択します。 上にラベル Web コントロールをドラッグ、`ItemTemplate`設定、その`ID`プロパティを`RoleNameLabel`、し、データ バインド設定を構成できるようにその`Text`プロパティにバインド`Container.DataItem`です。

使用するどのような方法に関係なく、GridView の宣言型マークアップの結果として得られるようになります次が完了したらです。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> データ バインド構文を使用して、配列の内容が表示される`<%# Container.DataItem %>`です。 配列の内容を表示する GridView にバインドされている場合にこの構文を使用する理由の詳細な説明についてはこのチュートリアルの範囲外です。 この問題の詳細についてを参照してください[データ Web コントロールへのスカラー配列のバインド](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)です。


現時点では、 `RoleList` GridView は、ページが初めてアクセスしたときに、のみの役割の一覧にバインドします。 新しいロールを追加するたびに、グリッドを更新する必要があります。 これを実現する、更新、`CreateRoleButton`ボタンの`Click`呼び出すためのイベント ハンドラー、`DisplayRolesInGrid`メソッドの場合は、新しいロールを作成します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

今すぐユーザーが新しいロールを追加すると、`RoleList`視覚的なフィードバック、ロールが正常に作成されたことを提供する GridView ポストバックで単に追加されたロールを示しています。 これを示すためには、次を参照してください。、`ManageRoles.aspx`ブラウザー内でページと、監督者をという名前のロールを追加します。 ロールの作成 ボタンをクリックすると、ポストバックが発生したりして、グリッドが更新され、新しいロール、監督者と管理者だけが含まれます。


[![スーパーバイザー ロールが追加されました](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**図 6**: スーパーバイザー ロールが追加されました ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>手順 6: ロールを削除します。

この時点でユーザーを新しいロールを作成してからのすべての既存のロールを表示できます、`ManageRoles.aspx`ページ。 ロールを削除するユーザーを許可してみましょう。 `Roles.DeleteRole`メソッドに次の 2 つのオーバー ロードがあります。

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/en-us/library/ek4sywc0.aspx)-ロール削除*roleName*です。 役割には、1 つまたは複数のメンバーが含まれている場合は、例外がスローされます。
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/en-us/library/38h6wf59.aspx)-ロール削除*roleName*です。 場合*throwOnPopulateRole*は`True`役割には、1 つまたは複数のメンバーが含まれている場合、例外がスローされます。 場合*throwOnPopulateRole*は`False`が含まれているすべてのメンバーかどうかどうか、ロールが削除されます。 内部的には、`DeleteRole(roleName)`メソッド呼び出し`DeleteRole(roleName, True)`です。

`DeleteRole`メソッドも例外がスローされる場合*roleName*は`Nothing`または空の文字列または*roleName*コンマが含まれています。 場合*roleName* 、システムに存在しません`DeleteRole`例外を発生させずに自動的に、失敗します。

みましょうで GridView を補強`ManageRoles.aspx`に含める、削除のボタンで、クリックすると、選択したロールを削除します。 フィールド ダイアログ ボックスに移動しが CommandField オプションである 削除 ボタンを追加する GridView の削除 ボタンを追加することで開始します。 列の左端のボタンをクリックし、設定の削除を行うその`DeleteText`ロールを削除するプロパティです。


[![RoleList GridView に [削除] ボタンを追加します。](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**図 7**: 削除ボタンを追加して、 `RoleList` GridView ([フルサイズのイメージを表示するをクリックして](creating-and-managing-roles-vb/_static/image21.png))


[削除] ボタンを追加すると、GridView の宣言型マークアップを次のようになります。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Gridview のイベント ハンドラーを次に、作成`RowDeleting`イベント。 これは、役割の削除 ボタンがクリックされたときにポストバック時に発生するイベントです。 イベント ハンドラーに次のコードを追加します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

プログラムで参照することによって、コードの開始、 `RoleNameLabel` Web コントロールの役割の削除ボタンがクリックされた行にします。 `Roles.DeleteRole`メソッドが呼び出されで渡すこと、`Text`の`RoleNameLabel`と`False`、それによってかどうかに関係なく、ロールを削除すると、ロールに関連付けられているすべてのユーザーがあります。 最後に、 `RoleList` GridView が更新されるので、単に削除された役割がグリッドに表示されなくなります。

> [!NOTE]
> 役割の削除 ボタンには、あらゆる種類のロールを削除する前に、ユーザーから送信される確認は不要です。 クライアント側の確認 ダイアログ ボックスで、操作を確認する最も簡単な方法の 1 つです。 この方法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-vb.aspx)です。


## <a name="summary"></a>概要

多くの web アプリケーションがある特定の承認規則またはページ レベルの機能だけがユーザーの特定のクラスを使用できます。 たとえば、管理者のみがアクセスできる web ページのセットが存在する可能性があります。 多くの場合、ユーザー単位のユーザーごとにこれらの承認規則を定義するのではなくは役割に基づいて規則を定義する方が実用的です。 ユーザー管理の web ページにアクセスするには、Scott および Jisun を明示的に許可するではなくより優れた方法は、管理者ロールのメンバーは、これらのページにアクセスを許可するようにし、およびを意味する Scott Jisun に属するユーザーとして、管理者の役割。

ロール フレームワークでは、簡単に作成し、ロールを管理します。 このチュートリアルで使用するロールのフレームワークを構成する方法を調べるお、 `SqlRoleProvider`、ロール ストアとして、Microsoft SQL Server データベースを使用します。 システムで既存のロールを一覧表示され、作成する新しいロールのする web ページ、および削除する既存のも作成しました。 以降のチュートリアルでロールベースの承認を適用する方法と、ユーザーをロールに割り当てる方法が表示されます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET 2.0 を確認するメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [ASP.NET 2.0 のロール マネージャーを使用する方法](https://msdn.microsoft.com/en-us/library/ms998314.aspx)
- [ロール プロバイダー](https://msdn.microsoft.com/en-us/library/aa478950.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [技術ドキュメント、`<roleManager>`要素](https://msdn.microsoft.com/en-us/library/ms164660.aspx)
- [メンバーシップとロール マネージャー Api を使用してください。](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Alicja Maziarz、Suchi Banerjee Teresa マーフィーなどがあります。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](role-based-authorization-cs.md)
[次へ](assigning-roles-to-users-vb.md)
