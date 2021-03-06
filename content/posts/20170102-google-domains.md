---
title: "Google Domainsが個人利用できないという話"
date: 2018-01-02T12:43:40+09:00
draft: false
---

# Google Domainsを使ってみようと思った
最近日本でも利用可能になった[Google Domains](https://domains.google/)に所有しているgTLDドメインを移行しようと思いました。
.com, .net, .orgのようなものは当然対応していますが、まだまだvalue-domainsなどに比べると見劣りするのも事実で、特に.jpは未対応なのは痛い。
使うメリットとしては（おそらく）その辺のドメイン登録業者が提供する権威DNSサーバと比べると可用性も高いはずだし、業者によっては有料オプションになるドメイン所有者の代理公開サービスが標準で提供されるところ、人によってはDNSSEC対応なのもメリットといえるかもしれません。

# 移管の申請自体はできるものの…
ドキュメントを見ても見つけられなかったのですが、移管作業の最後に連絡先を入力して、利用規約に同意する場所で
> Google Domainsのサービスは、現在お住まいの国で営利目的または商用で使用する場合にのみご利用いただけます。
という記述があるので、個人が趣味で使う分には使えないんですね…
{{< figure src="/media/2018-01-03/google-domains.png" title="連絡先設定時の警告">}}

結局Google Domainsはあきらめてvalue-domains+CloudFlareのDNSサービスを引き続き使うことにしました。

# 追記
このサイトはgithubへのpushをトリガーにして、[Netlify](https://www.netlify.com/)でhugoビルドされ、自動的にアップロードされるようになっています。Netlify自体がCDN機能を持つので、このサイトではCloudflareを使わず、権威DNSサービスを含めNetlifyで運用しています。