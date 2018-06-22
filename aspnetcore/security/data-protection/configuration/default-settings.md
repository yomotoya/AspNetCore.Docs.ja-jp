---
title: データ保護キーの管理および ASP.NET Core の有効期間
author: rick-anderson
description: データ保護キーの管理および ASP.NET Core の有効期間について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 54259b1e2f37cdbbd551038e80f2b0fa1d77f196
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277813"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>データ保護キーの管理および ASP.NET Core の有効期間

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>キー管理

アプリは、運用環境を検出し、独自のキーの構成の処理を試行します。

1. アプリがホストされている場合[Azure アプリ](https://azure.microsoft.com/services/app-service/)、キーに永続化、 *%HOME%\ASP.NET\DataProtection-Keys*フォルダーです。 このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。
   * 保存中のキーは保護されていません。
   * *データ保護キー*フォルダーは、1 つのデプロイ スロットで、アプリのすべてのインスタンスにキー リングを提供します。
   * ステージングや運用などの別のデプロイ スロットでは、キー リングが共有されません。 たとえばステージング、実稼働のスワップまたは A を使用して、デプロイ スロット間で交換する場合に/テスト B、すべてのアプリ データ保護を使用して、前のスロット内のキーのリングを使用して格納されたデータの暗号化を解除することはできません。 これはそのクッキーを保護するデータの保護を使用するように、標準の ASP.NET Core の cookie 認証を使用するアプリからログアウトされているユーザーにつながります。 スロットに依存しないキーリングを希望する場合は、Azure Blob ストレージ、Azure Key Vault SQL ストアなど、外部キー リング プロバイダーを使用または Redis キャッシュします。

1. キーが永続化、ユーザー プロファイルを使用できる場合、 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*フォルダーです。 オペレーティング システムが Windows DPAPI を使用して、キーが暗号化されます。

1. アプリが IIS でホストされている場合、キーは、ワーカー プロセス アカウントにのみ ACLed である特殊なレジストリ キー HKLM レジストリに保存されます。 キーは DPAPI を使用して保存時に暗号化されます。

1. これらの条件のいずれも一致しない場合、キーは、現在のプロセスの外部で永続化されます。 プロセスがシャット ダウン、生成されたすべてのキーが失われます。

開発者は常にフル コントロールであり、キーの格納場所と方法をオーバーライドできます。 上記の最初の 3 つのオプションは、ほとんどのアプリ方法と似ています適切な既定値を提供する必要があります、ASP.NET  **\<machineKey >** 過去で自動生成ルーチンが機能していた。 最後に、フォールバック オプションを指定する、開発者が必要な唯一のシナリオは、[構成](xref:security/data-protection/configuration/overview)キーの永続化したいが、このフォールバックはまれな状況でのみ発生する場合は、先行します。

Docker ボリュームの共有ボリューム (ホストでマウントされたボリューム コンテナーの有効期間を超えて保持) であるフォルダーでキーを保持するか、Docker コンテナーでホストしている、または外部プロバイダーのように[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)です。 外部プロバイダーは、アプリがネットワークの共有ボリュームにアクセスできない場合にも web ファームのシナリオで役に立ちます (を参照してください[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)詳細については)。

> [!WARNING]
> 開発者が上記で説明したルールの上書き、特定のキー リポジトリにデータ保護システムを指す場合は、残りの部分でのキーの自動暗号化が無効です。 残りの部分にある保護を使用して再度有効にすることができます[構成](xref:security/data-protection/configuration/overview)です。

## <a name="key-lifetime"></a>キーの有効期間

キーでは、既定では 90 日間有効期間があります。 キーが期限切れになったときに、アプリは自動的に新しいキーを生成し、アクティブ キーとして新しいキーを設定します。 提供終了になったキーは、システムに残っている限り、アプリは、それらに保護されている任意のデータを暗号化解除できます。 参照してください[キー管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)詳細についてはします。

## <a name="default-algorithms"></a>既定のアルゴリズム

使用される既定のペイロード保護アルゴリズムは AES 256-CBC 機密性と HMACSHA256 の信頼性です。 512 ビット マスター _ キー、90 日ごとに変更はペイロードあたりごとにこれらのアルゴリズムに使用される 2 つのサブ キーを派生させるために使用します。 参照してください[サブキーを派生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)詳細についてはします。

## <a name="see-also"></a>関連項目

* [キー管理の拡張性](xref:security/data-protection/extensibility/key-management)
