---
title: ASP.NET Core 構成を移行します。
author: ardalis
description: ASP.NET MVC プロジェクトから ASP.NET Core MVC プロジェクトに構成を移行する方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: 5a1c4d0cbbdf74a00073c654e78a05f44948caae
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205913"
---
# <a name="migrate-configuration-to-aspnet-core"></a><span data-ttu-id="b515a-103">ASP.NET Core 構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="b515a-103">Migrate configuration to ASP.NET Core</span></span>

<span data-ttu-id="b515a-104">作成者: [Steve Smith](https://ardalis.com/)、[Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="b515a-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="b515a-105">前の記事では[ASP.NET MVCプロジェクトからASP.NET Core MVCへの移行](xref:migration/mvc)を開始しました。</span><span class="sxs-lookup"><span data-stu-id="b515a-105">In the previous article, we began [migrate an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/mvc).</span></span> <span data-ttu-id="b515a-106">この記事では、構成を移行します。</span><span class="sxs-lookup"><span data-stu-id="b515a-106">In this article, we migrate configuration.</span></span>

<span data-ttu-id="b515a-107">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="b515a-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="setup-configuration"></a><span data-ttu-id="b515a-108">セットアップの構成</span><span class="sxs-lookup"><span data-stu-id="b515a-108">Setup configuration</span></span>

<span data-ttu-id="b515a-109">ASP.NET Core を使用しなく、 *Global.asax*と*web.config*ファイルを以前のバージョンの ASP.NET を使用します。</span><span class="sxs-lookup"><span data-stu-id="b515a-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="b515a-110">以前のバージョンの ASP.NET では、アプリケーションの起動ロジックが格納された、`Application_StartUp`メソッド内で*Global.asax*します。</span><span class="sxs-lookup"><span data-stu-id="b515a-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="b515a-111">ASP.NET MVC では、以降、 *Startup.cs* ; プロジェクトのルートにファイルが含まれているし、アプリケーションの起動時に呼び出されました。</span><span class="sxs-lookup"><span data-stu-id="b515a-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="b515a-112">ASP.NET Core はすべてのスタートアップ ロジックを配置することでこの方法を完全に採用が、 *Startup.cs*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b515a-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="b515a-113">*Web.config*ファイルは ASP.NET Core で置き換えられてもします。</span><span class="sxs-lookup"><span data-stu-id="b515a-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="b515a-114">説明されているアプリケーションのスタートアップ プロシージャの一部として、構成自体を構成ようになりましたことができます*Startup.cs*します。</span><span class="sxs-lookup"><span data-stu-id="b515a-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="b515a-115">構成は、XML ファイルにも引き続き使用できますが、通常 ASP.NET Core プロジェクトが配置の構成値で JSON 形式のファイルなど*appsettings.json*します。</span><span class="sxs-lookup"><span data-stu-id="b515a-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="b515a-116">ASP.NET Core の構成システムは、環境変数は、指定できますも簡単にアクセスできます、[よりセキュリティで保護された信頼性の高い場所](xref:security/app-secrets)環境固有の値。</span><span class="sxs-lookup"><span data-stu-id="b515a-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a [more secure and robust location](xref:security/app-secrets) for environment-specific values.</span></span> <span data-ttu-id="b515a-117">これは、接続文字列とソース管理にチェックインしないでください API キーのような機密情報に特に当てはまります。</span><span class="sxs-lookup"><span data-stu-id="b515a-117">This is especially true for secrets like connection strings and API keys that shouldn't be checked into source control.</span></span> <span data-ttu-id="b515a-118">参照してください[構成](xref:fundamentals/configuration/index)を ASP.NET Core での構成の詳細を参照してください。</span><span class="sxs-lookup"><span data-stu-id="b515a-118">See [Configuration](xref:fundamentals/configuration/index) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="b515a-119">この記事では、部分的に移行した ASP.NET Core プロジェクトで開始します[前の記事](xref:migration/mvc)します。</span><span class="sxs-lookup"><span data-stu-id="b515a-119">For this article, we are starting with the partially migrated ASP.NET Core project from [the previous article](xref:migration/mvc).</span></span> <span data-ttu-id="b515a-120">構成のセットアップ、次のコンス トラクターとプロパティを追加する、 *Startup.cs*プロジェクトのルートにあるファイル。</span><span class="sxs-lookup"><span data-stu-id="b515a-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

<span data-ttu-id="b515a-121">現時点ことに注意してください、 *Startup.cs*ように、次を追加する必要があります、ファイルはコンパイルされません`using`ステートメント。</span><span class="sxs-lookup"><span data-stu-id="b515a-121">Note that at this point, the *Startup.cs* file won't compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="b515a-122">追加、 *appsettings.json*適切な項目テンプレートを使用して、プロジェクトのルートにファイル。</span><span class="sxs-lookup"><span data-stu-id="b515a-122">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![AppSettings の JSON を追加します。](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="b515a-124">Web.config ファイルから構成設定を移行します。</span><span class="sxs-lookup"><span data-stu-id="b515a-124">Migrate configuration settings from web.config</span></span>

<span data-ttu-id="b515a-125">ASP.NET MVC プロジェクトに必要なデータベース接続文字列が含まれている*web.config*の`<connectionStrings>`要素。</span><span class="sxs-lookup"><span data-stu-id="b515a-125">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="b515a-126">この ASP.NET Core プロジェクトでこの情報を格納するつもりが、 *appsettings.json*ファイル。</span><span class="sxs-lookup"><span data-stu-id="b515a-126">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="b515a-127">開いている*appsettings.json*、および次既に含まれていることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="b515a-127">Open *appsettings.json*, and note that it already includes the following:</span></span>

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

<span data-ttu-id="b515a-128">強調表示された行を上に示したでデータベースの名前を変更 **_CHANGE_ME**データベースの名前にします。</span><span class="sxs-lookup"><span data-stu-id="b515a-128">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="b515a-129">まとめ</span><span class="sxs-lookup"><span data-stu-id="b515a-129">Summary</span></span>

<span data-ttu-id="b515a-130">ASP.NET Core では、1 つのファイル、これで、必要なサービスと依存関係を定義および構成できるアプリケーションのすべてのスタートアップ ロジックを配置します。</span><span class="sxs-lookup"><span data-stu-id="b515a-130">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="b515a-131">置き換えられます、 *web.config*ファイルのさまざまな環境変数と同様に、JSON などのファイル形式を利用できる柔軟な構成機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="b515a-131">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>
