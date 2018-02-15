---
title: "ASP.NET Core の環境タグ ヘルパー"
author: pkellner
description: "すべてのプロパティを含む、定義済みの ASP.NET Core 環境タグ ヘルパー"
manager: wpickett
ms.author: riande
ms.date: 07/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 7a99ee0e59c7f49a3208d2c86c11cabce4294889
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET Core の環境タグ ヘルパー

著者: [Peter Kellner](http://peterkellner.net)、[Hisham Bin Ateya](https://twitter.com/hishambinateya)

環境タグ ヘルパーは、現在のホスティング環境に基づき、囲まれたコンテンツを条件付きでレンダリングします。 その単一の属性 `names` は環境名のコンマ区切りリストであり、現在の環境に一致した場合は囲まれたコンテンツのレンダリングがトリガーされます。

## <a name="environment-tag-helper-attributes"></a>環境タグ ヘルパーの属性

### <a name="names"></a>名前

囲まれたコンテンツのレンダリングをトリガーする単一のホスティング環境名またはホスティング環境名のコンマ区切りリストを受け入れます。

これらの値は、ASP.NET Core の静的プロパティ `HostingEnvironment.EnvironmentName` から返される現在の値と比較されます。  この値は、**Staging**、**Development** または **Production** のいずれかになります。 比較では大文字と小文字の区別は無視されます。

有効な `environment` タグ ヘルパーの例を以下に示します。

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>include および exclude 属性

ASP.NET Core 2.x では `include` & `exclude` 属性が追加されます。 これらの属性は、含めるまたは除外するホスティング環境名に基づいて、囲まれたコンテンツのレンダリングを制御します。

### <a name="include-aspnet-core-20-and-later"></a>include (ASP.NET Core 2.0 以降)

`include` プロパティの動作は、ASP.NET Core 1.0 の `names` 属性の動作と同様です。

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>exclude (ASP.NET Core 2.0 以降)

一方、`exclude` プロパティでは、ユーザーが指定したものを除き、`EnvironmentTagHelper` ですべてのホスティング環境名の囲まれたコンテンツをレンダリングできます。

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
