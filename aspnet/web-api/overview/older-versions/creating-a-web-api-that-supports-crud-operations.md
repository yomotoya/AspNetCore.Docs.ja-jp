---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: ASP.NET Web API 1 で CRUD 操作の有効化 |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスで CRUD 操作をサポートする方法を示します。 チュートリアルの Visual Studio 2012 Web AP で使用されるソフトウェアのバージョン.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402929"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>ASP.NET Web API 1 で CRUD 操作を有効にします。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスで CRUD 操作をサポートする方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Visual Studio 2012
> - Web API 1 (Web API 2 での動作も)


CRUD の略&quot;作成、読み取り、更新、および削除、&quot; 4 つの基本的なデータベース操作であります。 多くの HTTP サービスでは、REST api または REST のような Api を介して、CRUD 操作もモデルします。

このチュートリアルでは、非常に単純な web 製品の一覧を管理する API を構築します。 各製品は、名前、価格、およびカテゴリが含まれます (など&quot;toys&quot;または&quot;ハードウェア&quot;)、さらに製品の id。

製品の API は、次のメソッドに公開します。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| すべての製品の一覧を取得します。 | GET | /api/products |
| ID によって製品を取得します。 | GET | /api/products/*id* |
| カテゴリによって製品を取得します。 | GET | /api/products?category=*category* |
| 新しい成果物を作成します。 | POST | /api/products |
| 製品を更新します。 | PUT | /api/products/*id* |
| 製品を削除します。 | Del | /api/products/*id* |

パスに製品 ID を含む Uri の一部に注目してください。 たとえば、製品 ID が 28 を取得するクライアント GET 要求を送信`http://hostname/api/products/28`します。

### <a name="resources"></a>リソース

製品の API では、2 つのリソースの種類の Uri を定義します。

| リソース | URI |
| --- | --- |
| すべての製品の一覧。 | /api/products |
| 個別の製品です。 | /api/products/*id* |

### <a name="methods"></a>メソッド

4 つの主な HTTP メソッド (GET、PUT、POST、および DELETE) は、CRUD 操作にマップすることができます。

- 取得した指定した URI のリソースの表現を取得します。 サーバーの副作用が GET 必要なしです。
- PUT は、指定された URI にリソースを更新します。 PUT こともできます、指定した URI で新しいリソースを作成する場合は、サーバーにより、クライアントが新しい Uri を指定します。 このチュートリアルでは、API では PUT の作成はサポートされません。
- POST は、新しいリソースを作成します。 サーバーは、新しいオブジェクトの URI を割り当てるし、応答メッセージの一部としてこの URI を返します。
- 指定された URI にリソースを削除します。

注: PUT メソッドには、製品全体のエンティティが置き換えられます。 つまり、更新された製品の完全な表現を送信するクライアントが必要です。 部分的な更新をサポートする場合は、PATCH メソッドをお勧めします。 このチュートリアルは、修正プログラムを実装していません。

## <a name="create-a-new-web-api-project"></a>新しい Web API プロジェクトを作成します。

Visual Studio を使用して起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトに名前を&quot;ProductStore&quot;  をクリック**OK**します。

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログ ボックスで、 **Web API**  をクリック**OK**します。

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>モデルの追加

*モデル*は、アプリケーションのデータを表すオブジェクトです。 ASP.NET Web api では、モデルとして厳密に型指定された CLR オブジェクトを使用して、自動的にシリアル化されます XML または JSON をクライアントの。

データで構成の製品のためという名前の新しいクラスを作成します ProductStore api`Product`します。

ソリューション エクスプ ローラーが表示されない場合は、クリックして、**ビュー**メニュー選択し、**ソリューション エクスプ ローラー**します。 ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダー。 コンテキスト メニューから次のように選択します。**追加**を選択し、**クラス**します。 クラスの名前&quot;製品&quot;します。

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

次のプロパティを追加、`Product`クラス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>リポジトリを追加

製品のコレクションを格納する必要があります。 サービス実装からコレクションを分割することをお勧めします。 これにより、サービス クラスを書き直すことがなく、バッキング ストアを変更できます。 この型のデザインが呼び出される、*リポジトリ*パターン。 まず、リポジトリのジェネリック インターフェイスを定義します。

ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダー。 選択**追加**を選択し、**新しい項目の**します。

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

**テンプレート**ペインで、**インストールされたテンプレート**c# ノードを展開します。 C# の場合は、選択**コード**します。 コード テンプレートの一覧で選択**インターフェイス**します。 インターフェイスの名前&quot;IProductRepository&quot;します。

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

次の実装を追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

という名前の Models フォルダーに別のクラスを今すぐ追加&quot;ProductRepository します。&quot;このクラスが `IProductRespository` インターフェイスを実装します。 次の実装を追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

リポジトリは、ローカル メモリ内のリストを保持します。 チュートリアルについては、[ok] になりますが、実際のアプリケーションでは、データを外部から、データベースであるかを格納する、またはクラウド ストレージ。 リポジトリ パターンが容易にできるように実装を後で変更します。

## <a name="adding-a-web-api-controller"></a>Web API コント ローラーの追加

ASP.NET MVC を使用する場合、し、既に慣れてコント ローラーを使用します。 ASP.NET Web api で、*コント ローラー*は、クライアントから HTTP 要求を処理するクラスです。 プロジェクト作成時に、新しいプロジェクト ウィザードは 2 つのコント ローラーを作成します。 これらを参照するには、ソリューション エクスプ ローラーで Controllers フォルダーを展開します。

- HomeController は、従来の ASP.NET MVC コント ローラーです。 サイトの HTML ページの提供を担当し、web API に直接関連しません。
- ValuesController では、例の WebAPI コント ローラーです。

ソリューション エクスプ ローラーでファイルを右クリックして、ValuesController を削除してください**を削除します。** 今すぐよう、新しいコント ローラーを追加します。

**ソリューション エクスプ ローラー**、Controllers フォルダーを右クリックします。 選択**追加**選び**コント ローラー**します。

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

**コント ローラーの追加**ウィザードで、名前、コント ローラー &quot;ProductsController&quot;します。 **テンプレート**ドロップダウン リストで、**空の API コント ローラー**します。 次に、 **[追加]** をクリックします。

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> コント ローラーをという名前のフォルダーに、contollers を格納する必要はありません。 フォルダー名は重要です。ソース ファイルを整理する便利な方法では単純にすることをお勧めします。


**コント ローラーの追加**ProductsController.cs Controllers フォルダーをという名前のファイルを作成します。 このファイルがまだ開いていない場合、は、開くファイルをダブルクリックします。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

保持するフィールドを追加、 **IProductRepository**インスタンス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> 呼び出す`new ProductRepository()`コント ローラーでないにとって最善の設計では、コント ローラーの特定の実装に結び付けるため`IProductRepository`します。 優れたアプローチでは、次を参照してください。 [Web API の依存関係競合回避モジュールを使用して](../advanced/dependency-injection.md)します。


## <a name="getting-a-resource"></a>リソースの取得

ProductStore API をいくつか公開&quot;読み取り&quot;の HTTP GET メソッドの操作。 各アクションのメソッドに対応させるには、`ProductsController`クラス。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| すべての製品の一覧を取得します。 | GET | /api/products |
| ID によって製品を取得します。 | GET | /api/products/*id* |
| カテゴリによって製品を取得します。 | GET | /api/products?category=*category* |

すべての製品の一覧を取得するには、このメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

メソッドの名前で始まる&quot;取得&quot;慣例 GET 要求にマップされるよう、します。 また、メソッドがパラメーターを持たないため、それを URI にマップが含まれていない、 *&quot;id&quot;* パス セグメント。

ID によって製品を取得するには、このメソッドを追加、`ProductsController`クラス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

このメソッドの名前からも始まります&quot;取得&quot;、という名前のパラメーターがメソッド*id*します。このパラメーターにマップされます、 &quot;id&quot; URI パスのセグメント。 ASP.NET Web API フレームワークは、ID を適切なデータ型に自動的に変換 (**int**) パラメーター。

GetProduct メソッド型の例外をスロー **HttpResponseException**場合*id*が無効です。 この例外は、フレームワークによって 404 (Not Found) エラーに変換されます。

最後に、カテゴリによって製品を検索するメソッドを追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

要求 URI にクエリ文字列がある場合は、Web API は、コント ローラー メソッドのパラメーターにクエリ パラメーターに一致を試みます。 フォームの URI ではそのため、"api/製品ですか? カテゴリ =*カテゴリ*"このメソッドにマップされます。

## <a name="creating-a-resource"></a>リソースを作成します。

次に、メソッドを追加します、`ProductsController`新しい製品を作成するクラス。 メソッドの単純な実装を次に示します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

この方法に関する 2 つのことに注意してください。

- メソッドの名前で始まる&quot;投稿しています.&quot;. 新しい製品を作成するには、クライアントは、HTTP POST 要求を送信します。
- メソッドは、製品の種類のパラメーターを受け取ります。 Web api では、複合型を持つパラメーターでは、要求本文から逆シリアル化します。 そのため、クライアントが XML または JSON 形式で、product オブジェクトのシリアル化された表現を送信する予定です。

この実装は機能しますが、完全なものはありません。 理想的には、次のように HTTP 応答を今回は。

- **応答コード:** 既定では、Web API フレームワークは 200 (OK) 応答ステータス コードを設定します。 ただし、http/1.1 プロトコルに従って POST 要求の結果、リソースの作成時に、サーバーという応答がステータス 201 (Created)。
- **場所:** 応答の Location ヘッダーで、新しいリソースの URI を含める必要があります、サーバーは、リソースを作成したときにします。

ASP.NET Web API を簡単に HTTP 応答メッセージを操作します。 強化された実装を次に示します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

メソッドの戻り値の型は今すぐ**HttpResponseMessage**します。 返すことによって、 **HttpResponseMessage**製品ではなく、状態コードと Location ヘッダーを含む、HTTP 応答メッセージの詳細を制御することができます。

**CreateResponse**メソッドを作成、 **HttpResponseMessage**本文に自動的に Product オブジェクトのシリアル化された表現を書き込みます fo 応答メッセージ。

> [!NOTE]
> この例では検証されません、`Product`します。 モデルの検証については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)します。


## <a name="updating-a-resource"></a>リソースの更新

Put の製品を更新することは簡単です。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

メソッドの名前で始まる&quot;を配置しています.&quot;ので、Web API の PUT 要求に一致します。 メソッドは、2 つのパラメーター、製品 ID、および製品の更新を受け取ります。 *Id*パラメーターは、URI のパスから取得し、*製品*パラメーターが要求本文から逆シリアル化します。 既定では、ASP.NET Web API フレームワークは、ルートから単純なパラメーターの型と、要求本文から複合型を使用します。

## <a name="deleting-a-resource"></a>リソースを削除します。

Resourse を削除するには、[削除] メソッドを定義します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

ステータスを記述するエンティティ本文を含むステータス 200 (OK) を返すことができます、DELETE 要求が成功すると、ステータス 202 (Accepted) 場合は、削除が保留中です。または 204 (No Content) エンティティ本文なしでのステータス。 ここで、`DeleteProduct`メソッドには、`void`型を返すため、ASP.NET Web API に自動的に変換このステータス コード 204 (No Content)。
