---
title: ASP.NET Core アプリを配置するための Visual Studio 発行プロファイル
author: rick-anderson
description: Visual Studio で発行プロファイルを作成し、それらを使用してさまざまなターゲットへの ASP.NET Core アプリの配置を管理する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3e626f99b06b0343360d6c46447e357890433dda
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148929"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>ASP.NET Core アプリを配置するための Visual Studio 発行プロファイル

作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントでは、Visual Studio 2017 を使用して、発行プロファイルを作成して使用する方法に焦点を当てます。 Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。 Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。

次のプロジェクト ファイルは、`dotnet new mvc` コマンドを使用して作成されました。

::: moniker range=">= aspnetcore-2.1"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.7" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.8" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

::: moniker-end

`<Project>` 要素の `Sdk` 属性は、次のタスクを実行します。

* 最初に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* からプロパティ ファイルをインポートします。
* 最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。

(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。

`Microsoft.NET.Sdk.Web` SDK は、以下に依存しています。

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

それにより、次のプロパティとターゲットのインポートが発生します。

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

使用される発行方法に基づいて、発行ターゲットは適切なターゲット セットをインポートします。

MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが発生します。

* プロジェクトをビルドする
* 発行するファイルを計算する
* ターゲットにファイルを発行する

## <a name="compute-project-items"></a>プロジェクト項目を比較する

プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。 `item type` 属性によって、ファイルの処理方法が決まります。 既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。 `Compile` 項目一覧のファイルがコンパイルされます。

`Content` 項目一覧には、ビルドの出力に加え、発行されるファイルが含まれています。 既定で、`wwwroot/**` というパターンと一致するファイルが `Content` 項目に含まれます。 `wwwroot/\*\*` [グロビング パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)は、*wwwroot* フォルダー**と**サブフォルダー内のすべてのファイルと一致します。 発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#include-files)」で説明されているように *.csproj* ファイルに直接ファイルを追加します。

Visual Studio で **[発行]** ボタンを選択するか、コマンド ラインから発行すると、以下が実行されます。

* プロパティ/項目が計算されます (ビルドに必要なファイル)。
* **Visual Studio のみ**: NuGet パッケージが復元されます  (CLI では、ユーザーが明示的に復元する必要があります)。
* プロジェクトがビルドされます。
* 発行項目が計算されます (発行に必要なファイル)。
* プロジェクトが発行されます (計算されたファイルが発行先にコピーされます)。

ASP.NET Core プロジェクトは、プロジェクト ファイルの `Microsoft.NET.Sdk.Web` を参照して、*app_offline.htm* ファイルを Web アプリのディレクトリのルートに配置します。 ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。

## <a name="basic-command-line-publishing"></a>基本的なコマンド ラインからの発行

コマンド ラインからの発行は、.NET Core をサポートするすべてのプラットフォームで機能し、Visual Studio は必要ありません。 次の例では、プロジェクト ディレクトリ (*.csproj* ファイルが含まれているディレクトリ) から [dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドが実行されます。 現在のフォルダーがプロジェクト フォルダーではない場合は、プロジェクト ファイル パスにパスを明示的に渡します。 例:

```console
dotnet publish C:\Webs\Web1
```

Web アプリを作成して発行するには、次のコマンドを実行します。

::: moniker range=">= aspnetcore-2.0"

```console
dotnet new mvc
dotnet publish
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```console
dotnet new mvc
dotnet restore
dotnet publish
```

::: moniker-end

[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドは、次のような出力を生成します。

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。 `$(Configuration)` の既定値は *Debug* です。 上記のサンプルでは、`<TargetFramework>` は `netcoreapp2.0`です。

`dotnet publish -h` では、発行のヘルプ情報が表示されます。

次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドは MSBuild を呼び出し、MSBuild は `Publish` ターゲットを呼び出します。 `dotnet publish` に渡されたすべてのパラメーターが MSBuild に渡されます。 `-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。 `-o` パラメーターは `OutputPath` にマップされます。

MSBuild のプロパティは、次のいずれかの形式を使用して渡すことができます。

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

次のコマンドは、`Release` ビルドをネットワーク共有に発行します。

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

ネットワーク共有は、スラッシュを使用して指定します (*//r8/*)。 .NET Core がサポートされるすべてのプラットフォームで使用できます。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

## <a name="publish-profiles"></a>プロファイルを発行する

このセクションでは、Visual Studio 2017 を使用して発行プロファイルを作成します。 作成したら、Visual Studio またはコマンド ラインから発行できます。

発行プロファイルは発行プロセスを簡略化でき、任意の数のプロファイルが存在できます。 次のいずれかの方法で、Visual Studio で発行プロファイルを作成します。

* ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。
* **[ビルド]** メニューの **[&lt;プロジェクト名&gt; の発行]** を選択します。

アプリ容量ページの **[発行]** タブが表示されます。 プロジェクトに発行プロファイルが含まれていない場合は、次のページが表示されます。

![アプリ容量ページの [発行] タブ](visual-studio-publish-profiles/_static/az.png)

**[フォルダー]** を選択した場合は、発行された資産を保存するフォルダーのパスを指定します。 既定のフォルダーは *bin\Release\PublishOutput* です。 **[プロファイルの作成]** ボタンをクリックして完了します。

発行プロファイルが作成されると、**[発行]** タブが変化します。 新しく作成したプロファイルがドロップダウン リストに表示されます。 別の新しいプロファイルを作成するには、**[新しいプロファイルの作成]** をクリックします。

![FolderProfile を示しているアプリ容量ページの [発行] タブ](visual-studio-publish-profiles/_static/create_new.png)

発行ウィザードは、次の発行ターゲットをサポートしています。

* Azure App Service
* Azure Virtual Machines
* IIS、FTP など (すべての Web サーバー)
* フォルダー
* インポート プロファイル

詳細については、「[状況に適した発行オプション](/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。

Visual Studio で発行プロファイルを作成すると、*Properties/PublishProfiles/&lt;発行名&gt;.pubxml* MSBuild ファイルが作成されます。 *.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。 このファイルを変更して、ビルドと発行プロセスをカスタマイズできます。 このファイルは、発行プロファイルで読み取られます。 `<LastUsedBuildConfiguration>` はグローバル プロパティであり、ビルドにインポートされるファイルには含めるべきではない特別なプロパティです。 詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。

Azure ターゲットに発行する場合、*.pubxml* ファイルには、Azure サブスクリプション識別子が含まれます。 そのターゲットの種類では、このファイルをソース管理に追加することはお勧めしません。 Azure 以外のターゲットに発行する場合は、*.pubxml* ファイルをチェックインするほうが安全です。

機微な情報 (発行パスワードなど) は、個々のユーザー/コンピューター レベルで暗号化されます。 それは、*Properties/PublishProfiles/&lt;プロファイル名&gt;.pubxml.user* ファイルに格納されます。 このファイルには機微な情報が格納される可能性があるため、ソース コード管理にチェックインしないでください。

ASP.NET Core で Web アプリを発行する方法の概要については、「[ホストと展開](xref:host-and-deploy/index)」を参照してください。 ASP.NET Core アプリを発行するために必要な MSBuild タスクとターゲットは、オープン ソースです (https://github.com/aspnet/websdk)。

`dotnet publish` は、フォルダー、MSDeploy、および [Kudu](https://github.com/projectkudu/kudu/wiki) 発行プロファイルを使用できます。

フォルダー (クロス プラットフォームで動作します):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (MSDeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy パッケージ (MSDeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

上記の例で、`deployonbuild` を `dotnet publish` に**渡さない**ようにしてください。

詳細については、「[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)」を参照してください。

`dotnet publish` は、任意のプラットフォームから Azure に発行する Kudu API をサポートしています。 Visual Studio の発行は、Kudu API をサポートしていますが、Azure へのクロスプラットフォームの発行は WebSDK がサポートしています。

次の内容の発行プロファイルを *Properties/PublishProfiles* フォルダーに追加します。

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

次のコマンドを実行すると、発行するコンテンツが圧縮され、Kudu API を使用して Azure に発行されます。

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

*FolderProfile* という名前のプロファイルを使用して発行する場合は、以下のいずれかのコマンドを実行できます。

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

[dotnet build](/dotnet/core/tools/dotnet-build) を呼び出すと、`msbuild` が呼び出されて、ビルドと発行プロセスが実行されます。 `dotnet build` または `msbuild` の呼び出しは、フォルダー プロファイルで渡す場合は同等です。 Windows で MSBuild を直接呼び出すと、MSBuild の .NET Framework バージョンが使用されます。 MSDeploy の発行は、現在 Windows コンピューターに制限されています。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。 フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。

次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

`<LastUsedBuildConfiguration>` は `Release` に設定されます。 Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。 `<LastUsedBuildConfiguration>` 構成プロパティは特別なプロパティなので、インポートされる MSBuild ファイルで上書きされないようにしてください。 このプロパティは、コマンド ラインからオーバーライドできます。

.NET Core CLI の使用:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

MSBuild の使用:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>コマンド ラインから MSDeploy エンドポイントに発行する

発行は、.NET Core CLI または MSBuild を使用して実行できます。 `dotnet publish` は .NET Core のコンテキストで実行されます。 `msbuild` コマンドは .NET Framework を必要とするため、Windows 環境に制限されています。

MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。

次の例では (`dotnet new mvc` を使用して) ASP.NET Core Web アプリが作成され、Visual Studio を使用して Azure 発行プロファイルが追加されます。

**VS 2017 用開発者コマンド プロンプト**から `msbuild` を実行します。 この開発者コマンド プロンプトのパスには、いくつかの MSBuild 変数が設定された正しい *msbuild.exe* が含まれています。

MSBuild は次の構文を使用します。

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

*\<発行名>.PublishSettings* ファイルから `Password` を取得します。 次のいずれかの方法で、*.PublishSettings* ファイルをダウンロードします。

* ソリューション エクスプローラー: Web アプリを右クリックし、**[発行プロファイルのダウンロード]** を選択します。
* Azure Portal: Web アプリの **[概要]** ウィンドウで **[発行プロファイルの取得]** をクリックします。

`Username` は発行プロファイルで確認できます。

次の例では、*Web11112 - Web Deploy* プロファイルを使用しています。

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>ファイルを除外する

ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。 `msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。 たとえば、次の `<Content>` 要素は、*wwwroot/content* フォルダーとそのすべてのサブフォルダーのすべてのテキスト (*.txt*) ファイルを除外します。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上記のマークアップは、発行プロファイルまたは *.csproj* ファイルに追加できます。 *.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。

次の `<MsDeploySkipRules>` 要素は、*wwwroot/content* フォルダーのすべてのファイルを除外します。

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` は、"*スキップされる*" ターゲットを配置サイトから削除しません。 `<Content>` のターゲットであるファイルとフォルダーは、配置サイトから削除されます。 たとえば、配置される Web アプリに次のファイルが含まれているとします。

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

次の `<MsDeploySkipRules>` 要素を追加した場合、これらのファイルは配置サイトでは削除されません。

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

前述の `<MsDeploySkipRules>` 要素は、"*スキップされる*" ファイルが配置されないようにします。 それは、いったん配置されたファイルは削除しません。

次の `<Content>` 要素は、配置サイトのターゲット ファイルを削除します。

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

前述の`<Content>` 要素を使用してコマンド ライン配置を行うと、次の出力が生成されます。

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>インクルード ファイル

次のマークアップは、プロジェクト ディレクトリの外部にある *images* フォルダーを、発行サイトの *wwwroot/images* フォルダーに含めます。

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。 *.csproj* ファイルに追加した場合は、プロジェクトの各発行プロファイルに含まれます。

次の強調表示されているマークアップは、この方法を示しています。

* プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。
* *wwwroot\Content* フォルダーを除外します。
* *Views\Home\About2.cshtml* を除外します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。

## <a name="run-a-target-before-or-after-publishing"></a>発行の前または後にターゲットを実行する

組み込みの `BeforePublish` と `AfterPublish` ターゲットは、ターゲットの発行前または後にターゲットを実行できます。 発行の前と後の両方でコンソール メッセージをログに記録するには、発行プロファイルに次の要素を追加します。

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>信頼されていない証明書を使用してサーバーに発行する

値が `True` の `<AllowUntrustedCertificate>` プロパティを発行プロファイルに追加します。

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu サービス

Azure App Service での Web アプリのデプロイに含まれるファイルを表示するには、[Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service) を使用します。 `scm` トークンを Web アプリ名に追加します。 例:

| URL                                    | 結果       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web アプリ      |
| `http://mysite.scm.azurewebsites.net/` | Kudu サービス |

ファイルの表示、編集、削除、追加を行うには、[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択します。

## <a name="additional-resources"></a>その他の技術情報

* [Web 配置](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) は、IIS サーバーへの Web アプリと Web サイトの配置を簡略化します。
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置でのファイルの問題と要求機能。
* [Visual Studio から Azure VM に ASP.NET Web アプリを発行する](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
