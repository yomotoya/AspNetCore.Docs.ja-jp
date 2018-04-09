---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: 電子メールの確認とパスワードのリセット (c#) でユーザー登録、セキュリティで保護された ASP.NET Web フォーム アプリを作成 |Microsoft ドキュメント
author: Erikre
description: このチュートリアルでは、ユーザー登録、確認の電子メール、ASP.NET Identity のメンバーを使用してパスワード リセットと ASP.NET Web フォーム アプリケーションをビルドする方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1dc7ace69473b45432fd942b9cf1ba32332cb707
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>電子メールの確認とパスワードのリセット (c#) でユーザー登録、セキュリティで保護された ASP.NET Web フォーム アプリを作成します。
====================
によって[Erik Reitan](https://github.com/Erikre)

> このチュートリアルでは、ユーザー登録、確認の電子メール、Asp.net メンバーシップ システムを使用してパスワード リセットと ASP.NET Web フォーム アプリケーションをビルドする方法を示します。 このチュートリアルは、Rick Anderson のに基づいてが[MVC チュートリアル](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)です。


## <a name="introduction"></a>はじめに

このチュートリアルに従って、ユーザー登録してセキュリティで保護された Web フォーム アプリを作成、電子メールの確認とパスワードのリセットを Visual Studio と ASP.NET 4.5 を使用した ASP.NET Web フォーム アプリケーションを作成するために必要な手順を実行します。

### <a name="tutorial-tasks-and-information"></a>チュートリアルのタスクと情報。

- [作成する ASP.NET Web フォーム アプリケーション](#createWebForms)
- [SendGrid をフックします。](#SG)
- [ログインする前に確認の電子メールが必要](#require)
- [パスワードの回復とリセット](#reset)
- [再送信する電子メールの確認のリンク](#rsend)
- [アプリのトラブルシューティング](#dbg)
- [その他のリソース](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>ASP.NET Web フォーム アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降同様にします。

> [!NOTE]
> 警告: インストールする必要あります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。


1. 新しいプロジェクトを作成 (**ファイル** - &gt; **新しいプロジェクト**) を選択し、 **ASP.NET Web アプリケーション**テンプレートと、最新の .NET Frameworkバージョン、**新しいプロジェクト** ダイアログ ボックス。
2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレート。 既定の認証としてのままにして**個々 のユーザー アカウント**です。 Azure でアプリケーションをホストする場合は、ままにして、**クラウド内のホスト**チェック ボックスをオンします。   
 [] をクリックして**OK**新しいプロジェクトを作成します。  
    ![新しい ASP.NET プロジェクト ダイアログ ボックス](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. プロジェクトの Secure Sockets Layer (SSL) を有効にします。 使用可能な手順に従って、**プロジェクトの SSL を有効にする**のセクション、 [Web フォーム チュートリアル シリーズの概要](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)です。
4. アプリを実行する をクリックして、**登録**リンクし、新しいユーザーを登録します。 この時点では、検証、電子メールにのみ基づきます、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)電子メール アドレスが整形式であることを確認する属性。 確認の電子メールを追加するコードを変更します。 ブラウザー ウィンドウを閉じます。
5. **サーバー エクスプ ローラー** Visual Studio の (**ビュー**  - &gt; **サーバー エクスプ ローラー**) に移動**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開き**です。 

    次の図は、`AspNetUsers`テーブルのスキーマ。

    ![AspNetUsers テーブル スキーマ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. **サーバー エクスプ ローラー**を右クリックし、 **AspNetUsers**テーブルを選択して**テーブル データの表示**です。  
  
    ![AspNetUsers テーブルのデータ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 この時点で登録されたユーザーの電子メールが確認されていません。
7. ユーザーをクリック delete を削除し、行をクリックします。 次の手順でこの電子メールをもう一度追加して、電子メール アドレスに確認メッセージを送信します。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装するしていないことを確認する新しいユーザーの登録中に、電子メールを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。 しないようにする、ディスカッション フォーラムするいたと仮定`"bob@cpandl.com"`としての登録から`"joe@contoso.com"`です。 電子メールの確認なし`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定します`"bib@cpandl.com"`いなかったが検出されました。 これは、と彼は、アプリは、正しいメール アドレスが見つからないため、パスワードの回復を使用できなくします。 確認の電子メールは、bot から制限の保護のみを提供し、決定スパム攻撃者から保護を提供しません。

一般にする新しいユーザーがいずれかの電子メール、SMS テキスト メッセージ、または別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐ。 以下のセクションでは確認の電子メールを有効にして新たに登録されたユーザーが自分の電子メールが確認されるまでログ記録するを防ぐためにコードを変更します。、 このチュートリアルでは、電子メール サービス SendGrid を使用します。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid をフックします。

このチュートリアルはのみを使用して電子メール通知を追加する方法を示しますが[SendGrid](http://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。

1. Visual Studio で開く、 **Package Manager Console** (**ツール** - &gt; **NuGet パッケージ マネージャー**  - &gt;**Package Manager Console**)、次のコマンドを入力します。  
    `Install-Package SendGrid`
2. 移動して、 [Azure SendGrid のサインアップ ページ](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)SendGrid アカウントの登録は無料とします。 無料の SendGrid アカウント上で直接もサインアップできます[SendGrid のサイト](http://www.sendgrid.com)です。
3. **ソリューション エクスプ ローラー**を開く、 *IdentityConfig.cs*ファイルで、*アプリ\_開始*フォルダー、に黄色で強調表示されている次のコードを追加`EmailService`を構成するクラス**SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. また、次の追加`using`ステートメントの先頭に、 *IdentityConfig.cs*ファイル。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. 電子メール サービス アカウントの値をこのサンプルをシンプルにするには、保存する、`appSettings`のセクションで、 *web.config*ファイル。 次の XML に黄色で強調表示を追加、 *web.config*プロジェクトのルートにあるファイル。

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データはストアことはありません。 この例に、アカウントと資格情報を格納します。、 **appSetting**のセクションで、 *Web.config*ファイル。 Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。 関連情報については、Rick Anderson のトピックを参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](https://go.microsoft.com/fwlink/?LinkId=513141)です。
6. (ユーザー名とパスワード)、SendGrid 認証値正常に実行できるように電子メール アプリから、送信を反映するように、電子メール サービスの値を追加します。 SendGrid を入力した電子メール アドレスではなく、SendGrid アカウント名を使用することを確認します。

### <a name="enable-email-confirmation"></a>確認の電子メールを有効にします。

 確認の電子メールを有効にするには、次の手順を使用して登録コードを変更します。  
 

1. *アカウント*フォルダーを開き、 *Register.aspx.cs*分離コードと更新、`CreateUser_Click`メソッドを次の強調表示された変更を有効にします。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. **ソリューション エクスプ ローラー**を右クリックして*Default.aspx*選択**スタート ページとして設定**です。
3. キーを押して、アプリの実行**f5 キーを押します。** ページが表示されたら、クリックして、**登録**登録ページを表示するリンクです。
4. 電子メールとパスワードを入力し、クリックして、**登録**SendGrid を使用して電子メール メッセージを送信するボタンをクリックします。  
   プロジェクトとコードの現在の状態は、ユーザーが自分のアカウントを確認していない場合でも、登録フォームを完了するログインを許可します。
5. 電子メール アカウントを確認し、電子メールを確認するリンクをクリックします。  
   登録フォームを送信すると後ログ記録します。  
    ![サンプル web サイトのサインイン](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>ログインする前に確認の電子メールが必要

電子メール アカウントを確認した後、この時点で内にある完全署名の検証電子メールに含まれているリンクをクリックする必要はありません。 次のセクションでは、新しいユーザーを認証するには記録前に確認された電子メールがある () を必要とするコードを変更します。

1. **ソリューション エクスプ ローラー** 、Visual Studio の更新、`CreateUser_Click`内のイベント、 *Register.aspx.cs*に含まれている分離コード、*アカウント*フォルダーが、次には、変更が強調表示されます。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. 更新プログラム、`LogIn`メソッドで、 *Login.aspx.cs*次の強調表示された変更と分離コード。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>アプリケーションを実行する

 これで、ユーザーの電子メール アドレスが確認されているかどうかをチェックするコードを実装している両方の機能を確認できます、**登録**と**ログイン**ページ。 

1. 内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。
2. アプリを実行する (**f5 キーを押して**) し、電子メール アドレスを確認するまでユーザーとして登録できないことを確認します。
3. だけに送信された電子メールを使用して、新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。  
 いないことにログインできなくなります、確認の電子メール アカウントに必要なが表示されます。
4. 電子メール アドレスのことを確認したら、アプリにログインします。

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>パスワードの回復とリセット

1. Visual Studio で、削除からコメント文字、`Forgot`メソッドで、 *Forgot.aspx.cs*に含まれている分離コード、*アカウント*フォルダーとして、メソッドが表示されるように依存しています。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. 開く、 *Login.aspx*ページ。 末尾付近のマークアップを置き換える、 **loginForm**下強調表示されているセクションします。 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. 開く、 *Login.aspx.cs*分離コードと、次のコード行から黄色で強調表示のコメントを解除、`Page_Load`イベントのハンドラー。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. キーを押して、アプリの実行**f5 キーを押します。** ページが表示されたら、クリックして、**ログイン**リンクします。
5. クリックして、**パスワードを忘れた場合ですか?**表示へのリンク、**パスワードを忘れた場合**ページ。
6. 電子メール アドレスを入力し、をクリックして、**送信**電子メール アドレスに送信する、パスワードをリセットできるボタンをクリックします。   
   電子メール アカウントを確認し、表示するリンクをクリックして、**パスワードのリセット**ページ。
7. **パスワードのリセット** ページで、電子メール、パスワード、および確認入力したパスワードを入力します。 次に、キーを押して、**リセット**ボタンをクリックします。  
   パスワードを正常にリセットすると、**パスワード変更**ページが表示されます。 これで、新しいパスワードでログインできます。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>再送信する電子メールの確認のリンク

ユーザーは、新しいローカル アカウントを作成した後が確認のリンクを使用してログオンする前にする必要が電子メールで送信します。 誤ってユーザーが、確認メールを削除または電子メールが到着することはありません、確認のリンクを再送信する必要があります。 次のコード変更では、これを有効にする方法を示します。

1. Visual Studio で開く、 **Login.aspx.cs**分離コードと後に次のイベント ハンドラーを追加、`LogIn`イベントのハンドラー。   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. 変更、`LogIn`内のイベント ハンドラー、 *Login.aspx.cs*次のように、黄色で強調表示コードを変更することによって分離コード。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. 更新プログラム、 *Login.aspx*ページで、次のように、黄色で強調表示のコードを追加します。 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. 内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。
5. アプリを実行する (**f5 キーを押して**) と、電子メール アドレスを登録します。
6. だけに送信された電子メールを使用して、新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。  
   いないことにログインできなくなります、確認の電子メール アカウントに必要なが表示されます。 さらに、電子メール アカウントに今すぐ確認メッセージを再送信することができます。
7. 電子メール アドレスとパスワードを入力し、キーを押します、**確認を再送信**ボタンをクリックします。
8. 新しく送信した電子メール メッセージに基づく電子メール アドレスのことを確認したら、アプリにログインします。

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>アプリのトラブルシューティング

場合は、資格情報を確認へのリンクを含む電子メールを受信しません。

- 迷惑メールまたは迷惑メール フォルダーをチェックします。
- SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)です。
- として、SendGrid のユーザーのアカウント名を使用することを確認して、 *Web.config* SendGrid アカウントの電子メール アドレスではなく値します。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Id へのリンクは、リソースを推奨](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web フォームのチュートリアル シリーズの OAuth 2.0 プロバイダーの追加します。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Azure App Service のメンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET Web フォーム アプリケーションを展開します。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web フォーム チュートリアル シリーズのプロジェクトの SSL を有効にします。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
