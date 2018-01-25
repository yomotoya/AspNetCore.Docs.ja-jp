---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Entity Framework 6 Database First MVC 5 を使用すると作業の開始 |Microsoft ドキュメント"
author: tfitzmac
description: "MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Entity Framework 6 Database First MVC 5 を使用すると作業の開始
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。 系列の最後の部分で、サイトとデータベースを Azure に配置します。
> 
> 系列のこの部分は、データベースを作成し、データを使用して設定する方法について説明します。
> 
> この系列は、Tom Dykstra と Rick Anderson のコントリビューションで記述されています。 コメント セクション内のユーザーからのフィードバックに基づいて、改良されました。


## <a name="introduction"></a>はじめに

このトピックでは、最初に使用する方法、既存データベースし、ユーザー データと対話できるようにする web アプリケーションをすばやく作成を示します。 Web アプリケーションをビルドするのに MVC 5 の場合、Entity Framework 6 を使用します。 ASP.NET スキャフォールディング機能では、表示、更新、作成およびデータを削除するためのコードを自動的に生成することができます。 Visual Studio 内で、発行ツールを使用して、簡単に展開できます、サイトとデータベースを Azure に。

このトピックでは、ここで、データベースがありそのデータベースのフィールドに基づいて、web アプリケーションのコードを生成するような状況を説明します。 この方法は、Database First の開発と呼ばれます。 既存のデータベースがない場合は、データ クラスを定義し、クラスのプロパティからデータベースの生成が行われる Code First の開発と呼ばれる手法を代わりに使用できます。

Code First の開発の導入例は、次を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)です。 高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションを Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。

使用する Entity Framework 方法の選択に関するガイダンスについては、次を参照してください。 [Entity Framework 開発方法](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)です。

## <a name="prerequisites"></a>必須コンポーネント

Visual Studio 2013 または Visual Studio Express 2013 for Web

## <a name="set-up-the-database"></a>データベースをセットアップします。

場合、既存のデータベース環境を模倣するためには最初にいくつか自動的に入力データをデータベースが作成され、データベースに接続する web アプリケーションを作成します。

このチュートリアルは、LocalDB を Visual Studio 2013 または Visual Studio Express 2013 for Web を使用して開発されました。 LocalDB は、代わりに既存のデータベース サーバーを使用することができますが、バージョンによっては、Visual Studio とデータベースの種類のすべて Visual Studio でのデータ ツールのサポートされていません。 ツールが、データベースで使用可能でない場合は、データベースの管理のスイートに含まれるデータベース固有の手順の一部を実行する必要があります。

Visual Studio のバージョンでデータベース ツールに関する問題があれば、データベース ツールの最新バージョンをインストールしたことを確認します。 更新や、データベース ツールのインストールについては、次を参照してください。 [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027)です。

Visual Studio を起動し、作成、 **SQL Server データベース プロジェクト**です。 プロジェクトに名前を**ContosoUniversityData**です。

![create database project](setting-up-database/_static/image1.png)

空のデータベース プロジェクトがあるようになりました。 プロジェクトのターゲット プラットフォームとして Azure SQL データベースを設定する必要がありますので、このチュートリアルの後半でを Azure にこのデータベースを配置します。 ターゲット プラットフォームの設定も実際に配置しないデータベースです。のみ、データベースの設計が、ターゲット プラットフォームと互換性があるデータベース プロジェクトを確認することを意味します。 ターゲット プラットフォームを設定するには、開く、**プロパティ**を選択してプロジェクト**Microsoft Azure SQL Database**ターゲット プラットフォーム。

![セットのターゲット プラットフォーム](setting-up-database/_static/image2.png)

テーブルを定義する SQL スクリプトを追加して、このチュートリアルに必要なテーブルを作成することができます。 プロジェクトを右クリックし、新しい項目を追加します。

![新しい項目を追加します。](setting-up-database/_static/image3.png)

受講者をという名前の新しいテーブルを追加します。

![学生のテーブルを追加します。](setting-up-database/_static/image4.png)

テーブルのファイルでは、テーブルの作成に次のコードで T-SQL コマンドを置き換えます。

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

デザイン ウィンドウが、コードと自動的に同期することに注意してください。 コードまたはデザイナーを使用できます。

![コードと設計を表示します。](setting-up-database/_static/image5.png)

別のテーブルを追加します。 現時点では、コースという名前を付けますし、次の T-SQL コマンドを使用します。

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

もう一度登録をという名前のテーブルを作成するを繰り返します。

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

データベースが配置された後に実行されるスクリプトを使用してデータを使用してデータベースを設定することができます。 配置後スクリプトをプロジェクトに追加します。 既定の名前を使用することができます。

![配置後スクリプトを追加します。](setting-up-database/_static/image6.png)

次の T-SQL コードを配置後スクリプトに追加します。 このスクリプトは、一致するレコードが見つからない場合は単にデータとデータベースに追加します。 データベースに入力したデータを削除や上書きにしません。

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

配置後スクリプトが実行されること、データベース プロジェクトを配置するたびに重要です。 そのため、このスクリプトを作成するときに、要件を慎重に検討する必要があります。 場合によっては、プロジェクトを配置するたびに、既知のデータのセットから最初からやり直すを構成することが必要です。 それ以外の場合で、任意の方法で既存のデータを変更することがあります避けたいです。 要件に基づき、配置後スクリプトまたはスクリプトに含める必要がありますが必要かどうかを決定できます。 配置後スクリプトを使用して、データベースの設定の詳細については、次を参照してください。 [SQL Server データベース プロジェクト内のデータを含む](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx)です。

4 つの SQL スクリプト ファイルがない実際のテーブルがあるようになりました。 Localdb にデータベース プロジェクトを配置する準備ができたらです。 Visual Studio でビルドして、データベース プロジェクトを配置する [スタート] ボタン (または f5 キー) をクリックします。 ビルドと配置が成功したことを確認する [出力] タブを確認してください。

![出力を表示します。](setting-up-database/_static/image7.png)

新しいデータベースが作成されたことを表示するには、開く**SQL Server オブジェクト エクスプ ローラー**し、適切なローカル データベース サーバーで、プロジェクトの名前を探します (ここでは**(localdb) \ProjectsV12**)。

![新しいデータベースを表示します。](setting-up-database/_static/image8.png)

表示するテーブルにデータが設定されている、テーブルを右クリックし、選択**ビュー データ**です。

![テーブル データを表示します。](setting-up-database/_static/image9.png)

テーブルのデータの編集可能なビューが表示されます。

![データのテーブルの結果を表示します。](setting-up-database/_static/image10.png)

データベースは今すぐセットアップし、データを設定します。 次のチュートリアルでは、データベースの web アプリケーションを作成します。

>[!div class="step-by-step"]
[次へ](creating-the-web-application.md)
