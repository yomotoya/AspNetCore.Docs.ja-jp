---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 作成する MVC 5 アプリを Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン (c#) |Microsoft Docs
author: Rick-Anderson
description: このチュートリアルでは、ユーザー外部の認証から資格情報で OAuth 2.0 を使用してログインできるようにする ASP.NET MVC 5 web アプリケーションを構築する方法を紹介しています.
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823790"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン (c#) で ASP.NET MVC 5 アプリを作成します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルで使用してユーザーにログインできるようにする ASP.NET MVC 5 web アプリケーションをビルドする方法は[OAuth 2.0](http://oauth.net/2/) Facebook、Twitter、LinkedIn、Microsoft、Google などの外部認証プロバイダーからの資格情報を使用します。 わかりやすくするため、このチュートリアルは Facebook や Google の資格情報の操作に焦点を当てます。
> 
> 数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるため、大きな利点を提供して web サイトでこれらの資格情報を有効にするとします。 これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。
> 
> 参照してください[SMS と電子メール 2 要素認証による ASP.NET MVC 5 アプリ](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)します。
> 
> メンバーシップ API を使用してロールを追加する方法と、ユーザーのプロファイル データを追加する方法についても説明します。 このチュートリアルの執筆者[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter 私に従ってください: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="start"></a>
## <a name="getting-started"></a>作業の開始

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。 Visual Studio のインストール[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。 Dropbox、GitHub、Linkedin、Instagram、バッファー、Salesforce、ストリーム、Stack Exchange、Tripit、Twitch、Twitter、yahoo!、および詳細については、この参照してください。[サンプル プロジェクト](https://github.com/matthewdunsdon/oauthforaspnet)します。

> [!NOTE]
> Visual Studio をインストールする必要があります[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降は、Google OAuth 2 を使用して、SSL の警告せずにローカルでデバッグします。


クリックして**新しいプロジェクト**から、**開始** ページで、かすることができます、メニューを使用してを選択**ファイル**、し**新しいプロジェクト**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>最初のアプリケーションを作成します。

をクリックして**新しいプロジェクト**を選択し、 **Visual c#** し、左側の**Web**選び**ASP.NET Web アプリケーション**します。 "MvcAuth"をプロジェクトに名前をクリックして**OK**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**します。 認証がない場合**個々 のユーザー アカウント**、クリックして、**認証の変更**ボタンをクリックし、選択**個々 のユーザー アカウント**します。 チェックして**クラウドでホスト**アプリを Azure でホストする非常に簡単になります。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

選択した場合**クラウドでホスト**、[構成] ダイアログを完了します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>NuGet を使用して、OWIN ミドルウェアを最新に更新するには

NuGet パッケージ マネージャーを使用して、更新、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)します。 選択**更新**左側のメニュー。 クリックして、**すべて更新**OWIN パッケージのみが (の次の図に示す) のボタンをクリックを検索できます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

次の図では、OWIN パッケージのみが表示されます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

入力したパッケージ マネージャー コンソール (PMC) から、`Update-Package`コマンドで、すべてのパッケージを更新します。

キーを押して**f5 キーを押して**または**Ctrl + F5**アプリケーションを実行します。 次の図では、ポート番号は、1234 です。 アプリケーションを実行するときに、別のポート番号を確認します。

ナビゲーション アイコンをクリックする必要があります、ブラウザー ウィンドウのサイズに応じて、**ホーム**、**について**、**にお問い合わせください**、 **登録**と**ログイン**リンク。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>プロジェクトの SSL の設定

Google や Facebook などの認証プロバイダーに接続するには SSL を使用する IIS Express を設定する必要があります。 ユーザー名とパスワードと、ネットワーク経由でクリア テキストで送信している SSL を使用せずに、シークレットと同様、ログイン cookie が、HTTP にバックアップを削除しないログイン後に SSL を使用して保持することは重要です。 ハンドシェイクを実行し、(つまり、HTTPS HTTP よりも低速の一括) チャネルをセキュリティで保護する時間を既に撮影する以外にも、MVC パイプラインを実行すると、前にログインしているために HTTP リダイレクトすることはありません、現在の要求または将来要求がはるかに高速です。

1. **ソリューション エクスプ ローラー**、クリックして、 **MvcAuth**プロジェクト。
2. プロジェクトのプロパティを表示する F4 キーを押します。 またはから、**ビュー**メニューを選択できます**プロパティ ウィンドウ**します。
3. 変更**SSL が有効になっている**を True にします。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. SSL URL をコピー (になる`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)。
5. **ソリューション エクスプ ローラー**を右クリックして、 **MvcAuth**順に選択して**プロパティ**します。
6. 選択、 **Web**タブをクリックしに SSL URL を貼り付けて、**プロジェクト Url**ボックス。 (Ctl + S) ファイルを保存します。 この URL を Facebook、Google の認証アプリを構成する必要があります。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. 追加、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)属性を`Home`すべての要求を必要とするコント ローラーは、HTTPS を使用する必要があります。 安全な方法は、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)フィルターがアプリケーションにします。 セクションを参照して&quot;SSL と承認属性を使用してアプリケーションを保護する&quot;マイ tutoral で[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)します。 Home コント ローラーの一部は、以下に示します。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 過去の証明書をインストールしている場合は、このセクションの残りの部分を省略し、ジャンプ[OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続する](#goog)、それ以外の場合、次の手順については、自己署名を信頼するにはIIS Express が生成した証明書。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. 読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Ie、*ホーム*ページし、SSL の警告はありません。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome も証明書を承認し、警告を表示せず、HTTPS コンテンツが表示されます。 警告に表示されるように、Firefox は独自の証明書ストアを使用します。 アプリケーションを安全にクリックして、**リスクを理解しました**します。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>OAuth 2 用の Google アプリを作成して、アプリ プロジェクトを接続します。

> [!WARNING]
> 現在 Google OAuth 手順については、次を参照してください。 [ASP.NET Core での構成の Google 認証](/aspnet/core/security/authentication/social/google-logins)します。

1. 移動し、 [Google Developers Console](https://console.developers.google.com/)します。
2. 前にプロジェクトを作成していない場合は、選択**資格情報**クリックして、左側のタブで**作成**です。
3. 左側のタブで次のようにクリックします。**資格情報**します。
4. クリックして**資格情報を作成**し**OAuth クライアント ID**します。 

    1. **Create Client ID**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。
    2. 設定、**承認済みの JavaScript**オリジン上記で使用する SSL URL を (`https://localhost:44300/`した SSL の他のプロジェクトを作成していない場合)
    3. 設定、 **Authorized リダイレクト URI**に。  
         `https://localhost:44300/signin-google`
5. OAuth 同意画面のメニュー項目をクリックし、電子メール アドレスと製品名を設定します。 フォームのクリックが完了したら**保存**します。
6. ライブラリ メニュー項目をクリックし、検索、 **Google + API**、それをクリックし、有効にするキーを押します。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   次の図は、有効になっている Api を示します。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Google Api API マネージャーを参照してください、**資格情報**を取得するには、タブ、**クライアント ID**します。 ダウンロードすると、アプリケーション シークレットの JSON ファイルを保存します。 コピーして貼り付け、 **ClientId**と**ClientSecret**に、`UseGoogleAuthentication`メソッド、 *Startup.Auth.cs*ファイル、 *App_Start*フォルダー。 **ClientId**と**ClientSecret**の下に表示される値のサンプルし、動作しません。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データは store ことはありません。 アカウントと資格情報は、サンプルを単純に上記のコードに追加されます。 参照してください[ASP.NET と Azure App Service にパスワードやその他の機密データを展開するためのベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)します。
8. キーを押して**CTRL + F5**をビルドして、アプリケーションを実行します。 をクリックして、**ログイン**リンク。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. [**別のサービスを使用してログイン**、] をクリックして**Google**します。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > 上記の手順のいずれかがない場合は、HTTP 401 エラーが表示されます。 上記の手順を再確認します。 必要な設定がない場合 (たとえば**製品名**) は、不足している項目を追加し、保存、認証が機能するのに数分かかることができます。
10. 資格情報を入力は Google のサイトにリダイレクトされます。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. 資格情報を入力した後は、作成した web アプリケーションへのアクセス許可を付与が求められます。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. クリックして**受け入れる**します。 今すぐにリダイレクトされます、**登録**Google アカウントを登録する MvcAuth アプリケーションのページ。 Gmail アカウントに使用されるローカルの電子メール登録名を変更するオプションがありますが、通常、既定の電子メール エイリアス (認証に使用するもの) を保持したいです。 **[登録]** をクリックします。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Facebook でアプリを作成し、アプリ プロジェクトを接続します。

> [!WARNING]
> 現在の Facebook OAuth2 認証の手順では、次を参照してください[構成の Facebook 認証。](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>メンバーシップ データを調べる

**ビュー**  メニューのをクリックして**サーバー エクスプ ローラー**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

展開**DefaultConnection (MvcAuth)**、展開**テーブル**を右クリックして**AspNetUsers**クリック**テーブル データの表示**。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers テーブル データ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>プロファイル データをユーザー クラスに追加します。

このセクションで追加します生年月日とホーム町のユーザー データを登録時に、次の図のようにします。

![出身地と Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

開く、 *Models\IdentityModels.cs*ファイルを開き生年月日の日付とホーム町のプロパティを追加します。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

開く、 *Models\AccountViewModels.cs*ファイルと、セットで日付とホームの町プロパティを birth`ExternalLoginConfirmationViewModel`します。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

開く、 *controllers \accountcontroller.cs*生年月日の日付とホーム町でのコードを追加するファイルを開き、`ExternalLoginConfirmation`に示すように、アクション メソッド。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

追加の生年月日とホームに町に来ました、 *Views\Account\ExternalLoginConfirmation.cshtml*ファイル。

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

もう一度アプリケーションを自分の Facebook アカウントを登録し、新しい生年月日とホーム町プロファイル情報を追加することを確認できるように、メンバーシップ データベースを削除します。

**ソリューション エクスプ ローラー**、クリックして、 **すべてのファイル**アイコン、し、右クリック*追加\_Data\aspnet-MvcAuth -&lt;日付スタンプ&gt;.mdf*クリック**削除**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**(PMC)。 PMC で、次のコマンドを入力します。

1. Enable-migrations
2. Add-migration Init
3. データベースの更新

アプリケーションを実行し、FaceBook、Google を使用してにログインし、一部のユーザーを登録します。

## <a name="examine-the-membership-data"></a>メンバーシップ データを調べる

**ビュー**  メニューのをクリックして**サーバー エクスプ ローラー**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

右クリックして**AspNetUsers**クリック**テーブル データの表示**します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown`と`BirthDate`フィールドを以下に示します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>アプリからログオフして、別のアカウントでのログ記録

Facebook、およびを使用してアプリにログオンして、ログアウトするとログインしようとしています。 もう一度 (同じブラウザーを使用)、別の Facebook アカウントですぐにログインされます以前に使用した Facebook アカウントにします。 別のアカウントを使用するには、Facebook に移動し、Facebook にログアウトする必要があります。 その他のサード パーティの認証プロバイダーに同じ規則が適用されます。 または、別のブラウザーを使用して別のアカウントを使用してログインすることができます。

## <a name="next-steps"></a>次の手順

参照してください[owin Yahoo と LinkedIn OAuth のセキュリティ プロバイダーの概要](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)Jerrie Pelser Yahoo と LinkedIn 手順については、します。 Jerrie を参照してください。 有効にするソーシャル ログイン ボタンを取得する ASP.NET MVC 5 用ソーシャル ログイン ボタン。

拙著のチュートリアルに従ってください[auth と SQL DB を使って、ASP.NET MVC アプリを作成し、Azure App Service にデプロイ](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)、このチュートリアルを続行して、次に示します。

1. アプリを Azure にデプロイする方法。
2. ロールを使用したアプリをセキュリティで保護する方法。
3. 使用してアプリをセキュリティで保護する方法、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)と[Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)フィルター。
4. メンバーシップ API を使用して、ユーザーとロールを追加する方法。

このチュートリアルの立った方法で改善できましたフィードバックを送信してください。 また、新しいトピックを要求することもできます。[表示 Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。 要求し、ASP.NET に追加する新しい機能に投票できます。 ツールを投票するなど、[の作成し、ユーザーとロールを管理します。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET の外部認証サービスのしくみの良いについては、Robert McMurray を参照してください。[外部認証サービス](https://asp.net/web-api/overview/security/external-authentication-services)します。 Robert の記事では、Microsoft、Twitter の認証の有効化の詳細にもなります。 Tom Dykstra の優れた[EF と MVC チュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Entity Framework を使用する方法を示しています。
