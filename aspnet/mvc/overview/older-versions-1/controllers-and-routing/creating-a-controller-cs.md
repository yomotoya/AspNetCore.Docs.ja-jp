---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: コント ローラー (c#) の作成 |Microsoft ドキュメント
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーション コント ローラーを追加する方法について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 86966f1064d09419e2102542c6d14c4162d153e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868361"
---
<a name="creating-a-controller-c"></a>コント ローラー (c#) の作成
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーション コント ローラーを追加する方法について説明します。


このチュートリアルの目的では、作成する新しい ASP.NET MVC コント ローラーについて説明します。 Visual Studio コント ローラーの追加 メニュー オプションを使用して、クラス ファイルを手動で作成することで、コント ローラーを作成する方法を学びます。

### <a name="using-the-add-controller-menu-option"></a>使用して、コント ローラーのメニュー オプションを追加

新しいコント ローラーを作成する最も簡単な方法は、Visual Studio ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックし、選択する、**追加、コント ローラー**メニュー オプション (図 1 を参照してください)。 このメニュー オプションを選択すると開きます、**コント ローラーの追加**ダイアログ (図 2 を参照してください)。


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**図 01**: 新しいコント ローラーの追加 ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image2.png))


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**図 02**:「コント ローラーの追加 ダイアログ ([フルサイズのイメージを表示するには、をクリックして](creating-a-controller-cs/_static/image4.png))


コント ローラー名の最初の部分が強調表示されていることを確認、**コント ローラーの追加**ダイアログ。 各コント ローラー名がサフィックスで終わる必要があります*コント ローラー*です。 たとえば、という名前のコント ローラーを作成することができます*ProductController*が名前付きコント ローラー以外*製品*です。


不足しているコント ローラーを作成する場合、*コント ローラー*コント ローラーを起動することはできませんし、サフィックスが付いています。 この--命このようなミスを行った後に膨大な時間は無駄になるしました。


**1 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

常に、コント ローラーのフォルダーに、コント ローラーを作成する必要があります。 それ以外の場合、ASP.NET MVC の規則に違反するし、アプリケーションの理解が難しくの時間がある他の開発者。

### <a name="scaffolding-action-methods"></a>スキャフォールディング アクション メソッド

アクション メソッドの作成、更新、および詳細情報を自動的に生成するオプションがあるコント ローラーを作成するときに (図 3 を参照してください)。 このオプションを選択したリスト 2 でコント ローラーのクラスが生成されます。


[![アクション メソッドを自動的に作成します。](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**図 03**: アクション メソッドを自動的に作成する ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image6.png))


**2 - Controllers\CustomerController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

これらの生成されたメソッドは、スタブ メソッドです。 作成、更新、および自分で顧客の詳細を表示、実際のロジックを追加する必要があります。 ただし、スタブ メソッドは、便利な開始点。

### <a name="creating-a-controller-class"></a>コント ローラー クラスを作成します。

ASP.NET MVC コント ローラーは、クラスだけです。 場合は、便利な Visual Studio コント ローラーのスキャフォールディングを無視し、コント ローラー クラスを手動で作成できます。 この場合は、以下の手順に従ってください。

1. Controllers フォルダーを右クリックし、メニュー オプションを選択**追加]、[新しい項目の**を選択し、**クラス**テンプレート (図 4 を参照してください)。
2. 新しいクラス PersonController.cs の名前を指定し、をクリックして、**追加**ボタンをクリックします。
3. 基本 System.Web.Mvc.Controller クラス (3 のリストを参照) からクラスを継承できるように、生成されたクラス ファイルを変更します。


[![新しいクラスを作成します。](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**図 04**: 新しいクラスを作成する ([フルサイズのイメージを表示するをクリックして](creating-a-controller-cs/_static/image8.png))


**3 - Controllers\PersonController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

リスト 3 でのコント ローラーは、文字列"Hello World!"を返します Index() をという名前の 1 つのアクションを公開します。 このコント ローラーのアクションを起動するには、アプリケーションを実行して、次のように URL を要求します。

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET 開発サーバーでは、ランダムなポート番号 (たとえば、40071) を使用します。 コント ローラーを起動する URL を入力するときに、右側のポート番号を指定する必要があります。 Windows 通知領域 (下の画面の右側) で ASP.NET 開発サーバーのアイコンの上にマウスを置くことによって、ポート番号を指定できます。
> 
> [!div class="step-by-step"]
> [前へ](adding-dynamic-content-to-a-cached-page-cs.md)
> [次へ](creating-an-action-cs.md)
