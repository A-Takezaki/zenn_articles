---
title: "DIVER OSINT CTF 2025 Writeup"
emoji: "🦎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ctf,osint]
published: true
---
## はじめに
2025/06/07-2025/06/08 に開催された DIVER OSINT CTF 2025 に、チームメイトと計 3 人で出場していた。    
競技中に残していた 3 人分のメモから Writeup を起こしたので残しておく。 

結果は 66 位/668 組だった。 
去年より下がってしまったので悔しい。

## introduction

### document (100pt / 280 solves)
アメリカ海軍横須賀基地司令部（CFAY）は、米軍の関係者向けに羽田空港・成田空港と基地の間でシャトルバスを運行している。2023 年に乗り場案内の書類を作成した人物の名前を答えよ。  

`cfay 2023 hnd nrt`などで検索して、[スケジュールを記載したPDF](https://cnrj.cnic.navy.mil/Portals/80/CFA_Yokosuka/Documents/Airport%20Shuttle/2023%2008%2014%20CFAY%20Airport%20Bus%20Schedule.pdf?ver=-K3r8A3eonOJl48Tkf9SnA%3D%3D)を発見。  
PDF ファイルの EXIF 情報を見たら Author 情報が残っていた。
```bash
ExifTool Version Number         : 12.40
File Name                       : 2023 08 14 CFAY Airport Bus Schedule.pdf
Directory                       : .
File Size                       : 1237 KiB
File Modification Date/Time     : 2025:06:07 12:07:42+09:00
File Access Date/Time           : 2025:06:08 13:25:55+09:00
File Inode Change Date/Time     : 2025:06:07 12:08:35+09:00
File Permissions                : -rwxrwxrwx
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.7
Linearized                      : No
Author                          : [REDACTED]
Create Date                     : 2023:08:14 07:57:00+09:00
Modify Date                     : 2023:08:14 07:57:00+09:00
Producer                        : Microsoft: Print To PDF
Title                           : C:\Users\[REDACTED]\AppData\Local\Temp\1\msoF678.tmp
Page Count                      : 1
```

### flight_from (100pt / 362 solves)
画像 / Image:
https://drive.google.com/file/d/14LSICiwmUqG9xqAfAm75IVdGd7P12uoy/view?usp=sharing

このヘリコプターが出発した飛行場の ICAO コード（4 レターコード）で答えよ。  


立川基地の番号入れたら正解した。

### louvre (176pt / 181 solves)
ルーブル美術館の公共 Wi-Fi アクセスポイントのうち、以下の条件を満たすもののベンダーを答えよ。

- 情報は 2025 年 2 月 28 日に収集されており、オンライン上で確認できる。
- ベンダーは BSSID に準拠して判定せよ。
[openwifimap](https://openwifimap.net/#map?bbox=48.853646831055556,2.3061203956604,48.87044540697354,2.3610520362854004)のサイトから探したけど、Map 上に Hit する spot なし

[wifimap](https://www.wifimap.io/map/1109-paris?wifiHotspot=da2d9651-d7df-41a1-a935-ea69fc12e0f0)で WiFI スポットは見れたけど、BSSID の取得ができず

[wigle](https://wigle.net/index)で BSSID の取得ができそうだったが、アクセスポイントが見つからない。  
もしかしたらアカウントを作ったら見れる情報が増えるかも、と思ってアカウントを作成したが、too many request と表示されて詳細な情報が出てこなかった。　 
ここで中断してチームメイトにパスしたところ、「AI くんに聞いたら答えを教えてくれたよ」とのこと。なんか悔しい。   

### night_accident (100pt / 269 solves)  
動画 / Video:
https://www.youtube.com/watch?v=jHgqCpJNL28

この動画で、車とバスが衝突しそうになった場所はどこか。  

バスに 52 と 58 の番号が確認できるのでこの 2 つの路線図を調べ、被っていそうな Bishan 駅周辺でストリートビューで確認。  
道路の黄色×印や周辺建物の感じを目安に探して発見。

### p2t (377pt / 112 solves)
この写真の左側に掲示されている写真には、ある動物が写っている。その名前を、日本語で書かれている通りに答えよ。
![p2t](/images/diver-osint-ctf-2025-writeup/p2t.jpg)  

右に見切れている文字から、立山という地名を発見。富山県にあるらしい。  
地域柄、バードウォッチングが盛んそうなのと、写真背景の雪景色から場所は立山だろうと推測。  
写真が古く綺麗な展示ではないことから、古い建物内の展示か有志の展示であると推測。  
実はこの段階で答えがある`立山自然保護センター`にはたどり着いていたのだが、中の展示が問題のそれより綺麗だったのでスルー。  
[こんなもの](https://www.tsm.toyama.toyama.jp/_ex/public/kenkyu/32.pdf)を見つけて、野鳥の会の研究報告を博物館に展示しているパターンや、登山ルート中の山小屋やホテルの一角に貼られているパターンを想定して長い事探索していた。  
結局、チームメイトが[このサイト](https://yachou.info/murodoudaira-raichou/)を見つけてくれてゴール。  

**備考**
これは別のチームメイトが問題に詰まった息抜きに作った AA。  
![p2t](/images/diver-osint-ctf-2025-writeup/aa.png)



### ship (100pt / 363 solves)
これは、ある組織が運用する船舶である。もし将来、この船が外国に売却されたとしても、変わらない番号を答えよ。
Flag 形式: Diver25{現地語での船名_番号}（例: 船名が「ペンギン饅頭号」で番号が 1234567 の場合、Flag は Diver25{ペンギン饅頭号_1234567} となる）  

TKT-1401 でググると船名が出る
船名の IMO 番号を検索してヒット

## geo

### advertisement (100pt / 233 solves)
記事 / Article:
https://web.archive.org/web/20250108154113/https://www.noticiasaominuto.com/mundo/2699746/kyiv-diz-que-russia-usou-como-recrutas-ate-180000-presidiarios

この記事の写真が撮影された場所はどこか。  

ロシアのサンクトペテルブルグにシゾフ通りがあり、そこにトーキョーステーションなるモールがある。
レンズで探すと馬の銅像の写真が出てきたので馬の銅像で再度レンズすると駅がヒット。

### hole (449pt / 72 solves)
この穴があった場所はどこか。
![hole](/images/diver-osint-ctf-2025-writeup/hole.png)
明らかに大きな滑走路っぽい道路と大穴。 右下の文章から、中国圏の飛行場やドローン実験場と推測。  
ドローン関係の映像を漁っていたが見つからず。断念。
### night_street (428pt / 86 solves)
画像の中心に写っている茶色の 2 階建ての建物に入る施設の正式名称を現地語表記で答えなさい。
Flag 形式: Diver25{施設名}（例: Diver25{お台場海浜公園前郵便局}）
![night](/images/diver-osint-ctf-2025-writeup/night_street.jpg)

リンガーハットが左にあるので、[ここ](https://shop.ringerhut.jp/all/?page=2)リンガーハットの場所調べて google maps でローラーを実施。  
イオンとかフードコートは無視で探すも数多すぎて無理。  
店舗検索の絞り込みで駐車場+ドライブスルーで全部見たけどなし。  
道路が片側 2 車線以上でコンクリ舗装のためトラックの交通が多い道、港湾道路とかかな～と思いつつ愛知県の店舗でローラーして発見

### convenience (462pt / 63 solves)
青森県内に、公園とコンビニ、スーパーマーケットが互いに約 100m 圏内に存在する場所がいくつかある。また、これは OpenStreetMap で確認可能である。
この条件を満たす 公園 のうち、最南端 のものについて、OpenStreetMap 上での Way Number を答えよ。
なお、「公園」の定義は、OpenStreetMap 上で "park" (leisure=park) と分類されているものに準拠する。

まずは Overpass API を使って、青森県のコンビニ、スーパー、公園を列挙。  
```
[out:json][timeout:25];
// 青森県の行政境界を検索
relation
  ["boundary"="administrative"]
  ["name"="青森県"]
  ["admin_level"="4"];
out ids;
```
```
[out:json][timeout:60];
// 1) 青森県エリアを指定
area(3600000 + 123456)->.aomori;

// 2) leisure=park の way（公園のポリゴン）を取得
way["leisure"="park"](area.aomori)->.parks;

// 3) 公園ポリゴンの周囲100m内に shop=convenience の node を検索
node["shop"="convenience"](around.parks:100)->.convs;

// 4) 同じく shop=supermarket の node を検索
node["shop"="supermarket"](around.parks:100)->.sups;

// 5) 上記両方の近接条件を満たす公園だけを抽出
//    parks が convs と sups の両方を持つもの
(.parks;  // all parks
  node.if((t["leisure"]=="park") && is_in(.convs) && is_in(.sups))
)->.matched_parks;

// 6) matched_parks を出力（ID と重心座標付き）
matched_parks
  out center tags;
```  

これで得られたデータを元に python のスクリプトを書いて、条件に当てはまるものを抽出、、したかったのだが、  
動かしてみた結果出力されたデータは incorrect。なんかどっかでロジックを間違えてそう。  
大会終了間際はこのスクリプトとにらめっこしてミスっているところを探していた。結局間に合わず。
```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import json
import math
import sys

def haversine(lat1, lon1, lat2, lon2):
    R = 6371000  # Earth radius in meters
    phi1, phi2 = math.radians(lat1), math.radians(lat2)
    dphi = math.radians(lat2 - lat1)
    dlambda = math.radians(lon2 - lon1)

    a = (
        math.sin(dphi / 2) ** 2
        + math.cos(phi1) * math.cos(phi2) * math.sin(dlambda / 2) ** 2
    )
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1 - a))
    return R * c


def parse_geojson(path):
    with open(path, "r", encoding="utf-8") as f:
        data = json.load(f)

    parks = []
    convs = []
    sups = []

    for feat in data.get("features", []):
        props = feat.get("properties", {})
        # Overpass Turbo GeoJSON stores the center point as geometry.coordinates
        geom = feat.get("geometry", {})
        coords = geom.get("coordinates", [None, None])
        lon, lat = coords[0], coords[1]

        # Extract the OSM ID and type ("way/xxxxx" or "relation/xxxxx" or "node/xxxxx")
        id_full = props.get("@id") or feat.get("id", "")
        if "/" in id_full:
            typ, id_str = id_full.split("/", 1)
        else:
            typ, id_str = (None, None)

        # Collect parks (both ways and multipolygon relations with leisure=park)
        if typ in ("way", "relation") and props.get("leisure") == "park":
            try:
                park_id = int(id_str)
            except (TypeError, ValueError):
                continue
            parks.append((park_id, lat, lon))
            print(f"Found park: {park_id} at ({lat}, {lon})")

        # Collect convenience stores
        elif typ == "node" and props.get("shop") == "convenience":
            convs.append((lat, lon))

        # Collect supermarkets
        elif typ == "node" and props.get("shop") == "supermarket":
            sups.append((lat, lon))
        
        print(convs)
    return parks, convs, sups


def main():
    geojson_path = "export.geojson"

    try:
        parks, convs, sups = parse_geojson(geojson_path)
    except FileNotFoundError:
        print(f"Error: ファイルが見つかりません: {geojson_path}", file=sys.stderr)
        sys.exit(1)

    valid_parks = []
    convs_sups = []

    for park_id, plat, plon in parks:
        # 公園から100m以内のコンビニ・スーパーをそれぞれ抽出
        nearby_convs = [
            (clat, clon)
            for clat, clon in convs
            if haversine(plat, plon, clat, clon) <= 100
        ]
        nearby_sups = [
            (slat, slon)
            for slat, slon in sups
            if haversine(plat, plon, slat, slon) <= 100
        ]
        if not nearby_convs or not nearby_sups:
            continue
        print('park clear')
        convs_sups.append((park_id, nearby_convs, nearby_sups))
        # コンビニ⇔スーパー間も100m以内のペアがあるかチェック
        pair_ok = False
        for clat, clon in nearby_convs:
            for slat, slon in nearby_sups:
                if haversine(clat, clon, slat, slon) <= 100:
                    pair_ok = True
                    break
            if pair_ok:
                break

        if pair_ok:
            valid_parks.append((park_id, plat))

    if not valid_parks:
        print("No park meets the criteria.")
        sys.exit(0)
    print(f"Found {len(valid_parks)} valid parks.")
    print(f"Valid parks: {valid_parks}")
    print(f"Convs and sups: {convs_sups}")
    # 緯度（plat）が最も小さいものが「最南端」
    southmost_id, southmost_lat = min(valid_parks, key=lambda x: x[1])
    print(f"Diver25{{{southmost_id}}}")
    print('realy?')


if __name__ == "__main__":
    main()
```

### Talentopolis (472pt / 53 solves)
記事 / Article:
https://www.guineaecuatorialpress.com/noticias/primera_edicion_de_talentopoli

"Primera edición de Talentopolis" という記事内に登場するステージの位置を答えよ。  

記事内のワードから、赤道ギニアのマラボの San Juan 地区だと特定。 
同イベントを取り上げた別記事を見つけ、外観の情報を補完。  
また、おなじようなイベントのチラシを発見し、会場が`A2 SAN JUAN`と書いてあるの発見。  
よくよく見たら元記事のイベントにはビルに`A-4`と書かれているのを発見し、これは集合住宅の棟番号かな、とチームメイトと話合っていた。
映り込みの角度や周囲の状況から`3.7397737525,8.7992656231`を submit したが、incorrect。その後時間切れ。  
ここらへんの地区が Google ストリートビューのデータがなくてとてもやりにくかった。


## recon

### 00_engineer (100pt / 346 solves)
東京駅の近くでソフトウェアエンジニアの名札を拾った。おそらく落とし物だろう。
このエンジニアが勤務している会社の Web サイト（トップページ）の URL を答えよ。
![kodai](/images/diver-osint-ctf-2025-writeup/engineer.jpg)

github でユーザ名検索したらアカウントが Hit。  
ポートフォリオのコード内に会社名が記載されていた。
### 01_asset (473pt / 53 solves)
"00_engineer" の問題で見つかった会社の CEO が持っていたスマートフォンの資産番号を答えよ。  

解けず。  
Kodai 氏の Github アカウントから X アカウントを発見。  
X の投稿で自作の Git ハンズオンを CEO が実施した旨が記載されていたので、該当レポジトリの Fork から CEO の Github アカウントを発見。  
（この段階で手癖で commit log を見て `03_ceo`を先に解いてしまった。  
CEO の GoogleCalender が漏洩していたので予定を確認したが、定期的な MTG とリリース記念の立食パーティの予定があるのみ。  
立食パーティの写真にスマホが映り込んでいる（もしくは`05_designer`の回答となる人物の情報が取得できる）ことを期待してしばらく掘ってみたが収穫なし。  
写真映り込み系だろうと見越して、X, FB, Instagram のアカウントを探したが見つからず。  

ところがどうやら Instagram にアカウントがあったらしい。  
後追いで確認すると、PC から Instagram でユーザー検索をすると、完全一致では Hit せず末尾の一文字を欠落させて検索すると Hit した。  
スマホから検索するとそのまま Hit するらしい。  
Instagram の仕様を理解していないことがバレた。そしてこの挙動は一体何、、、？  
![insta_not_found](/images/diver-osint-ctf-2025-writeup/insta_false.png)
![insta_found](/images/diver-osint-ctf-2025-writeup/insta_true.png)


### 02_recruit (488pt / 37 solves)
"00_engineer" の問題で見つかった会社で、採用を担当していると思われる人物の氏名を答えよ。
Flag 形式: Diver25{Shigeru Ishiba} (ローマ字表記)  

Kodai 氏の X を読んでいたところ、会社の HP のリプレースを実施したみたいな投稿をしていたので、過去の HP が WebArchives に載っていないかなと推測。 
[WayBackMachine](https://web.archive.org/)にはなかったが、[archive.today](https://archive.md/sD2R0)には過去のページが載っており、このページには採用要項ページのリンクが残っていた。  
当該ページが GoogleDocuments だったので、Author 情報を取れないかと試行錯誤。  
DL してきてプロファイルを見たりしても空。  
最終的に、`Google Account OSINT`とかで調べたら[xeuledoc](https://github.com/Malfrats/xeuledoc)というツールを発見。  
これでメールアドレスを取得した後、Ghunt したら GoogleMap のプロフィールページが公開されていたので Flag GET。

### 03_ceo (302pt / 142 solves)
"00_engineer" の問題で見つかった会社の、CEO のメールアドレス（Gmail）を答えよ。  


`01_asset`の調査中に、CEO の Github アカウントの commit log にメールアドレスが残っているのを発見した。 


### 04_internal (416pt / 93 solves)
"00_engineer" の問題で見つかった会社では、Web ブラウザからアクセス可能な社内用の DevOps プラットフォームが運用されている。
そのシステムのバージョン名を、表記されている通りに答えよ。

Web ブラウザからアクセス可能なプラットフォームと書いているので、とりあえず現状持っている情報のなかで唯一会社が管理してそうな Web Surface を持つ HP を調べようと方針決定。  
dig してから[open port](https://www.shodan.io/host/202.212.71.93)を見たら`Gitea`が動いているのを発見。
```
$ dig magneight.com

; <<>> DiG 9.18.12-0ubuntu0.22.04.2-Ubuntu <<>> magneight.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54868
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;magneight.com.                 IN      A

;; ANSWER SECTION:
magneight.com.          1997    IN      A       202.212.71.93

;; Query time: 43 msec
;; SERVER: 10.255.255.254#53(10.255.255.254) (UDP)
;; WHEN: Sat Jun 07 15:02:00 JST 2025
;; MSG SIZE  rcvd: 58
```
まぁ Git も DevOps プラットフォームか...と思いながら Web 当該ポートに Web アクセスすると左下にバージョン情報が書いてあった。  
ついでに、この段階では IP 直打ちでアクセスしており、サインイン画面にいくと Warning が出る。  
![gitea](/images/diver-osint-ctf-2025-writeup/gitea.png)
別途見つけていた`magn8soft.tokyo`というドメインに`gitea.magn8soft.tokyo`というサブドメインが生えており、こちらだと（https が使えるので）Warning を回避できる。  
社内の正規利用を模しているのと、もしかして`06_leaked`で実際にログインとかするのかなぁと思っていたら、このドメインも WebArchives 経由で情報が取れたらしい。  
これに気づかず`05_designer`は撃沈した。

## transportation

### 36_years_ago (347pt / 125 solves)
このニュース動画に映っている航空機に、1989 年 8 月時点で割り当てられていたトランスポンダの Mode S コードを 16 進数表記で答えてください。
https://www.youtube.com/watch?v=OvR2O_Vpwc0
Flag 形式: Diver25{1234AB}  


ひたすら調べてたら以下が見つかった。
https://www.aviationdb.com/Aviation/Aircraft/9/N9768L.shtm

### platform (136pt / 192 solves)
この写真が撮影された駅はどこか。駅名を答えよ（日本語でも英語でも可）。
（駅名は鉄道会社の公式サイトや Wikipedia に記載されている表記とする）
![platfomrm](/images/diver-osint-ctf-2025-writeup/platform.jpg)
写真に写っている足元の車両表示の色から、小田原線に目処をつける。  
あまり栄えていなさそうな駅だったので、西の方からあたっていくも見つからず断念。  
と思ったらチームメイトが「らいと」の Google map 検索から発見してくれた。  
一回検索してみても Hit 数が多すぎて諦めていたけど、そっちからちゃんとできたらしい。  
チームメイトに感謝。

### sanction (375pt / 112 solves)
2024 年 10 月 25 日、制裁下にあるロシア船籍の RORO 船「ANGARA」がある港湾に停泊していることが衛星画像で確認された。  
停泊位置を答えよ。  

ググると北朝鮮の羅津港とわかる。 
報告書を探すと衛星画像が載っていた。
## history

### bridge (263pt / 155 solves)
動画 / Video:
https://www.youtube.com/watch?v=fRMi8TXQRuo
（無音です / No audio）

この動画で列車が通過した橋梁は、ある災害で損傷した後に架け替えられたものである。架け替えに際して、他の橋梁の構造物が流用されたことがある文献に示されている。その流用元の橋梁名を答えよ（この橋の名前ではない）。
Flag 形式: Diver25{橋梁名}（例: Diver25{日本橋川橋梁}）  

熊本県の豊肥本線 JR 東海大学-竜田口間の橋であることを Google 検索から特定。 

以下の文献から、この橋の名前が第二白川橋梁であることを発見。  

https://kanenohashi.com/index.php?act=detl&id=1022&page=0
https://kumadai.repo.nii.ac.jp/record/23247/files/24-0097-1.pdf

## company

### bid (491pt / 31 solves)
2023 年、オマーンにおいて、ある施設に関連するニュースが報じられた。
https://www.youtube.com/watch?v=TFdubskF9Kw

その後、2024 年 10～11 月に、この施設に関連すると推定される、井戸およびパイプラインを建設するための入札が実施された。
このとき、3 位の金額で入札した企業の CEO の名前を、Web サイトに掲載されている英語表記で答えよ。  


映像内のアラビア語を OCR して翻訳
> التوقيع على اتفاقية لإدارة وتشغيل المستشفى البيطري بمنطقة البشائر بولاية أدم

日本語訳：

> アダム州ブシャーイル地域の獣医病院の運営管理に関する協定に署名  
オマーン国の入札情報を探しに行く。

https://www.omantender.com/tender/drilling-well-bore-supply-installation-pump-and-pipeline-al-bashayer-veterinary-hospital-wilayat-ad-68865d1.php?utm_source=chatgpt.com

https://etendering.tenderboard.gov.om/product/nitParameterView?mode=public&tenderNo=62644&PublicUrl=1&CTRL_STRDIRECTION=LTR&encparam=mode,tenderNo,PublicUrl,CTRL_STRDIRECTION,randomno&hashval=a34c6c2faa1d2a895fd3b37806bafc76958a9ed9396aad8afe5dd7ef034977f2

https://www.globaltenders.com/tender-detail/drilling-well-bore-supply-installation-of-p-96927743
 
公募の情報は公開されていたが、落札者情報は見つかられず。各種サイトのアカウントを作って中を除いてみたが、FreePlan の範囲だと見れなさそうだった。断念。

## hardware

### UART (495pt / 24 solves)
https://www.office-partner.de/tp-link-archer-ax20-12778639

この商品のイーサネットスイッチコントローラに直接 UART でアクセスを試みたい。どの部品の、どのピンにアクセスすればいいだろうか。
PCB 上の部品番号 と、その部品の UART RX / UART TX ピン番号 を答えよ。ピン番号は部品の仕様に準拠せよ。
なお、ピンヘッダやコネクタが利用可能な場合でも、イーサネットスイッチコントローラのピン番号を答えてほしい。  

解けず。  
[FCC ID](https://fccid.io/TE7AX20/Internal-Photos/18-Internal-Photos-4506039)から、中の基板の写真を入手。  
チップっぽいのを一つずつ眺めていった結果、おそらく対象のコントローラは`BCM53134SKFBG`だと推定。  
[データシート](https://www.farnell.com/datasheets/2830672.pdf)などを探索するも、答えになりそうなピンの対応表が見つからずに断念。  
ハードウェア系は自作キーボード作るときにしか触ったことがなかったので知見が浅い。 

### phone (441pt / 78 solves)
2016 年 7 月 23 日～24 日、この携帯電話の発売に先立って EMI 試験が行われた。試験は三重県の会社が実施したようだ。その試験に供された端末のシリアル番号を答えよ。
シリアル番号に / や - といった記号を含む場合、その記号も含めて記載すること。
Flag 形式（例）: Diver25{123-45/6789-0}  
![phone](/images/diver-osint-ctf-2025-writeup/phone.jpg)  
この携帯電話は、（多分多くの携帯電話がそうだと思うけど）各国で発売するために日本の技適の他に米国の FCC という規格も通しているらしい。  
そして FCC は審査に使ったレポートを公開している。
（技適番号から三重県の試験会社とやらを特定したけど使わなかった）

型番で調べると[説明書](https://www.docomo.ne.jp/binary/pdf/support/manual/SH-01J_J_syousai_FV2.pdf)が出てくるので、そこから FCC ID を`APYHRO00240`取得
FCC のサイトで ID を使って調べるとこんな[ページ](https://apps.fcc.gov/oetcf/eas/reports/ViewExhibitReport.cfm?mode=Exhibits&RequestTimeout=500&calledFromFrame=N&application_id=zFxEadpybDvi929GAjZCVw%3D%3D&fcc_id=APYHRO00240)にレポートが公開されている。

レポートに書いてあった Serial  Number を Submit して正解

## military

### object (359pt / 120 solves)
69.216246, 33.378242 には大きな構造物が存在する。この構造物のプロジェクト番号および、構造物の名称（固有名詞）を 現地語 で答えよ。
Flag 形式: Diver25{プロジェクト番号_名称}（例: Diver25{955А_Борей-А}）

画像検索をひたすら開くと、以下のサイトを見つけて、その中に Flag を発見。
https://sdelanounas.ru/blogs/113208/

