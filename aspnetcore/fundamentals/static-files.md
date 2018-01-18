---
title: "ASP.NET Core の静的ファイルを操作します。"
author: rick-anderson
description: "サービスを提供し、静的なファイルをセキュリティで保護および ASP.NET Core web アプリでのミドルウェアの動作をホストしている静的ファイルを構成する方法を説明します。"
keywords: "ASP.NET Core、静的なファイル、HTML、CSS、JavaScript の静的なアセット"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>ASP.NET Core の静的ファイルを操作します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)と[Scott Addie](https://twitter.com/Scott_Addie)

HTML、CSS、画像、および、JavaScript などの静的ファイルは、資産の ASP.NET Core アプリケーションが直接クライアントに機能します。 いくつかの構成は、これらのファイルの提供を有効にする必要があります。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="serve-static-files"></a>静的ファイルを提供します

静的ファイルは、プロジェクトの web ルート ディレクトリ内に格納されます。 既定のディレクトリは *\<content_root >/wwwroot*を使用して変更できますが、 [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)メソッドです。 参照してください[コンテンツのルート](xref:fundamentals/index#content-root)と[Web ルート](xref:fundamentals/index#web-root)詳細についてはします。

アプリの web ホストは、コンテンツのルート ディレクトリの対応に行う必要があります。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

`WebHost.CreateDefaultBuilder`メソッドは、現在のディレクトリへのコンテンツのルートを設定します。

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

呼び出すことによって、現在のディレクトリにコンテンツのルートを設定[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)内の`Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

静的なファイルは、web ルートに対する相対パスを経由してアクセスできます。 たとえば、 **Web アプリケーション**プロジェクト テンプレートには内でいくつかのフォルダーが含まれています、 *wwwroot*フォルダー。

* **wwwroot**
  * **css**
  * **images**
  * **js**

URI の形式でファイルへのアクセス、*イメージ*サブフォルダーは*http://\<server_address >/images/\<画像 >*です。 たとえば、 *http://localhost:9189/images/banner3.svg*です。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

.NET Framework を対象にする場合は、追加、 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)をプロジェクトにパッケージします。 .NET Core をターゲットとする場合、 [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)このパッケージが含まれています。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

追加、 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)をプロジェクトにパッケージします。

---

構成、[ミドルウェア](xref:fundamentals/middleware)静的ファイル サービスを有効にします。

### <a name="serve-files-inside-of-web-root"></a>Web ルート内でファイルを提供します

呼び出す、 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)メソッド内で`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

パラメーターなし`UseStaticFiles`メソッドのオーバー ロードが含んだとして web ルート内のファイルをマークします。 次のマークアップ参照*wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Web ルートの外部でファイルを提供します

Web ルートの外部で処理される静的ファイルが存在するディレクトリの階層を考慮してください。

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

要求にアクセスできる、 *banner1.svg*静的ファイル ミドルウェアを次のように構成することによってファイル。

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

上記のコードでは、 *MyStaticFiles*を介してディレクトリ階層をパブリックに公開される、 *StaticFiles* URI セグメント。 要求を*http://\<server_address >/StaticFiles/images/banner1.svg*機能、 *banner1.svg*ファイル。

次のマークアップ参照*MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>HTTP 応答ヘッダーを設定します。

A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions)を HTTP 応答ヘッダーを設定するオブジェクトを使用できます。 に加えて、web のルートからの静的なファイル サーバーを構成するには、次のコード セット、`Cache-Control`ヘッダー。

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)にメソッドが存在する、 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)パッケージです。

ファイルが加えられましたキャッシュ可能なパブリックに 10 分 (600 秒)。

![Cache-control ヘッダーを示す応答ヘッダーが追加されました](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>静的ファイルの承認

静的ファイル ミドルウェアは、承認チェックを提供していません。 すべてのファイル サービスを提供し、下にあるものを含む*wwwroot*はパブリックにアクセスします。 ファイルを提供するには、承認に基づいています。

* 外部に保存する*wwwroot*と静的ファイル ミドルウェアにアクセスできる任意のディレクトリ**と**
* 承認を適用するアクション メソッドを使用して、それらを提供します。 返す、 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)オブジェクト。

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>ディレクトリの参照を有効にします。

により、ディレクトリの一覧を表示する web アプリのユーザーと指定したディレクトリ内にあるファイルをディレクトリの参照します。 既定ではセキュリティ上の理由からディレクトリの参照が無効になっています (を参照してください[に関する考慮事項](#considerations))。 有効にするディレクトリの参照を呼び出すことによって、 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_)メソッド`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

呼び出すことによって必要なサービスを追加、 [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_)メソッドから`Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

上記のコードにより、ディレクトリの参照の*wwwroot/イメージ*フォルダーの URL を使用して*http://\<server_address >/により*、各ファイルおよびフォルダーへのリンク。

![ディレクトリの参照](static-files/_static/dir-browse.png)

参照してください[に関する考慮事項](#considerations)ブラウズを有効にする場合は、セキュリティ上のリスクにします。

2 つに注意してください`UseStaticFiles`は次の例で呼び出します。 最初の呼び出しにより、内の静的ファイルの提供、 *wwwroot*フォルダーです。 2 番目の呼び出しにより、ディレクトリの参照の*wwwroot/イメージ*フォルダーの URL を使用して*http://\<server_address >/により*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>既定のドキュメントを提供します。

既定のホーム ページの設定を提供訪問者論理の開始点は、サイトを訪問するとします。 既定のページは、URI の完全修飾ユーザーなし機能を呼び出して、 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)メソッドから`Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`前に呼び出す必要があります`UseStaticFiles`を既定のファイルを提供します。 `UseDefaultFiles`実際には、ファイルに貢献しない URL リライターは、します。 使用して、静的ファイル ミドルウェアを有効にする`UseStaticFiles`にファイルを提供します。

`UseDefaultFiles`フォルダーを検索する要求。

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

一覧から見つかった最初のファイルは、要求が完全修飾 URI であるかのように処理されます。 ブラウザーの URL は、要求された URI を反映するように続行します。

次のコードを既定のファイル名を変更する*mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_)の機能を結合`UseStaticFiles`、 `UseDefaultFiles`、および`UseDirectoryBrowser`です。

次のコードでは、静的なファイルおよび既定のファイルのサービスを使用できます。 ディレクトリの参照が有効になっています。

```csharp
app.UseFileServer();
```

次のコードは、ディレクトリの参照を有効にするによって、パラメーターなしのオーバー ロード時に作成されます。

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

次のディレクトリ階層を考慮してください。

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

次のコードにより、静的なファイル、既定のファイルおよびディレクトリの参照の`MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`呼び出す必要がある場合に、`EnableDirectoryBrowsing`プロパティの値が`true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

ファイル階層を使用して、前のコード、Url を解決するには、次のように。

| URI            |                             応答  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

既定の名前のファイルが存在しない場合、 *MyStaticFiles*ディレクトリ、 *http://\<server_address >/StaticFiles*ディレクトリでクリック可能なリンクの一覧を返します。

![静的ファイルの一覧](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`および`UseDirectoryBrowser`URL を使用して*http://\<server_address >/StaticFiles* 、末尾のスラッシュをクライアント側をトリガーせずにリダイレクト*http://\<server_address >/StaticFiles/*です。 末尾のスラッシュの追加を確認します。 ドキュメント内の相対 Url は、末尾のスラッシュは無効です。 と考えられます。

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)クラスが含まれています、`Mappings`プロパティの MIME コンテンツの種類にファイル拡張子のマッピングとして機能します。 次の例では、いくつかのファイル拡張子が既知の MIME の種類に登録されます。 *.Rtf*拡張機能を置き換え、および*.mp4*を削除します。

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

参照してください[MIME コンテンツ タイプ](http://www.iana.org/assignments/media-types/media-types.xhtml)です。

## <a name="non-standard-content-types"></a>非標準のコンテンツの種類

静的ファイル ミドルウェアは、ほぼ 400 の既知のファイル コンテンツの種類を認識しています。 ユーザーは、不明なファイル種類のファイルを要求している場合、静的ファイル ミドルウェアは、HTTP 404 (Not Found) 応答を返します。 ディレクトリの参照が有効になっている場合、ファイルへのリンクが表示されます。 URI には、HTTP 404 エラーが返されます。

次のコードでは、不明な型の機能を有効にし、イメージとして不明なファイルを表示します。

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

上記のコードでは、不明なコンテンツの種類のファイルに対する要求は、イメージとして返されます。

> [!WARNING]
> 有効にする[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes)セキュリティ上のリスクです。 既定では、無効になっているとその使用はお勧めします。 [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider)非標準の拡張子を持つファイルを処理するより安全な代替手段を提供します。

### <a name="considerations"></a>注意事項

> [!WARNING]
> `UseDirectoryBrowser`および`UseStaticFiles`機密データがリークすることができます。 無効にするディレクトリの実稼働環境で参照することを強くお勧めします。 使用して有効にするディレクトリを注意深く確認`UseStaticFiles`または`UseDirectoryBrowser`です。 全体のディレクトリとそのサブディレクトリにパブリックにアクセスできるようになります。 ストア ファイルをパブリック サービスに適した専用のディレクトリによう *\<content_root >/wwwroot*です。 MVC ビュー、構成ファイルなど、Razor ページ (2.x のみ)、これらのファイルと区別されます。

* Url で公開されているコンテンツを`UseDirectoryBrowser`と`UseStaticFiles`は大文字小文字の区別および基になるファイル システムの文字の制限に従って提供します。 たとえば、Windows は大文字小文字を区別&mdash;Mac と Linux されません。

* ASP.NET Core アプリケーションの使用の IIS でホストされている、 [ASP.NET Core モジュール (ANCM)](xref:fundamentals/servers/aspnet-core-module)静的ファイルの要求を含む、アプリへのすべての要求を転送します。 IIS の静的ファイル ハンドラーは使用されません。 ANCM で処理する前に、要求を処理する機会はありません。

* IIS マネージャーでサーバーまたは web サイト レベルで IIS の静的ファイル ハンドラーを削除する次の手順を実行するには。
    1. 移動し、**モジュール**機能します。
    1. 選択**: StaticFileModule**一覧にします。
    1. をクリックして**削除**で、**アクション**サイドバーです。

> [!WARNING]
> IIS の静的ファイル ハンドラーが有効になっている場合**と**ANCM が正しく構成されていない、静的なファイルが処理されます。 これは、場合、たとえば、 *web.config*ファイルが配置されていません。

* コード ファイルの配置 (含む*.cs*と*.cshtml*) アプリ プロジェクトの web ルートの外部でします。 そのため、論理的な分離がアプリのクライアント側のコンテンツとサーバー側コード間で作成されます。 これはサーバー側コードが漏洩することを防ぎます。

## <a name="additional-resources"></a>その他の技術情報

* [ミドルウェア](xref:fundamentals/middleware)

* [ASP.NET Core の概要](xref:index)