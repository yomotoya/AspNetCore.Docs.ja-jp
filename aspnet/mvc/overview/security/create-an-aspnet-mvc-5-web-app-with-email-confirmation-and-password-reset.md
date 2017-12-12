---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: "電子メールの確認とパスワードのリセット (c#) で、ログでセキュリティで保護された ASP.NET MVC 5 web アプリを作成 |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、確認の電子メールとパスワード リセット、Asp.net メンバーシップ システムを使用した ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。 . Ca"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: e3d8ad6e00b7fcb95f1c9bbe556021269c1a0624
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>電子メールの確認とパスワードのリセット (c#) で、ログでセキュリティで保護された ASP.NET MVC 5 web アプリを作成します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、確認の電子メールとパスワード リセット、Asp.net メンバーシップ システムを使用した ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。 完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)です。 ダウンロードには、電子メールや SMS プロバイダーを設定せずに確認の電子メールと SMS をテストすることのできるデバッグのヘルパーが含まれています。
> 
> このチュートリアルは、によって書き込まれました[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>ASP.NET MVC アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。

> [!NOTE]
> 警告: インストールする必要あります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームは、web フォーム アプリケーションで同じ手順を実行できなかったために、ASP.NET Identity もサポートします。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. 既定の認証としてのままにして**個々 のユーザー アカウント**です。 Azure でアプリケーションをホストする場合は、チェック ボックスをオンのままにします。 このチュートリアルで後ほどは、Azure に配置します。 実行できます[無料の Azure アカウントを開設](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)です。
3. 設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。
4. アプリを実行する をクリックして、**登録**リンクし、ユーザーを登録します。 この時点では、電子メールにのみ、検証、 [[EmailAddress]](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx)属性。
5. サーバー エクスプ ローラーに移動**データ Connections\DefaultConnection\Tables\AspNetUsers**を右クリックし、選択**テーブル定義を開き**です。

    次の図は、`AspNetUsers`スキーマ。

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. 右クリックして、 **AspNetUsers**テーブルを選択して**テーブル データの表示**です。  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 この時点で、電子メールが確認されていません。
7. 行をクリックし、[削除] を選択します。 次の手順でこの電子メールをもう一度追加して、確認の電子メールを送信します。

## <a name="email-confirmation"></a>確認の電子メール

他のユーザーを偽装するしていないことを確認する新規ユーザーの登録の電子メール アドレスを確認することをお勧め (つまり、まだに登録されている他のユーザーの電子メール)。 しないようにする、ディスカッション フォーラムするいたと仮定`"bob@example.com"`としての登録から`"joe@contoso.com"`です。 電子メールの確認なし`"joe@contoso.com"`アプリから不要な電子メールを取得する可能性があります。 として誤って Bob が登録されていると仮定します`"bib@example.com"`いなかったが検出されました。 これは、と彼は、アプリは、正しいメール アドレスが見つからないため、パスワードの回復を使用できなくします。 確認の電子メールが bot から制限の保護のみを提供し、決定スパム攻撃者から保護を提供しません、登録に使用できる多くの作業電子メール エイリアスが。

一般にする新しいユーザーが電子メール、SMS テキスト メッセージ、または別のメカニズムによって確認されて前に、web サイトにデータを送信するを防ぐ。 <a id="build"></a>以下のセクションでは確認の電子メールを有効にして新たに登録されたユーザーが自分の電子メールが確認されるまでログ記録するを防ぐためにコードを変更します。、

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>SendGrid をフックします。

このチュートリアルはのみを使用して電子メール通知を追加する方法を示しますが[SendGrid](http://sendgrid.com/)、SMTP、およびその他のメカニズムを使用して電子メールを送信することができます (を参照してください[その他のリソース](#addRes))。

1. パッケージ マネージャー コンソールで、次を入力して、次のコマンド。 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. 移動して、 [Azure SendGrid のサインアップ ページ](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409)SendGrid アカウントの登録は無料とします。 SendGrid を構成するのには、次のようなコードを追加します。

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

以下を追加する必要があります。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

このサンプルをシンプルにするを保存することがアプリの設定、 *web.config*ファイル。

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> セキュリティ - ソース コード内の機密データはストアことはありません。 アカウントと資格情報は、appSetting に格納されます。 Azure で安全に格納できますこれらの値で、 **[構成](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)**  タブで、Azure ポータルです。 参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。


### <a name="enable-email-confirmation-in-the-account-controller"></a>アカウント コント ローラーの確認の電子メールを有効にします。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

確認、 *Views\Account\ConfirmEmail.cshtml*ファイルが正しい razor 構文を持ちます。 (、@、最初の文字、行が、不足している可能性があります。 )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

アプリを実行して、登録のリンクをクリックします。 登録フォームを送信すると後ログインしています。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

電子メール アカウントを確認し、電子メールを確認するリンクをクリックします。

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>ログインする前に確認の電子メールが必要

現在、ユーザーが完了したら、登録フォームに記録されます。 ログを記録する前に自分の電子メールを確認する一般にできます。 以下のセクションでおを新しいユーザーを認証するには記録前に確認された電子メールがある () を必要とするコードを変更します。 更新プログラム、`HttpPost Register`メソッドを次の強調表示された変更。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

コメント アウトすることによって、`SignInAsync`メソッド、ユーザーは署名されません登録しています。 `TempData["ViewBagLink"] = callbackUrl;`行に使用できる[、アプリのデバッグ](#dbg)および電子メールを送信せずに登録をテストします。 `ViewBag.Message`確認の手順の表示に使用します。 [サンプルをダウンロード](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)電子メールの設定せずに確認の電子メールをテストするコードが含まれており、アプリケーションをデバッグするも使用できます。

作成、`Views\Shared\Info.cshtml`ファイルし、次の razor マークアップを追加します。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

追加、 [Authorize attribute](https://msdn.microsoft.com/en-us/library/system.web.mvc.authorizeattribute(v=vs.118).aspx)を`Contact`Home コント ローラーのアクション メソッド。 クリックを使用することができます、**連絡先**リンク匿名ユーザーがアクセスを持っていないし、認証されたユーザーがアクセスを確認します。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

更新する必要があります、`HttpPost Login`アクション メソッド。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

更新プログラム、 *Views\Shared\Error.cshtml*エラー メッセージを表示するビュー。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

内の任意のアカウントを削除、 **AspNetUsers**を使用してテストする電子メールのエイリアスを含むテーブル。 アプリを実行して、電子メール アドレスを確認するまでにログインできないことを確認します。 クリックして、電子メール アドレスのことを確認したら、**連絡先**リンクします。

<a id="reset"></a>
## <a name="password-recoveryreset"></a>パスワードの回復またはリセット

コメント文字を削除、`HttpPost ForgotPassword`でアカウント コント ローラー アクション メソッド。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

コメント文字を削除、 `ForgotPassword` [ActionLink](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx)で、 *Views\Account\Login.cshtml* razor ファイルの表示。

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

ページのログでは、パスワード リセットへのリンクが表示されます。

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>再送信する電子メールの確認のリンク

ユーザーは、新しいローカル アカウントを作成した後が確認のリンクを使用してログオンする前にする必要が電子メールで送信します。 誤ってユーザーが、確認メールを削除または電子メールが到着することはありません、確認のリンクを再送信する必要があります。 次のコード変更では、これを有効にする方法を示します。

一番下に次のヘルパー メソッドを追加、 *Controllers\AccountController.cs*ファイル。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

新しいヘルパーを使用して Register メソッドを更新します。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

ログイン メソッドを更新して、パスワードを再送信するときに、ユーザー アカウントが確認されていない場合。

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>ソーシャルとローカルのログイン アカウントを結合します。

電子メールのリンクをクリックすると、ローカルおよびソーシャルのアカウントを組み合わせることができます。 次の順序で **RickAndMSFT@gmail.com** が最初に、ローカル ログインとして作成しますが、最初に、ソーシャル ログインとしてアカウントを作成し、ローカル ログインを追加することができます。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

をクリックして、**管理**リンクします。 このアカウントに関連付けられた 0 外部 (ソーシャル ログイン) に注意してください。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

サービス内の別のログへのリンクをクリックし、アプリケーション要求を許可します。 2 つのアカウントが結合されている、いずれかのアカウントでログオンすることができます。 ユーザー認証サービスのソーシャル、ログがダウンしているか、ソーシャル アカウントにアクセスを紛失した可能性が高い場合に、ローカル アカウントを追加することができます。

次の図では、Tom は、ソーシャル ログイン、(からわかるようにする、**外部ログイン: 1**ページに表示されます)。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

クリックすると**パスワードを選択して**同じアカウントに関連付けられているを使用すると、ローカルのログを追加します。

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>さらに詳しく確認の電子メール

チュートリアル[アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)より詳細には、このトピックに移動します。

<a id="dbg"></a>
## <a name="debugging-the-app"></a>アプリのデバッグ

場合は、リンクを含む電子メールを取得しません。

- 迷惑メールまたは迷惑メール フォルダーをチェックします。
- SendGrid アカウントにログインし、をクリックして、[電子メール アクティビティ リンク](https://sendgrid.com/logs/index)です。

電子メールなしの確認リンクをテストするには、ダウンロード、[完成したサンプル](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)です。 ページで、確認のリンクと確認コードが表示されます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Id へのリンクは、リソースを推奨](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)回復とアカウントのパスワードの確認の詳細に入ります。
- [Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルは、Facebook、Google OAuth 2 の承認で ASP.NET MVC 5 アプリを記述する方法を示します。 ユーザー データベースにデータを追加する方法も示しています。
- [メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Azure にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 このチュートリアルは、Azure のデプロイを追加の役割を使用してアプリケーションをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法です。
- [OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook でアプリを作成し、プロジェクトに、アプリを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [プロジェクトで SSL を設定します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
