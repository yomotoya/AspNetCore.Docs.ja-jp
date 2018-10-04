---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、2 要素認証を使用した ASP.NET MVC 5 web アプリを構築する方法を示します。 Web アプリの作成、セキュリティで保護された ASP.NET MVC 5 を完了する必要があります.
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577601"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ
====================
によって[Rick Anderson]((https://twitter.com/RickAndMSFT))

> このチュートリアルでは、2 要素認証を使用した ASP.NET MVC 5 web アプリを構築する方法を示します。 完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成する](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。 完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)します。 ダウンロードには、確認の電子メールと SMS を電子メールまたは SMS プロバイダーをセットアップすることがなくテストできるデバッグ ヘルパーが含まれています。
> 
> このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


- [ASP.NET MVC アプリを作成します。](#createMvc)
- [2 要素認証のための SMS をセットアップします。](#SMS)
- [2 要素認証を有効にします。](#enable2)
- [その他のリソース](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>ASP.NET MVC アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。

> [!NOTE]
> : 警告完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリを作成する](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。 インストールする必要があります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降に、このチュートリアルを完了します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームには ASP.NET Identity もサポートしていますので、web フォーム アプリで同じ手順を実行できます。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. 既定の認証としてのままに**個々 のユーザー アカウント**します。 Azure でアプリをホストする場合は、チェック ボックスをオンのままにします。 チュートリアルの後半では、Azure を展開します。 できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)します。
3. 設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)します。

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   アドレス:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   名前空間:  
    `ASPSMSX2`
3. **SMS プロバイダーのユーザーの資格情報を見極める**  
  
   Twilio:  
   **ダッシュ ボード**、Twilio アカウントの コピーのタブ、**アカウント SID**と**Auth トークン**します。  
  
   ASPSMS:  
   移動しますから、アカウントの設定、**別子-Userkey**し、自己定義型と共にコピー**パスワード**します。  
  
   これらの値を後で保存、 *web.config*キー内のファイル`"SMSAccountIdentification"`と`"SMSAccountPassword"`します。
4. **SenderID を指定する/発信元**  
  
   Twilio:  
   **番号** タブで、Twilio 電話番号をコピーします。  
  
   ASPSMS:  
   内で、**発信者のロックを解除**] メニューの [発信者の 1 つまたは複数のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。  
  
   後でこの値を保存、 *web.config*キー内のファイル`"SMSAccountFrom"`します。
5. **アプリへの SMS プロバイダーの資格情報の転送**  
  
   資格情報と差出人の電話番号を使用できるように、アプリ。 これらの値を保存する単純なために、 *web.config*ファイル。 Azure にデプロイする、値で安全に保存できる、**アプリ設定**セクションで、web サイト タブを構成します。 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データは store ことはありません。 アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。 参照してください[ASP.NET と Azure へパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。
6. **SMS プロバイダーへのデータ転送の実装**  
  
   構成、`SmsService`クラス、*アプリ\_Start\IdentityConfig.cs*ファイル。  
  
   いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. 更新プログラム、 *Views\Manage\Index.cshtml* Razor ビュー: (注: だけで既存のコードにコメントを解除して、次のコードを使用しない)。  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. 確認、`EnableTwoFactorAuthentication`と`DisableTwoFactorAuthentication`内のアクション メソッド、`ManageController`が、[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)属性。  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. アプリを実行し、以前に登録したアカウントでログインします。
10. アクティブ化ユーザー ID を取得するには、をクリックして、`Index`内のアクション メソッド`Manage`コント ローラー。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. [追加] をクリックします。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力するダイアログ ボックスが表示されます。

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. 数秒で確認コードを含むテキスト メッセージが表示されます。 入力してキーを押して**送信**します。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. 電話番号が追加された、管理ビューを示しています。

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>2 要素認証を有効にします。

生成されたテンプレートのアプリケーションで UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。 2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

有効にする 2 fa をクリックします。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

ログアウトし、再びログインします。 電子メールを有効にした場合 (を参照してください、[前のチュートリアル](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa の電子メールを選択することができます。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

(SMS または電子メール) から、コードを入力できますが、コードの確認ページが表示されます。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

クリックすると、**このブラウザーを記憶する** チェック ボックスの除外 2 fa を使用して、チェック ボックスに、ブラウザーとデバイスを使用する場合にログインする必要がなくなります。 悪意のあるユーザーは、デバイスへのアクセスを得ることはできません、限り、2 fa を有効にすると、クリックすると、**このブラウザーを記憶する**は便利な 1 つのステップ パスワード アクセスを提供、すべてのアクセスの保護を強力な 2 fa を維持しながら信頼されていないデバイス。 これは、定期的に使用するプライベートあらゆるデバイスで行うことができます。

このチュートリアルでは、新しい ASP.NET MVC アプリで 2 fa を有効にする簡単な説明を提供します。 拙著のチュートリアル[ASP.NET Identity で SMS と電子メールを使用する 2 要素認証](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)サンプルの背後にあるコードの詳細を見ていきます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [ASP.NET Identity で SMS と電子メールを使用する 2 要素認証](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)に 2 要素認証の詳細
- [リンクを ASP.NET Identity 推奨リソース](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET Identity によるパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)の詳細については、回復、アカウントのパスワードの確認にします。
- [Facebook、Twitter、LinkedIn、Google の OAuth2 サインオンした MVC 5 アプリケーション](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルでは、Facebook や Google の OAuth 2 承認を持つ ASP.NET MVC 5 アプリを記述する方法。 Id のデータベースにデータを追加する方法も示します。
- [Azure の Web へのメンバーシップ、OAuth、SQL Database と安全な ASP.NET MVC アプリのデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 このチュートリアルでは、Azure のデプロイを追加します。 ロールを使用してアプリをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法。
- [OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook でアプリを作成し、アプリ プロジェクトを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [プロジェクトの SSL の設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
