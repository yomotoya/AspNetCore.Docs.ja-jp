---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: NerdDinner チュートリアルの概要 |Microsoft ドキュメント
author: shanselman
description: 新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。 このチュートリアルで ASP.NE を使用するサイズは小さいが完了すると、アプリケーションを構築する方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="introducing-the-nerddinner-tutorial"></a>NerdDinner チュートリアルの概要
====================
によって[Scott Hanselman](https://github.com/shanselman)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。 このチュートリアルでは、小さなの構築が完了すると、ASP.NET MVC 1 を使用してアプリケーションにする方法を説明し、一部の背後にある主要な概念について説明します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-tutorial"></a>NerdDinner チュートリアル

新しいフレームワークを学習する最善の方法では、何らかの処理をビルドします。 このチュートリアルでは、小さなの構築が完了すると、ASP.NET MVC を使用してアプリケーションにする方法を説明し、一部の背後にある主要な概念について説明します。

ビルドしようとして、アプリケーションは、"NerdDinner"と呼ばれます。 NerdDinner は、検索や整理ディナー オンライン方のための簡単な方法を提供します。

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner により、登録されたユーザーが作成、編集、およびディナーを削除します。 アプリケーション全体で一貫性のある一連の検証とビジネス ルールを適用します。

![](introducing-the-nerddinner-tutorial/_static/image2.png)

訪問者は、AJAX ベースのマップを使用して、それらの近くに保持される今後ディナーを検索できます。

![](introducing-the-nerddinner-tutorial/_static/image3.png)

クリックすると、dinner は移動に詳細 ページに学べる場所は、詳細については。

![](introducing-the-nerddinner-tutorial/_static/image4.png)

ログインするか、サイト上のレジスタ dinner への参加に関心のある: 場合

![](introducing-the-nerddinner-tutorial/_static/image5.png)

イベントへの参加を AJAX ベース RSVP リンクをクリックすることができます。

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>NerdDinner を実装します。

使用して、ファイルによって NerdDinner アプリケーションを開始しようとして&gt;新しい ASP.NET MVC プロジェクトを作成する Visual Studio 内で新しいプロジェクト コマンド。 お機能と特徴増分追加します。 その過程でおについて説明します。

1. [新しい ASP.NET MVC プロジェクトを作成する方法](# "新しい ASP.NET MVC プロジェクトを作成します。")
2. [データベースを作成する方法](# "データベースを作成します。")
3. [ビジネス ルールの検証を使用してモデルを構築する方法](# "ビジネス ルールの検証とモデルの構築")
4. [コント ローラーとビューを使用して、一覧/詳細 UI を実装する方法](# "一覧と詳細の UI を実装するを使用してコント ローラーとビュー")
5. [CRUD を提供する方法 (作成、読み取り、更新、削除) データ エントリのサポートを形成する](# "提供 CRUD (Create、Read、Update、Delete) データ形式のエントリをサポート")
6. [ViewData ViewModel クラスを実装する方法](# "ViewData の使用と ViewModel クラスの実装")
7. [マスター ページとパーシャルを使用して UI を再利用する方法](# "マスター ページを使用して UI を再利用とパーシャル")
8. [効率的なデータのページングを実装する方法](# "実装効率的なデータ ページング")
9. [認証と承認を使用してアプリケーションをセキュリティで保護する方法](# "セキュリティで保護されたアプリケーションを使用して認証と承認")
10. [AJAX を使用して、動的な更新プログラムを配信する方法](# "動的な更新プログラムを配信する AJAX を使用します。")
11. [AJAX を使用して、マッピングのシナリオを実装する方法](# "マッピング シナリオの実装を使用して AJAX")
12. [自動化された単体テストを有効にする方法](# "自動化された単体テストを有効にします。")

NerdDinner の独自のコピーをビルドすることができますをそれぞれを完了して最初からステップはこの章で、チュートリアルです。 代わりに、ソース コードをここでの完成版をダウンロードできます: [github NerdDinner](https://github.com/AspNetMVPSamples/NerdDinner)です。 また、必要に応じても行えます[このチュートリアルの無料の PDF バージョンをダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)オフライン チュートリアルを参照する場合。

Visual Studio 2008 または、空き Visual Web Developer 2008 Express のいずれかを使用するには、アプリケーションをビルドします。 データベースの SQL Server または無料の SQL Server Express のいずれかを使用することができます。

ASP.NET MVC、Visual Web Developer 2008 Express、および SQL Server Express (すべて無償) の V2 を使用してをインストールすることができます、 [Microsoft Web プラットフォーム インストーラー](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>今すぐ始めましょう.

後に取り上げる NerdDinner は、これで、袖をロールアップし、コードを記述してみましょう。

まずファイルを使用して、&gt;NerdDinner アプリケーションを作成する Visual Studio 内で新しいプロジェクト。

> [!div class="step-by-step"]
> [次へ](create-a-new-aspnet-mvc-project.md)
