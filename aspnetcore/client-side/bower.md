---
title: "ASP.NET Core での Bower を使用"
author: rick-anderson
description: "Bower でクライアント側のパッケージを管理します。"
keywords: ASP.NET Core, bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: df7c43da-280e-4df6-86cb-eecec8f12bfc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bower
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0258315e0e24d662086a3171b58112e08b9a40ab
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/14/2017
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>ASP.NET Core での Bower でクライアント側のパッケージを管理します。

によって[Rick Anderson](https://twitter.com/RickAndMSFT)、[ノエル ライス](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)、および[Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Bower を保持したまま、別のソリューションを使用してそれらを勧めします。 Webpack で yarn を 1 つの一般的な代替手段は、[移行手順](https://bower.io/blog/2017/how-to-migrate-away-from-bower/)を利用できます。

[Bower](https://bower.io/)呼び出し自体「web のパッケージ マネージャーです」。 .NET エコシステム内では、NuGet の静的なコンテンツ ファイルを配信できないことによってまま void が挿入されます。 ASP.NET Core プロジェクトでは、これらの静的ファイルはのようにクライアント側のライブラリに固有[jQuery](http://jquery.com/)と[ブートス トラップ](http://getbootstrap.com/)です。 .NET ライブラリを使用することも[NuGet](https://www.nuget.org/)パッケージ マネージャーです。

クライアント側の設定の ASP.NET Core プロジェクト テンプレートで作成した新しいプロジェクトはビルド プロセスです。 [jQuery](http://jquery.com/)と[ブートス トラップ](http://getbootstrap.com/)がインストールされている、Bower がサポートされているとします。

クライアント側のパッケージに記載されて、 *bower.json*ファイル。 ASP.NET Core プロジェクト テンプレートを構成*bower.json* jQuery、jQuery 検証、およびブートス トラップを使用します。

このチュートリアルではサポートを追加します[フォント優れた](http://fontawesome.io)です。 と共にインストールできる bower パッケージ、 **Bower パッケージの管理**UI または手動で、 *bower.json*ファイル。

### <a name="installation-via-manage-bower-packages-ui"></a>Bower パッケージの管理 UI を使用してインストール

* 新しい ASP.NET Core Web アプリを作成、 **ASP.NET Core Web アプリケーション (.NET Core)**テンプレート。 選択**Web アプリケーション**と**認証なし**です。

* ソリューション エクスプ ローラーでプロジェクトを右クリックし  **Bower パッケージの管理**(また、メイン メニューから**プロジェクト** > **Bowerパッケージの管理**).

* **Bower:\<プロジェクト名\>**ウィンドウで、「参照」 タブをクリックし、入力することで、パッケージ の一覧をフィルター`font-awesome`検索ボックスに。

 ![bower パッケージを管理します。](bower/_static/manage-bower-packages.png)

* いることを確認します"に変更を保存*bower.json*"チェック ボックスをオンします。 ドロップダウン リストからバージョンを選択し、**インストール**ボタンをクリックします。 **出力**ウィンドウは、インストールの詳細を表示します。

### <a name="manual-installation-in-bowerjson"></a>Bower.json に手動のインストール

開く、 *bower.json*ファイルおよび依存関係に「フォント優れた」を追加します。 IntelliSense は、利用可能なパッケージを示しています。 パッケージを選択すると、使用可能なバージョンが表示されます。 次の図は、古いし、表示される内容と一致しません。

![Bower パッケージ エクスプ ローラーの IntelliSense](bower/_static/add-package.png)

![bower バージョン IntelliSense](bower/_static/version-intelliSense.png)

使用を bower[セマンティック バージョニング](http://semver.org/)の依存関係を整理します。 セマンティック バージョニング、SemVer とも呼ばれる、番号付けスキームでパッケージを識別する\<メジャー >.\<マイナー >。\<パッチ >。 IntelliSense は、いくつかの一般的な選択肢のみを表示することによって、セマンティック バージョニングを簡略化します。 IntelliSense の一覧 (上記の例では 4.6.3) の最上位の項目は、パッケージの最新の安定バージョンと見なされます。 キャレット (^) 記号が最新のメジャー バージョンと一致し、ティルダ (~) には、最新のマイナー バージョンが一致します。

保存、 *bower.json*ファイル。 Visual Studio は、監視、 *bower.json*ファイルの変更。 保存時に、 *bower インストール*コマンドを実行します。 出力ウィンドウの表示**npm/Bower**正確なコマンドが実行を表示します。

開く、 *.bowerrc*下にあるファイル*bower.json*です。 `directory`プロパティに設定されている*wwwroot/lib*パッケージ資産をインストールする場所 Bower を示します。

```json
{
 "directory": "wwwroot/lib"
}
```

ソリューション エクスプ ローラーで、検索ボックスを使用して、見つけてフォント優れたパッケージを表示することができます。

開く、 *\shared\_Layout.cshtml*ファイルし、環境にフォント優れた CSS ファイルを追加[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)の`Development`します。 ソリューション エクスプ ローラーからドラッグ アンド ドロップ*フォント awesome.css*内、`<environment names="Development">`要素。

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

運用アプリにするには追加*フォント awesome.min.css* 、環境タグのヘルパーを`Staging,Production`です。

内容を置き換える、 *Views\Home\About.cshtml*次のマークアップ Razor ファイル。

[!code-html[Main](bower/sample/About.cshtml)]

アプリを実行し、フォント優れたパッケージの動作の確認 [バージョン情報] ビューに移動します。

## <a name="exploring-the-client-side-build-process"></a>クライアント側のビルド プロセスの検証

ほとんどの ASP.NET Core プロジェクト テンプレートは、Bower を使用してように既に構成済みです。 この次のチュートリアルでは、空の ASP.NET Core プロジェクトから開始して、プロジェクトで Bower を使用する方法について理解を取得できるように、手動で各部分を追加します。 プロジェクトの構造と、ランタイムが出力されている各構成の変更が行われたとおりに動作を確認できます。

Bower でクライアント側のビルド プロセスを使用する一般的な手順は次のとおりです。

* プロジェクトで使用されるパッケージを定義します。 <!-- once defined, you don't need to download them, VS does -->
* Web ページからのパッケージを参照します。

### <a name="define-packages"></a>パッケージを定義します。

パッケージを一覧表示した後、 *bower.json*ファイル、Visual Studio がダウンロードされます。 次の例で Bower を使用して jQuery とブートス トラップを読み込む、 *wwwroot*フォルダーです。

* 新しい ASP.NET Core Web アプリを作成、 **ASP.NET Core Web アプリケーション (.NET Core)**テンプレート。 選択、**空**プロジェクト テンプレートとクリック**OK**です。

* ソリューション エクスプ ローラーでプロジェクトを右クリックして >**新しい項目の追加**選択**Bower 構成ファイル**です。 注: A *.bowerrc*ファイルも追加します。

* 開いている*bower.json*、jquery を追加しをブートス トラップ、`dependencies`セクションです。 その結果、 *bower.json*ファイルは次の例のようになります。 バージョンは、時間の経過と共にが変更され、下の画像と一致しない可能性があります。

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* 保存、 *bower.json*ファイル。

 プロジェクトに含まれることを確認、*ブートス トラップ*と*jQuery*ディレクトリ*wwwroot/lib*です。 Bower 使用して、 *.bowerrc*ファイル内の資産をインストールする*wwwroot/lib*です。

 注:「Bower パッケージの管理」の UI は、手動ファイルを編集する代わりにを提供します。

### <a name="enable-static-files"></a>静的なファイルを有効にします。

* 追加、 `Microsoft.AspNetCore.StaticFiles` NuGet パッケージをプロジェクトにします。
* 静的ファイルで配信されることを有効にする、[静的ファイル ミドルウェア](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)です。 呼び出しを追加して[UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions)を`Configure`メソッドの`Startup`します。

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>参照パッケージ

このセクションでは、配置したパッケージにアクセスできることを確認する HTML ページを作成します。

* という名前の新しい HTML ページを追加*Index.html*を*wwwroot*フォルダーです。 注: HTML ファイルを追加する必要があります、 *wwwroot*フォルダーです。 既定では、外部の静的なコンテンツを処理できない*wwwroot*です。 参照してください[静的ファイルで作業](xref:fundamentals/static-files)詳細についてはします。

 内容を置き換える*Index.html*次のマークアップ。

[!code-html[Main](bower/sample/Index.html)]

* アプリを実行してに移動`http://localhost:<port>/Index.html`です。 代わりに、 *Index.html*開くと、キーを押して`Ctrl+Shift+W`です。 Jumbotron スタイルが適用される、jQuery コード応答、ボタンがクリックされたときに、ブートス トラップのボタンが状態を変更することを確認します。

 ![適用される jumbotron スタイル](bower/_static/jumbotron.png)
