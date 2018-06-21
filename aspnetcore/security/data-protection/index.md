---
title: ASP.NET Core のデータ保護
author: rick-anderson
description: このドキュメントは、さまざまな ASP.NET Core データ保護に関するトピックの目次として機能します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277125"
---
# <a name="data-protection-in-aspnet-core"></a>ASP.NET Core のデータ保護

* [データ保護の概要](xref:security/data-protection/introduction)

* [データ保護 API の概要](xref:security/data-protection/using-data-protection)

* [コンシューマー API](xref:security/data-protection/consumer-apis/index)

  * [コンシューマー API の概要](xref:security/data-protection/consumer-apis/overview)

  * [目的文字列](xref:security/data-protection/consumer-apis/purpose-strings)

  * [目的の階層とマルチテナント](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [パスワードのハッシュ](xref:security/data-protection/consumer-apis/password-hashing)

  * [保護されたペイロードの有効期間の制限](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [キーが取り消されたペイロードの保護の解除](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [構成](xref:security/data-protection/configuration/index)

  * [ASP.NET Core データ保護の構成](xref:security/data-protection/configuration/overview)

  * [既定の設定](xref:security/data-protection/configuration/default-settings)

  * [コンピューター全体のポリシー](xref:security/data-protection/configuration/machine-wide-policy)

  * [DI に対応しないシナリオ](xref:security/data-protection/configuration/non-di-scenarios)

* [拡張性 API](xref:security/data-protection/extensibility/index)

  * [Core の暗号の拡張性](xref:security/data-protection/extensibility/core-crypto)

  * [キー管理の拡張性](xref:security/data-protection/extensibility/key-management)

  * [その他の API](xref:security/data-protection/extensibility/misc-apis)

* [実装](xref:security/data-protection/implementation/index)

  * [認証された暗号化の詳細](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [サブキーの派生と認証された暗号化](xref:security/data-protection/implementation/subkeyderivation)

  * [コンテキスト ヘッダー](xref:security/data-protection/implementation/context-headers)

  * [キーの管理](xref:security/data-protection/implementation/key-management)

  * [キー ストレージ プロバイダー](xref:security/data-protection/implementation/key-storage-providers)

  * [保存時のキーの暗号化](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [キーの不変性と設定](xref:security/data-protection/implementation/key-immutability)

  * [キー ストレージの形式](xref:security/data-protection/implementation/key-storage-format)

  * [短期データ保護プロバイダー](xref:security/data-protection/implementation/key-storage-ephemeral)

* [互換性](xref:security/data-protection/compatibility/index)

  * [ASP.NET Core での ASP.NET <machineKey> の置換](xref:security/data-protection/compatibility/replacing-machinekey)
