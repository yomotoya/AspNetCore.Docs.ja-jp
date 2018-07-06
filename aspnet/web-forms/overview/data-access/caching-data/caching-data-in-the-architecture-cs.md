---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: アーキテクチャ (c#) でデータのキャッシュ |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、プレゼンテーション層でキャッシュを適用する方法について説明しました。 このチュートリアルでは、複数層の architectu を活用する方法を学習します.
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 80805bae14654d6817328232453031384ceadad6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820459"
---
<a name="caching-data-in-the-architecture-c"></a>データのキャッシュ アーキテクチャ (c#)
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe)または[PDF のダウンロード](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> 前のチュートリアルでは、プレゼンテーション層でキャッシュを適用する方法について説明しました。 このチュートリアルでは、ビジネス ロジック層でキャッシュ データを階層型アーキテクチャの利用方法について説明します。 このために、キャッシュ レイヤーを含めるアーキテクチャを拡張します。


## <a name="introduction"></a>はじめに

前のチュートリアルで説明したようには、2 つのプロパティを設定するだけでは ObjectDataSource のデータのキャッシュです。 残念ながら、ObjectDataSource は、ASP.NET ページのキャッシュ ポリシーを密に結合するプレゼンテーション層でキャッシュを適用します。 このような賢明を分類を許可する階層型アーキテクチャを作成する理由の 1 つです。 データ アクセス層、データ アクセスの詳細を分離するときに、ビジネス ロジック層は、ASP.NET ページなどのビジネス ロジックを分離します。 このビジネス ロジックとデータ アクセスの詳細の分離は、優先で、読みやすくより管理しやすく、および変更をより柔軟なシステムは、そのためです。 ドメインのナレッジとの作業分担、プレゼンテーション層は t で作業する開発者が自分のジョブを実行するためにデータベースの詳細を確認する必要があることもできます。 キャッシュ ポリシーをプレゼンテーション層から分離することと同様のメリットを提供します。

このチュートリアルでは、強化点、アーキテクチャを含める、*キャッシュ レイヤー* (または略して CL)、キャッシュ ポリシーを使用します。 キャッシュ レイヤーが含まれます、`ProductsCL`などのメソッドと製品情報へのアクセスを提供するクラス`GetProducts()`、`GetProductsByCategoryID(categoryID)`など、呼び出されたときに、キャッシュからデータを取得する最初の試行。 これらのメソッドが、適切な呼び出し、キャッシュが空の場合`ProductsBLL`に BLL は DAL からデータを入手さらには、内のメソッド。 `ProductsCL`メソッドが返す前に、BLL から取得されたデータをキャッシュします。

図 1 に示すよう、CL は、プレゼンテーション層とビジネス ロジック層の間存在します。


![キャッシュ レイヤー (CL) が別の層のアーキテクチャ](caching-data-in-the-architecture-cs/_static/image1.png)

**図 1**: The キャッシュ レイヤー (CL) が別の層のアーキテクチャ


## <a name="step-1-creating-the-caching-layer-classes"></a>手順 1: キャッシュ レイヤーのクラスを作成します。

このチュートリアルでは、1 つのクラスを使用して非常に単純な CL を作成しますが`ProductsCL`いくつかのメソッドのみを持ちます。 アプリケーション全体の作成が必要になります向けの完全なキャッシュ レイヤーの作成`CategoriesCL`、 `EmployeesCL`、および`SuppliersCL`クラス、およびこれらのキャッシュ レイヤーのクラスで BLL 内の各データ アクセスまたは変更メソッドのメソッドを提供します。 BLL と DAL でキャッシュ レイヤーが理想的ですとして実装を別のクラス ライブラリ プロジェクトただし、内のクラスとして実装しましたが、`App_Code`フォルダー。

Let s を DAL、BLL クラスからより多くのクリーンに独立した、CL クラスで新しいサブフォルダーが作成、`App_Code`フォルダー。 右クリックし、`App_Code`ソリューション エクスプ ローラーでフォルダーが、新しいフォルダーを選択し、新しいフォルダーを名前`CL`します。 このフォルダーを作成した後を追加してという名前の新しいクラス`ProductsCL.cs`します。


![CL をという名前の新しいフォルダーと ProductsCL.cs という名前のクラスを追加します。](caching-data-in-the-architecture-cs/_static/image2.png)

**図 2**: という名前の新しいフォルダーを追加`CL`とという名前のクラス `ProductsCL.cs`


`ProductsCL`クラスは、対応するビジネス ロジック層のクラスで見つかったデータ アクセスおよび変更方法の同じセットを含める必要があります (`ProductsBLL`)。 Let s を構築、これらのメソッドのすべての作成、CL で使用されるパターンの雰囲気をいくつかここではなく。 具体的には、追加します、`GetProducts()`と`GetProductsByCategoryID(categoryID)`手順 3 でのメソッドと`UpdateProduct`手順 4. でオーバー ロードします。 残りを追加する`ProductsCL`メソッドと`CategoriesCL`、 `EmployeesCL`、および`SuppliersCL`都合のクラス。

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>手順 2: 読み取りとデータのキャッシュへの書き込み

キャッシュ機能が内部的には、前のチュートリアルで調査されて ObjectDataSource は、BLL から取得されたデータを格納するのに ASP.NET のデータ キャッシュを使用します。 データ キャッシュもアクセスできますプログラムによって ASP.NET ページの分離コード クラスからでも、web アプリケーション アーキテクチャのクラス。 ASP.NET ページの分離コード クラスからのデータ キャッシュへの書き込みを読み取って、するには、次のパターンを使用します。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache`クラス](https://msdn.microsoft.com/library/system.web.caching.cache.aspx)s [ `Insert`メソッド](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx)がオーバー ロードの数。 `Cache["key"] = value` `Cache.Insert(key, value)`は同義ですし、定義済みの有効期限なしの指定したキーを使用してキャッシュに項目を追加両方。 通常、依存関係、時間ベースの有効期限、またはその両方として、キャッシュに項目を追加するときに、有効期限を指定します。 その他のいずれかを使用して、`Insert`依存関係または時間ベースの有効期限情報を提供するメソッドのオーバー ロードします。

メソッドは、そうである場合と、要求されたデータが、キャッシュにある場合をまず確認する必要がありますキャッシュ レイヤーは、そこを返します。 要求されたデータがキャッシュにない場合は、適切な BLL メソッドを呼び出す必要があります。 その戻り値はキャッシュされ、返される、次のシーケンス図に示すようにする必要があります。


![場合、キャッシュ層メソッドがキャッシュからデータを返すことの利用可能](caching-data-in-the-architecture-cs/_static/image3.png)

**図 3**: 場合、キャッシュ層メソッドがキャッシュからデータを返すことの利用可能


図 3 に示すように、シーケンスは、次のパターンを使用して、CL クラスで実現されます。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

ここでは、*型*キャッシュに格納されるデータの種類は、`Northwind.ProductsDataTable`などの*キー*キャッシュ項目を一意に識別するキーです。 場合、指定した項目*キー*し、キャッシュにない*インスタンス*なります`null`データは、適切な BLL メソッドから取得され、キャッシュに追加します。 時間によって`return instance`に達すると、*インスタンス*BLL から取り出したりキャッシュからいずれかと、データへの参照が含まれています。

キャッシュからデータにアクセスするときに、上記のパターンを使用することを確認します。 これは一見、それと同等では次のパターンには、競合状態が導入されていますが、わずかな違いが含まれています。 競合状態は、自身を散発的に明らかにし、再現が難しいため、デバッグが難しいです。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

この 2 つ目の違い、不適切なコード スニペットはではなく、キャッシュされた項目への参照を格納するローカル変数に、データ キャッシュが、条件付きステートメントに直接アクセス*と*で、`return`します。 このコードに達した場合を想像してください。`Cache["key"]`以外`null`、する前に、`return`ステートメントに達すると、システムの削除、*キー*キャッシュから。 このまれな場合は、コードから返される、`null`予期される型のオブジェクトではなく値します。

> [!NOTE]
> データ キャッシュとは、スレッド セーフであるのための単純な読み取りまたは書き込みスレッド アクセスを同期する必要はありません。 ただし、アトミックである必要があるキャッシュ内のデータに対して複数の操作を実行する必要がある場合は、ロックやその他のスレッド セーフを確保するメカニズムを実装する責任を負いますは。 参照してください[ASP.NET キャッシュへのアクセスの同期](http://www.ddj.com/184406369)詳細についてはします。


項目を使用して、データ キャッシュからプログラムで削除できる、 [ `Remove`メソッド](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx)ようになります。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>手順 3: から製品情報を返す、`ProductsCL`クラス

このチュートリアルが s から製品情報を返すための 2 つのメソッドを実装できるようにするため、`ProductsCL`クラス:`GetProducts()`と`GetProductsByCategoryID(categoryID)`します。 使用するような`ProductsBL`でビジネス ロジック層では、クラス、 `GetProducts()` 、cl メソッドとして製品のすべてに関する情報を返します、`Northwind.ProductsDataTable`オブジェクト、中に`GetProductsByCategoryID(categoryID)`指定したカテゴリからのすべての製品を返します。

次のコードでメソッドの一部を示しています、`ProductsCL`クラス。


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

最初に、注意してください、`DataObject`と`DataObjectMethodAttribute`クラスとメソッドに適用される属性。 これらの属性は、クラスおよびメソッドが、ウィザードの手順で表示する必要がありますされますを示す、ObjectDataSource のウィザードに情報を提供します。 CL クラスとメソッドがプレゼンテーション層で ObjectDataSource からアクセスされる、ために、デザイン時エクスペリエンスを強化するためにこれらの属性を追加しました。 参照、[ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-cs.md)」チュートリアルをこれらの属性とその影響をより詳しく説明します。

`GetProducts()`と`GetProductsByCategoryID(categoryID)`メソッドから返されたデータ、`GetCacheItem(key)`メソッドは、ローカル変数に割り当てられています。 `GetCacheItem(key)`メソッドで、間もなく、について説明しますがに基づいて、指定されたキャッシュから特定の項目を返します*キー*します。 キャッシュにこのようなデータが見つからない場合、対応するから取得される`ProductsBLL`クラス メソッドとを使用してキャッシュに追加し、`AddCacheItem(key, value)`メソッド。

`GetCacheItem(key)`と`AddCacheItem(key, value)`データ キャッシュ、読み取りと書き込みの値とそれぞれのメソッドのインターフェイスします。 `GetCacheItem(key)`メソッドが 2 つの簡単です。 単に渡されるを使用してキャッシュ クラスから値を返す*キー*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` 使用しません*キー*呼び出しが、代わりに、指定した値、`GetCacheKey(key)`を返すメソッド、*キー* ProductsCache-先頭に付加します。 `MasterCacheKeyArray`、によっても使用 ProductsCache、文字列を保持する、`AddCacheItem(key, value)`メソッドを一時的に表示されるようにします。

ASP.NET ページの分離コード クラスからのデータ キャッシュを使ってアクセスできます、`Page`クラス s [ `Cache`プロパティ](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)のような構文と`Cache["key"] = value`、手順 2. で説明したようにします。 アーキテクチャ内のクラス、データ キャッシュ アクセスできるいずれかを使用して`HttpRuntime.Cache`または`HttpContext.Current.Cache`します。 [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)のブログ エントリ[HttpRuntime.Cache vs します。HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache)を使用してわずかにパフォーマンスの利点をノート`HttpRuntime`の代わりに`HttpContext.Current`。 結局のところ、`ProductsCL`使用`HttpRuntime`します。

> [!NOTE]
> クラス ライブラリ プロジェクトを使用して、アーキテクチャを実装する場合への参照を追加する必要があります、`System.Web`アセンブリを使用するには、 [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx)と[HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)クラス。


キャッシュに項目が見つからない場合、`ProductsCL`クラス s のメソッドが BLL からデータを取得しを使用してキャッシュに追加、`AddCacheItem(key, value)`メソッド。 追加する*値*キャッシュには、60 秒間の有効期限を使用する次のコードを使用できます。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` 将来の中で 60 秒間の時間ベースの期限を指定[ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx)スライディング有効期限はありませんが s をことを示します。 この中に`Insert`メソッドのオーバー ロードが絶対パス両方のパラメーターを入力し、有効期限をスライディング、行うことができますのみ、2 つのいずれか。 絶対時刻と期間の両方を指定しようとした場合、`Insert`メソッドがスローされます、`ArgumentException`例外。

> [!NOTE]
> この実装の`AddCacheItem(key, value)`メソッドは、現在いくつかの欠点が。 アドレスを手順 4. でこれらの問題を克服します。


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>手順 4: は、アーキテクチャを変更はキャッシュと、データを無効化

データの取得方法とキャッシュ レイヤーは BLL として挿入、更新、およびデータを削除するのと同じ方法を提供する必要があります。 CL のデータ変更メソッドは、キャッシュされたデータを変更しないでくださいがではなく、BLL の対応するデータ変更メソッドを呼び出すし、キャッシュが無効にします。 これには、そのキャッシュ機能が有効にすると、ObjectDataSource が適用される同じ動作を前のチュートリアルで説明したように、その`Insert`、 `Update`、または`Delete`メソッドが呼び出されます。

次`UpdateProduct`オーバー ロードは、CL でデータの変更メソッドを実装する方法を示しています。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

適切なデータ変更のビジネス ロジック層のメソッドが呼び出されますが、その応答が返される前に、キャッシュを無効にする必要があります。 残念ながら、キャッシュを無効化がわかりやすいため、`ProductsCL`クラス s`GetProducts()`と`GetProductsByCategoryID(categoryID)`メソッドの各項目をキャッシュに追加、さまざまなキーを持つ、`GetProductsByCategoryID(categoryID)`メソッドは、それぞれ別のキャッシュ項目を追加します一意。*categoryID*します。

キャッシュを無効化、ときに削除する必要があります*すべて*によって追加された項目の`ProductsCL`クラス。 これは、関連付けることによって実現できます、*依存関係をキャッシュ*でキャッシュに追加された各項目で、`AddCacheItem(key, value)`メソッド。 一般に、キャッシュの依存関係は、キャッシュ、ファイル システム、または Microsoft SQL Server データベースからデータをファイルに別のアイテムを指定できます。 依存関係が変わるかがキャッシュから削除すると、関連付けられているキャッシュ項目は自動的にキャッシュから削除します。 このチュートリアルでは、キャッシュによって追加されたすべての項目の依存関係をキャッシュとして機能するには追加の項目を作成したい、`ProductsCL`クラス。 これにより、これらすべての項目できるキャッシュから削除、キャッシュの依存関係を削除するだけで。

Let s の更新プログラム、`AddCacheItem(key, value)`メソッドの各項目はこの方法でキャッシュに追加されるように関連付けられている 1 つのキャッシュ依存関係。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` ProductsCache、単一の値を保持する文字列配列です。 最初に、キャッシュ項目がキャッシュに追加され、現在の日付と時刻を割り当てられます。 キャッシュ項目が既に存在する場合は更新されます。 次に、キャッシュの依存関係が作成されます。 [ `CacheDependency`クラス](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx)コンス トラクターがオーバー ロードでは、多数ありますが、ここで使用されている 1 つでは、2 つが必要です`string`入力配列。 1 つ目は、依存関係として使用されるファイルのセットを指定します。 T をすべてファイル ベースの依存関係の値を使用することはありませんので`null`は最初の入力パラメーターの使用します。 2 番目の入力パラメーターには、依存関係として使用するキャッシュ キーのセットを指定します。 ここでは、単一の依存関係を指定しました`MasterCacheKeyArray`します。 `CacheDependency`に渡されるし、`Insert`メソッド。

この変更により`AddCacheItem(key, value)`invaliding、キャッシュは、依存関係を削除するだけです。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>手順 5: プレゼンテーション層からのキャッシュ レイヤーの呼び出し

データを操作する手法を使用してこのチュートリアルで調査して、キャッシュ層のクラスとメソッドを使用できます。 キャッシュされたデータの操作を示すためには、変更内容を保存、`ProductsCL`クラスを開き、`FromTheArchitecture.aspx`ページで、`Caching`フォルダー、GridView を追加します。 GridView のスマート タグから新しい ObjectDataSource を作成します。 ウィザードの最初の手順で表示する必要があります、`ProductsCL`クラスのドロップダウン リストからオプションのいずれか。


[![ビジネス オブジェクトのドロップダウン リストで ProductsCL クラスが含まれています。](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**図 4**:`ProductsCL`ビジネス オブジェクトのドロップダウン リストにクラスが含まれる ([フルサイズの画像を表示する をクリックします](caching-data-in-the-architecture-cs/_static/image6.png))。


選択した後`ProductsCL`、[次へ] をクリックします。 タブで、ドロップダウン リストが 2 つの項目 -`GetProducts()`と`GetProductsByCategoryID(categoryID)`更新プログラム タブであり、唯一`UpdateProduct`オーバー ロードします。 選択、`GetProducts()`メソッドを選択します タブから、`UpdateProducts`し更新 タブからメソッドを終了します。


[![ドロップダウン リストに、ProductsCL クラスのメソッドの一覧します。](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**図 5**: `ProductsCL` 、ドロップダウン リストにクラスのメソッドの一覧 ([フルサイズの画像を表示する をクリックします](caching-data-in-the-architecture-cs/_static/image9.png))。


ウィザードを完了すると、Visual Studio は、ObjectDataSource s を設定`OldValuesParameterFormatString`プロパティを`original_{0}`GridView に適切なフィールドを追加します。 変更、`OldValuesParameterFormatString`プロパティの既定値に戻す`{0}`、およびページング、並べ替え、および編集をサポートするために、GridView を構成します。 以降、 `UploadProducts` CL で使用するオーバー ロードは、編集した製品の名前と価格、これらのフィールドのみが編集可能になるよう、GridView を制限します。

GridView のフィールドを含める前のチュートリアルでは、 `ProductName`、 `CategoryName`、および`UnitPrice`フィールド。 この書式設定と構造体にレプリケートする自由、マークアップは、次のようになりますこの場合、宣言型、GridView コントロールと ObjectDataSource s:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

この時点で、キャッシュ層を使用するページがあります。 アクションでは、キャッシュを表示するにブレークポイントを設定、`ProductsCL`クラス s`GetProducts()`と`UpdateProduct`メソッド。 キャッシュからプルされた並べ替えとページング、データを表示するには、コードをステップ実行、ブラウザーでページを参照してください。 レコードを更新し、キャッシュが無効になり、データが GridView にバインドし直すときに、BLL から取得は、その結果、ことに注意してください。

> [!NOTE]
> 本稿付属のダウンロードで提供されるキャッシュ レイヤーは完了しません。 1 つのクラスが含まれている`ProductsCL`、いくつかのメソッドのスポーツがのみです。 1 つの ASP.NET ページのみが、CL を使用してさらに、(`~/Caching/FromTheArchitecture.aspx`) BLL を直接参照他のすべてのユーザーもいます。 アプリケーションで、CL を使用する場合は、プレゼンテーション層からのすべての呼び出しは必要がありますしなければ、CL CL のクラスに移動し、メソッドは、これらのクラスと BLL のプレゼンテーション層で使用される現在のメソッドについて説明します。


## <a name="summary"></a>まとめ

プレゼンテーション層を ASP.NET 2.0 の SqlDataSource、ObjectDataSource コントロールにあるキャッシュを適用できる、中に責任を理想的にはキャッシュは、アーキテクチャの別のレイヤーに委任します。 このチュートリアルでは、プレゼンテーション層とビジネス ロジック層の間に存在するキャッシュ レイヤーを作成しました。 キャッシュ レイヤーは、同じクラスと、BLL に存在し、プレゼンテーション層から呼び出されるメソッドのセットを提供する必要があります。

これであり、上記のチュートリアルで調査されているキャッシュ レイヤーの例は、発生*事後対応型の読み込み*します。 事後対応型の読み込み、データの要求が行われ、キャッシュからそのデータが不足している場合にのみ、データはキャッシュに読み込まれます。 データが*事前に読み込まれた*をキャッシュに、手法、データをキャッシュに前に読み込ま実際に必要です。 次のチュートリアルでは、アプリケーションの起動時にキャッシュに静的な値を格納する方法を見ると、プロアクティブな読み込みの例が表示されます。

満足のプログラミングです。

## <a name="about-the-author"></a>執筆者紹介

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックおよびの創設者の著者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、Microsoft Web テクノロジと 1998 年から携わっています。 Scott は、フリーのコンサルタント、トレーナー、およびライターとして動作します。 最新の著書は[ *Sams 教える自分で ASP.NET 2.0 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)します。 彼に到達できる[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com) 彼のブログにあるでまたは[ http://ScottOnWriting.NET](http://ScottOnWriting.NET)します。

## <a name="special-thanks-to"></a>特別なに感謝します。

このチュートリアル シリーズは、多くの便利なレビュー担当者によってレビューされました。 このチュートリアルでは、潜在顧客レビュー担当者は、Teresa Murph でした。 今後、MSDN の記事を確認したいですか。 場合は、筆者に[mitchell@4GuysFromRolla.comします。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [前へ](caching-data-with-the-objectdatasource-cs.md)
> [次へ](caching-data-at-application-startup-cs.md)
