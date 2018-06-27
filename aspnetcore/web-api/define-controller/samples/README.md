# <a name="aspnet-core-web-api-controller-sample"></a>ASP.NET Core Web API コントローラーのサンプル

このサンプル アプリは、次のプロジェクトで構成されています。

- **WebApiSample.Api**: .NET Core 2.1 を対象とする ASP.NET Core 2.1 プロジェクト。
- **WebApiSample.Api.Pre21**: .NET Core 2.0 を対象とする ASP.NET Core 2.0 プロジェクト。
- **WebApiSample.DataAccess**: 2 つの Web API プロジェクトのデータ アクセス層として機能する .NET Standard 2.0 クラス ライブラリ。

このサンプルでは、Web API コントローラー作成のバリエーションを示します。

- [ControllerBase からクラスを派生する](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#derive-class-from-controllerbase)
- [ApiControllerAttribute でクラスに注釈を付ける](https://docs.microsoft.com/en-us/aspnet/core/web-api/define-controller#annotate-class-with-apicontrollerattribute)