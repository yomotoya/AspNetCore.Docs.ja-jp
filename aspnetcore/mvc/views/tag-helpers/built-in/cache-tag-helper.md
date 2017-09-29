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
ms.openlocfilehash: da5b7b3bf1aa01ee22edf9bd003d8f79a00a5d0b
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a>ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。

著者: [Peter Kellner](http://peterkellner.net) 


キャッシュ タグ ヘルパーでは、内部の ASP.NET Core キャッシュ プロバイダーには、そのコンテンツをキャッシュすることによって、ASP.NET Core アプリケーションのパフォーマンスを大幅に改善する機能を提供します。

Razor ビュー エンジンが既定値を設定`expires-after`20 分にします。

次の Razor マークアップでは、日付/時刻によってキャッシュされます。

```cshtml
<Cache>@DateTime.Now<Cache>
```

最初の要求を含むページ`CacheTagHelper`現在の日付/時刻が表示されます。 キャッシュ (既定値は 20 分間) 有効期限が切れるか、メモリ不足によって削除されるまで、追加の要求は、キャッシュされた値を表示します。

次の属性を持つキャッシュの存続期間を設定できます。

## <a name="cache-tag-helper-attributes"></a>キャッシュ タグ ヘルパー属性

- - -

### <a name="enabled"></a>enabled    


| 属性の型    | 有効な値      |
|----------------   |----------------   |
| boolean           | "true"(既定値)  |
|                   | "false"   |


キャッシュ タグ ヘルパーで囲まれたコンテンツをキャッシュするかどうかを判断します。 既定値は、`true` です。  場合設定`false`このキャッシュ タグ ヘルパーには、表示される出力のキャッシュの効果はありません。

例:

```cshtml
<Cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-on"></a>有効期限が切れる 

| 属性の型    | 値の例     |
|----------------   |----------------   |
| DateTimeOffset    | "@new DateTime(2025,1,29,17,02,0)"    |


絶対有効期限を設定します。 次の例は、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29日の 5時 02分 PM までキャッシュされます。

例:

```cshtml
<Cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-after"></a>有効期限が切れた後

| 属性の型    | 値の例     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(120)"    |


内容をキャッシュする最初の要求時から時間の長さを設定します。 

例:

```cshtml
<Cache expires-on="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="expires-sliding"></a>スライド式有効期限が切れる

| 属性の型    | 値の例     |
|----------------   |----------------   |
| TimeSpan    | "@TimeSpan.FromSeconds(60)"     |


アクセスされていない場合、キャッシュ エントリを削除する必要がありますを時間を設定します。

例:

```cshtml
<Cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-header"></a>ヘッダーによって異なります

| 属性の型    | 値の例                |
|----------------   |----------------               |
| 文字列型            | 「ユーザー エージェント」                  |
|                   | 「ユーザー エージェント、コンテンツ エンコード」 |

1 つのヘッダー値または変更されるときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。 次の例は、ヘッダーの値を監視`User-Agent`です。 コンテンツをキャッシュする例では、すべて異なる`User-Agent`web サーバーに提示します。

例:

```cshtml
<Cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-query"></a>クエリの変更

| 属性の型    | 値の例                |
|----------------   |----------------               |
| 文字列型            | 「処理を行う」                |
|                   | 「製造元、モデル」 |

1 つのヘッダー値またはヘッダーの値が変化したときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。 次の例の値を調べ`Make`と`Model`です。

例:

```cshtml
<Cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-route"></a>ルートの変化

| 属性の型    | 値の例                |
|----------------   |----------------               |
| 文字列型            | 「処理を行う」                |
|                   | 「製造元、モデル」 |

1 つのヘッダー値またはルート データ パラメーターの値変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。 例:

*Startup.cs* 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
*Index.cshtml*

```cshtml
<Cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-cookie"></a>cookie の変化

| 属性の型    | 値の例                |
|----------------   |----------------               |
| 文字列型            | ".AspNetCore.Identity.Application"                |
|                   | ".AspNetCore.Identity.Application,HairColor" |

1 つのヘッダー値またはヘッダーの値 (秒) の変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。 次の例は、ASP.NET の Id に関連付けられているクッキーを検索します。 ユーザーが認証されるときに設定する要求 cookie キャッシュの更新をトリガーします。

例:

```cshtml
<Cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="vary-by-user"></a>ユーザーによって異なる

| 属性の型    | 値の例                |
|----------------   |----------------               |
| ブール型             | "true"                  |
|                     | "false"(既定値) |

ログインしたユーザー (またはコンテキストのプリンシパル) が変更されたときにキャッシュをリセットするかどうかを指定します。 現在のユーザー コンテキストの要求プリンシパルとも呼ばれます、Razor ビューで参照することで表示できます`@User.Identity.Name`です。

次の例は、現在ログインしているユーザーで検索します。  

例:

```cshtml
<Cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

この属性を使用してログインし、ログ アウト サイクルを使用してキャッシュの内容を保持します。  使用する場合`vary-by-user="true"`ログで、ログ アウト アクションが、認証されたユーザーのキャッシュを無効になります。  ログインの新しい一意の cookie の値が生成されるため、キャッシュは無効になります。 Cookie が存在するか有効期限が切れたときに、匿名の状態のキャッシュが保持されません。 これユーザーがログインしていない場合、キャッシュを維持することを意味します。

- - -

### <a name="vary-by"></a>によって異なる

| 属性の型    | 値の例                |
|----------------   |----------------               |
| 文字列型             | "@Model"                 |


どのようなデータを取得キャッシュのカスタマイズをできます。 属性の文字列値が変更された、キャッシュ タグ ヘルパーの内容によって参照されるオブジェクトが更新されたとき。 多くの場合、モデルの値の文字列の連結は、この属性に割り当てられます。  実際には、連結された値のいずれかの更新には、キャッシュが無効になります。 このためです。

次の例では、ビューの合計値に 2 つのルート パラメーターの整数値を表示するコント ローラー メソッド`myParam1`と`myParam2`、1 つのモデルのプロパティとしてを返します。 この合計が変更されたときに、キャッシュ タグ ヘルパーのコンテンツのレンダリングし、キャッシュがもう一度です。  

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
<Cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

- - -

### <a name="priority"></a>priority

| 属性の型    | 値の例                |
|----------------   |----------------               |
| CacheItemPriority  | 「高」                   |
|                    | 「低」 |
|                    | "NeverRemove" |
|                    | "Normal" |

組み込みのキャッシュ プロバイダーをキャッシュ立ち退きのガイダンスを提供します。 Web サーバーを削除し`Low`メモリ不足である場合に、最初のエントリをキャッシュします。

例:

```cshtml
<Cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</Cache>
```

`priority`属性はキャッシュの保有期間の特定のレベルを保証されません。 `CacheItemPriority`提案のみです。 この属性を設定する`NeverRemove`キャッシュ常に保持することは保証されません。 参照してください[その他のリソース](#additional-resources)詳細についてはします。

キャッシュ タグ ヘルパーが依存、[メモリ キャッシュ サービス](xref:performance/caching/memory)です。 キャッシュ タグ ヘルパーは、追加されていない場合に、サービスを追加します。

## <a name="additional-resources"></a>その他の技術情報

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
