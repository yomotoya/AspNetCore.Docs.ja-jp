---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: "モデル バインディング機能と web フォームを使用するプロジェクトに追加するビジネス ロジック層 |Microsoft ドキュメント"
author: tfitzmac
description: "このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインドは、データの操作詳細直線-しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: ca50690052cca73a718342a9725c8096a72f1187
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>モデル バインディング機能と web フォームを使用するプロジェクトに追加するビジネス ロジック層
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> このチュートリアルの系列では、モデル バインディングを使用して ASP.NET Web フォーム プロジェクトとの基本的な側面について説明します。 モデル バインディングは、ObjectDataSource または SqlDataSource) などのソース オブジェクトにデータを処理するよりもより簡単にデータの操作にします。 この系列は入門資料で始まるし、後のチュートリアルより高度な概念に移動されます。
> 
> このチュートリアルでは、ビジネス ロジック層でモデル バインディングを使用する方法を示します。 現在のページ以外のオブジェクトを使用するデータのメソッドを呼び出すことを指定する OnCallingDataMethods メンバーを設定します。
> 
> このチュートリアルで作成されたプロジェクトで、[以前](retrieving-data.md)シリーズの一部です。
> 
> 実行できます[ダウンロード](https://go.microsoft.com/fwlink/?LinkId=286116)プロジェクト、完全な c# または vb です。 ダウンロード可能なコードは、Visual Studio 2012 または Visual Studio 2013 のいずれかで動作します。 これは、このチュートリアルで示すように Visual Studio 2013 のテンプレートよりも若干異なる Visual Studio 2012 テンプレートを使用します。


## <a name="what-youll-build"></a>新機能のビルド

モデル バインディングでは、コードを記述してデータの対話、web ページの分離コード ファイルでクラスまたは個別のビジネス ロジック クラスにすることができます。 前のチュートリアルでは、データの相互作用のコードの分離コード ファイルを使用する方法を説明しました。 この方法は小規模なサイトが、コードの繰り返しと大きいが困難な大規模なサイトを保持する場合になることができます。 プログラムでコードをテストする抽象化レイヤーが存在しないために、分離コード ファイルに存在する非常に困難なことができます。

データの相互作用のコードを集中管理するには、すべてのデータと対話するためのロジックがビジネス ロジック層を作成できます。 ビジネス ロジック層は、web ページから呼び出します。 このチュートリアルでは、すべてのビジネス ロジック層には、前のチュートリアルで作成したコードを移動し、ページからそのコードを使用する方法を示します。

このチュートリアルでは、次の手順を。

1. ビジネス ロジック層に分離コード ファイルからコードを移動します。
2. メソッドを呼び出すビジネス ロジック層で、データ バインド コントロールを変更します。

## <a name="create-business-logic-layer"></a>ビジネス ロジック層を作成します。

ここで、web ページから呼び出されるクラスを作成します。 このクラスのメソッドは、前のチュートリアルで使用されるメソッドに似たし、値プロバイダー属性が含まれます。

という名前の新しいフォルダーを最初に、追加**BLL**です。

![フォルダーを追加します。](adding-business-logic-layer/_static/image1.png)

BLL フォルダーでは、という名前の新しいクラスを作成**SchoolBL.cs**です。 すべての分離コード ファイル内に置かれていたデータ操作が格納されます。 メソッドは、分離コード ファイル内のメソッドとほとんど同じですが、いくつかの変更が含まれます。

最も重要な変更点は、インスタンス内からコードを実行している不要になった**ページ**クラスです。 ページのクラスが含まれています、 **TryUpdateModel**メソッドおよび**ModelState**プロパティです。 ビジネス ロジック層にこのコードを移動すると、不要になったこれらのメンバーを呼び出すページ クラスのインスタンスがあります。 この問題を回避する必要がありますを追加する、 **ModelMethodContext** TryUpdateModel や ModelState にアクセスするすべてのメソッドのパラメーターです。 TryUpdateModel を呼び出したり、ModelState を取得するには、この ModelMethodContext パラメーターを使用します。 この新しいパラメーターのために web ページで何も変更する必要はありません。

SchoolBL.cs でコードを次のコードに置き換えます。

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>ビジネス ロジック層からデータを取得する既存のページを変更します。

最後に、ビジネス ロジック層を使用する分離コード ファイルでクエリの使用から Students.aspx、AddStudent.aspx、Courses.aspx のページが変換されます。

受講者、AddStudent、およびコースの分離コード ファイルを削除またはコメントに、次のクエリ方法。

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

今すぐが必要ないコード データ操作に関連する分離コード ファイルでです。

**OnCallingDataMethods**イベント ハンドラーでは、データのメソッドを使用するオブジェクトを指定することができます。 Students.aspx でそのイベント ハンドラーの値を追加し、ビジネス ロジックのクラスのメソッドの名前に、データのメソッドの名前を変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Students.aspx の分離コード ファイルでは、CallingDataMethods イベントのイベント ハンドラーを定義します。 このイベント ハンドラーでは、データ操作のビジネス ロジックのクラスを指定します。

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

AddStudent.aspx で同様に変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Courses.aspx で同様に変更します。

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

アプリケーションを実行し、以前と関数のすべてのページことに注意してください。 検証ロジックもが正しく動作します。

## <a name="conclusion"></a>まとめ

このチュートリアルでは、アプリケーションで、データ アクセス層とビジネス ロジック層を使用する再構造です。 データ コントロールがデータの操作の現在のページではないオブジェクトを使用することを指定しました。

>[!div class="step-by-step"]
[前へ](using-query-string-values-to-retrieve-data.md)
