---
title: "dotnet watch を使用した ASP.NET Core アプリの開発"
author: rick-anderson
description: "このチュートリアルでは、.NET Core CLI のファイル ウォッチャー (dotnet watch) ツールをインストールし、ASP.NET Core アプリケーションで使用する方法について説明します。"
keywords: "ASP.NET Core、dotnet watch の使用"
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 9baf2ce2a1270a728616a8a2ab45deca9a9cde6f
ms.sourcegitcommit: e7f01a649f240b6b57118c53314ab82f7f36f2eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/06/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>dotnet watch を使用した ASP.NET Core アプリの開発

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` は、ソース ファイルの変更時に [.NET Core CLI](/dotnet/core/tools) コマンドを実行するツールです。 たとえば、あるファイルを変更すると、コンパイル、テストの実行、展開が開始されます。

このチュートリアルでは、エンドポイントが 2 つの既存の Web API アプリを利用します。合計を返すエンドポイントと積を返すエンドポイントです。 積のメソッドにはバグが含まれますが、このチュートリアルで修正します。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。 これには、*WebApp* (ASP.NET Core Web API) および *WebAppTests* (Web API の単体テスト) という 2 つのプロジェクトが含まれています。

コマンド シェルで、*WebApp* フォルダーに移動し、次のコマンドを実行します。

```console
dotnet run
```

コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Web ブラウザーで、`http://localhost:<port number>/api/math/sum?a=4&b=5` に移動します。 結果として `9` が表示されます。

製品 API に移動します (`http://localhost:<port number>/api/math/product?a=4&b=5`)。 予想していた `20` ではなく、`9` が返されます。 これはチュートリアルの後半で修正します。

## <a name="add-dotnet-watch-to-a-project"></a>`dotnet watch` をプロジェクトに追加する

1. `Microsoft.DotNet.Watcher.Tools` パッケージ参照を *.csproj* ファイルに追加します。

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. 次のコマンドを実行して `Microsoft.DotNet.Watcher.Tools` パッケージをインストールします。
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a>`dotnet watch` を使用した .NET Core CLI コマンドの実行

[.NET Core CLI コマンド](/dotnet/core/tools#cli-commands) はいずれも、`dotnet watch` との組み合わせで実行することができます。 例:

| コマンド | コマンドと watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f netcoreapp2.0 | dotnet watch run -f netcoreapp2.0 |
| dotnet run -f netcoreapp2.0 -- --arg1 | dotnet watch run -f netcoreapp2.0 -- --arg1 |
| dotnet test | dotnet watch test |

*WebApp* フォルダーの `dotnet watch run` を実行します。 コンソール出力に、`watch` が起動したことが示されます。

## <a name="making-changes-with-dotnet-watch"></a>`dotnet watch` で変更する

`dotnet watch` が実行されていることを確認します。

*MathController.cs* の `Product` メソッドのバグを修正して、合計ではなく積を返すようにします。

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

ファイルを保存します。 コンソール出力により、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが表示されます。

`http://localhost:<port number>/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。

## <a name="running-tests-using-dotnet-watch"></a>`dotnet watch` でテストを実行する

1. *MathController.cs* の `Product` メソッドを元に戻して合計を返すようにし、ファイルを保存します。
1. コマンド シェルで、*WebAppTests* フォルダーに移動します。
1. `dotnet restore` を実行します。
1. `dotnet watch test` を実行します。 テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. 積を返すように `Product` メソッドのコードを修正します。 ファイルを保存します。

`dotnet watch` はファイル変更を検出し、テストを再実行します。 コンソール出力にテストの合格が示されます。

## <a name="dotnet-watch-in-github"></a>GitHub の dotnet-watch

dotnet-watch は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools)に含まれます。

[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) の [MSBuild セクション](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild)では、ウォッチされている MSBuild プロジェクト ファイルから dotnet-watch を構成する方法を説明しています。 [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) には、このチュートリアルで取り上げていない dotnet-watch に関する情報があります。
