---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: "パート 1: 概要と新しいプロジェクト]-> [ファイル |Microsoft ドキュメント"
author: jongalloway
description: "このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 パート 1 カバーの概要とファイルには、新しいプロジェクト]-> [です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1e3373a21c7d1766cfad390a7ba68b1363d8d895
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-1-overview-and-file-new-project"></a>パート 1: 概要と新しいプロジェクト]-> [ファイル
====================
によって[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Studio web 開発を使用する方法の詳細な手順について説明します。  
>   
> MVC 音楽ストアは、オンラインにして、音楽のアルバムを販売し、基本的なサイトの管理、ユーザー サインインおよび買い物カゴの機能を実装する軽量なサンプル ストアの実装です。  
>   
> このチュートリアルの系列では、すべての ASP.NET MVC Music Store サンプル アプリケーションをビルドする手順について説明します。 第 1 部は、概要ファイル-&gt;新しいプロジェクト。


## <a name="overview"></a>概要

MVC Music Store は、チュートリアル アプリケーションを紹介し、ASP.NET MVC と Visual Web Developer web 開発を使用する方法の詳細な手順について説明します。 お緩やかに変化を開始することを初級レベルの web 開発が発生するように問題ありません。

構築するアプリケーションは、単純な音楽ストアです。 アプリケーションに 3 つの主要な部分がある: ショッピング、チェック アウト、および管理します。

![](mvc-music-store-part-1/_static/image1.jpg)

閲覧者は、ジャンルのアルバムを参照できます。

![](mvc-music-store-part-1/_static/image2.jpg)

1 枚のアルバムを表示し、カートに追加することができます。

![](mvc-music-store-part-1/_static/image3.jpg)

必要がなくなったすべての項目を削除する、カートを確認することができます。

![](mvc-music-store-part-1/_static/image4.jpg)

チェック アウトに進むには、ログインまたはユーザー アカウントの登録にそれらを求められます。

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

アカウントを作成すると、配布と支払い情報を入力して、注文を完了することができます。 煩雑にならないように、驚くほどの昇格を実行している: すべての設定が「無料」のプロモーション コードを入力している場合は無料です。

![](mvc-music-store-part-1/_static/image5.jpg)

、順序付けした後に、単純な確認画面が参照してください。

![](mvc-music-store-part-1/_static/image6.jpg)

顧客 faceing ページだけでなくおをもアルバムの元の管理者を作成、編集の一覧を表示する管理者」セクションをビルドし、アルバムを削除します。

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1.ファイル -&gt;新しいプロジェクト

### <a name="installing-the-software"></a>ソフトウェアのインストール

このチュートリアルは、空き Visual Web Developer 2010 Express (これは無料) を使用して新しい ASP.NET MVC 3 プロジェクトを作成することで開始され、おを増分追加機能を完全に機能しているアプリケーションを作成します。 その過程では、ここデータベースへのアクセス、フォーム ポスト シナリオ、データの検証、ページ更新し、検証、ユーザーのログイン、および AJAX を使用して一貫したページ レイアウトのマスター ページを使用します。

順を追っていくことができますをまたはから完成したアプリケーションをダウンロードする[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)です。

Visual Studio 2010 SP1 または Visual Web Developer 2010 Express SP1 (無料版の Visual Studio 2010) のいずれかを使用するには、アプリケーションをビルドします。 使用する、SQL Server Compact (も無料) データベースをホストします。 開始する前に、以下に示す前提条件がインストールされていることを確認してください。


- [Visual Studio Web Developer Express SP1 の前提条件]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0] - ランタイムとツールの両方のサポートを含む


### <a name="creating-a-new-aspnet-mvc-3-project"></a>新しい ASP.NET MVC 3 プロジェクトを作成します。

まず、Visual Web Developer で [ファイル] メニューから新しいプロジェクトの""を選択します。 新しいプロジェクト ダイアログが表示されます。

![](mvc-music-store-part-1/_static/image5.png)

ここを選択すると、Visual c# で&gt;Web テンプレートは、左側のグループ化し、中央の列で、「ASP.NET MVC 3 Web アプリケーション」テンプレートを選択します。 MvcMusicStore、プロジェクトの名前を [ok] ボタンを押します。

![](mvc-music-store-part-1/_static/image8.jpg)

これにより、特定の MVC プロジェクトの設定を作成することが可能セカンダリ ダイアログが表示されます。 次を選択します。

プロジェクト テンプレート-空の選択

エンジンの表示 - Razor を選択

HTML5 セマンティック マークアップのチェックを使用します。

設定が、次のようには、し、[ok] ボタンを押すことを確認します。

![](mvc-music-store-part-1/_static/image9.jpg)

これにより、プロジェクトが作成されます。 ソリューション エクスプ ローラーの右側にあるアプリケーションに追加されているフォルダーを見てをみましょう。

![](mvc-music-store-part-1/_static/image10.jpg)

空の MVC 3 のテンプレートが完全に空でない – 基本的なフォルダー構造を追加します。

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC フォルダー名をいくつかの基本的な名前付け規則を利用します。

| **フォルダー** | **目的** |
| --- | --- |
| **/コント ローラー** | コント ローラーは、ブラウザーからの入力を操作を実行、およびユーザーへの応答を返すを決めてに応答します。 |
| **/ビュー** | ビューは、UI のテンプレートを保持します。 |
| **/モデル** | モデルを保持およびデータの操作 |
| **/コンテンツ** | このフォルダーは、イメージ、CSS、およびその他の静的なコンテンツを保持します。 |
| **/スクリプト** | このフォルダーの JavaScript ファイルを保存します。 |

既定では、ASP.NET MVC フレームワークは、「設定よりも規約」アプローチが使用され、いくつかのフォルダーの名前付け規則に基づく既定の前提条件ために、これらのフォルダーは空の ASP.NET MVC アプリケーションであっても含まれています。 たとえば、コント ローラーを検索ビュー フォルダー内のビュー既定では、コードでこれを明示的に指定する必要はありません。 書き込むには、必要なコードの量を削減する既定の規則に移れなくなることができますも容易にできるように、プロジェクトを理解するには、他の開発者とします。 これらのアプリケーションを構築するには、複数の規則を用いて説明します。

>[!div class="step-by-step"]
[次へ](mvc-music-store-part-2.md)
