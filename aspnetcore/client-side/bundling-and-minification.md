---
title: "バンドルと ASP.NET Core の縮小"
author: spboyer
description: 
keywords: "ASP.NET Core、バンドルと縮小、CSS、JavaScript、縮小、BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: d8512bdd49b61019f22a49900bdd65086d821a6b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>バンドルと ASP.NET Core の縮小

バンドルと縮小 2 つの方法は、web アプリケーションのページ読み込みのパフォーマンスを向上させるために ASP.NET で使用することができます。 複数のファイルを 1 つのファイルに連結するバンドルします。 縮小では、さまざまなスクリプトと CSS を小さいペイロードで結果を別のコードの最適化を実行します。 一緒に使用すると、バンドルと縮小ロード時間、パフォーマンスが向上サーバーへの要求の数を減らすと、要求されたアセット (CSS および JavaScript ファイルなど) のサイズを小さきます。

この記事では、バンドルと縮小を含む ASP.NET Core アプリケーションでこれらの機能を使用する方法を使用する利点について説明します。

## <a name="overview"></a>概要

ASP.NET Core アプリ バンドル化とクライアント側のリソースを縮小するための複数のオプションがあります。 MVC の中核となるテンプレートでは、構成ファイルと BuildBundlerMinifier NuGet パッケージを使用して既定のソリューションを提供します。 などのサード パーティ製ツール[Gulp](using-gulp.md)と[Grunt](using-grunt.md)がプロセスに必要なその他のワークフローまたは複雑な作業に同じタスクを実行することもできます。 デザイン時のバンドルと縮小を使用すると、縮小されたファイルは、アプリケーションの展開する前に作成されます。 バンドル化と縮小展開する前に、サーバー負荷の軽減の利点を提供します。 ただし、そのデザイン時のバンドルを認識することが重要と縮小ビルド複雑さが増加し、静的ファイルでのみ動作します。

バンドルと縮小、主に、最初のページ要求の読み込み時間を向上します。 ブラウザー キャッシュ資産 (JavaScript、CSS、および画像) で、web ページが要求された後、同じページを要求するときに、バンドルと縮小すると、すべてのパフォーマンスが向上が提供するされませんやページは、同じサイトの同じアセットを要求するようにします。 設定しない場合、ヘッダー、資産を正しく有効期限が切れるとバンドルと縮小を使用しない場合、ブラウザーの鮮度ヒューリスティックとしてマークされます資産古い数日後とブラウザーでは資産ごとに、検証要求が必要があります。 この例では、バンドルと縮小は、最初のページ要求した後でも、パフォーマンスが向上を提供します。

### <a name="bundling"></a>バンドル化

バンドルは、結合または 1 つのファイルを複数のファイルにバンドルしやすく機能です。 1 つのファイルに複数のファイルを結合するバンドル、ために、取得して、web ページなどの web アセットを表示するために必要なサーバーに要求の数を削減します。 CSS、JavaScript、およびその他のバンドルを作成することができます。 ファイル数を減らしてでは、サーバーに、ブラウザーまたはアプリケーションを提供するサービスから少数の HTTP 要求を意味します。 この結果では、最初のページ読み込みのパフォーマンスを向上します。

### <a name="minification"></a>縮小

縮小では、さまざまな (など、CSS、画像、JavaScript ファイル) の要求された資産のサイズを小さく、さまざまなコードの最適化を実行します。 縮小の一般的な結果には、不要な空白文字とコメントを削除して、1 文字に変数名を短くが含まれます。

次の JavaScript 関数を考慮してください。

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

縮小、後に、関数は、次に縮小されます。

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

コメントと不要な空白文字を削除するだけでなく、次のパラメーターと変数の名前が変更された (切り捨て) には、次のように。

元 | 名前の変更
--- | :---:
imageTagAndImageID | t
イメージ コンテキスト | a
imageElement | r

## <a name="impact-of-bundling-and-minification"></a>バンドルと縮小の影響

次の表は、すべてのアセットを個別に一覧表示して、バンドルと縮小を使用して、単純な web ページ上のいくつかの重要な違いを示しています。

操作 | B/M と | B/M なし | 変更
--- | :---: | :---: | :---:
ファイルの要求 |7 | 18 | 157%
サポート技術情報の転送 | 156 | 264.68 | 70%
読み込み時間 (ミリ秒) | 885 | 2360 | 167%

送信されたバイト数には、ブラウザーは要求に適用される HTTP ヘッダーを非常に冗長のバンドルを大幅に軽減が必要があります。 ただし、この例は、ローカルで実行された、読み込み時は、大規模な向上を示します。 アセットのバンドルと縮小を使用して、ネットワーク経由で転送されるときにパフォーマンスの向上が表示されます。

## <a name="using-bundling-and-minification-in-a-project"></a>プロジェクトでバンドルと縮小の使用

MVC プロジェクト テンプレートでは、`bundleconfig.json`構成ファイルの各バンドルのオプションを定義します。 カスタムの JavaScript の既定では、1 つのバンドルの構成が定義されている (`wwwroot/js/site.js`) とスタイル シート (`wwwroot/css/site.css`) ファイル。

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

バンドルのオプションは次のとおりです。

* outputFileName - 出力するバンドル ファイルの名前。 相対パスを含めることができます、`bundleconfig.json`ファイル。 **必須**
* inputFiles - をまとめるためにファイルの配列。 これらは、構成ファイルへの相対パスです。 **省略可能な**、*、空の値が空の出力ファイルの結果します。 [グロブ](http://www.tldp.org/LDP/abs/html/globbingref.html)パターンがサポートされています。
* 縮小 - 出力の縮小オプションを入力します。 **省略可能な**、*既定 -`minify: { enabled: true }`*
  * 出力ファイルの種類ごとの構成オプションのとおりです。
    * [CSS の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [HTML の縮小化](https://github.com/madskristensen/BundlerMinifier/wiki)
* includeInProject - 生成されたファイルをプロジェクト ファイルに追加します。 **省略可能な**、*既定 - false*
* ソースマップ - は、バンドルしたファイルのソース マップを生成します。 **省略可能な**、*既定 - false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 2017/

開いている`bundleconfig.json`Visual Studio で、環境内にインストールされている拡張機能がない場合、プロンプトが表示されますがあるこの種類のファイルを支援できなかったことを推奨します。

![BuildBundlerMinifier 拡張機能の提案](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

表示拡張機能を選択し、インストール、**バンドルと縮小化**拡張機能 (再起動が Visual Studio の必要があります)。

![BuildBundlerMinifier 拡張機能の提案](../client-side/bundling-and-minification/_static/view-extension.png)

再起動が完了したらを縮小して、クライアント側の資産のバンドルのプロセスを実行するビルドを構成する必要があります。 右クリックし、`bundleconfig.json`ファイルおよび選択した*ビルドで有効にするバンドルしています.*.

プロジェクトをビルドし、`bundleconfig.json`構成に基づいて、出力ファイルを生成するために、ビルド プロセスに含まれます。

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Visual Studio Code またはコマンド ライン

Visual Studio と拡張機能は、GUI のジェスチャを使用してバンドルと縮小のプロセスをドライブします。ただし、同じ機能がで使用可能な`dotnet`CLI および BuildBundlerMinifier NuGet パッケージです。

NuGet パッケージをプロジェクトに追加します。

```console
dotnet add package BuildBundlerMinifier
```

依存関係を復元します。

```console
dotnet restore
```

アプリをビルドします。

```console
dotnet build
```

ビルド コマンドからの出力は、縮小や新機能が構成されているに従ってバンドル化の結果を示します。

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>ファイルを追加します。

この例では、追加の CSS ファイルが、呼び出されたが追加`custom.css`バンドルと縮小に用に構成および`site.css`、1 つの結果として得られる`site.min.css`です。

custom.css

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

相対パスを追加`bundleconfig.json`です。

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> また、グロブ パターンを使用する可能性があります -`"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]`ファイルのすべての CSS を取得して、縮小されたファイルのパターンを除外します。

アプリケーションのビルドを開く場合と`site.min.css`、ようになりましたことがわかりますの内容`custom.css`ファイルの末尾に追加されていません。

## <a name="controlling-bundling-and-minification"></a>バンドルと縮小を制御します。

一般に、実稼働環境でのみ、アプリのバンドルと縮小されたファイルを使用します。 開発中は、するアプリがデバッグの簡略化のために、元のファイルを使用します。

スクリプトと、レイアウト ページで、環境タグ ヘルパーを使用して、ページに追加する CSS ファイルを指定できます (を参照してください[タグ ヘルパー](../mvc/views/tag-helpers/index.md))。 環境タグ ヘルパーは、特定の環境で実行する場合、その内容を表示のみされます。 参照してください[複数の環境で作業](../fundamentals/environments.md)詳細については、現在の環境を指定する方法です。

実行されているときに、次の環境タグは、未処理の CSS ファイルをレンダリングする、`Development`環境。

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

実行されているときにのみ、この環境タグは、バンドルと縮小された CSS ファイルをレンダリングする`Production`または`Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Gulp から bundleconfig.json の使用

アプリ バンドルと縮小ワークフローは、イメージ処理キャッシュ破壊、CDN assest 処理などの追加の処理を必要とする場合は、バンドルおよび Minify プロセスを Gulp を変換できます。

> [!NOTE]
> 変換オプションを Visual Studio 2015 および 2017 でのみ利用できます。

右クリックし、`bundleconfig.json`選択**Gulp に変換しています.**.これが生成されます、`gulpfile.js`し、必要な npm パッケージをインストールします。

![Gulp への変換します。](../client-side/bundling-and-minification/_static/convert-togulp.png)

`gulpfile.js`読み取りを生成、`bundleconfig.json`ファイルの構成、したがってそのを継続できますの入力/出力と設定を使用します。

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Visual Studio 2017 でプロジェクトのビルド時に Gulp を有効にするには、するには、*.csproj ファイルに、次を追加します。

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Visual Studio 2015 のプロジェクトのビルド時に Gulp を有効にするには、するには、次を追加、`project.json`ファイル。

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>その他の技術情報

* [Gulp の使用](using-gulp.md)
* [Grunt の使用](using-grunt.md)
* [複数の環境の使用](../fundamentals/environments.md)
* [タグ ヘルパー](../mvc/views/tag-helpers/index.md)
