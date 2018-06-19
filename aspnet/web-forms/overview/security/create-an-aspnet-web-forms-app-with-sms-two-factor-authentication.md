---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: ASP.NET Web 作成フォーム アプリで SMS 2 要素認証 (c#) |Microsoft ドキュメント
author: Erikre
description: このチュートリアルでは、2 要素認証で ASP.NET Web フォーム アプリケーションをビルドする方法を示します。 このチュートリアルは、「Cr チュートリアルを補完するように設計されました.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2014
ms.topic: article
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 6c040fd3e0592b8cfd230dcd85ed3293f0a22ba7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887426"
---
<a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>ASP.NET Web 作成フォーム アプリで SMS 2 要素認証 (c#)
====================
によって[Erik Reitan](https://github.com/Erikre)

[電子メールと SMS 2 要素認証を使って ASP.NET Web フォーム アプリをダウンロードします。](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> このチュートリアルでは、2 要素認証で ASP.NET Web フォーム アプリケーションをビルドする方法を示します。 このチュートリアルは、「チュートリアルを補完するために設計されました[ユーザー登録してセキュリティで保護された ASP.NET Web フォーム アプリを作成電子メールの確認とパスワードのリセット](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)です。 さらに、このチュートリアルは、Rick Anderson に基づきますが[MVC チュートリアル](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)です。


## <a name="introduction"></a>はじめに

このチュートリアルに従って、Visual Studio を使用する 2 要素認証をサポートする ASP.NET Web フォーム アプリケーションを作成するために必要な手順を実行します。 2 要素認証は、追加のユーザー認証手順です。 この余分な手順は、サインイン時に、一意の個人識別番号 (PIN) を生成します。 PIN は一般に電子メールまたは SMS メッセージとしてユーザーに送信されます。 アプリのユーザーから PIN を入力、追加の認証手段としてサインインするとき。

### <a name="tutorial-tasks-and-information"></a>チュートリアルのタスクと情報。

- [ASP.NET Web フォーム アプリを作成します。](#createWebForms)
- [SMS と 2 要素認証をセットアップします。](#SMS)
- [登録されたユーザーの 2 要素認証を有効にします。](#use2FA)
- [その他のリソース](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>ASP.NET Web フォーム アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降同様にします。 また、必要がありますを作成する、 [Twilio](https://www.twilio.com/try-twilio)アカウント、以下に示すようです。

> [!NOTE]
> 重要: インストールする必要があります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。


1. 新しいプロジェクトを作成 (**ファイル** - &gt; **新しいプロジェクト**) を選択し、 **ASP.NET Web アプリケーション**.NET Framework と一緒にテンプレートバージョン 4.5.2、**新しいプロジェクト** ダイアログ ボックス。
2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレート。 既定の認証としてのままにして**個々 のユーザー アカウント**です。 [] をクリックして**OK**新しいプロジェクトを作成します。  
    ![新しい ASP.NET プロジェクト ダイアログ ボックス](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. プロジェクトの Secure Sockets Layer (SSL) を有効にします。 使用可能な手順に従って、**プロジェクトの SSL を有効にする**のセクション、 [Web フォーム チュートリアル シリーズの概要](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)です。
4. Visual Studio で開く、 **Package Manager Console** (**ツール** - &gt; **NuGet パッケージ マネージャー**  - &gt;**Package Manager Console**)、次のコマンドを入力します。  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>SMS と 2 要素認証をセットアップします。

このチュートリアルは、Twilio を使用しますが、すべての SMS プロバイダーを使用することができます。

1. 作成、 [Twilio](https://www.twilio.com/try-twilio)アカウント。
2. **ダッシュ ボード**コピー、Twilio アカウントのタブ、**アカウント SID**と**認証トークンです。** それらをアプリに後で追加します。
3. **番号** タブで、コピー、Twilio**電話番号**もします。
4. Twilio を行う**アカウント SID**、**認証トークン**と**電話番号**アプリに使用します。 これらの値を簡単に格納する、 *web.config*ファイル。 Azure にデプロイするときにで安全の値を格納できる、 **appSettings**セクションの web サイトの構成 タブ。また、電話番号を追加する場合は、番号を使用のみ。   
   SendGrid の資格情報を追加することもできますに注意してください。 SendGrid は、電子メール通知サービスです。 SendGrid を有効にする方法の詳細については、「チュートリアルの ' フックを SendGrid' セクションを参照してください[確認とパスワード リセットを電子メールでおユーザー登録してセキュリティで保護された ASP.NET Web フォーム アプリを作成します。](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データはストアことはありません。 この例に、アカウントと資格情報を格納します。、 **appSettings**のセクションで、 *Web.config*ファイル。 Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。 関連情報については、Rick Anderson のトピックを参照してください。 [ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](https://go.microsoft.com/fwlink/?LinkId=513141)です。
5. 構成、`SmsService`クラス内で、*アプリ\_Start\IdentityConfig.cs*させて、次のファイルの変更が黄色で強調されています。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. 次の追加`using`ステートメントの先頭に、 *IdentityConfig.cs*ファイル。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. 更新プログラム、 *Account/Manage.aspx*黄色で強調表示されている行を削除することによってファイル。  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. `Page_Load`のハンドラー、 *Manage.aspx.cs*分離コード、コードとして表示されるように、黄色で強調表示の行に依存してコメントを解除します。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. 分離コード*アカウント*/*TwoFactorAuthenticationSignIn.aspx.cs*、更新、`Page_Load`黄色で強調表示されている次のコードを追加することによってハンドラー。 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   上記のコード変更をすることで認証のオプションを含む「プロバイダー」DropDownList は最初の値にリセットされません。 これによって、ユーザーが正常に認証、だけでなく、最初に使用するすべてのオプションを選択します。
10. **ソリューション エクスプ ローラー**を右クリックして*Default.aspx*選択**スタート ページとして設定**です。
11. アプリをテストすることによって最初にアプリをビルドします (**Ctrl**+**shift キーを押し**+**B**) し、アプリを実行 (**f5 キーを押して**) と選択するか**登録**を新しいユーザー アカウントを作成または選択**ログイン**ユーザー アカウントは既に登録されている場合。
12. (ユーザー) としてログイン、をクリック (電子メール アドレスなど) で、ユーザー ID を表示するには、ナビゲーション バーで、**アカウントの管理**ページ (Manage.aspx)。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. をクリックして**追加** の横に**電話番号**上、**アカウントの管理**ページ。  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. ここで (ユーザー) としてたい SMS メッセージ (メッセージのテキスト) を受信し、をクリックする電話番号を追加、**送信**ボタンをクリックします。   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    この時点で、アプリはから資格情報を使用、 *Web.config* Twilio に接続します。 SMS メッセージ (テキスト メッセージ) は、ユーザー アカウントに関連付けられている電話に送信されます。 Twilio ダッシュ ボードを表示することによって、Twilio メッセージが送信されたことを確認することができます。
15. 数秒後に、ユーザー アカウントに関連付けられている電話に確認コードを含むテキスト メッセージが表示されます。 確認コードとキーを押して入力**送信**です。  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>登録されたユーザーの 2 要素認証を有効にします。

この時点では、アプリの 2 要素認証を有効にします。 2 要素認証を使用するユーザー、単に UI を使用してその設定を変更できます。 

1. アプリのユーザーとしてを有効にする 2 要素認証、特定のアカウントのユーザー ID (電子メールのエイリアス) をクリックすると表示のナビゲーション バーで、**アカウントの管理**ページ。次に、をクリックして、**を有効にする**アカウントの 2 要素認証を有効にするリンクです。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. ログオフし、再びログインします。 電子メールを有効にする場合は、SMS または 2 要素認証用電子メールのいずれかを選択できます。 電子メールを有効にしていない場合は、「チュートリアルを参照して[セキュリティで保護された ASP.NET Web フォーム アプリケーションを作成するユーザー登録確認の電子メールとパスワードのリセット使用](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)です。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. 2 要素認証ページが表示されます (SMS または電子メール) からコードを入力できます。![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 要素認証を使用して、チェック ボックスに、ブラウザーとデバイスを使用する場合にログインする必要があるからです。 悪意のあるユーザーは、デバイスにアクセスできない、限り、2 要素認証を有効にしてをクリックすると、**このブラウザーに覚えて**がアクセスを提供便利な 1 つのステップ パスワード、strong を保ちながら信頼されていないデバイスからのすべてのアクセスの 2 要素認証を保護します。 これは、定期的に使用する任意のプライベート デバイスで行うことができます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity で SMS と電子メールを利用して 2 要素認証を行う](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [ASP.NET Id へのリンクは、リソースを推奨](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Azure の web サイトにメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web フォームのチュートリアル シリーズの OAuth 2.0 プロバイダーの追加します。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [ASP.NET Web フォーム チュートリアル シリーズのプロジェクトの SSL を有効にします。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Facebook でアプリを作成し、プロジェクトに、アプリを接続します。](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
