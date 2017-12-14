---
title: "ASP.NET Core のデータ保護"
author: rick-anderson
description: "このドキュメントは、さまざまな ASP.NET Core データ保護に関するトピックの目次として機能します。"
keywords: "ASP.NET Core,データ保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 8b42e65bb6121355120a6f4fbe8cd4d1fea153de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a>ASP.NET Core のデータ保護: コンシューマー API、構成、拡張 API と実装

* [データ保護の概要](introduction.md)

* [データ保護 API の概要](using-data-protection.md)

* [コンシューマー API](consumer-apis/index.md)

  * [コンシューマー API の概要](consumer-apis/overview.md)

  * [目的文字列](consumer-apis/purpose-strings.md)

  * [目的の階層とマルチテナント](consumer-apis/purpose-strings-multitenancy.md)

  * [パスワードのハッシュ](consumer-apis/password-hashing.md)

  * [保護されたペイロードの有効期間の制限](consumer-apis/limited-lifetime-payloads.md)

  * [キーが取り消されたペイロードの保護の解除](consumer-apis/dangerous-unprotect.md)

* [構成](configuration/index.md)

  * [データ保護の構成](configuration/overview.md)

  * [既定の設定](configuration/default-settings.md)

  * [コンピューター全体のポリシー](configuration/machine-wide-policy.md)

  * [DI に対応しないシナリオ](configuration/non-di-scenarios.md)

* [拡張性 API](extensibility/index.md)

  * [Core の暗号の拡張性](extensibility/core-crypto.md)

  * [キー管理の拡張性](extensibility/key-management.md)

  * [その他の API](extensibility/misc-apis.md)

* [実装](implementation/index.md)

  * [認証された暗号化の詳細](implementation/authenticated-encryption-details.md)

  * [サブキーの派生と認証された暗号化](implementation/subkeyderivation.md)

  * [コンテキスト ヘッダー](implementation/context-headers.md)

  * [キーの管理](implementation/key-management.md)

  * [キー ストレージ プロバイダー](implementation/key-storage-providers.md)

  * [保存時のキーの暗号化](implementation/key-encryption-at-rest.md)

  * [キーの不変性と設定の変更](implementation/key-immutability.md)

  * [キー ストレージの形式](implementation/key-storage-format.md)

  * [短期データ保護プロバイダー](implementation/key-storage-ephemeral.md)

* [互換性](compatibility/index.md)

  * [アプリケーション間での Cookie の共有](compatibility/cookie-sharing.md)

  * [ASP.NET での <machineKey> の置換](compatibility/replacing-machinekey.md)
