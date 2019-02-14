---
title: データ保護キーの管理と ASP.NET Core での有効期間
author: rick-anderson
description: データ保護キーの管理と ASP.NET Core での有効期間について説明します。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159212"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>データ保護キーの管理と ASP.NET Core での有効期間

作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>キーの管理

アプリは、その運用環境を検出して、独自のキーの構成の処理を試行します。

1. アプリがホストされている場合[Azure Apps](https://azure.microsoft.com/services/app-service/)、キーに保存される、 *%HOME%\ASP.NET\DataProtection-Keys*フォルダー。 このフォルダーはネットワーク ストレージにバックアップされ、アプリをホストしているすべてのマシンで同期されています。
   * 保存中のキーは保護されていません。
   * *DataProtection キー*フォルダーは、1 つのデプロイ スロットでアプリのすべてのインスタンスをキー リングを提供します。
   * ステージングや運用などの別のデプロイ スロットでは、キー リングが共有されません。 テスト B をたとえばステージング、実稼働のスワップまたは A を使用して、デプロイ スロット間をスワップする場合は/任意のアプリ データ保護を使用してキー リングを使用して、前のスロット内で格納されたデータを復号化することはできません。 これにより、ユーザーのデータ保護を使用して、その cookie を保護する標準の ASP.NET Core の cookie 認証を使用するアプリからログに記録します。 スロットに依存しないキー リングを希望する場合は、Azure Blob Storage、Azure Key Vault では、SQL ストアなど、外部キー リング プロバイダーを使用して、または Redis cache。

1. キーに保存されるユーザー プロファイルを使用できる場合、 *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys*フォルダー。 オペレーティング システムが Windows の場合は、キーは DPAPI を使用して暗号化します。

   アプリ プールの[setProfileEnvironment 属性](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration)も有効にする必要があります。 `setProfileEnvironment` の既定値は `true` です。 一部のシナリオ (たとえば、Windows OS) で`setProfileEnvironment`に設定されている`false`します。 ユーザー プロファイル ディレクトリに保存されていないキーが必要です。

   1. 移動し、 *%windir%/system32/inetsrv/config*フォルダー。
   1. 開く、 *applicationHost.config*ファイル。
   1. `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` 要素を探します。
   1. 確認します、`setProfileEnvironment`属性が含まれていない、既定値、値を`true`明示的に属性の値を設定または`true`します。

1. 場合は、アプリが IIS でホストされる、キーでは、ワーカー プロセス アカウントのみに対して特別なレジストリ キー HKLM レジストリに保存されます。 キーは DPAPI を使用して保存時に暗号化されます。

1. 一致するこれらの条件がない、キーは、現在のプロセスの外部で永続化されます。 プロセスがシャット ダウン、生成されたすべてのキーが失われます。

開発者は、常に完全に制御し、キーの格納場所と方法をオーバーライドできます。 上記の最初の 3 つのオプションは、方法のようなほとんどのアプリの適切な既定値を提供する必要があります、ASP.NET  **\<machineKey >** 自動生成ルーチンが過去で動作します。 最後に、フォールバック オプションを指定する、開発者が必要な唯一のシナリオは、[構成](xref:security/data-protection/configuration/overview)前払いの場合は、キーの永続化したいが、このフォールバックは、まれな状況でのみ発生します。

Docker ボリューム (共有ボリュームまたはホストにマウントされたボリューム コンテナーの有効期間を超えて保持) にあるフォルダーで、キーを保持するか、Docker コンテナーでホストする場合や、外部プロバイダーでなど[Azure Key Vault](https://azure.microsoft.com/services/key-vault/)または[Redis](https://redis.io/)します。 外部プロバイダーは、アプリがネットワークの共有ボリュームにアクセスできない場合にも web ファームのシナリオで役に立ちます (を参照してください[PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem)詳細については)。

> [!WARNING]
> 開発者は、上記で説明した規則をオーバーライドし、特定のキー リポジトリにデータ保護システムを指す、保存時のキーの自動暗号化は無効になります。 保存時の保護を使用して再度有効にできる[構成](xref:security/data-protection/configuration/overview)します。

## <a name="key-lifetime"></a>キーの有効期間

キーでは、既定で有効期間は 90 日間があります。 キーの期限が切れると、アプリは自動的に新しいキーを生成し、アクティブ キーとして新しいキーを設定します。 提供終了のキーがシステムに残っている限り、アプリは、それらに保護されていたデータを暗号化解除できます。 参照してください[キー管理](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling)詳細についてはします。

## <a name="default-algorithms"></a>既定のアルゴリズム

使用される既定のペイロードの保護アルゴリズムが、AES-CBC 256 機密性、および HMACSHA256 の信頼性。 90 日ごとに変更、512 ビットのマスター キーは、ペイロードごとにこれらのアルゴリズムを使用する 2 つのサブ キーの派生に使用されます。 参照してください[サブキーの派生](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation)詳細についてはします。

## <a name="additional-resources"></a>その他の技術情報

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
