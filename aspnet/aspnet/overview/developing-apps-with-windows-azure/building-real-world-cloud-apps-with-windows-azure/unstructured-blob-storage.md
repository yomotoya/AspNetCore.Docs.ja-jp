---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: 非構造化 Blob Storage (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1a56a76c9bf27fdd7afb090ca473ebeee4065fe2
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577419"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>非構造化 Blob Storage (Azure で現実世界のクラウド アプリの構築)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson]((https://twitter.com/RickAndMSFT))、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。 13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。


前の章でパーティション構成を調べるし、Fix It アプリが Azure Storage Blob サービス、およびその他のタスクのデータを Azure SQL Database でのイメージを格納する方法について説明します。 この章では Blob service に進むし、Fix It プロジェクトのコードでの実装方法を説明します。

## <a name="what-is-blob-storage"></a>Blob storage とは何ですか。

Azure Storage Blob service は、クラウドにファイルを格納する方法を提供します。 Blob service では、いくつかのファイルを格納するローカル ネットワーク ファイル システム上の利点があります。

- このコマンドは、高いスケーラビリティです。 1 つのストレージ アカウントに格納できる[数百のテラバイト](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)、複数のストレージ アカウントを持つことができます。 最大の Azure 顧客の一部は、数百ペタバイト規模を格納します。 Microsoft SkyDrive では、blob ストレージを使用します。
- 持続性になります。 Blob service に格納するすべてのファイルは自動的にバックアップします。
- 高可用性を提供します。 [SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promise 99.9% 以上または 99.99% の稼働時間、geo 冗長オプションに応じて選択します。
- つまりだけ格納し、を使用して、ストレージの実際の量に対してのみ料金を支払うファイルを取得するには、Azure のサービスとしてのプラットフォーム (PaaS) 機能は、Azure が自動的に処理を設定して、すべての Vm とディスクのドライブに必要な管理、サービス。
- REST API を使用して、または API のプログラミング言語を使用して Blob サービスにアクセスすることができます。 Sdk は、.NET、Java、Ruby、およびその他のユーザーに使用できます。
- Blob service 内にファイルを保存するときに行うことができます簡単に、公開されている、インターネット経由でします。
- 承認済みのユーザーのみができるように、これらのサービスがアクセスされる Blob 内のファイルをセキュリティで保護することができますか、利用できるようにユーザーに一定の時間に対してのみ一時的なアクセス トークンを行うことができます。

Azure のアプリを開発して、大量のオンプレミス環境でビデオ、イメージなどのファイル - で行われるデータを格納するときにいつでも Pdf、スプレッドシートなど、Blob service を検討します。

## <a name="creating-a-storage-account"></a>ストレージ アカウントの作成

Blob service を開始するには、Azure でストレージ アカウントを作成します。 ポータルで、次のようにクリックします**新規** -- **Data Services** -- **ストレージ** -- **簡易作成**、。URL とデータ センターの場所を入力します。 データ センターの場所は、web アプリと同じになります。

![ストレージ アカウントを作成します。](unstructured-blob-storage/_static/image1.png)

プライマリ リージョンを選択すると、コンテンツを保存して、選択したかどうか、 [geo レプリケーション](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)オプション、Azure で、国の別のリージョン内の別のデータ センターのすべてのデータのレプリカが作成されます。 たとえばを選択した場合になった Azure バック グラウンドでが、米国西部データ センターにもファイルを保存するときに、米国西部データ センターにコピー他米国のデータ センターのいずれか。 国の 1 つのリージョンで障害が発生した場合、データは安全なです。

Azure は、地政学的境界を越えてデータをレプリケートしませんファイルが別のリージョンは米国内にのみレプリケートされる、プライマリの場所が米国内にある場合は、。プライマリ ロケーションがオーストラリアである場合はのファイルがオーストラリアで別のデータ センターにレプリケートされます。

もちろん、先ほど説明したようには、スクリプトからコマンドを実行して、ストレージ アカウントを作成することができますも。 ストレージ アカウントを作成する Windows PowerShell コマンドを次に示します。

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

ストレージ アカウントを作成したら、ファイルを格納する Blob service ですぐに開始できます。

## <a name="using-blob-storage-in-the-fix-it-app"></a>Fix It アプリでの Blob storage の使用

Fix It アプリでは、写真をアップロードすることができます。

![Fix It タスクを作成します。](unstructured-blob-storage/_static/image2.png)

クリックすると**作成、FixIt**アプリケーションは指定したイメージ ファイルをアップロードし、Blob service に格納します。

### <a name="set-up-the-blob-container"></a>Blob コンテナーを設定します。

必要な Blob サービスでファイルを格納するために、*コンテナー*保存します。 Blob service のコンテナーは、ファイル システムのフォルダーに対応します。 環境の作成スクリプトで確認しましたが、[すべて自動化章](automate-everything.md)ストレージ アカウントの作成が、コンテナーが作成されません。 これの目的、`CreateAndConfigure`のメソッド、`PhotoService`クラスは、存在しない場合は、コンテナーを作成します。 このメソッドの呼び出し元、`Application_Start`メソッド*Global.asax*します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

ストレージ アカウント名とアクセス キーが格納されている、`appSettings`のコレクション、 *Web.config*ファイルを開き、コードで、`StorageUtils.StorageAccount`メソッドでは、これらの値を使用して接続文字列を作成し、接続を確立します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync`メソッドには、Blob service を表すオブジェクトを作成し、という名前"の images"の Blob service でコンテナー (フォルダー) を表すオブジェクト。

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

場合は、コンテナーの名前"の images"まだ存在しません - 初めて--新しいストレージ アカウントに対して、アプリを実行するコードはコンテナーを作成し、公開するアクセス許可を設定する場合は true になります。 (既定では、新しい blob コンテナーはプライベートであり、ストレージ アカウントにアクセスするアクセス許可を持つユーザーのみにアクセスします。)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Blob storage にアップロードした写真を格納します。

アップロードし、アプリでは、イメージ ファイルの保存、`IPhotoService`インターフェイスとのインターフェイスの実装、`PhotoService`クラス。 *PhotoService.cs*ファイルには、すべての修正、アプリで Blob サービスと通信するコードが含まれています。

ユーザーがクリックしたときに、次の MVC コント ローラー メソッドが呼び出されます**作成、FixIt**します。 このコードで`photoService`のインスタンスを参照する、`PhotoService`クラス、および`fixittask`のインスタンスを指す、`FixItTask`エンティティ クラスの新しいタスクのデータを格納します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync`メソッドで、`PhotoService`クラス、Blob service にアップロードされたファイルを格納して、新しい blob を指す URL を返します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

`CreateAndConfigure`メソッド、コードは、ストレージ アカウントに接続し、点を除いて、コンテナーが既に存在するものは、ここに「イメージ」の blob コンテナーを表すオブジェクトを作成します。

ファイル拡張子を持つ新しい GUID 値を連結して、アップロードするイメージの一意の識別子が作成されます。

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

コードでは、blob オブジェクトを作成する blob のコンテナー オブジェクトと、新しい一意識別子を使用して、ファイルの種類が、しを使用して、blob オブジェクト ファイルを blob ストレージに格納することを示すオブジェクトの属性を設定します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

最後に、blob を参照する URL を取得します。 この URL は、データベースに格納され、Fix It の web ページで、アップロードされたイメージを表示するのに使用できます。

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

この URL は、FixItTask テーブルの列の 1 つとして、データベースに格納されます。

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

データベース内の URL のみと Blob storage 内のイメージでは、Fix It アプリは、データベース維持小規模でスケーラブル、かつ低コストでストレージは安価なとテラバイトやペタバイトを処理できる、イメージが格納されます。 数百テラバイトの修正、写真を格納できる 1 つのストレージ アカウントを使用したのみ支払います。 このため、最初のギガバイトの有料 9 セントを小規模から開始し、追加のギガバイトあたり少額の他のイメージを追加できます。

### <a name="display-the-uploaded-file"></a>アップロードされたファイルを表示します。

Fix It アプリケーションは、タスクの詳細を表示するときに、アップロードされたイメージ ファイルを表示します。

![写真とタスクの詳細を修正します。](unstructured-blob-storage/_static/image3.png)

イメージを表示するにが含まれて、MVC ビューを実行する必要がありますすべて、`PhotoUrl`ブラウザーに送信される HTML 内の値。 Web サーバーとデータベースは、使用していないサイクル イメージを表示するイメージの URL に数バイトの提供のみ。 次の Razor コードで`Model`のインスタンスを指す、`FixItTask`エンティティ クラスです。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

表示するページの HTML を見ると、次のように、blob ストレージにイメージを直接指し示す URL が表示されます。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>まとめ

Fix It アプリが、Blob service と SQL database での画像 Url のみでイメージを格納する方法を説明しました。 それ以外の場合は、ほぼ無制限の数のタスクの最大スケールが可能になり、多くのコードを記述することがなく実行できますよりもはるかに小さい SQL database を保持、Blob サービスを使用します。

ストレージ アカウントには数百のテラバイトとストレージ コストは、開始月 + 小さいトランザクション料金あたりギガバイトあたり約 3 セント位置として、SQL Database ストレージよりも安価です。 最大容量ですが実際に格納する、アプリがスケーリングできる状態にすべてに余分な容量を払っているしないため、時間に対してのみを払っているできませんに注意してください。

[[次へ] の章](design-to-survive-failures.md)クラウド アプリの障害を適切に処理できることの重要性について説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

- [Azure BLOB Storage の概要、](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)します。 Mike Wood によるブログ。
- [.NET で Azure Blob ストレージ サービスを使用する方法](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)します。 MicrosoftAzure.com サイトの公式ドキュメント。 Blob を blob storage に接続する方法を紹介するコード例の後にストレージを簡単に紹介のコンテナーを作成するには、アップロードおよびダウンロード blob など。
- [フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 の部分で構成します。 Microsoft Customer ・ Advisory Team (CAT) の実際の顧客使用経験描画ストーリーと非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 Azure Storage サービスと blob の詳細については、35:13 始まるエピソード 5 を参照してください。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。 バレット キー パターンを参照してください。

> [!div class="step-by-step"]
> [前へ](data-partitioning-strategies.md)
> [次へ](design-to-survive-failures.md)
