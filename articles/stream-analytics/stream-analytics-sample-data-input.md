---
title: "Azure Stream Analytics でのクエリのテスト | Microsoft Docs"
description: "Stream Analytics ジョブのトラブルシューティングを行う際に問題を特定します。"
keywords: "入力のトラブルシューティング, 入力のサンプリング"
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: e2636b8b89b86bbb2a2991972386462535d5a10f
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2017
---
# <a name="azure-stream-analytics-query-testing-and-input-stream-sampling"></a>Azure Stream Analytics クエリのテストと入力ストリームのサンプリング

Azure Stream Analytics を使用すると、ファイルから取得したサンプル入力イベントをサンプリングし、ポータルでクエリをテストできます。このとき、ジョブを開始または停止する必要はありません。

## <a name="testing-your-query"></a>クエリのテスト

Stream Analytics ジョブの詳細ウィンドウで **[クエリ]** の下にあるクエリ名をクリックして、**[クエリ エディター]** ブレードを開きます  (このサンプル シナリオでは、クエリがまだ作成されていないため、プレースホルダー **< >** をクリックします)。

![Stream Analytics クエリ エディター](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

前のリリースと同様に、クエリを作成するためのリッチ エディター ブレードが表示されます。 このブレードは更新されており、新しいウィンドウが左側に追加されています。ここには、クエリで使用し、このジョブのために定義した入力と出力が表示されます。

![Stream Analytics クエリ エディターの入力と出力の一覧](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

定義されていない追加の入力と出力も表示されます。 これらは作業開始時に使用する新しいクエリ テンプレートがベースになっており、 クエリを編集すると変更されたり削除されたりします。 ここでは無視してかまいません。

サンプル入力データでテストするには、入力のいずれかを右クリックし、**[ファイルからサンプル データをアップロードする]** を選択します。

![Stream Analytics クエリ エディターの [ファイルからサンプル データをアップロードする] コマンド](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

アップロードが完了したら、**[テスト]** をクリックして、先ほど用意したサンプル データでこのクエリをテストします。

![Stream Analytics クエリ エディターの [テスト] ボタン](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

テストの出力を保存して後で使用する場合に備えて、ブラウザーに表示されるクエリの出力には、結果のダウンロードのためのリンクが存在します。 これで、クエリを簡単に何度も編集してテストを繰り返し行い、出力がどのように変わるかを確認することができます。

![Stream Analytics クエリ エディターのサンプル出力](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

上の図では、2 番目の出力 **HighAvgTempOutput** が追加されています。

クエリで複数の出力を使用すると、それぞれの出力結果を個別に確認でき、それらを簡単に切り替えることができます。

結果に問題がなければ、クエリを保存してジョブを開始します。あとは Stream Analytics の処理を待つだけです。

## <a name="get-help"></a>問い合わせ

さらにサポートが必要な場合は、 [Azure Stream Analytics フォーラム](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)を参照してください。

## <a name="next-steps"></a>次のステップ
* [Azure Stream Analytics の概要](stream-analytics-introduction.md)
* [Azure Stream Analytics の使用](stream-analytics-real-time-fraud-detection.md)
* [Azure Stream Analytics ジョブのスケーリング](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (Stream Analytics クエリ言語リファレンス)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics management REST API reference (Azure ストリーム分析の管理 REST API リファレンス)](https://msdn.microsoft.com/library/azure/dn835031.aspx)
