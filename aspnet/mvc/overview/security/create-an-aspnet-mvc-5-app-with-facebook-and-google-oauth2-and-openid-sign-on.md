---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 作成する MVC 5 アプリを Facebook、Twitter、LinkedIn および Google OAuth2 サイン オン (c#) |Microsoft ドキュメント
author: Rick-Anderson
description: このチュートリアルでは、OAuth 2.0 を使用して認証を使用する外部からの資格情報でログインできるようにする ASP.NET MVC 5 web アプリケーションをビルドする方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: aa4c91865f7b720846a5e8deb4281c3ca6933c8e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819098"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Facebook、Twitter、LinkedIn および Google OAuth2 サイン オン (c#) で ASP.NET MVC 5 アプリを作成します。
====================
によって[Rick Anderson](https://github.com/Rick-Anderson)

> このチュートリアルを使用してユーザーにログインできるようにする ASP.NET MVC 5 web アプリケーションを構築する方法を示します[OAuth 2.0](http://oauth.net/2/) Facebook、Twitter、LinkedIn、Microsoft、Google など、外部認証プロバイダーからの資格情報を使用します。 わかりやすくするため、このチュートリアルは、Facebook、Google からの資格情報の操作について説明します。
> 
> 数百万のユーザーでは、これらの外部プロバイダーを持つアカウントが既にあるために、大きな利点を提供して web サイトでこれらの資格情報を有効にします。 これらのユーザーを作成し、一連の資格情報を覚えていない場合、サイトにサインアップする傾向があります。
> 
> 関連項目[SMS と 2 要素認証の電子メールを使って ASP.NET MVC 5 アプリ](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)です。
> 
> このチュートリアルでは、メンバーシップ API を使用してロールを追加する方法と、ユーザーのプロファイル データを追加する方法も示します。 このチュートリアルは、によって書き込まれました[Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter me に従って操作してください: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。


<a id="start"></a>
## <a name="getting-started"></a>作業の開始

インストールと実行によって開始[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。 Visual Studio のインストール[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)またはそれ以降。 Dropbox、GitHub、Linkedin、Instagram、バッファー、Salesforce、ストリーム、スタック Exchange、Tripit、Twitch、Twitter、yahoo!、および詳細については、この参照してください。[サンプル プロジェクト](https://github.com/matthewdunsdon/oauthforaspnet)です。

> [!NOTE]
> Visual Studio をインストールする必要があります[2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)以上 Google OAuth 2 を使用して、SSL の警告せずにローカルでデバッグします。


をクリックして**新しいプロジェクト**から、**開始** ページで、またはメニューを使用して選択、**ファイル**、し**新しいプロジェクト**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>初めてアプリケーションの作成

をクリックして**新しいプロジェクト**選択してから、 **Visual c#** し、左側の**Web**し、 **ASP.NET Web アプリケーション**です。 プロジェクト"MvcAuth"の名前を指定し、をクリックして**OK**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、をクリックして**MVC**です。 認証がない場合**個々 のユーザー アカウント**をクリックして、**認証の変更**ボタンをクリックして**個々 のユーザー アカウント**です。 チェックして**クラウド内のホスト**アプリが非常に簡単に Azure でホストされます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

選択した場合は**クラウド内のホスト**構成 ダイアログを完了します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>NuGet を使用して、最新の OWIN ミドルウェアを更新

更新する NuGet パッケージ マネージャーを使用して、 [OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)です。 選択**更新**左側のメニュー。 クリックすると、**すべて更新**(の次の図に示す) OWIN パッケージのみのボタンをクリックするかを検索できます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

次の図では、OWIN パッケージのみが表示されます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

入力したパッケージ マネージャー コンソール (PMC) から、`Update-Package`コマンドでは、すべてのパッケージが更新されます。

キーを押して**f5 キーを押して**または**ctrl キーを押しながら f5 キーを押して**アプリケーションを実行します。 次の図では、ポート番号は、1234 です。 アプリケーションを実行するときに、別のポート番号が表示されます。

ブラウザー ウィンドウのサイズに応じて、ナビゲーション アイコンを表示する をクリックする必要があります、**ホーム**、**に関する**、**連絡先**、 **登録**と**ログイン**リンクします。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>プロジェクトで SSL を設定します。

Google、Facebook などの認証プロバイダーに接続するには、SSL を使用する IIS Express を設定する必要があります。 ユーザー名とパスワードと、ネットワーク経由でクリア テキストで送信する SSL を使用せずに、シークレットと同様、ログイン cookie が、引き続きログイン後に SSL を使用して、HTTP にバックアップを削除しないに重要です。 ハンドシェイクを実行し、(であるため、HTTPS HTTP よりも低速の一括) チャネルをセキュリティで保護する時間を既に撮影する以外にも、MVC パイプラインの実行前にログインしているために HTTP リダイレクトする行いません将来、現在の要求要求がはるかに高速です。

1. **ソリューション エクスプ ローラー**をクリックして、 **MvcAuth**プロジェクト。
2. プロジェクトのプロパティを表示する F4 キーを押します。 またから、**ビュー**メニューを選択できます**プロパティ ウィンドウ**します。
3. 変更**SSL を有効に**True に設定します。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. SSL URL のコピー (されます`https://localhost:44300/`SSL の他のプロジェクトを作成した場合を除く)。
5. **ソリューション エクスプ ローラー**を右クリックして、 **MvcAuth**プロジェクトし、選択**プロパティ**です。
6. 選択、 **Web**タブをクリックしに SSL URL を貼り付け、**プロジェクト Url**ボックス。 (Ctl + S) ファイルを保存します。 この URL は、Facebook、Google の認証アプリを構成する必要があります。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. 追加、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)属性を`Home`コント ローラーのすべての要求を要求するのには、HTTPS を使用する必要があります。 安全な方法は、追加する、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)アプリケーションをフィルターします。 セクションを参照して&quot;SSL および承認属性を持つアプリケーションを保護する&quot;my tutoral で[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)です。 Home コント ローラーの一部を次に示します。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 過去の証明書をインストールしている場合は、このセクションの残りの部分をスキップし、ジャンプする[OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続する](#goog)、それ以外の場合、手順については、自己署名を信頼するにはIIS Express が生成した証明書です。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. 読み取り、**セキュリティ警告**クリックしてダイアログ**はい**を表す localhost 証明書をインストールする場合。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE を示しています、*ホーム*ページし、SSL の警告はありません。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome も証明書を受け入れるし、警告を表示せずに HTTPS コンテンツが表示されます。 警告に表示されるように、Firefox は独自の証明書ストアを使用します。 アプリケーションを安全にクリックして、**リスクを理解しました**です。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>OAuth 2 では、Google のアプリを作成して、プロジェクトに、アプリを接続します。

> [!WARNING]
> 現在の Google OAuth 手順では、次を参照してください。 [ASP.NET Core の構成の Google 認証](/aspnet/core/security/authentication/social/google-logins)です。

1. 移動し、 [Google Developers Console](https://console.developers.google.com/)です。
2. 前にプロジェクトを作成していない場合は、選択**資格情報**クリックし、左側のタブで**作成**です。
3. 左側のタブをクリックして**資格情報**です。
4. をクリックして**資格情報を作成する**し**OAuth クライアント ID**です。 

    1. **クライアント ID を作成**ダイアログ ボックスで、既定値を保持**Web アプリケーション**アプリケーションの種類。
    2. 設定、**承認 JavaScript** SSL URL が上記で使用する配信元 (`https://localhost:44300/` SSL の他のプロジェクトを作成した場合を除く)
    3. 設定、**承認されているリダイレクト URI**に。  
         `https://localhost:44300/signin-google`
5. OAuth 同意画面のメニュー項目をクリックし、電子メール アドレスと製品名を設定します。 フォームのクリックを完了しているとき**保存**です。
6. ライブラリのメニュー項目をクリックすると、検索**Google + API**、それをクリックし、有効にするキーを押します。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   次の図は、有効になっている Api を示します。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Google Api API マネージャーからアクセスして、**資格情報**を取得するには、タブ、**クライアント ID**です。 ダウンロードすると、アプリケーション シークレットの JSON ファイルを保存します。 コピーし、貼り付け、 **ClientId**と**ClientSecret**に、`UseGoogleAuthentication`については、メソッド、 *Startup.Auth.cs*ファイルを*App_Start*フォルダーです。 **ClientId**と**ClientSecret**の下に表示される値のサンプルし、動作しません。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > セキュリティ - ソース コード内の機密データはストアことはありません。 アカウントと資格情報は、サンプルをシンプルにする上記のコードに追加されます。 参照してください[ASP.NET と Azure App Service へのパスワードなどの機密データの展開のベスト プラクティス](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)です。
8. キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションをビルドして実行します。 クリックして、**ログイン**リンクします。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. 下にある**のログインに別のサービスを使用して**をクリックして**Google**です。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > 上記の手順のいずれかがない場合は、HTTP 401 エラーが表示されます。 上記の手順を再確認します。 必要な設定がない場合 (たとえば**製品名**)、不足している項目を追加し、保存、以外の場合は認証が機能するまで、しばらく時間がかかることができます。
10. 資格情報を入力する Google サイトにリダイレクトされます。   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. 資格情報を入力すると後、は、作成した web アプリケーションへのアクセス許可を付与するよう求められます。
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. をクリックして**受け入れる**です。 今すぐにリダイレクトされます、**登録**Google アカウントを登録する MvcAuth アプリケーションのページです。 Gmail アカウントで使用するローカル電子メール登録名を変更することがあるが、通常、既定の電子メール エイリアス (つまり、認証に使用するもの) を保持します。 **[登録]** をクリックします。  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Facebook でアプリを作成し、プロジェクトに、アプリを接続します。

> [!WARNING]
> 現在の Facebook の OAuth2 認証手順では、次を参照してください[を構成する Facebook 認証。](/aspnet/core/security/authentication/social/facebook-logins)

Facebook の OAuth2 認証では、Facebook で作成したアプリケーションから一部の設定をプロジェクトにコピーする必要があります。

1. ブラウザーでに移動します[ https://developers.facebook.com/apps ](https://developers.facebook.com/apps)して Facebook 資格情報を入力してログインします。
2. 既に、Facebook 開発者として登録されていない場合にクリックして**開発者として登録**し登録するための指示に従います。
3. **アプリ** タブで、をクリックして**Create New App**です。

    ![新しいアプリを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. 入力、**アプリ名**と**カテゴリ**、順にクリックして**のアプリの作成**です。

    <strong>アプリ Namespace</strong>アプリが認証に Facebook アプリケーションへのアクセスに使用する URL の一部である (たとえば https\://apps.facebook.com/{App Namespace})。 指定しない場合は、<strong>アプリ Namespace</strong>、<strong>アプリ ID</strong> URL に使用されます。 <strong>アプリ ID</strong>はシステムによって生成された時間の数が次の手順で表示されます。

    ![新しいアプリ ダイアログを作成します。](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. 標準的なセキュリティ チェックを送信します。

    ![セキュリティ チェック](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. 選択**設定**左側のメニュー バーの![Facebook 開発者のメニュー バー](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. **基本**ページの設定 セクションを選択**プラットフォームを追加**を web サイト アプリケーションを追加することを指定します。 ![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. 選択**web サイト**のプラットフォームのいずれか。  
  
    ![プラットフォームの選択肢](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. メモ、**アプリ ID**と**アプリ シークレット**できるように、このチュートリアルで後ほど、MVC アプリケーションに両方を追加することができます。 また、サイトの URL を追加 (`https://localhost:44300/`)、MVC アプリケーションをテストします。 また、追加、 **Contact Email**です。 次に、選択**Save Changes**です。   

    ![基本的なアプリケーションの詳細 ページ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > 登録されている電子メールのエイリアスを使用して認証だけあることに注意してください。 その他のユーザーとテスト用のアカウントを登録することができなきます。 Facebook のアプリケーションにその他の Facebook アカウントのアクセスを許可できます**開発者ロール**タブです。
10. Visual Studio で開く*アプリ\_Start\Startup.Auth.cs*です。
11. コピーし、貼り付け、 **AppId**と**アプリ シークレット**に、`UseFacebookAuthentication`メソッドです。 **AppId**と**アプリ シークレット**の下に表示される値のサンプルし、は機能しません。

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. をクリックして**変更を保存**です。
13. キーを押して**ctrl キーを押しながら f5 キーを押して**アプリケーションを実行します。


選択**ログイン**ログイン ページを表示します。 をクリックして**Facebook** **のログインに別のサービスを使用します。**

Facebook 資格情報を入力します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

パブリック プロファイルと友人リストにアクセスするためにアプリケーションのアクセス許可を付与するように促されます。

![Facebook アプリの詳細](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

ログインしているようになりました。 アプリケーションでこのアカウントを登録できます。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

エントリを追加登録するときに、*ユーザー*メンバーシップ データベースのテーブルです。

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>メンバーシップ データを調べる

**ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

展開**DefaultConnection (MvcAuth)**、展開**テーブル**を右クリックして**AspNetUsers**  をクリック**テーブル データの表示**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers テーブルのデータ](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>プロファイル データ、ユーザー クラスを追加します。

このセクションで追加の生年月日とホーム町のユーザー データを登録の際に次の図のようにします。

![ホーム町と Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

開く、 *Models\IdentityModels.cs*ファイル birth date とホーム町のプロパティを追加します。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

開く、 *Models\AccountViewModels.cs*ファイルとセット birth で日付とホームの町プロパティ`ExternalLoginConfirmationViewModel`です。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

開く、 *Controllers\AccountController.cs*ファイルし、の生年月日の日付とホーム町にコードを追加、`ExternalLoginConfirmation`ようにアクション メソッド。

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

生年月日と出身地に追加、 *Views\Account\ExternalLoginConfirmation.cshtml*ファイル。

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

もう一度アプリケーションを使用して、Facebook アカウントを登録し、新しい生年月日とホーム町プロファイル情報を追加することを確認できるように、メンバーシップ データベースを削除します。

**ソリューション エクスプ ローラー**をクリックして、 **[すべてのファイル**アイコン、し、右クリック*追加\_Data\aspnet-MvcAuth -&lt;日付スタンプ&gt;.mdf* ] をクリック**削除**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

**ツール** メニューのをクリックして**NuGet パッケージ マネージャー**をクリックし、 **Package Manager Console** (PMC)。 PMC に次のコマンドを入力します。

1. 有効な移行
2. 追加の移行の初期化
3. データベースの更新

アプリケーションを実行し、FaceBook、Google を使用してログインし、一部のユーザーを登録します。

## <a name="examine-the-membership-data"></a>メンバーシップ データを調べる

**ビュー**  メニューをクリックして**サーバー エクスプ ローラー**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

右クリックして**AspNetUsers**  をクリック**テーブル データの表示**です。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown`と`BirthDate`フィールドを以下に示します。

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>アプリからログオフして、別のアカウントでのログ記録

Facebook、およびでのアプリへのログオンをログアウトしとにログインしようとしています。 もう一度 (同じブラウザーを使用)、別の Facebook アカウントではログインする必要がすぐに使用した前の Facebook アカウントにします。 別のアカウントを使用するのには、Facebook に移動し、Facebook にログアウトする必要があります。 その他のサード パーティ認証プロバイダーに、同じ規則が適用されます。 別のブラウザーを使用して、別のアカウントでログインすることができます。

## <a name="next-steps"></a>次の手順

参照してください[OWIN の Yahoo および LinkedIn OAuth のセキュリティ プロバイダーの導入](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)Jerrie Pelser Yahoo および LinkedIn の手順でします。 Jerrie を参照してください。 非常に有効にするソーシャル ログイン ボタンを取得する ASP.NET MVC 5 のソーシャル ログイン ボタン。

チュートリアルに従って[Azure App Service に認証と SQL DB の ASP.NET MVC アプリの作成し、展開](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)、このチュートリアルを続行して、次に示します。

1. アプリを Azure に配置する方法。
2. 役割を持つアプリをセキュリティで保護する方法。
3. 使用してアプリを保護する方法、 [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)と[Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)フィルター。
4. メンバーシップ API を使用して、ユーザーおよびロールを追加する方法。

このチュートリアルをリンクする方法と、何を改善にフィードバックを送信してください。 新しいトピックを要求することもできます。 [Me 方法でコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)です。 要求し、ASP.NET に追加する新しい機能に投票もできます。 などのツールには、返信できます[を作成し、ユーザーおよびロールを管理します。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

ASP.NET の外部認証サービスの動作の適切な詳細については、Robert McMurray を参照してください。[外部認証サービス](https://asp.net/web-api/overview/security/external-authentication-services)です。 Robert の記事は、Microsoft および Twitter 認証を有効にする方法の詳細にも移動します。 Tom Dykstra の優れた[EF と MVC チュートリアル](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)Entity Framework を使用する方法を示します。
