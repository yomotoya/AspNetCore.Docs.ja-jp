---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: "アクション フィルター (VB) を理解する |Microsoft ドキュメント"
author: microsoft
description: "このチュートリアルの目的では、アクション フィルターをについて説明します。 コント ローラーのアクションまたはコント ローラー全体に適用可能な属性をアクション フィルターには."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 483133ec5db27c2fa1ed4b463e37e17efab12e0f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="understanding-action-filters-vb"></a>アクション フィルター (VB) を理解します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> このチュートリアルの目的では、アクション フィルターをについて説明します。 アクション フィルターは、コント ローラーのアクション--または全体コント ローラー--アクションを実行する方法を変更するに適用可能な属性です。


## <a name="understanding-action-filters"></a>アクション フィルターを理解します。

このチュートリアルの目的では、アクション フィルターをについて説明します。 アクション フィルターは、コント ローラーのアクション--または全体コント ローラー--アクションを実行する方法を変更するに適用可能な属性です。 ASP.NET MVC フレームワークには、いくつかのアクション フィルターが含まれています。

- OutputCache – このアクション フィルターは、一定の時間のコント ローラー アクションの出力をキャッシュします。
- HandleError – このアクション フィルターは、コント ローラーのアクションを実行するときに発生したエラーを処理します。
- 承認: このアクション フィルターでは、特定のユーザーまたはロールにアクセスを制限することができます。

また、独自のカスタム アクション フィルターを作成することもできます。 たとえば、カスタム認証システムを実装するためにカスタム アクション フィルターを作成することができます。 または、コント ローラーのアクションによって返されるデータの表示を変更するアクション フィルターを作成する場合があります。

このチュートリアルを最初からアクション フィルターを構築する方法を学習します。 Visual Studio の出力ウィンドウに、アクションの処理のさまざまな段階をログに記録するログ アクション フィルターを作成します。

### <a name="using-an-action-filter"></a>アクション フィルターを使用します。

アクション フィルターは、属性です。 個々 のコント ローラー アクションまたは全体のコント ローラーには、ほとんどのアクション フィルターを適用できます。

リスト 1 のデータのコント ローラーがという名前のアクションを公開するなど、`Index()`現在の時刻を返します。 この操作で装飾されて、`OutputCache`アクション フィルター。 このフィルターによって、10 秒間キャッシュに保存する操作によって返される値。

**1 – を一覧表示します。`Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

繰り返し呼び出す場合、`Index()`お使いのブラウザーのアドレス バーに、URL データ/インデックスを入力して、更新のヒットのアクション ボタンは、同時に 10 秒間表示、複数回をクリックします。 出力、`Index()`アクションが (図 1 を参照してください) 10 秒間キャッシュします。


[![キャッシュ時間](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**図 01**: キャッシュ時間 ([フルサイズのイメージを表示するをクリックして](understanding-action-filters-vb/_static/image3.png))


1 では一覧表示する、1 つのアクション フィルター –、 `OutputCache` – アクション フィルターを適用、`Index()`メソッドです。 必要がある場合は、同じアクションに複数のアクション フィルターを適用できます。 両方を適用するなど、`OutputCache`と`HandleError`同じアクションにアクション フィルター。

リストの 1 の`OutputCache`にアクション フィルターが適用される、`Index()`アクション。 この属性を適用でしたも、`DataController`クラス自体です。 その場合は、コント ローラーによって公開される何らかの操作によって返される結果は 10 秒間キャッシュされます。

### <a name="the-different-types-of-filters"></a>さまざまな種類のフィルター

ASP.NET MVC フレームワークには、次の 4 つの異なる種類のフィルターがサポートされています。

1. 承認フィルター – を実装して、`IAuthorizationFilter`属性。
2. アクション フィルター – を実装して、`IActionFilter`属性。
3. 結果フィルター-を実装して、`IResultFilter`属性。
4. 例外フィルター – を実装して、`IExceptionFilter`属性。

フィルターは、上に示した順序で実行されます。 たとえば、承認フィルターは、常にアクション フィルターの前に実行し、例外フィルターはフィルターの他のすべての型の後に常に実行します。

承認フィルターは、コント ローラー アクションの認証と承認の実装に使用されます。 たとえば、承認フィルターは、承認フィルターの例です。

アクション フィルターには、コント ローラーのアクションが実行された前後に実行されるロジックが含まれます。 たとえば、アクション フィルターを使用するコント ローラーのアクションを返すビュー データを変更します。

結果フィルターには、ビューの結果が実行された前後に実行されるロジックが含まれます。 ビューの結果を変更するなど、右、ビューがブラウザーに表示される前にします。

例外フィルターは、最後に実行するフィルターの種類です。 例外フィルターを使用して、コント ローラーのアクションまたはコント ローラー アクションの結果のいずれかによって発生したエラーを処理することができます。 また、例外フィルターをエラーをログに使用することもできます。

各フィルターの種類は、特定の順序で実行されます。 同じ種類のフィルターが実行される順序を制御する場合は、フィルターの順序のプロパティを設定できます。

すべてのアクション フィルターの基本クラスは、`System.Web.Mvc.FilterAttribute`クラスです。 特定の種類のフィルターを実装する場合は、フィルターの基本クラスから継承し、1 つまたは複数の IAuthorizationFilter、IActionFilter、IResultFilter を実装するクラスを作成する必要があります。 または ExceptionFilter インターフェイスします。

### <a name="the-base-actionfilterattribute-class"></a>基本 ActionFilterAttribute クラス

ASP.NET MVC フレームワークにはやすくためにカスタム アクション フィルターを実装するためには、ベースが含まれています`ActionFilterAttribute`クラスです。 このクラスでは両方とも、`IActionFilter`と`IResultFilter`インターフェイスから継承して、`Filter`クラスです。

用語をここでは、完全一致しません。 技術的には、ActionFilterAttribute クラスから継承するクラスは、アクション フィルターと結果フィルターの両方です。 ただし、緩やかな意味では、word のアクション フィルターは、ASP.NET MVC フレームワーク内のフィルターの任意の型を参照するで使用されます。

基本 ActionFilterAttribute クラスでは、オーバーライド可能な次の方法があります。

- コント ローラーのアクションが実行される前に、OnActionExecuting – ときにこのメソッドが呼び出されます。
- コント ローラーのアクションを実行した後、OnActionExecuted – ときにこのメソッドが呼び出されます。
- コント ローラー アクションの結果が実行される前に、OnResultExecuting – ときにこのメソッドが呼び出されます。
- コント ローラー アクションの結果が実行された後に、OnResultExecuted – ときにこのメソッドが呼び出されます。

次のセクションでは、これらのさまざまなメソッドを実装する方法おを参照してください。

### <a name="creating-a-log-action-filter"></a>ログのアクション フィルターを作成します。

カスタム アクション フィルターを構築する方法がわかるようにするために、Visual Studio 出力ウィンドウをコント ローラーのアクションを処理の段階をログに記録するカスタム アクション フィルターを作成します。 当社`LogActionFilter`2 のリストに含まれています。

**2 – を一覧表示します。`ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

リストの 2 で、 `OnActionExecuting()`、 `OnActionExecuted()`、 `OnResultExecuting()`、および`OnResultExecuted()`すべてのメソッドを呼び出す、`Log()`メソッドです。 渡される、メソッドの名前、および現在のルート データ、`Log()`メソッドです。 `Log()`メソッドは、Visual Studio の出力ウィンドウにメッセージを書き込みます (図 2 を参照してください)。


[![Visual Studio の出力 ウィンドウへの書き込み](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**図 02**: Visual Studio の出力 ウィンドウへの書き込み ([フルサイズのイメージを表示するをクリックして](understanding-action-filters-vb/_static/image6.png))


3 の一覧で、Home コント ローラーは、全体のコント ローラー クラスに、ログのアクション フィルターを適用する方法を示しています。 ときに、Home コント ローラーによって公開される操作のいずれかが呼び出される – か、`Index()`メソッドまたは`About()`メソッド – アクションは、Visual Studio の出力ウィンドウにログ記録処理の段階です。

**3 – を一覧表示します。`Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>概要

このチュートリアルでは、ASP.NET MVC アクション フィルターに導入されました。 次の 4 つの異なる種類のフィルターについて説明しました。 承認フィルター、アクション フィルター、結果フィルター、および例外フィルター。 基本についても学びました`ActionFilterAttribute`クラスです。

最後に、簡単なアクション フィルターを実装する方法を学習します。 Visual Studio の出力ウィンドウにコント ローラーのアクションを処理の段階をログに記録するログ アクション フィルターを作成しました。

>[!div class="step-by-step"]
[前へ](asp-net-mvc-routing-overview-vb.md)
[次へ](improving-performance-with-output-caching-vb.md)
