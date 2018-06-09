---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET Identity のユーザーのプライマリ キーの変更 |Microsoft ドキュメント
author: tfitzmac
description: Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。 ASP.NET Id では、型を変更することができます、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26498231"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>ASP.NET Identity のユーザーのプライマリ キーの変更
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。 ASP.NET Id をデータの要件を満たすために、キーの型を変更することができます。 たとえば、整数、文字列からキーの型を変更できます。
> 
> このトピックでは、既定の web アプリケーションとキーを変更、ユーザー アカウントを整数で開始する方法を示します。 プロジェクト内のキーの任意の型を実装する同じ変更を行うこともできます。 既定の web アプリケーションでこれらの変更を行う方法を示していますが、カスタマイズされたアプリケーションのような変更を適用する可能性があります。 MVC または Web フォームを操作するときに必要な変更が表示されます。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Visual Studio 2013 Update 2 (またはそれ以降)
> - ASP.NET Identity 2.1 以降


このチュートリアルの手順を実行するには、Visual Studio 2013 Update 2 (またはそれ以降) と ASP.NET Web アプリケーション テンプレートから作成された web アプリケーションが必要です。 更新プログラム 3 で変更するテンプレートです。 このトピックでは、更新プログラム 2 と Update 3 のテンプレートを変更する方法を示します。

このトピックは、次のセクションで構成されています。

- [Id のユーザー クラス内のキーの種類を変更します。](#userclass)
- [キーの型を使用してカスタマイズした Id のクラスを追加します。](#customclass)
- [キーの型を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。](#context)
- [キーの型を使用するスタートアップ構成の変更](#startup)
- [Update 2 と MVC での変更をキーの型を渡す AccountController](#mvcupdate2)
- [更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。](#mvcupdate3)
- [Update 2 の Web フォーム、変更をキーの型を渡すアカウント ページ](#webformsupdate2)
- [更新プログラム 3 Web フォーム、変更をキーの型を渡すアカウント ページ](#webformsupdate3)
- [アプリケーションを実行します。](#run)
- [その他のリソース](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Id のユーザー クラス内のキーの種類を変更します。

ASP.NET Web アプリケーション テンプレートから作成されたプロジェクトで ApplicationUser クラスがユーザー アカウントのキーを整数値を使用することを指定します。 IdentityModels.cs、変更、ApplicationUser クラスの型を持つ IdentityUser から継承する**int** TKey ジェネリック パラメーター。 実装していないまだカスタマイズしたクラスは次の 3 つの名前を渡すこともできます。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

キーの種類を変更しましたが、既定では、アプリケーションの残りの部分も前提としています、キーは、文字列。 文字列が想定するコード内のキーの型を明示的に示す必要があります。

**ApplicationUser**クラス、変更、 **GenerateUserIdentityAsync**メソッドに次の強調表示されたコードに示すように、int を含めます。 この変更は、更新プログラム 3 のテンプレートを使用して Web フォーム プロジェクトの必要はありません。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>キーの型を使用してカスタマイズした Id のクラスを追加します。

IdentityUserRole IdentityUserClaim、IdentityUserLogin、IdentityRole、UserStore RoleStore など、他のユーザー クラスは、文字列のキーを使用するをまだ設定します。 キーの整数を指定してこれらのクラスの新しいバージョンを作成します。 これらのクラスで実装コードを提供する必要はありません、キーと int を設定するだけでは、主にします。

IdentityModels.cs ファイルに次のクラスを追加します。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>キーの型を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。

IdentityModels.cs での定義を変更、 **ApplicationDbContext**クラスを使用して、新しいクラスをカスタマイズして**int**キーを強調表示されたコードに示すようにします。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema パラメーターは、コンス トラクターでは有効ではありません。 ThrowIfV1Schema 値に合格していないために、コンス トラクターを変更します。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs を開き、変更、 **ApplicationUserManger**ストア データを保持するクラスの新しいユーザーを使用するクラスと**int**キー。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

更新プログラム 3 のテンプレートでは、ApplicationSignInManager クラスを変更する必要があります。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>キーの型を使用するスタートアップ構成の変更

Startup.Auth.cs では、次の強調表示されているように、OnValidateIdentity コードを置き換えます。 GetUserIdCallback 定義で文字列値を解析して、整数型にことに注意してください。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

プロジェクトが、ジェネリック型の実装を認識しないかどうか、 **GetUserId**メソッド、バージョン 2.1 に、ASP.NET Identity の NuGet パッケージを更新する必要があります

ASP.NET Id で使用されるインフラストラクチャ クラスに多数の変更を加えた。 プロジェクトをコンパイルしようとすると、多くのエラーがわかります。 しかし、残りのエラーは、すべてと似ています。 Id クラスは、キーの整数が必要ですが、コント ローラー (または Web フォーム) が文字列値を渡すことです。 各ケースで呼び出すことによって、文字列と整数から変換する必要があります。 **GetUserId&lt;int&gt;** です。 コンパイルすることで、エラー一覧を使用するか、以下の変更を追跡します。

残りの変更は、プロジェクトを作成して、Visual Studio でインストールした更新プログラムの種類によって異なります。 次のリンクから該当するセクションに直接移動することができます。

- [Update 2 と MVC での変更をキーの型を渡す AccountController](#mvcupdate2)
- [更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。](#mvcupdate3)
- [Update 2 の Web フォーム、変更をキーの型を渡すアカウント ページ](#webformsupdate2)
- [更新プログラム 3 Web フォーム、変更をキーの型を渡すアカウント ページ](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Update 2 と MVC での変更をキーの型を渡す AccountController

AccountController.cs ファイルを開きます。 次の方法を変更する必要があります。

**ConfirmEmail**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**関連付けを解除**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。

AccountController.cs ファイルを開きます。 次の方法を変更する必要があります。

**ConfirmEmail**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**されている SendCode**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

ManageController.cs ファイルを開きます。 次の方法を変更する必要があります。

**インデックス**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Update 2 の Web フォーム、変更をキーの型を渡すアカウント ページ

Update 2 の Web フォーム、するには、次のページを変更する必要があります。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>更新プログラム 3 Web フォーム、変更をキーの型を渡すアカウント ページ

更新プログラム 3 Web フォーム、するには、次のページを変更する必要があります。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>アプリケーションを実行します。

すべての既定の Web アプリケーション テンプレートに必要な変更が完了しました。 アプリケーションを実行し、新しいユーザーを登録します。 ユーザーを登録すると、AspNetUsers テーブルが整数である Id 列を持つことがわかります。

![新しいプライマリ キー](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

以前に作成した ASP.NET Identity テーブル別のプライマリ キーを持つ場合は、いくつか変更を加える必要があります。 可能であれば、既存のデータベースを削除します。 Web アプリケーションを実行し、新しいユーザーを追加するときに、データベースは、適切なデザインで再作成になります。 削除が不可能な場合、テーブルを変更するコード最初の移行を実行します。 ただし、新しい整数型の主キーがないとしてセットアップする、データベース内の SQL の ID プロパティ。 ID として、Id 列を手動で設定する必要があります。

<a id="other"></a>
## <a name="other-resources"></a>その他のリソース

- [ASP.NET Identity のカスタム ストレージ プロバイダーの概要](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [既存 Web サイトを SQL メンバーシップから ASP.NET Identity に移行する](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [メンバーシップと ASP.NET Identity のユーザー プロファイル用のユニバーサル プロバイダー データの移行](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [サンプル アプリケーション](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)変更された主キーを持つ
