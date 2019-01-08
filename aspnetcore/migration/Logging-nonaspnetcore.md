---
title: Microsoft.Extensions.Logging 2.1 から 2.2 へまたは 3.0 からの移行します。
author: pakrym
description: 2.1 から 2.2 または 3.0 に Microsoft.Extensions.Logging を使用する ASP.NET Core アプリケーションを移行する方法について説明します。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099472"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="0dadf-103">Microsoft.Extensions.Logging 2.1 から 2.2 へまたは 3.0 からの移行します。</span><span class="sxs-lookup"><span data-stu-id="0dadf-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="0dadf-104">この記事では、一般的な手順を使用する ASP.NET Core アプリケーションを移行するため`Microsoft.Extensions.Logging`2.1、2.2 または 3.0 から。</span><span class="sxs-lookup"><span data-stu-id="0dadf-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="0dadf-105">2.1 から 2.2 へ</span><span class="sxs-lookup"><span data-stu-id="0dadf-105">2.1 to 2.2</span></span>

<span data-ttu-id="0dadf-106">手動で作成`ServiceCollection`を呼び出すと`AddLogging`します。</span><span class="sxs-lookup"><span data-stu-id="0dadf-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="0dadf-107">2.1 の使用例:</span><span class="sxs-lookup"><span data-stu-id="0dadf-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="0dadf-108">2.2 の例:</span><span class="sxs-lookup"><span data-stu-id="0dadf-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="0dadf-109">2.1 から 3.0 へ</span><span class="sxs-lookup"><span data-stu-id="0dadf-109">2.1 to 3.0</span></span>

<span data-ttu-id="0dadf-110">3.0 では、次のように使用します。`LoggingFactory.Create`します。</span><span class="sxs-lookup"><span data-stu-id="0dadf-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="0dadf-111">2.1 の使用例:</span><span class="sxs-lookup"><span data-stu-id="0dadf-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="0dadf-112">3.0 の使用例:</span><span class="sxs-lookup"><span data-stu-id="0dadf-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="0dadf-113">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="0dadf-113">Additional resources</span></span>

<xref:fundamentals/logging/index>