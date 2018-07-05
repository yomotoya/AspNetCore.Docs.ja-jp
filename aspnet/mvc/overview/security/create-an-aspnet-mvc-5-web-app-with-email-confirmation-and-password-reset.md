---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: 確認とパスワードのリセット (c#) を電子メールでは、ログをセキュリティで保護された ASP.NET MVC 5 web アプリを作成 |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、確認の電子メールと、ASP.NET Identity メンバーシップ システムを使用してパスワード リセットによる ASP.NET MVC 5 web アプリをビルドする方法を示します。 . Ca
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 56c1a5c414fdcece8d827d1187144b4948d8eb93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387044"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>確認とパスワードのリセット (c#) を電子メールでは、ログをセキュリティで保護された ASP.NET MVC 5 web アプリを作成します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、確認の電子メールと、ASP.NET Identity メンバーシップ システムを使用してパスワード リセットによる ASP.NET MVC 5 web アプリをビルドする方法を示します。 完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。 ダウンロードには、確認の電子メールと SMS を電子メールまたは SMS プロバイダーをセットアップすることがなくテストできるデバッグ ヘルパーが含まれています。
> 
> このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>ASP.NET MVC アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。

> [!NOTE]
> 警告: をインストールする必要がある[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームには ASP.NET Identity もサポートしていますので、web フォーム アプリで同じ手順を実行できます。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. 既定の認証としてのままに**個々 のユーザー アカウント**します。 Azure でアプリをホストする場合は、チェック ボックスをオンのままにします。 チュートリアルの後半では、Azure を展開します。 できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。
3. 設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。
4. アプリを実行し、をクリックして、**登録**リンクし、ユーザーを登録します。 この時点では、電子メールの検証のみ、 [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
5. サーバー エクスプ ローラーに移動します。**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開く**します。

    次の図は、`AspNetUsers`スキーマ。

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. 右クリックして、 **AspNetUsers**テーブルを選択**テーブル データの表示**します。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 この時点で、電子メールが確認されていません。
7. 行をクリックし、[削除] を選択します。 次の手順でこの電子メールをもう一度追加し、確認の電子メールを送信します。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装していないことを確認する新規ユーザー登録の電子メール アドレスを確認することをお勧め (つまりに登録していない他のユーザーの電子メールで)。 ようにしたい、ディスカッション フォーラムが`"bob@example.com"`としての登録から`"joe@contoso.com"`します。 電子メールの確認を求めず`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定`"bib@example.com"`気付いていなかったとパスワードの回復を使用して、アプリは、正しいメール アドレスがある見つからないため、彼はできません。 確認の電子メールがボットから制限の保護のみを提供し、決定スパムから保護を提供しません、登録に使用できる多くの作業用電子メールの別名が。

新しいユーザーが電子メール、SMS テキスト メッセージまたは別のメカニズムによって確認されて前に、web サイトにデータを投稿するを防ぐために一般的にします。 <a id="build"></a>以下のセクションでは確認の電子メールを有効にし、新しく登録されたユーザーがログインするまで、自分の電子メールが確認されていることを防ぐためにコードを変更します。、

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid をフックします。

このチュートリアルを使用して電子メール通知を追加する方法だけでは[SendGrid](http://sendgrid.com/)、SMTP とその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。

1. パッケージ マネージャー コンソールで、次のコマンドを入力します。 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. 移動して、 [Azure SendGrid へのサインアップ ページ](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)し、無料の SendGrid アカウントに登録します。 SendGrid を構成するのには、次のようなコードを追加することで*App_Start/IdentityConfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

以下を追加する必要があります。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

アプリの設定を格納すると、このサンプルをシンプルにするには、 *web.config*ファイル。

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> セキュリティ - ソース コード内の機密データは store ことはありません。 アカウントと資格情報は、appSetting で格納されます。 Azure では安全に保管するこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portal でのタブ。 参照してください[ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。


### <a name="enable-email-confirmation-in-the-account-controller"></a>アカウント コント ローラーで確認の電子メールを有効にします。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

確認、 *Views\Account\ConfirmEmail.cshtml*ファイルが適切な razor 構文。 (、@ 文字が最初の行が見つからないことができます。 )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

アプリを実行し、登録リンクをクリックします。 登録フォームを送信するに記録されます。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

電子メール アカウントを確認し、電子メールを確認するには、リンクをクリックします。

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>ログインする前に確認の電子メールが必要です。

現在、ユーザーには、登録フォームが完了すると後ログに記録されます。 ログを記録する前に自分の電子メールを確認する一般にできます。 以下のセクションには、ログインには、確認された電子メール (認証された) 新しいユーザーを要求するコードを変更します。 更新プログラム、`HttpPost Register`メソッドを次の強調表示された変更。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

コメント アウトすることによって、`SignInAsync`メソッド、ユーザーは署名されません登録をします。 `TempData["ViewBagLink"] = callbackUrl;`行に使用できる[、アプリのデバッグ](#dbg)し、電子メールを送信することがなく登録をテストします。 `ViewBag.Message` 確認手順の表示に使用します。 [サンプルをダウンロード](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)メールをセットアップすることがなく確認の電子メールをテストするコードが含まれ、アプリケーションのデバッグにも使用できます。

作成、`Views\Shared\Info.cshtml`ファイルを開き、次の razor マークアップを追加します。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

追加、 [Authorize 属性](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)を`Contact`Home コント ローラーのアクション メソッド。 クリックして、**連絡先**匿名ユーザーにアクセスできないし、認証されたユーザーがアクセスを確認するリンク。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

更新することも必要があります、`HttpPost Login`アクション メソッド。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

更新プログラム、 *Views\Shared\Error.cshtml*エラー メッセージを表示します。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

内の任意のアカウントを削除、 **AspNetUsers**を使用してテストする電子メールのエイリアスが含まれているテーブル。 アプリを実行し、電子メール アドレスを確認するまでにログインできないことを確認します。 電子メール アドレスのことを確認したら、クリックして、**連絡先**リンク。

<a id="reset"></a>
## <a name="password-recoveryreset"></a>パスワード回復/リセット

コメント文字を削除、`HttpPost ForgotPassword`アクション メソッド、account コント ローラー。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

コメント文字を削除、 `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)で、 *Views\Account\Login.cshtml* razor ビュー ファイル。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

ログイン ページでは、パスワードのリセットへのリンクが表示されます。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>電子メールの確認リンクを再送信します。

ユーザーは、新しいローカル アカウントを作成した後は、確認リンクがログオンする前に、使用する必要が、メールで送信されます。 誤ってユーザーは、確認の電子メールを削除します。 または、電子メールが到着することはありません、確認リンクをもう一度送信される必要があります。 次のコード変更では、これを有効にする方法を示します。

下に次のヘルパー メソッドを追加、 *controllers \accountcontroller.cs*ファイル。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

新しいヘルパーを使用して Register メソッドを更新します。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

ユーザー アカウントが確認されていない場合は、パスワードを再送信する、Login メソッドを更新します。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>ソーシャル、ローカルのログイン アカウントを組み合わせる

電子メールのリンクをクリックして、ローカルおよびソーシャル アカウントを組み合わせることができます。 次の順序で**RickAndMSFT@gmail.com**が最初に、ローカル ログインとして作成は、まずソーシャル ログとしてアカウントを作成し、ローカル ログインを追加することができます。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

をクリックして、**管理**リンク。 注、**外部ログイン: 0**このアカウントに関連付けられています。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

サービス内の別のログへのリンクをクリックし、アプリの要求をそのまま使用します。 2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。 ユーザー、ソーシャル ログイン認証サービスがダウンしているか、自分のソーシャル アカウントにアクセスを紛失した可能性が高い場合は、ローカル アカウントを追加することができます。

次の図では、Tom は、ソーシャル ログイン (から確認できますが、**外部ログイン: 1**ページに表示)。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

クリックすると**パスワードの入力**同じアカウントに関連付けられたでローカルのログを追加することができます。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>より詳細に確認の電子メール

拙著のチュートリアル[アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、このトピックに移動します。

<a id="dbg"></a>
## <a name="debugging-the-app"></a>アプリのデバッグ

リンクを含むメールが届かない: 場合

- 迷惑メールまたはスパム メール フォルダーを確認します。
- SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)します。

電子メールなしの確認リンクをテストするには、ダウンロード、[完全なサンプル](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。 ページ確認のリンクと確認コードが表示されます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [リンクを ASP.NET Identity 推奨リソース](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、回復、アカウントのパスワードの確認にします。
- [Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルでは、Facebook や Google の OAuth 2 承認を持つ ASP.NET MVC 5 アプリを記述する方法。 Id のデータベースにデータを追加する方法も示します。
- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC アプリを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 このチュートリアルでは、Azure のデプロイを追加します。 ロールを使用してアプリをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法。
- [OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook でアプリを作成し、アプリ プロジェクトを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [プロジェクトの SSL の設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
