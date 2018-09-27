---
title: ASP.NET Core で LibMan コマンド ライン インターフェイス (CLI) を使用します。
author: scottaddie
description: ASP.NET Core プロジェクトで LibMan コマンド ライン インターフェイス (CLI) を使用する方法について説明します。
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: 5667f79648a60b8fd9496f8041ef08891ab766af
ms.sourcegitcommit: 9bdba90b2c97a4016188434657194b2d7027d6e3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2018
ms.locfileid: "47402086"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a>ASP.NET Core で LibMan コマンド ライン インターフェイス (CLI) を使用します。

作成者: [Scott Addie](https://twitter.com/Scott_Addie)

[LibMan](xref:client-side/libman/index) CLI は .NET Core がサポートされているすべての場所がサポートされるクロスプラット フォーム ツールです。

## <a name="prerequisites"></a>必須コンポーネント

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a>インストール

LibMan CLI のインストール。

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

A [.NET Core のグローバル ツール](/dotnet/core/tools/global-tools#install-a-global-tool)からインストールされている、 [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet パッケージ。

特定の NuGet パッケージのソースから LibMan CLI をインストールします。

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

ローカルの Windows コンピューターの前の例では、.NET Core のグローバル ツールがインストールされている*C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg*ファイル。

## <a name="usage"></a>使用法

CLI のインストールの成功後、次のコマンドを使用できます。

```console
libman
```

インストールされている CLI バージョンを表示します。

```console
libman --version
```

使用可能な CLI コマンドを表示するには。

```console
libman --help
```

上記のコマンドには、次のような出力が表示されます。

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

次のセクションでは、使用可能な CLI コマンドを説明します。

## <a name="initialize-libman-in-the-project"></a>LibMan をプロジェクトを初期化します。

`libman init`コマンドは、作成、 *libman.json*ファイルのいずれかが存在しない場合。 既定の項目テンプレートのコンテンツ ファイルが作成されます。

### <a name="synopsis"></a>構文

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a>オプション

次のオプションを使用できる、`libman init`コマンド。

* `-d|--default-destination <PATH>`

  現在のフォルダーの相対パス。 ライブラリ ファイルがこの場所にインストールされていない場合`destination`のライブラリのプロパティが定義されている*libman.json*します。 `<PATH>`に値が書き込まれる、`defaultDestination`プロパティの*libman.json*します。

* `-p|--default-provider <PROVIDER>`

  指定したライブラリのプロバイダーが定義されていない場合に使用するプロバイダー。 `<PROVIDER>`に値が書き込まれる、`defaultProvider`プロパティの*libman.json*します。 置換`<PROVIDER>`次の値のいずれかの。

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

作成する、 *libman.json* ASP.NET Core プロジェクト ファイル。

* プロジェクトのルートに移動します。
* 次のコマンドを実行します。

  ```console
  libman init
  ```

* キーを押して、既定のプロバイダーの名前を入力`Enter`CDNJS の既定のプロバイダーを使用します。 有効な値を次に示します。

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![libman init コマンドの既定のプロバイダー](_static/libman-init-provider.png)

A *libman.json*ファイル、プロジェクトのルート以下の内容に追加されます。

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a>ライブラリ ファイルを追加します。

`libman install`コマンドは、ダウンロードし、プロジェクトにライブラリ ファイルをインストールします。 A *libman.json*ファイルが存在していない場合に追加されます。 *Libman.json*ライブラリ ファイルの構成の詳細を格納するファイルを変更します。

### <a name="synopsis"></a>構文

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a>引数

`LIBRARY`

インストールするためにライブラリの名前。 この名前は、バージョン番号の表記法を含めることができます (たとえば、 `@1.2.0`)。

### <a name="options"></a>オプション

次のオプションを使用できる、`libman install`コマンド。

* `-d|--destination <PATH>`

  ライブラリをインストールする場所。 指定しない場合、既定の場所が使用されます。 ない場合は`defaultDestination`プロパティが指定されて*libman.json*、このオプションが必要です。

* `--files <FILE>`

  ライブラリからインストールするファイルの名前を指定します。 指定しない場合、ライブラリからすべてのファイルがインストールされます。 1 つ提供`--files`ファイルごとのオプションをインストールします。 相対パスはもサポートします。 たとえば、`--files dist/browser/signalr.js` のように指定します。

* `-p|--provider <PROVIDER>`

  ライブラリの取得を使用するプロバイダーの名前。 置換`<PROVIDER>`次の値のいずれかの。
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  指定しない場合、`defaultProvider`プロパティ*libman.json*使用されます。 ない場合は`defaultProvider`プロパティが指定されて*libman.json*、このオプションが必要です。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

次を考慮*libman.json*ファイル。

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

3.2.1 jQuery バージョンをインストールする*jquery.min.js*ファイルを*jqueryスクリプト/wwwroot/* CDNJS プロバイダーを使用してフォルダー。

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot/scripts/jquery --files jquery.min.js
```

*Libman.json*ファイルが、次に似ています。

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

インストールする、 *calendar.js*と*calendar.css*ファイルから*c:\\temp\\contosoCalendar\\* ファイル システムを使用します。プロバイダー:

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

次のプロンプトは、2 つの理由が表示されます。

* *Libman.json*ファイルが含まれていない、`defaultDestination`プロパティ。
* `libman install`コマンドが含まれていない、`-d|--destination`オプション。

![libman インストール コマンド - 変換先](_static/libman-install-destination.png)

既定の転送先に同意した後、 *libman.json*ファイルが、次に似ています。

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot/scripts/jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot/lib/contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a>ライブラリ ファイルを復元します。

`libman restore`コマンドで定義されているライブラリ ファイルはインストール*libman.json*します。 次の規則が適用されます。

* ない場合は*libman.json*プロジェクトのルートにファイルが存在するエラーが返されます。
* ライブラリは、プロバイダーを指定する場合、`defaultProvider`プロパティ*libman.json*は無視されます。
* ライブラリは、変換先を指定する場合、`defaultDestination`プロパティ*libman.json*は無視されます。

### <a name="synopsis"></a>構文

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a>オプション

次のオプションを使用できる、`libman restore`コマンド。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

定義されているライブラリ ファイルを復元する*libman.json*:

```console
libman restore
```

## <a name="delete-library-files"></a>ライブラリ ファイルを削除します。

`libman clean`コマンド LibMan を使用して以前に復元されたライブラリ ファイルを削除します。 この操作の後に空になったフォルダーは削除されます。 構成に関連するライブラリ ファイルの`libraries`プロパティの*libman.json*は削除されません。

### <a name="synopsis"></a>構文

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a>オプション

次のオプションを使用できる、`libman clean`コマンド。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

LibMan 経由でインストールされているライブラリ ファイルを削除します。

```console
libman clean
```

## <a name="uninstall-library-files"></a>ライブラリ ファイルをアンインストールします。

`libman uninstall`コマンド。

* 変換先から、指定したライブラリに関連付けられているすべてのファイルを削除します。 *libman.json*します。
* 関連するライブラリの構成を削除します*libman.json*します。

エラーが発生した場合。

* いいえ*libman.json*ファイル、プロジェクトのルートに存在します。
* 指定したライブラリが存在しません。

同じ名前の 1 つ以上のライブラリがインストールされている場合は、いずれかを選択するように求められます。

### <a name="synopsis"></a>構文

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a>引数

`LIBRARY`

アンインストールするライブラリの名前。 この名前は、バージョン番号の表記法を含めることができます (たとえば、 `@1.2.0`)。

### <a name="options"></a>オプション

次のオプションを使用できる、`libman uninstall`コマンド。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

次を考慮*libman.json*ファイル。

[!code-json[](samples/LibManSample/libman.json)]

* 次のコマンドのいずれかの成功を jQuery をアンインストールするには。

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* Lodash パッケージ ファイルを使用してインストールをアンインストールする、`filesystem`プロバイダー。

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a>ライブラリのバージョンを更新します。

`libman update`コマンドは、指定されたバージョンに LibMan 経由でインストールされているライブラリを更新します。

エラーが発生した場合。

* いいえ*libman.json*ファイル、プロジェクトのルートに存在します。
* 指定したライブラリが存在しません。

同じ名前の 1 つ以上のライブラリがインストールされている場合は、いずれかを選択するように求められます。

### <a name="synopsis"></a>構文

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a>引数

`LIBRARY`

更新するライブラリの名前。

### <a name="options"></a>オプション

次のオプションを使用できる、`libman update`コマンド。

* `-pre`

  ライブラリの最新のプレリリース バージョンを取得します。

* `--to <VERSION>`

  ライブラリの特定のバージョンを取得します。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

* JQuery を最新バージョンに更新します。

  ```console
  libman update jquery
  ```

* バージョン 3.3.1 に jQuery を更新します。

  ```console
  libman update jquery --to 3.3.1
  ```

* JQuery を最新のプレリリース バージョンに更新します。

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a>ライブラリのキャッシュを管理します。

`libman cache` LibMan ライブラリのキャッシュを管理するコマンド。 `filesystem`プロバイダーは、ライブラリのキャッシュを使用しません。

### <a name="synopsis"></a>構文

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a>引数

`PROVIDER`

のみ使用、`clean`コマンド。 クリーニングするプロバイダー キャッシュを指定します。 有効な値を次に示します。

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a>オプション

次のオプションを使用できる、`libman cache`コマンド。

* `--files`

  キャッシュされているファイルの名前を一覧表示します。

* `--libraries`

  キャッシュされているライブラリの名前を一覧表示します。

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a>使用例

* プロバイダーごとのキャッシュされたライブラリの名前を表示するには、次のコマンドのいずれかを使用します。

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  次のような出力が表示されます。

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

* プロバイダーごとにキャッシュされたライブラリ ファイルの名前を表示するには。

  ```console
  libman cache list --files
  ```

  次のような出力が表示されます。

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

  上記の出力に注意してください CDNJS プロバイダー バージョン 3.2.1 および 3.3.1 はキャッシュする jQuery を示します。

* CDNJS プロバイダーのライブラリのキャッシュを空にします。

  ```console
  libman cache clean cdnjs
  ```

  CDNJS プロバイダー キャッシュを空にした、`libman cache list`コマンドには、次が表示されます。

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

* すべてのキャッシュを空にするには、プロバイダーにサポートされています。

  ```console
  libman cache clean
  ```

  プロバイダーのすべてのキャッシュを空にした、`libman cache list`コマンドには、次が表示されます。

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a>その他の技術情報

* [グローバル ツールをインストールします。](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [LibMan の GitHub リポジトリ](https://github.com/aspnet/LibraryManager)
