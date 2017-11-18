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
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="developing-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="cb41d-104">dotnet watch を使用した ASP.NET Core アプリの開発</span><span class="sxs-lookup"><span data-stu-id="cb41d-104">Developing ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="cb41d-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="cb41d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="cb41d-106">`dotnet watch` は、ソース ファイルの変更時に [.NET Core CLI](/dotnet/core/tools) コマンドを実行するツールです。</span><span class="sxs-lookup"><span data-stu-id="cb41d-106">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="cb41d-107">たとえば、あるファイルを変更すると、コンパイル、テストの実行、展開が開始されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-107">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="cb41d-108">このチュートリアルでは、エンドポイントが 2 つの既存の Web API アプリを利用します。合計を返すエンドポイントと積を返すエンドポイントです。</span><span class="sxs-lookup"><span data-stu-id="cb41d-108">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="cb41d-109">積のメソッドにはバグが含まれますが、このチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-109">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="cb41d-110">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="cb41d-110">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="cb41d-111">これには、*WebApp* (ASP.NET Core Web API) および *WebAppTests* (Web API の単体テスト) という 2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="cb41d-111">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="cb41d-112">コマンド シェルで、*WebApp* フォルダーに移動し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-112">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="cb41d-113">コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="cb41d-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="cb41d-114">Web ブラウザーで、`http://localhost:<port number>/api/math/sum?a=4&b=5` に移動します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="cb41d-115">結果として `9` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-115">You should see the result of `9`.</span></span>

<span data-ttu-id="cb41d-116">製品 API に移動します (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="cb41d-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="cb41d-117">予想していた `20` ではなく、`9` が返されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="cb41d-118">これはチュートリアルの後半で修正します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-118">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="cb41d-119">`dotnet watch` をプロジェクトに追加する</span><span class="sxs-lookup"><span data-stu-id="cb41d-119">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="cb41d-120">`Microsoft.DotNet.Watcher.Tools` パッケージ参照を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-120">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="cb41d-121">次のコマンドを実行して `Microsoft.DotNet.Watcher.Tools` パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="cb41d-121">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="cb41d-122">`dotnet watch` を使用した .NET Core CLI コマンドの実行</span><span class="sxs-lookup"><span data-stu-id="cb41d-122">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="cb41d-123">[.NET Core CLI コマンド](/dotnet/core/tools#cli-commands) はいずれも、`dotnet watch` との組み合わせで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-123">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="cb41d-124">例:</span><span class="sxs-lookup"><span data-stu-id="cb41d-124">For example:</span></span>

| <span data-ttu-id="cb41d-125">コマンド</span><span class="sxs-lookup"><span data-stu-id="cb41d-125">Command</span></span> | <span data-ttu-id="cb41d-126">コマンドと watch</span><span class="sxs-lookup"><span data-stu-id="cb41d-126">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="cb41d-127">dotnet run</span><span class="sxs-lookup"><span data-stu-id="cb41d-127">dotnet run</span></span> | <span data-ttu-id="cb41d-128">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="cb41d-128">dotnet watch run</span></span> |
| <span data-ttu-id="cb41d-129">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="cb41d-129">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="cb41d-130">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="cb41d-130">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="cb41d-131">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="cb41d-131">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="cb41d-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="cb41d-132">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="cb41d-133">dotnet test</span><span class="sxs-lookup"><span data-stu-id="cb41d-133">dotnet test</span></span> | <span data-ttu-id="cb41d-134">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="cb41d-134">dotnet watch test</span></span> |

<span data-ttu-id="cb41d-135">*WebApp* フォルダーの `dotnet watch run` を実行します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-135">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="cb41d-136">コンソール出力に、`watch` が起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-136">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="cb41d-137">`dotnet watch` で変更する</span><span class="sxs-lookup"><span data-stu-id="cb41d-137">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="cb41d-138">`dotnet watch` が実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-138">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="cb41d-139">*MathController.cs* の `Product` メソッドのバグを修正して、合計ではなく積を返すようにします。</span><span class="sxs-lookup"><span data-stu-id="cb41d-139">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="cb41d-140">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-140">Save the file.</span></span> <span data-ttu-id="cb41d-141">コンソール出力により、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-141">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="cb41d-142">`http://localhost:<port number>/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-142">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="cb41d-143">`dotnet watch` でテストを実行する</span><span class="sxs-lookup"><span data-stu-id="cb41d-143">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="cb41d-144">*MathController.cs* の `Product` メソッドを元に戻して合計を返すようにし、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-144">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="cb41d-145">コマンド シェルで、*WebAppTests* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-145">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="cb41d-146">`dotnet restore` を実行します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-146">Run `dotnet restore`.</span></span>
1. <span data-ttu-id="cb41d-147">`dotnet watch test` を実行します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-147">Run `dotnet watch test`.</span></span> <span data-ttu-id="cb41d-148">テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-148">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="cb41d-149">積を返すように `Product` メソッドのコードを修正します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-149">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="cb41d-150">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-150">Save the file.</span></span>

<span data-ttu-id="cb41d-151">`dotnet watch` はファイル変更を検出し、テストを再実行します。</span><span class="sxs-lookup"><span data-stu-id="cb41d-151">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="cb41d-152">コンソール出力にテストの合格が示されます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-152">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="cb41d-153">GitHub の dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="cb41d-153">dotnet-watch in GitHub</span></span>

<span data-ttu-id="cb41d-154">dotnet-watch は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="cb41d-154">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/Microsoft.DotNet.Watcher.Tools).</span></span>

<span data-ttu-id="cb41d-155">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) の [MSBuild セクション](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild)では、ウォッチされている MSBuild プロジェクト ファイルから dotnet-watch を構成する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="cb41d-155">The [MSBuild section](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="cb41d-156">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) には、このチュートリアルで取り上げていない dotnet-watch に関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="cb41d-156">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/Microsoft.DotNet.Watcher.Tools/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
