---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "ASP.NET Web API 1 での CRUD 操作の有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスでの CRUD 操作をサポートする方法を示します。 ソフトウェアのバージョンがチュートリアルの Visual Studio 2012 Web AP で使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>ASP.NET Web API 1 での CRUD 操作を有効にします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> このチュートリアルでは、ASP.NET Web API を使用して HTTP サービスでの CRUD 操作をサポートする方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Visual Studio 2012
> - Web API 1 (も Web API 2 で機能します)


CRUD&quot;作成、読み取り、更新、および Delete、&quot;は 4 つの基本的なデータベース操作します。 多くの HTTP サービスでは、REST または REST のような Api を介して CRUD 操作もモデルします。

このチュートリアルでは、非常に単純な web 製品の一覧を管理する API を構築します。 各製品は、名前、価格、およびカテゴリが含まれます (など&quot;toys&quot;または&quot;ハードウェア&quot;)、さらに、製品の id。

製品の API は、次のメソッドに公開します。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| すべての製品の一覧を取得します。 | GET | /api/products |
| ID の製品を取得します。 | GET | /api/products/*id* |
| カテゴリによって製品を取得します。 | GET | /api/products?category=*category* |
| 新しい製品を作成します。 | POST | /api/products |
| 製品を更新します。 | PUT | /api/products/*id* |
| 製品を削除します。 | Del | /api/products/*id* |

パスに製品 ID を含む Uri の一部に注意してください。 たとえば、製品 ID が 28 を取得するクライアント GET 要求を送信`http://hostname/api/products/28`です。

### <a name="resources"></a>リソース

製品の API は、2 種類のリソースの Uri を定義します。

| リソース | URI |
| --- | --- |
| すべての製品の一覧。 | /api/products |
| 個々 の製品です。 | /api/products/*id* |

### <a name="methods"></a>メソッド

4 つの主要な HTTP メソッド (GET、PUT、POST、および削除) は、CRUD 操作にマップすることができます。

- GET では、指定された URI にあるリソースの表現を取得します。 サーバーの副作用が GET はありません。
- PUT は、指定された URI にリソースを更新します。 PUT こともできますを指定された URI で新しいリソースを作成する場合は、サーバーにより、クライアントが新しい Uri を指定します。 このチュートリアルでは、API では PUT による作成はサポートされません。
- 投稿では、新しいリソースを作成します。 サーバーは、新しいオブジェクトの URI が割り当てられ、応答メッセージの一部としてこの URI を返します。
- 指定された URI にリソースを削除します。

注: PUT メソッドでは、全体の product エンティティを置き換えます。 つまり、クライアントは、更新された製品の完全な表現を送信すると想定されます。 部分的な更新をサポートする場合は、PATCH メソッドをお勧めします。 このチュートリアルでは、修正プログラムが実装されていません。

## <a name="create-a-new-web-api-project"></a>新しい Web API プロジェクトを作成します。

Visual Studio を実行して起動し、選択**新しいプロジェクト**から、**開始**ページ。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトに名前を&quot;ProductStore&quot;  をクリック**OK**です。

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

**新しい ASP.NET MVC 4 プロジェクト**ダイアログで、 **Web API**  をクリック**OK**です。

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>モデルを追加します。

*モデル*は、アプリケーションのデータを表すオブジェクトです。 ASP.NET Web API では、モデルとして厳密に型指定された CLR オブジェクトを使用することができ、自動的にシリアル化する XML または JSON をクライアントのします。

ProductStore API データで構成されている製品、という名前の新しいクラスを作成するように`Product`です。

ソリューション エクスプ ローラーが表示されない場合にクリックして、**ビュー**メニュー**ソリューション エクスプ ローラー**です。 ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダーです。 コンテキスト meny から選択**追加**選択してから、**クラス**です。 クラスの名前を付けます&quot;製品&quot;です。

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

次のプロパティを追加、`Product`クラスです。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>リポジトリの追加

製品のコレクションを格納する必要があります。 サービス実装からコレクションを分割することをお勧めします。 ようにして、バッキング ストア サービス クラスを書き直すことがなく変更できます。 この型のデザインが呼び出される、*リポジトリ*パターン。 リポジトリの汎用インターフェイスを定義することで開始します。

ソリューション エクスプ ローラーで右クリックし、**モデル**フォルダーです。 選択**追加**選択してから、**新しい項目の**します。

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

**テンプレート**ペインで、**インストールされたテンプレート**(C#) ノードを展開します。 C# の場合、で **コード**です。 コード テンプレートの一覧で選択**インターフェイス**です。 インターフェイスの名前&quot;IProductRepository&quot;です。

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

次の実装を追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

という名前のモデル フォルダーを別のクラスを今すぐ追加&quot;ProductRepository です。&quot;このクラスが `IProductRespository` インターフェイスを実装します。 次の実装を追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

リポジトリでは、ローカル メモリの一覧が維持されます。 チュートリアルについては、[ok] をこれが実際のアプリケーションでは、データを格納する外部で、データベースであるか、またはクラウド ストレージにします。 リポジトリ パターンが容易にできるように実装を後で変更します。

## <a name="adding-a-web-api-controller"></a>Web API コント ローラーの追加

ASP.NET MVC を使用する場合、し、既に慣れているコント ローラー。 ASP.NET Web API で、*コント ローラー*クライアントから HTTP 要求を処理するクラスです。 プロジェクトが作成されたときに、新しいプロジェクト ウィザードによって次の 2 つのコント ローラーが作成されます。 これらを参照するには、ソリューション エクスプ ローラーで Controllers フォルダーを展開します。

- テンプレートを使用するとは、従来の ASP.NET MVC コント ローラーです。 サイトの HTML ページの要求を処理して、web API に直接関連しません。
- ValuesController は、例 WebAPI コント ローラーです。

ソリューション エクスプ ローラーでファイルを右クリックして、ValuesController を削除してください**を削除します。** 今すぐよう、新しいコント ローラーを追加します。

**ソリューション エクスプ ローラー**、コント ローラーのフォルダーを右クリックします。 選択**追加**し、**コント ローラー**です。

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

**コント ローラーの追加**ウィザードで、名前、コント ローラー &quot;ProductsController&quot;です。 **テンプレート**ドロップダウン リストで、**空の API コント ローラー**です。 次に、 **[追加]**をクリックします。

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> コント ローラーをという名前のフォルダーに、コントを配置する必要はありません。 フォルダー名が重要です。ソース ファイルを整理する便利な方法では単純にすることをお勧めします。


**コント ローラーの追加**ProductsController.cs Controllers フォルダーをという名前のファイルを作成します。 このファイルがまだ開いていないを開くには、ファイルをダブルクリックします。 次の追加**を使用して**ステートメント。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

格納するフィールドを追加、 **IProductRepository**インスタンス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> 呼び出す`new ProductRepository()`コント ローラーには最善の設計では、コント ローラーの特定の実装を結び付けるため`IProductRepository`です。 適切な方法では、次を参照してください。 [Web API の依存関係競合回避モジュールを使用して](../advanced/dependency-injection.md)です。


## <a name="getting-a-resource"></a>リソースの取得

ProductStore API では、いくつかは公開&quot;読み取り&quot;HTTP GET メソッドとアクション。 各アクション メソッドに対応させるには、`ProductsController`クラスです。

| アクション | HTTP メソッド | 相対 URI |
| --- | --- | --- |
| すべての製品の一覧を取得します。 | GET | /api/products |
| ID の製品を取得します。 | GET | /api/products/*id* |
| カテゴリによって製品を取得します。 | GET | /api/products?category=*category* |

すべての製品の一覧を取得するには、このメソッドは追加、`ProductsController`クラス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

メソッドの名前が始まる&quot;取得&quot;慣例 GET 要求にマップするため、します。 また、メソッドにパラメーターがあるないためにマップを含まない URI、  *&quot;id&quot;* パス内のセグメント。

ID によって製品を入手するには、このメソッドは追加、`ProductsController`クラス。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

このメソッドの名前からも始まります&quot;取得&quot;、メソッドがという名前のパラメーターが、 *id*です。このパラメーターにマップされて、 &quot;id&quot; URI パスのセグメント。 ASP.NET Web API フレームワークが、ID を正しいデータ型に自動的に変換 (**int**) のパラメーターです。

GetProduct メソッド型の例外をスロー **HttpResponseException**場合*id*が無効です。 この例外は、フレームワークによって 404 (Not Found) エラーに変換されます。

最後に、カテゴリによって製品を検索するメソッドを追加します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

要求 URI にクエリ文字列がある場合は、Web API では、コント ローラーのメソッドのパラメーターにクエリ パラメーターを照合します。 そのため、フォームの URI"api/製品ですか? カテゴリ =*カテゴリ*"このメソッドにマップされます。

## <a name="creating-a-resource"></a>リソースを作成します。

次に、追加する方法、`ProductsController`新しい製品を作成するクラス。 メソッドの単純な実装を次に示します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

このメソッドの 2 つの点に注意してください。

- メソッドの名前が始まる&quot;Post しています.&quot;. 新しい製品を作成するには、クライアントは、HTTP POST 要求を送信します。
- メソッドは、製品の種類のパラメーターを受け取ります。 Web API では、複合型を持つパラメーターが要求本文から逆シリアル化します。 したがって、XML または JSON のいずれかの形式で、製品オブジェクトのシリアル化された表現を送信するクライアントを見込んでいます。

この実装は機能しますが、完全なものはありません。 理想的には、HTTP 応答、以下を含むましょう。

- **応答コード:**既定では、Web API フレームワークは 200 (OK) に応答のステータス コードを設定します。 ただし、http/1.1 プロトコルに従って、POST 要求は、リソースの作成時に発生すると、サーバーという応答が 201 (Created) の状態。
- **場所:**応答の Location ヘッダーの新しいリソースの URI を含める必要があります、サーバーは、リソースを作成したときにします。

ASP.NET Web API 簡単を HTTP 応答メッセージを操作できます。 強化された実装を次に示します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

メソッドの戻り値の型は、今すぐ通知**HttpResponseMessage**です。 返すことによって、 **HttpResponseMessage**製品の代わりには、ステータス コードと Location ヘッダーを含む、HTTP 応答メッセージの詳細を制御できます。

**CreateResponse**メソッドを作成、 **HttpResponseMessage**と本文に Product オブジェクトのシリアル化された表現を自動的に書き込み fo 応答メッセージ。

> [!NOTE]
> この例では検証されません、`Product`です。 モデルの検証方法については、次を参照してください。 [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)です。


## <a name="updating-a-resource"></a>リソースの更新

Put の製品を更新することは簡単です。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

メソッドの名前が始まる&quot;を配置しています.&quot;ので、Web API が PUT 要求に一致します。 メソッドは、2 つのパラメーター、製品 ID と製品の更新を受け取ります。 *Id*パラメーターが URI パスから取得され、*製品*パラメーターが要求本文から逆シリアル化します。 既定では、ASP.NET Web API フレームワークは、ルートからの単純なパラメーターの型と、要求本文の複合型はクリックします。

## <a name="deleting-a-resource"></a>リソースを削除します。

Resourse を削除するには、「削除...」メソッドを定義します。

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

ステータスを記述するエンティティ本体を持つステータス 200 (OK) を返すことができます削除要求が成功すると、ステータス 202 (Accepted) 場合は、削除が保留中です。または、204 (No Content) エンティティ本文なしでの状態。 ここで、`DeleteProduct`メソッドには、`void`型を返すには、ため、ASP.NET Web API に自動的に変換このステータス コード 204 (No Content)。
