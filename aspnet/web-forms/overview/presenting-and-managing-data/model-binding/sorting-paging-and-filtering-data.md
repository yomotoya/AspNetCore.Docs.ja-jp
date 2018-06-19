---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: 並べ替え、ページング、およびモデル バインディング機能と web フォームを使用してデータをフィルタ リング |Microsoft ドキュメント
author: tfitzmac
description: このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: d63ebecadd392877e4cb1d1dffe9db2d1d231190
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885407"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>並べ替え、ページング、およびモデル バインディング機能と web フォームを使用してデータをフィルター処理
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> このチュートリアルでは、並べ替え、ページング、およびモデル バインディングによるデータのフィルター処理を追加する方法を示します。
> 
> このチュートリアルは、最初に作成されたプロジェクトに基づいて[一部](retrieving-data.md)シリーズのです。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の手順を。

1. 並べ替えとデータのページングを有効にします。
2. ユーザーが選択範囲に基づくデータのフィルター処理を有効にします。

## <a name="add-sorting"></a>並べ替えを追加します。

GridView で並べ替えを有効にすることは非常に簡単です。 Student.aspx ファイル内の設定だけで**AllowSorting**に**true** GridView でします。 設定する必要はありません、 **SortExpression** DataField が自動的に使用されるように各列の値します。 GridView は、選択した値によって、データの順序を含めるようにクエリを変更します。 並べ替えを有効にする必要がある追加の強調表示されたコードを次に示します。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Web アプリケーションを実行し、別の列の値で並べ替え学生レコードをテストします。

![並べ替えの受講者](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>ページングを追加します。

ページングを有効化も非常に簡単です。 GridView で次のように設定します。、 **AllowPaging**プロパティを**true**設定と、 **PageSize**プロパティを各ページに表示するレコードの数。 このチュートリアルでは 4 に設定することができます。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Web アプリケーションを実行し、これで、レコードが複数のページを 1 ページに表示される 4 個のレコードで割って算出することを確認します。

![ページングを追加します。](sorting-paging-and-filtering-data/_static/image4.png)

クエリの遅延実行では、アプリケーションの効率が向上します。 データ セット全体を取得するには、代わりには、GridView は、現在のページのレコードだけを取得するクエリを変更します。

## <a name="filter-records-by-user-selection"></a>ユーザーの選択内容でレコードをフィルター処理します。

モデル バインドは、モデル バインド メソッドのパラメーターの値を設定する方法を指定するいくつかの属性を追加します。 これらの属性はでは、 **System.Web.ModelBinding**名前空間。 Windows コモン コントロールには以下が含まれます。

- コントロール
- クッキー
- フォーム
- Profile
- QueryString
- RouteData
- セッション
- UserProfile
- ViewState

このチュートリアルでは、コントロールの値を使用して GridView に表示するレコードをフィルター処理します。 追加、**コントロール**以前作成したクエリ メソッドに属性します。 [後](using-query-string-values-to-retrieve-data.md)チュートリアルを適用する、 **QueryString**属性値は、クエリ文字列の値からパラメーター値を取得することを指定するパラメーターをします。

最初に、上、ValidationSummary には、ドロップ ダウン受講者が示すようにフィルター選択のリストを追加します。

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

分離コード ファイルで、コントロールから値を受け取る select メソッドを変更し、値を提供するコントロールの名前に、パラメーターの名前を設定します。

追加する必要があります、**を使用して**のステートメント、 **System.Web.ModelBinding**コントロールの属性を解決するのには名前空間。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

次のコードでは、ドロップダウン リストの値に基づいて、返されたデータをフィルター処理を再動作していた select メソッドを示します。 パラメーターは、このパラメーターの値は、同じ名前を持つコントロールから取得するを指定する前に、コントロールの属性を追加します。

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Web アプリケーションを実行し、ドロップダウン生徒数の一覧をフィルターするリストから別の値を選択します。

![フィルターの受講者](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは並べ替えとデータのページングを有効になります。 コントロールの値によって、データのフィルター処理するも有効にします。

次の[チュートリアル](integrating-jquery-ui.md)動的なデータ テンプレートに JQuery UI ウィジェットを統合することにより、UI を拡張します。

> [!div class="step-by-step"]
> [前へ](updating-deleting-and-creating-data.md)
> [次へ](integrating-jquery-ui.md)
