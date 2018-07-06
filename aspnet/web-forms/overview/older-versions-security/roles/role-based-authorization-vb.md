---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: ロール ベースの承認 (VB) |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、ロールのフレームワークがユーザーのセキュリティ コンテキストでユーザーのロールを関連付ける方法を参照してください。 次に、ロール ベースの URL を適用する方法を確認しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: b8cf8cc5fa802eb812482b236c8b2825423027e7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390986"
---
<a name="role-based-authorization-vb"></a>ロール ベースの承認 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> このチュートリアルでは、ロールのフレームワークがユーザーのセキュリティ コンテキストでユーザーのロールを関連付ける方法を参照してください。 ロール ベースの URL 承認規則を適用する方法を調べます。 次の宣言とプログラムの手段を使用して、表示されるデータと、ASP.NET ページによって提供される機能を変更するために注目するは、します。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル URL 承認を使用して、どのようなユーザーを参照してください、特定のページのセットを指定する方法を説明しました。 内のマークアップのほんの少しだけ`Web.config`、ASP.NET ページにアクセスする認証済みユーザーのみを許可するよう指示しますでした。 または、Tito と Bob のユーザーのみが許可されたことをディクテーションまたは、Sam を除くすべての認証済みユーザーが許可されたことを示すことできます。

URL の承認だけでなくも見てきました表示されるデータにアクセスしたユーザーに基づくページによって提供される機能を制御するための宣言とプログラムの手法です。 具体的には、現在のディレクトリの内容を一覧表示するページを作成します。 すべてのユーザーがこのページを参照してくださいし、認証されたユーザーのみがファイルの内容を表示できますが、Tito のみがファイルを削除できます。

ユーザー単位のユーザーごとに承認規則を適用することは、ブックキーピングの悪夢に拡張できます。 保守しやすくなりますアプローチでは、ロール ベースの承認を使用します。 良い知らせは、承認規則を適用することのできるツールが同様に機能しているユーザー アカウントの場合と、ロールにもします。 URL 承認規則は、ユーザーの代わりにロールを指定できます。 認証および匿名ユーザーの別の出力をレンダリングするには、LoginView コントロールでは、ログイン ユーザーのロールに基づいて異なるコンテンツを表示を構成できます。 ロール API には、ログインしているユーザーのロールを決定するためのメソッドが含まれています。

このチュートリアルでは、ロールのフレームワークがユーザーのセキュリティ コンテキストでユーザーのロールを関連付ける方法を参照してください。 ロール ベースの URL 承認規則を適用する方法を調べます。 次の宣言とプログラムの手段を使用して、表示されるデータと、ASP.NET ページによって提供される機能を変更するために注目するは、します。 それでは、始めましょう!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>役割の理解する方法が関連付けられているユーザーのセキュリティ コンテキスト

ASP.NET パイプラインに要求が入るたびに、要求元を識別する情報を含む、セキュリティ コンテキストに関連付けられます。 フォーム認証を使用する場合は、認証チケットが id トークンとして使用されます。 説明したように、 <a id="_msoanchor_2"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)と<a id="_msoanchor_3"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)チュートリアルでは、`FormsAuthenticationModule`中には、要求元の身元を確認する責任を負いますが、 [ `AuthenticateRequest` イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

有効な有効期限のない認証チケットが見つかった場合、`FormsAuthenticationModule`要求元の id を確認するためにデコードします。 新たに作成、`GenericPrincipal`オブジェクトし、これを割り当てます、`HttpContext.User`オブジェクト。 などのプリンシパルの目的は、 `GenericPrincipal`、認証されたユーザーの名前と内容を識別するためには、ロールに属しているユーザー。 この目的は明らかにすべてのプリンシパル オブジェクトがあるという事実によって、`Identity`プロパティおよび`IsInRole(roleName)`メソッド。 `FormsAuthenticationModule`、ただしの役割の情報を記録に興味がないと、`GenericPrincipal`オブジェクトが作成されますが、ロールを指定しません。

ロールのフレームワークが有効になっている場合、 [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP モジュールの後の手順を`FormsAuthenticationModule`中に認証されたユーザーのロールを識別し、 [ `PostAuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)を後に起動、`AuthenticateRequest`イベント。 要求が認証されたユーザーの場合、`RoleManagerModule`上書き、`GenericPrincipal`によって作成されたオブジェクト、`FormsAuthenticationModule`と置き換えられます、 [ `RolePrincipal`オブジェクト](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)します。 `RolePrincipal`クラスでは、ロール API を使用して、ユーザーが所属するどのような役割を決定します。

図 1 は、フォーム認証、ロールのフレームワークを使用する場合の ASP.NET パイプライン ワークフローを示しています。 `FormsAuthenticationModule`が最初に実行、自分の認証チケットを使用してユーザーを識別し、新たに作成します`GenericPrincipal`オブジェクト。 次に、`RoleManagerModule`担当し、上書き、`GenericPrincipal`オブジェクトを`RolePrincipal`オブジェクト。

匿名ユーザーがどちらもサイトを訪問している場合、`FormsAuthenticationModule`も`RoleManagerModule`プリンシパル オブジェクトを作成します。


[![フォーム認証、ロールのフレームワークを使用する場合は、認証されたユーザーの ASP.NET パイプライン イベント](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**図 1**: The の ASP.NET パイプライン イベントの認証されたユーザーとフォーム認証の使用とロール フレームワーク ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image3.png))。


### <a name="caching-role-information-in-a-cookie"></a>クッキーにキャッシュのロール情報

`RolePrincipal`オブジェクトの`IsInRole(roleName)`メソッド呼び出し`Roles`します。`GetRolesForUser` ユーザーがのメンバーであるかどうかを判断するために、ユーザーのロールを取得する*roleName*します。 使用する場合、 `SqlRoleProvider`、ロール ストア データベースにクエリをこの結果します。 ロール ベースの URL 承認規則を使用する場合、`RolePrincipal`の`IsInRole`ロール ベースの URL 承認規則によって保護されているページに対するすべての要求でメソッドが呼び出されます。 要求ごとに、データベース内のロール情報を参照する必要があるのではなく、`Roles`フレームワークには、cookie にユーザーのロールをキャッシュするためのオプションが含まれています。

Cookie にユーザーのロールをキャッシュするロール framework が構成されている場合、 `RoleManagerModule` ASP.NET パイプラインの中に、cookie を作成します[`EndRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)します。 この cookie は後続の要求で使用、 `PostAuthenticateRequest`、ときに、`RolePrincipal`オブジェクトが作成されます。 Cookie のデータが解析され、節約、ユーザーのロールの作成に使用される cookie が有効で切れていない場合、`RolePrincipal`への呼び出しを行うことから、`Roles`クラスは、ユーザーのロールを決定します。 図 2 は、このワークフローを示しています。


[![パフォーマンスを向上させるために、Cookie にユーザーのロール情報を格納できます。](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**図 2**: ユーザーのロール情報に格納できるパフォーマンスを向上させる Cookie ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image6.png))。


既定では、ロールのキャッシュ cookie メカニズムは無効になります。 有効にすることができます、 `<roleManager>`; 内の構成マークアップ`Web.config`します。 使用して説明した、 [ `<roleManager>`要素](https://msdn.microsoft.com/library/ms164660.aspx)でロール プロバイダーを指定する、 <a id="_msoanchor_4"> </a> [*ロールの管理の作成と*](creating-and-managing-roles-vb.md)チュートリアルでは、アプリケーションのこの要素が必要であるため`Web.config`ファイル。 属性としてロール キャッシュ cookie の設定が指定されて、 `<roleManager>`。 表 1 にまとめられている要素とは。

> [!NOTE]
> 表 1 に示す構成設定は、結果として得られるロール キャッシュのクッキーのプロパティを指定します。 Cookie、そのしくみ、およびそのさまざまなプロパティの詳細については、読み取る[この Cookie チュートリアル](http://www.quirksmode.org/js/cookies.html)します。


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>説明</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              クッキーのキャッシュを使用するかどうかを示すブール値。 既定値は `false` です。                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     ロールのキャッシュのクッキーの名前。 既定値は"。ASPXROLES"です。                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                ロール名のクッキーのパス。 Path 属性は、開発者は特定のディレクトリ階層のクッキーのスコープを制限できます。 既定値は「/」、ドメインへの要求に認証チケットのクッキーを送信するブラウザーに通知します。                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               ロールのキャッシュのクッキーを保護するどの手法を使用することを示します。 使用可能な値: `All` (既定)。`Encryption`;`None`; と`Validation`します。 手順 3. に戻って、 <a id="_anchor_5"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)これらの保護レベルの詳細についてはチュートリアル。                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   認証クッキーを送信する SSL 接続が必要かどうかを示すブール値。 既定値は `false` です。                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  ユーザーが 1 つのセッション中にサイトを訪問する cookie のタイムアウトが毎回をリセットするかどうかを示すブール値。 既定値は `false` です。 ときにこの値は関連のみ`createPersistentCookie`に設定されている`true`します。                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         認証チケットのクッキーの有効期限が切れるまでの分、時間を指定します。 既定値は `30` です。 ときにこの値は関連のみ`createPersistentCookie`に設定されている`true`します。                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   ロールのキャッシュのクッキーがセッション cookie または永続的なクッキーがかどうかを指定するブール値。 場合`false`(既定)、セッション クッキーを使用すると、ブラウザーが閉じられたときに、削除します。 場合`true`、永続的なクッキーが使用されます。 有効期限が切れる`cookieTimeout`分が作成された後の、または後の値に応じて、前の訪問数`cookieSlidingExpiration`します。                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Cookie のドメインの値を指定します。 既定値は、発行 (www.yourdomain.com) などがドメインを使用するブラウザーにより空の文字列。 クッキーが、この場合、<strong>いない</strong>admin.yourdomain.com などのサブドメインに要求を行う場合に送信されます。 すべてのサブドメインに渡される cookie が必要な場合は、カスタマイズする必要があります。、 `domain` "yourdomain.com"に設定する属性。                                                                                                                                                 |
|    `maxCachedResults`     | クッキーにキャッシュされるロール名の最大数を指定します。 既定値は 25 です。 `RoleManagerModule`に属しているユーザーの cookie を作成しない複数の`maxCachedResults`ロール。 その結果、`RolePrincipal`オブジェクトの`IsInRole`メソッドを使用して、`Roles`クラスは、ユーザーのロールを決定します。 理由`maxCachedResults`存在は多くのユーザー エージェントは 4,096 バイトを超える cookie を許可されていないためです。 したがってこの上限は、このサイズ制限を超えるの可能性を低減するものでは。 非常に長いロール名がある場合より小さいを指定するたい`maxCachedResults`値; contrariwise、非常に短いロール名があれば、することがおそらくこの値を大ききます。 |

**表 1.**: ロール キャッシュのクッキーの構成オプション

非永続的なロールのキャッシュのクッキーを使用するようにアプリケーションを構成しましょう。 これを行うには、更新、`<roleManager>`要素`Web.config`を cookie に関連する次の属性を含めます。

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

更新して、 `<roleManager>`; 3 つの属性を追加することで要素: `cacheRolesInCookie`、 `createPersistentCookie`、および`cookieProtection`します。 設定して`cacheRolesInCookie`に`true`、`RoleManagerModule`要求ごとに、ユーザーのロール情報を参照する必要がなく、cookie にユーザーのロールが自動的にキャッシュようになりました。 明示的に設定されている、`createPersistentCookie`と`cookieProtection`属性を`false`と`All`、それぞれします。 技術的には、永続的な cookie を使用していないことと cookie は、どちらも暗号化され、検証をオフに、既定値にだけに割り当てましたが、私に保存することを明示的にあるため、これらの属性の値を指定する必要はありませんでした。

GridView を ObjectDataSource にバインドされているユーザーのブラウザーでページ上の空白の領域でどのマークアップもレンダリングしません。 今後は、ロールのフレームワークでは、cookie にユーザーのロールがキャッシュされます。 ユーザーのブラウザーが cookie をサポートしていないか、cookie が削除されたか、紛失、何らかの形ではありません – たいした、`RolePrincipal`オブジェクトは単を使用して、`Roles`ない cookie (または、無効または期限切れの 1 つ) は、使用可能な場合のクラス。

> [!NOTE]
> Microsoft の Patterns &amp; Practices グループは、永続的なロールのキャッシュの cookie を使用してを抑制します。 ハッカーが有効なユーザーの cookie へのアクセスを何らかの形でできる場合は、ロールのキャッシュのクッキーを所有しているはロールのメンバーシップを証明するための十分なであるため、ユーザー権限を借用できます。 ユーザーのブラウザーで cookie が永続化される場合、この問題が発生する可能性が増加します。 このセキュリティの推奨事項とその他のセキュリティに関する注意事項の詳細についてを参照してください、 [ASP.NET 2.0 のセキュリティの質問リスト](https://msdn.microsoft.com/library/ms998375.aspx)します。


## <a name="step-1-defining-role-based-url-authorization-rules"></a>手順 1: ロール ベースの URL 承認規則を定義します。

説明したように、 <a id="_msoanchor_6"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルでは、一連のユーザーでのユーザーまたはロール-ロール別のページへのアクセスを制限するための手段を提供する URL 承認基準です。 URL 承認規則が記述された`Web.config`を使用して、 [ `<authorization>`要素](https://msdn.microsoft.com/library/8d82143t.aspx)で`<allow>`と`<deny>`子要素。 前のチュートリアルで説明されているユーザーに関連する承認規則だけでなく各`<allow>`と`<deny>`子要素を含めることもできます。

- 特定のロール
- ロールのコンマ区切りの一覧

たとえば、URL の承認規則は、管理者と管理者の役割のユーザーへのアクセス許可しますが、他のすべてのユーザーへのアクセスを拒否します。

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>`上記のマークアップ内の要素は、管理者と管理者のロールが許可されることを示す、 `<deny>`; 要素に指示する*すべて*ユーザーが拒否されます。

アプリケーションを構成しましょうように、 `ManageRoles.aspx`、 `UsersAndRoles.aspx`、および`CreateUserWizardWithRoles.aspx`ページは、管理者ロールのユーザーにアクセスできる、だけ中に、`RoleBasedAuthorization.aspx`ページがすべての訪問者にアクセスできる状態します。

これを行うには、追加することで開始、`Web.config`ファイルを`Roles`フォルダー。


[![ロールのディレクトリに Web.config ファイルを追加します。](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**図 3**: 追加、`Web.config`ファイルを`Roles`ディレクトリ ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image9.png))。


次の構成マークアップを次に、追加`Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>`内の要素、`<system.web>`セクションでは、管理者ロールのユーザーのみが ASP.NET のリソースを入手できますを示します、`Roles`ディレクトリ。 `<location>`要素は、別の URL 承認規則のセットを定義、 `RoleBasedAuthorization.aspx`  ページで、すべてのユーザー、ページにアクセスできるようにします。

変更を保存した後に`Web.config`管理者ロールではないユーザーとしてログインして、保護されたページのいずれかにアクセスするとします。 `UrlAuthorizationModule` ; 要求されたリソースにアクセスするためのアクセス許可がないことを検出、その結果、`FormsAuthenticationModule`ログイン ページにリダイレクトされます。 ログイン ページで、リダイレクトすることは、`UnauthorizedAccess.aspx`ページ (図 4 参照)。 この最終的にリダイレクトするログイン ページから`UnauthorizedAccess.aspx`コードのステップ 2 でのログイン ページに追加しましたが発生する、 <a id="_msoanchor_7"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル。 具体的には、ログイン ページは、認証されたユーザーに自動的をリダイレクト`UnauthorizedAccess.aspx`、クエリ文字列が含まれている場合、`ReturnUrl`パラメーターがこのパラメーターとして、ユーザーはページを表示しようとした後、ログイン ページに到着したことを示します表示する権限。


[![管理者ロールのユーザーのみが保護されたページを表示できます。](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**図 4**: 管理者ロールのユーザーのみが保護されているページを表示できます ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image12.png))。


ログオフし、管理者ロール内にあるユーザーとしてログインします。 これで 3 つの保護されたページを表示するはずです。


[![Tito はことができます、UsersAndRoles.aspx ページのため、彼は、管理者ロールを参照してください。](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**図 5**: Tito がアクセスできる、`UsersAndRoles.aspx`ページのため、彼は、管理者ロール ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image15.png))。


> [!NOTE]
> 役割またはユーザー – – URL 承認規則を指定するときにすることが重要に規則を最上部から、同時に分析された 1 つは、注意してください。 場合によっては、一致が見つかったで、一致が見つかるとすぐに、ユーザーがアクセス許可または拒否する`<allow>`または`<deny>`要素。 **一致が検出されない場合、ユーザーがアクセスを許可します。** その結果、1 つまたは複数のユーザー アカウントへのアクセスを制限する場合を使用することが不可欠な`<deny>`URL 承認の構成の最後の要素としての要素。 **URL 承認規則が含まれない場合、**`<deny>`**要素では、すべてのユーザー アクセスが許可されます。** URL の承認規則を分析する方法の詳細についてを参照、"方法を見て、`UrlAuthorizationModule`承認規則を使用してアクセス許可または拒否"のセクション、 <a id="_msoanchor_8"> </a> [ *ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル。


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>手順 2: 現在ログインしているユーザーのロールに基づいて機能を制限します。

URL の承認はその状態をどの id ルール粗い承認を指定することが容易は許可されており、特定のページ (またはフォルダーとそのサブフォルダー内のすべてのページ) を表示できないものは拒否されます。 ただし、場合によっては、ページを参照してください、ページの機能がアクセスしたユーザーの役割に基づいたを制限するすべてのユーザーを許可することがあります。 これにより、ユーザーの役割に基づいたまたは特定のロールに属しているユーザーに追加の機能を提供するデータを表示または非表示が伴う場合があります。

宣言的またはプログラム (または、2 つの組み合わせを通じて)、このような細かい粒度のロールベースの承認規則を実装できます。 次のセクションは、LoginView コントロールを使用して宣言型細かく承認を実装する方法が表示されます。 次は、プログラムによる手法について説明します。 細かく承認規則を適用することについて考えることができます、前に、まず必要があります機能を持つは、それにアクセスしたユーザーの役割によって異なります。 ページを作成します。

GridView でシステム内のすべてのユーザー アカウントを一覧表示するページを作成しましょう。 GridView には、各ユーザーのユーザー名、電子メール アドレス、最後のログイン日時、およびユーザーについてのコメントが含まれます。 に加えて、各ユーザーの情報を表示するには、GridView は編集を含めるし、機能を削除します。 このページを作成、編集と最初にされすべてのユーザーに使用できる機能の削除します。 「The LoginView コントロールを使用して」と「プログラムで制限する機能」のセクションでは、有効またはアクセスしたユーザー ロールに基づくこれらの機能を無効にする方法が表示されます。

> [!NOTE]
> 構築するためには ASP.NET ページでは、ユーザー アカウントを表示するのに GridView コントロールを使用します。 このチュートリアル シリーズは、フォーム認証、承認、ユーザー アカウント、およびロールに重点を置いています、以降しない GridView コントロールの内部動作を説明する時間をかけるにします。 このチュートリアルでは、このページをセットアップするための手順では、中の特定の選択を行った理由または影響の特定のプロパティが表示される出力が詳細には掘り下げてされません。 GridView コントロールの詳しく調べる場合は、チェック アウト、  *[ASP.NET 2.0 でデータを扱う](../../data-access/index.md)* チュートリアル シリーズです。


開いて開始、`RoleBasedAuthorization.aspx`ページで、`Roles`フォルダー。 GridView をドラッグしてページから、デザイナーとセットにその`ID`に`UserGrid`します。 すぐに呼び出すコードを記述しましたが、`Membership`します。`GetAllUsers` メソッドと、その結果をバインド`MembershipUserCollection`GridView にオブジェクト。 `MembershipUserCollection`が含まれています、`MembershipUser`システム内の各ユーザー アカウントのオブジェクト`MembershipUser`オブジェクトのようなプロパティがある`UserName`、`Email`、`LastLoginDate`など。

私たちは、ユーザー アカウントをグリッドにバインドするコードを記述する前に、GridView のフィールドを定義してみましょう最初。 GridView のスマート タグからのフィールド ダイアログを起動する「列の編集」リンクをクリックします。 ボックス (図 6 参照)。 ここでは、左下隅に「自動生成フィールド」チェック ボックスをオフにします。 編集および削除の機能が含まれて、CommandField を追加し、設定には、この GridView たいので、`ShowEditButton`と`ShowDeleteButton`プロパティを True にします。 次に、表示するための 4 つのフィールドを追加、 `UserName`、 `Email`、 `LastLoginDate`、および`Comment`プロパティ。 2 つの読み取り専用プロパティを BoundField を使用して (`UserName`と`LastLoginDate`) と 2 つの編集可能なフィールドの TemplateFields (`Email`と`Comment`)。

最初の BoundField 表示、`UserName`プロパティです。 セット、`HeaderText`と`DataField`プロパティが"UserName"にします。 このフィールドは編集できません、ため、設定、`ReadOnly`プロパティを True にします。 構成、 `LastLoginDate` BoundField を設定してその`HeaderText`「前回のログイン」にし、その`DataField`"LastLoginDate"にします。 みましょうこの BoundField の出力の書式設定 (日付と時刻) 代わりに日付のみが表示されます。 これを実現する設定のこの BoundField の`HtmlEncode`プロパティを False に、その`DataFormatString`プロパティを"{0:d}"。 設定しても、`ReadOnly`プロパティを True にします。

設定、 `HeaderText` "Email"と「コメント」を 2 つの TemplateFields のプロパティ。


[![GridView のフィールドは、[フィールド] ダイアログ ボックスで構成できます。](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**図 6**: GridView のフィールドでくように構成からのフィールド ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image18.png))。


ここで定義する必要があります、`ItemTemplate`と`EditItemTemplate`"Email"と「コメント」TemplateFields します。 各ラベルの Web コントロールを追加、`ItemTemplates`バインドと、`Text`プロパティを`Email`と`Comment`プロパティでは、それぞれします。

"Email"TemplateField、という名前のテキスト ボックスを追加`Email`にその`EditItemTemplate`バインドとその`Text`プロパティを`Email`双方向データ バインドを使用するプロパティ。 RequiredFieldValidator と RegularExpressionValidator を追加、`EditItemTemplate`訪問者が電子メールのプロパティの編集が有効な電子メール アドレスを入力したことを確認します。 「コメント」TemplateField、という名前の複数行テキスト ボックスを追加`Comment`にその`EditItemTemplate`します。 設定、テキスト ボックスの`Columns`と`Rows`プロパティを 40 と 4 は、それぞれ、し、バインド、`Text`プロパティを`Comment`双方向データ バインドを使用するプロパティ。

これら TemplateFields を構成した後、宣言型マークアップは次のようになります。

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

編集またはそのユーザーを把握する必要があります。 ユーザー アカウントを削除するときに`UserName`プロパティの値。 GridView の設定`DataKeyNames`プロパティが"UserName"にこの情報を GridView のを通じて利用できるように`DataKeys`コレクション。

最後に、ページに ValidationSummary コントロールを追加し、設定、`ShowMessageBox`プロパティを True に、その`ShowSummary`プロパティを False にします。 これらの設定で、ValidationSummary は、ユーザーが見つからないか無効な電子メール アドレスを持つユーザー アカウントの編集を試みる場合にクライアント側の警告を表示します。

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

このページの宣言型マークアップが完了しましたようになりました。 次のタスクでは、ユーザー アカウントのセットを GridView にバインドします。 という名前のメソッドを追加`BindUserGrid`を`RoleBasedAuthorization.aspx`ページの分離コード クラスをバインドする、`MembershipUserCollection`によって返される`Membership.GetAllUsers`を`UserGrid`GridView。 このメソッドを呼び出し、`Page_Load`最初のページ アクセス時にイベント ハンドラー。

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

このコードでは、ブラウザーを使用してページを参照してください。 図 7 に示す、システムの各ユーザー アカウントに関する情報を一覧表示する GridView が表示されます。


[![UserGrid GridView、システムで各ユーザーについての情報を一覧表示します。](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**図 7**: `UserGrid` GridView の一覧情報の各システム内のユーザー ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image21.png))。


> [!NOTE]
> `UserGrid` GridView には、すべての非ページ インターフェイスでユーザーが一覧表示されます。 この単純なグリッド インターフェイスは、シナリオに最適な数十個または複数のユーザーがいくつか。 1 つのオプションでは、ページングを有効にする GridView を構成します。 `Membership.GetAllUsers`メソッドに 2 つのオーバー ロードがあります。 入力パラメーターを受け取らず、すべてのユーザーとを受け取って、ページのインデックスと、ページ サイズの整数値で指定されたユーザーのサブセットのみを返す 1 つを返します。 ユーザー アカウントの正確なサブセットだけを返すのでより効率的にページング、ユーザーに 2 番目のオーバー ロードを使用できるのではなく*すべて*にします。 何千ものユーザー アカウントがあれば、フィルターに基づくインターフェイスをそれらのユーザー インスタンスのユーザー名を持つが、選択した文字で始まるだけを表示するいずれかを検討する可能性があります。 [ `Membership.FindUsersByName`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)はフィルターに基づくユーザー インターフェイスを構築するために最適です。 今後のチュートリアルでこのようなインターフェイスの構築を紹介します。


GridView コントロールでは、組み込みの編集と SqlDataSource や ObjectDataSource など、適切に構成されたデータ ソース コントロールにバインドするコントロールとサポートの削除を提供します。 `UserGrid` GridView、ただし、そのデータをプログラムでバインドにはそのため、これら 2 つのタスクを実行するコードを作成する必要があります。 具体的には、GridView のイベント ハンドラーを作成する必要が`RowEditing`、 `RowCancelingEdit`、 `RowUpdating`、および`RowDeleting`ビジターを GridView をクリックしたときに発生するイベントは、編集、Cancel、Update、またはボタンを削除します。

GridView のためのイベント ハンドラーを作成して開始`RowEditing`、 `RowCancelingEdit`、および`RowUpdating`イベントし、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing`と`RowCancelingEdit`イベント ハンドラーは、GridView を設定するだけです`EditIndex`プロパティと、グリッドへのアカウントのユーザーの一覧からの再バインドします。 興味深い処理が行われる、`RowUpdating`イベント ハンドラー。 このイベント ハンドラーがようにすることによって、データが有効では、取得が開始、`UserName`から編集済みのユーザー アカウントの値、`DataKeys`コレクション。 `Email`と`Comment`で 2 つの TemplateFields のテキスト ボックス`EditItemTemplate`s がプログラムで参照されます。 その`Text`プロパティには、編集済みの電子メール アドレスとコメントが含まれます。

まず呼び出しにはユーザーの情報を取得する必要があります、メンバーシップ API を介してユーザー アカウントを更新するには`Membership.GetUser(userName)`します。 返された`MembershipUser`オブジェクトの`Email`と`Comment`プロパティは、その編集インターフェイスから 2 つのテキスト ボックスに入力された値で更新します。 その変更を保存してへの呼び出しで最後に、 [ `Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)します。 `RowUpdating`事前編集インターフェイスに GridView を元に戻すイベント ハンドラーを完了します。

次に、作成、 `RowDeleting` RowDeleting イベント ハンドラーと、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

上記のイベント ハンドラーを取得することで開始、 `UserName` GridView から値`DataKeys`コレクション、この`UserName`のメンバーシップ クラスに値が渡される、 [ `DeleteUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)します。 `DeleteUser`メソッドは、(ロールなどこのユーザーが属する) 関連のメンバーシップ データを含む、システムからユーザー アカウントを削除します。 ユーザーの削除、グリッドの後に`EditIndex`(この場合、ユーザーでは、別の行が編集モードのときに、削除がクリックされた) が、-1 に設定し、`BindUserGrid`メソッドが呼び出されます。

> [!NOTE]
> Delete ボタンでは、あらゆる種類のユーザー アカウントを削除する前に、ユーザーから送信される確認は必要ありません。 誤って削除されているアカウントの可能性を軽減するためにユーザーの確認のいくつかの形式を追加することをお勧めします。 アクションを確認する最も簡単な方法の 1 つは、クライアント側の確認 ダイアログ ボックスからです。 この手法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-vb.aspx)します。


このページに期待どおりに機能することを確認します。 任意のユーザー アカウントを削除するだけでなく、すべてのユーザーの電子メール アドレスと、コメントを編集することができます。 以降、`RoleBasedAuthorization.aspx`ページにすべてのユーザーにアクセスできる、– も匿名の訪問者: すべてのユーザーは、このページを参照してください、編集、およびユーザー アカウントを削除します。 監督者および管理者のロール内のユーザーのみがユーザーの電子メール アドレスと、コメントを編集でき、管理者のみがユーザー アカウントを削除できるように、このページを更新してみましょう。

「、LoginView コントロールを使用して」セクションを見ます LoginView コントロールを使用するユーザーのロールに固有の説明を表示します。 このページにアクセスし、管理者ロール内のユーザー場合は、手順で編集し、ユーザーを削除する方法を示します。 管理者ロールのユーザーには、このページに達するは手順のユーザーを編集する方法を示します。 編集またはユーザー アカウント情報を削除することはできませんするを説明するメッセージを表示しますが、訪問者が匿名または監督者または管理者ロールでは場合、。 「プログラムで制限する機能」セクションではプログラムで表示またはユーザーのロールに基づいて、編集、削除ボタンを非表示にするコードを記述します。

### <a name="using-the-loginview-control"></a>LoginView コントロールを使用します。

過去のチュートリアルできたよう、LoginView コントロールは、認証と匿名ユーザーの異なるインターフェイスを表示するために便利ですが、LoginView コントロールがユーザーのロールに基づいて異なるマークアップを表示することもできます。 LoginView コントロールを使用して、アクセスしたユーザーのロールに基づいて異なる命令を表示しましょう。

上記の LoginView を追加することで開始、 `UserGrid` GridView。 先ほど説明した、LoginView コントロールでは 2 つの組み込みテンプレート:`AnonymousTemplate`と`LoggedInTemplate`します。 編集または任意のユーザー情報を削除することはできませんするユーザーに通知するこれらのテンプレートの両方で、簡単なメッセージを入力します。

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

加え、`AnonymousTemplate`と`LoggedInTemplate`、LoginView コントロールを含めることができます*Rolegroup*、ロール固有のテンプレートであります。 各 RoleGroup には、1 つのプロパティが含まれています。 `Roles`、に適用されます、RoleGroup ロールを指定します。 `Roles` (「管理者」) のような 1 つのロールまたはロール (「管理者、スーパーバイザー」) などのコンマ区切りのリストにプロパティを設定できます。

Rolegroup を管理するには、RoleGroup コレクション エディターを表示するコントロールのスマート タグから"Rolegroup 編集"リンクをクリックします。 2 つの新しい Rolegroup を追加します。 設定の最初の RoleGroup の`Roles`プロパティを"Administrators"、第 2 の「管理者」にします。


[![LoginView の役割に固有のテンプレート RoleGroup コレクション エディターを使用を管理します。](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**図 8**: LoginView の役割に固有のテンプレートを通じて、RoleGroup コレクション エディターを管理 ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image24.png))。


RoleGroup コレクション エディターを閉じるには、[ok] をクリックします。これにより、更新に含める LoginView の宣言型マークアップを`<RoleGroups>`セクションで、 `<asp:RoleGroup>` RoleGroup コレクション エディターが各 RoleGroup の子要素に定義されています。 さらに、「ビュー」のドロップダウン リスト LoginView のスマート タグ - 最初に記載されている、`AnonymousTemplate`と`LoggedInTemplate`– が追加された Rolegroup も含まれています。

管理者ロールのユーザーが管理者ロールのユーザーが編集および削除するための手順を示すように、ユーザー アカウントを編集する方法については表示されているように、Rolegroup を編集します。 これらの変更を行った後 LoginView の宣言型マークアップは、次のようになります。

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

これらの変更を行った後は、ページを保存し、ブラウザーを使用しを参照してください。 まず、匿名ユーザーとして、ページを参照してください。 メッセージを表示するか、"しないにログインしているシステム。 そのためことはできません編集または削除するユーザー情報。" 認証されたユーザーが管理者と管理者ロールのどちらである 1 つとしてログインします。 この時点"の監督者または管理者ロールのメンバーではないメッセージを表示する必要があります。 そのためことはできません編集または削除するユーザー情報。"

次に、管理者ロールのメンバーであるユーザーとしてログインします。 今度はスーパーバイザー ロール固有はずのメッセージ (図 9 参照)。 (図 10 参照) メッセージのことがわかります管理者役割に固有のロールの管理者のユーザーとしてログインする場合。


[![Bruce は、スーパーバイザー ロール固有のメッセージが表示されます。](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**図 9**: Bruce スーパーバイザー ロール固有のメッセージが表示されます ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image27.png))。


[![Tito は、管理者役割に固有のメッセージが表示されます。](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**図 10**: Tito 管理者役割に固有のメッセージが表示されます ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image30.png))。


図 9 のスクリーン ショットは、10 個表示として、LoginView では、複数のテンプレートが適用される場合でも、1 つのテンプレートがのみレンダリングします。 ユーザーは Bruce と Tito 両方記録はまだ、LoginView が一致する RoleGroup のみを表示および not、`LoggedInTemplate`します。 さらに、管理者と管理者の両方のロールに属している Tito まだ LoginView コントロール レンダリング スーパーバイザーではなく管理者役割に固有のテンプレート 1 つ。

図 11 は、LoginView コントロールをレンダリングするには、どのようなテンプレートを決定するために使用するワークフローを示しています。 1 つ以上の RoleGroup が指定された場合に、LoginView テンプレートがレンダリングことに注意してください、*最初*RoleGroup と一致します。 つまり、最初の RoleGroup としてスーパーバイザー RoleGroup と 2 つ目として管理者を配置しましたが場合、Tito このページを表示するときに彼はメッセージが表示されます、監督者。


[![表示するには、どのようなテンプレートを決定するため、LoginView コントロールのワークフロー](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**図 11**:、LoginView コントロールのワークフローを決定するものにするテンプレートのレンダリング ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image33.png))。


### <a name="programmatically-limiting-functionality"></a>プログラムで機能を制限します。

LoginView コントロールでは、ページにアクセスするユーザーの役割に基づくさまざまな手順が表示されます、編集のキャンセル ボタンはすべてに表示されたままです。 プログラムで匿名の訪問者の監督者でも、管理者ロールのユーザーに、編集、削除ボタンを非表示にする必要があります。 すべての管理者ではないユーザーの削除 ボタンを非表示にする必要があります。 これを実現するために少し [commandfield] の編集と削除の Linkbutton とセットをプログラムで参照するコードの記述は、`Visible`プロパティ`False`のために必要な場合は、します。

[Commandfield] コントロールをプログラムで参照する最も簡単な方法では、まず、テンプレートに変換します。 これを行うには、GridView のスマート タグから「列の編集」リンクをクリックして、「このフィールドを TemplateField に変換」リンクをクリックして、現在のフィールドの一覧から、[commandfield] を選択します。 TemplateField に、[commandfield] になります、`ItemTemplate`と`EditItemTemplate`します。 `ItemTemplate`編集と削除の Linkbutton 中に含まれています、 `EditItemTemplate` Linkbutton のキャンセルと更新プログラムを格納します。


[![[Commandfield] を TemplateField に変換します。](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**図 12**: 変換を TemplateField に [commandfield] ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image36.png))。


編集および削除の Linkbutton で更新、`ItemTemplate`で、設定、`ID`プロパティの値を`EditButton`と`DeleteButton`、それぞれします。

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

GridView 列挙内のレコードでデータが GridView にバインドされるたびにその`DataSource`プロパティし、対応する生成`GridViewRow`オブジェクト。 各`GridViewRow`オブジェクトが作成される、`RowCreated`イベントが発生します。 承認されていないユーザーの編集、削除ボタンを非表示にするためにこのイベントのイベント ハンドラーを作成し、編集と削除のある、設定をプログラムで参照する必要があります。 その`Visible`プロパティに応じて。

イベント ハンドラーを作成、`RowCreated`イベントと、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

注意、`RowCreated`イベントの発生*すべて*ヘッダー、フッター、ページのインターフェイスおよびなどを含め、GridView の行のできます。 のみを扱っている編集モードではなくデータ行 (編集モードでの行に編集および削除の代わりに更新し、[キャンセル] ボタンがあるため) 場合に、編集および削除の Linkbutton をプログラムで参照します。 このチェックはによって処理される、`If`ステートメント。

編集および削除の Linkbutton が参照されている場合は編集モードではないデータ行を扱っているとその`Visible`によって返されるブール値に基づいてプロパティが設定、`User`オブジェクトの`IsInRole(roleName)`メソッド。 `User`オブジェクトが参照によって作成されたプリンシパル、 `RoleManagerModule`。 結局のところ、、`IsInRole(roleName)`メソッドに、現在のユーザーが属しているかどうかを決定するロール API を使用して*roleName*します。

> [!NOTE]
> 使用ロール クラスを直接呼び出しを置き換える`User.IsInRole(roleName)`を呼び出して、 [ `Roles.IsUserInRole(roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)します。 プリンシパルのオブジェクトを使用することにしました`IsInRole(roleName)`メソッドをこの例ではロール API を直接使用するよりも効率的であります。 このチュートリアルの前半では、cookie にユーザーのロールをキャッシュするロール マネージャーを構成します。 これは、cookie のデータがキャッシュされたときにのみ使用されますが、プリンシパルの`IsInRole(roleName)`メソッドが呼び出されます。 ロール API への直接呼び出しは、ロール ストアへのトリップを常に関連します。 ロールがクッキーにキャッシュがいない場合でも、プリンシパル オブジェクトを呼び出す`IsInRole(roleName)`メソッドは、結果をキャッシュして要求時に初めてのときに呼び出されるため方が効率的です。 その一方で、ロール API は、任意のキャッシュとは行いません。 `RowCreated`イベントは、GridView では、すべての行に対して 1 回発生を使用して`User.IsInRole(roleName)`一方では、ロール ストアへの 1 つだけの乗車`Roles.IsUserInRole(roleName)`が必要です*N*の乗車場所*N*はグリッドに表示されるユーザー アカウントの数。


[編集] ボタンの`Visible`プロパティに設定されて`True`場合は、管理者または管理者ロールにこのページにアクセスするユーザーがそれ以外の場合に設定されて`False`します。 削除ボタンの`Visible`プロパティに設定されて`True`ユーザーが管理者ロールの場合のみです。

ブラウザーからこのページをテストします。 匿名の訪問者、または、監督者でも、管理者であるユーザーとして、ページにアクセスする場合、[commandfield] は空です。まだが存在するが、編集、または削除することがなくシン シルバーとしてボタンします。

> [!NOTE]
> [Commandfield] を非表示にすることはまったくと非監督者と管理者以外がページにアクセスします。 ままを演習として、リーダーの。


[![非管理者と管理者以外のユーザーの編集と削除ボタンを非表示します。](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**図 13**: 非監督者と非管理者の編集と削除ボタンを非表示 ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image39.png))。


管理者ロールに (ただし、管理者ロールにありません) に属しているユーザーはアクセスすると、彼と編集 ボタンが表示されます。


[![[削除] ボタンが非表示の編集] ボタンはスーパーバイザーに使用可能が、](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**図 14**: 中に、[編集] ボタンはスーパーバイザーに使用可能で、[削除] ボタンが非表示 ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image42.png))。


管理者アクセスすると、編集、削除の各ボタンにアクセスしています。


[![編集および削除ボタンは使用可能な管理者向け](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**図 15**: [編集と削除ボタンは使用可能な管理者 ([フルサイズの画像を表示する] をクリックします](role-based-authorization-vb/_static/image45.png))。


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>手順 3: クラスおよびメソッドへのロールベースの承認規則を適用します。

限定して手順 2. で監督者および管理者の役割のユーザーに機能を編集し、削除の管理者にのみ機能します。 これは、プログラムによる方法で承認されていないユーザーに関連付けられているユーザー インターフェイス要素を非表示にしていました。 このようなメジャーでは、未認証のユーザーが特権操作を実行することはするが保証されません。 後で追加されるユーザー インターフェイス要素または承認されていないユーザーの非表示に忘れての可能性があります。 または、ハッカーが他の必要なメソッドを実行する ASP.NET ページを取得する方法を検出可能性があります。

機能の特定の部分を未認証のユーザーがアクセスできないことを確認する簡単な方法は、そのクラスまたはメソッドを修飾するため、 [ `PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)します。 .NET ランタイム クラスを使用してまたはそのメソッドの 1 つ実行と、現在のセキュリティ コンテキストが権限を持っていることを確認するを確認します。 `PrincipalPermission`属性は、これらの規則を定義できますメカニズムを提供します。

使用したところ、`PrincipalPermission`属性に戻り、 <a id="_msoanchor_9"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル。 具体的には、GridView のための方法を説明しました`SelectedIndexChanged`と`RowDeleting`イベント ハンドラーがでしたのみ実行できるようにして認証されたユーザーと Tito、それぞれします。 `PrincipalPermission`の役割を持つ属性がうまく機能します。

使用する例を見てみましょう、 `PrincipalPermission` GridView の属性`RowUpdating`と`RowDeleting`承認されていないユーザーの実行を禁止するためのイベント ハンドラー。 実行する必要がありますすべては、各関数の定義上、適切な属性を追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

属性、`RowUpdating`イベント ハンドラーを指定、管理者または管理者ロールのユーザーのみがで属性の場所として、イベント ハンドラーを実行できる、`RowDeleting`イベント ハンドラーは、管理者のユーザーに実行を制限ロールです。


> [!NOTE]
> `PrincipalPermission`内のクラスとして表される属性、`System.Security.Permissions`名前空間。 追加してください、`Imports System.Security.Permissions`ステートメントをこの名前空間をインポートする、分離コード クラス ファイルの先頭にします。


かどうか、何らかの方法で、管理者以外を実行しようと、`RowDeleting`イベント ハンドラーを実行する試行を非監督者または管理者以外の場合、または、 `RowUpdating` .NET ランタイムで発生するイベント ハンドラー、`SecurityException`します。


[![セキュリティ コンテキストがメソッドを実行する権限がない場合、SecurityException がスローされます。](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**図 16**: セキュリティ コンテキストが、メソッドを実行する権限がない場合、`SecurityException`がスローされます ([フルサイズの画像を表示する をクリックします](role-based-authorization-vb/_static/image48.png))。


多くのアプリケーションは、ASP.NET ページだけでなく、ビジネス ロジックとデータ アクセス レイヤーなど、さまざまな層を含むアーキテクチャもあります。 これらの層は、クラス ライブラリとして実装され、クラスとビジネス ロジックとデータに関連する機能を実行するためのメソッドを提供します。 `PrincipalPermission`属性はこれらの層もに承認規則を適用するために便利です。

使用する方法について、`PrincipalPermission`属性をクラスやメソッドで承認規則を定義しを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ[ビジネス レイヤーを使用してデータして承認規則を追加します`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)。

## <a name="summary"></a>まとめ

このチュートリアルでは、粗いを指定する方法を説明しましたし、ユーザーのロールに基づいて細かく承認規則。 ASP します。NET の URL 承認の機能は、どの id を許可または拒否できるページへのアクセスを指定するページの開発者を使用できます。 説明したように、 <a id="_msoanchor_10"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル, の URL 承認規則は、ユーザーによってごとに適用できます。 これらも適用できますロール-ロール別に、このチュートリアルの手順 1. で説明したようにします。

宣言またはプログラムによって、細かく承認規則を適用できます。 手順 2 でアクセスしたユーザーの役割に基づく別の出力を表示するために、LoginView コントロールの Rolegroup 機能を使用しました。 プログラムによって、ユーザーがページの機能を適宜調整する方法と、特定のロールに属しているかを判断する方法についても説明しました。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ビジネス層とを使用してデータ層への承認規則の追加 `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 2.0 の検査のメンバーシップ、ロール、およびプロファイル: ロールの使用](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 のセキュリティの質問の一覧](https://msdn.microsoft.com/library/ms998375.aspx)
- [技術ドキュメント、`<roleManager>`要素](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者には、Suchi 著 Teresa Murphy がなどがあります。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](assigning-roles-to-users-vb.md)
