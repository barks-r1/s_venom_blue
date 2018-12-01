---
title: "20181202 Sustained Use Discount"
date: 2018-11-29T23:47:04+09:00
draft: false
---
# 知っておきたいSustained Use Discounts

Sustained Use Discountは自動的に適用されるため、深いことを考えなくても勝手に値段が下がってくれます。
とはいうものの、仕組みを知っておくともっと有効に割引を活用することができるようになるかもしれません。

Sustained Use Discountsは以下のルールに従って適用されます。

1. 下記のインスタンスのカテゴリーごとにvCPU, Memoryの合計値で計算される
    - Predefined machine types(standard, highcpu, highmem, sole tenant nodes)
    - Memory optimized instances(megamem, ultramem)
    - Custom machine types
    - Shared core machine types(f1-micro, g1-small)
    - GPU
2. 下記のリソースは対象外
    - Preemptible instance
    - Commited Use Discountsが適用されたvCPU, Memory

# 具体的な計算方法

まず、カテゴリーごとに同時に起動しているvCPU, Memoryの合計量を計算します。仮に月の前半にn1-standard-4を1台、月の後半にn1-standard-8を1台起動していたとしましょう。

この場合、月の前半は4vCPU, 15GiB Memory、月の後半は8vCPU, 30GiB Memory起動していたとみなされます。つまりn1-standard-4を1ヶ月、n1-standard-4(n1-standard-8とn1-standard-4の差分)を0.5ヶ月起動していたときと同様の扱いになります。

Sustained Use Discountsは合計値で計算されるため、vCPUは"8vCPU×1ヶ月"+"8vCPU×0.5ヶ月"利用されており、Memoryは"30GiB×1ヶ月"+"90GiB×0.5ヶ月"利用されていたという計算になります。なおこの算出方法は2018年10月から始まった新しい計算方法です。9月まではn1-standard-4, n1-standard-8が別個に計算されていたので、まだ気がついていない人もいるのではないでしょうか。

具体的な割引率は月ごとの利用期間によって以下の通りとなります。

|月あたりの利用時間|割引率|
|---|---|
|0%〜25%|0%|
|25%〜50%|20%|
|50%〜75%|40%|
|75%〜100%|60%|

たまに勘違いしている人がいますが、最大の60%引きは1ヶ月の75%以上の時間利用した場合に、75%以上の期間について適用される割引です。細かい計算は[公式ドキュメント](https://cloud.google.com/compute/docs/sustained-use-discounts)を読んでいただくとして、0.5ヶ月の起動で10%引き、1ヶ月の起動で30%引きという計算になります。

今回のケースではn1-standard-4が30%引き、差分n1-standard-4が10%引きになります。東京リージョンでの月額費用を計算すると、n1-standard-4が$124.68、差分n1-standard-4が$80.15、合計$204.84になります。これを旧来の計算方法で計算すると$240.46になるので、およそ15%の値下がりです。

# Sustained Use Discountについての誤解

- インスタンス台数を増減させるときには、Sustained Use Discountの割引を最大活用するために、月初に起動していたインスタンスは止めずに月の途中で起動したインスタンスを止める。

Sustained Use DiscountはvCPU, Memory(以前はインスタンス)が1ヶ月のうちどれだけ起動しているか、というルールで計算されるため、月の頭から起動しても月の途中から起動しても平等に計算されます。

- 割引には事前のコミットメントが必要

それは[Committed Use Discount](https://cloud.google.com/compute/docs/instances/signing-up-committed-use-discounts)です。

- Sutained Use Discountは課金アカウントに紐付いた全プロジェクトにまたがって計算される

残念ながら、現状プロジェクト単位で計算されます。AWSのRIはプロジェクトにまたがって計算してくれるのですが、GCEの場合Sustained Use DiscountでもCommited Use Discountでもプロジェクト単位を横断した割引はしてくれません。要望を出し続ければ実装してくれるかも？
