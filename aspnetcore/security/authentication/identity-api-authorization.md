---
title: ASP.NET Core でのシングル ページ アプリケーションの認証の概要
author: ''
description: ASP.NET Core アプリ内でホストされているシングル ページ アプリケーションでは、Id を使用します。
ms.author: ''
ms.date: 03/05/2018
uid: security/authentication/identity/spa
ms.openlocfilehash: cf04ec1ff0ae9afea066fd1864ab0a7956ace32c
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346787"
---
# <a name="authentication-and-authorization-for-spas"></a><span data-ttu-id="d8106-103">Spa の認証と承認</span><span class="sxs-lookup"><span data-stu-id="d8106-103">Authentication and authorization for SPAs</span></span>

<span data-ttu-id="d8106-104">ASP.NET 3.0、新しいサポート API の authorization を使用するシングル ページ アプリケーションでの認証のサポートが導入されました。</span><span class="sxs-lookup"><span data-stu-id="d8106-104">In ASP.NET 3.0 we are introducing support for authentication in single page applications using our new support for API authorization.</span></span> <span data-ttu-id="d8106-105">このサポートを認証および Open ID Connect を実装するためのユーザーと Identity Server を格納する ASP.NET Core Identity の組み合わせに基づきます。</span><span class="sxs-lookup"><span data-stu-id="d8106-105">This support is based on a combination of ASP.NET Core Identity for authenticating and storing users and Identity Server for implementing Open ID Connect.</span></span>

<span data-ttu-id="d8106-106">新しい認証パラメーターが追加されました、Angular にや React テンプレートは、mvc および razor ページのテンプレートでの認証パラメーターに似ていますが許容値 'None'、'個人'。</span><span class="sxs-lookup"><span data-stu-id="d8106-106">We have added a new authentication parameter to our Angular and React templates that is similar to the authentication parameter in our mvc and razor pages templates with allowed values 'None' and 'Individual'.</span></span>

## <a name="create-an-angular-app-with-api-authorization-support"></a><span data-ttu-id="d8106-107">API 認証のサポート、Angular アプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-107">Create an Angular app with API authorization support</span></span>

<span data-ttu-id="d8106-108">でユーザーの認証と承認のサポートを新しい Angular アプリを作成するには、コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d8106-108">To create a new Angular app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new angular -o <output_directory_name> -au Individual
```

<span data-ttu-id="d8106-109">上記のコマンドで ASP.NET Core アプリを作成し、 *ClientApp* Angular アプリを含むディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d8106-109">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the Angular app.</span></span>

## <a name="create-a-react-app-with-api-authorization-support"></a><span data-ttu-id="d8106-110">API 認証のサポートと React のアプリを作成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-110">Create a React app with API authorization support</span></span>

<span data-ttu-id="d8106-111">でユーザーの認証と承認のサポートを新しい React のアプリを作成するには、コマンド シェルを開き、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="d8106-111">To create a new React app with support for authentication and authorization of users, open a command shell and run the following command:</span></span>

```console
dotnet new react -o <output_directory_name> -au Individual
```

<span data-ttu-id="d8106-112">上記のコマンドで ASP.NET Core アプリを作成し、 *ClientApp* React のアプリを含むディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="d8106-112">The preceding command creates an ASP.NET Core app with a *ClientApp* directory containing the React app.</span></span>

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a><span data-ttu-id="d8106-113">アプリの ASP.NET Core コンポーネントの一般的な説明</span><span class="sxs-lookup"><span data-stu-id="d8106-113">General description of the ASP.NET Core components of the app</span></span>

<span data-ttu-id="d8106-114">プロジェクトにいくつかの点がある認証のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="d8106-114">There are several additions to the project when we include support for authentication:</span></span>

### <a name="startup-class"></a><span data-ttu-id="d8106-115">Startup クラス</span><span class="sxs-lookup"><span data-stu-id="d8106-115">Startup class</span></span>

<span data-ttu-id="d8106-116">次のスタートアップ クラスのコードを見て場合は、次のインクルードご提案できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-116">If we look at the code in the Startup class below we can appreciate the following inclusions:</span></span>
* <span data-ttu-id="d8106-117">内部`public void ConfigureServices(IServiceCollection services)`:</span><span class="sxs-lookup"><span data-stu-id="d8106-117">Inside `public void ConfigureServices(IServiceCollection services)`:</span></span>
  * <span data-ttu-id="d8106-118">既定の UI を持つ id。</span><span class="sxs-lookup"><span data-stu-id="d8106-118">Identity with the default UI.</span></span>
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * <span data-ttu-id="d8106-119">追加の AddApiAuthorization ヘルパー メソッドでの identity Server をセットアップいくつかの既定の Identity Server 上の ASP.NET 規則。</span><span class="sxs-lookup"><span data-stu-id="d8106-119">Identity Server with an additional AddApiAuthorization helper method that setups some default ASP.NET Conventions on top of Identity Server.</span></span>
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * <span data-ttu-id="d8106-120">Identity Server で生成される Jwt トークンを検証するアプリケーションを構成する追加の AddIdentityServerJwt ヘルパー メソッドを使用して認証します。</span><span class="sxs-lookup"><span data-stu-id="d8106-120">Authentication with an additional AddIdentityServerJwt helper method that configures the application to validate Jwt tokens produced by Identity Server.</span></span> 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* <span data-ttu-id="d8106-121">内部`public void Configure(IApplicationBuilder app)`:</span><span class="sxs-lookup"><span data-stu-id="d8106-121">Inside `public void Configure(IApplicationBuilder app)`:</span></span>
  * <span data-ttu-id="d8106-122">受信要求で資格情報を検証および要求のコンテキストでユーザーを設定するため、認証ミドルウェアです。</span><span class="sxs-lookup"><span data-stu-id="d8106-122">The authentication middleware that is responsible for validating the credentials in the incoming request and setting the user on the request context.</span></span>
    ```csharp
    app.UseAuthentication();
    ```
  * <span data-ttu-id="d8106-123">Open ID Connect エンドポイントを公開する identity server ミドルウェア。</span><span class="sxs-lookup"><span data-stu-id="d8106-123">The identity server middleware that exposes the Open ID Connect endpoints.</span></span>
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a><span data-ttu-id="d8106-124">AddApiAuthorization</span><span class="sxs-lookup"><span data-stu-id="d8106-124">AddApiAuthorization</span></span> 
<span data-ttu-id="d8106-125">このヘルパー メソッドは、サポートされている構成を使用する Id のサーバーを構成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-125">This helper method configures Identity Server to use our supported configuration.</span></span> <span data-ttu-id="d8106-126">Identity Server では、アプリケーションのセキュリティに関する注意事項を処理するための非常に強力で拡張可能なフレームワークが同時に、最も一般的なシナリオについて知っておく必要がありますしない複雑さの多くを公開するため選択、一連の規則とだと該当する構成オプションは、出発点として適しています。</span><span class="sxs-lookup"><span data-stu-id="d8106-126">Identity Server is a very powerful and extensible framework for handling application security concerns but at the same time that exposes a lot of complexity that we don't need to know about for the most common scenarios, so we choose a set of conventions and configuration options for you that we consider are a good starting point.</span></span> <span data-ttu-id="d8106-127">認証のニーズの変化と Identity Server の能力を最大限の値は、ニーズに合わせてカスタマイズするためも使用できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-127">Once your authentication needs change the full power of Identity Server is still available to you so you can customize it to suit your needs.</span></span>

### <a name="addidentityserverjwt"></a><span data-ttu-id="d8106-128">AddIdentityServerJwt</span><span class="sxs-lookup"><span data-stu-id="d8106-128">AddIdentityServerJwt</span></span>
<span data-ttu-id="d8106-129">このヘルパー メソッドは、既定の認証ハンドラーとして、アプリケーションのポリシー設定を構成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-129">This helper method configures a policy scheme for the application as the default authentication handler.</span></span> <span data-ttu-id="d8106-130">Identity Id url 空間で任意のサブパスに移動するすべての要求を処理できるようにするポリシーが構成されている"/Identity"と JwtBearerHandler の他のすべての要求を処理できるようにします。</span><span class="sxs-lookup"><span data-stu-id="d8106-130">The policy is configured to let identity handle all the requests that go to any subpath in the Identity url space "/Identity" and to let the JwtBearerHandler handle all other requests.</span></span>
<span data-ttu-id="d8106-131">Addionally このメソッドは、登録、 `<<ApplicationName>>API` Api リソースの identity server の既定のスコープを持つ`<<ApplicationName>>API`し、アプリケーションの Id サーバーによって発行されたトークンを検証する JWT ベアラ トークン ミドルウェアを構成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-131">Addionally this method registers an `<<ApplicationName>>API` Api resource with identity server with a default scope of `<<ApplicationName>>API` and configures the JWT Bearer token middleware to validate tokens issued by Identity Server for the application.</span></span>

### <a name="sampledatacontroller"></a><span data-ttu-id="d8106-132">SampleDataController</span><span class="sxs-lookup"><span data-stu-id="d8106-132">SampleDataController</span></span>
<span data-ttu-id="d8106-133">確認できます Controllers\SampleDataController.cs ファイルで見た場合、`[Authorize]`リソースにアクセスする既定のポリシーに基づいて、ユーザーを承認する必要があることを示すクラスに適用される属性。</span><span class="sxs-lookup"><span data-stu-id="d8106-133">If we look at the file Controllers\SampleDataController.cs we can observe the `[Authorize]` attribute applied to the class that indicates that the user needs to be authorized based on the default policy to access the resource.</span></span> <span data-ttu-id="d8106-134">既定の承認ポリシーによって設定される既定の認証スキームを使用するように構成が`AddIdentityServerJwt`上記で説明したしたポリシー構成、JwtBearer ハンドラーを作成して構成このようなヘルパー メソッドの既定のハンドラーアプリに要求します。</span><span class="sxs-lookup"><span data-stu-id="d8106-134">The default authorization policy happens to be configured to use the default authentication scheme which is set up by `AddIdentityServerJwt` to the policy scheme that we mentioned above, making the JwtBearer handler configured by such helper method the default handler for requests to the app.</span></span>

### <a name="applicationdbcontext"></a><span data-ttu-id="d8106-135">ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="d8106-135">ApplicationDbContext</span></span>
<span data-ttu-id="d8106-136">Data\ApplicationDbContext.cs でファイルを見てわかります ApiAuthorizationDbContext (IdentityDbContext からより強い派生クラス) を拡張 id、例外をで使用する同じ DbContext Identity Server のスキーマを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="d8106-136">If we look at the file in Data\ApplicationDbContext.cs we can see the same DbContext we use in identity with the exception that it extends ApiAuthorizationDbContext (a more derived class from IdentityDbContext) to include the schema for Identity Server.</span></span>
<span data-ttu-id="d8106-137">データベース スキーマを完全に制御する場合だけです利用可能な Identity DbContext クラスのいずれかから継承して呼び出すことによって id スキーマを含めるコンテキストの構成、`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上、`OnModelCreating`メソッド。</span><span class="sxs-lookup"><span data-stu-id="d8106-137">If we want full control of the database schema we can simply inherit from one of the available Identity DbContext classes and configure the context to include the identity schema by calling `builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)` on the `OnModelCreating` method.</span></span>

### <a name="oidcconfigurationcontroller"></a><span data-ttu-id="d8106-138">OidcConfigurationController</span><span class="sxs-lookup"><span data-stu-id="d8106-138">OidcConfigurationController</span></span>
<span data-ttu-id="d8106-139">見た場合、ファイル Controllers\OidcConfigurationController.cs エンドポイントを参照してくださいそのクライアントが使用する必要がある OIDC パラメーターを処理するためにスタンド アップします。</span><span class="sxs-lookup"><span data-stu-id="d8106-139">If we look at the file Controllers\OidcConfigurationController.cs we can see the endpoint that we stand-up to serve the OIDC parameters that the client needs to use.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="d8106-140">appsettings.json</span><span class="sxs-lookup"><span data-stu-id="d8106-140">appsettings.json</span></span>
<span data-ttu-id="d8106-141">場合は、プロジェクトのルートで appsettings.json ファイルを見ると、新しいわかります`IdentityServer`セクションの一覧を記述するには、クライアントが構成されているし、1 つのクライアントがあることがわかります。</span><span class="sxs-lookup"><span data-stu-id="d8106-141">If we look at the appsettings.json file on the root of the project, we can see a new `IdentityServer` section that describes the list of configured clients and we can see that there is a single client.</span></span> <span data-ttu-id="d8106-142">クライアントの名前は、アプリケーションの名前に対応し、oAuth クライアント Id パラメーターに規約によってマップされます。</span><span class="sxs-lookup"><span data-stu-id="d8106-142">The name of the client corresponds to the name of the application and is mapped by convention to the oAuth ClientId parameter.</span></span> <span data-ttu-id="d8106-143">プロファイルはを構成すると、アプリケーションの種類を示し、それを使用して内部的には、サーバーの構成プロセスを簡略化されるドライブの表記規則。</span><span class="sxs-lookup"><span data-stu-id="d8106-143">The profile indicates what type of application we are configuring, and we use it internally to drive conventions that simplify the configuration process for the server.</span></span> <span data-ttu-id="d8106-144">以下のセクションで説明されたいくつか使用可能なプロファイルがあります。</span><span class="sxs-lookup"><span data-stu-id="d8106-144">There are several profiles available explained in the section below.</span></span>

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a><span data-ttu-id="d8106-145">appsettings します。Development.json</span><span class="sxs-lookup"><span data-stu-id="d8106-145">appsettings.Development.json</span></span>
<span data-ttu-id="d8106-146">場合は、appsettings に注目します。Development.json ファイル、プロジェクトのルートで新しいわかります`IdentityServer`はトークンの署名に使用するキーについて説明したセクション。</span><span class="sxs-lookup"><span data-stu-id="d8106-146">If we look at the appsettings.Development.json file on the root of the project, we can see a new `IdentityServer` section that describes the key we are using to sign tokens.</span></span> <span data-ttu-id="d8106-147">運用環境にデプロイするときにキーをプロビジョニングし、次に示すように、アプリケーションと共にデプロイする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-147">When deploying to production a key needs to be provisioned and deployed alongside the application as explained below.</span></span>

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a><span data-ttu-id="d8106-148">Angular アプリケーションの一般的な説明</span><span class="sxs-lookup"><span data-stu-id="d8106-148">General description of the Angular application</span></span>
<span data-ttu-id="d8106-149">認証と Angular テンプレートでの API の承認のサポートは、独自の Angular モジュールに住んでいます。</span><span class="sxs-lookup"><span data-stu-id="d8106-149">The support for authentication and API authorization in the Angular template lives in its own Angular module.</span></span> <span data-ttu-id="d8106-150">ClientApp\src\api 承認し、これは、次の要素で構成されます。</span><span class="sxs-lookup"><span data-stu-id="d8106-150">Under ClientApp\src\api-authorization and it is composed of the following elements:</span></span>
* <span data-ttu-id="d8106-151">3 つのコンポーネント:</span><span class="sxs-lookup"><span data-stu-id="d8106-151">3 Components:</span></span>
  * <span data-ttu-id="d8106-152">ログインのコンポーネント:アプリのログイン フローを処理します。</span><span class="sxs-lookup"><span data-stu-id="d8106-152">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="d8106-153">ログアウト コンポーネント:アプリケーションのログアウト フローを処理します。</span><span class="sxs-lookup"><span data-stu-id="d8106-153">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="d8106-154">ログイン メニュー コンポーネント:現在を表示するウィジェットは、ユーザー プロファイルの管理し、ログアウト リンクを持つユーザーを認証またはリンク ログインまたはユーザーが認証されていないときに登録します。</span><span class="sxs-lookup"><span data-stu-id="d8106-154">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
* <span data-ttu-id="d8106-155">ルート guard`AuthorizeGuard`をルートに追加することができ、ルートにアクセスする前に認証するユーザーが必要です。</span><span class="sxs-lookup"><span data-stu-id="d8106-155">A route guard `AuthorizeGuard` that can be added to routes and requires a user to be authenticated before visiting the route.</span></span>
* <span data-ttu-id="d8106-156">Http インターセプター`AuthorizeInterceptor`ユーザーが認証されるときに、API を対象とする HTTP 要求を送信するアクセス トークンをアタッチします。</span><span class="sxs-lookup"><span data-stu-id="d8106-156">An http interceptor `AuthorizeInterceptor` that attaches the access token to outgoing HTTP requests targeting the API when the user is authenticated.</span></span>
* <span data-ttu-id="d8106-157">サービス`AuthorizeService`を認証プロセスの下位レベルの詳細を処理し、使用するため、アプリケーションの残りの部分に認証されたユーザーに関する情報を公開します。</span><span class="sxs-lookup"><span data-stu-id="d8106-157">A service `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>
* <span data-ttu-id="d8106-158">アプリケーションの認証部分に関連付けられているルートを定義し、ログイン] メニューの [コンポーネント、インターセプター、ガードおよび他のアプリケーションから消費量のサービスを公開する angular モジュール。</span><span class="sxs-lookup"><span data-stu-id="d8106-158">An angular module that defines routes associated with the authentication parts of the application and exposes the login menu component, the interceptor, the guard and the service for consumption from the rest of the application.</span></span>

## <a name="general-description-of-the-react-application"></a><span data-ttu-id="d8106-159">React のアプリケーションの一般的な説明</span><span class="sxs-lookup"><span data-stu-id="d8106-159">General description of the React application</span></span>
<span data-ttu-id="d8106-160">認証と API の承認 ClientApp\src\components\api authorization\ とその React テンプレート生活がどれほどのサポートは、次の要素で構成されます。</span><span class="sxs-lookup"><span data-stu-id="d8106-160">The support for authentication and API authorization in the React template lives under ClientApp\src\components\api-authorization\ and it is composed of the following elements:</span></span>
* <span data-ttu-id="d8106-161">4 個のコンポーネント:</span><span class="sxs-lookup"><span data-stu-id="d8106-161">4 Components:</span></span>
  * <span data-ttu-id="d8106-162">ログインのコンポーネント:アプリのログイン フローを処理します。</span><span class="sxs-lookup"><span data-stu-id="d8106-162">Login component: Handles the login flow for the app.</span></span>
  * <span data-ttu-id="d8106-163">ログアウト コンポーネント:アプリケーションのログアウト フローを処理します。</span><span class="sxs-lookup"><span data-stu-id="d8106-163">Logout component: Handles the logout flow for the app.</span></span>
  * <span data-ttu-id="d8106-164">ログイン メニュー コンポーネント:現在を表示するウィジェットは、ユーザー プロファイルの管理し、ログアウト リンクを持つユーザーを認証またはリンク ログインまたはユーザーが認証されていないときに登録します。</span><span class="sxs-lookup"><span data-stu-id="d8106-164">Login menu component: A widget that displays the current authenticated user with links to manage the user profile and log out or links to log in or register when the user is not authenticated.</span></span>
  * <span data-ttu-id="d8106-165">AuthorizeRoute:コンポーネントを表示する前に認証するユーザーを必要とするルート コンポーネントは、Component パラメーターで示されます。</span><span class="sxs-lookup"><span data-stu-id="d8106-165">AuthorizeRoute: A route component that requires a user to be authenticated before rendering the component indicated in the Component parameter.</span></span>
  * <span data-ttu-id="d8106-166">エクスポートした`authService`クラスのインスタンス`AuthorizeService`を認証プロセスの下位レベルの詳細を処理し、使用するため、アプリケーションの残りの部分に認証されたユーザーに関する情報を公開します。</span><span class="sxs-lookup"><span data-stu-id="d8106-166">An exported `authService` instance of class `AuthorizeService` that handles the lower level details of the authentication process and exposes information about the authenticated user to the rest of the application for consumption.</span></span>

<span data-ttu-id="d8106-167">できたので、ソリューションの主要なコンポーネントしたように、アプリケーションの個々 のシナリオに固有の外観を実行することができます。</span><span class="sxs-lookup"><span data-stu-id="d8106-167">Now that we've seen the main components of the solution, we can take a specific look at individual scenarios for the application:</span></span>

## <a name="requiring-authorization-on-a-new-api"></a><span data-ttu-id="d8106-168">新しい API での承認を必要とします。</span><span class="sxs-lookup"><span data-stu-id="d8106-168">Requiring authorization on a new API</span></span>
<span data-ttu-id="d8106-169">システムは、新しい Api の承認を要求するように簡単にする ボックスから構成されます。</span><span class="sxs-lookup"><span data-stu-id="d8106-169">The system is configured out of the box to make it trivial to require authorization for new APIs.</span></span> <span data-ttu-id="d8106-170">そのために、単に新しいコント ローラーを作成および追加、`[Authorize]`属性をコント ローラー クラスまたはコント ローラー内の任意のアクション。</span><span class="sxs-lookup"><span data-stu-id="d8106-170">In order to do so, simply create a new controller and add the `[Authorize]` attribute to the controller class or to any action within the controller.</span></span>

## <a name="protecting-a-client-side-route-angular"></a><span data-ttu-id="d8106-171">クライアント側ルート (Angular) を保護します。</span><span class="sxs-lookup"><span data-stu-id="d8106-171">Protecting a client-side route (Angular)</span></span>
<span data-ttu-id="d8106-172">クライアント側のルートの保護は、ルートを構成するときに実行する警備員の一覧に、承認の保護を追加することで行われます。</span><span class="sxs-lookup"><span data-stu-id="d8106-172">Protecting a client side route is done by adding the authorize guard to the list of guards to run when configuring a route.</span></span> <span data-ttu-id="d8106-173">例としては、メイン アプリケーションの angular モジュール内でのデータのフェッチのルートを構成する方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-173">As an example you can see how the fetch-data route is configured within the main app angular module:</span></span>

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

<span data-ttu-id="d8106-174">ルートを保護すると、実際のエンドポイントが保護しないことは言うまでも重要です (まだ必要があります、`[Authorize]`属性が適用されて) が認証されていない場合は、特定のクライアント側のルートに移動することから、ユーザーのみができないようにするがします。</span><span class="sxs-lookup"><span data-stu-id="d8106-174">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-angular"></a><span data-ttu-id="d8106-175">API の要求 (Angular) の認証します。</span><span class="sxs-lookup"><span data-stu-id="d8106-175">Authenticate API requests (Angular)</span></span>

<span data-ttu-id="d8106-176">アプリケーションで定義される HTTP クライアントのインターセプターを使用して、アプリケーションが自動的に行われますの辺にホストされている Api に要求の認証。</span><span class="sxs-lookup"><span data-stu-id="d8106-176">Authenticating requests to APIs hosted along side the application is done automatically through the use of the HTTP client interceptor defined by the application.</span></span>

## <a name="protect-a-client-side-route-react"></a><span data-ttu-id="d8106-177">クライアント側ルート (React) を保護します。</span><span class="sxs-lookup"><span data-stu-id="d8106-177">Protect a client-side route (React)</span></span>

<span data-ttu-id="d8106-178">クライアント側のルートの保護は、プレーンなルート コンポーネントの代わりに、AuthorizeRoute コンポーネントを使用して行われます。</span><span class="sxs-lookup"><span data-stu-id="d8106-178">Protecting a client side route is done by using the AuthorizeRoute component instead of the plain Route component.</span></span> <span data-ttu-id="d8106-179">例として、アプリ コンポーネント内のデータのフェッチのルートを構成する方法を確認できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-179">As an example you can see how the fetch-data route is configured within the App component:</span></span>

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

<span data-ttu-id="d8106-180">ルートを保護すると、実際のエンドポイントが保護しないことは言うまでも重要です (まだ必要があります、`[Authorize]`属性が適用されて) が認証されていない場合は、特定のクライアント側のルートに移動することから、ユーザーのみができないようにするがします。</span><span class="sxs-lookup"><span data-stu-id="d8106-180">It is important to mention that protecting a route doesn't protect the actual endpoint (which still requires an `[Authorize]` attribute applied to it) but that it only prevents the user from navigating to the given client side route when it is not authenticated.</span></span>

## <a name="authenticate-api-requests-react"></a><span data-ttu-id="d8106-181">API の要求 (React) の認証します。</span><span class="sxs-lookup"><span data-stu-id="d8106-181">Authenticate API requests (React)</span></span>

<span data-ttu-id="d8106-182">最初のインポートを行うを react での要求の認証、`authService`インスタンスから、 `AuthorizeService` authService からアクセス トークンを取得して、次に示すように、要求にアタッチします。</span><span class="sxs-lookup"><span data-stu-id="d8106-182">Authenticating requests with react is done by first importing the `authService` instance from the `AuthorizeService` and then retrieving the access token from the authService and attaching it to the request as shown below.</span></span> <span data-ttu-id="d8106-183">React コンポーネントで通常これは、componentDidMount ライフ サイクル メソッドで、または結果としていくつかのユーザー操作からです。</span><span class="sxs-lookup"><span data-stu-id="d8106-183">In react components this is typically done in the componentDidMount lifecycle method or as the result from some user interaction.</span></span>

### <a name="import-the-authservice-into-your-component"></a><span data-ttu-id="d8106-184">コンポーネントへのインポート、authService</span><span class="sxs-lookup"><span data-stu-id="d8106-184">Import the authService into your component</span></span>

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a><span data-ttu-id="d8106-185">取得し、アクセス トークンを応答にアタッチ</span><span class="sxs-lookup"><span data-stu-id="d8106-185">Retrieve and attach the access token to the response</span></span>

```js
async populateWeatherData() {
  const token = await authService.getAccessToken();
  const response = await fetch('api/SampleData/WeatherForecasts', {
    headers: !token ? {} : { 'Authorization': `Bearer ${token}` }
  });
  const data = await response.json();
  this.setState({ forecasts: data, loading: false });
}
```

## <a name="deploy-into-production"></a><span data-ttu-id="d8106-186">運用環境にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d8106-186">Deploy into production</span></span>

<span data-ttu-id="d8106-187">運用環境にアプリケーションを展開するためには、いくつかのリソースをプロビジョニングする必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-187">In order to deploy the application into production we need to provision several resources:</span></span>
* <span data-ttu-id="d8106-188">Id のユーザー アカウントと id のサーバーに格納するデータベースを許可します。</span><span class="sxs-lookup"><span data-stu-id="d8106-188">A database to store the Identity user accounts and the identity server grants.</span></span>
* <span data-ttu-id="d8106-189">トークンの署名に使用する運用証明書。</span><span class="sxs-lookup"><span data-stu-id="d8106-189">A production certificate to use for signing tokens.</span></span>
  * <span data-ttu-id="d8106-190">この証明書の特定の要件はありません。自己署名証明書または CA 機関を使用してプロビジョニング証明書を指定できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-190">There are no specific requirements for this certificate; it can be a self-signed certificate or a certificate provisioned through a CA authority.</span></span>
  * <span data-ttu-id="d8106-191">Powershell または openssl などの標準のツールによって生成できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-191">It can be generated through standard tools like powershell or openssl.</span></span>
  * <span data-ttu-id="d8106-192">ターゲット コンピューターの証明書ストアにインストールされているまたは強力なパスワードを含む pfx ファイルとしてデプロイされていることができます。</span><span class="sxs-lookup"><span data-stu-id="d8106-192">It can be installed into the certificate store on the target machines or deployed as a pfx file with a strong password.</span></span>

### <a name="example-deploy-into-azure-websites"></a><span data-ttu-id="d8106-193">例:Azure Websites にデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d8106-193">Example: Deploy into Azure Websites</span></span>

<span data-ttu-id="d8106-194">このセクションでは、証明書ストアに格納されている証明書を使用して Azure websites にアプリケーションをデプロイするいきます。</span><span class="sxs-lookup"><span data-stu-id="d8106-194">In this section we are going to deploy the application to Azure websites using a certificate stored in the certificate store.</span></span> <span data-ttu-id="d8106-195">証明書ストアから、証明書を読み込むアプリケーションを変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-195">We need to modify the application to load a certicate from the certificate store.</span></span> <span data-ttu-id="d8106-196">これを行うには、app service プランを後の手順で設定するときに、standard レベルで以上である必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-196">To do so, our app service plan needs to be at least on the standard tier when we configure in a later step.</span></span> <span data-ttu-id="d8106-197">アプリケーションでは単に IdentityServer セクションにキーの詳細を含める appsettings.json を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-197">In our application we simply need to modify the IdentityServer section on appsettings.json to include the key details:</span></span>
```json
  "IdentityServer": {
    "Key": {
      "Type": "Store",
      "StoreName": "My",
      "StoreLocation": "CurrentUser",
      "Name": "CN=MyApplication"
    }
  }
}
```
* <span data-ttu-id="d8106-198">証明書の名前プロパティは、識別証明書の件名と一致します。</span><span class="sxs-lookup"><span data-stu-id="d8106-198">The name property on certificate corresponds with the distinguished subject for the certificate.</span></span>
* <span data-ttu-id="d8106-199">ストアの場所は、(CurrentUrser または LocalMachine) から証明書を読み込む場所を表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-199">The store location represents where to load the certificate from (CurrentUrser or LocalMachine).</span></span>
* <span data-ttu-id="d8106-200">ストアの名前は、この場合は、ユーザーの個人ストアを指す、証明書を保存する場所、証明書ストアの名前を表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-200">The store name represents the name of the certificate store where the certificate is stored, in this case it points to the personal user store.</span></span>

<span data-ttu-id="d8106-201">Azure Websites にデプロイするには、次の手順では、アプリをデプロイ[アプリを Azure にデプロイ](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)を必要な Azure リソースを作成し、運用環境にアプリをデプロイします。</span><span class="sxs-lookup"><span data-stu-id="d8106-201">To deploy to Azure Websites, deploy the app following the steps in [Deploy the app to Azure](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure) to create the necessary Azure resources and deploy the app to production.</span></span>

<span data-ttu-id="d8106-202">これを行うアプリケーションは Azure にデプロイされますがまだ完全に機能するいないと、アプリケーションで使用される証明書を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="d8106-202">After doing this, the application is deployed into Azure but is not yet completely functional as we still need to setup the certificate to be used by the application.</span></span> <span data-ttu-id="d8106-203">これを行うには、ここで説明した手順を使用して、証明書の拇印を持っている私たち必要があります。[証明書の読み込み](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates)します。</span><span class="sxs-lookup"><span data-stu-id="d8106-203">To do so, we need to have the thumbprint for the certificate we are going to use and follow the steps described in [Load your certificates](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates).</span></span>

<span data-ttu-id="d8106-204">次の手順には、SSL が言うまでも、"プライベート証明書 セクションには、ポータルでアプリを使用する、プロビジョニング証明書をアップロードできます。</span><span class="sxs-lookup"><span data-stu-id="d8106-204">While these steps mention SSL, there is a "Private certificates" section on the portal where we can upload our provisioned certificate to use with our app.</span></span>

<span data-ttu-id="d8106-205">この手順の後、アプリケーションを再起動できる必要があり、完全に機能が必要です。</span><span class="sxs-lookup"><span data-stu-id="d8106-205">After this step, we should be able to restart our application and it should be completely functional.</span></span>

## <a name="other-configuration-options"></a><span data-ttu-id="d8106-206">他の構成オプション</span><span class="sxs-lookup"><span data-stu-id="d8106-206">Other configuration options</span></span>
<span data-ttu-id="d8106-207">API 認証のサポートは、一連の規則、既定値と、シングル ページ アプリケーションのエクスペリエンスを簡略化の機能強化を使用して、Id サーバー上にビルドします。</span><span class="sxs-lookup"><span data-stu-id="d8106-207">Our support for API authorization builds on top of Identity Server with a set of conventions, default values and enhancements to simplify the experience for Single Page Applications.</span></span> <span data-ttu-id="d8106-208">もちろん、Identity Server の能力を最大限はバック グラウンドで使用可能な提供されている統合は、シナリオで説明されていない場合です。</span><span class="sxs-lookup"><span data-stu-id="d8106-208">Needless to say, the full power of Identity Server is available behind the scenes if the integrations that we offer don't cover your scenario.</span></span> <span data-ttu-id="d8106-209">当社のサポートは、いわゆる「ファースト パーティ」アプリケーションは、すべてのアプリケーションを作成し、組織で展開を重視されています。</span><span class="sxs-lookup"><span data-stu-id="d8106-209">Our support is focused on what we call "first-party" applications, where all the applications are created and deployed by our organization.</span></span> <span data-ttu-id="d8106-210">そのための同意やフェデレーションなどに対してサポートを提供しません。</span><span class="sxs-lookup"><span data-stu-id="d8106-210">As such we don't offer support for things like consent or federation.</span></span> <span data-ttu-id="d8106-211">そのようなシナリオの推奨される Identity Server を使用して、次のドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="d8106-211">For those scenarios our recommendation is to use Identity Server and follow their documentation.</span></span>

### <a name="application-profiles"></a><span data-ttu-id="d8106-212">アプリケーション プロファイル</span><span class="sxs-lookup"><span data-stu-id="d8106-212">Application profiles</span></span>
<span data-ttu-id="d8106-213">アプリケーション プロファイルは、さらに、パラメーターを定義するアプリケーションの定義済みの構成を示します。</span><span class="sxs-lookup"><span data-stu-id="d8106-213">Application profiles are predefined configurations for applications that further define their parameters.</span></span> <span data-ttu-id="d8106-214">この時点では、2 つのプロファイルをサポートします。</span><span class="sxs-lookup"><span data-stu-id="d8106-214">At this time we support two profiles:</span></span>
* <span data-ttu-id="d8106-215">IdentityServerSPA:1 つの単位として Identity Server と共にホストされているシングル ページ アプリケーションを表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-215">IdentityServerSPA: Represents a single page application hosted alongside Identity Server as a single unit.</span></span>
  * <span data-ttu-id="d8106-216">Redirect_uri の既定値は`/authentication/login-callback`します。</span><span class="sxs-lookup"><span data-stu-id="d8106-216">The redirect_uri defaults to `/authentication/login-callback`.</span></span>
  * <span data-ttu-id="d8106-217">既定で、post_logout_redirect_uri`/authentication/logout-callback`します。</span><span class="sxs-lookup"><span data-stu-id="d8106-217">The post_logout_redirect_uri defaults to `/authentication/logout-callback`.</span></span>
  * <span data-ttu-id="d8106-218">スコープのセットが含まれています、 `openid`、 `profile`、およびアプリケーションの Api に対して定義されているすべてのスコープ。</span><span class="sxs-lookup"><span data-stu-id="d8106-218">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="d8106-219">許可されている OIDC 応答型のセットは、`id_token token`またはそれぞれ個別に (`id_token`、 `token`)。</span><span class="sxs-lookup"><span data-stu-id="d8106-219">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="d8106-220">許可される応答モードは`fragment`します。</span><span class="sxs-lookup"><span data-stu-id="d8106-220">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="d8106-221">SPA:Identity Server でホストされていない単一ページ アプリケーションを表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-221">SPA: Represents a single page application that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="d8106-222">スコープのセットが含まれています、 `openid`、 `profile`、およびアプリケーションの Api に対して定義されているすべてのスコープ。</span><span class="sxs-lookup"><span data-stu-id="d8106-222">The set of scopes includes the `openid`, `profile`, and every scope defined for the APIs in the application.</span></span>
  * <span data-ttu-id="d8106-223">許可されている OIDC 応答型のセットは、`id_token token`またはそれぞれ個別に (`id_token`、 `token`)。</span><span class="sxs-lookup"><span data-stu-id="d8106-223">The set of allowed OIDC response types is `id_token token` or each of them individually (`id_token`, `token`).</span></span>
  * <span data-ttu-id="d8106-224">許可される応答モードは`fragment`します。</span><span class="sxs-lookup"><span data-stu-id="d8106-224">The allowed response mode is `fragment`.</span></span>
* <span data-ttu-id="d8106-225">IdentityServerJwt:Identity Server でと共にホストされている API を表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-225">IdentityServerJwt: Represents an API that is hosted alongside with Identity Server.</span></span>
  * <span data-ttu-id="d8106-226">アプリケーションはその既定値は、アプリケーション名に 1 つのスコープを持つように構成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-226">The application is configured to have a single scope that defaults to the application name.</span></span>
* <span data-ttu-id="d8106-227">API:Identity Server でホストされていない API を表します。</span><span class="sxs-lookup"><span data-stu-id="d8106-227">API: Represents an API that is not hosted with Identity Server.</span></span>
  * <span data-ttu-id="d8106-228">アプリケーションはその既定値は、アプリケーション名に 1 つのスコープを持つように構成します。</span><span class="sxs-lookup"><span data-stu-id="d8106-228">The application is configured to have a single scope that defaults to the application name.</span></span>

### <a name="configuration-through-appsettings"></a><span data-ttu-id="d8106-229">AppSettings を使用した構成</span><span class="sxs-lookup"><span data-stu-id="d8106-229">Configuration through AppSettings</span></span>
<span data-ttu-id="d8106-230">クライアントやリソースの一覧をそれぞれに追加して、構成システムを通じて、アプリケーションを構成できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-230">We can configure the applications through our configuration system by adding them to the list of Clients or Resources respectively.</span></span> 

<span data-ttu-id="d8106-231">構成できるクライアントを構成するときに、`redirect_uri`と`post_logout_redirect_uri`次に示すよう。</span><span class="sxs-lookup"><span data-stu-id="d8106-231">When configuring clients we can configure the `redirect_uri` and the `post_logout_redirect_uri` as shown below:</span></span>
```json
  "IdentityServer": {
    "Clients": {
      "MySPA": {
        "Profile": "SPA",
        "RedirectUri": "https://www.example.com/authentication/login-callback",
        "LogoutUri": "https://www.example.com/authentication/logout-callback"
      }
    }
  }
```

<span data-ttu-id="d8106-232">リソースを構成するときに、次に示すようには、リソースのスコープを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="d8106-232">When configuring resources we can configure the scopes for the resource as shown below:</span></span>
```json
"IdentityServer": {
  "Resources": {
    "MyExternalApi": {
      "Profile": "API",
      "Scopes": "a b c",
    }
  }
}
```

### <a name="configuration-through-code"></a><span data-ttu-id="d8106-233">コードを使用した構成</span><span class="sxs-lookup"><span data-stu-id="d8106-233">Configuration through code</span></span>
<span data-ttu-id="d8106-234">また、クライアントとオプションを構成するアクションを取る AddApiAuthorization のオーバー ロードを使用してコードを使用したリソースを構成できます。</span><span class="sxs-lookup"><span data-stu-id="d8106-234">We can also configure the clients and resources through code using an overload of AddApiAuthorization that takes an action to configure options.</span></span>
```csharp
AddApiAuthorization<ApplicationUser, ApplicationDbContext>(options =>
{
    options.Clients.AddSPA(
        "My SPA",
        spa => spa.WithRedirectUri("http://www.example.com/authentication/login-callback")
            .WithLogoutRedirectUri("http://www.example.com/authentication/logout-callback"));

    options.ApiResources.AddApiResource("MyExternalApi", resource => resource.WithScopes("a", "b", "c"));
});
```
