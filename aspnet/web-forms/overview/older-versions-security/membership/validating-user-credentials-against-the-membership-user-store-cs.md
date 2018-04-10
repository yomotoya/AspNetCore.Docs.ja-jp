---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: メンバーシップ ユーザーのストア (c#) に対してユーザーの資格情報の検証 |Microsoft ドキュメント
author: rick-anderson
description: このチュートリアルでは、プログラムによる方法と、ログイン コントロールの両方を使用して、メンバーシップ ユーザー ストアに対してユーザーの資格情報を検証する方法を検討しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 484a0f16265ee2d887ee08f6ae7ada47047f1f04
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>メンバーシップ ユーザーのストア (c#) に対してユーザーの資格情報の検証
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> このチュートリアルでは、プログラム手段とログイン コントロールの両方を使用して、メンバーシップ ユーザーのストアに対してユーザーの資格情報を検証する方法を検討します。 ログイン コントロールの外観と動作をカスタマイズする方法をおも取り上げます。


## <a name="introduction"></a>はじめに

<a id="Tutorial05"> </a>[前のチュートリアル](creating-user-accounts-cs.md)メンバーシップ フレームワークで、新しいユーザー アカウントを作成する方法について説明しました。 プログラムでのユーザー アカウントの作成について説明しました最初、`Membership`クラスの`CreateUser`メソッド、しを使用して検証 CreateUserWizard Web コントロールです。 ただし、[ログイン] ページでは、現在のユーザー名とパスワードのペアのリストがハードコードに対して指定された資格情報を検証します。 メンバーシップ フレームワークのユーザーのストアに対して資格情報を検証するように、ログイン ページのロジックを更新する必要があります。

のようにユーザー アカウントの作成と資格情報検証できるプログラムから、または宣言によってです。 メンバーシップ API には、プログラムによって、ユーザーのストアに対してユーザーの資格情報を検証するためのメソッドが含まれています。 ASP.NET がユーザー名およびパスワードにログインするためのボタンのテキスト ボックスにユーザー インターフェイスをレンダリングする、ログイン Web コントロールが付属しています。

このチュートリアルでは、プログラム手段とログイン コントロールの両方を使用して、メンバーシップ ユーザーのストアに対してユーザーの資格情報を検証する方法を検討します。 ログイン コントロールの外観と動作をカスタマイズする方法をおも取り上げます。 開始しましょう!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>手順 1: メンバーシップ ユーザーのストアに対して資格情報の検証

フォーム認証を使用する web サイトでは、ユーザー ログオン、web サイトにログイン ページにアクセスし、自分の資格情報を入力します。 これらの資格情報は、ユーザー ストアとし、比較されます。 有効な場合は、ユーザーは、セキュリティ トークンの id と、ユーザーの信頼性を示すのフォーム認証のチケットを許可します。

メンバーシップ フレームワークに対してユーザーを検証するを使用して、`Membership`クラスの[`ValidateUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)です。 `ValidateUser`メソッドは、2 つの入力パラメーターで使用*`username`*と*`password`* -資格情報が有効であるかどうかを示すブール値を返します。 使用するような`CreateUser`メソッドの前のチュートリアルで確認して、`ValidateUser`メソッドは、構成済みのメンバーシップ プロバイダーに実際の検証を代行します。

`SqlMembershipProvider`経由で指定したユーザーのパスワードを取得することによって指定された資格情報を検証、`aspnet_Membership_GetPasswordWithFormat`ストアド プロシージャです。 注意してください、 `SqlMembershipProvider` 3 つの形式のいずれかを使用してユーザーのパスワードを格納します。 クリア、暗号化、またはハッシュします。 `aspnet_Membership_GetPasswordWithFormat`ストアド プロシージャは、その生の形式でパスワードを返します。 パスワードの暗号化またはハッシュされたパスワードの`SqlMembershipProvider`変換、 *`password`*に渡された値、`ValidateUser`暗号化をそれと同等のメソッド、または状態をハッシュされから返された結果と比較しますデータベースです。 データベースに格納されているパスワードには、ユーザーが入力した書式設定されたパスワードが一致すると、資格情報が無効です。

それでは、ログイン ページを更新 (~/`Login.aspx`) メンバーシップ framework ユーザー ストアに対して指定された資格情報を検証するようにします。 このログイン ページを作成したに戻り、 <a id="Tutorial02"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、ユーザー名とパスワードを 2 つのテキスト ボックスで、インターフェイスの作成、次回のため、チェック ボックスと [ログイン] ボタン (図 1 を参照してください)。 コードでは、(Scott/パスワード、Jisun/パスワード、および Sam/パスワード) は、ユーザー名とパスワードのペアのリストがハードコードに対して入力した資格情報を検証します。 <a id="Tutorial03"> </a> [*フォーム認証の構成と高度なトピック*](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)フォームに追加情報を格納する、ログイン ページのコードに更新されたチュートリアル認証チケットの`UserData`プロパティです。


[![ログイン ページのインターフェイスには、次の 2 つのテキスト ボックス、CheckBoxList、およびボタンが含まれています。](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**図 1**:、ログイン ページのインターフェイスが含まれています 2 つのテキスト ボックス、CheckBoxList、およびボタン ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


ログイン ページのユーザー インターフェイスは未変更ことができますが、ログイン ボタンのボタンに置き換える必要があります`Click`メンバーシップ framework ユーザー ストアに対してユーザーを検証するコードを持つイベント ハンドラー。 そのコードは次のように表示されるように、イベント ハンドラーを更新します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

このコードでは、非常に単純です。 まず、呼び出すことによって、`Membership.ValidateUser`メソッドを指定されたユーザー名とパスワードを渡します。 このメソッドは true を返すかどうかは、経由でのサイトには、ユーザーがサインイン、`FormsAuthentication`クラスの RedirectFromLoginPage メソッドです。 (で説明したよう、 <a id="Tutorial2"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルでは、`FormsAuthentication.RedirectFromLoginPage`フォーム認証チケットを作成し、ユーザーをリダイレクトします適切なページです。)資格情報が無効の場合、ただし、`InvalidCredentialsMessage`ラベルを表示すると、ユーザーに知らせることがユーザー名またはパスワードが間違っています。

すべてであることには!

ログイン ページが正しく動作するをテストするには、前のチュートリアルで作成したユーザー アカウントのいずれかのログインを試みます。 またはから 1 つを作成してください、アカウントをまだ作成がない場合、`~/Membership/CreatingUserAccounts.aspx`ページ。

> [!NOTE]
> Web サーバーにインターネット経由で自分のパスワードを含む、資格情報が転送されるときに、ユーザーは自分の資格情報を入力し、ログイン ページのフォームを送信する、*プレーン テキスト*です。 つまり、ハッカーによるネットワーク トラフィックを調べるには、ユーザー名とパスワードを確認できます。 これを回避するのにを使用して、ネットワーク トラフィックを暗号化するために不可欠[セキュリティで保護されたソケット レイヤー (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer)です。 これにより、web サーバーによって受信されるまでは、ブラウザーを離脱時から資格情報 (だけでなく、全体のページの HTML マークアップ) が暗号化されています。


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>メンバーシップ フレームワークが無効なログイン試行を処理する方法

訪問者がログイン ページに達すると、自分の資格情報を送信するときに、ブラウザーは、ログイン ページに HTTP 要求を作成します。 資格情報が有効な場合は、HTTP 応答には、cookie に認証チケットが含まれています。 そのため、サイトに侵入しようとするハッカーは入念有効なユーザー名とパスワードを推測でログイン ページに HTTP 要求を送信するプログラムを作成する可能性があります。 パスワード推測が正しい場合は、ログイン ページは、認証 cookie チケット、この時点で、プログラムを知っている、有効なユーザー名/パスワードの組み合わせを偶然を返します。 ブルート フォース攻撃、によっては、パスワードが脆弱な場合に特ににこのようなプログラムがユーザーのパスワードに遭遇すること可能性があります。

このようなブルート フォース攻撃を防ぐためには、メンバーシップ フレームワークを一定期間内で失敗したログイン試行数がある場合、ユーザーをロックします。 正確なパラメーターは、次の 2 つのメンバーシップ プロバイダー構成設定で構成できます。

- `maxInvalidPasswordAttempts` -無効なパスワードの数を指定します、アカウントがロックアウトされるまでの期間内のユーザーの試行が許可されます。既定値は 5 です。
- `passwordAttemptWindow` -分を指定した無効なログイン試行数により、アカウントがロックアウトされるまでの時間を示します。既定値は 10 です。

ユーザーがロックアウトされている場合、管理者が自分のアカウントをロック解除まで彼女はログインできません。 ユーザーがロックアウトされた場合、`ValidateUser`メソッドは*常に*返す`false`場合でも、有効な資格情報を入力します。 この動作は、ブルート フォース メソッドによって、サイトにハッカーが中断される可能性が少なくなります、中には、有効なユーザー パスワードを忘れた単にまたは誤って、Capslock にまたは 1 日に無効な入力があるユーザーのロックアウトを終了できます。

残念ながら、ユーザー アカウントをロック解除用の組み込みのツールではありません。 アカウントのロックを解除するために、データベースを変更することができますを直接の変更、`IsLockedOut`フィールドで、`aspnet_Membership`適切なユーザー アカウント用のテーブルか、web ベースのインターフェイスを一覧表示する、保護を解除するオプションを持つアカウントのロックアウトを作成します。 今後のチュートリアルで一般的なユーザー アカウントおよびロールに関連するタスクを実行するための管理インターフェイスの作成を見ていきます。

> [!NOTE]
> 1 つの欠点、`ValidateUser`メソッドは、指定された資格情報が有効でない場合は提供されないこと理由についての説明。 資格情報のユーザー ストアに一致するユーザー名/パスワードの組み合わせがないため、ユーザーが承認されていないためまたはユーザーがロックアウトされているために有効なことがあります。手順 4. で、ログイン試行が失敗したときに、ユーザーをより詳細なメッセージを表示する方法が表示されます。


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>手順 2: ログイン Web コントロールを通じて資格情報の収集

[ログイン Web コントロール](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)に戻り作成されたものとよく似ています、既定のユーザー インターフェイスを表示、 <a id="SKM5"> </a> [*フォーム認証の概要を*](../introduction/an-overview-of-forms-authentication-cs.md)チュートリアルです。 ログイン コントロールを使用して us、作業保存ビジター s 資格情報を収集するためのインターフェイスを作成する必要がなくなります。 さらに、ログイン コントロール自動的にサインアウト (送信済みの資格情報が有効な場合)、ユーザーにそれによって us 保存コードを記述する必要がなくなります。

更新みましょう`Login.aspx`手動で作成したインターフェイスを置き換えること、およびコードでは、ログイン コントロール。 既存のマークアップを削除することで起動し、コードで`Login.aspx`です。 、完全に削除するか、単にコメント アウトできます。宣言型マークアップをコメントには、周囲にあると、`<%--`と`--%>`区切り記号。 これらの区切り記号を手動で入力することができます。 または、図 2 に示すは、コメント アウトし、[ツールバーのアイコンを選択した行をコメント] をクリックするテキストを選択する可能性があります。 同様に、選択した行のアイコンをコメントを使用すると、分離コード クラスで選択したコードをコメント アウトします。


[![既存の宣言型マークアップと Login.aspx 内のソース コードのコメント](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**図 2**: コメント アウト、既存宣言型マークアップおよび内のソース コード`Login.aspx`([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Visual Studio 2005 で、宣言型マークアップを表示するときに、選択した行のアイコンをコメントは使用できません。 Visual Studio 2008 を使用していない場合は、手動で追加する必要があります、`<%--`と`--%>`区切り記号。


次に、ページ、ツールボックスからログイン コントロールをドラッグし、設定、`ID`プロパティを`myLogin`です。 この時点で、画面が図 3 のようになります。 ログイン コントロールの既定のインターフェイスが、ユーザー名とパスワード、次回、チェック ボックスとボタンでログのテキスト ボックス コントロールが含まれることに注意してください。 `RequiredFieldValidator`の 2 つのテキスト ボックスのコントロールです。


[![ログイン コントロールをページに追加します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**図 3**: ログイン コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


終了しました。 ポストバックが発生し、ログイン コントロールを呼び出すログイン コントロールの [ログイン] ボタンがクリックされたときに、`Membership.ValidateUser`メソッド、入力したユーザー名とパスワードを渡します。 資格情報が有効でない場合、ログイン コントロールには、ことを示すメッセージが表示されます。 、ただし、資格情報が有効な場合、ログイン コントロール、フォーム認証チケットを作成し、適切なページにユーザーをリダイレクトします。

ログイン コントロールでは、次の 4 つの要因を使って、ユーザーをリダイレクトして、正常にログインを適切なページを決定します。

- かどうか、ログイン コントロールが、ログイン ページで定義されている`loginUrl`この設定の既定値は、フォーム認証の構成の設定 `Login.aspx`
- 存在、 `ReturnUrl` querystring パラメーター
- ログイン コントロールの値[`DestinationUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- `defaultUrl`フォーム認証の構成設定に指定された値ですこの設定の既定値は。 `Default.aspx`

図 4 は、方法を示しています。 ログイン コントロールは、そのページの適切な意思決定に到着するこれら 4 つのパラメーターを使用します。


[![ログイン コントロールをページに追加します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**図 4**: ログイン コントロールをページに追加 ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


すぐをブラウザーを使用してサイトを訪問し、メンバーシップ framework の既存のユーザーとしてログインしてログイン コントロールをテストします。

ログイン コントロールのレンダリングされたインターフェイスは、柔軟な構成です。 その外観に影響を与えるプロパティの数があります。さらに、ログイン コントロールに変換できるテンプレートのレイアウトを正確に制御をユーザー インターフェイス要素です。 この手順の残りの部分は、レイアウトと外観をカスタマイズする方法を説明します。

### <a name="customizing-the-login-controls-appearance"></a>ログイン コントロールの外観のカスタマイズ

ログイン コントロールの既定のプロパティ設定のタイトルが (ログの) ユーザー インターフェイスをレンダリングする、チェック ボックスと、[ログイン] ボタン、次回ユーザー名とパスワード入力用テキスト ボックス、およびラベル コントロールが次に時間をします。 これらの要素の外観は、ログイン コントロールの多数のプロパティをすべて構成可能です。 さらに、プロパティ、または 2 つを設定して - 新しいユーザー アカウントを作成するページへのリンクなどの他のユーザー インターフェイス要素を追加できます。

では、ユーザーのログイン コントロールの外観を増やさずにしばらくご説明します。 以降、`Login.aspx`ページは既にログインを示すページの上部にあるテキスト、ログイン コントロールのタイトルは不要です。 したがって、クリア、 [ `TitleText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx)ログイン コントロールのタイトルを削除するために値。

: ユーザー名とパスワード: を通じて、2 つのテキスト ボックス コントロールの左にラベルをカスタマイズできる、 [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx)と[`PasswordLabelText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)、それぞれします。 ユーザー名を変更してみましょう。 ユーザー名を読み取れませんラベル: です。 ラベルとテキスト ボックスのスタイルが使用して構成できますが、 [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx)と[`TextBoxStyle`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)、それぞれします。

次回ログイン コントロールの次の時間チェック ボックスの Text プロパティを設定することができます[ `RememberMeText property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx)、既定のチェック状態を使用して構成できますが、 [ `RememberMeSet property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx) (既定値は False)。 さあ、設定、`RememberMeSet`既定でチェックする場合は True。 次回 チェック ボックスは、サインインできるようにするプロパティです。

ログイン コントロールには、そのユーザー インターフェイス コントロールのレイアウトを調整するための 2 つのプロパティが用意されています。 [ `TextLayout property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx)を示すかどうか、ユーザー名: とパスワード: 左側の対応するテキスト ボックス (既定)、またはその上にラベルが表示されます。 [ `Orientation property` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx)ユーザー名とパスワードの入力が垂直方向に置かれているかどうかを示します (上、他の 1 つ) または水平方向にします。 これら 2 つのプロパティには既定では、設定のままにしておくつもりが、これら 2 つのプロパティを結果として得られる効果を確認するには、その既定ではない値に設定を変更することをお勧めします。

> [!NOTE]
> 次のセクションで、ログイン コントロールのレイアウトの構成を見てみましょうレイアウト コントロールのユーザー インターフェイス要素の正確なレイアウトを定義するテンプレートを使用します。


ログイン コントロールのプロパティの設定を設定してラップ、 [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx)と[`CreateUserUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx)にまだ登録されていないか。 アカウントを作成してください。 および`~/Membership/CreatingUserAccounts.aspx`、それぞれします。 これで作成した、ページを指すログイン コントロールのインターフェイスにハイパーリンクを追加、 <a id="SKM6"> </a>[前のチュートリアル](creating-user-accounts-cs.md)です。 ログイン コントロールの[ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx)と[`HelpPageUrl`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx)と[ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx)と[ `PasswordRecoveryUrl` のプロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx)ヘルプ ページとパスワードの回復ページへのリンクを表示、同じ方法で動作します。

これらのプロパティの変更を加えたら、ログイン コントロールの宣言型マークアップおよび外観のようになります図 5 に示すです。


[![ログイン コントロールのプロパティの値の外観を決定します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**図 5**: の外観をディクテーション、ログイン コントロールのプロパティの値 ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>ログイン コントロールのレイアウトを構成します。

ログイン Web コントロールの既定のユーザー インターフェイスは、HTML では、インターフェイスによってレイアウト`<table>`です。 しかし、場合、表示される出力をより細かく制御が必要でしょうか。 置換することができます、`<table>`一連の`<div>`タグ。 または、アプリケーションが認証の追加の資格情報に必要な場合ですか。 多くの財務 web サイトには、だけでなく、ユーザー名とパスワードがも個人識別番号 (PIN)、または他の識別情報を指定するユーザーのインスタンスが必要です。 任意の原因が考えられます、可能であればログイン コントロールを明示的に定義できます、インターフェイスの宣言型マークアップ、テンプレートに変換します。

追加の資格情報を収集するログイン コントロールを更新するために 2 つの作業を行う必要があります。

1. 追加の資格情報を収集する Web コントロールを含めるログイン コントロールのインターフェイスを更新します。
2. ユーザー名とパスワードが有効であり、追加の資格情報が有効ですぎる場合にのみ、ユーザーが認証されるように、ログイン コントロールの内部認証ロジックをオーバーライドします。

最初のタスクを実行するには、ログイン コントロールをテンプレートに変換し、必要な Web コントロールを追加する必要があります。 2 番目のタスクとは、コントロールのイベント ハンドラーを作成することで、ログイン コントロールの認証ロジックが優先されることが[`Authenticate`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)です。

そのユーザー名、パスワード、および電子メール アドレスのユーザー入力を要求し、指定した電子メール アドレスには、ファイルの電子メール アドレスが一致する場合にのみユーザーを認証できるようにしましょうログイン コントロールを更新します。 まず、ログイン コントロールのインターフェイスをテンプレートに変換する必要があります。 ログイン コントロールのスマート タグから変換テンプレート オプションを選択します。


[![ログイン コントロールをテンプレートに変換します。](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**図 6**: ログイン コントロールをテンプレートに変換 ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> ログイン コントロールの事前 template バージョンに戻すには、するには、コントロールのスマート タグからリセット リンクをクリックします。


テンプレートからログイン コントロールへの変換を追加、 `LayoutTemplate` HTML 要素と、ユーザー インターフェイスを定義する Web コントロールのコントロールの宣言型マークアップにします。 図 7 に示すコントロールをテンプレートに変換するいくつかのプロパティから削除 [プロパティ] ウィンドウなど`TitleText`、`CreateUserUrl`などのように、ので、テンプレートを使用するときに、これらのプロパティ値は無視されます。


[![少ないプロパティが使用可能なときにログイン コントロールによっては、テンプレートに変換されます。](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**図 7**: より少ないプロパティが使用可能なときにログイン コントロールによっては、テンプレートに変換されます ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


内の HTML マークアップ、`LayoutTemplate`必要に応じて変更することがあります。 同様に、自由に、テンプレートには新しい Web コントロールを追加します。 ただし、これはそのログインのコア Web コントロール テンプレートのままになり、割り当てられた保持`ID`値。 具体的には、削除またはしない名前を変更、`UserName`または`Password`テキスト ボックス、 `RememberMe`  チェック ボックス、 `LoginButton`  ボタン、`FailureText`ラベル、または`RequiredFieldValidator`コントロール。

ユーザーの電子メール アドレスを収集するには、テンプレートにテキスト ボックスを追加する必要があります。 テーブルの行の間で、次の宣言型マークアップを追加 (`<tr>`) を格納している、 `Password`  テキスト ボックスおよび次にメール アドレスを保持するテーブルの行の時間をチェックします。

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

追加した後、 `Email`  ボックスに、ブラウザーでページを参照してください。 図 8 に示すログイン コントロールのユーザー インターフェイスが 3 つ目のテキスト ボックスが追加されます。


[![ログイン コントロールには、ユーザーの電子メール アドレス ボックスに、ようになりましたが含まれています](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**図 8**: ログイン コントロールには、ユーザーの電子メール アドレス ボックスに、ようになりましたが含まれています ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


この時点では、ログイン コントロールを使用している、`Membership.ValidateUser`指定された資格情報を検証するメソッド。 値が同じように、入力、 `Email`  テキスト ボックスに、ユーザーがログインできるかどうかに影響がありません。 手順 3 では、資格情報ファイル上の電子メール アドレスが一致する指定した電子メール アドレス、ユーザー名とパスワードが有効な場合有効と見なされるのみようにログイン コントロールの認証ロジックをオーバーライドする方法について取り上げます。

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>手順 3: ログイン コントロールの認証ロジックを変更します。

彼女の訪問者が指定した場合資格情報と、[ログイン] ボタン、ポストバックに陥りますクリックして、ログインは、その認証ワークフローの進捗状況を制御します。 ワークフローを開始させて、 [ `LoggingIn`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx)です。 このイベントに関連付けられているすべてのイベント ハンドラーを設定して操作のログを取り消しても、`e.Cancel`プロパティを`true`です。

発生させることによって、ワークフローが進行状況操作のログは取り消されません場合、 [ `Authenticate`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx)です。 イベント ハンドラーがある場合、`Authenticate`イベントがいない、または指定された資格情報が有効かどうかを判断します。 ログイン コントロールを使用してイベント ハンドラーが指定されていない場合、`Membership.ValidateUser`資格情報の有効性を確認します。

かどうかは、指定された資格情報が有効で、フォーム認証チケットを作成すると、 [ `LoggedIn`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx)が発生し、ユーザーは、適切なページにリダイレクトします。 ただし、資格情報と考えられる、無効な場合は、次に、 [ `LoginError`イベント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx)が発生し、ユーザーを通知するには、自分の資格情報が有効でないというメッセージが表示されます。 既定では、失敗した場合、ログイン コントロールだけを設定、`FailureText`コントロールのテキスト プロパティ (ログインの試行に失敗しましたエラー メッセージにラベルを付けます。 もう一度やり直してください)。 ただし場合、ログイン コントロールの[`FailureAction`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx)に設定されている`RedirectToLoginPage`、ログイン管理の問題、 `Response.Redirect` querystring パラメーターを追加するログイン ページに`loginfailure=1`(それが原因で、ログインコントロールをエラー メッセージが表示されます)。

図 9 には、認証ワークフローのフロー チャートが用意されています。


[![ログイン コントロールの認証ワークフロー](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**図 9**:「ログイン コントロールの認証ワークフロー ([フルサイズ イメージを表示するに、をクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> かわからない場合は使用すると、`FailureAction`の`RedirectToLogin`オプションのページで、次のシナリオを検討してください。 今すぐマイクロソフト`Site.master`マスター ページは現在、こんにちは、よそ者に表示されるテキスト、匿名ユーザーによってアクセスしたときに、左の列を持つが、ログイン コントロールでそのテキストを置き換えますたいことを想像してください。 匿名のユーザーのログイン ページに直接アクセスを必要とするのではなく、サイトのすべてのページからログインをこのようにするとします。 ただし、ユーザーがマスター ページで表示されたログイン コントロールを使用してログインできなかった場合有意義にログイン ページにリダイレクト (`Login.aspx`) 可能性が高いそのページには、その他については、リンク、およびリンクを作成するなどの他のヘルプが含まれているため、新しいアカウントまたはマスター ページに追加されませんでした - 失われた、パスワードを取得します。


### <a name="creating-theauthenticateevent-handler"></a>作成する、`Authenticate`イベント ハンドラー

を、カスタム認証ロジックで接続するために必要がありますログイン コントロールのイベント ハンドラーを作成する`Authenticate`イベント。 イベント ハンドラーの作成、`Authenticate`イベントが次のイベント ハンドラーの定義を生成します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

ご覧のように、`Authenticate`イベント ハンドラーは型のオブジェクトに渡されます[ `AuthenticateEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) 2 番目の入力パラメーターとして。 `AuthenticateEventArgs`クラスには、という名前のブール型プロパティが含まれています。`Authenticated`指定された資格情報が有効かどうかを指定するために使用されます。 タスクは、次に、コードを記述するここか、指定された資格情報が有効かどうかを決定してを設定するには、`e.Authenticate`プロパティに応じて。

### <a name="determining-and-validating-the-supplied-credentials"></a>特定し、指定された資格情報を検証します。

ログイン コントロールの使用[ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx)と[`Password`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx)をユーザーが入力したユーザー名とパスワードの資格情報を確認します。 その他の Web コントロールに入力された値を決定するために (など、 `Email`  ボックスに、前の手順で追加されました)、使用して*`LoginControlID`* `.FindControl`("*`controlID`*") を取得するプログラムへの参照 Web コントロール テンプレートを持つ`ID`プロパティと等しい *`controlID`*です。 たとえばへの参照を取得するため、 `Email`  ボックスに、次のコードを使用します。

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

ユーザーの資格情報を検証するためには、次の 2 つの作業を行う必要があります。

1. 指定したユーザー名とパスワードが有効であることを確認します。
2. 入力した電子メール アドレスにログインしようとしてください。 ユーザーのファイル上の電子メール アドレスと一致することを確認してください。

使用して単に最初のチェックを実行する、`Membership.ValidateUser`メソッド手順 1. で説明したようにします。 2 番目のチェックでは、テキスト ボックス コントロールに入力した電子メール アドレスにこれと比較できるように、ユーザーの電子メール アドレスを決定する必要があります。 特定のユーザーに関する情報を取得するを使用して、`Membership`クラスの[`GetUser`メソッド](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx)です。

`GetUser`メソッドはオーバー ロードの数。 すべてのパラメーターに渡すことがなく使用する場合は、現在ログインしているユーザーに関する情報を返します。 特定のユーザーに関する情報を取得するには、呼び出す`GetUser`ユーザー名を渡します。 どちらの方法でも、`GetUser`を返します、 [ `MembershipUser`オブジェクト](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)などのプロパティを持つ`UserName`、 `Email`、 `IsApproved`、`IsOnline`のようにします。

次のコードでは、これら 2 つのチェックを実装します。 両方に合格した場合、し`e.Authenticate`に設定されている`true`、それが割り当てられます`false`です。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

配置でこのコードには、適切なユーザー名、パスワード、および電子メール アドレスを入力する、有効なユーザーとしてログインを試みます。 再び、実行が、今回は意図的に正しくない電子メール アドレスを使用して (図 10 参照)。 最後に、やって存在しないユーザー名を使用して 3 番目の時間。 最初のケースにする必要がありますが正常にログオンする、サイトが最後の 2 つの場合、ログイン コントロールの無効な資格情報メッセージが表示する必要があります。


[![Tito は正しくない電子メール アドレスを指定するときにログインできません。](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**図 10**: Tito ことはできませんログのときに指定する無効な電子メール アドレス ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> 手順 1 で、どのように、メンバーシップ Framework 処理無効なログイン試行セクションで説明したようにときに、`Membership.ValidateUser`の無効なログインの試行を追跡して特定を超える場合、ユーザーをロックアウト、メソッドが呼び出され、無効な資格情報を渡す指定した時間枠内で無効な試行回数のしきい値。 以降、カスタム認証ロジックの呼び出し、`ValidateUser`メソッド、有効なユーザー名に正しくないパスワードには、無効なログイン試行カウンターが 1 ずつ増分されますが、このカウンターは、ユーザー名とパスワードが、有効な場合は加算されませんが、電子メール アドレスが正しくありません。 確率は、ハッカーは、ユーザー名とパスワードを知ってが、ブルート フォース手法を使用して、ユーザーの電子メール アドレスを決定する必要がある可能性は高くありませんので、この動作は、適切なです。


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>手順 4: ログイン コントロールの無効な資格情報のメッセージの向上

ユーザーが無効な資格情報でログオンしようとすると、ログイン コントロールには、ログインの試行が成功したことを示すメッセージが表示されます。 具体的には、コントロールがで指定されたメッセージが表示されます、 [ `FailureText`プロパティ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx)は、既定値は、ログインの試行に失敗しました。 やり直してください。

なぜユーザーの資格情報がありますいない有効な多くの理由があることに注意してください。

- ユーザー名がない可能性があります。
- ユーザー名が存在する、パスワードが無効です。
- ユーザー名とパスワードが有効では、ユーザーがまだ承認されていません。
- ユーザー名とパスワードが有効では、ユーザーがロックアウトされている出力 (最も可能性の高い特定の期間内の無効なログイン試行の数を超過したため)

カスタム認証ロジックを使用する場合、その他の理由にする可能性があります。 たとえば、コードにご説明したとおりステップ 3 で、ユーザー名とパスワードが有効である可能性がありますが、電子メール アドレスが正しくない可能性があります。

なぜ、資格情報が有効でないに関係なくログイン コントロールには、同じエラー メッセージが表示されます。 このフィードバックが不足しているがアカウントを持つはまだ承認されていないか、ユーザーがロックアウトされているユーザーの混乱する可能性です。作業の若干、ただしがあってもログイン コントロールをより適切なメッセージを表示します。

ユーザーがログイン コントロールが無効な資格情報でログインしようとしたときにその`LoginError`イベント。 このイベントのイベント ハンドラーを作成してください、次のコードを追加します。

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

上記のコードを開始するには、ログイン コントロールの`FailureText`プロパティを既定値に (ログインの試行に失敗しました。 もう一度やり直してください)。 のかどうか、指定されたユーザー名は既存のユーザー アカウントにマップを確認します。 そのためを参照して、結果として得られる場合`MembershipUser`オブジェクトの`IsLockedOut`と`IsApproved`プロパティを確認して、アカウントがロックアウトされていること、またはまだ承認されていないかどうか。 どちらの場合、`FailureText`プロパティは、対応する値を更新します。

このコードをテストするには、意図的にしようと、既存のユーザーとしてログインが、正しくないパスワードを使用します。 10 分のタイム フレーム内の行で 5 回の操作を行うし、アカウントはロックアウトされます。図 11 は、次回ログインの試行は常に失敗 (でもパスワードを使用して、正しい) が、ここでは表示わかりやすいとして、アカウントが無効なログインの回数が多すぎるためロックされてです。 アカウント ロック解除されたメッセージを管理者に問い合わせてください。


[![Tito が無効なログインを何度も実行され、ロックアウトされています。](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**図 11**: Tito 実行も無効なログインを何度もおよびがされてロックアウト ([フルサイズのイメージを表示するをクリックして](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>まとめ

以前このチュートリアルで、ログイン ページは、ハード コーディングされた一連のユーザー名とパスワードの組み合わせに対して指定された資格情報を検証します。 このチュートリアルでは、メンバーシップ フレームワークに対して資格情報を検証するページを更新します。 手順 1. でお見てを使用して、`Membership.ValidateUser`メソッド プログラムでします。 手順 2. で置き換え、手動で作成したユーザー インターフェイスとコード ログイン コントロールを使用します。

ログイン コントロールは、標準的なログイン ユーザー インターフェイスをレンダリングし、自動的にメンバーシップ フレームワークに対してユーザーの資格情報を検証します。 さらに、有効な資格情報が発生した場合、ログイン コントロール サインイン ユーザー フォーム認証を使用します。 つまり、完全に機能のログイン ユーザー エクスペリエンスは、ページ、ない余分な宣言型マークアップまたは必要なコードにログイン コントロールをドラッグするだけで使用できます。 詳細は、ログイン コントロールは、詳細にカスタマイズして、レンダリングされたユーザー インターフェイスと認証のロジックに制御のため、問題ありません。

この時点で当社の web サイトへの訪問者が新しいユーザー アカウントとログを作成、サイトの、認証されたユーザーに基づいてページへのアクセス制限に検索するまだです。 現時点では、認証済みまたは匿名、すべてのユーザーについては、サイト、任意のページを表示できます。 と共に、ユーザー単位のユーザーごとに、サイトのページへのアクセスを制御するには、機能を持つは、ユーザーによって異なります。 特定のページで必要があります。 次のチュートリアルのアクセスと、ログイン ユーザーに基づく ページで機能を制限する方法を見ます。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [ロックされており、承認されていないユーザーにカスタム メッセージを表示します。](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [ASP.NET 2.0 を確認するメンバーシップ、ロール、およびプロファイル](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [ASP.NET ログイン ページを作成する方法](https://msdn.microsoft.com/library/ms178331.aspx)
- [ログイン コントロールのテクニカル ドキュメント](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [ログイン コントロールを使用します。](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>作成者について

Scott Mitchell、複数の受け取りますブックの作成者と 4GuysFromRolla.com の創設者は、Microsoft の Web テクノロジと 1998 年取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書 *[Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*です。 Scott に到達できる[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)または彼のブログでを介して[ http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルの潜在顧客レビュー担当者は、Teresa マーフィーおよび Michael Olivero がいました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com)です。

> [!div class="step-by-step"]
> [前へ](creating-user-accounts-cs.md)
> [次へ](user-based-authorization-cs.md)
