---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: 非構造化 Blob ストレージ (Azure と実際のクラウド アプリのビルド) |Microsoft ドキュメント
author: MikeWasson
description: Azure の電子書籍と構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンと彼をできるベスト プラクティスについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: c2c82a579feb586287c40bb82eba53c5f84afaba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872573"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="ad3bf-104">非構造化 Blob ストレージ (Azure と実際のクラウド アプリのビルド)</span><span class="sxs-lookup"><span data-stu-id="ad3bf-104">Unstructured Blob Storage (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="ad3bf-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ad3bf-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ad3bf-106">[ダウンロード修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="ad3bf-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="ad3bf-107">**現実世界クラウド アプリのビルドと Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づいています。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="ad3bf-108">13 パターンについて説明します、プラクティスするのに役立つ、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="ad3bf-109">電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="ad3bf-110">前のチャプターは、パーティション分割スキームに検査し、修正、アプリが Azure Storage の Blob サービス、および Azure SQL データベース内の他のタスク データにイメージを格納する方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-110">In the previous chapter we looked at partitioning schemes and explained how the Fix It app stores images in the Azure Storage Blob service, and other task data in Azure SQL Database.</span></span> <span data-ttu-id="ad3bf-111">この章で、Blob サービスに深くし、修正コード プロジェクトでの実装方法を表示します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-111">In this chapter we go deeper into the Blob service and show how it's implemented in Fix It project code.</span></span>

## <a name="what-is-blob-storage"></a><span data-ttu-id="ad3bf-112">Blob ストレージとは何ですか。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-112">What is Blob storage?</span></span>

<span data-ttu-id="ad3bf-113">Azure Storage の Blob サービスでは、クラウド内のファイルを保存する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-113">The Azure Storage Blob service provides a way to store files in the cloud.</span></span> <span data-ttu-id="ad3bf-114">Blob サービスには、いくつかのファイルを格納するローカル ネットワーク ファイル システム上の利点があります。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-114">The Blob service has a number of advantages over storing files in a local network file system:</span></span>

- <span data-ttu-id="ad3bf-115">拡張性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-115">It's highly scalable.</span></span> <span data-ttu-id="ad3bf-116">1 つのストレージ アカウントを格納できます[数百テラバイト](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)、複数のストレージ アカウントを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-116">A single Storage account can store [hundreds of terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), and you can have multiple Storage accounts.</span></span> <span data-ttu-id="ad3bf-117">最大の Azure のお客様の一部は、数百単位からペタバイト単位を格納します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-117">Some of the biggest Azure customers store hundreds of petabytes.</span></span> <span data-ttu-id="ad3bf-118">Microsoft SkyDrive では、blob ストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-118">Microsoft SkyDrive uses blob storage.</span></span>
- <span data-ttu-id="ad3bf-119">持続性です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-119">It's durable.</span></span> <span data-ttu-id="ad3bf-120">Blob サービスに格納するすべてのファイルは自動的にバックアップされます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-120">Every file you store in the Blob service is automatically backed up.</span></span>
- <span data-ttu-id="ad3bf-121">高可用性を提供します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-121">It provides high availability.</span></span> <span data-ttu-id="ad3bf-122">[記憶域の SLA](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promise 99.9% または 99.99% の稼働時間、地理的冗長性オプションに応じてを選択します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-122">The [SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promises 99.9% or 99.99% uptime, depending on which geo-redundancy option you choose.</span></span>
- <span data-ttu-id="ad3bf-123">だけを格納してのみ、使用する記憶域の実際の容量を個別のファイルを取得するには、Azure のサービスとしてのプラットフォーム (PaaS) 機能であるし、Azure が自動的に処理を設定して、すべての Vm との必要なディスク ドライブを管理する、サービス。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-123">It's a platform-as-a-service (PaaS) feature of Azure, which means you just store and retrieve files, paying only for the actual amount of storage you use, and Azure automatically takes care of setting up and managing all of the VMs and disk drives required for the service.</span></span>
- <span data-ttu-id="ad3bf-124">REST API を使用して、または API のプログラミング言語を使用して、Blob サービスにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-124">You can access the Blob service by using a REST API or by using a programming language API.</span></span> <span data-ttu-id="ad3bf-125">Sdk は、.NET、Java、Ruby、およびその他のユーザーに利用できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-125">SDKs are available for .NET, Java, Ruby, and others.</span></span>
- <span data-ttu-id="ad3bf-126">Blob サービスにファイルを保存するときにすることができます簡単に、公開されて、インターネット経由でします。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-126">When you store a file in the Blob service, you can easily make it publicly available over the Internet.</span></span>
- <span data-ttu-id="ad3bf-127">承認済みのユーザーのみができるように、これらのサービスがアクセスされる Blob 内のファイルをセキュリティで保護することができますかに使用できるようにするユーザーが一定の期間にのみ一時的なアクセス トークンを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-127">You can secure files in the Blob service so they can accessed only by authorized users, or you can provide temporary access tokens that makes them available to someone only for a limited period of time.</span></span>

<span data-ttu-id="ad3bf-128">Azure のアプリをビルドして、大量の内部設置型環境でビデオ、イメージなど--ファイルで行われるデータを格納するいつでもは Pdf、スプレッドシートなど、Blob サービスを検討します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-128">Anytime you're building an app for Azure and you want to store a lot of data that in an on-premises environment would go in files -- such as images, videos, PDFs, spreadsheets, etc. -- consider the Blob service.</span></span>

## <a name="creating-a-storage-account"></a><span data-ttu-id="ad3bf-129">ストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-129">Creating a Storage account</span></span>

<span data-ttu-id="ad3bf-130">Blob サービスを開始するには、Azure でストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-130">To get started with the Blob service you create a Storage account in Azure.</span></span> <span data-ttu-id="ad3bf-131">ポータルで、をクリックして**新規** -- **Data Services** -- **ストレージ** -- **簡易作成**、URL とデータ センターの場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-131">In the portal, click **New** -- **Data Services** -- **Storage** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="ad3bf-132">データ センターの場所は、web アプリと同じにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-132">The data center location should be the same as your web app.</span></span>

![ストレージ アカウントを作成します。](unstructured-blob-storage/_static/image1.png)

<span data-ttu-id="ad3bf-134">プライマリ リージョンを選択して、コンテンツを保存して、選択したかどうか、 [geo レプリケーション](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)オプション、Azure のレプリカを作成、すべてのデータ、国の別の領域内の別のデータ センターにします。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-134">You pick the primary region where you want to store the content, and if you choose the [geo-replication](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) option, Azure creates replicas of all your data in a different data center in another region of the country.</span></span> <span data-ttu-id="ad3bf-135">たとえばを選択した場合、米国西部データ センターが Azure バック グラウンドでは、米国西部データ センターにもファイルを保存するときにコピー他の米のデータ センターのいずれか。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-135">For example, if you choose the Western US data center, when you store a file it goes to the Western US data center, but in the background Azure also copies it to one of the other US data centers.</span></span> <span data-ttu-id="ad3bf-136">1 つの地域、国の障害が発生した場合、データはまだも安全です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-136">If a disaster happens in one region of the country, your data is still safe.</span></span>

<span data-ttu-id="ad3bf-137">Azure は、地政学的な境界を越えてデータをレプリケートしませんファイルが米国; 内の別の領域にのみレプリケートされる場合は、プライマリの場所は、米国では、。1 次拠点がオーストラリアの場合は、ファイルのみオーストラリア内の別のデータ センターにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-137">Azure won't replicate data across geo-political boundaries: if your primary location is in the U.S., your files are only replicated to another region within the U.S.; if your primary location is Australia, your files are only replicated to another data center in Australia.</span></span>

<span data-ttu-id="ad3bf-138">もちろん、前の例とは、スクリプトからのコマンドを実行することによって、ストレージ アカウントを作成することができますも。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-138">Of course, you can also create a Storage account by executing commands from a script, as we saw earlier.</span></span> <span data-ttu-id="ad3bf-139">ストレージ アカウントを作成するには、Windows PowerShell コマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-139">Here's a Windows PowerShell command to create a Storage account:</span></span>

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

<span data-ttu-id="ad3bf-140">ストレージ アカウントを作成したら、Blob サービスにファイルを格納するをすぐに開始できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-140">Once you have a Storage account, you can immediately start storing files in the Blob service.</span></span>

## <a name="using-blob-storage-in-the-fix-it-app"></a><span data-ttu-id="ad3bf-141">Blob ストレージ、アプリを使用してください。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-141">Using Blob storage in the Fix It app</span></span>

<span data-ttu-id="ad3bf-142">修正アプリでは、写真をアップロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-142">The Fix It app enables you to upload photos.</span></span>

![修正タスクを作成します。](unstructured-blob-storage/_static/image2.png)

<span data-ttu-id="ad3bf-144">クリックすると**作成、FixIt**アプリケーションが指定されたイメージ ファイルをアップロードし、Blob サービス内に格納します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-144">When you click **Create the FixIt**, the application uploads the specified image file and stores it in the Blob service.</span></span>

### <a name="set-up-the-blob-container"></a><span data-ttu-id="ad3bf-145">Blob コンテナーをセットアップします。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-145">Set up the Blob container</span></span>

<span data-ttu-id="ad3bf-146">必要な Blob サービスで、ファイルを格納するために、*コンテナー*保存します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-146">In order to store a file in the Blob service you need a *container* to store it in.</span></span> <span data-ttu-id="ad3bf-147">Blob サービス コンテナーは、ファイル システム フォルダーに対応します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-147">A Blob service container corresponds to a file system folder.</span></span> <span data-ttu-id="ad3bf-148">環境の作成スクリプトで確認しましたが、[自動化すべて章](automate-everything.md)ストレージ アカウントの作成が、コンテナーが作成されません。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-148">The environment creation scripts that we reviewed in the [Automate Everything chapter](automate-everything.md) create the Storage account, but they don't create a container.</span></span> <span data-ttu-id="ad3bf-149">などの目的、`CreateAndConfigure`のメソッド、`PhotoService`クラスが存在しない場合は、コンテナーを作成するには。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-149">So the purpose of the `CreateAndConfigure` method of the `PhotoService` class is to create a container if it doesn't already exist.</span></span> <span data-ttu-id="ad3bf-150">このメソッドは、`Application_Start`メソッド*Global.asax*です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-150">This method is called from the `Application_Start` method in *Global.asax*.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

<span data-ttu-id="ad3bf-151">ストレージ アカウント名とアクセス キーが格納されている、`appSettings`のコレクション、 *Web.config*ファイル、およびコードで、`StorageUtils.StorageAccount`メソッドは接続文字列を作成し、接続を確立するそれらの値を使用します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-151">The storage account name and access key are stored in the `appSettings` collection of the *Web.config* file, and code in the `StorageUtils.StorageAccount` method uses those values to build a connection string and establish a connection:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

<span data-ttu-id="ad3bf-152">`CreateAndConfigureAsync`メソッドは、Blob サービスを表すオブジェクトを作成し、(フォルダー) のコンテナーを表すオブジェクトが、Blob サービスに「イメージ」をという名前します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-152">The `CreateAndConfigureAsync` method then creates an object that represents the Blob service, and an object that represents a container (folder) named "images" in the Blob service:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

<span data-ttu-id="ad3bf-153">場合は、コンテナーの名前に「イメージ」が存在しないまだ--場合は true。 初めて--新しいストレージ アカウントに対して、アプリを実行するコードはコンテナーが作成され、公開してアクセス許可が設定されます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-153">If a container named "images" doesn't exist yet -- which will be true the first time you run the app against a new storage account -- the code creates the container and sets permissions to make it public.</span></span> <span data-ttu-id="ad3bf-154">(既定では、新しい blob コンテナーは個人用で、ストレージ アカウントへのアクセス権限を持っているユーザーのみにアクセスします。)</span><span class="sxs-lookup"><span data-stu-id="ad3bf-154">(By default, new blob containers are private and are accessible only to users who have permission to access your storage account.)</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a><span data-ttu-id="ad3bf-155">ストア Blob ストレージにアップロードされた写真</span><span class="sxs-lookup"><span data-stu-id="ad3bf-155">Store the uploaded photo in Blob storage</span></span>

<span data-ttu-id="ad3bf-156">アップロードし、アプリの使用、イメージ ファイルの保存、`IPhotoService`インターフェイスとのインターフェイスの実装、`PhotoService`クラスです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-156">To upload and save the image file, the app uses an `IPhotoService` interface and an implementation of the interface in the `PhotoService` class.</span></span> <span data-ttu-id="ad3bf-157">*PhotoService.cs*ファイルには、すべての修正、アプリで、Blob サービスと通信するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-157">The *PhotoService.cs* file contains all of the code in the Fix It app that communicates with the Blob service.</span></span>

<span data-ttu-id="ad3bf-158">ユーザーがクリックしたときに、次の MVC コント ローラー メソッドが呼び出されます**作成、FixIt**です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-158">The following MVC controller method is called when the user clicks **Create the FixIt**.</span></span> <span data-ttu-id="ad3bf-159">このコードで`photoService`のインスタンスを指す、`PhotoService`クラス、および`fixittask`のインスタンスを参照する、`FixItTask`新しいタスクのデータを格納するエンティティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-159">In this code, `photoService` refers to an instance of the `PhotoService` class, and `fixittask` refers to an instance of the `FixItTask` entity class that stores data for a new task.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

<span data-ttu-id="ad3bf-160">`UploadPhotoAsync`メソッドで、`PhotoService`クラスが、Blob サービスにアップロードされたファイルを格納し、新しい blob の URL を返します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-160">The `UploadPhotoAsync` method in the `PhotoService` class stores the uploaded file in the Blob service and returns a URL that points to the new blob.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

<span data-ttu-id="ad3bf-161">同様、`CreateAndConfigure`メソッド、コードは、ストレージ アカウントに接続し、ここで、前提としています、コンテナーが既に存在する点を除いて、「イメージ」の blob コンテナーを表すオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-161">As in the `CreateAndConfigure` method, the code connects to the storage account and creates an object that represents the "images" blob container, except here it assumes the container already exists.</span></span>

<span data-ttu-id="ad3bf-162">ファイル拡張子を持つ新しい GUID 値を連結することにより、アップロードするイメージの一意の識別子を作成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-162">Then it creates a unique identifier for the image about to be uploaded, by concatenating a new GUID value with the file extension:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

<span data-ttu-id="ad3bf-163">コードでは、blob オブジェクトを作成する blob コンテナー オブジェクトおよび新しい一意の識別子を使用して、を示すファイルの種類には、blob ストレージにファイルを格納する blob オブジェクトを使用してそのオブジェクトの属性を設定します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-163">The code then uses the blob container object and the new unique identifier to create a blob object, sets an attribute on that object indicating what kind of file it is, and then uses the blob object to store the file in blob storage.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

<span data-ttu-id="ad3bf-164">最後に、blob を参照する URL を取得します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-164">Finally, it gets a URL that references the blob.</span></span> <span data-ttu-id="ad3bf-165">この URL は、データベースに格納され、アップロードされたイメージを表示する解決、web ページで使用できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-165">This URL will be stored in the database and can be used in Fix It web pages to display the uploaded image.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

<span data-ttu-id="ad3bf-166">この URL は、FixItTask テーブルの列の 1 つとして、データベースに格納されます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-166">This URL is stored in the database as one of the columns of the FixItTask table.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

<span data-ttu-id="ad3bf-167">データベース内の URL のみと Blob ストレージ内のイメージでは、修正アプリ間もデータベースを小規模でスケーラブル、かつ低コスト ストレージは安価とテラバイトや単位からペタバイト単位を処理できるイメージが格納されます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-167">With only the URL in the database, and images in Blob storage, the Fix It app keeps the database small, scalable, and inexpensive, while the images are stored where storage is cheap and capable of handling terabytes or petabytes.</span></span> <span data-ttu-id="ad3bf-168">1 つのストレージ アカウントが数百テラバイトの修正、写真を保存し、分だけ支払う際に使用します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-168">One storage account can store hundreds of terabytes of Fix It photos, and you only pay for what you use.</span></span> <span data-ttu-id="ad3bf-169">このため、最初のギガバイトの小規模の料金を払っている 9 セント オフで開始し、追加のギガバイトあたりペニーのイメージを追加できます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-169">So you can start off small paying 9 cents for the first gigabyte, and add more images for pennies per additional gigabyte.</span></span>

### <a name="display-the-uploaded-file"></a><span data-ttu-id="ad3bf-170">アップロードされたファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-170">Display the uploaded file</span></span>

<span data-ttu-id="ad3bf-171">修正アプリケーションは、タスクの詳細を表示するときに、アップロードされたイメージ ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-171">The Fix It application displays the uploaded image file when it displays details for a task.</span></span>

![写真とそのタスクの詳細を修正します。](unstructured-blob-storage/_static/image3.png)

<span data-ttu-id="ad3bf-173">表示するには、イメージを行うには、MVC ビューには、すべてを含める、`PhotoUrl`ブラウザーに送信される HTML の値。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-173">To display the image, all the MVC view has to do is include the `PhotoUrl` value in the HTML sent to the browser.</span></span> <span data-ttu-id="ad3bf-174">Web サーバーとデータベースは、使用していないサイクル イメージを表示するイメージの URL をいくつかのバイトをサービスのみです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-174">The web server and the database are not using cycles to display the image, they are only serving up a few bytes to the image URL.</span></span> <span data-ttu-id="ad3bf-175">次の Razor コードで`Model`のインスタンスを指す、`FixItTask`エンティティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-175">In the following Razor code, `Model` refers to an instance of the `FixItTask` entity class.</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

<span data-ttu-id="ad3bf-176">表示されるページの HTML を見ると、次のように、blob ストレージ内のイメージを直接指す URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-176">If you look at the HTML of the page that displays, you see the URL pointing directly to the image in blob storage, something like this:</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a><span data-ttu-id="ad3bf-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="ad3bf-177">Summary</span></span>

<span data-ttu-id="ad3bf-178">修正アプリケーションが、Blob サービスと SQL データベースでの画像 Url のみでイメージを格納する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-178">You've seen how the Fix It app stores images in the Blob service and only image URLs in the SQL database.</span></span> <span data-ttu-id="ad3bf-179">SQL データベースをそれ以外の場合、ほぼ無制限の数のタスクの最大小数点以下桁数が可能になりになり、大量のコードを記述することがなく実行できますよりもはるかに小さい保持、Blob サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-179">Using the Blob service keeps the SQL database much smaller than it otherwise would be, makes it possible to scale up to an almost unlimited number of tasks, and can be done without writing a lot of code.</span></span>

<span data-ttu-id="ad3bf-180">ストレージ アカウントに数百テラバイトのことができ、記憶域のコストは開始月と小さなトランザクション料金あたりギガバイトあたり約 3 セント位置として、SQL データベース ストレージよりもはるかに低コストです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-180">You can have hundreds of terabytes in a storage account, and the storage cost is much less expensive than SQL Database storage, starting at about 3 cents per gigabyte per month plus a small transaction charge.</span></span> <span data-ttu-id="ad3bf-181">いない料金を支払っている最大容量は実際に格納する、ので、アプリをスケールする準備が、そのすべての余分な容量を怠る量のみに留意してください。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-181">And keep in mind that you're not paying for the maximum capacity but only for the amount you actually store, so your app is ready to scale but you're not paying for all that extra capacity.</span></span>

<span data-ttu-id="ad3bf-182">[次のチャプター](design-to-survive-failures.md)クラウド アプリケーションのエラーを適切に処理の対応の重要性について説明します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-182">In the [next chapter](design-to-survive-failures.md) we'll talk about the importance of making a cloud app capable of gracefully handling failures.</span></span>

## <a name="resources"></a><span data-ttu-id="ad3bf-183">リソース</span><span class="sxs-lookup"><span data-stu-id="ad3bf-183">Resources</span></span>

<span data-ttu-id="ad3bf-184">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-184">For more information see the following resources:</span></span>

- <span data-ttu-id="ad3bf-185">[Azure BLOB ストレージの概要については](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-185">[An Introduction to Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/).</span></span> <span data-ttu-id="ad3bf-186">Mike 木ブログ。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-186">Blog by Mike Wood.</span></span>
- <span data-ttu-id="ad3bf-187">[.NET で Azure Blob ストレージ サービスを使用する方法](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-187">[How to use the Azure Blob Storage Service in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="ad3bf-188">MicrosoftAzure.com サイトに関する正式なドキュメントです。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-188">Official documentation on the MicrosoftAzure.com site.</span></span> <span data-ttu-id="ad3bf-189">簡単な概要を blob ストレージの blob ストレージに接続する方法を示すコード例に続けてコンテナーを作成するには、アップロードおよびなど、blob をダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-189">A brief introduction to blob storage followed by code examples showing how to connect to blob storage, create containers, upload and download blobs, etc.</span></span>
- <span data-ttu-id="ad3bf-190">[フェール セーフ: スケーラブル、かつ回復力のクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-190">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="ad3bf-191">Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 つの部分で構成します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-191">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="ad3bf-192">実際のお客様と Microsoft Customer ・ Advisory Team (CAT) エクスペリエンスから抽出されたストーリーで非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-192">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="ad3bf-193">Azure ストレージ サービスと blob の詳細については、35:13 始まるエピソード 5 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-193">For a discussion of Azure Storage service and blobs, see episode 5 starting at 35:13.</span></span>
- <span data-ttu-id="ad3bf-194">[Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-194">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="ad3bf-195">バレット キー パターンを参照してください。</span><span class="sxs-lookup"><span data-stu-id="ad3bf-195">See Valet Key pattern.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ad3bf-196">[前へ](data-partitioning-strategies.md)
> [次へ](design-to-survive-failures.md)</span><span class="sxs-lookup"><span data-stu-id="ad3bf-196">[Previous](data-partitioning-strategies.md)
[Next](design-to-survive-failures.md)</span></span>
