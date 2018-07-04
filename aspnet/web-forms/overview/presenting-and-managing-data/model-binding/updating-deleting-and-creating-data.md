---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: 更新、削除、およびモデルのバインディングと web フォームでデータの作成 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: b6cafe29d1cb46061a8743cbee62a7ffec6be990
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393877"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>更新、削除、およびモデルのバインディングと web フォームでデータを作成します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> このチュートリアルでは、作成、更新、およびモデル バインドでデータを削除する方法を示します。 次のプロパティを設定します。
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> これらのプロパティは、対応する操作を処理するメソッドの名前を受信します。 そのメソッド内でデータを操作するロジックを提供します。
> 
> このチュートリアルは、最初に作成されたプロジェクトでビルド[一部](retrieving-data.md)シリーズの。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>構築します

このチュートリアルではあります。

1. 動的なデータ テンプレートを追加します。
2. モデル バインド メソッドを介してデータを更新および削除を有効にします。
3. データ検証規則を適用 - データベースに新しいレコードの作成を有効にします。

## <a name="add-dynamic-data-templates"></a>動的なデータ テンプレートを追加します。

最適なユーザー エクスペリエンスを提供し、コードの繰り返しを最小限に抑えるには、動的なデータ テンプレートを使用します。 構築済みの動的なデータ テンプレートは、NuGet パッケージをインストールすることで、既存のサイトに簡単に統合できます。

**NuGet パッケージの管理**、インストール、 **DynamicDataTemplatesCS**します。

![動的なデータ テンプレート](updating-deleting-and-creating-data/_static/image1.png)

通知は、プロジェクトがという名前のフォルダーを今すぐに**DynamicData**します。 そのフォルダーでは、web フォームでのダイナミック コントロールに自動的に適用されているテンプレートが表示されます。

![動的なデータ フォルダー](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>有効にする更新および削除

ユーザーを更新し、データベース内のレコードを削除することは、データを取得するためのプロセスによく似ています。 **UpdateMethod**と**DeleteMethod**プロパティ、これらの操作を実行するメソッドの名前を指定します。 GridView コントロールとも編集の自動生成を指定し、ボタンを削除できます。 次の強調表示されたコードでは、GridView コードへの追加を示します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

分離コード ファイルに追加を使用して、ステートメントの**System.Data.Entity.Infrastructure**します。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

次の更新プログラムを追加し、メソッドを削除します。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**Tryupdatemodel に渡します**メソッドが web フォームからのデータ項目に一致するデータ バインドされた値を適用します。 データ項目は、id パラメーターの値に基づいて取得されます。

## <a name="enforce-validation-requirements"></a>検証の要件を適用します。

データを更新するときに、FirstName、LastName、および年の Student クラス プロパティに適用する検証属性が自動的に適用されます。 DynamicField コントロールは、検証属性に基づくクライアントとサーバーの検証コントロールを追加します。 FirstName および LastName プロパティが両方とも必要です。 FirstName が 20 文字を超えることはできず、LastName は 40 文字を超えることはできません。 年は AcademicYear 列挙体の有効な値である必要があります。

ユーザーは、検証の要件のいずれかに違反する場合、更新プログラムは続行されません。 エラー メッセージを確認するには、GridView 上 ValidationSummary コントロールを追加します。 モデル バインドからの検証エラーを表示するには、設定、 **ShowModelStateErrors**プロパティに設定**true**します。 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Web アプリケーションを実行および更新したレコードを削除します。

![データの更新](updating-deleting-and-creating-data/_static/image3.png)

編集モードで年のプロパティの値が自動的にドロップダウン リストとして表示する場合に注意します。 Year プロパティは、列挙値を列挙値の動的なデータ テンプレートが、ドロップダウン リストの編集を指定します。 開いて、そのテンプレートを見つけることができます、**列挙\_Edit.ascx**ファイル、 **DynamicData**/**FieldTemplates**フォルダー。

有効な値を指定する場合、更新プログラムが正常に完了します。 検証の要件のいずれかに違反した場合、更新プログラムは続行されませんし、グリッドの上に、エラー メッセージが表示されます。

![エラー メッセージ](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>新しいレコードを追加します。

GridView コントロールは含まれません、 **InsertMethod**プロパティと、モデル バインドで新しいレコードを追加するため使用することはできません。 InsertMethod プロパティを見つけることができます、 **FormView**、 **DetailsView**、または**ListView**コントロール。 このチュートリアルでは、新しいレコードを追加するのに FormView コントロールを使用します。

最初に、新しいレコードを追加用に作成する新しいページにリンクを追加します。 ValidationSummary を追加します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

新しいリンクは、学生のページのコンテンツの最上部に表示されます。

![新しいリンク](updating-deleting-and-creating-data/_static/image5.png)

次に、マスター ページを使用して、新しい web フォームを追加し、名前**AddStudent**します。 マスター ページとして Site.Master を選択します。

使用して新しい受講者を追加するためのフィールドをレンダリングするが、 **DynamicEntity**コントロール。 DynamicEntity コントロールは、ItemType プロパティで指定されたクラスでその編集可能なプロパティを表示します。 StudentID プロパティでマークを付けた、 **[ScaffoldColumn(false)]** 属性はレンダリングされませんので。 AddStudent ページの MainContent プレース ホルダーでは、次のコードを追加します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

分離コード ファイル (AddStudent.aspx.cs) で、追加、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

次に、新しいレコード と キャンセル ボタンのイベント ハンドラーを挿入する方法を指定する次のメソッドを追加します。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

すべての変更を保存します。

Web アプリケーションを実行し、新しい学生を作成します。

![新しい学生を追加します。](updating-deleting-and-creating-data/_static/image6.png)

クリックして**挿入**し、新しい学生が作成されたことを確認します。

![新しい学生を表示します。](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、更新、削除、およびデータの作成を有効になります。 データを操作するときに検証規則が適用されることを確認します。

次の[チュートリアル](sorting-paging-and-filtering-data.md)このシリーズでは有効にした並べ替え、ページング、およびデータのフィルター処理します。

> [!div class="step-by-step"]
> [前へ](retrieving-data.md)
> [次へ](sorting-paging-and-filtering-data.md)
