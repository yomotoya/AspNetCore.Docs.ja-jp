---
title: ASP.NET Core での一般的なデータ保護規制 (GDPR) のサポート
author: rick-anderson
description: Web アプリで ASP.NET Core GDPR 拡張ポイントにアクセスする方法を示します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688628"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="4be4a-103">ASP.NET Core でのヨーロッパ一般的なデータ保護規制 (GDPR) のサポート</span><span class="sxs-lookup"><span data-stu-id="4be4a-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="4be4a-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4be4a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4be4a-105">ASP.NET Core 提供の Api とテンプレートの一部を達成するために、[ヨーロッパ一般的なデータ保護規制 (GDPR)](https://www.eugdpr.org/)要件。</span><span class="sxs-lookup"><span data-stu-id="4be4a-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="4be4a-106">プロジェクト テンプレートには、拡張点およびプライバシーと cookie を使用してポリシーを置き換えることができますスタブのマークアップが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="4be4a-107">Cookie 同意機能は使用すると、同意を求める (および追跡) を個人情報を格納するため、ユーザーから使用できます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="4be4a-108">ユーザーがデータの収集に同意したいないと、アプリが設定されている場合[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded)に`true`不要な cookie はブラウザーに送信されません。</span><span class="sxs-lookup"><span data-stu-id="4be4a-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="4be4a-109">Cookie は、重要としてマークできます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="4be4a-110">ユーザーが同意していないと、追跡が無効になっている場合でも、重要な cookie がブラウザーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="4be4a-111">[TempData とセッションの cookie](#tempdata)追跡が無効にした場合は機能しません。</span><span class="sxs-lookup"><span data-stu-id="4be4a-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="4be4a-112">[Identity 管理](#pd)をダウンロードして、ユーザー データを削除するリンクを提供します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="4be4a-113">[サンプル アプリ](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)GDPR 拡張点および ASP.NET Core 2.1 テンプレートに追加された Api のほとんどをテストすることができます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="4be4a-114">参照してください、 [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)指示をテストするためのファイルです。</span><span class="sxs-lookup"><span data-stu-id="4be4a-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="4be4a-115">[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="4be4a-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="4be4a-116">テンプレートの ASP.NET Core GDPR サポートで生成されたコード</span><span class="sxs-lookup"><span data-stu-id="4be4a-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="4be4a-117">Razor ページと MVC プロジェクト テンプレートで作成したプロジェクトに次の GDPR サポートが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="4be4a-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0)と[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_)で設定されている`Startup`です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="4be4a-119">*_CookieConsentPartial.cshtml* [部分ビュー](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="4be4a-120">*Pages/Privacy.cshtml*または*Home/Privacy.cshtml*ビューには、サイトのプライバシー ポリシーの詳細のページが用意されています。</span><span class="sxs-lookup"><span data-stu-id="4be4a-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="4be4a-121">*_CookieConsentPartial.cshtml*ファイルには、プライバシーに関するページへのリンクが生成されます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="4be4a-122">アプリケーションの個々 のユーザー アカウントで作成された場合は、管理 ページをダウンロードして、削除するへのリンクを示します。[ユーザーの個人データ](#pd)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="4be4a-123">CookiePolicyOptions と UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="4be4a-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="4be4a-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0)で初期化される、`Startup`クラス`ConfigureServices`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4be4a-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="4be4a-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_)で呼び出される、`Startup`クラス`Configure`メソッド。</span><span class="sxs-lookup"><span data-stu-id="4be4a-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="4be4a-126">_CookieConsentPartial.cshtml 部分ビュー</span><span class="sxs-lookup"><span data-stu-id="4be4a-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="4be4a-127">*_CookieConsentPartial.cshtml*部分ビュー。</span><span class="sxs-lookup"><span data-stu-id="4be4a-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="4be4a-128">この一部:</span><span class="sxs-lookup"><span data-stu-id="4be4a-128">This partial:</span></span>

* <span data-ttu-id="4be4a-129">ユーザーの追跡の状態を取得します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="4be4a-130">同意を要求するように、アプリケーションが構成されている場合、cookie を追跡する前に、ユーザーが同意する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4be4a-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="4be4a-131">作成されたナビゲーション バーの上に cookie 同意 chrome は固定の同意が必要な場合、 *Pages/Shared/_Layout.cshtml*ファイル。</span><span class="sxs-lookup"><span data-stu-id="4be4a-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="4be4a-132">HTML を提供`<p>`プライバシーと cookie を要約する要素は、ポリシーを使用します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="4be4a-133">リンク*Pages/Privacy.cshtml*サイトのプライバシー ポリシーの詳細ことができます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="4be4a-134">重要なクッキー</span><span class="sxs-lookup"><span data-stu-id="4be4a-134">Essential cookies</span></span>

<span data-ttu-id="4be4a-135">同意が与えられていない場合は、不可欠なマークされている cookie だけがブラウザーに送信されます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="4be4a-136">次のコードでは、cookie を必須にします。</span><span class="sxs-lookup"><span data-stu-id="4be4a-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="4be4a-137">Tempdata のプロバイダーとセッションの状態 cookie は必須ではないです。</span><span class="sxs-lookup"><span data-stu-id="4be4a-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="4be4a-138">[Tempdata プロバイダー](xref:fundamentals/app-state#tempdata) cookie が必須ではありません。</span><span class="sxs-lookup"><span data-stu-id="4be4a-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="4be4a-139">追跡が無効になっている場合、Tempdata プロバイダーは機能しません。</span><span class="sxs-lookup"><span data-stu-id="4be4a-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="4be4a-140">追跡が無効になっている場合は、Tempdata プロバイダーを有効化、TempData としてマークに不可欠な`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="4be4a-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="4be4a-141">[セッション状態](xref:fundamentals/app-state)cookie が必須ではないです。</span><span class="sxs-lookup"><span data-stu-id="4be4a-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="4be4a-142">追跡が無効にすると、セッション状態は機能しません。</span><span class="sxs-lookup"><span data-stu-id="4be4a-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="4be4a-143">個人データ</span><span class="sxs-lookup"><span data-stu-id="4be4a-143">Personal data</span></span>

<span data-ttu-id="4be4a-144">個々 のユーザー アカウントで作成された ASP.NET Core アプリケーションをダウンロードして、個人データを削除するコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="4be4a-145">ユーザー名を選択し、**個人データ**:</span><span class="sxs-lookup"><span data-stu-id="4be4a-145">Select the user name and then select **Personal data**:</span></span>

![個人のデータ ページを管理します。](gdpr/_static/pd.png)

<span data-ttu-id="4be4a-147">メモ:</span><span class="sxs-lookup"><span data-stu-id="4be4a-147">Notes:</span></span>

* <span data-ttu-id="4be4a-148">生成する、`Account/Manage`コードは、「 [Scaffold Identity](xref:security/authentication/scaffold-identity)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="4be4a-149">削除し、影響の id の既定のデータのみをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="4be4a-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="4be4a-150">アプリを作成するカスタム ユーザー データを拡張して、カスタム ユーザー データの削除/ダウンロードする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4be4a-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="4be4a-151">GitHub 問題[Id にカスタム ユーザー データの追加/削除する方法](https://github.com/aspnet/Docs/issues/6226)カスタム/削除/ダウンロードのカスタム ユーザー データを作成する方法、提案されたアーティクルを追跡します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="4be4a-152">優先順位を付けるそのトピックを表示する場合は、問題に対する反応を親指のままにします。</span><span class="sxs-lookup"><span data-stu-id="4be4a-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="4be4a-153">Id のデータベース テーブルに格納されているユーザーのトークンを保存`AspNetUserTokens`理由のため、カスケード delete の動作を使用して、ユーザーが削除されるときに削除されます、[外部キー](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="4be4a-154">保存時の暗号化</span><span class="sxs-lookup"><span data-stu-id="4be4a-154">Encryption at rest</span></span>

<span data-ttu-id="4be4a-155">一部のデータベースとストレージのメカニズムは、保存時の暗号化できるようにします。</span><span class="sxs-lookup"><span data-stu-id="4be4a-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="4be4a-156">保存時の暗号化:</span><span class="sxs-lookup"><span data-stu-id="4be4a-156">Encryption at rest:</span></span>

* <span data-ttu-id="4be4a-157">自動的に格納されているデータを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="4be4a-158">構成、プログラミング、またはデータにアクセスする、ソフトウェアの他の作業なしで暗号化します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="4be4a-159">最も簡単で安全なオプションです。</span><span class="sxs-lookup"><span data-stu-id="4be4a-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="4be4a-160">データベースのキーと暗号化を管理することができます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="4be4a-161">例えば:</span><span class="sxs-lookup"><span data-stu-id="4be4a-161">For example:</span></span>

* <span data-ttu-id="4be4a-162">Microsoft SQL および Azure SQL 提供[Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE)。</span><span class="sxs-lookup"><span data-stu-id="4be4a-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="4be4a-163">SQL Azure は、既定では、データベースを暗号化します。</span><span class="sxs-lookup"><span data-stu-id="4be4a-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="4be4a-164">[Azure の Blob、ファイル、テーブル、およびキューの記憶域が既定で暗号化されて](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="4be4a-165">組み込みの保存時の暗号化を提供するデータベースには、保護が提供するディスクの暗号化を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="4be4a-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="4be4a-166">例えば:</span><span class="sxs-lookup"><span data-stu-id="4be4a-166">For example:</span></span>

* [<span data-ttu-id="4be4a-167">Windows server 用の Bitlocker</span><span class="sxs-lookup"><span data-stu-id="4be4a-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="4be4a-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="4be4a-168">Linux:</span></span>
  * [<span data-ttu-id="4be4a-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="4be4a-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="4be4a-170">[EncFS](https://github.com/vgough/encfs)です。</span><span class="sxs-lookup"><span data-stu-id="4be4a-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4be4a-171">その他のリソース</span><span class="sxs-lookup"><span data-stu-id="4be4a-171">Additional Resources</span></span>

* [<span data-ttu-id="4be4a-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="4be4a-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
