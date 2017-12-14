---
title: "ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。"
author: pkellner
description: "キャッシュ タグ ヘルパーを使用する方法を示しています。"
keywords: "ASP.NET Core,タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a012
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 1710a5781fb69aaa6101270d6b4fd44f92c7f06c
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="006fc-104">ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="006fc-104">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="006fc-105">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="006fc-105">By [Peter Kellner](http://peterkellner.net)</span></span> 


<span data-ttu-id="006fc-106">キャッシュ タグ ヘルパーでは、内部の ASP.NET Core キャッシュ プロバイダーには、そのコンテンツをキャッシュすることによって、ASP.NET Core アプリケーションのパフォーマンスを大幅に改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="006fc-106">The  Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="006fc-107">Razor ビュー エンジンが既定値を設定`expires-after`20 分にします。</span><span class="sxs-lookup"><span data-stu-id="006fc-107">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="006fc-108">次の Razor マークアップでは、日付/時刻によってキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="006fc-108">The following Razor markup caches the date/time:</span></span>

```cshtml
<Cache>@DateTime.Now<Cache>
```

<span data-ttu-id="006fc-109">最初の要求を含むページ`CacheTagHelper`現在の日付/時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="006fc-109">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="006fc-110">キャッシュ (既定値は 20 分間) 有効期限が切れるか、メモリ不足によって削除されるまで、追加の要求は、キャッシュされた値を表示します。</span><span class="sxs-lookup"><span data-stu-id="006fc-110">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="006fc-111">次の属性を持つキャッシュの存続期間を設定できます。</span><span class="sxs-lookup"><span data-stu-id="006fc-111">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="006fc-112">キャッシュ タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="006fc-112">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="006fc-113">enabled</span><span class="sxs-lookup"><span data-stu-id="006fc-113">enabled</span></span>    


| <span data-ttu-id="006fc-114">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-114">Attribute Type</span></span>    | <span data-ttu-id="006fc-115">有効な値</span><span class="sxs-lookup"><span data-stu-id="006fc-115">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="006fc-116">boolean</span><span class="sxs-lookup"><span data-stu-id="006fc-116">boolean</span></span>           | <span data-ttu-id="006fc-117">"true"(既定値)</span><span class="sxs-lookup"><span data-stu-id="006fc-117">"true" (default)</span></span>  |
|                   | <span data-ttu-id="006fc-118">"false"</span><span class="sxs-lookup"><span data-stu-id="006fc-118">"false"</span></span>   |


<span data-ttu-id="006fc-119">キャッシュ タグ ヘルパーで囲まれたコンテンツをキャッシュするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="006fc-119">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="006fc-120">既定値は、`true` です。</span><span class="sxs-lookup"><span data-stu-id="006fc-120">The default is `true`.</span></span>  <span data-ttu-id="006fc-121">場合設定`false`このキャッシュ タグ ヘルパーには、表示される出力のキャッシュの効果はありません。</span><span class="sxs-lookup"><span data-stu-id="006fc-121">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="006fc-122">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-122">Example:</span></span>

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="006fc-123">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="006fc-123">expires-on</span></span> 

| <span data-ttu-id="006fc-124">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-124">Attribute Type</span></span>    | <span data-ttu-id="006fc-125">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-125">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="006fc-126">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="006fc-126">DateTimeOffset</span></span>    | <span data-ttu-id="006fc-127">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="006fc-127">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="006fc-128">絶対有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="006fc-128">Sets an absolute expiration date.</span></span> <span data-ttu-id="006fc-129">次の例は、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29日の 5時 02分 PM までキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="006fc-129">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="006fc-130">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-130">Example:</span></span>

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="006fc-131">有効期限が切れた後</span><span class="sxs-lookup"><span data-stu-id="006fc-131">expires-after</span></span>

| <span data-ttu-id="006fc-132">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-132">Attribute Type</span></span>    | <span data-ttu-id="006fc-133">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-133">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="006fc-134">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="006fc-134">TimeSpan</span></span>    | <span data-ttu-id="006fc-135">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="006fc-135">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="006fc-136">内容をキャッシュする最初の要求時から時間の長さを設定します。</span><span class="sxs-lookup"><span data-stu-id="006fc-136">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="006fc-137">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-137">Example:</span></span>

```cshtml
<Cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="006fc-138">スライド式有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="006fc-138">expires-sliding</span></span>

| <span data-ttu-id="006fc-139">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-139">Attribute Type</span></span>    | <span data-ttu-id="006fc-140">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-140">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="006fc-141">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="006fc-141">TimeSpan</span></span>    | <span data-ttu-id="006fc-142">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="006fc-142">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="006fc-143">アクセスされていない場合、キャッシュ エントリを削除する必要がありますを時間を設定します。</span><span class="sxs-lookup"><span data-stu-id="006fc-143">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="006fc-144">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-144">Example:</span></span>

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="006fc-145">ヘッダーによって異なります</span><span class="sxs-lookup"><span data-stu-id="006fc-145">vary-by-header</span></span>

| <span data-ttu-id="006fc-146">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-146">Attribute Type</span></span>    | <span data-ttu-id="006fc-147">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-147">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-148">文字列型</span><span class="sxs-lookup"><span data-stu-id="006fc-148">String</span></span>            | <span data-ttu-id="006fc-149">「ユーザー エージェント」</span><span class="sxs-lookup"><span data-stu-id="006fc-149">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="006fc-150">「ユーザー エージェント、コンテンツ エンコード」</span><span class="sxs-lookup"><span data-stu-id="006fc-150">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="006fc-151">1 つのヘッダー値または変更されるときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="006fc-151">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="006fc-152">次の例は、ヘッダーの値を監視`User-Agent`です。</span><span class="sxs-lookup"><span data-stu-id="006fc-152">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="006fc-153">コンテンツをキャッシュする例では、すべて異なる`User-Agent`web サーバーに提示します。</span><span class="sxs-lookup"><span data-stu-id="006fc-153">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="006fc-154">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-154">Example:</span></span>

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="006fc-155">クエリの変更</span><span class="sxs-lookup"><span data-stu-id="006fc-155">vary-by-query</span></span>

| <span data-ttu-id="006fc-156">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-156">Attribute Type</span></span>    | <span data-ttu-id="006fc-157">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-157">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-158">文字列型</span><span class="sxs-lookup"><span data-stu-id="006fc-158">String</span></span>            | <span data-ttu-id="006fc-159">「処理を行う」</span><span class="sxs-lookup"><span data-stu-id="006fc-159">"Make"</span></span>                |
|                   | <span data-ttu-id="006fc-160">「製造元、モデル」</span><span class="sxs-lookup"><span data-stu-id="006fc-160">"Make,Model"</span></span> |

<span data-ttu-id="006fc-161">1 つのヘッダー値またはヘッダーの値が変化したときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="006fc-161">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="006fc-162">次の例の値を調べ`Make`と`Model`です。</span><span class="sxs-lookup"><span data-stu-id="006fc-162">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="006fc-163">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-163">Example:</span></span>

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="006fc-164">ルートの変化</span><span class="sxs-lookup"><span data-stu-id="006fc-164">vary-by-route</span></span>

| <span data-ttu-id="006fc-165">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-165">Attribute Type</span></span>    | <span data-ttu-id="006fc-166">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-166">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-167">文字列型</span><span class="sxs-lookup"><span data-stu-id="006fc-167">String</span></span>            | <span data-ttu-id="006fc-168">「処理を行う」</span><span class="sxs-lookup"><span data-stu-id="006fc-168">"Make"</span></span>                |
|                   | <span data-ttu-id="006fc-169">「製造元、モデル」</span><span class="sxs-lookup"><span data-stu-id="006fc-169">"Make,Model"</span></span> |

<span data-ttu-id="006fc-170">1 つのヘッダー値またはルート データ パラメーターの値変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="006fc-170">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="006fc-171">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-171">Example:</span></span>

<span data-ttu-id="006fc-172">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="006fc-172">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="006fc-173">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="006fc-173">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="006fc-174">cookie の変化</span><span class="sxs-lookup"><span data-stu-id="006fc-174">vary-by-cookie</span></span>

| <span data-ttu-id="006fc-175">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-175">Attribute Type</span></span>    | <span data-ttu-id="006fc-176">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-176">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-177">文字列型</span><span class="sxs-lookup"><span data-stu-id="006fc-177">String</span></span>            | <span data-ttu-id="006fc-178">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="006fc-178">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="006fc-179">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="006fc-179">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="006fc-180">1 つのヘッダー値またはヘッダーの値 (秒) の変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="006fc-180">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="006fc-181">次の例は、ASP.NET の Id に関連付けられているクッキーを検索します。</span><span class="sxs-lookup"><span data-stu-id="006fc-181">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="006fc-182">ユーザーが認証されるときに設定する要求 cookie キャッシュの更新をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="006fc-182">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="006fc-183">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-183">Example:</span></span>

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="006fc-184">ユーザーによって異なる</span><span class="sxs-lookup"><span data-stu-id="006fc-184">vary-by-user</span></span>

| <span data-ttu-id="006fc-185">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-185">Attribute Type</span></span>    | <span data-ttu-id="006fc-186">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-186">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-187">ブール型</span><span class="sxs-lookup"><span data-stu-id="006fc-187">Boolean</span></span>             | <span data-ttu-id="006fc-188">"true"</span><span class="sxs-lookup"><span data-stu-id="006fc-188">"true"</span></span>                  |
|                     | <span data-ttu-id="006fc-189">"false"(既定値)</span><span class="sxs-lookup"><span data-stu-id="006fc-189">"false" (default)</span></span> |

<span data-ttu-id="006fc-190">ログインしたユーザー (またはコンテキストのプリンシパル) が変更されたときにキャッシュをリセットするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="006fc-190">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="006fc-191">現在のユーザー コンテキストの要求プリンシパルとも呼ばれます、Razor ビューで参照することで表示できます`@User.Identity.Name`です。</span><span class="sxs-lookup"><span data-stu-id="006fc-191">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="006fc-192">次の例は、現在ログインしているユーザーで検索します。</span><span class="sxs-lookup"><span data-stu-id="006fc-192">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="006fc-193">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-193">Example:</span></span>

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="006fc-194">この属性を使用してログインし、ログ アウト サイクルを使用してキャッシュの内容を保持します。</span><span class="sxs-lookup"><span data-stu-id="006fc-194">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="006fc-195">使用する場合`vary-by-user="true"`ログで、ログ アウト アクションが、認証されたユーザーのキャッシュを無効になります。</span><span class="sxs-lookup"><span data-stu-id="006fc-195">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="006fc-196">ログインの新しい一意の cookie の値が生成されるため、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="006fc-196">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="006fc-197">Cookie が存在するか有効期限が切れたときに、匿名の状態のキャッシュが保持されません。</span><span class="sxs-lookup"><span data-stu-id="006fc-197">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="006fc-198">これユーザーがログインしていない場合、キャッシュを維持することを意味します。</span><span class="sxs-lookup"><span data-stu-id="006fc-198">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="006fc-199">によって異なる</span><span class="sxs-lookup"><span data-stu-id="006fc-199">vary-by</span></span>

| <span data-ttu-id="006fc-200">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-200">Attribute Type</span></span>    | <span data-ttu-id="006fc-201">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-201">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-202">文字列型</span><span class="sxs-lookup"><span data-stu-id="006fc-202">String</span></span>             | <span data-ttu-id="006fc-203">"@Model"</span><span class="sxs-lookup"><span data-stu-id="006fc-203">"@Model"</span></span>                 |


<span data-ttu-id="006fc-204">どのようなデータを取得キャッシュのカスタマイズをできます。</span><span class="sxs-lookup"><span data-stu-id="006fc-204">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="006fc-205">属性の文字列値が変更された、キャッシュ タグ ヘルパーの内容によって参照されるオブジェクトが更新されたとき。</span><span class="sxs-lookup"><span data-stu-id="006fc-205">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="006fc-206">多くの場合、モデルの値の文字列の連結は、この属性に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="006fc-206">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="006fc-207">実際には、連結された値のいずれかの更新には、キャッシュが無効になります。 このためです。</span><span class="sxs-lookup"><span data-stu-id="006fc-207">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="006fc-208">次の例では、ビューの合計値に 2 つのルート パラメーターの整数値を表示するコント ローラー メソッド`myParam1`と`myParam2`、1 つのモデルのプロパティとしてを返します。</span><span class="sxs-lookup"><span data-stu-id="006fc-208">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="006fc-209">この合計が変更されたときに、キャッシュ タグ ヘルパーのコンテンツのレンダリングし、キャッシュがもう一度です。</span><span class="sxs-lookup"><span data-stu-id="006fc-209">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="006fc-210">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-210">Example:</span></span>

<span data-ttu-id="006fc-211">アクション:</span><span class="sxs-lookup"><span data-stu-id="006fc-211">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="006fc-212">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="006fc-212">*Index.cshtml*</span></span>

```cshtml
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="006fc-213">priority</span><span class="sxs-lookup"><span data-stu-id="006fc-213">priority</span></span>

| <span data-ttu-id="006fc-214">属性の型</span><span class="sxs-lookup"><span data-stu-id="006fc-214">Attribute Type</span></span>    | <span data-ttu-id="006fc-215">値の例</span><span class="sxs-lookup"><span data-stu-id="006fc-215">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="006fc-216">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="006fc-216">CacheItemPriority</span></span>  | <span data-ttu-id="006fc-217">「高」</span><span class="sxs-lookup"><span data-stu-id="006fc-217">"High"</span></span>                   |
|                    | <span data-ttu-id="006fc-218">「低」</span><span class="sxs-lookup"><span data-stu-id="006fc-218">"Low"</span></span> |
|                    | <span data-ttu-id="006fc-219">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="006fc-219">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="006fc-220">"Normal"</span><span class="sxs-lookup"><span data-stu-id="006fc-220">"Normal"</span></span> |

<span data-ttu-id="006fc-221">組み込みのキャッシュ プロバイダーをキャッシュ立ち退きのガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="006fc-221">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="006fc-222">Web サーバーを削除し`Low`メモリ不足である場合に、最初のエントリをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="006fc-222">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="006fc-223">例:</span><span class="sxs-lookup"><span data-stu-id="006fc-223">Example:</span></span>

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

<span data-ttu-id="006fc-224">`priority`属性はキャッシュの保有期間の特定のレベルを保証されません。</span><span class="sxs-lookup"><span data-stu-id="006fc-224">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="006fc-225">`CacheItemPriority`提案のみです。</span><span class="sxs-lookup"><span data-stu-id="006fc-225">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="006fc-226">この属性を設定する`NeverRemove`キャッシュ常に保持することは保証されません。</span><span class="sxs-lookup"><span data-stu-id="006fc-226">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="006fc-227">参照してください[その他のリソース](#additional-resources)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="006fc-227">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="006fc-228">キャッシュ タグ ヘルパーが依存、[メモリ キャッシュ サービス](xref:performance/caching/memory)です。</span><span class="sxs-lookup"><span data-stu-id="006fc-228">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="006fc-229">キャッシュ タグ ヘルパーは、追加されていない場合に、サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="006fc-229">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="006fc-230">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="006fc-230">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
