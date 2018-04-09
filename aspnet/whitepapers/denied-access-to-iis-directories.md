---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET は IIS ディレクトリへのアクセスを拒否 |Microsoft ドキュメント
author: rick-anderson
description: このホワイト ペーパーでは、必要なことを ASP.NET アプリケーションに要求が、"ディレクトリ名のディレクトリを拒否する エラーを返す場合について説明します。 %S に失敗しました.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="fea11-104">ASP.NET は、IIS ディレクトリへのアクセスを拒否します。</span><span class="sxs-lookup"><span data-stu-id="fea11-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="fea11-105">このホワイト ペーパーでは、必要なことを ASP.NET アプリケーションに要求が、エラーを返した場合について説明します"へのアクセスを拒否*ディレクトリ名*ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="fea11-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="fea11-106">開始できませんでしたディレクトリの変更を監視します。"</span><span class="sxs-lookup"><span data-stu-id="fea11-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="fea11-107">ASP.NET 1.0 および 1.1 を ASP.NET に適用されます。</span><span class="sxs-lookup"><span data-stu-id="fea11-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="fea11-108">ASP.NET V1 RTM 小を使用してを実行して今すぐローカル コンピューター上に"ASPNET"アカウントとして登録されている windows アカウントの特権モードします。</span><span class="sxs-lookup"><span data-stu-id="fea11-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="fea11-109">一部のシステムがロックされて、このアカウント読み取ることが既定ではなくがセキュリティのアクセスを web サイトのコンテンツ ディレクトリ、アプリケーションのルート ディレクトリ、または web サイトのルート ディレクトリ。</span><span class="sxs-lookup"><span data-stu-id="fea11-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="fea11-110">ここで指定された web アプリケーションからページを要求するときに、次のエラーが表示されます。</span><span class="sxs-lookup"><span data-stu-id="fea11-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="fea11-111">これを解決するには、適切なディレクトリのセキュリティ アクセス許可を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="fea11-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="fea11-112">具体的には、ASP.NET には、読み取りが必要です、execute、および web サイトのルートの ASPNET アカウントのアクセスの一覧 (例: c:\inetpub\wwwroot または任意の代替サイト ディレクトリを IIS で構成した可能性があります)、コンテンツのディレクトリとアプリケーションのルート ディレクトリ構成ファイルの変更を監視するのにはします。</span><span class="sxs-lookup"><span data-stu-id="fea11-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="fea11-113">アプリケーションのルートは、IIS 管理ツール (inetmgr) でアプリケーションの仮想ディレクトリに関連付けられているフォルダーのパスに対応しています。</span><span class="sxs-lookup"><span data-stu-id="fea11-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="fea11-114">たとえば、wwwroot フォルダーの下にある次のアプリケーションの階層があるとします。</span><span class="sxs-lookup"><span data-stu-id="fea11-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="fea11-115">この例では、ASPNET アカウントには、上記で定義された、myapp と wwwroot ディレクトリの両方でコンテンツの読み取りアクセス許可が必要があります。</span><span class="sxs-lookup"><span data-stu-id="fea11-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="fea11-116">ルート フォルダーの 1 つの継承された ACL ことも必要に応じてできます両方のディレクトリに対して、入れ子になっている場合。</span><span class="sxs-lookup"><span data-stu-id="fea11-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="fea11-117">ディレクトリにアクセス許可を追加するには、次の手順を実行します。</span><span class="sxs-lookup"><span data-stu-id="fea11-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="fea11-118">Windows エクスプ ローラーを使用して、ディレクトリに移動します。</span><span class="sxs-lookup"><span data-stu-id="fea11-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="fea11-119">ディレクトリのフォルダーを右クリックし、[プロパティ] を選択</span><span class="sxs-lookup"><span data-stu-id="fea11-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="fea11-120">[プロパティ] ダイアログ ボックスの [セキュリティ] タブに移動します。</span><span class="sxs-lookup"><span data-stu-id="fea11-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="fea11-121">[追加] ボタンをクリックし、ASPNET アカウント名を続けてコンピューター名を入力します。</span><span class="sxs-lookup"><span data-stu-id="fea11-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="fea11-122">たとえば、"webdev"という名前のマシン上には、webdev\ASPNET を入力し、[OK] をクリックするとします。</span><span class="sxs-lookup"><span data-stu-id="fea11-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="fea11-123">ASPNET アカウントを持っていることを確認、"読み取り&amp;Execute"、「フォルダーの内容の一覧」および「読み取り」のチェック ボックスがオンになっています。</span><span class="sxs-lookup"><span data-stu-id="fea11-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="fea11-124">ダイアログ ボックスを閉じるし、変更を保存するには、[ok] をクリックします。</span><span class="sxs-lookup"><span data-stu-id="fea11-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="fea11-125">必要な場合、Windows にスクリプトまたは、付属する"cacls.exe"ツールを使用してこれらの変更を自動化できます。</span><span class="sxs-lookup"><span data-stu-id="fea11-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="fea11-126">詳細については、ASPNET アカウントを参照してください、 [FAQ ドキュメント](https://go.microsoft.com/fwlink/?LinkId=5828)です。</span><span class="sxs-lookup"><span data-stu-id="fea11-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="fea11-127">場合は、指定された web アプリケーションでは、書き込みに必要か、特定のフォルダーまたはファイルへのアクセス許可を変更、同じ手順に従って、「書き込み」または「変更」のチェック ボックスをチェックしてこれを許可できます。</span><span class="sxs-lookup"><span data-stu-id="fea11-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="fea11-128">すべてのユーザーまたはユーザー グループの (これは既定の構成)、これらのディレクトリへの読み取りアクセスを許可するコンピューターでは、問題が発生しないと、上記の手順は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="fea11-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
