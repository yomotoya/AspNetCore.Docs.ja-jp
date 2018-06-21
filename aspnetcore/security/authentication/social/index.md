---
title: Facebook、Google、ASP.NET Core での外部プロバイダーの認証
author: rick-anderson
description: このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。
ms.author: riande
ms.date: 11/01/2016
uid: security/authentication/social/index
ms.openlocfilehash: 58045504ce4588f854428273273d3ea8f181e12e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277999"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook、Google、ASP.NET Core での外部プロバイダーの認証

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してログインできる、ASP.NET Core 2.x アプリケーションを構築する方法について説明します。

ここでは、[Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins)、および [Microsoft](xref:security/authentication/microsoft-logins) の各プロバイダーを対象に説明します。 他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。

![Facebook、Twitter、Google+、Windows のソーシャル メディア アイコン](index/_static/social.png)

ユーザーが既存の資格情報でサインインできるようにすると、ユーザーにとって便利なだけでなく、サインイン プロセスを管理する複雑な処理の多くをサードパーティに移行できます。 ソーシャル ログインによってトラフィックとユーザーの変換を促進する方法の例については、[Facebook](https://www.facebook.com/unsupportedbrowser) と [Twitter](https://dev.twitter.com/resources/case-studies) によるケース スタディを参照してください。

注: ここで紹介するパッケージでは、OAuth 認証フローの複雑な処理の多くを抽象化していますが、トラブルシューティングの際には詳細を理解することが必要になる場合があります。 利用できる資料は多数あります。たとえば、「[Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)」(OAuth 2 の紹介) や「[Understanding OAuth 2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/)」(OAuth 2 の概要) を参照してください。 「[ASP.NET Core source code for the provider packages](https://github.com/aspnet/Security/tree/dev/src)」(プロバイダー パッケージ用の ASP.NET Core ソース コード) を参照すると、いくつかの問題を解決できます。

## <a name="create-a-new-aspnet-core-project"></a>新しい .NET Core プロジェクトを作成する

* Visual Studio 2017 のスタート ページから、または **[ファイル]、[新規作成]、[プロジェクト]** の順に選択して、新しいプロジェクトを作成します。

* **[Visual C#] > [.NET Core]** カテゴリにある **[ASP.NET Core Web アプリケーション]** テンプレートを選択します。

![[新しいプロジェクト] ダイアログ](index/_static/new-project.png)

* **[Web アプリケーション]** をタップし、**[認証]** が **[個人のユーザー アカウント]** に設定されていることを確認します

![[新しい Web アプリケーションの作成] ダイアログ](index/_static/select-project.png)

注: このチュートリアルは、ASP.NET Core 2.0 SDK バージョンに適用されます。バージョンはウィザードの上部で選択できます。

## <a name="apply-migrations"></a>移行を適用する

* アプリを実行して **[ログイン]** リンクを選択します。
* **[新規ユーザーとして登録する]** リンクを選択します。
* 新しいアカウントの電子メール アドレスとパスワードを入力し、**[登録]** を選択します。
* 指示に従って移行を適用します。

## <a name="require-ssl"></a>SSL を必須にする

OAuth 2.0 では、HTTPS プロトコル経由での認証に SSL を使用する必要があります。

注: 上の図のようにプロジェクト ウィザードの **[認証の変更]** ダイアログで **[個人のユーザー アカウント]** オプションを選択している場合、ASP.NET Core 2.x 用の **[Web アプリケーション]** または **[Web API]** プロジェクト テンプレートを使用して作成されたプロジェクトは、SSL を有効にし、https URL を使用して起動するように自動的に構成されます。

* 「[Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl)」(ASP.NET Core アプリで SSL を適用する) のトピックの手順に従ってサイトで SSL を必須にします。

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する

ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます (正確な名前はプロバイダーによって異なります)。

これらの値は、実質的に、アプリケーションが API へのアクセスに使用する*ユーザー名*と*パスワード*です。また、構成ファイルに直接格納したり、ハードコーディングしたりする代わりに、**シークレット マネージャー**のサポートによりアプリケーションの構成にリンクできる "シークレット" を構成します。

「[Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。

## <a name="setup-login-providers-required-by-your-application"></a>アプリケーションに必要なログイン プロバイダーをセットアップする

各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。

* [Facebook](xref:security/authentication/facebook-logins) の手順
* [Twitter](xref:security/authentication/twitter-logins) の手順
* [Google](xref:security/authentication/google-logins) の手順
* [Microsoft](xref:security/authentication/microsoft-logins) の手順
* [その他のプロバイダー](xref:security/authentication/otherlogins)の手順

[!INCLUDE[](~/includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>必要に応じてパスワードを設定する

外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。 そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。 外部ログイン プロバイダーを使用できない場合、Web サイトにログインすることができません。

外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。

* 右上にある **[Hello]&lt; 電子メール エイリアス&gt;** リンクをタップして**管理**ビューに移動します。

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* **[作成]** をタップします

![パスワード ページを設定する](index/_static/pass2a.png)

* 有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。

## <a name="next-steps"></a>次の手順

* このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。

* アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。
