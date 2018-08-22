---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: ロックを解除して、ユーザー アカウント (c#) を承認する |Microsoft Docs
author: rick-anderson
description: このチュートリアルは、管理者が管理する web ページを構築する方法を示しています。 ユーザーのロックアウトと状態を承認します。 新しいユーザーの o を承認する方法も表示されます.
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a8373f62833c3a76d2e7f96193e5ecbe2d9c593
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827002"
---
<a name="unlocking-and-approving-user-accounts-c"></a>ユーザー アカウントのロックを解除し、承認する (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> このチュートリアルは、管理者が管理する web ページを構築する方法を示しています。 ユーザーのロックアウトと状態を承認します。 また、電子メール アドレスを確認、後にのみ、新しいユーザーを承認する方法もわかります。


## <a name="introduction"></a>はじめに

各ユーザー アカウントが、ユーザーがサイトにログインできるかどうかを指定する 2 つの状態フィールドを持つユーザー名、パスワード、および電子メールと共に: ロックされており、承認します。 自動的に、分 (既定の設定は、10 分以内に無効なログイン試行が 5 の後にユーザーをロック) の指定した数値の中で指定された回数無効な資格情報を提供する場合に、ユーザーのロックアウトが作成します。 承認済みの状態は、新しいユーザーが、サイトにログオンする前に何らかのアクションが発生する必要があります、シナリオに役立ちます。 たとえば、ユーザーは、まず、電子メール アドレスを確認します。 または、ログインする前に、管理者が承認する必要があります。

ロックされた out または承認されていないユーザーがログインすることはできません、唯一の自然にこれらの状態をリセットする方法でしょうか。 ユーザーを管理するための Web コントロールがロックされており、これらの決定は、サイトごとに処理する必要があるために、一部の状態を承認または ASP.NET での組み込み機能が含まれません。 一部のサイトでは、すべての新しいユーザー アカウント (既定の動作) を自動的に承認があります。 管理者がある他のユーザーのサインアップ時に提供された電子メール アドレスに送信リンクにアクセスするまでユーザーを承認するか、新しいアカウントを承認します。 同様に、管理者がその状態をリセットするまで、一部のサイトがユーザーをロックアウト可能性があります、そのアカウントのロック解除にアクセスできる他のサイトでは、URL を使用して、ロックアウトされたユーザーに電子メールを送信中にします。

このチュートリアルは、管理者が管理する web ページを構築する方法を示しています。 ユーザーのロックアウトと状態を承認します。 また、電子メール アドレスを確認、後にのみ、新しいユーザーを承認する方法もわかります。

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>手順 1: 管理ユーザーのロックアウトし、承認の状態

<a id="Tutorial12"> </a> [*インターフェイスを構築する 1 つのユーザー アカウントの選択に多くの*](building-an-interface-to-select-one-user-account-from-many-cs.md)チュートリアルは、ページの場合は、各ユーザー アカウントを一覧表示するページを構築 GridView をフィルター処理します。 グリッドには、各ユーザーの名前と電子メール、その承認済みおよびロックされた状態、現在オンラインいるかどうかおよびユーザーに関するコメントが一覧表示します。 管理ユーザーの承認し、状態をロックアウトすることもできますこのグリッド編集可能です。 ユーザーの承認済みの状態を変更するには、管理者は最初ユーザー アカウントを見つけてチェックまたは承認済みのチェック ボックスをオフにする、対応する GridView の行を編集します。 または、個別の ASP.NET ページから承認済みおよびロックされた状態を管理します。

このチュートリアルでは 2 つの ASP.NET ページを使用してみましょう。`ManageUsers.aspx`と`UserInformation.aspx`します。 この考え方は`ManageUsers.aspx`システムでは、ユーザー アカウントを一覧表示中に`UserInformation.aspx`により、管理者は、特定のユーザーの承認済みおよびロックされた状態を管理します。 最初の注文のビジネスで GridView を拡張する`ManageUsers.aspx`に含める、内では、リンクの列として表示されます。 各リンク先にする`UserInformation.aspx?user=UserName`ここで、 *UserName*を編集するユーザーの名前を指定します。

> [!NOTE]
> コードをダウンロードしている場合、 <a id="Tutorial13"> </a> [ *Recovering とパスワードを変更する*](recovering-and-changing-passwords-cs.md)チュートリアルはお気付きかもしれませんが、`ManageUsers.aspx`ページは、一連の既に含まれています"。"リンクを管理し、`UserInformation.aspx`ページ選択したユーザーのパスワードを変更するためのインターフェイスを提供します。 メンバーシップ API を回避することと、ユーザーのパスワードを変更する SQL Server データベースを直接操作して、動作したので、このチュートリアルに関連付けられているコードでは、その機能をレプリケートしないようにしました。 このチュートリアルを使用して最初から開始、`UserInformation.aspx`ページ。


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>追加「管理」へのリンク、`UserAccounts`GridView

開く、`ManageUsers.aspx`ページおよびを内の追加、 `UserAccounts` GridView。 セット内の`Text`"manage"プロパティとその`DataNavigateUrlFields`と`DataNavigateUrlFormatString`プロパティを`UserName`と"UserInformation.aspx?user={0}"、それぞれします。 これらの設定は、テキスト"Manage"のハイパーリンクのすべての表示が各リンクは、適切なで合格するように、内を構成*UserName*に、クエリ文字列値。

GridView に追加すると、内、時間を表示するがかかる、`ManageUsers.aspx`ブラウザーを使用してページ。 図 1 に示す GridView 各行にはようになりました「管理」リンクが含まれます。 Bruce の [管理] リンクを指す`UserInformation.aspx?user=Bruce`、Dave の [管理] リンクを指す`UserInformation.aspx?user=Dave`します。


[![内に追加します。](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**図 1**: 内の各ユーザー アカウントの [管理] リンクを追加します ([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image3.png))。


ユーザー インターフェイスを作成しコードは、`UserInformation.aspx`みましょうトークで、時点が最初のページについて、ユーザーをプログラムで変更する方法のロックアウトし、承認の状態。 [ `MembershipUser`クラス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)が[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)と[`IsApproved`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx)します。 `IsLockedOut`プロパティは読み取り専用です。 プログラムでユーザーをロックアウトするメカニズムはありません。ユーザーのロックを解除するには、使用、`MembershipUser`クラスの[`UnlockUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)します。 `IsApproved`プロパティは読み取りも書き込み可能です。 このプロパティに変更を保存する必要がありますを呼び出す、`Membership`クラスの[`UpdateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)、変更を渡して、`MembershipUser`オブジェクト。

`IsApproved`プロパティは読み取りも書き込み可能な CheckBox コントロールは、このプロパティを構成するための最適なユーザー インターフェイス要素では可能性があります。 ただし、チェック ボックスに対しては機能しません、`IsLockedOut`プロパティをユーザーのロック解除のみ可能性があります彼女管理者は、ユーザーをロックアウトことはできません、ためです。 ための適切なユーザー インターフェイス、`IsLockedOut`プロパティは、ボタンをクリックすると、ユーザー アカウントのロックを解除します。 このボタンは、ユーザーがロックアウトされた場合にのみ有効です。

### <a name="creating-theuserinformationaspxpage"></a>作成、`UserInformation.aspx`ページ

ユーザー インターフェイスを実装する準備ができました`UserInformation.aspx`します。 このページを開き、次の Web コントロールを追加します。

- クリックすると、ハイパーリンク コントロールによって、管理者を返します、`ManageUsers.aspx`ページ。
- 選択したユーザーの名前を表示するためのラベルの Web コントロール。 このラベルの設定`ID`に`UserNameLabel`クリアとその`Text`プロパティ。
- という名前のチェック ボックス コントロール`IsApproved`します。 設定の`AutoPostBack`プロパティを`true`します。
- ユーザーを表示するためのラベル コントロールでは日付最後のロックアウトします。 このラベルの名前を付けます`LastLockedOutDateLabel`クリアとその`Text`プロパティ。
- ユーザーのロックを解除するためのボタン。 このボタンを名前`UnlockUserButton`設定とその`Text`プロパティを「ユーザーのロックを解除」します。
- 「ユーザーの承認済みの状態が更新されました」など、ステータス メッセージを表示するためのラベル コントロール このコントロールの名前を付けます`StatusMessage`チェック ボックスをオフにその`Text`プロパティ、およびセットの`CssClass`プロパティを`Important`します。 (、`Important`で CSS クラスが定義されている、`Styles.css`スタイル シート ファイルは、大規模な赤いフォントで、対応するテキストが表示されます)。

これらのコントロールを追加すると、Visual Studio でデザイン ビューは図 2 でスクリーン ショットのようになります。


[![UserInformation.aspx のユーザー インターフェイスを作成します。](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**図 2**: ユーザー インターフェイスを作成`UserInformation.aspx`([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image6.png))。


完全なユーザー インターフェイスでは、次のタスクを設定する、`IsApproved`チェック ボックスとその他のコントロールが選択したユーザーの情報に基づいています。 ページのイベント ハンドラーを作成`Load`イベントし、次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

上記のコードは、ページおよび以降のポストバックではないに初めてアクセスであることを確認して開始します。 これは、後、を通じて渡されるユーザー名を読み取ります、`user`クエリ文字列フィールドを使用して、そのユーザー アカウントに関する情報を取得し、`Membership.GetUser(username)`メソッド。 クエリ文字列を通じてユーザー名が指定されていないか、管理者に送信される場合は、指定されたユーザーが見つかりませんでした、`ManageUsers.aspx`ページ。

`MembershipUser`オブジェクトの`UserName`に表示される値、`UserNameLabel`と`IsApproved`チェック ボックスをオンに基づき、`IsApproved`プロパティの値。

`MembershipUser`オブジェクトの[`LastLockoutDate`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx)を返します、`DateTime`ロックされている場合、ユーザーが最後を示す値。ユーザーがロックアウトされていることはない場合、返される値は、メンバーシップ プロバイダーによって異なります。 新しいアカウントが作成されたときに、`SqlMembershipProvider`設定、`aspnet_Membership`テーブルの`LastLockoutDate`フィールドを`1754-01-01 12:00:00 AM`します。 上記のコードで空の文字列が表示されます、`LastLockoutDateLabel`場合、`LastLockoutDate`プロパティが 1 年前に発生 2000。 それ以外の日付部分、`LastLockoutDate`ラベルのプロパティが表示されます。 `UnlockUserButton'` S`Enabled`ロックの状態、つまり、ユーザーがロックアウトされた場合は、このボタンを有効のみがユーザーのプロパティが設定されます。

テストする少し、`UserInformation.aspx`ページがブラウザーを使用します。 もちろんで開始する必要がありますが、`ManageUsers.aspx`を管理するユーザー アカウントを選択します。 到着時に`UserInformation.aspx`、なお、`IsApproved`ユーザーが承認された場合、チェック ボックスをオンのみです。 ユーザーがロックアウトされていることを最後の日付がロックアウトが表示されます。 ユーザーが現在ロックアウトされる場合にのみ、ユーザーのロックを解除 ボタンが有効にします。オンまたはオフ、`IsApproved`チェック ボックスをオンまたはユーザーのロックを解除 ボタンをクリックしてが、ポストバックの原因が、変更がまったく加え、ユーザー アカウントにまだため、これらのイベントのイベント ハンドラーを作成します。

Visual Studio に戻り、イベント ハンドラーを作成、`IsApproved`チェック ボックスの`CheckedChanged`イベントと`UnlockUser`ボタンの`Click`イベント。 `CheckedChanged`イベント ハンドラーでは、設定、ユーザーの`IsApproved`プロパティを`Checked`プロパティのチェック ボックスをオンにしてから、呼び出しに変更を保存`Membership.UpdateUser`します。 `Click`イベント ハンドラーでは、単に呼び出し、`MembershipUser`オブジェクトの`UnlockUser`メソッド。 両方のイベント ハンドラー内で適切なメッセージを表示、`StatusMessage`ラベル。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>テスト、`UserInformation.aspx`ページ

これらのイベント ハンドラーで、ページに再アクセスおよび未承認ユーザー。 図 3 に示すようメッセージのことを示すページで、ユーザーの概要が表示されます`IsApproved`プロパティが正常に変更します。


[![Chris は未承認されました](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**図 3**: Chris は未承認されました ([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image9.png))。


次に、ログアウトし、アカウントを持つユーザーとしてログインしてください許可されませんでしただけです。 ユーザーが承認されていないため、ログインできません。 既定では、Login コントロールでは、理由に関係なく、ユーザーがログインできない場合に、同じメッセージが表示されます。 しかし、 <a id="Tutorial6"> </a> [*を検証するユーザーの資格情報に対して、メンバーシップ ユーザー ストア*](../membership/validating-user-credentials-against-the-membership-user-store-cs.md)チュートリアルがより適切なメッセージを表示するログインの制御を強化しました。 図 4 に示す、Chris が自分のアカウントがまだ承認されていないため、彼ログインできないことを説明するメッセージが表示されます。


[![Chris できないログインのため His アカウントが未承認](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**図 4**: Chris できないログインのため His アカウントが未承認 ([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image12.png))。


ロックアウトの機能をテストするには、承認済みのユーザーとしてログインしますが、正しくないパスワードを使用を試みます。 このプロセスに必要なユーザーのアカウントがロックアウトされてまでの時間数を繰り返します。Login コントロールは、カスタムの表示も更新されたメッセージの場合は、ロックアウトされたアカウントからログインしようとしています。 ログイン ページで、次のメッセージの表示を開始すると、アカウントがロックアウトされていることがわかって:"自分のアカウントがロックアウトされて無効なログインの回数が多すぎるためです。 管理者に問い合わせてください、アカウントのロックを解除します。"

戻り、`ManageUsers.aspx`ページし、ロックアウトされたユーザーの管理 リンクをクリックします。 図 5 に示すように値が表示する必要があります、`LastLockedOutDateLabel`ユーザーのロックを解除 ボタンを有効にする必要があります。 ユーザー アカウントのロックを解除するユーザーのロックを解除 ボタンをクリックします。 ユーザーのロックを解除すると、再度ログインできるされます。


[![Dave は、システムからロックアウトされている](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**図 5**: Dave がされているロックのうちシステム ([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image15.png))。


## <a name="step-2-specifying-new-users-approved-status"></a>手順 2: 新しいユーザーを指定する状態を承認

承認済みの状態は、何らかのアクションを新しいユーザーがログインして、サイトのユーザーに固有の機能にアクセスできるようにする前に実行するシナリオで役立ちます。 たとえば、ログインとサインアップのページを除く、すべてのページが認証されたユーザーのみがアクセスできるプライベート web サイト実行される可能性があります。 ただし、知らない人が、web サイトに達した場合、サインアップ ページを検索し、アカウントを作成しますか。 これを防ぐへのサインアップ ページを移動する可能性があります、`Administration`フォルダー、および管理者が各アカウントを手動で作成する必要があります。 または、だれもが、サインアップを許可することが管理者ユーザー アカウントを承認するまで、サイトへのアクセスを禁止できます。

既定では、CreateUserWizard コントロールは、新しいアカウントを承認します。 コントロールを使用してこの動作を構成する[`DisableCreatedUser`プロパティ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)します。 このプロパティを設定`true`新しいユーザー アカウントを承認しません。

> [!NOTE]
> 既定で CreateUserWizard コントロールは、新しいユーザー アカウントで自動的に記録します。 この動作は、コントロールの規定[`LoginCreatedUser`プロパティ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)します。 未承認ユーザーが、サイトにログインできないため、ときに`DisableCreatedUser`は`true`新しいユーザー アカウントがの値に関係なく、サイトにログインしていない、`LoginCreatedUser`プロパティ。


プログラムで使用して、新しいユーザー アカウントを作成している場合、`Membership.CreateUser`メソッドは、未承認のユーザー アカウントを作成するを使用して、新しいユーザーの同意をオーバー ロードの 1 つ`IsApproved`入力パラメーターとしてプロパティ値。

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>手順 3: 電子メール アドレスを確認することでユーザーを承認します。

登録するときに、指定した電子メール アドレスを確認するまで、ユーザー アカウントをサポートする多くの web サイトは新しいユーザーを承認されません。 この検証プロセスは、通常、検証済みの一意の電子メール アドレスが必要ですし、サインアップ プロセスで、追加の手順を追加します。 ボット、スパム、およびその他の役立たずを阻止するために使用されます。 このモデルでは、新しいユーザーはサインアップ時に送信されます検証ページへのリンクを含む電子メール メッセージ。 リンクにアクセスして、ユーザーは、電子メールを受信して、有効なそのためが電子メール アドレスを指定することを実証されました。 確認 ページは、ユーザーの承認を担当します。 これが発生する、自動的にこのページに到達できるすべてのユーザーを承認するため、ユーザーがなどいくつか追加の情報を提供した後にのみ、または、 [CAPTCHA](http://en.wikipedia.org/wiki/Captcha)します。

このワークフローを対応するためには、まず、アカウント作成ページを更新すると、新しいユーザーが承認されていない必要があります。 開く、`EnhancedCreateUserWizard.aspx`ページで、`Membership`フォルダーおよび CreateUserWizard コントロールをセット`DisableCreatedUser`プロパティを`true`します。

次に、自分のアカウントを確認する方法の手順で新しいユーザーに電子メールを送信する CreateUserWizard コントロールを構成する必要があります。 電子メールに記載のリンクを含める具体的には、`Verification.aspx`ページ (まだこれを作成)、新しいユーザーに渡して、`UserId`を通じて、クエリ文字列。 `Verification.aspx`ページは、指定されたユーザーを検索し、承認、としてマークします。

### <a name="sending-a-verification-email-to-new-users"></a>新しいユーザーに確認メールを送信します。

CreateUserWizard コントロールから、電子メールを送信する次のように構成します。 その`MailDefinition`プロパティ適切にします。 説明したように、 <a id="Tutorial13"> </a>[前のチュートリアル](recovering-and-changing-passwords-cs.md)、ChangePassword と PasswordRecovery コントロールを[`MailDefinition`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)と同様に動作します。CreateUserWizard コントロールの。

> [!NOTE]
> 使用する、`MailDefinition`メールの配信を指定する必要があるプロパティ オプション`Web.config`します。 詳細についてを参照してください[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)します。


という名前の新しい電子メール テンプレートを作成して開始`CreateUserWizard.txt`で、`EmailTemplates`フォルダー。 テンプレートについては、次のテキストを使用します。

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

設定、 `MailDefinition'` s`BodyFileName`プロパティを"~/EmailTemplates/CreateUserWizard.txt"とその`Subject`プロパティを"自分の web サイトへようこそ! アクティブ化してください、アカウントです。"

なお、`CreateUserWizard.txt`電子メール テンプレートが含まれています、`<%VerificationUrl%>`プレース ホルダーです。 これは、ような場合の URL、`Verification.aspx`ページに配置されます。 CreateUserWizard が自動的に置き換えられます、`<%UserName%>`と`<%Password%>`プレース ホルダーを新しいアカウントのユーザー名とパスワード、組み込みはありませんが、`<%VerificationUrl%>`プレース ホルダーです。 適切な検証 URL を使用して手動で置き換える必要があります。

これを実現するには、CreateUserWizard のイベント ハンドラーを作成[`SendingMail`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx)し、次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail`イベントが発生した後、`CreatedUser`イベント、上記のイベント ハンドラーは、新しいユーザーを実行するとき、アカウントが既に作成されたことを意味します。 新しいユーザーのアクセス`UserId`呼び出すことによって値、`Membership.GetUser`に渡して、メソッド、 `UserName` CreateUserWizard コントロールに入力します。 次に、検証 URL が形成されます。 ステートメント`Request.Url.GetLeftPart(UriPartial.Authority)`を返します、 `http://yourserver.com` ; URL の部分`Request.ApplicationPath`を返します。 パスが、アプリケーションのルートが指定されています。 URL として定義し、検証`Verification.aspx?ID=userId`です。 これら 2 つの文字列は、完全な URL をフォームに連結されます。 最後に、電子メール メッセージの本文 (`e.Message.Body`) が出現するすべての`<%VerificationUrl%>`の完全な URL に置き換えられます。

実質的な効果は、ある新しいユーザー承認されていない、つまりことは、サイトにログインすることはできません。 さらに、自動的に送信される電子メールのリンクを検証 URL (図 6 参照)。


[![新しいユーザー検証 URL へのリンクを電子メールを受信します。](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**図 6**: 新しいユーザー検証 URL へのリンクを電子メールの受信 ([フルサイズの画像を表示する をクリックします](unlocking-and-approving-user-accounts-cs/_static/image18.png))。


> [!NOTE]
> CreateUserWizard コントロールの既定の CreateUserWizard の手順では、自分のアカウントが作成され、[続行] ボタンを表示、ユーザーに通知するメッセージが表示されます。 これをクリックすると、ユーザーをコントロールの指定された URL が`ContinueDestinationPageUrl`プロパティ。 CreateUserWizard`EnhancedCreateUserWizard.aspx`新しいユーザーに送信するように構成、`~/Membership/AdditionalUserInfo.aspx`出身、ホームページの URL、および署名のユーザーが求められます。 この情報できますのみ追加するためログオン ユーザー、理にかなってサイトのホーム ページにユーザーを送信するには、このプロパティを更新 (`~/Default.aspx`)。 さらに、`EnhancedCreateUserWizard.aspx`検証電子メールが送信された、まで、このメールの指示に従いますが、自分のアカウントをアクティブ化されませんユーザーに通知するページまたは CreateUserWizard の手順を拡張する必要があります。 リーダーの練習としてこのような変更のままは。


### <a name="creating-the-verification-page"></a>確認ページを作成します。

最後のタスクが作成するには、`Verification.aspx`ページ。 このページを関連付けることで、ルート フォルダーに追加、`Site.master`マスター ページ。 ほとんどのサイトに追加する前のコンテンツ ページを参照するコンテンツ コントロールを削除、 `LoginContent` ContentPlaceHolder コンテンツ ページで、マスター ページを使用するよう既定のコンテンツ。

ラベル Web コントロールを追加、`Verification.aspx`設定ページで、その`ID`に`StatusMessage`しその text プロパティをオフにします。 次に、作成、`Page_Load`イベント ハンドラーを次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

上記のコードの大部分を検証する、 `UserId` 、クエリ文字列が存在する、有効であることを指定した`Guid`値、および既存のユーザー アカウントを参照していること。 ユーザー アカウントが承認されたすべてのこれらのチェックを渡す場合それ以外の場合、適切な状態メッセージが表示されます。

図 7 に示します、`Verification.aspx`ページをブラウザーからアクセスする場合。


[![新しいユーザーのアカウントが承認されるようになりました](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**図 7**: [新しいユーザーのアカウントが承認されるようになりました ([フルサイズの画像を表示する] をクリックします](unlocking-and-approving-user-accounts-cs/_static/image21.png))。


## <a name="summary"></a>まとめ

すべてのメンバーシップ ユーザーのアカウントがあるユーザーがサイトにログインできるかどうかを決定する 2 つの状態:`IsLockedOut`と`IsApproved`します。 これらのプロパティの両方があります`true`ユーザーがログインするためです。

ユーザーのロックアウト状態は、ブルート フォース攻撃の方法でサイトに重大なハッカーの可能性を低く、セキュリティ対策として使用されます。 具体的には、ユーザーがロックアウト時間帯内で無効なログイン試行数がある場合。 これらの境界は、メンバーシップ プロバイダーの設定で使用して構成できます`Web.config`します。

承認済みの状態は、新しいユーザーが何らかのアクションが指定されるまでログ記録することを禁止するための手段として通常使用されます。 おそらく、サイトでは最初に新しいアカウントを管理者が承認することが必要ですか、電子メール アドレスを確認し、手順 3. で説明したようにします。

満足のプログラミングです。

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](recovering-and-changing-passwords-cs.md)
> [次へ](building-an-interface-to-select-one-user-account-from-many-vb.md)
