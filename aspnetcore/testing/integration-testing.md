---
title: "ASP.NET Core でのテストの統合"
author: ardalis
description: "ASP.NET Core の統合アプリケーションのコンポーネントが正しく動作するようにテストを使用する方法。"
keywords: "ASP.NET Core、統合 Razor をテストします。"
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 155fd2f0663c6225531a4df6f323ebb30ab1ee73
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2017
---
# <a name="integration-testing-in-aspnet-core"></a>ASP.NET Core でのテストの統合

によって[Steve Smith](https://ardalis.com/)

統合テストにより、一緒にアセンブルときに、アプリケーションのコンポーネントが正しく動作します。 ASP.NET Core サポート統合が単体テスト フレームワークと、ネットワークのオーバーヘッドが要求を処理するために使用する組み込みのテストの web ホストを使用してテストします。

[表示またはダウンロードするサンプル コード](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample)([をダウンロードする方法](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>統合テストの概要

統合テストでは、アプリケーションのさまざまな部分が正しく動作する同時を確認します。 異なり[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)、統合テスト アプリケーション インフラストラクチャ上の問題、データベース、ファイル システム、ネットワーク リソース、または web 要求と応答など多くの場合します。 単体テストを使用して fakes またはこれらの問題の代わりにモック オブジェクトですが、統合テストの目的は、システムがこれらのシステムで正しく動作することを確認します。

統合テストとコードの大規模なセグメントが実行されるため、インフラストラクチャの要素に依存しているために次の桁違い単体テストよりも低速である傾向があります。 したがって、ことをお勧め統合テストの数を制限するを記述すると、単体テストで同じ動作をテストする場合に特にです。

> [!NOTE]
> 一部の動作をテストするには、単体テストまたは統合テストを使用して、場合は、単体テストを高速になりますほぼ常になるためです。 数十または数百の多くの異なる入力で単体テストがいくつかの最も重要なシナリオをカバーする統合テストだけがあります。

独自のメソッド内のロジックをテストは、通常、単体テストのドメインです。 ASP.NET Core やデータベースなど、そのフレームワーク内で、アプリケーションの動作のテストは統合をテストになるようにします。 多くの統合テストをデータベースに行を記述し、それを読み出すことがあることを確認することを受け取りません。 データ アクセス コードのすべての可能な順列をテストする必要はありません - のみ、アプリケーションが正常に動作している信頼することを十分にテストする必要があります。

## <a name="integration-testing-aspnet-core"></a>統合テスト ASP.NET Core

実行の統合テストに設定を取得するには、テスト プロジェクトの作成を ASP.NET Core web プロジェクトへの参照を追加およびテスト ランナーをインストールする必要があります。 このプロセスの説明、[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)ドキュメントについてより詳細な手順について、テストおよびテストおよびテスト クラスの名前付けに関する推奨事項を実行するとします。

> [!NOTE]
> 単体テストとさまざまなプロジェクトを使用して、統合テストで区切ります。 これにより、により、単体テストにインフラストラクチャ上の問題を誤って導入しないことを確認し、簡単に実行するテストのセットを選択することができます。

### <a name="the-test-host"></a>テスト ホスト

ASP.NET Core には、統合テスト プロジェクトに追加することができ、アプリケーションで使用される ASP.NET Core のホストに、実際の web ホストの必要としないサービス テストを要求するテスト ホストが含まれています。 提供されたサンプルには、統合テスト プロジェクト使用するように構成されていますにはが含まれています。 [xUnit](https://xunit.github.io)とホストをテストします。 使用して、 `Microsoft.AspNetCore.TestHost` NuGet パッケージです。

1 回、`Microsoft.AspNetCore.TestHost`パッケージがプロジェクトに含まれる、作成および構成することができます、`TestServer`テストにします。 次のテストは、サイトのルートへの要求が"Hello World!"を返すことを確認する方法を示しています。 必要が正常に実行の既定値に対する Visual Studio によって作成された ASP.NET Core の空の Web テンプレート。

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

このテストでは、配置 Act アサート パターンを使用しています。 配置手順のインスタンスを作成するコンス トラクターで行われます`TestServer`です。 構成`WebHostBuilder`作成に使用される、`TestHost`以外の場合は、この例では、 `Configure` (SUT) をテスト対象システムからメソッド`Startup`クラスに渡される、`WebHostBuilder`です。 このメソッドの要求パイプラインの構成に使用する、 `TestServer` SUT サーバーの構成方法とまったく同様にします。

テストの Act 部分で、要求が行われる、`TestServer`を文字列に戻す「/」パス、および応答のインスタンスは読み取りです。 この文字列は、"Hello World!"の期待される文字列と比較されます。 一致した場合、テストに合格します。それ以外の場合、失敗します。

これで、web アプリケーションを使用して素数のチェック機能が動作することを確認するいくつかの他の統合テストを追加することができます。

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

これらのテストを使用して、素数チェッカーの正確性をテストに実際にはしようとしていることではなく、web アプリケーションが期待どおりに表示を行う際に注意します。 得られるように信頼度で単体テスト カバレッジが既にある`PrimeService`、ここで確認できます。

![テスト エクスプローラー](integration-testing/_static/test-explorer.png)

単体テストに関する詳細については、[単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)資料です。


### <a name="integration-testing-mvcrazor"></a>Mvc Razor/テストの統合

Razor ビューを含むテスト プロジェクトが必要`<PreserveCompilationContext>`に設定するのには true、 *.csproj*ファイル。


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

この要素が見つからないプロジェクトは次のようなエラーが生成されます。
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>リファクタリング ミドルウェアを使用するには

リファクタリングとは、その動作を変更することがなくそのデザインを向上させるために、アプリケーションのコードを変更するプロセスです。 理想的には、一連のこれらのヘルプでは前に、と変更後、システムの動作は同じことを確認するために、テストを渡すことがある場合に行う必要があります。 Web アプリケーションのロジック チェック素数を実装する方法を見て`Configure`メソッドを参照してください。

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

このコードの動作ですが、方法として単純なようにこれは、ASP.NET Core アプリケーションでこのような機能を実装したいかけ離れていること。 新機能を想像してください、`Configure`他の URL エンドポイントを追加するたびにこの量のコードを追加する必要がある場合は、メソッドをようになります。

考慮すべき 1 つのオプションを追加する[MVC](xref:mvc/overview)素チェックを処理するアプリケーションとコント ローラーの作成にします。 ただし、現在ないかどうかと仮定した場合必要があるその他の MVC 機能はすべて、ビットが overkill です。

活用すること、ただし、ASP.NET Core[ミドルウェア](xref:fundamentals/middleware)、いるいただくと、独自のクラス内のロジック チェック素数をカプセル化し、改善を実現[関心の分離](http://deviq.com/separation-of-concerns/)で、 `Configure`メソッド。

ミドルウェア クラスが必要ですが、パラメーターとして指定するミドルウェアを使用してパスを許可する、`RequestDelegate`と`PrimeCheckerOptions`コンス トラクター内のインスタンス。 要求のパスでは、このミドルウェアは、新機能と一致しない場合、期待するように構成する単にチェーンで次のミドルウェアを呼び出すし、それ以上何もしないでください。 実装コードの残りの部分`Configure`は、現在、`Invoke`メソッドです。

> [!NOTE]
> ミドルウェアが異なりますので、`PrimeService`サービスは、コンス トラクターでは、このサービスのインスタンスを要求してもします。 このフレームワークは経由でこのサービスを提供[依存性の注入](xref:fundamentals/dependency-injection)に構成されているの例の場合を想定して`ConfigureServices`です。

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

呼び出しがないため、このミドルウェアは、そのパスが一致するときに、要求のデリゲートのチェーン内のエンドポイントとして機能、`_next.Invoke`このミドルウェアは要求を処理します。

設定され、いくつか便利な拡張メソッドをより簡単に構成するために作成、リファクタリングされたこのミドルウェア`Configure`メソッドは、次のようになります。

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

このリファクタリング後、web アプリケーションも動作する前とに、、統合テストにすべて合格ため確実に把握しています。

> [!NOTE]
> リファクタリングを完了して、テストに合格した後、ソース管理に変更をコミットすることをお勧めします。 場合は、テスト駆動開発を練習するとき[、赤、緑-リファクター サイクルへのコミットの追加を検討してください](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development)です。

## <a name="resources"></a>リソース

* [単体テスト](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [ミドルウェア](xref:fundamentals/middleware)
* [コントローラーのテスト](xref:mvc/controllers/testing)
