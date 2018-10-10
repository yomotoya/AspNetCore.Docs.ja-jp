---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2 の依存関係の挿入 |Microsoft Docs
author: MikeWasson
description: このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。 ソフトウェアのバージョンがチュートリアルの Web API 2 Unity Application Block で使用しています.
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: c58b06af0044144cf28cc36c16a41672aa1f6eb3
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911266"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2 の依存関係の挿入
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> このチュートリアルでは、ASP.NET Web API コント ローラーに依存関係を挿入する方法を示します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 2
> - [Unity Application Block](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 の (バージョン 5 の機能も)


## <a name="what-is-dependency-injection"></a>依存関係の挿入とは何ですか。

"*依存関係*" とは、他のオブジェクトが必要とする任意のオブジェクトのことです。 たとえば、定義する一般的なは、[リポジトリ](http://martinfowler.com/eaaCatalog/repository.html)データ アクセスを処理します。 例を使って説明しましょう。 最初に、ドメイン モデルを定義します。

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Entity Framework を使用して、データベース内の項目を格納する単純なリポジトリ クラスを次に示します。

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

ここでの GET 要求をサポートする Web API コント ローラーを定義してみましょう`Product`エンティティ。 (ままにしてあります POST とわかりやすくするためには、その他の方法です。)最初の試行を次に示します。

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

コント ローラー クラスが依存している通知`ProductRepository`を知らせ、コント ローラーを作成し、`ProductRepository`インスタンス。 ただし、いくつかの理由にハード コードの依存関係で、この方法をお勧めできませんを勧めします。

- 置換する場合`ProductRepository`別の実装にもする必要があるコント ローラー クラスを変更します。
- 場合、`ProductRepository`依存関係は、コント ローラー内でこれらを構成する必要があります。 複数のコント ローラーで、大規模なプロジェクトの構成コードがプロジェクトに分散します。
- コント ローラーが、データベースのクエリにハード コードされたため、単体テストには困難です。 単体テストでは、現在デザインでは実行できません、モックまたはスタブのリポジトリを使用する必要があります。

これらの問題を取り上げます*挿入*コント ローラーにリポジトリ。 最初に、リファクタリング、`ProductRepository`クラス、インターフェイスに。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

すると、`IProductRepository`コンス トラクター パラメーターとして。

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

この例では[コンス トラクターの挿入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)します。 使用することも*セッターの挿入*setter メソッドまたはプロパティから依存関係を設定します。

ただし、アプリケーションは、コント ローラーを直接作成しないため、問題があるようになりました。 Web API の要求をルーティングし、は認識しない Web API コント ローラーを作成します`IProductRepository`します。 これでの Web API の依存関係競合回避モジュールの出番です。

## <a name="the-web-api-dependency-resolver"></a>Web API の依存関係競合回避モジュール

Web API の定義、 **IDependencyResolver**依存関係を解決するためのインターフェイス。 インターフェイスの定義を次に示します。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope**インターフェイスが 2 つのメソッドには。

- **GetService**型の 1 つのインスタンスを作成します。
- **GetServices**指定した型のオブジェクトのコレクションを作成します。

**IDependencyResolver**メソッド継承**IDependencyScope**を追加し、 **BeginScope**メソッド。 このチュートリアルの後半でスコープについて説明します。

最初に呼び出す Web API コント ローラー インスタンスを作成するとき**IDependencyResolver.GetService**、コント ローラーの種類に渡します。 コント ローラーを作成するには、すべての依存関係を解決するのには、この拡張性フックを使用できます。 場合**GetService** null を返します、Web API コント ローラー クラスをパラメーターなしのコンス トラクターを検索します。

## <a name="dependency-resolution-with-the-unity-container"></a>Unity コンテナーと依存関係の解決

完全な記述できますが**IDependencyResolver** Web API と既存の IoC コンテナー間のブリッジとして機能する目的は、最初のインターフェイスから実装します。

IoC コンテナーは、依存関係の管理を担当するソフトウェア コンポーネントです。 型、コンテナーを登録し、コンテナーを使用して、オブジェクトを作成します。 コンテナーは、依存関係を自動的に検出します。 多くの IoC コンテナーでは、オブジェクトの有効期間とスコープなどを制御することもできます。

> [!NOTE]
> "IoC"「の制御の反転」の略フレームワークからアプリケーション コードを呼び出す、一般的なパターンは。 IoC コンテナーを構築、オブジェクト、コントロールの通常のフローの「反転」をします。


このチュートリアルで使用して[Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns から&amp;プラクティス。 (その他の一般的なライブラリを含める[Castle Windsor](http://www.castleproject.org/)、 [Spring.Net](http://www.springframework.net/)、 [Autofac](https://code.google.com/p/autofac/)、 [Ninject](http://www.ninject.org/)、および[StructureMap](http://docs.structuremap.net/).)NuGet パッケージ マネージャーを使用して、Unity をインストールすることができます。 **ツール** メニューの選択 Visual Studio で**NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

実装を次に示します**IDependencyResolver** Unity コンテナーをラップします。

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> 場合、 **GetService**返すかメソッドは、型を解決することはできません、 **null**します。 場合、 **GetServices**メソッドは、型を解決することはできません、空のコレクション オブジェクトを返す必要があります。 不明な型の例外をスローしません。


## <a name="configuring-the-dependency-resolver"></a>依存関係競合回避モジュールを構成します。

依存関係競合回避モジュールを設定、 **DependencyResolver**のグローバル プロパティ**HttpConfiguration**オブジェクト。

次のコードのレジスタ、 `IProductRepository` Unity を使用したインターフェイスを作成し、`UnityResolver`します。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>依存関係のスコープとコント ローラーの有効期間

コント ローラーは、要求ごとに作成されます。 オブジェクトの有効期間を管理する**IDependencyResolver**の概念を使用して、*スコープ*します。

アタッチされている依存関係競合回避モジュール、 **HttpConfiguration**オブジェクトはグローバル スコープを持ちます。 Web API コント ローラーを作成するときに呼び出す**BeginScope**します。 このメソッドが戻る、 **IDependencyScope**子スコープを表します。

Web API を呼び出して**GetService**コント ローラーを作成する子スコープにします。 Web API を呼び出す要求が完了したら、 **Dispose**子スコープにします。 使用して、 **Dispose**メソッドをコント ローラーの依存関係を破棄します。

どのように実装する**BeginScope** IoC コンテナーに依存します。 Unity のスコープは、子コンテナーに対応します。

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

ほとんどの IoC コンテナーのような対応があります。
