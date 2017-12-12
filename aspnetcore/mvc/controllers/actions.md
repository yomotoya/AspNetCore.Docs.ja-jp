---
title: "ASP.NET Core MVC のコント ローラーと要求の処理"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 5dc6c7dc70027bb79875f389d535119a2543b873
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>ASP.NET Core MVC のコント ローラーと要求の処理

によって[Steve Smith](https://ardalis.com/)と[Scott Addie](https://github.com/scottaddie)

コント ローラー、アクション、およびアクションの結果は、開発者が ASP.NET Core MVC を使用してアプリを構築する方法の基本的な部分です。

## <a name="what-is-a-controller"></a>コント ローラーとは何ですか。

コント ローラーは、定義およびアクションのセットをグループ化に使用されます。 アクション (または*アクション メソッド*) 要求を処理するコント ローラーのメソッドです。 コント ローラーを論理的にまとめると同様のアクション。 アクションのこの集計は、ルーティング、キャッシュ、および承認、総称して適用するなど、ルールの共通セットを許可します。 要求がによってアクションにマップされる[ルーティング](xref:mvc/controllers/routing)です。

慣例により、コント ローラー クラス。
* プロジェクトのルート レベルに存在する*コント ローラー*フォルダー
* 継承します。`Microsoft.AspNetCore.Mvc.Controller`

コント ローラーは、true は、次の条件の少なくとも 1 つにインスタンス化可能なクラスを示します。
* "Controller"でクラス名が付加されたもの
* 名前を持つに"Controller"サフィックスがクラスからクラスを継承します。
* クラスを装飾、`[Controller]`属性

コント ローラー クラスが関連付けられていない必要があります`[NonController]`属性。

コント ローラーが従う必要があります、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)です。 この原則を実装するのにはいくつかの方法はあります。 複数のコント ローラーのアクションは、同じサービスを必要とする場合は、使用を検討して[コンス トラクター インジェクション](xref:mvc/controllers/dependency-injection#constructor-injection)をこれらの依存関係を要求します。 サービスは、1 つのアクション メソッドのみが必要な場合は、使用を検討して[アクション インジェクション](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)を依存関係を要求します。

内で、 **M**odel -**V**ビュー -**C**ontroller パターンでは、コント ローラーは、要求の初期の処理と、モデルのインスタンス化を行います。 一般に、ビジネスの意思決定は、モデル内で実行してください。

コント ローラーは、モデルの処理 (存在する場合) の結果を取得し、適切なビューとその関連するビューのデータまたは API 呼び出しの結果を返します。 詳しくは[ASP.NET Core MVC の概要](xref:mvc/overview)と[ASP.NET Core MVC および Visual Studio の使用を開始する](xref:tutorials/first-mvc-app/start-mvc)です。

コント ローラーが、 *UI レベル*抽象化します。 その責任は、要求データが有効であることを確認するビュー (または API の結果) を返す必要があるを選択します。 十分に考慮されたアプリでは直接含まれませんデータ アクセスやビジネス ロジックです。 代わりに、コント ローラーは、これらの役割を処理するサービスに委任されます。

## <a name="defining-actions"></a>アクションの定義

コント ローラーで、パブリック メソッドを除くで修飾された、`[NonAction]`属性、操作を示します。 アクションのパラメーターは、データを要求にバインドされを使用して検証されます[モデル バインディング](xref:mvc/models/model-binding)です。 モデル バインドされているものすべてのモデルの検証が行われます。 `ModelState.IsValid`プロパティの値は、モデル バインディングと検証が成功したかどうかを示します。

アクション メソッドには、ビジネス問題を要求をマップするためのロジックを含める必要があります。 ビジネスに関する懸念がを介してコント ローラーにアクセスするサービスとして表される通常必要があります[依存性の注入](xref:mvc/controllers/dependency-injection)です。 アクションは、ビジネス アクションの結果をアプリケーションの状態にマップします。

アクションは、何も返すことができますのインスタンスを頻繁に返す`IActionResult`(または`Task<IActionResult>`async メソッドの場合) の応答を生成します。 選択するため、アクション メソッドは*応答の種類*です。 アクションの結果*、応答は*します。

### <a name="controller-helper-methods"></a>コント ローラーのヘルパー メソッド

コント ローラーが通常から継承[コント ローラー](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)、これは必須ではありません。 派生する`Controller`ヘルパー メソッドの 3 つのカテゴリへのアクセスを提供します。

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1.空の応答本文で結果として得られるメソッド

いいえ`Content-Type`応答本文を記述するコンテンツがないために、HTTP 応答ヘッダーが含まれています。

このカテゴリ内の 2 つの結果型がある: リダイレクトと HTTP ステータス コード。

* **HTTP 状態コード**

    この型には、HTTP ステータス コードが返されます。 この型のいくつかのヘルパー メソッドは、 `BadRequest`、 `NotFound`、および`Ok`です。 たとえば、`return BadRequest();`実行時に、400 状態コードが生成されます。 ときになどのメソッド`BadRequest`、 `NotFound`、および`Ok`は、オーバー ロードされた、適合しなく HTTP ステータス コード応答者としてコンテンツ ネゴシエーションが行われているためです。

* **リダイレクト**

    この型には、アクションまたは変換先へリダイレクトが返されます (を使用して`Redirect`、 `LocalRedirect`、 `RedirectToAction`、または`RedirectToRoute`)。 たとえば、`return RedirectToAction("Complete", new {id = 123});`にリダイレクト`Complete`、匿名オブジェクトを渡します。

    リダイレクトの結果の型は、主に加えての HTTP ステータス コード型とは異なる、 `Location` HTTP 応答ヘッダー。

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2.定義済みコンテンツの種類を空の応答本文で結果として得られるメソッド

このカテゴリのほとんどのヘルパー メソッドが含まれて、`ContentType`プロパティを設定することができます、`Content-Type`応答ヘッダーを応答本文を記述します。

このカテゴリ内の 2 つの結果型がある:[ビュー](xref:mvc/views/overview)と[応答の書式設定](xref:mvc/models/formatting)です。

* **表示**

    この型は、HTML を表示するために、モデルを使用するビューを返します。 たとえば、`return View(customer);`データ バインディングのビューにモデルを渡します。

* **形式の応答**

    この型は、特定の方法でオブジェクトを表すためには、JSON または類似のデータ交換形式を返します。 たとえば、 `return Json(customer);` JSON 形式を指定されたオブジェクトをシリアル化します。
    
    この型の他の一般的な方法`File`、 `PhysicalFile`、および`VirtualFile`です。 たとえば、`return PhysicalFile(customerFilePath, "text/xml");`によって記述された XML ファイルを返す、 `Content-Type` "text/xml"応答ヘッダーの値。

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3.空の応答本文のメソッドは、クライアントとのネゴシエーション コンテンツの種類で書式設定

このカテゴリより適切と呼ばれる**コンテンツ ネゴシエーション**です。 [コンテンツ ネゴシエーション](xref:mvc/models/formatting#content-negotiation)、操作が返された場合に適用されます、 [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult)型または以外の何か、 [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult)実装します。 以外を返すアクション`IActionResult`実装 (たとえば、 `object`) も、形式の応答を返します。

この型の一部のヘルパー メソッドを含める`BadRequest`、 `CreatedAtRoute`、および`Ok`です。 これらのメソッドの例として、 `return BadRequest(modelState);`、 `return CreatedAtRoute("routename", values, newobject);`、および`return Ok(value);`、それぞれします。 なお`BadRequest`と`Ok`値を渡すこと、なく代わりとして使用され、結果の HTTP ステータス コードの種類以外の値を渡す場合にのみ、コンテンツ ネゴシエーションを実行します。 `CreatedAtRoute`メソッド、その一方で、常にコンテンツ ネゴシエーションを実行値が渡されることが必要なすべてのオーバー ロード以降。

### <a name="cross-cutting-concerns"></a>横断的関心事

アプリケーションは、通常、ワークフローの部分を共有します。 例には、ショッピング カートにアクセスする認証を必要とするアプリまたはいくつかのページ上のデータをキャッシュするアプリが含まれます。 ロジックを実行するアクション メソッドの前後に、使用、*フィルター*です。 使用して[フィルター](xref:mvc/controllers/filters)横断的関心事に従うことの重複を減らすことができます、[しない繰り返します自分で (ドライ) 原則](http://deviq.com/don-t-repeat-yourself/)です。

最もなどの属性をフィルター処理`[Authorize]`、目的のレベルの粒度に応じてコント ローラーまたはアクションのレベルで適用されることができます。

エラー処理と応答のキャッシュは多くの場合、横断的関心事です。
   * [エラー処理](xref:mvc/controllers/filters#exception-filters)
   * [応答キャッシュ](xref:performance/caching/response)

フィルターまたはカスタムを使用して処理できる多くの横断的関心事[ミドルウェア](xref:fundamentals/middleware)です。
