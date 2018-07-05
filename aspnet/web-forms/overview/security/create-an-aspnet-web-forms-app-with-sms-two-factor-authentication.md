---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: 作成する ASP.NET Web フォーム アプリケーションを SMS 2 要素認証 (c#) |Microsoft Docs
author: Erikre
description: このチュートリアルでは、2 要素認証を使用して ASP.NET Web フォーム アプリをビルドする方法を示します。 このチュートリアルは、Cr の「チュートリアルを補完するものがしています.
ms.author: aspnetcontent
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 16045b116ca5c797e7840f2ee5944e5f2c6282eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803550"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>作成する ASP.NET Web フォーム アプリケーションを SMS 2 要素認証 (c#)
====================
によって[Erik Reitan](https://github.com/Erikre)

[電子メールと SMS 2 要素認証での ASP.NET Web フォーム アプリをダウンロードします。](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> このチュートリアルでは、2 要素認証を使用して ASP.NET Web フォーム アプリをビルドする方法を示します。 このチュートリアルは、「チュートリアルを補完するものが[ユーザー登録をセキュリティで保護された ASP.NET Web フォーム アプリケーションを作成、電子メール確認、パスワード リセットを](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)します。 さらに、このチュートリアルは、Rick Anderson に基づいてが[MVC チュートリアル](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)します。


## <a name="introduction"></a>はじめに

このチュートリアルでは、Visual Studio を使用した 2 要素認証をサポートする ASP.NET Web フォーム アプリケーションを作成するための手順に従って操作します。 2 要素認証は、追加のユーザーの認証手順です。 この余分な手順は、サインイン時に一意の個人識別番号 (PIN) を生成します。 PIN は一般ユーザーに電子メールまたは SMS メッセージとして送信されます。 アプリのユーザーに署名するときにし、暗証番号 (pin) と、追加の認証手段として入力します。

### <a name="tutorial-tasks-and-information"></a>チュートリアルのタスクと情報。

- [ASP.NET Web フォーム アプリを作成します。](#createWebForms)
- [SMS と 2 要素認証をセットアップします。](#SMS)
- [登録されたユーザーの 2 要素認証を有効にします。](#use2FA)
- [その他のリソース](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>ASP.NET Web フォーム アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降もします。 また、作成する必要がありますには、 [Twilio](https://www.twilio.com/try-twilio)アカウントを次に示すようにします。

> [!NOTE]
> 重要: インストールする必要があります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。


1. 新しいプロジェクトを作成 (**ファイル** - &gt; **新しいプロジェクト**) を選択し、 **ASP.NET Web アプリケーション**.NET Framework と共にテンプレートバージョン 4.5.2 から、**新しいプロジェクト** ダイアログ ボックス。
2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレート。 既定の認証としてのままに**個々 のユーザー アカウント**します。 をクリックし、 **OK**新しいプロジェクトを作成します。  
    ![新しい ASP.NET プロジェクト ダイアログ ボックス](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. プロジェクトの Secure Sockets Layer (SSL) を有効にします。 使用可能な手順に従って、**プロジェクトの SSL を有効にする**のセクション、 [Web フォームのチュートリアル シリーズの概要](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)します。
4. Visual Studio で開く、**パッケージ マネージャー コンソール**(**ツール** - &gt; **NuGet パッケージ マネージャー**  - &gt;**パッケージ マネージャー コンソール**)、次のコマンドを入力します。  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>SMS と 2 要素認証をセットアップします。

このチュートリアルは、Twilio, を使用しますが、SMS プロバイダーを使用することができます。

1. 作成、 [Twilio](https://www.twilio.com/try-twilio)アカウント。
2. **ダッシュ ボード**、Twilio アカウントの コピーのタブ、**アカウント SID**と**認証トークン。** それらをアプリに後で追加します。
3. **番号** タブで、コピー、Twilio**電話番号**もします。
4. Twilio**アカウント SID**、**認証トークン**と**電話番号**アプリに使用します。 簡潔でこれらの値を格納する、 *web.config*ファイル。 Azure にデプロイするときに、安全での値を格納できる、 **appSettings**セクションで、web サイト タブを構成します。また、電話番号を追加する場合はのみ、番号を使用します。   
   SendGrid の資格情報を追加することも注目してください。 SendGrid では、電子メールの通知サービスです。 SendGrid を有効にする方法の詳細については、「チュートリアルの ' フックを SendGrid' セクションを参照してください[ユーザー登録をセキュリティで保護された ASP.NET Web フォーム アプリを作成確認、パスワード リセットの電子メールを送信します。](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データは store ことはありません。 この例に、アカウントと資格情報を格納します。、 **appSettings**のセクション、 *Web.config*ファイル。 Azure では安全に保管するこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portal でのタブ。 関連情報については、Rick Anderson のトピックを参照してください。 [ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](https://go.microsoft.com/fwlink/?LinkId=513141)します。
5. 構成、`SmsService`クラス、*アプリ\_Start\IdentityConfig.cs*次のことでファイルの変更が黄色で強調表示されています。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. 次の追加`using`ステートメントの先頭に、 *IdentityConfig.cs*ファイル。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. 更新プログラム、 *Account/Manage.aspx*黄色で強調表示されている行を削除することによってファイル。  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. `Page_Load`のハンドラー、 *Manage.aspx.cs*分離コード、コードとして表示されるように、黄色で強調表示の行に依存してコメント解除します。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. 分離コードで*アカウント*/*TwoFactorAuthenticationSignIn.aspx.cs*、更新、`Page_Load`黄色で強調表示されている次のコードを追加することでハンドラー。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   上記のコードを変更するには、認証のオプションを含む「プロバイダー」DropDownList は最初の値にリセットされません。 これによって、ユーザーが正常にだけでなく、最初の認証に使用するすべてのオプションを選択します。
10. **ソリューション エクスプ ローラー**、右クリック*Default.aspx*選択と**スタート ページとして設定**します。
11. アプリをテストして、アプリをビルド最初 (**Ctrl**+**shift キーを押し**+**B**) し、アプリを実行し、(**f5 キーを押して**) と選択するか**登録**を新しいユーザー アカウントを作成または選択**ログイン**ユーザー アカウントが既に登録されている場合。
12. 表示のナビゲーション バーでユーザー ID (電子メール アドレス) にいったんログインする (ユーザー) として、クリックして、**アカウントの管理**ページ (Manage.aspx)。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. クリックして**追加**横に**電話番号**上、**アカウントの管理**ページ。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. 場所 (ユーザー) としてたい SMS メッセージ (テキスト メッセージ) を受信し、をクリックする電話番号を追加、**送信**ボタンをクリックします。   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    この時点で、アプリはから資格情報を使用、 *Web.config* Twilio にお問い合わせください。 SMS メッセージ (テキスト メッセージ) は、ユーザー アカウントに関連付けられている電話に送信されます。 Twilio ダッシュ ボードを表示することによって、Twilio メッセージが送信されたことを確認することができます。
15. 数秒後に、ユーザー アカウントに関連付けられている電話に確認コードを含むテキスト メッセージが表示されます。 確認コードとキーを押して入力**送信**します。  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>登録済みユーザーの 2 要素認証を有効にします。

この時点では、アプリに対して 2 要素認証を有効にします。 2 要素認証を使用するユーザーの設定が、UI を使用して、単に変更できます。 

1. アプリのユーザーとして有効にできます 2 要素認証、特定のアカウントのユーザー ID (電子メールのエイリアス) をクリックすると表示のナビゲーション バーで、**アカウントの管理**ページ。クリックし、**を有効にする**アカウントの 2 要素認証を有効にするリンク。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. ログオフし、再度ログインします。 電子メールを有効にした場合は、SMS または 2 要素認証用電子メールのいずれかを選択できます。 電子メールを有効にしていない場合は、「チュートリアルを参照してください[ユーザー登録、確認電子メール パスワードをリセットして、セキュリティで保護された ASP.NET Web フォーム アプリを作成する](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)します。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. 2 要素認証ページが表示されます (SMS または電子メール) からのコードを入力することができます。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 クリックすると、**このブラウザーを記憶する** チェック ボックスの除外 2 要素認証を使用して、チェック ボックスに、ブラウザーとデバイスを使用する場合にログインする必要がなくなります。 悪意のあるユーザーは、デバイスへのアクセスを得ることはできません、限り、2 要素認証を有効にすると、クリックすると、**このブラウザーを記憶する**は便利な 1 つのステップ パスワード アクセスを提供、厳密なを維持しながら信頼されていないデバイスからのすべてのアクセスの 2 要素認証の保護。 これは、定期的に使用するプライベートあらゆるデバイスで行うことができます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity で SMS と電子メールを利用して 2 要素認証を行う](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [リンクを ASP.NET Identity 推奨リソース](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [メンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリを Azure web サイトにデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web フォームのチュートリアル シリーズでは、OAuth 2.0 プロバイダーの追加します。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web フォーム チュートリアル シリーズで、プロジェクトの SSL を有効にします。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Facebook でアプリを作成し、アプリ プロジェクトを接続します。](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
