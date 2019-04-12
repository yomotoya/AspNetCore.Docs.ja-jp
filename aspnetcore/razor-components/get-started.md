---
title: Razor のコンポーネントを概要します。
author: guardrex
description: 作成と Razor コンポーネント プロジェクトを変更して Razor コンポーネントを開始する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/get-started
ms.openlocfilehash: 151e58497b0f22fa7c5a9bde1f665eeb73fd5dc3
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59515651"
---
# <a name="get-started-with-razor-components"></a>Razor のコンポーネントを概要します。

作成者: [Daniel Roth](https://github.com/danroth27)、[Luke Latham](https://github.com/guardrex)

# [<a name="visual-studio"></a>Visual Studio](#tab/visual-studio)

必要条件:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Visual Studio で、最初の Razor コンポーネント プロジェクトを作成するには

1. 最新のインストール[.NET Core 3.0 プレビュー SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)リリースします。
1. Visual Studio プレビュー Sdk の使用を有効にします。
   1. 開いている**ツール** > **オプション**メニュー バーにします。
   1. 開く、**プロジェクトおよびソリューション**ノード。 開く、 **.NET Core**タブ。
   1. チェック ボックスをオン **、.NET Core SDK のプレビューを使用して、** します。 **[OK]** を選択します。
1. 新しいプロジェクトを作成します。
1. **[ASP.NET Core Web アプリケーション]** を選択します。 **[次へ]** を選択します。
1. 名前を入力、**プロジェクト名**フィールド。 確認、**場所**エントリが正しいか、プロジェクトの場所を指定します。 **[作成]** を選択します。
1. 確認します **.NET Core**と**ASP.NET Core 3.0**上部にある選択されます。
1. 選択、 **Razor コンポーネント**テンプレートと選択**作成**です。
1. **F5 キー**を押してアプリを実行します。

おめでとうございます! 最初の Razor コンポーネント アプリを実行しました。

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# [<a name="net-core-cli"></a>.NET Core CLI](#tab/netcore-cli/)

必要条件:

* [.NET core SDK 3.0 プレビュー](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. コマンド シェルから、最初の Razor コンポーネント プロジェクトを作成するには

   ```console
   dotnet new razorcomponents -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. ブラウザーで、`https://localhost:5001` に移動します。

おめでとうございます! 最初の Razor コンポーネント アプリを実行しました。

---

## <a name="razor-components-project"></a>Razor のコンポーネントのプロジェクト

Razor のコンポーネントは、Razor 構文を使用して作成されますが、Razor ページと MVC のビューとは異なるコンパイルされます。 *.Razor* Razor コンポーネントを指定するファイル拡張子を使用します。 Razor ページと MVC ビューを引き続き使用する、 *.cshtml*ファイル拡張子。

> [!NOTE]
> 使用して razor コンポーネントを作成することができます、 *.cshtml* Razor コンポーネントのファイルを使用してそれらのファイルが検出された場合に限り、ファイル拡張子、 `_RazorComponentInclude` MSBuild プロパティです。 などのコンポーネントの Razor テンプレートを使用して作成されたアプリを指定するすべて *.cshtml*下でファイル、*コンポーネント*Razor コンポーネントとしてフォルダーを処理する必要があります。
>
> ```xml
> <_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
> ```

アプリを実行すると、複数のページは、サイドバー内のタブから利用できます。

* Home
* カウンター
* データをフェッチします。

Counter ページ上で **[クリックしてください]** ボタンを選択し、ページを更新することなくカウンターをインクリメントします。 Web ページでカウンターをインクリメントする場合、通常は JavaScript を記述することが必要ですが、Razor Components には C# を使ったより優れた方法が用意されています。

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

要求を`/counter`で指定したとおり、ブラウザーで、`@page`上部にあるディレクティブがそのコンテンツを表示するために、カウンターのコンポーネント。 コンポーネントは、柔軟で効率的な方法で UI を更新するために使用するレンダリング ツリーのメモリ内表現にレンダリングします。

毎回、 **Click me**ボタンが選択されています。

* `onclick`イベントが発生します。
* `IncrementCount` メソッドが呼び出された場合。
* `currentCount`がインクリメントされます。
* コンポーネントは、もう一度表示されます。

ランタイムは、前のコンテンツを新しいコンテンツを比較し、ドキュメント オブジェクト モデル (DOM) に変更されたコンテンツにのみ適用されます。

HTML のような構文を使用して別のコンポーネントにコンポーネントを追加します。 コンポーネントのパラメーターは、属性または子コンテンツを使用して指定されます。 追加することでカウンター コンポーネントをアプリのホーム ページに追加するなど、`<Counter />`インデックス コンポーネント要素。

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

アプリを実行します。 ホームページには、独自のカウンターを指定します。

カウンターのコンポーネントにパラメーターを追加する更新コンポーネントの`@functions`ブロック。

* プロパティを追加`IncrementAmount`で修飾された、`[Parameter]`属性。
* `currentCount` の値を増やすときに `IncrementAmount` を使うように `IncrementCount` メソッドを変更します。

*WebApplication1/Components/Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4,8)]

属性を使って、Home コンポーネントの `<Counter>` 要素に `IncrementAmount` パラメーターを指定します。

*WebApplication1/Components/Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

アプリを実行します。 ホームページには、10 たびにインクリメントする独自のカウンター、 **Click me**ボタンが選択されています。

## <a name="next-steps"></a>次の手順

<xref:tutorials/first-razor-components-app>
