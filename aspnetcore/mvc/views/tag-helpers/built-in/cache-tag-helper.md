---
title: ASP.NET Core MVC のキャッシュ タグ ヘルパー
author: pkellner
description: キャッシュ タグ ヘルパーを使用する方法を示します
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 969716e21211513053f52049368a0a7190ffba47
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276553"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC のキャッシュ タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net) 

キャッシュ タグ ヘルパーは、ASP.NET Core アプリの内容を内部 ASP.NET Core キャッシュ プロバイダーにキャッシュすることによって、アプリのパフォーマンスを大幅に改善する機能を提供します。

Razor ビュー エンジンは、既定の `expires-after` を 20 分に設定します。

次の Razor マークアップは、日付/時刻をキャッシュします。

```cshtml
<cache>@DateTime.Now</cache>
```

`CacheTagHelper` を含むページに対する最初の要求で、現在の日付/時刻が表示されます。 キャッシュの有効期限 (既定値は 20 分) が切れるか、メモリ不足により削除されるまで、それ以降の要求ではキャッシュされた値が表示されます。

キャッシュ期間は次の属性で設定できます。

## <a name="cache-tag-helper-attributes"></a>キャッシュ タグ ヘルパーの属性

- - -

### <a name="enabled"></a>enabled    


| 属性の型    | 有効な値      |
|----------------   |----------------   |
| boolean           | "true" (既定値)  |
|                   | "false"   |


キャッシュ タグ ヘルパーで囲まれた内容をキャッシュするかどうかを決定します。 既定値は、`true` です。  `false` に設定すると、そのキャッシュ タグ ヘルパーは表示される出力に対してキャッシュ効果を持ちません。

例:

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a>expires-on 

| 属性の型 |           値の例            |
|----------------|------------------------------------|
| DateTimeOffset | "@new DateTime(2025,1,29,17,02,0)" |

特定の日時で有効期限を設定します。 次の例では、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29 日午後 5 時 2 分までキャッシュします。

例:

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a>expires-after

| 属性の型 |        値の例         |
|----------------|------------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(120)" |

最初の要求があってから内容をキャッシュしている時間の長さを設定します。 

例:

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a>expires-sliding

| 属性の型 |        値の例        |
|----------------|-----------------------------|
|    TimeSpan    | "@TimeSpan.FromSeconds(60)" |

この属性で設定した時間だけアクセスがない場合、キャッシュ エントリを削除します。

例:

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a>vary-by-header

| 属性の型    | 値の例                |
|----------------   |----------------               |
| String            | "User-Agent"                  |
|                   | "User-Agent,content-encoding" |

変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。 次の例では、ヘッダー値 `User-Agent` を監視します。 この例は、Web サーバーに提示されるすべての異なる `User-Agent` の内容をキャッシュします。

例:

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a>vary-by-query

| 属性の型    | 値の例                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

ヘッダーの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。 次の例では、`Make` と `Model` の値を調べます。

例:

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a>vary-by-route

| 属性の型    | 値の例                |
|----------------   |----------------               |
| String            | "Make"                |
|                   | "Make,Model" |

ルート データ パラメーターの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。 例:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

*Index.cshtml*

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a>vary-by-cookie

| 属性の型    | 値の例                |
|----------------   |----------------               |
| String            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

ヘッダーの値が変化したときにキャッシュの更新をトリガーする 1 つのヘッダー値またはコンマで区切ったヘッダー値のリストを受け付けます。 次の例では、ASP.NET Identity に関連付けられている Cookie を調べます。 ユーザーが認証されると、キャッシュの更新をトリガーする要求 Cookie が設定されます。

例:

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a>vary-by-user

| 属性の型    | 値の例                |
|----------------   |----------------               |
| ブール型             | "true"                  |
|                     | "false" (既定値) |

ログインしているユーザー (またはコンテキストのプリンシパル) が変化したときにキャッシュをリセットするかどうかを指定します。 現在のユーザーは要求コンテキストのプリンシパルとも呼ばれ、`@User.Identity.Name` を参照することにより Razor ビューで表示できます。

次の例では、現在ログインしているユーザーを調べています。  

例:

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

この属性を使って、ユーザーがログインしてからログアウトするまでキャッシュの内容を保持します。  `vary-by-user="true"` を使うと、ログイン アクションとログアウト アクションで、認証されたユーザーのキャッシュが無効になります。  ログイン時に新しい一意の Cookie 値が生成されるため、キャッシュは無効になります。 Cookie が存在しない場合、または有効期限が切れた場合は、キャッシュは匿名状態で保持されます。 つまり、ユーザーがログインしていない場合でも、キャッシュは維持されます。

- - -

### <a name="vary-by"></a>vary-by

| 属性の型 | 値の例 |
|----------------|----------------|
|     String     |    "@Model"    |

キャッシュ対象のデータをカスタマイズできます。 属性の文字列値によって参照されているオブジェクトが変化すると、キャッシュ タグ ヘルパーの内容が更新されます。 多くの場合、モデル値の文字列を連結したものがこの属性に割り当てられます。  そのため、連結されている値のいずれかが更新されると、キャッシュは無効になります。

次の例では、ビューをレンダリングするコントローラー メソッドは 2 つのルート パラメーター `myParam1` と `myParam2` の整数値を合計値し、1 つのモデル プロパティとしてそれを返すものとします。 この合計が変化すると、キャッシュ タグ ヘルパーの内容がレンダリングされて、再びキャッシュされます。  

例:

アクション:

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

*Index.cshtml*

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a>priority

| 属性の型    | 値の例                |
|----------------   |----------------               |
| CacheItemPriority  | "High"                   |
|                    | "Low" |
|                    | "NeverRemove" |
|                    | "Normal" |

組み込みのキャッシュ プロバイダーにキャッシュ削除ガイダンスを提供します。 Web サーバーは、メモリ不足になると、`Low` キャッシュ エントリを最初に削除します。

例:

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

`priority` 属性によって、特定レベルのキャッシュ リテンション期間が保証されることはありません。 `CacheItemPriority` は提案するだけです。 この属性を `NeverRemove` に設定しても、キャッシュが常に保持されるという保証はありません。 詳しくは、「[その他の技術情報](#additional-resources)」をご覧ください。

キャッシュ タグ ヘルパーは、[メモリ キャッシュ サービス](xref:performance/caching/memory)に依存します。 サービスが追加されていない場合、キャッシュ タグ ヘルパーがサービスを追加します。

## <a name="additional-resources"></a>その他の技術情報

* [メモリ内キャッシュ](xref:performance/caching/memory)
* [Identity の概要](xref:security/authentication/identity)
