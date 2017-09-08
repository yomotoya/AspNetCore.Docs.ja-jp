---
title: "環境タグ ヘルパーの ASP.NET Core"
author: pkellner
description: "すべてのプロパティを含む ASP.Net Core 環境タグ ヘルパーの定義"
keywords: "ASP.NET Core、タグ ヘルパー"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/EnvironmentTagHelper
ms.openlocfilehash: 5870d9ebd02becf29f892c91310022d3b9a6b7af
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>環境タグ ヘルパーの ASP.NET Core

によって[Peter Kellner](http://peterkellner.net)と[Hisham Bin Ateya](https://twitter.com/hishambinateya)

環境タグ ヘルパーは、現在のホスト環境に基づいて囲まれたコンテンツを条件付きで表示します。 その 1 つの属性`names`環境のコンマ区切り一覧を示します、された名前と現在の環境に一致するいずれかの場合は、かっこで囲んだ表示される内容がトリガーされます。

## <a name="environment-tag-helper-attributes"></a>環境タグ ヘルパー属性

### <a name="names"></a>名前

1 つのホスティング環境名またはホスティング収録されているコンテンツのレンダリングをトリガーする環境名のコンマ区切りの一覧を受け入れます。

これらの値は、ASP.NET Core の静的プロパティから返された現在の値と比較されます`HostingEnvironment.EnvironmentName`です。  この値は、次のいずれかの:**ステージング**です。**開発**または**運用**です。 比較では、小文字を無視します。

有効な例`environment`タグ ヘルパーは。

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>属性を含めたり除外したり

ASP.NET Core 2.x の追加、 `include`  &  `exclude`属性。 属性のコントロールが含まれるか除外されたホスティング環境名に基づいて含まれているコンテンツのレンダリングこれらです。

### <a name="include-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 以降が含まれます

`include`プロパティの同様の動作には、 `names` ASP.NET Core 1.0 での属性です。

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 以降を除外します。

これに対し、`exclude`プロパティにより、`EnvironmentTagHelper`指定したそのを除くすべてのホスティング環境の名前をかっこで囲んだコンテンツをレンダリングします。

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>