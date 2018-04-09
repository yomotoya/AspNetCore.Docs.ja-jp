---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: クエリ文字列の値を使用してモデル バインディングでのデータをフィルター処理し、web フォーム |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>モデル バインディングと web フォームをデータのフィルター選択するクエリ文字列の値を使用します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> このチュートリアルでは、クエリ文字列の値を渡すし、その値を使用して、モデル バインディングからデータを取得する方法を示します。
> 
> このチュートリアルで作成されたプロジェクトで、[以前](retrieving-data.md)シリーズの一部です。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の手順を。

1. 登録済みのコースを受講者に表示するのに新しいページを追加します。
2. 登録済みのコースをクエリ文字列の値に基づいて、選択したスチューデントの取得します。
3. 新しいページに、グリッド ビューからクエリ文字列の値を持つハイパーリンクを追加します。

このチュートリアルの手順はどのようなにほぼ一致には、前述[チュートリアル](sorting-paging-and-filtering-data.md)をドロップダウン リストでユーザーの選択に基づいて表示される受講者をフィルター処理します。 そのチュートリアルでは使用して、**コントロール**コントロールから、パラメーターの値を取得する select メソッドの属性です。 このチュートリアルで使用する、 **QueryString**クエリ文字列からパラメーターの値を取得する select メソッドの属性です。

## <a name="add-new-page-for-displaying-a-students-courses"></a>スチューデントのコースを表示するための新しいページを追加します。

Site.master のマスター ページを使用する新しい web フォームを追加し、ページの名前を付けます**コース**です。

**Courses.aspx**ファイルに、選択した受講者コースを表示するグリッド ビューを追加します。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Select メソッドを定義します。

**Courses.aspx.cs**、グリッド ビューの指定した名前の選択メソッドを追加するは**SelectMethod**プロパティです。 このメソッドには、スチューデントのコースを取得するためにクエリを定義し、パラメーターと同じ名前を持つクエリ文字列値から、パラメーターを取得するを指定します。

最初に、次を追加する必要があります**を使用して**ステートメントです。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

その後、Courses.aspx.cs に次のコードを追加します。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

クエリ文字列の属性は、StudentID をという名前のクエリ文字列値がこのメソッドのパラメーターに自動的に割り当てられていることを意味します。

## <a name="add-hyperlink-with-query-string-value"></a>クエリ文字列の値を持つハイパーリンクを追加します。

Students.aspx で、グリッド ビューでは、新しいコース ページにリンクするハイパーリンク フィールドを追加します。 ハイパーリンクは、スチューデントの id を持つクエリ文字列値が含まれます。

Students.aspx では、合計のクレジットの次のフィールドをフィールドのすぐ下のグリッド ビューの列に追加します。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

アプリケーションを実行し、グリッド ビューに今すぐコースのリンクが含まれていることを確認します。

![ハイパーリンクを追加します。](using-query-string-values-to-retrieve-data/_static/image1.png)

クリックすると、リンクのいずれか、そのスチューデントの登録済みのコースが表示されます。

![コースを表示します。](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、クエリ文字列の値を持つリンクを追加します。 Select メソッドのパラメーター値には、そのクエリ文字列の値を使用します。

次の[チュートリアル](adding-business-logic-layer.md)、ビジネス ロジック層とデータ アクセス層に分離コード ファイルからコードを移動します。

> [!div class="step-by-step"]
> [前へ](integrating-jquery-ui.md)
> [次へ](adding-business-logic-layer.md)
