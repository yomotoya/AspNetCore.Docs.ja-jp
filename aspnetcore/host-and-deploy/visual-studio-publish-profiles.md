---
title: Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。
author: rick-anderson
description: 作成する方法、発行プロファイルを Visual Studio で、さまざまな対象の ASP.NET Core アプリ展開を管理するために使用します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio ASP.NET Core アプリケーションの展開用のプロファイルを発行します。

作成者: [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このドキュメントに焦点を当てます Visual Studio 2017 作成および使用すると、発行プロファイル。 Visual Studio で作成された発行プロファイルは、MSBuild と Visual Studio 2017 から実行できます。 Azure に発行する方法については、「[Visual Studio を使用して Azure App Service に ASP.NET Core アプリを発行する](xref:tutorials/publish-to-azure-webapp-using-vs)」を参照してください。

次のプロジェクト ファイルは、コマンドを使用して作成された`dotnet new mvc`:

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

`<Project>`要素の`Sdk`属性は、次のタスクを実行します。

* プロパティ ファイルからインポート *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*先頭にします。
* 最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。

(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。

`Microsoft.NET.Sdk.Web` SDK によって異なります。

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

これは、次のプロパティとをインポートするターゲットによってが発生します。

* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$ (MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

ターゲットのインポートの右側を使用する発行方法に基づいてターゲットの設定を発行します。

MSBuild または Visual Studio プロジェクトが読み込まれると、次の高度な操作が行われます。

* プロジェクトをビルドする
* 発行するファイルを計算する
* ターゲットにファイルを発行する

## <a name="compute-project-items"></a>プロジェクト項目を比較する

プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。 `item type` 属性によって、ファイルの処理方法が決まります。 既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。 `Compile` 項目一覧のファイルがコンパイルされます。

`Content`項目リストには、ビルドの出力だけでなく公開されているファイルが含まれています。 既定では、ファイル、パターンに一致する`wwwroot/**`に含まれる、`Content`項目。 `wwwroot/\*\*` [グロブ パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)内のすべてのファイルと一致する、 *wwwroot*フォルダー**と**サブフォルダーです。 発行の一覧にファイルを明示的に追加するで直接ファイルを追加、 *.csproj*に示すように、ファイル[ファイルを含める](#include-files)です。

選択すると、**発行**Visual Studio またはコマンドラインから発行するときにボタンをクリックします。

* プロパティ/項目が計算されます (ビルドに必要なファイル)。
* **Visual Studio のみ**: NuGet パッケージを復元します。 (CLI では、ユーザーが明示的に復元する必要があります)。
* プロジェクトがビルドされます。
* 発行項目が計算されます (発行に必要なファイル)。
* プロジェクトを発行 (計算されたファイルは、発行先にコピーされます)。

ASP.NET Core プロジェクトの参照と`Microsoft.NET.Sdk.Web`プロジェクト ファイルで、 *app_offline.htm*ファイルは、web アプリのディレクトリのルートに配置します。 ファイルが存在する場合、ASP.NET Core モジュールはアプリを正常にシャットダウンし、展開中に *app_offline.htm* ファイルを提供します。 詳細については、「[ASP.NET Core モジュール構成リファレンス](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)」を参照してください。

## <a name="basic-command-line-publishing"></a>基本的なコマンド ライン発行

コマンド ライン パブリッシングでは、.NET Core でサポートされているすべてのプラットフォームでは機能し、Visual Studio は必要ありません。 以下のサンプルで、 [dotnet 発行](/dotnet/core/tools/dotnet-publish)プロジェクト ディレクトリからコマンドを実行 (が含まれています、 *.csproj*ファイル)。 指定しない場合、プロジェクト フォルダー内に明示的に渡すプロジェクト ファイルのパスにします。 例えば:

```console
dotnet publish C:\Webs\Web1
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

既定の発行フォルダーは `bin\$(Configuration)\netcoreapp<version>\publish` です。 既定の`$(Configuration)`は*デバッグ*です。 上記のサンプル、`<TargetFramework>`は`netcoreapp2.0`します。

`dotnet publish -h` では、発行のヘルプ情報が表示されます。

次のコマンドでは、`Release` ビルドと発行ディレクトリを指定します。

```console
dotnet publish -c Release -o C:\MyWebs\test
```

[Dotnet 発行](/dotnet/core/tools/dotnet-publish)コマンドを呼び出し、MSBuild を呼び出す、`Publish`ターゲットです。 渡されるパラメーター `dotnet publish` MSBuild に渡されます。 `-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。 `-o` パラメーターは `OutputPath` にマップされます。

MSBuild プロパティは、次の形式のいずれかを使用して渡すことができます。

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

次のコマンドは、`Release` ビルドをネットワーク共有に発行します。

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

ネットワーク共有は、スラッシュを使用して指定します (*//r8/*).NET Core がサポートされるすべてのプラットフォームで使用できます。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

## <a name="publish-profiles"></a>プロファイルを発行する

このセクションでは、Visual Studio 2017 を使用して、発行プロファイルを作成します。 作成した後は、Visual Studio またはコマンドラインからの発行を使用できます。

発行プロファイルには、発行プロセスが簡略化され、任意の数のプロファイルが存在できます。 次のパスのいずれかを選択して、Visual Studio で発行プロファイルを作成します。

* ソリューション エクスプ ローラーでプロジェクトを右クリックし **発行**です。
* 選択**発行&lt;project_name&gt;** から、**ビルド**メニュー。

**発行**アプリ容量ページのタブが表示されます。 プロジェクトでは、発行プロファイルがない、次のページが表示されます。

![アプリの容量がページの [発行] タブ](visual-studio-publish-profiles/_static/az.png)

ときに**フォルダー**は選択すると、パブリッシュされたメディアを保存するフォルダー パスを指定します。 既定のフォルダーは*bin\Release\PublishOutput*です。 クリックして、**プロファイルの作成**[完了] をクリックします。

発行プロファイルを作成した後、**発行** タブの変更。 新しく作成したプロファイルは、ドロップダウン リストに表示されます。 をクリックして**新しいプロファイルを作成**を別の新しいプロファイルを作成します。

![FolderProfile を表示するアプリの容量がページの [発行] タブ](visual-studio-publish-profiles/_static/create_new.png)

発行ウィザードは、次の発行ターゲットをサポートしています。

* Azure App Service
* Azure Virtual Machines
* IIS、FTP など (の任意の web サーバー)
* フォルダー
* プロファイルのインポート

詳細については、次を参照してください。[適切な発行オプション](/visualstudio/ide/not-in-toc/web-publish-options)です。

Visual Studio が、発行プロファイルを作成するときに、*プロパティ/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild ファイルを作成します。 *.Pubxml*ファイルは、MSBuild とファイルが含まれています構成設定を発行します。 このファイルは、ビルドのカスタマイズし、発行プロセスに変更することができます。 このファイルは、発行プロファイルで読み取られます。 `<LastUsedBuildConfiguration>` 特殊なため、グローバル プロパティであり、ビルドでインポートするすべてのファイルではないはずです。 参照してください[MSBuild: 構成プロパティを設定する方法](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)詳細についてはします。

ターゲットの Azure にパブリッシュする場合、 *.pubxml*ファイルには、Azure サブスクリプション識別子が含まれています。 そのターゲット型とソース管理にこのファイルを追加することをお勧めします。 Azure 以外のターゲットに発行するときは、チェックインする安全、 *.pubxml*ファイル。

上 (パブリッシュ用のパスワード) などの機密情報は暗号化されて、個々 のユーザー/コンピューター レベル。 格納されている、*プロパティ/PublishProfiles/&lt;profile_name&gt;. pubxml.user*ファイル。 このファイルでは、機密情報を格納できるため、ソース管理にチェックインするべきではありません。

ASP.NET Core 上の web アプリを公開する方法の詳細については、次を参照してください。[ホストを展開および](xref:host-and-deploy/index)です。 MSBuild タスクおよび ASP.NET Core アプリケーションを発行するために必要なターゲットは、オープン ソースでhttps://github.com/aspnet/websdkです。

`dotnet publish` MSDeploy のフォルダーを使用し、 [Kudu](https://github.com/projectkudu/kudu/wiki)プロファイルの発行します。

フォルダー (クロスプラット フォームで機能):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (現在こののみで機能します Windows MSDeploy クロスプラット フォームのないため)。

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy パッケージ (現在こののみで機能します Windows MSDeploy クロスプラット フォームのないため)。

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

上記のサンプルで**しない**渡す`deployonbuild`に`dotnet publish`です。

詳細については、次を参照してください。 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)です。

`dotnet publish` 任意のプラットフォームから Azure に発行する Kudu Api をサポートしています。 Visual Studio では、サポートが Kudu Api でサポートされて WebSDK クロスプラット フォームが Azure に発行を公開します。

発行プロファイルを追加、*プロパティ/PublishProfiles*次のコンテンツ フォルダー。

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

発行の内容を zip 圧縮し、Kudu Api を使用して Azure に発行するには、次のコマンドを実行します。

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

という名前のプロファイルで発行するときに*FolderProfile*、次のコマンドのいずれかを実行することができます。

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

呼び出すときに[dotnet ビルド](/dotnet/core/tools/dotnet-build)、呼び出す`msbuild`ビルドを実行してプロセスを発行します。 いずれかを呼び出す`dotnet build`または`msbuild`フォルダー プロファイルに渡す場合と同じです。 Windows 上で直接には、MSBuild を呼び出し、MSBuild の .NET Framework のバージョンが使用されます。 MSDeploy の発行は、現在 Windows コンピューターに制限されています。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。 フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。

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

`<LastUsedBuildConfiguration>` は `Release` に設定されます。 Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。 `<LastUsedBuildConfiguration>`構成プロパティは特別であり、インポートされた MSBuild ファイル内でオーバーライドすることはできません。 コマンドラインからこのプロパティをオーバーライドすることができます。

.NET Core CLI を使用します。

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

MSBuild の使用:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>コマンド ラインから MSDeploy エンドポイントに発行する

発行は、.NET Core CLI または MSBuild を使用して実現できます。 `dotnet publish` は .NET Core のコンテキストで実行されます。 `msbuild`コマンドには、.NET Framework は、Windows 環境に制限が必要です。

MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。

次の例では、ASP.NET Core web アプリを作成 (を使用して`dotnet new mvc`)、Visual Studio で Azure の発行プロファイルを追加します。

実行`msbuild`から、 **VS 2017 用開発者コマンド プロンプト**です。 開発者コマンド プロンプトが、正しい*msbuild.exe*一部 MSBuild 変数セットをそのパスにします。

MSBuild は次の構文を使用します。

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

取得、`Password`から、 *\<発行名 >。PublishSettings*ファイル。 ダウンロード、*です。PublishSettings*からいずれかのファイル。

* ソリューション エクスプ ローラー: は、この Web アプリでを右クリックし、選択**発行プロファイルのダウンロード**です。
* Azure ポータル: をクリックして**Get 発行プロファイル**Web app の**概要**パネルです。

`Username` は発行プロファイルで確認できます。

次のサンプルは、 *Web11112 - Web Deploy*発行プロファイル。

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>ファイルを除外します。

ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。 `msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。 たとえば、次`<Content>`要素には、すべてのテキストが含まれません (*.txt*) ファイルから、 *wwwroot/コンテンツ*フォルダーとそのすべてのサブフォルダーです。

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上記のマークアップは、発行プロファイルに追加することができます、または *.csproj*ファイル。 *.csproj* ファイルに追加すると、プロジェクト内のすべての発行プロファイルにルールが追加されます。

次`<MsDeploySkipRules>`要素からすべてのファイルを除外する、 *wwwroot/コンテンツ*フォルダー。

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

場合、次`<MsDeploySkipRules>`要素が追加される、展開のサイトでそれらのファイルは削除はありません。

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

前述の`<MsDeploySkipRules>`要素を防ぐ、*スキップ*ファイルを展開します。 それらのファイルは、配置されると、削除されません。

次`<Content>`要素は、展開のサイトで対象となるファイルを削除します。

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

コマンドライン配置を使用して、前述の`<Content>`要素には、次の出力が得られます。

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

次のマークアップが含まれています、*イメージ*フォルダーをプロジェクト ディレクトリの外部、 *wwwroot/イメージ*発行サイトのフォルダー。

```xml
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

## <a name="run-a-target-before-or-after-publishing"></a>発行の前または後にターゲットを実行する

組み込み`BeforePublish`と`AfterPublish`発行先の前後に、ターゲットがターゲットを実行します。 前に、と発行後に、コンソール メッセージをログに発行プロファイルを次の要素を追加します。

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>信頼されていない証明書を使用してサーバーに発行します。

追加、`<AllowUntrustedCertificate>`の値を持つプロパティ`True`発行プロファイルに。

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu サービス

Azure App Service web アプリの展開で、ファイルを表示する、 [Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)です。 追加、 `scm` web アプリ名をトークンです。 例えば:

| URL                                    | 結果       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Web アプリ      |
| `http://mysite.scm.azurewebsites.net/` | Kudu サービス |

選択、[デバッグ コンソール](https://github.com/projectkudu/kudu/wiki/Kudu-console)メニュー項目を表示、編集、削除、またはファイルを追加します。

## <a name="additional-resources"></a>その他の技術情報

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) が IIS サーバーへの web サイトと web アプリの展開を簡略化します。
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): ファイルに問題、機能の展開を要求してください。
