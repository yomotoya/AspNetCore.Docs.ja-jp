---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.7 Web フォームと Visual Studio 2017 の概要 |Microsoft Docs
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズが ASP.NET 4.7 および Microsoft Visual Studio を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を講義します。
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341554"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>ASP.NET 4.5 Web フォームと Visual Studio 2017 の概要
====================

[Wingtip Toys のサンプル プロジェクト (C#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

このチュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio 2017 で ASP.NET Web フォーム アプリケーションを構築する方法を示します。 

## <a name="introduction"></a>はじめに

このチュートリアル シリーズでは、Visual Studio 2017 と ASP.NET 4.5 を使用して ASP.NET Web フォーム アプリケーションを作成する手順に従ってできます。 という名前のアプリケーションを作成します**Wingtip Toys** - 販売品目をオンラインの簡略化された店舗の web サイト。 このシリーズでは、中には、ASP.NET 4.5 の新機能が強調表示されます。

### <a name="target-audience"></a>対象ユーザー

ASP.NET Web フォームに慣れていない開発者は、このチュートリアル シリーズの対象者です。

ある程度の知識は、次の領域が必要です。

- オブジェクト指向プログラミング (OOP) および言語
- Web 開発 (HTML、CSS、JavaScript)
- リレーショナル データベース
- N 層アーキテクチャ

これらの領域を確認するには、次の内容を調査を検討してください。

- [Visual C# の概要](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web 開発](https://msdn.microsoft.com/beginner/bb308760.aspx)、 [HTML、CSS、JavaScript、SQL、PHP、JQuery](http://w3schools.com/)
- [リレーショナル データベース](http://en.wikipedia.org/wiki/Relational_database)
- [複数層アーキテクチャ](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>アプリケーションの機能

このシリーズに表示される ASP.NET Web フォームの機能は次のとおりです。

- Web アプリケーション プロジェクト (Web サイト プロジェクトではない)
- Web フォーム
- マスター ページの構成
- ブートス トラップ
- Entity Framework コードの最初に、LocalDB
- 要求の検証
- 厳密に型指定されたデータ コントロール
- モデル バインド
- データの注釈
- 値プロバイダー
- SSL と OAuth
- ASP.NET Identity、構成、および承認
- 控え目な検証
- ルーティング
- ASP.NET エラー処理

### <a name="application-scenarios-and-tasks"></a>アプリケーションのシナリオとタスク

チュートリアル シリーズのタスクは次のとおりです。

- 新しいプロジェクトの作成、レビュー、および
- データベース構造を作成します。
- 初期化とデータベースをシード処理
- スタイル、グラフィック、およびマスター ページの UI のカスタマイズ
- ページおよびナビゲーションの追加
- メニューの詳細と製品データを表示します。
- ショッピング カートの作成
- 追加の SSL と OAuth サポート
- 支払い方法を追加します。
- 管理者のロールとアプリケーションのユーザーを含む
- 特定のページとフォルダーへのアクセスを制限します。
- Web アプリケーションにファイルをアップロードします。
- 入力の検証を実装します。
- Web アプリケーションのルートを登録します。
- エラー処理とエラーのログ記録を実装します。

## <a name="overview"></a>概要

このチュートリアル シリーズは、プログラミングの概念を理解して他のユーザーは新しい ASP.NET Web フォームには。 ASP.NET Web フォームを使い慣れている場合は、このシリーズまだできます ASP.NET 4.5 の新機能について説明します。 プログラミングの概念と、ASP.NET Web フォームに不慣れで提供される追加の Web フォームのチュートリアルを参照してください、 [Getting Started](../../../index.md) ASP.NET Web サイトに関するセクション。

このチュートリアル シリーズで提供される ASP.NET 4.5 には、次の機能が含まれています。

- 提供するプロジェクトを作成するための単純な UI[多くの ASP.NET フレームワークのサポート](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。
- [ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウト、テーマ、および応答性の高い設計のフレームワークです。
- [ASP.NET Identity](../../../../identity/index.md)、IIS 以外のソフトウェアをホストしている web を使用したすべての ASP.NET フレームワークと動作で同様に動作する新しい ASP.NET メンバーシップ システムです。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  有効にすると、Entity Framework に更新します。
  - 取得し、厳密に型指定されたオブジェクトとしてデータを操作します。
  - データを非同期的にアクセスします。
  - 一時的な接続エラーを処理します。
  - SQL ステートメントのログ

完全な ASP.NET 4.5 機能の一覧を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../../visual-studio/overview/2013/release-notes.md)します。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys のサンプル アプリケーション

次のスクリーン ショットは、このチュートリアル シリーズで作成した ASP.NET Web フォーム アプリケーションです。 Visual Studio でアプリケーションを実行すると、次の web のホーム ページが表示されます。

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

新しいユーザーとして登録したり、既存のユーザーとしてサインインできます。 上部のナビゲーションには、データベースから製品カテゴリとその製品へのリンクがあります。

選択した場合**製品**、利用可能なすべての製品が表示されます。 

![Wingtip Toys の製品](introduction-and-overview/_static/image2.png)

特定の製品を選択した場合は、製品の詳細が表示されます。


![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してサインインできます。 このチュートリアルでは、既存の Gmail アカウントを使用してサインインする方法も説明します。 さらに、追加し、データベースから製品を削除するには、管理者としてサインインすることができます。

![Wingtip Toys のサインイン](introduction-and-overview/_static/image4.png)

ユーザーとしてサインインした後は、ショッピング カートと精算と PayPal に製品を追加できます。 サンプル アプリケーションは、PayPal の開発者のサンド ボックスで作業する設計されています。 実際の金額のトランザクションは実行されません。

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

PayPal は、アカウント、注文、支払情報を確認します。

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

PayPal から返された後、は、確認し、注文を完了できます。

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、次のソフトウェアがコンピューターにインストールされていることを確認します。

- [Microsoft Visual Studio 2017 または Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/)します。

.NET Framework は、自動的にインストールされます。

このチュートリアル シリーズでは、Microsoft Visual Studio Community 2017 を使用します。 ボリュームを使用しているか、またはこのチュートリアル シリーズを完了する、Microsoft Visual Studio 2017。

Visual Studio については、次に注意してください。

* Microsoft Visual Studio 2017 および Microsoft Visual Studio Community 2017 と呼びます*Visual Studio*このチュートリアル シリーズ全体でします。

* Visual Studio 2017 が既にインストールされている古いバージョンの横にあるインストールされます。 以前のバージョンで作成されたサイトでは、Visual Studio 2017 で開くことができ、以前のバージョンを開くに進みます。

* 初めて Visual Studio を開始した、選択したと見なされます、 *Web 開発*設定します。 詳細については、「[方法 :Web 開発環境の設定を選択して](https://msdn.microsoft.com/library/ff521558.aspx)します。

前提条件をインストールすると、このチュートリアル シリーズで表示される Web プロジェクトの作成を開始する準備ができました。

## <a name="download-the-sample-application"></a>サンプル アプリケーションをダウンロードします。

 MSDN のサンプル サイトからの完全なサンプル applicatiion をいつでもダウンロードできます。

[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) 

 このダウンロードには、次のものがあります。

- サンプル アプリケーションは、 *WingtipToys*フォルダー。
- サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*フォルダーで、 *WingtipToys*フォルダー。

ダウンロードが、 *.zip*ファイル。 このチュートリアル シリーズを作成する完成したプロジェクト、検索と選択して、 *C#* .zip ファイル内のフォルダー。 保存、C#を使用する Visual Studio プロジェクトと作業フォルダーのフォルダー。 既定では、Visual Studio 2017 のプロジェクト フォルダーは次のとおりです。

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

名前を変更、 ***C#*** フォルダー ***WingtipToys***です。

> [!NOTE]
> という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *C#* フォルダー *WingtipToys*です。

完成したプロジェクトを実行するには、開く、 *WingtipToys*フォルダーをダブルクリックします、 *WingtipToys.sln*ファイル。 Visual Studio 2017 では、プロジェクトを開きます。 次に、右クリックし、 *Default.aspx*ファイル**ソリューション エクスプ ローラー**選択**ブラウザーで表示**します。

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>ASP.NET Web フォーム クイズにコンテンツを確認するには

チュートリアルのシリーズを完了すると、知識をテストし、主要な概念を強調するクイズに挑戦します。 各質問は、説明とその他のガイダンスへのリンクを提供します。

 * [ASP.NET Web フォームのクイズ](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>チュートリアルのサポートとコメント

質問やコメントは、使用、Q & A のセクションに含まれています、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル ページ。

このチュートリアル シリーズのコメントは、ようこそ。 このチュートリアル シリーズを更新すると、機能強化に関する修正または提案を検討する努力が行われます。

かどうか、エラーが発生した、対応するエラー メッセージが、混乱を招く可能性がありますでないその修正方法を説明します。 ヘルプについては、確認することができます、 [ASP.NET フォーラム](https://forums.asp.net/)します。 別の適切なソースは、Q & A のセクションで、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル ページ。 

> [!div class="step-by-step"]
> [次へ](create-the-project.md)
