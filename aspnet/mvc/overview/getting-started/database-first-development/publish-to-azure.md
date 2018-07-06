---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Azure Database First の MVC サイトを発行 |Microsoft Docs
author: tfitzmac
description: MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアルの化しています.
ms.author: aspnetcontent
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 0aaa8e2a586a89f6ea5eaeb4f3d280993342b2f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835749"
---
<a name="publish-mvc-database-first-site-to-azure"></a>Azure Database First の MVC サイトを発行します。
====================
によって[Tom FitzMacken](https://github.com/tfitzmac)

> MVC、Entity Framework、および ASP.NET のスキャフォールディングを使用して、既存のデータベースへのインターフェイスを提供する web アプリケーションを作成することができます。 このチュートリアル シリーズでは、自動的に表示、編集、作成、ユーザーを有効にするコードを生成し、データベース テーブルに存在するデータを削除する方法を示します。 生成されたコードは、データベース テーブル内の列に対応します。
> 
> シリーズのこの部分は、web アプリとデータベースを Azure に発行について説明します。 Web アプリとデータベースを発行する方法について説明しますが、実際には、チュートリアルの先頭から開始する必要がありますの手順を実行する、このトピックを読むことができます。 参照してください[Getting Started](setting-up-database.md)します。


## <a name="deploy-your-web-app-on-azure"></a>Azure で web アプリをデプロイします。

Azure アカウントに、このチュートリアルを完了する必要があります。

- できます[無料 Azure アカウントを開く](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-クレジットを取得する有料の Azure サービスを使用することができますをも使用されるアカウントは維持する最大無料の Azure サービスを使用します。
- できます[MSDN サブスクライバーの特典をアクティブ化](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-MSDN サブスクリプションでは、クレジットの有料の Azure サービスを使用することができる毎月。

Web アプリを発行するには、プロジェクトを右クリックして**発行**します。

![サイトを発行します。](publish-to-azure/_static/image1.png)

Microsoft Azure の web サイトを選択します。

![Azure を選択します。](publish-to-azure/_static/image2.png)

Azure にサインインしていない場合は、Azure アカウントの資格情報を提供します。 次に、新しい web アプリを作成する新規を選択します。

![新しいサイト](publish-to-azure/_static/image3.png)

Web アプリの一意の名前を作成します。 [名前] フィールドの右側に緑色のチェック マークが表示される場合、名前は一意ですがするわかります。 Web アプリのリージョンを選択します。 選択**新しいサーバーの作成**データベースのこの新しいデータベース サーバーのユーザー名とパスワードを指定します。 完了したら、クリックして**作成**です。

![サイトを作成します。](publish-to-azure/_static/image4.png)

接続の値がすべて設定されました。 これらの値をそのままにしておくことができます。

![connection](publish-to-azure/_static/image5.png)

**[次へ]** をクリックします。

設定については、通知の 2 つのデータベース接続が指定された - ApplicationDBContext と ContosoUniversityDataEntities します。 ApplicationDBContext は、ユーザー アカウントのテーブルの接続です。 これらの値は、データベースの接続文字列のみを表示します。 サイトを発行するときに、これらのデータベースを公開することは限りません。 Web アプリの発行が完了した後、データベース プロジェクトが発行されます。

![](publish-to-azure/_static/image6.png)

データベース接続の横にある省略記号 (...) では、接続文字列の詳細を表示します。 ContosoUniversityDataEntities の横にある省略記号をクリックします。

![](publish-to-azure/_static/image7.png)

データベース サーバーとデータベースの名前に注意してください。 サーバー名がランダムに生成されます。 データベース名は単純なを使用してサイトの名前 **\_db**追加されます。 データベースを発行するときに、両方の名前を後で必要があります。

クリックして**OK**データベース接続文字列 ウィンドウを閉じます。

Web の発行] ウィンドウで、[**次**プレビューを表示します。

![](publish-to-azure/_static/image8.png)

発行するファイルの一覧を表示するには、プレビューの開始をクリックすることができます。 これは、このサイトを発行した最初の時間であるために、一覧は、すべての関連するプロジェクト ファイルになります。

**[発行]** をクリックします。

[出力] ウィンドウ、パブリケーションの結果が表示されます。

![](publish-to-azure/_static/image9.png)

発行した後は、サイトは、web ブラウザーで起動 immedialely が。 サイトがデプロイされ、サイトに新しいユーザーを登録することができます。ただし、ContosoUniversityData プロジェクトにテーブルがまだ公開されていません。 受講者のリンクの一覧をクリックすると、エラーが表示されます。

## <a name="publish-database-to-sql-azure"></a>データベースを SQL Azure に発行します。

データベースを発行する前に、ローカル コンピューターは、データベース サーバーに接続できることを確認してください。 データベース サーバーのファイアウォールは、マシンがデータベースに接続可能を制限します。 ファイアウォールの許可された IP アドレスに、コンピューターの IP アドレスを追加する必要があります。

Azure portal で Azure アカウントにログインします。

新しいデータベースを選択し、選択**管理**します。

![管理します。](publish-to-azure/_static/image10.png)

お使いのコンピューターから接続を許可するデータベース サーバーを構成する必要があります。 管理 を選択すると、データベース サーバーに許可されているように、現在の IP アドレスを追加するように求められます。 [はい] を選択します。

![ip アドレスを追加します。](publish-to-azure/_static/image11.png)

前の手順で追加した IP アドレスは、接続用に構成する必要がある唯一の IP アドレスではない可能性があります。 接続を正しく設定されているかどうかをデータベースにログインしようとすることができます。 ユーザーと以前に作成したパスワードを入力します。

![ログイン](publish-to-azure/_static/image12.png)

エラー メッセージを受信する場合は、別の IP アドレスを追加する必要があります。 エラーに関する詳細を表示するエラー メッセージをクリックします。 詳細を追加する必要のある IP アドレスが表示されます。 この IP アドレスに注意してください。

![許可されていません](publish-to-azure/_static/image13.png)

このログイン ウィンドウを閉じるし、Azure portal に戻ります。

データベースのダッシュ ボードに移動します。 クリックして**使用できる IP アドレス管理**します。

![ip アドレスを管理します。](publish-to-azure/_static/image14.png)

エラー メッセージから IP アドレスを追加する必要がありますようになりました。 含める 1 つのエラー メッセージから許可された IP アドレスの範囲を変更するか、個別のエントリとしてその IP アドレスを追加します。

![新しいアドレスを追加します。](publish-to-azure/_static/image15.png)

許可された IP アドレスに変更を保存します。

管理、 をクリックして、データベースにログインを再試行してください。 許可された IP アドレスが正しく構成されて、ファイアウォールの前に数分待機する必要があります。 データベースに正常にログインすることができます、ときに、データベースへの接続の設定が完了しました。

おくことができます 管理ウィンドウでこの開いてすぐデータベース デプロイの結果は確認のためです。

データベース プロジェクトに戻ります。 プロジェクトを右クリックして**発行**します。

![データベースを発行します。](publish-to-azure/_static/image16.png)

データベースの公開 ウィンドウで次のように選択します。**編集**します。

![編集](publish-to-azure/_static/image17.png)

サーバーのデータベース サーバーと、認証資格情報の名前を指定します。 資格情報を提供するには、利用可能なデータベースの一覧から作成したデータベースを選択します。 既定では、Visual Studio は、作成したデータベースと同じである可能性がありますいないプロジェクトの名前をデータベース フィールドの名前を設定します。

![](publish-to-azure/_static/image18.png)

[OK] をクリックします。

すべての接続情報を再入力しなくても、今後の更新プログラムを公開できるように、このプロファイルを保存するが可能性があります。 **[プロファイルの作成]** を選択します。

![プロファイルを保存します。](publish-to-azure/_static/image19.png)

同じ名前のプロジェクトにファイルが作成されます**ContosoUniversityData.publish.xml**します。 次回データベースを Azure に発行するは、単にそのプロファイルを読み込みます。

をクリックして**発行**を Azure にデータベースを作成します。

![publish](publish-to-azure/_static/image20.png)

実行後、しばらくの間の実行結果の発行が表示されます。

![results](publish-to-azure/_static/image21.png)

ここで、することができますに戻り、管理ポータル、データベースの。 デザイン ビューを更新し、3 つのテーブルに自動的に入力データが配置されていることを確認します。

![新しいテーブル](publish-to-azure/_static/image22.png)

Azure にデプロイされている web アプリをテストする準備が整いました。 Azure で web アプリに移動します (などhttp://contosositeexample.azurewebsites.net/)します。 受講者の一覧のリンクをクリックし、受講者の index ビューを表示する必要があります。

![ビュー](publish-to-azure/_static/image23.png)

場合によっては、データベースと接続は、適切に構成するのには、少し時間を必要があります。 エラーが発生した場合数分待ってから、もう一度やり直してください。

## <a name="conclusion"></a>まとめ

このシリーズでは、ユーザーを編集、更新、作成、およびデータを削除できるように既存のデータベースからコードを生成する方法の簡単な例が用意されています。 ASP.NET MVC 5、Entity Framework および ASP.NET スキャフォールディングは、プロジェクトの作成に使用されます。

Code First の開発の基本的な例を参照してください。 [ASP.NET MVC 5 の概要](../introduction/getting-started.md)します。

高度な例では、次を参照してください。 [ASP.NET MVC 4 アプリケーションの Entity Framework データ モデルを作成する](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 Database First のデータ処理に使用する DbContext API は Code First のデータを操作するために使用する API と同じことに注意してください。 Database First を使用する場合でも、コードの最初のチュートリアルからなど、同時実行の競合を処理、読み取りと、関連するデータの更新などのより複雑なシナリオを処理する方法を学習できます。 唯一の違いは、データベース、コンテキストのクラスおよびエンティティ クラスを作成する方法には。

> [!div class="step-by-step"]
> [前へ](enhancing-data-validation.md)
