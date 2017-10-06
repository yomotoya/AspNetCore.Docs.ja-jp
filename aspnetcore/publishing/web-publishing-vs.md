---
title: "Visual Studio および MSBuild 用の発行プロファイルを作成する"
author: rick-anderson
description: "Visual Studio の Web 発行について説明します。"
keywords: "ASP.NET Core, Web 発行, 発行, msbuild, Web 配置, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>ASP.NET Core アプリを展開するために、Visual Studio および MSBuild 用の発行プロファイルを作成する

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

上記のマークアップで (1 行目にある) `<Project>` 要素の `Sdk` 属性は、以下の処理を実行しています。

* 最初に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* から `props` ファイルをインポートします。
* 最後に *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* からターゲット ファイルをインポートします。

(Visual Studio 2017 Enterprise の場合) `MSBuildSDKsPath` の既定の場所は、*%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* フォルダーです。

`Microsoft.NET.Sdk.Web` は以下に依存しています。

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

そのため、以下の `props` と `targets` がインポートされます。

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

使用される発行方法に基づいて、発行ターゲットは適切なターゲット セットをもう一度インポートします。

MSBuild または Visual Studio がプロジェクトを読み込むと、次の高レベルのアクションが実行されます。

* プロジェクトをビルドする
* 発行するファイルを計算する
* ターゲットにファイルを発行する

### <a name="compute-project-items"></a>プロジェクト項目を比較する

プロジェクトが読み込まれると、プロジェクト項目 (ファイル) が計算されます。 `item type` 属性によって、ファイルの処理方法が決まります。 既定で、*.cs* ファイルは `Compile` 項目一覧に含まれています。 `Compile` 項目一覧のファイルがコンパイルされます。

`Content` 項目一覧には、ビルド出力に加え、発行されるファイルが含まれています。 既定で、wwwroot/** というパターンに一致するファイルが `Content` 項目に含まれます。 [wwwroot/** は glob パターンであり](https://gruntjs.com/configuring-tasks#globbing-patterns)、*wwwroot* フォルダー**および**サブフォルダーのすべてのファイルが指定されます。 発行一覧に明示的にファイルを追加するには、「[ファイルを含める](#including-files)」で説明されているように、*.csproj* ファイルに直接ファイルを追加します。

Visual Studio で **[発行]** ボタンを選択するか、コマンド ラインから発行した場合:

- プロパティ/項目が計算されます (ビルドに必要なファイル)。
- Visual Studio のみ: NuGet パッケージが復元されます  (CLI では、ユーザーが明示的に復元する必要があります)。
- プロジェクトがビルドされます。
- 発行項目が計算されます (発行に必要なファイル)。
- プロジェクトが発行されます (計算されたファイルは、発行先にコピーされます)。

## <a name="basic-command-line-publishing"></a>基本的なコマンドラインによる発行

このセクションの方法は、.NET Core がサポートされるすべてのプラットフォームに使用できます。Visual Studio は必要ありません。 以下の例では、プロジェクト ディレクトリ (*.csproj* ファイルが含まれているディレクトリ) から `dotnet publish` コマンドが実行されます。 現在のフォルダーがプロジェクト フォルダーではない場合は、プロジェクト ファイル パスで明示的に渡すことができます。 例:

```console
dotnet publish  c:/webs/web1
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

--------------

`dotnet publish` では次のような出力が生成されます。

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

`dotnet publish` コマンドは、`Publish` ターゲットを呼び出す `MSBuild` を呼び出します。 `dotnet publish` に渡されたすべてのパラメーターは、`MSBuild` に渡されます。 `-c` パラメーターは、`Configuration` MSBuild プロパティにマップされます。 `-o` パラメーターは `OutputPath` にマップされます。

次のいずれかの形式を使用して MSBuild プロパティを渡すことができます。

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

次のコマンドは、`Release` ビルドをネットワーク共有に発行します。

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

ネットワーク共有は、スラッシュを使用して指定します (*//r8/*)。.NET Core がサポートされるすべてのプラットフォームで使用できます。

配置用に発行したアプリが実行されていないことを確認します。 アプリが実行中は、*publish* フォルダー内のファイルがロックされます。 ロックされているファイルはコピーできないため、配置は行われません。

## <a name="publish-profiles"></a>プロファイルを発行する

このセクションでは、Visual Studio 2017 以降を使用して発行プロファイルを作成します。 作成したら、Visual Studio またはコマンド ラインから発行できます。

発行プロファイルによって、発行プロセスが簡単になります。 複数の発行プロファイルを持つことができます。 Visual Studio で発行プロファイルを作成するには、ソリューション エクスプローラーでプロジェクトを右クリックし、**[発行]** を選択します。 また、ビルド メニューから **[\<プロジェクト名> の発行]** を選択する方法もあります。 アプリケーション容量ページの **[発行]** タブが表示されます。 プロジェクトに発行プロファイルが含まれていない場合、次のページが表示されます。

![アプリケーション容量ページの **[発行]** タブに、Azure、IIS、FTB、フォルダーが表示され、Azure が選択されている図。 また、[新規作成] と [既存のものを選択] のラジオ ボタンも表示されます。](web-publishing-vs/_static/az.png)

**[フォルダー]** が選択されている場合、**[発行]** ボタンをクリックすると、フォルダーの発行プロファイルが作成され、発行されます。

![Azure、IIS、FTB、フォルダーが表示されているアプリケーション容量ページの **[発行]** タブ](web-publishing-vs/_static/pub1.png)

発行プロファイルが作成されると、**[発行]** タブが変わり、**[新しいプロファイルの作成]** を選択して新しいプロファイルを作成します。

![最後の手順で作成した FolderProfile と、[新しいプロファイルの作成] リンクが表示されたアプリケーション容量ページの **[発行]** タブ](web-publishing-vs/_static/create_new.png)

発行ウィザードは、次の発行ターゲットをサポートしています。

- Microsoft Azure App Service
- IIS、FTP など (任意の Web サーバーの場合)
- フォルダー
- プロファイルのインポート (プロファイルをインポートできます)。
- Microsoft Azure Virtual Machines

詳細については、「[状況に適した発行オプション](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)」を参照してください。

Visual Studio で発行プロファイルを作成すると、*Properties/PublishProfiles/\<発行名>.pubxml* MSBuild ファイルが作成されます。 この *.pubxml* ファイルは MSBuild ファイルであり、発行構成設定が含まれています。 このファイルを変更して、ビルドと発行プロセスをカスタマイズすることができます。 このファイルは、発行プロファイルで読み取られます。 `<LastUsedBuildConfiguration>` は、グローバル プロパティであり、ビルドにインポートされるファイル内には含めないので、特殊です。 詳細については、「[MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)」(MSBuild: 構成プロパティの設定方法) を参照してください。
*.pubxml* ファイルはソース コード管理にチェックインしないことをお勧めします。このファイルは *.user* ファイルに依存しています。 また、*.user* ファイルは、機密情報が含まれている可能性があり、1 ユーザーおよびコンピューターに対してのみ有効なので、ソース コード管理にチェックインしないでください。

機密情報 (発行のパスワードなど) はユーザー/コンピューターごとのレベルで暗号化され、*Properties/PublishProfiles/\<発行名>.pubxml.user* ファイルに保存されます。 このファイルには機密情報が含まれている可能性があるため、ソース コード管理に**チェックインしないでください**。

ASP.NET Core で Web アプリを発行する方法の概要については、「[Publishing and Deployment](index.md)」(発行と配置) を参照してください。 [Publishing and Deployment](index.md) は、https://github.com/aspnet/websdk のオープン ソース プロジェクトです。

 `dotnet publish` は Msdeploy のフォルダーを使用することができ、[KUDU](https://github.com/projectkudu/kudu/wiki) は次のプロファイルを発行します。
 
フォルダー (クロスプラット フォームで機能) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

Msdeploy (msdeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Msdeploy パッケージ (msdeploy はクロスプラットフォームではないため、これは現在 Windows のみで機能します): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

上記の例で、`deployonbuild` を `dotnet publish` に**渡さない**ようにしてください。

詳細については、「[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)」を参照してください

`dotnet publish` は、任意のプラットフォームから Azure に発行する KUDU API をサポートしています。 Visual Studio の発行は、KUDU API をサポートしていますが、Azure へのクロスプラットフォームの発行は websdk がサポートしています。

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

次のコマンドを実行すると発行するコンテンツが圧縮され、KUDU API を使用して Azure に発行されます。

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

発行プロファイルを使用する場合、次の MSBuild プロパティを設定します。

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

たとえば、*FolderProfile* というプロファイルを使用して発行する場合、以下のいずれかのコマンドを実行できます。

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

`dotnet build` を呼び出すと、`msbuild` が呼び出され、ビルドと発行プロセスが実行されます。 `dotnet build` または `msbuild` の呼び出しは、フォルダー プロファイルで渡す場合と基本的に同じです。 Windows で MSBuild を直接呼び出すときに、MSBuild の .NET Framework バージョンを取得します。  MSDeploy の発行は、現在 Windows コンピューターに制限されています。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと MSBuild が呼び出され、MSBuild はフォルダー以外のプロファイルで MSDeploy を使用します。 フォルダー以外のプロファイルで `dotnet build` を呼び出すと、(MSDeploy を使用して) MSBuild が呼び出され、(Windows プラットフォームで実行している場合でも) エラーになります。 フォルダー以外のプロファイルで発行するには、MSBuild を直接呼び出します。

次のフォルダー発行プロファイルは、Visual Studio で作成され、ネットワーク共有に発行されます。

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

`<LastUsedBuildConfiguration>` は `Release` に設定されます。  Visual Studio から発行すると、発行プロセスが開始されたときの値を使用して、`<LastUsedBuildConfiguration>` 構成プロパティ値が設定されます。 `<LastUsedBuildConfiguration>` 構成プロパティは特殊なので、インポートされる MSBuild ファイルで上書きされないようにしてください。 コマンド ラインからこのプロパティを上書きできます。 例:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

MSBuild の使用:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>コマンド ラインから MSDeploy エンドポイントに発行する

前述のように、`dotnet publish` または `msbuild` コマンドを使用して発行できます。 `dotnet publish` は .NET Core のコンテキストで実行されます。 `msbuild` には .NET Framework が必要なので、Windows 環境に制限されます。

MSDeploy を使用して発行するには、Visual Studio 2017 でまず発行プロファイルを作成し、コマンド ラインからプロファイルを使用する方法が最も簡単です。

次の例では、(`dotnet new mvc` を使用して) ASP.NET Core Web アプリが作成され、Visual Studio を使用して Azure 発行プロファイルが追加されます。

**[開発者コマンド プロンプト for VS 2017]** から `msbuild` を実行します。 開発者コマンド プロンプトのパスには、正しい *msbuild.exe* が表示され、いくつかの MSBuild 編集が設定されます。

MSBuild は次の構文を使用します。

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

*\<発行名>.PublishSettings* ファイルから `Password` を取得できます。 *.PublishSettings* ファイルは次の機能でダウンロードできます。

- ソリューション エクスプローラー: Web アプリを右クリックし、**[発行プロファイルのダウンロード]** を選択します。
- Microsoft Azure の管理ポータル: Web アプリ ブレードから **[発行プロファイルの取得]** を選択します。

`Username` は発行プロファイルで確認できます。

次の例では、"Web11112 - Web 配置" 発行プロファイルを使用しています。

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>ファイルの除外

ASP.NET Core Web アプリを発行すると、ビルド成果物と *wwwroot* フォルダーの内容が含まれます。 `msbuild` は [glob パターン](https://gruntjs.com/configuring-tasks#globbing-patterns)をサポートしています。 たとえば、次の `<Content>` 要素マークアップでは、*wwwroot/content* フォルダーとそのすべてのサブフォルダーからすべてのテキスト (*.txt*) ファイルが除外されます。

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
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` は、配置サイトから *skip* ターゲットは削除されません。 `<Content>` のターゲットとなるファイルとフォルダーは、配置サイトから削除されます。 たとえば、次のファイルを含む Web アプリを配置したとします。

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

次の `<MsDeploySkipRules>` マークアップを追加した場合、配置サイトのそれらのファイルは削除されません。

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

上記の `<MsDeploySkipRules>` マークアップは*スキップ対象の*ファイルの配置を防ぎますが、配置されているそれらのファイルは削除しません。

次の `<Content>` マークアップは、配置サイトのターゲット ファイルを削除します。

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

上記の `<Content>` マークアップを指定してコマンド ライン配置を使用すると、次のような出力になります。

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

次のマークアップを使用して、プロジェクト ディレクトリにない *images* フォルダーを発行サイトの *wwwroot/images* フォルダーに含めることができます。

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

このマークアップは、*.csproj* ファイルまたは発行プロファイルに追加できます。 *.csproj* ファイルに追加した場合、プロジェクトの各発行プロファイルに含まれます。

次の強調表示されているマークアップは、この方法を示しています。

* プロジェクトの外部にあるファイルを *wwwroot* フォルダーにコピーします。
* *wwwroot\Content* フォルダーを除外します。
* *Views\Home\About2.cshtml* を除外します。

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

他の配置例については、[Web SDK の Readme](https://github.com/aspnet/websdk) を参照してください。

### <a name="run-a-target-before-or-after-publishing"></a>発行の前または後にターゲットを実行する

組み込みの `BeforePublish` および `AfterPublish` ターゲットを使用して、発行ターゲットの前または後にターゲットを実行できます。 次のマークアップを発行プロファイルに追加して、発行の前と後にメッセージのログをコンソールの出力に記録することができます。

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Kudu サービス

Azure Web アプリでファイルを表示するには、[Kudu サービス](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)を使用します。 `scm` トークンを名前または Web アプリに追加します。 例:

| URL               | 結果|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Web アプリ |
| `http://mysite.scm.azurewebsites.net/` | Kudu サービス |

[[デバッグ コンソール]](https://github.com/projectkudu/kudu/wiki/Kudu-console) メニュー項目を選択して、ファイルの表示、編集、削除、追加を行います。

## <a name="additional-resources"></a>その他の技術情報

- [Web 配置](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) を使用すると、Web アプリケーションと Web サイトを IIS サーバーに配置する処理を簡略化できます。

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): 配置のファイルの問題と要求機能。
