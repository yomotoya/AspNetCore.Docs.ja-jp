---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: ASP.NET Identity でユーザーのプライマリ キーの変更 |Microsoft Docs
author: tfitzmac
description: Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。 ASP.NET Identity では、型を変更することができます、.
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 2ec9894df2f9a48ef482715ce71bb09fb4758127
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837140"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>ASP.NET Identity でユーザーのプライマリ キーの変更
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> Visual Studio 2013 では、既定の web アプリケーションは、ユーザー アカウントのキーの文字列値を使用します。 ASP.NET Identity では、データの要件を満たすために、キーの種類を変更することができます。 たとえば、文字列から整数に、キーの種類を変更できます。
> 
> このトピックでは、既定の web アプリケーションとユーザーのアカウント キーを整数に変更から始める方法を示します。 プロジェクト内の任意の種類のキーを実装するために、同じ変更を使用できます。 既定の web アプリケーションでこれらを変更する方法を示しますが、カスタマイズされたアプリケーションのような変更を適用する可能性があります。 MVC または Web フォームを使用する場合に必要な変更が表示されます。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Visual Studio 2013 with Update 2 (またはそれ以降)
> - ASP.NET Identity 2.1 以降


このチュートリアルでは、手順を実行するには、Visual Studio 2013 Update 2 (またはそれ以降) と ASP.NET Web アプリケーション テンプレートから作成された web アプリケーションが必要です。 更新プログラム 3 で変更するテンプレートです。 このトピックでは、更新プログラム 2 と Update 3 でテンプレートを変更する方法を示します。

このトピックは、次のセクションで構成されています。

- [Id のユーザー クラスのキーの種類を変更します。](#userclass)
- [キーの種類を使用して、カスタマイズされた Id のクラスを追加します。](#customclass)
- [キーの種類を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。](#context)
- [キーの種類を使用するスタートアップ構成の変更](#startup)
- [Update 2 と MVC でのキーの型を渡す AccountController を変更します。](#mvcupdate2)
- [更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。](#mvcupdate3)
- [更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。](#webformsupdate2)
- [Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。](#webformsupdate3)
- [アプリケーションを実行します。](#run)
- [その他のリソース](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Id のユーザー クラスのキーの種類を変更します。

ASP.NET Web アプリケーション テンプレートから作成されたプロジェクトで、ApplicationUser クラスで整数を使用して、ユーザー アカウントのキーのことを指定します。 IdentityModels.cs、変更、ApplicationUser クラスの型を持つ IdentityUser から継承する**int** TKey ジェネリック パラメーター。 また、まだ実装していない場合が 3 つのカスタマイズしたクラスの名前を渡します。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

キーの種類を変更しましたが、既定では、アプリケーションの残りの部分も前提としています、キーは、文字列です。 文字列が想定するコード内のキーの型を明示的に示す必要があります。

**ApplicationUser**クラス、変更、 **GenerateUserIdentityAsync**メソッドは以下の強調表示されたコードに示すように、int を含めます。 この変更は、更新プログラム 3 のテンプレートを使用した Web フォーム プロジェクトの必要はありません。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>キーの種類を使用して、カスタマイズされた Id のクラスを追加します。

IdentityUserRole、IdentityUserClaim、IdentityUserLogin、IdentityRole、UserStore、RoleStore など、他の Id クラスは、文字列キーを使用するをまだ設定します。 これらのキーを示す整数を指定するクラスの新しいバージョンを作成します。 これらのクラスで多くの実装コードを提供する必要はありません、キーとして int を設定するだけでは主にします。

IdentityModels.cs ファイルに次のクラスを追加します。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>キーの種類を使用するコンテキスト クラスおよびユーザーのマネージャーを変更します。

IdentityModels.cs での定義を変更、 **ApplicationDbContext**クラスを使用して、新しいクラスをカスタマイズして、 **int**の強調表示されたコードに示すように、キー。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

ThrowIfV1Schema パラメーターは、コンス トラクターでは有効ではありません。 ThrowIfV1Schema 値に合格していないため、コンス トラクターを変更します。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs を開き、変更、 **ApplicationUserManger**クラスの新しいユーザーを使用するストアのデータを保持するクラスと**int**キー。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

更新プログラム 3 のテンプレートでは、ApplicationSignInManager クラスを変更する必要があります。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>キーの種類を使用するスタートアップ構成の変更

Startup.Auth.cs で以下の強調表示されている、OnValidateIdentity コードに置き換えます。 GetUserIdCallback の定義を整数に文字列値を解析することに注意してください。

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

プロジェクトがの汎用実装を認識しないかどうか、 **GetUserId**メソッドでは、バージョン 2.1 に ASP.NET Identity の NuGet パッケージを更新する必要があります

ASP.NET Identity で使用されるインフラストラクチャ クラスに多くの変更を加えました。 プロジェクトをコンパイルしようとすると、多くのエラーが表示されます。 さいわい、残りのエラーは、すべてと似ています。 Id クラスには、キーの整数が必要がありますが、コント ローラー (または Web フォーム) が文字列値を渡すこと。 各ケースでは、呼び出すことによって、文字列と整数から変換する必要があります**GetUserId&lt;int&gt;** します。 コンパイルのエラー一覧を使用するか、以下の変更を追跡します。

残りの変更は、プロジェクトを作成して、Visual Studio でインストールした更新プログラムの種類によって異なります。 次のリンクから、関連するセクションに直接移動することができます。

- [Update 2 と MVC でのキーの型を渡す AccountController を変更します。](#mvcupdate2)
- [更新プログラム 3 での MVC、キーの型を渡す AccountController と ManageController を変更します。](#mvcupdate3)
- [更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。](#webformsupdate2)
- [Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Update 2 と MVC でのキーの型を渡す AccountController を変更します。

AccountController.cs ファイルを開きます。 次のメソッドを変更する必要があります。

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

**SendCode**メソッド

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

ManageController.cs ファイルを開きます。 次のメソッドを変更する必要があります。

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
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>更新プログラム 2 で、Web フォームのキーの型を渡すアカウント ページを変更します。

Update 2 の Web フォーム、次のページを変更する必要があります。

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

できるようになりました[アプリケーションを実行](#run)し、新しいユーザーを登録します。

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Update 3 では、Web フォームのキーの型を渡すアカウント ページを変更します。

Update 3 では、Web フォーム、次のページを変更する必要があります。

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

すべての既定の Web アプリケーション テンプレートに必要な変更が完了しました。 アプリケーションを実行し、新しいユーザーを登録します。 ユーザーを登録すると、AspNetUsers テーブルの整数である Id 列であることがわかります。

![新しいプライマリ キー](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

以前に作成した ASP.NET Identity テーブル別のプライマリ キーを持つ場合は、いくつか変更を加える必要があります。 可能であれば、既存のデータベースを削除します。 Web アプリケーションを実行し、新しいユーザーを追加する場合に、データベースは、適切なデザインで再作成になります。 削除できない場合は、code first migrations のテーブルを変更するを実行します。 ただし、新しい整数の主キーがある設定されていません、データベース内の SQL ID プロパティとして。 Id 列は、ID として手動で設定する必要があります。

<a id="other"></a>
## <a name="other-resources"></a>その他のリソース

- [ASP.NET Identity のカスタム ストレージ プロバイダーの概要](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [既存 Web サイトを SQL メンバーシップから ASP.NET Identity に移行する](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [メンバーシップと ASP.NET identity のユーザー プロファイル用のユニバーサル プロバイダー データの移行](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [サンプル アプリケーション](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt)主キーが変更されました。
