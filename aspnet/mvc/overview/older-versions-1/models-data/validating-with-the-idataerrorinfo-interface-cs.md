---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: IDataErrorInfo インターフェイス (c#) の検証 |Microsoft ドキュメント
author: StephenWalther
description: Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: b5028b2e07c4144efa59824885ce96cd8b037dff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>IDataErrorInfo インターフェイス (c#) を検証します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。


このチュートリアルの目的は、ASP.NET MVC アプリケーションの検証の実行に 1 つの方法を説明するためです。 他のユーザーから必要なフォーム フィールドの値を指定せず、HTML フォームの送信を回避する方法について学びます。 このチュートリアルでは、IErrorDataInfo インターフェイスを使用して検証を実行する方法を学習します。

## <a name="assumptions"></a>外部からの影響

このチュートリアルでは、MoviesDB データベースとムービーのデータベース テーブルを使用します。 このテーブルは、次の列を持っています。

<a id="0.5_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | Nvarchar(100) | False |
| ディレクター | Nvarchar(100) | False |
| DateReleased | DateTime | False |


このチュートリアルでは、データベース モデル クラスを生成するのに Microsoft Entity Framework を使用します。 Entity Framework によって生成されたムービー クラスは、図 1 に表示されます。


[![ムービー エンティティ](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**図 01**:「ムービー エンティティ ([フルサイズのイメージを表示するには、をクリックして](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Entity Framework を使用して、データベース モデル クラスを生成する方法の詳細についてを参照してください チュートリアルと Entity Framework モデル クラスを作成します。


## <a name="the-controller-class"></a>コント ローラー クラス

ムービーの一覧に、Home コント ローラーを使用し、新しいムービーを作成します。 このクラスのコードは、1 のリストに含まれます。

**1 - controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

リスト 1 で、ホーム コント ローラー クラスには、次の 2 つの Create() アクションが含まれています。 最初のアクションは、新しいムービーを作成するための HTML フォームを表示します。 2 番目の Create() アクションは、データベースに新しいムービーの実際の挿入を実行します。 最初の Create() アクションによって表示されるフォームがサーバーに送信されるときに、2 番目の Create() アクションが呼び出されます。

2 番目の Create() アクションが次のコード行が含まれていることを確認します。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

IsValid プロパティは、検証エラーがある場合に false を返します。 その場合は、ムービーを作成するための HTML フォームを含むビューを作成するが再表示されます。

## <a name="creating-a-partial-class"></a>部分クラスを作成します。

ムービー クラスは、Entity Framework によって生成されます。 ソリューション エクスプ ローラー ウィンドウで、MoviesDBModel.edmx ファイルを展開すると、コード エディターで MoviesDBModel.Designer.cs ファイルを開く、ムービー クラスのコードを確認できます (図 2 を参照してください)。


[![ムービー エンティティのコード](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**図 02**: ムービー エンティティのコード ([フルサイズのイメージを表示するをクリックして](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


ムービー クラスは、部分クラスです。 つまり、ムービー クラスの機能を拡張する同じ名前の別の部分クラスを追加できます。 新しい部分クラスに、検証ロジックを追加します。

Models フォルダーに一覧表示する 2 で、クラスを追加します。

**2 - Models\Movie.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

通知を一覧表示する 2 でクラスを含む、*部分*修飾子です。 任意のメソッドやプロパティをこのクラスに追加する Entity Framework によって生成されたムービー クラスの一部になります。

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>OnChanging と OnChanged 部分メソッドを追加します。

Entity Framework では、エンティティ クラスを生成するとき、Entity Framework 部分メソッドをクラスに自動的に追加します。 Entity Framework では、クラスの各プロパティに対応する OnChanging と OnChanged の部分的なメソッドを生成します。

ムービー クラスの場合は、Entity Framework は、次のメソッドを作成します。

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

対応するプロパティを変更するには、OnChanging メソッドが適切に呼び出されます。 プロパティが変更された後は、OnChanged メソッドが適切に呼び出されます。

ムービー クラスに検証ロジックを追加するこれらの部分メソッドの利用できます。 更新リスト 3 のムービー クラスは、タイトルとディレクター プロパティが空でない値を割り当てられているを確認します。

> [!NOTE] 
> 
> 部分メソッドは、実装する必要はありません、クラスで定義されている方法です。 部分メソッドを実装しない場合は、コンパイラがメソッドのシグネチャを削除し、メソッドをこのようにすべての呼び出しには、部分メソッドに関連付けられているランタイムのコストはありません。 Visual Studio コード エディターで、キーワードを入力して部分メソッドを追加することができます*部分*後にスペースを実装するパーシャルの一覧を表示します。


**3 - Models\Movie.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

たとえば、タイトルのプロパティに空の文字列を代入しようとすると、し、エラー メッセージに割り当てられますという名前のディクショナリ\_エラーです。

この時点では、実際に何も、Title プロパティに空の文字列を割り当てるし、エラーが、プライベートに追加\_エラー フィールドです。 ASP.NET MVC フレームワークにこれらの検証エラーを公開する IDataErrorInfo インターフェイスを実装する必要があります。

## <a name="implementing-the-idataerrorinfo-interface"></a>IDataErrorInfo インターフェイスを実装します。

IDataErrorInfo インターフェイスは、最初のバージョンから .NET framework の一部をされました。 このインターフェイスは、非常に単純なインターフェイスです。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

クラスは、IDataErrorInfo インターフェイスを実装する場合、クラスのインスタンスを作成するときに、ASP.NET MVC フレームワークはこのインターフェイスを使用します。 たとえば、Home コント ローラー Create() アクションは、ムービー クラスのインスタンスを受け入れます。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

ASP.NET MVC フレームワークは、モデル バインダー (DefaultModelBinder) を使用して、Create() アクションに渡される、ムービーのインスタンスを作成します。 モデル バインダーは、ムービー オブジェクトのインスタンスには HTML フォームのフィールドを連結してムービー オブジェクトのインスタンスを作成します。

DefaultModelBinder クラス IDataErrorInfo インターフェイスを実装するかどうかを検出します。 クラスは、このインターフェイスを実装している場合、モデル バインダーは、クラスの各プロパティの IDataErrorInfo.this インデクサーを呼び出します。 インデクサーにエラー メッセージが返された場合、モデル バインダーは状態を自動的にモデル化するには、このエラー メッセージを追加します。

DefaultModelBinder は IDataErrorInfo.Error プロパティも確認します。 このプロパティは、クラスに関連付けられたプロパティ以外の特定の検証エラーを表すために使用します。 たとえば、ムービー クラスの複数のプロパティの値に依存する検証規則を適用することができます。 その場合は、エラー プロパティから、検証エラーを返すとします。

コード サンプル 4 の更新後のムービー クラスでは、IDataErrorInfo インターフェイスを実装します。

**4 - Models\Movie.cs (IDataErrorInfo を実装) を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

インデクサー プロパティは、チェックを一覧表示する 4 で、\_インデクサーに渡されたプロパティの名前に対応するキーが含まれているかどうかはエラーのコレクション。 プロパティに関連付けられている検証エラーがない場合は、空の文字列が返されます。

変更したムービー クラスを使用する任意の方法で、Home コント ローラーを変更する必要はありません。 図 3 に表示されるページは、タイトルやディレクター フォーム フィールドの値が入力されていない場合の動作を示しています。


[![アクション メソッドを自動的に作成します。](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**図 03**: 欠損値を含むフォーム ([フルサイズのイメージを表示するをクリックして](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


DateReleased 値を自動的に検証することを確認します。 DateReleased プロパティが NULL 値を受理しないため、DefaultModelBinder は、値があるないときに自動的にこのプロパティの検証エラーを生成します。 DateReleased プロパティは、エラー メッセージを変更する場合は、カスタム モデル バインダーを作成する必要があります。

## <a name="summary"></a>まとめ

このチュートリアルでは、IDataErrorInfo インターフェイスを使用して、検証エラー メッセージを生成する方法を学習します。 最初に、Entity Framework によって生成された部分的なムービー クラスの機能を拡張する部分ムービー クラスを作成します。 次にムービー クラス OnTitleChanging() と OnDirectorChanging() 部分メソッドに検証ロジックを追加します。 最後に、ASP.NET MVC フレームワークにこれらの検証メッセージを公開するために、IDataErrorInfo インターフェイスを実装しました。

> [!div class="step-by-step"]
> [前へ](performing-simple-validation-cs.md)
> [次へ](validating-with-a-service-layer-cs.md)
