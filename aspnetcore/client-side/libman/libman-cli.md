---
title: ASP.NET Core で LibMan コマンド ライン インターフェイス (CLI) を使用します。
author: scottaddie
description: ASP.NET Core プロジェクトで LibMan コマンド ライン インターフェイス (CLI) を使用する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: ad81af2e789a31382f50ed37754bfc94469eb197
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336038"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="5deb5-103">ASP.NET Core で LibMan コマンド ライン インターフェイス (CLI) を使用します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="5deb5-104">作成者: [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="5deb5-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="5deb5-105">[LibMan](xref:client-side/libman/index) CLI は .NET Core がサポートされているすべての場所がサポートされるクロスプラット フォーム ツールです。</span><span class="sxs-lookup"><span data-stu-id="5deb5-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5deb5-106">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="5deb5-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="5deb5-107">インストール</span><span class="sxs-lookup"><span data-stu-id="5deb5-107">Installation</span></span>

<span data-ttu-id="5deb5-108">LibMan CLI のインストール。</span><span class="sxs-lookup"><span data-stu-id="5deb5-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="5deb5-109">A [.NET Core のグローバル ツール](/dotnet/core/tools/global-tools#install-a-global-tool)からインストールされている、 [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet パッケージ。</span><span class="sxs-lookup"><span data-stu-id="5deb5-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="5deb5-110">特定の NuGet パッケージのソースから LibMan CLI をインストールします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="5deb5-111">ローカルの Windows コンピューターの前の例では、.NET Core のグローバル ツールがインストールされている*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5deb5-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="5deb5-112">使用法</span><span class="sxs-lookup"><span data-stu-id="5deb5-112">Usage</span></span>

<span data-ttu-id="5deb5-113">CLI のインストールの成功後、次のコマンドを使用できます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="5deb5-114">インストールされている CLI バージョンを表示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="5deb5-115">使用可能な CLI コマンドを表示するには。</span><span class="sxs-lookup"><span data-stu-id="5deb5-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="5deb5-116">上記のコマンドには、次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="5deb5-117">次のセクションでは、使用可能な CLI コマンドを説明します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="5deb5-118">LibMan をプロジェクトを初期化します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="5deb5-119">`libman init`コマンドは、作成、 *libman.json*ファイルのいずれかが存在しない場合。</span><span class="sxs-lookup"><span data-stu-id="5deb5-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="5deb5-120">既定の項目テンプレートのコンテンツ ファイルが作成されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-121">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5deb5-122">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-122">Options</span></span>

<span data-ttu-id="5deb5-123">次のオプションを使用できる、`libman init`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="5deb5-124">現在のフォルダーの相対パス。</span><span class="sxs-lookup"><span data-stu-id="5deb5-124">A path relative to the current folder.</span></span> <span data-ttu-id="5deb5-125">ライブラリ ファイルがこの場所にインストールされていない場合`destination`のライブラリのプロパティが定義されている*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="5deb5-126">`<PATH>`に値が書き込まれる、`defaultDestination`プロパティの*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="5deb5-127">指定したライブラリのプロバイダーが定義されていない場合に使用するプロバイダー。</span><span class="sxs-lookup"><span data-stu-id="5deb5-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="5deb5-128">`<PROVIDER>`に値が書き込まれる、`defaultProvider`プロパティの*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="5deb5-129">置換`<PROVIDER>`次の値のいずれかの。</span><span class="sxs-lookup"><span data-stu-id="5deb5-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-130">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-130">Examples</span></span>

<span data-ttu-id="5deb5-131">作成する、 *libman.json* ASP.NET Core プロジェクト ファイル。</span><span class="sxs-lookup"><span data-stu-id="5deb5-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="5deb5-132">プロジェクトのルートに移動します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-132">Navigate to the project root.</span></span>
* <span data-ttu-id="5deb5-133">次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="5deb5-134">キーを押して、既定のプロバイダーの名前を入力`Enter`CDNJS の既定のプロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="5deb5-135">有効な値を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init コマンドの既定のプロバイダー](_static/libman-init-provider.png)

<span data-ttu-id="5deb5-137">A *libman.json*ファイル、プロジェクトのルート以下の内容に追加されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="5deb5-138">ライブラリ ファイルを追加します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-138">Add library files</span></span>

<span data-ttu-id="5deb5-139">`libman install`コマンドは、ダウンロードし、プロジェクトにライブラリ ファイルをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="5deb5-140">A *libman.json*ファイルが存在していない場合に追加されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="5deb5-141">*Libman.json*ライブラリ ファイルの構成の詳細を格納するファイルを変更します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-142">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5deb5-143">引数</span><span class="sxs-lookup"><span data-stu-id="5deb5-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5deb5-144">インストールするためにライブラリの名前。</span><span class="sxs-lookup"><span data-stu-id="5deb5-144">The name of the library to install.</span></span> <span data-ttu-id="5deb5-145">この名前は、バージョン番号の表記法を含めることができます (たとえば、 `@1.2.0`)。</span><span class="sxs-lookup"><span data-stu-id="5deb5-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="5deb5-146">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-146">Options</span></span>

<span data-ttu-id="5deb5-147">次のオプションを使用できる、`libman install`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="5deb5-148">ライブラリをインストールする場所。</span><span class="sxs-lookup"><span data-stu-id="5deb5-148">The location to install the library.</span></span> <span data-ttu-id="5deb5-149">指定しない場合、既定の場所が使用されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-149">If not specified, the default location is used.</span></span> <span data-ttu-id="5deb5-150">ない場合は`defaultDestination`プロパティが指定されて*libman.json*、このオプションが必要です。</span><span class="sxs-lookup"><span data-stu-id="5deb5-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="5deb5-151">ライブラリからインストールするファイルの名前を指定します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="5deb5-152">指定しない場合、ライブラリからすべてのファイルがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="5deb5-153">1 つ提供`--files`ファイルごとのオプションをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="5deb5-154">相対パスはもサポートします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-154">Relative paths are supported too.</span></span> <span data-ttu-id="5deb5-155">たとえば、`--files dist/browser/signalr.js` のように指定します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="5deb5-156">ライブラリの取得を使用するプロバイダーの名前。</span><span class="sxs-lookup"><span data-stu-id="5deb5-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="5deb5-157">置換`<PROVIDER>`次の値のいずれかの。</span><span class="sxs-lookup"><span data-stu-id="5deb5-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="5deb5-158">指定しない場合、`defaultProvider`プロパティ*libman.json*使用されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="5deb5-159">ない場合は`defaultProvider`プロパティが指定されて*libman.json*、このオプションが必要です。</span><span class="sxs-lookup"><span data-stu-id="5deb5-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-160">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-160">Examples</span></span>

<span data-ttu-id="5deb5-161">次を考慮*libman.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5deb5-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="5deb5-162">3.2.1 jQuery バージョンをインストールする*jquery.min.js*ファイルを*wwwroot\scripts\jquery* CDNJS プロバイダーを使用してフォルダー。</span><span class="sxs-lookup"><span data-stu-id="5deb5-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot\scripts\jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

<span data-ttu-id="5deb5-163">*Libman.json*ファイルが、次に似ています。</span><span class="sxs-lookup"><span data-stu-id="5deb5-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="5deb5-164">インストールする、 *calendar.js*と*calendar.css*ファイルから*c:\\temp\\contosoCalendar\\* ファイル システムを使用します。プロバイダー:</span><span class="sxs-lookup"><span data-stu-id="5deb5-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="5deb5-165">次のプロンプトは、2 つの理由が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="5deb5-166">*Libman.json*ファイルが含まれていない、`defaultDestination`プロパティ。</span><span class="sxs-lookup"><span data-stu-id="5deb5-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="5deb5-167">`libman install`コマンドが含まれていない、`-d|--destination`オプション。</span><span class="sxs-lookup"><span data-stu-id="5deb5-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![libman インストール コマンド - 変換先](_static/libman-install-destination.png)

<span data-ttu-id="5deb5-169">既定の転送先に同意した後、 *libman.json*ファイルが、次に似ています。</span><span class="sxs-lookup"><span data-stu-id="5deb5-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot\\lib\\contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="5deb5-170">ライブラリ ファイルを復元します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-170">Restore library files</span></span>

<span data-ttu-id="5deb5-171">`libman restore`コマンドで定義されているライブラリ ファイルはインストール*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="5deb5-172">次の規則が適用されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-172">The following rules apply:</span></span>

* <span data-ttu-id="5deb5-173">ない場合は*libman.json*プロジェクトのルートにファイルが存在するエラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="5deb5-174">ライブラリは、プロバイダーを指定する場合、`defaultProvider`プロパティ*libman.json*は無視されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="5deb5-175">ライブラリは、変換先を指定する場合、`defaultDestination`プロパティ*libman.json*は無視されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-176">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5deb5-177">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-177">Options</span></span>

<span data-ttu-id="5deb5-178">次のオプションを使用できる、`libman restore`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-179">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-179">Examples</span></span>

<span data-ttu-id="5deb5-180">定義されているライブラリ ファイルを復元する*libman.json*:</span><span class="sxs-lookup"><span data-stu-id="5deb5-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="5deb5-181">ライブラリ ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-181">Delete library files</span></span>

<span data-ttu-id="5deb5-182">`libman clean`コマンド LibMan を使用して以前に復元されたライブラリ ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="5deb5-183">この操作の後に空になったフォルダーは削除されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="5deb5-184">構成に関連するライブラリ ファイルの`libraries`プロパティの*libman.json*は削除されません。</span><span class="sxs-lookup"><span data-stu-id="5deb5-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-185">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="5deb5-186">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-186">Options</span></span>

<span data-ttu-id="5deb5-187">次のオプションを使用できる、`libman clean`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-188">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-188">Examples</span></span>

<span data-ttu-id="5deb5-189">LibMan 経由でインストールされているライブラリ ファイルを削除します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="5deb5-190">ライブラリ ファイルをアンインストールします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-190">Uninstall library files</span></span>

<span data-ttu-id="5deb5-191">`libman uninstall`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="5deb5-192">変換先から、指定したライブラリに関連付けられているすべてのファイルを削除します。 *libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="5deb5-193">関連するライブラリの構成を削除します*libman.json*します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="5deb5-194">エラーが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="5deb5-194">An error occurs when:</span></span>

* <span data-ttu-id="5deb5-195">いいえ*libman.json*ファイル、プロジェクトのルートに存在します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="5deb5-196">指定したライブラリが存在しません。</span><span class="sxs-lookup"><span data-stu-id="5deb5-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="5deb5-197">同じ名前の 1 つ以上のライブラリがインストールされている場合は、いずれかを選択するように求められます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-198">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5deb5-199">引数</span><span class="sxs-lookup"><span data-stu-id="5deb5-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5deb5-200">アンインストールするライブラリの名前。</span><span class="sxs-lookup"><span data-stu-id="5deb5-200">The name of the library to uninstall.</span></span> <span data-ttu-id="5deb5-201">この名前は、バージョン番号の表記法を含めることができます (たとえば、 `@1.2.0`)。</span><span class="sxs-lookup"><span data-stu-id="5deb5-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="5deb5-202">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-202">Options</span></span>

<span data-ttu-id="5deb5-203">次のオプションを使用できる、`libman uninstall`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-204">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-204">Examples</span></span>

<span data-ttu-id="5deb5-205">次を考慮*libman.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="5deb5-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="5deb5-206">次のコマンドのいずれかの成功を jQuery をアンインストールするには。</span><span class="sxs-lookup"><span data-stu-id="5deb5-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="5deb5-207">Lodash パッケージ ファイルを使用してインストールをアンインストールする、`filesystem`プロバイダー。</span><span class="sxs-lookup"><span data-stu-id="5deb5-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="5deb5-208">ライブラリのバージョンを更新します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-208">Update library version</span></span>

<span data-ttu-id="5deb5-209">`libman update`コマンドは、指定されたバージョンに LibMan 経由でインストールされているライブラリを更新します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="5deb5-210">エラーが発生した場合。</span><span class="sxs-lookup"><span data-stu-id="5deb5-210">An error occurs when:</span></span>

* <span data-ttu-id="5deb5-211">いいえ*libman.json*ファイル、プロジェクトのルートに存在します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="5deb5-212">指定したライブラリが存在しません。</span><span class="sxs-lookup"><span data-stu-id="5deb5-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="5deb5-213">同じ名前の 1 つ以上のライブラリがインストールされている場合は、いずれかを選択するように求められます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-214">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5deb5-215">引数</span><span class="sxs-lookup"><span data-stu-id="5deb5-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="5deb5-216">更新するライブラリの名前。</span><span class="sxs-lookup"><span data-stu-id="5deb5-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="5deb5-217">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-217">Options</span></span>

<span data-ttu-id="5deb5-218">次のオプションを使用できる、`libman update`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="5deb5-219">ライブラリの最新のプレリリース バージョンを取得します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="5deb5-220">ライブラリの特定のバージョンを取得します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-221">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-221">Examples</span></span>

* <span data-ttu-id="5deb5-222">JQuery を最新バージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="5deb5-223">バージョン 3.3.1 に jQuery を更新します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="5deb5-224">JQuery を最新のプレリリース バージョンに更新します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="5deb5-225">ライブラリのキャッシュを管理します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-225">Manage library cache</span></span>

<span data-ttu-id="5deb5-226">`libman cache` LibMan ライブラリのキャッシュを管理するコマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="5deb5-227">`filesystem`プロバイダーは、ライブラリのキャッシュを使用しません。</span><span class="sxs-lookup"><span data-stu-id="5deb5-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="5deb5-228">構文</span><span class="sxs-lookup"><span data-stu-id="5deb5-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="5deb5-229">引数</span><span class="sxs-lookup"><span data-stu-id="5deb5-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="5deb5-230">のみ使用、`clean`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-230">Only used with the `clean` command.</span></span> <span data-ttu-id="5deb5-231">クリーニングするプロバイダー キャッシュを指定します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="5deb5-232">有効な値を次に示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="5deb5-233">オプション</span><span class="sxs-lookup"><span data-stu-id="5deb5-233">Options</span></span>

<span data-ttu-id="5deb5-234">次のオプションを使用できる、`libman cache`コマンド。</span><span class="sxs-lookup"><span data-stu-id="5deb5-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="5deb5-235">キャッシュされているファイルの名前を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="5deb5-236">キャッシュされているライブラリの名前を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="5deb5-237">使用例</span><span class="sxs-lookup"><span data-stu-id="5deb5-237">Examples</span></span>

* <span data-ttu-id="5deb5-238">プロバイダーごとのキャッシュされたライブラリの名前を表示するには、次のコマンドのいずれかを使用します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="5deb5-239">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="5deb5-240">プロバイダーごとにキャッシュされたライブラリ ファイルの名前を表示するには。</span><span class="sxs-lookup"><span data-stu-id="5deb5-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="5deb5-241">次のような出力が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="5deb5-242">上記の出力に注意してください CDNJS プロバイダー バージョン 3.2.1 および 3.3.1 はキャッシュする jQuery を示します。</span><span class="sxs-lookup"><span data-stu-id="5deb5-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="5deb5-243">CDNJS プロバイダーのライブラリのキャッシュを空にします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="5deb5-244">CDNJS プロバイダー キャッシュを空にした、`libman cache list`コマンドには、次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="5deb5-245">すべてのキャッシュを空にするには、プロバイダーにサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5deb5-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="5deb5-246">プロバイダーのすべてのキャッシュを空にした、`libman cache list`コマンドには、次が表示されます。</span><span class="sxs-lookup"><span data-stu-id="5deb5-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="5deb5-247">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="5deb5-247">Additional resources</span></span>

* [<span data-ttu-id="5deb5-248">グローバル ツールをインストールします。</span><span class="sxs-lookup"><span data-stu-id="5deb5-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="5deb5-249">LibMan の GitHub リポジトリ</span><span class="sxs-lookup"><span data-stu-id="5deb5-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
