# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="5078c-101">セキュリティで保護されたユーザー データのサンプルをビルド/実行する方法</span><span class="sxs-lookup"><span data-stu-id="5078c-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="5078c-102">Secret Manager ツールを使用したパスワードを設定します。</span><span class="sxs-lookup"><span data-stu-id="5078c-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="5078c-103">データベースを更新します。</span><span class="sxs-lookup"><span data-stu-id="5078c-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="5078c-104">プロジェクトで HTTPS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="5078c-104">Enable HTTPS in the project</span></span>
