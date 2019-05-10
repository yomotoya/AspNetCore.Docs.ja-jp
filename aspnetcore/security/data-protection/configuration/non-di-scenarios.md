---
title: ASP.NET Core でのデータ保護のために非 DI 対応シナリオ
author: rick-anderson
description: できませんまたは依存関係の挿入によって提供されるサービスを使用したくないの位置に、データ保護シナリオをサポートする方法について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 34354c8443f6ae00bcce6ad9bdb6c11aaaa25bf8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897309"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>ASP.NET Core でのデータ保護のために非 DI 対応シナリオ

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core データ保護システムは、通常[サービス コンテナーに追加](xref:security/data-protection/consumer-apis/overview)と依存関係の注入 (DI) を使用して依存コンポーネントで使用します。 ただし、これは不可能または、必要な場合があります、既存のアプリをシステムをインポートするときに特にです。

このようなシナリオをサポートするために、 [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)パッケージは、具象型を提供します[DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider)、データ保護を使用する簡単な方法を提供します。DI に頼らずにします。 `DataProtectionProvider`実装の入力[IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider)します。 構築`DataProtectionProvider`のみを提供する必要があります、 [DirectoryInfo](/dotnet/api/system.io.directoryinfo)を次のコード サンプルに示すように、プロバイダーの暗号化キーの格納場所を示すインスタンス。

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

既定で、`DataProtectionProvider`具象型が生のキー マテリアルを暗号化する前に、ファイル システムに永続化します。 これは、ネットワーク共有とデータ保護システム開発者ポイント rest での適切なキーの暗号化メカニズムの減少をできません。 自動的にシナリオをサポートします。

さらに、`DataProtectionProvider`具象型は[アプリを分離](xref:security/data-protection/configuration/overview#per-application-isolation)既定。 同じディレクトリにキーを使用してすべてのアプリが同じくらいペイロードを共有、[目的でパラメーター](xref:security/data-protection/consumer-apis/purpose-strings)と一致します。

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)コンス トラクターは、システムの動作を調整するのに使用できるオプションの構成コールバックを受け取ります。 次の例に示しますを明示的に呼び出すと復元の分離[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)します。 このサンプルでは、Windows DPAPI を使用して永続化されたキーを自動的に暗号化するシステムの構成も示します。 関連するすべてのコンピューター間で共有証明書を配布してへの呼び出しで証明書ベースの暗号化を使用するシステムを構成するになる場合、ディレクトリは、UNC 共有を指して、 [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate)します。

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> インスタンス、`DataProtectionProvider`具象型は作成する手間がかかります。 アプリは、この種類の複数のインスタンスを保持し、それらすべてを使用している同じディレクトリにキー記憶域場合は、アプリのパフォーマンスが低下する可能性があります。 使用する場合、`DataProtectionProvider`型、お勧めこの型を 1 回作成し、可能な限り再利用します。 `DataProtectionProvider`タイプとすべて[IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector)から作成されたインスタンスはスレッド セーフの複数の呼び出し元。
