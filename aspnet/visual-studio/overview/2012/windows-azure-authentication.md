---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure Authentication |Microsoft ドキュメント"
author: Rick-Anderson
description: "Windows Azure Active Directory 用の Microsoft ASP.NET ツールを簡単に Windows Azure Web サイトでホストされている web アプリケーションの認証を有効にしています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1cb38d66bd0373159e54abf822fba9c5829774ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="windows-azure-authentication"></a>Windows Azure の認証
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET ツールを Windows Azure Active Directory では、ホストされている web アプリケーションの認証を有効にする単純な[Windows Azure Web サイト](https://www.windowsazure.com/en-us/home/features/web-sites/)です。 Windows Azure 認証を使用して、組織、内部設置型 Active Directory から同期された会社のアカウントまたはカスタムの Windows Azure Active Directory ドメインで作成されたユーザーから Office 365 のユーザーを認証することができます。 Windows Azure 認証を有効にすると、1 つを使用してユーザーを認証するアプリケーションを構成[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)テナントです。
> 
> ASP.NET Windows Azure 認証ツールは web ロール、クラウド サービスでサポートされていませんが、そのためには、将来のリリースで予定します。 [Windows Identity Foundation](https://msdn.microsoft.com/en-us/library/hh291066(v=VS.110).aspx) (WIF) は Windows Azure web ロールでサポートされています。
> 
> 内部設置型 Active Directory と Windows Azure Active Directory テナント間の同期をセットアップする方法の詳細については、「[実装および管理を使用して AD FS 2.0 シングル サインオン](https://technet.microsoft.com/en-us/library/jj205462.aspx)です。
> 
> Windows Azure Active Directory は現在として使用できますが、[プレビュー サービスを無料](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。


## <a name="requirements"></a>要件:

- Visual Studio 2012 または[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express)
- [Web ツールの Visual Studio 2012 用の拡張機能](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)または[Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET Tools Windows for Visual Studio 2012 – Azure Active Directory](https://go.microsoft.com/fwlink/?LinkID=282306)または[Microsoft ASP.NET Tools Windows 用 Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Visual Studio 2012 での ASP.NET Web アプリケーションを作成します。

Visual Studio 2012 で任意の Web アプリケーションを作成することができます、このチュートリアルは、ASP.NET MVC イントラネット テンプレートを使用します。

1. 新しい ASP.NET MVC 4 イントラネット アプリケーションを作成し、すべての既定値をそのまま使用します。 (In する必要があります**検査**net ではなく**を入力してください**net プロジェクト)。  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>(この場合、テナントのグローバル管理者である場合)、Window Azure 認証を有効にします。

サインアップ、新しいテナントを作成するには (既存の Office 365 アカウント) を使用して、既存の Windows Azure Active Directory テナントがあるない場合、[新しい Windows Azure Active Directory アカウント](http://g.microsoftonline.com/0AX00en/5)です。

1. [プロジェクト] メニューから選択**を有効にする Windows Azure Authentication**:  
  
 ![](windows-azure-authentication/_static/image2.png)

2. Windows Azure Active Directory テナント (たとえば、contoso.onmicrosoft.com) のドメインを入力し、クリックして**を有効にする**:

![](windows-azure-authentication/_static/image3.png)

3. Windows Azure Active Directory テナントの管理者としてで Web 認証ダイアログ サインイン。  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Window Azure をテナントの管理者以外で有効にします。

Windows Azure Active Directory テナントのグローバル管理者特権がない、アプリケーションのプロビジョニングのチェック ボックスをオフにすることができます。

![](windows-azure-authentication/_static/image6.png)

ダイアログ ボックスが表示されます、**ドメイン**、**アプリケーションのプリンシパル Id**と**応答 URL** Azure Active Directory とアプリケーションのプロビジョニングに必要な理念です。 アプリケーションをプロビジョニングするための十分な特権を持つユーザーにこの情報を提供する必要があります。 参照してください[Windows Azure Active Directory の ASP.NET アプリケーションにシングル サインオンを実装する方法](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)コマンドレットを使用して手動でサービス プリンシパルを作成する方法の詳細。  
アプリケーションが正常にプロビジョニングされるをクリックすると**、選択した設定を web.config の更新を続行**です。 クリックして、発生する可能性にプロビジョニングするための待機中にアプリケーションの開発を続行する場合**プロジェクト ファイルで設定を保存するに近い**です。 次回を有効にする Windows Azure 認証を呼び出すし、プロビジョニングのチェック ボックスをオフに同じ設定が表示されますをクリックして**続行**、クリックして、 **web.configでこれらの設定を適用**.

1. アプリケーションが Windows Azure の認証用に構成され、Windows Azure Active Directory でプロビジョニングされています。
2. アプリケーションの Windows Azure 認証を有効にすると、クリックして**閉じる。** 

    ![](windows-azure-authentication/_static/image7.png)
3. F5 キーを押して、アプリケーションを実行します。 ログイン ページに自動的にリダイレクトする必要があります。 ディレクトリ テナントのユーザーの資格情報を使用して、アプリケーションにログインする.  

    ![](windows-azure-authentication/_static/image1.jpg)
4. アプリケーションが現在テスト用の自己署名証明書を使用しているため、証明書が信頼された証明機関から発行されていません、ブラウザーから、警告が表示されます。

    この警告は無視してかまいませんローカルの開発中をクリックして**このサイトの閲覧を続行します。** 

    ![](windows-azure-authentication/_static/image8.png)
5. 今すぐログインに成功した Windows Azure 認証を使用して、アプリケーションに!

    ![](windows-azure-authentication/_static/image2.jpg)

Windows Azure を有効にする認証は、アプリケーションに、次の変更を加えます。

- 対策、クロスサイト リクエスト フォージェリ ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) クラス (*アプリ\_Start\AntiXsrfConfig.cs* ) プロジェクトに追加します。
- NuGet パッケージの`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`プロジェクトに追加します。
- Windows Identity Foundation 設定、アプリケーションでは、Windows Azure Active Directory テナントからセキュリティ トークンを受け入れるように構成されます。 変更の展開ビューを表示する次の画像をクリックして、 *Web.config*ファイル。  
  
     ![](windows-azure-authentication/_static/image9.png)
- Windows Azure Active Directory テナントでアプリケーションのサービス プリンシパルがプロビジョニングされます。
- HTTPS が有効になっているとします。

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure にアプリケーションを配置します。

完全な手順については、次を参照してください。 [ASP.NET Web アプリケーションを、Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)です。

Azure Web サイトを Windows Azure 認証を使用してアプリケーションを発行するには。

1. アプリケーションを右クリックし、選択**発行。** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. [Web の発行] ダイアログ ボックスからダウンロードして、Azure Web サイトの発行プロファイルをインポートします。

    ![](windows-azure-authentication/_static/image4.jpg)
3. **接続** タブの表示、**送信先 URL** (、パブリックに公開されたアプリケーションの URL)。 をクリックして**接続の検証**接続をテストします。

    ![](windows-azure-authentication/_static/image5.jpg)
4. この Azure Web サイトを発行している場合は、確認してください以前、**先で追加ファイルを削除する**アプリケーションを確認するには設定が正常に発行します。 通知、**を有効にする Windows Azure Authentication**  チェック ボックスは slected します。  

    ![](windows-azure-authentication/_static/image10.png)
5. : 省略可能**プレビュー**タブをクリックして**開始プレビュー**に展開されたファイルを参照してください。

    ![](windows-azure-authentication/_static/image6.jpg)
6. をクリックして**を発行します。**

    ターゲット ホストを Windows Azure 認証を有効にするように促されます。 をクリックして**を有効にする**を続行します。

    ![](windows-azure-authentication/_static/image11.png)
7. Windows Azure Active Directory テナントの管理者の資格情報を入力します。

    ![](windows-azure-authentication/_static/image7.jpg)
8. アプリケーションが正常に発行されると、公開された web サイト、ブラウザーが開きます。

    > [!NOTE]
    > 5 分 (通常は少ない) アプリケーションのターゲット ホストの Windows Azure 認証を有効にした後は Windows Azure Active Directory でプロビジョニングされている完全に時間がかかる場合があります。 初めて実行する場合、アプリケーション エラー ACS50001 発生した場合: 名前 '[領域]' の証明書利用者が見つかりませんでした。 数分待ってから、し、もう一度アプリケーションを実行してください。
9. メッセージが表示されたら、ディレクトリ内のユーザーとしてログインします。

    ![](windows-azure-authentication/_static/image8.jpg)
10. 正常に、Azure にログインして今すぐ Windows Azure 認証を使用してアプリケーションをホストします。  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>既知の問題

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Windows Azure Authentication </o:p >< を使用して失敗したロールベースの承認//o:p >

Windows Azure Authentication が提供されていません、必要なロール要求ロールベースの承認を実行できるようにします。 認証されたユーザーのロールを手動で取得から Windows Azure Active Directory </o:p ><//o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>エラーの Windows Azure の認証結果を含むアプリケーションへの参照「ACS20016 にログオンしたユーザー (live.com) のドメインと一致しません、許可されてこのドメイン STS」< o:p ><//o:p >

400 エラー応答が返る可能性があります (たとえば、hotmail.com、live.com、outlook.com など) は、Microsoft アカウントにログインして既におり、Windows Azure 認証が有効になっているアプリケーションにアクセスしようとする場合、Microsoft アカウントのドメインWindows Azure Active Directory では認識されません。 アプリケーションへのログイン、ログアウトして Microsoft アカウントから最初にします </o:p ><//o:p >。

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a></O:p >< accounts.accesscontrol.windows.net 証明書の証明書の検証エラーの結果なし 以外の場合、アプリケーションと Windows Azure 認証が有効になっていると、X509CertificateValidationMode にログイン//o:p >

証明書の検証は不要であり無効のままにする必要があります。 WSFederationAuthenticationModule の </o:p >< が発行者証明書の拇印を検証//o:p >。

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Web 認証ダイアログがエラーを表示する Windows Azure 認証を有効にしようとすると"ACS20016: ログインのユーザー (contoso.onmicrosoft.com) のドメインが、この STS の許可されたどのドメインと一致しません"。</o:p ><//o:p >

正常に同一の Visual Studio プロセス内から別の Windows Azure Active Directory アカウントを使用してログインしていたときに、このエラーが発生します。 指定されたアカウントからログアウトするか、Visual Studio を再起動します。 以前に記録「にサインインしたまま」するオプションを選択しと、ブラウザーの cookie </o:p >< をオフにする必要があります//o:p >。

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: 要求が有効な Ws-federation プロトコル メッセージ </o:p ><//o:p >

これは、既に Azure サービスのいずれかにその他のいくつかの Microsoft ID でログインしている場合に発生することができます。 使用するプライベート ブラウザー ウィンドウでは、IE で InPrivate または Incognito Chrome 内と同様にまたは、すべての cookie を消去します。 </o:p ><//o:p >

## <a name="additional-resources"></a>その他のリソース

- [Microsoft ASP.NET Tools Windows for Visual Studio 2012 – Azure Active Directory](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure の機能: Identity](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/en-us/library/hh967619.aspx)
- [Windows Azure Active Directory: 組織向けアプリの開発](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: 複数の組織向けアプリの開発](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Windows Azure Active Directory にシングル サインオンを実装する方法](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [シングル サインオンが windows Azure Active Directory: Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [実装および管理を使用して AD FS 2.0 シングル サインオン](https://technet.microsoft.com/en-us/library/jj205462.aspx)
