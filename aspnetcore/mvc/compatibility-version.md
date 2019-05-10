---
title: ASP.NET Core MVC の互換バージョン
author: rick-anderson
description: ASP.NET Core の Startup クラスがサービスとアプリケーションの要求パイプラインをどのように構成しているかを説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085886"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>ASP.NET Core MVC の互換バージョン

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> メソッドを使用すると、ASP.NET Core MVC 2.1 以降に導入されている、互換性に影響する重大な変更をオプトインまたはオプトアウトすることができます。 互換性に影響する可能性のあるこれらの重大な変更は、ほとんどの場合、MVC サブシステムの動作方法と、ランタイムで**ユーザーのコード**が呼び出される方法についてです。 オプトインした場合、最新の動作と ASP.NET Core の最新の動作を得ることができます。

次のコードにより、互換性モードは ASP.NET Core 2.2 に設定されます。

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

最新のバージョン (`CompatibilityVersion.Version_2_2`) を使用してアプリをテストすることをお勧めします。 最新のバージョンを使用しても、ほとんどのアプリで重大な動作変更はないと見込まれます。

`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すアプリは、ASP.NET Core 2.1 MVC およびそれ以降の 2. バージョンで導入された重大な動作変更の可能性から保護されています。 この保護とは、次のとおりです。

* 2.1 以降のすべての変更には該当しません。これは、MVC サブシステムの ASP.NET Core ランタイムの互換性に影響する可能性のある重大な変更のみを対象としています。
* 次のメジャー バージョンには適用されません。

`SetCompatibilityVersion` を呼び出さ**ない** ASP.NET Core 2.1 およびそれ以降の 2.x アプリケーションの既定の互換性は、2.0 の互換性です。 つまり、`SetCompatibilityVersion` を呼び出さないことは、`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` を呼び出すことと同じです。

次のコードでは、以下の動作を除き、互換性モードを ASP.NET Core 2.2 に設定します。

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

互換性に影響する重大な変更があったアプリでは、適切な互換性スイッチを使用することにより、次が得られます。

* 最新のリリースを使用し、互換性に影響する特定の重大な変更をオプトアウトできます。
* アプリが最新の変更に対応するよう、更新を行う時間を得られます。

<xref:Microsoft.AspNetCore.Mvc.MvcOptions> ドキュメントでは、変更があった個所とそれらの改善が多くのユーザーに与えるメリットについて説明しています。

将来的に、[ASP.NET Core 3.0 バージョン](https://github.com/aspnet/Home/wiki/Roadmap)がリリースされます。 3.0 バージョンでは、互換性スイッチによってサポートされている古い動作は削除されます。 これらの正の変更は、ほぼすべてのユーザーにとってメリットとなると感じています。 これらの変更を導入することにより、ほとんどのアプリでメリットを得られるようになり、またその他のユーザーにはアプリをアップデートするための時間ができます。
