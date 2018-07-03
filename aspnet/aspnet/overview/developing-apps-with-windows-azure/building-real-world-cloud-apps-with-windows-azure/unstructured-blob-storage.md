---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: 非構造化 Blob Storage (Azure で現実世界のクラウド アプリの構築) |Microsoft Docs
author: MikeWasson
description: Azure 電子書籍で構築実世界クラウド アプリは、Scott Guthrie が開発したプレゼンテーションに基づいています。 13 のパターンとプラクティスを彼がについて説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 1840bea3b4183838ffdfe710e987b864a05d53fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364185"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="40336-104">非構造化 Blob Storage (Azure で現実世界のクラウド アプリの構築)</span><span class="sxs-lookup"><span data-stu-id="40336-104">Unstructured Blob Storage (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="40336-105">によって[Mike Wasson](https://github.com/MikeWasson)、 [Rick Anderson](https://github.com/Rick-Anderson)、 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="40336-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="40336-106">[ダウンロードその修正プロジェクト](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)または[電子書籍をダウンロード](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="40336-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="40336-107">**構築現実世界の Cloud Apps with Azure**電子書籍は Scott Guthrie が開発したプレゼンテーションに基づきます。</span><span class="sxs-lookup"><span data-stu-id="40336-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="40336-108">13 のパターンについて説明しするのに役立つプラクティスは、クラウドの web アプリの開発が成功します。</span><span class="sxs-lookup"><span data-stu-id="40336-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="40336-109">電子書籍の詳細については、次を参照してください。[第 1 章](introduction.md)します。</span><span class="sxs-lookup"><span data-stu-id="40336-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="40336-110">前の章でパーティション構成を調べるし、Fix It アプリが Azure Storage Blob サービス、およびその他のタスクのデータを Azure SQL Database でのイメージを格納する方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="40336-110">In the previous chapter we looked at partitioning schemes and explained how the Fix It app stores images in the Azure Storage Blob service, and other task data in Azure SQL Database.</span></span> <span data-ttu-id="40336-111">この章では Blob service に進むし、Fix It プロジェクトのコードでの実装方法を説明します。</span><span class="sxs-lookup"><span data-stu-id="40336-111">In this chapter we go deeper into the Blob service and show how it's implemented in Fix It project code.</span></span>

## <a name="what-is-blob-storage"></a><span data-ttu-id="40336-112">Blob storage とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="40336-112">What is Blob storage?</span></span>

<span data-ttu-id="40336-113">Azure Storage Blob service は、クラウドにファイルを格納する方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="40336-113">The Azure Storage Blob service provides a way to store files in the cloud.</span></span> <span data-ttu-id="40336-114">Blob service では、いくつかのファイルを格納するローカル ネットワーク ファイル システム上の利点があります。</span><span class="sxs-lookup"><span data-stu-id="40336-114">The Blob service has a number of advantages over storing files in a local network file system:</span></span>

- <span data-ttu-id="40336-115">このコマンドは、高いスケーラビリティです。</span><span class="sxs-lookup"><span data-stu-id="40336-115">It's highly scalable.</span></span> <span data-ttu-id="40336-116">1 つのストレージ アカウントに格納できる[数百のテラバイト](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)、複数のストレージ アカウントを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="40336-116">A single Storage account can store [hundreds of terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), and you can have multiple Storage accounts.</span></span> <span data-ttu-id="40336-117">最大の Azure 顧客の一部は、数百ペタバイト規模を格納します。</span><span class="sxs-lookup"><span data-stu-id="40336-117">Some of the biggest Azure customers store hundreds of petabytes.</span></span> <span data-ttu-id="40336-118">Microsoft SkyDrive では、blob ストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="40336-118">Microsoft SkyDrive uses blob storage.</span></span>
- <span data-ttu-id="40336-119">持続性になります。</span><span class="sxs-lookup"><span data-stu-id="40336-119">It's durable.</span></span> <span data-ttu-id="40336-120">Blob service に格納するすべてのファイルは自動的にバックアップします。</span><span class="sxs-lookup"><span data-stu-id="40336-120">Every file you store in the Blob service is automatically backed up.</span></span>
- <span data-ttu-id="40336-121">高可用性を提供します。</span><span class="sxs-lookup"><span data-stu-id="40336-121">It provides high availability.</span></span> <span data-ttu-id="40336-122">[SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promise 99.9% 以上または 99.99% の稼働時間、geo 冗長オプションに応じて選択します。</span><span class="sxs-lookup"><span data-stu-id="40336-122">The [SLA for Storage](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promises 99.9% or 99.99% uptime, depending on which geo-redundancy option you choose.</span></span>
- <span data-ttu-id="40336-123">つまりだけ格納し、を使用して、ストレージの実際の量に対してのみ料金を支払うファイルを取得するには、Azure のサービスとしてのプラットフォーム (PaaS) 機能は、Azure が自動的に処理を設定して、すべての Vm とディスクのドライブに必要な管理、サービス。</span><span class="sxs-lookup"><span data-stu-id="40336-123">It's a platform-as-a-service (PaaS) feature of Azure, which means you just store and retrieve files, paying only for the actual amount of storage you use, and Azure automatically takes care of setting up and managing all of the VMs and disk drives required for the service.</span></span>
- <span data-ttu-id="40336-124">REST API を使用して、または API のプログラミング言語を使用して Blob サービスにアクセスすることができます。</span><span class="sxs-lookup"><span data-stu-id="40336-124">You can access the Blob service by using a REST API or by using a programming language API.</span></span> <span data-ttu-id="40336-125">Sdk は、.NET、Java、Ruby、およびその他のユーザーに使用できます。</span><span class="sxs-lookup"><span data-stu-id="40336-125">SDKs are available for .NET, Java, Ruby, and others.</span></span>
- <span data-ttu-id="40336-126">Blob service 内にファイルを保存するときに行うことができます簡単に、公開されている、インターネット経由でします。</span><span class="sxs-lookup"><span data-stu-id="40336-126">When you store a file in the Blob service, you can easily make it publicly available over the Internet.</span></span>
- <span data-ttu-id="40336-127">承認済みのユーザーのみができるように、これらのサービスがアクセスされる Blob 内のファイルをセキュリティで保護することができますか、利用できるようにユーザーに一定の時間に対してのみ一時的なアクセス トークンを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="40336-127">You can secure files in the Blob service so they can accessed only by authorized users, or you can provide temporary access tokens that makes them available to someone only for a limited period of time.</span></span>

<span data-ttu-id="40336-128">Azure のアプリを開発して、大量のオンプレミス環境でビデオ、イメージなどのファイル - で行われるデータを格納するときにいつでも Pdf、スプレッドシートなど、Blob service を検討します。</span><span class="sxs-lookup"><span data-stu-id="40336-128">Anytime you're building an app for Azure and you want to store a lot of data that in an on-premises environment would go in files -- such as images, videos, PDFs, spreadsheets, etc. -- consider the Blob service.</span></span>

## <a name="creating-a-storage-account"></a><span data-ttu-id="40336-129">ストレージ アカウントの作成</span><span class="sxs-lookup"><span data-stu-id="40336-129">Creating a Storage account</span></span>

<span data-ttu-id="40336-130">Blob service を開始するには、Azure でストレージ アカウントを作成します。</span><span class="sxs-lookup"><span data-stu-id="40336-130">To get started with the Blob service you create a Storage account in Azure.</span></span> <span data-ttu-id="40336-131">ポータルで、次のようにクリックします**新規** -- **Data Services** -- **ストレージ** -- **簡易作成**、。URL とデータ センターの場所を入力します。</span><span class="sxs-lookup"><span data-stu-id="40336-131">In the portal, click **New** -- **Data Services** -- **Storage** -- **Quick Create**, and then enter a URL and a data center location.</span></span> <span data-ttu-id="40336-132">データ センターの場所は、web アプリと同じになります。</span><span class="sxs-lookup"><span data-stu-id="40336-132">The data center location should be the same as your web app.</span></span>

![ストレージ アカウントを作成します。](unstructured-blob-storage/_static/image1.png)

<span data-ttu-id="40336-134">プライマリ リージョンを選択すると、コンテンツを保存して、選択したかどうか、 [geo レプリケーション](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage)オプション、Azure で、国の別のリージョン内の別のデータ センターのすべてのデータのレプリカが作成されます。</span><span class="sxs-lookup"><span data-stu-id="40336-134">You pick the primary region where you want to store the content, and if you choose the [geo-replication](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) option, Azure creates replicas of all your data in a different data center in another region of the country.</span></span> <span data-ttu-id="40336-135">たとえばを選択した場合になった Azure バック グラウンドでが、米国西部データ センターにもファイルを保存するときに、米国西部データ センターにコピー他米国のデータ センターのいずれか。</span><span class="sxs-lookup"><span data-stu-id="40336-135">For example, if you choose the Western US data center, when you store a file it goes to the Western US data center, but in the background Azure also copies it to one of the other US data centers.</span></span> <span data-ttu-id="40336-136">国の 1 つのリージョンで障害が発生した場合、データは安全なです。</span><span class="sxs-lookup"><span data-stu-id="40336-136">If a disaster happens in one region of the country, your data is still safe.</span></span>

<span data-ttu-id="40336-137">Azure は、地政学的境界を越えてデータをレプリケートしませんファイルが別のリージョンは米国内にのみレプリケートされる、プライマリの場所が米国内にある場合は、。プライマリ ロケーションがオーストラリアである場合はのファイルがオーストラリアで別のデータ センターにレプリケートされます。</span><span class="sxs-lookup"><span data-stu-id="40336-137">Azure won't replicate data across geo-political boundaries: if your primary location is in the U.S., your files are only replicated to another region within the U.S.; if your primary location is Australia, your files are only replicated to another data center in Australia.</span></span>

<span data-ttu-id="40336-138">もちろん、先ほど説明したようには、スクリプトからコマンドを実行して、ストレージ アカウントを作成することができますも。</span><span class="sxs-lookup"><span data-stu-id="40336-138">Of course, you can also create a Storage account by executing commands from a script, as we saw earlier.</span></span> <span data-ttu-id="40336-139">ストレージ アカウントを作成する Windows PowerShell コマンドを次に示します。</span><span class="sxs-lookup"><span data-stu-id="40336-139">Here's a Windows PowerShell command to create a Storage account:</span></span>

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

<span data-ttu-id="40336-140">ストレージ アカウントを作成したら、ファイルを格納する Blob service ですぐに開始できます。</span><span class="sxs-lookup"><span data-stu-id="40336-140">Once you have a Storage account, you can immediately start storing files in the Blob service.</span></span>

## <a name="using-blob-storage-in-the-fix-it-app"></a><span data-ttu-id="40336-141">Fix It アプリでの Blob storage の使用</span><span class="sxs-lookup"><span data-stu-id="40336-141">Using Blob storage in the Fix It app</span></span>

<span data-ttu-id="40336-142">Fix It アプリでは、写真をアップロードすることができます。</span><span class="sxs-lookup"><span data-stu-id="40336-142">The Fix It app enables you to upload photos.</span></span>

![Fix It タスクを作成します。](unstructured-blob-storage/_static/image2.png)

<span data-ttu-id="40336-144">クリックすると**作成、FixIt**アプリケーションは指定したイメージ ファイルをアップロードし、Blob service に格納します。</span><span class="sxs-lookup"><span data-stu-id="40336-144">When you click **Create the FixIt**, the application uploads the specified image file and stores it in the Blob service.</span></span>

### <a name="set-up-the-blob-container"></a><span data-ttu-id="40336-145">Blob コンテナーを設定します。</span><span class="sxs-lookup"><span data-stu-id="40336-145">Set up the Blob container</span></span>

<span data-ttu-id="40336-146">必要な Blob サービスでファイルを格納するために、*コンテナー*保存します。</span><span class="sxs-lookup"><span data-stu-id="40336-146">In order to store a file in the Blob service you need a *container* to store it in.</span></span> <span data-ttu-id="40336-147">Blob service のコンテナーは、ファイル システムのフォルダーに対応します。</span><span class="sxs-lookup"><span data-stu-id="40336-147">A Blob service container corresponds to a file system folder.</span></span> <span data-ttu-id="40336-148">環境の作成スクリプトで確認しましたが、[すべて自動化章](automate-everything.md)ストレージ アカウントの作成が、コンテナーが作成されません。</span><span class="sxs-lookup"><span data-stu-id="40336-148">The environment creation scripts that we reviewed in the [Automate Everything chapter](automate-everything.md) create the Storage account, but they don't create a container.</span></span> <span data-ttu-id="40336-149">これの目的、`CreateAndConfigure`のメソッド、`PhotoService`クラスは、存在しない場合は、コンテナーを作成します。</span><span class="sxs-lookup"><span data-stu-id="40336-149">So the purpose of the `CreateAndConfigure` method of the `PhotoService` class is to create a container if it doesn't already exist.</span></span> <span data-ttu-id="40336-150">このメソッドの呼び出し元、`Application_Start`メソッド*Global.asax*します。</span><span class="sxs-lookup"><span data-stu-id="40336-150">This method is called from the `Application_Start` method in *Global.asax*.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

<span data-ttu-id="40336-151">ストレージ アカウント名とアクセス キーが格納されている、`appSettings`のコレクション、 *Web.config*ファイルを開き、コードで、`StorageUtils.StorageAccount`メソッドでは、これらの値を使用して接続文字列を作成し、接続を確立します。</span><span class="sxs-lookup"><span data-stu-id="40336-151">The storage account name and access key are stored in the `appSettings` collection of the *Web.config* file, and code in the `StorageUtils.StorageAccount` method uses those values to build a connection string and establish a connection:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

<span data-ttu-id="40336-152">`CreateAndConfigureAsync`メソッドには、Blob service を表すオブジェクトを作成し、という名前"の images"の Blob service でコンテナー (フォルダー) を表すオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="40336-152">The `CreateAndConfigureAsync` method then creates an object that represents the Blob service, and an object that represents a container (folder) named "images" in the Blob service:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

<span data-ttu-id="40336-153">場合は、コンテナーの名前"の images"まだ存在しません - 初めて--新しいストレージ アカウントに対して、アプリを実行するコードはコンテナーを作成し、公開するアクセス許可を設定する場合は true になります。</span><span class="sxs-lookup"><span data-stu-id="40336-153">If a container named "images" doesn't exist yet -- which will be true the first time you run the app against a new storage account -- the code creates the container and sets permissions to make it public.</span></span> <span data-ttu-id="40336-154">(既定では、新しい blob コンテナーはプライベートであり、ストレージ アカウントにアクセスするアクセス許可を持つユーザーのみにアクセスします。)</span><span class="sxs-lookup"><span data-stu-id="40336-154">(By default, new blob containers are private and are accessible only to users who have permission to access your storage account.)</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a><span data-ttu-id="40336-155">Blob storage にアップロードした写真を格納します。</span><span class="sxs-lookup"><span data-stu-id="40336-155">Store the uploaded photo in Blob storage</span></span>

<span data-ttu-id="40336-156">アップロードし、アプリでは、イメージ ファイルの保存、`IPhotoService`インターフェイスとのインターフェイスの実装、`PhotoService`クラス。</span><span class="sxs-lookup"><span data-stu-id="40336-156">To upload and save the image file, the app uses an `IPhotoService` interface and an implementation of the interface in the `PhotoService` class.</span></span> <span data-ttu-id="40336-157">*PhotoService.cs*ファイルには、すべての修正、アプリで Blob サービスと通信するコードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="40336-157">The *PhotoService.cs* file contains all of the code in the Fix It app that communicates with the Blob service.</span></span>

<span data-ttu-id="40336-158">ユーザーがクリックしたときに、次の MVC コント ローラー メソッドが呼び出されます**作成、FixIt**します。</span><span class="sxs-lookup"><span data-stu-id="40336-158">The following MVC controller method is called when the user clicks **Create the FixIt**.</span></span> <span data-ttu-id="40336-159">このコードで`photoService`のインスタンスを参照する、`PhotoService`クラス、および`fixittask`のインスタンスを指す、`FixItTask`エンティティ クラスの新しいタスクのデータを格納します。</span><span class="sxs-lookup"><span data-stu-id="40336-159">In this code, `photoService` refers to an instance of the `PhotoService` class, and `fixittask` refers to an instance of the `FixItTask` entity class that stores data for a new task.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

<span data-ttu-id="40336-160">`UploadPhotoAsync`メソッドで、`PhotoService`クラス、Blob service にアップロードされたファイルを格納して、新しい blob を指す URL を返します。</span><span class="sxs-lookup"><span data-stu-id="40336-160">The `UploadPhotoAsync` method in the `PhotoService` class stores the uploaded file in the Blob service and returns a URL that points to the new blob.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

<span data-ttu-id="40336-161">`CreateAndConfigure`メソッド、コードは、ストレージ アカウントに接続し、点を除いて、コンテナーが既に存在するものは、ここに「イメージ」の blob コンテナーを表すオブジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="40336-161">As in the `CreateAndConfigure` method, the code connects to the storage account and creates an object that represents the "images" blob container, except here it assumes the container already exists.</span></span>

<span data-ttu-id="40336-162">ファイル拡張子を持つ新しい GUID 値を連結して、アップロードするイメージの一意の識別子が作成されます。</span><span class="sxs-lookup"><span data-stu-id="40336-162">Then it creates a unique identifier for the image about to be uploaded, by concatenating a new GUID value with the file extension:</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

<span data-ttu-id="40336-163">コードでは、blob オブジェクトを作成する blob のコンテナー オブジェクトと、新しい一意識別子を使用して、ファイルの種類が、しを使用して、blob オブジェクト ファイルを blob ストレージに格納することを示すオブジェクトの属性を設定します。</span><span class="sxs-lookup"><span data-stu-id="40336-163">The code then uses the blob container object and the new unique identifier to create a blob object, sets an attribute on that object indicating what kind of file it is, and then uses the blob object to store the file in blob storage.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

<span data-ttu-id="40336-164">最後に、blob を参照する URL を取得します。</span><span class="sxs-lookup"><span data-stu-id="40336-164">Finally, it gets a URL that references the blob.</span></span> <span data-ttu-id="40336-165">この URL は、データベースに格納され、Fix It の web ページで、アップロードされたイメージを表示するのに使用できます。</span><span class="sxs-lookup"><span data-stu-id="40336-165">This URL will be stored in the database and can be used in Fix It web pages to display the uploaded image.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

<span data-ttu-id="40336-166">この URL は、FixItTask テーブルの列の 1 つとして、データベースに格納されます。</span><span class="sxs-lookup"><span data-stu-id="40336-166">This URL is stored in the database as one of the columns of the FixItTask table.</span></span>

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

<span data-ttu-id="40336-167">データベース内の URL のみと Blob storage 内のイメージでは、Fix It アプリは、データベース維持小規模でスケーラブル、かつ低コストでストレージは安価なとテラバイトやペタバイトを処理できる、イメージが格納されます。</span><span class="sxs-lookup"><span data-stu-id="40336-167">With only the URL in the database, and images in Blob storage, the Fix It app keeps the database small, scalable, and inexpensive, while the images are stored where storage is cheap and capable of handling terabytes or petabytes.</span></span> <span data-ttu-id="40336-168">数百テラバイトの修正、写真を格納できる 1 つのストレージ アカウントを使用したのみ支払います。</span><span class="sxs-lookup"><span data-stu-id="40336-168">One storage account can store hundreds of terabytes of Fix It photos, and you only pay for what you use.</span></span> <span data-ttu-id="40336-169">このため、最初のギガバイトの有料 9 セントを小規模から開始し、追加のギガバイトあたり少額の他のイメージを追加できます。</span><span class="sxs-lookup"><span data-stu-id="40336-169">So you can start off small paying 9 cents for the first gigabyte, and add more images for pennies per additional gigabyte.</span></span>

### <a name="display-the-uploaded-file"></a><span data-ttu-id="40336-170">アップロードされたファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="40336-170">Display the uploaded file</span></span>

<span data-ttu-id="40336-171">Fix It アプリケーションは、タスクの詳細を表示するときに、アップロードされたイメージ ファイルを表示します。</span><span class="sxs-lookup"><span data-stu-id="40336-171">The Fix It application displays the uploaded image file when it displays details for a task.</span></span>

![写真とタスクの詳細を修正します。](unstructured-blob-storage/_static/image3.png)

<span data-ttu-id="40336-173">イメージを表示するにが含まれて、MVC ビューを実行する必要がありますすべて、`PhotoUrl`ブラウザーに送信される HTML 内の値。</span><span class="sxs-lookup"><span data-stu-id="40336-173">To display the image, all the MVC view has to do is include the `PhotoUrl` value in the HTML sent to the browser.</span></span> <span data-ttu-id="40336-174">Web サーバーとデータベースは、使用していないサイクル イメージを表示するイメージの URL に数バイトの提供のみ。</span><span class="sxs-lookup"><span data-stu-id="40336-174">The web server and the database are not using cycles to display the image, they are only serving up a few bytes to the image URL.</span></span> <span data-ttu-id="40336-175">次の Razor コードで`Model`のインスタンスを指す、`FixItTask`エンティティ クラスです。</span><span class="sxs-lookup"><span data-stu-id="40336-175">In the following Razor code, `Model` refers to an instance of the `FixItTask` entity class.</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

<span data-ttu-id="40336-176">表示するページの HTML を見ると、次のように、blob ストレージにイメージを直接指し示す URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="40336-176">If you look at the HTML of the page that displays, you see the URL pointing directly to the image in blob storage, something like this:</span></span>

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a><span data-ttu-id="40336-177">まとめ</span><span class="sxs-lookup"><span data-stu-id="40336-177">Summary</span></span>

<span data-ttu-id="40336-178">Fix It アプリが、Blob service と SQL database での画像 Url のみでイメージを格納する方法を説明しました。</span><span class="sxs-lookup"><span data-stu-id="40336-178">You've seen how the Fix It app stores images in the Blob service and only image URLs in the SQL database.</span></span> <span data-ttu-id="40336-179">それ以外の場合は、ほぼ無制限の数のタスクの最大スケールが可能になり、多くのコードを記述することがなく実行できますよりもはるかに小さい SQL database を保持、Blob サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="40336-179">Using the Blob service keeps the SQL database much smaller than it otherwise would be, makes it possible to scale up to an almost unlimited number of tasks, and can be done without writing a lot of code.</span></span>

<span data-ttu-id="40336-180">ストレージ アカウントには数百のテラバイトとストレージ コストは、開始月 + 小さいトランザクション料金あたりギガバイトあたり約 3 セント位置として、SQL Database ストレージよりも安価です。</span><span class="sxs-lookup"><span data-stu-id="40336-180">You can have hundreds of terabytes in a storage account, and the storage cost is much less expensive than SQL Database storage, starting at about 3 cents per gigabyte per month plus a small transaction charge.</span></span> <span data-ttu-id="40336-181">最大容量ですが実際に格納する、アプリがスケーリングできる状態にすべてに余分な容量を払っているしないため、時間に対してのみを払っているできませんに注意してください。</span><span class="sxs-lookup"><span data-stu-id="40336-181">And keep in mind that you're not paying for the maximum capacity but only for the amount you actually store, so your app is ready to scale but you're not paying for all that extra capacity.</span></span>

<span data-ttu-id="40336-182">[[次へ] の章](design-to-survive-failures.md)クラウド アプリの障害を適切に処理できることの重要性について説明します。</span><span class="sxs-lookup"><span data-stu-id="40336-182">In the [next chapter](design-to-survive-failures.md) we'll talk about the importance of making a cloud app capable of gracefully handling failures.</span></span>

## <a name="resources"></a><span data-ttu-id="40336-183">リソース</span><span class="sxs-lookup"><span data-stu-id="40336-183">Resources</span></span>

<span data-ttu-id="40336-184">詳細については、次のリソースを参照してください。</span><span class="sxs-lookup"><span data-stu-id="40336-184">For more information see the following resources:</span></span>

- <span data-ttu-id="40336-185">[Azure BLOB Storage の概要、](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/)します。</span><span class="sxs-lookup"><span data-stu-id="40336-185">[An Introduction to Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/).</span></span> <span data-ttu-id="40336-186">Mike Wood によるブログ。</span><span class="sxs-lookup"><span data-stu-id="40336-186">Blog by Mike Wood.</span></span>
- <span data-ttu-id="40336-187">[.NET で Azure Blob ストレージ サービスを使用する方法](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs)します。</span><span class="sxs-lookup"><span data-stu-id="40336-187">[How to use the Azure Blob Storage Service in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="40336-188">MicrosoftAzure.com サイトの公式ドキュメント。</span><span class="sxs-lookup"><span data-stu-id="40336-188">Official documentation on the MicrosoftAzure.com site.</span></span> <span data-ttu-id="40336-189">Blob を blob storage に接続する方法を紹介するコード例の後にストレージを簡単に紹介のコンテナーを作成するには、アップロードおよびダウンロード blob など。</span><span class="sxs-lookup"><span data-stu-id="40336-189">A brief introduction to blob storage followed by code examples showing how to connect to blob storage, create containers, upload and download blobs, etc.</span></span>
- <span data-ttu-id="40336-190">[フェール セーフ: スケーラブルで回復力のあるクラウド サービスの構築](https://channel9.msdn.com/Series/FailSafe)します。</span><span class="sxs-lookup"><span data-stu-id="40336-190">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="40336-191">Ulrich Homann、Marc Mercuri、Mark Simms、ビデオ シリーズを 9 の部分で構成します。</span><span class="sxs-lookup"><span data-stu-id="40336-191">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="40336-192">Microsoft Customer ・ Advisory Team (CAT) の実際の顧客使用経験描画ストーリーと非常にアクセス可能な興味深い方法で高度な概念とアーキテクチャの原則を説明します。</span><span class="sxs-lookup"><span data-stu-id="40336-192">Presents high-level concepts and architectural principles in a very accessible and interesting way, with stories drawn from Microsoft Customer Advisory Team (CAT) experience with actual customers.</span></span> <span data-ttu-id="40336-193">Azure Storage サービスと blob の詳細については、35:13 始まるエピソード 5 を参照してください。</span><span class="sxs-lookup"><span data-stu-id="40336-193">For a discussion of Azure Storage service and blobs, see episode 5 starting at 35:13.</span></span>
- <span data-ttu-id="40336-194">[Microsoft Patterns and Practices - Azure ガイダンス](https://msdn.microsoft.com/library/dn568099.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="40336-194">[Microsoft Patterns and Practices - Azure Guidance](https://msdn.microsoft.com/library/dn568099.aspx).</span></span> <span data-ttu-id="40336-195">バレット キー パターンを参照してください。</span><span class="sxs-lookup"><span data-stu-id="40336-195">See Valet Key pattern.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40336-196">[前へ](data-partitioning-strategies.md)
> [次へ](design-to-survive-failures.md)</span><span class="sxs-lookup"><span data-stu-id="40336-196">[Previous](data-partitioning-strategies.md)
[Next](design-to-survive-failures.md)</span></span>
