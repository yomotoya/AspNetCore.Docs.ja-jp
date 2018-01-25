---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "非構造化 Blob ストレージ (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント"
author: MikeWasson
description: "Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 489769533a26c99404c6a5186d66f560385dcffd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>非構造化 Blob ストレージ (Azure と実際のクラウド アプリのビルド)
====================
によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)

[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。 13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。 電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。


前のチャプターは、パーティション分割スキームに検査し、修正、アプリが Azure Storage の Blob サービス、および Azure SQL データベース内の他のタスク データにイメージを格納する方法を説明します。 この章で、Blob サービスに深くし、修正コード プロジェクトでの実装方法を表示します。

## <a name="what-is-blob-storage"></a>Blob ストレージとは何ですか。

Azure Storage の Blob サービスでは、クラウド内のファイルを保存する方法を提供します。 Blob サービスには、いくつかのファイルを格納するローカル ネットワーク ファイル システム上の利点があります。

- 拡張性が高くなります。 1 つのストレージ アカウントを格納できます[数百テラバイト](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)、複数のストレージ アカウントを持つことができます。 最大の Azure のお客様の一部は、数百単位からペタバイト単位を格納します。 Microsoft SkyDrive では、blob ストレージを使用します。
- 持続性です。 Blob サービスに格納するすべてのファイルは自動的にバックアップされます。
- 高可用性を提供します。 [記憶域の SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promise 99.9% または 99.99% の稼働時間、地理的冗長性オプションに応じてを選択します。
- だけを格納してのみ、使用する記憶域の実際の容量を個別のファイルを取得するには、Azure のサービスとしてのプラットフォーム (PaaS) 機能であるし、Azure が自動的に処理を設定して、すべての Vm との必要なディスク ドライブを管理する、サービス。
- REST API を使用して、または API のプログラミング言語を使用して、Blob サービスにアクセスすることができます。 Sdk は、.NET、Java、Ruby、およびその他のユーザーに利用できます。
- Blob サービスにファイルを保存するときにすることができます簡単に、公開されて、インターネット経由でします。
- 承認済みのユーザーのみができるように、これらのサービスがアクセスされる Blob 内のファイルをセキュリティで保護することができますかに使用できるようにするユーザーが一定の期間にのみ一時的なアクセス トークンを提供することができます。

Azure のアプリをビルドして、大量の内部設置型環境でビデオ、イメージなど--ファイルで行われるデータを格納するいつでもは Pdf、スプレッドシートなど、Blob サービスを検討します。

## <a name="creating-a-storage-account"></a>ストレージ アカウントを作成します。

Blob サービスを開始するには、Azure でストレージ アカウントを作成します。 ポータルで、をクリックして**新規** -- **Data Services** -- **ストレージ** -- **簡易作成**、URL とデータ センターの場所を入力します。 データ センターの場所は、web アプリと同じにする必要があります。

![ストレージ アカウントを作成します。](unstructured-blob-storage/_static/image1.png)

プライマリ リージョンを選択して、コンテンツを保存して、選択したかどうか、 [geo レプリケーション](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)オプション、Azure のレプリカを作成、すべてのデータ、国の別の領域内の別のデータ センターにします。 たとえばを選択した場合、米国西部データ センターが Azure バック グラウンドでは、米国西部データ センターにもファイルを保存するときにコピー他の米のデータ センターのいずれか。 1 つの地域、国の障害が発生した場合、データはまだも安全です。

Azure は、地政学的な境界を越えてデータをレプリケートしませんファイルが米国; 内の別の領域にのみレプリケートされる場合は、プライマリの場所は、米国では、。1 次拠点がオーストラリアの場合は、ファイルのみオーストラリア内の別のデータ センターにレプリケートされます。

もちろん、前の例とは、スクリプトからのコマンドを実行することによって、ストレージ アカウントを作成することができますも。 ストレージ アカウントを作成するには、Windows PowerShell コマンドを次に示します。

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

ストレージ アカウントを作成したら、Blob サービスにファイルを格納するをすぐに開始できます。

## <a name="using-blob-storage-in-the-fix-it-app"></a>Blob ストレージ、アプリを使用してください。

修正アプリでは、写真をアップロードすることができます。

![修正タスクを作成します。](unstructured-blob-storage/_static/image2.png)

クリックすると**作成、FixIt**アプリケーションが指定されたイメージ ファイルをアップロードし、Blob サービス内に格納します。

### <a name="set-up-the-blob-container"></a>Blob コンテナーをセットアップします。

必要な Blob サービスで、ファイルを格納するために、*コンテナー*保存します。 Blob サービス コンテナーは、ファイル システム フォルダーに対応します。 環境の作成スクリプトで確認しましたが、[自動化すべて章](automate-everything.md)ストレージ アカウントの作成が、コンテナーが作成されません。 などの目的、`CreateAndConfigure`のメソッド、`PhotoService`クラスが存在しない場合は、コンテナーを作成するには。 このメソッドは、`Application_Start`メソッド*Global.asax*です。

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

ストレージ アカウント名とアクセス キーが格納されている、`appSettings`のコレクション、 *Web.config*ファイル、およびコードで、`StorageUtils.StorageAccount`メソッドは接続文字列を作成し、接続を確立するそれらの値を使用します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

`CreateAndConfigureAsync`メソッドは、Blob サービスを表すオブジェクトを作成し、(フォルダー) のコンテナーを表すオブジェクトが、Blob サービスに「イメージ」をという名前します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

場合は、コンテナーの名前に「イメージ」が存在しないまだ--場合は true。 初めて--新しいストレージ アカウントに対して、アプリを実行するコードはコンテナーが作成され、公開してアクセス許可が設定されます。 (既定では、新しい blob コンテナーは個人用で、ストレージ アカウントへのアクセス権限を持っているユーザーのみにアクセスします。)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>ストア Blob ストレージにアップロードされた写真

アップロードし、アプリの使用、イメージ ファイルの保存、`IPhotoService`インターフェイスとのインターフェイスの実装、`PhotoService`クラスです。 *PhotoService.cs*ファイルには、すべての修正、アプリで、Blob サービスと通信するコードが含まれています。

ユーザーがクリックしたときに、次の MVC コント ローラー メソッドが呼び出されます**作成、FixIt**です。 このコードで`photoService`のインスタンスを指す、`PhotoService`クラス、および`fixittask`のインスタンスを参照する、`FixItTask`新しいタスクのデータを格納するエンティティ クラスです。

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

`UploadPhotoAsync`メソッドで、`PhotoService`クラスが、Blob サービスにアップロードされたファイルを格納し、新しい blob の URL を返します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

同様、`CreateAndConfigure`メソッド、コードは、ストレージ アカウントに接続し、ここで、前提としています、コンテナーが既に存在する点を除いて、「イメージ」の blob コンテナーを表すオブジェクトを作成します。

ファイル拡張子を持つ新しい GUID 値を連結することにより、アップロードするイメージの一意の識別子を作成します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

コードでは、blob オブジェクトを作成する blob コンテナー オブジェクトおよび新しい一意の識別子を使用して、を示すファイルの種類には、blob ストレージにファイルを格納する blob オブジェクトを使用してそのオブジェクトの属性を設定します。

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

最後に、blob を参照する URL を取得します。 この URL は、データベースに格納され、アップロードされたイメージを表示する解決、web ページで使用できます。

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

この URL は、FixItTask テーブルの列の 1 つとして、データベースに格納されます。

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

データベース内の URL のみと Blob ストレージ内のイメージでは、修正アプリ間もデータベースを小規模でスケーラブル、かつ低コスト ストレージは安価とテラバイトや単位からペタバイト単位を処理できるイメージが格納されます。 1 つのストレージ アカウントが数百テラバイトの修正、写真を保存し、分だけ支払う際に使用します。 このため、最初のギガバイトの小規模の料金を払っている 9 セント オフで開始し、追加のギガバイトあたりペニーのイメージを追加できます。

### <a name="display-the-uploaded-file"></a>アップロードされたファイルを表示します。

修正アプリケーションは、タスクの詳細を表示するときに、アップロードされたイメージ ファイルを表示します。

![写真とそのタスクの詳細を修正します。](unstructured-blob-storage/_static/image3.png)

表示するには、イメージを行うには、MVC ビューには、すべてを含める、`PhotoUrl`ブラウザーに送信される HTML の値。 Web サーバーとデータベースは、使用していないサイクル イメージを表示するイメージの URL をいくつかのバイトをサービスのみです。 次の Razor コードで`Model`のインスタンスを指す、`FixItTask`エンティティ クラスです。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

表示されるページの HTML を見ると、次のように、blob ストレージ内のイメージを直接指す URL が表示されます。

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>まとめ

修正アプリケーションが、Blob サービスと SQL データベースでの画像 Url のみでイメージを格納する方法を説明しました。 SQL データベースをそれ以外の場合、ほぼ無制限の数のタスクの最大小数点以下桁数が可能になりになり、大量のコードを記述することがなく実行できますよりもはるかに小さい保持、Blob サービスを使用します。

ストレージ アカウントに数百テラバイトのことができ、記憶域のコストは開始月と小さなトランザクション料金あたりギガバイトあたり約 3 セント位置として、SQL データベース ストレージよりもはるかに低コストです。 いない料金を支払っている最大容量は実際に格納する、ので、アプリをスケールする準備が、そのすべての余分な容量を怠る量のみに留意してください。

[次のチャプター](design-to-survive-failures.md)クラウド アプリケーションのエラーを適切に処理の対応の重要性について説明します。

## <a name="resources"></a>リソース

詳細については、次のリソースを参照してください。

- [Azure BLOB ストレージの概要については](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)します。 Mike 木ブログ。
- [.NET で Azure Blob ストレージ サービスを使用する方法](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)です。 MicrosoftAzure.com サイトに関する正式なドキュメントです。 簡単な概要を blob ストレージの blob ストレージに接続する方法を示すコード例に続けてコンテナーを作成するには、アップロードおよびなど、blob をダウンロードします。
- [フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。 Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 つの部分で構成します。 実際のお客様と Microsoft Customer ・ Advisory Team (CAT) エクスペリエンスから抽出されたストーリーで非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。 Azure ストレージ サービスと blob の詳細については、35:13 始まるエピソード 5 を参照してください。
- [Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。 バレット キー パターンを参照してください。

>[!div class="step-by-step"]
[前へ](data-partitioning-strategies.md)
[次へ](design-to-survive-failures.md)
