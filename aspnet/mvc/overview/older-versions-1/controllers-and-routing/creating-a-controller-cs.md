---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: コント ローラー (c#) の作成 |Microsoft Docs
author: StephenWalther
description: このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーションをコント ローラーを追加する方法について説明します。
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fe880775a5d477aefcf0fe6cedb3e719760ed90
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835864"
---
<a name="creating-a-controller-c"></a>コント ローラー (c#) の作成
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> このチュートリアルでは、Stephen Walther は、ASP.NET MVC アプリケーションをコント ローラーを追加する方法について説明します。


このチュートリアルの目的では、作成する新しい ASP.NET MVC コント ローラーについて説明します。 Visual Studio コント ローラーの追加 メニュー オプションを使用して、クラス ファイルを手動で作成して、コント ローラーを作成する方法について説明します。

### <a name="using-the-add-controller-menu-option"></a>使用して、コント ローラーのメニュー オプションの追加

新しいコント ローラーを作成する最も簡単な方法は、Visual Studio ソリューション エクスプ ローラー ウィンドウで、Controllers フォルダーを右クリックして、選択、**追加、コント ローラー**メニュー オプション (図 1 参照)。 このメニュー オプションを選択すると表示、**コント ローラーの追加**ダイアログ (図 2 参照)。


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**図 01**: 新しいコント ローラーの追加 ([フルサイズの画像を表示する をクリックします](creating-a-controller-cs/_static/image2.png))。


[![[新しいプロジェクト] ダイアログ ボックス](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**図 02**:、コント ローラーの追加 ダイアログ ([フルサイズの画像を表示する をクリックします](creating-a-controller-cs/_static/image4.png))。


コント ローラー名の最初の部分が強調表示されますが、**コント ローラーの追加**ダイアログ。 サフィックスを持つすべてのコント ローラー名が終了する必要があります*コント ローラー*します。 たとえば、という名前のコント ローラーを作成することができます *「productcontroller」* がという名前のコント ローラー以外*製品*します。


不足しているコント ローラーを作成する場合、*コント ローラー*コント ローラーを起動することはできませんし、サフィックスが付いています。 これを行わない--この間違いを犯す後に自分の人生に膨大な時間を無駄しました。


**1 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Controllers フォルダーで、常にコント ローラーを作成する必要があります。 それ以外の場合、ASP.NET MVC の規則に違反するし、他の開発者がアプリケーションを理解することが難しく時間が必要があります。

### <a name="scaffolding-action-methods"></a>アクション メソッドのスキャフォールディング

コント ローラーを作成するときに自動的に作成、更新、および詳細のアクション メソッドを生成するオプションがある (図 3 を参照してください)。 このオプションを選択する場合は、リスト 2 でコント ローラー クラスが生成されます。


[![アクション メソッドを自動的に作成します。](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**図 03**: アクション メソッドを自動的に作成する ([フルサイズの画像を表示する をクリックします](creating-a-controller-cs/_static/image6.png))。


**2 - Controllers\CustomerController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

これらの生成されたメソッドはスタブ メソッドです。 作成、更新、および自分で顧客の詳細を示す実際のロジックを追加する必要があります。 優れた出発点に、スタブ メソッドが提供します。

### <a name="creating-a-controller-class"></a>コント ローラー クラスを作成します。

ASP.NET MVC コント ローラーは、単なるクラスです。 場合は、便利な Visual Studio コント ローラーのスキャフォールディングを無視し、コント ローラー クラスを手動で作成できます。 この場合は、以下の手順に従ってください。

1. Controllers フォルダーを右クリックし、メニュー オプションを選択**追加]、[新しい項目の**を選択し、**クラス**テンプレート (図 4 参照)。
2. 新しいクラス PersonController.cs の名前を指定し、をクリックして、**追加**ボタンをクリックします。
3. (リスト 3 参照)、基本 System.Web.Mvc.Controller クラスからクラスを継承するので、結果として得られるクラス ファイルを変更します。


[![新しいクラスを作成します。](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**図 04**: 新しいクラスを作成する ([フルサイズの画像を表示する をクリックします](creating-a-controller-cs/_static/image8.png))。


**3 - Controllers\PersonController.cs を一覧表示します。**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

リスト 3 のコント ローラーは、文字列"Hello World!"を返します Index() という名前の 1 つのアクションを公開します。 このコント ローラー アクションを起動するには、アプリケーションを実行して、次のような URL を要求します。

`http://localhost:40071/Person`

> [!NOTE]
> 
> ASP.NET 開発サーバーは、ランダムなポート番号 (たとえば、40071) を使用します。 コント ローラーを呼び出す URL を入力すると、右側のポート番号を指定する必要があります。 Windows 通知領域 (画面の下部にある-右側) で、ASP.NET 開発サーバーのアイコンの上にマウスを置くことによって、ポート番号を指定できます。
> 
> [!div class="step-by-step"]
> [前へ](adding-dynamic-content-to-a-cached-page-cs.md)
> [次へ](creating-an-action-cs.md)
