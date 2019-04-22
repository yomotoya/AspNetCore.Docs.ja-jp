---
title: 'チュートリアル: .NET Core gRPC クライアントを作成する'
author: juntaoluo
description: このチュートリアル シリーズでは、ASP.NET Core で gRPC サービスを作成する方法を示します。 gRPC サービス プロジェクトの作成方法、proto ファイルの編集方法、二重ストリーミング呼び出しの追加方法について学習します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674164"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>チュートリアル: .NET Core gRPC クライアントを作成する

作成者: [John Luo](https://github.com/juntaoluo)

このチュートリアルでは、gRPC サービスと通信できる .NET Core [gRPC](https://grpc.io/docs/guides/) クライアントを作成する方法を紹介します。

最終的に、gRPC あいさつサービスと通信する gRPC クライアントが与えられます。

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * gRPC クライアントを作成します。
> * このサービスを前のチュートリアルで作成した gRPC あいさつサービスに対して実行します。
> * プロジェクト ファイルを確認する

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>.NET コンソール アプリケーションを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[こちら](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution)の指示に従い、*GrpcGreeterClient* という名前でコンソール アプリを作成します。

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>必要なパッケージを追加する

次のパッケージを gRPC クライアント プロジェクトに追加します。

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core)。これには C-core クライアント用の C# API が含まれています。
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/)。これに C# の protobuf メッセージ API が含まれています。
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)。これには protobuf ファイルの C# ツール サポートが含まれています。 ツール パッケージは実行時に不要であり、依存関係には `PrivateAssets="All"` のマークが付きます。

パッケージは次の方法で追加できます。

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>パッケージをインストールするための PMC オプション

* Visual Studio で **[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順に選択します。
* **[パッケージ マネージャー コンソール]** ウィンドウから、*GrpcGreeterClient.csproj* ファイルがあるディレクトリに移動します。
* 次のコマンドを実行します。

    ```powershell
    Install-Package Grpc.Core
    ```

* Google.Protobuf と Grpc.Tools に対して `Install-Package` を繰り返します

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>パッケージをインストールするための [NuGet パッケージの管理] オプション

* **[ソリューション エクスプローラー]** > **[NuGet パッケージの管理]** でプロジェクトを右クリックします。
* **パッケージ ソース**を "nuget.org" に設定します。
* 検索ボックスに "Grpc.Core" と入力します
* **[参照]** タブから "Grpc.Core" パッケージを選択し、**[インストール]** をクリックします
* Google.Protobuf と Grpc.Tools に対して繰り返します

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**統合端末**からから次のコマンドを実行します。

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Google.Protobuf と Grpc.Tools に対して繰り返します

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[Solution Pad]** > **[パッケージを追加]** で [*パッケージ*] フォルダーを右クリックします。
* **[パッケージを追加]** ウィンドウの **[ソース]** ドロップダウンを "nuget.org" に設定します。
* 検索ボックスに "Grpc.Core" と入力します
* 結果ウィンドウから "Grpc.Core" パッケージを選択し、**[パッケージを追加]** をクリックします
* Google.Protobuf と Grpc.Tools に対して繰り返します

---

## <a name="add-the-greetproto-file"></a>greet.proto ファイルを追加する

gRPC あいさつサービスから gRPC クライアント プロジェクトに **Protos\greet.proto** ファイルをコピーします。 GrpcGreeterClient プロジェクト ファイルの `<Protobuf>` 項目グループに **greet.proto** ファイルを追加します。

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> プロジェクトを右クリックし、ドロップダウン メニューから **[Edit GrpcGreeterClient.csproj]\(GrpcGreeterClient.csproj の編集\)** オプションを選択すると、GrpcGreeterClient のプロジェクト ファイルを開くことができます。
>
> ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/edit_csproj.png)
>
> あるいは、GrpcGreeterClient ディレクトリに移動し、普段お使いのエディターで `GrpcGreeterClient.csproj` を編集できます。

`GrpcServices="Client"` 属性が追加されます。そのため、含まれる protobuf ファイルに対して C# クライアント アセットのみが生成されます。 クライアント プロジェクトをビルドし、C# クライアント アセットの生成をトリガーします。

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>GreeterClient を作成し、単項呼び出し SayHello を呼び出す

gRPC クライアント プロジェクトの `Main` ファイルの `Program.cs` メソッドに次のコードを追加します。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

必須の型にアクセスするには、次の using ステートメントが必要です。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

gRPC サービスへの接続を作成するための情報を含む `Channel` をインスタンス化し、それを利用して `GreeterClient` を構築することで GreeterClient が作成されます。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient には単項呼び出し `SayHello` が含まれます。これは非同期で呼び出すことができます。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

`SayHello` 呼び出しの結果は `reply` に格納されます。結果は次のように表示できます。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

クライアントによって使用される `Channel` は、操作が完了してリソースが解放されたとき、シャットダウンする必要があります。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> **Greeter** 名前空間の型を解決するには、プロジェクトをビルドする必要があります。 これらの型はビルド中に自動的に生成されます。ビルドの実行前には利用できません。

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>gRPC あいさつサービスで gRPC クライアントをテストする

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 前のチュートリアルで作成したあいさつサービスが実行中であることを確認します。

* サービスが実行中であれば、**GrpcGreeterClient** プロジェクトに戻り、それをスタートアップ プロジェクトとして設定します。 Ctrl+F5 キーを押し、デバッガーなしでクライアントを実行します。

  クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。 サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/client.png)

  サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。

  ![新しい ASP.NET Core Web アプリケーション](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

* 前のチュートリアルで作成したあいさつサービスが実行中であることを確認します。

* `dotnet run` を使用して、コマンド ラインからクライアント プロジェクト GrpcGreeter.Client を実行します。

クライアントがその名前 "GreeterClient" を含むメッセージを使用して、サービスにあいさつを送信します。 サービスはコマンド プロンプトに表示される応答として、メッセージ "Hello GreeterClient" を送信します。

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

サービスは、成功した呼び出しの詳細をコマンド プロンプトに書き込まれるログに記録します。

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
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>gRPC プロジェクトのプロジェクト ファイルを調査する

gRPC クライアントの GrpcGreeterClient ファイル:

*Program.cs* には、gRPC クライアントのエントリ ポイントとロジックが含まれています。

## <a name="additional-resources"></a>その他の技術情報

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * gRPC クライアントを作成します。
> * このサービスを前のチュートリアルで作成した gRPC あいさつサービスに対して実行します。
> * プロジェクト ファイルを確認する

> [!div class="step-by-step"]
> [前へ:gRPC あいさつサービスを作成する](xref:tutorials/grpc/grpc-start)
