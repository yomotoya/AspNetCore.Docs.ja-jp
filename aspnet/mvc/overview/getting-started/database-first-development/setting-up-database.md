---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: Entity Framework 6 Database First と MVC 5 の使用の概要 |Microsoft Docs
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
ms.date: 10/01/2014
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: 20e590ad1b3a59f93d1ba48a2564ddc1a5cbb315
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836121"
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Entity Framework 6 Database First と MVC 5 の使用の概要
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。 シリーズの最後の部分では、Azure をサイトとデータベースをデプロイします。
> 
> シリーズのこの部分は、データベースを作成し、データを設定することについて説明します。
> 
> このシリーズは、Tom Dykstra と Rick Anderson の投稿で記述されています。 [コメント] セクションのユーザーからのフィードバックに基づいて、改良されました。


## <a name="introduction"></a>はじめに

このトピックでは最初に使用する方法、既存データベース ユーザー データと対話できるようにする web アプリケーションをすばやく作成します。 Entity Framework 6 と MVC 5 web アプリケーションの構築に使用します。 ASP.NET のスキャフォールディング機能では、表示、更新、作成およびデータを削除するためのコードを自動的に生成することができます。 Visual Studio 内で発行ツールを使用することができます簡単に、サイトとデータベース Azure にデプロイします。

このトピックをデータベースがあり、そのデータベースのフィールドに基づく web アプリケーションのコードを生成するような状況を説明します。 このアプローチには、Database First の開発が呼び出されます。 既存のデータベースがあるまだない場合は、データ クラスを定義し、クラスのプロパティからデータベースを生成するには Code First の開発と呼ばれるアプローチを代わりに使用できます。

Code First の開発の基本的な例を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)します。 高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションの Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。

Entity Framework を使用する方法の選択に関するガイダンスについては、次を参照してください。 [Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)します。

## <a name="prerequisites"></a>必須コンポーネント

Visual Studio 2013 または Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>データベースを設定します。

場合、既存のデータベース環境を模倣するためには最初に自動的に入力データ、データベースが作成され、データベースに接続する web アプリケーションを作成し。

このチュートリアルは、LocalDB を Visual Studio 2013 または Visual Studio Express 2013 for Web を使用して開発されました。 LocalDB は、代わりに既存のデータベース サーバーを使用することができますが、によって、バージョンの Visual Studio とデータベースの種類では、すべて Visual Studio でデータ ツールの可能性がありますがサポートされません。 ツールが、データベースの利用できない場合は、データベースの管理スイートに含まれるデータベース固有の手順の一部を実行する必要があります。

Visual Studio のバージョンのデータベース ツールの問題があれば、データベース ツールの最新バージョンをインストールしておくことを確認します。 更新またはデータベース ツールをインストールする方法については、次を参照してください。 [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)します。

Visual Studio を起動し、作成、 **SQL Server データベース プロジェクト**します。 プロジェクトに名前を**ContosoUniversityData**します。

![データベース プロジェクトを作成します。](setting-up-database/_static/image1.png)

空のデータベース プロジェクトがあるようになりました。 プロジェクトのターゲット プラットフォームとして Azure SQL Database を設定する必要があります、このチュートリアルで後で Azure には、このデータベースはデプロイされます。 ターゲット プラットフォームの設定も、データベースは実際には配置しませんデータベースの設計が、ターゲット プラットフォームと互換性があるデータベース プロジェクトを確認するということです。 ターゲット プラットフォームを設定するには、開く、**プロパティ**を選択してプロジェクト**Microsoft Azure SQL Database**のターゲット プラットフォーム。

![ターゲット プラットフォームの設定](setting-up-database/_static/image2.png)

テーブルを定義する SQL スクリプトを追加することで、このチュートリアルに必要なテーブルを作成することができます。 プロジェクトを右クリックし、新しい項目を追加します。

![新しい項目を追加します。](setting-up-database/_static/image3.png)

学生をという名前の新しいテーブルを追加します。

![student テーブルを追加します。](setting-up-database/_static/image4.png)

テーブルのファイルでの T-SQL コマンドをテーブルを作成する次のコードに置き換えます。

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

デザイン ウィンドウが、コードと自動的に同期することに注意してください。 コードまたはデザイナーを使用できます。

![コードと設計を表示します。](setting-up-database/_static/image5.png)

別のテーブルを追加します。 この時点では、コースという名前を付けますし、次の T-SQL コマンドを使用します。

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

1 つの登録をという名前のテーブルの作成に時間を繰り返します。

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

データベースを展開した後に実行されるスクリプトを使用してデータを使用してデータベースを設定することができます。 配置後スクリプトをプロジェクトに追加します。 既定の名前を使用することができます。

![配置後スクリプトを追加します。](setting-up-database/_static/image6.png)

次の T-SQL コードを配置後スクリプトに追加します。 このスクリプトは、一致するレコードが見つからない場合単データとデータベースに追加します。 上書きしたり、データベースに入力したデータを削除しません。

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

データベース プロジェクトをデプロイするたびに、配置後スクリプトが実行されるに注意してください。 そのため、このスクリプトを記述するときに、要件を慎重に検討する必要があります。 場合によっては、プロジェクトを配置するたびに既知のデータ セットから開始する可能性があります。 それ以外の場合、何らかの方法で既存のデータを変更することがありますしません。 お客様の要件に基づき、配置後スクリプトまたはスクリプトに含める必要がある必要があるかどうかを決定できます。 配置後スクリプトを使用してデータベースの作成についての詳細については、次を参照してください。[などのデータを SQL Server データベース プロジェクトで](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)します。

4 つの SQL スクリプト ファイルがない実際のテーブルがあるようになりました。 Localdb にデータベース プロジェクトをデプロイする準備が整いました。 Visual Studio で構築して、データベース プロジェクトをデプロイする [スタート] ボタン (または f5 キー) をクリックします。 ビルド、配置が成功したことを確認する [出力] タブを確認します。

![出力の表示します。](setting-up-database/_static/image7.png)

新しいデータベースが作成されたことを表示する**SQL Server オブジェクト エクスプ ローラー**し、適切なローカル データベース サーバーで、プロジェクトの名前を探します (この場合 **(localdb) \ProjectsV12**)。

![新しいデータベースを表示します。](setting-up-database/_static/image8.png)

テーブルにデータが設定されているを表示するには、テーブルを右クリックして**ビュー データ**します。

![テーブル データを表示します。](setting-up-database/_static/image9.png)

テーブル データの編集可能なビューが表示されます。

![テーブル データの結果を表示します。](setting-up-database/_static/image10.png)

これで、データベースを設定して、データ設定されます。 次のチュートリアルでは、データベースの web アプリケーションを作成します。

> [!div class="step-by-step"]
> [次へ](creating-the-web-application.md)
