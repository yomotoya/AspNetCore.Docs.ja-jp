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
ms.openlocfilehash: 6a8943619e6174dbcd3d901b7bb736ba5d3af95d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="985b4-104">dotnet watch を使用した ASP.NET Core アプリの開発</span><span class="sxs-lookup"><span data-stu-id="985b4-104">Developing ASP.NET Core apps using dotnet watch</span></span>


<span data-ttu-id="985b4-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="985b4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="985b4-106">`dotnet watch` は、ソース ファイルの変更時に `dotnet` コマンドを実行するツールです。</span><span class="sxs-lookup"><span data-stu-id="985b4-106">`dotnet watch` is a tool that runs a `dotnet` command when source files change.</span></span> <span data-ttu-id="985b4-107">たとえば、あるファイルを変更すると、コンパイル、テスト、展開が開始します。</span><span class="sxs-lookup"><span data-stu-id="985b4-107">For example, a file change can trigger compilation, tests, or deployment.</span></span>

<span data-ttu-id="985b4-108">このチュートリアルでは、エンドポイントが 2 つの既存の Web API アプリを利用します。合計を返すエンドポイントと積を返すエンドポイントです。</span><span class="sxs-lookup"><span data-stu-id="985b4-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="985b4-109">積のメソッドにはバグが含まれますが、このチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="985b4-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="985b4-110">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="985b4-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="985b4-111">これには 2 つのプロジェクトが含まれています。`WebApp` (Web アプリ) と `WebAppTests` (Web アプリの単体テスト) です。</span><span class="sxs-lookup"><span data-stu-id="985b4-111">It contains two projects, `WebApp` (a web app) and `WebAppTests` (unit tests for the web app).</span></span>

<span data-ttu-id="985b4-112">コンソールで、WebApp フォルダーに移動し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-112">In a console, navigate to the WebApp folder and run the following commands:</span></span>

- `dotnet restore`
- `dotnet run`

<span data-ttu-id="985b4-113">コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="985b4-113">The console output will show messages similar to the following (indicating that the app is running and waiting for requests):</span></span>

```console
$ dotnet run
Hosting environment: Production
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="985b4-114">Web ブラウザーで `http://localhost:5000/api/math/sum?a=4&b=5` に移動すると、結果 `9` が表示されるはずです。</span><span class="sxs-lookup"><span data-stu-id="985b4-114">In a web browser, navigate to `http://localhost:5000/api/math/sum?a=4&b=5`, you should see the result `9`.</span></span>

<span data-ttu-id="985b4-115">積の API (`http://localhost:5000/api/math/product?a=4&b=5`) に移動します。予想される `20` ではなく、`9` が返されます。</span><span class="sxs-lookup"><span data-stu-id="985b4-115">Navigate to the product API (`http://localhost:5000/api/math/product?a=4&b=5`), it returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="985b4-116">これはチュートリアルの後半で修正します。</span><span class="sxs-lookup"><span data-stu-id="985b4-116">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="985b4-117">`dotnet watch` をプロジェクトに追加する</span><span class="sxs-lookup"><span data-stu-id="985b4-117">Add `dotnet watch` to a project</span></span>

- <span data-ttu-id="985b4-118">`Microsoft.DotNet.Watcher.Tools` を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="985b4-118">Add `Microsoft.DotNet.Watcher.Tools` to the *.csproj* file:</span></span>
 ```xml
 <ItemGroup>
   <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
 </ItemGroup> 
 ```

- <span data-ttu-id="985b4-119">`dotnet restore` を実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-119">Run `dotnet restore`.</span></span>

## <a name="running-dotnet-commands-using-dotnet-watch"></a><span data-ttu-id="985b4-120">`dotnet watch` を利用して `dotnet` コマンドを実行する</span><span class="sxs-lookup"><span data-stu-id="985b4-120">Running `dotnet` commands using `dotnet watch`</span></span>

<span data-ttu-id="985b4-121">次のように、あらゆる `dotnet` コマンドを `dotnet watch` で実行できます。</span><span class="sxs-lookup"><span data-stu-id="985b4-121">Any `dotnet` command can be run with `dotnet watch`, for example:</span></span>

| <span data-ttu-id="985b4-122">コマンド</span><span class="sxs-lookup"><span data-stu-id="985b4-122">Command</span></span> | <span data-ttu-id="985b4-123">コマンドと watch</span><span class="sxs-lookup"><span data-stu-id="985b4-123">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="985b4-124">dotnet run</span><span class="sxs-lookup"><span data-stu-id="985b4-124">dotnet run</span></span> | <span data-ttu-id="985b4-125">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="985b4-125">dotnet watch run</span></span> |
| <span data-ttu-id="985b4-126">dotnet run -f net451</span><span class="sxs-lookup"><span data-stu-id="985b4-126">dotnet run -f net451</span></span> | <span data-ttu-id="985b4-127">dotnet watch run -f net451</span><span class="sxs-lookup"><span data-stu-id="985b4-127">dotnet watch run -f net451</span></span> |
| <span data-ttu-id="985b4-128">dotnet run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="985b4-128">dotnet run -f net451 -- --arg1</span></span> | <span data-ttu-id="985b4-129">dotnet watch run -f net451 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="985b4-129">dotnet watch run -f net451 -- --arg1</span></span> |
| <span data-ttu-id="985b4-130">dotnet test</span><span class="sxs-lookup"><span data-stu-id="985b4-130">dotnet test</span></span> | <span data-ttu-id="985b4-131">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="985b4-131">dotnet watch test</span></span> |

<span data-ttu-id="985b4-132">`WebApp` フォルダーで `dotnet watch run` を実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-132">Run `dotnet watch run` in the `WebApp` folder.</span></span> <span data-ttu-id="985b4-133">コンソール出力に、`watch` が起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="985b4-133">The console output will indicate `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="985b4-134">`dotnet watch` で変更する</span><span class="sxs-lookup"><span data-stu-id="985b4-134">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="985b4-135">`dotnet watch` が実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="985b4-135">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="985b4-136">合計ではなく積を返すように、`MathController` の `Product` メソッドのバグを修正します。</span><span class="sxs-lookup"><span data-stu-id="985b4-136">Fix the bug in the `Product` method of the `MathController` so it returns the product and not the sum.</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="985b4-137">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="985b4-137">Save the file.</span></span> <span data-ttu-id="985b4-138">コンソール出力にメッセージが表示され、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが伝えられます。</span><span class="sxs-lookup"><span data-stu-id="985b4-138">The console output will show messages indicating that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="985b4-139">`http://localhost:5000/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="985b4-139">Verify `http://localhost:5000/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="985b4-140">`dotnet watch` でテストを実行する</span><span class="sxs-lookup"><span data-stu-id="985b4-140">Running tests using `dotnet watch`</span></span>

- <span data-ttu-id="985b4-141">`MathController` の `Product` メソッドを元に戻して合計を返すようにし、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="985b4-141">Change the `Product` method of the `MathController` back to returning the sum and save the file.</span></span>
- <span data-ttu-id="985b4-142">コマンド ウィンドウで、`WebAppTests` フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="985b4-142">In a command window, naviagate to the `WebAppTests` folder.</span></span>
- <span data-ttu-id="985b4-143">`dotnet restore` を実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-143">Run `dotnet restore`</span></span>
- <span data-ttu-id="985b4-144">`dotnet watch test` を実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-144">Run `dotnet watch test`.</span></span> <span data-ttu-id="985b4-145">テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。</span><span class="sxs-lookup"><span data-stu-id="985b4-145">You see output indicating that a test failed and that watcher is waiting for file changes:</span></span>

 ```console
 Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
 Test Run Failed.
  ```
- <span data-ttu-id="985b4-146">積を返すように `Product` メソッドのコードを修正します。</span><span class="sxs-lookup"><span data-stu-id="985b4-146">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="985b4-147">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="985b4-147">Save the file.</span></span>

<span data-ttu-id="985b4-148">`dotnet watch` はファイル変更を検出し、テストを再実行します。</span><span class="sxs-lookup"><span data-stu-id="985b4-148">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="985b4-149">コンソール出力にテストの合格が示されます。</span><span class="sxs-lookup"><span data-stu-id="985b4-149">The console output will show the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="985b4-150">GitHub の dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="985b4-150">dotnet-watch in GitHub</span></span>

<span data-ttu-id="985b4-151">dotnet-watch は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="985b4-151">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="985b4-152">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) の [MSBuild セクション](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild)では、ウォッチされている MSBuild プロジェクト ファイルから dotnet-watch を構成する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="985b4-152">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="985b4-153">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) には、このチュートリアルで取り上げていない dotnet-watch に関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="985b4-153">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
