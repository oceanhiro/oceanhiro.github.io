---
layout: post
title:  "NetBox入門"
date:   2024-09-04 23:25:00 +0900
categories: jekyll update
---

# NetBox入門

NetBoxを勉強したので備忘録をメモする  
IaCを行う際のAnsibleやTerraformから参照する「Network Source of Truth」として活用したい  

## 学習の流れ

---

[NetBox - Zero To Hero Course](https://github.com/netbox-community/netbox-zero-to-hero)に取り組んだ。  
一通りNetBoxの機能を触ることのできるチュートリアルになっていて、全12セクションに分かれており、1セクションあたり1時間もあれば完了できる。  
DockerでNetBoxをローカルで動かしながら実際にAnsibleやPostman(API),Pythonスクリプトを用いてNetBoxを操作できる。  
最新バージョンだとドキュメント通りにやっても動かない箇所があるので、明示されているNetBoxのバージョンに合わせてやったほうがよい。  
最新版でやってもコードを見ながらカラム名を修正したりすれば動く(※後述する)  。  

[NetBox Zero To Hero - Video 1-1 - Introduction to the NetBox Web Interface](https://www.youtube.com/watch?v=zT82jOUCcW4&list=PL7sEPiUbBLo_iTds-NV-9Tu05Gg2Aj8N7&index=1)
NetBox - Zero To Hero Courseの動画解説。  
これを見ながら実際に操作をまねる形になる。  

## NetBox - Zero To Hero Courseのつまずきポイント

---

チュートリアルを行うにあたり手順通りにやってもエラーになる箇所がある  
以下につまずきポイントとしてメモしておく  

1 デバイスインポート時にエラーになる  

```
原因 : カラム名がバージョンアップに伴い変更されている  
対応 : 「device_role」カラムを「role」に変更するとインポートできる  
ref  : https://github.com/netbox-community/netbox-zero-to-hero/blob/main/modules/8-what-about-virtualization/csv_data/devices.csv  
```

2 デバイスタイプ「ProLiant DL380 Gen9」のリンクが切れてる

```
原因 : ファイル名が変更された模様。 `_` -> `-`  
対応 :   
  旧 : https://github.com/netbox-community/devicetype-library/blob/master/device-types/HPE/ProLiant_DL380_Gen9.yaml  
  新 : https://github.com/netbox-community/devicetype-library/blob/master/device-types/HPE/ProLiant-DL380-Gen9.yaml  
ref  : https://github.com/netbox-community/netbox-zero-to-hero/blob/main/modules/8-what-about-virtualization/8-video-script.md  
```

3 電源ケーブルインポート時にエラーになる

```
原因 : 「ProLiant DL380 Gen9」のPSUがテンプレートインポート時に登録されていない
対応 : 手動で「ProLiant DL380 Gen9」から作成されているデバイス「AUBRI01-VSP-1」と「AUBRI01-VSP-2」の「Power Ports」に「PSU1」と「PSU2」を追加する
ref  : https://github.com/netbox-community/netbox-zero-to-hero/blob/main/modules/9-powering-up/csv_data/cables.csv
```

4 カスタムスクリプト「NewBranchScript」の実行に失敗する

```
原因 : カラム名がバージョンアップに伴い変更されている
対応 :スクリプト内の 「device_role」カラムを「role」に変更すると実行できる
ref  : https://github.com/netbox-community/netbox-zero-to-hero/blob/main/modules/11-custom-scripts/11-custom-scripts.md
```

---

それではよいNetBoxライフを。
