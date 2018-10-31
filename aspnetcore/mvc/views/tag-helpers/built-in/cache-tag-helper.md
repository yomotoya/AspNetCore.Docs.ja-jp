---
title: ASP.NET Core MVC のキャッシュ タグ ヘルパー
author: pkellner
description: キャッシュ タグ ヘルパーを使用する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 7d64c500168166b0a7a29d5b92473726d5a9f49a
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325342"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="c6e0c-103">ASP.NET Core MVC のキャッシュ タグ ヘルパー</span><span class="sxs-lookup"><span data-stu-id="c6e0c-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="c6e0c-104">作成者: [Peter Kellner](http://peterkellner.net)、[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c6e0c-104">By [Peter Kellner](http://peterkellner.net) and [Luke Latham](https://github.com/guardrex)</span></span> 

<span data-ttu-id="c6e0c-105">キャッシュ タグ ヘルパーは、ASP.NET Core アプリの内容を内部 ASP.NET Core キャッシュ プロバイダーにキャッシュすることによって、アプリのパフォーマンスを改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="c6e0c-106">タグ ヘルパーの概要については、「<xref:mvc/views/tag-helpers/intro>」をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="c6e0c-107">次の Razor マークアップは、現在の日付をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="c6e0c-108">タグ ヘルパーを含むページに対する最初の要求で、現在の日付が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="c6e0c-109">キャッシュの有効期限 (既定値は 20 分) が切れるか、キャッシュされた日付がキャッシュから削除されるまで、それ以降の要求ではキャッシュされた値が表示されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="c6e0c-110">キャッシュ タグ ヘルパーの属性</span><span class="sxs-lookup"><span data-stu-id="c6e0c-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="c6e0c-111">enabled</span><span class="sxs-lookup"><span data-stu-id="c6e0c-111">enabled</span></span>

| <span data-ttu-id="c6e0c-112">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-112">Attribute Type</span></span>  | <span data-ttu-id="c6e0c-113">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-113">Examples</span></span>        | <span data-ttu-id="c6e0c-114">既定値</span><span class="sxs-lookup"><span data-stu-id="c6e0c-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="c6e0c-115">ブール型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-115">Boolean</span></span>         | <span data-ttu-id="c6e0c-116">`true`、 `false`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="c6e0c-117">`enabled` によってキャッシュ タグ ヘルパーで囲まれた内容をキャッシュするかどうかが決定されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="c6e0c-118">既定値は、`true` です。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-118">The default is `true`.</span></span> <span data-ttu-id="c6e0c-119">`false` に設定すると、作成された出力はキャッシュ**されません**。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="c6e0c-120">例:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="c6e0c-121">expires-on</span><span class="sxs-lookup"><span data-stu-id="c6e0c-121">expires-on</span></span>

| <span data-ttu-id="c6e0c-122">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-122">Attribute Type</span></span>   | <span data-ttu-id="c6e0c-123">例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="c6e0c-124">`expires-on` によって、特定の日時でキャッシュされた項目の有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="c6e0c-125">次の例では、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29 日午後 5 時 2 分までキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="c6e0c-126">expires-after</span><span class="sxs-lookup"><span data-stu-id="c6e0c-126">expires-after</span></span>

| <span data-ttu-id="c6e0c-127">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-127">Attribute Type</span></span> | <span data-ttu-id="c6e0c-128">例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-128">Example</span></span>                      | <span data-ttu-id="c6e0c-129">既定値</span><span class="sxs-lookup"><span data-stu-id="c6e0c-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="c6e0c-130">20 分</span><span class="sxs-lookup"><span data-stu-id="c6e0c-130">20 minutes</span></span> |

<span data-ttu-id="c6e0c-131">`expires-after` では、最初の要求があってから内容をキャッシュしている時間の長さが設定されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="c6e0c-132">例:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="c6e0c-133">Razor ビュー エンジンでは、`expires-after` の規定値が 20 分に設定されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="c6e0c-134">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="c6e0c-134">expires-sliding</span></span>

| <span data-ttu-id="c6e0c-135">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-135">Attribute Type</span></span> | <span data-ttu-id="c6e0c-136">例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="c6e0c-137">ここで設定した時間だけキャッシュ エントリの値に対するアクセスがなかった場合、キャッシュ エントリを削除します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="c6e0c-138">例:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="c6e0c-139">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="c6e0c-139">vary-by-header</span></span>

| <span data-ttu-id="c6e0c-140">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-140">Attribute Type</span></span> | <span data-ttu-id="c6e0c-141">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="c6e0c-142">String</span><span class="sxs-lookup"><span data-stu-id="c6e0c-142">String</span></span>         | <span data-ttu-id="c6e0c-143">`User-Agent`、 `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="c6e0c-144">`vary-by-header` には、変化したときにキャッシュの更新をトリガーする、コンマで区切ったヘッダー値のリストを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="c6e0c-145">次の例では、ヘッダー値 `User-Agent` を監視します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="c6e0c-146">この例は、Web サーバーに提示されるすべての異なる `User-Agent` の内容をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="c6e0c-147">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="c6e0c-147">vary-by-query</span></span>

| <span data-ttu-id="c6e0c-148">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-148">Attribute Type</span></span> | <span data-ttu-id="c6e0c-149">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="c6e0c-150">String</span><span class="sxs-lookup"><span data-stu-id="c6e0c-150">String</span></span>         | <span data-ttu-id="c6e0c-151">`Make`、 `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="c6e0c-152">`vary-by-query` には、ヘッダー値が変化したときにキャッシュの更新をトリガーする、コンマで区切ったヘッダー値のリストを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-152">`vary-by-query` accepts a comma-delimited list of header values that trigger a cache refresh when the header value changes.</span></span>

<span data-ttu-id="c6e0c-153">次の例では、`Make` と `Model` の値を監視します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="c6e0c-154">この例は、Web サーバーに提示されるすべての異なる `Make` と `Model` の内容をキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="c6e0c-155">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="c6e0c-155">vary-by-route</span></span>

| <span data-ttu-id="c6e0c-156">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-156">Attribute Type</span></span> | <span data-ttu-id="c6e0c-157">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="c6e0c-158">String</span><span class="sxs-lookup"><span data-stu-id="c6e0c-158">String</span></span>         | <span data-ttu-id="c6e0c-159">`Make`、 `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="c6e0c-160">`vary-by-route` には、ルート データのパラメーター値が変化したときにキャッシュの更新をトリガーする、コンマで区切ったヘッダー値のリストを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-160">`vary-by-route` accepts a comma-delimited list of header values that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="c6e0c-161">例:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-161">Example:</span></span>

<span data-ttu-id="c6e0c-162">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="c6e0c-163">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-cookie"></a><span data-ttu-id="c6e0c-164">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="c6e0c-164">vary-by-cookie</span></span>

| <span data-ttu-id="c6e0c-165">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-165">Attribute Type</span></span> | <span data-ttu-id="c6e0c-166">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="c6e0c-167">String</span><span class="sxs-lookup"><span data-stu-id="c6e0c-167">String</span></span>         | <span data-ttu-id="c6e0c-168">`.AspNetCore.Identity.Application`、 `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="c6e0c-169">`vary-by-cookie` には、ヘッダー値が変化したときにキャッシュの更新をトリガーする、コンマで区切ったヘッダー値のリストを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-169">`vary-by-cookie` accepts a comma-delimited list of header values that trigger a cache refresh when the header values change.</span></span>

<span data-ttu-id="c6e0c-170">次の例では、ASP.NET Core ID に関連付けられている Cookie を監視します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="c6e0c-171">ユーザーが認証されると、Id Cookie の変更によってキャッシュの更新がトリガーされます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="c6e0c-172">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="c6e0c-172">vary-by-user</span></span>

| <span data-ttu-id="c6e0c-173">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-173">Attribute Type</span></span>  | <span data-ttu-id="c6e0c-174">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-174">Examples</span></span>        | <span data-ttu-id="c6e0c-175">既定値</span><span class="sxs-lookup"><span data-stu-id="c6e0c-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="c6e0c-176">ブール型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-176">Boolean</span></span>         | <span data-ttu-id="c6e0c-177">`true`、 `false`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="c6e0c-178">`vary-by-user` では、サインインしているユーザー (またはコンテキストのプリンシパル) が変化したときにキャッシュをリセットするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="c6e0c-179">現在のユーザーは要求コンテキストのプリンシパルとも呼ばれ、`@User.Identity.Name` を参照することにより Razor ビューで表示できます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="c6e0c-180">次の例では、現在ログインしているユーザーを監視して、キャッシュの更新をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="c6e0c-181">この属性を使って、ユーザーがサインインしてからサインアウトするまでキャッシュの内容を保持します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="c6e0c-182">値を `true` に設定すると、認証サイクルによって認証されたユーザーのキャッシュが無効にされます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="c6e0c-183">ユーザーが認証されると新しい一意の Cookie 値が生成されるため、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="c6e0c-184">Cookie が存在しない場合、または Cookie の有効期限が切れている場合は、キャッシュは匿名状態で保持されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="c6e0c-185">ユーザーが認証**されない**場合、キャッシュは保持されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="c6e0c-186">vary-by</span><span class="sxs-lookup"><span data-stu-id="c6e0c-186">vary-by</span></span>

| <span data-ttu-id="c6e0c-187">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-187">Attribute Type</span></span> | <span data-ttu-id="c6e0c-188">例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="c6e0c-189">String</span><span class="sxs-lookup"><span data-stu-id="c6e0c-189">String</span></span>         | `@Model` |

<span data-ttu-id="c6e0c-190">`vary-by` を使うと、キャッシュ対象のデータをカスタマイズできます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="c6e0c-191">属性の文字列値によって参照されているオブジェクトが変化すると、キャッシュ タグ ヘルパーの内容が更新されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="c6e0c-192">多くの場合、モデル値の文字列を連結したものがこの属性に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="c6e0c-193">このため、事実上連結されている値のいずれかが更新されると、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="c6e0c-194">次の例では、ビューをレンダリングするコントローラー メソッドは 2 つのルート パラメーター `myParam1` と `myParam2` の整数値を合計し、1 つのモデル プロパティとしてその合計値を返すものとします。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="c6e0c-195">この合計が変化すると、キャッシュ タグ ヘルパーの内容がレンダリングされて、再びキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="c6e0c-196">アクション:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="c6e0c-197">*Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="c6e0c-198">priority</span><span class="sxs-lookup"><span data-stu-id="c6e0c-198">priority</span></span>

| <span data-ttu-id="c6e0c-199">属性の型</span><span class="sxs-lookup"><span data-stu-id="c6e0c-199">Attribute Type</span></span>      | <span data-ttu-id="c6e0c-200">使用例</span><span class="sxs-lookup"><span data-stu-id="c6e0c-200">Examples</span></span>                               | <span data-ttu-id="c6e0c-201">既定値</span><span class="sxs-lookup"><span data-stu-id="c6e0c-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="c6e0c-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="c6e0c-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="c6e0c-203">`priority` により、組み込みのキャッシュ プロバイダーにキャッシュ削除ガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="c6e0c-204">Web サーバーは、メモリ不足になると、`Low` キャッシュ エントリを最初に削除します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="c6e0c-205">例:</span><span class="sxs-lookup"><span data-stu-id="c6e0c-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="c6e0c-206">`priority` 属性によって、特定レベルのキャッシュ リテンション期間が保証されることはありません。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="c6e0c-207">`CacheItemPriority` は提案するだけです。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="c6e0c-208">この属性を `NeverRemove` に設定しても、キャッシュされた項目が常に保持されるという保証はありません。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="c6e0c-209">詳細については、「[その他のリソース](#additional-resources)」セクションにあるトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="c6e0c-210">キャッシュ タグ ヘルパーは、[メモリ キャッシュ サービス](xref:performance/caching/memory)に依存します。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="c6e0c-211">サービスが追加されていない場合、キャッシュ タグ ヘルパーによってサービスが追加されます。</span><span class="sxs-lookup"><span data-stu-id="c6e0c-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c6e0c-212">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="c6e0c-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
