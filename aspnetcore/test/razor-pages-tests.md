---
title: ASP.NET Core で razor ページの単体テスト
author: guardrex
description: Razor ページのアプリの単体テストを作成する方法を説明します。
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2017
uid: test/razor-pages-tests
ms.openlocfilehash: bde1bef78fcc7ac1d570057d54636ea0f5490de8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274408"
---
# <a name="razor-pages-unit-tests-in-aspnet-core"></a>ASP.NET Core で razor ページの単体テスト

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core では、Razor Pages のアプリの単体テストをサポートします。 データ アクセス層 (DAL) のテストとページ モデルによって、次が保証されます。

* Razor ページのアプリの部分を使用とは別に単位としてまとめてアプリの構築中にあります。
* クラスとメソッドには、責任の範囲が制限されます。
* その他のドキュメントは、アプリの動作に存在します。
* コードのアップデートによってもたらされたエラーである回帰は、自動ビルドと配置中に検出されました。

このトピックでは、Razor ページのアプリと単体テストの基本的な知識があることを前提としています。 Razor ページのアプリやテストの概念に習熟していない場合は、次のトピックを参照してください。

* [Razor ページを始める](xref:razor-pages/index)
* [Razor ページの概要](xref:tutorials/razor-pages/razor-pages-start)
* [単体テスト c# dotnet テスト、xUnit を使用して .NET Core](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/test/razor-pages-tests/samples)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

サンプル プロジェクトは、2 個のアプリで構成されます。

| アプリ         | プロジェクト フォルダー                        | 説明 |
| ----------- | ------------------------------------- | ----------- |
| メッセージ アプリ | *src/RazorPagesTestSample*            | ユーザーを追加、1 つを削除、削除、およびメッセージを分析できます。 |
| アプリのテスト    | *tests/RazorPagesTestSample.Tests*    | 単体テスト メッセージ アプリを使用します。 データ アクセス層 (DAL) およびインデックス ページのモデル。 |

などの IDE の組み込みのテスト機能を使用してテストを実行できる[Visual Studio](https://www.visualstudio.com/vs/)です。 使用して場合[Visual Studio Code](https://code.visualstudio.com/)またはコマンド プロンプトで次のコマンドを実行、コマンドライン、 *tests/RazorPagesTestSample.Tests*フォルダー。

```console
dotnet test
```

## <a name="message-app-organization"></a>メッセージ アプリ組織

メッセージ アプリでは、次の特性を持つ単純な Razor ページ メッセージ システムを示します。

* アプリのインデックス ページ (*Pages/Index.cshtml*と*Pages/Index.cshtml.cs*) UI とページを追加、削除、およびメッセージ (メッセージあたりの平均ワード) の分析を制御するモデルのメソッドを提供.
* メッセージは、`Message`クラス (*Data/Message.cs*) 2 つのプロパティを持つ: `Id` (キー) と`Text`(メッセージ)。 `Text`プロパティは必須であり 200 文字までに制限されます。
* 使用してメッセージが格納される[Entity Framework のメモリ内のデータベース](/ef/core/providers/in-memory/)&#8224;。
* アプリには、データベース コンテキスト クラスのデータ アクセス層 (DAL) が含まれています`AppDbContext`(*Data/AppDbContext.cs*)。 DAL メソッドをマーク`virtual`、これにより、テストで使用するためのメソッドをモックします。
* データベースがアプリの起動時に空の場合、メッセージ ストアは、3 つのメッセージで初期化されます。 これら*メッセージをシード*テストにも使用します。

&#8224;EF トピック[InMemory を伴うテスト](/ef/core/miscellaneous/testing/in-memory)MSTest でテストにメモリ内のデータベースを使用する方法について説明します。 このトピックでは、 [xUnit](https://xunit.github.io/)テスト フレームワーク。 テストの概念と別のテスト フレームワークの間でのテストの実装は、似ているが同一でです。

アプリを使用していないが、[リポジトリ パターン](http://martinfowler.com/eaaCatalog/repository.html)の有効な例が表示されない、[作業単位 (UoW) パターン](https://martinfowler.com/eaaCatalog/unitOfWork.html)、Razor ページには、開発のこれらのパターンがサポートされています。 詳細については、次を参照してください[インフラストラクチャの永続性レイヤーをデザイン](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)、 [ASP.NET MVC アプリケーションでリポジトリおよび単位の作業パターンを実装する](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application)、および[テスト コント ローラー。ロジック](/aspnet/core/mvc/controllers/testing)(このサンプルは、リポジトリ パターンを実装)。

## <a name="test-app-organization"></a>組織のアプリをテストします。

テスト アプリケーションは、コンソール アプリケーション内、 *tests/RazorPagesTestSample.Tests*フォルダーです。

| App フォルダーのテスト | 説明 |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs* DAL の単体テストが含まれています。</li><li>*IndexPageTests.cs*インデックス ページのモデルの単体テストが含まれています。</li></ul> |
| *ユーティリティ*     | 含まれています、`TestingDbContextOptions`メソッドは、データベースがその基準条件テストごとにリセットできるように各 DAL 単体テストのコンテキストのオプションで新しいデータベースを作成するために使用します。 |

テスト フレームワークが[xUnit](https://xunit.github.io/)です。 フレームワークのモック オブジェクトが[Moq](https://github.com/moq/moq4)です。

## <a name="unit-tests-of-the-data-access-layer-dal"></a>単体テストでのデータのレイヤー (DAL) のアクセスします。

メッセージ アプリを持つ、DAL に含まれている 4 つのメソッド、`AppDbContext`クラス (*src/RazorPagesTestSample/Data/AppDbContext.cs*)。 各メソッドでは、テスト アプリの 1 つまたは 2 つの単体テストがあります。

| DAL メソッド               | 関数                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | 取得、`List<Message>`順に並べ替えて、データベースから、`Text`プロパティです。 |
| `AddMessageAsync`        | 追加、`Message`データベースにします。                                          |
| `DeleteAllMessagesAsync` | すべて削除`Message`データベースからのエントリ。                           |
| `DeleteMessageAsync`     | 1 つを削除`Message`によってデータベースから`Id`です。                      |

DAL の単体テストを必要と[DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)新たに作成するときに`AppDbContext`テストごとにします。 作成する方法の 1 つ、`DbContextOptions`各テストは、使用するため、 [DbContextOptionsBuilder](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptionsbuilder):

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

このアプローチの問題は、各テストがどのような状態、前のテストのための状態でデータベースを受信します。 これは問題となる互いに干渉しないアトミック単位テストを作成しようとしています。 強制的に、`AppDbContext`するには、新しいデータベース コンテキストを使用して、各テストについて、指定、`DbContextOptions`新しいサービス プロバイダーに基づいているインスタンス。 テスト アプリを使用してこれを行う方法を示しています。 その`Utilities`クラス メソッド`TestingDbContextOptions`(*tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs*)。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

使用して、 `DbContextOptions` DAL 単体テストで、新しいデータベース インスタンスにアトミックに実行するには、各テスト。

```csharp
using (var db = new AppDbContext(Utilities.TestingDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

内の各テスト メソッド、`DataAccessLayerTest`クラス (*UnitTests/DataAccessLayerTest.cs*) のような配置 Act アサート パターンに従います。

1. 配置: データベースが構成されているテストや、予想される結果が定義されています。
1. Act: テストを実行します。
1. アサート: アサーションに対するテスト結果が成功したかどうかを判断します。

たとえば、`DeleteMessageAsync`で識別される 1 つのメッセージを削除するため、メソッドはその`Id`(*src/RazorPagesTestSample/Data/AppDbContext.cs*)。

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

このメソッドの 2 つのテストがあります。 1 つのテストは、メソッドは、メッセージがデータベースに存在する場合、メッセージを削除を確認します。 データベースを変更しないこと、他のメソッド テスト メッセージ`Id`削除が存在しないためです。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`メソッドを次に示します。

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

最初に、メソッドが実行の配置手順 Act 手順の準備が行われる。 シード処理のメッセージを取得し、保持されている`seedMessages`です。 シード処理のメッセージは、データベースに保存されます。 メッセージを`Id`の`1`削除用に設定されています。 ときに、`DeleteMessageAsync`メソッドが実行され、予期されるメッセージには、すべてのメッセージを 1 つを除くが必要な`Id`の`1`します。 `expectedMessages`変数はこの予想される結果を表します。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

メソッドの動作:`DeleteMessageAsync`で渡すメソッドが実行される、`recId`の`1`:

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

メソッドの最後に、取得、`Messages`コンテキストからとを比較、 `expectedMessages` 2 つが等しいことをアサートするとします。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

比較するために、2 つ`List<Message>`は、同じです。

* メッセージが順`Id`です。
* メッセージのペアの比較、`Text`プロパティです。

ようなテスト メソッド、`DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound`が存在しないメッセージを削除しようとしての結果を確認します。 ここでは、データベース内の予期されるメッセージが、実際のメッセージの後に等しいにする必要があります、`DeleteMessageAsync`メソッドを実行します。 ありません、データベースのコンテンツを変更します。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>ページのモデルのメソッドの単体テスト

別の一連の単体テストは、ページのモデルのメソッドのテストを担当します。 メッセージのアプリで、インデックス ページのモデル内にある、`IndexModel`クラス*src/RazorPagesTestSample/Pages/Index.cshtml.cs*です。

| ページ モデル メソッド | 関数 |
| ----------------- | -------- |
| `OnGetAsync` | 使用して、UI の DAL からメッセージを取得、`GetMessagesAsync`メソッドです。 |
| `OnPostAddMessageAsync` | 場合、`ModelState`が有効では、呼び出しの`AddMessageAsync`をデータベースにメッセージを追加します。 |
| `OnPostDeleteAllMessagesAsync` | 呼び出し`DeleteAllMessagesAsync`をすべてのデータベースにメッセージを削除します。 |
| `OnPostDeleteMessageAsync` | 実行`DeleteMessageAsync`でメッセージを削除する、`Id`指定します。 |
| `OnPostAnalyzeMessagesAsync` | 1 つまたは複数のメッセージが、データベース内にある場合は、1 つのメッセージの単語の平均数を計算します。 |

7 つのテストを使用してページ モデル メソッドがテストされます、`IndexPageTests`クラス (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*)。 テストは、使い慣れた配置をアサート Act パターンを使用します。 これらのテストが焦点になりました。

* かどうか方法に従って、正しい動作を決定するときに、`ModelState`が無効です。
* 正しいメソッドを確認する生成`IActionResult`です。
* プロパティ値の割り当てが正しく行われたことを確認しています。

このテストのグループは、多くの場合、ページ モデル メソッドが実行される Act ステップに必要なデータを生成するために DAL のメソッドをモックします。 たとえば、`GetMessagesAsync`のメソッド、`AppDbContext`出力を生成するモックします。 ページのモデル メソッドは、このメソッドを実行するとき、モックは結果を返します。 データベースからは、データがありません。 これには、DAL を使用してページ モデルのテストでの予測可能な信頼性の高いテスト条件が作成されます。

`OnGetAsync_PopulatesThePageModel_WithAListOfMessages`番組をテストする方法、`GetMessagesAsync`メソッドは、ページのモデルのモックします。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

ときに、 `OnGetAsync` Act の手順でメソッドが実行され、ページのモデルの呼び出し`GetMessagesAsync`メソッドです。

単体テストの Act の手順 (*tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs*)。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage` ページのモデルの`OnGetAsync`メソッド (*src/RazorPagesTestSample/Pages/Index.cshtml.cs*)。

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync` DAL でメソッドが、このメソッドの呼び出しの結果を返さない。 モック バージョンのメソッドは、結果を返します。

`Assert`手順、実際のメッセージ (`actualMessages`) から割り当てられた、`Messages`ページ モデルのプロパティです。 型チェックは、メッセージが割り当てられている場合にも実行されます。 比較に予測と実際のメッセージはその`Text`プロパティです。 テストをアサートする 2 つ`List<Message>`インスタンスが同じメッセージを格納します。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

このグループの他のテストの作成 ページを含むモデル オブジェクト、 `DefaultHttpContext`、 `ModelStateDictionary`、`ActionContext`確立するために、 `PageContext`、 `ViewDataDictionary`、および`PageContext`です。 これらは、テストを実施するのに便利です。 たとえば、メッセージ アプリを確立、`ModelState`でエラーが`AddModelError`ことを確認する有効な`PageResult`ときに返される`OnPostAddMessageAsync`が実行されます。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>その他の技術情報

* [単体テスト c# dotnet テスト、xUnit を使用して .NET Core](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [テスト コントローラー](xref:mvc/controllers/testing)
* [単体テスト、コード](/visualstudio/test/unit-test-your-code)(Visual Studio)
* [統合テスト](xref:test/integration-tests)
* [xUnit.net](https://xunit.github.io/)
* [XUnit.net (.NET Core/ASP.NET Core) の概要](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq クイック スタート](https://github.com/Moq/moq4/wiki/Quickstart)
