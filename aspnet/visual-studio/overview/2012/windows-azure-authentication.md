---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure Authentication |Microsoft Docs
author: Rick-Anderson
description: Windows Azure Active Directory 用の Microsoft ASP.NET ツールをシンプルな Windows Azure Web サイトでホストされている web アプリケーションの認証を有効にしています.
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: d5d055d90b263050ef6defa1b98b139c4f8e4dee
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829271"
---
<a name="windows-azure-authentication"></a>Windows Azure の認証
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> Windows Azure Active Directory でホストされている web アプリケーションの認証を有効にする単純な用のツールは Microsoft ASP.NET [Windows Azure Websites](https://www.windowsazure.com/home/features/web-sites/)します。 Windows Azure の認証を使用して、組織、オンプレミスの Active Directory から同期された会社のアカウントまたはカスタムの Windows Azure Active Directory ドメインで作成されたユーザーから Office 365 ユーザーを認証することができます。 Windows Azure Authentication を有効にすると、1 つを使用してユーザーを認証するアプリケーションを構成します[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)テナント。
> 
> クラウド サービスの web ロール ASP.NET Windows Azure Authentication ツールがサポートされていませんが、将来のリリースで予定します。 [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) が Windows Azure の web ロールでサポートされています。
> 
> 組織のオンプレミスの Active Directory と Windows Azure Active Directory テナント間の同期をセットアップする方法の詳細については、「[実装および管理を使用して AD FS 2.0 シングル サインオン](https://technet.microsoft.com/library/jj205462.aspx)します。
> 
> Windows Azure Active Directory が現在として使用できますが、[無料のプレビュー サービス](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)します。


## <a name="requirements"></a>要件:

- Visual Studio 2012 または[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Web ツールの Visual Studio 2012 用拡張機能](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)または[Web ツールの拡張機能の Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET ツールの Windows の Visual Studio 2012 – Azure Active Directory](https://go.microsoft.com/fwlink/?LinkID=282306)または[Microsoft ASP.NET ツールの Windows 用 Azure Active Directory-Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Visual Studio 2012 での ASP.NET Web アプリケーションを作成します。

任意の Web アプリケーションを作成するには Visual Studio 2012 で、このチュートリアルは、ASP.NET MVC イントラネット テンプレートを使用します。

1. 新しい ASP.NET MVC 4 イントラネット アプリケーションを作成し、すべての既定値をそのまま使用します。 (In する必要があります**tra** net ではなく**を入力してください**net プロジェクト)。  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>(この場合、テナントのグローバル管理者である場合)、Window Azure の認証を有効にします。

サインアップして、新しいテナントを作成するには (既存の Office 365 アカウント) を使用して既存の Windows Azure Active Directory テナントがあるない場合、[新しい Windows Azure Active Directory アカウント](http://g.microsoftonline.com/0AX00en/5)します。

1. [プロジェクト] メニューから選択**を有効にする Windows Azure Authentication**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Windows Azure Active Directory テナント (contoso.onmicrosoft.com など) のドメインを入力し、クリックして**を有効にする**:

![](windows-azure-authentication/_static/image3.png)

3. 管理者は、Windows Azure Active Directory テナントで Web 認証ダイアログ サインイン。  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>理念でも、非管理者によって Window Azure を有効にします。

Windows Azure Active Directory テナントのグローバル管理者特権がない、アプリケーションをプロビジョニングするためのチェック ボックスをオフにできます。

![](windows-azure-authentication/_static/image6.png)

ダイアログ ボックスが表示されます、**ドメイン**、**アプリケーション プリンシパル Id**と**応答 URL** Azure Active Directory とアプリケーションのプロビジョニングに必要な理念でもあります。 情報は、アプリケーションをプロビジョニングするための十分な特権を持っている人に通知する必要があります。 参照してください[Windows Azure Active Directory - ASP.NET アプリケーションでシングル サインオンを実装する方法](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)コマンドレットを使用して手動でサービス プリンシパルを作成する方法の詳細について。  
クリックすることができます、アプリケーションが正常にプロビジョニングされたら、 **、選択した設定を web.config の更新を続行**します。 クリックすることができますが発生します。 へのプロビジョニングを待っている間に、アプリケーションの開発を続ける場合**に近いプロジェクト ファイル内の設定を保存**します。 次回の Windows Azure の認証の有効化を呼び出すし、プロビジョニングのチェック ボックスをオフにする、同じ設定が表示されますおよびクリックすることができます**続行**、クリックして、そして**web.configでこれらの設定を適用**.

1. アプリケーションが Windows Azure の認証用に構成して Windows Azure Active Directory でプロビジョニングされています。
2. アプリケーションの Windows Azure Authentication を有効にすると、クリックして**閉じる。** 

    ![](windows-azure-authentication/_static/image7.png)
3. アプリケーションを実行するには、f5 キーを押します。 ログイン ページにする自動的にリダイレクトする必要があります。 ディレクトリのテナント ユーザーの資格情報を使用して、アプリケーションにログインする.  

    ![](windows-azure-authentication/_static/image1.jpg)
4. アプリケーションには、テスト用の自己署名証明書が現在使用しているため、証明書が信頼された証明機関から発行されていませんが、ブラウザーから、警告が表示されます。

    この警告は無視してかまいませんローカルの開発中をクリックして**このサイトの閲覧を続行します。** 

    ![](windows-azure-authentication/_static/image8.png)
5. 今すぐが正常にログインする Windows Azure の認証を使用して、アプリケーションに!

    ![](windows-azure-authentication/_static/image2.jpg)

Windows Azure を有効にする認証は、アプリケーションに、次の変更を加えます。

- 対策、クロスサイト リクエスト フォージェリ ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) クラス (*アプリ\_Start\AntiXsrfConfig.cs* ) をプロジェクトに追加されます。
- NuGet パッケージ`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`をプロジェクトに追加されます。
- Windows Identity Foundation の設定、アプリケーションでは、Windows Azure Active Directory テナントからのセキュリティ トークンを受け入れるように構成されます。 加えられた変更の展開ビューを表示する次の画像をクリックして、 *Web.config*ファイル。  
  
     ![](windows-azure-authentication/_static/image9.png)
- Windows Azure Active Directory テナントでアプリケーションのサービス プリンシパルがプロビジョニングされます。
- HTTPS が有効になっているとします。

## <a name="deploy-the-application-to-windows-azure"></a>Windows Azure へのアプリケーションをデプロイします。

完全な手順については、次を参照してください。 [ASP.NET Web アプリケーションを Windows Azure Web サイトを展開する](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)します。

Windows Azure Authentication を Azure Web サイトを使用してアプリケーションを発行するには。

1. アプリケーションを右クリックし、選択**発行。** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Web の発行 ダイアログ ボックスからダウンロードし、Azure Web サイトの発行プロファイルをインポートします。

    ![](windows-azure-authentication/_static/image4.jpg)
3. **接続** タブの表示、**送信先 URL** (アプリケーションのパブリックに公開された URL)。 クリックして**接続の検証**接続をテストします。

    ![](windows-azure-authentication/_static/image5.jpg)
4. チェック検討以前この Azure Web サイトにパブリッシュした場合、**転送先に追加のファイルを削除**アプリケーションのことを確認するには設定が正常に発行します。 通知、**を有効にする Windows Azure Authentication**  チェック ボックスが slected します。  

    ![](windows-azure-authentication/_static/image10.png)
5. : 省略可能**プレビュー**タブをクリックして**プレビューの開始**に展開されたファイルを参照してください。

    ![](windows-azure-authentication/_static/image6.jpg)
6. クリックして**を発行します。**

    ターゲット ホスト用の Windows Azure Authentication を有効にするように促されます。 をクリックして**を有効にする**を続行します。

    ![](windows-azure-authentication/_static/image11.png)
7. Windows Azure Active Directory テナントの管理者の資格情報を入力します。

    ![](windows-azure-authentication/_static/image7.jpg)
8. アプリケーションが正常に公開されると、ブラウザーが発行した web サイトを開きます。

    > [!NOTE]
    > 5 分 (通常必要が少なく) アプリケーションのターゲット ホスト用の Windows Azure Authentication を有効にした後、Windows Azure Active Directory と完全にプロビジョニングするに時間がかかる場合があります。 初めて実行する場合、アプリケーション エラー ACS50001 が発生する場合: 名前 '[realm]' の証明書利用者が見つかりませんでした。 数分待ってから、やし、もう一度アプリケーションを実行してみてください。
9. 求められたら、ディレクトリ内のユーザーとしてログインします。

    ![](windows-azure-authentication/_static/image8.jpg)
10. 正常に、Azure にログインして今すぐ Windows Azure の認証を使用してアプリケーションをホストします。  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>既知の問題

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Windows Azure Authentication < o:p >< を使用する場合、ロール ベースの承認が失敗した/o:p >

Windows Azure の認証は必要なロール クレームが役割ベースの承認を実行できるように現在提供していません。 Windows Azure Active Directory から < o:p ><、認証されたユーザーのロールを手動で取得する必要があります/o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>ブラウズのエラーの結果を Windows Azure Authentication アプリケーションに「ACS20016 (live.com) ログインしたユーザーのドメインと一致しません、許可されているこのドメイン STS」< o:p ></o:p >

400 エラー応答が取得が Windows Azure Authentication を有効になっているアプリケーションにアクセスしようとした場合、Microsoft アカウント (hotmail.com、live.com、outlook.com など) にログインして既に Microsoft アカウントのドメインWindows Azure Active Directory では認識されません。 アプリケーションにログイン、ログアウト、Microsoft アカウントから最初にします < o:p ></o:p >。

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>[なし] < o:p >< accounts.accesscontrol.windows.net 証明書の証明書の検証エラーの結果以外の Windows Azure の認証が有効なアプリケーションと、X509CertificateValidationMode にログイン/o:p >

証明書の検証は必要でないし、無効のままにする必要があります。 WSFederationAuthenticationModule の < o:p >< が発行者証明書の拇印を検証/o:p >。

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Web 認証ダイアログがエラーを示します Windows Azure Authentication を有効にしようとしています"ACS20016: ログインしているユーザー (contoso.onmicrosoft.com) のドメインがこの STS の許可されたどのドメインと一致しません。"。< o:p ></o:p >

このエラーは、Visual Studio の同じプロセス内からさまざまな Windows Azure Active Directory アカウントを使用して正常にログインしたときに表示があります。 指定されたアカウントからログアウトするか、Visual Studio を再起動します。 以前に記録「サインインしたまま」するオプションを選択してかどうか、ブラウザーの cookie < o:p >< をクリアする必要があります/o:p >。

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: 要求が有効な Ws-federation プロトコル メッセージ < o:p ></o:p >

これは、既に Azure サービスのいずれかにその他の Microsoft ID を使用してログインしている場合に発生することができます。 使用してプライベート ブラウザー ウィンドウなどの IE で InPrivate または chrome Incognito またはすべての cookie をクリアします。 <o:p></o:p>

## <a name="additional-resources"></a>その他のリソース

- [Microsoft ASP.NET ツールの Windows 用 Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure の機能: Identity](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: 組織のアプリの開発](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: 複数の組織向けアプリの開発](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Windows Azure Active Directory でのシングル サインオンを実装する方法](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [シングル サインオンが windows Azure Active Directory: Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [実装および管理を使用して AD FS 2.0 シングル サインオン](https://technet.microsoft.com/library/jj205462.aspx)
