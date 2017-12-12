---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "更新、削除、およびデータのモデル バインディングと web フォームを作成 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>更新、削除、およびモデル バインディングと web フォームをデータの作成
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> このチュートリアルでは、作成、更新、およびモデル バインディングでデータを削除する方法を示します。 次のプロパティが設定されます。
> 
> - deleteMethod
> - InsertMethod
> - UpdateMethod
> 
> これらのプロパティは、対応する操作を処理するメソッドの名前を受信します。 メソッドは、内では、データと対話するため、ロジックを指定します。
> 
> このチュートリアルは、最初に作成されたプロジェクトに基づいて[一部](retrieving-data.md)シリーズのです。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>新機能のビルド

このチュートリアルでは、次の手順を。

1. 動的なデータ テンプレートを追加します。
2. モデル バインド メソッドを介してデータを更新し、削除を有効にします。
3. データ検証ルールを適用 - 新しいレコードを作成するデータベースで有効にします。

## <a name="add-dynamic-data-templates"></a>動的なデータ テンプレートを追加します。

最適なユーザー エクスペリエンスを提供し、コードの繰り返しを最小限に抑えるには、動的なデータ テンプレートを使用します。 構築済みの動的なデータ テンプレートは、NuGet パッケージをインストールして、既存のサイトに簡単に統合できます。

**NuGet パッケージの管理**、インストール、 **DynamicDataTemplatesCS**です。

![動的なデータ テンプレート](updating-deleting-and-creating-data/_static/image1.png)

プロジェクトに今すぐという名前のフォルダーが含まれることに注意してください**ダイナミック**です。 そのフォルダーでは、web フォームでのダイナミック コントロールに自動的に適用されているテンプレートが見つかります。

![動的なデータ フォルダー](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>有効にする更新および削除

更新およびデータベース内のレコードを削除するユーザーの有効化は、データを取得するためのプロセスによく似ています。 **UpdateMethod**と**DeleteMethod**プロパティ、これらの操作を実行するメソッドの名前を指定します。 GridView コントロールにも編集の自動生成を指定し、ボタンを削除できます。 次の強調表示されたコードでは、GridView コードの追加機能を示します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

分離コード ファイル内の追加を使用して、ステートメントの**System.Data.Entity.Infrastructure**です。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

次の更新プログラムを追加し、メソッドを削除します。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel**メソッドは、一致するデータ バインドされた値を web フォームからのデータ項目に適用します。 Id パラメーターの値に基づいて、データ項目が取得されます。

## <a name="enforce-validation-requirements"></a>検証要件を強制します。

データを更新するときに、Student クラスでは、FirstName、LastName、および年のプロパティに適用される検証の属性が自動的に適用されます。 DynamicField コントロールは、検証の属性に基づくクライアントとサーバーの検証コントロールを追加します。 FirstName および LastName から構成プロパティの両方が必要です。 FirstName が 20 文字を超えることはできませんし、LastName は 40 文字を超えることはできません。 年は AcademicYear 列挙の有効な値である必要があります。

ユーザーは、検証の要件のいずれかに違反する場合、更新プログラムは続行されません。 エラー メッセージを表示するには、GridView 上 ValidationSummary コントロールを追加します。 モデル バインディングの検証エラーを表示する設定、 **ShowModelStateErrors**プロパティに設定**true**です。 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Web アプリケーションを実行および更新し、任意のレコードを削除します。

![データの更新](updating-deleting-and-creating-data/_static/image3.png)

編集モードで年プロパティの値が自動的にドロップダウン リストとして表示する場合は、ことを確認します。 年プロパティは、列挙値を列挙値の動的なデータ テンプレートは、ドロップダウン リストを編集するためを指定します。 そのテンプレートを検索するには、開く、**列挙\_Edit.ascx**ファイルを**ダイナミック**/**FieldTemplates**フォルダーです。

有効な値を指定する場合、更新プログラムが正常に完了します。 検証の要件のいずれかに違反した場合、更新プログラムは続行されませんし、グリッドの上に、エラー メッセージが表示されます。

![エラー メッセージ](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>新しいレコードを追加します。

GridView コントロールは含まれません、 **InsertMethod**プロパティ モデル バインディングで、新しいレコードを追加するため使用することはできません。 InsertMethod のプロパティを見つけることができます、 **FormView**、 **DetailsView**、または**ListView**コントロール。 このチュートリアルでは、FormView コントロールを使用して、新しいレコードを追加します。

最初に、新しいレコードを追加するを作成する新しいページにリンクを追加します。 ValidationSummary を追加します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

新しいリンクは、受講者 ページのコンテンツの上部に表示されます。

![新しいリンク](updating-deleting-and-creating-data/_static/image5.png)

次に、マスター ページを使用して新しい web フォームを追加し、名前**AddStudent**です。 マスター ページとして Site.Master を選択します。

使用して、新しい受講者を追加するためのフィールドを表示する、 **DynamicEntity**コントロール。 DynamicEntity コントロールは、その ItemType プロパティで指定されたクラスの編集可能なプロパティを表示します。 学生 Id プロパティとして設定されていた、 **[ScaffoldColumn(false)]**属性のためはレンダリングされません。 メインのプレース ホルダー AddStudent ページのでは、次のコードを追加します。

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

分離コード ファイル (AddStudent.aspx.cs) で、追加、**を使用して**のステートメント、 **ContosoUniversityModelBinding.Models**名前空間。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

次に、新しいレコード と キャンセル ボタンのイベント ハンドラーを挿入する方法を指定する次のメソッドを追加します。

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

すべての変更を保存します。

Web アプリケーションを実行し、新しい学生を作成します。

![新しい学生を追加します。](updating-deleting-and-creating-data/_static/image6.png)

をクリックして**挿入**し新しい学生が作成されたことを確認します。

![新しい学生を表示します。](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>まとめ

このチュートリアルでは、更新、削除、およびデータを作成して有効になります。 データを操作するときに検証規則を適用することを確認します。

次の[チュートリアル](sorting-paging-and-filtering-data.md)この系列には有効にした並べ替え、ページング、およびデータのフィルター処理します。

>[!div class="step-by-step"]
[前へ](retrieving-data.md)
[次へ](sorting-paging-and-filtering-data.md)
