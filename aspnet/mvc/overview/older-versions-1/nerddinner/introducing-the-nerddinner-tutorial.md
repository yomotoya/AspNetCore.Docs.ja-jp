---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner チュートリアルの紹介 |Microsoft Docs
author: shanselman
description: 新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。 このチュートリアル ASP.NE を使用して、サイズは小さいが完了すると、アプリケーションを構築する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 735a775752beda3fc24852422b1ff9d60a7c46b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371629"
---
<a name="introducing-the-nerddinner-tutorial"></a>NerdDinner チュートリアルの紹介
====================
によって[Scott Hanselman](https://github.com/shanselman)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。 このチュートリアルでは、小さなをビルドしても、ASP.NET MVC 1 を使用してアプリケーションを実行する方法について説明し、その背後にある主要な概念の一部が導入されています。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-tutorial"></a>NerdDinner チュートリアル

新しいフレームワークを学習する最善の方法では、何らかの処理を構築します。 このチュートリアルでは、小さなをビルドしても、ASP.NET MVC を使用してアプリケーションを実行する方法について説明し、その背後にある主要な概念の一部が導入されています。

アプリケーションをビルドするには、"NerdDinner"が呼び出されます。 NerdDinner では、ユーザーの検索し、整理の dinners をオンラインに簡単な方法を提供します。

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner では、登録済みユーザーを作成、編集、および dinners を削除できるようにします。 アプリケーション全体で一貫性のある一連の検証とビジネス ルールを適用します。

![](introducing-the-nerddinner-tutorial/_static/image2.png)

訪問者は、AJAX ベースのマップを使用して、近くに保持されている次回の dinners を検索できます。

![](introducing-the-nerddinner-tutorial/_static/image3.png)

クリックすると、dinner は持ち運ぶ詳細ページに学ぶことができる詳細については。

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Dinner への参加に興味を持つ場合は、ログインまたはサイトに登録できます。

![](introducing-the-nerddinner-tutorial/_static/image5.png)

イベントに出席する RSVP の AJAX ベースのリンクをクリックしています。

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>NerdDinner を実装します。

ファイル-を使用して、NerdDinner アプリケーションを開始しようとして&gt;Visual Studio 内で新しい ASP.NET MVC プロジェクトを作成する新しいプロジェクト コマンド。 私たちの機能と機能増分追加します。 その過程について説明します。

1. [新しい ASP.NET MVC プロジェクトを作成する方法](# "新しい ASP.NET MVC プロジェクトの作成")
2. [データベースを作成する方法](# "データベースを作成します。")
3. [ビジネス ルール検証でモデルを構築する方法](# "ビジネス ルール検証とモデルの構築")
4. [コント ローラーとビューを使用して、リスティング/詳細 UI を実装する方法](# "リスティング/詳細 UI を実装するを使用して、コント ローラーとビュー")
5. [CRUD を提供する方法 (作成、読み取り、更新、削除) データ フォーム エントリ サポート](# "提供の CRUD (作成、読み取り、更新、削除) データ フォーム エントリ サポート")
6. [ViewData を使用し、ViewModel クラスを実装する方法](# "ViewData を使用し、ViewModel クラスの実装")
7. [マスター ページおよびパーシャルを使用して UI を再利用する方法](# "UI を使用してマスター ページの再利用およびパーシャル")
8. [効率的なデータ ページングを実装する方法](# "実装効率的なデータ ページング")
9. [認証と承認を使用してアプリケーションをセキュリティで保護する方法](# "セキュリティで保護されたアプリケーションを使用して認証と承認")
10. [AJAX を使用して、動的な更新プログラムを配信する方法](# "動的な更新プログラムを配信する AJAX を使用して、")
11. [AJAX を使用して、マッピングのシナリオを実装する方法](# "マッピング シナリオの実装を使用して AJAX")
12. [自動化された単体テストを有効にする方法](# "自動単体テストを有効にします。")

NerdDinner の独自のコピーを構築する各を実行して最初から手順チュートリアルの「します。 または、ここで、ソース コードの完成版をダウンロードできます: [GitHub で NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)します。 また、必要に応じて[このチュートリアルの無料 PDF 版をダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)オフライン チュートリアルを読みたい場合。

Visual Studio 2008 または、無料の Visual Web Developer 2008 Express のいずれかを使用するには、アプリケーションをビルドします。 データベースの SQL Server または無料の SQL Server Express のいずれかを使用することができます。

ASP.NET MVC、Visual Web Developer 2008 Express、および SQL Server Express (すべて無料) の V2 を使用してをインストールすることができます、 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>今すぐ開始しましょう.

NerdDinner が、ここで説明した、これで、とりかかりましょうをいくつかのコードを記述しましょう。

まず、ファイルを使用して&gt;NerdDinner アプリケーションを作成する Visual Studio 内で新しいプロジェクト。

> [!div class="step-by-step"]
> [次へ](create-a-new-aspnet-mvc-project.md)
