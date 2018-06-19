---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: ASP.NET MVC で最初の EF データベース データの検証を拡張 |。Microsoft ドキュメント
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879609"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>ASP.NET MVC で最初の EF データベース データの検証を拡張。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、データ注釈検証の要件を指定し、書式設定を表示するには、データ モデルを追加するについて説明します。 コメント セクション内のユーザーからのフィードバックに基づいて、改良されました。


## <a name="add-data-annotations"></a>データ注釈を追加します。

以前のトピックで説明したとおり、一部のデータ検証ルールは、ユーザー入力に自動的に適用されます。 たとえば、学年プロパティの数値をのみ提供できます。 データ検証ルールを指定するには、モデル クラスをデータ注釈を追加できます。 これらの注釈は、指定したプロパティの web アプリケーション全体に適用されます。 プロパティの表示方法を変更する書式属性を適用することもできます。など、テキスト ラベルに使用する値を変更します。

このチュートリアルでは、FirstName、LastName、および MiddleName プロパティに指定された値の長さを制限するデータ注釈を追加します。 データベースにこれらの値は 50 文字までに制限されます。ただし、web アプリケーションでその文字の制限が現在適用されません。 ユーザーは、それらの値のいずれかの 50 個を超える文字を提供する場合、値をデータベースに保存しようとするときに、ページがクラッシュします。 値は 0 ~ 4 をグレードを制限することもされます。

開く、 **Student.cs**ファイルで、**モデル**フォルダーです。 クラスに次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Enrollment.cs では、次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

ソリューションをビルドします。

編集または受講者を作成するためのページを参照します。 50 個を超える文字を入力しようとすると、エラー メッセージが表示されます。

![エラー メッセージを表示します。](enhancing-data-validation/_static/image1.png)

登録を編集するためページを参照し、4 上のレベルを提供しようとしてください。

![グレード範囲エラー](enhancing-data-validation/_static/image2.png)

プロパティとクラスに適用できるデータの検証の注釈の一覧については、次を参照してください。 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)です。

## <a name="add-metadata-classes"></a>メタデータ クラスを追加します。

データベースを変更; されないようにする場合にモデル クラスを直接検証属性を追加ただし場合は、データベースを変更して、モデル クラスを再生成する必要がありますは、すべてのモデル クラスを適用した属性をできなくなります。 この方法には、非常に効率がよくないになり、重要な検証規則を失うことができます。

この問題を回避するのには、属性を格納するメタデータ クラスを追加できます。 メタデータ クラスのモデル クラスを関連付けるときに、これらの属性は、モデルに適用されます。 この方法ですべてのメタデータ クラスに適用されている属性を失うことがなく、モデル クラスを再生成することができます。

**モデル**フォルダー、という名前のクラスを追加**Metadata.cs**です。

![メタデータ クラスを追加します。](enhancing-data-validation/_static/image3.png)

Metadata.cs でコードを次のコードに置き換えます。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

これらのメタデータ クラスには、すべてのモデル クラスを適用した以前の検証属性が含まれます。 **表示**テキスト ラベルに使用される値を変更する属性を使用します。

ここで、メタデータの各クラスでモデル クラスを関連付ける必要があります。

**モデル**フォルダー、という名前のクラスを追加**PartialClasses.cs**です。

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

各クラスとしてマークされていることを確認、`partial`クラス、および各一致する名前空間と名前が自動的に生成するクラスとして。 メタデータの属性を部分クラスに適用することでは、データの検証属性が自動的に生成されたクラスに適用されることを確認します。 これらの属性は、メタデータの属性が再生成されません部分クラスに適用されるため、モデル クラスを再生成するときに失われたできません。

自動的に生成されたクラスを再生成するには、ContosoModel.edmx ファイルを開きます。 デザイン画面と選択をもう一度右クリックし**データベースからモデルを更新**です。 データベースを変更していない場合でもこのプロセスは、クラスを再生成します。 **更新**] タブで [**テーブル**と**完了**です。

![テーブルを更新します。](enhancing-data-validation/_static/image4.png)

変更を適用する ContosoModel.edmx ファイルを保存します。

Student.cs ファイルまたは Enrollment.cs ファイルを開くし、以前に適用するデータの検証属性がファイルには不要になったことに注意してください。 ただし、アプリケーションを実行し、検証規則が入力したデータをまだ適用されていることを確認します。

> [!div class="step-by-step"]
> [前へ](customizing-a-view.md)
> [次へ](publish-to-azure.md)
