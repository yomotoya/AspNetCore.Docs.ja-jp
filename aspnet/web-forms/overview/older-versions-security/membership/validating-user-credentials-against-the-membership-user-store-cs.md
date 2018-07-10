---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: メンバーシップ ユーザー ストア (c#) に対してユーザー資格情報の検証 |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、プログラムによる方法と、ログイン コントロールの両方を使用して、メンバーシップ ユーザー ストアに対してユーザーの資格情報を検証する方法を説明しています.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: e8c46d09a7ebab19204f7c439ec4333e0c36b73e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828956"
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>メンバーシップ ユーザー ストア (c#) に対してユーザー資格情報を検証しています
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> このチュートリアルでは、プログラムによる方法と、ログイン コントロールの両方を使用して、メンバーシップ ユーザー ストアに対してユーザーの資格情報を検証する方法を説明します。 ログイン コントロールの外観と動作をカスタマイズする方法に注目するはもします。


## <a name="introduction"></a>はじめに

<a id="Tutorial05"> </a>[前のチュートリアル](creating-user-accounts-cs.md)メンバーシップ フレームワークで新しいユーザー アカウントを作成する方法を説明しました。 最初にプログラムで使用してユーザー アカウントを作成しました、`Membership`クラスの`CreateUser`メソッド、CreateUserWizard Web コントロールを使用して、調査とします。 ただし、ログイン ページは現在のユーザー名とパスワードのペアのハード コーディングされた一覧を指定された資格情報を検証します。 メンバーシップ フレームワークのユーザー ストアに対して資格情報を検証するために、ログイン ページのロジックを更新する必要があります。

はるかのようなユーザー アカウントの作成と資格情報検証できるプログラムから、または宣言によって。 メンバーシップ API には、ユーザー ストアに対してユーザーの資格情報をプログラムで検証するためのメソッドが含まれています。 ASP.NET がユーザー名とパスワード ログインするためのボタンのテキスト ボックスにユーザー インターフェイスをレンダリングする、ログイン Web コントロールが付属しています。

このチュートリアルでは、プログラムによる方法と、ログイン コントロールの両方を使用して、メンバーシップ ユーザー ストアに対してユーザーの資格情報を検証する方法を説明します。 ログイン コントロールの外観と動作をカスタマイズする方法に注目するはもします。 それでは、始めましょう!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>手順 1: メンバーシップ ユーザー ストアに対して資格情報の検証

フォーム認証を使用する web サイトでは、ユーザー ログオンの web サイトにログイン ページにアクセスして資格情報を入力します。 これらの資格情報は、ユーザー ストアに対してと比較されます。 有効な場合は、ユーザーの間でフォーム認証チケット、id および訪問者の信頼性を示すセキュリティ トークンが付与されます。

メンバーシップ フレームワークを対象にユーザーを検証するには、使用、`Membership`クラスの[`ValidateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)します。 `ValidateUser`メソッドは 2 つの入力パラメーター - *`username`* と*`password`* -資格情報が有効かどうかを示すブール値を返します。 使用するような`CreateUser`メソッドの前のチュートリアルで調べる、`ValidateUser`メソッドが実際の検証を構成したメンバーシップ プロバイダーを委任します。

`SqlMembershipProvider`を使用して、指定したユーザーのパスワードを取得することによって指定された資格情報を検証、`aspnet_Membership_GetPasswordWithFormat`ストアド プロシージャ。 いることを思い出してください、 `SqlMembershipProvider` 3 つの形式のいずれかを使用してユーザーのパスワードを格納: clear、暗号化、またはハッシュします。 `aspnet_Membership_GetPasswordWithFormat`ストアド プロシージャは、未加工の形式でパスワードを返します。 暗号化またはハッシュされたパスワードの場合、`SqlMembershipProvider`変換、 *`password`* に渡された値、`ValidateUser`暗号化をそれと同等のメソッド、または状態をハッシュされから返された結果と比較、データベース。 データベースに格納されているパスワードには、ユーザーが入力した書式設定されたパスワードが一致すると、資格情報が無効です。

このログイン ページを更新しましょう (~/`Login.aspx`) メンバーシップ フレームワークのユーザー ストアに対して指定された資格情報を検証します。 このログイン ページを作成したに戻り、 <a id="Tutorial02"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、ユーザー名とパスワードの 2 つのテキスト ボックスで、インターフェイスの作成をチェック ボックスをオンし、ログイン ボタンに、そのアカウントを記憶する (図 1 参照)。 コードでは、ハード コーディングされた (Scott/パスワード、Jisun/パスワード、および Sam/パスワード) は、ユーザー名とパスワードのペアのリストに対して入力した資格情報を検証します。 <a id="Tutorial03"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)チュートリアルは、フォームに追加情報を格納する、ログイン ページのコードを更新しました認証チケットの`UserData`プロパティ。


[![ログイン ページのインターフェイスには、2 つのテキスト ボックス、CheckBoxList、およびボタンが含まれます。](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**図 1**: [ログイン ページのインターフェイスが含まれています 2 つのテキスト ボックス、CheckBoxList、およびボタン ([フルサイズの画像を表示する] をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))。


ログイン ページのユーザー インターフェイスは変わらないことができますが、ログイン ボタンのボタンに置き換える必要があります`Click`メンバーシップ フレームワークのユーザー ストアに対してユーザーを検証するコードを持つイベント ハンドラー。 イベント ハンドラーは、そのコードは次のように表示されるように更新します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

このコードでは、非常に単純です。 まず、呼び出すことによって、`Membership.ValidateUser`メソッドを指定されたユーザー名とパスワードを渡します。 そのメソッドが true を返すかどうかは、ユーザーが経由でのサイトにサインインして、`FormsAuthentication`クラスの RedirectFromLoginPage メソッド。 (で説明したように、 <a id="Tutorial2"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、`FormsAuthentication.RedirectFromLoginPage`フォーム認証チケットを作成し、ユーザーをリダイレクトします適切なページです。)資格情報が無効の場合、ただし、`InvalidCredentialsMessage`ラベルを表示すると、ユーザーに知らせることがユーザー名またはパスワードが間違っています。

すべてです。

ログイン ページが期待どおりに動作をテストするには、前のチュートリアルで作成したユーザー アカウントのいずれかでログインするしようとします。 または、いずれかから作成してください、アカウントをまだ作成がない場合、`~/Membership/CreatingUserAccounts.aspx`ページ。

> [!NOTE]
> 自分のパスワードを含む、資格情報がで web サーバーにインターネット経由で転送されるときに、ユーザーは自分の資格情報を入力し、ログイン ページのフォームを送信する、*プレーン テキスト*します。 つまり、ネットワーク トラフィックをスニッフィング ハッカーは、ユーザー名とパスワードを確認できます。 これを防ぐにを使用して、ネットワーク トラフィックを暗号化するために不可欠[セキュリティで保護されたソケット レイヤー (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)します。 これにより、web サーバーによって受信されるまで、ブラウザーを離れる時点から、資格情報 (およびその全体のページの HTML マークアップ) を暗号化することが保証されます。


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>メンバーシップ フレームワークが無効なログイン試行を処理する方法

訪問者は、ログイン ページに達するし、自分の資格情報を送信する、ブラウザーは、ログイン ページに HTTP 要求を作成します。 資格情報が有効な場合は、HTTP 応答には、cookie に認証チケットが含まれています。 そのため、サイトに分割しようとしています。 ハッカーは徹底的有効なユーザー名とパスワードの推測されたログイン ページに HTTP 要求を送信するプログラムを作成します。 パスワードの推測が正しい場合は、ログイン ページは、プログラムが有効なユーザー名/パスワードのペアを偶然を知ってこの時点で、認証チケットのクッキーを返します。 ブルート フォース攻撃、によっては、パスワードが脆弱な場合に特にに、このようなプログラムがユーザーのパスワードに遭遇すること可能性があります。

このようなブルート フォース攻撃を防ぐためには、メンバーシップ フレームワークを一定期間内で失敗したログイン試行数がある場合、ユーザーをロックします。 正確なパラメーターは、次の 2 つのメンバーシップ プロバイダーの構成設定を使用して構成できます。

- `maxInvalidPasswordAttempts` -無効なパスワードの数を指定します、アカウントがロックアウトされるまでの期間内のユーザーの試行が許可されます。既定値は 5 です。
- `passwordAttemptWindow` -指定された無効なログイン試行回数が、アカウントがロックアウトを原因を分単位で期間を示します。既定値は 10 です。

ユーザーがロックアウトされている場合は、管理者が自分のアカウントをロック解除まで彼女ログインできません。 ユーザーがロックアウトされた場合、`ValidateUser`メソッドは*常に*を返す`false`場合でも、有効な資格情報を入力します。 この動作は、ハッカーはサイト ブルート フォース攻撃の方法で分割される可能性を軽減、中には、ユーザーが自分のパスワードを忘れた単に、Capslock にまたは誤って不適切な入力日がある有効なユーザーがロックアウトを終了できます。

残念ながら、ユーザー アカウントをロック解除用の組み込みのツールではありません。 アカウントのロックを解除するには、データベースを変更することができますを直接の変更、`IsLockedOut`フィールドに、 `aspnet_Membership` - 適切なユーザー アカウントのテーブルか、web ベースのインターフェイスを一覧表示、保護を解除するためのオプションを持つアカウントのロックアウトを作成します。 今後のチュートリアルでは、一般的なユーザー アカウントと役割に関連するタスクを実行するための管理インターフェイスの作成を見ていきます。

> [!NOTE]
> 1 つの欠点、`ValidateUser`メソッドは、指定された資格情報が有効でない場合に提供されません理由についても説明します。 資格情報、ユーザー ストアに一致するユーザー名とパスワードの組み合わせがないため、ユーザーが承認されていないため、またはユーザーがロックアウトされているために有効なことがあります。手順 4 は、そのログインが失敗したときに、ユーザーに詳細なメッセージを表示する方法が表示されます。


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>手順 2: ログインの Web コントロールを使用して資格情報の収集

[ログイン Web コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)に戻り作成したものとよく似ています、既定のユーザー インターフェイスを表示、 <a id="SKM5"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアル。 ログイン コントロールを使用する私たちと保存、ユーザー資格情報を収集するインターフェイスを作成するための作業します。 さらに、Login コントロールが自動的にサインイン (送信された資格情報が有効なを想定) ユーザーのためご保存コードを記述する必要がなくなります。

更新`Login.aspx`、手動で作成したインターフェイスを交換およびコードでは、Login コントロール。 既存のマークアップを削除することで起動し、コードで`Login.aspx`します。 削除しても、最初から、または単にコメント アウトする可能性があります。宣言型マークアップをコメントに、それを囲む、`<%--`と`--%>`区切り記号。 これらの区切り記号を手動で入力することができますか、図 2 に示すよう、コメント アウトし、ツールバーのアイコンを選択した行をコメントをクリックするテキストを選択します。 同様に、選択した行のアイコンをコメントを使用すると、分離コード クラスで選択したコードをコメント アウトします。


[![既存の宣言型マークアップと Login.aspx 内のソース コード コメント](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**図 2**: コメント アウト、既存宣言マークアップとソース コードで`Login.aspx`([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))。


> [!NOTE]
> Visual Studio 2005 で宣言型マークアップを表示するときに、選択した行のアイコンをコメントは使用できません。 Visual Studio 2008 を使用していない場合は、手動で追加する必要があります。、`<%--`と`--%>`区切り記号。


次に、ログイン コントロールをページにツールボックスからドラッグし、設定、`ID`プロパティを`myLogin`します。 この時点で、画面に図 3 のようになります。 ログイン コントロールの既定のインターフェイスがユーザー名とパスワードを次回チェック ボックスをオンおよびログの ボタンのテキスト ボックス コントロールが含まれることに注意してください。 `RequiredFieldValidator`の 2 つのテキスト ボックス コントロール。


[![Login コントロールをページに追加します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**図 3**: Login コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))。


完了します。 ログイン コントロールのログイン ボタンがクリックされたときにポストバックが発生し、Login コントロールが呼び出す、`Membership.ValidateUser`メソッド、入力したユーザー名とパスワードを渡します。 資格情報が有効でない場合、Login コントロールには、このようなことを示すメッセージが表示されます。 ただし、資格情報が有効で場合、Login コントロールは、フォーム認証チケットを作成し、ユーザーを適切なページにリダイレクトします。

Login コントロールでは、4 つの要因を使って、正常にログインとユーザーをリダイレクトする、適切なページを決定します。

- 定義されているのログイン ページで、Login コントロールがかどうかは`loginUrl`この設定の既定値は、フォーム認証の構成の設定 `Login.aspx`
- 有無、 `ReturnUrl` querystring パラメーター
- ログイン コントロールの値[`DestinationUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl`フォーム認証の構成設定に指定された値は、この設定の既定値は `Default.aspx`

図 4 は、方法を示しています、Login コントロールがそのページの適切な意思決定に到着するこれら 4 つのパラメーターを使用します。


[![Login コントロールをページに追加します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**図 4**: Login コントロールをページに追加 ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))。


ブラウザーを使用してサイトを訪問し、メンバーシップ フレームワークで既存のユーザーとしてログインして、ログイン コントロールをテストする時間がかかります。

ログイン コントロールのレンダリングされたインターフェイスでは、高度に構成できます。 その外観を与えるプロパティのいくつかさらに、Login コントロールがテンプレートのレイアウトを細かく制御する場合にユーザー インターフェイス要素変換することができます。 この手順の残りの部分では、レイアウトと外観をカスタマイズする方法について説明します。

### <a name="customizing-the-login-controls-appearance"></a>ログイン コントロールの外観のカスタマイズ

ログイン コントロールの既定のプロパティ設定のタイトルと共に (ログ) のユーザー インターフェイスをレンダリングする、チェック ボックス、および Log In ボタン、アカウントを記憶 ユーザー名とパスワード入力用テキスト ボックス、およびラベル コントロールが次に時刻します。 これらの要素の外観は、ログイン コントロールの多数のプロパティをすべて構成可能です。 さらに、プロパティ、または 2 つの設定 - 新しいユーザー アカウントを作成するページへのリンクなどの他のユーザー インターフェイス要素を追加できます。

では、ユーザーのログイン コントロールの外観を増やさずに少しご説明します。 以降、`Login.aspx`ページが既にログインを示すページの上部にあるテキスト、ログイン コントロールのタイトルは不要です。 したがって、クリア、 [ `TitleText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx)ログイン コントロールのタイトルを削除するには値。

: ユーザー名とパスワード: を通じての、2 つの TextBox コントロールの左にラベルをカスタマイズできる、 [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx)と[`PasswordLabelText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)、それぞれします。 ユーザー名を変更してみましょう。 Username を読み取るためのラベル: です。 ラベルとテキスト ボックスのスタイルを使用して構成できますが、 [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx)と[`TextBoxStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)、それぞれします。

メール アドレス、ログイン コントロールから、[次へ] の時間チェック ボックスの Text プロパティを設定できます[ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)、および既定のチェック状態を使用して構成できますが、 [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (既定値は False)。 さあ、設定、`RememberMeSet`プロパティをチェック ボックスを次にときにアカウントを記憶する を True には、既定でチェックします。

Login コントロールは、そのユーザー インターフェイス コントロールのレイアウトを調整するための 2 つのプロパティを提供します。 [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)を示すかどうか、ユーザー名: とパスワード: またはの上に (既定)、対応する、テキスト ボックスの左にラベルが表示されます。 [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)ユーザー名とパスワードの入力が垂直方向に設置されているかどうかを示します (上、もう 1 つ) または水平方向にします。 これら 2 つのプロパティには既定では、設定のままにしますが、ぜひこれら 2 つのプロパティを結果として得られる効果を表示する、既定以外の値に設定してください。

> [!NOTE]
> 次のセクションで、ログイン コントロールのレイアウトを構成するに注目レイアウト コントロールのユーザー インターフェイス要素の正確なレイアウトを定義するテンプレートを使用します。


設定して、ログイン コントロールのプロパティの設定をまとめ、 [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx)と[`CreateUserUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx)にまだ登録されていませんか? アカウントを作成します。 `~/Membership/CreatingUserAccounts.aspx`、それぞれします。 これで作成したページを指す、ログイン コントロールのインターフェイスにハイパーリンクを追加、 <a id="SKM6"> </a>[前のチュートリアル](creating-user-accounts-cs.md)します。 ログイン コントロールの[ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx)と[`HelpPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx)と[ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx)と[ `PasswordRecoveryUrl` プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)ヘルプ ページと、パスワード回復ページへのリンクを表示、同じ方法で作業します。

これらのプロパティの変更を行った後、ログイン コントロールの宣言型マークアップと外観のようなります図 5 に示されています。


[![ログイン コントロールのプロパティの値の外観を決定します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**図 5**: の外観をディクテーション、ログイン コントロールのプロパティの値 ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))。


### <a name="configuring-the-login-controls-layout"></a>ログイン コントロールのレイアウトを構成します。

ログイン Web コントロールの既定のユーザー インターフェイスを html インターフェイス レイアウト`<table>`します。 しかしレンダリングされた出力をより細かく制御が必要な場合でしょうか。 置換することができます、`<table>`は一連の`<div>`タグ。 または、アプリケーションが認証の追加の資格情報に必要な場合でしょうか。 多くの財務 web サイトには、ユーザーだけでなく、ユーザー名とパスワードも個人識別番号 (PIN)、または他の識別情報を指定するなどが必要です。 任意理由かもしれませんがが Login コントロールにテンプレートを明示的に定義できます、インターフェイスの宣言型マークアップを変換します。

追加の資格情報を収集するログイン コントロールを更新するには 2 つの作業を行う必要があります。

1. 含める追加の資格情報を収集する Web コントロール、ログイン コントロールのインターフェイスを更新します。
2. ユーザー名とパスワードが有効とその他の資格情報が有効ですぎる場合にのみ、ユーザーが認証されるように、ログイン コントロールの内部の認証ロジックをオーバーライドします。

最初のタスクを実行するには、ログイン コントロールをテンプレートに変換し、必要な Web コントロールを追加する必要があります。 2 番目のタスクとは、コントロールのイベント ハンドラーを作成して、ログイン コントロールの認証ロジックが優先されることが[`Authenticate`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)します。

指定された電子メール アドレス、電子メール アドレス宛に一致する場合にのみユーザーを認証し、ユーザー名、パスワード、および電子メール アドレスのユーザー入力を求めるようにしてみましょう Login コントロールを更新します。 まず、ログイン コントロールのインターフェイスをテンプレートに変換する必要があります。 ログイン コントロールのスマート タグから変換テンプレート オプションを選択します。


[![Login コントロールをテンプレートに変換します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**図 6**: Login コントロールをテンプレートに変換 ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))。


> [!NOTE]
> Login コントロールの template 前のバージョンに戻すには、コントロールのスマート タグからのリセット リンクをクリックします。


Login コントロールをテンプレートに変換を追加、 `LayoutTemplate` HTML 要素およびユーザー インターフェイスを定義する Web コントロールを使用するコントロールの宣言型マークアップにします。 図 7 に示すようコントロールをテンプレートに変換するさまざまなプロパティから削除プロパティ ウィンドウなど`TitleText`、`CreateUserUrl`などのように、ため、テンプレートを使用する場合、これらのプロパティ値は無視されます。


[![以下のプロパティは、使用可能なときのログイン コントロールがテンプレートに変換されます。](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**図 7**: より少ないプロパティが使用可能なときのログイン コントロールがテンプレートに変換されます ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))。


内の HTML マークアップ、`LayoutTemplate`必要に応じて変更することがあります。 同様に、自由にテンプレートに新しい Web コントロールを追加します。 ただし、重要なは、そのログイン コントロールのコア Web コントロールがテンプレートに保持され、割り当てられた保持は`ID`値。 具体的には、削除したりしないでの名前を変更、`UserName`または`Password`、テキスト ボックス、`RememberMe`チェック ボックスをオン、 `LoginButton`  ボタン、`FailureText`ラベル、または`RequiredFieldValidator`コントロール。

訪問者の電子メール アドレスを収集するには、テンプレートにテキスト ボックスを追加する必要があります。 テーブルの行の間で次の宣言型マークアップを追加 (`<tr>`) を格納している、 `Password` TextBox およびアカウントを記憶する を次に保持するテーブルの行の時間をチェック ボックスをオンします。

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

追加した後、 `Email`  ボックスに、ブラウザーを使用してページを参照してください。 図 8 に示すよう、ログイン コントロールのユーザー インターフェイスの 3 つ目のテキスト ボックスに追加されました。


[![Login コントロールのユーザーの電子メール アドレス テキスト ボックスになりました](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**図 8**: Login コントロールのユーザーの電子メール アドレス テキスト ボックスになりました ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))。


この時点では、Login コントロールがまだを使用して、`Membership.ValidateUser`指定された資格情報を検証するメソッド。 値が同様に、入力、 `Email` TextBox は、ユーザーがログインできるかどうかに影響を与えません。 手順 3 では、資格情報、ユーザー名とパスワードが有効とファイルの電子メール アドレスが一致する指定した電子メール アドレスの場合有効と見なされるのみログイン コントロールの認証ロジックをオーバーライドする方法に注目します。

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>手順 3: ログイン コントロールの認証ロジックを変更します。

彼女の訪問者を指定すると、数回のクリック、[ログイン] ボタン、ポストバックに陥ります、ログインの資格情報、認証ワークフローの進捗状況を制御します。 ワークフローを開始することで、 [ `LoggingIn`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx)します。 このイベントに関連付けられたイベント ハンドラーを設定して操作のログを取り消すことが、`e.Cancel`プロパティを`true`します。

発生させることによって、ワークフローの進行の操作のログがキャンセルされない場合、 [ `Authenticate`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)します。 イベント ハンドラーがある場合、`Authenticate`イベント、されますか、指定された資格情報が有効かどうかを判断する責任を負います。 Login コントロールを使用してイベント ハンドラーが指定されていない場合、`Membership.ValidateUser`資格情報の有効性を判断するメソッド。

指定された資格情報は、有効なかどうかは、フォーム認証チケットを作成すると、 [ `LoggedIn`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx)が発生し、ユーザーは、適切なページにリダイレクトされます。 ただし、資格情報と見なされる、無効な場合は、 [ `LoginError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx)が発生し、自分の資格情報が有効なされていないことをユーザーに通知メッセージが表示されます。 既定では、失敗した場合、ログイン コントロールだけを設定、 `FailureText` (ログインの試行に失敗しましたエラー メッセージをコントロールのテキスト プロパティ ラベルします。 もう一度やり直してください)。 ただし場合、ログイン コントロールの[`FailureAction`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx)に設定されている`RedirectToLoginPage`、ログイン管理の問題から、`Response.Redirect`クエリ文字列パラメーターを追加するログイン ページに`loginfailure=1`(これにより、ログインコントロールをエラー メッセージを表示する)。

図 9 には、認証ワークフローのフロー チャートが用意されています。


[![ログイン コントロールの認証ワークフロー](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**図 9**:、ログイン コントロールの認証ワークフロー ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))。


> [!NOTE]
> 使用する場合を迷っている場合、`FailureAction`の`RedirectToLogin`オプションのページで、次のシナリオを検討してください。 今すぐ、`Site.master`マスター ページは、現在、こんにちは、よそ者に表示されるテキスト、匿名ユーザーに表示したときに、左側の列をいますが、ログイン コントロールでそのテキストを置き換えますしたいことを想像してください。 匿名ユーザーがログイン ページに直接アクセスを必要とするのではなく、サイトの任意のページからのログインをこのようにするとします。 ただし、ユーザーは、マスター ページでレンダリングのログイン コントロールを使用してログインできなくなりますが場合、有意義なことをログイン ページにリダイレクトする (`Login.aspx`) そのページの可能性には、その他については、リンク、およびリンクを作成するなどの他のヘルプが含まれているため、新しいアカウントまたはマスター ページに追加されませんでした - 紛失のパスワードを取得します。


### <a name="creating-theauthenticateevent-handler"></a>作成、`Authenticate`イベント ハンドラー

ログイン コントロールのイベント ハンドラーを作成する、カスタム認証ロジックをプラグインするために`Authenticate`イベント。 イベント ハンドラーの作成、`Authenticate`イベントは、次のイベント ハンドラーの定義を生成します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

ご覧のとおり、`Authenticate`イベント ハンドラーの型のオブジェクトが渡される[ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) 2 番目の入力パラメーターとして。 `AuthenticateEventArgs`クラスには、という名前のブール型プロパティが含まれています。`Authenticated`指定された資格情報が有効かどうかを指定するために使用されます。 このタスクは、か、指定された資格情報が有効かどうかを決定します。 コードをここで記述し、を設定する、`e.Authenticate`プロパティに応じて。

### <a name="determining-and-validating-the-supplied-credentials"></a>決定し、指定された資格情報を検証しています

使用して、ログイン コントロールの[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx)と[`Password`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx)をユーザーが入力したユーザー名とパスワードの資格情報を確認します。 Web コントロールに入力された値を決定するために (など、`Email`テキスト ボックスに、前の手順で追加された) を使用して*`LoginControlID`* `.FindControl`("*`controlID`*") を取得するプログラムへの参照 Web コントロール テンプレートを持つ`ID`プロパティは等しく *`controlID`* します。 参照を取得する例を`Email` ボックスに、次のコードを使用します。

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

ユーザーの資格情報を検証するためには、2 つの作業を行う必要があります。

1. 指定されたユーザー名とパスワードが有効であることを確認します。
2. 入力した電子メール アドレスがログインしようとしてください。 ユーザーのファイルの電子メール アドレスと一致することを確認します。

使用できる最初のチェックを実行する、`Membership.ValidateUser`メソッド手順 1. で説明したようにします。 2 番目のチェックでは、TextBox コントロールに入力した電子メール アドレスにこれと比較できるように、ユーザーの電子メール アドレスを決定する必要があります。 特定のユーザーに関する情報を取得する、`Membership`クラスの[`GetUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)します。

`GetUser`メソッドがオーバー ロードの数。 すべてのパラメーターに渡されずに使用する場合は、現在ログインしているユーザーに関する情報を返します。 特定のユーザーに関する情報を取得するには、呼び出す`GetUser`ユーザー名を渡します。 どちらの方法でも、`GetUser`を返します、 [ `MembershipUser`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)などのプロパティを持つ`UserName`、 `Email`、 `IsApproved`、`IsOnline`など。

次のコードでは、これら 2 つのチェックを実装します。 次の両方の成功した場合`e.Authenticate`に設定されている`true`、それ以外の場合、割り当てられている`false`。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

このコードでは、適切なユーザー名、パスワード、および電子メール アドレスを入力する、有効なユーザーとしてログインを試みます。 もう一度お試しくださいがこの時間は意図的に正しくない電子メール アドレスを使用して (図 10 参照)。 最後に、試して、存在しないユーザー名を使用して 3 回目です。 最初のケースでする必要がありますが正常にログオンして、サイトが最後の 2 つの場合、ログイン コントロールの無効な資格情報のメッセージが表示されます。


[![Tito が正しくない電子メール アドレスを指定するときにログインできません。](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**図 10**: Tito ことはできませんログのときに指定が正しくない電子メール アドレス ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))。


> [!NOTE]
> 手順 1 で、方法、メンバーシップ フレームワークを処理無効なログイン試行のセクションで説明したときに、`Membership.ValidateUser`の無効なログイン試行を追跡し、特定数を超える場合、ユーザーをロックアウトするメソッドが呼び出され、無効な資格情報を渡して、指定された時間ウィンドウ内の無効な試行のしきい値。 以降、カスタム認証ロジックの呼び出し、`ValidateUser`メソッド、間違ったパスワードの有効なユーザー名には、無効なログイン試行カウンターが 1 ずつ増分されますが、このカウンターは、ユーザー名とパスワードは、有効な場合は加算されませんが、電子メール アドレスが正しくありません。 ハッカーがユーザー名とパスワードがユーザーの電子メール アドレスを決定するブルート フォース手法を使用する必要がないために、この動作は、適切な可能性があります。


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>手順 4: ログイン コントロールの無効な資格情報のメッセージの向上

ユーザーが無効な資格情報でログオンしようとすると、ログイン コントロールには、ログインの試行が成功したことを説明するメッセージが表示されます。 具体的には、コントロールがで指定されたメッセージが表示されます、 [ `FailureText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx)ログインの試行の既定値を持つに失敗しました。 やり直してください。

なぜユーザーの資格情報がありますいない有効な多くの理由があることを思い出してください。

- ユーザー名がない可能性があります。
- ユーザー名が存在しますが、パスワードが無効です。
- ユーザー名とパスワードが有効では、ユーザーがまだ承認されていません。
- ユーザー名とパスワードが有効では、ユーザーがロックアウトされている出力 (指定した期間内の無効なログイン試行の回数を超過したため、最も高い)

カスタムの認証ロジックを使用する場合、その他の理由にすることがあります。 たとえば、手順 3 では、ユーザー名で作成したコードとパスワードは、有効な可能性がありますが、電子メール アドレスが正しくない可能性があります。

なぜ、資格情報は、無効なに関係なく、Login コントロールには、同じエラー メッセージが表示されます。 フィードバックのこの不足をロックアウトされているユーザー アカウントが承認されていない、またはユーザーの混乱を招くことはできます。ただし、若干の開発では、使用より適切なメッセージを表示する、ログイン コントロールがおできます。

ユーザーがログイン コントロールで発生し、無効な資格情報でログインしようとしたときにその`LoginError`イベント。 さあと、このイベントのイベント ハンドラーを作成し、次のコードを追加します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

上記のコードを開始するには、ログイン コントロールの`FailureText`プロパティを既定値 (ログインの試行に失敗しました。 もう一度やり直してください)。 次のかどうか、指定したユーザー名は、既存のユーザー アカウントにマップされます。 表示する確認します。 そのため、その結果、調べて場合`MembershipUser`オブジェクトの`IsLockedOut`と`IsApproved`プロパティを確認して、アカウントがロックアウトされているか、承認されていないかどうか。 どちらの場合、`FailureText`プロパティは、対応する値に更新されます。

このコードをテストするには、意図的しようとすると、既存のユーザーとしてログインしますが、正しくないパスワードを使用します。 10 分間のタイム フレーム内の行で 5 回を行い、アカウントをロックアウトします。図 11 に示す試行は常に (正しいパスワード) でも失敗しますが、ここでは、その後のログインよりわかりやすい表示と、アカウントが無効なログインの回数が多すぎるためロックされています。 アカウント ロック解除されたメッセージが管理者にお問い合わせください。


[![Tito が無効なログインを何度もを実行して、ロックアウトされています。](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**図 11**: Tito 実行が無効なログインも間違えるとがされてロックアウト ([フルサイズの画像を表示する をクリックします](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))。


## <a name="summary"></a>まとめ

以前このチュートリアルでは、弊社のログイン ページは、ハード コーディングされたユーザー名/パスワードのペアのリストに対して指定された資格情報を検証します。 このチュートリアルでは、メンバーシップ フレームワークに対して資格情報を検証するページを更新しました。 手順 1 でしましたを使用して、`Membership.ValidateUser`メソッド プログラムを使用します。 手順 2. で置き換え、手動で作成したユーザー インターフェイスとコード ログイン コントロールを使用します。

Login コントロールは、標準的なログインのユーザー インターフェイスをレンダリングし、自動的にメンバーシップ フレームワークに対してユーザーの資格情報を検証します。 さらに、有効な資格情報が発生した場合、Login コントロール ユーザーでサインイン フォーム認証を使用しています。 つまり、完全に機能のログインのユーザー エクスペリエンスはページ、ない余分な宣言型マークアップまたは必要なコードに、ログイン コントロールをドラッグするだけで使用できます。 詳細については、ログイン コントロールは、自由にカスタマイズ、レンダリングされたユーザー インターフェイスと認証のロジックを制御のきめ細かなすることができます。

この時点で、web サイトへの訪問者を作成できます、新しいユーザー アカウントとログをサイトにまだ、認証されたユーザーに基づいて、ページにアクセスを制限することを確認します。 現時点では、任意のユーザーが認証済みまたは匿名では、サイトのいずれかのページを表示できます。 と共に、ユーザー単位のユーザーごとに、サイトのページへのアクセスを制御する機能を持つは、ユーザーによって異なります。 特定のページがあります。 次のチュートリアルでは、アクセスとログインのユーザーに基づくページで機能を制限する方法を検索します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ロックされており、承認されていないユーザーにカスタム メッセージを表示します。](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [ASP.NET 2.0 の検査のメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [方法: ASP.NET ログイン ページを作成します。](https://msdn.microsoft.com/library/ms178331.aspx)
- [ログイン コントロールのテクニカル ドキュメント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [ログイン コントロールを使用します。](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>執筆者紹介

Scott Mitchell、複数の受け取ります書籍の著者と、4GuysFromRolla.com の創設者では、1998 年から、Microsoft Web テクノロジに取り組んできました。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は *[Sams 教える自分で ASP.NET 2.0 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* します。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログ[ http://ScottOnWriting.NET](http://scottonwriting.net/)します。

### <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murphy および Michael Olivero でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)します。

> [!div class="step-by-step"]
> [前へ](creating-user-accounts-cs.md)
> [次へ](user-based-authorization-cs.md)
