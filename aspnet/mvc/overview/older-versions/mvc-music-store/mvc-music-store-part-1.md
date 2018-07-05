---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'パート 1: 概要と新しいプロジェクト]-> [ファイル |Microsoft Docs'
author: jongalloway
description: このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 第 1 部では概要とファイルには、新しいプロジェクト]-> [です。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 57856a4a78a650e4abe872004e5be5f8f3b2dbcd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816344"
---
<a name="part-1-overview-and-file-new-project"></a>パート 1: 概要と新しいプロジェクト]-> [ファイル
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC のミュージック ストアは、チュートリアル アプリケーションを紹介し、web 開発用の ASP.NET MVC と Visual Studio を使用する方法をステップ バイ ステップについて説明します。  
>   
> MVC のミュージック ストアは、オンラインで音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインし、買い物カゴの機能を実装する軽量サンプル ストア実装です。  
>   
> このチュートリアル シリーズでは、すべての ASP.NET MVC のミュージック ストア サンプル アプリケーションをビルドする手順について説明します。 パート 1 では、概要とファイルの&gt;新しいプロジェクト。


## <a name="overview"></a>概要

MVC のミュージック ストアは、紹介し、web 開発用の ASP.NET MVC と Visual Web Developer を使用する方法を順を追って説明するチュートリアル アプリケーションです。 私たちはゆっくりと開始されます、初心者レベルの web 開発のエクスペリエンスは問題ありません。

ビルドするアプリケーションは、単純な音楽ストアです。 アプリケーションに 3 つの主要な部分がある: ショッピング、チェック アウト、および管理します。

![](mvc-music-store-part-1/_static/image1.jpg)

訪問者には、ジャンルのアルバムを参照できます。

![](mvc-music-store-part-1/_static/image2.jpg)

1 つのアルバムを表示およびカートに追加できます。

![](mvc-music-store-part-1/_static/image3.jpg)

必要がなくなったすべての項目を削除する、カートを確認することができます。

![](mvc-music-store-part-1/_static/image4.jpg)

チェック アウトに進むには、ログインまたはユーザー アカウントを登録することを求められます。

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

アカウントを作成すると、配布と支払い情報を入力して、注文を完了します。 すばらしい昇格実行をことをシンプルにする: すべてがプロモーション コード"FREE"を入力している場合は、無料!

![](mvc-music-store-part-1/_static/image5.jpg)

、順序付けの後に、単純な確認画面が参照してください。

![](mvc-music-store-part-1/_static/image6.jpg)

だけでなく、顧客 faceing ページは、アルバムをから管理者を作成できます、編集の一覧を表示する管理者セクションを構築し、アルバムを削除しましたがも。

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1.ファイル -&gt;新しいプロジェクト

### <a name="installing-the-software"></a>ソフトウェアのインストール

このチュートリアルでは、無料 Visual Web Developer 2010 Express (これは無料) を使用して新しい ASP.NET MVC 3 プロジェクトの作成、まず、差分を追加してみます完全に機能しているアプリケーションを作成する機能。 その過程について説明しますデータベースへのアクセス、フォーム ポスト シナリオ、データの検証、ページ更新と検証、ユーザーのログイン、および AJAX を使用して、一貫性のあるページ レイアウトのマスター ページを使用します。

ステップ バイ ステップに従うことができますか、完成したアプリケーションをダウンロードできます[MVC-ミュージック ストア](https://github.com/evilDave/MVC-Music-Store)します。

Visual Studio 2010 SP1 または Visual Web Developer 2010 Express SP1 (Visual Studio 2010 の無料版) のいずれかを使用するには、アプリケーションをビルドします。 使用する、SQL Server Compact (も無料)、データベースをホストします。 始める前に、以下の前提条件がインストールされていることを確認します。


- [Visual Studio Web Developer Express SP1 の前提条件]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0] - ランタイムとツールの両方のサポートを含む


### <a name="creating-a-new-aspnet-mvc-3-project"></a>新しい ASP.NET MVC 3 プロジェクトを作成します。

まず、Visual Web Developer で [ファイル] メニューから [新しいプロジェクト] を選択します。 新しいプロジェクト ダイアログが表示されます。

![](mvc-music-store-part-1/_static/image5.png)

選択、Visual c# -&gt; Web テンプレートは、左側のグループ化し、中央の列で、「ASP.NET MVC 3 Web アプリケーション」テンプレートを選択します。 MvcMusicStore をプロジェクトに名前を [ok] ボタンを押します。

![](mvc-music-store-part-1/_static/image8.jpg)

これにより、特定の MVC プロジェクトの設定を作成することが可能セカンダリ ダイアログが表示されます。 次を選択します。

プロジェクト テンプレート - 空を選択します。

エンジンの表示 - Razor を選択します。

HTML5 セマンティック マークアップ - チェックの使用します。

設定は、次に示すように、[ok] ボタンを押して確認します。

![](mvc-music-store-part-1/_static/image9.jpg)

これにより、プロジェクトが作成されます。 右側にある ソリューション エクスプ ローラーで、アプリケーションに追加されたフォルダーを見ていきましょう。

![](mvc-music-store-part-1/_static/image10.jpg)

空の MVC 3 のテンプレートが完全に空でない – 基本的なフォルダー構造を追加します。

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC フォルダー名をいくつかの基本的な名前付け規則の使用します。

| **フォルダー** | **目的** |
| --- | --- |
| **/コント ローラー** | コント ローラーは、ブラウザーからの入力を操作、およびユーザーへの応答を返す方法を決めるに応答します。 |
| **/Views** | ビューは、UI テンプレートを保持します。 |
| **/モデル** | モデルの保持し、データの操作 |
| **/Content** | このフォルダーは、イメージ、CSS、およびその他の静的コンテンツを保持します。 |
| **/Scripts** | このフォルダーは、JavaScript ファイルを保持します。 |

既定では、ASP.NET MVC フレームワークは、「設定より規約」の手法を使用し、フォルダーの名前付け規則に基づくいくつかの既定の解釈は、ために、これらのフォルダーが空の ASP.NET MVC アプリケーションであっても含まれます。 たとえば、コント ローラーを検索、Views フォルダー内のビュー既定では、コードでこれを明示的に指定する必要がありません。 既定の規則を使用することでコードを記述する必要がある量を削減できますも容易にできるように、プロジェクトを理解するには、他の開発者とします。 これらの規則は、アプリケーションの構築に詳しく説明します。

> [!div class="step-by-step"]
> [次へ](mvc-music-store-part-2.md)
