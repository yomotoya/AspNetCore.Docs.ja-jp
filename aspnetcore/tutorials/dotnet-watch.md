---
title: dotnet watch を使用した ASP.NET Core アプリの開発
author: rick-anderson
description: このチュートリアルでは、.NET Core CLI のファイル ウォッチャー (dotnet watch) ツールをインストールし、ASP.NET Core アプリケーションで使用する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 10/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/dotnet-watch
ms.openlocfilehash: c3ece3a5b936b2ea7b7772eee10e598cb557b361
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="develop-aspnet-core-apps-using-dotnet-watch"></a><span data-ttu-id="09b70-103">dotnet watch を使用した ASP.NET Core アプリの開発</span><span class="sxs-lookup"><span data-stu-id="09b70-103">Develop ASP.NET Core apps using dotnet watch</span></span>

<span data-ttu-id="09b70-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="09b70-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="09b70-105">`dotnet watch` は、ソース ファイルの変更時に [.NET Core CLI](/dotnet/core/tools) コマンドを実行するツールです。</span><span class="sxs-lookup"><span data-stu-id="09b70-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="09b70-106">たとえば、あるファイルを変更すると、コンパイル、テストの実行、展開が開始されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="09b70-107">このチュートリアルでは、エンドポイントが 2 つの既存の Web API アプリを利用します。合計を返すエンドポイントと積を返すエンドポイントです。</span><span class="sxs-lookup"><span data-stu-id="09b70-107">In this tutorial, we use an existing Web API app with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="09b70-108">積のメソッドにはバグが含まれますが、このチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="09b70-108">The product method contains a bug that we'll fix as part of this tutorial.</span></span>

<span data-ttu-id="09b70-109">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="09b70-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="09b70-110">これには、*WebApp* (ASP.NET Core Web API) および *WebAppTests* (Web API の単体テスト) という 2 つのプロジェクトが含まれています。</span><span class="sxs-lookup"><span data-stu-id="09b70-110">It contains two projects: *WebApp* (an ASP.NET Core Web API) and *WebAppTests* (unit tests for the Web API).</span></span>

<span data-ttu-id="09b70-111">コマンド シェルで、*WebApp* フォルダーに移動し、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="09b70-111">In a command shell, navigate to the *WebApp* folder and run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="09b70-112">コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="09b70-112">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="09b70-113">Web ブラウザーで、`http://localhost:<port number>/api/math/sum?a=4&b=5` に移動します。</span><span class="sxs-lookup"><span data-stu-id="09b70-113">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="09b70-114">結果として `9` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-114">You should see the result of `9`.</span></span>

<span data-ttu-id="09b70-115">製品 API に移動します (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="09b70-115">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="09b70-116">予想していた `20` ではなく、`9` が返されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-116">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="09b70-117">これはチュートリアルの後半で修正します。</span><span class="sxs-lookup"><span data-stu-id="09b70-117">We'll fix that later in the tutorial.</span></span>

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="09b70-118">`dotnet watch` をプロジェクトに追加する</span><span class="sxs-lookup"><span data-stu-id="09b70-118">Add `dotnet watch` to a project</span></span>

1. <span data-ttu-id="09b70-119">`Microsoft.DotNet.Watcher.Tools` パッケージ参照を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="09b70-119">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup> 
    ```

1. <span data-ttu-id="09b70-120">次のコマンドを実行して `Microsoft.DotNet.Watcher.Tools` パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="09b70-120">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>
    
    ```console
    dotnet restore
    ```

## <a name="running-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="09b70-121">`dotnet watch` を使用した .NET Core CLI コマンドの実行</span><span class="sxs-lookup"><span data-stu-id="09b70-121">Running .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="09b70-122">[.NET Core CLI コマンド](/dotnet/core/tools#cli-commands) はいずれも、`dotnet watch` との組み合わせで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="09b70-122">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="09b70-123">例:</span><span class="sxs-lookup"><span data-stu-id="09b70-123">For example:</span></span>

| <span data-ttu-id="09b70-124">コマンド</span><span class="sxs-lookup"><span data-stu-id="09b70-124">Command</span></span> | <span data-ttu-id="09b70-125">コマンドと watch</span><span class="sxs-lookup"><span data-stu-id="09b70-125">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="09b70-126">dotnet run</span><span class="sxs-lookup"><span data-stu-id="09b70-126">dotnet run</span></span> | <span data-ttu-id="09b70-127">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="09b70-127">dotnet watch run</span></span> |
| <span data-ttu-id="09b70-128">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="09b70-128">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="09b70-129">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="09b70-129">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="09b70-130">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="09b70-130">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="09b70-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="09b70-131">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="09b70-132">dotnet test</span><span class="sxs-lookup"><span data-stu-id="09b70-132">dotnet test</span></span> | <span data-ttu-id="09b70-133">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="09b70-133">dotnet watch test</span></span> |

<span data-ttu-id="09b70-134">*WebApp* フォルダーの `dotnet watch run` を実行します。</span><span class="sxs-lookup"><span data-stu-id="09b70-134">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="09b70-135">コンソール出力に、`watch` が起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-135">The console output indicates `watch` has started.</span></span>

## <a name="making-changes-with-dotnet-watch"></a><span data-ttu-id="09b70-136">`dotnet watch` で変更する</span><span class="sxs-lookup"><span data-stu-id="09b70-136">Making changes with `dotnet watch`</span></span>

<span data-ttu-id="09b70-137">`dotnet watch` が実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="09b70-137">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="09b70-138">*MathController.cs* の `Product` メソッドのバグを修正して、合計ではなく積を返すようにします。</span><span class="sxs-lookup"><span data-stu-id="09b70-138">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
} 
```

<span data-ttu-id="09b70-139">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="09b70-139">Save the file.</span></span> <span data-ttu-id="09b70-140">コンソール出力により、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-140">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="09b70-141">`http://localhost:<port number>/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="09b70-141">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="running-tests-using-dotnet-watch"></a><span data-ttu-id="09b70-142">`dotnet watch` でテストを実行する</span><span class="sxs-lookup"><span data-stu-id="09b70-142">Running tests using `dotnet watch`</span></span>

1. <span data-ttu-id="09b70-143">*MathController.cs* の `Product` メソッドを元に戻して合計を返すようにし、ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="09b70-143">Change the `Product` method of *MathController.cs* back to returning the sum and save the file.</span></span>
1. <span data-ttu-id="09b70-144">コマンド シェルで、*WebAppTests* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="09b70-144">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="09b70-145">[dotnet restore](/dotnet/core/tools/dotnet-restore) を実行します。</span><span class="sxs-lookup"><span data-stu-id="09b70-145">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="09b70-146">`dotnet watch test` を実行します。</span><span class="sxs-lookup"><span data-stu-id="09b70-146">Run `dotnet watch test`.</span></span> <span data-ttu-id="09b70-147">テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-147">Its output indicates that a test failed and that watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="09b70-148">積を返すように `Product` メソッドのコードを修正します。</span><span class="sxs-lookup"><span data-stu-id="09b70-148">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="09b70-149">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="09b70-149">Save the file.</span></span>

<span data-ttu-id="09b70-150">`dotnet watch` はファイル変更を検出し、テストを再実行します。</span><span class="sxs-lookup"><span data-stu-id="09b70-150">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="09b70-151">コンソール出力にテストの合格が示されます。</span><span class="sxs-lookup"><span data-stu-id="09b70-151">The console output indicates the tests passed.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="09b70-152">GitHub の dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="09b70-152">dotnet-watch in GitHub</span></span>

<span data-ttu-id="09b70-153">dotnet-watch は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="09b70-153">dotnet-watch is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>

<span data-ttu-id="09b70-154">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) の [MSBuild セクション](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild)では、ウォッチされている MSBuild プロジェクト ファイルから dotnet-watch を構成する方法を説明しています。</span><span class="sxs-lookup"><span data-stu-id="09b70-154">The [MSBuild section](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch#msbuild) of the [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) explains how dotnet-watch can be configured from the MSBuild project file being watched.</span></span> <span data-ttu-id="09b70-155">[dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) には、このチュートリアルで取り上げていない dotnet-watch に関する情報があります。</span><span class="sxs-lookup"><span data-stu-id="09b70-155">The [dotnet-watch ReadMe](https://github.com/aspnet/DotNetTools/blob/dev/src/dotnet-watch/README.md) contains information on dotnet-watch not covered in this tutorial.</span></span>
