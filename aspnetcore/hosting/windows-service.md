---
title: "Windows サービスのホスト"
author: tdykstra
description: "Windows サービスで ASP.NET Core アプリケーションをホストする方法を説明します。"
keywords: "ASP.NET Core、Windows サービスをホストしています。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: 5b54c77ff9e019b1d550aa687923077a3e9ba5c2
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Windows サービスでの ASP.NET Core アプリをホストします。

によって[Tom Dykstra](https://github.com/tdykstra)

IIS を使用しない場合は、Windows 上の ASP.NET Core アプリケーションをホストするための推奨方法がで実行するには、 [Windows サービス](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications)です。 このようにして自動的に開始できますが再起動し、クラッシュの後にログインするユーザーが待機することがなく。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)を参照してください、[次の手順](#next-steps)それを実行する方法についてのセクションでします。

## <a name="prerequisites"></a>必須コンポーネント

* .NET framework ランタイムでは、アプリが実行する必要があります。  *.Csproj*ファイルでの適切な値を指定[TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks)と[RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog)です。 次に例を示します。

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Visual Studio で、プロジェクトを作成するときに使用して、 **ASP.NET Core アプリケーション (.NET Framework)**テンプレート。

* アプリは、要求を取得する (内部ネットワーク) からだけでなく、インターネットからは場合は、使用する必要があります、 [WebListener](xref:fundamentals/servers/weblistener) web サーバーではなくより[Kestrel](xref:fundamentals/servers/kestrel)です。  Kestrel は、IIS とエッジ展開するために使用する必要があります。  詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

## <a name="getting-started"></a>作業の開始

このセクションでは、サービスで実行する既存の ASP.NET Core プロジェクトを設定するために必要な最小の変更について説明します。

* NuGet パッケージのインストール[Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/)です。

* 次の変更を加え`Program.Main`:
  
  * 呼び出す`host.RunAsService`の代わりに`host.Run`です。
  
  * コードを呼び出す場合`UseContentRoot`パスの代わりに、発行場所を使用します。`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* フォルダーにアプリケーションを発行します。

  使用して[dotnet 発行](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish)または[発行プロファイルを Visual Studio](xref:publishing/web-publishing-vs)フォルダーに発行します。

* テストを作成し、サービスを開始します。

  使用する管理者コマンド プロンプト ウィンドウを開き、 [sc.exe](https://technet.microsoft.com/library/bb490995)コマンド ライン ツールを作成し、サービスを開始します。  
  
  アプリを公開する場合は MyService サービスの名前を入力`c:\svc`AspNetCoreService の名前は、アプリ自体は、コマンドは次のようになります。

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  `binPath`値は、アプリの実行可能ファイル、実行可能ファイル名そのものを含むへのパス。

  ![コンソール ウィンドウを作成して例の開始](windows-service/_static/create-start.png)

  これらのコマンドが完了は、コンソール アプリケーションとして実行したときと同じパスを参照することができます (既定では、 `http://localhost:5000`)

  ![サービスで実行されています。](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>サービスの外部で実行するための手段します。

テストおよびが通常の操作を呼び出すコードを追加するために、サービスの外部で実行している場合にデバッグしやすく`host.RunAsService`特定の条件下でのみです。  たとえば、でしたを実行するコンソール アプリケーションとして表示される場合、`--console`コマンドライン引数や、デバッガーがアタッチされているかどうか。

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>停止および開始イベントを処理します。

処理する場合`OnStarting`、 `OnStarted`、および`OnStopping`イベントでは、次の変更、追加。

* `WebHostService` から派生するクラスを作成します。

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* 拡張メソッドを作成する`IWebHost`独自で渡された`WebHostService`に`ServiceBase.Run`です。

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* `Program.Main`変更メソッドを呼び出して、新しい拡張機能の代わりに`host.RunAsService`です。

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

場合、カスタム`WebHostService`(ロガー) などの依存関係の挿入からサービスを取得する必要があるコードから取得できます、`Services`プロパティ`IWebHost`です。

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>次のステップ

[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample)に付属しているこの記事は、前のコード例に示すように変更されている単純な MVC web アプリです。  実行するには、サービスで、次の手順を行います。

* 公開*c:\svc*です。

* 管理者のウィンドウを開きます。

* 次のコマンドを入力します。

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * ブラウザーで実行されていることを確認する http://localhost:5000 に移動します。

エラー メッセージにアクセスできるようにする簡単な方法がなどのログ プロバイダーを追加するには、アプリが、サービスで実行する場合を想定どおりに起動しない場合、 [Windows イベント ログ プロバイダー](xref:fundamentals/logging#eventlog)です。

## <a name="acknowledgments"></a>受信確認

この記事は、既に発行されたソースを利用して記述されています。 最も古いとそれらの最も役に立つ以下のとおりです。

* [Windows サービスとしての ASP.NET Core のホスト](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Windows サービスで、ASP.NET Core をホストする方法](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
