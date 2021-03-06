---
title: "Azure Cloud Shell の概要 | Microsoft Docs"
description: "Azure Cloud Shell について概説します。"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: juluk
ms.openlocfilehash: 05c4d419f6d7b42a14b9bb13570daaa666d52db3
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/29/2017
---
# <a name="overview-of-azure-cloud-shell"></a>Azure Cloud Shell の概要
Azure Cloud Shell は、Azure リソースを管理するための、ブラウザーでアクセスできるインタラクティブなシェルです。
Azure Cloud Shell には、業務に最適のシェル操作を選択できる柔軟性があります。
Linux ユーザーは Bash を、Windows ユーザーは PowerShell を選ぶことができます。

Azure Portal を使用して Cloud Shell アイコンから起動します。

![ポータルの起動](media/overview/portal-launch-icon.png)

シェル セレクターのドロップダウンから Bash または PowerShell を利用します。

![Cloud Shell の Bash](media/overview/overview-bash-pic.png)

![Cloud Shell (プレビュー) の PowerShell](media/overview/overview-ps-pic.png)

## <a name="features"></a>Features (機能)
### <a name="browser-based-shell-experience"></a>ブラウザーベースのシェル環境
Cloud Shell は、Azure の管理タスクを視野に入れて開発されたブラウザーベースのコマンド ライン環境です。
Cloud Shell を利用することで、ローカル コンピューターに拘束されることなくクラウドならではの作業が可能となります。

### <a name="choice-of-preferred-shell-experience"></a>最適なシェル エクスペリエンスの選択
Azure Cloud Shell には、業務に最適のシェル操作を選択できる柔軟性があります。
Linux ユーザーは Cloud Shell の Bash を選択できるのに対して、Windows ユーザーは Cloud Shell (プレビュー) の PowerShell を選択できます。

### <a name="authenticated-and-configured-azure-workstation"></a>認証済みかつ構成済みの Azure ワークステーション
Cloud Shell は、作業のスピードを高めるために、広く使われているコマンド ライン ツールや言語サポートと共にプレインストールされるので、Microsoft で管理されます。 さらに、Azure CLI 2.0 または Azure PowerShell コマンドレットですばやくリソースにアクセスできるよう、Cloud Shell では安全に自動認証が行われます。

すべてのツールの一覧については、[Bash エクスペリエンス](features.md#tools)および [PowerShell (プレビュー) エクスペリエンス](features-powershell.md#tools)をご覧ください。

### <a name="multiple-access-points"></a>複数のアクセス ポイント
Azure Portal から使用可能な Cloud Shell に加えて、以下からもアクセスできます。
* [Azure CLI 2.0 の "お試しください" のドキュメント](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)
* [Azure Mobile App](https://azure.microsoft.com/features/azure-portal/mobile-app/)
* [Visual Studio Code 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)

### <a name="connect-your-microsoft-azure-files-storage"></a>Microsoft Azure Files ストレージの接続
Cloud Shell マシンは一時的なものであるため、お使いの $Home ディレクトリを永続化するためには、Azure Files 共有を `clouddrive` としてマウントする必要があります。
Cloud Shell の初回起動時に、リソース グループ、ストレージ アカウント、Azure ファイル共有を作成するように求められます。 これは 1 回限りの作業であり、それ以降はすべてのセッションで自動的に接続されます。 Cloud Shell の Bash と PowerShell (プレビュー) を使用して、単一の Azure ファイル共有をマッピングし、使用できます。

#### <a name="create-new-storage"></a>新しいストレージの作成
![](media/overview/basic-storage.png)

ローカル冗長ストレージ (LRS) アカウントと Azure Files 共有が自動的に作成されます。 Bash 環境と PowerShell 環境の両方を選ぶと、Azure Files 共有は両方の環境で使われます。 ストレージのコストは通常どおりに適用されます。

以下の 3 つのリソースが自動的に作成されます。
1. `cloud-shell-storage-<region>` という名前のリソース グループ
2. `cs<uniqueGuid>` という名前のストレージ アカウント
3. `cs-<user>-<domain>-com-<uniqueGuid>` という名前のファイル共有

> [!Note]
> Cloud Shell の Bash は、`$Home` を永続化するための既定の 5 GB ディスク イメージも作成します。 SSH キーなどの $Home ディレクトリ内のすべてのファイルが、マウントされた Azure ファイル共有に格納されたユーザー ディスク イメージ内に保持されます。 $Home ディレクトリとマウントされた Azure ファイル共有へのファイルの保存時に、ベスト プラクティスを適用します。

#### <a name="use-existing-resources"></a>既存のリソースの使用
![](media/overview/advanced-storage.png)

既にあるリソースを Cloud Shell に関連付けるため、詳細オプションが用意されています。
ストレージのセットアップを促す画面が表示されたら、[詳細設定の表示] をクリックして、追加オプションを表示します。
ドロップダウン ボックスには、割り当てられている Cloud Shell リージョンとローカル/グローバル冗長ストレージ アカウントが一覧表示されます。

[Cloud Shell ストレージ、Azure ファイル共有の更新、ファイルのアップロード/ダウンロードについては、こちらをご覧ください。](persisting-shell-storage.md)

## <a name="concepts"></a>概念
* Cloud Shell は、ユーザーごとにセッション単位で一時的に提供されるホスト上で実行されます。
* Cloud Shell は、無操作状態で 20 分経過するとタイムアウトとなります。
* Cloud Shell では、Azure ファイル共有がマウントされている必要があります
* Cloud Shell では、Bash と PowerShell に対して同じ Azure ファイル共有が使用されます
* Cloud Shell には、ユーザー アカウントごとに 1 台のマシンが割り当てられます。
* Bash では、標準の Linux ユーザーとしてアクセス許可が設定されます。

[Cloud Shell の Bash](features.md) と [Cloud Shell (プレビュー) の PowerShell](features-powershell.md) の機能に関する詳細をご覧ください。

## <a name="examples"></a>例
* スクリプトを使って Azure の管理タスクを自動化します
* Azure Portal と Azure コマンドライン ツールを使って同時にリソースを管理します
* Azure CLI 2.0 または Azure PowerShell コマンドレットを試用します

[Cloud Shell の Bash](quickstart.md) および [Cloud Shell (プレビュー) の PowerShell](quickstart-powershell.md) のクイックスタートで、これらの例を試してください。

## <a name="pricing"></a>価格
Cloud Shell のホストとなるマシンは無料です。ただし、前提条件として Azure Files 共有をマウントする必要があります。 ストレージのコストは通常どおりに適用されます。

## <a name="next-steps"></a>次のステップ
[Cloud Shell の Bash のクイックスタート](quickstart.md) <br>
[Cloud Shell での PowerShell (プレビュー) のクイックスタート](quickstart-powershell.md)