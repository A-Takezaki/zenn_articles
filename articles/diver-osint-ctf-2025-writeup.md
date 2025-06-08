---
title: "DIVER OSINT CTF 2025 Writeup"
emoji: "🦎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [ctf,osint]
published: false
---
## はじめに
2025/06/07-2025/06/08に開催されたDIVER OSINT CTF 2025に、チームメイト3人と出場していた。  
競技中に残していた3人分のメモからWriteupを起こしたので残しておく。

## introduction

### document (100pt / 280 solves)
アメリカ海軍横須賀基地司令部（CFAY）は、米軍の関係者向けに羽田空港・成田空港と基地の間でシャトルバスを運行している。2023年に乗り場案内の書類を作成した人物の名前を答えよ。  

`cfay 2023 hnd nrt`などで検索して、[スケジュールを記載したPDF](https://cnrj.cnic.navy.mil/Portals/80/CFA_Yokosuka/Documents/Airport%20Shuttle/2023%2008%2014%20CFAY%20Airport%20Bus%20Schedule.pdf?ver=-K3r8A3eonOJl48Tkf9SnA%3D%3D)を発見。  
PDFファイルのEXIF情報を見たらAuthor情報が残っていた。
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

このヘリコプターが出発した飛行場のICAOコード（4レターコード）で答えよ。  


立川基地の番号入れたら正解した。

### louvre (176pt / 181 solves)
ルーブル美術館の公共Wi-Fiアクセスポイントのうち、以下の条件を満たすもののベンダーを答えよ。

- 情報は2025年2月28日に収集されており、オンライン上で確認できる。
- ベンダーはBSSIDに準拠して判定せよ。

### 解法

https://openwifimap.net/#map?bbox=48.853646831055556,2.3061203956604,48.87044540697354,2.3610520362854004ここのサイトから探したけど、Map上にHitするspotなし

https://www.wifimap.io/map/1109-paris?wifiHotspot=da2d9651-d7df-41a1-a935-ea69fc12e0f0このサイトでWiFIスポットは見れたけど、BSSIDの取得ができず

https://wigle.net/indexBSSIDの取得ができそうだったが、アクセスポイントが見つからない。もしかしたらアカウントを作ったら見れる情報が増えるかも。  
と思ってアカウントを作成したが、too many requestと表示されて詳細な情報が出てこなかった。　 
ここで中断してチームメイトにパスしたところ、「AIくんに聞いたら答えを教えてくれたよ」とのこと。なんか悔しい。   

### night_accident (100pt / 269 solves)  
動画 / Video:
https://www.youtube.com/watch?v=jHgqCpJNL28

この動画で、車とバスが衝突しそうになった場所はどこか。  

バスに52と58の番号が確認できるのでこの2つの路線図を調べ、被っていそうなBishan駅周辺でストリートビューで確認。  
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

備考  
これは別のチームメイトが問題に詰まった息抜きに作ったAA。  
![p2t](/images/diver-osint-ctf-2025-writeup/image.png)



### ship (100pt / 363 solves)
これは、ある組織が運用する船舶である。もし将来、この船が外国に売却されたとしても、変わらない番号を答えよ。
Flag形式: Diver25{現地語での船名_番号}（例: 船名が「ペンギン饅頭号」で番号が 1234567 の場合、Flagは Diver25{ペンギン饅頭号_1234567} となる）  

TKT-1401でググると船名が出る
船名のIMO番号を検索してヒット

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
明らかに大きな滑走路っぽい道路と大穴。 右下の文章から、中国圏のドローンの飛行場や実験場と推測。  
ドローン関係の映像を漁っていたが見つからず。断念。
### night_street (428pt / 86 solves)
画像の中心に写っている茶色の2階建ての建物に入る施設の正式名称を現地語表記で答えなさい。
Flag形式: Diver25{施設名}（例: Diver25{お台場海浜公園前郵便局}）
![night](/images/diver-osint-ctf-2025-writeup/night_street.jpg)

リンガーハットが左にあるので、[ここ](https://shop.ringerhut.jp/all/?page=2)リンガーハットの場所調べてgoogle mapsでローラー。  
イオンとかフードコートは無視で探すも数多すぎて無理、店舗検索の絞り込みで駐車場+ドライブスルーで全部見たけどなし、道路が片側2車線以上でコンクリ舗装のためトラックの交通が多い道、港湾道路とかかな～と思いつつ愛知県の店舗でローラーして発見

### convenience (462pt / 63 solves)
青森県内に、公園とコンビニ、スーパーマーケットが互いに約100m圏内に存在する場所がいくつかある。また、これはOpenStreetMapで確認可能である。
この条件を満たす 公園 のうち、最南端 のものについて、OpenStreetMap上での Way Number を答えよ。
なお、「公園」の定義は、OpenStreetMap上で "park" (leisure=park) と分類されているものに準拠する。

まずはOverpass APIを使って、青森県のコンビニ、スーパー、公園を列挙。  
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

これで得られたデータを元にpythonのスクリプトを書いて、条件に当てはまるものを抽出、、したかったのだが、  
動かしてみた結果出力されたデータはincorrect。なんかどっかでロジックを間違えてそう。  
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

記事内のワードから、赤道ギニアのマラボのSan Juan地区だと特定。 
同イベントを取り上げた別記事を見つけ、外観の情報を補完。  
また、おなじようなイベントのチラシを発見し、会場が`A2 SAN JUAN`と書いてあるの発見。  
よくよく見たら元記事のイベントにはビルに`A-4`と書かれているのを発見し、これは集合住宅の棟番号かな、とチームメイトと話合っていた。
映り込みの角度や周囲の状況から`3.7397737525,8.7992656231`をsubmitしたが、incorrect。その後時間切れ。  
ここらへんの地区がGoogleストリートビューのデータがなくてとてもやりにくかった。


## recon

### 00_engineer (100pt / 346 solves)

### 01_asset (473pt / 53 solves)

### 02_recruit (488pt / 37 solves)

### 03_ceo (302pt / 142 solves)

### 04_internal (416pt / 93 solves)

### 05_designer (500pt / 7 solves)

### 06_leaked (499pt / 12 solves)

## transportation

### 36_years_ago (347pt / 125 solves)

### air2air (495pt / 24 solves)

### listen (473pt / 53 solves)

### next_train (100pt / 245 solves)

### platform (136pt / 192 solves)

### sanction (375pt / 112 solves)

## history

### bridge (263pt / 155 solves)

### internment (478pt / 48 solves)

## company

### bid (491pt / 31 solves)
2023年、オマーンにおいて、ある施設に関連するニュースが報じられた。
https://www.youtube.com/watch?v=TFdubskF9Kw

その後、2024年10～11月に、この施設に関連すると推定される、井戸およびパイプラインを建設するための入札が実施された。
このとき、3位の金額で入札した企業のCEOの名前を、Webサイトに掲載されている英語表記で答えよ。  


映像内のアラビア語をOCRして翻訳
> التوقيع على اتفاقية لإدارة وتشغيل المستشفى البيطري بمنطقة البشائر بولاية أدم

日本語訳：

> アダム州ブシャーイル地域の獣医病院の運営管理に関する協定に署名  
オマーン国の入札情報を探しに行く。

https://www.omantender.com/tender/drilling-well-bore-supply-installation-pump-and-pipeline-al-bashayer-veterinary-hospital-wilayat-ad-68865d1.php?utm_source=chatgpt.com

https://etendering.tenderboard.gov.om/product/nitParameterView?mode=public&tenderNo=62644&PublicUrl=1&CTRL_STRDIRECTION=LTR&encparam=mode,tenderNo,PublicUrl,CTRL_STRDIRECTION,randomno&hashval=a34c6c2faa1d2a895fd3b37806bafc76958a9ed9396aad8afe5dd7ef034977f2

https://www.globaltenders.com/tender-detail/drilling-well-bore-supply-installation-of-p-96927743
 
公募の情報は公開されていたが、落札者情報は見つかられず。各種サイトのアカウントを作って中を除いてみたが、FreePlanの範囲だと見れなさそうだった。断念。

### expense (499pt / 13 solves)

## hardware

### UART (495pt / 24 solves)

### phone (441pt / 78 solves)

## military

### object (359pt / 120 solves)

### worker (499pt / 12 solves)

### radar (481pt / 45 solves)

