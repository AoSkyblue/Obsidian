---
title: "AWS Hands-on for Beginners Network編#1 AWS上にセキュアなプライベートネットワーク空間を作成する | AWS Webinar"
source: "https://pages.awscloud.com/JAPAN-event-OE-Hands-on-for-Beginners-Network1-2022-confirmation_945.html"
author:
  - "[[Amazon Web Services]]"
  - "[[Inc.]]"
published:
created: 2025-08-06
description: "“AWS Hands-on for Beginners - Network編#1 AWS上にセキュアなプライベートネットワーク空間を作成する” では、AWS上でネットワークを構成する基本のサービスであるAmazon VPCを中心に講義を進めていきます。 Amazon VPCの基本を学んでいただき、VPC内の通信とVPCからVPC外に接続する方法について学べる内容になっています。動作確認も多く実施するハンズオンになっており、各設定の意味を深く理解できるよう工夫した構成になっています。  ※本ウェビナーは2022年にレコーディングされたものです。"
tags:
  - "clippings"
image: "https://a0.awsstatic.com/main/images/logos/aws_logo_smile_1200x630.png"
PermanentNote:
---
以下よりハンズオンを確認いただけます  

まずはじめに、本コースのゴール・前提知識とアジェンダについてご紹介します。  
また、本コースのハンズオンで中心となるAmazon VPCの概要を説明し、ハンズオンの最終構成図について紹介します。

  

![](https://www.youtube.com/watch?v=5m7WCkw6sj0)

  

Amazon VPCのハンズオンを行うにあたり、構成するサブネットやインターネットゲートウェイなどの機能の説明と共に、ハンズオンの流れについて説明します。

  

![](https://www.youtube.com/watch?v=VKr8Csx1RHs)

  

  

AWSアカウントにログインいただき、いよいよハンズオンを行います。  
AWSアカウントを準備するにあたり、「 [AWS アカウントの作り方 & IAM基本のキ](https://pages.awscloud.com/event_JAPAN_Ondemand_Hands-on-for-Beginners-1st-Step_LP.html) 」を先に視聴しておくことをお勧めします。

  

![](https://www.youtube.com/watch?v=P1mkeqnKSMc)

  

  

Amazon VPC内に構成するパブリックサブネットとプライベートサブネットの定義を理解し、ルートテーブルを用いてAmazon VPC内のルーティングの設定方法について学びます。

  

![](https://www.youtube.com/watch?v=c1sRdUshJGo)

  

  

Amazon VPCにて作成したパブリックサブネットにWebサーバを構築し、これまでに作成した環境の動作テストを行います。  
(Webサーバ作成時に紹介しているインストールスクリプトはこちらからコピーしてください)

\*\*\*\*\*インストールスクリプトはこちら\*\*\*\*\*  
#!/bin/bash

yum -y update  
yum -y install httpd  
systemctl enable httpd.service  
systemctl start httpd.service  
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  

![](https://www.youtube.com/watch?v=QlJDto9Z3fM)

  

  

プライベートサブネットからインターネットに接続する要件例をご紹介し、要件をクリアする方法について説明し、ハンズオンにて設定していただきます。

  

![](https://www.youtube.com/watch?v=tuORDOfV1YU)

  

  

Amazon VPC内に設置しているサーバからAmazon VPC外のAWSサービスに対して接続する要件例をご紹介し、要件をクリアする方法について説明します。

  

![](https://www.youtube.com/watch?v=Fo0Iy7HR1Es)

  

  

VPC Endpoint(interface型)を実装し、プライベートサブネットに設置したサーバからAWS Systems Managerに接続するハンズオンを行います。

  

![](https://www.youtube.com/watch?v=vxc-rXx4dWM)

  

  

VPC Endpoint(Gateway型)を実装し、プライベートサブネットに設置したサーバからAmazon S3に接続するハンズオンを行います。

  

![](https://www.youtube.com/watch?v=Ne5lq50pReM)

  

  

本コースで学んでいただいたことを振り返り、まとめについて説明します。

  

![](https://www.youtube.com/watch?v=4W1lhTcTijo)

  

  

本コースのハンズオンで作成したAWSリソースを全て削除します。 作成した環境を完全に削除されたい方は、動画の手順にしたがって削除を行ってください。

  

![](https://www.youtube.com/watch?v=yQYPz7LHq0M)

  

  

  

  

  

  

直近の AWS 主催イベント  

  

AWS イベントスケジュール：直近の AWS 主催イベントは、 [こちら](https://aws.amazon.com/jp/events/) よりアクセスいただけます。