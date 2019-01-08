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
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Microsoft.Extensions.Logging 2.1 から 2.2 へまたは 3.0 からの移行します。

この記事では、一般的な手順を使用する ASP.NET Core アプリケーションを移行するため`Microsoft.Extensions.Logging`2.1、2.2 または 3.0 から。

## <a name="21-to-22"></a>2.1 から 2.2 へ

手動で作成`ServiceCollection`を呼び出すと`AddLogging`します。

2.1 の使用例:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2.2 の例:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 から 3.0 へ

3.0 では、次のように使用します。`LoggingFactory.Create`します。

2.1 の使用例:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 の使用例:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>その他の技術情報

<xref:fundamentals/logging/index>