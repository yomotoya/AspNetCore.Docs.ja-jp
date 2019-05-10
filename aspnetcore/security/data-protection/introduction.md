---
title: ASP.NET Core データ保護
author: rick-anderson
description: データ保護の概念と ASP.NET Core データ保護 Api の設計原則について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/introduction
ms.openlocfilehash: 37f170a3e8a46ef2215b0999358d46dd402636df
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897989"
---
# <a name="aspnet-core-data-protection"></a>ASP.NET Core データ保護

多くの場合、web アプリケーションは、機密データを格納する必要があります。 Windows デスクトップ アプリケーションの DPAPI を提供しますが、これは web アプリケーションに適したではありません。 ASP.NET Core データ保護スタックは、単純な使いやすい暗号化の API が開発者キー管理とローテーションをなど、データの保護を使用することができますを提供します。

ASP.NET Core データ保護スタックが長期にわたって置き換えとして機能するように設計、 &lt;machineKey&gt; ASP.NET 内の要素の 1.x 4.x です。 これは、ほとんどのユース ケースが最新のアプリケーションが発生する可能性があるため、ボックスのソリューションを提供しますが、古い暗号化スタックの欠点の多くに対処する設計されました。

## <a name="problem-statement"></a>問題の説明

全体的な問題ステートメントは、1 つの文で簡潔に記述できます。後で取得は、信頼できる情報を保持する必要がありますが、永続化メカニズムを信頼しません。 Web には、これが書き込む「必要な信頼されていないクライアント経由でのラウンドト リップの信頼された状態にします」

この標準的な例は、認証 cookie またはベアラー トークンです。 サーバーを生成、"Groot いて、xyz のアクセス許可がある"トークンし、クライアントに渡すことです。 いくつかの将来の日付で、クライアントが、サーバーにそのトークンを提示しますが、サーバーがある種のクライアントがトークンを偽造されていないことを保証する必要があります。 したがって、最初の要件: 信頼性 (別名。 整合性、改ざんから)。

永続化の状態は、サーバーによって信頼されて、ために、この状態で運用環境に固有の情報が含まれることが予想されます。 ファイルのパスをアクセス許可をハンドル、またはその他の間接参照のフォームまたは他の一部のサーバーに固有のデータになります。 このような情報一般に公開すべき、信頼されていないクライアントにします。 したがって、2 番目の要件: 機密性。

最後に、ため最新のアプリケーションがコンポーネント化、おさらいは個々 のコンポーネントは、システム内の他のコンポーネントに関係なく、このシステムを利用するでしょう。 たとえば、ベアラー トークンのコンポーネントは、このスタックで使用されている場合は可能性があります、同じスタックを使用することもある CSRF 対策機構から競合することがなく動作する必要があります。 したがって最終的な要件: 分離します。

提供できますさらに制約、要件の範囲を限定するためにします。 暗号方式の範囲内で動作するすべてのサービスが平等に信頼されているし、データが生成または、直接の管理下にあるサービスの外部で使用する必要があることを仮定します。 さらに、web サービスへの各要求を暗号方式 1 回以上移行するための操作ができるだけ高速である必要があります。 これにより、対称暗号化は、ここでは、最適となど、必要な時間まで非対称暗号化方式を割引います。

## <a name="design-philosophy"></a>デザインの理念

既存のスタックの問題を識別することによって開始されています。 後で既存のソリューションのランドス ケープを対象し、した、既存のソリューション非常になかった検索機能を終了しました。 ここには、いくつかの基本原則に基づいてソリューションを設計されています。

* システムでは、わかりやすくするための構成を提供する必要があります。 システム構成になり、開発者がスムーズに理想的です。 開発者が (キーのリポジトリ) などの特定の側面を構成する必要がある場合、これら特定の構成を簡単に行うに考慮対象としてを指定する必要があります。

* 単純なコンシューマー向けの API を提供します。 Api を正しく使用する簡単かつ正しく使用するが困難になります。

* 開発者は、キー管理の原則について説明します。 システムには、アルゴリズムの選択と、開発者の代わりにキーの有効期間を処理する必要があります。 理想的には、開発者は、生のキー マテリアルへのアクセスをすらが必要です。

* 可能であれば rest でキーを保護する必要があります。 システムは、適切な既定の保護メカニズムを確認し、自動的に適用する必要があります。

これらの原則を念頭に、単純なを開発した[使いやすい](xref:security/data-protection/using-data-protection)データ保護スタック。

ASP.NET Core データ保護 Api が機密のペイロードの無期限の永続化の主に意図されていません。 などの他のテクノロジ[Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx)と[Azure Rights Management](/rights-management/)無制限のストレージのシナリオに適したこれに応じて拡大縮小の強力なキー管理機能があるとします。 ただし、機密データの長期的な保護の ASP.NET Core データ保護 Api を使用してから、開発者を禁止すること何もないです。

## <a name="audience"></a>対象ユーザー

データ保護システムは、5 つのメイン パッケージに分割されます。 これらの Api のさまざまなターゲットの 3 つの主な対象ユーザー。

1. [コンシューマー Api の概要](xref:security/data-protection/consumer-apis/overview)アプリケーションとフレームワークの開発者を対象します。

   "しないスタックの動作方法について、またはを構成する方法について説明します。 単純にする Api を正常に使用する可能性が高いできるだけ単純になんらかの操作方法を実行します。"

2. [構成 Api](xref:security/data-protection/configuration/overview)アプリケーション開発者およびシステム管理者を対象します。

   「必要がありますに自分の環境には、既定以外のパスや設定が必要であるデータ保護システムに指示します。」

3. カスタム ポリシーの実装を担当 Api 対象の機能拡張開発者。 これらの Api の使用状況はまれな状況に限定し、が発生した、セキュリティ対応の開発者は。

   "本当に固有の動作要件があるため、システム内でコンポーネント全体を置換する必要があります。 私は自分の要件を満たしているプラグインを構築するために、API サーフェスのことに使用される部分を学習する意欲。"

## <a name="package-layout"></a>パッケージのレイアウト

データ保護スタックは、5 つのパッケージで構成されます。

* [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)が含まれています、<xref:Microsoft.AspNetCore.DataProtection.IDataProtectionProvider>と<xref:Microsoft.AspNetCore.DataProtection.IDataProtector>データ保護サービスを作成するインターフェイス。 これらの型を操作するための便利な拡張メソッドも含まれています (たとえば、 [IDataProtector.Protect](xref:Microsoft.AspNetCore.DataProtection.DataProtectionCommonExtensions.Protect*))。 データ保護システムが他の場所でインスタンス化された API を使用している場合は、参照`Microsoft.AspNetCore.DataProtection.Abstractions`します。

* [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/)コア暗号化操作、キー管理、構成、および機能拡張を含む、データ保護システムのコア実装が含まれています。 データ保護システムをインスタンス化する (たとえば、追加することを<xref:Microsoft.Extensions.DependencyInjection.IServiceCollection>) 変更、またはその動作を拡張するを参照または`Microsoft.AspNetCore.DataProtection`。

* [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/)開発者が役に立つ場合がありますが、コア パッケージに属することはありませんが、追加の Api が含まれています。 たとえば、このパッケージが依存関係の挿入せず、ファイル システム上の場所にキーを格納するデータ保護システムのインスタンスを作成するファクトリ メソッドが含まれます (を参照してください<xref:Microsoft.AspNetCore.DataProtection.DataProtectionProvider>)。 保護されたペイロードの有効期間を制限するための拡張メソッドも含まれています (を参照してください<xref:Microsoft.AspNetCore.DataProtection.ITimeLimitedDataProtector>)。

* [Microsoft.AspNetCore.DataProtection.SystemWeb](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.SystemWeb/)をリダイレクトする既存の ASP.NET 4.x アプリケーションをインストールすることができます、`<machineKey>`新しい ASP.NET Core データ保護スタックを使用する操作。 詳細については、「 <xref:security/data-protection/compatibility/replacing-machinekey> 」を参照してください。

* [Microsoft.AspNetCore.Cryptography.KeyDerivation](https://www.nuget.org/packages/Microsoft.AspNetCore.Cryptography.KeyDerivation/) PBKDF2 パスワード ハッシュのルーチンの実装を提供しがユーザーのパスワードを安全に処理するシステムで使用できます。 詳細については、「 <xref:security/data-protection/consumer-apis/password-hashing> 」を参照してください。

## <a name="additional-resources"></a>その他の技術情報

<xref:host-and-deploy/web-farm>
