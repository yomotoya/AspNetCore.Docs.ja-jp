---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'EF Database First と ASP.NET MVC: データ検証の拡張 |Microsoft Docs'
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
ms.date: 12/29/2014
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 9a7c6e200caa72aea61a80d6496ec1a1569e5e48
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816319"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>EF Database First と ASP.NET MVC: データ検証の拡張
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、検証の要件を指定し、書式設定を表示するには、データ モデルへのデータ注釈の追加について説明します。 [コメント] セクションのユーザーからのフィードバックに基づいて、改良されました。


## <a name="add-data-annotations"></a>データ注釈を追加します。

以前のトピックで説明するように、いくつかのデータ検証規則は、ユーザーの入力に自動的に適用されます。 たとえば、グレード プロパティの数値をのみ提供できます。 複数のデータ検証規則を指定するには、モデル クラスにデータ注釈を追加できます。 これらの注釈は、指定したプロパティの web アプリケーション全体で適用されます。 プロパティの表示方法を変更する書式属性を適用することもできます。次のようにテキスト ラベルに使用される値を変更します。

このチュートリアルでは、FirstName、LastName、および MiddleName プロパティに指定された値の長さを制限するデータの注釈を追加します。 データベースにこれらの値は 50 文字までに制限されていますただし、web アプリケーションでその文字の制限は現在適用されません。 ユーザーがこれらの値のいずれかの 50 を超える文字と、値をデータベースに保存しようとしています。 ページがクラッシュします。 グレードは、0 から 4 までの値に制限します。

開く、 **Student.cs**ファイル、**モデル**フォルダー。 クラスには、次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Enrollment.cs では、次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

ソリューションをビルドします。

編集または作成の学生のページを参照します。 50 を超える文字を入力しようとすると、エラー メッセージが表示されます。

![エラー メッセージを表示します。](enhancing-data-validation/_static/image1.png)

登録を編集するためページを参照し、4 上のレベルを提供しようとしてください。

![グレード範囲エラー](enhancing-data-validation/_static/image2.png)

プロパティとクラスに適用できるデータ検証注釈の一覧については、次を参照してください。 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)します。

## <a name="add-metadata-classes"></a>メタデータ クラスを追加します。

を変更するデータベースが予期しないときの動作のモデル クラスを直接検証属性を追加します。ただし、データベースを変更する必要がある場合、モデル クラスを再生成する、のモデル クラスに適用した属性がすべて失われます。 このアプローチは、非常に非効率的な重要な検証規則を喪失することができます。

この問題を回避するには、属性を格納するメタデータ クラスを追加することができます。 メタデータ クラスのモデル クラスを関連付けると、それらの属性は、モデルに適用されます。 この方法ですべてのメタデータ クラスに適用されている属性を失うことがなく、モデル クラスを再生成することができます。

**モデル**フォルダー、という名前のクラスを追加**Metadata.cs**します。

![メタデータ クラスを追加します。](enhancing-data-validation/_static/image3.png)

Metadata.cs でコードを次のコードに置き換えます。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

これらのメタデータ クラスには、すべての以前にモデル クラスに適用する必要がある検証属性が含まれます。 **表示**テキスト ラベルに使用される値を変更する属性を使用します。

ここで、メタデータ クラスでモデル クラスを関連付ける必要があります。

**モデル**フォルダー、という名前のクラスを追加**PartialClasses.cs**します。

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

各クラスがマークされている通知を`partial`クラス、および各一致する名前空間と名前が自動的に生成するクラスとして。 メタデータ属性を部分クラスに適用すると、データの検証属性が自動的に生成されたクラスに適用されることを確認します。 これらの属性は、メタデータ属性は再生成されません部分クラスに適用されるため、モデル クラスを再生成するときに失われたできません。

自動的に生成されたクラスを再生成するには、ContosoModel.edmx ファイルを開きます。 デザイン画面を選択します右クリックして、もう一度**データベースからモデルを更新**します。 データベースを変更していない場合でもこのプロセスは、クラスを再生成します。 **更新**] タブで [**テーブル**と**完了**します。

![テーブルを更新します。](enhancing-data-validation/_static/image4.png)

変更を適用する ContosoModel.edmx ファイルを保存します。

Student.cs ファイルまたは Enrollment.cs ファイルを開き、以前に適用するデータの検証属性が、ファイル、不要になったことに注意してください。 ただし、アプリケーションを実行し、データを入力すると、検証規則が適用されることに注意してください。

> [!div class="step-by-step"]
> [前へ](customizing-a-view.md)
> [次へ](publish-to-azure.md)
