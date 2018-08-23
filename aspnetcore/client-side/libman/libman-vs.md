---
title: LibMan を Visual Studio で ASP.NET Core を使用します。
author: scottaddie
description: LibMan を Visual Studio を使用した ASP.NET Core プロジェクトで使用する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: b44769f1d0925f38523d6570858de17f37e32c2b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909875"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="66c44-103">LibMan を Visual Studio で ASP.NET Core を使用します。</span><span class="sxs-lookup"><span data-stu-id="66c44-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="66c44-104">作成者: [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="66c44-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="66c44-105">Visual Studio はの組み込みサポート[LibMan](xref:client-side/libman/index)など、ASP.NET Core プロジェクトで。</span><span class="sxs-lookup"><span data-stu-id="66c44-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="66c44-106">構成とビルドで LibMan 復元操作の実行をサポートします。</span><span class="sxs-lookup"><span data-stu-id="66c44-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="66c44-107">LibMan をトリガーするためのメニュー項目は、復元し、操作をクリーンアップします。</span><span class="sxs-lookup"><span data-stu-id="66c44-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="66c44-108">ライブラリを検索して、ファイルをプロジェクトに追加する検索ダイアログ。</span><span class="sxs-lookup"><span data-stu-id="66c44-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="66c44-109">編集のサポート*libman.json*&mdash;LibMan マニフェスト ファイル。</span><span class="sxs-lookup"><span data-stu-id="66c44-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="66c44-110">[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(ダウンロードする方法)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="66c44-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66c44-111">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="66c44-111">Prerequisites</span></span>

* <span data-ttu-id="66c44-112">Visual Studio 2017 バージョン 15.8 以降と、 **ASP.NET および web 開発**ワークロード</span><span class="sxs-lookup"><span data-stu-id="66c44-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="66c44-113">ライブラリ ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="66c44-113">Add library files</span></span>

<span data-ttu-id="66c44-114">ライブラリ ファイルは、2 つの方法で ASP.NET Core プロジェクトに追加できます。</span><span class="sxs-lookup"><span data-stu-id="66c44-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="66c44-115">クライアント側ライブラリを追加 ダイアログを使用します。</span><span class="sxs-lookup"><span data-stu-id="66c44-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="66c44-116">LibMan マニフェスト ファイルのエントリを手動で構成します。</span><span class="sxs-lookup"><span data-stu-id="66c44-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="66c44-117">クライアント側ライブラリを追加 ダイアログを使用します。</span><span class="sxs-lookup"><span data-stu-id="66c44-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="66c44-118">クライアント側ライブラリをインストールする手順に従います。</span><span class="sxs-lookup"><span data-stu-id="66c44-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="66c44-119">**ソリューション エクスプ ローラー**ファイルを追加するプロジェクト フォルダーを右クリックします。</span><span class="sxs-lookup"><span data-stu-id="66c44-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="66c44-120">選択**追加** > **クライアント側ライブラリ**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="66c44-121">**クライアント側ライブラリを追加**ダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![クライアント側ライブラリを追加ダイアログ](_static/add-library-dialog.png)

* <span data-ttu-id="66c44-123">ライブラリ プロバイダーの選択、**プロバイダー**ドロップダウンします。</span><span class="sxs-lookup"><span data-stu-id="66c44-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="66c44-124">CDNJS は、既定のプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="66c44-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="66c44-125">フェッチするライブラリの名前を入力、**ライブラリ**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="66c44-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="66c44-126">IntelliSense では、指定されたテキストで始まるライブラリの一覧を示します。</span><span class="sxs-lookup"><span data-stu-id="66c44-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="66c44-127">IntelliSense の一覧からライブラリを選択します。</span><span class="sxs-lookup"><span data-stu-id="66c44-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="66c44-128">ライブラリ名が付くに注意してください、`@`記号と、選択したプロバイダーに既知の最新の安定バージョン。</span><span class="sxs-lookup"><span data-stu-id="66c44-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="66c44-129">含めるファイルを決定します。</span><span class="sxs-lookup"><span data-stu-id="66c44-129">Decide which files to include:</span></span>
  * <span data-ttu-id="66c44-130">選択、**ライブラリのすべてのファイルを含める**ラジオ ボタンをすべてのライブラリのファイルを含めます。</span><span class="sxs-lookup"><span data-stu-id="66c44-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="66c44-131">選択、**特定のファイルを選択**ラジオ ボタンは、ライブラリのファイルのサブセットが含まれます。</span><span class="sxs-lookup"><span data-stu-id="66c44-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="66c44-132">ラジオ ボタンを選択すると、ファイルのセレクターのツリーが有効になります。</span><span class="sxs-lookup"><span data-stu-id="66c44-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="66c44-133">ダウンロードするファイル名の左側にあるボックスを確認します。</span><span class="sxs-lookup"><span data-stu-id="66c44-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="66c44-134">内のファイルを格納するためのプロジェクト フォルダーを指定して、**ターゲットの場所**テキスト ボックス。</span><span class="sxs-lookup"><span data-stu-id="66c44-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="66c44-135">、推奨事項としては、別のフォルダーに各ライブラリを格納します。</span><span class="sxs-lookup"><span data-stu-id="66c44-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="66c44-136">提案される**ターゲットの場所**フォルダーは、ダイアログの起動元の場所に基づきます。</span><span class="sxs-lookup"><span data-stu-id="66c44-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="66c44-137">場合は、プロジェクトのルートから起動します。</span><span class="sxs-lookup"><span data-stu-id="66c44-137">If launched from the project root:</span></span>
    * <span data-ttu-id="66c44-138">*wwwroot/lib*場合に使用*wwwroot*存在します。</span><span class="sxs-lookup"><span data-stu-id="66c44-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="66c44-139">*lib*場合に使用*wwwroot*が存在しません。</span><span class="sxs-lookup"><span data-stu-id="66c44-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="66c44-140">プロジェクト フォルダーから起動する場合は、対応するフォルダー名が使用されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="66c44-141">フォルダーの提案は、ライブラリ名サフィックスとして付けられます。</span><span class="sxs-lookup"><span data-stu-id="66c44-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="66c44-142">次の表は、Razor ページ プロジェクトで jQuery をインストールするときに、フォルダーの推奨事項を示します。</span><span class="sxs-lookup"><span data-stu-id="66c44-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="66c44-143">場所を起動します。</span><span class="sxs-lookup"><span data-stu-id="66c44-143">Launch location</span></span>                           |<span data-ttu-id="66c44-144">推奨されるフォルダー</span><span class="sxs-lookup"><span data-stu-id="66c44-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="66c44-145">プロジェクトのルート (場合*wwwroot*が存在する)</span><span class="sxs-lookup"><span data-stu-id="66c44-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="66c44-146">*wwwroot/ライブラリ/jquery/*</span><span class="sxs-lookup"><span data-stu-id="66c44-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="66c44-147">プロジェクトのルート (場合*wwwroot*が存在しない)</span><span class="sxs-lookup"><span data-stu-id="66c44-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="66c44-148">*lib/jquery/*</span><span class="sxs-lookup"><span data-stu-id="66c44-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="66c44-149">*ページ*プロジェクト フォルダー</span><span class="sxs-lookup"><span data-stu-id="66c44-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="66c44-150">*ページ/jquery/*</span><span class="sxs-lookup"><span data-stu-id="66c44-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="66c44-151">をクリックして、**インストール**ボタンで構成ごとのファイルをダウンロードする*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="66c44-152">レビュー、**ライブラリ マネージャー**のフィード、**出力**インストールの詳細についてはウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="66c44-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="66c44-153">例えば:</span><span class="sxs-lookup"><span data-stu-id="66c44-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="66c44-154">LibMan マニフェスト ファイルのエントリを手動で構成します。</span><span class="sxs-lookup"><span data-stu-id="66c44-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="66c44-155">Visual Studio でのすべての LibMan 操作は、プロジェクトのルートの LibMan マニフェストの内容に基づきます (*libman.json*)。</span><span class="sxs-lookup"><span data-stu-id="66c44-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="66c44-156">手動で編集できます*libman.json*プロジェクトのライブラリ ファイルを構成します。</span><span class="sxs-lookup"><span data-stu-id="66c44-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="66c44-157">Visual Studio は、すべてのライブラリ ファイルを 1 回復元*libman.json*保存されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="66c44-158">開くには*libman.json*編集に関しては、次のオプションが存在します。</span><span class="sxs-lookup"><span data-stu-id="66c44-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="66c44-159">ダブルクリックして、 *libman.json*ファイル**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="66c44-160">プロジェクトを右クリックして**ソリューション エクスプ ローラー**選択**クライアント側ライブラリを管理**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="66c44-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="66c44-161">**&#8224;**</span></span>
* <span data-ttu-id="66c44-162">選択**クライアント側ライブラリを管理**Visual Studio から**プロジェクト**メニュー。</span><span class="sxs-lookup"><span data-stu-id="66c44-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="66c44-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="66c44-163">**&#8224;**</span></span>

<span data-ttu-id="66c44-164">**&#8224;** 場合、 *libman.json*プロジェクトのルートにファイルが存在しない、既定の項目テンプレートの内容で作成されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="66c44-165">Visual Studio では、豊富な JSON 編集の色付けなどのサポート、書式設定、IntelliSense、およびスキーマ検証を提供します。</span><span class="sxs-lookup"><span data-stu-id="66c44-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="66c44-166">LibMan マニフェストの JSON スキーマで見つかった[ http://json.schemastore.org/libman](http://json.schemastore.org/libman)します。</span><span class="sxs-lookup"><span data-stu-id="66c44-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="66c44-167">LibMan がで定義されている構成ごとにファイルを取得する次のマニフェスト ファイルで、`libraries`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="66c44-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="66c44-168">内で定義されているオブジェクト リテラルの説明`libraries`に従います。</span><span class="sxs-lookup"><span data-stu-id="66c44-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="66c44-169">サブセットの[jQuery](https://jquery.com/)バージョン 3.3.1 が CDNJS プロバイダーから取得します。</span><span class="sxs-lookup"><span data-stu-id="66c44-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="66c44-170">サブセットが定義されている、`files`プロパティ&mdash;*jquery.min.js*、 *jquery.js*、および*jquery.min.map*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="66c44-171">ファイルはプロジェクトの配置*wwwroot/ライブラリ/jquery*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="66c44-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="66c44-172">全体[ブートス トラップ](https://getbootstrap.com/)4.1.3 のバージョンが取得され、配置、*ブートス トラップwwwroot/ライブラリ/* フォルダー。</span><span class="sxs-lookup"><span data-stu-id="66c44-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="66c44-173">オブジェクト リテラルの`provider`プロパティのオーバーライド、`defaultProvider`プロパティの値。</span><span class="sxs-lookup"><span data-stu-id="66c44-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="66c44-174">LibMan は、unpkg プロバイダーからブートス トラップ ファイルを取得します。</span><span class="sxs-lookup"><span data-stu-id="66c44-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="66c44-175">サブセットの[Lodash](https://lodash.com/)組織内で管理本体によって承認されました。</span><span class="sxs-lookup"><span data-stu-id="66c44-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="66c44-176">*Lodash.js*と*lodash.min.js*からローカル ファイル システムのファイルが取得*c:\\tmp\\*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\tmp\\*.</span></span> <span data-ttu-id="66c44-177">ファイルをプロジェクトのコピーは*wwwroot/ライブラリ/lodash*フォルダー。</span><span class="sxs-lookup"><span data-stu-id="66c44-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="66c44-178">LibMan は、各プロバイダーから各ライブラリの 1 つのバージョンのみをサポートします。</span><span class="sxs-lookup"><span data-stu-id="66c44-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="66c44-179">*Libman.json*ファイル指定されたプロバイダーの同じライブラリ名を持つ 2 つのライブラリが含まれているスキーマの検証は失敗します。</span><span class="sxs-lookup"><span data-stu-id="66c44-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="66c44-180">ライブラリ ファイルを復元します。</span><span class="sxs-lookup"><span data-stu-id="66c44-180">Restore library files</span></span>

<span data-ttu-id="66c44-181">Visual Studio 内からのライブラリ ファイルを復元する必要があります、有効な*libman.json*プロジェクト ルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="66c44-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="66c44-182">復元されたファイルは、各ライブラリの指定された場所にプロジェクトに配置されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="66c44-183">2 つの方法で ASP.NET Core プロジェクトでは、ライブラリ ファイルを復元できます。</span><span class="sxs-lookup"><span data-stu-id="66c44-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="66c44-184">ビルド中にファイルを復元します。</span><span class="sxs-lookup"><span data-stu-id="66c44-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="66c44-185">ファイルを手動で復元します。</span><span class="sxs-lookup"><span data-stu-id="66c44-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="66c44-186">ビルド中にファイルを復元します。</span><span class="sxs-lookup"><span data-stu-id="66c44-186">Restore files during build</span></span>

<span data-ttu-id="66c44-187">LibMan は、ビルド プロセスの一部として定義されているライブラリ ファイルを復元できます。</span><span class="sxs-lookup"><span data-stu-id="66c44-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="66c44-188">既定で、*ビルド時の復元*動作を無効にします。</span><span class="sxs-lookup"><span data-stu-id="66c44-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="66c44-189">有効にして、ビルド時の復元の動作をテストします。</span><span class="sxs-lookup"><span data-stu-id="66c44-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="66c44-190">右クリック*libman.json*で**ソリューション エクスプ ローラー**選択と**復元クライアント側のライブラリでビルドを有効にする**コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="66c44-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="66c44-191">をクリックして、**はい**NuGet パッケージをインストールするように求められたらボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="66c44-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="66c44-192">[Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet パッケージがプロジェクトに追加します。</span><span class="sxs-lookup"><span data-stu-id="66c44-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="66c44-193">LibMan ファイル復元が発生することを確認します。 プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="66c44-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="66c44-194">`Microsoft.Web.LibraryManager.Build`パッケージは、プロジェクトのビルド操作中に LibMan を実行する MSBuild ターゲットを挿入します。</span><span class="sxs-lookup"><span data-stu-id="66c44-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="66c44-195">レビュー、**ビルド**のフィード、**出力**LibMan アクティビティ ログのウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="66c44-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="66c44-196">ビルド時の復元動作が有効にすると、 *libman.json*コンテキスト メニューが表示されます、**復元クライアント側のライブラリでビルドを無効にする**オプション。</span><span class="sxs-lookup"><span data-stu-id="66c44-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="66c44-197">このオプションを選択すると削除、`Microsoft.Web.LibraryManager.Build`パッケージ、プロジェクト ファイルから参照します。</span><span class="sxs-lookup"><span data-stu-id="66c44-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="66c44-198">その結果、クライアント側ライブラリは不要になった各ビルドで復元されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="66c44-199">ビルド時の復元の設定に関係なく手動で復元できますから、いつでも、 *libman.json*コンテキスト メニュー。</span><span class="sxs-lookup"><span data-stu-id="66c44-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="66c44-200">詳細については、次を参照してください。[ファイルを手動で復元](#restore-files-manually)します。</span><span class="sxs-lookup"><span data-stu-id="66c44-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="66c44-201">ファイルを手動で復元します。</span><span class="sxs-lookup"><span data-stu-id="66c44-201">Restore files manually</span></span>

<span data-ttu-id="66c44-202">ライブラリ ファイルを手動で復元するには。</span><span class="sxs-lookup"><span data-stu-id="66c44-202">To manually restore library files:</span></span>

* <span data-ttu-id="66c44-203">ソリューション内のすべてのプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="66c44-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="66c44-204">ソリューション名を右クリックして**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="66c44-205">選択、**クライアント側ライブラリを復元**オプション。</span><span class="sxs-lookup"><span data-stu-id="66c44-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="66c44-206">特定のプロジェクト。</span><span class="sxs-lookup"><span data-stu-id="66c44-206">For a specific project:</span></span>
  * <span data-ttu-id="66c44-207">右クリックし、 *libman.json*ファイル**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="66c44-208">選択、**クライアント側ライブラリを復元**オプション。</span><span class="sxs-lookup"><span data-stu-id="66c44-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="66c44-209">復元操作の実行: 中</span><span class="sxs-lookup"><span data-stu-id="66c44-209">While the restore operation is running:</span></span>

* <span data-ttu-id="66c44-210">Visual Studio のステータス バーにタスク ステータス センター (TSC) アイコンがアニメーション化して、読み取りは*復元操作が開始された*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="66c44-211">アイコンをクリックすると、既知のバック グラウンド タスクを一覧表示するツールヒントが開きます。</span><span class="sxs-lookup"><span data-stu-id="66c44-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="66c44-212">メッセージは、ステータス バーに送信して、**ライブラリ マネージャー**のフィード、**出力**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="66c44-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="66c44-213">例えば:</span><span class="sxs-lookup"><span data-stu-id="66c44-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="66c44-214">ライブラリ ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="66c44-214">Delete library files</span></span>

<span data-ttu-id="66c44-215">実行する、*クリーン*操作で、Visual Studio で以前に復元されたライブラリ ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="66c44-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="66c44-216">右クリックし、 *libman.json*ファイル**ソリューション エクスプ ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="66c44-217">選択、**クライアント側ライブラリをクリーン**オプション。</span><span class="sxs-lookup"><span data-stu-id="66c44-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="66c44-218">ライブラリ以外のファイルの意図しない削除を防ぐためには、クリーンの操作は、ディレクトリ全体を削除されません。</span><span class="sxs-lookup"><span data-stu-id="66c44-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="66c44-219">前の復元に含まれていたファイルを削除するだけです。</span><span class="sxs-lookup"><span data-stu-id="66c44-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="66c44-220">クリーン操作が実行されている場合: 中</span><span class="sxs-lookup"><span data-stu-id="66c44-220">While the clean operation is running:</span></span>

* <span data-ttu-id="66c44-221">Visual Studio のステータス バーに TSC アイコンは、アニメーション化して、読み取りは*クライアント ライブラリの操作を開始*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="66c44-222">アイコンをクリックすると、既知のバック グラウンド タスクを一覧表示するツールヒントが開きます。</span><span class="sxs-lookup"><span data-stu-id="66c44-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="66c44-223">ステータス バーに送信されるメッセージと**ライブラリ マネージャー**のフィード、**出力**ウィンドウ。</span><span class="sxs-lookup"><span data-stu-id="66c44-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="66c44-224">例えば:</span><span class="sxs-lookup"><span data-stu-id="66c44-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="66c44-225">クリーン操作では、ファイルがプロジェクトからのみ削除します。</span><span class="sxs-lookup"><span data-stu-id="66c44-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="66c44-226">ライブラリ ファイルは、将来の復元操作に高速な検索のキャッシュに保存します。</span><span class="sxs-lookup"><span data-stu-id="66c44-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="66c44-227">ローカル コンピューターのキャッシュに格納されているライブラリ ファイルを管理するには、LibMan CLI を使用します。</span><span class="sxs-lookup"><span data-stu-id="66c44-227">To manage library files stored in the local machine's cache, use the LibMan CLI.</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="66c44-228">ライブラリ ファイルをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="66c44-228">Uninstall library files</span></span>

<span data-ttu-id="66c44-229">ライブラリ ファイルをアンインストールするには。</span><span class="sxs-lookup"><span data-stu-id="66c44-229">To uninstall library files:</span></span>

* <span data-ttu-id="66c44-230">開いている*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-230">Open *libman.json*.</span></span>
* <span data-ttu-id="66c44-231">対応の内側にキャレットを配置`libraries`オブジェクト リテラル。</span><span class="sxs-lookup"><span data-stu-id="66c44-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="66c44-232">左の余白に表示される電球アイコンをクリックし、選択**アンインストール\<library_name > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="66c44-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![ライブラリのコンテキスト メニュー オプションをアンインストールします。](_static/uninstall-menu-option.png)

<span data-ttu-id="66c44-234">または、手動で編集して保存できます LibMan マニフェスト (*libman.json*)。</span><span class="sxs-lookup"><span data-stu-id="66c44-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="66c44-235">[復元操作に](#restore-library-files)ファイルを保存するときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="66c44-236">ライブラリ ファイルで定義されている不要になった*libman.json*プロジェクトから削除されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="66c44-237">ライブラリのバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="66c44-237">Update library version</span></span>

<span data-ttu-id="66c44-238">ライブラリの更新バージョンを確認します。</span><span class="sxs-lookup"><span data-stu-id="66c44-238">To check for an updated library version:</span></span>

* <span data-ttu-id="66c44-239">開いている*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="66c44-239">Open *libman.json*.</span></span>
* <span data-ttu-id="66c44-240">対応の内側にキャレットを配置`libraries`オブジェクト リテラル。</span><span class="sxs-lookup"><span data-stu-id="66c44-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="66c44-241">左余白に表示される電球アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="66c44-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="66c44-242">ポインターを合わせる**更新プログラムの確認**します。</span><span class="sxs-lookup"><span data-stu-id="66c44-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="66c44-243">LibMan は、インストールされているバージョンより新しいバージョンのライブラリをチェックします。</span><span class="sxs-lookup"><span data-stu-id="66c44-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="66c44-244">結果は次が発生することができます。</span><span class="sxs-lookup"><span data-stu-id="66c44-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="66c44-245">A**なしの更新が見つかりました**最新バージョンが既にインストールされている場合、メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="66c44-246">最新の安定バージョンが表示されていない場合は。</span><span class="sxs-lookup"><span data-stu-id="66c44-246">The latest stable version is displayed if not already installed.</span></span>

  ![コンテキスト メニュー オプションの更新プログラムを確認します。](_static/update-menu-option.png)

* <span data-ttu-id="66c44-248">インストールされているバージョンより新しいのプレリリース版を使用できる場合は、プレリリース版が表示されます。</span><span class="sxs-lookup"><span data-stu-id="66c44-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="66c44-249">ライブラリの以前のバージョンにダウン グレードする手動で編集、 *libman.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="66c44-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="66c44-250">保存すると、ファイルは、LibMan[復元操作に](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="66c44-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="66c44-251">以前のバージョンからの余分なファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="66c44-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="66c44-252">新しいバージョンから、新規および更新されたファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="66c44-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66c44-253">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="66c44-253">Additional resources</span></span>

* [<span data-ttu-id="66c44-254">LibMan GitHub リポジトリ</span><span class="sxs-lookup"><span data-stu-id="66c44-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
