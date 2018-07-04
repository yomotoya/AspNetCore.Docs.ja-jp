---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: セキュリティで保護された ASP.NET Web フォーム アプリを作成するユーザー登録、電子メール確認、パスワードのリセット (c#) |Microsoft Docs
author: Erikre
description: このチュートリアルでは、ユーザーの登録、確認の電子メールと ASP.NET Identity のメンバーを使用してパスワード リセットによる ASP.NET Web フォーム アプリを構築する方法を紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 79755282f5f146ac6969c3adfeb285610db530ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391279"
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>セキュリティで保護された ASP.NET Web フォーム アプリを作成するユーザー登録、電子メール確認、パスワードのリセット (c#)
====================
によって[Erik Reitan](https://github.com/Erikre)

> このチュートリアルでは、ユーザーの登録、確認の電子メールと、ASP.NET Identity メンバーシップ システムを使用してパスワード リセットによる ASP.NET Web フォーム アプリをビルドする方法を示します。 このチュートリアルは、Rick Anderson のに基づいてが[MVC チュートリアル](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)します。


## <a name="introduction"></a>はじめに

このチュートリアルでは、Visual Studio および ASP.NET 4.5 をユーザーの登録をセキュリティで保護された Web フォーム アプリケーションを作成、電子メール確認、パスワード リセットを使用する ASP.NET Web フォーム アプリケーションを作成するための手順に従って操作します。

### <a name="tutorial-tasks-and-information"></a>チュートリアルのタスクと情報。

- [作成する ASP.NET Web フォーム アプリケーション](#createWebForms)
- [SendGrid をフックします。](#SG)
- [ログインする前に確認の電子メールが必要です。](#require)
- [パスワードの回復とリセット](#reset)
- [電子メールの確認リンクを再送信します。](#rsend)
- [アプリのトラブルシューティング](#dbg)
- [その他のリソース](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>ASP.NET Web フォーム アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降もします。

> [!NOTE]
> 警告: をインストールする必要がある[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。


1. 新しいプロジェクトを作成 (**ファイル** - &gt; **新しいプロジェクト**) を選択し、 **ASP.NET Web アプリケーション**テンプレートと、最新の .NET Frameworkバージョンから、**新しいプロジェクト** ダイアログ ボックス。
2. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレート。 既定の認証としてのままに**個々 のユーザー アカウント**します。 Azure でアプリをホストする場合は、ままにして、**クラウドでホスト**チェック ボックスをオンします。   
 をクリックし、 **OK**新しいプロジェクトを作成します。  
    ![新しい ASP.NET プロジェクト ダイアログ ボックス](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. プロジェクトの Secure Sockets Layer (SSL) を有効にします。 使用可能な手順に従って、**プロジェクトの SSL を有効にする**のセクション、 [Web フォームのチュートリアル シリーズの概要](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)します。
4. アプリを実行し、をクリックして、**登録**リンクし、新しいユーザーを登録します。 電子メールにのみ検証に基づいての時点で、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)電子メール アドレスが正しく構成されていることを確認する属性。 確認の電子メールを追加するコードを変更します。 ブラウザー ウィンドウを閉じます。
5. **サーバー エクスプ ローラー** Visual Studio の (**ビュー**  - &gt; **サーバー エクスプ ローラー**) に移動します**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開く**します。 

    次の図は、`AspNetUsers`テーブル スキーマ。

    ![AspNetUsers テーブル スキーマ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. **サーバー エクスプ ローラー**を右クリックし、 **AspNetUsers**テーブルを選択**テーブル データの表示**します。  
  
    ![AspNetUsers テーブル データ](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 この時点で登録されたユーザーの電子メールが確認されていません。
7. ユーザーの行を削除する削除を選択します をクリックします。 次の手順でこの電子メールをもう一度追加し、電子メール アドレスに確認メッセージを送信します。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装していないことを確認する新しいユーザーの登録時に電子メールを確認することをお勧め (つまりに登録していない他のユーザーの電子メールで)。 ようにしたい、ディスカッション フォーラムが`"bob@cpandl.com"`としての登録から`"joe@contoso.com"`します。 電子メールの確認を求めず`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定`"bib@cpandl.com"`認識していないと、アプリは、正しいメール アドレスがある見つからないため、パスワードの回復を使用して、彼はできません。 確認の電子メールはボットからの限られた保護のみを提供し、決定スパムから保護を提供しません。

新しいユーザーがいずれかの電子メール、SMS テキスト メッセージまたは別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐために一般的にします。 以下のセクションでは確認の電子メールを有効にし、新しく登録されたユーザーがログインするまで、自分の電子メールが確認されていることを防ぐためにコードを変更します。、 このチュートリアルでは、SendGrid 電子メール サービスを使用します。

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid をフックします。

このチュートリアルを使用して電子メール通知を追加する方法だけでは[SendGrid](http://sendgrid.com/)、SMTP とその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。

1. Visual Studio で開く、**パッケージ マネージャー コンソール**(**ツール** - &gt; **NuGet パッケージ マネージャー**  - &gt;**パッケージ マネージャー コンソール**)、次のコマンドを入力します。  
    `Install-Package SendGrid`
2. 移動して、 [Azure SendGrid のサインアップ ページ](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/)と無料の SendGrid アカウントを登録します。 無料の SendGrid アカウントに直接もサインアップできます[SendGrid のサイト](http://www.sendgrid.com)します。
3. **ソリューション エクスプ ローラー**を開く、 *IdentityConfig.cs*ファイル、*アプリ\_開始*フォルダー、に黄色で強調表示されている次のコードを追加`EmailService`を構成するクラス**SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. また、次の追加`using`ステートメントの先頭に、 *IdentityConfig.cs*ファイル。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. このサンプルをシンプルにするでのサービス アカウントの電子メールの値を格納します、`appSettings`のセクション、 *web.config*ファイル。 次の XML を黄色で強調表示を追加、 *web.config*プロジェクトのルートにあるファイル。

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データは store ことはありません。 この例に、アカウントと資格情報を格納します。、 **appSetting**のセクション、 *Web.config*ファイル。 Azure では安全に保管するこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portal でのタブ。 関連情報」というタイトルの Rick Anderson のトピックを参照してください。 [ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](https://go.microsoft.com/fwlink/?LinkId=513141)します。
6. (ユーザー名とパスワード) は、SendGrid の認証の値を正常に行えるように電子メール アプリから送信を反映するように、電子メール サービスの値を追加します。 SendGrid を指定した電子メール アドレスではなく、SendGrid アカウント名を使用することを確認します。

### <a name="enable-email-confirmation"></a>確認の電子メールを有効にします。

 確認の電子メールを有効にするには、次の手順を使用して登録コードを変更します。  
 

1. *アカウント*フォルダーを開き、 *Register.aspx.cs*分離コードと更新、`CreateUser_Click`メソッドは、次の強調表示されている変更を有効にします。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. **ソリューション エクスプ ローラー**、右クリック*Default.aspx*選択と**スタート ページとして設定**します。
3. キーを押してアプリを実行**f5 キー。** ページが表示されたら、クリックして、**登録**登録ページを表示するリンク。
4. 電子メールとパスワードを入力し、クリックして、**登録**SendGrid で電子メール メッセージを送信するボタンをクリックします。  
   自分のプロジェクトとコードの現在の状態により、ユーザーが自分のアカウントを確認していない場合でも、登録フォームを完了した後にログインします。
5. 電子メール アカウントを確認し、電子メールを確認するには、リンクをクリックします。  
   登録フォームを送信する記録されます。  
    ![サンプルの web サイトのサインイン](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>ログインする前に確認の電子メールが必要です。

電子メール アカウントを確認した後、この時点で完全にサインインする検証電子メールに含まれているリンクをクリックする必要はありません。 次のセクションには、ログインには、確認された電子メール (認証された) 新しいユーザーを必要とするコードを変更します。

1. **ソリューション エクスプ ローラー** 、Visual Studio の更新、`CreateUser_Click`内のイベント、 *Register.aspx.cs*分離のコードに含まれている、*アカウント*フォルダーで、次の強調表示された変更: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. 更新プログラム、`LogIn`メソッドで、 *Login.aspx.cs*以下の強調表示されている変更を分離コード。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>アプリケーションを実行する

 これで、ユーザーの電子メール アドレスが確認されているかどうかをチェックするコードを実装している、両方で機能を確認できます、**登録**と**ログイン**ページ。 

1. 内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。
2. アプリを実行する (**F5**) まで、メール アドレスを確認した後にユーザーとして登録できないことを確認します。
3. だけが送信された電子メールを使用して新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。  
 ログインできるようにされていないことと、確認の電子メール アカウントに必要なを確認します。
4. 電子メール アドレスのことを確認したら、アプリにログインします。

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>パスワードの回復とリセット

1. Visual Studio で、削除からコメント文字、`Forgot`メソッドで、 *Forgot.aspx.cs*分離のコードに含まれている、*アカウント*フォルダーとして、メソッドが表示されるように従います。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. 開く、 *Login.aspx*ページ。 末尾付近のマークアップを置き換える、 **loginForm**として以下の強調表示されたセクションします。 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. 開く、 *Login.aspx.cs*分離コードと次のコードから黄色で強調表示の行のコメントを解除、`Page_Load`イベント ハンドラー。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. キーを押してアプリを実行**f5 キー。** ページが表示されたら、クリックして、**ログイン**リンク。
5. をクリックして、**パスワードを忘れた場合でしょうか。** 表示へのリンク、**パスワードを忘れた場合**ページ。
6. 電子メール アドレスを入力し、をクリックして、**送信**パスワードをリセットすることができる、アドレスに電子メールを送信するボタンをクリックします。   
   電子メール アカウントを確認し、表示するリンクをクリックして、**パスワードのリセット**ページ。
7. **パスワードのリセット**ページで、電子メール、パスワード、および確認入力したパスワードを入力します。 次に、キーを押して、**リセット**ボタンをクリックします。  
   正常にパスワードをリセットすると、**パスワード変更**ページが表示されます。 これで、新しいパスワードを使用してログインできます。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>電子メールの確認リンクを再送信します。

ユーザーは、新しいローカル アカウントを作成した後は、確認リンクがログオンする前に、使用する必要が、メールで送信されます。 誤ってユーザーは、確認の電子メールを削除します。 または、電子メールが到着することはありません、確認リンクをもう一度送信される必要があります。 次のコード変更では、これを有効にする方法を示します。

1. Visual Studio で開く、 **Login.aspx.cs**分離コードと後に次のイベント ハンドラーを追加、`LogIn`イベント ハンドラー。   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. 変更、`LogIn`内のイベント ハンドラー、 *Login.aspx.cs*次のように黄色で強調表示コードを変更することで分離コード。 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. 更新プログラム、 *Login.aspx*次のように黄色で強調表示コードを追加してページ。 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. 内の任意のアカウントを削除、 **AspNetUsers**をテストする電子メールのエイリアスを含むテーブル。
5. アプリを実行する (**F5**) し、電子メール アドレスを登録します。
6. だけが送信された電子メールを使用して新しいアカウントを確認するには、前に、新しいアカウントでログインを試みます。  
   ログインできるようにされていないことと、確認の電子メール アカウントに必要なを確認します。 さらに、電子メール アカウントに今すぐ確認メッセージを再送信することができます。
7. 電子メール アドレスとパスワードを入力します。 キーを押します、**確認を再送信**ボタンをクリックします。
8. 新しく送信された電子メール メッセージに基づく電子メール アドレスのことを確認したら、アプリにログインします。

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>アプリのトラブルシューティング

資格情報を確認するリンクを含むメールが届かない: 場合

- 迷惑メールまたはスパム メール フォルダーを確認します。
- SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)します。
- SendGrid ユーザー アカウント名としての使用を特定する、 *Web.config* SendGrid アカウントの電子メール アドレスではなく値します。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [リンクを ASP.NET Identity 推奨リソース](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web フォームのチュートリアル シリーズでは、OAuth 2.0 プロバイダーの追加します。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Azure App Service へのメンバーシップ、OAuth、SQL Database を使用したセキュリティで保護された ASP.NET Web フォーム アプリをデプロイします。](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web フォーム チュートリアル シリーズで、プロジェクトの SSL を有効にします。](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
