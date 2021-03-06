---
title: "クイックスタート - Linux 用 Azure Kubernetes クラスター"
description: "Azure CLI を使用して AKS で Linux コンテナー用 Kubernetes クラスターを作成する方法を簡単に説明します。"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: quickstart
ms.date: 11/15/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc, devcenter
ms.openlocfilehash: 1933fdf881adb7ea8e0f4b6ebca6abe074124c6b
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/06/2017
---
# <a name="deploy-an-azure-container-service-aks-cluster"></a>Azure Container Service (AKS) クラスターのデプロイ

このクイック スタートでは、Azure CLI を使用して AKS クラスターをデプロイします。 次に、このクラスターで、Web フロントエンドと Redis インスタンスで構成される複数コンテナー アプリケーションが実行されます。 完了すると、このアプリケーションはインターネット経由でアクセス可能になります。

![Azure Vote にブラウザーでアクセスしたところ](media/container-service-kubernetes-walkthrough/azure-vote.png)

このクイック スタートは、Kubernetes の概念についての基礎知識があることを前提としています。Kubernetes の詳細については、[Kubernetes のドキュメント]( https://kubernetes.io/docs/home/)を参照してください。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI をローカルにインストールして使用する場合、このクイックスタートを実施するには、Azure CLI バージョン 2.0.21 以降を実行している必要があります。 バージョンを確認するには、`az --version` を実行します。 インストールまたはアップグレードする必要がある場合は、[Azure CLI のインストール]( /cli/azure/install-azure-cli)に関するページを参照してください。

## <a name="enabling-aks-preview-for-your-azure-subscription"></a>Azure サブスクリプションでの AKS プレビューの有効化
AKS がプレビューである間、新しいクラスターを作成するには、サブスクリプションに機能フラグが必要です。 使用する任意の数のサブスクリプションに対して、この機能を要求することができます。 AKS プロバイダーを登録するには、次のように `az provider register` コマンドを使用します。

```azurecli-interactive
az provider register -n Microsoft.ContainerService
```

登録すると、AKS で Kubernetes クラスターを作成できるようになります。

## <a name="create-a-resource-group"></a>リソース グループの作成

[az group create](/cli/azure/group#create) コマンドでリソース グループを作成します。 Azure リソース グループとは、Azure リソースのデプロイと管理に使用する論理グループです。

次の例では、*myResourceGroup* という名前のリソース グループを *eastus* に作成します。

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

出力:

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-aks-cluster"></a>AKS クラスターの作成

次の例では、*myK8sCluster* という名前のクラスターを 1 つのノードで作成します。

```azurecli-interactive
az aks create --resource-group myResourceGroup --name myK8sCluster --node-count 1 --generate-ssh-keys
```

数分してコマンドが完了すると、このクラスターに関する情報が JSON 形式で表示されます。

## <a name="connect-to-the-cluster"></a>クラスターへの接続

Kubernetes クラスターを管理するには、Kubernetes のコマンドライン クライアントである [kubectl](https://kubernetes.io/docs/user-guide/kubectl/) を使用します。

Azure Cloud Shell を使用している場合、kubectl は既にインストールされています。 ローカルにインストールする場合は、次のコマンドを実行します。


```azurecli
az aks install-cli
```

Kubernetes クラスターに接続するように kubectl を構成するには、次のコマンドを実行します。 この手順では、資格情報をダウンロードし、Kubernetes CLI を構成してそれらの資格情報を使用します。

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myK8sCluster
```

クラスターへの接続を確認するには、[kubectl get](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) コマンドを使用して、クラスター ノードの一覧を返します。

```azurecli-interactive
kubectl get nodes
```

出力:

```
NAME                          STATUS    ROLES     AGE       VERSION
k8s-myk8scluster-36346190-0   Ready     agent     2m        v1.7.7
```

## <a name="run-the-application"></a>アプリケーションの実行

Kubernetes のマニフェスト ファイルでは、どのようなコンテナー イメージを実行するかというようなことも含め、クラスターの望ましい状態を定義します。 この例では、マニフェストを使用して、Azure Vote アプリケーションを実行するために必要なすべてのオブジェクトを作成します。

`azure-vote.yml` という名前のファイルを作成し、そこに以下の YAML コードをコピーします。 Azure Cloud Shell で作業している場合、仮想システムまたは物理システムで作業するときと同じように vi または Nano を使用してこのファイルを作成できます。

```yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-back
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-back
    spec:
      containers:
      - name: azure-vote-back
        image: redis
        ports:
        - containerPort: 6379
          name: redis
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-back
spec:
  ports:
  - port: 6379
  selector:
    app: azure-vote-back
---
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      containers:
      - name: azure-vote-front
        image: microsoft/azure-vote-front:redis-v1
        ports:
        - containerPort: 80
        env:
        - name: REDIS
          value: "azure-vote-back"
---
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

[kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) コマンドを使用してアプリケーションを実行します。

```azurecli-interactive
kubectl create -f azure-vote.yml
```

出力:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-the-application"></a>アプリケーションをテストする

アプリケーションが実行されると、アプリケーション フロントエンドをインターネットに公開する [Kubernetes サービス](https://kubernetes.io/docs/concepts/services-networking/service/)が作成されます。 このプロセスが完了するまでに数分かかることがあります。

進行状況を監視するには、[kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) コマンドを `--watch` 引数と一緒に使用します。

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

最初に、*azure-vote-front* サービスの *EXTERNAL-IP* が "*保留中*" として表示されます。

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* アドレスが "*保留中*" から "*IP アドレス*" に変わったら、`CTRL-C` を使用して kubectl ウォッチ プロセスを停止します。

```
azure-vote-front   LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

これで、外部 IP アドレスを参照して Azure Vote アプリを表示できるようになりました。

![Azure Vote にブラウザーでアクセスしたところ](media/container-service-kubernetes-walkthrough/azure-vote.png)

## <a name="delete-cluster"></a>クラスターを削除する
クラスターが必要なくなったら、[az group delete](/cli/azure/group#delete) コマンドを使用して、リソース グループ、コンテナー サービス、およびすべての関連リソースを削除できます。

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="get-the-code"></a>コードの入手

このクイック スタートでは、事前に作成したコンテナー イメージを使用して、Kubernetes のデプロイを作成しました。 関連するアプリケーション コード、Dockerfile、および Kubernetes マニフェスト ファイルは、GitHub で入手できます。

[https://github.com/Azure-Samples/azure-voting-app-redis](https://github.com/Azure-Samples/azure-voting-app-redis.git)

## <a name="next-steps"></a>次のステップ

このクイック スタートでは、Kubernetes クラスターをデプロイし、そこに複数コンテナー アプリケーションをデプロイしました。

AKS の詳細を参照し、デプロイの例の完全なコードを確認するには、Kubernetes クラスター チュートリアルに進んでください。

> [!div class="nextstepaction"]
> [AKS クラスターの管理](./tutorial-kubernetes-prepare-app.md)
