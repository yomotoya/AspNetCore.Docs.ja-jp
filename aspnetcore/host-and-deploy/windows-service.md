---
title: Windows サービスで ASP.NET Core をホストします。
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
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Windows サービスで ASP.NET Core をホストします。

著者: [Tom Dykstra](https://github.com/tdykstra)

含まないで実行するには IIS を使用して、Windows 上の ASP.NET Core アプリケーションをホストすることをお勧め、 [Windows サービス](/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。 Windows サービスとしてホストされた場合、アプリが自動的に実行できます後の開始を再起動し、人間の介入を必要とせずにクラッシュします。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。 サンプル アプリを実行する方法の手順は、サンプルを参照してください*README.md*ファイル。

## <a name="prerequisites"></a>必須コンポーネント

* アプリは、.NET Framework ランタイムで実行する必要があります。 *.Csproj*ファイルでの適切な値を指定[TargetFramework](/nuget/schema/target-frameworks)と[RuntimeIdentifier](/dotnet/articles/core/rid-catalog)です。 次に例を示します。

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)** テンプレート。

* アプリは、(内部ネットワーク) からだけでなく、インターネットから要求を受け取る場合は使用する必要があります、 [HTTP.sys](xref:fundamentals/servers/httpsys) web サーバー (以前の[WebListener](xref:fundamentals/servers/weblistener) 1.x アプリの ASP.NET Core)ではなく[Kestrel](xref:fundamentals/servers/kestrel)です。 IIS は推奨リバース プロキシ サーバーとして使用する Kestrel でエッジに導入されます。 詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

## <a name="get-started"></a>作業開始

このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。

1. NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。

2. 次の変更を加え`Program.Main`:

   * 呼び出す`host.RunAsService`の代わりに`host.Run`です。

   * コードを呼び出す場合`UseContentRoot`、パスの代わりに、発行場所を使用する`Directory.GetCurrentDirectory()`です。

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. フォルダーに、アプリを発行します。 使用して[dotnet 発行](/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles)フォルダーに発行します。

4. テストを作成し、サービスを開始します。

   使用する管理者の特権でコマンド シェルを開いて、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。 MyService サービスの名前が場合に、発行`c:\svc`AspNetCoreService をという名前のコマンドとします。

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   `binPath`値は、アプリの実行可能ファイル、実行可能ファイル名を含むへのパス。

   ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

   これらのコマンドが完了、[参照] と同じパスに、コンソール アプリケーションとして実行する場合 (既定では、 `http://localhost:5000`)。

   ![サービスで実行されています。](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>サービスの外部で実行するための手段します。

テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行中にデバッグしやすく`RunAsService`特定の条件下でのみです。 コンソール アプリとしてアプリを実行できますなど、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントを処理します。

処理するために`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加します。

1. 派生するクラスを作成する`WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. 拡張メソッドを作成する`IWebHost`カスタムをパスする`WebHostService`に`ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. `Program.Main`、新しい拡張メソッドを呼び出し、`RunAsCustomService`の代わりに`RunAsService`:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

場合、カスタム`WebHostService`コード (ロガー) などの依存関係の挿入からサービスを必要と、これからは取得、`Services`プロパティ`IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>プロキシ サーバーとロード バランサーのシナリオ

インターネットや企業ネットワークからの要求との対話しプロキシの背後にある、またはロード バランサーをサービスには、追加の構成を必要があります。 詳細については、「[プロキシ サーバーとロード バランサーを使用するために ASP.NET Core を構成する](xref:host-and-deploy/proxy-load-balancer)」を参照してください。

## <a name="acknowledgments"></a>謝辞

この記事の内容が公開されているリソースを利用して書き込まれます。

* [Windows サービスとしての ASP.NET Core のホスト](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Windows サービスで、ASP.NET Core をホストする方法](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
