---
title: "Facebook、Google、および他の外部プロバイダーを使用する認証の有効化"
author: rick-anderson
description: "このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。"
keywords: "ASP.NET Core, 認証, ソーシャル, 認証プロバイダー, google, facebook, twitter, microsoft アカウント"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 9fc0d6c3e9691f8c3fa0d769ac53c3337d822fc5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Facebook、Google、および他の外部プロバイダーを使用する認証の有効化

<a name="security-authentication-social-logins"></a>

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してログインできる、ASP.NET Core 2.x アプリケーションを構築する方法について説明します。

ここでは、[Facebook](facebook-logins.md)、[Twitter](twitter-logins.md)、[Google](google-logins.md)、および [Microsoft](microsoft-logins.md) の各プロバイダーを対象に説明します。 他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。

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

## <a name="require-ssl"></a>SSL を必須にする

OAuth 2.0 では、HTTPS プロトコル経由での認証に SSL を使用する必要があります。

注: 上の図のようにプロジェクト ウィザードの **[認証の変更]** ダイアログで **[個人のユーザー アカウント]** オプションを選択している場合、ASP.NET Core 2.x 用の **[Web アプリケーション]** または **[Web API]** プロジェクト テンプレートを使用して作成されたプロジェクトは、SSL を有効にし、https URL を使用して起動するように自動的に構成されます。

* [「ASP.NET Core アプリケーションで SSL を適用します」](xref:security/enforcing-ssl)トピックの手順に従ってサイトで SSL を必須にします。

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する

ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます (正確な名前はプロバイダーによって異なります)。

これらの値は、実質的に、アプリケーションが API へのアクセスに使用する*ユーザー名*と*パスワード*です。また、構成ファイルに直接格納したり、ハードコーディングしたりする代わりに、**シークレット マネージャー**のサポートによりアプリケーションの構成にリンクできる "シークレット" を構成します。

「[Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリケーションのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。

## <a name="setup-login-providers-required-by-your-application"></a>アプリケーションに必要なログイン プロバイダーをセットアップする

各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。

* [Facebook](facebook-logins.md) の手順
* [Twitter](twitter-logins.md) の手順
* [Google](google-logins.md) の手順
* [Microsoft](microsoft-logins.md) の手順
* [その他のプロバイダー](other-logins.md)の手順

## <a name="optionally-set-password"></a>必要に応じてパスワードを設定する

外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。 そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。 外部ログイン プロバイダーを使用できない場合、Web サイトにログインすることができません。

外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。

* 右上にある **[Hello]<email alias>** リンクをタップして**管理**ビューに移動します。

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* **[作成]** をタップします

![パスワード ページを設定する](index/_static/pass2a.png)

* 有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。

## <a name="next-steps"></a>次のステップ

* このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。

* アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。
