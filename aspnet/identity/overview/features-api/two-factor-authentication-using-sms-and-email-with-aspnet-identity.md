---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: ASP.NET Identity で SMS と電子メールを使用する 2 要素認証 |Microsoft Docs
author: HaoK
description: このチュートリアルでは、SMS と電子メールを使用して 2 要素認証 (2 fa) を設定する方法を説明します。 この記事の執筆者は、Rick Anderson ( @RickAndMSFT ) あたり、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0519ae69b3d2ef129d206a936b199f781fef5bf6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399680"
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a>ASP.NET Identity で SMS と電子メールを使用する 2 要素認証
====================
によって[Hao 力](https://github.com/HaoK)、 [Pranav Rastogi](https://github.com/rustd)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Suhas Joshi](https://github.com/suhasj)

> このチュートリアルでは、SMS と電子メールを使用して 2 要素認証 (2 fa) を設定する方法を説明します。
> 
> この記事の執筆者は、Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT))、Pranav Rastogi ([@rustd](https://twitter.com/rustd))、Hao 力、および Suhas Joshi します。 NuGet のサンプルは、主に Hao 力によって記述されています。


このトピックでは、次の項目について説明します。

- [ビルド Id サンプル](#build)
- [2 要素認証のための SMS をセットアップします。](#SMS)
- [2 要素認証を有効にします。](#enable2)
- [2 要素認証プロバイダーを登録する方法](#reg)
- [ソーシャル、ローカルのログイン アカウントを組み合わせる](#combine)
- [ブルート フォース攻撃からのアカウントのロックアウト](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>ビルド Id サンプル

このセクションでは、処理を中心にサンプルをダウンロードする NuGet を使用します。 インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 Visual Studio のインストール[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。

> [!NOTE]
> 警告: Visual Studio をインストールする必要があります[2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521)このチュートリアルを完了します。


1. 新規作成***空***ASP.NET Web プロジェクト。
2. パッケージ マネージャー コンソールで、次を入力します。 次のコマンド。  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   このチュートリアルで使用します[SendGrid](http://sendgrid.com/)電子メールを送信して[Twilio](https://www.twilio.com/)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms のようにします。 `Identity.Samples`パッケージで使用されるコードをインストールします。
3. 設定、 [SSL を使用するプロジェクト](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。
4. *省略可能な*: の指示に従って、[電子メール確認チュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md)SendGrid をフックし、アプリを実行し、電子メール アカウントを登録します。
5. * 省略可能: * デモ電子メール リンク確認コード サンプルから削除 (、 `ViewBag.Link` account コント ローラー コード。 参照してください、`DisplayEmail`と`ForgotPasswordConfirmation`アクション メソッドと razor ビュー)。
6. <em>省略可能: * 削除、`ViewBag.Status`コードの管理およびアカウント コント ローラーと、*Views\Account\VerifyCode.cshtml から</em>と<em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor ビュー。 または、保持することができます、`ViewBag.Status`テスト フックして、電子メールや SMS メッセージを送信することがなくローカルでこのアプリの動作を表示します。

> [!NOTE]
> 警告: このサンプルでは、セキュリティ設定のいずれかを変更すると、運用アプリが行われた変更を明示的に呼び出すセキュリティ監査を受ける必要があります。


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>2 要素認証のための SMS をセットアップします。

このチュートリアルでは、Twilio または ASPSMS のいずれかの使用方法について説明しますが、他の SMS プロバイダーを使用することができます。

1. **SMS プロバイダーとユーザー アカウントを作成します。**  
  
   作成、 [Twilio](https://www.twilio.com/try-twilio)または[ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/)アカウント。
2. **その他のパッケージをインストールまたはサービス参照の追加**  
  
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
3. **SMS プロバイダーのユーザーの資格情報を見極める**  
  
   Twilio:  
   **ダッシュ ボード**、Twilio アカウントの コピーのタブ、**アカウント SID**と**Auth トークン**します。  
  
   ASPSMS:  
   移動しますから、アカウントの設定、**別子-Userkey**し、自己定義型と共にコピー**パスワード**します。  
  
   これらの値を変数に保存後で`SMSAccountIdentification`と`SMSAccountPassword`します。
4. **SenderID を指定する/発信元**  
  
   Twilio:  
   **番号** タブで、Twilio 電話番号をコピーします。  
  
   ASPSMS:  
   内で、**発信者のロックを解除**] メニューの [発信者の 1 つまたは複数のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。  
  
   この値を変数に後で保存`SMSAccountFrom`します。
5. **アプリへの SMS プロバイダーの資格情報の転送**  
  
   資格情報と差出人の電話番号を使用できるように、アプリ。

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データは store ことはありません。 アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。 Jon Atten を参照してください。 [ASP.NET MVC: ソース管理のプライベート設定の出力を保持](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)します。
6. **SMS プロバイダーへのデータ転送の実装**  
  
   構成、`SmsService`クラス、*アプリ\_Start\IdentityConfig.cs*ファイル。  
  
   いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. アプリを実行し、以前に登録したアカウントでログインします。
8. アクティブ化ユーザー ID を取得するには、をクリックして、`Index`内のアクション メソッド`Manage`コント ローラー。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. [追加] をクリックします。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. 数秒で確認コードを含むテキスト メッセージが表示されます。 入力してキーを押して**送信**します。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. 電話番号が追加された、管理ビューを示しています。  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>コードを確認します

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

`Index`内のアクション メソッド`Manage`コント ローラーは、前のアクションに基づいて、ステータス メッセージを設定し、ローカル パスワードを変更またはローカル アカウントを追加するリンクを提供します。 `Index`メソッドは、状態もが表示されます。 または、2 fa 電話番号、外部ログイン、2 fa を有効にし、このブラウザー (後述) の 2 fa メソッドに注意してください。 タイトル バーで、ユーザー ID (電子メール) をクリックすると、メッセージが合格しなかった。 クリックすると、**電話番号: 削除**パスをリンク`Message=RemovePhoneSuccess`クエリ文字列として。  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

`AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力するダイアログ ボックスが表示されます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

クリックすると、**確認コードを送信**ボタンは、HTTP POST に電話番号をポスト`AddPhoneNumber`アクション メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

`GenerateChangePhoneNumberTokenAsync`メソッドは、SMS メッセージで設定がセキュリティ トークンを生成します。 SMS サービスが構成されている場合、トークンは、文字列として送信&quot;セキュリティ コードが&lt;トークン&gt;&quot;します。 `SmsService.SendAsync`にメソッドが非同期的に呼び出されるに、アプリがリダイレクトされる、 `VerifyPhoneNumber` (これは、次のダイアログが表示されます) アクション メソッドでは、確認コードを入力することができます。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

コードが HTTP POST にポストされたコードを入力して送信 をクリックすると、一度`VerifyPhoneNumber`アクション メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

`ChangePhoneNumberAsync`メソッドは、ポストされたセキュリティ コードを確認します。 電話番号を追加するコードが正しい場合、`PhoneNumber`のフィールド、`AspNetUsers`テーブル。 その呼び出しが成功した場合、`SignInAsync`メソッドが呼び出されます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

`isPersistent`パラメーターは、認証セッションは複数の要求にわたって保持するかどうかを設定します。

新しいセキュリティ スタンプが生成されに格納されているセキュリティ プロファイルを変更すると、`SecurityStamp`のフィールド、 *AspNetUsers*テーブル。 メモ、`SecurityStamp`フィールドとは異なるセキュリティ クッキー。 セキュリティ クッキーに格納されていない、`AspNetUsers`テーブル (または任意の場所で Identity DB)。 使用して、セキュリティ cookie トークンが自己署名[DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx)で作成し、`UserId, SecurityStamp`と有効期限の時刻情報。

Cookie ミドルウェアは、各要求の cookie を確認します。 `SecurityStampValidator`メソッドで、`Startup`クラスが、DB をヒットして、定期的にセキュリティ スタンプをチェックに指定された、`validateInterval`します。 これは、セキュリティ プロファイルを変更しない限り (サンプル) では 30 分のみ発生します。 データベースへのトリップを最小限に抑える、30 分間隔が選択されました。

`SignInAsync`メソッドは、セキュリティ プロファイルに変更されるときに呼び出される必要があります。 データベースの更新プログラムは、セキュリティ プロファイルが変更されたときに、`SecurityStamp`フィールド、および呼び出さず、`SignInAsync`メソッドでログオンしたまま、*のみ*OWIN パイプラインを次回までデータベースにアクセスする (、 `validateInterval`). これをテストするには変更することで、`SignInAsync`直ちにを返すメソッドをおよび cookie を設定`validateInterval`を 5 秒に 30 分間のプロパティ。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

上記のコード変更では、セキュリティ プロファイルを変更することができます (の状態を変更することで、 **2 つの要素の有効な**) と 5 秒以内にログアウトするはときに、`SecurityStampValidator.OnValidateIdentity`メソッドは失敗します。 戻り値の行を削除して、`SignInAsync`メソッド、もう 1 つのセキュリティ プロファイルが変更を行い、する記録されません。`SignInAsync`メソッドは新しいセキュリティ クッキーを生成します。

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>2 要素認証を有効にします。

サンプル アプリでは、UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。 2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
有効にする 2 fa をクリックします。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) ログアウトし、再度ログインします。 電子メールを有効にした場合 (を参照してください、[前のチュートリアル](account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa の電子メールを選択することができます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) (SMS または電子メール) から、コードを入力できますが、コードの確認ページが表示されます。![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) クリックすると、**このブラウザーを記憶する** チェック ボックスの除外 2 fa を使用して、そのコンピューターとブラウザーを使用してログオンする必要がなくなります。 2 fa を有効にして、クリックすると、**このブラウザーを記憶する**くれます 2 fa の強力な保護、コンピューターへのアクセスがあるない限り、アカウントにアクセスしようとした悪意のあるユーザーから。 これは、定期的に使用してプライベートのマシンで行うことができます。 設定して**このブラウザーを記憶する**を定期的に使用しないコンピューターから 2 fa の追加のセキュリティを取得する、独自のコンピューターに 2 fa を経由する必要があるの利便性を取得します。 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>2 要素認証プロバイダーを登録する方法

新しい MVC プロジェクトを作成するときに、 *IdentityConfig.cs*ファイルには、2 要素認証プロバイダーを登録するには、次のコードが含まれています。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>2 fa の電話番号を追加します。

`AddPhoneNumber`内のアクション メソッド、`Manage`コント ローラーは、セキュリティ トークンを生成および指定した番号に電話に送信します。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

リダイレクト、トークンを送信した後、`VerifyPhoneNumber`アクション メソッド、2 fa の SMS を登録するコードを入力することができます。 SMS 2 fa は、電話番号を確認するまでは使用されません。

## <a name="enabling-2fa"></a>2 fa を有効にします。

`EnableTFA`アクション メソッドは、2 fa を使用できます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

注、`SignInAsync`有効にする 2 fa は、セキュリティ プロファイルに変更するために呼び出す必要があります。 2 fa を有効にすると、ユーザーは、ログインには、2 fa を使用する必要が (SMS と電子メールのサンプル) を登録した 2 fa のアプローチを使用してあります。

QR コード ジェネレーターなど、追加の 2 fa プロバイダーを追加するか、自分が所有するを記述できます (を参照してください[ASP.NET Identity での Google Authenticator を使用して](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/))。

> [!NOTE]
> 使用して、2 fa コードが生成された[ワンタイム パスワード アルゴリズムの時間ベース](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm)と 6 分間のコードは無効です。 6 分以上のコードを入力する場合、無効なコード エラー メッセージが表示されます。


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>ソーシャル、ローカルのログイン アカウントを組み合わせる

電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。 次の順序で&quot; RickAndMSFT@gmail.com &quot;が最初に、ローカル ログインとして作成は、まずソーシャル ログとしてアカウントを作成し、ローカル ログインを追加することができます。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

をクリックして、**管理**リンク。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

サービス内の別のログへのリンクをクリックし、アプリの要求をそのまま使用します。 2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。 ユーザー、ソーシャル ログイン認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合は、ローカル アカウントを追加することができます。

次の図では、Tom は、ソーシャル ログイン (から確認できますが、**外部ログイン: 1**ページに表示)。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

クリックすると**パスワードの入力**同じアカウントに関連付けられたでローカルのログを追加することができます。

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>ブルート フォース攻撃からのアカウントのロックアウト

辞書攻撃のアプリにアカウントを保護するには、ユーザーのロックアウトを有効にします。 次のコードで、`ApplicationUserManager Create`メソッドのロックアウトを構成します。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

上記のコードでは、2 要素認証のみのロックアウトを使用できます。 変更することでログインをロックアウトを有効にすることができます、`shouldLockout`を true に、 `Login` account コント ローラーのメソッド、お勧めしますを有効にしないでログインのロックを可能になったため、アカウントに影響を受けやすい[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ログイン攻撃です。 作成した管理者アカウントのサンプル コードでロックアウトが無効に、`ApplicationDbInitializer Seed`メソッド。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>検証済みの電子メール アカウントを持っているユーザーを必要とします。

次のコードでは、ユーザーにログインできるようにする前に、検証済みの電子メール アカウントを持つことが必要です。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>SignInManager が 2 fa の要件を確認する方法

ローカル ログインとソーシャル ログイン 2 fa が有効になっているかどうかにチェックします。 2 fa が有効になっている場合、 `SignInManager` logon メソッドを返します`SignInStatus.RequiresVerification`とユーザーにリダイレクトされます、`SendCode`シーケンスのログを実行するコードを入力する必要が、アクション メソッド。 RememberMe 場合は、ユーザーが設定されている場合、ユーザーのローカル cookie で、`SignInManager`戻ります`SignInStatus.Success`2 fa を経由する必要がないとします。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

次のコードは、`SendCode`アクション メソッド。 A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)は、ユーザーに有効なすべての 2 fa 方式で作成されます。 [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx)に渡される、 [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx)ヘルパーで、2 fa のアプローチ (通常は電子メールと SMS) を選択できます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

ユーザーは、2 fa のアプローチを投稿すると、`HTTP POST SendCode`アクション メソッドが呼び出されると、`SignInManager`送信に 2 fa のコードと、ユーザーがリダイレクト、`VerifyCode`アクション メソッドが、ログインを実行するコードを入力することができます。

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>2 fa のロックアウト

ログイン パスワードの試行の失敗では、アカウントのロックアウトを設定できますが、そのアプローチにより、ログインを受けやすい[DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack)ロックアウトします。 2 fa でのみアカウントのロックアウトを使用することをお勧めします。 ときに、`ApplicationUserManager`はサンプル コードは、作成されると、2 fa のロックアウトを設定および`MaxFailedAccessAttemptsBeforeLockout`を 5 つです。 (ローカル アカウントまたはソーシャル アカウント) を使って、ユーザーがログインし、2 fa に失敗した場合はそれぞれが格納されているし、ユーザーが 5 分間ロックアウトしている最大試行回数に達すると、(ロックアウトの時間を設定する`DefaultAccountLockoutTimeSpan`)。

<a id="addRes"></a>

## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity 推奨リソース](../getting-started/aspnet-identity-recommended-resources.md)Identity ブログ、ビデオ、チュートリアル、および優れたの完全な一覧は、リンクします。
- [Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)プロファイル情報をユーザー テーブルに追加する方法についても説明します。
- [ASP.NET MVC と Id 2.0: 基本を理解する](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx)John Atten でします。
- [アカウントの確認と ASP.NET Identity によるパスワードの回復](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Identity 入門](../getting-started/introduction-to-aspnet-identity.md)
- [ASP.NET Identity 2.0.0 の RTM の発表](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)Pranav Rastogi が。
- [アカウントの検証と 2 要素認証を設定する ASP.NET Identity 2.0:](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten でします。
