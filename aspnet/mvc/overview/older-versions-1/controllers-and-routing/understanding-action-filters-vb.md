---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: アクション フィルター (VB) について理解する |Microsoft Docs
author: microsoft
description: このチュートリアルの目的では、アクション フィルターについて説明します。 コント ローラーのアクションまたはコント ローラー全体に適用できる属性をアクション フィルターには.
ms.author: riande
ms.date: 10/16/2008
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 0116306afdf21cb24a374013bb54ada54e5699ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827714"
---
<a name="understanding-action-filters-vb"></a>アクション フィルター (VB) について理解します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> このチュートリアルの目的では、アクション フィルターについて説明します。 アクション フィルターは、コント ローラーのアクションまたは全体コント ローラー--アクションを実行する方法を変更するに適用できる属性です。


## <a name="understanding-action-filters"></a>アクション フィルターについて理解します。

このチュートリアルの目的では、アクション フィルターについて説明します。 アクション フィルターは、コント ローラーのアクションまたは全体コント ローラー--アクションを実行する方法を変更するに適用できる属性です。 ASP.NET MVC フレームワークには、いくつかのアクション フィルターが含まれます。

- OutputCache – このアクション フィルターは、一定の時間のコント ローラー アクションの出力をキャッシュします。
- HandleError – このアクション フィルターは、コント ローラーのアクションを実行するときに発生したエラーを処理します。
- 承認-このアクション フィルターでは、特定のユーザーまたはロールのアクセスを制限することができます。

また、独自のカスタム アクション フィルターを作成することもできます。 たとえば、カスタム認証システムを実装するためにカスタム アクション フィルターを作成する可能性があります。 または、コント ローラーのアクションによって返されるデータの表示を変更するアクション フィルターを作成したい場合があります。

このチュートリアル ゼロからアクション フィルターを構築する方法について説明します。 Visual Studio の出力ウィンドウをアクションの処理のさまざまな段階に記録するログのアクション フィルターを作成します。

### <a name="using-an-action-filter"></a>アクション フィルターの使用

アクション フィルターは、属性です。 ほとんどのアクション フィルターは、個々 のコント ローラー アクションまたはコント ローラー全体に適用できます。

たとえば、リスト 1 でのデータのコント ローラーはという名前のアクションを公開`Index()`現在の時刻を返します。 このアクションがで修飾された、`OutputCache`アクション フィルター。 このフィルターによって、10 秒間キャッシュされるアクションによって返される値。

**1 – を一覧表示します。 `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

繰り返しを起動する場合、`Index()`お使いのブラウザーのアドレス バーに URL データ/インデックスを入力して、更新操作ボタンを複数回同時には 10 秒間表示されます。 出力、`Index()`アクションが (図 1 参照)、10 秒間キャッシュされます。


[![キャッシュされる時間](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**図 01**: キャッシュ時間 ([フルサイズの画像を表示する をクリックします](understanding-action-filters-vb/_static/image3.png))。


リスト 1、1 つのアクション フィルター – で、`OutputCache`アクション フィルター – に適用されます、`Index()`メソッド。 が必要な場合は、同じアクションに複数のアクション フィルターを適用できます。 両方を適用するなど、`OutputCache`と`HandleError`同じアクションにアクション フィルター。

リストの 1 で、`OutputCache`にアクション フィルターを適用、`Index()`アクション。 この属性を適用できますも、`DataController`クラス自体。 その場合は、コント ローラーによって公開される何らかの操作によって返される結果は 10 秒間キャッシュされます。

### <a name="the-different-types-of-filters"></a>さまざまな種類のフィルター

ASP.NET MVC フレームワークには、次の 4 つの異なる種類のフィルターがサポートされています。

1. 承認フィルター – 実装、`IAuthorizationFilter`属性。
2. アクション フィルター – 実装、`IActionFilter`属性。
3. 結果フィルター – 実装、`IResultFilter`属性。
4. 例外フィルター – 実装、`IExceptionFilter`属性。

フィルターは、上に示した順序で実行されます。 たとえば、承認フィルターは、常にアクション フィルターの前に実行し、例外フィルターは、常に他のすべての種類フィルターの後に実行します。

承認フィルターは、コント ローラー アクションの認証と承認を実装するために使用されます。 たとえば、承認フィルターは、承認フィルターの例です。

アクション フィルターには、コント ローラーのアクションが実行された前後に実行されるロジックが含まれます。 たとえば、コント ローラーのアクションが返すデータの表示を変更するアクション フィルターを使用できます。

結果フィルターには、ビューの結果が実行された前後に実行されるロジックが含まれます。 ビューの結果を変更するなど、ブラウザーにビューが表示される直前です。

例外フィルターは、最後に実行するフィルターの種類です。 コント ローラー アクションまたはコント ローラー アクションの結果のいずれかによって発生したエラーを処理するために、例外フィルターを使用することができます。 また、例外フィルターをエラーをログに使用することもできます。

各フィルターの別の種類は、特定の順序で実行されます。 同じ種類のフィルターが実行される順序を制御する場合は、フィルターの順序のプロパティを設定できます。

すべてのアクション フィルターの基本クラスは、`System.Web.Mvc.FilterAttribute`クラス。 特定の種類のフィルターを実装する場合は、フィルターの基本クラスから継承し、IAuthorizationFilter、IActionFilter、IResultFilter、1 つ以上実装するクラスを作成する必要があります。 または ExceptionFilter のインターフェイスします。

### <a name="the-base-actionfilterattribute-class"></a>基本 ActionFilterAttribute クラス

簡単なカスタム アクション フィルターを実装するために、ASP.NET MVC framework にはベースが含まれています`ActionFilterAttribute`クラス。 このクラスは、両方を実装、`IActionFilter`と`IResultFilter`インターフェイスから継承して、`Filter`クラス。

用語をここでは、完全一致しません。 技術的には、ActionFilterAttribute クラスから継承するクラスは、アクション フィルターと結果フィルターの両方です。 ただし、loose の意味では、word のアクション フィルターは、ASP.NET MVC フレームワークでのフィルターの任意の型を参照する使用されます。

基本 ActionFilterAttribute クラスには、次のメソッドをオーバーライドすることができますがあります。

- OnActionExecuting – コント ローラーのアクションが実行される前に、このメソッドが呼び出されます。
- OnActionExecuted – コント ローラーのアクションが実行された後、このメソッドが呼び出されます。
- OnResultExecuting – コント ローラー アクションの結果が実行される前に、このメソッドが呼び出されます。
- OnResultExecuted – コント ローラー アクションの結果が実行された後、このメソッドが呼び出されます。

次のセクションでそれぞれのさまざまな方法を実装する方法が表示されます。

### <a name="creating-a-log-action-filter"></a>ログのアクション フィルターを作成します。

カスタム アクション フィルターを作成する方法を示すためには、するには、Visual Studio 出力ウィンドウにコント ローラー アクションの処理のステージを記録するカスタム アクション フィルターを作成します。 この`LogActionFilter`リスト 2 に含まれています。

**2 – を一覧表示します。 `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

リストの 2 で、 `OnActionExecuting()`、 `OnActionExecuted()`、 `OnResultExecuting()`、および`OnResultExecuted()`すべてのメソッドを呼び出す、`Log()`メソッド。 メソッドと現在のルート データの名前に渡される、`Log()`メソッド。 `Log()`メソッドは、Visual Studio の出力ウィンドウにメッセージを書き込みます (図 2 参照)。


[![Visual Studio の出力ウィンドウへの書き込み](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**図 02**: Visual Studio の出力ウィンドウへの書き込み ([フルサイズの画像を表示する をクリックします](understanding-action-filters-vb/_static/image6.png))。


リスト 3 の Home コント ローラーは、全体のコント ローラー クラスに、ログのアクション フィルターを適用する方法を示しています。 たびに、Home コント ローラーによって公開されるアクションのいずれかが呼び出される – か、`Index()`メソッドまたは`About()`メソッド – Visual Studio の出力ウィンドウに、アクションがログに記録する処理の段階です。

**3 – を一覧表示します。 `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC のアクション フィルターに導入されました。 4 つの異なる種類のフィルターについて説明しました。 承認フィルター、アクション フィルター、結果フィルター、および例外フィルター。 基本についても学習しました`ActionFilterAttribute`クラス。

最後に、簡単なアクション フィルターを実装する方法を学習しました。 Visual Studio の出力ウィンドウにコント ローラー アクションの処理のステージを記録するログのアクション フィルターを作成しました。

> [!div class="step-by-step"]
> [前へ](asp-net-mvc-routing-overview-vb.md)
> [次へ](improving-performance-with-output-caching-vb.md)
