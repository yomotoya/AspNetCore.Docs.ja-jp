---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Visual Studio 2013 で ASP.NET スキャフォールディング |Microsoft ドキュメント"
author: tfitzmac
description: "ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Visual Studio 2013 で ASP.NET スキャフォールディング
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。


## <a name="overview"></a>概要

ASP.NET のスキャフォールディングとは、ASP.NET Web アプリケーションのコード生成フレームワークです。 Visual Studio 2013 には、MVC、Web API プロジェクトに対して事前インストールされているコード ジェネレーターが含まれています。 データ モデルと対話するコードを簡単に追加するときに、スキャフォールディングをプロジェクトに追加します。 スキャフォールディングを使用すると、プロジェクトでの標準的なデータ操作を開発する時間を削減できます。

既定では、Visual Studio 2013 は Web フォームは、プロジェクト コードの生成をサポートしていませんが、プロジェクトに MVC 依存関係を追加するか、または拡張機能のインストールで Web フォームでスキャフォールディングを使用することができます。 両方の方法は、以下に示します。

Visual Studio 2013 Update 2 (現在 RC) は、シナリオの要件を満たすにスキャフォールディングを ASP.NET を拡張する機能を提供します。 この機能により、カスタマイズされたスキャフォールディング テンプレートを作成し、新しい Scaffold の追加 ダイアログ ボックスに追加することができます。 カスタマイズされたテンプレート内のスキャフォールディングされた項目を追加するときに生成されるコードを指定します。 詳細については、次を参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)です。

## <a name="prerequisites"></a>必須コンポーネント

ASP.NET のスキャフォールディングを使用するのには、次が必要です。

- Microsoft Visual Studio 2013
- Web 開発者用ツール (既定の Visual Studio 2013 のインストールの一部)
- ASP.NET Web フレームワークとツールの 2013 (既定の Visual Studio 2013 のインストールの一部)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>MVC または Web API のスキャフォールディングされた項目を追加します。

スキャフォールディングを追加するには、プロジェクトまたはプロジェクト内のフォルダーを右クリックして**追加**–**スキャフォールディングされた新しい項目**の次の図に示すようにします。

![Scaffold 項目を追加します。](aspnet-scaffolding-overview/_static/image1.png)

**追加 Scaffold**ウィンドウで、追加する scaffold の種類を選択します。

![Scaffold の種類を選択します。](aspnet-scaffolding-overview/_static/image2.png)

**コント ローラーの追加**ウィンドウが正しければ、コント ローラーは、Entity Framework 6 から新しい非同期機能を使用するかどうかなどを生成するためのオプションを選択します。

![コント ローラーを追加します。](aspnet-scaffolding-overview/_static/image3.png)

シナリオに関連するクラスとページが作成されます。 たとえば、次の図は、MVC コント ローラーと映画をという名前のモデル クラスをスキャフォールディングによって作成されたビューを示しています。

![作成したファイル](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>スキャフォールディングされた項目を Web フォームに追加します。

Web フォームのコードを生成するためのスキャフォールディングを追加するには、Visual studio 拡張機能のインストールか、MVC 依存関係を追加する必要があります。 両方の方法は、以下に示しますが、のみこれらのアプローチのいずれかを実行する必要があります。

### <a name="web-forms-scaffolding-extension"></a>拡張機能をスキャフォールディング web フォーム

プロジェクトを Web フォームのスキャフォールディングを使用するため、Visual Studio 拡張機能をインストールすることができます。 Visual Studio で、次のように選択します。**ツール**し**拡張機能と更新プログラム**です。 このダイアログ ボックスからの Visual Studio ギャラリーを検索**Web フォームのスキャフォールディング**です。

![web フォームのスキャフォールディングをインストールします。](aspnet-scaffolding-overview/_static/image5.png)

詳細については、次を参照してください。 [Web フォームのスキャフォールディング](https://go.microsoft.com/fwlink/p/?LinkId=396478)です。

### <a name="mvc-dependencies"></a>MVC 依存関係

MVC 依存関係を追加するには、次のように選択します。**追加** - **スキャフォールディングされた新しい項目**です。 Scaffold の追加 ウィンドウで、次のように選択します。 **MVC 依存関係**、次のようにします。

![MVC 依存関係を追加します。](aspnet-scaffolding-overview/_static/image6.png)

MVC; スキャフォールディングの 2 つのオプションがあります。最小限かつ完全します。 最低限を選択した場合は、NuGet パッケージの管理と ASP.NET MVC の参照のみがプロジェクトに追加されます。 [完全] オプションを選択した場合、MVC プロジェクトの必要なコンテンツ ファイルだけでなく、最小の依存関係が追加されます。 スキャフォールディングを簡単に使用するには、すべての依存関係を選択します。

![すべての依存関係を選択します。](aspnet-scaffolding-overview/_static/image7.png)

依存関係を追加すると、表示されます、 **readme.txt**ファイル。 慎重に、指示に従ってこのファイルをプロジェクトが正常に動作することを確認します。

Readme.txt ファイル」の手順を完了すると、MVC および Web API に関する前のセクションで示すように、新しいスキャフォールディングされた項目を追加できます。 自動的に生成されたビューとコント ローラーは、プロジェクト内で正しく機能します。

## <a name="tutorials"></a>チュートリアル

カスタマイズされた scaffolder を作成するを参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)です。

生成されたファイルをカスタマイズするを参照してください。[スキャフォールディングされた新しい項目 ダイアログ ボックスから生成されたファイルをカスタマイズする方法について](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)です。

スキャフォールディングを使用する例については**Database First の開発**を参照してください[EF Database First ASP.NET MVC で](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)です。

スキャフォールディングを使用する例については、 **MVC**プロジェクトを参照してください[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)です。

スキャフォールディングを使用する例については、 **Web API**プロジェクトを参照してください[属性のルーティングが Web API 2 の REST API の作成](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)です。
