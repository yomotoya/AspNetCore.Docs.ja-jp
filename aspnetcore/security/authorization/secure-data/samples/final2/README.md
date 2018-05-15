# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="2541c-101">ユーザーのセキュリティで保護されたデータ サンプルをビルド/実行する方法</span><span class="sxs-lookup"><span data-stu-id="2541c-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="2541c-102">シークレット マネージャー ツールを使用してパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="2541c-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="2541c-103">データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="2541c-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="2541c-104">プロジェクトで SSL を有効にします。</span><span class="sxs-lookup"><span data-stu-id="2541c-104">Enable SSL in the project</span></span>