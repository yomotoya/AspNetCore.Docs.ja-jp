---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトの ASP.NET Web 内の外部のサイトを使用してログイン |Microsoft ドキュメント
author: tfitzmac
description: この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して、ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サポートする方法.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530171"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web Pages (Razor) サイトの外部のサイトを使用してログイン
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して、ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サイトで OAuth および OpenID をサポートする方法です。
> 
> 学習する内容。
> 
> - WebMatrix のスターター サイト テンプレートを使用するときに、他のサイトからのログインを有効にする方法です。
> 
> これは、アーティクルで導入された ASP.NET の機能です。
> 
> - `OAuthWebSecurity`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web Pages にはサポートが含まれています[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。 これらのプロバイダーを使用して、Facebook、Twitter、Microsoft と Google からの既存の資格情報を使用して、サイトにユーザーがログインをさせることができます。 たとえば、Facebook アカウントを使用してログイン、ユーザーだけを選択できます Facebook アイコンには、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。 Facebook ログインをサイトで各自のアカウント、関連付けることができますし、します。 Web ページのメンバーシップ機能に関連する拡張機能です。 ユーザーが (ソーシャル ネットワー キング サイトからのログインを含む) 複数のログインに関連付けることができる web サイトで 1 つのアカウント。

この図では、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、Google、または Microsoft のアイコンを選択できます。

![外部プロバイダー](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

数行のコードのコメントを解除して OAuth および OpenID のメンバーシップを有効にすることができます、**スターター サイト**テンプレート。 メソッドとプロパティを使用する作業で、OAuth および OpenID プロバイダーでは、`WebMatrix.Security.OAuthWebSecurity`クラスです。 **スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースのすべてのコードと完了する必要がローカルの資格情報または別のサイトからこれらを使用してサイトにユーザーがログインを使用できます.

このセクションでは、外部サイトから基になっているサイトにログインできるようにする方法の例を示します、**スターター サイト**テンプレート。 スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。

- OAuth プロバイダー (Facebook、Twitter、および Microsoft) を使用しているサイトでは、外部のサイトにアプリケーションを作成します。 これにより、それらのサイトのログインの機能を実行するために必要となるアプリケーション キー。
- OpenID プロバイダー (Google) を使用するサイトでは、アプリケーションを作成するはありません。 ログインするために、開発者向けアプリケーションを作成して、これらのサイトのすべてのアカウントが必要です。

    > [!NOTE]
    > Microsoft アプリケーションでは、作業用の web サイトの URL をライブのみ承認し、ログインをテストするため、ローカルの web サイトの URL を使用することはできません。
- 適切な認証プロバイダーを指定するために、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。

この記事では、次のタスクを個別の手順では。

- [Google ログインを有効にします。](#To_enable_Google_logins)
- [Facebook ログインを有効にします。](#To_enable_Facebook_logins)
- [Twitter ログインを有効にします。](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Google ログインを有効にします。

1. 作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページし、次のコード行のコメントを解除します。 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Google ログインのテスト

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタン。 この例では、Google ログインを使用します。 

    Web ページは、Google ログイン ページに、要求をリダイレクトします。

    ![Google のサインイン](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. 既存の Google アカウントの資格情報を入力します。
4. Google は、許可するかどうかを求める場合*Localhost*情報を使用して、アカウントから、をクリックして**許可**です。

    コードでは、Google トークンを使用して、ユーザーを認証し、web サイトでこのページに返します。 このページでは、Google ログインを web サイト上の既存のアカウントに関連付けるユーザーまたは新しいアカウントに外部ログインとを関連付けるには、サイトに登録します。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. 選択、**関連付ける**ボタンをクリックします。 ブラウザーは、アプリケーションのホーム ページに戻ります。

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Facebook ログインを有効にします。

1. 移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(ログインされていないログインしている場合)。
2. 選択、 **Create New App**ボタンをクリックし、指示に従って名前を指定し、新しいアプリケーションを作成します。
3. セクションで**アプリを Facebook と統合する方法を選択**、選択、 **web サイト**セクションです。
4. 入力、**サイトの URL**フィールドをサイトの URL (たとえば、 `http://www.example.com`)。 **ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。 

    > [!NOTE]
    > URL を使用して、ローカル コンピューターにサイトを実行している場合と同様に`http://localhost:12345`(数は、ローカル ポート番号)、するには、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールドです。 ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイト URL**アプリケーションのフィールドです。
5. 選択、 **Save Changes**ボタンをクリックします。
6. 選択、**アプリ**もう一度、タブし、アプリケーションのスタート ページを表示します。
7. コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードで Facebook プロバイダーに渡されます。
8. Facebook の開発者向けサイトを終了します。

今すぐを変更する 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用して、サイトにログインすることです。

1. 作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページおよび Facebook OAuth プロバイダーのコードのコメントを解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、 `appId` (引用符を除く) のパラメーターです。
4. コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`appSecret`パラメーターの値。
5. ファイルを保存して閉じます。

### <a name="testing-facebook-login"></a>Facebook ログインをテストします。

1. サイトの実行*default.cshtml*ページし、選択、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Facebook**アイコン。 

    Web ページは、Facebook のログイン ページに、要求をリダイレクトします。

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Facebook アカウントにログインします。 

    コードでは、Facebook トークンを使用してユーザーを認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますページに返します。 ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールドです。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーのホーム ページに返しに記録されます。

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Twitter ログインを有効にします。

1. 参照、 [Twitter 開発者サイト](https://dev.twitter.com/)です。
2. 選択、**アプリの作成**リンクし、サイトにログインします。
3. **アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールドです。
4. **Web サイト**フィールドに、サイトの URL を入力してください (たとえば、 `http://www.example.com`)。 

    > [!NOTE]
    > ローカル サイトをテストする場合は (のように URL を使用して`http://localhost:12345`)、Twitter、URL を受け付けないことがあります。 ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。 これには、アプリケーションをローカルでのテストのプロセスが簡略化します。 ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新、 **web サイト**アプリケーションのフィールドです。
5. **コールバック URL**フィールドに、ユーザーが Twitter にログインした後に返される web サイト内のページの URL を入力します。 たとえば、ユーザーを (それらのログインの状態が認識されます) をスターター サイトのホーム ページに送信 を同じ URL を入力に入力した、 **web サイト**フィールドです。
6. 条項に同意し、選択、 **、Twitter アプリケーションを作成**ボタンをクリックします。
7. **マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。
8. **詳細** タブで、一番下までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。
9. **詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値し、一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードで Twitter プロバイダーに渡すします。
10. Twitter のサイトを終了します。

今すぐを変更する 2 つのページ、web サイトのユーザーが自分の Twitter アカウントを使用して、サイトにログインできるようにします。

1. 作成するか、WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードのコメントを解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. コピー、**コンシューマー キー**の値として Twitter アプリケーションからの値、 `consumerKey` (引用符を除く) のパラメーターです。
4. コピー、**コンシューマー シークレット**の値として Twitter アプリケーションからの値、`consumerSecret`パラメーター。
5. ファイルを保存して閉じます。

### <a name="testing-twitter-login"></a>Twitter ログインのテスト

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**のログインに別のサービスを使用して**セクションで、選択、 **Twitter**アイコン。 

    Web ページは、作成したアプリケーションの Twitter ログイン ページに、要求をリダイレクトします。

    ![oauth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Twitter アカウントにログインします。
4. コードは、Twitter のトークンを使用してユーザーを認証し、返すページに関連付けることができます、web サイトのアカウントでログイン。 名前や電子メール アドレスを格納、**電子メール**フォームのフィールドです。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーのホーム ページに返しに記録されます。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [サイト全体の動作をカスタマイズします。](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ASP.NET Web Pages サイトへのセキュリティとメンバーシップの追加](https://go.microsoft.com/fwlink/?LinkID=202904)
