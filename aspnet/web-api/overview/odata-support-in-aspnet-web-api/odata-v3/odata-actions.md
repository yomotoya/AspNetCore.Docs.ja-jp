---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET web API 2 OData アクションをサポートする |Microsoft ドキュメント
author: MikeWasson
description: 'OData では、アクションは、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 などのアクションを使用する一部: を実装しています.'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508391"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>ASP.NET web API 2 OData アクションをサポートします。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> OData では、*アクション*エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法は、します。 いくつかの操作の使用は、次のとおりです。
> 
> - 複雑なトランザクションを実装します。
> - 一度に複数のエンティティを操作します。
> - エンティティの特定のプロパティにのみ更新を許可します。
> - エンティティで定義されていないサーバーに情報を送信します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>例: 製品の評価

この例では、製品を評価し、各製品の平均評価を公開できるようにすることができます。 データベースでは、評価と適合する製品の一覧を格納します。

Entity Framework の評価を表す使用モデルを次に示します。

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

クライアントが POST したくありませんが、 `ProductRating` 「評価」コレクションにオブジェクト。 直感的に、評価は製品コレクションに関連付けられ、クライアントを規制の値をポストするだけです。

そのため、通常の CRUD 操作を使用する代わりに製品をクライアントが呼び出すことのできるアクションを定義します。 OData の用語で、アクションは*バインド*Product エンティティにします。

>操作には、サーバー上に副作用があります。 このため、HTTP POST 要求を使用してに呼び出されます。 操作は、パラメーターと戻り値の型が、サービス メタデータで説明されていることができます。 クライアントが要求本文内のパラメーターを送信し、サーバーが応答本文に、戻り値を送信します。 "レート Product"アクションを呼び出すには、クライアントで次のような URI に POST を送信します。

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST 要求のデータだけで、製品評価しています。

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>エンティティ データ モデルのアクションを宣言します。

構成では、Web API には、entity data model (EDM) にアクションを追加します。

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

このコードは、製品のエンティティで実行できるアクションとして"RateProduct"を定義します。 実行する操作も宣言、 **int**パラメーター「評価」をという名前を返します、 **int**値。

## <a name="add-the-action-to-the-controller"></a>コント ローラーにアクションを追加します。

"RateProduct"アクションは、製品のエンティティにバインドされます。 アクションを実装するには、という名前のメソッドを追加`RateProduct`Products コント ローラーにします。

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

メソッド名が、EDM のアクションの名前と一致することに注意してください。 メソッドには、2 つのパラメーターがあります。

- *キー*: 速度に製品のキー。
- *パラメーター*: アクション パラメーターの値のディクショナリ。

既定のルーティング規則を使用している場合キー パラメーターを「キー」という必要があります。 含める必要も、 **[FromOdataUri]** 属性が示すようにします。 この属性では、Web API 要求 URI のキーを解析する場合は、OData 構文の規則を使用するように指示します。

使用して、*パラメーター*アクション パラメーターを取得するためのディクショナリ。

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

クライアントが適切で、アクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は true です。 その場合は、使用することができます、 **ODataActionParameters**パラメーター値を取得するためのディクショナリ。 この例では、`RateProduct`アクションが「評価」をという名前の単一パラメーターを受け取ります。

## <a name="action-metadata"></a>アクションのメタデータ

サービス メタデータを表示するには、/odata/$ メタデータに GET 要求を送信します。 ここでは、メタデータを宣言するの部分であり、`RateProduct`アクション。

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport**要素は、アクションを宣言します。 ほとんどのフィールドが自明ですが 2 つの注意。

- **IsBindable**アクションをターゲット エンティティには、少なくともから呼び出されることができますを意味する時間の一部です。
- **IsAlwaysBindable**ターゲット エンティティに対して常にアクションを呼び出せることを意味します。

違いは、いくつかのアクションは、常に、クライアントが使用できるが、その他のアクションがエンティティの状態によって異なります。 たとえば、「購買」アクションを定義するとします。 在庫にある項目のみ購入できます。 在庫切れ、アイテムがある場合、クライアントはそのアクションを呼び出すことはできません。

EDM を定義するとき、**アクション**メソッドは常にバインド可能なアクションを作成します。

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Not を常に、バインド可能な操作について説明します (とも呼ばれる*一時的な*アクション) このトピックで後述します。

## <a name="invoking-the-action"></a>アクションを呼び出す

これで、クライアントはこの操作を呼び出す方法を確認してみましょう。 クライアントがプロダクトの product ID への 2 の年齢区分を付与すると 4 を = です。 要求本文の JSON 形式を使用して、例要求メッセージを次に示します。

[!code-console[Main](odata-actions/samples/sample9.cmd)]

応答メッセージを次に示します。

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>エンティティ セットへのアクションのバインド

前の例では、アクションが単一のエンティティにバインドされて: クライアントが 1 つの製品を評価します。 アクションは、エンティティのコレクションにバインドすることもできます。 次の変更を加えるだけ。

EDM では、アクションを追加するエンティティの**コレクション**プロパティです。

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

コント ローラーのメソッドで省略、*キー*パラメーター。

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

クライアントは、Products エンティティ セットに対するアクションを呼び出すようになりました。

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>コレクションのパラメーターを使って操作

アクション パラメーターを値のコレクションを受け取ることができます。 EDM では、次のように使用します。 **CollectionParameter&lt;T&gt;** パラメーターを宣言します。

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

コレクションを受け取って「評価」をという名前のパラメーターを宣言してこの**int**値。 メソッドでは、コント ローラー、まだ値を取得するパラメーターから、 **ODataActionParameters**オブジェクトが現在値は、 **ICollection&lt;int&gt;** 値。

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>一時的な操作

例では、"RateProduct"、ユーザーことができます常に製品を評価する、アクションが常に使用できるようにします。 一部の操作は、エンティティの状態によって異なります。 たとえば、ビデオ レンタル サービスで、「チェック アウト」アクションは常に使用できません。 (これによって異なるそのビデオのコピーが使用できるかどうかです。)この種類の操作が呼び出されます、*一時的な*アクション。

一時的なアクションには、サービス メタデータで**IsAlwaysBindable**を false に等しい。 既定値は実際には、メタデータは次のようになりますのでです。

[!code-xml[Main](odata-actions/samples/sample16.xml)]

ここでこれが重要な理由: サーバーがアクションを使用すると、クライアントに指示する必要があるアクションが一時的である場合。 この機能を使用するには、エンティティのアクションへのリンクを含みます。 ムービーのエンティティの例を次に示します。

[!code-console[Main](odata-actions/samples/sample17.cmd)]

"#CheckOut"プロパティには、チェック アウト操作へのリンクが含まれています。 アクションが使用できない場合、サーバーは、リンクを除外します。

EDM の一時的なアクションを宣言するを呼び出して、 **TransientAction**メソッド。

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

また、特定のエンティティのアクション リンクを返す関数を用意する必要があります。 この関数を呼び出すことによって設定**HasActionLink**です。 関数は、ラムダ式として記述できます。

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

アクションがある場合、ラムダ式は、アクションにリンクを返します。 OData シリアライザーには、エンティティをシリアル化時に、このリンクが含まれます。 アクションを使用できない場合、関数を返します`null`です。

## <a name="additional-resources"></a>その他のリソース

[OData アクションのサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
