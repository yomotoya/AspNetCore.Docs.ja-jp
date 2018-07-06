---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: IDataErrorInfo インターフェイス (c#) の検証 |Microsoft Docs
author: StephenWalther
description: Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: d3d3e379e5b2cfdd1385d724c0d9bf2a83ce339a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836028"
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>IDataErrorInfo インターフェイス (c#) の検証
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther では、モデル クラスで IDataErrorInfo インターフェイスを実装することによってカスタムの検証エラー メッセージを表示する方法を示します。


このチュートリアルの目的では、ASP.NET MVC アプリケーションで検証を実行するための 1 つの方法をについて説明します。 必要なフォーム フィールドの値を指定せず、HTML フォームを送信されないようにする方法について説明します。 このチュートリアルでは、IErrorDataInfo インターフェイスを使用して検証を実行する方法について説明します。

## <a name="assumptions"></a>外部からの影響

このチュートリアルで MoviesDB データベースと映画データベース テーブルを使用します。 このテーブルは、次の列を持っています。

<a id="0.5_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | Int | False |
| Title | nvarchar (100) | False |
| ディレクター | nvarchar (100) | False |
| DateReleased | DateTime | False |


このチュートリアルでは、私のデータベース モデル クラスを生成するのに Microsoft Entity Framework を使用します。 Entity Framework によって生成されたムービー クラスは、図 1 に表示されます。


[![ムービー エンティティ](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**図 01**: The Movie エンティティ ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))。


> [!NOTE] 
> 
> Entity Framework を使用して、データベース モデル クラスを生成する方法の詳細については、拙著のチュートリアルという、Entity Framework でモデル クラスを作成するを参照してください。


## <a name="the-controller-class"></a>コント ローラー クラス

ムービーの一覧に、Home コント ローラーを使用して新しいムービーを作成します。 リスト 1 で、このクラスのコードが含まれています。

**1 - controllers \homecontroller.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

リスト 1 で、ホーム コント ローラー クラスには、2 つの Create() アクションが含まれています。 最初のアクションは、新しいムービーを作成するための HTML フォームを表示します。 2 番目の Create() アクションでは、データベースに新しいムービーの実際の挿入を実行します。 最初の Create() アクションによって表示されるフォームがサーバーに送信されたときに、2 番目の Create() アクションが呼び出されます。

2 番目の Create() アクションに次のコード行が含まれていることを確認します。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

検証エラーがある場合に、IsValid プロパティが false を返します。 その場合は、ムービーを作成するための HTML フォームを含む、作成ビューが再表示されます。

## <a name="creating-a-partial-class"></a>部分クラスを作成します。

ムービー クラスは、Entity Framework によって生成されます。 ソリューション エクスプ ローラー ウィンドウで、MoviesDBModel.edmx ファイルを展開すると、コード エディターで MoviesDBModel.Designer.cs ファイルを開き、ムービー クラスのコードを確認できます (図 2 参照)。


[![ムービー エンティティのコード](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**図 02**: ムービー エンティティのコード ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))。


ムービー クラスは、部分クラスです。 ムービー クラスの機能を拡張する同じ名前の別の部分クラスを追加できることを意味します。 新しい部分クラスに、検証ロジックを追加します。

リスト 2 で、クラスを Models フォルダーに追加します。

**2 - Models\Movie.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

リスト 2 でクラスが含まれていますが、*部分*修飾子。 メソッドまたはこのクラスに追加するプロパティは、Entity Framework によって生成されたムービー クラスの一部になります。

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>OnChanging と OnChanged 部分メソッドを追加します。

Entity Framework では、エンティティ クラスを生成するとき、Entity Framework の部分メソッドをクラスに自動的に追加します。 Entity Framework では、クラスの各プロパティに対応する OnChanging と OnChanged の部分的なメソッドを生成します。

ムービー クラスの場合は、Entity Framework は、次のメソッドを作成します。

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

OnChanging メソッドは、対応するプロパティが変更される前に、適切なと呼ばれます。 OnChanged メソッドは、プロパティが変更された後、適切なと呼ばれます。

ムービー クラスに検証ロジックを追加するこれらの部分メソッドの利用できます。 更新リスト 3 のムービー クラスは、タイトルとディレクター プロパティが空でない値に割り当てられていることを確認します。

> [!NOTE] 
> 
> 部分メソッドは、実装する必要はありませんが、クラスで定義されたメソッドです。 部分メソッドを実装しない場合、コンパイラがメソッド シグネチャを削除し、メソッドのためにすべての呼び出しには、部分メソッドに関連付けられている実行時のコストはありません。 Visual Studio コード エディターで、キーワードを入力して、部分メソッドに追加できます*部分*後にスペースを実装するパーシャルの一覧を表示します。


**3 - Models\Movie.cs を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

たとえば、Title プロパティに空の文字列を代入しようとすると、し、エラー メッセージに割り当てられますという名前のディクショナリ\_エラー。

この時点では、実際に何タイトルのプロパティに空の文字列を割り当てるし、エラーが、プライベートに追加\_エラー フィールド。 ASP.NET MVC フレームワークにこれらの検証エラーを公開する IDataErrorInfo インターフェイスを実装する必要があります。

## <a name="implementing-the-idataerrorinfo-interface"></a>IDataErrorInfo インターフェイスを実装します。

IDataErrorInfo インターフェイスは、最初のバージョンから .NET framework の一部をしました。 このインターフェイスは、非常に単純なインターフェイスです。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

クラスは、IDataErrorInfo インターフェイスを実装する場合、クラスのインスタンスを作成するときに、ASP.NET MVC フレームワークはこのインターフェイスを使用します。 たとえば、ホーム コント ローラー Create() アクションは、Movie クラスのインスタンスを受け取ります。

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

ASP.NET MVC フレームワークは、モデル バインダー (DefaultModelBinder) を使用して Create() アクションに渡されたムービーのインスタンスを作成します。 モデル バインダーは、ムービー オブジェクトのインスタンスには HTML フォームのフィールドを連結してムービー オブジェクトのインスタンスを作成します。

DefaultModelBinder クラスは、IDataErrorInfo インターフェイスを実装するかどうかを検出します。 クラスは、このインターフェイスを実装している場合、モデル バインダーは、クラスの各プロパティの IDataErrorInfo.this インデクサーを呼び出します。 インデクサーには、エラー メッセージが返されます。 モデル バインダーによって、状態を自動的にモデル化するには、このエラー メッセージが追加されます。

また、DefaultModelBinder は IDataErrorInfo.Error プロパティを確認します。 このプロパティは、クラスに関連付けられているプロパティ以外の特定の検証エラーを表すものです。 たとえば、Movie クラスの複数のプロパティの値に依存している検証規則を適用します。 この例ではエラー プロパティから、検証エラーを返します。

リスト 4 の更新された Movie クラスは、IDataErrorInfo インターフェイスを実装します。

**4 - Models\Movie.cs (IDataErrorInfo の実装) を一覧表示します。**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

リスト 4 の場合、インデクサー プロパティを確認します、\_エラー コレクションにプロパティ名に対応するキーが含まれているかどうかは、インデクサーに渡されます。 プロパティに関連付けられている検証エラーがない場合は空の文字列が返されます。

変更されたムービー クラスを使用する任意の方法で、Home コント ローラーを変更する必要はありません。 図 3 に表示されるページは、タイトルまたはディレクター フォーム フィールドの値が入力されていないときの動作を示しています。


[![アクション メソッドを自動的に作成します。](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**図 03**: 不足値を含むフォーム ([フルサイズの画像を表示する をクリックします](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))。


DateReleased 値が自動的に検証されたことに注意してください。 DateReleased プロパティが NULL 値を受け付けないので、DefaultModelBinder は、値があるないときに自動的にこのプロパティの検証エラーを生成します。 DateReleased プロパティのエラー メッセージを変更する場合は、カスタム モデル バインダーを作成する必要があります。

## <a name="summary"></a>まとめ

このチュートリアルでは、IDataErrorInfo インターフェイスを使用して、検証エラー メッセージを生成する方法について説明しました。 最初に、Entity Framework によって生成される部分的なムービー クラスの機能を拡張する部分のムービー クラスを作成します。 次に、ムービー クラス OnTitleChanging() と OnDirectorChanging() 部分メソッドに検証ロジックを追加しました。 最後に、ASP.NET MVC フレームワークにこれらの検証メッセージを公開するために、IDataErrorInfo インターフェイスを実装しました。

> [!div class="step-by-step"]
> [前へ](performing-simple-validation-cs.md)
> [次へ](validating-with-a-service-layer-cs.md)
