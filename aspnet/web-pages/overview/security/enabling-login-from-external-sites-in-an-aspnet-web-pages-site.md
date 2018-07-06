---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: ページ (Razor) サイトの外部の ASP.NET web サイトを使用してログ記録 |Microsoft Docs
author: tfitzmac
description: この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サポートする方法.
ms.author: aspnetcontent
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: e6c5b578b4c74fd04136d31751d51b59ba9197ba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825438"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>ASP.NET Web ページ (Razor) サイトで外部のサイトを使用したログイン
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> この記事は、Facebook、Google、Twitter、Yahoo、およびその他のサイトを使用して ASP.NET Web Pages (Razor) サイトにログインする方法を説明します-つまり、サイトの OAuth および OpenID をサポートする方法。
> 
> 学習内容。
> 
> - WebMatrix のスターター サイト テンプレートを使用する場合は、他のサイトからのログインを有効にする方法。
> 
> これは、この記事で導入された ASP.NET の機能です。
> 
> - `OAuthWebSecurity`ヘルパー。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

ASP.NET Web Pages にはサポートが含まれています[OAuth](http://oauth.net/)と[OpenID](http://openid.net/)プロバイダー。 これらのプロバイダーを使用して、Facebook、Twitter、Microsoft、および Google から既存の資格情報を使用して、サイトに、ユーザーがログインをさせることができます。 たとえば、Facebook アカウントを使用してログイン、ユーザーことができます選択 Facebook アイコンは、各自のユーザー情報を入力する Facebook ログイン ページにリダイレクトします。 サイトでユーザーのアカウント、Facebook ログインに関連付けることができます。 Web ページのメンバーシップ機能に関連する拡張機能は、web サイトの 1 つのアカウントを持つユーザーが複数回のログイン (ソーシャル ネットワーク サイトからのログインを含む) を関連付けることができます。

この図は、ログイン ページから、**スターター サイト**テンプレート、ユーザーが外部のアカウントでログインを有効にする、Facebook、Twitter、Google、Microsoft のアイコンを選択できます。

![外部プロバイダー](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

数行のコードのコメントを解除して OAuth と OpenID のメンバーシップを有効にすることができます、**スターター サイト**テンプレート。 OAuth を使用するように使用するメソッドとプロパティおよび OpenID プロバイダーはで、`WebMatrix.Security.OAuthWebSecurity`クラス。 **スターター サイト**テンプレートには、完全なメンバーシップ インフラストラクチャが含まれています、ログイン ページ、メンバーシップ データベースでは、すべてのコードと完了する必要がありますをローカルの資格情報または別のサイトからこれらのいずれかを使用してサイトに、ユーザーがログインできるようにするには.

このセクションがユーザーに基づいているサイト外部のサイトからにログインできるようにする方法の例では、**スターター サイト**テンプレート。 スターター サイトを作成すると、この (詳細のフォロー) する操作を行います。

- OAuth プロバイダー (Facebook、Twitter、および Microsoft) を使用するサイトでは、外部のサイトにアプリケーションを作成します。 これにより、それらのサイトのログイン機能を起動するために必要となるアプリケーションのキー。
- OpenID プロバイダー (Google) を使用して、サイトでは、アプリケーションを作成する必要はありません。 これらのサイトのすべてにログインするために、開発者のアプリケーションを作成して、アカウントが必要です。

    > [!NOTE]
    > Microsoft アプリケーションでは、ログインをテストするため、ローカルの web サイトの URL を使用することはできませんので、作業用の web サイトのライブの URL のみ受け入れます。
- 適切な認証プロバイダーを指定するには、web サイトのいくつかのファイルを編集し、使用すると、サイトへのログインを送信します。

この記事では、次のタスクを個別の手順では。

- [Google ログインを有効にします。](#To_enable_Google_logins)
- [Facebook ログインを有効にします。](#To_enable_Facebook_logins)
- [Twitter ログインを有効にします。](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Google ログインを有効にします。

1. 作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページし、次のコード行をコメント解除します。 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Google ログインをテストします。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*] ページの [、**別のサービスを使用してログイン**セクションで、いずれかを選択、 **Google**または**Yahoo**送信ボタンをクリックします。 この例では、Google ログインを使用します。 

    Web ページは、Google ログイン ページに、要求をリダイレクトします。

    ![Google のサインイン](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. 既存の Google アカウントの資格情報を入力します。
4. Google は、許可するかどうかを確認する場合*Localhost* 、アカウントから情報を使用する をクリックして**許可**します。

    コードでは、Google トークンを使用して、ユーザーを認証して、web サイトにこのページに戻ります。 このページでは、Google ログインを web サイト、既存のアカウントに関連付けるユーザーまたはで外部ログインに関連付けるサイトで新しいアカウントを登録することができます。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. 選択、**関連付ける**ボタンをクリックします。 ブラウザーは、アプリケーションのホーム ページを返します。

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Facebook ログインを有効にします。

1. 移動して、 [Facebook 開発者サイト](https://developers.facebook.com/apps)(まだログインしている場合ログ記録)。
2. 選択、 **Create New App**ボタンをクリックし、新しいアプリケーションを作成して名前を指示に従います。
3. セクションで**アプリを Facebook と統合する方法を選択します。**、選択、 **web サイト**セクション。
4. 入力、**サイトの URL**フィールドに、サイトの URL (たとえば、 `http://www.example.com`)。 **ドメイン**フィールドはオプションです。 これを使用して、ドメイン全体の認証を提供することができます (など*example.com*)。 

    > [!NOTE]
    > などの URL を使用してローカル コンピューターにサイトを実行している場合`http://localhost:12345`(番号が、ローカル ポート番号)、この値を追加することができます、**サイトの URL**サイトのテストに対応するフィールド。 ただし、いつでも、ローカル サイトの変更のポート番号、更新する必要があります、**サイトの URL**アプリケーションのフィールド。
5. 選択、**変更の保存**ボタンをクリックします。
6. 選択、**アプリ**タブをもう一度、し、アプリケーションのスタート ページを表示します。
7. コピー、**アプリ ID**と**アプリ シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。 これらの値は、web サイトのコードに Facebook プロバイダーに渡されます。
8. Facebook の開発者向けサイトを終了します。

今すぐに変更を加える 2 つのページ、web サイトのユーザーができるように自分の Facebook アカウントを使用してサイトにログインすること。

1. 作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページし、Facebook OAuth プロバイダーのコードをコメント解除します。 コメント解除されたコード ブロックは、次のようになります。 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. コピー、**アプリ ID**の値としての Facebook アプリケーションからの値、`appId`パラメーター (引用符を除く)。
4. コピー**アプリ シークレット**としての Facebook アプリケーションからの値、`appSecret`パラメーターの値。
5. ファイルを保存して閉じます。

### <a name="testing-facebook-login"></a>Facebook ログインをテストします。

1. サイトの実行*default.cshtml*ページ、**ログイン**ボタンをクリックします。
2. *ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Facebook**アイコン。 

    Web ページは、Facebook のログイン ページに、要求をリダイレクトします。

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Facebook アカウントにログインします。 

    コードでは、Facebook トークンを使用して、認証し、サイトのログインを持つ、Facebook ログインを関連付けることができますをページに返します。 ユーザー名または電子メール アドレスを格納、**電子メール**フォームのフィールド。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーがホーム ページに戻るしに記録されます。

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Twitter ログインを有効にします。

1. 参照、 [Twitter 開発者サイト](https://dev.twitter.com/)します。
2. 選択、**アプリを作成する**リンクし、サイトにログインします。
3. **アプリケーションを作成する**フォームで、入力、**名前**と**説明**フィールド。
4. **Web サイト**フィールドに、サイトの URL を入力します (たとえば、 `http://www.example.com`)。 

    > [!NOTE]
    > サイトをローカルでテストする場合 (ような URL を使用して`http://localhost:12345`)、Twitter の URL を受け付けないことがあります。 ただし、するローカル ループバック IP アドレスを使用することがあります (たとえば`http://127.0.0.1:12345`)。 これには、アプリケーションをローカルでのテストのプロセスが簡略化します。 ただし、ローカル サイトのポート番号が変更されるたびにする必要がありますを更新する、 **web サイト**アプリケーションのフィールド。
5. **コールバック URL**フィールドに、ユーザーが Twitter にログインした後に戻り、web サイトのページの URL を入力します。 たとえば、スターター サイト (これは、ログイン ステータスが認識されます) のホーム ページにユーザーを送信するで入力したのと同じ URL を入力、 **web サイト**フィールド。
6. 条項に同意し、選択、 **Twitter アプリケーションを作成**ボタンをクリックします。
7. **マイ アプリケーション**ランディング ページで、作成したアプリケーションを選択します。
8. **詳細** タブで、下部までスクロールし、選択、**個人用アクセス トークンを作成**ボタンをクリックします。
9. **詳細** タブで、コピー、**コンシューマー キー**と**コンシューマー シークレット**アプリケーションの値を一時的なテキスト ファイルに貼り付けます。 Web サイトのコードで、Twitter プロバイダーにこれらの値を渡すします。
10. Twitter サイトを終了します。

今すぐに変更を加える 2 つのページ、web サイトでユーザーが自分の Twitter アカウントを使用してサイトにログインできるようにします。

1. 作成または WebMatrix のスターター サイト テンプレートに基づいている ASP.NET Web Pages サイトを開きます。
2. 開く、  *\_AppStart.cshtml*ページし、Twitter OAuth プロバイダーのコードをコメント解除します。 コメント解除されたコード ブロックのようになります。 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. コピー、**コンシューマー キー**の値としての Twitter アプリケーションからの値、`consumerKey`パラメーター (引用符を除く)。
4. コピー、**コンシューマー シークレット**の値としての Twitter アプリケーションからの値、`consumerSecret`パラメーター。
5. ファイルを保存して閉じます。

### <a name="testing-twitter-login"></a>Twitter ログインをテストします。

1. 実行、 *default.cshtml*サイトのページを選択し、**ログイン**ボタンをクリックします。
2. *ログイン*ページで、**別のサービスを使用してログイン**セクションで、選択、 **Twitter**アイコン。 

    Web ページは、要求を作成したアプリケーションの Twitter ログイン ページにリダイレクトします。

    ![oauth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Twitter アカウントにログインします。
4. コードは、ユーザーの認証に Twitter のトークンを使用し、返すページに関連付けることができます、web サイトのアカウントでログイン。 名前または電子メール アドレスを格納、**電子メール**フォームのフィールド。

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. 選択、**関連付ける**ボタンをクリックします。 

    ブラウザーがホーム ページに戻るしに記録されます。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>その他のリソース


- [サイト全体の動作をカスタマイズする](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ASP.NET Web ページ サイトへのセキュリティとメンバーシップの追加](https://go.microsoft.com/fwlink/?LinkID=202904)
