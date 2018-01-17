---
title: "ASP.NET Core での複数の環境での作業"
author: rick-anderson
description: "ASP.NET Core が複数の環境間でのアプリの動作を制御するためのサポートを提供する方法について説明します。"
keywords: "ASP.NET Core、環境の設定、ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 12/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 784d176145c3e4e44ddc0ea06b6702f70cd4b08c
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2018
---
# <a name="working-with-multiple-environments"></a>複数の環境での作業

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core は、環境変数と実行時にアプリケーションの動作を設定するためのサポートを提供します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="environments"></a>環境

ASP.NET Core が環境変数を読み取る`ASPNETCORE_ENVIRONMENT`でアプリケーションの起動およびストア内の値が[IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)です。 `ASPNETCORE_ENVIRONMENT`任意の値に設定することができますが、 [3 つの値](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)フレームワークでサポートされて:[開発](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[ステージング](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)、および[運用](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0)です。 場合`ASPNETCORE_ENVIRONMENT`は既定に設定されている`Production`です。

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet)]

上のコードでは以下の操作が行われます。

* 呼び出し[UseDeveloperExceptionPage](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_)と[UseBrowserLink](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_)とき`ASPNETCORE_ENVIRONMENT`に設定されている`Development`です。
* 呼び出し[UseExceptionHandler](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_)ときの値`ASPNETCORE_ENVIRONMENT`設定されている、次のいずれか。

    * `Staging`
    * `Production`
    * `Staging_2`

[環境タグ ヘルパー](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)の値を使用して`IHostingEnvironment.EnvironmentName`要素内のマークアップを追加または除外します。

[!code-html[Main](environments/sample/WebApp1/Pages/About.cshtml)]

注: Windows および macOS で環境変数と値いない大文字と小文字が区別されます。 Linux 環境変数と値は**大文字小文字を区別**既定です。

### <a name="development"></a>開発

開発環境には、実稼働環境で公開するべきではない機能が有効にすることができます。 たとえば、ASP.NET Core テンプレートを有効にする、[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)開発環境でします。

ローカル コンピューターの開発環境を設定することができます、 *Properties\launchSettings.json*プロジェクトのファイルです。 環境の値で設定*launchSettings.json*システムの環境で設定値をオーバーライドします。

次の XML は次の 3 つのプロファイルから、 *launchSettings.json*ファイル。

[!code-xml[Main](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

アプリケーションを起動するときに`dotnet run`、最初のプロファイルが`"commandName": "Project"`使用されます。 値`commandName`を起動する web サーバーを指定します。 `commandName`いずれかを指定できます。

* IIS Express
* IIS
* プロジェクトを起動 Kestrel)

アプリを起動するときに`dotnet run`:

* *launchSettings.json*は読み取り可能な場合です。 `environmentVariables`設定*launchSettings.json*環境変数をオーバーライドします。
* ホスティング環境が表示されます。


次の出力は、アプリの使用を開始を示します`dotnet run`:
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio**デバッグ**タブを編集する GUI を使用する、 *launchSettings.json*ファイル。

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

プロジェクトのプロファイルに加えられた変更は可能性があります、web サーバーが再起動されるまで有効になりません。 自体は、その環境に加えられた変更を検出するには、kestrel を再起動する必要があります。

>[!WARNING]
> *launchSettings.json*機密情報を格納する必要があります。 [シークレット マネージャー ツール](xref:security/app-secrets)ローカル開発用のシークレットを格納するために使用できます。

### <a name="production"></a>実稼働

実稼働環境は、セキュリティ、パフォーマンス、およびアプリケーションの堅牢性を最大化するように構成する必要があります。 一般的な運用環境が存在する可能性のある設定の開発とは異なりますは次のとおりです。

* キャッシュします。
* クライアント側のリソースのバンドル、縮小、および CDN から供給される可能性がありますしていること。
* 診断エラー ページが無効になっています。
* わかりやすいエラー ページが有効にします。
* 実稼働のログ記録と監視を有効にします。 たとえば、 [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/)です。

## <a name="setting-the-environment"></a>環境の設定

テストするための特定の環境を設定すると便利です。 環境が設定されていない場合を既定`Production`ほとんどのデバッグ機能を無効にします。

環境を設定するためのメソッドは、オペレーティング システムによって異なります。

### <a name="azure"></a>Azure

Azure アプリケーション サービスの場合

* 選択、**アプリケーション設定**ブレードです。
* キーを追加して、値で**アプリ設定**です。


### <a name="windows"></a>Windows
設定する、`ASPNETCORE_ENVIRONMENT`を使用して、アプリが開始された場合、現在のセッションの`dotnet run`、次のコマンドを使用

**コマンド ライン**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

これらのコマンドは、現在のウィンドウに対してのみ有効になります。 ウィンドウが閉じられたときに、ASPNETCORE_ENVIRONMENT 設定は、既定の設定またはコンピューターの値に戻ります。 Windows を開いたとき、値をグローバルに設定するために、**コントロール パネルの**  > **システム** > **システムの詳細設定**を追加または編集、 `ASPNETCORE_ENVIRONMENT`値。

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET コア環境変数](environments/_static/windows_aspnetcore_environment.png)


**web.config**

参照してください、*環境変数の設定*のセクションで、 [ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)トピックです。

**IIS アプリケーション プール単位**

(IIS 10.0 以降でサポート) 分離のアプリケーション プールで実行されている個々 のアプリの環境変数を設定するを参照してください、 *AppCmd.exe コマンド*のセクションで、[環境変数\<。environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe)トピックです。

### <a name="macos"></a>macOS
MacOS の現在の環境を設定する場合に実行できます行で、アプリケーションを実行しています。

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
使用してまたは`export`をアプリを実行する前に設定します。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
コンピューターのレベルの環境変数が設定されて、*なる*または*.bash_profile*ファイル。 任意のテキスト エディターを使用して、ファイルを編集し、次のステートメントを追加します。

```
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Linux ディストリビューションの場合を使用して、`export`セッション ベースの変数の設定のコマンドラインでコマンドと*bash_profile*マシン レベルの環境設定のファイルです。

### <a name="configuration-by-environment"></a>環境での構成

参照してください[環境によって構成](xref:fundamentals/configuration/index#configuration-by-environment)詳細についてはします。

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>環境がスタートアップ クラスおよびメソッドに基づく

ASP.NET Core アプリケーションの起動時、[スタートアップ クラス](xref:fundamentals/startup)アプリをブートス トラップします。 クラス`Startup{EnvironmentName}`存在する場合、そのクラスが呼び出されること`EnvironmentName`:

[!code-csharp[Main](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

注: 呼び出す[WebHostBuilder.UseStartup<TStartup> ](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_)構成セクションをオーバーライドします。

[構成](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_)と[ConfigureServices](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0)フォームの環境の特定バージョンをサポートして`Configure{EnvironmentName}`と`Configure{EnvironmentName}Services`:

[!code-csharp[Main](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>その他のリソース

* [アプリケーションの起動](xref:fundamentals/startup)
* [構成](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
