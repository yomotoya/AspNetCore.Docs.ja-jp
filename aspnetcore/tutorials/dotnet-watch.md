---
title: "dotnet watch を使用した ASP.NET Core アプリの開発"
author: rick-anderson
description: "dotnet watch の使い方を示します。"
keywords: "ASP.NET Core、dotnet watch の使用"
ms.author: riande
manager: wpickett
ms.date: 03/09/2017
ms.topic: article
ms.assetid: 563ffb3f-d369-4aa5-bf0a-7300b4e7832c
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/dotnet-watch
ms.openlocfilehash: 30e0d07bdfbd16a475e03c1a21cdd10220bd1630
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a>dotnet watch を使用した ASP.NET Core アプリの開発


作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)

`dotnet watch` は、ソース ファイルの変更時に `dotnet` コマンドを実行するツールです。 たとえば、あるファイルを変更すると、コンパイル、テスト、展開が開始します。

このチュートリアルでは、エンドポイントが 2 つの既存の Web API アプリを利用します。合計を返すエンドポイントと積を返すエンドポイントです。 積のメソッドにはバグが含まれますが、このチュートリアルで修正します。

[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。 これには 2 つのプロジェクトが含まれています。`WebApp` (Web アプリ) と `WebAppTests` (Web アプリの単体テスト) です。

コンソールで、WebApp フォルダーに移動し、次のコマンドを実行します。

- `dotnet restore`
- `dotnet run`

コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Web ブラウザーで `http://localhost:5000/api/math/sum?a=4&b=5` に移動すると、結果 `9` が表示されるはずです。

積の API (`http://localhost:5000/api/math/product?a=4&b=5`) に移動します。予想される `20` ではなく、`9` が返されます。 これはチュートリアルの後半で修正します。

## <a name="add-dotnet-watch-to-a-project"></a>`dotnet watch` をプロジェクトに追加する

- `Microsoft.DotNet.Watcher.Tools` を *.csproj* ファイルに追加します。
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="1.0.0" />
 </ItemGroup> 
 ```

- `dotnet restore` を実行します。

## <a name="running-dotnet-commands-using-dotnet-watch"></a>`dotnet watch` を利用して `dotnet` コマンドを実行する

次のように、あらゆる `dotnet` コマンドを `dotnet watch` で実行できます。

| コマンド | コマンドと watch |
| ---- | ----- |
| dotnet run | dotnet watch run |
| dotnet run -f net451 | dotnet watch run -f net451 |
| dotnet run -f net451 -- --arg1 | dotnet watch run -f net451 -- --arg1 |
| dotnet test | dotnet watch test |

`WebApp` フォルダーで `dotnet watch run` を実行します。 コンソール出力に、`watch` が起動したことが示されます。

## <a name="making-changes-with-dotnet-watch"></a>`dotnet watch` で変更する

`dotnet watch` が実行されていることを確認します。

合計ではなく積を返すように、`MathController` の `Product` メソッドのバグを修正します。

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

ファイルを保存します。 コンソール出力にメッセージが表示され、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが伝えられます。

`http://localhost:5000/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。

## <a name="running-tests-using-dotnet-watch"></a>`dotnet watch` でテストを実行する

- `MathController` の `Product` メソッドを元に戻して合計を返すようにし、ファイルを保存します。
- コマンド ウィンドウで、`WebAppTests` フォルダーに移動します。
- `dotnet restore` を実行します。
- `dotnet watch test` を実行します。 テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- 積を返すように `Product` メソッドのコードを修正します。 ファイルを保存します。

`dotnet watch` はファイル変更を検出し、テストを再実行します。 コンソール出力にテストの合格が示されます。

## <a name="dotnet-watch-in-github"></a>GitHub の dotnet-watch

dotnet-watch は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools)に含まれます。

[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) の [MSBuild セクション](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild)では、ウォッチされている MSBuild プロジェクト ファイルから dotnet-watch を構成する方法を説明しています。 [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) には、このチュートリアルで取り上げていない dotnet-watch に関する情報があります。
