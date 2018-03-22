---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要 |Microsoft ドキュメント
author: Erikre
description: このステップ バイ ステップ チュートリアル シリーズでは、ASP.NET 4.5 と Microsoft Visual Studio の有効期限を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このステップ バイ ステップ チュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> 知識をテストし、ASP.NET Web フォーム Quiz 実行することで重要な概念を補足します。 このチュートリアルのシリーズに含まれるコンテンツからこのクイズが用意されました。 クイズの各質問では、追加のガイダンスへのリンクと共に説明します。


## <a name="introduction"></a>はじめに

この一連のチュートリアルでは、Web と ASP.NET 4.5 の Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションを作成するための手順を説明します。

作成するアプリケーションの名前が**Wingtip Toys**です。 オンラインの項目を販売しているフロント ストアの web サイトの簡単な例です。 このチュートリアルのシリーズには、ASP.NET 4.5 の新機能が強調表示されます。

コメントへようこそ は、に関するご意見に基づいてこのチュートリアルのシリーズを更新するには、あらゆる努力がしましょう。

### <a name="download-completed-project"></a>ダウンロードが完了したプロジェクト

完了したチュートリアルを含む c# プロジェクトをダウンロードすることができます。

- [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>関連する ASP.NET Web フォーム クイズを実行してコンテンツを確認します。

このチュートリアルを完了した後、ナレッジをテストし、実行することで重要な概念を補足、 [ASP.NET Web フォーム Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001)です。 このチュートリアルのシリーズに含まれるコンテンツからこのクイズが用意されました。 クイズの各質問では、追加のガイダンスへのリンクと共に説明します。

- [ASP.NET Web フォームのクイズ](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>対象ユーザー

このチュートリアルの一連の対象読者は、ASP.NET Web フォームに追加された新しい経験を積んだ開発者です。 開発者はこのチュートリアル シリーズの目的は、次のスキルを持つ必要があります。

- 経験のあるオブジェクト指向プログラミング (OOP) 言語
- 経験のある Web 開発の概念 (HTML、CSS、JavaScript)
- リレーショナル データベースの概念を理解しています。
- N 層アーキテクチャの概念を理解しています。

上記の領域を確認する場合は、次の内容を確認することを検討してください。

- [Visual c# の概要](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Web 開発](https://msdn.microsoft.com/beginner/bb308760.aspx)、 [HTML、CSS、JavaScript、SQL、PHP、JQuery](http://w3schools.com/)
- [リレーショナル データベース](http://en.wikipedia.org/wiki/Relational_database)
- [多層アーキテクチャ](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>アプリケーションの機能

この系列に表示される ASP.NET Web フォームの機能は次のとおりです。

- Web アプリケーション プロジェクト (Web サイト プロジェクトではなく)
- Web フォーム
- マスター ページの構成
- ブートス トラップ
- Entity Framework コードの最初に、LocalDB
- 要求の検証
- バインディング、データ注釈をモデルし、値のプロバイダー データ コントロールを厳密に型指定
- SSL と OAuth
- ASP.NET の Id、構成、および承認
- 控えめな検証
- ルーティング
- ASP.NET エラー処理

### <a name="application-scenarios-and-tasks"></a>アプリケーションのシナリオとタスク

この系列に示されているタスクは次のとおりです。

- 新しいプロジェクトを実行して作成し、レビュー
- データベース構造を作成します。
- 初期化し、データベースのシード
- スタイル、グラフィックス、およびマスター ページを使用して UI をカスタマイズします。
- ページとナビゲーションを追加します。
- メニューの詳細と製品データを表示します。
- ショッピング カートを作成します。
- 追加の SSL と OAuth のサポート
- 支払方法を追加します。
- 管理者ロールおよびアプリケーションのユーザーを含む
- 特定のページとフォルダーへのアクセスを制限します。
- Web アプリケーションへのファイルのアップロード
- 入力の検証を実装します。
- Web アプリケーションのルートを登録します。
- エラー処理およびエラーのログ記録を実装します。

## <a name="overview"></a>概要

ASP.NET Web フォームに慣れていない場合、プログラミングの概念に関する知識が右のチュートリアルです。 ASP.NET Web フォームを使い慣れている場合は、ASP.NET 4.5 の新機能によって、このチュートリアルのシリーズを活用できます。 プログラミングに関する概念および ASP.NET Web フォームに慣れていない場合は、Web フォームで提供される追加のチュートリアルを参照してください[作業の開始](../../../index.md)セクションの ASP.NET Web サイトです。

特定**最新**ASP.NET 4.5 機能がこの Web フォームで一連のチュートリアルについて、次のとおり指定します。

- 作成するための単純な UI がその提供をプロジェクト[の複数の ASP.NET フレームワークをサポートして](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add)(Web フォーム、MVC、および Web API)。
- [ブートス トラップ](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)レイアウトとテーマのフレームワークで、応答性の高いデザインとテーマの機能を提供します。
- [ASP.NET Identity](../../../../identity/index.md)、新しい ASP.NET メンバーシップ システム web ホスティング IIS 以外のソフトウェアと連携するすべての ASP.NET フレームワークと動作で同じです。
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)型指定されたオブジェクトを取得して、厳密にデータを操作するにより、Entity Framework の更新、データへのアクセスは、非同期的に一時的な接続エラーを処理し、SQL ステートメントを記録します。

ASP.NET 4.5 機能の一覧については、次を参照してください。 [ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート](../../../../visual-studio/overview/2013/release-notes.md)です。

### <a name="the-wingtip-toys-sample-application"></a>Wingtip Toys のサンプル アプリケーション

次のスクリーン ショットは、このチュートリアルの系列で作成する ASP.NET Web フォーム アプリケーションの簡易ビューを提供します。 Visual Studio Express 2013 for Web からアプリケーションを実行するときに、次の web のホーム ページが表示されます。

![Wingtip Toys の既定のページ](introduction-and-overview/_static/image1.png)

新しいユーザーとして登録したり、既存のユーザーとしてログインできます。 ナビゲーションは、データベースから使用可能な製品を取得することによって各製品カテゴリの最上部で提供されます。

製品のリンクを選択すると、使用可能なすべての製品の一覧を表示することができます。

![Wingtip Toys - 製品](introduction-and-overview/_static/image2.png)

表示されている製品のいずれかを選択して個々 の製品の詳細を表示できます。

![Wingtip Toys の製品の詳細](introduction-and-overview/_static/image3.png)

ユーザーは、登録し、Web フォーム テンプレートの既定の機能を使用してログインできます。 このチュートリアルでは、gmail などの既存のアカウントを使用してログインする方法についても説明します。 さらに、追加し、データベースから製品を削除するには、管理者としてログインすることができます。

![Wingtip Toys をログインします。](introduction-and-overview/_static/image4.png)

いったんユーザーとしてログインが、PayPal のチェック アウト、ショッピング カートに製品を追加できます。 このサンプル アプリケーションが PayPal の開発者のサンド ボックスで動作するように設計することに注意してください。 実際の money トランザクションは行われません。

![Wingtip Toys のショッピング カート](introduction-and-overview/_static/image5.png)

PayPal は、アカウント、順序、お支払い情報を確認します。

![Wingtip Toys の PayPal](introduction-and-overview/_static/image6.png)

PayPal から返された後、は、確認し、注文を完了できます。

![Wingtip Toys の注文の確認](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>必須コンポーネント

開始する前に、コンピューターにインストールされている次のソフトウェアがあることを確認します。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。 .NET Framework は、自動的にインストールされます。

この一連のチュートリアルについては、Web の Microsoft Visual Studio Express 2013 を使用します。 このチュートリアルのシリーズを完了するのに、Microsoft Visual Studio Express 2013 for Web か、Microsoft Visual Studio 2013 を使用することができます。

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web が多くの場合、参照されます Visual Studio としてこのチュートリアルの系列全体です。


インストールされている Visual Studio のバージョンが既にあるを場合は、インストール プロセスが、既存のバージョンの横にあるに Visual Studio 2013 または Microsoft Visual Studio Express 2013 for Web をインストールします。 以前のバージョンで作成したサイトは、Visual Studio 2013 で開くことができ、以前のバージョンを開くに進みます。

> [!NOTE] 
> 
> このチュートリアルでは、選択されている、 *Web 開発*設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[する方法: Web 開発環境設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)です。


## <a name="download-the-sample-application"></a>サンプル アプリケーションをダウンロードします。

前提条件をインストールすると、このチュートリアルの系列には、表示される新しい Web プロジェクトの作成を開始する準備ができたらです。 たい場合**必要に応じて**このチュートリアルのシリーズを作成するサンプル アプリケーションを実行する、サイトからダウンロードできる、MSDN Samples です。 このダウンロードには、次のものが含まれています。

- サンプル アプリケーション、 *WingtipToys*フォルダーです。
- サンプル アプリケーションの作成に使用されるリソース、 *WingtipToys 資産*内のフォルダー、 *WingtipToys*フォルダーです。

#### <a name="download-the-file-from-msdn-samples-site"></a>MSDN サンプルのサイトからファイルをダウンロードします。

[ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(c#) 

ダウンロードが、 *.zip*ファイル。 このチュートリアルのシリーズを作成する、完成したプロジェクトで検索と選択を表示する、 *c#*内のフォルダー、 *.zip*ファイル。 保存、 *c#*フォルダーに Visual Studio 2013 プロジェクトの作業を使用するフォルダーです。 既定では、Visual Studio 2013 プロジェクト フォルダーは次のよう

**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**

名前を変更、 ***c#***フォルダー ***WingtipToys***です。

> [!NOTE]
> という名前のフォルダーが既にある場合*WingtipToys* Projects フォルダーに名前を一時的にその既存のフォルダーの名前変更する前に、 *c#*フォルダー *WingtipToys*です。


実行するには、完成したプロジェクトを開く、 *WingtipToys*フォルダーをダブルクリック、 *WingtipToys.sln*ファイル。 Visual Studio 2013 には、プロジェクトが開きます。 次を右クリックし、 *Default.aspx*ソリューション エクスプ ローラー ウィンドウでファイルを右クリック メニューからブラウザーで表示をクリックします。

### <a name="tutorial-support-and-comments"></a>チュートリアルのサポートとコメント

含まれている Q &AMP; A セクションを使用して、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)質問またはコメントのサンプル (C# の場合)。

このチュートリアル シリーズのコメントへようこそ は、このチュートリアルのシリーズが更新されたときにすべての作業量になりますチュートリアルのコメントに用意された機能強化に関するアカウントの修正または提案を考慮します。

開発中は、エラーの発生時、または Web サイトが正しく実行されない場合は、エラー メッセージは、問題の原因に複雑な手掛かりを与える可能性があります。 または、その修正方法については説明があります。 問題の一般的なシナリオで役立つ、使用することも、 [ASP.NET フォーラム](https://forums.asp.net/)または Q &AMP; A セクションに含まれている、 [ASP.NET 4.5 Web フォームと Visual Studio 2013 - Wingtip Toys の概要](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)(C#) サンプルです。 エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、上記の場所を確認することを確認します。

>[!div class="step-by-step"]
[次へ](create-the-project.md)
