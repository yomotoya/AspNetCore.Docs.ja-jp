---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: "ロール ベースの承認 (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルは、ロール、フレームワークがそのユーザーのセキュリティ コンテキストがユーザーのロールを関連付ける方法を見てを開始します。 次に、ロール ベースの URL を適用する方法を確認しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 331282dfa3c05dd4bd6fef19dcfe7e5c0adad84d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="role-based-authorization-vb"></a>ロール ベースの承認 (VB)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> このチュートリアルは、ロール、フレームワークがそのユーザーのセキュリティ コンテキストがユーザーのロールを関連付ける方法を見てを開始します。 ロール ベースの URL 承認規則を適用する方法を調べます。 次の宣言とプログラムの手段を使用して表示されるデータと ASP.NET ページによって提供される機能を変更するために見ていきます、します。


## <a name="introduction"></a>はじめに

<a id="_msoanchor_1"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアル URL 承認を使用して、どのようなユーザーを参照してください、特定のページのセットを指定する方法を説明しました。 内のマークアップの少しだけ`Web.config`は ASP.NET で認証されたユーザーのみがページにアクセスできるようにするように指示できます。 またはお Tito と Bob のユーザーのみが許可されたことを指定または Sam を除くすべての認証されたユーザーが許可されたことを示すためできます。

URL 承認だけでなくについても説明しました表示されるデータとへのアクセスのユーザーに基づくページによって提供される機能を制御するための宣言とプログラムの手法について説明します。 具体的には、現在のディレクトリの内容を一覧表示するページを作成します。 すべてのユーザーがこのページを参照してくださいが認証されたユーザーのみがファイルの内容を表示でしたし、Tito のみがファイルを削除できなかった。

ユーザー単位のユーザーごとに承認規則を適用することは、ブックキーピング大変な作業にまで拡大できます。 優れたアプローチでは、ロールベースの承認を使用します。 良いニュースは、承認規則を適用するため、自由に使用できるツールが同様に機能しているユーザー アカウントの場合との役割と同様です。 URL 承認規則は、ユーザーの代わりにロールを指定できます。 LoginView コントロール、認証および匿名ユーザーの別の出力を表示するには、これは、ログイン ユーザーの役割に基づいたさまざまなコンテンツを表示する構成できます。 ロール API には、ログイン ユーザーのロールを決定するためのメソッドが含まれています。

このチュートリアルは、ロール、フレームワークがそのユーザーのセキュリティ コンテキストがユーザーのロールを関連付ける方法を見てを開始します。 ロール ベースの URL 承認規則を適用する方法を調べます。 次の宣言とプログラムの手段を使用して表示されるデータと ASP.NET ページによって提供される機能を変更するために見ていきます、します。 開始しましょう!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>役割の理解する方法が関連付けられているユーザーのセキュリティ コンテキスト

ASP.NET パイプラインに要求が入るたびに関連付けられている、セキュリティ コンテキスト、要求元を識別する情報が含まれます。 フォーム認証を使用する場合は、認証チケットが id トークンとして使用されます。 説明したよう、 <a id="_msoanchor_2"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-vb.md)と<a id="_msoanchor_3"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)チュートリアルについては、`FormsAuthenticationModule`が中には、要求元の id を判断する、 [ `AuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

有効、期限切れでない認証チケットが見つかった場合、`FormsAuthenticationModule`要求元の身元を確認するためにデコードします。 新たに作成、`GenericPrincipal`オブジェクトし、これに割り当てます、`HttpContext.User`オブジェクト。 などのプリンシパルの目的は、 `GenericPrincipal`、認証されたユーザーの名前と内容を識別する役割に属しているユーザーです。 この目的は、すべてのプリンシパル オブジェクトがあるという事実によって明らかに`Identity`プロパティおよび`IsInRole(roleName)`メソッドです。 `FormsAuthenticationModule`、ただしの役割の情報を記録に興味がないと、`GenericPrincipal`オブジェクトが作成されますが、ロールが指定されていません。

ロールのフレームワークが有効になっている場合、 [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP モジュールの後の手順を`FormsAuthenticationModule`中に認証されたユーザーのロールを識別し、 [ `PostAuthenticateRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)、後に起動、`AuthenticateRequest`イベント。 要求が認証されたユーザーからの場合、`RoleManagerModule`を上書き、`GenericPrincipal`によって作成されたオブジェクト、`FormsAuthenticationModule`で置き換えて、 [ `RolePrincipal`オブジェクト](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)です。 `RolePrincipal`クラスでは、ロールの API を使用して、ユーザーが所属するどのような役割を決定します。

図 1 は、フォーム認証とロール フレームワークを使用する場合、ASP.NET のパイプライン ワークフローを示しています。 `FormsAuthenticationModule`が最初に実行、自分の認証チケットを使用してユーザーを識別し、新たに作成`GenericPrincipal`オブジェクト。 次に、`RoleManagerModule`ステップし、上書き、`GenericPrincipal`オブジェクトを`RolePrincipal`オブジェクト。

匿名ユーザーがどちらも、サイトにアクセスする場合、`FormsAuthenticationModule`も`RoleManagerModule`プリンシパル オブジェクトを作成します。


[![フォーム認証とロール フレームワークを使用する場合、認証されたユーザーの ASP.NET パイプライン イベント](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**図 1**: 認証されたユーザーとフォーム認証の使用およびロール Framework 用の ASP.NET パイプライン イベント ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Cookie にロール情報のキャッシュ

`RolePrincipal`オブジェクトの`IsInRole(roleName)`メソッド呼び出し`Roles`です。`GetRolesForUser` ユーザーのメンバーであるかどうかを判断するため、ユーザーのロールを取得する*roleName*です。 使用する場合、 `SqlRoleProvider`、これによって、クエリでデータベースへのロール ストア。 ロール ベースの URL 承認規則を使用する場合、`RolePrincipal`の`IsInRole`メソッドは、役割に基づいた URL 承認規則によって保護されているページへの要求ごとに呼び出されます。 要求ごとに、データベースにロール情報を参照する必要があるのではなく、`Roles`フレームワークには、cookie にユーザーのロールをキャッシュするためのオプションが含まれています。

Cookie にユーザーのロールをキャッシュするロールのフレームワークが構成されている場合、 `RoleManagerModule` ASP.NET パイプラインの中に、cookie を作成[`EndRequest`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)です。 この cookie は後続の要求で使用、 `PostAuthenticateRequest`、ときに、`RolePrincipal`オブジェクトを作成します。 Cookie のデータが解析され、節約、ユーザーのロールの作成に使用する場合は、cookie が有効が切れていない、`RolePrincipal`への呼び出しを行うことから、`Roles`ユーザーのロールを決定するクラス。 図 2 は、このワークフローを示しています。


[![パフォーマンスを向上させるために、Cookie にユーザーのロール情報を保存できます。](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**図 2**: ユーザーのロール情報を保存できますが Cookie パフォーマンスを向上させるに ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image6.png))


既定では、ロールのキャッシュのクッキー メカニズムは無効です。 有効にすることができます、 `<roleManager>`; で構成マークアップ`Web.config`です。 ここで説明を使用して、 [ `<roleManager>`要素](https://msdn.microsoft.com/library/ms164660.aspx)でロール プロバイダーを指定する、 <a id="_msoanchor_4"> </a> [*の作成とロールの管理*](creating-and-managing-roles-vb.md)チュートリアルでは、既にこの要素で、アプリケーションのため`Web.config`ファイル。 属性としてロール キャッシュ cookie の設定が指定されて、 `<roleManager>`; 要素であり、表 1 に集計します。

> [!NOTE]
> 表 1 に示した構成設定は、結果として得られるロール キャッシュのクッキーのプロパティを指定します。 Cookie、しくみ、およびそのさまざまなプロパティの詳細については、「[この Cookie チュートリアル](http://www.quirksmode.org/js/cookies.html)です。


| **Property** | **説明** |
| --- | --- |
| `cacheRolesInCookie` | クッキーのキャッシュを使用するかどうかを示すブール値。 既定値は `false` です。 |
| `cookieName` | ロールのキャッシュのクッキーの名前。 既定値は"です。ASPXROLES"です。 |
| `cookiePath` | ロール名のクッキーのパス。 Path 属性は、開発者は、特定のディレクトリ階層にクッキーのスコープを制限できます。 既定値は、「/」をドメインに加えられたすべての要求に認証チケットを送信するブラウザーに通知します。 |
| `cookieProtection` | どのような手法を使用してロールのキャッシュのクッキーを保護することを示します。 使用可能な値: `All` (既定)。`Encryption`;`None`; と`Validation`です。 手順 3. に戻って、 <a id="_anchor_5"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)これらの保護レベルの詳細についてのチュートリアルです。 |
| `cookieRequireSSL` | 認証 cookie の送信に SSL 接続が必要かどうかを示すブール値。 既定値は `false` です。 |
| `cookieSlidingExpiration` | ユーザーが 1 つのセッション中にサイトを訪問する cookie のタイムアウトがリセットされるたびにするかどうかを示すブール値。 既定値は `false` です。 ときにこの値は適切なのみ`createPersistentCookie`に設定されている`true`です。 |
| `cookieTimeout` | 認証チケットの有効期限が切れるまでの分単位で時間を指定します。 既定値は `30` です。 ときにこの値は適切なのみ`createPersistentCookie`に設定されている`true`です。 |
| `createPersistentCookie` | ロールのキャッシュのクッキーがセッション cookie または永続的な cookie がかどうかを指定するブール値。 場合`false`(既定)、セッションの cookie を使用すると、ブラウザーを閉じたときに、削除します。 場合`true`、永続的な cookie が使用されます。 有効期限が切れる`cookieTimeout`が作成されてから何分経つまたはの値に応じて、前の訪問後番号`cookieSlidingExpiration`です。 |
| `domain` | Cookie のドメイン値を指定します。 既定値は、ブラウザー (www.yourdomain.com) などから発行されたドメインを使用すると、空の文字列です。 この場合、cookie は**されません**admin.yourdomain.com などのサブドメインに要求を作成する場合に送信されます。カスタマイズする必要があるすべてのサブドメインに渡される cookie をする場合、`domain`属性に、「ユーザー」に設定するとします。 |
| `maxCachedResults` | Cookie では、キャッシュされるロール名の最大数を指定します。 既定値は 25 です。 `RoleManagerModule`に属しているユーザーに対してクッキーを作成しません以上`maxCachedResults`ロール。 その結果、`RolePrincipal`オブジェクトの`IsInRole`メソッドを使用して、`Roles`ユーザーのロールを決定するクラス。 理由`maxCachedResults`が存在する多数のユーザー エージェントは 4,096 バイトを超える cookie を許可しないためです。 これをこのサイズの制限を超過する可能性を減らすためにこのキャップが意図したものです。 非常に長いロール名がある場合は、場合より小さいを指定する`maxCachedResults`値; contrariwise、非常に短いロール名があれば、することができます可能性この値を大ききます。 |

**表 1**: ロール キャッシュのクッキーの構成オプション

非永続的なロール キャッシュのクッキーを使用するようにアプリケーションを構成してみましょう。 これを実現する、更新、`<roleManager>`内の要素`Web.config`次 cookie に関連する属性を含める。

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

更新しました、 `<roleManager>`; 3 つの属性を追加することによって要素: `cacheRolesInCookie`、 `createPersistentCookie`、および`cookieProtection`です。 設定して`cacheRolesInCookie`に`true`、`RoleManagerModule`要求ごとに、ユーザーのロール情報を参照することのではなく、cookie にユーザーのロールが自動的にキャッシュようになりました。 明示的に設定すれば、`createPersistentCookie`と`cookieProtection`属性を`false`と`All`、それぞれします。 技術的には、それらが既定値に割り当てましただけはそれらをここに記述に明示的になりますので、これらの属性の値をオフに永続的な cookie を使用していないこと、暗号化し検証 cookie は、両方を指定する必要はありませんでした。

すべてであることには! 今後は、ロール、フレームワークでは、cookie にユーザーのロールがキャッシュされます。 ユーザーのブラウザーが cookie をサポートしていないか、cookie が削除されたか、失われた、何らかの理由はたいした問題 –、`RolePrincipal`オブジェクトを使用して、単に、`Roles`ない cookie (または、無効または期限切れの 1 つ) が使用可能なことである場合のクラスです。

> [!NOTE]
> Microsoft の Patterns&amp;プラクティス グループは、永続的なロールのキャッシュの cookie を使用してでは推奨されません。 場合は、ハッカーが有効なユーザーのクッキーにアクセスできる何らかの方法では、ロールのキャッシュのクッキーを所有しているがロールのメンバーシップを証明するために十分なために、そのユーザーを偽装彼できます。 ユーザーのブラウザーで cookie が永続化される場合は、この問題が発生する可能性が増加します。 このセキュリティの推奨設定だけでなく他のセキュリティに関する注意事項の詳細についてを参照してください、 [ASP.NET 2.0 用のセキュリティの質問の一覧](https://msdn.microsoft.com/library/ms998375.aspx)です。


## <a name="step-1-defining-role-based-url-authorization-rules"></a>手順 1: ロール ベースの URL 承認規則を定義します。

説明したように、 <a id="_msoanchor_6"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルでは、ユーザー単位のユーザーまたはロール別の役割のページのセットへのアクセスを制限するための手段を提供する URL 承認ベース。 URL 承認規則が記述された`Web.config`を使用して、 [ `<authorization>`要素](https://msdn.microsoft.com/library/8d82143t.aspx)で`<allow>`と`<deny>`子要素です。 前のチュートリアルで説明されているユーザーに関連する承認規則だけでなく各`<allow>`と`<deny>`子要素も含めることができます。

- 特定のロール
- ロールのコンマ区切りの一覧

たとえば、URL 承認規則は、管理者と管理者の役割で、それらのユーザーにアクセスを許可しますが、他のすべてのユーザーへのアクセスを拒否します。

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

`<allow>`上記マークアップ内の要素を示す管理者と管理者ロールが許可される、 `<deny>`; 要素に指示する*すべて*ユーザーが拒否されました。

アプリケーションを構成しましょうできるように、 `ManageRoles.aspx`、 `UsersAndRoles.aspx`、および`CreateUserWizardWithRoles.aspx`ページは、管理者の役割内のユーザーにアクセスできるだけ中に、`RoleBasedAuthorization.aspx`ページがの利用者すべてにアクセスできる状態です。

これを実現する、追加することで開始、`Web.config`ファイルの名前を`Roles`フォルダーです。


[![ロールのディレクトリに Web.config ファイルを追加します。](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**図 3**: 追加、`Web.config`ファイルの名前を`Roles`ディレクトリ ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image9.png))


次に示す構成マークアップを次に、追加`Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

`<authorization>`内の要素、`<system.web>`セクションでは、管理者ロールのユーザーのみが、ASP.NET のリソースをアクセス可能性があることを示します、`Roles`ディレクトリ。 `<location>`要素が別の URL 承認規則のセットを定義、 `RoleBasedAuthorization.aspx`  ページで、すべてのユーザー ページにアクセスできるようにします。

変更を保存した後に`Web.config`、管理者ロールに含まれていないユーザーとしてログインしをやり直して、保護されているページのいずれかを参照してください。 `UrlAuthorizationModule`ことすることはありません。 要求されたリソースにアクセスするアクセス許可を検出その結果、、`FormsAuthenticationModule`ログイン ページにリダイレクトされます。 ログイン ページで、リダイレクトすることは、`UnauthorizedAccess.aspx`ページ (図 4 を参照してください)。 この最終的にリダイレクトするログイン ページから`UnauthorizedAccess.aspx`のステップ 2 でログイン ページに追加したコードが原因で発生、 <a id="_msoanchor_7"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルです。 具体的には、ログイン ページを自動的にリダイレクト認証されたユーザーに`UnauthorizedAccess.aspx`、クエリ文字列が含まれている場合、`ReturnUrl`パラメーターはこのパラメーターとしてユーザーはページを表示しようとした後、ログイン ページに到着したことを示します表示する権限。


[![管理者ロールのユーザーのみが保護されているページを表示できます。](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**図 4**: 管理者ロールのユーザーのみが保護されているページを表示できます ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image12.png))


ログオフし、管理者ロールに属しているユーザーとしてログインします。 これで 3 つの保護されたページを表示することができます。


[![Tito はことができます、UsersAndRoles.aspx ページためには、管理者ロールを参照してください。](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**図 5**: Tito がアクセスできる、`UsersAndRoles.aspx`ページためには、管理者ロール ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> 役割またはユーザー:: URL 承認規則を指定するときにすることが重要規則が一番上から、一度に 1 つの分析を停止していることに注意してください。 場合によっては、一致が見つかったで、一致が見つかるとすぐに、ユーザーがアクセス許可または拒否する`<allow>`または`<deny>`要素。 **一致するものが見つからない場合、ユーザーがアクセスを許可します。** その結果、1 つまたは複数のユーザー アカウントにアクセスを制限する場合を使用すること、 `<deny>` URL 承認の構成の最後の要素としての要素。 **URL 承認規則が含まれない場合、**`<deny>`**要素、すべてのユーザー アクセスが許可されます。** URL 承認規則を分析する方法の詳細についてを参照、"方法を見て、`UrlAuthorizationModule`承認規則を使用してアクセス許可または拒否"のセクションで、 <a id="_msoanchor_8"> </a> [ *ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルです。


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>手順 2: 現在ログインしているユーザーのロールに基づいて機能を制限します。

URL 承認の作成が簡単に粗い承認を指定するルールの状態にあるどの id が許可されており、どれが特定のページ (ファイルまたはフォルダーおよびそのサブフォルダー内のすべてのページ) の表示から拒否されました。 ただし、場合によっては、ページにアクセスして、アクセスしたユーザーのロールに基づいて、ページの機能を制限するためのすべてのユーザーを許可するたい場合があります。 これにより、ユーザーのロールに基づいて、特定のロールに属しているユーザーに追加の機能を提供するデータを表示または非表示が伴う場合があります。

宣言またはプログラムによって (または、2 つの組み合わせを通じて)、このような細かい粒度のロールベースの承認規則を実装することができます。 次のセクションで LoginView コントロールを使用して宣言きめ細かく承認を実装する方法が表示されます。 次は、プログラムによる手法について説明します。 きめ細かく承認規則を適用することで見ることができます、前にただし、まずいただくために機能を持つは、それにアクセスしたユーザーのロールによって異なります。 ページを作成します。

GridView では、システム内のすべてのユーザー アカウントを一覧するページを作成してみましょう。 GridView は、各ユーザーのユーザー名、電子メール アドレス、最後のログイン日時、およびユーザーについてのコメントが含まれます。 ユーザーごとの情報を表示するだけでなく、GridView は編集を含めるし、機能を削除します。 編集のこのページを作成し、すべてのユーザーに使用可能な機能を削除おは最初にします。 「LoginView コントロールを使用して、」と「プログラムで制限する機能」のセクションで有効するにまたはアクセスしたユーザーのロールに基づくこれら機能無効にする方法が表示されます。

> [!NOTE]
> ビルドには ASP.NET ページは、ユーザー アカウントを表示するのに GridView コントロールを使用します。 このチュートリアルでは、系列は、フォーム認証、承認、ユーザー アカウント、およびロールに焦点を当てています、以降しない GridView コントロールの内部動作をについて説明する時間をかけるにします。 このチュートリアルでは、このページを設定するための特定の詳細な手順についてはの特定の選択肢が行われた理由または影響の特定のプロパティが表示される出力が詳細には掘り下げてされません。 GridView コントロールの詳細については、チェック アウト my  *[ASP.NET 2.0 のデータを扱う](../../data-access/index.md)*一連のチュートリアルです。


開いて開始、 `RoleBasedAuthorization.aspx`  ページで、`Roles`フォルダーです。 ページから、デザイナーとセットに GridView をドラッグしてその`ID`に`UserGrid`です。 すぐを呼び出すコードを記述して、`Membership`です。`GetAllUsers` メソッドと、その結果をバインド`MembershipUserCollection`GridView するオブジェクト。 `MembershipUserCollection`が含まれています、`MembershipUser`システム内の各ユーザー アカウントのオブジェクト`MembershipUser`オブジェクトと同様にプロパティがある`UserName`、`Email`、`LastLoginDate`となります。

ユーザー アカウントをグリッドにバインドするコードを記述して前に、最初を定義してみましょう GridView のフィールドです。 GridView のスマート タグからリンクをクリックして、"列の編集 フィールドのダイアログを起動するボックス (図 6 を参照してください)。 ここでは、左下隅の「フィールドの自動生成」チェック ボックスをオフにします。 この GridView、CommandField を追加、編集および機能を削除して、設定するので、`ShowEditButton`と`ShowDeleteButton`プロパティを True にします。 次に、表示するための 4 つのフィールドを追加、 `UserName`、 `Email`、 `LastLoginDate`、および`Comment`プロパティです。 BoundField を使用して、2 つの読み取り専用プロパティ (`UserName`と`LastLoginDate`) と 2 つの編集可能なフィールドに対して TemplateFields (`Email`と`Comment`)。

最初の BoundField ディスプレイ、`UserName`プロパティ以外のセット、`HeaderText`と`DataField`"UserName"をプロパティです。 このフィールドは編集できません、設定によりその`ReadOnly`プロパティを True にします。 構成、 `LastLoginDate` BoundField を設定してその`HeaderText`「最後のログイン」に、その`DataField`"LastLoginDate"にします。 (日付と時刻) 代わりに日付のみが表示されないようにこの BoundField の出力の書式設定してみましょう。 これを実現する設定のこの BoundField の`HtmlEncode`プロパティを False に、その`DataFormatString`プロパティを"{0: d}"。 設定も、`ReadOnly`プロパティを True にします。

設定、 `HeaderText` "Email"と「コメント」を 2 つの TemplateFields のプロパティです。


[![GridView のフィールドは、[フィールド] ダイアログ ボックスで構成できます。](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**図 6**: GridView のフィールドでくように構成を通じて [フィールド] ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image18.png))


ここで定義する必要があります、`ItemTemplate`と`EditItemTemplate`"Email"と「コメント」TemplateFields です。 各ラベル Web コントロールを追加、`ItemTemplates`し、バインド、`Text`プロパティを`Email`と`Comment`プロパティ、それぞれします。

"Email"TemplateField という名前のテキスト ボックスを追加`Email`にその`EditItemTemplate`バインドとその`Text`プロパティを`Email`双方向データ バインドを使用してプロパティです。 RequiredFieldValidator と RegularExpressionValidator を追加、`EditItemTemplate`を電子メールのプロパティを編集訪問者が有効な電子メール アドレスを入力したことを確認します。 「コメント」TemplateField という名前の複数行テキスト ボックスを追加`Comment`にその`EditItemTemplate`です。 テキスト ボックスの設定`Columns`と`Rows`40 および 4 では、プロパティをそれぞれ、し、バインド、`Text`プロパティを`Comment`双方向データ バインドを使用してプロパティです。

これら TemplateFields を構成した後、宣言型マークアップを次のようになります。

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

編集またはそのユーザーを知っている必要があります。 ユーザー アカウントを削除するときに`UserName`プロパティの値。 GridView の設定`DataKeyNames`プロパティ"UserName"をこの情報を GridView のを通じて利用できるように`DataKeys`コレクション。

最後に、ValidationSummary コントロールをページに追加し、設定、`ShowMessageBox`プロパティを True に、その`ShowSummary`プロパティを False にします。 これらの設定で、ValidationSummary はユーザーが存在しないか無効な電子メール アドレスを持つユーザー アカウントを編集する場合にクライアント側の警告を表示します。

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

このページの宣言型マークアップがこれで完了しました。 次のタスクは、ユーザー アカウントのセットを GridView にバインドを開始します。 という名前のメソッドを追加`BindUserGrid`を`RoleBasedAuthorization.aspx`ページの分離コード クラスをバインドする、`MembershipUserCollection`によって返される`Membership.GetAllUsers`を`UserGrid`GridView です。 このメソッドを呼び出して、`Page_Load`最初のページ アクセス時にイベント ハンドラー。

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

配置でこのコードでは、ブラウザーでページを参照してください。 図 7 に示す、システム内の各ユーザー アカウントに関する情報を一覧表示する GridView が表示されます。


[![UserGrid GridView システム内の各ユーザーに関する情報を一覧します。](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**図 7**: `UserGrid` GridView の一覧については各システム内のユーザー ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> `UserGrid` GridView には、すべて非ページ インターフェイス内のユーザーの一覧表示します。 この単純なグリッド インターフェイスは、シナリオに適した 1 ダース以上のユーザーがいくつか。 1 つのオプションでは、ページングを有効にする GridView を構成します。 `Membership.GetAllUsers`メソッドに次の 2 つのオーバー ロードがあります: 入力パラメーターを受け取らず、すべてのユーザーと受け取り、ページのインデックスと、ページ サイズの整数値を指定したユーザーのサブセットのみを返すものを返します。 2 番目のオーバー ロードは、ユーザー アカウントの正確なサブセットだけを返すので、ユーザーをより効率的にページングに使用できるのではなく*すべて*にします。 何千ものユーザー アカウントを使っている場合は、フィルターに基づくインターフェイスだけそれらのユーザー インスタンスの開始に選択した文字を持つユーザー名を表示するいずれかを検討することができます。 [ `Membership.FindUsersByName`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)はフィルターに基づくユーザー インターフェイスを構築するために最適です。 今後のチュートリアルでこのようなインターフェイスの構築で見ていきます。


GridView コントロールでは、組み込みの編集とコントロールが SqlDataSource または ObjectDataSource など、正しく構成されているデータ ソース コントロールにバインドされている場合サポートの削除を提供します。 `UserGrid` GridView、ただし、そのデータをプログラムでバインドにはそのため、これら 2 つのタスクを実行するコードを作成する必要があります。 Gridview のイベント ハンドラーを作成する必要がありますが具体的には、 `RowEditing`、 `RowCancelingEdit`、 `RowUpdating`、および`RowDeleting`訪問者が GridView をクリックしたときに発生するイベントは、編集、Cancel、Update、またはボタンを削除します。

Gridview のイベント ハンドラーを作成して開始`RowEditing`、 `RowCancelingEdit`、および`RowUpdating`イベントし、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

`RowEditing`と`RowCancelingEdit`単にイベント ハンドラーを設定、GridView の`EditIndex`プロパティと、再バインドが、グリッドへのアカウントのユーザーの一覧です。 行われます興味深いもの、`RowUpdating`イベント ハンドラー。 ようにすることによって、データが有効では取得し、このイベント ハンドラーを開始、`UserName`から編集済みのユーザー アカウントの値、`DataKeys`コレクション。 `Email`と`Comment`2 TemplateFields' 内のテキスト ボックス`EditItemTemplate`s は、プログラムで参照します。 その`Text`プロパティには、編集した電子メール アドレスとコメントが含まれます。

最初の呼び出しを使用して作業を行うのユーザーの情報を取得する必要があります、メンバーシップ API を介してユーザー アカウントを更新するために`Membership.GetUser(userName)`です。 返された`MembershipUser`オブジェクトの`Email`と`Comment`編集インターフェイスから 2 つのテキスト ボックスに入力した値を持つプロパティを更新しています。 呼び出しでこのような変更を保存する最後に、 [ `Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)です。 `RowUpdating`イベント ハンドラーの編集済みインターフェイスを GridView を戻すことによって完了します。

次に、作成、 `RowDeleting` RowDeleting イベント ハンドラーと、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

上記のイベント ハンドラーを取得することで開始、 `UserName` GridView から値`DataKeys`コレクション; この`UserName`に渡される値のメンバーシップ クラスは、 [ `DeleteUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)です。 `DeleteUser`メソッドは、(ロールなどこのユーザーが属する) 関連のメンバーシップ データを含む、システムからユーザー アカウントを削除します。 ユーザーを削除する、グリッドの後に`EditIndex`(ユーザーでは、別の行の編集モードの間に、削除がクリックされた) 場合は、-1 に設定され、`BindUserGrid`メソッドが呼び出されます。

> [!NOTE]
> [削除] ボタンには、あらゆる種類のユーザー アカウントを削除する前に、ユーザーから送信される確認は不要です。 皆さんが誤って削除されているアカウントの可能性を低くためにユーザーに確認のいくつかのフォームを追加します。 クライアント側の確認 ダイアログ ボックスで、操作を確認する最も簡単な方法の 1 つです。 この方法の詳細については、次を参照してください。[削除時にクライアント側の確認を追加する](https://asp.net/learn/data-access/tutorial-42-vb.aspx)です。


このページが期待どおりに機能することを確認します。 任意のユーザー アカウントを削除するだけでなく、すべてのユーザーの電子メール アドレスと、コメントを編集することができます。 以降、`RoleBasedAuthorization.aspx`ページはすべてのユーザーにアクセスできる、任意のユーザー – 匿名でも閲覧者 – できますこのページにアクセスおよび編集し、ユーザー アカウントを削除します。 このページを更新して、監督者および管理者の役割の唯一のユーザーは、ユーザーの電子メール アドレスと、コメントを編集でき、管理者のみがユーザー アカウントを削除できますようにしましょう。

「LoginView コントロールを使用して、」セクションでは、LoginView コントロールを使用して、ユーザーのロールに固有の指示が表示されます。 場合は、管理者ロールのユーザーは、このページにアクセス、編集し、ユーザーを削除する方法の手順を示します。 管理者ロールのユーザーには、このページに達すると、ユーザーの編集の手順を示します。 され、訪問者が匿名または管理者または Administrators ロールでは場合、は、編集またはユーザー アカウント情報を削除することできませんを説明するメッセージを表示おされます。 「プログラムで制限する機能」セクションでは、プログラムで表示または非表示、編集および削除ボタンがユーザーの役割に基づいたコードを書き込みます。

### <a name="using-the-loginview-control"></a>LoginView コントロールを使用します。

過去のチュートリアルで見てきたよう LoginView コントロールは、認証と匿名ユーザーの異なるインターフェイスを表示するために役立ちますが、LoginView コントロールを使用して、ユーザーのロールに基づいて、異なるマークアップを表示することもできます。 アクセスしたユーザーのロールに基づく別の手順を表示するのに LoginView コントロールを使用してみましょう。

上記の LoginView を追加することで開始、 `UserGrid` GridView です。 した前に説明した、LoginView コントロールは 2 つの組み込みテンプレート:`AnonymousTemplate`と`LoggedInTemplate`です。 ユーザーに通知する、できませんを編集したり削除したり、ユーザー情報をこれらのテンプレートの両方で、簡単なメッセージを入力します。

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

加え、`AnonymousTemplate`と`LoggedInTemplate`、LoginView コントロールを含めることができます*RoleGroups*、ロール固有のテンプレートがあります。 各 RoleGroup には、1 つのプロパティが含まれています。 `Roles`、、RoleGroup が対象となる役割を指定します。 `Roles` ("Administrators") のような 1 つのロールまたは (「管理者、スーパーバイザー」) のようにロールのコンマ区切りの一覧に、プロパティを設定できます。

RoleGroups を管理するには、RoleGroup コレクション エディターを表示するコントロールのスマート タグから"編集 RoleGroups"リンクをクリックします。 2 つの新しい RoleGroups を追加します。 設定の最初の RoleGroup の`Roles`プロパティを"Administrators"し、2 番目の「管理者」です。


[![LoginView の役割に固有のテンプレート RoleGroup コレクション エディターを使用を管理します。](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**図 8**: LoginView の役割に固有のテンプレートを RoleGroup コレクション エディターを管理 ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image24.png))


[Ok]、RoleGroup コレクション エディターを閉じます。これにより、更新に含める LoginView の宣言型マークアップ、`<RoleGroups>`セクションで、`<asp:RoleGroup>`各 RoleGroup の子要素が RoleGroup コレクション エディターで定義されています。 LoginView のスマート タグ内で最初に記載されている「ビュー」のドロップダウン リストがさらに、一覧、`AnonymousTemplate`と`LoggedInTemplate`– 追加 RoleGroups もが含まれています。

管理者ロールのユーザーが表示されている管理者ロールのユーザーが編集および削除するための手順を示すように、ユーザー アカウントを編集する方法について、RoleGroups を編集します。 これらの変更を加えたら、LoginView の宣言型マークアップは、次のようになります。

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

これらの変更を加えたら、ページを保存し、ブラウザーを使用しを参照してください。 最初に匿名ユーザーとして、ページを参照してください。 メッセージを表示するか、"いないにログインしているシステム。 そのためことはできません編集または削除するユーザーの情報。" 認証されたユーザーが、監督者も管理者ロールのどちらにあるものとしてログインします。 この時間をメッセージが表示、"監督者または管理者ロールのメンバーではないです。 そのためことはできません編集または削除するユーザーの情報。"

次に、管理者ロールのメンバーであるユーザーとしてログインします。 今度はスーパーバイザー ロール固有はずメッセージ (図 9 を参照してください)。 管理者の役割の役割に固有の管理者が表示されますが (図 10 参照) をメッセージ内のユーザーとしてログインする場合とします。


[![ブルースはスーパーバイザー ロール固有のメッセージを表示します。](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**図 9**: ブルースはスーパーバイザー ロール固有のメッセージを表示 ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image27.png))


[![Tito には、管理者の役割固有のメッセージが表示されます。](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**図 10**: Tito が管理者の役割固有のメッセージを表示 ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image30.png))


図 9 にスクリーン ショット、10 のスライド ショーとして、LoginView では、複数のテンプレートが適用される場合でも、1 つのテンプレートがのみ表示します。 ブルースおよび Tito 両方ログに記録ユーザー で、まだ、LoginView が一致する RoleGroup のみを表示および not、`LoggedInTemplate`です。 さらに、管理者と管理者の両方のロールに属している Tito まだ LoginView コントロールを表示、監督者の代わりに管理者の役割固有のテンプレートいずれか。

図 11 は、表示するためにどのようなテンプレートを特定 LoginView コントロールによって使用されるワークフローを示しています。 RoleGroup が指定された 1 つ以上がある場合、LoginView テンプレートが表示されるに注意してください、*最初*RoleGroup と一致します。 つまり、おは、最初の RoleGroup としてスーパーバイザー RoleGroup と 2 つ目として管理者に配置されているが場合、Tito このページを表示するときに彼はメッセージが表示、監督者です。


[![LoginView コントロールのワークフローをレンダリングするテンプレートを決定します。](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**図 11**:「LoginView コントロール ワークフローを決定するものにするテンプレートのレンダリング ([フルサイズ イメージを表示するに、をクリックして](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>プログラムで機能を制限します。

LoginView コントロールには、ページを参照するユーザーの役割に基づいたさまざまな手順が表示されますが、編集し、[キャンセル] ボタンはすべてに表示されたままです。 プログラムで匿名の訪問者および管理者でも、管理者ロールのユーザーには、編集および削除のボタンを非表示にする必要があります。 すべての管理者ではないユーザーの削除 ボタンを非表示にする必要があります。 これを実現するためにプログラムで CommandField の編集と削除のあるおよび参照を設定するコードのビットを書き込みます、`Visible`プロパティ`False`必要に応じて、します。

CommandField でコントロールをプログラムで参照する最も簡単な方法では、最初のテンプレートに変換します。 これを実現するには、GridView のスマート タグで"列の編集] リンクをクリックして、[現在のフィールドの一覧から、CommandField を選択し、「このフィールドを TemplateField に変換」リンクをクリックします。 これを TemplateField に、CommandField を有効にする`ItemTemplate`と`EditItemTemplate`です。 `ItemTemplate`編集と削除のある中に含まれています、`EditItemTemplate`キャンセルのある、更新プログラムを格納します。


[![CommandField を TemplateField に変換します。](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**図 12**: 変換を TemplateField に CommandField ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image36.png))


編集と削除のあるで更新、 `ItemTemplate`、設定、`ID`プロパティの値を`EditButton`と`DeleteButton`、それぞれします。

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

GridView 列挙内のレコードでデータが GridView にバインドされているときにその`DataSource`プロパティを対応する生成`GridViewRow`オブジェクト。 各として`GridViewRow`オブジェクトが、作成、`RowCreated`イベントが発生します。 無許可のユーザーの編集および削除のボタンを非表示にするためにこのイベントのイベント ハンドラーを作成し、編集および削除のある、設定をプログラムで参照するお必要があります。 その`Visible`プロパティに応じて。

イベント ハンドラーを作成、`RowCreated`イベントし、次のコードを追加します。

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

注意してください、`RowCreated`イベントの発生*すべて*のヘッダー、フッター、ポケットベル インターフェイス、およびなどを含め、GridView 行です。 のみを扱っている編集モードではなく、データ行 (編集モードで行に編集および削除の代わりに更新し、[キャンセル] ボタンがあるため) 場合に、編集および削除のあるをプログラムで参照するとします。 このチェックはによって処理される、`If`ステートメントです。

編集および削除のあるが参照されている場合は編集モードに含まれていないデータ行を扱っている、され、その`Visible`によって返されるブール値に基づいてプロパティが設定、`User`オブジェクトの`IsInRole(roleName)`メソッドです。 `User`オブジェクトによって作成されたプリンシパルを参照して、`RoleManagerModule`以外の場合は、その結果、`IsInRole(roleName)`メソッドに現在のユーザーが属しているかどうかを決定するロール API を使用して*roleName*です。

> [!NOTE]
> お使用することも、ロール クラスを直接への呼び出しを置き換える`User.IsInRole(roleName)`を呼び出して、 [ `Roles.IsUserInRole(roleName)`メソッド](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)です。 プリンシパルのオブジェクトを使用することにしました`IsInRole(roleName)`この例で、ロールの API を直接使用するよりも効率的になっているためです。 このチュートリアルで前に、cookie にユーザーのロールをキャッシュするロール マネージャーを構成しました。 これは、cookie のデータがキャッシュされたときにのみ使用されますが、プリンシパルの`IsInRole(roleName)`メソッドが呼び出されます。 ロール API への直接呼び出しは、ロール ストアへのトリップを常に関連します。 プリンシパル オブジェクトを呼び出す場合でも、cookie には、ロールはキャッシュされません、`IsInRole(roleName)`メソッドは、ときにこれを呼び出すため、結果をキャッシュして、要求時に初めての方が効率的です。 その一方で、ロール API、キャッシュとは行いません。 `RowCreated`イベントが GridView で行ごとに 1 回発生を使用して`User.IsInRole(roleName)`一方で、ロール ストアへの 1 つだけのトリップは、`Roles.IsUserInRole(roleName)`必要があります*N* 、トリップを*N*はグリッドに表示されるユーザー アカウントの数。


[編集] ボタンの`Visible`プロパティに設定されている`True`場合、このページにアクセスしたユーザーは管理者または管理者ロールにあるそれ以外の場合に設定されている`False`です。 削除ボタンの`Visible`プロパティに設定されている`True`ユーザーが管理者ロールの場合のみです。

ブラウザーからこのページをテストします。 匿名ユーザーとして、または、スーパーバイザーでも、管理者であるユーザーとして、ページにアクセスする場合、CommandField は空です。存在する場合、されますが、編集、または削除することがなくシン シルバーとしてボタンをクリックします。

> [!NOTE]
> CommandField を非表示にすることはまったくときに、非監督者と非管理者がページにアクセスします。 私のままに練習として、リーダーの。


[![非監督者と非管理者の編集と削除ボタンを非表示します。](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**図 13**:、編集および削除のボタンが非監督者と非管理者のユーザーに対して非表示 ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image39.png))


スーパーバイザー ロールに (ただし、管理者ロールにない) に属しているユーザーにアクセスするとすると、[編集] ボタンが表示されます。


[![[削除] ボタンを非表示にスーパーバイザーに使用可能では、[編集] ボタン](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**図 14**: 中に、[編集] ボタンがスーパーバイザーに使用可能で、[削除] ボタンが非表示 ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image42.png))


管理者アクセスすると、彼女は、編集および削除の両方のボタンにアクセスします。


[![編集および削除のボタンは使用可能な管理者向け](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**図 15**:、編集および削除のボタンは使用可能な管理者にとって ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>手順 3: クラスとメソッドにロールベースの承認規則を適用します。

限定して手順 2. で監督者および管理者の役割のユーザーに機能を編集し、管理者に権限のみを削除します。 プログラムによる方法で承認されていないユーザーに関連付けられているユーザー インターフェイス要素を非表示にすることで実現しました。 このようなメジャーでは、承認されていないユーザーは特権のアクションを実行できませんは保証されません。 後で追加されるユーザー インターフェイス要素や承認されていないユーザーに対して非表示にすることを忘れた場合の可能性があります。 または、ハッカーが目的のメソッドを実行する ASP.NET ページを取得するその他の何らかの方法に気付く場合があります。

機能の特定の部分を未認証のユーザーがアクセスできないことを確認する簡単な方法は、そのクラスまたはメソッドを装飾するが、 [ `PrincipalPermission`属性](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx)です。 .NET ランタイム クラスを使用して、そのメソッドの 1 つを実行時に、現在のセキュリティ コンテキストが権限を持っていることを確認するを確認します。 `PrincipalPermission`属性は、これらの規則を定義できる機構を提供します。

使用してきました、`PrincipalPermission`属性に戻り、 <a id="_msoanchor_9"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルです。 GridView を装飾する方法を説明しました具体的には、`SelectedIndexChanged`と`RowDeleting`イベント ハンドラーがでしたによってのみ実行される認証されたユーザーと Tito、それぞれできるようにします。 `PrincipalPermission`属性は、の役割と適切に動作します。

使用する例を見てみましょう、 `PrincipalPermission` GridView の属性`RowUpdating`と`RowDeleting`権限のないユーザーの実行を禁止するイベント ハンドラー。 必要なことを行うには、各関数の定義の上に適切な属性を追加です。

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

属性、`RowUpdating`イベント ハンドラーを指定の属性として、管理者または管理者ロールのユーザーのみが、イベント ハンドラーを実行できることを`RowDeleting`イベント ハンドラーは、管理者のユーザーに実行を制限ロールです。


> [!NOTE]
> `PrincipalPermission`属性がクラスとして表される、`System.Security.Permissions`名前空間。 追加してください、`Imports System.Security.Permissions`この名前空間をインポートする、分離コード クラス ファイルの上部にあるステートメントです。


かどうか、何らかの理由で、管理者以外を実行しようと、`RowDeleting`イベント ハンドラー、またはを実行するときにスーパーバイザー以外または管理者以外の場合、 `RowUpdating` .NET ランタイムで発生するイベント ハンドラー、`SecurityException`です。


[![SecurityException がスローされるセキュリティ コンテキストが、メソッドを実行する権限がない場合](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**図 16**: セキュリティ コンテキストが、メソッドを実行する権限がない場合、`SecurityException`がスローされます ([フルサイズのイメージを表示するをクリックして](role-based-authorization-vb/_static/image48.png))


ASP.NET ページだけでなく多くのアプリケーションでは、ビジネス ロジックとデータ アクセス レイヤーなど、さまざまな層が含まれています、アーキテクチャもが存在します。 これらのレイヤーでは、一般に、クラス ライブラリとして実装し、クラスとビジネス ロジックとデータに関連する機能を実行するためのメソッドを提供します。 `PrincipalPermission`属性はこれらのレイヤーもに承認規則を適用するために役立ちます。

使用する方法について、`PrincipalPermission`属性をクラスやメソッドで承認規則を定義しを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ エントリ[ビジネス レイヤーを使用してデータして承認規則を追加します。`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>まとめ

このチュートリアルでは、粗いを指定する方法について説明しましたをきめ細かく承認規則は、ユーザーのロールに基づきます。 ASP です。NET の URL 承認の機能には、どの id を許可または拒否ページへのアクセスを指定するページの開発者ができるようにします。 バックアップで示したように、 <a id="_msoanchor_10"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルの URL 承認規則をユーザー単位のユーザーごとに適用できます。 これらも適用できますロール別のロールに基づいて、このチュートリアルの手順 1 で示したようにします。

宣言またはプログラムによって、きめ細かく承認規則を適用可能性があります。 手順 2 でアクセスしたユーザーの役割に基づいた別の出力を表示するために、LoginView コントロールの RoleGroups 機能を使用してについて説明しました。 プログラムによって、ユーザーがページの機能を必要に応じて調整する方法と、特定のロールに属しているかを判断する方法についても説明しました。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ビジネスおよびデータ層を使用する承認規則の追加`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET 2.0 を確認するメンバーシップ、ロール、およびプロファイル: ロールの使用](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [ASP.NET 2.0 用のセキュリティの質問の一覧](https://msdn.microsoft.com/library/ms998375.aspx)
- [技術ドキュメント、`<roleManager>`要素](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Suchi Banerjee および Teresa マーフィーが含まれます。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](assigning-roles-to-users-vb.md)
