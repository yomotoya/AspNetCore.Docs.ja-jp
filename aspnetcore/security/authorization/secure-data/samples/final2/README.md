---
ms.openlocfilehash: 6246247788eefc8f094ec1a7f156cb36715edb53
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210107"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="b84e9-101">セキュリティで保護されたユーザー データのサンプルをビルド/実行する方法</span><span class="sxs-lookup"><span data-stu-id="b84e9-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="b84e9-102">Secret Manager ツールを使用したパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="b84e9-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="b84e9-103">データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="b84e9-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="b84e9-104">プロジェクトで HTTPS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="b84e9-104">Enable HTTPS in the project</span></span>
