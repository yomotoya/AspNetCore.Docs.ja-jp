---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: "回復して、パスワード (VB) の変更 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET には、復旧し、パスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールは、彼失わ pa を回復するビジターを使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: f7f6e7e4bc3a8cc7e70911bc22a28d385f762af0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="recovering-and-changing-passwords-vb"></a>回復して、パスワード (VB) の変更
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip)または[PDF のダウンロード](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET には、復旧し、パスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールを使用すると、失われたパスワードを回復する訪問者。 ChangePassword コントロールは、自分のパスワードを更新するユーザーを使用します。 その他のログインに関連する Web コントロールと同様にこれまで見てきたこのチュートリアル シリーズ、PasswordRecovery 全体および ChangePassword がバック グラウンドでユーザーのパスワードをリセットまたは変更メンバーシップ framework での作業を制御します。


## <a name="introduction"></a>はじめに

マイ銀行、電力会社、電話会社、電子メール アカウント、および個人用に設定された web ポータルの web サイトの間で、ほとんどの人と同様にある多数の異なるパスワードしか記憶します。 最近を記憶する非常に多くの資格情報でユーザーが自分のパスワードを忘れたをためは珍しいことではありません。 このアカウントには、ユーザー アカウントを提供している web サイトをユーザーが自分のパスワードを回復する方法を含める必要があります。 通常、このプロセスには、新しいランダムなパスワードを生成して、ファイル上のユーザーの電子メール アドレスに電子メールが含まれます。 新しいパスワードを受信した後ほとんどのユーザー サイトに戻り、パスワードを変更して、覚えやすいものにランダムに生成されたものです。

ASP.NET には、復旧し、パスワードの変更を支援するための 2 つの Web コントロールが含まれています。 PasswordRecovery コントロールを使用すると、失われたパスワードを回復する訪問者。 ChangePassword コントロールは、自分のパスワードを更新するユーザーを使用します。 その他のログインに関連する Web コントロールと同様にこれまで見てきたこのチュートリアル シリーズ、PasswordRecovery 全体および ChangePassword がバック グラウンドでユーザーのパスワードをリセットまたは変更メンバーシップ framework での作業を制御します。

このチュートリアルでは、これら 2 つのコントロールの使い方を見ていきます。 表示されています。 プログラムで変更してからユーザーのパスワードをリセットする方法、`MembershipUser`クラスの`ChangePassword`と`ResetPassword`メソッドです。

## <a name="step-1-helping-users-recover-lost-passwords"></a>手順 1: 支援ユーザー回復パスワードを紛失

ユーザー アカウントをサポートするすべての web サイトは、ユーザーのパスワードを忘れた場合に回復するための機構を提供する必要があります。 良いニュースは、ASP.NET でこのような機能を実装する PasswordRecovery Web コントロール感謝もとても簡単です。 PasswordRecovery コントロールは、ユーザー名、ユーザーを要求するインターフェイスを表示し、それらのセキュリティの質問に対する回答が必要な場合です。 これをメールで送信、ユーザーのパスワード。

> [!NOTE]
> 電子メール メッセージにプレーン テキストでネットワーク経由で送信されるのでセキュリティ上のリスクが電子メールでユーザーのパスワードを送信するに関係します。


PasswordRecovery コントロールは、3 つのビューで構成されます。

- **ユーザー名**-ビジター、ユーザー名を求められます。 これは、最初のビューです。
- **質問**-ユーザーのセキュリティの質問に対する回答を入力するためのテキスト ボックスと、テキストとしてユーザーのユーザー名とセキュリティの質問が表示されます。
- **成功**-自分のパスワードは電子メールで送信されたユーザーに通知メッセージが表示されます。

ビューが表示され、PasswordRecovery コントロールによって実行される操作は、次のメンバーシップ構成設定によって異なります。

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

メンバーシップ フレームワークの`RequiresQuestionAndAnswer`設定では、アカウントの登録時に、セキュリティの質問と回答を指定する必要があるかどうかを示します。 説明したよう、 <a id="_msoanchor_1"> </a> [*ユーザー アカウントを作成する*](../membership/creating-user-accounts-vb.md)チュートリアル、if`RequiresQuestionAndAnswer`が True (既定値) CreateUserWizard のインターフェイスには、テキスト ボックスが含まれています。新しいユーザーのセキュリティの質問と回答; のコントロール場合`RequiresQuestionAndAnswer`false で、このような情報は収集されません。 同様に場合、`RequiresQuestionAndAnswer`が true の場合、ユーザーが、ユーザー名を入力した後に、質問を表示 PasswordRecovery コントロールの表示以外のパスワードを回復すると、ユーザーが正しいセキュリティ返答を入力した場合にのみです。 場合`RequiresQuestionAndAnswer`が False で、ただし、PasswordRecovery コントロール、ユーザー名ビューから直接、成功がビューに移動します。

場合、ユーザーを自分のユーザー名、または自分のユーザー名とセキュリティの回答が提供後`RequiresQuestionAndAnswer`PasswordRecovery True では、自分のパスワードをユーザーに電子メール。 場合、`EnablePasswordRetrieval`オプションが True に設定し、ユーザーが、現在のパスワードを電子メールで送信します。 False に設定されている場合と`EnablePasswordReset`PasswordRecovery コントロールは、ユーザーの新しいランダムなパスワードを生成し、この新しいパスワードに電子メールで送信し、True に設定します。 両方`EnablePasswordRetrieval`と`EnablePasswordReset`false、PasswordRecovery コントロールが例外をスローします。

> [!NOTE]
> 注意してください、 `SqlMembershipProvider` 3 つの形式のいずれかでユーザーのパスワードを保存します。: オフ、Hashed (既定)、または暗号化します。 使用記憶域メカニズムは、メンバーシップの構成設定によって異なります。デモ アプリケーションでは、Hashed パスワードの形式を使用します。 Hashed パスワードの形式を使用する場合、`EnablePasswordRetrieval`オプションは、システムは、データベースに格納されているハッシュのバージョンから、ユーザーの実際のパスワードを決定できないために、False に設定する必要があります。


図 1 は、どのように PasswordRecovery のインターフェイスと動作が影響を受けますメンバーシップの構成を示します。


[![RequiresQuestionAndAnswer、EnablePasswordRetrieval、および EnablePasswordReset PasswordRecovery コントロールの外観と動作に影響を与える](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**図 1**: `RequiresQuestionAndAnswer`、 `EnablePasswordRetrieval`、および`EnablePasswordReset`PasswordRecovery コントロールの外観と動作に影響を与える ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> <a id="_msoanchor_2"> </a> [*メンバーシップ スキーマを作成する SQL Server で*](../membership/creating-the-membership-schema-in-sql-server-vb.md)チュートリアルを設定してメンバーシップ プロバイダーを構成しました`RequiresQuestionAndAnswer`を True に`EnablePasswordRetrieval`にFalse の場合、および`EnablePasswordReset`True に設定します。


### <a name="using-the-passwordrecovery-control"></a>PasswordRecovery コントロールの使用

ASP.NET ページで PasswordRecovery コントロールの使用を見てみましょう。 開いている`RecoverPassword.aspx`とドラッグおよび PasswordRecovery コントロールをツールボックスからデザイナーにドロップの設定、`ID`に`RecoverPwd`です。 ログインと CreateUserWizard Web コントロールと同様には、PasswordRecovery コントロールのビューは、ラベル、テキスト ボックス、ボタン、および検証コントロールが含まれる豊富な複合インターフェイスをレンダリングします。 コントロールのスタイル プロパティまたはビューをテンプレートに変換することによって、ビューの外観をカスタマイズすることができます。 I のままに練習として、興味のリーダーの。

ユーザーがこのページを訪問したときに、自分のユーザー名を入力し、送信ボタンをクリックして彼女はします。 設定したため、`RequiresQuestionAndAnswer`プロパティをメンバーシップの構成設定、PasswordRecovery で True を制御し、質問ビューを表示します。 ユーザーは自分正しいセキュリティ返答を入力し、送信をクリックすると、PasswordRecovery コントロールをランダムに生成されたものでは、ユーザーのパスワードを更新し、ファイル上の電子メール アドレスにこのパスワードを電子メールで送信します。 これはすべてできました私たちが 1 行のコードを記述することです。

このページをテストする前にしようとする構成の最後の 1 つの部分がある: メール配信の設定を指定する必要があります`Web.config`です。 PasswordRecovery コントロールは、電子メールを送信するため、これらの設定に依存します。

メール配信の構成を指定、 [ `<system.net>`要素](https://msdn.microsoft.com/en-us/library/6484zdc1.aspx)の[`<mailSettings>`要素](https://msdn.microsoft.com/en-us/library/w355a94k.aspx)です。 使用して、 [ `<smtp>`要素](https://msdn.microsoft.com/en-us/library/ms164240.aspx)配信方法とそのアドレスから既定値を示すためにします。 次のマークアップがという名前のネットワークの SMTP サーバーを使用するメールの設定を構成`smtp.example.com`ポート 25 でユーザー名とパスワードのユーザー名/パスワードの資格情報を使用しています。

> [!NOTE]
> `<system.net>`ルートの子要素は、`<configuration>`要素との兄弟`<system.web>`です。 そのため、配置しない、`<system.net>`内の要素、`<system.web>`要素です。 代わりに、同じレベルに配置します。


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

に加えて、ネットワーク上の SMTP サーバーを使用して、別の方法として、ピックアップ ディレクトリを送信する電子メール メッセージの格納場所を指定できます。

SMTP 設定を構成した場合を参照してください。、`RecoverPassword.aspx`ページがブラウザーを使用します。 まず、ユーザー ストアに存在しないユーザー名を入力してください。 図 2 に示す PasswordRecovery コントロールには、ユーザー情報にアクセスできなかったことを示すメッセージが表示されます。 コントロールのメッセージのテキストをカスタマイズできる[`UserNameFailureText`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx)です。


[![無効なユーザー名が入力した場合、エラー メッセージが表示されます。](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**図 2**: 無効なユーザー名が入力した場合、エラー メッセージが表示されます ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image6.png))


ここで、ユーザー名を入力します。 電子メール アドレス アクセスでき、セキュリティの回答することで、システム内のアカウントのユーザー名の確認に使用します。 ユーザー名を入力し、送信をクリックして、後に、PasswordRecovery コントロールは、その質問ビューを表示します。 としてと、ユーザー名ビューを入力する場合、正しくない回答、PasswordRecovery コントロールが表示されます (図 3 を参照してください) を示すエラー メッセージします。 使用して、 [ `QuestionFailureText`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx)このエラー メッセージをカスタマイズします。


[![ユーザーが、無効なセキュリティ返答を入力した場合、エラー メッセージが表示されます。](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**図 3**: ユーザーが、無効なセキュリティ返答を入力した場合、エラー メッセージが表示されます ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image9.png))


最後に、適切なセキュリティ返答を入力し、[送信] をクリックします。 背後では、PasswordRecovery コントロール ランダム パスワードを生成、ユーザー アカウントに割り当てます、その新しいパスワードのユーザーに通知電子メールを送信 (図 4 を参照)、し、成功ビューを表示します。


[![ユーザーが His の新しいパスワードを使用して電子メールを送信します。](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**図 4**: ユーザーは His の新しいパスワードを使用して電子メールを送信 ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>電子メールのカスタマイズ

PasswordRecovery コントロールによって送信される既定の電子メールではなく鈍い (図 4 を参照) です。 メッセージがで指定されたアカウントから送信された、`<smtp>`要素の`from`サブジェクトのパスワードを持つ属性と、プレーン テキスト本文。

サイトに戻り、次の情報を使用してログインしてください。

ユーザー名:*ユーザー名*

パスワード:*パスワード*

このメッセージは PasswordRecovery コントロールのイベント ハンドラーによってプログラムでカスタマイズできる[`SendingMail`イベント](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx)、を通じて宣言的または、 [ `MailDefinition`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx)です。 これらのオプションのいずれも見てみましょう。

`SendingMail`イベントが発生する直前に、電子メール メッセージが送信され、最後のチャンスをプログラムによって電子メール メッセージを調整します。 このイベントが発生したときに、イベント ハンドラーが渡される型のオブジェクト[ `MailMessageEventArgs`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.mailmessageeventargs.aspx)が`Message`プロパティには、送信する電子メールへの参照が含まれています。

イベント ハンドラーを作成、`SendingMail`イベント プログラムで追加する次のコードを追加および`webmaster@example.com`[cc] 一覧にします。

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

宣言型の方法で、電子メール メッセージを構成することもできます。 PasswordRecovery の`MailDefinition`プロパティは、オブジェクトの種類の[ `MailDefinition`](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.aspx)です。 `MailDefinition`クラスには、ホストを含む、電子メールに関連するプロパティの`From`、 `CC`、 `Priority`、 `Subject`、 `IsBodyHtml`、 `BodyFileName`、およびその他。 まず、設定、 [ `Subject`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.subject.aspx)(パスワード) が既定では、パスワードがリセットされたなどに使用されるものよりもわかりやすいものにしています.

別の電子メール テンプレート ファイルを作成する必要があります、電子メール メッセージの本文をカスタマイズするには、本文の内容を含むです。 という名前の web サイトに新しいフォルダーを作成して開始`EmailTemplates`です。 次に、このという名前のフォルダーに新しいテキスト ファイルを追加`PasswordRecovery.txt`し、次のコンテンツを追加します。

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

プレース ホルダーの使用に注意してください`<%UserName%>`と`<%Password%>`です。 PasswordRecovery コントロールは、ユーザーのユーザー名と電子メールを送信する前に回復パスワードで自動的にこれら 2 つのプレース ホルダーを置き換えます。

最後に、ポイント、`MailDefinition`の[`BodyFileName`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx)先ほど作成した、電子メール通知テンプレートを (`~/EmailTemplates/PasswordRecovery.txt`)。

これらの変更見直す後、`RecoverPassword.aspx`ページし、ユーザー名とセキュリティの回答を入力します。 受信した電子メールを次の図 5 のようにする必要があります。 なお`webmaster@example.com`[cc]、および件名と本文が更新されていることにしました。


[![件名、本文、および [cc] ボックスの一覧が更新されました](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**図 5**:、件名、本文、および [cc] ボックスの一覧が更新されました ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image15.png))


HTML 形式の電子メールを送信する次のように設定します。 [ `IsBodyHtml` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx)に HTML を含めるには、True (既定値は False) と、電子メール通知テンプレートを更新します。

`MailDefinition`プロパティが PasswordRecovery クラスには一意ではありません。 手順 2. でよう、ChangePassword コントロールも用意されています、`MailDefinition`プロパティです。 CreateUserWizard コントロールがこのようなプロパティを含むさらに、自動的に"ようこそ"電子メール メッセージを送信する新しいユーザーを構成することができます。

> [!NOTE]
> 現在リンクが存在しない、左側のナビゲーションに到達するために、`RecoverPassword.aspx`ページ。 ユーザーはだけ彼女が正常にサイトにログオンするできなかった場合は、このページを訪問します。 このため、更新、`Login.aspx`をへのリンクを含めるようにページ、`RecoverPassword.aspx`ページ。


### <a name="programmatically-resetting-a-users-password"></a>プログラムによって、ユーザーのパスワードをリセットします。

制御呼び出しユーザーのパスワード、PasswordRecovery をリセットするときに、`MembershipUser`オブジェクトの[`ResetPassword`メソッド](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.resetpassword.aspx)です。 このメソッドでは、2 つのオーバー ロードがあります。

- **[`ResetPassword`](https://msdn.microsoft.com/en-us/library/d94bdzz2.aspx)**-ユーザーのパスワードをリセットします。 場合、このオーバー ロードを使用して`RequiresQuestionAndAnswer`は False です。
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/en-us/library/d90zte4w.aspx)**-指定されたユーザーの場合にパスワードのみをリセット*securityAnswer*が正しい。 場合、このオーバー ロードを使用して`RequiresQuestionAndAnswer`true を指定します。

両方のオーバー ロードは、ランダムに生成された新しいパスワードを返します。

メンバーシップ フレームワーク内の他のメソッドを使用するような`ResetPassword`に構成されているプロバイダーのメソッドのデリゲート。 `SqlMembershipProvider`呼び出します、`aspnet_Membership_ResetPassword`ストアド プロシージャを渡すことで、ユーザーのユーザー名、新しいパスワードおよびその他のフィールドの間で、指定されたパスワードの回答。 ストアド プロシージャによりことパスワードの回答と一致するユーザーのパスワードを更新します。

いくつかの低レベルの実装に関するメモ:

- ロックされたユーザーは自分のパスワードをリセットできません。 ただし、未承認のユーザーでは次のことがあります。 について説明します、ロックをしで詳細に状態を承認、 <a id="_msoanchor_3"> </a> [ *Unlocking とユーザーの承認*](unlocking-and-approving-user-accounts-vb.md)アカウント チュートリアルです。
- パスワードの回答が正しくない場合は、ユーザーの失敗したパスワード回答試行回数がインクリメントされます。 指定した時間枠内で無効なセキュリティ解答の試行数を指定する場合は、ユーザーはロックアウトされます。

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>ランダムなパスワード方法で単語が生成されます。

図 4 と 5 で電子メール メッセージで表示される、ランダムに生成されたパスワードがのメンバーシップ クラスは、によって作成された[`GeneratePassword`メソッド](https://msdn.microsoft.com/en-us/library/system.web.security.membership.generatepassword.aspx)です。 このメソッドは、次の 2 つの整数の入力パラメーター - を受け取ります*長さ*と*numberOfNonAlphanumericCharacters* -文字列を返すには、少なくとも*長さ*で文字の長さ少なくとも*numberOfNonAlphanumericCharacters*英数字以外の文字数。 これら 2 つのパラメーターの値はメンバーシップの構成によって決まりますメンバーシップ クラスまたはログインに関連する Web コントロール内では、このメソッドからが呼び出されると、`MinRequiredPasswordLength`と`MinRequiredNonalphanumericCharacters`プロパティで、それぞれ 7 と 1 に設定します。

`GeneratePassword`メソッドでは、暗号強度が高い乱数ジェネレーターを使用して、どのようなランダムな文字が選択されているでバイアスがないことを確認してください。 さらに、`GeneratePassword`は`Public`、使用することを ASP.NET アプリケーションから直接ランダムな文字列やパスワードを生成する必要がある場合を意味します。

> [!NOTE]
> `SqlMembershipProvider`クラスは、ランダムなパスワードを常に生成には、少なくとも 14 文字、ため`MinRequiredPasswordLength`14 より小さい場合、その値は無視されます。


## <a name="step-2-changing-passwords"></a>手順 2: パスワードの変更

ランダムに生成されたパスワードは覚えにくいです。 図 4 に示すようにパスワードを検討してください:`WWGUZv(f2yM:Bd`です。 メモリをコミットを再試行してください。 言うまでも、ユーザーには、この種のランダムに生成されたパスワードが送信された後、覚えやすいパスワードを変更彼女はします。

ChangePassword コントロールを使用すると、ユーザーが自分のパスワードを変更するためのインターフェイスを作成できます。 かなり PasswordRecovery コントロールと同様、ChangePassword コントロールでは 2 つのビュー: パスワードの変更状況と成功します。 パスワードの変更のビューは、古いマスター_キーと新しいパスワードをユーザーに求めます。 古いパスワードと最小の長さと英数字以外の文字の要件を満たす新しいパスワードを指定して、時に、ChangePassword コントロールは、ユーザーのパスワードを更新し、成功ビューを表示します。

> [!NOTE]
> ChangePassword コントロールを呼び出すことによって、ユーザーのパスワードを変更する、`MembershipUser`オブジェクトの[`ChangePassword`メソッド](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.changepassword.aspx)です。 ChangePassword メソッドでは、2 つ受け取ります`String`入力パラメーター - *oldPassword*と*newPassword*-を持つユーザーのアカウントを更新し、 *newPassword*、指定されたと仮定して*oldPassword*が正しい。


開く、`ChangePassword.aspx`ページし、パスワードの変更をコントロールに名前を付け、ページを追加`ChangePwd`です。 この時点では、デザイン ビューがパスワードの変更を表示する必要があります (図 6 を参照してください) を表示します。 同様に PasswordRecovery コントロールを使用することができますビューを切り替える、コントロールのスマート タグを使用しています。 さらに、これらのビューの外観は、そのようなさまざまなスタイルのプロパティ、またはテンプレートに変換することでカスタマイズできます。


[![ChangePassword コントロールをページに追加します。](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**図 6**: ChangePassword コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image18.png))


ChangePassword コントロールは、現在ログインしているユーザーのパスワードを更新できます*または*別に、指定したユーザーのパスワード。 既定のパスワードの変更の表示が 3 つのテキスト ボックス コントロールを表示する図 6 に示す: 古いパスワードと新しいパスワードの 2 つのいずれか。 この既定のインターフェイスを使用して、現在ログオンしているユーザーのパスワードを更新します。

コントロールを使用する、パスワードの変更を別のユーザーのパスワードを更新して、設定コントロールの[`DisplayUserName`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.changepassword.displayusername.aspx)True に設定します。 これにより、ページで、ユーザーのユーザー名のパスワードを変更するように要求する 4 つ目のテキスト ボックスを追加します。

設定`DisplayUserName`を True にログアウト、ユーザーにログインしなくてもパスワードを変更する場合に便利です。 個人的には、彼女自分のパスワードの変更を許可する前にユーザーがログインを必要とすると問題があると思われます。 そのためのままにして`DisplayUserName`False (既定値) に設定します。 この決定を行うにただし、お基本的には、なし機能匿名ユーザーがこのページに到達します。 匿名ユーザーにアクセスを拒否するために、サイトの URL 承認規則を更新`ChangePassword.aspx`です。 URL 承認規則の構文上のメモリを更新する必要がある場合に戻って、 <a id="_msoanchor_4"> </a> [*ユーザー ベースの承認*](../membership/user-based-authorization-vb.md)チュートリアルです。

> [!NOTE]
> 感じるかもしれませんが、`DisplayUserName`プロパティは、他のユーザーのパスワードを変更する管理者を許可する場合に役立ちます。 ただし、場合でも`DisplayUserName`が古いパスワードを既知し、入力する必要がありますを True に設定します。 手順 3 でユーザーのパスワードを変更する管理者を許可する手法について説明します。


参照してください、`ChangePassword.aspx`ブラウザー内でページし、パスワードを変更します。 パスワードの長さとメンバーシップの構成で指定された英数字以外の文字の要件を満たしていない場合の新しいパスワードを入力する場合、エラー メッセージが表示されることに注意してください (図 7 を参照してください)。


[![ChangePassword コントロールをページに追加します。](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**図 7**: ChangePassword コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image21.png))


ユーザーの古いパスワードと、ログオンしている有効な新しいパスワードを入力時にパスワードが変更され、成功ビューが表示されます。

### <a name="sending-a-confirmation-email"></a>確認の電子メールを送信します。

既定では、ChangePassword コントロールは、パスワードが更新されました、ユーザーに電子メール メッセージを送信しません。 電子メールを送信する場合は、単に構成するコントロールの`MailDefinition`プロパティです。 ユーザーは、新しいパスワードを含む HTML 形式の電子メールを送信できるようにパスワードの変更をするコントロールを構成してみましょう。

新しいファイルを作成して開始、`EmailTemplates`という名前のフォルダー`ChangePassword.htm`です。 次のマークアップを追加します。

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

次に、設定、パスワードの変更をコントロールの`MailDefinition`プロパティの`BodyFileName`、 `IsBodyHtml`、および`Subject`プロパティ ~ EmailTemplates/ChangePassword.htm、true の場合、およびパスワードが変更された/!、それぞれします。

これらの変更を行った後、ページを再確認し、もう一度パスワードを変更します。 現時点では、ChangePassword コントロール、HTML 形式のカスタマイズされたメールを送信ファイル上のユーザーの電子メール アドレス (図 8 を参照してください)。


[![ユーザーをそのパスワードが変更通知の電子メール メッセージ](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**図 8**: に通知する電子メール メッセージ、ユーザー、そのパスワードが変更 ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>手順 3: ユーザーのパスワードを変更する管理者を許可します。

ユーザー アカウントをサポートするアプリケーションで共通の機能は、他のユーザーのパスワードを変更する管理ユーザーの機能です。 場合によって、システムがユーザーに、自分のパスワードを変更する機能がないためには、この機能が必要です。 このような場合は、そのパスワードを回復するユーザーの唯一の方法では、管理者に、新しいパスワードを割り当てます。 PasswordRecovery と ChangePassword コントロールを使用ただし、管理ユーザー必要がありますビジーになっていない自体、ユーザーのパスワードの変更をユーザーがこの自体の処理を実行できるようです。

しかし、場合、クライアントでは管理ユーザーが他のユーザーのパスワードを変更できることと主張してでしょうか。 残念ながら、この機能を追加すると、作業のビットを指定できます。 ユーザーのパスワードを変更するには、新旧両方のパスワードを指定する必要があります、`MembershipUser`オブジェクトの`ChangePassword`メソッドは、管理者は、それを変更するのには、ユーザーのパスワードを知っている含めることはできません。

1 つの回避策では、最初に、ユーザーのパスワードをリセットし、次のようにコードを使用して新しいパスワードを変更します。

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

このコードの開始に関する情報を取得することによって*ユーザー名*、これは、ユーザーを変更することを希望管理者パスワードを持つ。 次に、`ResetPassword`メソッドが呼び出され、どの割り当てとユーザーの新しいランダムなパスワード。 パスワードをランダムに生成されたが、メソッドによって返され、変数に格納されている`resetPwd`です。 呼び出しを使用して変更できますが、ユーザーのパスワードがわかった`ChangePassword`です。

問題は、このコードは、メンバーシップ システムの構成が設定されている場合のみが機能するように`RequiresQuestionAndAnswer`は False です。 場合`RequiresQuestionAndAnswer`です。 アプリケーションでは、True の場合は、次に、`ResetPassword`メソッドは、セキュリティの回答を渡される必要がある、それ以外の場合、例外がスローされます。

まだ、クライアントでは管理者が、ユーザーのパスワードを変更できることと主張してメンバーシップ フレームワークがセキュリティの質問と解答を要求するように構成されている場合、次の 3 つのオプションがあります。

- 無線で自分の手をスローし、実行できない処理を 1 つだけのものであることをクライアントに指示します。
- 設定`RequiresQuestionAndAnswer`を false に設定します。 これにより、安全性の低いアプリケーション。 悪意のユーザーが別のユーザーの電子メールの受信トレイへのアクセスを得られることを想像してください。 おそらく侵害されているユーザーは各自のデスク昼食に移動する状態になっているし、そのワークステーションをロックしませんでしたしたり可能性があります、公共の端末から自分の電子メールにアクセス サインアウトがありませんでした。どちらの場合、悪意のユーザーがアクセスできる、`RecoverPassword.aspx`ページし、ユーザーのユーザー名を入力します。 システムは、セキュリティの回答を求めることがなく回復パスワードを電子メールです。
- メンバーシップ framework および SQL Server データベースを直接操作によって作成された抽象化レイヤーをバイパスします。 メンバーシップのスキーマには、という名前のストアド プロシージャが含まれています。`aspnet_Membership_SetPassword`をユーザーのパスワードを設定し、そのタスクを実行するためにセキュリティの回答や古いパスワードは必要ありません。

これらのオプションは特に魅力的なものが、方法、開発者の有効期間になる場合があります。

あらかじめし、3 番目の手法をバイパスするコードの記述を実装すれば、`Membership`と`MembershipUser`クラスし、直接作用、`SecurityTutorials`データベース。

> [!NOTE]
> データベースを直接操作、メンバーシップ、フレームワークによって提供されるカプセル化が割れてます。 この決定を結び付けること、 `SqlMembershipProvider`、移植可能な以下のコードを作成します。 さらに、このコードが期待どおりに将来のバージョンの ASP.NET メンバーシップ スキーマが変更された場合は機能しません。 この方法は、問題を回避する、およびほとんどの回避策のようにないベスト プラクティスの例です。


コードは、一部が不適切になる bits を持ち、非常に長きます。 そのため、しないことの詳細な検査を使用してこのチュートリアルの煩雑化します。 詳細に知りたい場合は、このチュートリアルとアクセス用コードをダウンロード、`~/Administration/ManageUsers.aspx`ページ。 このページで作成した、 <a id="_msoanchor_5"> </a>[前のチュートリアル](building-an-interface-to-select-one-user-account-from-many-vb.md)、各ユーザーを一覧表示します。 リンクを含める GridView を更新すれば、 `UserInformation.aspx`  ページで、クエリ文字列を選択したユーザーのユーザー名を渡します。 `UserInformation.aspx`ページには、自分のパスワードを変更するためのテキスト ボックスと選択したユーザーに関する情報が表示されます (図 9 を参照してください)。

ポストバックに陥ります新しいパスワードを入力する、2 つ目のテキスト ボックスで確認し、更新ユーザー ボタンをクリックした後、`aspnet_Membership_SetPassword`ストアド プロシージャが呼び出される、ユーザーのパスワードを更新します。 それらをコードに慣れるやり直して、パスワードを変更したユーザーに電子メールを送信する対象の機能を拡張するこの機能に興味リーダーことをお勧めします。


[![管理者がユーザーのパスワードを変更します。](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**図 9**: 管理者がユーザーのパスワードを変更する ([フルサイズのイメージを表示するをクリックして](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> `UserInformation.aspx`ページは現在動作のみをクリアまたは Hashed 形式でパスワードを保存するメンバーシップ フレームワークが構成されている場合。 この機能を追加する招待されていますが、新しいパスワードを暗号化するためのコードが不足しています。 必要なコードを追加することをお勧めの方法は、のようにコンパイラを使用する[Reflector](http://www.aisto.com/roeder/dotnet/) ; .NET Framework のメソッドのソース コードを調べることを調べることで開始、`SqlMembershipProvider`クラスの`ChangePassword`メソッドです。 この手法を使用して、パスワードのハッシュを作成するためのコードを記述します。


## <a name="summary"></a>概要

ASP.NET には、ユーザーが自分のパスワードを管理できる 2 つのコントロールが用意されています。 PasswordRecovery コントロールは、自分のパスワードを忘れてしまった方のために役立ちます。 メンバーシップ フレームワークの構成によっては、ユーザーは、既存のパスワードや、ランダムに生成された新しいパスワードか、電子メールで送信します。 ChangePassword コントロールにより、パスワードを更新できます。

ログインと CreateUserWizard コントロールと同様には、PasswordRecovery と ChangePassword のコントロールは、宣言型マークアップがまったくまたは行のコードを記述することがなく、豊富なユーザー インターフェイスをレンダリングします。 既定のユーザー インターフェイスがお客様のニーズを満たしていない場合は、さまざまなスタイルのプロパティをカスタマイズできます。 代わりに、コントロールのインターフェイスも細かく制御のためのテンプレートに変換することがあります。 バック グラウンドでこれらのコントロールは、メンバーシップ API を使用して呼び出し、`MembershipUser`オブジェクトの`ResetPassword`と`ChangePassword`メソッドです。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ChangePassword コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [PasswordRecovery コントロールのクイック スタート](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [ASP.NET で電子メールを送信します。](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`よく寄せられる質問](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者には、Michael Emmings および Suchi Banerjee が含まれます。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](building-an-interface-to-select-one-user-account-from-many-vb.md)
[次へ](unlocking-and-approving-user-accounts-vb.md)
