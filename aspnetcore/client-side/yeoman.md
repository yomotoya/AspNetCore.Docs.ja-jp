---
title: "ASP.NET Core で Yeoman でプロジェクトをビルドします。"
author: spboyer
description: "この記事の内容が ASP.NET Core、Yeoman を使用して web アプリケーションの作成過程 macos ジェネレーター。"
keywords: "ASP.NET Core、Yeoman、クロス プラットフォーム yo aspnet"
ms.author: spboyer
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: fda0c2a8-1743-4505-be1a-7f8ceeef8647
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/yeoman
ms.openlocfilehash: d7411c1635e9fef2857f9a03e7310224ee8d7344
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-building-projects-with-yeoman-in-aspnet-core"></a>ASP.NET Core で Yeoman とプロジェクトのビルドの概要

[Yeoman](http://yeoman.io/)は、さまざまなアプリケーションを作成するためのプロジェクトのスキャフォールディング システムです。 Yeoman ASP.NET Core のジェネレーターには、さまざまな新しい web、MVC、またはコンソール アプリケーションを起動するためのプロジェクト テンプレートが含まれています。

## <a name="install-nodejs-npm-and-yeoman"></a>Node.js、npm、および Yeoman をインストールします。

### <a name="prerequisites"></a>必須コンポーネント

Node.js と npm Yeoman に必要です。 ダウンロード[Node.js](https://nodejs.org/)です。 インストーラーが含まれています[Node.js](https://nodejs.org/)と[npm](https://www.npmjs.com/)です。 Bower ものブートス トラップのような UI フレームワークのインストールに必要です。

Yeoman と Bower をインストールするには、次のコマンドを実行します。

```console
npm install -g yo bower
```

>[!Note]
>エラーが発生した場合`npm ERR! Please try running this command again as root/Administrator.`macOS などで、次のコマンドを使用して、実行[sudo](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man8/sudo.8.html):`sudo npm install -g yo bower`

コマンド プロンプトから、ASP.NET コード ジェネレーターをインストールします。

```console
npm install -g generator-aspnet
```

> [!NOTE]
> アクセス許可のエラーが発生した場合は、下にあるコマンドを実行`sudo`前述のとおりです。

`–g`フラグ インストール ジェネレーター グローバルに、任意のパスから使用できるようにします。

## <a name="create-an-aspnet-app"></a>ASP.NET アプリを作成します。

Yeoman ベースの ASP.NET コード ジェネレーターを実行します。

```console
yo aspnet
```

ジェネレーターには、メニューが表示されます。 下にある矢印、 **Web アプリケーション基本 [せず、メンバーシップと承認]**プロジェクトの順にタップ**Enter**:

![コマンド ウィンドウ: を作成するアプリケーションの種類か。 アプリケーションの種類のメニュー](yeoman/_static/yeoman-yo-aspnet.png)

UI フレームワークとしてブートス トラップを選択し、タップ**Enter**です。

使用して"**MyWebApp**"アプリ名と順にタップ**Enter**です。

Yeoman には、プロジェクトとそのサポート ファイルがスキャフォールディングされます。 推奨される次の手順は、コマンドの形式でも提供されます。

![コマンド ウィンドウ: ASP.NET アプリケーションの名前は何ですか。 コマンド プロンプト](yeoman/_static/yeoman-yo-aspnet-created.png)

[ASP.NET ジェネレーター](https://www.npmjs.com/package/generator-aspnet) ASP.NET Core を Visual Studio のコードを Visual Studio に読み込まれることができますか、コマンドラインから実行するプロジェクトを作成します。

## <a name="restore-build-and-run"></a>復元、ビルド、および実行

次のディレクトリを変更することで、推奨されるコマンド、`MyWebApp`ディレクトリ。 実行して`dotnet restore`です。

![[コマンド] ウィンドウ](yeoman/_static/dotnet-restore.png)

ビルドおよび実行するアプリを使用して、`dotnet build`と`dotnet run`:

![[コマンド] ウィンドウ](yeoman/_static/dotnet-build-run.png)

この時点では、アプリをテストする、新しく作成された ASP.NET Core 表示される URL に移動することができます。

## <a name="client-side-packages"></a>クライアント側のパッケージ

フロント エンド サーバーのリソースは、Yeoman から、テンプレートによって提供されるジェネレーターを使用して、 [Bower](xref:client-side/bower)クライアント側のパッケージ マネージャーを追加する*bower.json*と*.bowerrc*Bower を使用してクライアント側のパッケージを復元するファイル。

[BundlerMinifier](xref:client-side/bundling-and-minification)コンポーネントが既定では、使いやすさ (バンドル) を連結したものと CSS、JavaScript、および HTML の縮小も含まれています。

## <a name="building-and-running-from-visual-studio"></a>ビルドと Visual Studio から実行

Visual Studio に直接生成された ASP.NET Core web プロジェクトを読み込むをビルドし、そこから、プロジェクトを実行します。 Yeoman を使用して、新しい ASP.NET Core アプリケーションをスキャフォールディングするには、上記の手順に従います。 このとき、選択**Web アプリケーション**メニューと、アプリの名前から`MyWebApp`です。

Visual Studio を開きます。 [ファイル] メニューから開く ‣ プロジェクト/ソリューションを選択します。

プロジェクトを開くダイアログ ボックスに移動、 *.csproj*ファイルを選択し、をクリックして、**開く**ボタンをクリックします。 ソリューション エクスプ ローラーで、プロジェクトはようになります次のスクリーン ショット。

![ソリューション エクスプ ローラーで新しいプロジェクトのファイルとフォルダー](yeoman/_static/yeoman-solution.png)

Yeoman scaffolds MVC web アプリケーション、両方を持つ完全なサーバー側とクライアント側のサポートを構築します。 サーバー側の依存関係が表示されます、**の依存関係/NuGet**ノード、および内のクライアント側の依存関係、**の依存関係/Bower**ソリューション エクスプ ローラーのノードです。 依存関係は、プロジェクトが読み込まれるときに自動的に復元されます。

![ソリューション エクスプ ローラーのツリー ビューの依存関係ノードの下、Bower が開いているその依存関係を一覧表示します。](yeoman/_static/yeoman-loading-dependencies.png)

すべての依存関係を復元すると、キーを押して**f5 キーを押して**プロジェクトを実行します。 既定のホーム ページをブラウザーで表示します。

![Microsoft Edge で開いている web アプリケーション](yeoman/_static/yeoman-home-page.png)

## <a name="restoring-building-and-hosting-from-a-command-line"></a>復元する、ビルド、およびコマンド ラインからホスティング

準備し、.NET Core CLI を使用して web アプリケーションをホストできます。

コマンド プロンプトで、プロジェクトを含むフォルダーに現在のディレクトリを変更します (つまり、フォルダーを含む、 *.csproj*ファイル)。

```console
cd src\MyWebApp
```

プロジェクトの NuGet パッケージの依存関係を復元します。

```console
dotnet restore
```

アプリケーションを実行します。

```console
dotnet run
```

クロス プラットフォーム[Kestrel](xref:fundamentals/servers/kestrel) 5000 のポートでリッスンしている web サーバーが開始されます。

Web ブラウザーを開きに移動`http://localhost:5000`です。

![Microsoft Edge で開いている web アプリケーション](yeoman/_static/yeoman-home-page_5000.png)

## <a name="adding-to-your-project-with-sub-generators"></a>Sub ジェネレーターで、プロジェクトに追加します。

Yeoman を使用して[ジェネレーターを sub](https://github.com/omnisharp/generator-aspnet)、いずれかを追加することができます、`nuget.config`または`web.config`プロジェクトを作成した後です。 たとえば、ファイルを作成するディレクトリから次のコマンドを実行します。

```console
yo aspnet:nugetconfig
```

結果は、という名前の NuGet 構成ファイル`nuget.config`次の内容。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
 <packageSources>
    <!--To inherit the global NuGet package sources remove the <clear/> line below -->
    <clear />
 </packageSources>
</configuration>
```

## <a name="additional-resources"></a>その他の技術情報

* [サーバー (Kestrel と WebListener)](xref:fundamentals/servers/index)
* [ASP.NET Core の基礎の概要](xref:fundamentals/index)
