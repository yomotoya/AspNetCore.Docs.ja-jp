---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: 回復およびパスワード (c#) の変更 |Microsoft Docs
author: rick-anderson
description: ASP.NET には、回復、およびパスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールを使用すると、彼の失われた pa を回復するビジターをしています.
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: c04ed8ae18a3739f5519e30dea7768b8f6c7c7ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839010"
---
<a name="recovering-and-changing-passwords-c"></a>回復およびパスワード (c#) の変更
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET には、回復、およびパスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールを使用すると、彼が紛失したパスワードを回復する訪問者。 ChangePassword コントロールは、自分のパスワードを更新するユーザーを使用できます。 このチュートリアル シリーズで、PasswordRecovery 全体でその他のログインに関連する Web コントロールと同様に説明しましたし、ChangePassword がバック グラウンドでユーザーのパスワードをリセットまたは変更メンバーシップ フレームワークで作業を制御します。


## <a name="introduction"></a>はじめに

マイ銀行、公益事業会社、電話会社、電子メール アカウント、およびパーソナライズされた web ポータルの web サイトの間で、ほとんどの人がある多数の異なるパスワードを覚えておきます。 最近を記憶する非常に多くの資格情報で、パスワードを忘れることは珍しいはありません。 このアカウントには、ユーザー アカウントを提供している web サイトをユーザーが自分のパスワードを回復する方法を含める必要があります。 通常、このプロセスには、新しいランダム パスワードを生成して、ファイル上のユーザーの電子メール アドレスに電子メールで送信が含まれます。 新しいパスワードを受信した後は、ほとんどのユーザーは、サイトに戻り、覚えやすい 1 つをランダムに生成された 1 つから自分のパスワードを変更します。

ASP.NET には、回復、およびパスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールを使用すると、彼が紛失したパスワードを回復する訪問者。 ChangePassword コントロールは、自分のパスワードを更新するユーザーを使用できます。 このチュートリアル シリーズで、PasswordRecovery 全体でその他のログインに関連する Web コントロールと同様に説明しましたし、ChangePassword がバック グラウンドでユーザーのパスワードをリセットまたは変更メンバーシップ フレームワークで作業を制御します。

このチュートリアルでは、これら 2 つのコントロールを見ていきます。 プログラムで変更し、使用して、ユーザーのパスワードをリセットする方法もわかる、`MembershipUser`クラスの`ChangePassword`と`ResetPassword`メソッド。

## <a name="step-1-helping-users-recover-lost-passwords"></a>手順 1: 支援ユーザーの回復パスワードを紛失

ユーザー アカウントをサポートするすべての web サイトは、ユーザー パスワードを忘れた場合に復旧するためのいくつかのメカニズムを提供する必要があります。 良い知らせでは、ASP.NET でこのような機能の実装は PasswordRecovery Web コントロールに協力してくれた簡単であることです。 PasswordRecovery コントロール、ユーザー名のユーザーに求めるインターフェイスをレンダリングして、必要に応じて、そのセキュリティの質問に対する回答。 これは、メールで送信するユーザーのパスワード。

> [!NOTE]
> 電子メール メッセージにプレーン テキストでネットワーク経由で送信されるのではセキュリティ上のリスクを電子メールでユーザーのパスワードを送信します。


PasswordRecovery コントロールは、3 つのビューで構成されます。

- **ユーザー名**-ユーザー名の訪問者を求められます。 これは、最初のビューです。
- **質問**-と共に、ユーザーのセキュリティの質問に対する回答を入力するテキスト ボックスのテキストとしてユーザーのユーザー名とセキュリティの質問を表示します。
- **成功**-自分のパスワードが電子メールで送信されたユーザーに知らせるメッセージが表示されます。

ビューが表示され、PasswordRecovery コントロールによって実行されたアクションは、次のメンバーシップの構成設定によって異なります。

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

メンバーシップ フレームワークの`RequiresQuestionAndAnswer`設定では、アカウントの登録時に、セキュリティの質問と回答を指定する必要があるかどうかを示します。 説明したように、 <a id="_msoanchor_1"> </a> [*ユーザー アカウントを作成する*](../membership/creating-user-accounts-cs.md)チュートリアル, if`RequiresQuestionAndAnswer`が True (既定)、CreateUserWizard のインターフェイスには、テキスト ボックスが含まれています。コントロールの新しいユーザーのセキュリティの質問と回答です。場合`RequiresQuestionAndAnswer`false で、このような情報は収集されません。 同様に場合、`RequiresQuestionAndAnswer`が true の場合、ユーザーが、ユーザー名を入力した後に、質問が表示 PasswordRecovery コントロールの表示は、ユーザーが適切なセキュリティの回答を入力した場合にのみ、パスワードを復旧します。 場合`RequiresQuestionAndAnswer`が False で、ただし、ユーザー名ビューから直接、PasswordRecovery コントロールが正常に完了ビューに移動します。

場合に、ユーザーが自分のユーザー名、または自分のユーザー名とセキュリティの回答を提供が後`RequiresQuestionAndAnswer`PasswordRecovery True - は、自分のパスワードをユーザーにメール送信します。 場合、`EnablePasswordRetrieval`オプションが True に設定し、ユーザーは、現在のパスワードを電子メールで送信されます。 False に設定されている場合と`EnablePasswordReset`PasswordRecovery コントロールが、ユーザーの新しいランダム パスワードを生成し、この新しいパスワードを電子メールで送信し、True に設定されます。 両方`EnablePasswordRetrieval`と`EnablePasswordReset`False、PasswordRecovery コントロールは、例外をスローします。

> [!NOTE]
> いることを思い出してください、 `SqlMembershipProvider` 3 つの形式のいずれかのユーザーのパスワードを格納: オフ、Hashed (既定)、または暗号化します。 使用ストレージ メカニズムは、メンバーシップの構成設定によって異なります。デモ アプリケーションでは、Hashed パスワードの形式を使用します。 ハッシュされたパスワードの形式を使用する場合、`EnablePasswordRetrieval`オプションは、システムは、データベースに格納されているハッシュされたバージョンからのユーザーの実際のパスワードを決定できないために、False に設定する必要があります。


図 1 は、メンバーシップの構成によって PasswordRecovery のインターフェイスと動作を反映する方法を示します。


[![RequiresQuestionAndAnswer、EnablePasswordRetrieval、および EnablePasswordReset PasswordRecovery コントロールの外観と動作に影響を与える](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**図 1**: `RequiresQuestionAndAnswer`、 `EnablePasswordRetrieval`、および`EnablePasswordReset`PasswordRecovery コントロールの外観と動作に影響を与える ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image3.png))。


> [!NOTE]
> <a id="_msoanchor_2"> </a> [ *SQL Server でメンバーシップ スキーマを作成する*](../membership/creating-the-membership-schema-in-sql-server-cs.md)メンバーシップ プロバイダーを設定して構成したチュートリアル`RequiresQuestionAndAnswer`を True に`EnablePasswordRetrieval`にFalse の場合、および`EnablePasswordReset`を True にします。


### <a name="using-the-passwordrecovery-control"></a>PasswordRecovery コントロールを使用します。

ASP.NET ページで、PasswordRecovery コントロールの使用を見てみましょう。 開いている`RecoverPassword.aspx`とドラッグ; PasswordRecovery コントロールをツールボックスからデザイナーにドロップし、設定、`ID`に`RecoverPwd`します。 ログインおよび CreateUserWizard Web コントロールと同様には、PasswordRecovery コントロールのビューは、ラベル、テキスト ボックス、ボタン、および検証コントロールを含む豊富な複合インターフェイスをレンダリングします。 コントロールのスタイル プロパティまたはビューをテンプレートに変換することによって、ビューの外観をカスタマイズすることができます。 ままを演習として興味を持った読者にします。

ユーザーがこのページにアクセスしたときは、自分のユーザー名を入力し、[送信] ボタンをクリックします。 彼女はします。 設定があるため、`RequiresQuestionAndAnswer`プロパティをメンバーシップの構成設定、PasswordRecovery で True には、制御がし、質問ビューを表示します。 ユーザーが、適切なセキュリティの回答を入力して、送信 をクリックした後、PasswordRecovery コントロールにランダムに生成されたものでは、ユーザーのパスワードを更新し、ファイル上の電子メール アドレスには、このパスワードを電子メールで送信します。 1 行のコードを記述することがなく可能なすべてのでした。

このページをテストする前にしようとする構成の最後の 1 つの部分がある: メール配信の設定を指定する必要があります`Web.config`します。 PasswordRecovery コントロールは、電子メールを送信するため、これらの設定に依存します。

使用してメール配信の構成を指定、 [ `<system.net>`要素](https://msdn.microsoft.com/library/6484zdc1.aspx)の[`<mailSettings>`要素](https://msdn.microsoft.com/library/w355a94k.aspx)します。 使用して、 [ `<smtp>`要素](https://msdn.microsoft.com/library/ms164240.aspx)を配信方法とそのアドレスから既定値を示します。 次のマークアップは、という名前のネットワークの SMTP サーバーを使用するメールの設定を構成します。`smtp.example.com`ポート 25 でユーザー名とパスワードのユーザー名/パスワードの資格情報を使用しています。

> [!NOTE]
> `<system.net>` ルートの子要素は、`<configuration>`要素との兄弟`<system.web>`します。 そのため、配置しないでください、`<system.net>`内の要素、`<system.web>`要素です。 代わりに、同じレベルに配置します。


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

に加えて、SMTP サーバーを使用して、ネットワーク上、または送信される電子メール メッセージの格納場所のピックアップ ディレクトリを指定できます。

SMTP 設定を構成した後は、次を参照してください。、`RecoverPassword.aspx`ページがブラウザーを使用します。 まず、ユーザー ストアに存在しないユーザー名を入力してください。 図 2 に示すよう、PasswordRecovery コントロールには、ユーザー情報がアクセスしないことを示すメッセージが表示されます。 コントロールのメッセージのテキストをカスタマイズできる[`UserNameFailureText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)します。


[![無効なユーザー名を入力した場合、エラー メッセージが表示されます。](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**図 2**: 無効なユーザー名を入力した場合、エラー メッセージが表示されます ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image6.png))。


これで、ユーザー名を入力します。 アクセスできる、セキュリティの回答を電子メール アドレスを持つシステム内のアカウントのユーザー名の確認に使用します。 ユーザー名を入力し、送信をクリックすると、PasswordRecovery コントロールには、その質問ビューが表示されます。 としてユーザー名ビューを入力した場合が不適切な回答、PasswordRecovery コントロールが表示されます (図 3 参照) を示すエラー メッセージします。 使用して、 [ `QuestionFailureText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx)このエラー メッセージをカスタマイズします。


[![ユーザーが、無効な秘密の答えを入力した場合、エラー メッセージが表示されます。](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**図 3**: ユーザーが、無効な秘密の答えを入力した場合、エラー メッセージが表示されます ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image9.png))。


最後に、適切なセキュリティの回答を入力し、[送信] をクリックします。 背後では、PasswordRecovery コントロール ランダム パスワードを生成、ユーザー アカウントに割り当てられますの新しいパスワードをユーザーに通知電子メールを送信します (図 4 参照)、し、正常に完了ビューを表示します。


[![His の新しいパスワードを使用してメールをユーザーが送信されます。](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**図 4**: います彼の新しいパスワードを使用してメールをユーザーが送信されます ([フルサイズの画像を表示する をクリックします。](recovering-and-changing-passwords-cs/_static/image12.png))。


### <a name="customizing-the-email"></a>電子メールをカスタマイズします。

PasswordRecovery コントロールから送信された既定の電子メールは、退屈ではなく (図 4 参照) です。 指定されたアカウントから、メッセージが送信された、`<smtp>`要素の`from`サブジェクトのパスワードを持つ属性とプレーン テキストの本文。

サイトに戻り、次の情報を使用してログインしてください。

ユーザー名:*ユーザー名*

パスワード:*パスワード*

このメッセージは PasswordRecovery コントロールのイベント ハンドラーを通じてプログラムでカスタマイズできる[`SendingMail`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)、を通じて宣言的または、 [ `MailDefinition`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)します。 これらのオプションの両方を見てみましょう。

`SendingMail`電子メール メッセージが送信され、最後のチャンスをプログラムで電子メール メッセージを調整する直前にイベントが発生します。 このイベントが発生したときに、イベント ハンドラーが渡される型のオブジェクト[ `MailMessageEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)が`Message`プロパティに送信する電子メールへの参照が含まれています。

イベント ハンドラーを作成、`SendingMail`イベント プログラムに追加する次のコードを追加および`webmaster@example.com`[cc] 一覧にします。

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

電子メール メッセージは、宣言型の手段でも構成できます。 PasswordRecovery の`MailDefinition`プロパティ型のオブジェクトは、 [ `MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx)します。 `MailDefinition`クラスを含む電子メール関連のプロパティのホストは提供`From`、 `CC`、 `Priority`、 `Subject`、 `IsBodyHtml`、 `BodyFileName`、およびその他。 まず、設定、 [ `Subject`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx)パスワードがリセットされた場合などに、既定値 (パスワード) によって使用されるものよりもわかりやすいものにしています.

別のメール テンプレート ファイルを作成する必要があります、電子メール メッセージの本文をカスタマイズするには、本文の内容が含まれています。 という名前の web サイトで新しいフォルダーを作成して開始`EmailTemplates`します。 次に、このという名前のフォルダーに新しいテキスト ファイルを追加`PasswordRecovery.txt`し、次の内容を追加します。

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

プレース ホルダーの使用に注意してください`<%UserName%>`と`<%Password%>`します。 PasswordRecovery コントロールは、ユーザーのユーザー名と電子メールを送信する前に回復パスワードを自動的にこれら 2 つのプレース ホルダーを置き換えます。

最後に、ポイント、`MailDefinition`の[`BodyFileName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)を先ほど作成した電子メールのテンプレート (`~/EmailTemplates/PasswordRecovery.txt`)。

これらの変更内容起きた後、`RecoverPassword.aspx`ページし、ユーザー名とセキュリティの質問を入力します。 受信した次の図 5 のような電子メールをする必要があります。 なお`webmaster@example.com`CC、および件名と本文が更新されていることにしました。


[![件名、本文、および [cc] ボックスの一覧が更新されました](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**図 5**:、件名、本文、および [cc] 一覧は更新されている ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image15.png))。


HTML 形式の電子メールを送信する次のように設定します。 [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx)に HTML を含めるには、True (既定値は False) と電子メール テンプレートを更新します。

`MailDefinition`プロパティは、PasswordRecovery クラスには一意ではありません。 手順 2. でよう、ChangePassword コントロールも用意されています、`MailDefinition`プロパティ。 CreateUserWizard コントロールがこのようなプロパティを含むさらに、自動的に新しいユーザーにウェルカム メール メッセージを送信するように構成できます。

> [!NOTE]
> 現在リンクが存在しないに到達するための左側のナビゲーションで、`RecoverPassword.aspx`ページ。 ユーザーはだけ彼女が正常にサイトにログオンするできなかった場合は、このページを訪問します。 このため、更新、`Login.aspx`をへのリンクを含めるようにページ、`RecoverPassword.aspx`ページ。


### <a name="programmatically-resetting-a-users-password"></a>プログラムによってユーザーのパスワードのリセット

通話を制御、PasswordRecovery のユーザーのパスワードをリセットするときに、`MembershipUser`オブジェクトの[`ResetPassword`メソッド](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx)します。 このメソッドでは、2 つのオーバー ロードがあります。

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -ユーザーのパスワードをリセットします。 場合、このオーバー ロードを使用して`RequiresQuestionAndAnswer`は False です。
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** -指定されたユーザーの場合にパスワードのみをリセット*securityAnswer*が正しい。 場合、このオーバー ロードを使用して`RequiresQuestionAndAnswer`は True です。

両方のオーバー ロードでは、ランダムに生成された新しいパスワードが返されます。

メンバーシップ フレームワークの他のメソッドを使用するような`ResetPassword`構成済みのプロバイダーにメソッドのデリゲート。 `SqlMembershipProvider`呼び出す、`aspnet_Membership_ResetPassword`ストアド プロシージャを渡すことで、ユーザーのユーザー名、新しいパスワード、およびその他のフィールドの間で、指定されたパスワードの回答。 ストアド プロシージャにより、パスワードの回答が一致して、ユーザーのパスワードを更新します。

低レベルの実装に関する注意事項のいくつかあります。

- ロックされたユーザーは自分のパスワードをリセットできません。 ただし、ユーザーが承認されていない可能性があります。 ロックされている説明し、承認の状態について詳しく、 <a id="_msoanchor_3"> </a> [ *Unlocking とユーザーの承認*](unlocking-and-approving-user-accounts-cs.md)アカウント チュートリアル。
- パスワードの回答が正しくない場合は、ユーザーの失敗したパスワード回答試行回数が増加します。 無効なセキュリティの解答の指定した数が、指定された時間内で発生した場合、ユーザーはロックアウトされます。

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>生成される、ランダムなパスワード方法で単語

図 4 および 5 の電子メール メッセージに示すように、ランダムに生成されたパスワードがメンバーシップ クラスのによって作成された[`GeneratePassword`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)します。 このメソッドは 2 つの整数の入力パラメーターの*長さ*と*numberOfNonAlphanumericCharacters* -少なくとも文字列を返します*長さ*で文字の長さ少なくとも*numberOfNonAlphanumericCharacters*英数字以外の文字の数。 これら 2 つのパラメーターの値はメンバーシップの構成によって決まりますメンバーシップ クラスまたはログインに関連する Web コントロール内では、このメソッドからが呼び出されると、`MinRequiredPasswordLength`と`MinRequiredNonalphanumericCharacters`プロパティで、それぞれ 7 と 1 に設定します。

`GeneratePassword`メソッドでは、暗号強度が高い乱数ジェネレーターを使用して、どのようなランダムな文字が選択されているのバイアスがないことを確認します。 さらに、`GeneratePassword`は`public`、使用することは、ASP.NET アプリケーションから直接ランダムな文字列やパスワードを生成する必要がある場合を意味します。

> [!NOTE]
> `SqlMembershipProvider`クラスは、常にランダムなパスワードを生成ため場合、少なくとも 14 文字`MinRequiredPasswordLength`14 より小さい場合、その値は無視されます。


## <a name="step-2-changing-passwords"></a>手順 2: パスワードの変更

ランダムに生成されたパスワードは覚えにくいです。 図 4 に示すようにパスワードを検討してください:`WWGUZv(f2yM:Bd`します。 メモリをコミットすることをお試しください。 もちろん、ユーザーには、この種のランダムに生成されたパスワードが送信された後、覚えやすいパスワードを変更彼女はします。

ChangePassword コントロールを使用すると、自分のパスワードを変更するためのインターフェイスを作成できます。 はるか PasswordRecovery コントロールと同様に、ChangePassword コントロールから成る 2 つのビュー: パスワードの変更と成功します。 パスワードの変更ビューには、古いマスター_キーと新しいパスワードのユーザー メッセージが表示されます。 古いパスワードと最小の長さと文字の英数字以外の要件を満たす新しいパスワードを指定し、時に、ChangePassword コントロールは、ユーザーのパスワードを更新し、正常に完了ビューを表示します。

> [!NOTE]
> ChangePassword コントロールを呼び出すことによって、ユーザーのパスワードを変更する、`MembershipUser`オブジェクトの[`ChangePassword`メソッド](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx)します。 ChangePassword メソッドが受け取る 2 つ`string`入力パラメーターの*oldPassword*と*newPassword*-で、ユーザーのアカウントを更新し、 *newPassword*、指定されたと仮定すると*oldPassword*が正しい。


開く、`ChangePassword.aspx`ページし、その名前を付け、ページに ChangePassword コントロールを追加`ChangePwd`します。 この時点で、デザイン ビューがパスワードの変更を表示する必要があります (図 6 参照) を表示します。 ような PasswordRecovery コントロールが、切り替えることができます、コントロールのスマート タグを使用してビュー。 さらに、これらのビューの外観は、そのようなさまざまなスタイルのプロパティまたはテンプレートに変換してカスタマイズできます。


[![ChangePassword コントロールをページに追加します。](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**図 6**: ChangePassword コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image18.png))。


ChangePassword コントロールは、現在ログインしているユーザーのパスワードを更新できます*または*、指定した別のユーザーのパスワード。 既定のパスワードの変更ビューが 3 つのテキスト ボックス コントロールをレンダリング図 6 に示すよう。 古いパスワードと新しいパスワードの 2 つのいずれか。 この既定のインターフェイスは、現在ログオンしているユーザーのパスワードの更新に使用されます。

ChangePassword コントロールを使用すると、別のユーザーのパスワードを更新して、設定、コントロールの[`DisplayUserName`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx)を True にします。 これにより、ページで、パスワードを変更するユーザーのユーザー名の入力を求めるに 4 つ目のテキスト ボックスを追加します。

設定`DisplayUserName`を True をログに記録されたユーザーにログインしなくても、自分のパスワードを変更できるようにする場合に便利です。 個人的には、自分のパスワードを変更することを許可する前にユーザーのログインを必要とする問題には何もないと思います。 そのためのままに`DisplayUserName`False (既定値) に設定します。 この決定を行うにただし基本的にはにおいて匿名ユーザーからこのページに到達します。 匿名ユーザーにアクセスを拒否するために、サイトの URL 承認規則を更新`ChangePassword.aspx`します。 URL 承認規則の構文上のメモリを更新する必要がある場合に戻って、 <a id="_msoanchor_4"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-cs.md)チュートリアル。

> [!NOTE]
> 思えるかもしれませんが、`DisplayUserName`プロパティは、管理者は他のユーザーのパスワードを変更することに役立ちます。 ただし、場合でも`DisplayUserName`が古いパスワードを正常し、入力する必要がありますを True に設定します。 手順 3 でユーザーのパスワードを変更する管理者を許可する手法について説明します。


参照してください、`ChangePassword.aspx`ブラウザーでページし、パスワードを変更します。 パスワードの長さとメンバーシップの構成で指定された文字の英数字以外の要件を満たすために失敗した新しいパスワードを入力すると、エラー メッセージが表示されることに注意してください (図 7 を参照してください)。


[![ChangePassword コントロールをページに追加します。](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**図 7**: ChangePassword コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image21.png))。


ユーザーの古いパスワードと、ログに記録された有効な新しいパスワードを入力するパスワードが変更され、正常に完了ビューが表示されます。

### <a name="sending-a-confirmation-email"></a>確認の電子メールを送信します。

既定では、ChangePassword コントロールは、パスワードが更新されました、ユーザーに電子メール メッセージを送信しません。 電子メールを送信する場合は、単に構成するコントロールの`MailDefinition`プロパティ。 ユーザーには、新しいパスワードを含む HTML 形式の電子メールが送信されるように、ChangePassword コントロールを構成しましょう。

新しいファイルを作成して開始、`EmailTemplates`という名前のフォルダー`ChangePassword.htm`します。 次のマークアップを追加します。

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

ChangePassword コントロールを次に、設定`MailDefinition`プロパティの`BodyFileName`、 `IsBodyHtml`、および`Subject`プロパティを ~ EmailTemplates/ChangePassword.htm、true の場合、およびパスワードが変更された/!、それぞれします。

これらの変更を加えたら、ページを再確認し、もう一度パスワードを変更します。 今回は、ChangePassword コントロールで、ファイルで、ユーザーの電子メール アドレスに、HTML 形式のカスタマイズされたメールが送信します (図 8 参照)。


[![ユーザー、そのパスワードが変更通知の電子メール メッセージ](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**図 8**: An 電子メール メッセージの通知ユーザーをそのパスワードが変更 ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image24.png))。


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>手順 3: 管理者がユーザーのパスワードの変更を許可します。

ユーザー アカウントをサポートするアプリケーションで一般的な機能は、他のユーザーのパスワードを変更する管理ユーザーの機能です。 場合によってシステムには、ユーザーが自分のパスワードを変更する機能が不足しているためには、この機能が必要です。 このような場合は、ユーザーが忘れたパスワードを回復する唯一の方法は、管理者は新しいパスワードを割り当ててになります。 PasswordRecovery、ChangePassword コントロールとただし、管理ユーザー必要がありますビジーになっていない自体、ユーザーのパスワードを変更するとユーザーはこれを自身の処理を実行できるようします。

しかし、クライアントが管理ユーザーが他のユーザーのパスワードを変更できることを追い求める場合でしょうか。 残念ながら、この機能の追加の作業を指定できます。 ユーザーのパスワードを変更するに新旧両方のパスワードを指定する必要があります、`MembershipUser`オブジェクトの`ChangePassword`メソッドが、管理者は、それを変更するには、ユーザーのパスワードを知っている必要はありません。

1 つの回避策は、まずユーザーのパスワードをリセットし、次のようなコードを使用して、新しいパスワードに変更するは。

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

このコードの開始に関する情報を取得することによって*ユーザー名*、パスワード管理者が変更する必要があるユーザーであります。 次に、`ResetPassword`メソッドが呼び出されると、どの割り当てとユーザーの新しいランダム パスワード。 このランダムに生成されたパスワードが、メソッドによって返され、変数に格納されている`resetPwd`します。 ユーザーのパスワードがわかっているので、呼び出しに変更できます`ChangePassword`します。

問題は、メンバーシップ システムの構成が設定されている場合に、このコードをのみ機能するように`RequiresQuestionAndAnswer`は False です。 場合`RequiresQuestionAndAnswer`true の場合、アプリケーションについては、`ResetPassword`メソッドは、秘密の答えを渡される必要があります、それ以外の場合、例外がスローされます。

メンバーシップ フレームワークがセキュリティの質問と解答を要求するように構成されているし、まだ、クライアントでは管理者ユーザーのパスワードを変更することであると主張して、次の 3 つのオプションがあります。

- 無線で手をスローし、実行できないことの 1 つだけであることをクライアントに指示します。
- 設定`RequiresQuestionAndAnswer`を False にします。 これにより、安全性の低いアプリケーション。 不正なユーザーが別のユーザーの電子メールの受信トレイへのアクセスを得られることを想像してください。 おそらく、侵害されたユーザーに各自のデスクを辞めたり、ワークステーションをロックしませんでしたやパブリック ターミナルから自分の電子メールにアクセスし、サインアウトしていない、かもしれません。どちらの場合、不正なユーザーがアクセスできる、`RecoverPassword.aspx`ページし、ユーザーのユーザー名を入力します。 システムは、セキュリティの回答を求めることがなく回復パスワードが電子メールされます。
- メンバーシップ フレームワークと、SQL Server データベースと直接作業によって作成された抽象化レイヤーをバイパスします。 メンバーシップ スキーマには、という名前のストアド プロシージャが含まれています。`aspnet_Membership_SetPassword`をユーザーのパスワードを設定し、そのタスクを実行するには秘密の答えや古いパスワードは必要ありません。

これらのオプションのいずれも特に魅力的なが開発者の有効期間が場合がありますが方法です。

先へ進み、バイパス コードの記述 3 番目のアプローチを実装し、`Membership`と`MembershipUser`クラスを直接操作し、`SecurityTutorials`データベース。

> [!NOTE]
> データベースを直接操作、メンバーシップ フレームワークによって提供されるカプセル化が割れています。 この決定を結び付けるを`SqlMembershipProvider`、移植可能な以下のコードを作成します。 さらに、このコードが期待どおりに将来のバージョンの ASP.NET メンバーシップ スキーマが変更された場合は機能しません。 この方法は、問題を回避して、ほとんどの回避策などを利用するベスト プラクティスの例はありません。


コードは、いくつかの魅力的ではありませんビットを備えは非常に長い。 そのため、その詳細を調べる場合で、このチュートリアルを乱雑にしません。 詳細に関心がある場合は、このチュートリアルとアクセス コードをダウンロード、`~/Administration/ManageUsers.aspx`ページ。 このページで作成した、 <a id="_msoanchor_5"> </a>[前のチュートリアル](building-an-interface-to-select-one-user-account-from-many-cs.md)、各ユーザーを一覧表示されます。 リンクを含める GridView が更新されました、 `UserInformation.aspx`  ページで、クエリ文字列を選択したユーザーのユーザー名を渡します。 `UserInformation.aspx`ページは、自分のパスワードを変更するため、選択したユーザーとテキスト ボックスに関する情報を表示します (図 9 参照)。

新しいパスワードを入力する、2 つ目のテキスト ボックスの確認、ユーザーの更新ボタンをクリックして後のポストバックに陥りますと`aspnet_Membership_SetPassword`ストアド プロシージャが呼び出される、ユーザーのパスワードを更新します。 コードについての理解を含めるパスワードを変更したユーザーに電子メールを送信するための機能を拡張してこの機能の対象読者ことをお勧めします。


[![管理者がユーザーのパスワードを変更します。](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**図 9**: 管理者ユーザーのパスワードを変更することがあります ([フルサイズの画像を表示する をクリックします](recovering-and-changing-passwords-cs/_static/image27.png))。


> [!NOTE]
> `UserInformation.aspx`クリアまたは Hashed 形式でパスワードを格納するメンバーシップ フレームワークが構成されている場合でのみ動作ページ現在します。 この機能を追加する招待されていますが、新しいパスワードを暗号化するためのコードが不足しています。 必要なコードを追加することをお勧めします。 方法はなどのデコンパイラを使用する[Reflector](http://www.aisto.com/roeder/dotnet/) 、.NET Framework のメソッドのソース コードを調べることを調べることで開始、`SqlMembershipProvider`クラスの`ChangePassword`メソッド。 これは、パスワードのハッシュを作成するためのコードを記述するために使用する手法です。


## <a name="summary"></a>まとめ

ASP.NET には、自分のパスワードを管理するために 2 つのコントロールが用意されています。 PasswordRecovery コントロールは、自分のパスワードを忘れてしまった方のために便利です。 メンバーシップ フレームワークの構成によって、ユーザーは、既存のパスワードまたはランダムに生成された、新しいパスワードか電子メールで送信します。 ChangePassword コントロールでは、自分のパスワードを更新するユーザーを使用します。

ログインと CreateUserWizard コントロールと同様には、PasswordRecovery、ChangePassword コントロールは、宣言型マークアップがまったくまたは行のコードを記述することがなく豊富なユーザー インターフェイスをレンダリングします。 既定のユーザー インターフェイスでは、お客様のニーズを満たしていない場合は、さまざまなスタイルのプロパティをカスタマイズできます。 または、コントロールのインターフェイスは、コントロールのさらに細分化の次数のテンプレートに変換することがあります。 これらのコントロールが、メンバーシップ API を使用して、バック グラウンドでの呼び出し、`MembershipUser`オブジェクトの`ResetPassword`と`ChangePassword`メソッド。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ChangePassword コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [PasswordRecovery コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [ASP.NET で電子メールを送信します。](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` よく寄せられる質問](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者には、Michael Emmings Suchi 著がなどがあります。 今後、MSDN の記事を確認したいですか。 場合は、筆者に [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](building-an-interface-to-select-one-user-account-from-many-cs.md)
> [次へ](unlocking-and-approving-user-accounts-cs.md)
