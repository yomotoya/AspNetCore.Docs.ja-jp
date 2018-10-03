---
title: ASP.NET Core Identity の概要
author: rick-anderson
description: ASP.NET Core アプリでは、Id を使用します。 パスワードの要件 (RequireDigit、RequiredLength、RequiredUniqueChars など) を設定する方法について説明します。
ms.author: riande
ms.date: 08/08/2018
uid: security/authentication/identity
ms.openlocfilehash: d427932bb175c09105534379be4d71760f4e04e5
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860954"
---
# <a name="introduction-to-identity-on-aspnet-core"></a>ASP.NET Core Identity の概要

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core Identity は、ASP.NET Core アプリにログイン機能を追加するメンバーシップ システムです。 Id に格納されているログイン情報で、ユーザーがアカウントの作成または外部ログイン プロバイダーを使用することができます。 サポートされている外部ログイン プロバイダーには、 [Facebook、Google、Microsoft アカウント、および Twitter](xref:security/authentication/social/index)します。

Id は、ユーザー名、パスワード、およびプロファイル データを格納する SQL Server データベースを使用して構成できます。 または、別の永続ストア使用できます、たとえば、Azure Table Storage。

[表示またはサンプル コードをダウンロードします。](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/identity/sample/src/ASPNETCore-IdentityDemoComplete/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)

このトピックを登録するには、ログイン Id を使用する方法について説明し、ユーザーをログアウトします。 Id を使用するアプリの作成について詳細な手順については、この記事の最後に次の手順を参照してください。

::: moniker range=">= aspnetcore-2.1"

<a name="adi"></a>
## <a name="adddefaultidentity-and-addidentity"></a>AddDefaultIdentity と AddIdentity

[AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionUIExtensions_AddDefaultIdentity__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__) ASP で導入されました。Core 2.1 です。 呼び出す`AddDefaultIdentity`は、次の呼び出しに似ています。

* [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.addidentity?view=aspnetcore-2.1#Microsoft_Extensions_DependencyInjection_IdentityServiceCollectionExtensions_AddIdentity__2_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Identity_IdentityOptions__)
* [AddDefaultUI](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderuiextensions.adddefaultui?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderUIExtensions_AddDefaultUI_Microsoft_AspNetCore_Identity_IdentityBuilder_)
* [AddDefaultTokenProviders](/dotnet/api/microsoft.aspnetcore.identity.identitybuilderextensions.adddefaulttokenproviders?view=aspnetcore-2.1#Microsoft_AspNetCore_Identity_IdentityBuilderExtensions_AddDefaultTokenProviders_Microsoft_AspNetCore_Identity_IdentityBuilder_)

参照してください[AddDefaultIdentity ソース](https://github.com/aspnet/Identity/blob/2634637fd535b229762b5e4a49cdd128f4d8f12e/src/UI/IdentityServiceCollectionUIExtensions.cs#L47-L64)詳細についてはします。

::: moniker-end

## <a name="create-a-web-app-with-authentication"></a>認証を使用した Web アプリを作成します。

個々 のユーザー アカウントを使って、ASP.NET Core Web アプリケーション プロジェクトを作成します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **[ファイル]** > **[新規作成]** > **[プロジェクト]** を順に選択します。
* **[ASP.NET Core Web アプリケーション]** を選択します。 プロジェクトに名前を**WebApp1**にプロジェクトのダウンロードとして同じ名前空間。 **[OK]** をクリックします。
* ASP.NET Core を選択します。 **Web アプリケーション**ASP.NET Core 2.1 では、を選択し、**認証の変更**します。
* 選択**個々 のユーザー アカウント** をクリック**OK**します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```cli
dotnet new webapp --auth Individual -o WebApp1
```

---

生成されたプロジェクトは、 [ASP.NET Core Identity](xref:security/authentication/identity)として、 [Razor クラス ライブラリ](xref:razor-pages/ui-class)します。

### <a name="test-register-and-login"></a>テスト登録とログイン

アプリを実行し、ユーザーを登録します。 参照するナビゲーションのトグル ボタンを選択する必要があります、画面サイズに応じて、**登録**と**ログイン**リンク。

![ナビゲーション バーのトグル ボタン](identity/_static/navToggle.png)

[!INCLUDE[](~/includes/view-identity-db.md)]

<a name="pw"></a>
### <a name="configure-identity-services"></a>Id サービスを構成します。

サービスを追加`ConfigureServices`します。 一般的なパターンは、すべての `Add{Service}` メソッドを呼び出した後、すべての `services.Configure{Service}` メソッドを呼び出すことです。 次のコードに生成されたテンプレートが含まれていない`CookiePolicyOptions`:

::: moniker range=">= aspnetcore-2.1"

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configureservices)]

上記のコードでは、オプションの既定値で Identity を構成します。 サービスを使用してアプリで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。

   Id を呼び出すことによって有効に[UseAuthentication](/dotnet/api/microsoft.aspnetcore.builder.authappbuilderextensions.useauthentication#Microsoft_AspNetCore_Builder_AuthAppBuilderExtensions_UseAuthentication_Microsoft_AspNetCore_Builder_IApplicationBuilder_)します。 `UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Startup.cs?name=snippet_configure&highlight=18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,11-28,30-42)]

   使用して、アプリケーションで利用できるサービス[依存関係の注入](xref:fundamentals/dependency-injection)します。

   呼び出すことによって、アプリケーションの id が有効に`UseAuthentication`で、`Configure`メソッド。 `UseAuthentication` 認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo/Startup.cs?name=snippet_configure&highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configureservices&highlight=7-9,13-33)]

   これらのサービスを使用して、アプリケーションで利用できる[依存関係の注入](xref:fundamentals/dependency-injection)します。

   呼び出すことによって、アプリケーションの id が有効に`UseIdentity`で、`Configure`メソッド。 `UseIdentity` cookie ベースの認証を追加します。[ミドルウェア](xref:fundamentals/middleware/index)要求パイプラインにします。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Startup.cs?name=snippet_configure&highlight=21)]

::: moniker-end

詳細については、次を参照してください。、 [IdentityOptions クラス](/dotnet/api/microsoft.aspnetcore.identity.identityoptions)と[アプリケーションの起動](xref:fundamentals/startup)します。

## <a name="scaffold-register-login-and-logout"></a>登録、ログイン、およびログアウトをスキャフォールディングします。

に従って、[の id の権限を持つ Razor プロジェクトにスキャフォールディング](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-razor-project-with-authorization)指示します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

登録、ログイン、ログアウト ファイルを追加します。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

名前のプロジェクトを作成する場合**WebApp1**、次のコマンドを実行します。 それ以外の場合、正しい名前空間を使用して、 `ApplicationDbContext`:

```cli
dotnet aspnet-codegenerator identity -dc WebApp1.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

PowerShell では、コマンドの区切り記号としてセミコロンを使用します。 PowerShell を使用する場合は、ファイルの一覧でセミコロンをエスケープまたはファイルのリストを前の例のように、二重引用符に配置します。

---

### <a name="examine-register"></a>登録を確認します。

::: moniker range=">= aspnetcore-2.1"

   ユーザーがクリックすると、**登録**、リンク、`RegisterModel.OnPostAsync`アクションが呼び出されます。 によって、ユーザーが作成された[CreateAsync](/dotnet/api/microsoft.aspnetcore.identity.usermanager-1.createasync#Microsoft_AspNetCore_Identity_UserManager_1_CreateAsync__0_System_String_)上、`_userManager`オブジェクト。 `_userManager` によって提供される依存関係の挿入)。

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Register.cshtml.cs?name=snippet&highlight=7,22)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   ユーザーがクリックすると、**登録**、リンク、`Register`でアクションが呼び出された`AccountController`。 `Register`アクションを呼び出して、ユーザーを作成します`CreateAsync`上、`_userManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_register&highlight=11)]

::: moniker-end

   ユーザーが正常に作成された場合、ユーザーがログインへの呼び出しによって`_signInManager.SignInAsync`します。

   **注:** を参照してください[アカウントの確認](xref:security/authentication/accconfirm#prevent-login-at-registration)登録時の即時のログインを防止する手順についてはします。

### <a name="log-in"></a>ログイン

::: moniker range=">= aspnetcore-2.1"

ログイン フォームが表示されるとき。

* **ログイン** リンクを選択します。
* ユーザーがいないに認証されるページにアクセスするときに**または**承認されると、ログイン ページにリダイレクトされます。

ログイン ページのフォームが送信されたときに、`OnPostAsync`アクションが呼び出されます。 `PasswordSignInAsync` 呼び出される、 `_signInManager` (依存関係の挿入によって提供される) オブジェクト。

   [!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Login.cshtml.cs?name=snippet&highlight=10-11)]

   基本`Controller`クラスでは、`User`コント ローラー メソッドからアクセスできるプロパティです。 たとえば、列挙することができます`User.Claims`承認決定を行うとします。 詳細については、次を参照してください。[承認](xref:security/authorization/index)します。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ユーザー選択すると、ログイン フォームが表示されます、**ログイン**リンクまたは認証が必要なページにアクセスするときにリダイレクトされます。 ユーザーがログイン ページで、フォームを送信するときに、 `AccountController` `Login`アクションが呼び出されます。

`Login`アクション呼び出し`PasswordSignInAsync`上、`_signInManager`オブジェクト (に提供される`AccountController`依存関係の挿入によって)。

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_login&highlight=13-14)]

基本 (`Controller`または`PageModel`) クラスでは、`User`プロパティ。 たとえば、`User.Claims`承認決定を行うために列挙できることができます。

::: moniker-end

### <a name="log-out"></a>ログアウトします。

::: moniker range=">= aspnetcore-2.1"

**ログアウト**リンクを呼び出す、`LogoutModel.OnPost`アクション。 

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/Logout.cshtml.cs)]

[SignOutAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.signoutasync#Microsoft_AspNetCore_Identity_SignInManager_1_SignOutAsync) cookie に格納されているユーザーの要求をクリアします。 呼び出した後にリダイレクトしない`SignOutAsync`ユーザーまたは**いない**サインアウトします。

投稿がで指定された、 *Pages/Shared/_LoginPartial.cshtml*:

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/_LoginPartial.cshtml?highlight=10)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

   クリックすると、**ログアウト**呼び出しのリンク、`LogOut`アクション。

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo/Controllers/AccountController.cs?name=snippet_logout&highlight=7)]

   上記のコードの呼び出し、`_signInManager.SignOutAsync`メソッド。 `SignOutAsync`メソッドは、cookie に格納されているユーザーの要求をクリアします。

::: moniker-end

## <a name="test-identity"></a>テスト Id

既定の web プロジェクト テンプレートは、ホーム ページへの匿名アクセスを許可します。 Id をテストするには追加[ `[Authorize]` ](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute) About ページにします。

[!code-csharp[](identity/sample/src/ASPNETv2.1-IdentityDemo/About.cshtml.cs)]

サインイン場合、サインアウトします。アプリを実行し、選択、**について**リンク。 ログイン ページにリダイレクトされます。

::: moniker range=">= aspnetcore-2.1"

### <a name="explore-identity"></a>Id を調べる

Identity をさらに詳しく調査。

* [完全な id の UI のソースを作成します。](xref:security/authentication/scaffold-identity#create-full-identity-ui-source)
* 各ページと、デバッガーをステップのソースを確認します。

::: moniker-end

## <a name="identity-components"></a>Id コンポーネント

::: moniker range=">= aspnetcore-2.1"

すべての Id 依存の NuGet パッケージが含まれている、 [Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app)します。

::: moniker-end

プライマリ パッケージの Id が[Microsoft.AspNetCore.Identity](https://www.nuget.org/packages/Microsoft.AspNetCore.Identity/)します。 このパッケージは、ASP.NET Core Identity のインターフェイスのコア セットが含まれていてに含まれている`Microsoft.AspNetCore.Identity.EntityFrameworkCore`します。

## <a name="migrating-to-aspnet-core-identity"></a>ASP.NET Core Identity に移行します。

詳細と、既存の Id ストアを移行のガイダンスについては、次を参照してください。[移行の認証と Id](xref:migration/identity)します。

## <a name="setting-password-strength"></a>パスワードの強度を設定

参照してください[構成](#pw)のパスワードの最小要件を設定するサンプルです。

## <a name="next-steps"></a>次の手順

* [Identity の構成](xref:security/authentication/identity-configuration)
* <xref:security/authorization/secure-data>
* <xref:security/authentication/add-user-data>
* <xref:security/authentication/identity-enable-qrcodes>
* <xref:migration/identity>
* <xref:security/authentication/accconfirm>
* <xref:security/authentication/2fa>
* <xref:host-and-deploy/web-farm>
