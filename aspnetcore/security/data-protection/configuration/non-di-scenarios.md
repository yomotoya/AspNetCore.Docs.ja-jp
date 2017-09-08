---
title: "非 DI 対応したシナリオ"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a>非 DI 対応したシナリオ

データ保護システムが正常に設計された[サービス コンテナーに追加する](../consumer-apis/overview.md)DI メカニズムを使用して依存コンポーネントに提供するとします。 ただし、場合によっては、これを実現できない、システムを既存のアプリケーションにインポートする際に特にがあります。

これらのシナリオをサポートするためには、パッケージ Microsoft.AspNetCore.DataProtection.Extensions は具象型システムを使用して、データ保護 DI に固有のコード パスを経由せずに簡単な手段が提供される DataProtectionProvider を提供します。 型自体が IDataProtectionProvider を実装し、このプロバイダーの暗号化キーの格納場所 DirectoryInfo を提供することと同じくらい簡単にを構築します。

例:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

>[!WARNING]
> 既定では、DataProtectionProvider 具象型では暗号化されません生のキー マテリアル前に、ファイル システムに保存します。 これは、ネットワークへの開発者のポイントを共有している、シナリオをサポートする場合、データ保護システムに自動的を推測できません rest での適切なキーの暗号化メカニズムです。
>
>さらに、DataProtectionProvider 具象型は[アプリケーションの分離](overview.md#data-protection-configuration-per-app-isolation)既定では、すべてのアプリケーション、同じキー ディレクトリで参照されている共有できますペイロードその用途のパラメーターと一致する限りです。

必要な場合は、アプリケーション開発者はこれらの両方で対処できます。 DataProtectionProvider コンス トラクターを受け入れる、[オプションの構成のコールバック](overview.md#data-protection-configuration-callback)これは、システムの動作の調整を使用できます。 以下のサンプルは SetApplicationName、明示的な呼び出しを使用して復元中の分離を示し、自動的に Windows DPAPI を使用して永続化されたキーを暗号化するシステムの構成も示します。 ディレクトリは、UNC 共有をポイントしている場合に関連するすべてのコンピューター間で共有の証明書を配布して、呼び出しを経由して代わりに証明書ベースの暗号化を使用するシステムを構成する[ProtectKeysWithCertificate](overview.md#configuring-x509-certificate)です。

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

>[!TIP]
> DataProtectionProvider 具象型のインスタンスでは、作成する手間がかかります。 アプリケーションは、この種類の複数のインスタンスを保持し、すべて同一のキー記憶域ディレクトリにポイントしている場合は、アプリケーションのパフォーマンスが低下する可能性があります。 使用目的は、アプリケーション開発者がこの型を 1 回インスタンス化し、この参照を 1 つを再利用を可能な限り保持することができます。 DataProtectionProvider 型およびこれから作成されたすべての IDataProtector インスタンスは、スレッド セーフである複数の呼び出し元のです。
