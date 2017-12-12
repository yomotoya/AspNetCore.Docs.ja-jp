---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "SMS と 2 要素認証の電子メールを使って ASP.NET MVC 5 アプリ |Microsoft ドキュメント"
author: Rick-Anderson
description: "このチュートリアルでは、2 要素認証による ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。 Web アプリの作成をセキュリティで保護された ASP.NET MVC 5 を完了する必要があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>SMS と 2 要素認証の電子メールを使って ASP.NET MVC 5 アプリ
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルでは、2 要素認証による ASP.NET MVC 5 web アプリケーションをビルドする方法を示します。 完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。 完成したアプリケーションをダウンロードする[ここ](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952)です。 ダウンロードには、電子メールや SMS プロバイダーを設定せずに確認の電子メールと SMS をテストすることのできるデバッグのヘルパーが含まれています。
> 
> このチュートリアルは、によって書き込まれました[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


- [ASP.NET MVC アプリを作成します。](#createMvc)
- [2 要素認証のための SMS をセットアップします。](#SMS)
- [2 要素認証を有効にします。](#enable2)
- [その他のリソース](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>ASP.NET MVC アプリを作成します。

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 インストール[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)またはそれ以降。

> [!NOTE]
> 警告: 完了する必要があります[ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)続行する前にします。 インストールする必要があります[Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465)以降このチュートリアルを完了します。


1. 新しい ASP.NET Web プロジェクトを作成し、MVC テンプレートを選択します。 Web フォームは、web フォーム アプリケーションで同じ手順を実行できなかったために、ASP.NET Identity もサポートします。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. 既定の認証としてのままにして**個々 のユーザー アカウント**です。 Azure でアプリケーションをホストする場合は、チェック ボックスをオンのままにします。 このチュートリアルで後ほどは、Azure に配置します。 実行できます[無料の Azure アカウントを開設](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)です。
3. 設定、 [SSL を使用するプロジェクト](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)です。

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
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 アドレス:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 名前空間:  
    `ASPSMSX2`
3. **SMS プロバイダーのユーザーの資格情報を見つけ出し**  
  
 Twilio:  
 **ダッシュ ボード**コピー、Twilio アカウントのタブ、**アカウント SID**と**認証トークン**です。  
  
 ASPSMS:  
 アカウントの設定からに移動**ユーザー キー**自己定義と共にコピー**パスワード**です。  
  
 これらの値を保存後で、 *web.config*キー内のファイル`"SMSAccountIdentification"`と`"SMSAccountPassword"`です。
4. **SenderID を指定する/発信元**  
  
 Twilio:  
 **番号** タブで、Twilio 電話番号をコピーします。  
  
 ASPSMS:  
 内で、**発信者のロックを解除** メニューの 1 つまたは複数の発信者のロックを解除または発信元が英数字であることを (すべてのネットワークではサポートされていません) を選択します。  
  
 この値では後で格納、 *web.config*キー内のファイル`"SMSAccountFrom"`です。
5. **アプリに SMS プロバイダーの資格情報を転送します。**  
  
 資格情報と差出人の電話番号を使用できるように、アプリ。 これらの値を格納お煩雑にならないように、 *web.config*ファイル。 Azure にデプロイして時に、安全に値を格納おできます、**アプリ設定**セクションの web サイトの構成 タブ。 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データはストアことはありません。 アカウントと資格情報は、サンプルをシンプルにする上記のコードに追加されます。 参照してください[ASP.NET と Azure へのパスワードや他の機密データの展開のベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。
6. **SMS プロバイダーへのデータ転送の実装**  
  
 構成、`SmsService`クラス内で、*アプリ\_Start\IdentityConfig.cs*ファイル。  
  
 いずれかのアクティブ化に使用される SMS プロバイダーによって、 **Twilio**または**ASPSMS**セクション。 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. 更新プログラム、 *Views\Manage\Index.cshtml* Razor ビュー: (注: しないでだけ、既存のコードにコメントを解除して、次のコードを使用します)。  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. 確認、`EnableTwoFactorAuthentication`と`DisableTwoFactorAuthentication`内のアクション メソッド、`ManageController`が、[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx)属性。  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. アプリを実行し、以前に登録したアカウントでログインします。
10. ユーザー ID とアクティブ化するをクリックして、`Index`でアクション メソッド`Manage`コント ローラー。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. [追加] をクリックします。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. `AddPhoneNumber`アクション メソッドには、SMS メッセージを受信できる電話番号を入力 ダイアログ ボックスが表示されます。

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. 数秒で、確認コードと共にテキスト メッセージが表示されます。 これを入力し、キーを押して**送信**です。  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. 電話番号が追加された、管理ビューを示しています。

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>2 要素認証を有効にします。

生成されたテンプレートのアプリでは、UI を使用して、2 要素認証 (2 fa) を有効にする必要があります。 2 fa を有効にするには、ナビゲーション バーで、ユーザー ID (電子メールのエイリアス) をクリックします。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

有効にする 2 fa をクリックします。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

ログアウト ページ、し、再度ログオンします。 電子メールを有効にした場合 (を参照してください [前のチュートリアル](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md))、SMS または 2 fa 用の電子メールを選択できます。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

コードの確認 ページが表示されます (SMS または電子メール) からコードを入力できます。

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

クリックすると、**このブラウザーに覚えて** チェック ボックスの除外を 2 fa を使用して、チェック ボックスに、ブラウザーとデバイスを使用する場合にログインする必要があるからです。 悪意のあるユーザーは、デバイスにアクセスできない、限り 2 fa を有効にしてをクリックすると、**このブラウザーに覚えて**が提供されます便利な 1 つのステップのパスワードのアクセス権を持つすべてのアクセスの保護を強力な 2 fa を引き続き維持しながら信頼されていないデバイス。 これは、定期的に使用する任意のプライベート デバイスで行うことができます。

このチュートリアルでは、新しい ASP.NET MVC アプリケーションで 2 fa を有効にする簡単な説明を提供します。 チュートリアル[SMS や電子メールを使用して、ASP.NET の Id を持つ 2 要素認証](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)サンプル分離コードの詳細を見ていきます。

<a id="addRes"></a>
## <a name="additional-resources"></a>その他のリソース

- [2 要素認証が SMS や電子メールを使用して、ASP.NET の Id を持つ](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)に 2 要素認証の詳細
- [ASP.NET Id へのリンクは、リソースを推奨](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [アカウントの確認と ASP.NET の Id とパスワードの回復](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)回復とアカウントのパスワードの確認の詳細に入ります。
- [Facebook、Twitter、LinkedIn および Google OAuth2 サインオン MVC 5 アプリ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)このチュートリアルは、Facebook、Google OAuth 2 の承認で ASP.NET MVC 5 アプリを記述する方法を示します。 ユーザー データベースにデータを追加する方法も示しています。
- [メンバーシップ、OAuth、SQL データベースでのセキュリティで保護された ASP.NET MVC アプリケーションを Azure Web に配置](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 このチュートリアルは、Azure のデプロイを追加の役割を使用してアプリケーションをセキュリティで保護する方法、メンバーシップ API を使用して、ユーザーとロール、および追加のセキュリティ機能を追加する方法です。
- [OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Facebook でアプリを作成し、プロジェクトに、アプリを接続します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [プロジェクトで SSL を設定します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
