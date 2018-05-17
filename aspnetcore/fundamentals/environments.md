---
title: ASP.NET Core で複数の環境を使用する
author: rick-anderson
description: ASP.NET Core では、複数の環境にわたり、アプリの動作を制御できます。ここではそのしくみについて説明します。
manager: wpickett
ms.author: riande
ms.date: 12/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/environments
ms.openlocfilehash: b9c3b8a15424ca637a2486450bfdde2762204935
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="work-with-multiple-environments-in-aspnet-core"></a>ASP.NET Core で複数の環境を使用する

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core では、環境変数を利用し、アプリケーションの実行時動作を設定できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="environments"></a>環境

ASP.NET Core はアプリケーションの起動時に環境変数 `ASPNETCORE_ENVIRONMENT` を読み込み、その値を [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName) に格納します。 `ASPNETCORE_ENVIRONMENT` は任意の値に設定できますが、フレームワークは [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development?view=aspnetcore-2.0)、[Staging](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.staging?view=aspnetcore-2.0)、[Production](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.production?view=aspnetcore-2.0) という [3 つの値](/dotnet/api/microsoft.aspnetcore.hosting.environmentname?view=aspnetcore-2.0)を指定できます。 `ASPNETCORE_ENVIRONMENT` が設定されていない場合、既定で `Production` が使用されます。

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet)]

上のコードでは以下の操作が行われます。

* `ASPNETCORE_ENVIRONMENT` が `Development` に設定されているとき、[UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_DeveloperExceptionPageExtensions_UseDeveloperExceptionPage_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseBrowserLink](/dotnet/api/microsoft.aspnetcore.builder.browserlinkextensions.usebrowserlink?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_BrowserLinkExtensions_UseBrowserLink_Microsoft_AspNetCore_Builder_IApplicationBuilder_) を呼び出します。
* `ASPNETCORE_ENVIRONMENT` の値が次のいずれかに設定されているとき、[UseExceptionHandler](/dotnet/api/microsoft.aspnetcore.builder.exceptionhandlerextensions.useexceptionhandler?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ExceptionHandlerExtensions_UseExceptionHandler_Microsoft_AspNetCore_Builder_IApplicationBuilder_) を呼び出します。

    * `Staging`
    * `Production`
    * `Staging_2`

[環境タグ ヘルパー ](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) は `IHostingEnvironment.EnvironmentName` の値を使用し、要素にマークアップを追加するか、要素からマークアップを除外します。

[!code-html[](environments/sample/WebApp1/Pages/About.cshtml)]

注: Windows と macOS では、環境変数と値で大文字と小文字が区別されません。 Linux では、環境変数と値について、既定で**大文字と小文字が区別されます**。

### <a name="development"></a>開発

開発環境では、実稼働で公開すべきではない機能を有効にすることがあります。 たとえば、ASP.NET Core テンプレートは、開発環境で[開発者例外ページ](xref:fundamentals/error-handling#the-developer-exception-page)を有効にします。

ローカル コンピューターの開発環境をプロジェクトの *Properties\launchSettings.json* ファイルで設定できます。 *launchSettings.json* に設定されている環境値で、システム環境に設定されている値がオーバーライドされます。

次の JSON では、*launchSettings.json* ファイルの 3 つのプロファイルが表示されます。

[!code-json[](environments/sample/WebApp1/Properties/launchSettings.json?highlight=10,11,18,26)]

::: moniker range=">= aspnetcore-2.1"
> [!NOTE]
> *launchSettings.json* の `applicationUrl` プロパティでは、サーバー URL の一覧を指定できます。 一覧の URL 間には、セミコロンを次のように使用します。
>
> ```json
> "WebApplication1": {
>    "commandName": "Project",
>    "launchBrowser": true,
>    "applicationUrl": "https://localhost:5001;http://localhost:5000",
>    "environmentVariables": {
>      "ASPNETCORE_ENVIRONMENT": "Development"
>    }
> }
> ```
::: moniker-end

アプリケーションが [dotnet run](/dotnet/core/tools/dotnet-run) で起動すると、`"commandName": "Project"` を含む最初のプロファイルが使用されます。 `commandName` の値により、起動する Web サーバーが指定されます。 `commandName` は次のいずれかになります。

* IIS Express
* IIS
* プロジェクト (Kestrel を起動する)

アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動されるとき:

* 利用できる場合、*launchSettings.json* が読み込まれます。 *launchSettings.json* の `environmentVariables` 設定により環境変数がオーバーライドされます。
* ホスティング環境が表示されます。


次の出力には、[dotnet run](/dotnet/core/tools/dotnet-run) で起動したアプリが表示されます。
```bash
PS C:\Webs\WebApp1> dotnet run
Using launch settings from C:\Webs\WebApp1\Properties\launchSettings.json...
Hosting environment: Staging
Content root path: C:\Webs\WebApp1
Now listening on: http://localhost:54340
Application started. Press Ctrl+C to shut down.
```

Visual Studio の **[デバッグ]** タブには、*launchSettings.json* ファイルを編集するための GUI があります。

![プロジェクト プロパティ設定の環境変数](environments/_static/project-properties-debug.png)

プロジェクト プロファイルを変更した場合、Web サーバーを再起動するまで変更は適用されません。 Kestrel がその環境に行われた変更を検出するには、先に再起動する必要があります。

>[!WARNING]
> *launchSettings.json* にはシークレットを格納しないでください。 ローカル開発のシークレットを格納するには、[シークレット マネージャー ツール](xref:security/app-secrets)を利用できます。

### <a name="production"></a>実稼働

実稼働環境は、セキュリティ、パフォーマンス、アプリケーションの堅牢性が最大化されるように構成してください。 開発とは異なる一般的な設定は次のとおりです。

* キャッシュ。
* クライアント側のリソースはバンドルされ、小さくされ、場合によっては CDN からサービスが提供されます。
* 診断エラー ページが無効。
* フレンドリ エラー ページが有効。
* 実稼働のログ記録と監視が有効。 [Application Insights](/azure/application-insights/app-insights-asp-net-core) など。

## <a name="setting-the-environment"></a>環境を更新する

テスト用の環境を構築すると便利な場合があります。 環境を設定しない場合、既定で `Production` に設定されます。実稼働ではほとんどのデバッグ機能が無効になっています。

環境を設定する手法は、オペレーティング システムによって異なります。

### <a name="azure"></a>Azure

Azure アプリ サービスの場合:

* **[アプリケーションの設定]** ブレードを選択します。
* **[アプリの設定]** でキーと値を追加します。


### <a name="windows"></a>Windows
現在のセッションに `ASPNETCORE_ENVIRONMENT` を設定するには、アプリが [dotnet run](/dotnet/core/tools/dotnet-run) で起動する場合、次のコマンドを使用します。

**コマンド ライン**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

これらのコマンドは、現在のウィンドウでのみ有効になります。 ウィンドウが閉じられると、ASPNETCORE_ENVIRONMENT 設定が初期設定またはコンピューター値に戻ります。 Windows でグローバルな値を設定するには、**[コントロール パネル]**、**[システム]**、**[システムの詳細設定]** の順に開き、`ASPNETCORE_ENVIRONMENT` 値を追加または編集します。

![システムの詳細プロパティ](environments/_static/systemsetting_environment.png)

![ASPNET Core 環境変数](environments/_static/windows_aspnetcore_environment.png)


**web.config**

「[ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#setting-environment-variables)」(ASP.NET Core Module 構成参照) トピックの「*Setting environment variables*」(環境変数の設定) セクションを参照してください。

**IIS アプリケーション プール**

分離されたアプリケーション プール (IIS 10.0 以降でサポートされています) で実行する個別アプリに対して環境変数を設定するには、[環境変数 \<environmentVariables>](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) のトピックにある *AppCmd.exe コマンド*のセクションを参照してください。

### <a name="macos"></a>macOS
MacOS の現在の環境は、アプリケーション実行時に設定できます。

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
あるいは、`export` を利用し、アプリの実行前に設定できます。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```
コンピューター レベルの環境変数は *.bashrc* または *.bash_profile* ファイルに設定されます。 任意のテキスト エディターでファイルを編集し、次のステートメントを追加します。

```bash
export ASPNETCORE_ENVIRONMENT=Development
```

### <a name="linux"></a>Linux
Linux ディストリビューションの場合、セッション ベースの変数設定にはコマンド ラインで `export` コマンドを使用し、コンピューター レベルの環境設定には *bash_profile* ファイルを使用します。

### <a name="configuration-by-environment"></a>環境別の構成

詳細については、[環境別の構成](xref:fundamentals/configuration/index#configuration-by-environment)に関するページを参照してください。

<a name="startup-conventions"></a>
## <a name="environment-based-startup-class-and-methods"></a>環境別の起動のクラスとメソッド

ASP.NET Core アプリの起動時、[Startup クラス](xref:fundamentals/startup)がアプリをブートストラップします。 クラス `Startup{EnvironmentName}` が存在する場合、その `EnvironmentName` に対してそのクラスが呼び出されます。

[!code-csharp[](environments/sample/WebApp1/StartupDev.cs?name=snippet&highlight=1)]

注: [WebHostBuilder.UseStartup<TStartup>](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_WebHostBuilderExtensions_UseStartup__1_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) を呼び出すと、構成セクションがオーバーライドされます。

[Configure](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configure?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_StartupBase_Configure_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.startupbase.configureservices?view=aspnetcore-2.0) は、フォーム `Configure{EnvironmentName}` と `Configure{EnvironmentName}Services` の環境固有バージョンに対応しています。

[!code-csharp[](environments/sample/WebApp1/Startup.cs?name=snippet_all&highlight=15,37)]

## <a name="additional-resources"></a>その他の技術情報

* [アプリケーションの起動](xref:fundamentals/startup)
* [構成](xref:fundamentals/configuration/index)
* [IHostingEnvironment.EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname?view=aspnetcore-2.0#Microsoft_AspNetCore_Hosting_IHostingEnvironment_EnvironmentName)
