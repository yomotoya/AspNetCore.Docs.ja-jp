---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: モデルおよびコント ローラーの追加 |Microsoft ドキュメント
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879583"
---
<a name="add-models-and-controllers"></a>モデルおよびコント ローラーを追加します。
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、データベースのエンティティを定義するモデル クラスを追加します。 それらのエンティティに対する CRUD 操作を実行する Web API コント ローラーを追加します。

## <a name="add-model-classes"></a>モデル クラスを追加します。

このチュートリアルでは、データベース"Code First"アプローチ Entity Framework (EF) を使用して作成します。 Code First では、データベース テーブルに対応する c# クラスを作成して EF には、データベースが作成されます。 (詳細については、次を参照してください[Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)。

した poco から (従来の CLR オブジェクト) として、ドメイン オブジェクトを定義することで開始します。 次のした poco からを作成されます。

- 作成者
- 書籍

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 選択**追加**選択してから、**クラス**です。 クラスに `Author` という名前を付けます。

![](part-2/_static/image1.png)

次のコードでは、すべての Author.cs に定型コードを置き換えます。

[!code-csharp[Main](part-2/samples/sample1.cs)]

という名前の別のクラスを追加`Book`、次のコードにします。

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework では、これらのモデルをデータベース テーブルの作成に使用します。 モデルごとに、`Id`プロパティは、データベース テーブルの主キー列になります。

Book クラスで、`AuthorId`への外部キーの定義、`Author`テーブル。 (わかりやすくするため、すればいると仮定すると書籍ごとに 1 つの作成者です。)Book クラスには、関連するナビゲーション プロパティも含まれています`Author`です。 ナビゲーション プロパティを使用するには、関連するアクセス`Author`のコードにします。 第 4 部でのナビゲーション プロパティの詳細音声[処理エンティティ関係](part-4.md)です。

## <a name="add-web-api-controllers"></a>Web API コント ローラーを追加します。

このセクションでは、CRUD 操作をサポートする Web API コント ローラーを追加します (作成、読み取り、更新、および削除) します。 コント ローラーでは、Entity Framework をデータベース層との通信に使用します。

最初に、Controllers/ValuesController.cs ファイルを削除することができます。 このファイルには、例の Web API コント ローラーが含まれていますが、このチュートリアルでは必要ありません。

![](part-2/_static/image2.png)

次に、プロジェクトをビルドします。 Web API のスキャフォールディングでは、リフレクションを使用して、モデル クラスを見つけるため、コンパイルされたアセンブリが必要があります。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**選択してから、**コント ローラー**です。

![](part-2/_static/image3.png)

**追加 Scaffold**ダイアログで、"Web API 2 Entity Framework を使用してコント ローラーをアクションがある"です。 **[追加]** をクリックします。

![](part-2/_static/image4.png)

**コント ローラーの追加**ダイアログ ボックスで、次の操作します。

1. **モデル クラス**ドロップダウンで、、`Author`クラスです。 (が、ドロップダウン リストに表示されることが表示されない場合は、プロジェクトを作成することを確認) します。
2. 「を使用して非同期コント ローラーのアクション」を確認してください。
3. としてコント ローラーの名前をそのまま&quot;AuthorsController&quot;です。
4. クリック プラス (+) ボタンを**データ コンテキスト クラス**です。

![](part-2/_static/image5.png)

**新しいデータ コンテキスト**ダイアログ ボックスで、既定の名前のままにし、をクリックして**追加**です。

![](part-2/_static/image6.png)

をクリックして**追加**を完了する、**コント ローラーの追加**ダイアログ。 ダイアログ ボックスでは、2 つのクラスをプロジェクトに追加します。

- `AuthorsController` Web API コント ローラーを定義します。 コント ローラーでは、クライアントの作成者の一覧での CRUD 操作を実行に使用する REST API を実装します。
- `BookServiceContext` 実行中に、データベース、変更の追跡、およびデータの永続化から、データベースにデータを使用してオブジェクトの設定が含まれるエンティティ オブジェクトを管理します。 継承`DbContext`です。

![](part-2/_static/image7.png)

この時点では、プロジェクトを再度ビルドします。 ここでの API コント ローラーを追加する同じ手順を進める`Book`エンティティです。 この時点で、`Book`モデル クラス、および既存の選択の`BookServiceContext`データ コンテキスト クラスのクラスです。 (しないは新しいデータ コンテキストを作成します。)をクリックして**追加**コント ローラーを追加します。

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [前へ](part-1.md)
> [次へ](part-3.md)
