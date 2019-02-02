---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'チュートリアル: ASP.NET MVC アプリで EF Database First のデータ検証を強化します。'
description: このチュートリアルでは、検証の要件を指定し、書式設定を表示するには、データ モデルへのデータ注釈の追加について説明します。
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 897cd7c6a40445e2a4abede50d81e101372d3233
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667623"
---
# <a name="tutorial-enhance-data-validation-for-ef-database-first-with-aspnet-mvc-app"></a>チュートリアル: ASP.NET MVC アプリで EF Database First のデータ検証を強化します。

MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。

このチュートリアルでは、検証の要件を指定し、書式設定を表示するには、データ モデルへのデータ注釈の追加について説明します。 [コメント] セクションのユーザーからのフィードバックに基づいて、改良されました。

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * データ注釈を追加します。
> * メタデータ クラスを追加します。

## <a name="prerequisites"></a>必須コンポーネント

* [ビューをカスタマイズします。](customizing-a-view.md)

## <a name="add-data-annotations"></a>データ注釈を追加します。

以前のトピックで説明するように、いくつかのデータ検証規則は、ユーザーの入力に自動的に適用されます。 たとえば、グレード プロパティの数値をのみ提供できます。 複数のデータ検証規則を指定するには、モデル クラスにデータ注釈を追加できます。 これらの注釈は、指定したプロパティの web アプリケーション全体で適用されます。 プロパティの表示方法を変更する書式属性を適用することもできます。次のようにテキスト ラベルに使用される値を変更します。

このチュートリアルでは、FirstName、LastName、および MiddleName プロパティに指定された値の長さを制限するデータの注釈を追加します。 データベースにこれらの値は 50 文字までに制限されていますただし、web アプリケーションでその文字の制限は現在適用されません。 ユーザーがこれらの値のいずれかの 50 を超える文字と、値をデータベースに保存しようとしています。 ページがクラッシュします。 グレードは、0 から 4 までの値に制限します。

選択**モデル** > **ContosoModel.edmx** > **ContosoModel.tt**を開くと、 *Student.cs*ファイル。 クラスには、次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

開いている*Enrollment.cs*し、次の強調表示されたコードを追加します。

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

ソリューションをビルドします。

をクリックして**受講者の一覧**選択**編集**します。 50 を超える文字を入力しようとすると、エラー メッセージが表示されます。

![エラー メッセージを表示します。](enhancing-data-validation/_static/image1.png)

ホーム ページに戻ります。 をクリックして**登録の一覧**選択**編集**します。 4 上のレベルを提供しようとしてください。 このエラーが表示されます。*フィールド グレードは、0 ~ 4 にする必要があります。*

## <a name="add-metadata-classes"></a>メタデータ クラスを追加します。

を変更するデータベースが予期しないときの動作のモデル クラスを直接検証属性を追加します。ただし、データベースを変更する必要がある場合、モデル クラスを再生成する、のモデル クラスに適用した属性がすべて失われます。 このアプローチは、非常に非効率的な重要な検証規則を喪失することができます。

この問題を回避するには、属性を格納するメタデータ クラスを追加することができます。 メタデータ クラスのモデル クラスを関連付けると、それらの属性は、モデルに適用されます。 この方法ですべてのメタデータ クラスに適用されている属性を失うことがなく、モデル クラスを再生成することができます。

**モデル**フォルダー、という名前のクラスを追加*Metadata.cs*します。

コードに置き換えます*Metadata.cs*を次のコード。

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

これらのメタデータ クラスには、すべての以前にモデル クラスに適用する必要がある検証属性が含まれます。 **表示**テキスト ラベルに使用される値を変更する属性を使用します。

ここで、メタデータ クラスでモデル クラスを関連付ける必要があります。

**モデル**フォルダー、という名前のクラスを追加*PartialClasses.cs*します。

ファイルの内容を次のコードに置き換えます。

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

各クラスがマークされている通知を`partial`クラス、および各一致する名前空間と名前が自動的に生成するクラスとして。 メタデータ属性を部分クラスに適用すると、データの検証属性が自動的に生成されたクラスに適用されることを確認します。 これらの属性は、メタデータ属性は再生成されません部分クラスに適用されるため、モデル クラスを再生成するときに失われたできません。

自動的に生成されたクラスを再生成するには、開く、 *ContosoModel.edmx*ファイル。 デザイン画面を選択します右クリックして、もう一度**データベースからモデルを更新**します。 データベースを変更していない場合でもこのプロセスは、クラスを再生成します。 **更新**] タブで [**テーブル**と**完了**します。

保存、 *ContosoModel.edmx*ファイル変更を適用します。

開く、 *Student.cs*ファイルまたは*Enrollment.cs*ファイル、および以前に適用するデータの検証属性は、不要になったファイルに注意してください。 ただし、アプリケーションを実行し、データを入力すると、検証規則が適用されることに注意してください。

## <a name="conclusion"></a>まとめ

このシリーズでは、ユーザーを編集、更新、作成、およびデータを削除できるように既存のデータベースからコードを生成する方法の簡単な例が用意されています。 ASP.NET MVC 5、Entity Framework および ASP.NET スキャフォールディングは、プロジェクトの作成に使用されます。 

Code First の開発の基本的な例を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)します。 

高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションの Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 Database First のデータ処理に使用する DbContext API は Code First のデータを操作するために使用する API と同じことに注意してください。 Database First を使用する場合でも、コードの最初のチュートリアルからなど、同時実行の競合を処理、読み取りと、関連するデータの更新などのより複雑なシナリオを処理する方法を学習できます。 唯一の違いは、データベース、コンテキストのクラスおよびエンティティ クラスを作成する方法には。

## <a name="additional-resources"></a>その他の技術情報

プロパティとクラスに適用できるデータ検証注釈の一覧については、次を参照してください。 [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)します。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、次の作業を行いました。

> [!div class="checklist"]
> * 追加されたデータ注釈
> * 追加のメタデータ クラス

Web アプリと SQL database を Azure App Service にデプロイする方法については、このチュートリアルを参照してください。
> [!div class="nextstepaction"]
> [Azure App Service への .NET アプリをデプロイします。](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase/)
