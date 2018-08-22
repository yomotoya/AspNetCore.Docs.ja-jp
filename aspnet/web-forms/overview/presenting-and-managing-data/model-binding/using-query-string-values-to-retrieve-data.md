---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: クエリ文字列の値を使用してモデル バインドでデータをフィルター処理し、web フォーム |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828285"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>モデル バインディングと web フォームでデータをフィルター処理クエリ文字列の値を使用します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> このチュートリアルでは、クエリ文字列の値を渡すし、その値を使用してモデル バインドでデータを取得する方法を示します。
> 
> このチュートリアルで作成したプロジェクトで、[以前](retrieving-data.md)系列の部分。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

1. 学生の登録済みのコースを表示する新しいページを追加します。
2. クエリ文字列の値に基づいて選択した学生の登録済みのコースを取得します。
3. 新しいページに、グリッド ビューからクエリ文字列の値を持つハイパーリンクを追加します。

このチュートリアルの手順ではどのようなにかなり似ています、以前に[チュートリアル](sorting-paging-and-filtering-data.md)をドロップダウン リストでユーザーの選択に基づいて表示されている受講者をフィルター処理します。 このチュートリアルで使用して、**コントロール**select メソッド パラメーターの値がコントロールから取得されるかを指定する属性。 このチュートリアルで使用する、 **QueryString**クエリ文字列からパラメーターの値を取得することを指定する select メソッドの属性。

## <a name="add-new-page-for-displaying-a-students-courses"></a>受講者のコースを表示するための新しいページを追加します。

Site.master のマスター ページを使用する新しい web フォームを追加し、ページの名前**コース**します。

**Courses.aspx**ファイルを選択した受講者コースを表示するグリッド ビューを追加します。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Select メソッドを定義します。

**Courses.aspx.cs**、グリッド ビューの指定した名前を持つ select メソッドを追加するは**SelectMethod**プロパティ。 メソッドには、受講者のコースでは、取得するためのクエリを定義し、パラメーターがパラメーターと同じ名前のクエリ文字列値から取得されるかを指定します。

最初に、次を追加する必要があります**を使用して**ステートメント。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

次に、Courses.aspx.cs に次のコードを追加します。

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

クエリ文字列の属性は、このメソッドのパラメーターに StudentID という名前のクエリ文字列値が自動的に割り当てられているを意味します。

## <a name="add-hyperlink-with-query-string-value"></a>クエリ文字列の値を持つハイパーリンクを追加します。

Students.aspx で、グリッド ビューは、新しいコースのページにリンクするハイパーリンクのフィールドを追加します。 ハイパーリンクは、学生の id を持つクエリ文字列の値が含まれます。

Students.aspx では、クレジットの合計を次のフィールドをフィールドのすぐ下のグリッド ビューの列に追加します。

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

アプリケーションを実行し、グリッド ビューに今すぐコースのリンクが含まれていることを確認します。

![ハイパーリンクを追加します。](using-query-string-values-to-retrieve-data/_static/image1.png)

いずれかのリンクをクリックすると、学生の登録済みのコースを確認します。

![コースを表示します。](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、クエリ文字列の値を持つリンクを追加します。 Select メソッドのパラメーター値には、そのクエリ文字列の値を使用します。

次の[チュートリアル](adding-business-logic-layer.md)、ビジネス ロジック層とデータ アクセス層に分離コード ファイルからコードを移動します。

> [!div class="step-by-step"]
> [前へ](integrating-jquery-ui.md)
> [次へ](adding-business-logic-layer.md)
