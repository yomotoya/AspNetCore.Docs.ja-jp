---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: モデル バインディングと web フォームを使用するプロジェクトに追加のビジネス ロジック層 |Microsoft Docs
author: tfitzmac
description: このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線にしています.
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 70e7ae6ad12c26ea5001ff68b25c0431a7440a77
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837874"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>モデル バインディングと web フォームを使用するプロジェクトに追加のビジネス ロジック層
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアル シリーズでは、モデル バインドを使用して ASP.NET Web フォーム プロジェクトでの基本的な側面について説明します。 モデルのバインドは、(ObjectDataSource や SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作を使用します。 このシリーズでは、入門用資料から開始して、後のチュートリアルで高度な概念に移動します。
> 
> このチュートリアルでは、ビジネス ロジック層でモデルのバインディングを使用する方法を示します。 現在のページ以外のオブジェクトを使用するデータ メソッドを呼び出すことを指定する OnCallingDataMethods メンバーを設定します。
> 
> このチュートリアルで作成したプロジェクトで、[以前](retrieving-data.md)系列の部分。
> 
> できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)c# または VB. で完全なプロジェクト ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 テンプレートと若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>構築します

モデル バインドでは、web ページの分離コード ファイル内、または個別のビジネス ロジック クラスで、データ操作のコードを配置することができます。 前のチュートリアルは、データ操作のコードの分離コード ファイルを使用する方法を説明します。 このアプローチは、小規模なサイトは、大規模なサイトを保守する際に、繰り返しおよびより難解をコーディングする可能性しますが、あります。 プログラムで抽象化レイヤーがないために、分離コード ファイルに含まれているコードをテストする非常に難しいことができます。

データ操作のコードを集中管理するには、すべてのデータを操作するためのロジックを含むビジネス ロジック層を作成できます。 ビジネス ロジック層は、web ページから呼び出します。 このチュートリアルでは、すべてにビジネス ロジック層では、前のチュートリアルで作成したコードの移動し、ページからそのコードを使用する方法を示します。

このチュートリアルではあります。

1. ビジネス ロジック層に分離コード ファイルからコードを移動します。
2. ビジネス ロジック層で、メソッドを呼び出す、データ バインド コントロールを変更します。

## <a name="create-business-logic-layer"></a>ビジネス ロジック層を作成します。

ここで、web ページから呼び出されるクラスを作成します。 このクラスのメソッドのように、前のチュートリアルで使用する方法と、値プロバイダー属性が含まれます。

最初に、という名前の新しいフォルダーを追加**BLL**します。

![フォルダーを追加します。](adding-business-logic-layer/_static/image1.png)

という名前の新しいクラスを作成、BLL フォルダーで**SchoolBL.cs**します。 すべての分離コード ファイル内に置かれていたデータ操作が含まれます。 メソッドは、分離コード ファイル内のメソッドとほぼ同じですが、いくつかの変更が含まれます。

最も重要な変更に注意してがのインスタンス内からコードを実行しているされなく**ページ**クラス。 ページ クラスが含まれています、 **tryupdatemodel に渡します**メソッドと**ModelState**プロパティ。 ビジネス ロジック層にこのコードを移動すると、不要になったこれらのメンバーを呼び出すページ クラスのインスタンスがあります。 この問題を回避するには、追加する必要があります、 **ModelMethodContext** tryupdatemodel に渡しますまたは ModelState にアクセスする任意のメソッドのパラメーター。 Tryupdatemodel に渡しますを呼び出したり、ModelState を取得するには、この ModelMethodContext パラメーターを使用します。 この新しいパラメーターに対応する web ページで何も変更する必要はありません。

SchoolBL.cs でコードを次のコードに置き換えます。

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>ビジネス ロジック層からデータを取得する既存のページを修正します。

最後に、ビジネス ロジック層を使用する分離コード ファイルにクエリを使用してから、Students.aspx、AddStudent.aspx、Courses.aspx のページが変換されます。

学生や AddStudent、コースの分離コード ファイルで削除するか、次のクエリ メソッドをコメントします。

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

ようになりましたが必要ないコード データ操作に関連する分離コード ファイルです。

**OnCallingDataMethods**イベント ハンドラーでは、データ メソッドを使用するオブジェクトを指定することができます。 Students.aspx では、そのイベント ハンドラーの値を追加し、ビジネス ロジック クラスのメソッドの名前に、データのメソッドの名前を変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Students.aspx の分離コード ファイルでは、CallingDataMethods イベントのイベント ハンドラーを定義します。 このイベント ハンドラーでは、データ操作のビジネス ロジック クラスを指定します。

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

AddStudent.aspx で同様に変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Courses.aspx で同様に変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

アプリケーションを実行し、以前保持していたとおりに機能のすべてのページことに注意してください。 検証ロジックが正常に動作することもできます。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、データ アクセス層とビジネス ロジック層を使用するアプリケーションを再構造。 データ コントロールがデータ操作の現在のページではないオブジェクトを使用することを指定しました。

> [!div class="step-by-step"]
> [前へ](using-query-string-values-to-retrieve-data.md)
