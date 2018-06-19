---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: サービス層 (c#) |Microsoft ドキュメント
author: StephenWalther
description: コント ローラーのアクションから、別のサービス層には、検証ロジックを移動する方法を説明します。 Stephen Walther このチュートリアルで説明する方法をしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869037"
---
<a name="validating-with-a-service-layer-c"></a>サービス層 (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> コント ローラーのアクションから、別のサービス層には、検証ロジックを移動する方法を説明します。 このチュートリアルでは、Stephen Walther は、コント ローラーの層から、サービス層を分離することによって、関心のシャープな分離を維持する方法について説明します。


このチュートリアルの目的は、ASP.NET MVC アプリケーションで検証を実行する 1 つの方法を説明します。 このチュートリアルでは、コント ローラーから、別のサービス層には、検証ロジックを移動する方法を学習します。

## <a name="separating-concerns"></a>上の問題を分離します。

ASP.NET MVC アプリケーションをビルドするときに、コント ローラーのアクションの内部データベース ロジックを配置する必要がありますできません。 データベースとコント ローラーのロジックを混在させるとが、アプリケーションの時間の経過と共に維持するために困難になります。 すべてのデータベース ロジックを別のリポジトリのレイヤーに配置することをお勧めします。

たとえば、1 一覧表示するにはには、ProductRepository をという名前の単純なリポジトリが含まれています。 製品のリポジトリには、すべてのアプリケーションのデータ アクセス コードが含まれています。 一覧には、製品のリポジトリを実装する IProductRepository インターフェイスも含まれています。

**1--Models\ProductRepository.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

リスト 2 でコント ローラーは、その Index() および Create() 操作の両方のリポジトリ レイヤーを使用します。 このコント ローラーに、データベース、任意のロジックが含まれていないことに注意してください。 リポジトリのレイヤーを作成するには、上の問題を明確に分離を維持することができます。 コント ローラーは、アプリケーションのフロー制御ロジックとリポジトリはデータ アクセス ロジックを担当します。

**2 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>サービス レイヤーを作成します。

そのため、アプリケーションのフロー制御ロジック、コント ローラーに属しリポジトリにデータ アクセス ロジックが属しています。 その場合は、操作を配置する、検証ロジックしますか。 1 つのオプションは、検証ロジックを配置する、*サービス層*です。

サービス層は、ASP.NET MVC アプリケーション コント ローラーおよびリポジトリ レイヤー間の通信を仲介するレイヤーが追加されます。 サービス層には、ビジネス ロジックが含まれています。 具体的には、検証ロジックが含まれています。

たとえば、3 の一覧で製品のサービス層では、CreateProduct() メソッドがあります。 CreateProduct() メソッドは、製品を製品リポジトリに渡す前に、新しい製品を検証する ValidateProduct() メソッドを呼び出します。

**3 - Models\ProductService.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

リポジトリ レイヤーではなく、サービス レイヤーを使用するコード サンプル 4 の製品のコント ローラーが更新されました。 コント ローラー レイヤーは、サービス層を説明します。 サービス レイヤーは、リポジトリのレイヤーに説明します。 各レイヤーは、個別の責任を持ちます。

**4 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

製品のサービスが、product コント ローラーのコンス トラクターで作成されたことに注意してください。 製品のサービスが作成されると、モデル状態ディクショナリは、サービスに渡されます。 製品のサービスでは、モデルの状態を使用して、コント ローラーに検証エラー メッセージを渡します。

## <a name="decoupling-the-service-layer"></a>サービス層を分離

私たちは、コント ローラーと 1 つの点では、サービス層に分離に失敗しました。 コント ローラーとサービス層は、モデルの状態を通信します。 つまり、サービス層、ASP.NET MVC フレームワークの特定の機能に依存しています。

可能な限り、コント ローラー層から、サービス レイヤーを分離することができます。 理論上は、どのタイプのアプリケーションと ASP.NET MVC アプリケーションだけでなく、サービス レイヤーを使用することもあります。 たとえば、今後、可能性があること、WPF アプリケーションのフロント エンドをビルドできます。 サービス層からお語る ASP.NET MVC 依存関係を削除する方法モデルの状態を検索する必要があります。

リストの 5 では、サービス層が更新されましたモデルの状態が使用されないようにします。 代わりに、IValidationDictionary インターフェイスを実装する任意のクラスを使用します。

**5 - Models\ProductService.cs (分離) を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

IValidationDictionary インターフェイスは、一覧表示する 6 で定義されます。 この単純なインターフェイスは、1 つのメソッドとプロパティを 1 つがします。

**6 - Models\IValidationDictionary.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

名前付き ModelStateWrapper クラスを一覧表示する 7 クラスは、IValidationDictionary インターフェイスを実装します。 モデル状態ディクショナリをコンス トラクターに渡すことによって、ModelStateWrapper クラスをインスタンス化することができます。

**7 - Models\ModelStateWrapper.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

最後に、リスト 8 に更新されたコント ローラーは、そのコンス トラクターで、サービス レイヤーを作成するときに、ModelStateWrapper を使用します。

**8 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

IValidationDictionary を使用してインターフェイスを ModelStateWrapper クラスことにより、コント ローラーの層から、サービス層を完全に分離します。 サービス層がモデル状態に依存するではなくなりました。 サービス層に IValidationDictionary インターフェイスを実装する任意のクラスを渡すことができます。 たとえば、WPF アプリケーションは、単純なコレクション クラスを使用 IValidationDictionary インターフェイスを実装する可能性があります。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC アプリケーションで検証を実行するための 1 つの方法を説明することでした。 このチュートリアルでは、すべてのコント ローラーから、別のサービス層に、検証ロジックを移動する方法について学習しました。 ModelStateWrapper クラスを作成することで、コント ローラーの層から、サービス層を分離する方法も学習しました。

> [!div class="step-by-step"]
> [前へ](validating-with-the-idataerrorinfo-interface-cs.md)
> [次へ](validation-with-the-data-annotation-validators-cs.md)
