---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC データベースの最初のサイトを Azure に発行 |Microsoft ドキュメント
author: tfitzmac
description: MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの seri しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869960"
---
<a name="publish-mvc-database-first-site-to-azure"></a>MVC データベースの最初のサイトを Azure に発行します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework と ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの系列では、自動的にユーザーを表示、編集、作成するにようにコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブルの列に対応します。
> 
> 系列のこの部分は、web アプリとデータベースを Azure に発行について説明します。 実際に手順を実行する、チュートリアルの先頭から開始する必要がありますが、web アプリとデータベースの発行に関する情報については、このトピックを読み取ることができます。 参照してください[作業の開始](setting-up-database.md)です。


## <a name="deploy-your-web-app-on-azure"></a>Azure で web アプリを配置します。

Azure アカウントをこのチュートリアルを完了する必要があります。

- 実行できます[無料の Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-クレジットを取得するでも使用されているアカウントを維持する最大と使用する無料の Azure サービスおよび有料の Azure サービスを試す使用できます。
- 実行できます[MSDN サブスクライバー特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-、MSDN サブスクリプションでは、クレジット有料の Azure サービスを使用できるすべての月です。

Web アプリを発行するには、プロジェクトを右クリックして**発行**です。

![サイトを発行します。](publish-to-azure/_static/image1.png)

Microsoft Azure web サイトを選択します。

![Azure を選択します](publish-to-azure/_static/image2.png)

Azure にサインインしていない場合は、Azure アカウント資格情報を指定します。 新しい web アプリを作成する新規をクリックします。

![新しいサイト](publish-to-azure/_static/image3.png)

Web アプリの一意の名前を作成します。 わかります名前が一意では、name フィールドの右側に緑のチェック マークが表示される場合。 Web アプリ用の領域を選択します。 選択**を作成する新しいサーバー**データベースの新しいデータベース サーバーのユーザー名とパスワードを指定します。 終了したら、クリックして**作成**です。

![サイトを作成します。](publish-to-azure/_static/image4.png)

接続の値がすべて設定されています。 これらの値をそのままにしておくことができます。

![connection](publish-to-azure/_static/image5.png)

**[次へ]** をクリックします。

設定については、2 つのデータベース接続であることに注意してくださいが指定された - ApplicationDBContext と ContosoUniversityDataEntities です。 ApplicationDBContext は、ユーザー アカウントのテーブルの接続です。 これらの値は、データベースの接続文字列のみを表示します。 サイトを発行するときに、これらのデータベースを公開することがありません。 Web アプリを発行した後は、データベース プロジェクトを発行します。

![](publish-to-azure/_static/image6.png)

データベース接続の横にある省略記号 (...) では、接続文字列の詳細を表示します。 ContosoUniversityDataEntities の横にある省略記号ボタンをクリックします。

![](publish-to-azure/_static/image7.png)

データベース サーバーとデータベースの名前に注意してください。 サーバー名がランダムに生成されます。 データベース名は単純なを使用してサイトの名前 **\_db**追加されます。 データベースを発行するときに、両方の名前を後で必要があります。

をクリックして**OK**データベース接続文字列 ウィンドウを閉じます。

[Web の発行] ウィンドウ**次**プレビューを表示します。

![](publish-to-azure/_static/image8.png)

発行するファイルの一覧を表示するプレビューの開始をクリックすることができます。 これはこのサイトを発行した最初の時間であるため、プロジェクト内のすべての関連するファイルは表示されません。

**[発行]** をクリックします。

[出力] ペインのパブリケーションには、結果が表示されます。

![](publish-to-azure/_static/image9.png)

発行した後は、サイトは、web ブラウザーで起動 immedialely がします。 サイトが展開され、サイトに新しいユーザーを登録することができます。ただし、ContosoUniversityData プロジェクトにテーブルがまだ発行されていません。 学生のリンクの一覧をクリックすると、エラーが表示されます。

## <a name="publish-database-to-sql-azure"></a>データベースを SQL Azure に発行します。

データベースを発行する前に、ローカル コンピューターが、データベース サーバーに接続できることを確認しての操作を行う必要があります。 データベース サーバーのファイアウォールは、マシンが、データベースに接続可能を制限します。 ファイアウォールの許可された IP アドレスに、コンピューターの IP アドレスを追加する必要があります。

Azure ポータルで Azure アカウントにログインします。

新しいデータベースを選択し、選択**管理**です。

![管理します。](publish-to-azure/_static/image10.png)

お使いのコンピューターから接続を許可するようにデータベース サーバーを構成する必要があります。 管理を選択すると、データベース サーバーに許可されている現在の IP アドレスを追加する必要があります。 [はい] を選択します。

![ip アドレスを追加します。](publish-to-azure/_static/image11.png)

前の手順で追加した IP アドレスは、接続用に構成する必要。 唯一の IP アドレスではない可能性があります。 接続を正しく設定されているかどうかをデータベースにログインしようとすることができます。 以前に作成したパスワードとユーザーを入力します。

![ログイン](publish-to-azure/_static/image12.png)

エラー メッセージを受信する場合は、別の IP アドレスを追加する必要があります。 エラーに関する詳細を表示するエラー メッセージをクリックします。 詳細情報を追加する必要のある IP アドレスが表示されます。 この IP アドレスに注意してください。

![許可されていません](publish-to-azure/_static/image13.png)

このログイン ウィンドウを閉じて、Azure ポータルに戻ります。

データベースのダッシュ ボードに移動します。 をクリックして**使用できる IP アドレス管理**です。

![ip アドレスを管理します。](publish-to-azure/_static/image14.png)

エラー メッセージから IP アドレスを追加する必要があります。 エラー メッセージから 1 つを含めるに許可される IP アドレスの範囲を変更するか、別のエントリとしてその IP アドレスを追加します。

![新しいアドレスを追加します。](publish-to-azure/_static/image15.png)

許可される IP アドレスに変更を保存します。

[管理] をクリックして、データベースにログインを再試行してください。 許可される IP アドレスが正しく構成されて、ファイアウォールの前に、数分を待機する必要があります。 データベースが正常にログインすることができます、ときに、データベースへの接続の設定が完了しました。

ことができますこの管理ウィンドウを開いたまま、データベースの配置の結果を間もなくチェックされますが、します。

データベース プロジェクトに戻ります。 プロジェクトを右クリックし **発行**です。

![データベースを発行します。](publish-to-azure/_static/image16.png)

データベースの公開 ウィンドウで選択**編集**です。

![編集](publish-to-azure/_static/image17.png)

サーバーのデータベース サーバーと、認証資格情報の名前を指定します。 資格情報を提供すると、利用可能なデータベースの一覧から作成したデータベースを選択します。 既定は、Visual Studio は、データベース フィールドの名前を作成したデータベースと同じである可能性がありますいないプロジェクトの名前を設定します。

![](publish-to-azure/_static/image18.png)

[OK] をクリックします。

今後の更新プログラムを公開するには、すべての接続情報を再度入力せずにこのプロファイルを保存するが可能性があります。 **[プロファイルの作成]** を選択します。

![プロファイルを保存します。](publish-to-azure/_static/image19.png)

同じ名前のプロジェクトで、ファイルが作成されます**ContosoUniversityData.publish.xml**です。 次回データベースを Azure に発行するには、そのプロファイルを単に読み込みます。

ここで、をクリックして**発行**を Azure にデータベースを作成します。

![publish](publish-to-azure/_static/image20.png)

実行後、しばらくの間実行結果を発行が表示されます。

![results](publish-to-azure/_static/image21.png)

ここで、する戻ることができます、管理ポータルに、データベースにします。 [デザイン] ビューを更新し、3 つのテーブルに自動的に入力データが配置されていることを確認します。

![新しいテーブル](publish-to-azure/_static/image22.png)

Azure に展開されている web アプリをテストする準備が整いました。 Azure で web アプリに移動 (などhttp://contosositeexample.azurewebsites.net/)です。 学生のリストのリンクをクリックし、受講者のインデックス ビューを表示する必要があります。

![ビュー](publish-to-azure/_static/image23.png)

場合によっては、データベースとの接続は、適切に構成するのには少し時間を必要があります。 エラーが発生した場合、数分待ってからもう一度やり直してください。

## <a name="conclusion"></a>まとめ

この系列には、ユーザーが編集、更新、作成、およびデータを削除するように既存のデータベースからコードを生成する方法の簡単な例が用意されています。 これにより、ASP.NET MVC 5 の場合、Entity Framework と ASP.NET のスキャフォールディングがプロジェクトの作成に使用されます。

Code First の開発の導入例は、次を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)です。

高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションを Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 DbContext API を使用するデータベースを最初にデータを扱うのために、Code First でのデータを操作するために使用する API と同じことに注意してください。 Database First を使用する場合でも、コードの最初のチュートリアルからなど、同時実行の競合を処理、読み取りと、関連するデータの更新などのより複雑なシナリオを処理する方法を確認できます。 唯一の違いは、データベース、コンテキスト クラス、およびエンティティ クラスを作成する方法です。

> [!div class="step-by-step"]
> [前へ](enhancing-data-validation.md)
