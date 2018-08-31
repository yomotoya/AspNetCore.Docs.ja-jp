---
title: ASP.NET Core のコントローラーのロジックをテストする
author: ardalis
description: Moq と xUnit を使って ASP.NET Core のコントローラーのロジックをテストする方法を説明します。
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/testing
ms.openlocfilehash: d0b2a25d00187c088671be147844aa892f824c6e
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2018
ms.locfileid: "41751490"
---
# <a name="test-controller-logic-in-aspnet-core"></a>ASP.NET Core のコントローラーのロジックをテストする

作成者: [Steve Smith](https://ardalis.com/)

コントローラーは、すべての ASP.NET Core MVC アプリケーションの中心になるものです。 そのため、アプリが意図するとおりに動作するという信頼が必要です。 自動テストを行うと、この信頼が得られ、運用環境に提供する前にエラーを検出できます。 コントローラーに必要のない機能を持たせないようにし、テストではコントローラーの機能だけに注目することが重要です。

コントローラーのロジックは最小限のものにして、ビジネス ロジックやインフラストラクチャに関すること (たとえば、データ アクセス) には目を向けないようにする必要があります。 フレームワークではなく、コントローラーのロジックをテストします。 有効または無効な入力に基づいて、コントローラーの "*動作*" をテストします。 コントローラーが実行するビジネス操作の結果に基づいて、コントローラーの応答をテストします。

コントローラーの一般的な機能:

* `ModelState.IsValid` を確認します。
* `ModelState` が無効である場合は、エラー応答を返します。
* ビジネス エンティティを永続化から取得します。
* ビジネス エンティティに対してアクションを実行します。
* ビジネス エンティティを永続化に保存します。
* 適切な `IActionResult` を返します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/testing/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="unit-tests-of-controller-logic"></a>コントローラー ロジックの単体テスト

[単体テスト](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)では、アプリの一部をインフラストラクチャや依存関係から切り離してテストすることが必要とされます。 コントローラー ロジックの単体テストを行うときは、それが依存しているものやフレームワーク自体の動作ではなく、単一のアクションの内容のみをテストします。 コントローラーのアクションの単体テストでは、その動作にだけ注目するようにします。 コントローラーの単体テストからは、[フィルター](xref:mvc/controllers/filters)、[ルーティング](xref:fundamentals/routing)、[モデル バインド](xref:mvc/models/model-binding)などに関することは除外します。 1 つの事柄だけに注目してテストすることにより、一般に、単体テストを簡単に作成してすばやく実行できるようになります。 適切に記述された一連の単体テストは、大きなオーバーヘッドなしに頻繁に実行できます。 ただし、単体テストではコンポーネント間の相互作用の問題は検出しません。これは、[統合テスト](xref:test/integration-tests)の目的です。

カスタム フィルターやルートを作成している場合、それらの単体テストは、コントローラーの特定のアクションに対するテストの一部として行うのではなく、分離環境で行う必要があります。

> [!TIP]
> [Visual Studio で単体テストを作成して実行します](/visualstudio/test/unit-test-your-code)。

単体テストの方法を説明するための実例として、次のコントローラーを使います。 このコントローラーは、ブレーンストーミング セッションの一覧を表示し、POST を使って新しいブレーンストーミング セッションを作成できます。

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/HomeController.cs?highlight=12,16,21,42,43)]

このコントローラーは、[明示的な依存関係の原則](http://deviq.com/explicit-dependencies-principle/)に従い、依存関係の挿入を使って `IBrainstormSessionRepository` のインスタンスを提供されるものと想定しています。 これにより、[Moq](https://www.nuget.org/packages/Moq/) などのモック オブジェクト フレームワークを使ったテストが非常に簡単になります。 `HTTP GET Index` メソッドにはループや分岐はなく、1 つのメソッドを呼び出すだけです。 この `Index` メソッドをテストするには、リポジトリの `List` メソッドからの `ViewModel` で、`ViewResult` が返されることを確認する必要があります。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=17-18&range=1-33,76-95)]

`HomeController` `HTTP POST Index` メソッド (上記) では、次のことを確認する必要があります。

* `ModelState.IsValid` が `false` の場合、アクション メソッドが無効な要求の `ViewResult` と適切なデータを返すこと。

* `ModelState.IsValid` が true の場合、リポジトリで `Add` メソッドが呼び出されて、`RedirectToActionResult` が適切な引数と共に返されること

以下の最初のテストで示すように、`AddModelError` を使ってエラーを追加することで、無効なモデルの状態をテストできます。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/HomeControllerTests.cs?highlight=8,15-16,37-39&range=35-75)]

最初のテストでは、`ModelState` が有効ではないときに、`GET` 要求の場合と同じ `ViewResult` が返されることを確認します。 テストでは無効なモデルを渡そうとしていないことに注意してください。 モデル バインドが動作していないので、いずれにしてもうまくいきません (ただし、[統合テスト](xref:test/integration-tests)では演習のモデル バインドを使います)。 この場合、モデル バインドはテストされていません。 これらの単体テストでは、アクション メソッドのコードだけをテストしています。

2 番目のテストでは、`ModelState` が有効であるときに、新しい `BrainstormSession` が (リポジトリによって) 追加され、メソッドが予期されるプロパティと共に `RedirectToActionResult` を返すことを確認します。 呼び出されないモックの呼び出しは通常は無視されますが、Setup の呼び出しの最後で `Verifiable` を呼び出すと、テスト内で検証できます。 これは `mockRepo.Verify` の呼び出しで行われ、予期されるメソッドが呼び出されないとテストは失敗します。

> [!NOTE]
> このサンプルで使われている Moq ライブラリにより、検証可能な ("厳密な") モックと検証不可能なモック ("厳密でない" モックまたはスタブとも呼ばれます) を簡単に混在させることができます。 詳しくは、Moq の「[Customizing Mock Behavior](https://github.com/Moq/moq4/wiki/Quickstart#customizing-mock-behavior)」(モックの動作のカスタマイズ) をご覧ください。

アプリの別のコントローラーでは、特定のブレーンストーミング セッションに関する情報が表示されます。 無効な ID 値を扱うためのロジックが含まれています。

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Controllers/SessionController.cs?highlight=19,20,21,22,25,26,27,28)]

コントローラーのアクションには、各 `return` ステートメントに 1 つずつ、3 つのテスト ケースがあります。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/SessionControllerTests.cs?highlight=27,28,29,46,47,64,65,66,67,68)]

アプリでは、Web API として機能が公開されています (ブレーンストーミング セッションに関連付けられているアイデアの一覧と、新しいアイデアをセッションに追加するためのメソッド)。

[!code-csharp[](testing/sample/TestingControllersSample/src/TestingControllersSample/Api/IdeasController.cs?highlight=21,22,27,30,31,32,33,34,35,36,41,42,46,52,65)]

`ForSession` メソッドは、`IdeaDTO` 型の一覧を返します。 API の呼び出しで直接ビジネス ドメイン エンティティを返さないでください。ビジネス ドメイン エンティティには API クライアントが要求しているもの以外のデータが含まれることが多く、アプリの内部ドメイン モデルと外部に公開される API との間に必要のない結合が生じます。 ドメイン エンティティと、ネットワーク経由で戻される型の間のマッピングは、手動で (ここで示すように LINQ `Select` を使って)、または [AutoMapper](https://github.com/AutoMapper/AutoMapper) などのライブラリを使って、行うことができます。

API メソッド `Create` と `ForSession` の単体テストは次のとおりです。

[!code-csharp[](testing/sample/TestingControllersSample/tests/TestingControllersSample.Tests/UnitTests/ApiIdeasControllerTests.cs?highlight=18,23,29,33,38-39,43,50,58-59,68-70,76-78&range=1-83,121-135)]

前に説明したように、`ModelState` が無効のときのメソッドの動作をテストするには、テストの一部としてコントローラーにモデル エラーを追加します。 単体テストでは、モデルの検証またはモデル バインドのテストを試さないでください。`ModelState` の特定の値に遭遇したときのアクション メソッドの動作だけをテストしてください。

2 番目のテストではリポジトリが null を返す必要があるので、モック リポジトリは null を返すように構成されています。 テスト データベース (メモリ内またはそれ以外の場所) を作成する必要はなく、この結果を返すクエリを作成します。これは、次のように単一のステートメントで実行できます。

最後のテストでは、リポジトリの `Update` メソッドが呼び出されることを確認します。 前に行ったように、`Verifiable` ではモックが呼び出され、モック リポジトリの `Verify` メソッドが呼び出されて、検証可能なメソッドが実行されたことを確認します。 `Update` メソッドがデータを保存したことを確認するのは、単体テストの役割ではありません。これは、統合テストで実行できます。

## <a name="additional-resources"></a>その他の技術情報

* <xref:test/integration-tests>
