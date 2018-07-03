---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: データ ストレージ オプション (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 43e7a03f3bebcca820452ea10e13459d8d275b5a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363508"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>データ ストレージ オプション (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


ほとんどの人は、リレーショナル データベースに使用され、クラウド アプリを設計するときにその他のデータ ストレージ オプションは見落とされがちになります。 結果は、そのため、最適なパフォーマンス、高い費用は、または最悪の場合があります[NoSQL](http://en.wikipedia.org/wiki/NoSQL) (非リレーショナル) データベースでは、いくつかのタスクをリレーショナル データベースよりも効率的に処理できます。 お客様は、重要なデータ ストレージの問題を解決するための尋ね、ときに多くの場合、NoSQL オプションのいずれかならば、強化、リレーショナル データベースがあるため。 そのような状況で、顧客がでした方場合は、運用環境にアプリを展開する前に、NoSQL ソリューションを実装する必要があります。

その一方で、想定、NoSQL データベース何でもできる、または十分に間違いがあります。 すべてのデータ ストレージ タスクの最適なデータ管理選択肢を 1 つはありません。別のデータ管理ソリューションは、さまざまなタスク用に最適化されています。 ほとんどの現実世界のクラウド アプリでは、さまざまなデータ ストレージの要件がありますされ、複数のデータ ストレージ ソリューションの組み合わせで最適な多くの場合、処理されます。

この章では、クラウド アプリとに自分のシナリオに適したものを選択する方法に関する基本的なガイダンスを利用できるデータ ストレージ オプションの幅広いを把握できるようにします。 使用可能なオプションを認識してアプリケーションを開発する前に、それぞれの長所と短所について考えることをお勧めします。 実稼働アプリでのデータ ストレージ オプションを変更する飛行機のフライト中には、jet エンジンを変更することのように、非常に困難です。

## <a name="data-storage-options-on-azure"></a>Azure でのデータ ストレージ オプション

クラウドでは、比較的簡単に、さまざまなリレーショナルと NoSQL データ ストアを使用できます。 Azure で使用できるデータのストレージ プラットフォームの一部を示します。

![](data-storage-options/_static/image1.png)

表には、次の 4 つの種類の NoSQL データベースを示します。

- [キー/値データベース](https://msdn.microsoft.com/library/dn313285.aspx#sec7)キー値ごとに 1 つのシリアル化されたオブジェクトを格納します。 大量の場所を特定のキー値の 1 つの項目を取得してがないデータを格納する場合に適していますが、項目の他のプロパティに基づくクエリをします。

    [Azure Blob storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/)はフォルダーとファイル名に対応するキーの値を持つ、クラウド内のファイル ストレージのように機能するキー/値データベースです。 フォルダーとファイル名で、ファイルの内容の値を検索ではなく、ファイルを取得します。

    [Azure Table storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)キー/値データベースになります。 各値と呼ばれる、*エンティティ*(パーティション キーと行キーで識別される、行に似ています) が複数には含まれています*プロパティ*(列のようなテーブル内のすべてのエンティティが同じを共有する必要しますが、列の場合)。 キー以外の列に対してクエリを実行して、非常に効率的ではありません、避ける必要があります。 たとえば、1 人のユーザーに関する情報を格納する 1 つのパーティションを持つユーザーのプロファイル データを格納できます。 1 つのエンティティのプロパティを個別にまたは、同じパーティションに個別のエンティティでは、ユーザー名やパスワードのハッシュ、生年月日などのデータを格納できます。 指定の生年月日の日付の範囲を持つすべてのユーザーに対してクエリを実行したくし、プロファイル テーブルと別のテーブル間の結合クエリを実行することはできません。 テーブル ストレージはよりスケーラブルとリレーショナル データベースより安価ですが、複雑なクエリや結合は有効になりません。
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8)値のキー/値データベース*ドキュメント*します。 「ドキュメント」は、ここでは、Word または Excel ドキュメントの意味で使用されていないが、名前付きフィールドと値、子ドキュメントをあるうちのいずれかのコレクションのことを意味します。 たとえば、注文の履歴テーブルに注文ドキュメントがあります注文番号、注文日、および顧客フィールド顧客フィールドには、名前と住所のフィールドが存在します。 データベースは、XML、YAML、JSON、BSON; などの形式でフィールドのデータをエンコードします。または、プレーン テキストを使用できます。 キー/値データベースとは別のドキュメント データベースを設定する機能の 1 つは、非キー フィールドに対してクエリを実行し、クエリを効率的にするためにセカンダリ インデックスを定義する機能です。 この機能により、ドキュメント データベースのドキュメント キーの値よりもさらに複雑な条件に基づいてデータを取得する必要があるアプリケーションに適してします。 たとえば、販売注文履歴のドキュメント データベースは、製品 ID、顧客 ID、顧客名などのさまざまなフィールドでクエリでした。 [MongoDB](http://www.mongodb.org/)は一般的なドキュメント データベースです。
- [列ファミリ データベース](https://msdn.microsoft.com/library/dn313285.aspx#sec9)キー/値の列ファミリと呼ばれる関連する列のコレクションに構造化データ ストレージを有効にするデータ ストアが。 たとえば、国勢調査データベースにユーザーの名前の列の 1 つのグループがある (最初に、ミドル ネーム、姓)、個人のアドレスでは、1 つのグループとユーザーのプロファイル情報 (DOB、性別など) の 1 つのグループ。 データベースは各列ファミリをすべての同じキーに関連する 1 人のユーザーのデータを維持しながら別のパーティションに格納し、ことができます。 すべての名前と住所の情報も読み取る必要がないすべてのプロファイル情報を確認できます。 [Cassandra](http://cassandra.apache.org/)は人気のある列ファミリ データベースです。
- [グラフ データベースを](https://msdn.microsoft.com/library/dn313285.aspx#sec10)オブジェクトとリレーションシップのコレクションとして情報を格納します。 グラフ データベースの目的は、それらの間のオブジェクトとリレーションシップのネットワークを通過するクエリを効率的に実行するアプリケーションを有効にすること。 たとえば、オブジェクトには、人事データベース内の従業員が可能性がありますを見つけてたい場合がありますを容易にするクエリなど"全従業員を直接的または間接的に Scott" [Neo4j](http://www.neo4j.org/)は人気のあるグラフ データベースです。

リレーショナル データベースと比較して、NoSQL オプションでは、はるかに優れたスケーラビリティと非構造化データの格納および分析の費用対効果を提供します。 トレードオフは、豊富な queryability とリレーショナル データベースの信頼性の高いデータの整合性機能を提供することはありません。 NoSQL が適しています IIS ログ データは、結合クエリの不要な高ボリュームが含まれます。 NoSQL が使用できないようにも銀行取引を絶対データの整合性が必要ですし、その他のアカウントに関連するデータを多数のリレーションシップが含まれます。

呼ばれるデータベース プラットフォームの新しいカテゴリがある[また](http://en.wikipedia.org/wiki/NewSQL)queryability およびリレーショナル データベースのトランザクションの整合性と NoSQL データベースのスケーラビリティを組み合わせてです。 分散ストレージとクエリの処理は、"OldSQL"データベースに実装するは難しいは、新規データベースが設計されています。 [NuoDB](http://www.nuodb.com/) Azure で使用できる新規データベースの例を示します。

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop と MapReduce

大量の NoSQL データベースに格納できるデータは、適切なタイミングで効率的に分析にくい場合があります。 などのフレームワークを使用することを行う[Hadoop](http://hadoop.apache.org/)実装[MapReduce](http://en.wikipedia.org/wiki/MapReduce)機能します。 基本的にどのような MapReduce プロセスでは、次に示します。

- 制限は、データを選択して処理する必要があるデータのサイズが実際に分析する必要があるデータだけを格納します。 たとえば、ユーザー プロファイルのデータ ストアから誕生年のみを選択するために、ユーザーが生まれた年ベースの構成を知りたいです。
- 部分にデータを分割し、処理のための別のコンピューターに送信します。 コンピューター A は 1950 1959 日付を持つユーザーの数を計算しますが、コンピューター B では 1960 1969 年など。このコンピューター グループと呼ばれる、 *Hadoop クラスター*します。
- 結果を置く各部分のまとめ部分に処理が完了後します。 各生年月日年の数人の比較的短い一覧があるようになりましたし、この全体の一覧でパーセンテージを計算するタスクは管理しやすい。

Azure では、 [HDInsight](https://azure.microsoft.com/services/hdinsight/)すると、処理、分析、および Hadoop の電源を使用して big data から新しい洞察を得ることができます。 たとえば、web サーバー ログを分析に使用できます。

- ストレージ アカウントに、web サーバーのログ記録を有効にします。 これは、ログをアプリケーションに HTTP 要求ごとに Blob Service に書き込む Azure を設定します。 Blob Service は、基本的に、クラウド ファイル ストレージと HDInsight を適切に統合されています。 

    ![Blob Storage にログ](data-storage-options/_static/image2.png)
- アプリがトラフィックと、web サーバーの IIS ログは、Blob storage に書き込まれます。 

    ![Web サーバーのログ](data-storage-options/_static/image3.png)
- ポータルで、次のようにクリックします**新規** - **Data Services** - **HDInsight** - **簡易作成**、。HDInsight クラスター名、クラスターのサイズ (HDInsight クラスターのデータ ノード数)、およびユーザー名と、HDInsight クラスターのパスワードを指定します。 

    ![HDInsight](data-storage-options/_static/image4.png)

これで、ログを分析しなどの質問に対する回答を取得する MapReduce ジョブを設定できます。

- 1 日の時間はどのようなアプリで、最大股は最小のトラフィックを取得しますか。
- どの国では、自分からのトラフィックですか。
- 私のトラフィックに由来する領域の平均近所収入とは (パブリック IP アドレスによって、近所の収入を提供するデータセットがあるし、に対して web サーバーのログ内の IP アドレスと一致することができます)。
- 近所の収入と特定のページまたはサイトの製品の関連付け方法でしょうか。

顧客は、関心がある可能性に応じてまたは特定の製品を購入する可能性が広告の対象に同様の質問に対する回答を使用できます。

説明したように、[すべて自動化章](automate-everything.md)ポータルで行うことができます関数の多くを自動化すること、およびを設定して、HDInsight の分析ジョブの実行が含まれます。 一般的な HDInsight スクリプトには、次の手順が含まれます。

- HDInsight クラスターをプロビジョニングし、Blob storage の入力のストレージ アカウントにリンクします。
- HDInsight クラスターに MapReduce ジョブの実行可能ファイル (.jar または .exe ファイル) をアップロードします。
- Blob storage に出力データを格納する MapReduce を送信します。
- ジョブが完了するまで待ちます。
- HDInsight クラスターを削除します。
- Blob storage から出力にアクセスします。

このすべてを実行するスクリプトを実行して、コストを最小限に抑えられます HDInsight クラスターがプロビジョニングされると、時間を最小化します。

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>As a Service (IaaS) インフラストラクチャとサービス (PaaS) としてのプラットフォーム

前に示したデータ ストレージ オプションには、プラットフォームとしてのサービス (PaaS) とインフラストラクチャとしてのサービス (IaaS) ソリューションの両方が含まれます。 PaaS では、ハードウェアとソフトウェアのインフラストラクチャを管理し、単にサービスを使用することを意味します。 SQL Database は、Azure の PaaS 機能です。 データベースを要求して、バック グラウンドで Azure を設定し、Vm の構成しそれらのデータベースを設定します。 Vm に直接アクセスできないし、それらを管理する必要はありません。IaaS では、セットアップ、構成、および、データ センター インフラストラクチャで実行する Vm を管理し、それらに必要な値を配置することを意味します。 一般的な VM 構成の事前構成済み VM イメージのギャラリーを提供しています。 たとえば、Windows Server 2008、Windows Server 2012、BizTalk Server、Oracle WebLogic Server、Oracle データベースなどの事前構成済み VM イメージをインストールできます。

Azure が提供する PaaS データ ソリューションは次のとおりです。

- Azure SQL Database は、(以前は SQL Azure と呼ばれます)。 SQL Server に基づく、クラウドのリレーショナル データベース。
- Azure Table storage。 キー/値の NoSQL データベース。
- Azure Blob storage。 クラウド内のファイル ストレージ。

Iaas、たとえば、VM 上に読み込むことができます何もを実行できます。

- SQL Server、Oracle、MySQL、SQL Compact、SQLite、または Postgres などのリレーショナル データベース。
- Memcached、Redis、Cassandra、および Riak など、キー/値データが格納されます。
- HBase などの列のデータを格納します。
- MongoDB、RavenDB、CouchDB などのドキュメント データベースです。
- Neo4j などのグラフ データベースです。

![Azure でのデータ ストレージ オプション](data-storage-options/_static/image5.png)

IaaS オプションは、ほぼ無制限のデータ ストレージのオプションを提供し、事前構成済みイメージを使用して Vm を作成するため、それらの多くは特に簡単に使用します。 たとえば、管理ポータルに移動**仮想マシン**、 をクリックして、**イメージ**タブをクリックし、をクリックして**VM Depot の参照**します。

![VM Depot を参照します。](data-storage-options/_static/image6.png)

一覧を確認し、[何百もの事前構成済み VM イメージ](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)、し、MongoDB、Neo4J、Redis、Cassandra、CouchDB など、プレインストールされているデータベース管理システムを含むイメージから VM を作成することができます。

![VM depot の MongoDB](data-storage-options/_static/image7.png)

Azure で IaaS データ ストレージ オプションは、使いやすくする、PaaS サービスはよりコスト効率に優れたおよび多くのシナリオで実用的な多くの利点を持っています。

- Vm を作成する必要はありません、するだけ、ポータルまたはスクリプトをデータ ストアのセットアップを使用します。 200 のテラバイトのデータ ストアにする場合とだけボタンをクリックします。 または、コマンドを実行できます (秒) が使用できるようになります。
- 管理やサービスで使用される Vm のパッチを適用する必要はありません。Microsoft ことができます-スケーリングまたは高可用性のインフラストラクチャのセットアップについて心配する必要はありません。Microsoft は、すべてを処理します。
- のライセンスを購入する必要はありませんサービスの料金には、ライセンス料が含まれます。
- 使用したユーザーだけ支払います。

Azure での PaaS データ ストレージ オプションには、サード パーティ プロバイダーによってソリューションが含まれます。 たとえば、選択、 [MongoLab アドオン](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/)サービスとしての MongoDB データベースをプロビジョニングする Azure ストアから。

## <a name="choosing-a-data-storage-option"></a>データ ストレージ オプションを選択します。

1 つの方法ではありませんは、すべてのシナリオのとおりです。 すべてのユーザーが、このテクノロジが、応答であるの場合は最初に確認することは「何が質問ですか?」、さまざまなソリューションはさまざまな処理用に最適化されたため。 リレーショナル モデルへの明確な利点があります。その理由は存在していたために長い時間。 ダウン側の NoSQL ソリューションで対処できる SQL にもあります。

多くの場合、ここに表示される内容最適な作業は合成的なアプローチでは、1 つのソリューションで SQL および NoSQL を使用する場所。 ときにユーザー言っても、採用 NoSQL、行ってする多くの場合、ドリルダウンする場合検索のいくつかのさまざまな NoSQL フレームワークを使用していること: を使用している[CouchDB](http://wiki.apache.org/couchdb/Introduction)、および[Redis](http://redis.io/)、および[Riak](http://basho.com/riak/)さまざまなものにします。 偶数の Facebook は、NoSQL を広範囲にわたって使用するは、サービスのさまざまな部分をさまざまな NoSQL フレームワークを使用します。 柔軟性を混在させると、データ記憶域のアプローチは、簡単に複数のデータ ソリューションを使用して、それらを 1 つのアプリに統合するために、クラウドのメリットのことの 1 つ。

アプローチを選択するときについて考慮するいくつかの質問を次に示します。

| セマンティック データ | -、中核となるデータ ストレージとデータ アクセスはセマンティック (がするデータを格納するリレーショナルまたは非構造化) ですか? メディア ファイルなどの非構造化データを blob ストレージに最適します。など、製品、インベントリ、サプライヤー、顧客の注文など、関連するデータのコレクションは、リレーショナル データベースに最も適合します。 |
| --- | --- |
| クエリのサポート | にデータを照会することは簡単する方法でしょうか。 -どのような種類の質問は、効率的によく寄せられるでしょうか。 キー/値データ ストアには、キーの値を指定した 1 つの行を取得するのには非常に良好なが適切で複雑なクエリはありませんが。 特定のユーザーが 1 つのデータを常に取得する場所、ユーザー プロファイル データ ストアでのキー/値のデータ ストアがも; 機能さまざまな製品の属性に基づいてさまざまなグループを取得する製品カタログのリレーショナル データベースはより適しています。 NoSQL データベースでは、大量のデータを効率的に格納できますが、アプリが、データを照会する方法の周囲のデータベース構造があるし、これにより、アドホック クエリは、実行が困難になります。 リレーショナル データベースでは、ほとんどの種類のクエリを構築できます。 |
| 機能のプロジェクション | -質問ことができます、集計などがサーバー側で実行されるでしょうか。 SELECT COUNT を実行する場合 (\*) sql テーブルから非常に効率よくサーバー上のすべての作業を行うし、数値を探してを返すことができます。 集計をサポートしていない、NoSQL データ ストアから同じ計算したい場合は、これは、非効率的な「バインド解除済みのクエリ」であり、タイムアウトになるのでしょう。クエリが成功した場合でも、クライアントにサーバーからすべてのデータを取得し、クライアント上の行をカウントする必要があります。 -どのような言語または式の型を使用できますか。 リレーショナル データベースでは、SQL を使用できます。 使用する Azure Table storage など、一部の NoSQL データベースで[OData](http://www.odata.org/)、および操作できるものは主キーでフィルター処理し、(使用可能なフィールドのサブセットを選択) を予測します。 |
| 簡易なスケーラビリティ | -多くの場合、どのようにスケールする「データ量必要がありますか。 -は、プラットフォームはネイティブに、スケール アウトを実装しますか。 に (サイズとスループット) の容量の追加/削除することは簡単する方法でしょうか。 リレーショナル データベースとテーブルは自動的にパーティション分割、スケーラブルなさせることをいくつかの制限より多くするにくいので。 Azure Table storage などの NoSQL データ ストアが本質的にすべてのパーティションし、パーティションを追加することに制限はほとんどありません。 テーブル ストレージは最大で 200 のテラバイトを容易に拡張できますが、Azure SQL Database のデータベースの最大サイズは 500 ギガバイトです。 複数のデータベースに分割することでリレーショナル データをスケールすることができますが、そのモデルをサポートするためにアプリケーション設定には、多くプログラミング作業にはが含まれます。 |
| インストルメンテーションと管理 | -簡単に、インストルメント化、監視、および管理するプラットフォームですか。 プラットフォームは、無料でどのようなメトリックを事前に知る必要がありますので、正常性と、データ ストアのパフォーマンスに関する最新情報を保持する必要があり、自分で開発する必要があります。 |
| オペレーション | -簡単に、Azure にデプロイして実行するプラットフォームですか。 PaaS ですか。 IaaS でしょうか。 Linux でしょうか。 Table Storage と SQL Database は、簡単に Azure で設定できます。 組み込みの Azure PaaS ソリューションではないプラットフォームでは、多くの労力が必要です。 |
| API のサポート | -をプラットフォームを使用するが容易にする API 使用可能なのですか。 Azure Table サービスには .NET 4.5 の非同期プログラミング モデルをサポートする .NET API と SDK があります。 .NET アプリを作成する場合、それを記述し、キー/値列のデータ ストアする別のプラットフォームの API がない、または少ない包括的な 1 つと比較して、Azure Table Service のコードをテストするはるかに簡単になります。 |
| トランザクションの整合性とデータの一貫性 | では、プラットフォームがデータの整合性を保証するためにトランザクションをサポートすることが重要でしょうか。 一括電子メールが送信されると、パフォーマンスとストレージ コストの低いデータがトランザクションか、参照整合性、データ プラットフォームでの自動サポートより重要な可能性がありますを追跡するため、Azure Table サービス、適切な選択を行っています。 追跡銀行口座の残高や発注書の方が適切になります厳密なトランザクション上の保証を提供するリレーショナル データベース プラットフォームです。 |
| ビジネス継続性 | にバックアップ、復元、およびディザスター リカバリーを簡単する方法にはでしょうか。 実稼働データの破損はやがてと元に戻す関数をする必要があります。 リレーショナル データベースには、多くの場合、時間の時点に復元する機能など、きめ細かな復元機能があります。 考慮すべき重要な要因を検討している各プラットフォームで利用できる復元機能を理解します。 |
| コスト | 場合は、複数のプラットフォームでは、データ ワークロードをサポートできますが、どのような違いの料金はいくらですか。 たとえば、ASP.NET Identity を使用する場合は、Azure Table サービスまたは Azure SQL Database でユーザー プロファイル データを格納できます。 豊富なクエリの SQL Database の機能を必要としない場合がありますテーブルを選択した Azure の一部多くのコストがかかりますので、記憶域の指定された量の少ない。 |

どのような一般的な推奨事項は、データ ストレージ ソリューションを選択する前にこれらの各カテゴリの質問に対する回答がわからない。

さらに、ワークロードには、他のユーザーよりも優れたいくつかのプラットフォームをサポートできる特定の要件があります。 例えば:

- アプリケーションの必要な監査の機能ですか。
- データの長期にわたって要件--アーカイブまたは削除の自動化された機能が必要ですか。
- 特殊なセキュリティ ニーズがあるでしょうか。 たとえば、データには PII (個人を特定できる情報) が含まれていますが、PII がクエリ結果から除外されたことを確認できる必要があります。
- 規制や技術上の理由からクラウドに格納できない一部のデータがある場合は、オンプレミス ストレージとの統合を容易にするクラウド データ ストレージ プラットフォームを必要があります。

## <a name="demo--using-sql-database-in-azure"></a>デモ: Azure で SQL Database の使用

Fix It アプリでは、リレーショナル データベースを使用して、タスクを保存します。 環境作成用 Windows PowerShell のスクリプトに示すように、[すべて自動化章](automate-everything.md)2 つの SQL Database インスタンスを作成します。 クリックして、ポータルでこれらを表示できます、 **SQL データベース**タブ。

![ポータルでの SQL データベース](data-storage-options/_static/image8.png)

ポータルを使用してデータベースを作成する簡単です。

クリックして**Data Services では新しい--** -- **SQL Database** -- **簡易作成**をデータベース名を入力し、お客様のアカウントが既にあるサーバーを選択または、新しく作成し、をクリックして**SQL データベースの作成**です。

![新しい SQL Database](data-storage-options/_static/image9.png)

いくつか秒待つし、データベースがある azure を使用できるようになります。

![作成された新しい SQL データベース](data-storage-options/_static/image10.png)

Azure はいくつかの秒何かかる場合がありますが、1 日、または 1 週間以上で、オンプレミス環境を実現します。 スクリプトまたは管理 API を使用してデータベースを自動的に作成できますが同じくらい簡単にため、動的にスケール アウトできます < o:p > の複数のデータベース間でデータを分散させることで、アプリケーションがそのようプログラムされた限りとします </o。: p >

これは、サービスとしてのプラットフォーム モデルの例です。 サーバーを管理する必要はありません、行います。 バックアップについて心配する必要はありません、行います。 高可用性 – で実行されている 3 台のサーバー間では、データベース内のデータが自動的にレプリケートされます。 マシンが実行されていない場合自動的にフェールオーバーし、データは失われません。 サーバーに定期的に修正プログラムが、気にする必要はありません。

ボタンをクリックし、必要があるし、新しいデータベースの使用をすぐに開始する正確な接続文字列を取得します。

![接続文字列](data-storage-options/_static/image11.png)

ダッシュ ボードは、接続の履歴とストレージの使用量を示しています。

![SQL データベース ダッシュ ボード](data-storage-options/_static/image12.png)

Portal でデータベースを管理することができます、または SQL Server ツールを使用して既に慣れている、SQL Server Management Studio (SSMS) および SQL Server オブジェクト エクスプ ローラー (SSOX) およびサーバー エクスプ ローラーは、Visual Studio ツールなど。

![SSOX](data-storage-options/_static/image13.png)

もう 1 つ優れている点は、価格モデルです。 無料の 20 MB データベースで開発を開始することができ、実稼働データベースが 1 か月あたり約 5 ドルから開始します。 実際には、最大容量ではなく、データベースに格納するデータ量に対してのみ支払います。 ライセンスを購入する必要はありません。

SQL Database は、簡単にスケールできます。 Fix It アプリでは、自動化スクリプトを作成するデータベースが 1 ギガに制限されます。 スケール アップ 150 ギガする場合は、だけポータルに移動し、その設定を変更したり、REST API コマンドを実行し、(秒) にデータをデプロイできる 150 ギガ データベースがあります。

![SQL Database のエディションとサイズ](data-storage-options/_static/image14.png)

迅速かつ簡単にインフラストラクチャを立ち上げ、それをすぐに使用するクラウドの機能です。

Fix It アプリ メンバーシップ (認証と承認) 用と、データの 2 つの SQL データベースの使用し、プロビジョニングし、スケールとするために必要になります。 先ほど見たで Windows PowerShell スクリプトとも、ポータルで行うにはいかに簡単かを確認したので、データベースをプロビジョニングする方法。

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework と ADO.NET を使用してデータベースに直接アクセス

アプリ Fix It では、Entity Framework を使用してこれらのデータベースがアクセスする、Microsoft の .NET アプリケーションの ORM (オブジェクト リレーショナル マッパー) をお勧めします。 ORM は開発者の生産性を容易にする優れたツールが、一部のシナリオでパフォーマンスの低下を犠牲生産性が発揮されます。 実際のクラウド アプリでは、EF を使用して、直接 ADO.NET を使用するかの選択肢を作成する必要はありません--両方を使用します。 通常、データベースで動作するコードを作成する場合は、最大のパフォーマンスが重要でないと、簡略化されたコーディングと Entity Framework を入手することをテストの利用できます。 EF のオーバーヘッドが許容できないパフォーマンスを原因と状況では、記述し、ストアド プロシージャを呼び出すことによって理想的には、ADO.NET を使用して、独自のクエリを実行できます。

「おしゃべり」可能な限り最小限に抑える、データベースへのアクセスに使用するどのような方法を指定します。 つまり、1 つの大きなクエリ結果を数十または数百の小さなではなくセットでは、必要なすべてのデータを取得できますを通常が望ましい方法です。 たとえば、受講者の一覧をコースに登録している必要がありますをすべて 1 つの結合クエリではなく 1 つのクエリでの受講者の取得と各受講者コースの個別のクエリを実行するデータの取得に通常お勧めします。

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL データベースと Fix It アプリで Entity Framework

Fix It アプリで、 `FixItContext` Entity Framework から派生したクラス、`DbContext`クラス、データベースを識別し、データベース内のテーブルを指定します。 コンテキストは、タスクでは、エンティティ セット (テーブル) を指定し、コードでをコンテキスト接続文字列名。 その名前は、Web.config ファイルで定義されている接続文字列を指します。

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

内の接続文字列、 *Web.config* appdb (ここでは、ローカルの開発用データベースを指す) という名前のファイルは。

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework で作成、 *FixItTasks*のプロパティに基づいて、テーブル、`FixItTask`エンティティ クラスです。 継承しないまたは Entity Framework のすべての依存関係があることを意味する単純な POCO (Plain Old CLR Object) クラスです。 Entity Framework は、それに基づくテーブルを作成する方法を知っているしでの CRUD (作成、読み取り、更新、削除) 操作を実行します。

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks テーブル](data-storage-options/_static/image15.png)

Fix It アプリには、CRUD 操作のデータ ストアの操作に使用するリポジトリ インターフェイスが含まれています。

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

リポジトリ メソッドは、すべてのデータ アクセスは完全に非同期の方法で行えますあるため、すべての非同期ことに注意してください。

など、データを操作するリポジトリの実装呼び出し Entity Framework の非同期メソッドは LINQ クエリ、挿入の場合とにも更新、および削除操作。 Fix It タスクを検索するためのコードの例を次に示します。

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

いくつかのタイミングとエラー ログ コードをここでもが、紹介を後でわかります、 [』 の「監視とテレメトリ](monitoring-and-telemetry.md)します。

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Azure での VM (IaaS) での SQL Server と SQL Database (PaaS) を選択します。

SQL Server と Azure SQL Database のメリットは、その両方の主要なプログラミング モデルが同じであります。 同じスキルのほとんどは、両方の環境で使用できます。 クラウドで、開発での SQL Server データベースと SQL Database インスタンスを使用することも、修正、アプリの設定であります。

代わりに、ことができます、同じ SQL Server をクラウドで実行する IaaS Vm でインストールすることによって、オンプレミスを実行することです。 一部のレガシ アプリケーションの VM で SQL Server を実行しているより優れたソリューションがある可能性があります。 SQL Server データベースは、専用の VM で実行される、ので共有サーバー上で実行される SQL Database のデータベースよりも利用できるその他のリソースがあります。 つまり、SQL Server データベースが大きくなるし、も引き続き実行します。 一般に小さくなる、データベースのサイズとテーブルのサイズ、パフォーマンスが向上ユース ケースは SQL database (PaaS)。

2 つのモデルを選択する方法のいくつかのガイドラインを示します。

| Azure SQL Database (PaaS) | 仮想マシン (IaaS) で SQL Server |
| --- | --- |
| **プロフェッショナル**-作成または Vm の管理、更新、または OS や SQL 修正プログラムを適用する必要はありませんAzure によっては、ためです。 -組み込みの高可用性、データベース レベルの sla。 -(ライセンスが必要) の使用に対してのみ課金するされるため、総保有コスト (tco) が低い。 多数の小さなデータベースを処理する優れた (&lt;= それぞれ 500 GB)。 -動的に作成する簡単な新しいデータベースを有効にするスケール アウトします。 | ***プロフェッショナル***- オンプレミスの SQL Server の機能と互換性のあります。 SQL Server の実装- [AlwaysOn を介した高可用性](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx)2 + Vm、VM レベルの sla でします。 -SQL の管理方法を完全に制御があります。 の SQL ライセンスが既に所有、またはいずれかの時間単位で支払いを再利用ことができます。 以下の処理に適してがサイズが大きく (1 TB 超) データベース。 |
| **短所**-機能、オンプレミスの SQL Server とのギャップの一部 (が不足している[CLR 統合](https://technet.microsoft.com/library/ms131102.aspx)、 [TDE](https://technet.microsoft.com/library/bb934049.aspx)、[圧縮サポート](https://technet.microsoft.com/library/cc280449.aspx)、 [SQL ServerReporting Services](https://technet.microsoft.com/library/ms159106.aspx)など) で 500 GB のデータベース サイズの上限。 | ***短所***- 更新プログラム (OS と SQL) は、適切に作成し、Db の管理はお客様の責任の制限 (16 個のデータ ドライブ) を使用して 8000 約ディスク IOPS (1 秒あたりの入力/出力操作)。 |

VM で SQL Server を使用する場合は、SQL Server ライセンスを使用するまたはの時間に 1 つを支払うことができます。 たとえば、ポータルまたは REST API 経由では、SQL Server イメージを使用して新しい VM を作成できます。

![SQL Server での VM を作成します。](data-storage-options/_static/image16.png)

![SQL Server VM イメージの一覧](data-storage-options/_static/image17.png)

作成すると、VM、SQL Server イメージは日割り計算を SQL Server のライセンス コストは VM の使用状況に基づいて、時間単位で。 プロジェクト数か月に実行するだけである場合は場合、は、コスト、時間単位で支払いになります。 プロジェクトが年の最後に移動する場合がコストがかからない普通に行う方法、ライセンスを購入します。

## <a name="summary"></a>まとめ

クラウド コンピューティングは実用的に混在させるし、アプリケーションのニーズに最適な一致データ記憶域のアプローチです。 新しいアプリケーションを構築する場合は引き続きアプリケーションの成長する場合の動作方法を選択するには次のとおり、質問について慎重に検討します。 [[次へ] の章](data-partitioning-strategies.md)は複数のデータ記憶域のアプローチを結合するために使用できるいくつかのパーティション分割の方法について説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

データベース プラットフォームを選択します。

- [拡張性の高いソリューションへのデータ アクセス: SQL、NoSQL、および Polyglot の永続化を使用して](http://aka.ms/dag-doc)します。 電子書籍では、Microsoft Patterns and Practices を深さでは、異なる種類のデータには、クラウド アプリケーションで使用できる格納します。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/ff898430.aspx)します。 データ整合性入門、データのレプリケーション、および同期ガイダンスについては、インデックス テーブル パターン、具体化されたビュー パターンを参照してください。
- [BASE: Acid 代わりに](http://queue.acm.org/detail.cfm?id=1394128)します。 データの整合性とスケーラビリティのトレードオフに関する記事します。
- [7 週間で 7 つのデータベース: 最新のデータベースと NoSQL 移動ガイド](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)します。 Eric Redmond と Jim R. Wilson 著。 自分で現在利用可能なデータのストレージ プラットフォームに導入するため強くお勧めします。

SQL Server と SQL データベースの選択。

- [SQL Database のガイダンスの premium プレビュー](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx)します。 SQL データベースの Premium、および SQL Database Web および Business エディションを選択するタイミングに関するガイダンスを紹介します。
- [ガイドラインと制限事項 (Azure SQL データベース)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)します。 SQL Database の制限事項に関するドキュメントにリンクされているポータルのページでは、その SQL データベースを SQL Server の機能に注目した 1 つ以上サポートされていません。
- [Azure Virtual Machines における SQL Server](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx)します。 Azure で SQL Server を実行する方法のドキュメントにリンクされているポータル ページです。
- [Scott Guthrie が Azure で SQL Database について説明します](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/)します。 Scott Guthrie による SQL データベースへの 6 分間ビデオの概要。
- [アプリケーション パターンと Azure Virtual Machines における SQL Server の開発戦略](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx)します。

ASP.NET Web アプリでの Entity Framework と SQL Database の使用

- [MVC 5 を使用して EF 6 の概要](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)します。 MVC アプリをビルドする手順 9-構成のチュートリアル シリーズでは、EF を使用し、Azure と SQL Database、データベースを展開します。
- [Visual Studio を使用して ASP.NET Web 配置](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)します。 EF Code First を使用してデータベースを展開する方法について詳しく説明するチュートリアル シリーズの 12 の部分で構成します。
- [メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET MVC 5 アプリを Azure の Web サイトにデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)します。 Web アプリの作成について説明するステップ バイ ステップ チュートリアル認証を使用して、メンバーシップ データベースにアプリケーションのテーブルを格納、データベース スキーマを変更、アプリを Azure にデプロイします。
- [ASP.NET データ アクセス コンテンツ マップ](https://go.microsoft.com/fwlink/p/?LinkId=282414)します。 EF と SQL Database を操作するためのリソースへのリンク。

Azure で MongoDB を使用します。

- [MongoLab - Azure で MongoDB](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/)します。 ドキュメントについては、Azure で MongoDB の実行に関するポータル ページです。
- [Azure の仮想マシンで実行される MongoDB に接続する Azure の web サイト作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)です。 ASP.NET web アプリケーションで MongoDB データベースを使用する方法を示すステップ バイ ステップ チュートリアルです。

HDInsight (Hadoop on Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/)します。 ポータルは、HDInsight のドキュメントを[Azure](https://azure.microsoft.com/) web サイト。
- [Hadoop と HDInsight: Azure でのビッグ データ](https://msdn.microsoft.com/magazine/dn385705.aspx)します。 Bruno Terkaly と Ricardo Villalobos、Hadoop on Azure を導入して MSDN Magazine の記事。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 MapReduce のパターンを参照してください。

> [!div class="step-by-step"]
> [前へ](single-sign-on.md)
> [次へ](data-partitioning-strategies.md)
