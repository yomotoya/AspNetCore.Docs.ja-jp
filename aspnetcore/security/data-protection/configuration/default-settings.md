---
title: "キー管理と有効期間"
author: rick-anderson
description: "キー管理と有効期間について説明します。"
keywords: "ASP.NET Core キー管理を DPAPI データ保護"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 913eda69f88ef05a990d9465024f4fa6b08cd1b7
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="key-management-and-lifetime"></a>キー管理と有効期間

<a name=data-protection-default-settings></a>

## <a name="key-management"></a>キー管理

システムしようと、運用環境を検出し、適切な 0 の動作の既定の設定を提供します。 使用されるヒューリスティックのとおりです。

1. 場合は、システムは、Azure Web サイトでホストされている、キーが"%home%\asp.net\dataprotection-keys"フォルダーに保存されます。 このフォルダーはネットワーク ストレージによってバックアップされは、アプリケーションをホストしているすべてのマシン間で同期します。 残りの部分では、キーは保護されません。 このフォルダーは、単一のデプロイ スロット内のアプリケーションのすべてのインスタンスにキー リングを提供します。 ステージングと運用環境などの別のデプロイ スロットは、キーのリングを共有していません。 たとえば実稼働環境にステージングのスワップまたは A を使用して、デプロイ スロット間で交換する場合に/テスト B、データ保護を使用して任意のシステムは、前のスロット内のキーのリングを使用して格納されたデータの暗号化を解除することはできません。 これは、そのクッキーを保護するデータの保護を使用するように、標準の ASP.NET cookie ミドルウェアを使用する ASP.NET アプリケーションからログアウトするユーザーにつながります。 スロットに依存しないキーリングを希望する場合は、Azure Blob ストレージ、Azure Key Vault SQL ストアなど、外部キー リング プロバイダーを使用または Redis キャッシュします。

2. ユーザー プロファイルを使用できる場合は、キーが"%localappdata%\asp.net\dataprotection-keys"フォルダーに保存されます。 さらに、オペレーティング システムが Windows の場合は、DPAPI を使用して残りの部分で、暗号化されます。

3. アプリケーションが IIS でホストされている場合、キーは、ワーカー プロセス アカウントにのみ ACLed である特殊なレジストリ キー HKLM レジストリに保存されます。 キーは、DPAPI を使用して暗号化されます。

4. いずれの条件に一致する場合、現在のプロセスの外部キーは保存されません。 プロセスがシャット ダウン、生成されたすべてのキーが失われます。

開発者は常にフル コントロールであり、キーの格納場所と方法をオーバーライドできます。 上記の最初の 3 つのオプションには、方法と似ていますほとんどのアプリケーションの適切な既定値が必要があります、ASP.NET<machineKey>過去で自動生成ルーチンが機能していた。 最終的にフォール フェールバック オプションは、唯一のシナリオを指定する開発者を本当に必要とする[構成](overview.md)キーの永続化したいが、このフォールバックはまれな状況でのみ発生する場合は、先行します。

>[!WARNING]
> 開発者は、このヒューリスティックをオーバーライドし、特定のキー リポジトリにデータ保護システムを指す、残りの部分でのキーの自動暗号化は無効になります。 残りの部分では、保護を使用して再度有効になる可能性が[構成](overview.md)です。

## <a name="key-lifetime"></a>キーの有効期間

既定でキーには、90 日間有効期間があります。 キーが期限切れになったときに、システムは自動的に新しいキーを生成し、アクティブ キーとして新しいキーを設定します。 提供終了になったキーがシステムに残っている限り、それらに保護されている任意のデータを復号化することができます。 参照してください[キー管理](../implementation/key-management.md#data-protection-implementation-key-management-expiration)詳細についてはします。

## <a name="default-algorithms"></a>既定のアルゴリズム

使用される既定のペイロード保護アルゴリズムは AES 256-CBC 機密性と HMACSHA256 の信頼性です。 90 日ごとにロールバックされます、512 ビット マスター _ キーをペイロードあたりごとにこれらのアルゴリズムに使用される 2 つのサブ キーを派生させるために使用されます。 参照してください[サブキーを派生](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad)詳細についてはします。
