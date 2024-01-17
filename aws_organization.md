
斎尾署は一つのAWSアカウントですべてが完結している状態で、


AWSアカウントを複数持っているとアカウントの数だけ請求書が飛ぶ。

１００アカウントをあると１００の支払いになって、
一つのAWSアカウントに全リソースを集約したほうが訳すなるという

一つのAWSアカウントに集約していたら、使える割り引きが使えないという
現象が起きた。


AWS Organization作って、
AWS identityを有効にする。

そうすると管理画面のログインだけ許可されていて、何もできない画面になるので、

ここで権限を与えましょう。


でここのurlなんやねんということなんですが、
このグループにグループごとのurlがあるので
ログインサイトがここに当たると考えたらいいです。


Multi account permissionを使って

OrganizationでOrganizationでUserを作ってそこに

Organizationでiamで作成したユーザーを

AWSアカウントとIAMアカウントがごっちゃになる。

aws identity center password設定


identity centerで作成されたユーザー、グループに対して、
複数のAWS organizationのユーザーが存在する。
メールが複数ある。

identity centerが作成された。

このidentity centerで作成されたユーザーを使って代理としてログインするみたいな感じ。

Permisionごとのログインになる。

Billingとしてログインする

AWS Organization 

何かって言うと、組織ごと、部署ごとに
リソースを管理するためのもの、
これがないと、どの
にあるユーザー
がこの単位でお金がかかる。

AWSアカウントとメールアドレスはユニーク。

なので、いらなくなったらこれを削除したら良い。

でこれでOUを削除したら全部削除できるようになりました。

でこっからログインしてリソースを触ったらいいじゃないか？
と言う話なんですけど、これもまた問題があって、

二つの問題がある。

1. 常に同じ権限で触ることになる。変更権限があるユーザーでも、
常に変更権限持った状態でログインするから危ない。
たまたま画面見てて、戻ってきてカチッと押したら、リソース変更してしまいました。
とか普通にありそう。

2. 他の人に操作を助けてもらう時
リモートで他の人に操作助けてもらう場合は実際にログインするしかないわけだけど、
その場合はその人にAWSアカウントのパスワードを教えるため危険。

また、その人が勝手に変な操作をしたかどうかとかが分からない。
(実際は変なことしていないけど、そうでないかを証明する方法がない。)
他のプロジェクトのAWS詳しいエンジニアにちょっと見てもらうとか。


これを解決するために

AWSアカウントとは別にログイン用のユーザーを作成して、
ログイン時に決められたパーミッション(権限)ごとにポータルサイト(Console)を用意したら良い。

AWSOrganizationは個人のgmailとかでなくて、
一般人などのメールアドレスになる。


社内マネージャーグループとか、
社内エンジニアグループとか、


勉強用にAWSのサーバーを実際触れるサービスとかが
外国にあるんですけど、多分これを使っている。

新規でAWSアカウント作る場合はAWSさんはこういうふうにしてって
言ってるので、一人で使う場合もこのようにしましょう。

identity center内でメールアドレスはユニーク

aws organization内でメールアドレスはユニーク
個人のメールアドレス(gmail.comやyahoo.co.jp)みたいなものでなくて、
企業のドメインのメールアドレスを想定している。

identity center username -> ログインに使うので一生変えられない。
identity center　個人のメールアドレスでも良い想定の作り、

組織を抜けるのがクッソ面倒くさい。

remove じゃなくてcloseを使う。

パスワードがないのはログインして使わなくても良いから。

developerservice1@mywork.co.jp
みたいなメールアドレスを作っておいて、
そこのメールが来たら、

このグループに所属しているメールアドレスに
メールがみんな行くみたいになる。

AWS organizaion  一つに
一つのidentity centerしか作成できない。

一度USにidentity centerを作ってしまたら、
Osakaにidentity centerを作ることができない。
もし、作りたいならUSのidentity centerを削除する必要がある。

AWS Organizations supports IAM Identity Center in only one AWS Region at a time. To enable IAM Identity Center in this Region, you must first delete the current IAM Identity Center configuration in the US East (N. Virginia) Region. For more information, see IAM Identity Center Prerequisites .

aws user username いつでも変えれる
identity center 

viewonlyaccess はコンテンツが見えなくて、
外にバケット名が見えますよとか。
セキュリティ上問題にならないのが見える。

ReadonlyAcess実際のデータやソースコードやら見えちゃうから、
内部の人だけ。

identity center Description 日本語で書けない。

PoweruserAccessとありますが、
iam Userを作成できないので、Lambdaを作成できません！
lambda

System administratorとAdiministratorAccess
の違いはIAMを変更できるかどうか。

[Lambda FullAccess](https://us-east-1.console.aws.amazon.com/iamv2/home?region=ap-northeast-3#/policies/details/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAWSLambda_FullAccess?section=permissions)


どこからpermission確認するかと言うと、
Change Permissionをクリックして確認するしかない。


export AWS

aws configureしなくてもそれだけで見てくれるので、

aws ec2 describe-instances --region ap-northeast-3

で、ユーザーを削除した場合はどうなるか？

デプロイとか、ネットワークの管理とか、
ec2は難しいので、あまり使いたくないから、
app runner使うんですが、

これはgcpのcloud runと同じものと思っていただけたらと。

public.ecr.aws/docker/library/node:lts-bookworm

public.ecr.aws/docker/library/hello-world:nanoserver-1809

LambdaFullAccess
iamの権限もあるし、いける
と思いつつ、lambdaのサービスロールを作る権限がないというファッキンな作り。


```bash
aws lambda list-functions --region ap-northeast-3
```

PowerUserAccess-> 一般開発者ユーザー

Lambda作成できない。
S3Bucket作成できない。

->サービスの作成、停止はLinuxでいうルート権限並に重い権限だと思っている

作るのは

SystemAdminiStrator、AdministratorAccess権限でないとだめ。

AdministratorAccessは

例えば、会社の立ち上げメンバーのAさんにルート権限でAWSアカウント作成して、
プラットフォームを作りました。
となったら、それはAさんの持ち物になるんですよ。
だから外部の人やメンバーにやってもらうとしても、
会社のCEOのメールアドレスを使って、AWSアカウント作成して

AWS Organizaiotnで
サービス名@会社名.co.jp
みたいなサービスと一蓮托生のメールアドレスを作って、
そこにidentity centerで実際作業する人と有効なパーミッションを割り当てる。
という使い方になる。

セキュrリティの問題になりそうなパーミッションの変更はできない。
バージョニングなどはできる。

S3 ACL設定しないでください。

Administrator Accessでしか作成できない。
これと紐づいたRoleを使おうとするなら。

権限管理が分かると、そのアプリケーションが
何を想定して作られたかがわかる。

気をきかして何にかするとか難しい。

AWS Organizationsのサービス見ると、全て推奨されません。
なので、推奨されない。そもそも。

cost explorer

linked account にアカウントごとのかかった金額があるから、
これを見ると良い。
