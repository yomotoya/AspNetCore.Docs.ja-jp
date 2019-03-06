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
# <a name="authentication-and-authorization-for-spas"></a>Spa の認証と承認

ASP.NET 3.0、新しいサポート API の authorization を使用するシングル ページ アプリケーションでの認証のサポートが導入されました。 このサポートを認証および Open ID Connect を実装するためのユーザーと Identity Server を格納する ASP.NET Core Identity の組み合わせに基づきます。

新しい認証パラメーターが追加されました、Angular にや React テンプレートは、mvc および razor ページのテンプレートでの認証パラメーターに似ていますが許容値 'None'、'個人'。

## <a name="create-an-angular-app-with-api-authorization-support"></a>API 認証のサポート、Angular アプリを作成します。

でユーザーの認証と承認のサポートを新しい Angular アプリを作成するには、コマンド シェルを開き、次のコマンドを実行します。

```console
dotnet new angular -o <output_directory_name> -au Individual
```

上記のコマンドで ASP.NET Core アプリを作成し、 *ClientApp* Angular アプリを含むディレクトリ。

## <a name="create-a-react-app-with-api-authorization-support"></a>API 認証のサポートと React のアプリを作成します。

でユーザーの認証と承認のサポートを新しい React のアプリを作成するには、コマンド シェルを開き、次のコマンドを実行します。

```console
dotnet new react -o <output_directory_name> -au Individual
```

上記のコマンドで ASP.NET Core アプリを作成し、 *ClientApp* React のアプリを含むディレクトリ。

## <a name="general-description-of-the-aspnet-core-components-of-the-app"></a>アプリの ASP.NET Core コンポーネントの一般的な説明

プロジェクトにいくつかの点がある認証のサポートが含まれています。

### <a name="startup-class"></a>Startup クラス

次のスタートアップ クラスのコードを見て場合は、次のインクルードご提案できます。
* 内部`public void ConfigureServices(IServiceCollection services)`:
  * 既定の UI を持つ id。
    ```csharp
    services.AddDbContext<ApplicationDbContext>(options =>
      options.UseSqlite(Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<ApplicationUser>()
      .AddDefaultUI(UIFramework.Bootstrap4)
      .AddEntityFrameworkStores<ApplicationDbContext>();
    ```
  * 追加の AddApiAuthorization ヘルパー メソッドでの identity Server をセットアップいくつかの既定の Identity Server 上の ASP.NET 規則。
    ```csharp
    services.AddIdentityServer()
      .AddApiAuthorization<ApplicationUser, ApplicationDbContext>();
    ```
  * Identity Server で生成される Jwt トークンを検証するアプリケーションを構成する追加の AddIdentityServerJwt ヘルパー メソッドを使用して認証します。 
    ```csharp
    services.AddAuthentication()
      .AddIdentityServerJwt();
    ```
* 内部`public void Configure(IApplicationBuilder app)`:
  * 受信要求で資格情報を検証および要求のコンテキストでユーザーを設定するため、認証ミドルウェアです。
    ```csharp
    app.UseAuthentication();
    ```
  * Open ID Connect エンドポイントを公開する identity server ミドルウェア。
    ```csharp
    app.UseIdentityServer();
    ```

### <a name="addapiauthorization"></a>AddApiAuthorization 
このヘルパー メソッドは、サポートされている構成を使用する Id のサーバーを構成します。 Identity Server では、アプリケーションのセキュリティに関する注意事項を処理するための非常に強力で拡張可能なフレームワークが同時に、最も一般的なシナリオについて知っておく必要がありますしない複雑さの多くを公開するため選択、一連の規則とだと該当する構成オプションは、出発点として適しています。 認証のニーズの変化と Identity Server の能力を最大限の値は、ニーズに合わせてカスタマイズするためも使用できます。

### <a name="addidentityserverjwt"></a>AddIdentityServerJwt
このヘルパー メソッドは、既定の認証ハンドラーとして、アプリケーションのポリシー設定を構成します。 Identity Id url 空間で任意のサブパスに移動するすべての要求を処理できるようにするポリシーが構成されている"/Identity"と JwtBearerHandler の他のすべての要求を処理できるようにします。
Addionally このメソッドは、登録、 `<<ApplicationName>>API` Api リソースの identity server の既定のスコープを持つ`<<ApplicationName>>API`し、アプリケーションの Id サーバーによって発行されたトークンを検証する JWT ベアラ トークン ミドルウェアを構成します。

### <a name="sampledatacontroller"></a>SampleDataController
確認できます Controllers\SampleDataController.cs ファイルで見た場合、`[Authorize]`リソースにアクセスする既定のポリシーに基づいて、ユーザーを承認する必要があることを示すクラスに適用される属性。 既定の承認ポリシーによって設定される既定の認証スキームを使用するように構成が`AddIdentityServerJwt`上記で説明したしたポリシー構成、JwtBearer ハンドラーを作成して構成このようなヘルパー メソッドの既定のハンドラーアプリに要求します。

### <a name="applicationdbcontext"></a>ApplicationDbContext
Data\ApplicationDbContext.cs でファイルを見てわかります ApiAuthorizationDbContext (IdentityDbContext からより強い派生クラス) を拡張 id、例外をで使用する同じ DbContext Identity Server のスキーマを含めることができます。
データベース スキーマを完全に制御する場合だけです利用可能な Identity DbContext クラスのいずれかから継承して呼び出すことによって id スキーマを含めるコンテキストの構成、`builder.ConfigurePersistedGrantContext(_operationalStoreOptions.Value)`上、`OnModelCreating`メソッド。

### <a name="oidcconfigurationcontroller"></a>OidcConfigurationController
見た場合、ファイル Controllers\OidcConfigurationController.cs エンドポイントを参照してくださいそのクライアントが使用する必要がある OIDC パラメーターを処理するためにスタンド アップします。

### <a name="appsettingsjson"></a>appsettings.json
場合は、プロジェクトのルートで appsettings.json ファイルを見ると、新しいわかります`IdentityServer`セクションの一覧を記述するには、クライアントが構成されているし、1 つのクライアントがあることがわかります。 クライアントの名前は、アプリケーションの名前に対応し、oAuth クライアント Id パラメーターに規約によってマップされます。 プロファイルはを構成すると、アプリケーションの種類を示し、それを使用して内部的には、サーバーの構成プロセスを簡略化されるドライブの表記規則。 以下のセクションで説明されたいくつか使用可能なプロファイルがあります。

```json
"IdentityServer": {
  "Clients": {
    "angularindividualpreview3final": {
      "Profile": "IdentityServerSPA"
    }
  }
}
```

### <a name="appsettingsdevelopmentjson"></a>appsettings します。Development.json
場合は、appsettings に注目します。Development.json ファイル、プロジェクトのルートで新しいわかります`IdentityServer`はトークンの署名に使用するキーについて説明したセクション。 運用環境にデプロイするときにキーをプロビジョニングし、次に示すように、アプリケーションと共にデプロイする必要があります。

```json
  "IdentityServer": {
    "Key": {
      "Type": "Development"
    }
  }
}
```

## <a name="general-description-of-the-angular-application"></a>Angular アプリケーションの一般的な説明
認証と Angular テンプレートでの API の承認のサポートは、独自の Angular モジュールに住んでいます。 ClientApp\src\api 承認し、これは、次の要素で構成されます。
* 3 つのコンポーネント:
  * ログインのコンポーネント:アプリのログイン フローを処理します。
  * ログアウト コンポーネント:アプリケーションのログアウト フローを処理します。
  * ログイン メニュー コンポーネント:現在を表示するウィジェットは、ユーザー プロファイルの管理し、ログアウト リンクを持つユーザーを認証またはリンク ログインまたはユーザーが認証されていないときに登録します。
* ルート guard`AuthorizeGuard`をルートに追加することができ、ルートにアクセスする前に認証するユーザーが必要です。
* Http インターセプター`AuthorizeInterceptor`ユーザーが認証されるときに、API を対象とする HTTP 要求を送信するアクセス トークンをアタッチします。
* サービス`AuthorizeService`を認証プロセスの下位レベルの詳細を処理し、使用するため、アプリケーションの残りの部分に認証されたユーザーに関する情報を公開します。
* アプリケーションの認証部分に関連付けられているルートを定義し、ログイン] メニューの [コンポーネント、インターセプター、ガードおよび他のアプリケーションから消費量のサービスを公開する angular モジュール。

## <a name="general-description-of-the-react-application"></a>React のアプリケーションの一般的な説明
認証と API の承認 ClientApp\src\components\api authorization\ とその React テンプレート生活がどれほどのサポートは、次の要素で構成されます。
* 4 個のコンポーネント:
  * ログインのコンポーネント:アプリのログイン フローを処理します。
  * ログアウト コンポーネント:アプリケーションのログアウト フローを処理します。
  * ログイン メニュー コンポーネント:現在を表示するウィジェットは、ユーザー プロファイルの管理し、ログアウト リンクを持つユーザーを認証またはリンク ログインまたはユーザーが認証されていないときに登録します。
  * AuthorizeRoute:コンポーネントを表示する前に認証するユーザーを必要とするルート コンポーネントは、Component パラメーターで示されます。
  * エクスポートした`authService`クラスのインスタンス`AuthorizeService`を認証プロセスの下位レベルの詳細を処理し、使用するため、アプリケーションの残りの部分に認証されたユーザーに関する情報を公開します。

できたので、ソリューションの主要なコンポーネントしたように、アプリケーションの個々 のシナリオに固有の外観を実行することができます。

## <a name="requiring-authorization-on-a-new-api"></a>新しい API での承認を必要とします。
システムは、新しい Api の承認を要求するように簡単にする ボックスから構成されます。 そのために、単に新しいコント ローラーを作成および追加、`[Authorize]`属性をコント ローラー クラスまたはコント ローラー内の任意のアクション。

## <a name="protecting-a-client-side-route-angular"></a>クライアント側ルート (Angular) を保護します。
クライアント側のルートの保護は、ルートを構成するときに実行する警備員の一覧に、承認の保護を追加することで行われます。 例としては、メイン アプリケーションの angular モジュール内でのデータのフェッチのルートを構成する方法を確認できます。

```ts
RouterModule.forRoot([
  // ...
  { path: 'fetch-data', component: FetchDataComponent, canActivate: [AuthorizeGuard] },
])
```

ルートを保護すると、実際のエンドポイントが保護しないことは言うまでも重要です (まだ必要があります、`[Authorize]`属性が適用されて) が認証されていない場合は、特定のクライアント側のルートに移動することから、ユーザーのみができないようにするがします。

## <a name="authenticate-api-requests-angular"></a>API の要求 (Angular) の認証します。

アプリケーションで定義される HTTP クライアントのインターセプターを使用して、アプリケーションが自動的に行われますの辺にホストされている Api に要求の認証。

## <a name="protect-a-client-side-route-react"></a>クライアント側ルート (React) を保護します。

クライアント側のルートの保護は、プレーンなルート コンポーネントの代わりに、AuthorizeRoute コンポーネントを使用して行われます。 例として、アプリ コンポーネント内のデータのフェッチのルートを構成する方法を確認できます。

```jsx
<AuthorizeRoute path='/fetch-data' component={FetchData} />
```

ルートを保護すると、実際のエンドポイントが保護しないことは言うまでも重要です (まだ必要があります、`[Authorize]`属性が適用されて) が認証されていない場合は、特定のクライアント側のルートに移動することから、ユーザーのみができないようにするがします。

## <a name="authenticate-api-requests-react"></a>API の要求 (React) の認証します。

最初のインポートを行うを react での要求の認証、`authService`インスタンスから、 `AuthorizeService` authService からアクセス トークンを取得して、次に示すように、要求にアタッチします。 React コンポーネントで通常これは、componentDidMount ライフ サイクル メソッドで、または結果としていくつかのユーザー操作からです。

### <a name="import-the-authservice-into-your-component"></a>コンポーネントへのインポート、authService

```js
import authService from './api-authorization/AuthorizeService'
```

### <a name="retrieve-and-attach-the-access-token-to-the-response"></a>取得し、アクセス トークンを応答にアタッチ

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

## <a name="deploy-into-production"></a>運用環境にデプロイします。

運用環境にアプリケーションを展開するためには、いくつかのリソースをプロビジョニングする必要があります。
* Id のユーザー アカウントと id のサーバーに格納するデータベースを許可します。
* トークンの署名に使用する運用証明書。
  * この証明書の特定の要件はありません。自己署名証明書または CA 機関を使用してプロビジョニング証明書を指定できます。
  * Powershell または openssl などの標準のツールによって生成できます。
  * ターゲット コンピューターの証明書ストアにインストールされているまたは強力なパスワードを含む pfx ファイルとしてデプロイされていることができます。

### <a name="example-deploy-into-azure-websites"></a>例:Azure Websites にデプロイします。

このセクションでは、証明書ストアに格納されている証明書を使用して Azure websites にアプリケーションをデプロイするいきます。 証明書ストアから、証明書を読み込むアプリケーションを変更する必要があります。 これを行うには、app service プランを後の手順で設定するときに、standard レベルで以上である必要があります。 アプリケーションでは単に IdentityServer セクションにキーの詳細を含める appsettings.json を変更する必要があります。
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
* 証明書の名前プロパティは、識別証明書の件名と一致します。
* ストアの場所は、(CurrentUrser または LocalMachine) から証明書を読み込む場所を表します。
* ストアの名前は、この場合は、ユーザーの個人ストアを指す、証明書を保存する場所、証明書ストアの名前を表します。

Azure Websites にデプロイするには、次の手順では、アプリをデプロイ[アプリを Azure にデプロイ](xref:tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)を必要な Azure リソースを作成し、運用環境にアプリをデプロイします。

これを行うアプリケーションは Azure にデプロイされますがまだ完全に機能するいないと、アプリケーションで使用される証明書を設定する必要があります。 これを行うには、ここで説明した手順を使用して、証明書の拇印を持っている私たち必要があります。[証明書の読み込み](/azure/app-service/app-service-web-ssl-cert-load#load-your-certificates)します。

次の手順には、SSL が言うまでも、"プライベート証明書 セクションには、ポータルでアプリを使用する、プロビジョニング証明書をアップロードできます。

この手順の後、アプリケーションを再起動できる必要があり、完全に機能が必要です。

## <a name="other-configuration-options"></a>他の構成オプション
API 認証のサポートは、一連の規則、既定値と、シングル ページ アプリケーションのエクスペリエンスを簡略化の機能強化を使用して、Id サーバー上にビルドします。 もちろん、Identity Server の能力を最大限はバック グラウンドで使用可能な提供されている統合は、シナリオで説明されていない場合です。 当社のサポートは、いわゆる「ファースト パーティ」アプリケーションは、すべてのアプリケーションを作成し、組織で展開を重視されています。 そのための同意やフェデレーションなどに対してサポートを提供しません。 そのようなシナリオの推奨される Identity Server を使用して、次のドキュメントです。

### <a name="application-profiles"></a>アプリケーション プロファイル
アプリケーション プロファイルは、さらに、パラメーターを定義するアプリケーションの定義済みの構成を示します。 この時点では、2 つのプロファイルをサポートします。
* IdentityServerSPA:1 つの単位として Identity Server と共にホストされているシングル ページ アプリケーションを表します。
  * Redirect_uri の既定値は`/authentication/login-callback`します。
  * 既定で、post_logout_redirect_uri`/authentication/logout-callback`します。
  * スコープのセットが含まれています、 `openid`、 `profile`、およびアプリケーションの Api に対して定義されているすべてのスコープ。
  * 許可されている OIDC 応答型のセットは、`id_token token`またはそれぞれ個別に (`id_token`、 `token`)。
  * 許可される応答モードは`fragment`します。
* SPA:Identity Server でホストされていない単一ページ アプリケーションを表します。
  * スコープのセットが含まれています、 `openid`、 `profile`、およびアプリケーションの Api に対して定義されているすべてのスコープ。
  * 許可されている OIDC 応答型のセットは、`id_token token`またはそれぞれ個別に (`id_token`、 `token`)。
  * 許可される応答モードは`fragment`します。
* IdentityServerJwt:Identity Server でと共にホストされている API を表します。
  * アプリケーションはその既定値は、アプリケーション名に 1 つのスコープを持つように構成します。
* API:Identity Server でホストされていない API を表します。
  * アプリケーションはその既定値は、アプリケーション名に 1 つのスコープを持つように構成します。

### <a name="configuration-through-appsettings"></a>AppSettings を使用した構成
クライアントやリソースの一覧をそれぞれに追加して、構成システムを通じて、アプリケーションを構成できます。 

構成できるクライアントを構成するときに、`redirect_uri`と`post_logout_redirect_uri`次に示すよう。
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

リソースを構成するときに、次に示すようには、リソースのスコープを構成することができます。
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

### <a name="configuration-through-code"></a>コードを使用した構成
また、クライアントとオプションを構成するアクションを取る AddApiAuthorization のオーバー ロードを使用してコードを使用したリソースを構成できます。
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
