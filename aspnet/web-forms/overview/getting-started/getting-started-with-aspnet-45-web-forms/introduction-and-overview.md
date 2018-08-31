---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要 |Microsoft Docs
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio の表現を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 8e3ae964dafc73bdf703cd7cbab430bbc99a6188
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336025"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

このステップ バイ ステップ チュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET)  

## <a name="introduction"></a>はじめに

この一連のチュートリアルに従って、Web アプリケーションおよび ASP.NET 4.5 用の Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションを作成するために必要な手順を実行します。

作成するアプリケーションの名前は**Wingtip Toys**します。 オンラインのアイテムを販売するストア フロントの web サイトの簡略化された例です。 このチュートリアル シリーズには、ASP.NET 4.5 の新機能が強調表示されます。

コメントは、[ようこそ] と、このチュートリアル シリーズのご提案に基づいてを更新するには、あらゆる努力をしましょう。

### <a name="download-completed-project"></a>プロジェクトのダウンロードが完了しました

完了したチュートリアルを含む c# プロジェクトをダウンロードすることができます。

- [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>関連する ASP.NET Web フォームのクイズを実行してコンテンツを確認します。

このチュートリアルを完了すると、知識をテストしを実行して重要な概念を補足、 [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)します。 このクイズは、このチュートリアル シリーズに含まれるコンテンツからに設計されています。 クイズの各項目について追加のガイダンスへのリンクと共に説明します。

- [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>対象ユーザー

初心者に ASP.NET Web フォームは経験豊富な開発者はこのチュートリアル シリーズの対象とします。 開発者はこのチュートリアル シリーズでは、次のスキルを持つ必要があります。

- 使い慣れたオブジェクト指向プログラミング (OOP) 言語
- 経験のある Web 開発の概念 (HTML、CSS、JavaScript)
- リレーショナル データベースの概念を理解しています。
- N 層アーキテクチャの概念を理解

上記の領域を確認したい場合は、次の内容の確認を検討してください。

- [Visual c# の概要](https://msdn.microsoft.com/library/a72418yk.aspx)
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
- 厳密に型指定されたデータ コントロールでは、モデルのバインディング、データの注釈と値のプロバイダー
- SSL と OAuth
- ASP.NET Identity、構成、および承認
- 控え目な検証
- ルーティング
- ASP.NET エラー処理

### <a name="application-scenarios-and-tasks"></a>アプリケーションのシナリオとタスク

このシリーズで説明するタスクは次のとおりです。

- 新しいプロジェクトを実行して作成し、レビュー
- データベース構造を作成します。
- 初期化とデータベースのシード処理
- スタイル、グラフィックス、およびマスター ページを使用して UI のカスタマイズ
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

ASP.NET Web フォームに慣れていない場合がプログラミングの概念に関する知識があるが、適切なチュートリアルがあります。 ASP.NET Web フォーム理解している場合は、ASP.NET 4.5 の新機能によって、このチュートリアル シリーズを活用できます。 プログラミングに関する概念および ASP.NET Web フォームに慣れていない場合は、Web フォームで提供される追加のチュートリアルを参照してください[Getting Started](../../../index.md) ASP.NET Web サイトに関するセクション。

特定**最新**チュートリアル シリーズの次のとおりこの Web フォームで提供される ASP.NET 4.5 の機能します。

- 作成するための単純な UI がそのオファーをプロジェクト[複数の ASP.NET フレームワークのサポート](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。
- [ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウトとテーマのフレームワークで、応答性の高いデザインおよびテーマの適用の機能を提供します。
- [ASP.NET Identity](../../../../identity/index.md)、IIS 以外のソフトウェアをホストしている web を使用したすべての ASP.NET フレームワークと動作で同様に動作する新しい ASP.NET メンバーシップ システムです。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)型指定されたオブジェクトを取得し、厳密にデータを操作できる Entity Framework の更新プログラム、データへのアクセスは、非同期的に一時的な接続エラーのエラーを処理し、SQL ステートメントを記録します。

ASP.NET 4.5 機能の完全な一覧を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 リリース ノート](../../../../visual-studio/overview/2013/release-notes.md)します。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys のサンプル アプリケーション

次のスクリーン ショットでは、このチュートリアル シリーズで作成する ASP.NET Web フォーム アプリケーションの簡易ビューを提供します。 Visual Studio Express 2013 for Web からアプリケーションを実行する場合は、次の web のホーム ページが表示されます。

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

新しいユーザーとして登録したり、既存のユーザーとしてログインできます。 ナビゲーションは、データベースから使用可能な製品を取得することによって各製品カテゴリの上部で提供されます。

製品リンクを選択すると、すべての利用可能な製品の一覧を表示することができます。

![Wingtip Toys の製品](introduction-and-overview/_static/image2.png)

以下の製品のいずれかを選択して個々 の製品の詳細を確認できます。

![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してログインできます。 このチュートリアルでは、既存の Gmail アカウントを使用してログインする方法も説明します。 さらに、追加し、データベースから製品を削除するには、管理者としてログインすることができます。

![Wingtip Toys をログインします。](introduction-and-overview/_static/image4.png)

ユーザー ログインすると、ショッピング カートと PayPal によるチェック アウトする製品を追加できます。 このサンプル アプリケーションが PayPal の開発者のサンド ボックスで機能するように設計することに注意してください。 実際の金額のトランザクションは行われません。

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

PayPal は、アカウント、順序、および支払い情報を確認します。

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

PayPal から返された後、は、確認し、注文を完了できます。

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている、次のソフトウェアがあることを確認します。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。 .NET Framework は、自動的にインストールされます。

このチュートリアル シリーズでは、Web の Microsoft Visual Studio Express 2013 を使用します。 Microsoft Visual Studio Express 2013 for Web、または Microsoft Visual Studio 2013 のいずれかを使用して、このチュートリアル シリーズを完了することができます。

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。


Visual Studio のバージョンがインストールされている、既にある場合は、インストール プロセスが、既存のバージョンの横にあるに Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web をインストールします。 以前のバージョンで作成したサイトでは、Visual Studio 2013 で開くことし、以前のバージョンで開きます。

> [!NOTE] 
> 
> このチュートリアルでは、選択した、 *Web 開発*設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。


## <a name="download-the-sample-application"></a>サンプル アプリケーションをダウンロードします。

前提条件をインストールすると、このチュートリアル シリーズで表示される、新しい Web プロジェクトの作成を開始する準備が整いました。 たい場合**必要に応じて**このチュートリアル シリーズを作成するサンプル アプリケーションを実行するからダウンロードできますが、MSDN のサンプル サイト。 このダウンロードには、次のものが含まれています。

- サンプル アプリケーションは、 *WingtipToys*フォルダー。
- サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*フォルダーで、 *WingtipToys*フォルダー。

#### <a name="download-the-file-from-msdn-samples-site"></a>MSDN のサンプル サイトからファイルをダウンロードします。

[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#) 

ダウンロードが、 <em>.zip</em>ファイル。 このチュートリアル シリーズを作成する完成したプロジェクト、検索と選択して、 <em>c#</em>フォルダーで、 <em>.zip</em>ファイル。 保存、 <em>c#</em>フォルダー、フォルダーを使用して、Visual Studio 2013 プロジェクトを操作します。 既定では、次に、Visual Studio 2013 のプロジェクト フォルダー示します。

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013 \projects</strong>

名前を変更、 ***c#*** フォルダー ***WingtipToys***です。

> [!NOTE]
> という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *c#* フォルダー *WingtipToys*です。


完成したプロジェクトを実行するには、開く、 *WingtipToys*フォルダーをダブルクリックします、 *WingtipToys.sln*ファイル。 Visual Studio 2013 には、プロジェクトが開きます。 次に、右クリックし、 *Default.aspx*ソリューション エクスプ ローラー ウィンドウでファイルを開き、右クリック メニューからブラウザーで表示をクリックします。

### <a name="tutorial-support-and-comments"></a>チュートリアルのサポートとコメント

付属の Q &AMP; A セクションを使用して、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)質問またはコメントのサンプル (c#)。

このチュートリアル シリーズのコメントは、[ようこそ]、およびあらゆる努力をアカウントの修正または提案のチュートリアルのコメントに用意されている改善点を考慮できるようにこのチュートリアル シリーズが更新されたときにします。

開発中にエラーが発生したとき、または Web サイトが正しく実行されない場合は、エラー メッセージは、問題の原因に複雑な手がかりを与える可能性がありますか、その修正方法については説明があります。 一般的な問題のシナリオで役立つするには、使用の[ASP.NET フォーラム](https://forums.asp.net/)またはに付属の Q &AMP; A セクション、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys 概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプル。 エラー メッセージを取得する、または、チュートリアルを進めるときに機能しない、必ず上記の場所を確認してください。

> [!div class="step-by-step"]
> [次へ](create-the-project.md)
