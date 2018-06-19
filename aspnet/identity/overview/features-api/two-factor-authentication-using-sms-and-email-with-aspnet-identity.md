---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: SMS と電子メール ASP.NET の Id を使用した 2 要素認証 |Microsoft ドキュメント
author: HaoK
description: このチュートリアルでは、SMS や電子メールを使用した 2 要素認証 (2 fa) を設定する方法を示します。 この記事は、Rick Anderson によって書き込まれました ( @RickAndMSFT ) あたり、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876138"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>SMS と電子メール ASP.NET の Id を使用した 2 要素認証
====================
によって[ハオ最後にトリム](https://github.com/HaoK)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルでは、SMS や電子メールを使用した 2 要素認証 (2 fa) を設定する方法を示します。
> 
> この記事は、Rick Anderson によって書き込まれました ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、最後に、ハオ トリムと Suhas Joshi です。 NuGet サンプルは、最後にハオ トリムによって、主に書き込まれました。


このトピックは、次について説明します。

- [Id サンプルのビルド](#build)
- [2 要素認証のための SMS をセットアップします。](#SMS)
- [2 要素認証を有効にします。](#enable2)
- [2 要素認証プロバイダーを登録する方法](#reg)
- [ソーシャルとローカルのログイン アカウントを結合します。](#combine)
- [ブルート フォース攻撃からのアカウントのロックアウト](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Id サンプルのビルド

このセクションでは、させるサンプルをダウンロードする NuGet を使用します。 インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 Visual Studio のインストール[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。

> [!NOTE]
> 警告: Visual Studio をインストールする必要があります[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)このチュートリアルを完了します。


1. 新しい***空***ASP.NET Web プロジェクトです。
2. パッケージ マネージャー コンソールで、次を入力して、次のコマンド。  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   このチュートリアルで使用されます[SendGrid](http://sendgrid.com/)電子メールを送信して[Twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms テキストのです。 `Identity.Samples`パッケージは操作するコードをインストールします。
3. 設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。
4. *省略可能な*: の指示に従って、my[電子メール確認チュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md)に SendGrid をフックするため、アプリを実行して、電子メール アカウントを登録します。
5. * 省略可能: * サンプルからデモ電子メール リンク確認コードを削除 (、`ViewBag.Link`アカウント コント ローラー内のコード。 参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。
6. <em>省略可能: * 削除する、`ViewBag.Status`コードと、*Views\Account\VerifyCode.cshtml 管理およびアカウント コント ローラーから</em>と<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor ビュー。 または、保持することができます、`ViewBag.Status`をフックして電子メールと SMS メッセージを送信しなくてもローカルでのこのアプリの動作をテストを表示します。

> [!NOTE]
> 警告: このサンプルでは、セキュリティ設定のいずれかを変更すると、運用アプリが行われた変更を明示的に呼び出すのセキュリティ監査を受ける必要があります。


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>2 要素認証のための SMS をセットアップします。

このチュートリアルは、Twilio または ASPSMS のいずれかを使用するための手順を説明しますが、他の SMS プロバイダーを使用することができます。

1. **SMS プロバイダーとユーザー アカウントの作成**  
  
   作成、 [Twilio](https://www.twilio.com/try-twilio)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)アカウント。
2. **その他のパッケージをインストールするか、サービス参照の追加**  
  
   Twilio:  
   パッケージ マネージャー コンソールで、次のコマンドを入力します。  
    `Install-Package Twilio`  
  
   ASPSMS:  
   次のサービス参照を追加する必要があります。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   アドレス:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   名前空間:  
    `ASPSMSX2`
3. **SMS プロバイダーのユーザーの資格情報を見つけ出し**  
  
   Twilio:  
   **ダッシュ ボード**コピー、Twilio アカウントのタブ、**アカウント SID**と**認証トークン**です。  
  
   ASPSMS:  
   アカウントの設定からに移動**ユーザー キー**自己定義と共にコピー**パスワード**です。  
  
   変数に後でこれらの値を格納は`SMSAccountIdentification`と`SMSAccountPassword`です。
4. **SenderID を指定する/発信元**  
  
   Twilio:  
   **番号** タブで、Twilio 電話番号をコピーします。  
  
   ASPSMS:  
   内で、**発信者のロックを解除** メニューの 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。  
  
   変数に、この値を格納おは後で`SMSAccountFrom`です。
5. **アプリに SMS プロバイダーの資格情報を転送します。**  
  
   資格情報と差出人の電話番号を使用できるように、アプリ。

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データはストアことはありません。 アカウントと資格情報は、サンプルをシンプルにする上記のコードに追加されます。 Jon Atten を参照してください[ASP.NET MVC: ソース管理のプライベート設定の出力を保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)です。
6. **SMS プロバイダーへのデータ転送の実装**  
  
   構成、`SmsService`クラス内で、*アプリ\_Start\IdentityConfig.cs*ファイル。  
  
   いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. アプリを実行し、以前に登録したアカウントでログインします。
8. ユーザー ID とアクティブ化するをクリックして、`Index`でアクション メソッド`Manage`コント ローラー。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. [追加] をクリックします。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. 数秒で、確認コードと共にテキスト メッセージが表示されます。 これを入力し、キーを押して**送信**です。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. 電話番号が追加された、管理ビューを示しています。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>コードを調べます

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index`でアクション メソッド`Manage`コント ローラーは、前のアクションに基づいて、ステータス メッセージを設定し、ローカル パスワードを変更またはローカル アカウントを追加するリンクを提供します。 `Index`メソッドも、状態を表示したり、2 fa 電話番号、外部ログイン、有効な場合、2 fa (後述) このブラウザーの 2 fa メソッドに注意してください。 タイトル バーに、ユーザー ID (電子メール) をクリックすると、メッセージに合格しなかったです。 クリックすると、**電話番号: 削除**パスをリンク`Message=RemovePhoneSuccess`クエリ文字列として。  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力 ダイアログ ボックスが表示されます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

クリックすると、**確認コードを送信**ボタンは、HTTP POST に電話番号をポスト`AddPhoneNumber`アクション メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync`メソッドは、これは、SMS メッセージで設定がセキュリティ トークンを生成します。 トークンが文字列として送信される場合は、SMS サービスが構成されている、&quot;セキュリティ コードが&lt;トークン&gt;&quot;です。 `SmsService.SendAsync`するメソッドは、非同期的にし、アプリにリダイレクト、 `VerifyPhoneNumber` (次のダイアログ ボックスが表示されます) をアクション メソッド、確認コードを入力することができます。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

コードが HTTP POST にポストされたコードを入力して送信 をクリックすると、`VerifyPhoneNumber`アクション メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync`メソッドは、ポストされたセキュリティ コードをチェックします。 コードが正しい場合は、電話番号が追加、`PhoneNumber`のフィールド、`AspNetUsers`テーブル。 その呼び出しが成功した場合、`SignInAsync`メソッドが呼び出されます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent`パラメーターは、複数の要求で認証セッションが保存されるかどうかを設定します。

新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更するときに、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。 注意してください、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。 セキュリティ クッキーに保存されていない、`AspNetUsers`テーブル (または Identity db では、他の場所から)。 Cookie のセキュリティ トークンは、自己署名を使用して[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。

Cookie ミドルウェアでは、各要求の cookie を確認します。 `SecurityStampValidator`メソッドで、`Startup`クラスの DB のヒット数およびセキュリティ スタンプを定期的にチェックで指定されたとおり、`validateInterval`です。 これはセキュリティ プロファイルを変更する場合を除きます (この例では) で 30 分おきのみ発生します。 データベースへのトリップを最小限に抑える、30 分間隔が選択されました。

`SignInAsync`メソッドは、セキュリティ プロファイルに変更されるときに呼び出される必要があります。 データベースの更新プログラムは、セキュリティ プロファイルが変更されたときに、`SecurityStamp`フィールド、およびを呼び出さず、`SignInAsync`メソッドでログオンしたまま、*のみ*OWIN パイプラインを次回までデータベースにアクセスする (、 `validateInterval`). これをテストするには変更することによって、`SignInAsync`メソッドをすぐに返すし、cookie を設定`validateInterval`を 5 秒に 30 分からのプロパティ。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

上記のコード変更では、セキュリティ プロファイルを変更することができます (の状態を変更することによって、 **2 つの要素の有効な**) 5 秒以内にログアウトするは、ときに、`SecurityStampValidator.OnValidateIdentity`メソッドは失敗します。 戻り値の行を削除して、`SignInAsync`メソッド、別のセキュリティ プロファイルが変更を行い、する記録されません。`SignInAsync`メソッドは、新しいセキュリティ クッキーを生成します。

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>2 要素認証を有効にします。

サンプル アプリでは、UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。 2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
有効にする 2 fa をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) ログアウトし、再びログインします。 電子メールを有効にした場合 (を参照してください [前のチュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa 用の電子メールを選択できます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) コードの確認 ページが表示されます (SMS または電子メール) からコードを入力できます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、そのコンピューターとブラウザーを使用してログオンする必要があるからです。 2 fa を有効にしてをクリックすると、**このブラウザーに覚えて**が提供されます 2 fa の強力な保護でコンピューターへのアクセスがあるない限り、アカウントにアクセスしようとしている悪意のあるユーザーからです。 これは、定期的に使用するすべてのプライベート コンピューターで行うことができます。 設定して**このブラウザーに覚えて**を定期的に使用しないコンピューターから 2 fa の追加のセキュリティを取得する、および自分のコンピューターで 2 fa を通過する必要がないで利便性を取得します。 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>2 要素認証プロバイダーを登録する方法

新しい MVC プロジェクトを作成するときに、 *IdentityConfig.cs*ファイルには、2 要素認証プロバイダーを登録する次のコードが含まれています。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>2 fa の電話番号を追加します。

`AddPhoneNumber`でアクション メソッド、`Manage`コント ローラーは、セキュリティ トークンを生成する番号、電話に送信が指定したとします。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

トークンを送信した後にリダイレクトする、`VerifyPhoneNumber`アクション メソッド、2 fa に SMS を登録するコードを入力することができます。 電話番号を確認するまで、SMS 2 fa は使用されません。

## <a name="enabling-2fa"></a>2 fa を有効にします。

`EnableTFA`アクション メソッドが 2 fa を有効にします。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

注、`SignInAsync`有効にする 2 fa が、セキュリティ プロファイルに変更のために呼び出す必要があります。 2 fa を有効にすると、ユーザーはログインに、2 fa を使用する必要は (SMS やサンプル電子メール) を登録した 2 fa アプローチを使用しています。

QR コード ジェネレーターなどの追加の 2 fa プロバイダーを追加したり、自分が所有するを記述することができます (を参照してください[ASP.NET Id の Google Authenticator を使用して](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。

> [!NOTE]
> 使用して 2 fa コードが生成された[ワンタイム パスワードのアルゴリズムの時間に基づく](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)とコードが 6 分以内に有効です。 コードを入力する 6 分以上を実行する場合は、無効なコード エラー メッセージが表示されます。


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>ソーシャルとローカルのログイン アカウントを結合します。

電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。 次の順序で&quot; RickAndMSFT@gmail.com &quot;が最初に、ローカル ログインとして作成しますが、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加することができます。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

をクリックして、**管理**リンクします。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

サービス内の別のログへのリンクをクリックし、アプリケーション要求を許可します。 2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。 ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。

次の図では、Tom は、ソーシャル ログイン、(からわかるようにする、**外部ログイン: 1**ページに表示されます)。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

クリックすると**パスワードを選択して**同じアカウントに関連付けられているを使用すると、ローカルのログを追加します。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>ブルート フォース攻撃からのアカウントのロックアウト

辞書攻撃からアプリケーションにアカウントを保護するには、ユーザーのロックアウトを有効にします。 次のコードで、`ApplicationUserManager Create`メソッドはロックアウトされ、構成します。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

上記のコードでは、2 要素認証のみのロックアウトを使用できます。 変更することでのログインのロックアウトを有効にすることができます、`shouldLockout`を true に、`Login`アカウント コント ローラーのメソッド、お勧めを有効にしないでのログインのロックを可能になったため、アカウントの影響を受け[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ログイン攻撃があります。 サンプル コードでロックアウトは無効で、管理者アカウントの作成、`ApplicationDbInitializer Seed`メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>検証された電子メール アカウントのユーザーが必要

次のコードには、ユーザー ログインすることが検証済みの電子メール アカウントを持つことが必要です。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>2 fa 要件の SignInManager をチェックする方法

ローカル ログインとソーシャル ログイン 2 fa が有効かどうかを確認します。 2 fa が有効になっている場合、`SignInManager`ログオン メソッドを返します`SignInStatus.RequiresVerification`、ユーザーにリダイレクトして、`SendCode`アクション メソッド、シーケンス内のログを完了するコードを入力する必要があります。 ユーザーは RememberMe 場合、が設定されている場合、ユーザーのローカル cookie で、`SignInManager`戻ります`SignInStatus.Success`2 fa を通過する必要がないとします。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

次のコードは、`SendCode`アクション メソッド。 A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)ユーザーに対して有効になっているすべての 2 fa メソッドで作成されます。 [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)に渡される、 [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)ヘルパーに渡し、ユーザーが 2 fa アプローチ (通常の電子メールと SMS) を選択します。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

ユーザーがポスト 2 fa アプローチと、`HTTP POST SendCode`アクション メソッドが呼び出されると、 `SignInManager` 2 fa コードと、ユーザーがリダイレクトされている場合に送信、`VerifyCode`アクション メソッドでログを完了するコードを入力することができます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2 fa ロックアウト

ログイン パスワード試行の失敗では、アカウントのロックアウトを設定できますが、その手法では、現在のログインの影響を受け[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトされます。 アカウントのロックアウトを 2 fa でのみ使用することをお勧めします。 ときに、`ApplicationUserManager`が作成されると、サンプル コードは 2 fa ロックアウトを設定および`MaxFailedAccessAttemptsBeforeLockout`を 5 つです。 (ローカル アカウントまたはアカウントのソーシャル) 経由ユーザーをログに記録し 2 fa に失敗した場合はそれぞれが格納されているし、最大試行回数に達すると、ユーザーはロックアウトを 5 分間 (で時間のロックアウトを設定することができます`DefaultAccountLockoutTimeSpan`)。

<a id="addRes"></a>

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Id のリソースをお勧め](../getting-started/aspnet-identity-recommended-resources.md)Identity ブログ、ビデオ、チュートリアルおよび優れたの完全な一覧は、リンクします。
- [Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。
- [ASP.NET MVC と Id 2.0: 基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。
- [アカウントの確認と ASP.NET の Id とパスワードの回復](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity 入門](../getting-started/introduction-to-aspnet-identity.md)
- [Announcing ASP.NET Identity 2.0.0 の RTM](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi でします。
- [アカウントの検証と 2 要素認証を設定する ASP.NET Identity 2.0:](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten でします。
