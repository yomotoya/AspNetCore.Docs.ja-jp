---
title: "Web Api を Azure Active Directory B2C のクラウド認証"
author: camsoper
description: "ASP.NET Core Web API と Azure Active Directory B2C の認証を設定する方法を検出します。 認証されている web API Postman をテストします。"
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: c79f1152afd2f55f53bf5deb9208fa5b4d5ef64d
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c"></a>Web Api を Azure Active Directory B2C のクラウド認証

作成者: [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) は、web およびモバイル アプリケーションのクラウド id 管理ソリューションです。 サービスは、クラウドとオンプレミスでホストされているアプリの認証を提供します。 認証の種類には、ソーシャル ネットワーク アカウントでは、個々 のアカウントを含めるし、エンタープライズ アカウントをフェデレーションします。 また、Azure AD B2C では、最小構成で多要素認証を提供できます。

> [!TIP]
> Azure Active Directory (Azure AD) の Azure AD B2C の別の製品サービスしています。 Azure AD テナントは、組織を表し、Azure AD B2C テナントは証明書利用者アプリケーションで使用される id のコレクションを表します。 詳細については、次を参照してください。 [Azure AD B2C: よく寄せられる質問 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)です。

Web Api には、ユーザー インターフェイスがあるありません、ためにいないユーザーを Azure AD B2C のような場合は、セキュリティ トークン サービスにリダイレクトすることです。 代わりに、この API は、呼び出し元のアプリは、Azure AD B2C を持つユーザーが既に認証からベアラー トークンを渡されます。 API は、ユーザーが直接やり取りせずトークンを検証します。

このチュートリアルで学習する方法。

> [!div class="checklist"]
> * Azure Active Directory B2C テナントを作成します。
> * Azure AD B2C で Web API を登録します。
> * Visual Studio を使用すると、Azure AD B2C テナント認証を使用するのにように構成、Web API を作成できます。
> * Azure AD B2C テナントの動作を制御するポリシーを構成します。
> * ログイン ダイアログを表示する web アプリをシミュレートするために使用する Postman は、トークンを取得し、それを使用して、web API に対する要求します。

## <a name="prerequisites"></a>必須コンポーネント

以下は、このチュートリアルで必要です。

* [Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (任意のエディション)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C テナントを作成します。

Azure AD B2C テナントを作成する[ドキュメント」の説明に従って](/azure/active-directory-b2c/active-directory-b2c-get-started)です。 メッセージが表示されたら、テナントを Azure サブスクリプションに関連付けるは省略可能なこのチュートリアルです。

## <a name="configure-a-sign-up-or-sign-in-policy"></a>サインアップまたはサインイン ポリシーを構成します。

Azure AD B2C のドキュメントで手順を使用して[サインアップまたはサインイン ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)です。 ポリシーに名前**SiUpIn**です。  ドキュメントの例の値を使用して**Id プロバイダー**、**サインアップ属性**、および**アプリケーション要求**です。 使用して、**を今すぐ実行**ドキュメント」の説明に従って、ポリシーをテストするにはボタンは省略可能です。

## <a name="register-the-api-in-azure-ad-b2c"></a>Azure AD B2C の登録 API

新しく作成された Azure AD B2C テナントの登録 API を使用して、[ドキュメント」の手順](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下にある、 **web API を登録**セクションです。

次の値を使用します。

| 設定                       | [値]               | メモ                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *&lt;API 名&gt;*  | 入力、**名前**コンシューマーにアプリを説明するアプリのです。                     |
| **Web アプリは、ロールまたは web API** | [はい]                 |                                                                                        |
| **暗黙のフローを許可します。**       | [はい]                 |                                                                                        |
| **応答 URL**                 | `https://localhost` | 応答 Url は、エンドポイントが Azure AD B2C がアプリを要求するすべてのトークンを返します。 |
| **アプリ ID URI**                | *api*               | URI は、物理アドレスに解決する必要はありません。 のみ一意である必要があります。     |
| **ネイティブ クライアントが含まれます**     | ×                  |                                                                                        |

この API は、登録後、テナントでアプリケーションと Api の一覧が表示されます。 だけに登録されている API を選択します。 選択、**コピー**の右側にあるアイコン、**アプリケーション ID**をクリップボードにコピーするフィールドです。 選択**スコープをパブリッシュ**既定値を確認してください*user_impersonation*スコープが存在します。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017 で ASP.NET Core アプリを作成します。

Visual Studio Web アプリケーション テンプレートは、認証に使用する Azure AD B2C テナント構成されていることができます。

Visual Studio で。

1. 新しい ASP.NET Core Web アプリケーションを作成します。 
2. 選択**Web API**テンプレートの一覧からです。
3. 選択、**認証の変更**ボタンをクリックします。
    
    ![変更の認証 ボタン](./azure-ad-b2c-webapi/change-auth-button.png)

4. **認証の変更**ダイアログで、**個々 のユーザー アカウント**、し、 **、クラウド内の既存のユーザー ストアへの接続**ドロップダウン リストにします。 
    
    ![認証の変更 ダイアログ](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. 次の値をフォームに入力します。
    
    | 設定                       | [値]                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **ドメイン名**               | *&lt;B2C テナントのドメイン名&gt;*          |
    | **アプリケーション ID**            | *&lt;アプリケーション ID をクリップボードから貼り付けます&gt;* |
    | **サインアップまたはサインイン ポリシー** | `B2C_1_SiUpIn`                                        |
    
    選択**OK**を閉じる、**認証の変更**ダイアログ。 選択**OK** web アプリを作成します。

Visual Studio では、という名前のコント ローラーで、web API を作成*ValuesController.cs* GET 要求に対してハード コーディングされた値を返します。 クラスを装飾、 [Authorize attribute](xref:security/authorization/simple)ので、すべての要求が認証を必要とします。

## <a name="run-the-web-api"></a>Web API を実行します。

Visual Studio で、API を実行します。 Visual Studio では、API のルート URL を指すブラウザーを起動します。 アドレス バーの URL をメモして、バック グラウンドで実行されている API のままにします。

> [!NOTE] ルートの URL に対して定義されているコント ローラーがないため、ブラウザーには、404 (ページが見つかりません) エラーが表示されます。 これは通常の動作です。

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Postman を使用して、トークンを取得および API のテスト

[Postman](https://getpostman.com/postman)はテスト用ツールの web Api です。 このチュートリアルでは、Postman は、ユーザーの代理で web API にアクセスする web アプリをシミュレートします。

### <a name="register-postman-as-a-web-app"></a>Web アプリとして Postman を登録します。

Postman は、Azure AD B2C テナントからトークンを入手できる、web アプリをシミュレート、ので必要があります登録する必要が、テナントで web アプリとします。 登録を使用して Postman[ドキュメント」の手順](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下にある、 **web アプリを登録**セクションです。 停止時刻、 **web アプリ クライアント シークレットの作成**セクションです。 クライアント シークレットは、このチュートリアルでは必要ありません。 

次の値を使用します。

| 設定                       | [値]                            | メモ                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Web アプリは、ロールまたは web API** | [はい]                              |                                 |
| **暗黙のフローを許可します。**       | [はい]                              |                                 |
| **応答 URL**                 | `https://getpostman.com/postman` |                                 |
| **アプリ ID URI**                | *&lt;空白のままに&gt;*            | このチュートリアルでは必要ありません。 |
| **ネイティブ クライアントが含まれます**     | ×                               |                                 |

新しく登録されている web アプリでは、ユーザーの代理で web API にアクセスする権限が必要です。  

1. 選択**Postman**クリックしてアプリの一覧で**API アクセス**左側のメニュー。
2. 選択**+ 追加**です。
3. **選択 API**ドロップダウン リストで、web API の名前を選択します。
4. **選択スコープ**ドロップダウン リストで、すべてのスコープが選択されていることを確認します。
5. 選択**Ok**です。

ベアラー トークンを取得するためには、Postman アプリのアプリケーションの ID に注意してください。

### <a name="create-a-postman-request"></a>Postman 要求を作成します。

Postman を起動します。 既定では、Postman が表示されます、**新規作成**ダイアログを起動するとします。 ダイアログ ボックスが表示されていない場合は、選択、 **+ 新規**左上のボタンをクリックします。

**新規作成** ダイアログ。

1. 選択**要求**です。
    
    ![要求 ボタン](./azure-ad-b2c-webapi/postman-create-new.png)

2. 入力*値の取得*で、**要求名**ボックス。
3. 選択**+ コレクションの作成**要求を格納するための新しいコレクションを作成します。 コレクションの名前を付けます*ASP.NET Core チュートリアル*し、チェック マークを選択します。
    
    ![新しいコレクションを作成します。](./azure-ad-b2c-webapi/postman-create-collection.png)

4. 選択、 **ASP.NET Core チュートリアルへ保存**ボタンをクリックします。

### <a name="test-the-web-api-withoutauthentication"></a>Web API withoutauthentication をテストします。

Web API に認証が必要であることを確認するには、最初に認証を使用しない要求を作成します。

1. **要求 URL を入力**ボックスで、URL を入力`ValuesController`です。 URL はブラウザーに表示されるものと同じ**api/値**追加されます。 例としては`https://localhost:44375/api/values`します。
2. 選択、**送信**ボタンをクリックします。
3. 応答のステータスは*401 Unauthorized*です。

    ![401 承認されていない応答](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>ベアラー トークンを取得します。

Web API に認証された要求にベアラー トークンが必要です。 Postman 簡単に Azure AD B2C テナントにサインインし、トークンを取得できます。

1. **承認**] タブで、**型**ドロップダウンで、[ **OAuth 2.0**です。 **承認データを追加して**ドロップダウンで、**要求ヘッダー**です。 選択**新しいアクセス トークンを取得**です。
    
    ![[承認] タブの設定](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. 完了、**新しいアクセス トークンの取得**ようダイアログ。
    
    | 設定                   | [値]                                                                                         | メモ                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **トークン名**            | *&lt;token name&gt;*                                                                          | トークンのわかりやすい名前を入力します。                                                    |
    | **付与の種類**            | 暗黙的                                                                                      |                                                                                            |
    | **コールバック URL**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **Auth URL**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | 置き換える*&lt;テナントのドメイン名&gt;*山かっこせず、テナントのドメイン名を使用します。 |
    | **クライアント ID**             | *&lt;入力 Postman アプリの<b>アプリケーション ID</b>&gt;*                                       |                                                                                            |
    | **クライアント シークレット**         | *&lt;空白のままに&gt;*                                                                         |                                                                                            |
    | **スコープ**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | 置き換える*&lt;テナントのドメイン名&gt;*山かっこせず、テナントのドメイン名を使用します。 |
    | **クライアント認証** | 本文でクライアントの資格情報を送信します。                                                               |                                                                                            |
    
3. 選択、**トークン要求**ボタンをクリックします。

4. Postman には、ダイアログ ボックスで、Azure AD B2C テナントの記号を含む新しいウィンドウが開きます。 (1 つには、ポリシーのテストが作成された) 場合は、既存のアカウントでサインインまたは選択**今すぐサインアップ**新しいアカウントを作成します。 **パスワードを忘れた場合ですか?**忘れてもパスワードをリセットするリンクを使用します。

5. 正常にサインインした後、ウィンドウが閉じられると、**アクセス トークンの管理**ダイアログが表示されます。 クリックし、一番下まで下にスクロール、**使用トークン**ボタンをクリックします。
    
    !["使用 Token"ボタンを検索する場所](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>認証を使用して、web API をテストします。

選択、**送信**をもう一度要求を送信するボタンをクリックします。 現時点では、応答ステータスが*200 OK* JSON ペイロードが、応答に表示されると**本文** タブ。
    
![ペイロードと成功状態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
> * Azure Active Directory B2C テナントを作成します。
> * Azure AD B2C で Web API を登録します。
> * Visual Studio を使用すると、Azure AD B2C テナント認証を使用するのにように構成、Web API を作成できます。
> * Azure AD B2C テナントの動作を制御するポリシーを構成します。
> * ログイン ダイアログを表示する web アプリをシミュレートするために使用する Postman は、トークンを取得し、それを使用して、web API に対する要求します。

学習することにより、API の開発を続行するには。

* [セキュリティで保護された ASP.NET Core web アプリケーションを Azure AD B2C](xref:security/authentication/azure-ad-b2c)です。
* [.NET web API を使用して Azure AD B2C .NET web アプリから呼び出す](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)です。
* [Azure AD B2C ユーザー インターフェイスのカスタマイズ](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)です。
* [パスワードの複雑さの要件を構成する](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)です。
* [多要素認証を有効にする](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)です。
* など、追加の id プロバイダーを構成[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)、およびその他。
* [Azure AD Graph API を使用して](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)Azure AD B2C テナントからグループ メンバーシップなど、追加のユーザー情報を取得します。