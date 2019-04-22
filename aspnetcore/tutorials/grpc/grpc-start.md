---
title: 'チュートリアル: ASP.NET Core の gRPC の概要'
author: juntaoluo
description: このチュートリアル シリーズでは、ASP.NET Core で gRPC サービスを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、および二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672568"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>チュートリアル: ASP.NET Core の gRPC サービスの概要

作成者: [John Luo](https://github.com/juntaoluo)

このチュートリアルでは、ASP.NET Core で gRPC サービスをビルドする基本について説明します。

最後には、あいさつをエコーする gRPC サービスができます。

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * gRPC サービスを作成します。
> * gRPC サービスを実行します。
> * プロジェクト ファイルを確認する

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>gRPC サービスの作成

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio の **[ファイル]** メニューから、**[新規作成]** > **[プロジェクト]** の順に選択します。
* 新しい ASP.NET Core Web アプリケーションを作成します。
  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.1.png)
* プロジェクトに **GrpcGreeter** という名前を付けます。 コードのコピーおよび貼り付けを行う際に名前空間が一致するように、プロジェクトに *GrpcGreeter* という名前を付けることが重要です。
  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/np_3_0.2.png)
* ドロップダウンで **[.NET Core]** と **[ASP.NET Core 3.0]** を選択します。 **gRPC サービス** テンプレートを選択します。

  次のスターター プロジェクトが作成されます。

  ![ソリューション エクスプローラー](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。
* ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。
* 次のコマンドを実行します。

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * `dotnet new` コマンドでは、*GrpcGreeter* フォルダー内に新しい gRPC サービスが作成されます。
  * `code` コマンドでは、Visual Studio Code の新しいインスタンス内に *GrpcGreeter* フォルダーが開かれます。

  "**ビルドとデバッグに必要な資産が 'GrpcGreeter' にありません。追加しますか?**" という内容のダイアログ ボックスが表示されたら、
* **[はい]** を選択します

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

端末から、次のコマンドを実行します。

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

上記のコマンドでは、[.NET Core CLI](/dotnet/core/tools/dotnet) を使用して、gRPC サービスが作成されます。

### <a name="open-the-project"></a>プロジェクトを開く

Visual Studio から、**[ファイル]、[開く]** の順に選択し、*GrpcGreeter.sln* ファイルを選択します。

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>サービスを実行する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* デバッガーなしで gRPC サービスを実行するには、Ctrl+F5 キーを押します。

  Visual Studio では、コマンド プロンプトでサービスが実行されます。 サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* `dotnet run` を使用し、コマンド ラインから gRPC あいさつプロジェクト GrpcGreeter を実行します。 サービスが `http://localhost:50051` でリッスンを開始したことがログに示されます。

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

次のチュートリアルでは、挨拶サービスをテストするために必要な gRPC クライアントを構築する方法について説明します。

### <a name="examine-the-project-files-of-the-grpc-project"></a>gRPC プロジェクトのプロジェクト ファイルを調査する

GrpcGreeter ファイル:

* greet.proto:*Protos/greet.proto* ファイルは、`Greeter` gRPC を定義し、gRPC サーバー資産を生成するために使用されます。 詳細については、「<xref:grpc/index>」を参照してください。
* *Services* フォルダー:`Greeter` サービスの実装が含まれます。
* *appSettings.json*: Kestrel で使用されるプロトコルなどの構成データが含まれています。 詳細については、「<xref:fundamentals/configuration/index>」を参照してください。
* *Program.cs*:gRPC サービスのエントリ ポイントが含まれています。 詳細については、「<xref:fundamentals/host/web-host>」を参照してください。
* Startup.cs:アプリの動作を構成するコードが含まれています。 詳細については、「<xref:fundamentals/startup>」を参照してください。

### <a name="test-the-service"></a>サービスをテストする

## <a name="additional-resources"></a>その他の技術情報

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * gRPC サービスを作成する。
> * gRPC サービスを実行しました。
> * プロジェクト ファイルを確認する。

> [!div class="step-by-step"]
> [次へ: .NET Core gRPC クライアントを作成する](xref:tutorials/grpc/grpc-client)