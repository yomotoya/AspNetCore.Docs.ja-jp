---
title: "ASP.NET Core でのデータ保護のための非 DI 対応したシナリオ"
author: rick-anderson
description: "データ保護のシナリオをすることはできませんまたは依存関係の挿入によって提供されるサービスを使用したくない位置をサポートする方法を説明します。"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 1c84cfcf44086359a7d6900ca52781dc6f3b1b10
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET Core でのデータ保護のための非 DI 対応したシナリオ

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core データ保護システムは、通常[サービス コンテナーに追加された](xref:security/data-protection/consumer-apis/overview)依存性の注入 (DI) を使用して依存コンポーネントによって消費されているとします。 ただし、これは、実現可能または必要な場所の場合は、システムを既存のアプリケーションにインポートする際に特にです。

これらのシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)パッケージは、具象型を提供[DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider)、データ保護を使用する簡単な方法を提供します。DI に頼らずにします。 `DataProtectionProvider`実装を型[IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider)です。 構築する`DataProtectionProvider`のみを提供する必要があります、 [DirectoryInfo](/dotnet/api/system.io.directoryinfo)を次のコード例に示すように、プロバイダーの暗号化キーの格納場所を示すインスタンス。

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

既定では、`DataProtectionProvider`具象型は、生のキー マテリアルを暗号化しない前に、ファイル システムに保存します。 これは、ネットワーク共有とデータ保護システムに開発者ポイント rest での適切なキーの暗号化メカニズムの推測をできません自動的にシナリオをサポートします。

さらに、`DataProtectionProvider`具象型しない[アプリを分離](xref:security/data-protection/configuration/overview#per-application-isolation)既定です。 同じキー ディレクトリを使用してすべてのアプリと同じくらいペイロードを共有できます、[パラメーターの目的](xref:security/data-protection/consumer-apis/purpose-strings)と一致します。

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)コンス トラクターは、システムの動作の調整に使用できるオプションの構成コールバックを受け取ります。 以下のサンプルに示しますを明示的に呼び出すと復元の分離[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)です。 サンプルは、Windows DPAPI を使用して永続化されたキーを自動的に暗号化するシステムの構成も示します。 ディレクトリは、UNC 共有をポイントしている場合に関連するすべてのコンピューター間で共有の証明書を配布して、システムを使用する証明書ベースの暗号化への呼び出しを構成する[ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)です。

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> インスタンス、`DataProtectionProvider`具象型では、作成する手間がかかります。 アプリは、この種類の複数のインスタンスを保持し、それらすべてを使用している、同一のキー記憶域ディレクトリ場合は、アプリのパフォーマンスが低下する可能性があります。 使用する場合、`DataProtectionProvider`型、ことをお勧めこの種類を 1 回作成し、再利用可能な限りのことです。 `DataProtectionProvider`型とすべて[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)から作成されたインスタンスはスレッド セーフである複数の呼び出し元のです。
