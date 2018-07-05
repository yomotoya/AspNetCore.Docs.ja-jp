---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 並べ替え、ページング、およびモデルのバインディングと web フォームを使用してデータをフィルター処理 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 7529c811e6196327094f8f735de1bd65be76ee3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398308"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>並べ替え、ページング、およびモデルのバインディングと web フォームを使用してデータをフィルター処理
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> このチュートリアルでは、並べ替え、ページング、およびモデル バインドによるデータのフィルター処理を追加する方法を示します。
> 
> このチュートリアルは、最初に作成されたプロジェクトでビルド[一部](retrieving-data.md)シリーズの。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

1. 並べ替えとデータのページングを有効にします。
2. ユーザーは選択に基づくデータのフィルター処理を有効にします。

## <a name="add-sorting"></a>並べ替えを追加します。

GridView の並べ替え機能を有効にすることは非常に簡単です。 Student.aspx ファイル内の設定だけ**AllowSorting**に**true** GridView にします。 設定する必要はありません、 **SortExpression** DataField は自動的に使用されるため、各列の値します。 GridView は選択した値で、データの並べ替えを含めるようにクエリを変更します。 次の強調表示されたコードでは、並べ替えを有効にするために必要な追加を示します。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Web アプリケーションを実行し、別の列に値によって並べ替え生徒の記録をテストします。

![並べ替えの受講者](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>ページングを追加します。

ページングを有効にも非常に簡単です。 Gridview では、設定、 **AllowPaging**プロパティを**true**設定と、 **PageSize**プロパティに各ページに表示するレコードの数。 このチュートリアルで 4 に設定できます。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Web アプリケーションを実行して、レコードは、1 ページに表示される 4 個のレコードに複数のページに分かれています。 これを注意してください。

![ページングを追加します。](sorting-paging-and-filtering-data/_static/image4.png)

クエリの遅延実行では、アプリケーションの効率が向上します。 データ セット全体を取得するには、代わりに、GridView は、現在のページのレコードのみを取得するクエリを変更します。

## <a name="filter-records-by-user-selection"></a>ユーザーの選択でレコードをフィルター処理します。

モデル バインドでは、モデル バインディングのメソッドでパラメーターの値を設定する方法を指定することを可能にするいくつかの属性を追加します。 これらの属性は、 **System.Web.ModelBinding**名前空間。 Windows コモン コントロールには以下が含まれます。

- コントロール
- クッキー
- フォーム
- Profile
- QueryString
- RouteData
- セッション
- UserProfile
- ViewState

このチュートリアルでは、GridView に表示するレコードをフィルター処理するのにコントロールの値を使用します。 追加、**コントロール**属性を前に作成したクエリ メソッド。 [後で](using-query-string-values-to-retrieve-data.md)チュートリアルが適用されます、 **QueryString**属性をクエリ文字列の値からパラメーター値を取得することを指定するパラメーター。

まず、ValidationSummary 上には、ドロップ ダウン受講者が示すようにフィルター処理するためのリストを追加します。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

分離コード ファイルで、コントロールから値を受信する select メソッドを変更し、値を提供するコントロールの名前に、パラメーターの名前を設定します。

追加する必要があります、**を使用して**のステートメント、 **System.Web.ModelBinding**コントロールの属性を解決する名前空間。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

次のコードでは、ドロップダウン リストの値に基づいて、返されるデータをフィルター処理する再作業 select メソッドを示します。 指定するパラメーターはこのパラメーターの値が同じ名前のコントロールから取得される前に、コントロールの属性を追加します。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Web アプリケーションを実行し、ドロップダウン リストの学生の一覧をフィルター処理から異なる値を選択します。

![フィルターの受講者](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは並べ替えおよびデータのページングを有効になります。 有効にしても、コントロールの値によって、データのフィルター処理します。

次の[チュートリアル](integrating-jquery-ui.md)に動的なデータ テンプレートの JQuery UI ウィジェットを統合することによって、UI を拡張します。

> [!div class="step-by-step"]
> [前へ](updating-deleting-and-creating-data.md)
> [次へ](integrating-jquery-ui.md)
