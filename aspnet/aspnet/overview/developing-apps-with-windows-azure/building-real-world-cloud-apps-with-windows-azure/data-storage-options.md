---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: データ ストレージ オプション (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: d638dca331cb24c340a4471e5964a00b75bb608a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879713"
---
<a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>データ ストレージ オプション (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


クラウド アプリケーションをデザインするときに、その他のデータ記憶域オプションを見落としがちになることが多いし、リレーショナル データベースにほとんどの人が使用されます。 結果は、最適なパフォーマンス、高コストを引き下げ、または、さらに悪化のため[NoSQL](http://en.wikipedia.org/wiki/NoSQL) (非リレーショナル) データベースでは、いくつかのタスクをリレーショナル データベースよりも効率的に処理できます。 お客様は、重要なデータ ストレージの問題を解決するためのみ、ときに多くの場合、NoSQL オプションのいずれかがうまく機能、リレーショナル データベースがあるためです。 ような状況で、顧客がさらに優れたオフ、実稼働環境にアプリを展開する前に、NoSQL ソリューションを実装する必要があります。

その一方で、NoSQL database 実行できることすべてが十分に間違いがあります。 単一データの管理に最適です。 すべてのデータ記憶域タスクはありません。別のデータ管理ソリューションは、さまざまなタスク用に最適化されています。 ほとんどの実際のクラウド アプリは、さまざまなデータ記憶域の要件を持ち、複数のデータ記憶域ソリューションの組み合わせによって最適な多くの場合、処理されます。

この章の目的は、クラウド アプリケーションと、シナリオに合ったものを選択する方法のいくつかの基本的なガイダンスを使用できるデータ記憶域オプションの広範な感を与えるです。 使用できるオプションを認識し、アプリケーションを開発する前に、それらの長所と短所について検討することをお勧めします。 運用アプリケーションでデータ ストレージ オプションを変更すると、非常に困難ですが、平面は飛行中は、jet エンジンを変更することのようなことができます。

## <a name="data-storage-options-on-azure"></a>Azure でのデータ記憶域オプション

クラウド比較的簡単、さまざまなリレーショナルおよび NoSQL データ ストアを使用します。 Azure で使用できるデータのストレージ プラットフォームのいくつか示します。

![](data-storage-options/_static/image1.png)

テーブルは、次の 4 つの種類の NoSQL データベースを示しています。

- [キー/値データベース](https://msdn.microsoft.com/library/dn313285.aspx#sec7)キー値ごとに 1 つのシリアル化されたオブジェクトを格納します。 大量のデータを指定したキー値の 1 つの項目を取得するがないと場所を格納するのに適している、項目の他のプロパティに基づくクエリにします。

    [Azure Blob ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/)キー/値のデータベース フォルダーとファイル名に対応するキーの値を持つ、クラウド内のファイル ストレージのように機能です。 ファイルの内容の値を検索ではなく、そのフォルダーとファイル名でファイルを取得します。

    [Azure テーブル ストレージ](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/)はキー/値データベースもします。 それぞれの値が呼び出されます、*エンティティ*(パーティション キーと行キーで識別される行に似ています) が複数には含まれています*プロパティ*(列と同様、テーブル内のすべてのエンティティが同じを共有する必要しますが、列)。 キー以外の列に対してクエリを実行して、非常に効率的ではありません、避ける必要があります。 たとえば、1 人のユーザーに関する情報を格納する 1 つのパーティションを持つユーザー プロファイル データを格納できます。 同じパーティション内の別のエンティティまたはエンティティが 1 つの個別のプロパティでは、ユーザー名やパスワードのハッシュ、生年月日などのデータを格納できます。 すべてのユーザーの誕生日の特定の範囲でクエリを実行したくし、プロファイル テーブルと別のテーブル間の結合クエリを実行することはできません。 テーブル ストレージはリレーショナル データベースより安価とスケーラブルなが複雑なクエリまたは結合を有効にしないこと。
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8)キー/値のデータベースが、値を*ドキュメント*です。 "Document"は、ここでは、Word または Excel のドキュメントの意味で使用されていないが、名前付きフィールドと子ドキュメントあるうちいずれかの値のコレクションのことを意味します。 たとえば、注文履歴テーブルに注文ドキュメントがあります注文番号、発注日、および顧客フィールドです。顧客のフィールドの名前と住所のフィールドが存在します。 データベースは、XML、YAML、JSON、または BSON; などの形式でフィールドのデータをエンコードします。またはテキスト形式を使用できます。 キー/値のデータベースとは別のドキュメント データベースを設定する機能の 1 つは、非キー フィールドにクエリを実行し、クエリをより効率的にするためにセカンダリ インデックスを定義する機能です。 この機能によって、ドキュメント キーの値よりも複雑な条件に基づいてデータを取得する必要があるアプリケーションに適したドキュメント データベース。 たとえば、販売注文履歴ドキュメント データベースは、製品 ID、顧客 ID、顧客名などのさまざまなフィールドで照会できます。 [MongoDB](http://www.mongodb.org/)は一般的なドキュメント データベースです。
- [列ファミリ データベース](https://msdn.microsoft.com/library/dn313285.aspx#sec9)キーと値を使用すると、データ記憶域の構造列のファミリと呼ばれる関連列のコレクションにデータ ストアはします。 たとえば、国勢調査データベースがユーザーの名前の列の 1 つのグループを含めることが (最初に、ミドル ネーム、姓)、個人のアドレス、1 つのグループとユーザーのプロファイル情報 (DOB、性別など) の 1 つのグループです。 データベースは各列のファミリをすべて、同じキーに関連する 1 人のユーザーのデータを維持しながら別のパーティションに格納し、できます。 すべては、名前とアドレス情報を読み取る必要がないすべてのプロファイル情報を確認できます。 [Cassandra](http://cassandra.apache.org/)は人気のある列ファミリ データベースです。
- [データベースをグラフ化](https://msdn.microsoft.com/library/dn313285.aspx#sec10)オブジェクトとリレーションシップのコレクションとして情報を格納します。 グラフのデータベースの目的は、それらの間のオブジェクトとのリレーションシップのネットワークを通過するクエリを効率的に実行するアプリケーションを有効にします。 たとえば、オブジェクトには、人事管理データベース内の従業員が可能性がありますを見つけてたい場合がありますを容易にするクエリなど"全従業員を直接または間接的に Scott にします" [Neo4j](http://www.neo4j.org/)はグラフの一般的なデータベースです。

リレーショナル データベースと比較して、NoSQL オプションでは、はるかに多くのスケーラビリティと記憶域および非構造化データの分析を与える対費用効果を提供します。 その代わりには、豊富な queryability およびリレーショナル データベースの信頼性の高いデータの整合性機能を提供しないことです。 NoSQL 適して IIS ログ データは、結合クエリの必要なしに高ボリュームが含まれます。 NoSQL が使用できないようにも銀行取引、絶対データの整合性を必要し、他のアカウントに関連するデータが多のリレーションシップが含まれます。

呼ばれるデータベース プラットフォームの新しいカテゴリがある[また](http://en.wikipedia.org/wiki/NewSQL)queryability およびリレーショナル データベースのトランザクションの整合性と NoSQL データベースのスケーラビリティを組み合わせてです。 新規データベースは、分散ストレージとクエリ処理は、"OldSQL"データベースに実装するは難しい設計されています。 [NuoDB](http://www.nuodb.com/) Azure で使用できる新規データベースの例に示します。

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop と MapReduce

大量の NoSQL データベースに格納できるデータは、適切なタイミングで効率よく分析にくい場合があります。 などのフレームワークを使用できることを行う[Hadoop](http://hadoop.apache.org/)を実装する[MapReduce](http://en.wikipedia.org/wiki/MapReduce)機能します。 基本的にはどのような MapReduce プロセスでは、次に示します。

- 制限データからを選択して処理する必要があるデータのサイズが実際には、分析が必要なデータだけを格納します。 たとえば、ユーザー プロファイルのデータ ストアから誕生年のみを選択するように、誕生年によって、ユーザーの構成を知りたいです。
- データの部分に分割し、処理のための別のコンピューターに送信します。 コンピューター A 1950 1959 日付を持つユーザーの数を計算するか、コンピューター B 1960 1969 年などです。このコンピューターのグループと呼ばれる、 *Hadoop クラスター*です。
- 結果を置く各部分のまとめ部分での処理が完了後します。 各生まれた年の人数の比較的短い形式の一覧があるようになりましたと、この全体的なリスト内の割合を計算するタスクが管理しやすいです。

Azure で[HDInsight](https://azure.microsoft.com/services/hdinsight/)すると、処理、分析、および Hadoop の電源を使用して大規模なデータから新しい見識を得ることができます。 たとえば、web サーバーのログを分析するのに使用する可能性があります。

- ストレージ アカウントに、web サーバーのログ記録を有効にします。 これは、ログの書き込みをアプリケーションに HTTP 要求ごとに、Blob サービスに Azure を設定します。 Blob サービスは、基本的にクラウド ファイルの記憶域と、その HDInsight 統合適切です。 

    ![Blob ストレージにログ](data-storage-options/_static/image2.png)
- アプリは、トラフィックを取得、web サーバーの IIS ログは、Blob ストレージに書き込まれます。 

    ![Web サーバーのログ](data-storage-options/_static/image3.png)
- ポータルで、をクリックして**新規** - **Data Services** - **HDInsight** - **簡易作成**、HDInsight クラスター名、クラスターのサイズ (HDInsight クラスターのデータ ノード数)、およびユーザー名とパスワード、HDInsight クラスターを指定します。 

    ![HDInsight](data-storage-options/_static/image4.png)

ログ分析などの質問に対する回答を取得して MapReduce ジョブを設定することができますようになりました。

- どのような時間帯を探すアプリで、最大股は最小トラフィックを入手できますか。
- どのような国では、自分からのトラフィックですか。
- 自分のトラフィックがある領域の平均近所収入とは (パブリック IP アドレスによるコンピューター の収入を提供するデータセットがあるし、web サーバーのログ内の IP アドレスを一致することができます。)
- 近所収入は、特定のページまたはサイト内の製品をどのように関連付けるか。

顧客が興味なります、その確率に基づいて、または特定の製品を購入する可能性が広告の対象に上記のような質問に対する回答を使用できます。

説明に従って、[自動化すべて章](automate-everything.md)ポータルで行うことができます関数の多くを自動化すること、およびを設定して、HDInsight 分析ジョブの実行が含まれる。 一般的な HDInsight スクリプトには、次の手順が含まれます。

- HDInsight クラスターをプロビジョニングし、Blob ストレージ入力についてもストレージ アカウントにリンクします。
- HDInsight クラスターへ、MapReduce ジョブの実行可能ファイル (.jar ファイルまたは .exe ファイル) をアップロードします。
- Blob ストレージへの出力データを格納する MapReduce を送信します。
- ジョブが完了するまで待ちます。
- HDInsight クラスターを削除します。
- Blob ストレージからの出力にアクセスします。

このすべてを実行するスクリプトを実行するが、コストを最小化、HDInsight クラスターがプロビジョニングされると、時間が最小化します。

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>Platform as a Service (PaaS) インフラストラクチャと as a Service (IaaS)

以前に一覧表示するデータ記憶域オプションには、プラットフォームの as a Service (PaaS) およびインフラストラクチャ-as a Service (IaaS) ソリューションの両方が含まれます。 PaaS では、ハードウェアとソフトウェアのインフラストラクチャの管理おと同様に、サービスを使用することを意味します。 SQL データベースは、Azure の PaaS 機能です。 データベースに問い合わせる必要が、バック グラウンドで Azure を設定し、Vm を構成してにデータベースを設定します。 Vm に直接アクセスできないし、それらを管理する必要はありません。IaaS と設定、構成、および、データ センター インフラストラクチャで実行されている Vm の管理に必要なあらゆるを配置します。 マイクロソフトは、一般的な VM 構成の構成済み VM イメージのギャラリーを提供します。 たとえば、Windows Server 2008、Windows Server 2012、BizTalk Server、Oracle WebLogic Server、Oracle データベースなどの構成済み VM イメージをインストールできます。

Azure では、PaaS データのソリューションは次のとおりです。

- Azure の SQL データベース (以前は SQL Azure と呼ばれます)。 SQL Server に基づくクラウド リレーショナル データベース。
- Azure テーブル ストレージです。 キー/値 NoSQL データベース。
- Azure Blob ストレージです。 クラウド内のファイル ストレージ。

IaaS、たとえば、VM 上に読み込むことができます何もを実行してください。

- SQL Server、Oracle、MySQL、SQL Compact、SQLite、または Postgres などのリレーショナル データベースです。
- Memcached、Redis、Cassandra、Riak などキー/値のデータを格納します。
- HBase などの列データを格納します。
- MongoDB、RavenDB、および CouchDB などのデータベースを文書化します。
- グラフ Neo4j などのデータベースを表示します。

![Azure でのデータ記憶域オプション](data-storage-options/_static/image5.png)

IaaS オプションには、ほぼ無制限データ記憶域オプション、およびそれらの多くは事前構成済みイメージを使用して Vm を作成するため、特に簡単に使用します。 たとえば、管理ポータルに移動**仮想マシン**、] をクリックして、**イメージ**タブをクリックし、をクリックして **[VM Depot の参照**です。

![VM Depot を参照します。](data-storage-options/_static/image6.png)

次の一覧に表示[何百もあらかじめ構成された VM イメージの](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)MongoDB、Neo4J、Redis、Cassandra、CouchDB など、プレインストールされているデータベース管理システムをもつイメージから VM を作成できます。

![VM Depot の参照で MongoDB](data-storage-options/_static/image7.png)

Azure で IaaS データ ストレージ オプションは、できるだけ、使いやすくする、PaaS サービスはよりコスト効果の高いおよび多くのシナリオについて実用的で多くの利点を持っています。

- Vm を作成する必要はありません、だけ使用するポータルまたはスクリプトのデータ ストアを設定します。 200 テラバイトのデータ ストアを実行する場合に、だけ、ボタンをクリックするか、コマンドを実行し、(秒) を使用するための準備がします。
- 管理またはサービスによって使用される Vm の修正プログラムを適用する必要はありません。Microsoft は、自動的にします-スケールまたは高可用性のインフラストラクチャの設定について心配する必要はありません。Microsoft は、すべてを処理します。
- 以外のライセンスを購入する必要はありませんサービスの料金には、ライセンス料が含まれます。
- 分だけ支払う際に使用します。

Azure での PaaS データ ストレージのオプションには、サード パーティ プロバイダーによってソリューションが含まれます。 たとえば、選択することができます、 [MongoLab アドオン](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/)をサービスとして MongoDB データベースをプロビジョニングする Azure ストアからです。

## <a name="choosing-a-data-storage-option"></a>データ記憶域オプションを選択します。

1 つの方法ではありませんは、すべてのシナリオに適したです。 すべてのユーザーは、このテクノロジの答えはあると場合、は、まずに確認してください「という質問とは」、さまざまなソリューションは、さまざまな用に最適化されたためです。 リレーショナル モデルへの明確な利点があります。理由が経過している囲むために長い時間です。 NoSQL ソリューションで対処できます SQL へのダウン側もあります。

多くの場合、表示される内容お最適な作業は合成的なアプローチでは、場所を使用する SQL、NoSQL 1 つのソリューションです。 ときにユーザー言ってもしていることを望んで NoSQL、いる何する多くの場合にドリル ダウンする場合は、検索のいくつかのさまざまな NoSQL フレームワークを使用していること: を使用している[CouchDB](http://wiki.apache.org/couchdb/Introduction)、および[Redis](http://redis.io/)、および[Riak](http://basho.com/riak/)さまざまなものにします。 偶数の Facebook は、NoSQL を広範囲にわたって使用するは、サービスのさまざまな部分をさまざまな NoSQL フレームワークを使用します。 柔軟に混在させるし、データ記憶域のアプローチを一致は、複数のデータ ソリューションを使用して、それらを 1 つのアプリに統合を簡単になっているため、クラウドの良いの機能の 1 つです。

その方法を選択する場合について検討するいくつかの質問を次に示します。

| セマンティック データ | -、中核となるデータ ストレージとデータ アクセスはセマンティック (は、データを格納するリレーショナルまたは非構造化) ですか? Blob ストレージにメディア ファイルなどの非構造化データに最も適した製品のインベントリ、仕入先、顧客の注文など、関連するデータのコレクションは、リレーショナル データベースに最も適合します。 |
| --- | --- |
| クエリのサポート | -どの程度簡単は、データを照会するか。 -質問の種類を指定できます効率よく寄せられるか。 キー/値のデータ ストアには、キーの値を指定した 1 つの行を取得するのには非常に良好がこれには適しません複雑なクエリがあります。 ユーザー プロファイル データのストアに対して 1 つの特定のユーザーのデータを常に取得するキー/値のデータ ストアでしたが適切に機能します。さまざまな製品属性に基づくさまざまなグループを取得する製品カタログのリレーショナル データベースが優れています。 NoSQL データベースは大量のデータを効率的に格納できますが、アプリでのデータのクエリの周囲のデータベース構造があり、これにより、アドホック クエリを行うことが困難です。 リレーショナル データベースでは、ほとんどの種類のクエリを構築できます。 |
| 関数型のプロジェクション | -質問ことができます、集計などは、サーバー側で実行しますか。 選択数を実行する場合 (\*) から sql テーブルが非常に効率的にサーバー上のすべての作業を行うされ数値を探しているを返します。 集計がサポートされていない NoSQL データ ストアから同じ計算した場合は、これは、非効率的な「バインド解除済みクエリ」なでおそらくはタイムアウトが発生します。クエリが成功した場合でも、クライアントにサーバーからのすべてのデータを取得し、クライアント上の行をカウントする必要があります。 -どのような言語または式の型を使用できますか。 リレーショナル データベースでは、SQL を使用できます。 Azure テーブル ストレージなど、一部の NoSQL データベースで使用[OData](http://www.odata.org/)、し、これを行うには、主キーに対してフィルター処理およびプロジェクション (利用可能なフィールドのサブセットを選択) を取得します。 |
| 簡易なスケーラビリティ | -頻度および量は、データ スケールが必要ですか。 は、プラットフォームはネイティブ、スケール アウトを実装しますか。 -どの程度簡単は容量 (サイズとスループット) の追加/削除するか。 リレーショナル データベースとテーブルは自動的にパーティション分割、拡張性の高いようにもように、いくつかの制限を超えるスケールするが困難。 Azure テーブル ストレージのような NoSQL データ ストアが本質的にすべてのパーティションし、パーティションを追加するのには制限はほとんどありません。 テーブル ストレージは、最大で 200 テラバイトを容易に拡張できますが、Azure SQL データベースの最大データベース サイズが 500 ギガバイトです。 複数のデータベースにパーティション分割することで、リレーショナル データを拡張できますが、多数の作業をプログラミングでは、そのモデルをサポートするために、アプリケーションを設定します。 |
| インストルメンテーション、および管理容易性 | -どの程度簡単をインストルメントし、監視、および管理プラットフォームとは プラットフォームは、空き、どのようなメトリックを前もって知る必要があるため、正常性と、データ ストアのパフォーマンスに関する情報を入手を保持する必要があり、自分で開発する必要があります。 |
| オペレーション | -どの程度簡単、Azure を配置して実行するプラットフォームとは PaaS ですか。 IaaS? Linux しますか。 テーブル ストレージと SQL データベースは、簡単に Azure にセットアップします。 組み込みの Azure PaaS ソリューションのないプラットフォームでは、多くの労力が必要です。 |
| API のサポート | プラットフォームで作業しやすいできる API が使用可能なのですか。 Azure テーブル サービス、.NET 4.5 非同期プログラミング モデルをサポートする .NET API と SDK があります。 .NET アプリケーションを作成する場合、書き込みおよび別キーと値列のデータ ストア搭載されているプラットフォームの API がないか、小さい包括的なと比較して、Azure テーブル サービスのコードをテストするはるかに簡単になります。 |
| トランザクションの整合性とデータの一貫性 | では、プラットフォームがデータの整合性を保証するためにトランザクションをサポートすることが重要ですか。 一括電子メールが送信されると、パフォーマンスとストレージ コストの低いデータをトランザクションまたはデータ プラットフォームで参照整合性の自動サポートよりも重要にする可能性がありますを追跡するため、Azure テーブル サービス、適切な選択を行います。 追跡銀行口座の残高や、発注書方が適切になります厳密なトランザクションの保証を提供するリレーショナル データベース プラットフォームです。 |
| ビジネス継続性 | -どの程度簡単はバックアップ、復元、および災害復旧です。 実稼働データが破損いつかと元に戻す関数を必要があります。 リレーショナル データベースには、多くの場合、時間の時点に復元する機能など、多くの粒度の細かい復元機能があります。 考慮すべき重要な要因にはどのような復元機能を利用することを検討している各プラットフォームで理解することです。 |
| コスト | -複数のプラットフォームでは、データ ワークロードをサポートできますが場合、は、どのコストか。 たとえば、ASP.NET の Id を使用する場合は、Azure テーブル サービス、または Azure SQL データベースでユーザー プロファイル データを格納できます。 SQL データベースの施設が設備を照会する豊富なを必要としない場合可能性がありますテーブルを選択した Azure 一部にコストが多くあるため、一定量の記憶域を少なくします。 |

どのような一般的な推奨事項は、データ記憶域ソリューションを選択する前にこれらの各カテゴリの質問に対する回答がわからない。

さらに、ワークロードには、一部のプラットフォームが他より優れたサポートできる特定の要件があります。 例えば:

- 監査機能をアプリケーションの必要なは?
- データの長期にわたって要件は、新機能--自動のアーカイブや、パージ機能が必要ですか。
- 特殊なセキュリティ ニーズはありますか。 たとえば、データには PII (個人を特定できる情報) が含まれていますが、PII はクエリ結果から除外されているかどうかを確認できるようにする必要があります。
- 一部のデータの規制上または技術的な理由からクラウドに保存することはできませんがある場合は、クラウド データ ストレージ プラットフォームで、内部設置型のストレージとの統合を容易にする必要があります。

## <a name="demo--using-sql-database-in-azure"></a>デモ: Azure で SQL データベースの使用

修正、アプリは、タスクを格納するのにリレーショナル データベースを使用します。 環境の作成の Windows PowerShell スクリプトに示すように、[自動化すべて章](automate-everything.md)2 つの SQL データベース インスタンスを作成します。 表示する、ポータルでこれらをクリックすると、 **SQL データベース**タブです。

![ポータルで SQL データベース](data-storage-options/_static/image8.png)

ポータルを使用してデータベースを作成する簡単です。

をクリックして**データ サービス新--** -- **SQL データベース** -- **簡易作成**をデータベース名を入力し、お客様のアカウントが既にあるサーバーを選択新しく作成し、をクリックしてまたは**SQL データベースの作成**です。

![新しい SQL Database](data-storage-options/_static/image9.png)

、数秒待ってから、Azure を使用するための準備完了であるデータベース。

![新しい SQL データベースを作成](data-storage-options/_static/image10.png)

Azure はいくつかの秒の新機能かかる場合がある、1 日、または、1 週間以上を内部設置型環境で実行します。 同様の簡単さで作成できるためデータベースに自動的にスクリプトまたは管理 API を使用して、動的にスケール アウトできます </o:p > の複数のデータベース間でデータを分散させることによってアプリケーションされたプログラムには、その限りとします </o。: p >

これは、サービスとしてのプラットフォーム モデルの例です。 サーバーを管理する必要はありません、なるでしょう。 バックアップについて心配する必要はありません、なるでしょう。 高可用性 – で実行されている 3 台のサーバー間では、データベース内のデータが自動的にレプリケートされます。 マシンが停止した場合は自動的にフェールオーバーし、データは失われません。 します。 サーバーに定期的に修正プログラムが、心配する必要はありません。

ボタンをクリックしと必要とする新しいデータベースの使用をすぐに開始の正確な接続文字列を取得します。

![接続文字列](data-storage-options/_static/image11.png)

ダッシュ ボードは、接続の履歴と使用されるストレージの量を示しています。

![SQL データベース ダッシュ ボード](data-storage-options/_static/image12.png)

ポータルでデータベースを管理することも SQL Server のツールを使用して既にに慣れている、SQL Server Management Studio (SSMS) および SQL Server オブジェクト エクスプ ローラー (SSOX) およびサーバー エクスプ ローラー、Visual Studio ツールを含むです。

![SSOX](data-storage-options/_static/image13.png)

別優れている点は、価格のモデルです。 無料の 20 MB データベース開発を開始することができ、実稼働データベースが 1 か月あたり約 5 ドルで開始します。 実際には最大容量ではなく、データベースに格納するデータ量のみを支払います。 ライセンスを購入する必要はありません。

SQL データベースは拡張が容易です。 修正アプリケーション用 1 ギガで作成すると、オートメーション スクリプトにデータベースが制限されます。 150 ギガまで拡張する場合は、だけポータルに移動し、設定を変更したり、REST API コマンドを実行し、(秒) にデータを展開できる 150 ギガ データベースがあります。

![SQL データベースのエディションとサイズ](data-storage-options/_static/image14.png)

インフラストラクチャ迅速かつ簡単に立ち上がってがすぐに使用を開始するためにクラウドの能力です。

修正アプリで使用する 2 つの SQL データベース、メンバーシップ (認証と承認) と 1 つのデータ、これとはそれをプロビジョニングし、拡大または縮小するために実行します。 先ほど見た、データベースで Windows PowerShell スクリプト、およびポータルで行うには簡単な方法も説明しましたをプロビジョニングする方法です。

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework ではなく ADO.NET を使用してデータベースに直接アクセス

修正アプリ、Entity Framework を使用して、これらのデータベースにアクセスする、Microsoft の .NET アプリケーションの ORM (オブジェクト リレーショナル マッパー) をお勧めします。 ORM は開発者の生産性を容易にするための優れたツールですが、生産性が高く一部のシナリオでパフォーマンスの低下。 実際のクラウド アプリを直接 ADO.NET を使用する EF を使用するかを作成する必要はありません--両方を使用します。 ほとんどの場合、データベースを操作するコードを作成している場合は、最大のパフォーマンスが重要でないと、シンプルなコーディングとテストの Entity Framework を入手することの利用します。 EF オーバーヘッドが原因となり、パフォーマンスが著しく悪化状況では、作成し、ストアド プロシージャを呼び出すことによって理想的には、ADO.NET を使用して、独自のクエリを実行できます。

可能な限りの「おしゃべり」を最小限に抑えたいどの方法を使用してデータベースにアクセスします。 つまり、1 つ大きいクエリの結果セット数十または数百台の小規模なものではなくで必要なすべてのデータを取得できますは通常よりも適しています。 たとえば、一覧の受講者をコースに登録されているとしている必要がありますをすべて 1 つの結合クエリではなく、受講者を取得する 1 つのクエリおよび各受講者コースの個別のクエリを実行するデータの取得に通常お勧めします。

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>SQL データベースおよび Entity Framework で修正アプリ

修正アプリで、 `FixItContext` Entity Framework から派生するクラス`DbContext`クラス、データベースを識別し、データベース内のテーブルを指定します。 コンテキストは、タスクについては、エンティティ セット (テーブル) を指定し、コード渡しますコンテキストに接続文字列名。 その名前は、Web.config ファイルで定義されている接続文字列を指します。

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

内の接続文字列、 *Web.config* (ここでは、ローカル開発用データベースを指す) appdb はファイルの名前します。

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

Entity Framework を作成、 *FixItTasks*に含まれるプロパティに基づいて、テーブル、`FixItTask`エンティティ クラスです。 これは、単純な POCO (Plain Old CLR Object) クラスから継承または Entity Framework の依存関係があるいないことを意味します。 Entity Framework に基づくテーブルを作成する方法を知っているしでの CRUD (作成、読み取り、更新と削除) 操作を実行します。

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![FixItTasks テーブル](data-storage-options/_static/image15.png)

修正がアプリには、CRUD 操作データ ストアの操作に使用する、リポジトリ インターフェイスが含まれています。

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

リポジトリ メソッドは、すべてのデータ アクセスは、完全に非同期的な方法で実行できるためすべての非同期ことに注意してください。

リポジトリの実装の呼び出し Entity Framework 非同期メソッドを含む、データを使用は、更新、および削除操作 LINQ が insert と同様にクエリを実行します。 修正タスクを検索するためのコードの例を次に示します。

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

かがわかりますもいくつかのタイミングとエラーのログ コードをここで見ていきますを後で、[監視と遠隔測定の章](monitoring-and-telemetry.md)です。

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Azure で仮想マシン (IaaS) で SQL Server と SQL データベース (PaaS) を選択します。

SQL Server と Azure SQL Database、優れている点は、それらの両方に対して、コア プログラミング モデルが同じであります。 同じスキルのほとんどは、両方の環境で使用できます。 クラウドに、開発中の SQL Server データベースと SQL データベース インスタンスを使用することも、修正、アプリの設定であります。

代わりに、実行できますが同じ SQL Server クラウドの IaaS Vm にインストールして、内部設置型を実行すること。 一部のレガシ アプリケーションでは、VM で SQL Server を実行するとより良いソリューションは可能性があります。 SQL Server データベースは、専用の VM で実行されて、ため共有サーバーで実行されている SQL データベースのデータベースよりも使用できる多くのリソースがあります。 つまり、SQL Server データベースが大きくなるし、も引き続き実行します。 一般が小さいほど、データベースのサイズとテーブル サイズほど、ユース ケースに対して機能 SQL データベース (PaaS) します。

2 つのモデルの選択方法のいくつかのガイドラインを示します。

| Azure SQL データベース (PaaS) | 仮想マシン (IaaS) で SQL Server |
| --- | --- |
| **プロフェッショナル**-作成または Vm を管理、更新、または OS または SQL; 修正プログラムを適用する必要はありませんAzure です。 組み込みの高可用性、データベース レベルの SLA とします。 -(ライセンスが必要) の使用にだけ支払えばよいために、総保有コスト (tco) を低です。 多数の小さいデータベースの処理に適した (&lt;= 500 GB)。 動的に作成する簡単新しいデータベースを有効にするスケール アウトします。 | ***プロフェッショナル***- 内部設置型 SQL server 機能と互換性のあります。 SQL Server の実装- [AlwaysOn 高可用性](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx)2 + 仮想マシン、VM レベル SLA でします。 -SQL を管理する方法を完全に制御があります。 の既に所有する、または 1 つの時間単位で料金を支払う SQL ライセンスを再利用ことができます。 以下の処理に適してが大きい (1 TB +) データベース。 |
| **Cons** -一部の機能の内部設置型 SQL Server と比較してギャップ (が不足している[CLR 統合](https://technet.microsoft.com/library/ms131102.aspx)、 [TDE](https://technet.microsoft.com/library/bb934049.aspx)、[圧縮サポート](https://technet.microsoft.com/library/cc280449.aspx)、 [SQL ServerReporting Services](https://technet.microsoft.com/library/ms159106.aspx)など) で 500 GB のデータベース サイズの上限。 | ***Cons*** - 更新プログラム/修正プログラム (OS および SQL) は、ユーザーの責任の作成、Db の管理 (16 データ ドライブ) を使用して約 8000 を上限とするディスクの IOPS (1 秒あたりの入力/出力操作) - ユーザーの責任です。 |

VM で SQL Server を使用する場合は、独自の SQL Server ライセンスを使用するまたはの時間に 1 つの料金を支払うことができます。 たとえば、または REST API を使用して、ポータルでは、SQL Server イメージを使用して新しい VM を作成できます。

![SQL server VM を作成します。](data-storage-options/_static/image16.png)

![SQL Server VM イメージの一覧](data-storage-options/_static/image17.png)

作成する場合、VM プロアクティブ レートは SQL Server イメージを含む SQL Server のライセンス コストの使用状況に基づいて、VM の時間単位で。 のみか月間のいくつか実行する予定のプロジェクトの場合が、1 時間単位で料金を支払う安いです。 プロジェクトが先となる最後の年と思われる場合が、通常どおり、ライセンスを購入する安いです。

## <a name="summary"></a>まとめ

クラウド コンピューティングの実用的に混在するためし、アプリケーションのニーズに合わせてに最も一致するデータ記憶域のアプローチです。 新しいアプリケーションをビルドする場合について慎重に検討質問は引き続き、アプリケーションが拡張する場合にも動作方法を選択するために次のとおりです。 [次のチャプター](data-partitioning-strategies.md)データ記憶域の複数の方法を組み合わせるに使用できるいくつかのパーティション分割の方法を説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

データベース プラットフォームを選択します。

- [拡張性の高いソリューションへのデータ アクセス: SQL、NoSQL、および多言語の永続化を使用して](http://aka.ms/dag-doc)です。 クラウド アプリケーションの使用可能な電子書籍 Microsoft Patterns and Practices によってデータのさまざまな種類に徹底的に送られるを格納します。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/ff898430.aspx)です。 データ整合性の概要、データの複製と同期ガイダンスについては、インデックス テーブルのパターン、ビューの具体化パターンを参照してください。
- [ベース: Acid 代替](http://queue.acm.org/detail.cfm?id=1394128)です。 データの整合性とスケーラビリティの間のトレードオフに関する記事です。
- [7 週に 7 つのデータベース: 最新のデータベースや、NoSQL 移動にガイド](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921)です。 Eric Redmond、Jim R. Wilson book です。 自分で現在利用可能なデータ ストレージのプラットフォームの範囲を導入するためお勧めします。

SQL Server と SQL データベース間の選択。

- [ガイダンスについては SQL データベースの premium プレビュー](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx)です。 SQL データベース Premium、および SQL データベース Web および Business エディションを選択する場合のガイダンスを紹介します。
- [ガイドラインと制限事項 (Azure SQL データベース)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)です。 SQL データベースの制限事項に関するドキュメントにリンクするポータルのページでは、SQL データベースを SQL Server の機能に特化した 1 つを含むサポートされていません。
- [Azure の仮想マシンにおける SQL Server](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx)です。 Azure の SQL Server の実行に関するドキュメントにリンクするポータル ページです。
- [Scott Guthrie の説明で、Azure SQL データベース](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/)です。 6 分 Scott Guthrie による SQL データベースへの紹介ビデオです。
- [アプリケーション パターンと Azure の仮想マシンにおける SQL Server の開発戦略](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx)です。

ASP.NET Web アプリで Entity Framework と SQL データベースの使用

- [MVC 5 を使用する EF 6 にあたって](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。 EF を使用し、Azure と SQL データベースにデータベースを展開する MVC アプリケーションのビルドについて説明するチュートリアル シリーズの 9 つの部分で構成します。
- [Visual Studio を使用して ASP.NET Web 配置](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md)です。 EF Code First を使用してデータベースを展開する方法について詳細に送られるチュートリアル シリーズの 12 個の部分で構成します。
- [Azure の Web サイトにメンバーシップ、OAuth、および SQL データベースでのセキュリティで保護された ASP.NET MVC 5 アプリを展開](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)です。 Web アプリを作成する手順を説明する詳しい手順のチュートリアルは、認証を使用して、メンバーシップ データベースにアプリケーションのテーブルを格納、データベース スキーマを変更、および、アプリを Azure に配置をします。
- [ASP.NET データ アクセス コンテンツ マップ](https://go.microsoft.com/fwlink/p/?LinkId=282414)です。 EF と SQL データベースを操作するためのリソースへのリンク。

Azure で MongoDB の使用。

- [MongoLab - Azure での MongoDB](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/)です。 Azure で MongoDB の実行に関するドキュメントについてはポータル ページです。
- [Azure の仮想マシンで実行されている MongoDB に接続する Azure の web サイトを作成](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/)です。 ASP.NET web アプリケーションで MongoDB データベースを使用する方法についてステップ バイ ステップ チュートリアルです。

HDInsight (Hadoop on Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). ポータルで、HDInsight ドキュメントを[Azure](https://azure.microsoft.com/) web サイトです。
- [Hadoop と HDInsight: Azure での Big Data](https://msdn.microsoft.com/magazine/dn385705.aspx)です。 MSDN マガジン アーティクル Bruno Terkaly して Ricardo Villalobos、Hadoop on Azure の概要です。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 MapReduce のパターンを参照してください。

> [!div class="step-by-step"]
> [前へ](single-sign-on.md)
> [次へ](data-partitioning-strategies.md)
