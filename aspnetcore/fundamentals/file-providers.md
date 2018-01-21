---
title: "ASP.NET Core ファイル プロバイダー"
author: ardalis
description: "ASP.NET Core ファイル プロバイダーを使用してファイル システムへのアクセスを抽象化する方法について説明します。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: db207f19b7ddc24dea36009138840be6efebdb84
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="file-providers-in-aspnet-core"></a>ASP.NET Core ファイル プロバイダー

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core は、ファイルのプロバイダーを使用してファイル システムへのアクセスを抽象化します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="file-provider-abstractions"></a>ファイル プロバイダーの抽象化

ファイル プロバイダーは、ファイル システム経由で抽象化です。 メイン インターフェイスは`IFileProvider`します。 `IFileProvider`ファイル情報を取得するメソッドを公開 (`IFileInfo`)、ディレクトリ情報 (`IDirectoryContents`)、変更通知を設定して (を使用して、 `IChangeToken`)。

`IFileInfo`メソッドと個々 のファイルまたはディレクトリのプロパティを提供します。 2 つのブール型プロパティがある`Exists`と`IsDirectory`、ファイルの説明するプロパティだけでなく`Name`、 `Length` (単位: バイト)、および`LastModified`日付。 使用してファイルから読み取ることができます、`CreateReadStream`メソッドです。

## <a name="file-provider-implementations"></a>ファイル プロバイダーの実装

3 つの実装の`IFileProvider`を利用できます: 物理、埋め込み、および複合です。 実際のシステムのファイルにアクセスするには、物理プロバイダーが使用されます。 アセンブリに埋め込まれているファイルにアクセスするには、埋め込みのプロバイダーが使用します。 その他の 1 つまたは複数のプロバイダーからファイルとディレクトリに統合されたアクセスを提供する複合プロバイダーが使用されます。

### <a name="physicalfileprovider"></a>PhysicalFileProvider

`PhysicalFileProvider`物理ファイル システムへのアクセスを提供します。 それがラップ、`System.IO.File`型 (物理プロバイダー) のすべてのパスのディレクトリとその子のスコープを設定します。 このスコープの設定と、特定のディレクトリとその子は、この境界の外側で、ファイル システムへのアクセスを防止するアクセスが制限されます。 このプロバイダーをインスタンス化するときに、このプロバイダに行われたすべての要求のベース パス (および、このパスの外部アクセスを制限する) として機能するため、ディレクトリのパスに提供する必要があります。 アプリケーションでは ASP.NET Core、インスタンス化できる、`PhysicalFileProvider`プロバイダーを直接、またはを要求できます、`IFileProvider`を通じてサービスのコンス トラクターまたはコント ローラーで[依存性の注入](dependency-injection.md)です。 後者のアプローチには、通常より柔軟なテストが容易なソリューションが生成されます。

以下のサンプルを作成する方法を示しています、`PhysicalFileProvider`です。


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

そのディレクトリの内容を繰り返し処理したり、サブパスを提供することによって、特定のファイルの情報を取得できます。

コント ローラーから、プロバイダーを要求するには、コント ローラーのコンス トラクターで指定し、ローカル フィールドを割り当てます。 アクション メソッドからのローカル インスタンスを使用します。

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

その結果、アプリのプロバイダーを作成`Startup`クラス。

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

*Index.cshtml*を反復処理、表示、`IDirectoryContents`指定します。

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

結果:

![ファイル プロバイダー サンプル アプリケーションの一覧の物理ファイルとフォルダー](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a>EmbeddedFileProvider

`EmbeddedFileProvider`アセンブリに埋め込まれているファイルにアクセスするために使用します。 .NET Core でのアセンブリにファイルを埋め込む、`<EmbeddedResource>`内の要素、 *.csproj*ファイル。

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

使用することができます[グロブ パターン](#globbing-patterns)アセンブリに埋め込むファイルを指定する場合。 これらのパターンは、1 つまたは複数のファイルの一致するように使用できます。

> [!NOTE]
> 実際には、アセンブリでプロジェクトのすべての .js ファイルを埋め込むたい可能性はありません。上記のサンプルでは、デモの目的でのみです。

作成するときに、 `EmbeddedFileProvider`、読み取り、コンス トラクターにアセンブリを渡します。

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

上記のスニペットは、作成する方法を示します、`EmbeddedFileProvider`現在実行中のアセンブリへのアクセス権を持つ。

使用するサンプル アプリを更新、`EmbeddedFileProvider`次の出力結果します。

![ファイル プロバイダーのサンプル アプリケーションが埋め込みファイルを一覧表示します。](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> 埋め込みリソースでは、ディレクトリが公開されません。 (その名前空間) 経由でリソースへのパスがファイル名を使用して、埋め込まれているではなく、`.`区切り記号。

> [!TIP]
> `EmbeddedFileProvider`コンス トラクターは、省略可能な受け取ります`baseNamespace`パラメーター。 呼び出しのスコープはこれを指定する`GetDirectoryContents`指定された名前空間の下には、そのリソースにします。

### <a name="compositefileprovider"></a>CompositeFileProvider

`CompositeFileProvider`結合`IFileProvider`インスタンス、複数のプロバイダーからのファイルを操作するための 1 つのインターフェイスを公開します。 作成するときに、 `CompositeFileProvider`、1 つまたは複数を渡す`IFileProvider`コンス トラクターにインスタンス。

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

使用するサンプル アプリを更新、`CompositeFileProvider`を以前に構成されている物理と埋め込みプロバイダー両方にはが含まれています、次の出力結果します。

![ファイル プロバイダー サンプル アプリケーションの物理ファイルおよびフォルダーと埋め込みファイルの両方を一覧表示します。](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a>変更の監視

`IFileProvider` `Watch`メソッドが 1 つまたは複数のファイルまたはディレクトリの変更を監視する方法を提供します。 このメソッドが使用できるパス文字列を受け取ります[グロブ パターン](#globbing-patterns)複数のファイル、および返しますを指定する、`IChangeToken`です。 このトークンを公開、`HasChanged`検査できる、プロパティ、および`RegisterChangeCallback`指定したパス文字列に対する変更が検出されたときに呼び出されるメソッド。 1 つの変更への応答で、関連付けられたコールバックを各変更トークンがのみが呼び出すことに注意してください。 定数の監視を有効に使用することができます、 `TaskCompletionSource` 、次のように作成または再`IChangeToken`インスタンスの変更に応答します。

この記事のサンプルでは、テキスト ファイルが変更されるたびにメッセージを表示するコンソール アプリケーションを構成します。

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

複数回、ファイルを保存した後の結果:

![Quotes.txt ファイルの変更を監視するアプリケーションを示しています実行 dotnet を実行し、ファイルは 5 回変更された後は、コマンド ウィンドウをします。](file-providers/_static/watch-console.png)

> [!NOTE]
> Docker コンテナー、ネットワーク共有など、一部のファイル システムは、変更通知を確実に送信いない可能性があります。 設定、`DOTNET_USE_POLLINGFILEWATCHER`環境変数を`1`または`true`4 秒ごとに、ファイル システムの変更をポーリングします。

## <a name="globbing-patterns"></a>グロブ パターン

ファイル システム パスと呼ばれるワイルドカード パターンを使用して*グロブ パターン*です。 ファイルのグループを指定するのには、これらの単純なパターンを使用できます。 2 つのワイルドカード文字は`*`と`**`です。

**`*`**

   現在のフォルダー レベル、または任意のファイル名、または任意のファイル拡張子以外に一致します。 一致がによって終了`/`と`.`ファイルのパス内の文字です。

<strong><code>**</code></strong>

   複数のディレクトリ レベルで何かと一致します。 再帰的に使用できるディレクトリ階層内の多数のファイルと一致します。

### <a name="globbing-pattern-examples"></a>グロブ パターンの例

**`directory/file.txt`**

   特定のディレクトリ内の特定のファイルと一致します。

**<code>directory/*.txt</code>**

   一致するすべてのファイルが`.txt`特定のディレクトリに拡張します。

**`directory/*/bower.json`**

   すべてを満たす`bower.json`下の 正確に 1 レベルのディレクトリ内のファイル、`directory`ディレクトリ。

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   一致するすべてのファイルが`.txt`拡張機能は、下にある任意の場所が見つかりません、`directory`ディレクトリ。

## <a name="file-provider-usage-in-aspnet-core"></a>ファイルの ASP.NET Core でプロバイダーの使用

ASP.NET Core のパーツがいくつかは、ファイルのプロバイダーを利用します。 `IHostingEnvironment`アプリのコンテンツのルートととして web ルート公開`IFileProvider`型です。 静的ファイル ミドルウェアでは、ファイルのプロバイダーを使用して、静的なファイルを見つけます。 Razor 多用`IFileProvider`ビューを検索します。 Dotnet のでは、どのファイルをパブリッシュする必要がありますを指定するには、使用してファイルのプロバイダーの機能とグロブ パターンを発行します。

## <a name="recommendations-for-use-in-apps"></a>アプリで使用するための推奨事項

ASP.NET Core アプリケーションは、ファイル システムへのアクセスを必要とする場合は、インスタンスを要求することができます`IFileProvider`依存関係の挿入を使用してこのサンプルで示すように、アクセスを実行するメソッドを使用します。 これにより、アプリの起動、および実装の種類のアプリをインスタンス化の数が減る場合に、1 回、プロバイダーを構成することができます。
