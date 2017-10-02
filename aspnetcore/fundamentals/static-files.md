---
title: "ASP.NET Core での静的ファイルの操作"
author: rick-anderson
description: "ASP.NET Core の静的ファイルを操作する方法を説明します。"
keywords: "ASP.NET Core、静的なファイル、HTML、CSS、JavaScript の静的なアセット"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e099c4767958f153134e0fb6b3de8132ab1ead82
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a>ASP.NET Core での静的ファイルの操作

<a name=fundamentals-static-files></a>

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

HTML、CSS、画像、および、JavaScript などの静的ファイルは、ASP.NET Core アプリケーションが直接クライアントに使用できる資産です。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))

## <a name="serving-static-files"></a>静的ファイルの提供

静的ファイルにある、 `web root` (*\<コンテンツ ルート >/wwwroot*) フォルダー。 参照してください[コンテンツのルート](xref:fundamentals/index#content-root)と[Web ルート](xref:fundamentals/index#web-root)詳細についてはします。 通常、現在のディレクトリのようにコンテンツのルートを設定できるように、プロジェクトの`web root`開発中に検出されます。

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

静的なファイルは任意のフォルダーの下に置くことができます、`web root`し、そのルートへの相対パスを使用してアクセスします。 たとえば、Visual Studio を使用して既定の Web アプリケーション プロジェクトを作成する場合があるいくつかのフォルダー内に作成された、 *wwwroot*フォルダー - *css*、*イメージ*、および*js*です。 イメージにアクセスする URI、*イメージ*サブフォルダー。

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

静的なファイルを提供するためには、構成する必要があります、[ミドルウェア](middleware.md)をパイプラインに静的なファイルを追加します。 依存関係を追加することによって静的ファイル ミドルウェアを構成することができます、 *Microsoft.AspNetCore.StaticFiles*パッケージ、プロジェクトと、呼び出し元に、`UseStaticFiles`から拡張メソッド`Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`内のファイルは、 `web root` (*wwwroot*既定で) 含んだです。 後で説明するその他のディレクトリの内容を含んだ方法`UseStaticFiles`です。

NuGet パッケージ"Microsoft.AspNetCore.StaticFiles"を含める必要があります。

> [!NOTE]
> `web root`既定値は、 *wwwroot*がディレクトリを設定できる、`web root`ディレクトリが`UseWebRoot`です。

外部サービスを提供する静的ファイルがプロジェクトの階層があると、`web root`です。 例:

* wwwroot
  * css
  * イメージ
  * ...
* MyStaticFiles
  * test.png

アクセス要求に対して*test.png*、静的ファイル ミドルウェアを次のように構成します。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

要求を`http://<app>/StaticFiles/test.png`果たす、 *test.png*ファイル。

`StaticFileOptions()`応答ヘッダーを設定できます。 たとえば、次のコード設定静的ファイルの提供から、 *wwwroot*フォルダーと設定、 `Cache-Control` 10 分 (600 秒) をパブリックにキャッシュできるようにするヘッダー。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Cache-control ヘッダーを示す応答ヘッダーが追加されました](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>静的ファイルの承認

静的ファイル モジュールでは、**ありません**承認チェックします。 すべてのファイル サービスを提供し、下にあるものを含む*wwwroot*は一般に公開します。 ファイルを提供するには、承認に基づいています。

* 外部に保存する*wwwroot*と静的ファイル ミドルウェアにアクセスできる任意のディレクトリ**と**

* 返す、コント ローラーのアクションを提供する`FileResult`承認が適用されています。

## <a name="enabling-directory-browsing"></a>ディレクトリの参照を有効にします。

ディレクトリの参照ディレクトリおよび指定したディレクトリ内のファイルの一覧を表示する web アプリのユーザーに許可します。 既定ではセキュリティ上の理由からディレクトリの参照が無効になっています (を参照してください[に関する考慮事項](#considerations))。 ディレクトリの参照を有効にするを呼び出して、`UseDirectoryBrowser`から拡張メソッド`Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

必要なサービスを呼び出すことによって追加および`AddDirectoryBrowser`から拡張メソッド`Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

上記のコードにより、ディレクトリの参照の*wwwroot/イメージ*URL http:// を使用してフォルダー\<アプリ >/により、各ファイルおよびフォルダーへのリンク。

![ディレクトリの参照](static-files/_static/dir-browse.png)

参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。

2 つに注意してください`app.UseStaticFiles`呼び出しです。 最初の 1 つが、CSS、画像とでの JavaScript を処理に必要な*wwwroot*フォルダー、およびディレクトリの参照のための 2 番目の呼び出し、 *wwwroot/イメージ*URL http:// を使用してフォルダー\<アプリ>/により。

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>既定のドキュメントを提供しています。

サイトの訪問者にサイトを訪問するときに開始する場所は、既定のホーム ページを設定できます。 Web アプリをユーザーが、URI の完全修飾する必要がなく、既定のページを処理するためには、呼び出し、`UseDefaultFiles`から拡張メソッド`Startup.Configure`次のようにします。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`前に呼び出す必要があります`UseStaticFiles`を既定のファイルを提供します。 `UseDefaultFiles`実際には、ファイルに貢献しない URL 再ライターです。 静的ファイル ミドルウェアを有効にする必要があります (`UseStaticFiles`) ファイルを提供します。

`UseDefaultFiles`フォルダーへの要求が検索されます。

* default.htm
* default.html
* index.htm
* index.html

要求が (ブラウザーの URL は、要求された URI を表示し続けます) には、完全修飾 URI が場合と、見つかった最初のファイルの一覧からに提供します。

次のコードを既定のファイル名を変更する方法を示しています。 *mydefault.html*です。

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`機能を結合`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`です。

次のコードによって、静的なファイルを提供する既定のファイルはディレクトリの参照を許可しません。

```csharp
app.UseFileServer();
   ```

次のコードには、静的なファイル、既定のファイルとディレクトリの参照が有効にします。

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。 同様に`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`外部に存在するファイルを提供する場合、`web root`のインスタンスを作成して構成する、`FileServerOptions`へのパラメーターとして渡されたオブジェクト`UseFileServer`です。 たとえば、Web アプリで、次のディレクトリ階層を指定します。

* wwwroot

  * css

  * イメージ

  * ...

* MyStaticFiles

  * test.png

  * default.html

上記の階層の例を使用して、次のように静的なファイル、既定のファイルとの参照を有効にする可能性があります、`MyStaticFiles`ディレクトリ。 次のコード スニペットで 1 回の呼び出しで実現を`FileServerOptions`です。

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

場合`enableDirectoryBrowsing`に設定されている`true`呼び出しに必要な`AddDirectoryBrowser`から拡張メソッド`Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

ファイルの階層と上記のコードを使用します。

| URI            |                             応答  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

既定の名前付きのファイルがない場合、 *MyStaticFiles*ディレクトリ、http://\<アプリ >/StaticFiles がディレクトリでクリック可能なリンクの一覧を返します。

![静的ファイルの一覧](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles``UseDirectoryBrowser` url http:// かかります\<アプリ > http:// に末尾のスラッシュと原因せず StaticFiles クライアント側のリダイレクト/\<アプリ >/StaticFiles/(末尾にスラッシュを追加する)。 なし、ドキュメント内の末尾のスラッシュ相対 Url が正しくないあります。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

`FileExtensionContentTypeProvider`クラスには、ファイル拡張子を MIME コンテンツの種類にマップするコレクションが含まれています。 次のサンプルでは、既知の MIME の種類に複数のファイル拡張子が登録されている、".rtf"が置き換えられ、".mp4"が削除されます。

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

参照してください[MIME コンテンツ タイプ](http://www.iana.org/assignments/media-types/media-types.xhtml)です。

## <a name="non-standard-content-types"></a>非標準のコンテンツの種類

ASP.NET の静的ファイル ミドルウェアは、ほぼ 400 の既知のファイル コンテンツの種類を認識しています。 ユーザーは、不明なファイル種類のファイルを要求している場合、静的ファイル ミドルウェアは、HTTP 404 (Not found) 応答を返します。 ディレクトリの参照が有効になっている場合は、ファイルへのリンクが表示されますが、URI は、HTTP 404 エラーを返します。

次のコードでは、不明な型の機能を有効にし、不明なファイルをイメージとして表示されます。

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

上記のコードでは、イメージとして、不明なコンテンツの種類のファイルの要求が返されます。

>[!WARNING]
> 有効にする`ServeUnknownFileTypes`セキュリティ上のリスクは、使用することはお勧めします。  `FileExtensionContentTypeProvider`(上記で説明した) 非標準の拡張子を持つファイルを処理するより安全な代替手段を提供します。

### <a name="considerations"></a>注意事項

>[!WARNING]
> `UseDirectoryBrowser`および`UseStaticFiles`機密データがリークすることができます。 お勧めする**いない**有効にするディレクトリの実稼働環境で参照します。 注意が必要で有効にするディレクトリに関する`UseStaticFiles`または`UseDirectoryBrowser`ようにディレクトリ全体とすべてのサブディレクトリはアクセスできなくなります。 独自のディレクトリなどのパブリック コンテンツを保持することをお勧め*\<コンテンツのルート >/wwwroot*アプリケーションのビューや構成ファイルなどから離れた場所。

* Url で公開されているコンテンツを`UseDirectoryBrowser`と`UseStaticFiles`は大文字小文字の区別および基になるファイル システムの文字の制限に従って提供します。 たとえば、Windows では大文字と小文字が Mac と Linux。

* IIS でホストされている ASP.NET Core アプリケーションでは、ASP.NET のコア モジュールを使用して、静的ファイルに対する要求を含むアプリケーションにすべての要求を転送します。 ASP.NET Core モジュールによって処理される前に、要求を処理する機会を取得しないために、IIS の静的ファイル ハンドラーは使用されません。

* (サーバーまたは web サイト レベルで IIS の静的ファイル ハンドラーを削除するには。

     * 移動し、**モジュール**機能

     * 選択**: StaticFileModule**一覧で

     * タップ**削除**で、**アクション**サイド バー

>[!WARNING]
> IIS 静的ファイル ハンドラーが有効になっている場合**と**ASP.NET Core モジュール (ANCM) が正しく構成されていない (たとえば場合*web.config*が展開されていない)、静的ファイルが処理されます。

* コード ファイル (c# および Razor を含む) は、アプリ プロジェクトの外側に配置する必要があります`web root`(*wwwroot*既定)。 これには、明確な区別する、アプリのクライアント側のコンテンツとサーバー側コードが漏洩するを防ぎますサーバー側ソース コードが作成されます。

## <a name="additional-resources"></a>その他のリソース

* [ミドルウェア](middleware.md)

* [ASP.NET Core の概要](../index.md)
