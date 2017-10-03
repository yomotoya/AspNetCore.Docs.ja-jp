---
title: "ASP.NET Core でのテスト コント ローラー ロジック"
author: ardalis
description: "ASP.NET Core Moq と xUnit でコント ローラーのロジックをテストする方法を説明します。"
keywords: "ASP.NET Core、コント ローラー、テスト、テスト、単体テスト、単体テスト、統合テスト、統合テスト、xUnit"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: dd4135ec-2b15-410c-b3fb-3d12eed4a1ac
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/testing
ms.openlocfilehash: b8ba5740c96b116f9be3feb1967b91c2d675a97d
ms.sourcegitcommit: 5ee9b2ab62acaafe78ad06f1dc4ba624811ab630
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/02/2017
---
# <a name="testing-controller-logic-in-aspnet-core"></a>ASP.NET Core でのテスト コント ローラー ロジック

によって[Steve Smith](https://ardalis.com/)

ASP.NET MVC アプリケーション内のコント ローラーは、ユーザー インターフェイスの問題に焦点を当てており、小規模にする必要があります。 UI 以外の問題に対処する大規模なコント ローラーは、テストや保守が困難です。

[GitHub から表示またはダウンロードのサンプル](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)

## <a name="testing-controllers"></a>テスト コント ローラー

コント ローラーは、すべての ASP.NET Core MVC アプリケーションの中央部分です。 そのため、アプリが意図したとおりに動作する信頼が必要です。 自動テストは、この信頼を提供でき、実稼働環境に到達する前に、エラーを検出できます。 ことが重要、コント ローラー内で不要な責任を配置することを回避し、テストのみに集中コント ローラーの機能を確認してください。

コント ローラー ロジックは最小限にする必要があり、ビジネス ロジックまたはインフラストラクチャの問題 (たとえば、データ アクセス) に注目せずします。 フレームワークではない、コント ローラー ロジックをテストします。 テスト方法、コント ローラー*動作*有効または無効な入力に基づきます。 実行するビジネス操作の結果に基づくコント ローラーの応答をテストします。

一般的なコント ローラーの機能:

* 確認`ModelState.IsValid`です。
* 場合、エラー応答を返す`ModelState`が無効です。
* ビジネス エンティティを永続化ストアから取得します。
* ビジネス エンティティに対して操作を実行します。
* 永続化されたビジネス エンティティを保存します。
* 適切な返す`IActionResult`です。

## <a name="unit-testing"></a>単体テスト

[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)インフラストラクチャとの依存関係から分離でのアプリの一部のテストが含まれます。 単体テスト コント ローラー ロジック、1 つのアクションの内容のみをテストするときに、その依存関係や framework 自体の動作できません。 ユニットとしてテスト コント ローラーのアクションをその動作にだけ専念するかどうかを確認します。 単体テストをコント ローラーなどの回避[フィルター](filters.md)、[ルーティング](../../fundamentals/routing.md)、または[モデル バインディング](../models/model-binding.md)です。 1 つだけをテストに焦点を当てた、単体テストは一般に書き込む単純なとを実行するクイックです。 多くのオーバーヘッドなし、適切に記述された一連の単体テストを頻繁に実行できます。 ただし、単体テストでは問題を検出されないコンポーネント間の対話での目的は[統合テスト](xref:mvc/controllers/testing#integration-testing)です。

単体テストする必要がありますカスタム フィルターやルートなどを記述している場合、特定のコント ローラー アクションで、テストの一部ではなく、します。 これらは、分離環境でテストしてください。

> [!TIP]
> [作成し、Visual Studio での単体テストの実行](https://docs.microsoft.com/visualstudio/test/unit-test-your-code)です。

単体テストを示すためには、次のコント ローラーを確認します。 ブレーンストーミング セッションの一覧を表示し、新しいブレーンストーミングを投稿して作成されるセッションを許可します。

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

コント ローラーは、次の[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)のインスタンスに提供する依存関係の挿入を指定してください`IBrainstormSessionRepository`です。 これにより、非常に簡単にテストと同様に、モック オブジェクト フレームワークを使用して[Moq](https://www.nuget.org/packages/Moq/)です。 `HTTP GET Index`メソッドにはないループおよび分岐のみ呼び出し 1 つのメソッドです。 これをテストする`Index`メソッド、ことを確認する必要があります、`ViewResult`返されると、`ViewModel`リポジトリから`List`メソッドです。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` (上記) 方法を確認する必要があります。

* アクション メソッドは、無効な要求を返します`ViewResult`に適切なデータと`ModelState.IsValid`は`false`

* `Add`リポジトリでメソッドが呼び出されると`RedirectToActionResult`が適切な引数と共に返されるときに`ModelState.IsValid`は true です。

使用してエラーを追加することで無効なモデルの状態をテストできる`AddModelError`最初のテストを以下に示すようにします。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

ときに最初のテストのことを確認`ModelState`が有効でない同じ`ViewResult`用として返される、`GET`要求します。 テストが無効なモデルに渡すしようとしないに注意してください。 はうまくままモデル バインディングが実行されていないので (ただし、[統合テスト](xref:mvc/controllers/testing#integration-testing)演習モデル バインディングを使用)。 この場合、モデル バインディングいないテスト中です。 これらの単体テストでは、アクション メソッドのコードが何をテストするだけです。

2 番目のテストの検証時に`ModelState`が有効で、新しい`BrainstormSession`(リポジトリ) を使用して追加が返されます、`RedirectToActionResult`予期されたプロパティを持つ。 呼び出されないモックの呼び出しは、通常は無視されますが、呼び出し`Verifiable`セットアップの最後の呼び出しで許可されているテストで検証します。 これは、呼び出したときに`mockRepo.Verify`、予想されるメソッドが呼び出されなかった場合、テストが失敗します。

> [!NOTE]
> このサンプルで使用される Moq ライブラリしやすい検証不可能のモック (「厳密でない」モックまたはスタブとも呼ばれます) を使用して、検証可能なまたは"strict"、モックを混在させます。 詳細については[Moq とモック動作をカスタマイズする](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)です。

アプリで別のコント ローラーには、特定のブレーンストーミング セッションに関連する情報が表示されます。 無効な id 値を扱うためのロジックが含まれています。

[!code-csharp[Main](./testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

コント ローラーのアクションが 3 つのケースをテストする場合に、1 つずつ`return`ステートメント。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

アプリでは、web API (ブレーンストーミング セッションと新しいアイデアをセッションに追加するためのメソッドに関連付けられているアイデアの一覧) と機能を公開します。

<a name=ideas-controller></a>

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession`メソッドの一覧を返します`IdeaDTO`型です。 API 呼び出し経由で直接、ビジネス ドメイン エンティティが返されないように、頻繁に以降 API クライアントを必要として、アプリの内部ドメイン モデル外部に公開する API で不必要に結合するより多くのデータが含まれます。 ドメイン エンティティと、ネットワーク経由で戻ります型間のマッピングを手動で行うことができます (LINQ を使用して`Select`次のように) のようなライブラリを使用してまたは[AutoMapper](https://github.com/AutoMapper/AutoMapper)

用の単体テスト、`Create`と`ForSession`API メソッド。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

既に説明したよう、メソッドの動作をテストするときに`ModelState`が有効でない、テストの一部としてコント ローラーにモデル エラーを追加します。 モデルの検証またはモデルのバインディングを単体テストでテスト - 特定が増したときのアクション メソッドの動作をテストするということは避けます`ModelState`値。

2 番目のテストは、null を返します、モック リポジトリが構成されているため、null を返して、リポジトリに依存します。 テスト データベースを作成する必要はありません (メモリ内またはそれ以外の場合) をこの結果を返すクエリを構築および - ようにには、単一のステートメントで実行することができます。

前回のテストであることを確認、リポジトリの`Update`メソッドが呼び出されます。 モックがで呼び出された以前と同様、`Verifiable`とし、モック リポジトリの`Verify`メソッドが呼び出され、検証可能なメソッドが実行されたことを確認します。 いることを確認する単体テストの責任ではありません、`Update`メソッドは、データを保存; 統合テストを実行できます。

## <a name="integration-testing"></a>統合テスト

[統合テスト](../../testing/integration-testing.md)は、アプリ内の個別のモジュールを正常に確実に行われます。 一般に、単体テストをテストすることが何も、テストすることも、統合テストでが逆で true はありません。 ただし、統合テストは単体テストよりも非常に低くなる傾向があります。 したがって、何でも単体テストの場合は、して統合テストを使用して、複数の共同作業者に関連するシナリオをテストすることをお勧めします。

便利可能性があります、モック オブジェクトがほとんど統合テストで使用します。 単体テストでのモック オブジェクトは、テストの目的でテストされている単体の外部での共同作業者の動作を制御する効果的な方法です。 統合テストで実際のコラボレーターを使用して、全体のサブシステムが正しくが連携することを確認します。

### <a name="application-state"></a>アプリケーションの状態

1 つの重要な考慮事項統合テストを実行するときは、アプリの状態を設定する方法を示します。 テストが、互いに独立して実行する必要があり、各テストが既知の状態でアプリを開始する必要がありますのでします。 アプリがデータベースを使用するすべての永続化、または問題にこの可能性があります。 ただし、ほとんどのアプリで現実世界では、1 つのテストによって行われた変更は、データ ストアをリセットしない限り、別のテストに影響がようににいくつかの種類のデータ ストアにその状態が保持されます。 組み込みを使用して`TestServer`は、統合テスト内で ASP.NET Core アプリケーションをホストする非常に簡単ですが、使用するデータへのアクセスを許可しないとは限りません。 1 つの方法は、テスト データベースに接続するアプリがあるを実際のデータベースを使用している場合は、各テストの実行前に、既知の状態にリセットしてテストできますにアクセスすることを確認します。

このサンプル アプリケーションで使用している Entity Framework Core InMemoryDatabase サポート、だけテスト プロジェクトからそれに接続できないようにします。 公開は、代わりに、`InitializeDatabase`メソッドから、アプリの`Startup`クラスは、アプリの起動時にある場合にお電話させて、`Development`環境。 統合テストに自動的にパフォーマンスが向上環境を設定する限り`Development`です。 InMemoryDatabase はアプリを再起動するたびにリセットされた後、データベースのリセットについて心配する必要はありません。

`Startup`クラス。

[!code-csharp[Main](testing/sample/TestingControllersSample/src/TestingControllersSample/Startup.cs?highlight=19,20,34,35,43,52)]

表示されます、`GetTestSession`以下の統合テストで頻繁に使用されるメソッド。

### <a name="accessing-views"></a>ビューへのアクセス

各統合テスト クラスの構成、 `TestServer` ASP.NET Core アプリケーションを実行します。 既定では、`TestServer`が実行されている - この場合、テスト プロジェクト フォルダーのフォルダーに web アプリケーションをホストします。 したがって、しようとするテスト コント ローラーのアクションを返す`ViewResult`、このエラーが発生する可能性があります。

```
The view 'Index' was not found. The following locations were searched:
(list of locations)
```

この問題を解決するには、テスト対象プロジェクトのビューを見つけることができるように、サーバーのコンテンツのルートを構成する必要があります。 これへの呼び出しで`UseContentRoot`で、`TestFixture`次に示すクラス。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/TestFixture.cs?highlight=30,33)]

`TestFixture`クラスは、構成および作成を担当する、`TestServer`を設定、`HttpClient`通信するために、`TestServer`です。 使用して、統合の各テスト、`Client`プロパティをテスト サーバーに接続し、要求を行います。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/HomeControllerTests.cs?highlight=20,26,29,30,31,35,38,39,40,41,44,47,48)]

上記の最初のテスト、`responseString`ビューで、期待どおりの結果が含まれていることを確認するを検査することができますの実際のレンダリングされる HTML を保持します。

2 番目のテストはフォーム ポストを一意のセッションの名前を構築、アプリにポストし、必要なリダイレクトが返されることを確認します。

### <a name="api-methods"></a>API のメソッド

アプリが web Api では、そのテストを自動化されていることをお勧めは期待どおりに実行することを確認を公開するかどうか。 組み込み`TestServer`web Api をテストするが容易です。 常に確認する必要がある場合は、API のメソッドは、モデル バインディングを使用している、 `ModelState.IsValid`、統合テストは、適切な場所に、モデルの検証が正しく動作していることを確認します。

次のテストの対象のセット、`Create`メソッドで、 [IdeasController](xref:mvc/controllers/testing#ideas-controller)上に示したクラス。

[!code-csharp[Main](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/IntegrationTests/ApiIdeasControllerTests.cs)]

HTML ビューを返すアクションの統合テストとは異なり web API メソッド結果を返す通常できます厳密に型指定のオブジェクトに逆シリアル化されたように上記の最後のテストを示しています。 ここでは、テストが結果を逆シリアル化、`BrainstormSession`インスタンス、およびアイデアがアイデアのコレクションに正しく追加されていることを確認します。

この記事の内容の統合テストの他の例があります[サンプル プロジェクト](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)です。
