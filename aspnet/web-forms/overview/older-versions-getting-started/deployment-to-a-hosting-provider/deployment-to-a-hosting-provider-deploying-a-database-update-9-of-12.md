---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベース更新の展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズには、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています.
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5edb3cae785d578674f47f1aa3d5be7dfa6a8791
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822096"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: 12 の 9 - データベース更新の展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行) ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC を for Web を使用して、SQL Server Compact データベースが含まれています。 Web の発行の更新をインストールする場合は、Visual Studio 2010 を使用することもできます。 シリーズの概要については、次を参照してください。[シリーズの最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)します。
> 
> Visual Studio 2012 RC のリリース後に導入された展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションをデプロイする方法を示しています、および Azure App Service Web Apps にデプロイする方法を示していますチュートリアルでは、次を参照してください。 [ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルで行ったデータベースの変更と関連するコードの変更、Visual Studio での変更をテストし、テストと運用環境の両方の環境に更新を展開します。

リマインダー: エラー メッセージを取得する、または、チュートリアルを進めるときに機能しないを必ず確認、[トラブルシューティング ページ](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)します。

## <a name="adding-a-new-column-to-a-table"></a>テーブルに新しい列の追加

このセクションに生年月日の列を追加、`Person`の基本クラス、`Student`と`Instructor`エンティティ。 新しい列を表示するように、インストラクター データを表示するページを更新します。

*ContosoUniversity.DAL*プロジェクトを開き、 *Person.cs*の末尾に次のプロパティを追加し、`Person`クラス (あります終わり波かっこの後に 2 つ)。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

次に、新しい列の値を提供するように、Seed メソッドを更新します。 開いている*migrations \configuration.cs*と置換を開始するコード ブロック`var instructors = new List<Instructor>`誕生日に関する情報を含む次のコード ブロックで。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

ContosoUniversity プロジェクトで開きます*Instructors.aspx*し、誕生日を表示する新しいテンプレート フィールドを追加します。 雇用日とオフィスの割り当てのためのものの間に追加します。

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(コードのインデントとれていない場合は、することができますキーを押して CTRL + K し CTRL-D ファイルを自動的に再フォーマットします。)

ソリューションをビルドしを開き、**パッケージ マネージャー コンソール**ウィンドウ。 として ContosoUniversity.DAL がまだ選択されていることを確認、**既定のプロジェクト**します。

**パッケージ マネージャー コンソール**ウィンドウで、 **ContosoUniversity.DAL**として、**既定のプロジェクト**、次のコマンドを入力します。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

このコマンドが完了したら、Visual Studio は、新しいを定義するクラス ファイルを開きます`DbMIgration`クラス、および、`Up`メソッド、新しい列を作成するコードを表示できます。

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

ソリューションをビルドおよびでは、次のコマンドを入力し、**パッケージ マネージャー コンソール**ウィンドウ (ContosoUniversity.DAL プロジェクトが選択されていることを確認してください)。

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

コマンドが完了したら、アプリケーションを実行し、Instructors ページを選択します。 ページが読み込まれると、新しいがあるを参照してください。 生年月日フィールド。

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>データベースの更新をテスト環境に展開します。

**ソリューション エクスプ ローラー** ContosoUniversity プロジェクトを選択します。

**Web の 1 クリックして発行**ツールバーで、**テスト**発行プロファイルをクリックして**Web の発行**します。 (ツールバーが無効になっている場合で ContosoUniversity プロジェクトを選択します**ソリューション エクスプ ローラー**。)。

Visual Studio が更新されたアプリケーションを配置し、ホーム ページにブラウザーを開きます。 更新プログラムが正常にデプロイされたことを確認するには、Instructors ページを実行します。 Code First アプリケーションは、このページのデータベースにアクセスする際に、データベース スキーマを更新して実行、`Seed`メソッド。 予期される確認ページが表示されたら、**生年月日**で日付を含む列。

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>運用環境にデータベースの更新を展開します。

運用環境に展開できます。 唯一の違いは、使用する*アプリ\_offline.htm*ユーザー サイトへのアクセスと変更をデプロイするときに、データベースが更新できないようにします。 運用環境のデプロイでは、次の手順を実行します。

- アップロード、*アプリ\_offline.htm*ファイルを実稼働サイト。
- Visual Studio での実稼働プロファイルを選択、 **Web の 1 クリックして発行**ツールバーとクリック**Web の発行**します。
- 削除、*アプリ\_offline.htm*運用サイトからのファイル。

> [!NOTE]
> アプリケーションを運用環境で使用中には、バックアップ計画を実装する必要があります。 つまり、する必要がありますが定期的にコピーして、*の学校 Prod.sdf*と*aspnet Prod.sdf*運用環境からファイルをセキュリティで保護された記憶域の場所、サイトし、このようないくつかの世代を保存する必要がありますバックアップします。 データベースを更新するときに、変更の直前のバックアップ コピーをする必要があります。 次に、設定を間違えたして運用環境に配置した後に検出されるまでそのしない場合が破損する前に、の状態にデータベースを復旧できます。


Visual Studio がブラウザーで開いたら、ホーム ページの URL、*アプリ\_offline.htm*ページが表示されます。 削除した後、*アプリ\_offline.htm*ファイル、更新プログラムが正常にデプロイされたことを確認するには、もう一度、ホーム ページを参照することができます。

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

これで、テストと運用環境の両方にデータベースの変更を含むアプリケーションの更新プログラムをデプロイしました。 次のチュートリアルでは、SQL Server Compact から SQL Server Express と SQL Server にデータベースを移行する方法を示します。

> [!div class="step-by-step"]
> [前へ](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [次へ](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
