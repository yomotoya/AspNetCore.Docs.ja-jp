---
title: "Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。"
author: rick-anderson
description: "作成する方法を説明します。 Visual Studio での ASP.NET Core アプリケーションのプロファイルを発行します。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: d2c4ec317f235c6d042bd130dbf79f6cb5e2d47d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。

作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)

この記事では、Visual Studio 2017 を使用して発行プロファイルを作成する方法に焦点を当てます。 Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。 この記事では、発行プロセスについて詳しく説明します。 Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。

次の *.csproj* ファイルは、コマンド `dotnet new mvc` で作成されました。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

上記のマークアップで (1 行目にある) `<Project>` 要素の `Sdk` 属性は、以下の処理を実行しています。

* プロパティ ファイルからインポート*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*先頭にします。
* 最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。

(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。

`Microsoft.NET.Sdk.Web` は以下に依存しています。

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

これは、次のプロパティとをインポートするターゲットによってが発生します。

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

ターゲットのインポートの右側を使用する発行方法に基づいてターゲットの設定を発行します。

MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが実行されます。

* プロジェクトをビルドする
* 発行するファイルを計算する
* ターゲットにファイルを発行する

### <a name="compute-project-items"></a>プロジェクト項目を比較する

プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。 `item type` 属性によって、ファイルの処理方法が決まります。 既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。 `Compile` 項目一覧のファイルがコンパイルされます。

`Content`項目リストには、ビルドの出力だけでなく公開されているファイルが含まれています。 既定では、ファイル、パターンに一致する`wwwroot/**`に含まれる、`Content`項目。 [wwwroot/\* \*グロブ パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)内のすべてのファイルを指定する、 *wwwroot*フォルダー**と**サブフォルダーです。 発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#including-files)」で説明されているように、*.csproj* ファイルに直接ファイルを追加します。

選択すると、**発行**Visual Studio またはコマンドラインから発行するときにボタンをクリックします。

* プロパティ/項目が計算されます (ビルドに必要なファイル)。
* Visual Studio のみ: NuGet パッケージが復元されます (CLI では、ユーザーが明示的に復元する必要があります)。
* プロジェクトがビルドされます。
* 発行項目が計算されます (発行に必要なファイル)。
* プロジェクトが発行されます (計算されたファイルは、発行先にコピーされます)。

ASP.NET Core プロジェクトの参照と`Microsoft.NET.Sdk.Web`プロジェクト ファイルで、 *app_offline.htm*ファイルは、web アプリのディレクトリのルートに配置します。 ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#appofflinehtm)」を参照してください。

## <a name="basic-command-line-publishing"></a>基本的なコマンド ライン発行

コマンド ライン パブリッシングでは、すべての .NET Core のサポートされているプラットフォームで動作し、Visual Studio は必要ありません。 以下のサンプルで、 [dotnet 発行](/dotnet/core/tools/dotnet-publish)プロジェクト ディレクトリからコマンドを実行 (が含まれています、 *.csproj*ファイル)。 指定しない場合、プロジェクト フォルダー内に明示的に渡すプロジェクト ファイルのパスにします。 例:

```console
dotnet publish c:/webs/web1
```

Web アプリを作成して発行するには、次のコマンドを実行します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

[Dotnet 発行](/dotnet/core/tools/dotnet-publish)コマンドには、次のような出力が生成されます。

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。 `$(Configuration)` の既定値は Debug です。 上の例で `<TargetFramework>` は `netcoreapp2.0` です。

`dotnet publish -h` では、発行のヘルプ情報が表示されます。

次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。

```console
dotnet publish -c Release -o C:/MyWebs/test
```

[Dotnet 発行](/dotnet/core/tools/dotnet-publish)コマンドによって実行される MSBuild を呼び出し、`Publish`ターゲットです。 渡されるパラメーター `dotnet publish` MSBuild に渡されます。 `-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。 `-o` パラメーターは `OutputPath` にマップされます。

MSBuild プロパティは、次の形式のいずれかを使用して渡すことができます。

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

次のコマンドは、`Release` ビルドをネットワーク共有に発行します。

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

ネットワーク共有は、スラッシュを使用して指定します (*//r8/*).NET Core がサポートされるすべてのプラットフォームで使用できます。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

## <a name="publish-profiles"></a>プロファイルを発行する

このセクションでは、Visual Studio 2017 以降を使用して発行プロファイルを作成します。 作成した後は、Visual Studio またはコマンドラインからの発行を使用できます。

発行プロファイルによって、発行プロセスが簡単になります。 複数の発行プロファイルが存在します。 Visual Studio での発行プロファイルを作成するには、ソリューション エクスプ ローラーでプロジェクトを右クリックして**発行**です。 また、選択**発行\<プロジェクト名 >**ビルド メニューからです。 アプリケーション容量ページの **[発行]** タブが表示されます。 プロジェクトに発行プロファイルが含まれていない場合、次のページが表示されます。

![Azure、IIS、FTB、Azure とフォルダーを示すアプリケーション容量ページの [発行] タブを選択します。 また、[新規作成] と [既存のものを選択] のラジオ ボタンも表示されます。](visual-studio-publish-profiles/_static/az.png)

**[フォルダー]** が選択されている場合、**[発行]** ボタンをクリックすると、フォルダーの発行プロファイルが作成され、発行されます。

![Azure、IIS、FTB、フォルダーが表示されているアプリケーション容量ページの **[発行]** タブ](visual-studio-publish-profiles/_static/pub1.png)

発行プロファイルを作成した後、**発行** タブの変更、および選択**新しいプロファイルを作成する**新しいプロファイルを作成します。

![最後の手順で作成した FolderProfile と、[新しいプロファイルの作成] リンクが表示されたアプリケーション容量ページの **[発行]** タブ](visual-studio-publish-profiles/_static/create_new.png)

発行ウィザードは、次の発行ターゲットをサポートしています。

* Microsoft Azure App Service
* IIS、FTP など (任意の Web サーバーの場合)
* フォルダー
* プロファイル (プロファイルのインポートを許可する) をインポートします。
* Microsoft Azure Virtual Machines

詳細については、「[状況に適した発行オプション](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。

Visual Studio が、発行プロファイルを作成するときに、*プロパティ/PublishProfiles/\<発行名 > .pubxml* MSBuild ファイルを作成します。 この *.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。 このファイルは、ビルドのカスタマイズし、発行プロセスに変更することができます。 このファイルは、発行プロファイルで読み取られます。 `<LastUsedBuildConfiguration>` 特殊なため、グローバル プロパティであり、ビルドでインポートするすべてのファイルではないはずです。 詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。
*.Pubxml*に依存しているために、ソース管理にファイルをチェックインしないでください、 *.user*ファイル。 また、*.user* ファイルは、機密情報が含まれている可能性があり、1 ユーザーおよびコンピューターに対してのみ有効なので、ソース コード管理にチェックインしないでください。

機密情報 (発行のパスワードなど) はユーザー/コンピューターごとのレベルで暗号化され、*Properties/PublishProfiles/\<発行名>.pubxml.user* ファイルに保存されます。 このファイルには機密情報が含まれている可能性があるため、ソース コード管理に**チェックインしないでください**。

ASP.NET Core 上の web アプリを公開する方法の概要については、次を参照してください。[ホストを展開および](index.md)です。 [ホストし、展開](index.md)https://github.com/aspnet/websdk でオープン ソース プロジェクトです。

`dotnet publish` MSDeploy のフォルダーを使用し、 [KUDU](https://github.com/projectkudu/kudu/wiki)プロファイルの発行します。
 
フォルダー (クロスプラット フォームで機能): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (現在こののみで機能します windows MSDeploy クロスプラット フォームのないため)。 `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

MSDeploy パッケージ (現在こののみで機能します windows MSDeploy クロスプラット フォームのないため)。 `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

上記のサンプルで**しない**渡す`deployonbuild`に`dotnet publish`です。

詳細については、次を参照してください。 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)です。

`dotnet publish` は、任意のプラットフォームから Azure に発行する KUDU API をサポートしています。 Visual Studio では、サポートが KUDU Api でサポートされて websdk クロス plat が Azure に発行を公開します。

次の内容の発行プロファイルを *[Properties/PublishProfiles]* フォルダーに追加します。

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

次のコマンドを実行している公開内容を圧縮し、KUDU Api を使用して Azure に発行します。

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

という名前のプロファイルで発行するときに*FolderProfile*、次のコマンドのいずれかを実行することができます。

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

呼び出すときに[dotnet ビルド](/dotnet/core/tools/dotnet-build)、呼び出す`msbuild`ビルドを実行してプロセスを発行します。 呼び出す`dotnet build`または`msbuild`はフォルダー プロファイルに渡す場合、本質的に同等です。 Windows 上で直接には、MSBuild を呼び出し、MSBuild の .NET Framework のバージョンが使用されます。 MSDeploy の発行は、現在 Windows コンピューターに制限されています。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。 フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。

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

`<LastUsedBuildConfiguration>` は `Release` に設定されます。 Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。 `<LastUsedBuildConfiguration>`構成プロパティは特別であり、インポートされた MSBuild ファイル内でオーバーライドすることはできません。 コマンドラインからこのプロパティをオーバーライドすることができます。 例:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

MSBuild の使用:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>コマンド ラインから MSDeploy エンドポイントに発行する

既に触れましたが、発行を使用して実現できる`dotnet publish`または`msbuild`コマンド。 `dotnet publish` は .NET Core のコンテキストで実行されます。 `msbuild`コマンドは、.NET framework が必要であるため、Windows 環境に制限されます。

MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。

次の例では、ASP.NET Core web アプリを作成 (を使用して`dotnet new mvc`)、Visual Studio で Azure の発行プロファイルを追加します。

実行`msbuild`から、 **VS 2017 用開発者コマンド プロンプト**です。 開発者コマンド プロンプトが、正しい*msbuild.exe*一部 MSBuild 変数セットをそのパスにします。

MSBuild は次の構文を使用します。

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

取得、`Password`から、 *\<発行名 >。PublishSettings*ファイル。 ダウンロード、*です。PublishSettings*からいずれかのファイル。

* ソリューション エクスプ ローラー: は、この Web アプリでを右クリックし、選択**発行プロファイルのダウンロード**です。
* Azure Management Portal: 選択**Get 発行プロファイル**Web アプリのブレードからです。

`Username` は発行プロファイルで確認できます。

次の例では、"Web11112 - Web 配置" 発行プロファイルを使用しています。

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>ファイルの除外

ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。 `msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。 たとえば、次`<Content>`要素マークアップには、すべてのテキストが含まれません (*.txt*) ファイルから、 *wwwroot/コンテンツ*フォルダーとそのすべてのサブフォルダーです。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上記のマークアップは、発行プロファイルまたは *.csproj* ファイルに追加できます。 *.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。

次の `<MsDeploySkipRules>` 要素マークアップは、*wwwroot/content* フォルダーからすべてのファイルを除外します。

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` 削除されません、*スキップ*配置サイトからターゲットです。 `<Content>` 対象となるファイルとフォルダーは、配置サイトから削除されます。 たとえば、展開された web アプリケーションが、次のファイルとします。

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

場合、次`<MsDeploySkipRules>`マークアップが追加される、展開のサイトでそれらのファイルは削除はありません。

``` xml
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

`<MsDeploySkipRules>`マークアップの前に示したように、*スキップ*depoyed されないファイルは、配置されると、それらのファイルを削除しません。

次`<Content>`マークアップは、展開のサイトで対象となるファイルを削除します。

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

コマンド ラインの展開を使用して、`<Content>`マークアップ上、次のような出力結果。

``` console
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

## <a name="including-files"></a>ファイルを含める

次のマークアップが含まれています、*イメージ*フォルダーをプロジェクト ディレクトリの外部、 *wwwroot/イメージ*発行サイトのフォルダー。

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。 追加された場合、 *.csproj*ファイルが含まれているプロジェクト内の発行プロファイルごとにします。

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

### <a name="run-a-target-before-or-after-publishing"></a>発行の前または後にターゲットを実行する

組み込み`BeforePublish`と`AfterPublish`発行先の前後にターゲットを実行するターゲットを使用できます。 次のマークアップを発行プロファイルに追加して、発行の前と後にメッセージのログをコンソールの出力に記録することができます。

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Kudu サービス

内のファイルを表示する、アプリのサービスを Azure web アプリの展開、使用、 [Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)です。 追加、`scm`トークンを web アプリの名前。 例:

| URL                                    | 結果      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | Web アプリ     |
| `http://mysite.scm.azurewebsites.net/` | Kudu サービス |

[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択して、ファイルの表示、編集、削除、追加を行います。

## <a name="additional-resources"></a>その他の技術情報

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) が IIS サーバーへの web サイトと web アプリの展開を簡略化します。
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置のファイルの問題と要求機能。
