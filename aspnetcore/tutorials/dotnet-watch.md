---
title: ファイル ウォッチャーを使用した ASP.NET Core アプリの開発
author: rick-anderson
description: このチュートリアルでは、.NET Core CLI のファイル ウォッチャー (dotnet watch) ツールをインストールし、ASP.NET Core アプリで使用する方法について説明します。
ms.author: riande
ms.date: 05/31/2018
uid: tutorials/dotnet-watch
ms.openlocfilehash: 2a59267b36faf1e00ea2f0cc7e2b9ceb9828f791
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278854"
---
# <a name="develop-aspnet-core-apps-using-a-file-watcher"></a><span data-ttu-id="4f85b-103">ファイル ウォッチャーを使用した ASP.NET Core アプリの開発</span><span class="sxs-lookup"><span data-stu-id="4f85b-103">Develop ASP.NET Core apps using a file watcher</span></span>

<span data-ttu-id="4f85b-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT) と [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span><span class="sxs-lookup"><span data-stu-id="4f85b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Victor Hurdugaci](https://twitter.com/victorhurdugaci)</span></span>

<span data-ttu-id="4f85b-105">`dotnet watch` は、ソース ファイルの変更時に [.NET Core CLI](/dotnet/core/tools) コマンドを実行するツールです。</span><span class="sxs-lookup"><span data-stu-id="4f85b-105">`dotnet watch` is a tool that runs a [.NET Core CLI](/dotnet/core/tools) command when source files change.</span></span> <span data-ttu-id="4f85b-106">たとえば、あるファイルを変更すると、コンパイル、テストの実行、展開が開始されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-106">For example, a file change can trigger compilation, test execution, or deployment.</span></span>

<span data-ttu-id="4f85b-107">このチュートリアルでは、エンドポイントが 2 つの既存の Web API を利用します。合計を返すエンドポイントと積を返すエンドポイントです。</span><span class="sxs-lookup"><span data-stu-id="4f85b-107">This tutorial uses an existing web API with two endpoints: one that returns a sum and one that returns a product.</span></span> <span data-ttu-id="4f85b-108">積のメソッドにはバグがあり、このチュートリアルで修正します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-108">The product method has a bug, which is fixed in this tutorial.</span></span>

<span data-ttu-id="4f85b-109">[サンプル アプリ](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample)をダウンロードしてください。</span><span class="sxs-lookup"><span data-stu-id="4f85b-109">Download the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/dotnet-watch/sample).</span></span> <span data-ttu-id="4f85b-110">これは、*WebApp* (ASP.NET Core Web API) と *WebAppTests* (Web API の単体テスト) という 2 つのプロジェクトで構成されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-110">It consists of two projects: *WebApp* (an ASP.NET Core web API) and *WebAppTests* (unit tests for the web API).</span></span>

<span data-ttu-id="4f85b-111">コマンド シェルで、*WebApp* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-111">In a command shell, navigate to the *WebApp* folder.</span></span> <span data-ttu-id="4f85b-112">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-112">Run the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="4f85b-113">コンソール出力に、次のようなメッセージが表示されます。アプリが実行中であり、要求を待っていることを示しています。</span><span class="sxs-lookup"><span data-stu-id="4f85b-113">The console output shows messages similar to the following (indicating that the app is running and awaiting requests):</span></span>

```console
$ dotnet run
Hosting environment: Development
Content root path: C:/Docs/aspnetcore/tutorials/dotnet-watch/sample/WebApp
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

<span data-ttu-id="4f85b-114">Web ブラウザーで、`http://localhost:<port number>/api/math/sum?a=4&b=5` に移動します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-114">In a web browser, navigate to `http://localhost:<port number>/api/math/sum?a=4&b=5`.</span></span> <span data-ttu-id="4f85b-115">結果として `9` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-115">You should see the result of `9`.</span></span>

<span data-ttu-id="4f85b-116">製品 API に移動します (`http://localhost:<port number>/api/math/product?a=4&b=5`)。</span><span class="sxs-lookup"><span data-stu-id="4f85b-116">Navigate to the product API (`http://localhost:<port number>/api/math/product?a=4&b=5`).</span></span> <span data-ttu-id="4f85b-117">予想していた `20` ではなく、`9` が返されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-117">It returns `9`, not `20` as you'd expect.</span></span> <span data-ttu-id="4f85b-118">この問題は、チュートリアルで後ほど修正します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-118">That problem is fixed later in the tutorial.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="add-dotnet-watch-to-a-project"></a><span data-ttu-id="4f85b-119">`dotnet watch` をプロジェクトに追加する</span><span class="sxs-lookup"><span data-stu-id="4f85b-119">Add `dotnet watch` to a project</span></span>

<span data-ttu-id="4f85b-120">`dotnet watch` ファイル ウォッチャー ツールは、.NET Core SDK のバージョン 2.1.300 に付属しています。</span><span class="sxs-lookup"><span data-stu-id="4f85b-120">The `dotnet watch` file watcher tool is included with version 2.1.300 of the .NET Core SDK.</span></span> <span data-ttu-id="4f85b-121">これより前のバージョンの .NET Core SDK を使用する場合は、次の手順が必要です。</span><span class="sxs-lookup"><span data-stu-id="4f85b-121">The following steps are required when using an earlier version of the .NET Core SDK.</span></span>

1. <span data-ttu-id="4f85b-122">`Microsoft.DotNet.Watcher.Tools` パッケージ参照を *.csproj* ファイルに追加します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-122">Add a `Microsoft.DotNet.Watcher.Tools` package reference to the *.csproj* file:</span></span>

    ```xml
    <ItemGroup>
        <DotNetCliToolReference Include="Microsoft.DotNet.Watcher.Tools" Version="2.0.0" />
    </ItemGroup>
    ```

1. <span data-ttu-id="4f85b-123">次のコマンドを実行して `Microsoft.DotNet.Watcher.Tools` パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="4f85b-123">Install the `Microsoft.DotNet.Watcher.Tools` package by running the following command:</span></span>

    ```console
    dotnet restore
    ```

::: moniker-end

## <a name="run-net-core-cli-commands-using-dotnet-watch"></a><span data-ttu-id="4f85b-124">`dotnet watch` を使用した .NET Core CLI コマンドの実行</span><span class="sxs-lookup"><span data-stu-id="4f85b-124">Run .NET Core CLI commands using `dotnet watch`</span></span>

<span data-ttu-id="4f85b-125">[.NET Core CLI コマンド](/dotnet/core/tools#cli-commands) はいずれも、`dotnet watch` との組み合わせで実行することができます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-125">Any [.NET Core CLI command](/dotnet/core/tools#cli-commands) can be run with `dotnet watch`.</span></span> <span data-ttu-id="4f85b-126">例:</span><span class="sxs-lookup"><span data-stu-id="4f85b-126">For example:</span></span>

| <span data-ttu-id="4f85b-127">コマンド</span><span class="sxs-lookup"><span data-stu-id="4f85b-127">Command</span></span> | <span data-ttu-id="4f85b-128">コマンドと watch</span><span class="sxs-lookup"><span data-stu-id="4f85b-128">Command with watch</span></span> |
| ---- | ----- |
| <span data-ttu-id="4f85b-129">dotnet run</span><span class="sxs-lookup"><span data-stu-id="4f85b-129">dotnet run</span></span> | <span data-ttu-id="4f85b-130">dotnet watch run</span><span class="sxs-lookup"><span data-stu-id="4f85b-130">dotnet watch run</span></span> |
| <span data-ttu-id="4f85b-131">dotnet run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="4f85b-131">dotnet run -f netcoreapp2.0</span></span> | <span data-ttu-id="4f85b-132">dotnet watch run -f netcoreapp2.0</span><span class="sxs-lookup"><span data-stu-id="4f85b-132">dotnet watch run -f netcoreapp2.0</span></span> |
| <span data-ttu-id="4f85b-133">dotnet run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="4f85b-133">dotnet run -f netcoreapp2.0 -- --arg1</span></span> | <span data-ttu-id="4f85b-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span><span class="sxs-lookup"><span data-stu-id="4f85b-134">dotnet watch run -f netcoreapp2.0 -- --arg1</span></span> |
| <span data-ttu-id="4f85b-135">dotnet test</span><span class="sxs-lookup"><span data-stu-id="4f85b-135">dotnet test</span></span> | <span data-ttu-id="4f85b-136">dotnet watch test</span><span class="sxs-lookup"><span data-stu-id="4f85b-136">dotnet watch test</span></span> |

<span data-ttu-id="4f85b-137">*WebApp* フォルダーの `dotnet watch run` を実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-137">Run `dotnet watch run` in the *WebApp* folder.</span></span> <span data-ttu-id="4f85b-138">コンソール出力に、`watch` が起動したことが示されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-138">The console output indicates `watch` has started.</span></span>

## <a name="make-changes-with-dotnet-watch"></a><span data-ttu-id="4f85b-139">`dotnet watch` で変更を行う</span><span class="sxs-lookup"><span data-stu-id="4f85b-139">Make changes with `dotnet watch`</span></span>

<span data-ttu-id="4f85b-140">`dotnet watch` が実行されていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-140">Make sure `dotnet watch` is running.</span></span>

<span data-ttu-id="4f85b-141">*MathController.cs* の `Product` メソッドのバグを修正して、合計ではなく積を返すようにします。</span><span class="sxs-lookup"><span data-stu-id="4f85b-141">Fix the bug in the `Product` method of *MathController.cs* so it returns the product and not the sum:</span></span>

```csharp
public static int Product(int a, int b)
{
  return a * b;
}
```

<span data-ttu-id="4f85b-142">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-142">Save the file.</span></span> <span data-ttu-id="4f85b-143">コンソール出力により、`dotnet watch` がファイル変更を検出し、アプリを再起動したことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-143">The console output indicates that `dotnet watch` detected a file change and restarted the app.</span></span>

<span data-ttu-id="4f85b-144">`http://localhost:<port number>/api/math/product?a=4&b=5` が正しい結果を返すことを確認します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-144">Verify `http://localhost:<port number>/api/math/product?a=4&b=5` returns the correct result.</span></span>

## <a name="run-tests-using-dotnet-watch"></a><span data-ttu-id="4f85b-145">`dotnet watch` を使用してテストを実行する</span><span class="sxs-lookup"><span data-stu-id="4f85b-145">Run tests using `dotnet watch`</span></span>

1. <span data-ttu-id="4f85b-146">*MathController.cs* の `Product` メソッドを元に戻して合計を返すようにします。</span><span class="sxs-lookup"><span data-stu-id="4f85b-146">Change the `Product` method of *MathController.cs* back to returning the sum.</span></span> <span data-ttu-id="4f85b-147">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-147">Save the file.</span></span>
1. <span data-ttu-id="4f85b-148">コマンド シェルで、*WebAppTests* フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-148">In a command shell, navigate to the *WebAppTests* folder.</span></span>
1. <span data-ttu-id="4f85b-149">[dotnet restore](/dotnet/core/tools/dotnet-restore) を実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-149">Run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>
1. <span data-ttu-id="4f85b-150">`dotnet watch test` を実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-150">Run `dotnet watch test`.</span></span> <span data-ttu-id="4f85b-151">テストに失敗し、ウォッチャーがファイル変更を待っていることが出力に示されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-151">Its output indicates that a test failed and that the watcher is awaiting file changes:</span></span>

     ```console
     Total tests: 2. Passed: 1. Failed: 1. Skipped: 0.
     Test Run Failed.
     ```

1. <span data-ttu-id="4f85b-152">積を返すように `Product` メソッドのコードを修正します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-152">Fix the `Product` method code so it returns the product.</span></span> <span data-ttu-id="4f85b-153">ファイルを保存します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-153">Save the file.</span></span>

<span data-ttu-id="4f85b-154">`dotnet watch` はファイル変更を検出し、テストを再実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-154">`dotnet watch` detects the file change and reruns the tests.</span></span> <span data-ttu-id="4f85b-155">コンソール出力にテストの合格が示されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-155">The console output indicates the tests passed.</span></span>

## <a name="customize-files-list-to-watch"></a><span data-ttu-id="4f85b-156">監視するファイル リストのカスタマイズ</span><span class="sxs-lookup"><span data-stu-id="4f85b-156">Customize files list to watch</span></span>

<span data-ttu-id="4f85b-157">既定では、`dotnet-watch` は次の glob パターンに一致するすべてのファイルを追跡します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-157">By default, `dotnet-watch` tracks all files matching the following glob patterns:</span></span>

* `**/*.cs`
* `*.csproj`
* `**/*.resx`

<span data-ttu-id="4f85b-158">ウォッチ リストに他の項目を追加するには、*.csproj* ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-158">More items can be added to the watch list by editing the *.csproj* file.</span></span> <span data-ttu-id="4f85b-159">項目は個別に指定することも、glob パターンを使用して指定することもできます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-159">Items can be specified individually or by using glob patterns.</span></span>

```xml
<ItemGroup>
    <!-- extends watching group to include *.js files -->
    <Watch Include="**\*.js" Exclude="node_modules\**\*;**\*.js.map;obj\**\*;bin\**\*" />
</ItemGroup>
```

## <a name="opt-out-of-files-to-be-watched"></a><span data-ttu-id="4f85b-160">ウォッチするファイルのオプトアウト</span><span class="sxs-lookup"><span data-stu-id="4f85b-160">Opt-out of files to be watched</span></span>

<span data-ttu-id="4f85b-161">既定の設定を無視するように `dotnet-watch` を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-161">`dotnet-watch` can be configured to ignore its default settings.</span></span> <span data-ttu-id="4f85b-162">特定のファイルを無視するには、*.csproj* ファイルで項目の定義に `Watch="false"` 属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-162">To ignore specific files, add the `Watch="false"` attribute to an item's definition in the *.csproj* file:</span></span>

```xml
<ItemGroup>
    <!-- exclude Generated.cs from dotnet-watch -->
    <Compile Include="Generated.cs" Watch="false" />

    <!-- exclude Strings.resx from dotnet-watch -->
    <EmbeddedResource Include="Strings.resx" Watch="false" />

    <!-- exclude changes in this referenced project -->
    <ProjectReference Include="..\ClassLibrary1\ClassLibrary1.csproj" Watch="false" />
</ItemGroup>
```

## <a name="custom-watch-projects"></a><span data-ttu-id="4f85b-163">カスタム ウォッチ プロジェクト</span><span class="sxs-lookup"><span data-stu-id="4f85b-163">Custom watch projects</span></span>

<span data-ttu-id="4f85b-164">`dotnet-watch` は C# プロジェクトだけに限定されていません。</span><span class="sxs-lookup"><span data-stu-id="4f85b-164">`dotnet-watch` isn't restricted to C# projects.</span></span> <span data-ttu-id="4f85b-165">カスタム ウォッチ プロジェクトは、さまざまなシナリオを処理するために作成できます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-165">Custom watch projects can be created to handle different scenarios.</span></span> <span data-ttu-id="4f85b-166">次のプロジェクト レイアウトを考えてみましょう。</span><span class="sxs-lookup"><span data-stu-id="4f85b-166">Consider the following project layout:</span></span>

* <span data-ttu-id="4f85b-167">**test/**</span><span class="sxs-lookup"><span data-stu-id="4f85b-167">**test/**</span></span>
  * <span data-ttu-id="4f85b-168">*UnitTests/UnitTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="4f85b-168">*UnitTests/UnitTests.csproj*</span></span>
  * <span data-ttu-id="4f85b-169">*IntegrationTests/IntegrationTests.csproj*</span><span class="sxs-lookup"><span data-stu-id="4f85b-169">*IntegrationTests/IntegrationTests.csproj*</span></span>

<span data-ttu-id="4f85b-170">両方のプロジェクトを監視するのが目的である場合、両方のプロジェクトを監視するように構成されたカスタム プロジェクト ファイルを作成します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-170">If the goal is to watch both projects, create a custom project file configured to watch both projects:</span></span>

```xml
<Project>
    <ItemGroup>
        <TestProjects Include="**\*.csproj" />
        <Watch Include="**\*.cs" />
    </ItemGroup>

    <Target Name="Test">
        <MSBuild Targets="VSTest" Projects="@(TestProjects)" />
    </Target>

    <Import Project="$(MSBuildExtensionsPath)\Microsoft.Common.targets" />
</Project>
```

<span data-ttu-id="4f85b-171">両方のプロジェクトでファイルの監視を開始するには、*test* フォルダーに変更します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-171">To start file watching on both projects, change to the *test* folder.</span></span> <span data-ttu-id="4f85b-172">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="4f85b-172">Execute the following command:</span></span>

```console
dotnet watch msbuild /t:Test
```

<span data-ttu-id="4f85b-173">VSTest は、いずれかのテスト プロジェクトでファイルが変更されたときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-173">VSTest executes when any file changes in either test project.</span></span>

## <a name="dotnet-watch-in-github"></a><span data-ttu-id="4f85b-174">GitHub での `dotnet-watch`</span><span class="sxs-lookup"><span data-stu-id="4f85b-174">`dotnet-watch` in GitHub</span></span>

<span data-ttu-id="4f85b-175">`dotnet-watch` は GitHub [DotNetTools リポジトリ](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch)に含まれます。</span><span class="sxs-lookup"><span data-stu-id="4f85b-175">`dotnet-watch` is part of the GitHub [DotNetTools repository](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-watch).</span></span>
