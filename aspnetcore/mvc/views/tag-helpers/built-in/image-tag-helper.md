---
title: "画像のタグ ヘルパー |Microsoft ドキュメント"
author: pkellner
description: "イメージ タグ ヘルパーを使用する方法を示しています。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: d0857e1926c341b2357bc824fa379c4fc30affbc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="imagetaghelper"></a><span data-ttu-id="b404b-103">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="b404b-103">ImageTagHelper</span></span>

<span data-ttu-id="b404b-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="b404b-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="b404b-105">イメージ タグ ヘルパーの強化、 `img` (`<img>`) タグ。</span><span class="sxs-lookup"><span data-stu-id="b404b-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="b404b-106">必要な`src`タグだけでなく`boolean`属性`asp-append-version`です。</span><span class="sxs-lookup"><span data-stu-id="b404b-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="b404b-107">場合、イメージ ソース (`src`) 静的なファイルでは、ホスト web サーバーでは、文字列を破壊一意のキャッシュ、画像のソースにクエリ パラメーターとして付加されます。</span><span class="sxs-lookup"><span data-stu-id="b404b-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="b404b-108">これにより、ホストの web サーバー上のファイルが変更された場合、一意の要求 URL が生成されることを更新された要求パラメーターを含みます。</span><span class="sxs-lookup"><span data-stu-id="b404b-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="b404b-109">文字列を破壊キャッシュは、静的なイメージ ファイルのハッシュを表す一意の値です。</span><span class="sxs-lookup"><span data-stu-id="b404b-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="b404b-110">場合、イメージ ソース (`src`) は (たとえばリモート URL またはファイル サーバーに存在しません、) 静的ファイルではありません、`<img>`タグの`src`クエリ文字列パラメーターを破壊キャッシュなしの属性を生成します。</span><span class="sxs-lookup"><span data-stu-id="b404b-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="b404b-111">イメージ タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="b404b-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="b404b-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="b404b-112">asp-append-version</span></span>

<span data-ttu-id="b404b-113">と共に指定されている場合、`src`イメージ タグ ヘルパーの属性が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="b404b-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="b404b-114">有効な例`img`タグ ヘルパーは。</span><span class="sxs-lookup"><span data-stu-id="b404b-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="b404b-115">静的ファイルがディレクトリに存在する場合は*.wwwroot/images/asplogo.png*生成された html が (ハッシュは異なるされます)、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="b404b-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="b404b-116">パラメーターに割り当てられた値`v`ディスク上のファイルのハッシュ値です。</span><span class="sxs-lookup"><span data-stu-id="b404b-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="b404b-117">Web サーバーがへの読み取りアクセスを取得できない場合、静的なファイル参照、いいえ`v`にパラメーターを追加、`src`属性。</span><span class="sxs-lookup"><span data-stu-id="b404b-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="b404b-118">src</span><span class="sxs-lookup"><span data-stu-id="b404b-118">src</span></span>

<span data-ttu-id="b404b-119">イメージ タグ ヘルパーをアクティブ化する src 属性が必要な`<img>`要素。</span><span class="sxs-lookup"><span data-stu-id="b404b-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="b404b-120">イメージ タグ ヘルパーを使用して、 `Cache` 、集計を格納するローカル web サーバー上のプロバイダー`Sha512`の指定されたファイル。</span><span class="sxs-lookup"><span data-stu-id="b404b-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="b404b-121">ファイルが再度要求される場合、`Sha512`再計算する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="b404b-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="b404b-122">ファイルに関連付けられているファイルの監視でキャッシュが無効になりませんときに、ファイルの`Sha512`計算されます。</span><span class="sxs-lookup"><span data-stu-id="b404b-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b404b-123">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="b404b-123">Additional resources</span></span>

* <xref:performance/caching/memory>
