---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: ロックを解除して、ユーザー アカウント (c#) の承認 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルは、管理する管理者用 web ページを構築する方法を示しています。 ユーザーのロックアウトとステータスを承認します。 表示されています。 新しいユーザー o を承認する方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b3d69e96513192bc73a2a6a7cbb4c6e33eb610b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891400"
---
<a name="unlocking-and-approving-user-accounts-c"></a>ユーザー アカウントのロック解除および承認を行う (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> このチュートリアルは、管理する管理者用 web ページを構築する方法を示しています。 ユーザーのロックアウトとステータスを承認します。 ユーザーの電子メール アドレスが検証されてからにのみ、新しいユーザーを承認する方法も表示されます。


## <a name="introduction"></a>はじめに

と共に、ユーザー名、パスワード、および電子メールの場合は、各ユーザー アカウントが、ユーザーがサイトにログインできるかどうかを指定する 2 つの状態フィールド: ロックされており、承認します。 自動的に、資格情報が無効 (既定の設定は、ユーザーを 10 分以内に 5 つの無効なログイン試行後をロックアウト) 指定された分数内で指定された回数を指定する場合に、ユーザーのロックアウトが作成。 承認済みの状態は、何らかのアクションが、新しいユーザーが、サイトにログオンする前に経過する必要のシナリオで役立ちます。 たとえば、ユーザーは、まず、電子メール アドレスを確認または管理者がログインする前に承認する必要があります。

ロックされた out または承認されていないユーザーがログインできないのはのみ自然にこれらのステータスのリセット方法不思議に思えるかもしれません。 ASP.NET が組み込みの機能をすべてを含んでいないか、ユーザーを管理するための Web コントロールがロックされており、これらの決定は、サイトごとに処理する必要があるために、一部のステータスを承認します。 一部のサイトでは、すべての新しいユーザー アカウント (既定の動作) を自動的に承認があります。 管理者がある他のユーザーを新しいアカウントを承認またはサインアップするときに指定した電子メール アドレスに送信されるリンクにアクセスするまでにユーザーを承認されません。 同様に、管理者がその状態をリセットするまで、ユーザーが一部のサイトがロックされることが、自分のアカウントのロックを解除するアクセスできる他のサイトでは、URL を使用してロックアウトされたユーザーに電子メールを送信中にします。

このチュートリアルは、管理する管理者用 web ページを構築する方法を示しています。 ユーザーのロックアウトとステータスを承認します。 ユーザーの電子メール アドレスが検証されてからにのみ、新しいユーザーを承認する方法も表示されます。

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>手順 1: 管理ユーザーのロックアウトし、承認の状態

<a id="Tutorial12"> </a> [*多くから 1 つのユーザー アカウントを選択するインターフェイスを構築*](building-an-interface-to-select-one-user-account-from-many-cs.md)チュートリアル、ページ内の各ユーザー アカウントを一覧表示するページを構築して GridView をフィルター処理します。 グリッドには、各ユーザーの名前と電子メール、その承認済みロックアウト状態を現在オンラインいるかどうかおよびユーザーに関するコメントが一覧表示します。 管理ユーザーの承認し、ステータスをロックアウトすることもこのグリッド編集可能です。 ユーザーの承認済みの状態を変更するには、管理者は最初にユーザー アカウントを見つけてチェックまたは承認済みのチェック ボックスをオフにする、対応する GridView の行を編集します。 また、でした管理するには、個別の ASP.NET ページから承認済みとロックアウト状態です。

このチュートリアルでは 2 つの ASP.NET ページを使用してみましょう。`ManageUsers.aspx`と`UserInformation.aspx`です。 という考え方です`ManageUsers.aspx`システムでは、ユーザー アカウントを一覧表示中に`UserInformation.aspx`により、管理者は、承認されたロックアウトの状態を特定のユーザーを管理します。 GridView を拡張するために、最初の注文のビジネスは、`ManageUsers.aspx`に含める、内では、リンクの列として表示されます。 指す各リンクを選びました`UserInformation.aspx?user=UserName`ここで、 *UserName*を編集するユーザーの名前を指定します。

> [!NOTE]
> コードをダウンロードしている場合、 <a id="Tutorial13"> </a> [ *Recovering とパスワードの変更*](recovering-and-changing-passwords-cs.md)お気付きするチュートリアル、`ManageUsers.aspx`ページは、一連の既に含まれています"。"リンクを管理し、`UserInformation.aspx`ページは、選択したユーザーのパスワードを変更するためのインターフェイスを提供します。 メンバーシップ API を回避する方法と、ユーザーのパスワードを変更する SQL Server データベースと直接動作で機能したため、このチュートリアルに関連付けられているコードにこの機能をレプリケートしないようにしました。 このチュートリアルを最初から開始、`UserInformation.aspx`ページ。


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>追加する"Manage"へのリンク、`UserAccounts`GridView

開く、`ManageUsers.aspx`ページおよびを内の追加、 `UserAccounts` GridView です。 セット内の`Text`プロパティを"Manage"に、その`DataNavigateUrlFields`と`DataNavigateUrlFormatString`プロパティを`UserName`と"UserInformation.aspx?user={0}"、それぞれします。 これらの設定を構成、内など、ハイパーリンクのすべての表示テキスト"Manage"が、適切な各リンクに渡します*ユーザー名*に、クエリ文字列値です。

GridView に、内を追加した後すぐを表示、`ManageUsers.aspx`ブラウザーを使用してページ。 図 1 に示す各 GridView の行が"Manage"リンクには含まれています。 ブルースを"Manage"リンクを指す`UserInformation.aspx?user=Bruce`"Manage"Dave のリンクに対し、`UserInformation.aspx?user=Dave`です。


[![内に追加します。](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**図 1**: 内の各ユーザー アカウントを"Manage"リンクを追加 ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image3.png))


ユーザー インターフェイスを作成し、コードには、`UserInformation.aspx`トークみましょう時点が最初のページについてユーザーをプログラムで変更する方法のロックアウトし、ステータスを承認します。 [ `MembershipUser`クラス](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)が[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)と[`IsApproved`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx)です。 `IsLockedOut`プロパティは読み取り専用です。 プログラムからユーザーをロックアウトするメカニズムはありません。ユーザーのロックを解除するには、`MembershipUser`クラスの[`UnlockUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)です。 `IsApproved`プロパティは読み取り可能な値を設定します。 このプロパティに任意の変更を保存する必要がありますを呼び出して、`Membership`クラスの[`UpdateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx)、変更を渡して、`MembershipUser`オブジェクト。

`IsApproved`プロパティが読み取り/書き込み可能で、CheckBox コントロールは、このプロパティを構成するための最適なユーザー インターフェイス要素では可能性があります。 ただし、チェック ボックスは適していません、`IsLockedOut`プロパティのため、管理者は、ユーザーをロックアウトことはできません、彼女がのみロックを解除ユーザー。 ための適切なユーザー インターフェイス、`IsLockedOut`プロパティはボタンをクリックすると、ユーザー アカウントのロックを解除します。 このボタンは、ユーザーがロックアウトされた場合にのみ有効です。

### <a name="creating-theuserinformationaspxpage"></a>作成する、`UserInformation.aspx`ページ

ユーザー インターフェイスを実装する準備が整いました`UserInformation.aspx`です。 このページを開き、次の Web コントロールを追加します。

- クリックすると、ハイパーリンク コントロールによって、管理者が返されます、`ManageUsers.aspx`ページ。
- 選択したユーザーの名前を表示するためのラベルの Web コントロールです。 このラベルの設定`ID`に`UserNameLabel`をクリアし、`Text`プロパティです。
- という名前のチェック ボックス コントロール`IsApproved`です。 設定の`AutoPostBack`プロパティを`true`です。
- ユーザーを表示するためのラベル コントロールでは日付最後のロックアウトします。 このラベルの名前を付けます`LastLockedOutDateLabel`をクリアし、`Text`プロパティです。
- ユーザーのロックを解除するためのボタンをクリックします。 このボタンの名前を付けます`UnlockUserButton`設定とその`Text`「ユーザーのロック解除」プロパティです。
- 「ユーザーの承認済みの状態が更新されました」など、ステータス メッセージを表示するためのラベル コントロール このコントロールの名前を付けます`StatusMessage`をクリア、その`Text`プロパティ、およびセットその`CssClass`プロパティを`Important`です。 (、`Important`で CSS クラスが定義されている、`Styles.css`スタイル シート ファイルを大規模な赤いフォントで、対応するテキストを表示します)。

これらのコントロールを追加すると、Visual Studio でデザイン ビューは図 2 でスクリーン ショットのようになります。


[![UserInformation.aspx のユーザー インターフェイスを作成します。](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**図 2**: ユーザー インターフェイスを作成します`UserInformation.aspx`([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image6.png))


完全なユーザー インターフェイスと、次のタスクを設定、 `IsApproved`  チェック ボックスおよびその他のコントロールは、選択したユーザーの情報に基づいています。 ページのイベント ハンドラーを作成`Load`イベントし、次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

上記のコードは、これは、ページおよび以降のポストバックいないに初めてアクセスであることを確認して起動します。 を通じて渡されるユーザー名を読み取り、 `user` querystring フィールドを使用してそのユーザー アカウントに関する情報を取得し、`Membership.GetUser(username)`メソッドです。 クエリ文字列を通じてユーザー名が指定されていないか、管理者に送信される場合は、指定したユーザーが見つかりませんでした、`ManageUsers.aspx`ページ。

`MembershipUser`オブジェクトの`UserName`に値が表示されます、`UserNameLabel`と`IsApproved`チェック ボックスがに基づいて、`IsApproved`プロパティの値。

`MembershipUser`オブジェクトの[`LastLockoutDate`プロパティ](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx)を返します、`DateTime`ロックされている場合、ユーザーが最後を示す値。ユーザーがロックアウトされていることはない場合、に、返される値は、メンバーシップ プロバイダーによって異なります。 新しいアカウントの作成時に、`SqlMembershipProvider`設定、`aspnet_Membership`テーブルの`LastLockoutDate`フィールドを`1754-01-01 12:00:00 AM`です。 上記のコードで空の文字列が表示されます、`LastLockoutDateLabel`場合、`LastLockoutDate`プロパティが 1 年前に発生 2000、それ以外の日付部分、`LastLockoutDate`ラベルでプロパティを表示します。 `UnlockUserButton'` S`Enabled`プロパティに設定、ユーザーの状態、つまりことは、このボタンのみ有効となる、ユーザーがロックアウトされた場合はロックアウトされます。

テストをとって、`UserInformation.aspx`ブラウザーを使用してページ。 もちろんで開始する必要がありますが、`ManageUsers.aspx`を管理するユーザー アカウントを選択します。 到着時に`UserInformation.aspx`、なお、`IsApproved`のみチェック ボックスをユーザーが承認された場合。 ユーザーがロックアウトされていること場合、最後の日付がロックアウトが表示されます。 ユーザーのロックを解除 ボタンは、ユーザーは現在ロックアウトされている場合にのみ有効です。オンまたはオフにして、`IsApproved`チェック ボックスまたは ユーザーのロックを解除 ボタンをクリックとポストバックしますが、変更は行われません、ユーザー アカウントにきたところためこれらのイベントのイベント ハンドラーを作成します。

Visual Studio に戻るし、イベント ハンドラーを作成、`IsApproved`チェック ボックスの`CheckedChanged`イベントおよび`UnlockUser`ボタンの`Click`イベント。 `CheckedChanged` 、イベント ハンドラーを設定、ユーザーの`IsApproved`プロパティを`Checked`プロパティのチェック ボックスの呼び出しに変更を保存し`Membership.UpdateUser`です。 `Click`イベント ハンドラー、単に呼び出し、`MembershipUser`オブジェクトの`UnlockUser`メソッドです。 両方のイベント ハンドラー内で適切なメッセージを表示する、`StatusMessage`ラベル。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>テスト、`UserInformation.aspx`ページ

場所でこれらのイベント ハンドラーで、ページを再検討し、承認されていないユーザーです。 図 3 では、メッセージのことを示すページで、ユーザーの概要が表示されます`IsApproved`プロパティが正常に変更します。


[![クリスは未承認されました](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**図 3**: クリスが未承認されました ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image9.png))


次に、ログアウトしようとしてアカウントを持つユーザーとしてログイン許可されませんでしただけです。 ユーザーが承認されていないため、ログインできません。 既定では、ログイン コントロールでは、理由に関係なく、ユーザーがログインできない場合に、同じメッセージが表示されます。 しかし、 <a id="Tutorial6"> </a> [*を検証するユーザーの資格情報に対してメンバーシップ ユーザー ストアでは、* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md)チュートリアルより適切なメッセージを表示するログイン コントロールの強化について説明しました。 図 4 に示す、Chris に自分のアカウントがまだ承認されていないため彼はログインできませんを説明するメッセージが表示されます。


[![Chris できませんログインのため His アカウントがある承認されていません。](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**図 4**: Chris できませんログインのため His アカウントがある承認されていない ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image12.png))


ロックアウトの機能をテストするには、承認されたユーザーとしてログインしますが、正しくないパスワードを使用して試みます。 このプロセスに必要な回数、ユーザーのアカウントがロックアウトされてまでを繰り返します。ログイン コントロールは、カスタムの表示も更新されたメッセージの場合はロックアウトされたアカウントからログインを試行します。 ログイン ページで、次のメッセージの表示を開始すると、アカウントがロックアウトされていることがわかって:"自分のアカウントがロックアウトされて無効なログイン失敗の回数が多すぎます。 管理者に問い合わせてくださいには、アカウントのロックを解除します。"

戻り、`ManageUsers.aspx`ページし、ロックアウトされたユーザーの管理 リンクをクリックします。 図 5 に示すように値を表示する必要があります、`LastLockedOutDateLabel`ユーザーのロックを解除 ボタンを有効にする必要があります。 ユーザー アカウントのロックを解除するユーザーのロックを解除 ボタンをクリックします。 ユーザーのロックを解除すると再度ログインできるようされます。


[![Dave がシステムからロックアウトされています。](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**図 5**: Dave がされたロックをシステムの ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>手順 2: 新しいユーザーを指定する状態を承認

承認済みの状態は、いくつか実行するアクションを新しいユーザーがログインして、サイトのユーザーに固有の機能にアクセスできるようにする前にしようとする場合に役立ちます。 など、ログインやサインアップ ページを除く、すべてのページが認証されたユーザーのみからアクセスできるプライベート web サイト実行する可能性があります。 サインアップ ページを検索し、アカウントが作成される知らない人が、web サイトに達するとどうなりますか。 これを防ぐには、サインアップ ページを移動する可能性があります、`Administration`フォルダーで、管理者は、各アカウントを手動で作成する必要があります。 代わりに、だれでもサインアップを許可することが管理者ユーザー アカウントに承認されるまで、サイトへのアクセスを禁止できます。

既定では、CreateUserWizard コントロールは、新しいアカウントを承認します。 使用して、コントロールの動作を構成する[`DisableCreatedUser`プロパティ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx)です。 このプロパティを設定`true`に新しいユーザー アカウントを承認されません。

> [!NOTE]
> 既定では CreateUserWizard コントロールは、自動的に新しいユーザー アカウントをログオンします。 この動作は、コントロールのによって決まります[`LoginCreatedUser`プロパティ](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)です。 未承認のユーザーが、サイトにログインできないためと`DisableCreatedUser`は`true`の値に関係なく、サイトに新しいユーザー アカウントがログインしていない、`LoginCreatedUser`プロパティです。


プログラムで使用して、新しいユーザー アカウントを作成する場合、`Membership.CreateUser`未承認のユーザー アカウントを作成する、メソッドを使用して、新しいユーザーを許容するオーバー ロードのいずれかの`IsApproved`入力パラメーターとしてプロパティ値。

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>手順 3: ユーザーの電子メール アドレスを確認することでユーザーを承認します。

登録するときに指定できるよう、電子メール アドレスを確認するまで、ユーザー アカウントをサポートする多くの web サイトは新しいユーザーを許可しないでください。 この検証プロセスは、通常、検証済みの一意の電子メール アドレスを必要とし、サインアップ プロセスで余分な手順を追加、bot、迷惑メール、およびその他の ne'er-do-wells を阻止するために使用されます。 このモデルでは、新しいユーザーがサインアップする時に送信される電子メール メッセージの検証 ページへのリンクが含まれていますが、します。 リンクにアクセスして、ユーザーは電子メールを受信して、そのため、メール アドレス指定する有効ではことを実証されました。 検証ページは、ユーザーを承認します。 これが発生する自動的に、これにより、このページに到達するすべてのユーザーを承認またはユーザーなどの追加情報がいくつかの提供後にのみ、 [CAPTCHA](http://en.wikipedia.org/wiki/Captcha)です。

このワークフローに合わせてには、新しいユーザーが承認されていないように、まず、アカウントの作成 ページを更新する必要があります。 開く、 `EnhancedCreateUserWizard.aspx`  ページで、`Membership`フォルダーと CreateUserWizard コントロールをセット`DisableCreatedUser`プロパティを`true`です。

次に、そのアカウントを検証する方法について記載された新しいユーザーに電子メールを送信する CreateUserWizard コントロールを構成する必要があります。 電子メールのリンクを含めるお具体的には、`Verification.aspx`ページ (きたところを作成)、新しいユーザーに渡して、`UserId`クエリ文字列を通じてします。 `Verification.aspx`  ページを指定したユーザーを検索し、承認にマークします。

### <a name="sending-a-verification-email-to-new-users"></a>新しいユーザーに確認の電子メールを送信します。

CreateUserWizard コントロールから電子メールを送信する次のように構成します。 その`MailDefinition`プロパティ適切にします。 説明したように、 <a id="Tutorial13"> </a>[前のチュートリアル](recovering-and-changing-passwords-cs.md)、ChangePassword と PasswordRecovery コントロールに含まれる、 [ `MailDefinition`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx)と同じ方法で動作します。CreateUserWizard コントロールのです。

> [!NOTE]
> 使用する、`MailDefinition`でメールの配信を指定する必要があります。 プロパティのオプション`Web.config`です。 詳細についてを参照してください[ASP.NET で電子メールを送信する](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)です。


まず、という名前の新しい電子メール テンプレートを作成して`CreateUserWizard.txt`で、`EmailTemplates`フォルダーです。 テンプレートの次のテキストを使用します。

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

設定、 `MailDefinition'` s`BodyFileName`プロパティを"~/EmailTemplates/CreateUserWizard.txt"およびその`Subject`プロパティを"My web サイトへようこそ! アクティブ化してください、アカウントです。"

なお、`CreateUserWizard.txt`電子メール テンプレートが含まれています、`<%VerificationUrl%>`プレース ホルダーです。 これは、ような場合の URL、`Verification.aspx`ページに配置されます。 CreateUserWizard が自動的に置き換えられます、`<%UserName%>`と`<%Password%>`プレース ホルダーを新しいアカウントのユーザー名とパスワードで組み込みはありませんが、`<%VerificationUrl%>`プレース ホルダーです。 手動で置き換えて、適切な検証 URL が必要です。

これを実現するには、CreateUserWizard のイベント ハンドラーを作成[`SendingMail`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx)し、次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

`SendingMail`イベントが発生した後、`CreatedUser`イベント、上記のイベント ハンドラーが、新しいユーザーを実行するとき、アカウントが既に作成されたことを意味します。 新しいユーザーのアクセスできる`UserId`を呼び出して値、`Membership.GetUser`に渡して、メソッド、 `UserName` CreateUserWizard コントロールに入力します。 次に、検証 URL が形成されます。 ステートメント`Request.Url.GetLeftPart(UriPartial.Authority)`を返します、 `http://yourserver.com` ; URL の部分`Request.ApplicationPath`を返します。 パスが、アプリケーションのルートが指定されています。 URL として定義し、検証`Verification.aspx?ID=userId`です。 これら 2 つの文字列は、完全な URL を形成し、連結されます。 最後に、電子メール メッセージの本文 (`e.Message.Body`) が出現するすべての`<%VerificationUrl%>`完全な URL に置き換えられます。

実際の影響は、新しいユーザーがないことを承認されているサイトにログインできないことを意味です。 さらに、自動的に送信されるリンクを含む電子メール検証 URL (図 6 を参照してください)。


[![新しいユーザーが検証 URL へのリンクを電子メールを受け取る](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**図 6**: 新しいユーザーが検証 URL へのリンクを電子メールを受け取ります ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> CreateUserWizard コントロールの既定の CreateUserWizard ステップには、自分のアカウントが作成され、[続行] ボタンの表示をユーザーに知らせるメッセージが表示されます。 これをクリックすると、ユーザーをコントロールの指定された URL が`ContinueDestinationPageUrl`プロパティです。 CreateUserWizard`EnhancedCreateUserWizard.aspx`新しいユーザーに送信するように構成、 `~/Membership/AdditionalUserInfo.aspx`hometown、ホームページの URL と署名のユーザーを要求します。 この情報のみ要素を追加できログオン ユーザー、意味をユーザーに返信するサイトのホーム ページには、このプロパティを更新する (`~/Default.aspx`)。 さらに、`EnhancedCreateUserWizard.aspx`検証電子メールが送信されましたが、このメールの手順を実行するまでは、自分のアカウントをアクティブ化されませんユーザーに通知するページまたは CreateUserWizard ステップを補完する必要があります。 これらの変更は、リーダーの課題として退出です。


### <a name="creating-the-verification-page"></a>確認ページを作成します。

最後のタスクが作成するには、`Verification.aspx`ページ。 このページを関連付けることで、ルート フォルダーに追加、`Site.master`マスター ページ。 ほとんどのサイトに追加する前のコンテンツ ページで実行した、としてを参照するコンテンツ コントロールを削除、 `LoginContent` ContentPlaceHolder コンテンツ ページで、マスター ページを使用するよう既定のコンテンツ。

ラベル Web コントロールを追加、`Verification.aspx`設定 ページで、その`ID`に`StatusMessage`し、text プロパティをオフにします。 次に、作成、`Page_Load`イベント ハンドラーを次のコードを追加します。

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

上記のコードの大部分であることを確認、 `UserId` 、クエリ文字列が存在し、有効であることを指定された`Guid`値、および既存のユーザー アカウントを参照しています。 これらのチェックすべてを渡すと、ユーザー アカウントが承認されます。それ以外の場合、適切なステータス メッセージが表示されます。

図 7 は、`Verification.aspx`ページをブラウザーからアクセスする場合。


[![新しいユーザーのアカウントが承認されるようになりました](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**図 7**: の新しいユーザーのアカウントが承認されるようになりました ([フルサイズのイメージを表示するをクリックして](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>まとめ

すべてのメンバーシップ ユーザーのアカウントがあるユーザーが、サイトにログインできるかどうかを決定する 2 つの状態:`IsLockedOut`と`IsApproved`です。 これらのプロパティの両方があります`true`ユーザーがログインするためです。

ユーザーのロックアウト状態はブルート フォースのメソッドを使用するサイトに互換性に影響するハッカーの可能性を減らすため、セキュリティ対策として使用します。 具体的には、ユーザーはロックアウト時間の特定のウィンドウ内で無効なログイン試行数がある場合。 これらの境界は、メンバーシップ プロバイダーの設定によって構成可能な`Web.config`します。

承認済みの状態はで何らかのアクションが指定されるまでのログ記録を新しいユーザーを禁止するための手段としてよく使用されます。 おそらく、サイトでは最初に新しいアカウントを管理者によって承認することが必要ですか、ユーザーの電子メール アドレスを確認し、手順 3. で示したようにします。

満足プログラミング!

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>特別に感謝しています.

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](recovering-and-changing-passwords-cs.md)
> [次へ](building-an-interface-to-select-one-user-account-from-many-vb.md)
