---
title: Windows サービスでの ASP.NET Core のホスト
author: rick-anderson
description: Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153530"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスでの ASP.NET Core のホスト

著者: [Tom Dykstra](https://github.com/tdykstra)

IIS を使用せずに Windows で ASP.NET Core アプリケーションをホストするお勧めの方法は、[Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)でこれを実行することです。 Windows サービスとしてホストされると、再起動やクラッシュの後、人の介入なしにアプリを自動的に起動できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。 サンプル アプリを実行する方法については、サンプルの *README.md* ファイルを参照してください。

## <a name="prerequisites"></a>必須コンポーネント

* アプリは、.NET Framework ランタイムで実行する必要があります。 *.csproj* ファイルで、[TargetFramework](/nuget/schema/target-frameworks) と [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) の適切な値を指定します。 次に例を示します。

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Visual Studio でプロジェクトを作成する場合は、**ASP.NET Core アプリケーション (.NET Framework)** テンプレートを使用します。

* アプリが (内部ネットワークからだけではなく)、インターネットからの要求も受け取る場合は、アプリは [Kestrel](xref:fundamentals/servers/kestrel) ではなく、[HTTP.sys](xref:fundamentals/servers/httpsys) Web サーバー (旧称: ASP.NET Core 1.x アプリ向け [WebListener](xref:fundamentals/servers/weblistener)) を使用する必要があります。 エッジ展開用に Kestrel と共にリバース プロキシ サーバーとして IIS を使用することをお勧めします。 詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

## <a name="get-started"></a>作業開始

このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。

1. NuGet パッケージ [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/) をインストールします。

2. `Program.Main` で次の変更を行います。

   * `host.Run` の代わりに `host.RunAsService` を呼び出します。

   * コードが `UseContentRoot` を呼び出した場合、`Directory.GetCurrentDirectory()` の代わりに発行場所のパスを使用します。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. アプリをフォルダーに発行します。 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) またはフォルダーに発行する [Visual Studio 発行プロファイル](xref:host-and-deploy/visual-studio-publish-profiles)を使用します。

4. サービスを作成、開始してテストします。

   管理特権を持つコマンド シェルを開いて、[sc.exe](https://technet.microsoft.com/library/bb490995) コマンドライン ツールを使用して、サービスを作成して開始します。 サービスに MyService という名前を付けて `c:\svc` に発行し、AspNetCoreService をという名前を付けた場合、コマンドは次のようになります。

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath` 値はアプリの実行可能ファイルへのパスです。これには、実行可能ファイルの名前が含まれます。

   ![コンソール ウィンドウの作成および開始例](windows-service/_static/create-start.png)

   これらのコマンドが完了したら、コンソール アプリとしての実行時と同じパスを参照します (既定では `http://localhost:5000`)。

   ![サービスでの実行](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>サービスの外部で実行するための方法の提供

サービスの外部で実行する場合のほうがテストおよびデバッグは簡単です。このため、特定の条件下でのみ `RunAsService` を呼び出すコードを追加することが一般的です。 たとえば、`--console` コマンドライン引数を使用してアプリをコンソール アプリとしてアプリを実行できます。または、デバッガーがアタッチされている場合は、以下を実行します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントの処理

`OnStarting`、`OnStarted`、および `OnStopping` イベントを処理するには、次の追加の変更を行います。

1. `WebHostService` から派生するクラスを作成します。

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. カスタム `WebHostService` を `ServiceBase.Run` に渡す `IWebHost` の拡張メソッドを作成します。

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main` で、`RunAsService` ではなく、新しい拡張メソッド `RunAsCustomService` を呼び出します。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

カスタム `WebHostService` コードに依存関係の挿入からのサービスが必要な場合は (ロガーなど)、`IWebHost` の `Services` プロパティから取得します。

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットまたは企業ネットワークからの要求とやり取りするサービスやプロキシまたはロード バランサーの背後にあるサービスでは、追加の構成が必要になる場合があります。 詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。

## <a name="acknowledgments"></a>謝辞

この記事は、次の公開されているソースを参照して作成されました。

* [Hosting ASP.NET Core as Windows service (Windows サービスとしての ASP.NET Core のホスト)](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [How to host your ASP.NET Core in a Windows Service (Windows サービスで ASP.NET Core をホストする方法)](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
