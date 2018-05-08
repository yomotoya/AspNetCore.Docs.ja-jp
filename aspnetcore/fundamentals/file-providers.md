---
title: ASP.NET Core でのファイル プロバイダー
author: ardalis
description: ASP.NET Core がファイル プロバイダーを使用してファイル システムへのアクセスを抽象化する方法について説明します。
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/file-providers
ms.openlocfilehash: cdbffdadd9616fe941809d67dc2c0bbd52149561
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2018
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core でのファイル プロバイダー

作成者: [Steve Smith](https://ardalis.com/)

ASP.NET Core は、ファイル プロバイダーを使用してファイル システムへのアクセスを抽象化します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="file-provider-abstractions"></a>ファイル プロバイダー抽象化

ファイル プロバイダーは、ファイル システムに対する抽象化です。 メイン インターフェイスは `IFileProvider` です。 `IFileProvider` はファイル情報を取得するメソッド (`IFileInfo`)、ディレクトリ情報を取得するメソッド (`IDirectoryContents`) を公開し、変更通知を設定します (`IChangeToken` を使用)。

`IFileInfo` は個々のファイルまたはディレクトリに関するメソッドおよびプロパティを提供します。 そのプロパティとしては、2 つのブール型プロパティ (`Exists` および `IsDirectory`) と、ファイルの名前 (`Name`)、バイト単位でのファイルの長さ (`Length`)、および日付 (`LastModified`) を示すプロパティがあります。 この `CreateReadStream` メソッドを使用することで、ファイルから情報を読み取ることができます。

## <a name="file-provider-implementations"></a>ファイル プロバイダーの実装

`IFileProvider` の実装には、物理、埋め込み、およびコンポジットの 3 つがあります。 物理プロバイダーは、実際のシステム ファイルにアクセスする場合に使用します。 埋め込みプロバイダーは、アセンブリに埋め込まれているファイルにアクセスする場合に使用します。 コンポジット プロパイダーは、その他の 1 つまたは複数のプロバイダーからのファイルおよびディレクトリに対するアクセスを結合する場合に使用されます。

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider` は、物理ファイル システムへのアクセス許可を提供します。 これは、`System.IO.File` 型をラップし (物理プロバイダーの場合)、ディレクトリとその子へのすべてのパスのスコープを設定します。 このスコープ設定により、特定のディレクトリとその子へのアクセスが制限され、この境界の外側でのファイル システムへのアクセスが防止されます。 このプロバイダーをインスタンス化する場合は、ディレクトリ パスを指定する必要があります。ディレクトリ パスは、このプロバイダーに送られるすべての要求のベース パスとして機能します (また、このパス以外からのアクセスを制限します)。 ASP.NET Core アプリでは、`PhysicalFileProvider` プロバイダーを直接インスタンス化することも、[依存関係挿入](dependency-injection.md)を介してコントローラーまたはサーバーのコンストラクター内で `IFileProvider` を要求することもできます。 後者のアプローチでは、通常、柔軟でテストが容易なソリューションがもたらされます。

次のサンプルは、`PhysicalFileProvider` の作成方法を示します。


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

そのディレクトリの内容を繰り返し処理したり、サブパスを提供することによって、特定のファイルの情報を取得できます。

コントローラーからプロバイダーを要求するには、コントローラーのコンストラクター内でプロバイダーを指定し、ローカル フィールドに割り当てます。 アクション メソッドからのローカル インスタンスを使用します。

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

次に、アプリの `Startup` クラスでプロバイダーを作成します。

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

*Index.cshtml* ビューで、指定された `IDirectoryContents` を繰り返します。

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

結果は次のとおりです。

![物理ファイルおよびフォルダーを一覧したファイル プロバイダー サンプル アプリケーション](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider` は、アセンブリに埋め込まれているファイルにアクセスする場合に使用します。 .NET Core では、*.csproj* ファイルの `<EmbeddedResource>` 要素を使用してアセンブリにファイルを埋め込みます。

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

アセンブリに埋め込むファイルを指定する場合は、[glob パターン](#globbing-patterns)を使用できます。 これらのパターンを使用すれば、1 つまたは複数のファイルを照合することができます。

> [!NOTE]
> ほとんどの場合、プロジェクト内のすべての .js ファイルを、そのアセンブリに実際に埋め込むことはありません。上記のサンプルは、デモのみを目的としています。

`EmbeddedFileProvider` を作成する場合、それが読み取るアセンブリをそのコンストラクターに渡します。

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

上記のスニペットは、現在実行中のアセンブリへのアクセス許可を使用して `EmbeddedFileProvider` を作成する方法を示しています。

`EmbeddedFileProvider` を使用するサンプル アプリを更新すると、次の出力が生成されます。

![埋め込みファイルを一覧表示するファイル プロバイダー サンプル アプリケーション](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> 埋め込みリソースではディレクトリは公開されません。 その代わり、リソースへのパス (名前空間を経由する) は、`.` 区切り記号を使用して、リソースのファイル名に埋め込まれます。

> [!TIP]
> `EmbeddedFileProvider` では、`baseNamespace` という省略可能なパラメーターも受け入れます。 このパラメーターを指定すると、`GetDirectoryContents` の呼び出しが、指定した名前空間の下にあるリソースにスコープされます。

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider` は、`IFileProvider` インスタンスを結合し、複数のプロバイダーからのファイルを操作するための 1 つのインターフェイスを公開します。 `CompositeFileProvider` を作成する場合、1 つまたは複数の `IFileProvider` インスタンスをコンストラクターに渡します。

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

以前に構成した物理プロバイダーと埋め込みプロバイダーの両方を含んだ `CompositeFileProvider` を使用するようにサンプル アプリを構成すると、次の出力が生成されます。

![物理ファイル/フォルダーと埋め込みファイルを両方とも一覧表示しているファイル プロバイダー サンプル アプリケーション](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>変更の監視

`IFileProvider` `Watch` メソッドは、変更がないかどうか 1 つ以上のファイルまたはディレクトリを監視する方法を提供します。 このメソッドは、パス文字列 ([glob パターン](#globbing-patterns)を使用して複数のファイルを指定することができる) を受け取り、`IChangeToken` を返します。 このトークンは、検査可能な `HasChanged` プロパティと、変更が検出されたときに呼び出される `RegisterChangeCallback` メソッドを、指定したパス文字列に公開します。 各変更トークンは、単一の変更に応じて、関連付けられたコールバックを呼び出すのみとなります。 定数の監視を有効に使用するには、次に示すように `TaskCompletionSource` を使用するか、変更に応じて `IChangeToken` インスタンスを再作成します。

この記事のサンプルでは、テキスト ファイルが変更されるたびにメッセージを表示するようにコンソール アプリケーションが構成されています。

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

ファイルを複数回保存した後の結果:

![dotnet run を実行した後のコマンド ウィンドウであり、変更がないかどうか quotes.txt ファイルを監視するアプリケーションを示しています。ファイルは 5 回変更されています。](file-providers/_static/watch-console.png)

> [!NOTE]
> Docker コンテナーやネットワーク共有など、一部のファイル システムは、変更通知を確実に送信しない可能性があります。 `DOTNET_USE_POLLINGFILEWATCHER` 環境変数を `1` または `true` に設定することで、4 秒ごとにファイル システムをポーリングして変更がないかどうか確認します。

## <a name="globbing-patterns"></a>glob パターン

ファイル システム パスは、"*glob パターン*" と呼ばれるワイルドカード パターンを使用します。 このような単純なパターンを使用すれば、ファイルのグループを指定できます。 2 つのワイルドカード文字は、`*` と `**` です。

**`*`**

   現在のフォルダー レベルにある任意の要素、または任意のファイル名、または任意のファイル拡張子を照合します。 照合はファイル パス内の `/` 文字および `.` 文字によって終了します。

<strong><code>**</code></strong>

   複数のディレクトリ レベルにわたって任意の要素を照合します。 ディレクトリ階層内の多数のファイルを再帰的に照合する場合に使用できます。

### <a name="globbing-pattern-examples"></a>glob パターンの例

**`directory/file.txt`**

   特定のディレクトリ内の特定のファイルを照合します。

**<code>directory/*.txt</code>**

   特定のディレクトリ内の `.txt` 拡張子を持つすべてのファイルを照合します。

**`directory/*/bower.json`**

   `directory` ディレクトリのちょうど 1 つ下のレベルにあるディレクトリ内のすべての `bower.json` ファイルを照合します。

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   `directory` ディレクトリの下の任意の場所で見つかった `.txt` 拡張子を持つすべてのファイルを照合します。

## <a name="file-provider-usage-in-aspnet-core"></a>ASP.NET Core でのファイル プロバイダーの使用

ASP.NET Core の複数の部分でファイルのプロバイダーが利用されます。 `IHostingEnvironment` では、アプリのコンテンツ ルートと Web ルートを `IFileProvider` 型として公開します。 静的ファイル ミドルウェアでは、ファイル プロバイダーを使用して静的なファイルを見つけます。 Razor では、ビューを検索する際に `IFileProvider` を多用します。 Dotnet の公開機能では、ファイル プロバイダーと glob パターンを使用して公開する必要があるファイルを指定します。

## <a name="recommendations-for-use-in-apps"></a>アプリで使用する場合の推奨事項

ASP.NET Core アプリでファイル システムへのアクセスを必要とする場合は、サンプルに示されているように、依存関係挿入を通して `IFileProvider` のインスタンスを要求し、そのメソッドを使用してアクセスを実行することができます。 この場合、アプリが起動し、ご使用のアプリによってインスタンス化された実装の種類の数が削減されたときに、プロバイダーを 1 回構成することができます。
