---
title: "ASP.NET のコアでアクティブなディレクトリの B2C を Azure クラウド認証"
author: camsoper
description: "ASP.NET Core での Azure Active Directory B2C の認証を設定する方法を検出します。"
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>ASP.NET のコアでアクティブなディレクトリの B2C を Azure クラウド認証

作成者: [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory の B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) は、web およびモバイル アプリケーションのクラウドのアイデンティティ管理ソリューションです。 サービスは、クラウドとオンプレミスでホストされているアプリの認証を提供します。 認証の種類は、ソーシャル ネットワーク アカウントでは、個々 のアカウントを含めるし、エンタープライズ アカウントをフェデレーションします。 また、Azure AD B2C では、最小構成で多要素認証を提供できます。

> [!TIP]
> Azure Active Directory (Azure AD) の Azure AD B2C の別の製品サービスしています。 Azure AD テナントは、組織を表し、Azure AD B2C テナントは証明書利用者アプリケーションで使用される id のコレクションを表します。 詳細については、次を参照してください。 [Azure AD B2C: よく寄せられる質問 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)です。

このチュートリアルで学習する方法。

> [!div class="checklist"]
> * Azure Active Directory B2C テナントを作成します。
> * Azure AD B2C にアプリを登録します。
> * Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core web アプリを作成するには
> * Azure AD B2C テナントの動作を制御するポリシーを構成します。

## <a name="prerequisites"></a>必須コンポーネント

以下は、このチュートリアルで必要です。

* [Microsoft Azure サブスクリプション](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017年](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)(任意のエディション)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C テナントを作成します。

Azure Active Directory B2C テナントを作成する[ドキュメント」の説明に従って](/azure/active-directory-b2c/active-directory-b2c-get-started)です。 メッセージが表示されたら、テナントを Azure サブスクリプションに関連付けるは省略可能なこのチュートリアルです。

## <a name="register-the-app-in-azure-ad-b2c"></a>Azure AD B2C でアプリを登録します。

新しく作成された Azure AD B2C テナント登録を使用してアプリを[ドキュメント」の手順](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下にある、 **web アプリを登録**セクション。 停止時刻、 **web アプリ クライアント シークレットの作成**セクションです。 クライアント シークレットは、このチュートリアルでは必要ありません。 

次の値を使用します。

| 設定                       | [値]                     | メモ                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;アプリ名&gt;*        | 入力、**名前**コンシューマーにアプリを説明するアプリのです。                                                                                                                                 |
| **Web アプリは、ロールまたは web API** | [はい]                       |                                                                                                                                                                                                    |
| **暗黙のフローを許可します。**       | [はい]                       |                                                                                                                                                                                                    |
| **応答 URL**                 | `https://localhost:44300` | 応答 Url は、エンドポイントが Azure AD B2C がアプリを要求するすべてのトークンを返します。 Visual Studio では、使用する返信 URL を提供します。 ここでは、次のように入力します。`https://localhost:44300`フォームを完了します。 |
| **アプリ ID URI**                | 空白のままに               | このチュートリアルでは必要ありません。                                                                                                                                                                    |
| **ネイティブ クライアントは、します。**     | ×                        |                                                                                                                                                                                                    |

> [!WARNING]
> かどうかの注意してください、localhost 以外の応答 URL を設定する、[応答 URL の一覧で許可されている制約](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)です。 

アプリを登録すると、テナント内のアプリの一覧が表示されます。 登録されたアプリを選択します。 選択、**コピー**の右側にあるアイコン、**アプリケーション ID**をクリップボードにコピーするフィールドです。

何も複数現時点では、Azure AD B2C テナントで構成できますが、ブラウザー ウィンドウを開いたままにしておきます。 多くの構成がある ASP.NET Core アプリケーションの作成後にします。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Visual Studio 2017 で ASP.NET Core アプリを作成します。

Visual Studio Web アプリケーション テンプレートは、認証に使用する Azure AD B2C テナント構成されていることができます。

Visual Studio で。

1. 新しい ASP.NET Core Web アプリケーションを作成します。 
2. 選択**Web アプリケーション**テンプレートの一覧からです。
3. 選択、**認証の変更**ボタンをクリックします。
    
    ![変更の認証 ボタン](./azure-ad-b2c/_static/changeauth.png)

4. **認証の変更**ダイアログで、**個々 のユーザー アカウント**、し、 **、クラウド内の既存のユーザー ストアへの接続**ドロップダウン リストにします。 
    
    ![認証の変更 ダイアログ](./azure-ad-b2c/_static/changeauthdialog.png)

5. 次の値をフォームに入力します。
    
    | 設定                       | [値]                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **ドメイン名**               | *&lt;B2C のテナントのドメイン名&gt;*          |
    | **アプリケーション ID**            | *&lt;アプリケーション ID、クリップボードからの貼り付け&gt;* |
    | **コールバックのパス**             | *&lt;既定値を使用します。&gt;*                       |
    | **サインアップまたはサイン インのポリシー** | `B2C_1_SiUpIn`                                        |
    | **パスワード ポリシーをリセットします。**     | `B2C_1_SSPR`                                          |
    | **プロファイルのポリシーを編集します。**       | *&lt;空白のままに&gt;*                                 |
    
    選択、**コピー**  の横にリンク**Reply URI** Reply URI をクリップボードにコピーします。 選択**OK**を閉じる、**認証の変更**ダイアログ。 選択**OK** web アプリを作成します。

## <a name="finish-the-b2c-app-registration"></a>B2C アプリの登録を完了します。

ブラウザー ウィンドウで開かれたまま B2C アプリのプロパティに戻ります。 一時的な変更**応答 URL** Visual Studio から以前の値にコピーを指定します。 選択**保存**ウィンドウの上部にあります。

> [!TIP]
> 応答 URL をコピーしていない場合、web プロジェクト プロパティで、[デバッグ] タブから SSL アドレスを使用し、追加、 **CallbackPath**値から*される appsettings.json*です。

## <a name="configure-policies"></a>ポリシーを構成します。

Azure AD B2C のドキュメントの手順を使用[サインアップまたはサインイン ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)、し[パスワード リセット ポリシーを作成する](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)です。 ドキュメントの例の値を使用して**Id プロバイダー**、**サインアップ属性**、および**アプリケーション要求**です。 使用して、**を今すぐ実行**ドキュメント」の説明に従って、ポリシーをテストするにはボタンは省略可能です。

> [!WARNING]
> ポリシー名は、ドキュメントで説明されているとおりに正確にこれらのポリシーで使用されていたことを確認、**認証の変更**Visual Studio でのダイアログ。 ポリシー名を検証できます*される appsettings.json*です。

## <a name="run-the-app"></a>アプリを実行する

Visual Studio で、キーを押して**f5 キーを押して**アプリをビルドして実行します。 Web アプリが起動後、選択**サインイン**です。

![アプリへのサインインします。](./azure-ad-b2c/_static/signin.png)

ブラウザーは、Azure AD B2C テナントにリダイレクトします。 (1 つには、ポリシーのテストが作成された) 場合は、既存のアカウントでサインインまたは選択**今すぐサインアップ**新しいアカウントを作成します。 **パスワードを忘れた場合ですか?**忘れてもパスワードをリセットするリンクを使用します。

![Azure AD B2C ログイン](./azure-ad-b2c/_static/b2csts.png)

正常にサインインした後は、ブラウザーは、web アプリにリダイレクトします。

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行う方法を学びました。

> [!div class="checklist"]
> * Azure Active Directory B2C テナントを作成します。
> * Azure AD B2C にアプリを登録します。
> * Visual Studio を使用して Azure AD B2C テナント認証を使用するように構成 ASP.NET Core Web アプリケーションを作成するには
> * Azure AD B2C テナントの動作を制御するポリシーを構成します。

ASP.NET Core アプリケーションが認証に使用する Azure AD B2C、構成されていること、 [Authorize attribute](xref:security/authorization/simple)アプリをセキュリティで保護するために使用できます。 学習することにより、アプリケーションの開発を続行するには。

* [Azure AD B2C のユーザー インターフェイスをカスタマイズする](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)です。
* [パスワードの複雑さの要件を構成する](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)です。
* [多要素認証を有効にする](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* など、追加の id プロバイダーを構成[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)、およびその他。
* [Azure AD グラフ API を使用して](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)Azure AD B2C テナントから、グループ メンバーシップなどの追加のユーザー情報を取得します。
* [Azure AD B2C を使用して web API を ASP.NET のコア セキュリティで保護された](xref:security/authentication/azure-ad-b2c-webapi)です。
* [Azure AD B2C を使用して .NET web アプリケーションから .NET web API を呼び出す](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
