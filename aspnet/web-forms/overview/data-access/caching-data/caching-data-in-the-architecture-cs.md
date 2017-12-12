---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: "(C#)、アーキテクチャのデータのキャッシュ |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルでは、プレゼンテーション層でキャッシュを適用する方法を学習します。 このチュートリアルでは、当社レイヤード architectu を活用する方法を学習しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 60bdab9dc8317c7f8753891b461da18f02993812
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="caching-data-in-the-architecture-c"></a>(C#)、アーキテクチャのデータのキャッシュ
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe)または[PDF のダウンロード](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> 前のチュートリアルでは、プレゼンテーション層でキャッシュを適用する方法を学習します。 このチュートリアルでは、利用するために、複数層のアーキテクチャのビジネス ロジック層でデータをキャッシュする方法について説明します。 キャッシュ レイヤーを含めるアーキテクチャを拡張することによってこの作業を行います。


## <a name="introduction"></a>はじめに

前のチュートリアルで説明したとおり、ObjectDataSource のデータのキャッシュは、いくつかのプロパティを設定するなどの単純です。 残念ながら、ASP.NET ページのキャッシュ ポリシーを密に結合するプレゼンテーション層でキャッシュ、ObjectDataSource が適用されます。 解除するには、このような couplings を許可する、複数層のアーキテクチャを作成する理由の 1 つです。 データ アクセス層、データ アクセスの詳細を分離するときに、ビジネス ロジック層はたとえば、ASP.NET ページからビジネス ロジックを切り離します。 このビジネス ロジックとデータ アクセスの詳細の分離は優先、一部を読みやすく、保守性の向上、および変更をより柔軟なシステムがいるためです。 ドメインのナレッジとプレゼンテーション層が t の開発者が自分のジョブを実行するために、データベースの詳細について理解しておく必要がある作業分担こともできます。 キャッシュ ポリシーをプレゼンテーション層から分離することは、同様のメリットを提供します。

このチュートリアルではこのアーキテクチャによりを補強おは、*キャッシュ レイヤー* (または略して CL) のキャッシュ ポリシーを使用します。 キャッシュ レイヤーが含まれます、`ProductsCL`などのメソッドに製品情報へのアクセスを提供するクラス`GetProducts()`、`GetProductsByCategoryID(categoryID)`などと、呼び出されるとは、キャッシュからデータを取得する最初の試行に、します。 これらのメソッドを適切な呼び出しは、キャッシュが空の場合は、 `ProductsBLL` DAL からデータを入手はさらに、BLL 内のメソッドです。 `ProductsCL`メソッドが返す前に、BLL から取得されたデータをキャッシュします。

図 1 に示す、CL は、プレゼンテーション層とビジネス ロジック層との間存在します。


![キャッシュ レイヤー (CL) が別のレイヤーのアーキテクチャ](caching-data-in-the-architecture-cs/_static/image1.png)

**図 1**:「キャッシュ レイヤー (CL) が別のレイヤーのアーキテクチャ


## <a name="step-1-creating-the-caching-layer-classes"></a>手順 1: キャッシュ レイヤー クラスを作成します。

このチュートリアルでは 1 つのクラスを使用して非常に単純な CL を作成、`ProductsCL`メソッドのほんの一部のみを持ちます。 完全なキャッシュ レイヤー全体のアプリケーションの作成が必要になります作成`CategoriesCL`、 `EmployeesCL`、および`SuppliersCL`クラス、および BLL 内の各データ アクセスまたは変更メソッドのこれらのキャッシュ レイヤーのクラスのメソッドを提供します。 同様に、BLL および DAL では、キャッシュ レイヤー理想的として実装してください、別のクラス ライブラリ プロジェクトただし、内のクラスとして実装しましたが、`App_Code`フォルダーです。

Let s に新しいサブフォルダーを作成、DAL および BLL クラスからより多くのクリーンに独立した、CL クラスに、`App_Code`フォルダーです。 右クリックし、`App_Code`ソリューション エクスプ ローラーでフォルダーが新しいフォルダーを選択し、新しいフォルダーの名前を付けます`CL`です。 このフォルダーを作成した後を追加してという名前の新しいクラス`ProductsCL.cs`です。


![CL をという名前の新しいフォルダーと ProductsCL.cs をという名前のクラスを追加します。](caching-data-in-the-architecture-cs/_static/image2.png)

**図 2**: という名前の新しいフォルダーを追加`CL`とという名前のクラス`ProductsCL.cs`


`ProductsCL`クラスは、対応するビジネス ロジック層クラスにあるデータ アクセスと変更方法の同じセットを含める必要があります (`ProductsBLL`)。 Let s だけで使用したビルドをここでは、いくつかのパターンを大まかに、CL でこれらのメソッドの作成する代わり。 具体的には、追加、`GetProducts()`と`GetProductsByCategoryID(categoryID)`手順 3. でメソッドおよび`UpdateProduct`手順 4. でオーバー ロードします。 残りを追加する`ProductsCL`メソッドおよび`CategoriesCL`、 `EmployeesCL`、および`SuppliersCL`都合のクラスです。

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>手順 2: 読み取りと、データ キャッシュへの書き込み

前のチュートリアルで内部的に探索機能をキャッシュ ObjectDataSource BLL から取得されたデータを格納するのに ASP.NET データ キャッシュを使用します。 データ キャッシュもアクセスできますプログラムで ASP.NET ページの分離コード クラスとは、web アプリケーションのアーキテクチャのクラスから。 読み書き可能なデータ キャッシュにページ s の ASP.NET 分離コード クラスから、するには、次のパターンを使用します。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache`クラス](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.aspx)s [ `Insert`メソッド](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.insert.aspx)数のオーバー ロードがします。 `Cache["key"] = value``Cache.Insert(key, value)`同意語として使用し、定義済みの有効期限なしの指定したキーを使用して、キャッシュに項目を追加両方です。 通常は、依存関係、時間ベースの期限切れ、またはその両方として、キャッシュにアイテムを追加するときに、有効期限を指定します。 その他のいずれかを使用して`Insert`依存関係または時間ベースの有効期限情報を提供するメソッドのオーバー ロードします。

最初とを確認する場合は、要求されたデータがキャッシュに、必要な場合は、s メソッドが必要なキャッシュ レイヤーは、そこからそれを返します。 要求されたデータがキャッシュにない場合は、適切な BLL メソッド呼び出す必要があります。 その戻り値はキャッシュされ、返される、次のシーケンス図に示すようにする必要があります。


![場合、キャッシュ レイヤーのメソッドが、キャッシュからデータを返す、s 利用可能](caching-data-in-the-architecture-cs/_static/image3.png)

**図 3**: 場合、キャッシュ レイヤーのメソッドが、キャッシュからデータを返す、s 利用可能


図 3 に、シーケンスは、CL のクラスに次のパターンを使用して行われます。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

ここでは、*型*キャッシュに格納されているデータの種類は、`Northwind.ProductsDataTable`などの*キー*キャッシュ アイテムを一意に識別するキーです。 場合、指定した項目*キー*し、キャッシュに含まれない*インスタンス*なります`null`データが適切な BLL メソッドから取得され、キャッシュに追加します。 時間によって`return instance`に達すると、*インスタンス*BLL から取り出したりキャッシュからいずれかと、データへの参照が含まれています。

キャッシュからデータにアクセスするときに、上記のパターンを使用することを確認します。 一見すると、それと同等のように検索する次のパターンには、競合状態を導入する微妙な違いが含まれています。 競合状態は散発的明らかにそれ自体を再現するが困難なためにデバッグが困難です。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

この 2 番目の差を不適切なコード スニペットはではなく、ローカル変数にキャッシュされた項目への参照を格納する、データ キャッシュにアクセス条件ステートメント内で直接*と*で、`return`です。 このコードに達するを想像してください。`Cache["key"]`以外`null`、その前に、`return`ステートメントに達すると、システムが削除*キー*キャッシュからです。 このまれなケースで、コードが返されます、`null`予期された型のオブジェクトではなく値します。

> [!NOTE]
> データ キャッシュの単純な読み取りまたは書き込みスレッド アクセスを同期する必要があるため、スレッド セーフであるとは。 ただし、アトミックである必要がキャッシュ内のデータに対して複数の操作を実行する必要がある場合は、ロック、またはその他のスレッド セーフを確保するメカニズムを実装する責任は。 参照してください[ASP.NET キャッシュへのアクセスの同期](http://www.ddj.com/184406369)詳細についてはします。


項目をプログラムによって、データを使用してキャッシュから削除する、 [ `Remove`メソッド](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.remove.aspx)次のようにします。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>手順 3: がから製品情報を返す、`ProductsCL`クラス

このチュートリアル let s から製品情報を返すための 2 つのメソッドを実装するため、`ProductsCL`クラス:`GetProducts()`と`GetProductsByCategoryID(categoryID)`です。 使用するような`ProductsBL`ビジネス ロジック層内のクラス、`GetProducts()`メソッド、CL では、すべての製品としてに関する情報を返します、`Northwind.ProductsDataTable`オブジェクト、中に`GetProductsByCategoryID(categoryID)`指定したカテゴリからの製品のすべてを返します。

次のコード内のメソッドの一部を示しています、`ProductsCL`クラス。


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

最初に、注意してください、`DataObject`と`DataObjectMethodAttribute`クラスおよびメソッドに適用される属性です。 これらの属性は、クラスとメソッドが、ウィザードの手順で表示する必要がありますされるを示す、ObjectDataSource のウィザードに情報を提供します。 CL クラスとメソッドがプレゼンテーション層で、ObjectDataSource からアクセスされる、ため、デザイン時のエクスペリエンスを強化するためにこれらの属性を追加します。 参照、[ビジネス ロジック層を作成する](../introduction/creating-a-business-logic-layer-cs.md)チュートリアルこれらの属性とその影響についてより詳細に説明します。

`GetProducts()`と`GetProductsByCategoryID(categoryID)`メソッドから返されたデータ、`GetCacheItem(key)`メソッドは、ローカルの変数に代入されます。 `GetCacheItem(key)`メソッドで、間もなく検証、に基づいて、指定されたキャッシュから特定の項目が返されます*キー*です。 キャッシュ内でこのようなデータが見つからない場合は、対応するから取得`ProductsBLL`クラス メソッドとを使用してキャッシュに追加し、`AddCacheItem(key, value)`メソッドです。

`GetCacheItem(key)`と`AddCacheItem(key, value)`メソッド インターフェイスは、それぞれデータ キャッシュ、読み取りおよび書き込み値を使用します。 `GetCacheItem(key)`メソッドは、2 つのシンプルです。 渡されたを使用してキャッシュ クラスから単純に、値が返されます*キー*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)`使用しません*キー*呼び出し、代わりに、指定されたように値、`GetCacheKey(key)`を返すメソッド、*キー* ProductsCache-先頭に付加します。 `MasterCacheKeyArray`、ProductsCache、文字列を保持するもで使用される、`AddCacheItem(key, value)`メソッドをすぐにおわかりのようです。

ASP.NET ページの分離コード クラスから、データ キャッシュにアクセスする使用、`Page`クラス s [ `Cache`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.page.cache.aspx)のような構文と`Cache["key"] = value`手順 2. で説明したように、します。 アーキテクチャ内のクラスから、データ キャッシュを使ってアクセスできますか`HttpRuntime.Cache`または`HttpContext.Current.Cache`です。 [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)のブログ エントリ[HttpRuntime.Cache vs です。HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache)でわずかなパフォーマンスを得ることをノート`HttpRuntime`の代わりに`HttpContext.Current`以外の場合はその結果、`ProductsCL`を使用して`HttpRuntime`です。

> [!NOTE]
> クラス ライブラリ プロジェクトを使用して、アーキテクチャが実装されたかどうかへの参照を追加する必要があります、`System.Web`アセンブリを使用するために、 [HttpRuntime](https://msdn.microsoft.com/en-us/library/system.web.httpruntime.aspx)と[HttpContext](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.aspx)クラスです。


項目は、キャッシュ内で見つからない場合、`ProductsCL`クラス s のメソッドが BLL からデータを取得しを使用してキャッシュに追加する、`AddCacheItem(key, value)`メソッドです。 追加する*値*キャッシュに 60 秒の時刻の有効期限を使用する次のコードを使用でした。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)`将来の中で時間ベースの有効期限が 60 秒を指定[ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/en-us/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx)ありませんスライド式有効期限が s を示します。 この中に`Insert`メソッドのオーバー ロードには絶対両方のパラメーターの入力の有効期限をスライドさせて、提供し、のみ、2 つのいずれか。 絶対時刻とする時間間隔の両方を指定しようとする場合、`Insert`メソッドがスローされます、`ArgumentException`例外。

> [!NOTE]
> この実装、`AddCacheItem(key, value)`メソッドは現在いくつかの欠点がします。 対処し、手順 4. でこれらの問題を解決しています。


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>手順 4: は、アーキテクチャを変更では、ときに、データ キャッシュを無効になります

データの取得方法、と共にキャッシュ レイヤーは挿入、更新、およびデータを削除するため、BLL として同じメソッドを提供する必要があります。 CL のデータの変更方法は、キャッシュされたデータを変更しないでくださいがではなくメソッドを呼び出し、BLL s 対応するデータ変更をし、キャッシュが無効にします。 これには、そのキャッシュの機能が有効な場合に、ObjectDataSource が適用される同じ動作を前のチュートリアルで説明したとおり、およびその`Insert`、 `Update`、または`Delete`メソッドが呼び出されます。

次`UpdateProduct`オーバー ロードは、CL でデータの変更メソッドを実装する方法を示します。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

適切なデータ変更のビジネス ロジック層メソッドが呼び出されますが、その応答が返される前に、キャッシュを無効にする必要があります。 残念ながら、キャッシュを無効には単純なため、`ProductsCL`クラス s`GetProducts()`と`GetProductsByCategoryID(categoryID)`メソッドの各項目をキャッシュに追加、異なるキーを持つ、`GetProductsByCategoryID(categoryID)`メソッドごとに異なるキャッシュ項目を追加する一意*categoryID*です。

キャッシュが無効になる、ときに削除する必要があります*すべて*によって追加された項目の`ProductsCL`クラスです。 これには、関連付けることによって、*依存関係をキャッシュ*でキャッシュに追加された各項目に、`AddCacheItem(key, value)`メソッドです。 一般に、キャッシュの依存関係は、キャッシュ、ファイル システム、または Microsoft SQL Server データベースからデータをファイルに別のアイテムを指定できます。 依存関係が変わるかがキャッシュから削除すると、関連付けられているキャッシュ項目は自動的にキャッシュから削除します。 このチュートリアルでは、キャッシュによって追加されるすべてのアイテムの依存関係をキャッシュとして機能するためには追加の項目を作成する、`ProductsCL`クラスです。 こうすれば、これらの項目すべて削除できますキャッシュから、キャッシュの依存関係を削除します。

使用すると、s の更新プログラム、`AddCacheItem(key, value)`メソッドの各項目がこのメソッドで、キャッシュに追加されるように関連付けられて、1 つのキャッシュの依存関係。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray`ProductsCache、単一の値を保持する文字列配列です。 最初に、キャッシュ項目がキャッシュに追加され、現在の日付と時刻を割り当てられます。 キャッシュ項目が既に存在する場合は更新されます。 次に、キャッシュの依存関係が作成されます。 [ `CacheDependency`クラス](https://msdn.microsoft.com/en-US/library/system.web.caching.cachedependency(VS.80).aspx)s のコンス トラクターのオーバー ロードの数が、ここで使用されている 1 つでは、2 つが必要です`string`の入力を配列します。 1 つ目は、一連の依存関係として使用するファイルを指定します。 ファイル ベースの依存関係の値を使用する必要ありませんので`null`は最初の入力パラメーターを使用します。 2 番目の入力パラメーターは、依存関係として使用するキャッシュ キーのセットを指定します。 ここでは、単一の依存関係を指定して`MasterCacheKeyArray`です。 `CacheDependency`に渡され、`Insert`メソッドです。

この変更により`AddCacheItem(key, value)`invaliding、キャッシュは、依存関係を削除するように単純です。


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>手順 5: は、プレゼンテーション層から、キャッシュ レイヤーを呼び出す

データを操作する手法を使用してこれらのチュートリアル全体で検証したキャッシュ レイヤーのクラスとメソッドを使用できます。 キャッシュされたデータの操作を示すためには、に対する変更を保存、`ProductsCL`クラスを開き、 `FromTheArchitecture.aspx`  ページで、`Caching`フォルダー GridView を追加します。 GridView s のスマート タグから新しい ObjectDataSource を作成します。 ウィザードの最初の手順で表示されるはずの`ProductsCL`クラスのドロップダウン リストからオプションのいずれか。


[![ビジネス オブジェクトのドロップダウン リストで、ProductsCL クラスが含まれています。](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**図 4**:`ProductsCL`ビジネス オブジェクトのドロップダウン リストにクラスが含まれる ([フルサイズのイメージを表示するをクリックして](caching-data-in-the-architecture-cs/_static/image6.png))


選択した後`ProductsCL`、[次へ] をクリックします。 タブで、ドロップダウン リストが 2 つの項目の`GetProducts()`と`GetProductsByCategoryID(categoryID)`し、[更新] タブが唯一の`UpdateProduct`オーバー ロードします。 選択、`GetProducts()`メソッドの選択 タブおよび`UpdateProducts`更新 タブをクリックしてからメソッドを完了します。


[![ドロップダウン リストに ProductsCL クラスのメソッドの一覧します。](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**図 5**:`ProductsCL`ドロップダウン リストにクラスのメソッドの一覧 ([フルサイズのイメージを表示するをクリックして](caching-data-in-the-architecture-cs/_static/image9.png))


ウィザードを完了すると、Visual Studio は、ObjectDataSource s を設定`OldValuesParameterFormatString`プロパティを`original_{0}`GridView に適切なフィールドを追加します。 変更、`OldValuesParameterFormatString`プロパティの既定値に戻す`{0}`、およびページング、並べ替え、および編集をサポートする GridView を構成します。 以降、 `UploadProducts` CL で使用するオーバー ロードには、編集した製品の名前と価格、これらのフィールドのみが編集できるように、GridView を制限します。

前のチュートリアルでは、定義したため、フィールドを含める GridView、 `ProductName`、 `CategoryName`、および`UnitPrice`フィールドです。 自由書式設定と構造体をレプリケートすることは、宣言、GridView と ObjectDataSource s 場合マークアップは、次のようになります。


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

この時点で、キャッシュ レイヤーを使用するページがあります。 アクションでキャッシュを表示するブレークポイントを設定、`ProductsCL`クラス s`GetProducts()`と`UpdateProduct`メソッドです。 並べ替えとページングのデータを表示するのには、キャッシュから取得したときに、ブラウザーとコードをステップ内のページを参照してください。 レコードを更新し、キャッシュを無効にし、データが GridView にバインドし直すときに、BLL から取得は、その結果、ことに注意してください。

> [!NOTE]
> この記事の付属のダウンロードで提供されるキャッシュ レイヤーは完了しません。 1 つのクラスが含まれている`ProductsCL`、のみに、いくつかのメソッドをスポーツをします。 1 つの ASP.NET ページのみが、CL を使用してさらに、(`~/Caching/FromTheArchitecture.aspx`) BLL を直接参照他のすべてのユーザーもいます。 アプリケーションで、CL を使用する場合は、プレゼンテーション層からすべての呼び出しは必要があるを必要とすると、CL CL のクラスに移動し、メソッドは、これらのクラスとプレゼンテーション層によって現在使用 BLL でメソッドについて説明します。


## <a name="summary"></a>概要

キャッシュは、ASP.NET 2.0 の SqlDataSource によるプレゼンテーション層と ObjectDataSource コントロールに適用できる、中に責任を理想的にはキャッシュは、アーキテクチャの別のレイヤーに委任します。 このチュートリアルでは、プレゼンテーション層とビジネス ロジック層の間にあるキャッシュ レイヤーを作成しました。 キャッシュ レイヤーは、同じクラスと BLL 内に存在し、プレゼンテーション層から呼び出されるメソッドのセットを提供する必要があります。

この例と前のチュートリアルでは、探索おキャッシュ レイヤーの例は、発生*事後対応型の読み込み*です。 事後対応型の読み込みとデータの要求が行われ、キャッシュからそのデータが不足している場合にのみ、データはキャッシュに読み込まれます。 データが*能動的に読み込まれた*をキャッシュに技法にデータを読み込む、キャッシュが実際に必要とせずにします。 次のチュートリアルでは、キャッシュ アプリケーションの起動時に静的な値を格納する方法について詳しく見てみると、プロアクティブな読み込みの例が表示されます。

満足プログラミング!

## <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。

## <a name="special-thanks-to"></a>感謝の特別な

このチュートリアルの系列は既に多くの便利なレビュー担当者によって確認済みです。 このチュートリアルのレビュー担当者の潜在顧客が Teresa Murph しました。 今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[mitchell@4GuysFromRolla.comです。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[前へ](caching-data-with-the-objectdatasource-cs.md)
[次へ](caching-data-at-application-startup-cs.md)
