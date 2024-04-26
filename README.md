# Assignment-14
## 目的  
① tutorial-aws-deploy-simple-java-app-to-ec2の実施。  
② AWS上にEC2インスタンスとRDSインスタンスを構築し、EC2からRDSへ接続確認を行う。  

## 動作確認  
### ①tutorial-aws-deploy-simple-java-app-to-ec2の実施
1. AWSチュートリアル
AWSアカウント、IAMアカウントの作成
<img width="400" alt="IAM" src="https://github.com/pon02/Assignment-14/assets/140311845/ad844f89-07e1-4a9a-9b38-fd464b9bb1d8">

2. VPS作成  
<img width="400" alt="VPC" src="https://github.com/pon02/Assignment-14/assets/140311845/99e47e41-13aa-4a69-8eee-f8cff3e9ec78">

リソースマップ  
<img width="400" alt="VPCリソースマップ" src="https://github.com/pon02/Assignment-14/assets/140311845/5f1f54d6-14e5-4916-a8d9-c6f7cbfebd0a">

3. EC2インスタンス作成
<img width="400" alt="EC2インスタンス" src="https://github.com/pon02/Assignment-14/assets/140311845/807e24d2-2532-4ab6-976d-b4e517c8cc93">

VPCは先ほど作成したpon_tutorial-vcpを使用  
<img width="400" alt="スクリーンショット 2024-04-23 22 08 01" src="https://github.com/pon02/Assignment-14/assets/140311845/866583ae-dc27-4332-97f8-3da2a160c28a">

4. .sshフォルダにキーペアを移動

最初CUIにてダウンロードフォルダからキーペアの移動を行ったが、.sshフォルダを見ても移動してきておらず、最終的にドラッグ＆ドロップで移動した。

6. EC2に接続
<img width="400" alt="初回EC2接続" src="https://github.com/pon02/Assignment-14/assets/140311845/e16f104a-9000-4f43-aafe-35a63baf1b87">

7. EC2へGit・Javaのインストール、リポジトリのクローンを行い、Spring Bootを起動
<img width="400" alt="Spring Boot起動" src="https://github.com/pon02/Assignment-14/assets/140311845/3999ada6-c869-4136-8179-e33703ebccfd">

8. curlにてリクエストを送信しレスポンスが得られることを確認

レスポンスコード：200  
レスポンス    ：/hello →  {"message":"hello_world"}  
            /hello?name=yamada → {"message":"hello yamada"}  
<img width="400" alt="レスポンス確認" src="https://github.com/pon02/Assignment-14/assets/140311845/e331689d-b101-4432-8465-91fdc18d4f3a">  
※最初はEC2を立ち上げたターミナルからリクエストを送付しようとしたが、コマンド入力ができない状態で立ち往生。  
別のターミナルよりリクエストを送る旨ご指摘いただいて成功した。

### ②AWS上にEC2インスタンスとRDSインスタンスを構築し、EC2からRDSへ接続確認を行う。  
1. RDSを作成
<img width="400" alt="RDS00" src="https://github.com/pon02/Assignment-14/assets/140311845/7e944975-8e97-4721-ac68-ef224cd5306b">

※Security Groupが自分が設定したものと違うものが入っていて、見てみるとMySQL用の作成した記憶のないSecurity Groupができていたり、調べているうちにVPCにも作成していないサブネットができていることに気づき、

<img width="400" alt="VPCリソースマップ 2" src="https://github.com/pon02/Assignment-14/assets/140311845/227bbd97-72bd-4e94-a45b-a12d8ef3d873">

どこで作成してしまったのかを調べたら、RDSインスタンス作成のこの選択を「EC2コンピューティングリソースに接続」にしていたためだった。

<img width="400" alt="スクリーンショット 2024-04-26 17 38 16" src="https://github.com/pon02/Assignment-14/assets/140311845/e36b874f-96d8-406f-a085-36375a508dbc">

今回はVPCで作成したprivateサブネットにRDSを置きたいと思っていたので、整理するためにももう一度VPCの作成から行った。

2. VPC再作成
<img width="400" alt="VPC2" src="https://github.com/pon02/Assignment-14/assets/140311845/60b0fc95-7fee-4f23-9b9f-b3c013deebe3">

リソースマップ  
<img width="400" alt="VPC2リソースマップ" src="https://github.com/pon02/Assignment-14/assets/140311845/1eb67e24-a7a7-4a8e-a6ae-e0d1772680c3">  
※最初はルートテーブルが３つだったが、どこかで作成した記憶のないルートテーブルがメインで作成されていた。削除できず、ルートテーブルについても把握に時間がかかりそうなので今後のタスクとする。  

3. EC2再作成
<img width="400" alt="EC2-2" src="https://github.com/pon02/Assignment-14/assets/140311845/726f2136-89fe-49d6-b929-8ec5bdbbb08f">

EC2のSecurity Groupは①のチュートリアルと同じもので作成した。

<img width="400" alt="EC2セキュリティ" src="https://github.com/pon02/Assignment-14/assets/140311845/f6b2294a-035f-4c7f-b617-6e07a530566b">

4. RDS再作成
<img width="400" alt="RDS2" src="https://github.com/pon02/Assignment-14/assets/140311845/8ed89ad4-7b01-43c0-b1df-e77481cc9c7c">



* VPCはEC2と同じものを使用  
* VPCのprivateサブネット２つでサブネットグループを作成、RDSをEC2と同じAZのprivateサブネットに設置  
* RDSがEC2とのみ通信を行うための新しいSecurity Groupを作成  
<img width="400" alt="RDS詳細" src="https://github.com/pon02/Assignment-14/assets/140311845/f5e65025-6ea3-4f19-ba32-59272dd6f760">

RDS用のSecurity GroupのソースにEC2で使用したSecurity GroupのIDを使用  
<img width="400" alt="RDSセキュリティ" src="https://github.com/pon02/Assignment-14/assets/140311845/a428eea5-9e59-4c34-9173-c398bbf97251">  
5. EC2を起動  
<img width="400" alt="EC2接続2" src="https://github.com/pon02/Assignment-14/assets/140311845/f76ccc53-e04c-4fd7-ab5c-e20df097ac0b">  

6. MySQLをインストール

MySQLがうまくインストールできなかったので、調べるとAmazon Linux 2023に対応したGPGキーがないためだったので、以下のコマンドでダウンロード

```sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023```  

無事MySQLをインストールできた。  

<img width="400" alt="mysqlダウンロード" src="https://github.com/pon02/Assignment-14/assets/140311845/337570df-12e2-4e9f-ae14-4ca985357ee1">

7. RDS接続確認

mysqlコマンドでデータベースにアクセス成功  

<img width="400" alt="RDS接続確認" src="https://github.com/pon02/Assignment-14/assets/140311845/476d170d-471e-4470-a44c-92521dbde7d9">

8. curlコマンドでRDS接続確認

EC2接続中のターミナルから下記のcurlコマンドで接続確認ができるという記事を見たので実施。  

```curl -v telnet://[RDSインスタンスのエンドポイント]:[ポート番号]```  

Amazon Linux2023に標準装備されているcurlのバージョンがminimalだったので、fullバージョンをインストールし、  

```sudo dnf install --allowerasing curl-full libcurl-full```  

コマンド実行して接続確認ができた。  

<img width="400" alt="RDS接続確認2" src="https://github.com/pon02/Assignment-14/assets/140311845/8418effd-a652-46d8-9d20-d8efab6ecbc6">

```curl -v http://[RDSインスタンスのエンドポイント]:[ポート番号]```  
<img width="400" alt="RDS接続確認3" src="https://github.com/pon02/Assignment-14/assets/140311845/e7af2768-4d7c-4882-9cdd-ed665c741e55">  
こちらでも接続できたが、接続確認の記事ではtelnetを使用したものばかりだった。  
接続確認でプロトコルがtelnetだと繋げることが目的で、httpでは繋げてGETで情報を取得しようとしたが、取得する内容がないため繋がったことだけが確認できたと解釈した。  

## 所感  
何度かやり直すうち、少しずつAWSの単語や設定が理解できてきたとともに、EC2のターミナル操作を調べるための記事についても理解ができるようになっていたので時間がかかったが少しは手応えを感じることができた。  
AWSの設定については作業中に把握しきるのは到底無理であったので、AWSコースの受講等勉強を進めていく必要性を感じた。  
今回の内容で、特に追求できずに終わったことは  
・ キーペアの移動ができなかった理由  
・ ルートテーブルの理解  
の2点で、今後のタスクとする。
