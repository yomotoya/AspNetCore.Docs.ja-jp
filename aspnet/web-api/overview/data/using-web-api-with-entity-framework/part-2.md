---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: モデルとコント ローラーの追加 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: f127f239bcc665f71976bb34f6d3387f8e0b72a7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393151"
---
<a name="add-models-and-controllers"></a>モデルとコント ローラーを追加します。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](https://github.com/MikeWasson/BookService)

このセクションでは、データベースのエンティティを定義するモデル クラスを追加します。 これらのエンティティに対する CRUD 操作を実行する Web API コント ローラーを追加します。

## <a name="add-model-classes"></a>モデル クラスを追加します。

このチュートリアルで、データベース「Code First」アプローチを Entity Framework (EF) を使用して作成します。 Code First でのデータベース テーブルに対応する c# クラスを作成して EF がデータベースを作成します。 (詳細については、次を参照してください[Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)。)。

まず、Poco (plain-old CLR object) としてのドメイン オブジェクトを定義します。 次の Poco を作成します。

- 作成者
- 書籍

ソリューション エクスプ ローラーで、Models フォルダーを右クリックします。 選択**追加**を選択し、**クラス**します。 クラスに `Author` という名前を付けます。

![](part-2/_static/image1.png)

すべての Author.cs の定型コードを次のコードで置き換えます。

[!code-csharp[Main](part-2/samples/sample1.cs)]

という名前の別のクラスを追加`Book`、次のコード。

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework では、これらのモデルを使用して、データベース テーブルを作成します。 各モデルの場合、`Id`プロパティは、データベース テーブルの主キー列になります。

Book クラスで、`AuthorId`への外部キーの定義、`Author`テーブル。 (わかりやすくするために、前提に話を各書籍には、1 つの作成者がいる。)Book クラスは、関連するナビゲーション プロパティも含まれています。`Author`します。 ナビゲーション プロパティを使用するには、関連するへのアクセスに`Author`コード。 第 4 部でナビゲーション プロパティの詳細と言った[エンティティ関係の処理](part-4.md)します。

## <a name="add-web-api-controllers"></a>Web API コント ローラーを追加します。

このセクションでは、CRUD 操作をサポートする Web API コント ローラーを追加します (作成、読み取り、更新、および削除) します。 コント ローラーでは、データベース層との通信に Entity Framework を使用します。

最初に、Controllers/ValuesController.cs ファイルを削除することができます。 このファイルには、例の Web API コント ローラーが含まれていますが、このチュートリアルでは必要ありません。

![](part-2/_static/image2.png)

次に、プロジェクトをビルドします。 Web API のスキャフォールディングでは、リフレクションを使用して、コンパイル済みのアセンブリが必要があるため、モデル クラスを検索します。

ソリューション エクスプ ローラーで、Controllers フォルダーを右クリックします。 選択**追加**を選択し、**コント ローラー**します。

![](part-2/_static/image3.png)

**スキャフォールディングの追加**ダイアログ ボックスで、"Web API 2 コント ローラーとアクション、Entity Framework を使用"です。 **[追加]** をクリックします。

![](part-2/_static/image4.png)

**コント ローラーの追加**ダイアログ ボックスで、次の操作を行います。

1. **モデル クラス**] ドロップダウンで、[、`Author`クラス。 (ドロップダウンの一覧に表示されない場合、は、プロジェクトを作成することを確認します)。
2. 「非同期コント ローラー アクションを使用する」を確認します。
3. としてコント ローラーの名前をそのまま&quot;AuthorsController&quot;します。
4. クリック プラス (+) ボタンを**データ コンテキスト クラス**します。

![](part-2/_static/image5.png)

**新しいデータ コンテキスト**ダイアログ ボックスで、既定の名前のままにし、をクリックして**追加**します。

![](part-2/_static/image6.png)

クリックして**追加**を完了する、**コント ローラーの追加**ダイアログ。 ダイアログ ボックスでは、プロジェクトに 2 つのクラスを追加します。

- `AuthorsController` Web API コント ローラーを定義します。 コント ローラーは、作成者の一覧に対する CRUD 操作を実行するクライアントを使用する REST API を実装します。
- `BookServiceContext` データベース、変更の追跡、およびデータの永続化から、データベースにデータを使用してオブジェクトの設定が含まれています。 ランタイム処理中にエンティティ オブジェクトを管理します。 継承`DbContext`します。

![](part-2/_static/image7.png)

この時点では、プロジェクトを再度ビルドします。 API コント ローラーを追加する同じ手順に従って`Book`エンティティ。 この時点で、`Book`既存を選択し、モデル クラスの`BookServiceContext`データ コンテキスト クラスのクラス。 (しないは新しいデータ コンテキストを作成します。)クリックして**追加**コント ローラーを追加します。

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [前へ](part-1.md)
> [次へ](part-3.md)
