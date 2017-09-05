---
title: "キー管理の機能拡張"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: fb74905660015b9a83503e1f74b25c66ae9df9e3
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="key-management-extensibility"></a>キー管理の機能拡張

<a name=data-protection-extensibility-key-management></a>

>[!TIP]
> 読み取り、[キー管理](../implementation/key-management.md#data-protection-implementation-key-management)ようにこれらの Api の背後にある基本的な概念について説明します、このセクションを読む前にセクションです。

>[!WARNING]
> 次のインターフェイスのいずれかを実装する型はスレッド セーフである必要があります複数の呼び出し元のです。

## <a name="key"></a>キー

IKey インターフェイスは、暗号方式のキーの基本表現です。 用語キーは、抽象の意味での「暗号キー マテリアル」文字どおりの意味ではなくここで使用されます。 キーには、次のプロパティがあります。

* アクティブ化、作成、および有効期限の日付

* 失効状態

* キー識別子 (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

IKey が CreateEncryptor メソッドを作成するために使用できるを公開するさらに、 [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

IKey が CreateEncryptorInstance メソッドを作成するために使用できるを公開するさらに、 [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor)インスタンスは、このキーに関連付けられています。

---

> [!NOTE]
> 生の暗号化マテリアルを IKey インスタンスから取得する API はありません。

## <a name="ikeymanager"></a>IKeyManager

IKeyManager インターフェイスは、一般的なキーの格納、取得、および操作を担当するオブジェクトを表します。 3 つの高度な操作を公開します。

* 新しいキーを作成し、記憶域に保存します。

* 記憶域からすべてのキーを取得します。

* 1 つまたは複数のキーを取り消すし、記憶域に失効情報の保持されます。

>[!WARNING]
> IKeyManager を記述、非常に高度なタスクは、開発者の大部分しないで。 代わりに、ほとんどの開発者を活用してによって提供される機能、 [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager)クラスです。

<a name=data-protection-extensibility-key-management-xmlkeymanager></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

XmlKeyManager 型は、IKeyManager のボックスでの具象実装です。 キー エスクロー、残りの部分でのキーの暗号化など、いくつかに役立つ機能を提供します。 このシステム内のキーは、XML 要素として表現されます (具体的には、 [XElement](https://msdn.microsoft.com/library/system.xml.linq.xelement(v=vs.110).aspx))。

XmlKeyManager は、そのタスクを満たす操作の過程でその他のいくつかのコンポーネントに依存します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

* AlgorithmConfiguration は、新しいキーで使用されるアルゴリズムを決定します。

* IXmlRepository、どのコントロールのキーがストレージに保存されます。

* IXmlEncryptor [省略可能] が静止したキーを暗号化できます。

* IKeyEscrowSink [オプション]、キー エスクロー サービスを提供します。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* IXmlRepository、どのコントロールのキーがストレージに保存されます。

* IXmlEncryptor [省略可能] が静止したキーを暗号化できます。

* IKeyEscrowSink [オプション]、キー エスクロー サービスを提供します。

---

どのように、これらのコンポーネントは XmlKeyManager 内で一緒にワイヤードを示す高度な図を次に示します。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

   ![キーの作成](key-management/_static/keycreation2.png)

   *キーの作成/CreateNewKey*

CreateNewKey の実装で、一意な IAuthenticatedEncryptorDescriptor は、XML としてシリアル化しを作成する AlgorithmConfiguration コンポーネントを使用します。 キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。 暗号化されていない XML は、実行、IXmlEncryptor を通じて (必要な場合)、暗号化された XML ドキュメントが生成されます。 この暗号化されたドキュメントは、IXmlRepository 経由での長期的なストレージに保存されます。 (IXmlEncryptor が構成されていない暗号化されていないドキュメントに保存するか、IXmlRepository。)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![キーの作成](key-management/_static/keycreation1.png)

   *キーの作成/CreateNewKey*

CreateNewKey の実装で、一意な IAuthenticatedEncryptorDescriptor は、XML としてシリアル化しを作成する IAuthenticatedEncryptorConfiguration コンポーネントを使用します。 キー エスクロー シンクが存在する場合は、長期的な記憶域の生の (暗号化されていない) XML は、シンクに提供されます。 暗号化されていない XML は、実行、IXmlEncryptor を通じて (必要な場合)、暗号化された XML ドキュメントが生成されます。 この暗号化されたドキュメントは、IXmlRepository 経由での長期的なストレージに保存されます。 (IXmlEncryptor が構成されていない暗号化されていないドキュメントに保存するか、IXmlRepository。)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x のコア](#tab/aspnetcore2x)

   ![キーの取得](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![キーの取得](key-management/_static/keyretrieval1.png)

---

   *キーの取得/GetAllKeys*

GetAllKeys の実装では、キーと失効を表す XML ドキュメントは、基になる IXmlRepository から読み取られます。 これらのドキュメントが暗号化されている場合、システムは自動的に復号化にします。 XmlKeyManager では、個々 の IKey インスタンスで、ラップされている IAuthenticatedEncryptorDescriptor インスタンスに戻す、ドキュメントを逆シリアル化する適切な IAuthenticatedEncryptorDescriptorDeserializer インスタンスを作成します。 この IKey インスタンスのコレクションは、呼び出し元に返されます。

特定の XML 要素の詳細についてで参照できる、[キーの格納形式のドキュメント](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format)です。

## <a name="ixmlrepository"></a>IXmlRepository

IXmlRepository インターフェイスは、XML を永続化したり、バッキング ストアから XML を取得できる型を表します。 2 つの Api を公開します。

* GetAllElements(): IReadOnlyCollection<XElement>

* StoreElement (XElement 要素、文字列の friendlyName)

IXmlRepository の実装は、それらに渡される XML の解析に必要ありません。 上位のレイヤーを生成して、ドキュメントの解析について心配を非透過的に XML ドキュメントを扱うさせている必要があります。

IXmlRepository を実装する 2 つの組み込み具体的な種類があります: FileSystemXmlRepository と RegistryXmlRepository です。 参照してください、[キー記憶域プロバイダーのドキュメント](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers)詳細についてはします。 カスタム IXmlRepository を登録するとする別に使用する適切な方法バッキング ストア、Azure Blob ストレージなどです。 既定リポジトリ アプリケーション全体を変更するには、サービス プロバイダーのカスタム シングルトン IXmlRepository を登録します。

<a name=data-protection-extensibility-key-management-ixmlencryptor></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

IXmlEncryptor インターフェイスは、プレーン テキストの XML 要素を暗号化する型を表します。 単一の API を公開します。

* (XElement plaintextElement) の暗号化: EncryptedXmlInfo

XmlKeyManager が構成されている IXmlEncryptor の暗号化の方法でこれらの要素を実行し、シリアル化された IAuthenticatedEncryptorDescriptor「暗号化が必要」としてマークされているすべての要素が含まれている場合、および enciphered 要素ではなく保持されます。IXmlRepository にプレーン テキストの要素。 暗号化のメソッドの出力は、EncryptedXmlInfo オブジェクトです。 このオブジェクトは、結果の enciphered XElement との対応する要素を復号化するために使用できる IXmlDecryptor を表す型の両方を含むラッパーです。

IXmlEncryptor を実装している 4 つの組み込み具体的な種類があります: CertificateXmlEncryptor、DpapiNGXmlEncryptor、DpapiXmlEncryptor、および NullXmlEncryptor です。 参照してください、 [rest ドキュメント キーの暗号化](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)詳細についてはします。 既定のキーの暗号化に rest メカニズム アプリケーション全体を変更するには、サービス プロバイダーのカスタム シングルトン IXmlEncryptor を登録します。

## <a name="ixmldecryptor"></a>IXmlDecryptor

IXmlDecryptor インターフェイスは、型、IXmlEncryptor を介して enciphered が XElement の暗号化を解除する方法を把握していることを表します。 単一の API を公開します。

* (XElement encryptedElement) を暗号化解除: XElement

復号化メソッドは、IXmlEncryptor.Encrypt によって実行された暗号化を元に戻します。 通常、各具象 IXmlEncryptor 実装では、対応する IXmlDecryptor 具象があります。

IXmlDecryptor を実装する型は、次の 2 つのパブリック コンス トラクターのいずれかが必要です。

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> コンス トラクターに渡される IServiceProvider を null にすることがあります。

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

IKeyEscrowSink インターフェイスは、機密情報のエスクローを実行できる型を表します。 導入にこれは、シリアル化された記述子は、(暗号化マテリアル) などの機密情報を含む可能性がありますに注意してください、 [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)最初の場所を入力します。 ただし、事故発生する可能性があり、keyrings 削除できるか、または壊れています。

エスクロー インターフェイスには、構成されているいずれかが変換される前に、生のシリアル化された XML へのアクセスを許可、緊急脱出口が用意されています[IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor)です。 インターフェイスは、単一の API を公開します。

* ストア (Guid keyId、XElement 要素)

ビジネス ポリシーと一致するセキュリティで保護された方法で指定した要素を処理する IKeyEscrowSink 実装の責任です。 ここでは、証明書の秘密キーはエスクローされました。 既知の企業 X.509 証明書を使用して XML 要素を暗号化するエスクロー シンクの 1 つの可能な実装がある可能性があります。この CertificateXmlEncryptor 型に役立ちます。 IKeyEscrowSink 実装は、指定した要素を適切に保持する役割もできます。

既定ではエスクロー メカニズムが有効でない、サーバー管理者は、[これをグローバルに構成](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy)です。 構成することもプログラムを介して、 *IDataProtectionBuilder.AddKeyEscrowSink*メソッド以下のサンプルで示すようにします。 *AddKeyEscrowSink*メソッドのオーバー ロードのミラー、 *IServiceCollection.AddSingleton*と*IServiceCollection.AddInstance* IKeyEscrowSink とオーバー ロードシングルトン インスタンスを想定します。 IKeyEscrowSink の複数のインスタンスが登録されている場合、キーを同時に複数のメカニズムをエスクローできるようにに、1 つずつはキーの生成中に呼び出されます。

API IKeyEscrowSink インスタンスから情報を読み取るにはありません。 これはエスクロー機構の設計理論と一致: キー マテリアルを信頼された機関にアクセスできるようにするためのものが、および独自 escrowed マテリアルへのアクセスを含めることはできませんので、アプリケーション自体がない信頼された証明機関でします。

次のサンプル コードは、作成、"CONTOSODomain Admins"のメンバーだけが回復できるようにキーはエスクロー場所、IKeyEscrowSink を登録する方法について説明します。

> [!NOTE]
> このサンプルを実行するドメインに参加している Windows 8 で Windows Server 2012 マシン、およびドメイン コント ローラーが Windows Server 2012 以降にする必要があります。

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
