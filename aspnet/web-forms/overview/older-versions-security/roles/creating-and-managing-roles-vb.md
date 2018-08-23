---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: ロール (VB の) 作成と管理 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成および削除するロールを作成します。
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: e51fa6de3d2fe7b5c9cd84900d154070eb1960b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832779"
---
<a name="creating-and-managing-roles-vb"></a>作成と管理ロール (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成および削除するロールを作成します。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル URL 承認を使用して、ページのセットから特定のユーザーを制限する見てお宣言を検討し、アクセスしたユーザーに基づく ASP.NET ページの機能を調整するためのプログラムによる手法。 ページにアクセスまたはユーザーでのユーザーごとに機能に対するアクセス許可を付与するには、ただしになれるメンテナンス悪夢のシナリオが多くのユーザー アカウントがある場合、またはユーザーの特権を頻繁に変更します。 ユーザーを取得したり、特定のタスクを実行するための承認を失ったり、いつでも、管理者は、適切な URL 承認規則、宣言型マークアップは、コードを更新する必要があります。

通常、ユーザーをグループに分類しやすくまたは*ロール*し、アクセス許可をロール別のロールごとに適用します。 たとえば、ほとんどの web アプリケーションでは、ページまたは管理ユーザーに対してのみに予約されているタスクの特定のセットがあります。 学習した手法を使用して、*ユーザー ベースの承認*適切な URL 承認規則、宣言型マークアップ、および管理タスクを実行する指定されたユーザー アカウントを許可するコードを追加、チュートリアルでは、します。 返すし、構成ファイルや web ページを更新する必要が新しい管理者を追加した場合、または既存の管理者が失効して自分の管理権限を持っている必要な場合。 ただし、ロールで Administrators というロールを作成およびこれらの信頼されたユーザーを管理者ロールに割り当てるでしたいます。 次に、適切な URL 承認規則、宣言型マークアップ、およびさまざまな管理タスクを実行する管理者ロールを許可するコードは追加します。 このインフラストラクチャと、サイトへの新しい管理者の追加、削除する既存が、含むまたは管理者ロールからユーザーを削除して同じくらい簡単です。 構成、宣言型のマークアップまたはコードの変更は必要ありません。

ASP.NET には、ロールの定義とユーザー アカウントに関連付けるロール フレームワークが用意されています。 ロールのフレームワークを作成してロールを削除、またはロールからユーザーを削除、特定のロールに属し、特定のロールにユーザーが属しているかどうかを通知するユーザーのセットを決定するユーザーを追加します。 ロール フレームワークを構成するは、URL 承認規則をロール別のロールごとにページへのアクセスを制限してと表示または、追加の情報または現在ログオンしているユーザーのロールに基づいて、ページの機能を非表示にするできます。

このチュートリアルでは、ロールのフレームワークを構成するために必要な手順について説明します。 次は、web ページを作成および削除するロールを作成します。 <a id="_msoanchor_2"> </a> [*ユーザー ロールの割り当て*](assigning-roles-to-users-vb.md)チュートリアルを追加し、ロールからユーザーを削除する方法を紹介します。 および、 <a id="_msoanchor_3"> </a> [*ロールベースの承認*](role-based-authorization-vb.md)ページ機能によってを調整する方法とロールを役割ごとにページへのアクセスを制限する方法を説明するチュートリアルアクセスしたユーザーのロール。 それでは、始めましょう!

## <a name="step-1-adding-new-aspnet-pages"></a>手順 1: 新しい ASP.NET ページの追加

このチュートリアルで次の 2 つ検査さまざまなロール関連の機能と機能します。 一連のトピックでは、このチュートリアルで調査を実装するために ASP.NET ページを必要になります。 これらのページを作成して、サイト マップを更新してみましょう。

という名前のプロジェクトで新しいフォルダーを作成して開始`Roles`します。 4 つの新しい ASP.NET ページを次に、追加、`Roles`フォルダーを使用する各ページのリンク、`Site.master`マスター ページ。 ページの名前を付けます。

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

この時点で、プロジェクトのソリューション エクスプ ローラーのスクリーン ショット、図 1 に示すようなはずです。


[![[ロール] フォルダーに 4 つの新しいページが追加されました](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**図 1**: 4 つ新しいページに追加されている、`Roles`フォルダー ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image3.png))。


各ページが 2 つのコンテンツ コントロールは、マスター ページの ContentPlaceHolders ごとに 1 つがある必要があります、この時点では、:`MainContent`と`LoginContent`します。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

いることを思い出してください、 `LoginContent` ContentPlaceHolder の既定のマークアップがログオンまたはログオフ、ユーザーが認証されているかどうかによっては、サイトへのリンクが表示されます。 有無、`Content2`コンテンツ、ASP.NET ページ内のコントロールは、ただし、マスター ページの既定のマークアップを上書きします。 説明したよう<a id="_msoanchor_4"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)チュートリアルでは、既定のマークアップのオーバーライドは、たくないを表示するログインに関連するページで便利です左側の列のオプションです。

これら 4 つのページでは、ただし、表示することのマスター ページの既定のマークアップ、`LoginContent`プレース ホルダーです。 そのため、宣言型マークアップを削除する、`Content2`コンテンツ コントロール。 その後、1 つだけのコンテンツ コントロールを含めることがそれぞれ 4 つのページのマークアップの必要があります。

最後に、サイト マップを更新してみましょう (`Web.sitemap`) に、これらの新しい web ページが含まれます。 追加した後、次の XML、`<siteMapNode>`チュートリアルでは、メンバーシップを追加しました。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

更新サイト マップでは、ブラウザーを使用してサイトを参照してください。 図 2 に示す、左側のナビゲーションには、ロール チュートリアルについては、アイテムが含まれます。


[![[ロール] フォルダーに 4 つの新しいページが追加されました](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**図 2**: 4 つ新しいページに追加されている、`Roles`フォルダー ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image6.png))。


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>手順 2: 指定して、ロール Framework プロバイダーの構成

メンバーシップ フレームワークのようなプロバイダー モデルの上の役割のフレームワークが構築されています。 説明したように、 <a id="_msoanchor_5"> </a> [*セキュリティの基礎と ASP.NET のサポート*](../introduction/security-basics-and-asp-net-support-vb.md)チュートリアルでは、次の 3 つの組み込みのロール プロバイダーが .NET Framework が付属しています: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx)、 [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)、および[ `SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)します。 このチュートリアル シリーズについて重点的に、 `SqlRoleProvider`、ロール ストアとして Microsoft SQL Server データベースを使用します。

内部での役割のフレームワークと`SqlRoleProvider`メンバーシップ フレームワークと同様の作業と`SqlMembershipProvider`します。 .NET Framework には、`Roles`ロール フレームワークを API として機能するクラス。 `Roles`クラスなどのメソッドが共有されている`CreateRole`、 `DeleteRole`、 `GetAllRoles`、 `AddUserToRole`、`IsUserInRole`となります。 これらのメソッドのいずれかが呼び出されると、`Roles`クラスが構成済みのプロバイダーへの呼び出しを代行させます。 `SqlRoleProvider`役割に固有のテーブルを処理 (`aspnet_Roles`と`aspnet_UsersInRoles`) で応答します。

使用するには、`SqlRoleProvider`プロバイダー アプリケーションでは、ストアとして使用するデータベースの内容を指定する必要があります。 `SqlRoleProvider`は特定のデータベース テーブル、ビュー、およびストアド プロシージャに指定されたロール ストアが必要です。 使用して、これらの必要なデータベース オブジェクトを追加することができます、 [ `aspnet_regsql.exe`ツール](https://msdn.microsoft.com/library/ms229862.aspx)します。 この時点で既にデータベースがあるために必要なスキーマを使用して、`SqlRoleProvider`します。 戻り、 <a id="_msoanchor_6"> </a> [ *SQL Server でメンバーシップ スキーマを作成*](../membership/creating-the-membership-schema-in-sql-server-vb.md)という名前のデータベースを作成したチュートリアル`SecurityTutorials.mdf`使用`aspnet_regsql.exe`してアプリケーションを追加するにはサービスに必要なデータベース オブジェクトに含まれる、`SqlRoleProvider`します。 したがってするだけの役割のフレームワークをロールのサポートを有効に使用するかを知らせる、`SqlRoleProvider`で、`SecurityTutorials.mdf`ロール ストアとしてのデータベース。

使用して、ロール framework が構成されている、`<roleManager>`アプリケーションの要素`Web.config`ファイル。 既定では、ロールのサポートは無効になります。 これを有効に設定する必要があります、 [ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx)要素の`enabled`属性を`true`ようになります。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

既定では、すべての web アプリケーションがあるという名前のロール プロバイダー`AspNetSqlRoleProvider`型の`SqlRoleProvider`します。 この既定のプロバイダーが登録されている`machine.config`(ある`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`)。

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

プロバイダーの`connectionStringName`属性が使用されるロール ストアを指定します。 `AspNetSqlRoleProvider`プロバイダーでは、この属性を設定`LocalSqlServer`でも定義されている`machine.config`と既定では、SQL Server 2005 Express Edition のデータベースに、ポイント、`App_Data`という名前のフォルダー`aspnet.mdf`します。

その結果、アプリケーションでの任意のプロバイダー情報を指定せず単に有効にするロール framework 場合`Web.config`ファイル、アプリケーションは、登録されている既定のロール プロバイダーを使用して`AspNetSqlRoleProvider`します。 場合、`~/App_Data/aspnet.mdf`データベースが存在しないか、ASP.NET ランタイムに自動的にそれを作成し、アプリケーションのサービス スキーマを追加します。 ただし、使用するたく、`aspnet.mdf`を使用する代わりに、データベースでは、`SecurityTutorials.mdf`データベースを既に作成して、アプリケーションのサービス スキーマを追加します。 この変更は、2 つの方法のいずれかで実行できます。

- <strong>値を指定、</strong><strong>`LocalSqlServer`</strong><strong>接続文字列名を</strong><strong>`Web.config`</strong><strong>します。</strong> 上書きすることで、`LocalSqlServer`の接続文字列名値`Web.config`、登録されている既定のロール プロバイダーを使用できます (`AspNetSqlRoleProvider`) で正しく動作させることが、`SecurityTutorials.mdf`データベース。 この手法の詳細については、次を参照してください。 [Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[使用して SQL Server 2000 または SQL Server 2005 に ASP.NET 2.0 アプリケーション サービスを構成する](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)します。
- <strong>型の新しい登録済みプロバイダーの追加</strong><strong>`SqlRoleProvider`</strong><strong>構成とその</strong><strong>`connectionStringName`</strong><strong>をポイントする設定</strong><strong>`SecurityTutorials.mdf`</strong><strong>データベース。</strong> これが推奨しで使用される方法、 <a id="_msoanchor_7"> </a> [ *SQL Server でメンバーシップ スキーマを作成する*](../membership/creating-the-membership-schema-in-sql-server-vb.md)チュートリアルでは、これはもこのチュートリアルで使用する方法。

次の役割の構成マークアップを追加、`Web.config`ファイル。 このマークアップは、という名前の新しいプロバイダーを登録します。 `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

上記のマークアップを定義、`SecurityTutorialsSqlRoleProvider`として既定のプロバイダー (を使用して、`defaultProvider`属性、`<roleManager>`要素)。 また、設定、`SecurityTutorialsSqlRoleProvider`の`applicationName`設定`SecurityTutorials`、同じ`applicationName`メンバーシップ プロバイダーによって使用される設定 (`SecurityTutorialsSqlMembershipProvider`)。 ここでは、示されていません中に、 [ `<add>`要素](https://msdn.microsoft.com/library/ms164662.aspx)の`SqlRoleProvider`も含めることができます、`commandTimeout`データベースのタイムアウト期間を秒単位で指定する属性。 既定値は、30 です。

この場所での構成マークアップで、アプリケーション内でロール機能の使用を開始する準備が整いました。

> [!NOTE]
> 使用して、上記の構成マークアップを示しています、`<roleManager>`要素の`enabled`と`defaultProvider`属性。 ロールのフレームワークがユーザー単位のユーザーごとにロール情報を関連付ける方法に影響するその他の属性を数多くあります。 これらの設定では説明、 <a id="_msoanchor_8"> </a> [*ロールベースの承認*](role-based-authorization-vb.md)チュートリアル。


## <a name="step-3-examining-the-roles-api"></a>手順 3: ロール API のチェック

使用して、ロールのフレームワークの機能が公開されている、 [ `Roles`クラス](https://msdn.microsoft.com/library/system.web.security.roles.aspx)、ロール ベースの操作を実行するための 13 個の共有メソッドが含まれています。 作成して、手順 4 でのロールを削除するのに注目し、6 を使用、 [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx)と[ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx)メソッドを追加またはロールをシステムから削除します。

システムのすべての役割の一覧を取得する、 [ `GetAllRoles`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)(手順 5 を参照してください)。 [ `RoleExists`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx)指定されたロールが存在するかどうかを示すブール値を返します。

次のチュートリアルでは、ユーザー ロールを関連付ける方法を説明します。 `Roles`クラスの[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)、 [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx)、 [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)、および[ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx)メソッドは、1 つまたは複数のロールに 1 つまたは複数のユーザーを追加します。 ロールからユーザーを削除するには、使用、 [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)、 [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx)、 [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)、または[ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx)メソッド。

<a id="_msoanchor_9"> </a> [*ロールベースの承認*](role-based-authorization-vb.md)プログラムから表示、または、現在ログインしているユーザーのロールに基づいて機能を非表示にする方法を紹介するチュートリアル。 これを実現するロール クラスを使用してできる[ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx)、 [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)、 [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)、または[ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)メソッド。

> [!NOTE]
> これらのメソッドのいずれかが呼び出されることに留意してください、`Roles`クラスが構成済みのプロバイダーへの呼び出しを代行させます。 ここでは、つまり、呼び出しに送信されている、`SqlRoleProvider`します。 `SqlRoleProvider`呼び出されたメソッドに基づいて、適切なデータベース操作を実行します。 たとえば、コード`Roles.CreateRole("Administrators")`結果、`SqlRoleProvider`を実行する、`aspnet_Roles_CreateRole`ストアド プロシージャに新しいレコードを挿入する、`aspnet_Roles`管理者という名前のテーブル。


このチュートリアルの残りの部分を使用して調べ、`Roles`クラスの`CreateRole`、 `GetAllRoles`、および`DeleteRole`システムの役割を管理する方法。

## <a name="step-4-creating-new-roles"></a>手順 4: 新しいロールの作成

任意のグループのユーザーにロールを使うし、承認規則を適用する方が便利にこのグループ化を使用する最も一般的です。 ただし、まず、アプリケーション内にあるロールを定義する必要があります承認メカニズムとしての役割を使用するには。 残念ながら、ASP.NET では、CreateRoleWizard コントロールは含まれません。 新しいロールを追加するには、適切なユーザー インターフェイスを作成し、自分たちの役割の API を呼び出す必要があります。 良い知らせは、非常に簡単であります。

> [!NOTE]
> CreateRoleWizard Web コントロールはありませんが、中には、 [ASP.NET Web サイト管理ツール](https://msdn.microsoft.com/library/ms228053.aspx)、これは、ローカルの ASP.NET アプリケーションを表示して、web アプリケーションの構成の管理を支援するように設計します。 ただし、私しない ASP.NET Web サイトの管理ツールの 2 つの理由の大ファンです。 まず、少しバグが、ユーザー エクスペリエンスが望ましい場合に多くのまま。 次に、ASP.NET Web サイトの管理ツールは設計されています、ローカルでのみ動作するライブ サイトの役割をリモートで管理する必要がある場合、独自のロールの管理 web ページを構築する必要があります。 これら 2 つの理由から、このチュートリアルと次の重点的に ASP.NET Web サイトの管理ツールに依存するのではなく、必要なロール、web ページで、管理ツールを構築します。


開く、`ManageRoles.aspx`ページで、`Roles`フォルダーと、ページに、テキスト ボックスとボタンの Web コントロールを追加します。 TextBox コントロールの設定`ID`プロパティを`RoleName`とボタンの`ID`と`Text`プロパティ`CreateRoleButton`とロールの作成、それぞれします。 この時点では、ページの宣言型マークアップは、次のようになります。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

次をダブルクリック、`CreateRoleButton`ボタン コントロールを作成するデザイナーで、`Click`イベント ハンドラーし、次のコードを追加します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

入力されたトリムされたロール名を割り当てることで、上記のコードを開始、`RoleName`するテキスト ボックス、`newRoleName`変数。 次に、`Roles`クラスの`RoleExists`かを判断するメソッドが呼び出されたロール`newRoleName`システムに既に存在します。 呼び出しを使用して作成、ロールが存在しない場合、`CreateRole`メソッド。 場合、`CreateRole`メソッドには、システムに既に存在するロール名が渡されます、`ProviderException`例外がスローされます。 これが、理由コードは、ロールが呼び出す前に、システムに存在しないことを確認します。 まず`CreateRole`します。 `Click`イベント ハンドラーの終了オフにすると、`RoleName`テキスト ボックスの`Text`プロパティ。

> [!NOTE]
> 疑問が起こる、ユーザーに任意の値を入力しない場合、`RoleName`テキスト ボックス。 値が渡された場合、`CreateRole`メソッドは`Nothing`または空の文字列、例外が発生しました。 同様に、ロール名にコンマが含まれている場合は、例外が発生します。 その結果、ページには、ユーザーが役割を移行して、コンマが含まれていないことを確認する検証コントロールを含める必要があります。 私を演習としては、リーダーのままにします。


Administrators というロールを作成してみましょう。 参照してください、`ManageRoles.aspx`ブラウザーでページで、管理者、テキスト ボックスに入力 (図 3 参照)、ロールの作成 ボタンを順にクリックします。


[![管理者ロールを作成します。](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**図 3**: 管理者の役割を作成 ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image9.png))。


何がどうなりますか。 ポストバックが発生したが、実際には、ロールが視覚的な合図がありませんがシステムに追加します。 手順 5 を視覚的なフィードバックを含めるには、このページを更新します。 ここでは、ただし、ことを確認に移動して、ロールが作成されたこと、`SecurityTutorials.mdf`データベースとデータを表示する、`aspnet_Roles`テーブル。 図 4 に示すよう、`aspnet_Roles`テーブルに追加したばかりの管理者ロールのレコードが含まれています。


[![Aspnet_Roles テーブルでは、行を持つ管理者は、](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**図 4**:`aspnet_Roles`テーブルでは、行を持つ管理者は、([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image12.png))。


## <a name="step-5-displaying-the-roles-in-the-system"></a>手順 5: システムの役割を表示します。

拡張しましょう、`ManageRoles.aspx`をシステムの現在のロールの一覧を含めるようにページ。 これを実現するページに GridView コントロールを追加し、設定、`ID`プロパティを`RoleList`します。 という名前のページの分離コード クラスにメソッドを次に、追加`DisplayRolesInGrid`次のコードを使用します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

`Roles`クラスの`GetAllRoles`メソッドすべての役割のシステムとして返します文字列の配列。 この文字列の配列は、GridView にバインドします。 ページが最初に読み込まれたときに、ロールの一覧を GridView にバインドする必要がありますを呼び出す、`DisplayRolesInGrid`メソッドから、ページの`Page_Load`イベント ハンドラー。 次のコードは、ページが初めてアクセスしたときに、以降のポストバックではなく、このメソッドを呼び出します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

このコードでは、ブラウザーを使用してページを参照してください。 図 5 に示す項目というラベルの付いた 1 つの列のグリッドが表示されます。 グリッドには、手順 4 で追加された管理者ロールに 1 行が含まれています。


[![GridView には、1 つの列で、役割が表示されます。](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**図 5**: GridView では、1 つの列で、役割が表示されます ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image15.png))。


GridView のため、項目というラベルの付いた唯一の列を表示する GridView の`AutoGenerateColumns`プロパティ内の各プロパティの列を自動的に作成する GridView が True (既定値) に設定されてその`DataSource`します。 配列には、GridView では、1 つの列のため、配列内の要素を表す 1 つのプロパティがあります。

GridView を使用してデータを表示するには、する場合に、GridView で暗黙的に生成されることにするのではなく、明示的に列を定義するとします。 列を明示的に定義することで、データの書式設定、列を並べ替える、およびその他の一般的なタスクを実行するはるかに簡単です。 そのため、更新 GridView の宣言型マークアップの列が明示的に定義されているように。

GridView の設定で開始`AutoGenerateColumns`プロパティを False にします。 TemplateField をグリッドに追加します。 次に、設定、 `HeaderText` 、ロールにプロパティを構成するとその`ItemTemplate`配列の内容を表示するようです。 これを実現するという名前のラベルの Web コントロールを追加`RoleNameLabel`を`ItemTemplate`バインドとその`Text`プロパティを `Container.DataItem.`

これらのプロパティと`ItemTemplate`の内容を宣言的または GridView のフィールド] ダイアログ ボックスおよび [テンプレートの編集インターフェイスを介して設定できます。 ダイアログ ボックスで、フィールドに到達するには、GridView のスマート タグで列の編集リンクをクリックします。 次に、チェック ボックスをオフの自動生成フィールドを設定する、`AutoGenerateColumns`プロパティを False に設定、GridView を TemplateField を追加し、その`HeaderText`ロールにプロパティ。 定義する、`ItemTemplate`の内容、GridView のスマート タグからのテンプレートの編集 オプションを選択します。 ラベル Web コントロールを上にドラッグして、`ItemTemplate`に設定して、その`ID`プロパティを`RoleNameLabel`、およびそのデータ バインド設定を構成ようにその`Text`プロパティにバインドする`Container.DataItem`。

使用するどのようなアプローチに関係なく GridView の宣言型マークアップの結果として得られるようになります、次が完了します。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> データ バインディング構文を使用して、配列の内容が表示される`<%# Container.DataItem %>`します。 配列の内容を表示する GridView にバインドされている場合にこの構文を使用する理由の詳細な説明では、このチュートリアルの範囲外です。 この問題の詳細についてを参照してください[データ Web コントロールにスカラーの配列をバインド](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)します。


現時点では、`RoleList`ページが初めてアクセスしたときに、GridView がロールの一覧にバインドされているだけです。 新しいロールが追加されるたびに、グリッドを更新する必要があります。 これを行うには、更新、`CreateRoleButton`ボタンの`Click`イベント ハンドラーを呼び出すため、`DisplayRolesInGrid`メソッドの場合は、新しいロールを作成します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

ユーザーが新しいロールを追加するときに今すぐ、`RoleList`視覚的なフィードバック、ロールが正常に作成されたことを提供する GridView、ポストバックのだけで追加したロールを示しています。 これを示すためには、次を参照してください。、`ManageRoles.aspx`ブラウザーでページと、監督者をという名前のロールを追加します。 ロールの作成 ボタンをクリックすると、ポストバックが発生したりして、グリッドが更新され、管理者として、新しいロール、監督者などがあります。


[![スーパーバイザー ロールが追加されました](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**図 6**: 監督者役割が追加されました ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image18.png))。


## <a name="step-6-deleting-roles"></a>手順 6: ロールを削除します。

ユーザーがこの時点で新しいロールを作成しからのすべての既存のロールを表示、`ManageRoles.aspx`ページ。 ロールを削除するユーザーを許可します。 `Roles.DeleteRole`メソッドに 2 つのオーバー ロードがあります。

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -ロールを削除します。 *roleName*します。 ロールには、1 つまたは複数のメンバーが含まれている場合、例外がスローされます。
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -ロールを削除します。 *roleName*します。 場合*throwOnPopulateRole*は`True`役割には、1 つまたは複数のメンバーが含まれている場合、例外がスローされます。 場合*throwOnPopulateRole*は`False`、またはしないすべてのメンバーを含んでいるかどうか、ロールが削除されます。 内部的には、`DeleteRole(roleName)`メソッド呼び出し`DeleteRole(roleName, True)`します。

`DeleteRole`場合は、メソッドに例外はスローも*roleName*は`Nothing`または空の文字列場合*roleName*コンマが含まれています。 場合*roleName* 、システムに存在しません`DeleteRole`サイレント モードでは、例外を発生させずに失敗します。

GridView を拡張してみましょう`ManageRoles.aspx`に Delete ボタンであり、クリックすると、選択したロールを削除します。 最初、フィールド ダイアログ ボックスに移動し、commandfield オプションの下にある削除 ボタンを追加して GridView に Delete ボタンを追加します。 列の左端のボタンをクリックし、設定の削除を行うその`DeleteText`ロールを削除するプロパティ。


[![RoleList GridView に Delete ボタンを追加します。](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**図 7**: 削除ボタンを追加して、 `RoleList` GridView ([フルサイズの画像を表示する をクリックします](creating-and-managing-roles-vb/_static/image21.png))。


削除ボタンを追加すると、GridView の宣言型マークアップを次のようになります。

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

GridView のイベント ハンドラーを次に、作成`RowDeleting`イベント。 これは、ロールの削除 ボタンがクリックされたときにポストバック時に発生するイベントです。 イベント ハンドラーに次のコードを追加します。

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

プログラムで参照することによって、コードの先頭、 `RoleNameLabel` Web コントロールの役割の削除ボタンがクリックされた行にします。 `Roles.DeleteRole`メソッドが呼び出されてを渡して、`Text`の`RoleNameLabel`と`False`のため、かどうかに関係なく、ロールを削除するロールに関連付けられているすべてのユーザーがあります。 最後に、 `RoleList` GridView が更新されるので、単に削除された役割がグリッドに表示されなくなります。

> [!NOTE]
> ロールの削除 ボタンでは、あらゆる種類のロールを削除する前に、ユーザーから送信される確認は必要ありません。 アクションを確認する最も簡単な方法の 1 つは、クライアント側の確認 ダイアログ ボックスからです。 この手法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-vb.aspx)します。


## <a name="summary"></a>まとめ

多くの web アプリケーションでは、特定の承認規則またはのみがユーザーの特定のクラスを利用できるページ レベルの機能があります。 たとえば、管理者のみがアクセスできる web ページのセットがあります。 多くの場合でユーザーごとにこれらの承認規則を定義するのではなくは、役割に基づいたルールを定義する役立ちます。 これはユーザー管理の web ページにアクセスするには、Scott と Jisun を明示的に許可するではなく、保守しやすくなりますアプローチがある管理者ロールのメンバーは、これらのページにアクセスを許可するようにし、次を Scott と Jisun を示すために属しているユーザーとして、管理者の役割。

ロールのフレームワークでは、簡単に作成し、ロールを管理します。 このチュートリアルで使用するロールのフレームワークを構成する方法を調べる、 `SqlRoleProvider`、ロール ストアとして Microsoft SQL Server データベースを使用します。 既存のロール システムの一覧が表示され、新しいロールを作成するは、web ページおよび削除する既存のも作成しました。 以降のチュートリアルでロールベースの承認を適用する方法とユーザー ロールを割り当てる方法が表示されます。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ASP.NET 2.0 の検査のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [ASP.NET 2.0 でロール マネージャーを使用する方法](https://msdn.microsoft.com/library/ms998314.aspx)
- [ロール プロバイダー](https://msdn.microsoft.com/library/aa478950.aspx)
- [独自の web サイト管理ツールのローリング](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [技術ドキュメント、`<roleManager>`要素](https://msdn.microsoft.com/library/ms164660.aspx)
- [メンバーシップとロール マネージャー Api を使用してください。](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者には、Alicja Maziarz、Suchi 著、Teresa Murphy などがあります。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](role-based-authorization-cs.md)
> [次へ](assigning-roles-to-users-vb.md)
