---
title: "Windows サービスのホスト"
author: tdykstra
description: "Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。"
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Windows サービスでの ASP.NET Core アプリをホストします。

によって[Tom Dykstra](https://github.com/tdykstra)

含まないで実行するには IIS を使用して、Windows 上の ASP.NET Core アプリケーションをホストすることをお勧め、 [Windows サービス](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。 このようにして自動的に開始できますが再起動し、クラッシュの後にログインするユーザーが待機することがなく。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))。 参照してください、[次の手順](#next-steps)それを実行する方法についてのセクションでします。

## <a name="prerequisites"></a>必須コンポーネント

* アプリは、.NET Framework ランタイムで実行する必要があります。  *.Csproj*ファイルでの適切な値を指定[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)と[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)です。 次に例を示します。

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)**テンプレート。

* アプリは、(内部ネットワーク) からだけでなく、インターネットから要求を受け取る場合は使用する必要があります、 [WebListener](xref:fundamentals/servers/weblistener) web サーバーではなくより[Kestrel](xref:fundamentals/servers/kestrel)です。  Kestrel は、IIS とエッジ展開するために使用する必要があります。  詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

## <a name="getting-started"></a>作業の開始

このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。

* NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。

* 次の変更を加え`Program.Main`:
  
  * 呼び出す`host.RunAsService`の代わりに`host.Run`です。
  
  * コードを呼び出す場合`UseContentRoot`パスの代わりに、発行場所を使用します。`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* フォルダーにアプリケーションを発行します。

  使用して[dotnet 発行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles)フォルダーに発行します。

* テストを作成し、サービスを開始します。

  使用する管理者コマンド プロンプト ウィンドウを開き、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。  
  
  MyService サービスの名前が場合に、アプリの発行`c:\svc`AspNetCoreService の名前は、アプリ自体は、コマンドは次のようになります。

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  `binPath`値は、アプリの実行可能ファイル、実行可能ファイル名そのものを含むへのパス。

  ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

  これらのコマンドが完了、[参照] と同じパスに、コンソール アプリケーションとして実行する場合 (既定では、 `http://localhost:5000`)

  ![サービスで実行されています。](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>サービスの外部で実行するための手段します。

テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行中にデバッグしやすく`host.RunAsService`特定の条件下でのみです。  コンソール アプリとしてアプリを実行できますなど、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントを処理します。

処理するために`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加します。

* `WebHostService` から派生するクラスを作成します。

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* 拡張メソッドを作成する`IWebHost`カスタムをパスする`WebHostService`に`ServiceBase.Run`です。

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* `Program.Main`変更メソッドを呼び出して、新しい拡張機能の代わりに`host.RunAsService`です。

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

場合、カスタム`WebHostService`(ロガー) などの依存関係の挿入からサービスを取得から取得する必要があるコード、`Services`プロパティ`IWebHost`です。

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>次の手順

[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample)に付属しているこの記事は、前のコード例に示すように変更されている単純な MVC web アプリです。  実行するには、サービスで、次の手順を行います。

* 公開*c:\svc*です。

* 管理者のウィンドウを開きます。

* 次のコマンドを入力します。

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * ブラウザーで実行されていることを確認する http://localhost:5000 に移動します。

エラー メッセージにアクセスできるようにする簡単な方法がなどのログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](xref:fundamentals/logging/index#eventlog)です。

## <a name="acknowledgments"></a>受信確認

この記事は、既に発行されたソースを利用して記述されています。 最も古いとそれらの最も役に立つ以下のとおりです。

* [Windows サービスとしての ASP.NET Core のホスト](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Windows サービスで、ASP.NET Core をホストする方法](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
