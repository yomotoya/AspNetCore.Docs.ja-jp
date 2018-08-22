---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013 で ASP.NET スキャフォールディング |Microsoft Docs
author: tfitzmac
description: ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828767"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Visual Studio 2013 で ASP.NET スキャフォールディング
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET のスキャフォールディングとは、Visual Studio 2013 に含まれている新機能です。


## <a name="overview"></a>概要

ASP.NET のスキャフォールディングは、ASP.NET Web アプリケーションのコード生成フレームワークです。 Visual Studio 2013 には、MVC と Web API プロジェクトに対して事前にインストールされているコード ジェネレーターが含まれています。 データ モデルとやり取りするコードをすばやく追加するときに、スキャフォールディングをプロジェクトに追加します。 スキャフォールディングを使用すると、開発プロジェクトでの標準的なデータ操作に時間を削減できます。

既定では、Visual Studio 2013 は Web フォーム プロジェクトでは、コードの生成をサポートしていませんが、MVC 依存関係をプロジェクトに追加するか、拡張機能のインストールで Web フォームのスキャフォールディングを使用することができます。 どちらの方法は、以下に示します。

Visual Studio 2013 Update 2 (現在 RC) は、シナリオの要件を満たすために ASP.NET スキャフォールディングを拡張する機能を提供します。 この機能により、カスタマイズされたスキャフォールディング テンプレートを作成し、新しいスキャフォールディングの追加 ダイアログ ボックスに追加することができます。 カスタマイズされたテンプレート内では、スキャフォールディングされた項目を追加するときに生成されるコードを指定します。 詳細については、次を参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)します。

## <a name="prerequisites"></a>必須コンポーネント

ASP.NET のスキャフォールディングを使用するには、が必要です。

- Microsoft Visual Studio 2013
- Web 開発者ツール (既定の Visual Studio 2013 のインストールの一部)
- ASP.NET Web Frameworks and Tools 2013 (Visual Studio 2013 の既定のインストールの一部)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>MVC または Web API へのスキャフォールディングされた項目を追加します。

スキャフォールディングを追加するには、プロジェクトまたはプロジェクト内のフォルダーを右クリックして**追加**–**スキャフォールディングされた新しい項目**、次の図のようにします。

![スキャフォールディングの項目を追加します。](aspnet-scaffolding-overview/_static/image1.png)

**スキャフォールディングの追加**ウィンドウで、スキャフォールディングの追加の種類を選択します。

![スキャフォールディングの種類を選択します](aspnet-scaffolding-overview/_static/image2.png)

**コント ローラーの追加**ウィンドウは、Entity Framework 6 からの新しい非同期機能を使用するかどうかなど、コント ローラーを生成するためのオプションを選択することを示します。

![コント ローラーを追加します。](aspnet-scaffolding-overview/_static/image3.png)

自分のシナリオは、関連するクラスとページが作成されます。 たとえば、次の図は、MVC コント ローラーと映画をという名前のモデル クラスのスキャフォールディングを使用して作成されたビューを示します。

![作成したファイル](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Web フォームのスキャフォールディングされた項目を追加します。

Web フォームのコードを生成するスキャフォールディングを追加するには、Visual studio 拡張機能をインストールするか、MVC 依存関係を追加する必要があります。 どちらの方法は、以下に示しますが、これらの方法のいずれかを実行するだけで済みます。

### <a name="web-forms-scaffolding-extension"></a>Web フォームの拡張機能のスキャフォールディング

Web フォーム プロジェクトでスキャフォールディングを使用するための Visual Studio 拡張機能をインストールすることができます。 Visual Studio で、次のように選択します。**ツール**し**拡張機能と更新**します。 このダイアログ ボックスから検索用の Visual Studio ギャラリー **Web フォームのスキャフォールディング**します。

![web フォームのスキャフォールディングをインストールします。](aspnet-scaffolding-overview/_static/image5.png)

詳細については、次を参照してください。 [Web フォームのスキャフォールディング](https://go.microsoft.com/fwlink/p/?LinkId=396478)します。

### <a name="mvc-dependencies"></a>MVC 依存関係

MVC 依存関係を追加するには、次のように選択します。**追加** - **スキャフォールディングされた新しい項目**します。 [スキャフォールディングの追加] ウィンドウで、次のように選択します。 **MVC 依存関係の**以下に示すようにします。

![MVC 依存関係を追加します。](aspnet-scaffolding-overview/_static/image6.png)

MVC; をスキャフォールディングするための 2 つのオプションがあります。最小限かつ完全です。 最低限を選択した場合は、NuGet パッケージと ASP.NET MVC の参照のみがプロジェクトに追加されます。 すべてのオプションを選択した場合、MVC プロジェクトに必要なコンテンツ ファイルと、最小の依存関係が追加されます。 スキャフォールディングを簡単に使用するには、完全な依存関係を選択します。

![完全な依存関係を選択します。](aspnet-scaffolding-overview/_static/image7.png)

依存関係を追加した後、 **readme.txt**ファイル。 慎重にプロジェクトが正しく動作するためには、このファイルの指示に従ってください。

Readme.txt ファイル」の手順を完了すると、ときに、MVC と Web API の詳細については、前のセクションで示すように新しくスキャフォールディングした項目を追加できます。 自動的に生成されたビューとコント ローラーは、プロジェクト内で正しく機能します。

## <a name="tutorials"></a>チュートリアル

カスタマイズされた scaffolder を作成するを参照してください。 [for Visual Studio カスタム Scaffolder を作成する](https://go.microsoft.com/fwlink/p/?LinkId=395029)します。

生成されたファイルをカスタマイズするを参照してください。[スキャフォールディングされた新しい項目 ダイアログ ボックスから生成されたファイルをカスタマイズする方法について](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx)します。

スキャフォールディングを使用する例については**Database First の開発**を参照してください[EF Database First と ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md)します。

スキャフォールディングを使用する例については、 **MVC**プロジェクトを参照してください[ASP.NET MVC 5 の概要](../../../mvc/overview/getting-started/introduction/getting-started.md)します。

スキャフォールディングを使用する例については、 **Web API**プロジェクトを参照してください[属性ルーティングで Web API 2 で REST API の作成](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md)です。
