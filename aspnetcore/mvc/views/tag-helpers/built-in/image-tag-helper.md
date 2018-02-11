---
title: "ASP.NET Core のイメージ タグ ヘルパー"
author: pkellner
description: "イメージ タグ ヘルパーを使用する方法を示します"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="17767-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="17767-103">ImageTagHelper</span></span>

<span data-ttu-id="17767-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="17767-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="17767-105">イメージ タグ ヘルパーは `img` (`<img>`) タグを拡張します。</span><span class="sxs-lookup"><span data-stu-id="17767-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="17767-106">`src` タグと `boolean` 属性 `asp-append-version` が必要です。</span><span class="sxs-lookup"><span data-stu-id="17767-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="17767-107">イメージ ソース (`src`) がホスト Web サーバーの静的ファイルである場合、一意のキャッシュ バスティング文字列がイメージ ソースにクエリ パラメーターとし追加されます。</span><span class="sxs-lookup"><span data-stu-id="17767-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="17767-108">これにより、ホスト Web サーバーのファイルが変更された場合に、更新された要求パラメーターを含む一意の要求 URL が確実に生成されます。</span><span class="sxs-lookup"><span data-stu-id="17767-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="17767-109">キャッシュ バスティング文字列は、静的なイメージ ファイルのハッシュを表す一意の値です。</span><span class="sxs-lookup"><span data-stu-id="17767-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="17767-110">イメージ ソース (`src`) が静的ファイルでない場合 (たとえば、リモート URL またはファイルがサーバーに存在しない場合)、`<img>` タグの `src` 属性はキャッシュ バスティング クエリ文字列パラメーターなしで生成されます。</span><span class="sxs-lookup"><span data-stu-id="17767-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="17767-111">イメージ タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="17767-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="17767-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="17767-112">asp-append-version</span></span>

<span data-ttu-id="17767-113">`src` 属性と共に指定されている場合、イメージ タグ ヘルパーが呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="17767-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="17767-114">有効な `img` タグ ヘルパーの例を以下に示します。</span><span class="sxs-lookup"><span data-stu-id="17767-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="17767-115">静的ファイルがディレクトリ *..wwwroot/images/asplogo.png* に存在する場合、生成された html は次のようになります (ハッシュは異なります)。</span><span class="sxs-lookup"><span data-stu-id="17767-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="17767-116">パラメーター `v` に割り当てられた値は、ディスク上のファイルのハッシュ値です。</span><span class="sxs-lookup"><span data-stu-id="17767-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="17767-117">Web サーバーが参照される静的ファイルに対する読み取りアクセスを取得できない場合、`v` パラメーターは `src` 属性に追加されません。</span><span class="sxs-lookup"><span data-stu-id="17767-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="17767-118">src</span><span class="sxs-lookup"><span data-stu-id="17767-118">src</span></span>

<span data-ttu-id="17767-119">イメージ タグ ヘルパーをアクティブにするには、`<img>` 要素に src 属性が必要です。</span><span class="sxs-lookup"><span data-stu-id="17767-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="17767-120">イメージ タグ ヘルパーはローカル Web サーバーで `Cache` プロバイダーを使用して、特定のファイルの計算された `Sha512` を格納します。</span><span class="sxs-lookup"><span data-stu-id="17767-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="17767-121">ファイルが再度要求された場合、`Sha512` は再計算する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="17767-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="17767-122">キャッシュは、ファイルの `Sha512` が計算されたときにファイルにアタッチされるファイル ウォッチャーによって無効になります。</span><span class="sxs-lookup"><span data-stu-id="17767-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17767-123">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="17767-123">Additional resources</span></span>

* <xref:performance/caching/memory>
