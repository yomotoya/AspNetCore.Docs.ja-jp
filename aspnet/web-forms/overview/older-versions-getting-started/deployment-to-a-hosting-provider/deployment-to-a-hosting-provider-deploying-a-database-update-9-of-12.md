---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベースの更新を展開する |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベースの更新を展開します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルで行うデータベースの変更と関連するコードの変更、Visual Studio で、変更をテストし、テストと実稼働環境に、更新プログラムを展開します。

アラーム: エラー メッセージを取得する、または、チュートリアルを通過するとおりに機能しない、確認して、[トラブルシューティングのページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)です。

## <a name="adding-a-new-column-to-a-table"></a>テーブルに新しい列を追加します。

このセクションで、生年月日に列を追加する、`Person`の基本クラス、`Student`と`Instructor`エンティティです。 新しい列を表示するように、インストラクター データを表示するページを更新します。

*ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (存在する必要があります終わり波かっこの後に続く 2)。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

次に、新しい列の値を提供できるようにシード メソッドを更新します。 開いている*Migrations\Configuration.cs*が開始されるコード ブロックを置き換える`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

ContosoUniversity プロジェクトで開きます*Instructors.aspx*生年月日を表示する新しいテンプレート フィールドを追加します。 雇用日とオフィス割り当てのためのものの間に追加します。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(場合はコードのインデントがとれていない状態を取得することができますキーを押して CTRL K し CTRL-D ファイルを自動的に書式設定を変更します。)

ソリューションをビルドし、開きます、 **Package Manager Console**ウィンドウです。 ContosoUniversity.DAL として選択されていることを確認してください、**既定のプロジェクト**です。

**Package Manager Console**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

このコマンドが完了すると、Visual Studio は新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`方法、新しい列を作成するコードを確認することができます。

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

ソリューションをビルドしに次のコマンドを入力、 **Package Manager Console**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

コマンドが完了したら、アプリケーションを実行し、インストラクター ページを選択します。 ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールドです。

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>データベースの更新をテスト環境に展開します。

**ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。

**Web 1 つをクリックして 発行**ツールバーで、**テスト**発行プロファイルをクリックして**Web の発行**です。 (ツールバーを無効にした場合で ContosoUniversity プロジェクトを選択**ソリューション エクスプ ローラー**)。

Visual Studio は、更新済みのアプリケーションを配置し、ホーム ページにブラウザーを開きます。 更新プログラムが正常に展開されたことを確認するインストラクター ページを実行します。 Code First、アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッドです。 予期される表示ページが表示されたら、**生年月日**で日付を含む列。

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>データベースの更新を実稼働環境に展開します。

これで、実稼働環境に配置できます。 唯一の違いは、使用すること*アプリ\_offline.htm*サイトへのアクセスと変更を展開しているときに、データベースが更新を禁止します。 運用環境のデプロイには、次の手順を実行します。

- アップロード、*アプリ\_offline.htm*ファイルを実稼働サイトです。
- Visual Studio で、プロファイルを選択して、実稼働環境で、 **Web 1 つをクリックして 発行**ツールバーとクリック**Web の発行**です。
- 削除、*アプリ\_offline.htm*実稼働サイトからのファイルです。

> [!NOTE]
> アプリケーションが、実稼働環境で使用するときは、バックアップ計画を実装する必要があります。 つまり、する必要がある定期的にコピーして、*学校 Prod.sdf*と*aspnet Prod.sdf*セキュリティで保護された記憶域の場所にサイトを運用環境からのファイルと、このようないくつかの世代を保存する必要がありますバックアップします。 データベースを更新するときに、変更の直前からバックアップ コピーを作成する必要があります。 次に、更新を間違えたして実稼働環境に配置した後まで、検出されない場合もことができますが破損する前に、の状態にデータベースを回復します。


Visual Studio は、ブラウザーで、ホーム ページの URL を開くときに、*アプリ\_offline.htm*ページが表示されます。 削除した後、*アプリ\_offline.htm*ファイル、更新プログラムが正常に展開されたことを確認するには、もう一度、ホーム ページを閲覧することができます。

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

テストと実稼働環境の両方にデータベースの変更が含まれているアプリケーションの更新プログラムを展開したようになりました。 次のチュートリアルでは、SQL Server Compact から SQL Server Express と SQL Server にデータベースを移行する方法を示します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [次へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
