---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
title: 単体テストの ASP.NET MVC アプリケーションの作成 (c#) |Microsoft Docs
author: StephenWalther
description: コント ローラー アクションの単体テストを作成する方法について説明します。 このチュートリアルでは、Stephen Walther はコント ローラーのアクションは、parti を返すかどうかをテストする方法を示します.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: d3a270b9-d7b1-47f2-8775-fc3beb518b5c
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-cs
msc.type: authoredcontent
ms.openlocfilehash: f9e6945a379d37f1539c7135041f50dcc7041750
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826680"
---
<a name="creating-unit-tests-for-aspnet-mvc-applications-c"></a>単体テストの ASP.NET MVC アプリケーションの作成 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

[PDF のダウンロード](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_CS.pdf)

> コント ローラー アクションの単体テストを作成する方法について説明します。 このチュートリアルでは、Stephen Walther はコント ローラー アクションの特定のビューを返します、特定のデータのセットを返しますまたは別の種類のアクションの結果を返すかどうかをテストする方法を示します。


このチュートリアルの目的を作成する方法、コント ローラーの単体テスト、ASP.NET MVC でアプリケーションを示すことです。 次の 3 つの異なる種類の単体テストを作成する方法について説明します。 コント ローラーのアクションによって返されるデータの表示をテストする方法、および 1 つのコント ローラー アクションが 2 番目のコント ローラー アクションにリダイレクトするかどうかをテスト コント ローラーのアクションによって返されるビューをテストする方法について説明します。

## <a name="creating-the-controller-under-test"></a>テスト コント ローラーの作成

テストしようとするコント ローラーを作成してみましょう。 という名前のコント ローラー、 `ProductController`、リスト 1 に含まれています。

**1 – を一覧表示します。 `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample1.cs)]

`ProductController`という 2 つのアクション メソッドが含まれています`Index()`と`Details()`します。 両方のアクション メソッドでは、ビューを返します。 なお、`Details()`アクション id という名前のパラメーターを受け取る。

## <a name="testing-the-view-returned-by-a-controller"></a>ビューをテスト コント ローラーによって返される

テストすることを想像してくださいかどうか、`ProductController`右側のビューを返します。 確認するときに、`ProductController.Details()`アクションが呼び出されると、詳細ビューが返されます。 リスト 2 でのテスト クラスには、テストによって返されるビュー用の単体テストが含まれています、`ProductController.Details()`アクション。

**2 – を一覧表示します。 `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample2.cs)]

リスト 2 でクラスには、という名前のテスト メソッドが含まれています。`TestDetailsView()`します。 このメソッドには、次の 3 つの行コードにはが含まれています。 コードの最初の行の新しいインスタンスを作成し、`ProductController`クラス。 コードの 2 行目がコント ローラーを呼び出す`Details()`アクション メソッド。 ビューがによって返されるかどうか、コードのチェックの最後の行、最後に、`Details()`操作は、詳細ビュー。

`ViewResult.ViewName`プロパティは、コント ローラーによって返されるビューの名前を表します。 このプロパティのテストに関するビッグ警告を 1 つ。 これには、コント ローラーがビューを返すことができます 2 つの方法があります。 コント ローラーには、このようなビューを明示的に返されます。

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample3.cs)]

また、ビューの名前は、このようなコント ローラー アクションの名前から推論できます。

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample4.cs)]

という名前のビューを返すコント ローラー アクション`Details`します。 ただし、ビューの名前は、アクション名から推測されます。 ビュー名をテストする場合は、コント ローラー アクションからビュー名に明示的に返す必要があります。

リスト 2 で単体テストを実行するには、キーボードの組み合わせを入力するか、 **CTRL + R、A**かをクリックして、**ソリューション内のすべてのテストを実行**(図 1 参照) ボタンをクリックします。 テストが成功した場合は、図 2 でテスト結果 ウィンドウを確認します。


[![ソリューション内のすべてのテストを実行します。](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image1.png)

**図 01**: ソリューション内のすべてのテストの実行 ([フルサイズの画像を表示する をクリックします](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image3.png))。


[![Success!](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image4.png)

**図 02**: 成功! ([フルサイズの画像を表示する をクリックします](creating-unit-tests-for-asp-net-mvc-applications-cs/_static/image6.png))。


## <a name="testing-the-view-data-returned-by-a-controller"></a>コント ローラーによって返されるデータの表示のテスト

MVC コント ローラーがビューと呼ばれるものを使用してデータを渡します *`View Data`* します。 たとえばを呼び出すときに、特定の製品の詳細を表示する、`ProductController Details()`アクション。 インスタンスを作成する場合、 `Product` (モデルで定義された) クラスにインスタンスを渡すと、`Details`活用してビュー`View Data`します。

変更された`ProductController`リスト 3 で更新が含まれています`Details()`積を返すアクション。

**3 – を一覧表示します。 `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample5.cs)]

まず、`Details()`アクションの新しいインスタンスを作成する、`Product`ラップトップ コンピューターを表すクラスです。 次のインスタンス、`Product`クラスは、2 番目のパラメーターとして渡される、`View()`メソッド。

単体テストを記述するビューにデータで含まれている必要なデータがあるかどうかをテストします。 呼び出すとき、ラップトップ コンピューターを表す製品が返されるかどうかに、リスト 4 のテストでは、単体テスト、`ProductController Details()`アクション メソッド。

**4 – を一覧表示します。 `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample6.cs)]

リストの 4、`TestDetailsView()`メソッド呼び出しによって返されるデータの表示のテスト、`Details()`メソッド。 `ViewData`にプロパティとして公開、`ViewResult`呼び出しによって返される、`Details()`メソッド。 `ViewData.Model`プロパティには、ビューに渡される、製品が含まれています。 テストでは、単に名ラップトップをビューのデータに含まれる製品であることを確認します。

## <a name="testing-the-action-result-returned-by-a-controller"></a>コント ローラーによって返されるアクションの結果をテストします。

複雑なコント ローラー アクションには、さまざまな種類のコント ローラー アクションに渡されるパラメーターの値に応じてアクションの結果を返す可能性があります。 コント ローラーのアクションは、さまざまな種類のアクションの結果などを返すことができます、 `ViewResult`、 `RedirectToRouteResult`、または`JsonResult`します。

たとえば、変更された`Details()`リスト 5 でアクションを返します、`Details`アクションに有効なプロダクト Id を渡すときに表示します。 リダイレクトして 1--よりも小さい無効な製品 Id - Id 値を渡す場合、`Index()`アクション。

**5 – を一覧表示します。 `ProductController.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample7.cs)]

動作をテストすることができます、`Details()`リスト 6 で単体テストを操作します。 6 の一覧で、単体テストにリダイレクトされることを確認します、`Index`に値が-1 の Id が渡されるときの表示、`Details()`メソッド。

**6 – を一覧表示します。 `ProductControllerTest.cs`**

[!code-csharp[Main](creating-unit-tests-for-asp-net-mvc-applications-cs/samples/sample8.cs)]

呼び出すと、`RedirectToAction()`メソッドでコント ローラー アクション、コント ローラー アクションを返します、`RedirectToRouteResult`します。 テスト チェックするかどうか、`RedirectToRouteResult`という名前のコント ローラー アクションにユーザーをリダイレクト`Index`します。

## <a name="summary"></a>まとめ

このチュートリアルでは、MVC コント ローラー アクションの単体テストを作成する方法について説明しました。 最初に、右側のビューがコント ローラーのアクションによって返されるかどうかを確認する方法を学習しました。 使用する方法を学習しました、`ViewResult.ViewName`プロパティをビューの名前を確認します。

内容をテストする方法を調べて次に、`View Data`します。 適切な製品が返されたかどうかを確認する方法を学習しました`View Data`コント ローラーのアクションを呼び出した後。

最後に、さまざまな種類のアクションの結果は、コント ローラー アクションから返されるかどうかをテストする方法について説明します。 コント ローラーを返すかどうかをテストする方法を学習しました、`ViewResult`または`RedirectToRouteResult`します。

> [!div class="step-by-step"]
> [次へ](creating-unit-tests-for-asp-net-mvc-applications-vb.md)
