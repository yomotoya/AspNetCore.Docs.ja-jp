---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 の依存関係の挿入 |Microsoft ドキュメント
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。 ソフトウェアのバージョンがチュートリアル Web API 2 Unity アプリケーション ブロックで使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036516"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2 の依存関係の挿入
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトをダウンロードします。](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - Web API 2
> - [Unity アプリケーション ブロック](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (バージョン 5 でも動作)


## <a name="what-is-dependency-injection"></a>依存関係の挿入とは何ですか。

A*依存関係*別のオブジェクトを必要とする任意のオブジェクトします。 たとえばを定義する一般的なは、[リポジトリ](http://martinfowler.com/eaaCatalog/repository.html)データ アクセスを処理します。 例を使って説明しましょう。 最初に、ドメイン モデルを定義します。

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Entity Framework を使用して、データベース内の項目を格納する単純なリポジトリ クラスを次に示します。

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

ここでの GET 要求をサポートする Web API コント ローラーを定義してみましょう`Product`エンティティです。 (いい POST とわかりやすくするためには、他のメソッドを出力します。)最初の試行を次に示します。

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

コント ローラーのクラスが依存している通知`ProductRepository`、私たちは、コント ローラーを作成できるようにすることと、`ProductRepository`インスタンス。 ただし、いくつかの理由、ハード コードの依存関係でこのようにすることは適切を勧めします。

- 置換する場合`ProductRepository`別の実装にする必要があります、コント ローラー クラスを変更します。
- 場合、`ProductRepository`依存関係がある、この内部コント ローラーを構成する必要があります。 複数のコント ローラーで、大規模なプロジェクトの構成コードがプロジェクトに分散します。
- コント ローラーは、データベースのクエリにハードコードされているため、単体テストに困難です。 単体テストでは、現在デザインでは可能では、モックまたはスタブのリポジトリを使用する必要があります。

これらの問題を解決お*送り込んで*コント ローラーにリポジトリです。 まず、リファクタリング、`ProductRepository`クラス インターフェイスを。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

指定し、`IProductRepository`コンス トラクターのパラメーターとして。

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

この例では[コンス トラクター インジェクション](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)です。 使用することも*set アクセス操作子インジェクション*、setter メソッドまたはプロパティを依存関係を設定します。

アプリケーションでは、コント ローラーを直接作成しないため、問題があるようになりました。 Web API は、要求をルーティングし、は認識しない Web API コント ローラーを作成`IProductRepository`です。 これは、Web API の依存関係競合回避モジュールの出番です。

## <a name="the-web-api-dependency-resolver"></a>Web API の 依存関係競合回避モジュール

Web API の定義、 **IDependencyResolver**の依存関係を解決するためのインターフェイスです。 インターフェイスの定義を次に示します。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope**インターフェイスが 2 つのメソッドには。

- **GetService**型の 1 つのインスタンスを作成します。
- **GetServices**指定した型のオブジェクトのコレクションを作成します。

**IDependencyResolver**メソッドを継承**IDependencyScope**を追加し、 **BeginScope**メソッドです。 このチュートリアルで後ほどスコープについて説明します。

Web API コント ローラーのインスタンスを作成すると、最初に呼び出す**IDependencyResolver.GetService**コント ローラーの種類を渡して、します。 この拡張フックを使用すると、すべての依存関係を解決する、コント ローラーを作成します。 場合**GetService**は null を返します、Web API コント ローラー クラスにパラメーターなしのコンス トラクターを検索します。

## <a name="dependency-resolution-with-the-unity-container"></a>Unity コンテナーとの依存関係の解決

完全な記述できますが**IDependencyResolver**最初、インターフェイスから実装は実際に、Web API と既存の IoC コンテナー間の仲介役として機能するように設計されています。

IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。 型のコンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。 コンテナーは、依存関係を自動的に判別されます。 多くの IoC コンテナーを使用すると、オブジェクトの有効期間、スコープなどを制御できます。

> [!NOTE]
> "IoC"が「コントロールの反転」の略フレームワークがアプリケーション コードを呼び出す、一般的なパターンはします。 IoC コンテナーは、通常の制御フローの「反転」に、オブジェクトに構築します。


このチュートリアルで使用します[Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns から&amp;プラクティスです。 (その他の一般的なライブラリを含む[城 Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Ninject](http://www.ninject.org/)、および[StructureMap](http://docs.structuremap.net/).)NuGet Package Manager を使用して、Unity をインストールすることができます。 **ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

実装を次に示します**IDependencyResolver** Unity コンテナーをラップします。

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> 場合、 **GetService**メソッドが型を解決することはできませんが返されます**null**です。 場合、 **GetServices**メソッドは、型を解決することはできません、空のコレクション オブジェクトを返す必要があります。 不明な型の例外をスローしません。


## <a name="configuring-the-dependency-resolver"></a>依存関係競合回避モジュールを構成します。

依存関係競合回避モジュールを設定、 **DependencyResolver**のグローバル プロパティ**HttpConfiguration**オブジェクト。

次のコードのレジスタ、 `IProductRepository` Unity とやり取りし、作成、`UnityResolver`です。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>依存関係スコープとコント ローラーの有効期間

コント ローラーは、要求ごとに作成されます。 オブジェクトの有効期間を管理する**IDependencyResolver**の概念を使用して、*スコープ*です。

接続されている依存関係競合回避モジュール、 **HttpConfiguration**オブジェクトはグローバル スコープを持ちます。 Web API コント ローラーを作成すると呼び出す**BeginScope**です。 このメソッドが戻る、 **IDependencyScope**子スコープを表すです。

Web API を呼び出します**GetService**コント ローラーを作成する子スコープでします。 要求が完了したら、Web API を呼び出す**Dispose**子スコープでします。 使用して、 **Dispose**コント ローラーの依存関係を破棄するメソッド。

どのように実装する**BeginScope** IoC コンテナーによって異なります。 Unity のスコープは、子コンテナーに対応します。

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

ほとんどの IoC コンテナーは、対応するようなソリューションを持ちます。
