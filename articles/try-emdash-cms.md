---
title: "WordPress後継CMS「EmDash」を触ってみる"
emoji: "🦎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [Cloudflare,WordPress,EmDash]
published: false
---
## はじめに

2026/04/02の朝に起きたら、CloudflareがWordPressの後継を名乗るCMSを公開していた。

いろんな人がWordPressを雑に使って世界中にセキュアじゃないWebサイトが乱立している現状がどうにかならんかなーと思っていた立場として、気になったからとりあえずセットアップから管理画面の操作、記事反映の挙動、デプロイまでを一通り触ってみた結果を公開する。

https://x.com/z4ck_key/status/2039482948354027829?s=20

今回は「WordPress ライクな操作感を持ちつつ、モダンな技術スタックで構成された CMS」としてどのような印象だったかを、実際に触って分かったことベースでまとめる。

## EmDash とは

EmDash は、WordPress のような親しみやすい管理画面を持ちながら、Astro や Cloudflare Workers などのモダンな構成で動かせる CMS。

実際に触ってみると、**編集体験は WordPress に寄せつつ、実装基盤はかなりAstroが残っていて**記事の追加等のコンテンツ管理はWordPressが触れたら実行できるし、テーマのカスタマイズはAstroを触ったことがあれば簡単そう、と使い勝手は良さそうな感触を得た。

Playground はこちら
https://emdashcms.com/_emdash/admin/
リポジトリはこちら
https://github.com/emdash-cms/emdash/

## まずはプロジェクトを作成する

まずは `npm create emdash@latest` でプロジェクトを作成する。

対話型シェルで聞かれる項目はこんな感じ。

```bash
$ npm create emdash@latest
  — E M D A S H —

┌  Create a new EmDash project
│
◆  Project name?
│  my-site
◆  Where will you deploy?
│  ● Cloudflare Workers (D1 + R2)
│  ○ Node.js

◆  Which template?
│  ● Blog (A blog with posts, pages, and authors)
│  ○ Starter
│  ○ Marketing
│  ○ Portfolio
│  ○ Blank
◆  Which package manager?
│  ○ pnpm
│  ● npm
│  ○ yarn
│  ○ bun
◆  Install dependencies?
│  ● Yes / ○ No
└
```

作成後はこのようにセットアップが完了する。

```bash
  — E M D A S H —

┌  Create a new EmDash project
│
◇  Project name?
│  my-emdash-site
│
◇  Where will you deploy?
│  Cloudflare Workers
│
◇  Which template?
│  Marketing
│
◇  Which package manager?
│  npm
│
◇  Install dependencies?
│  Yes
│
◇  Project created!
│
◇  Dependencies installed!
│
◇  Next steps ────────╮
│                     │
│  cd my-emdash-site  │
│  npm run dev        │
│                     │
├─────────────────────╯
│
└  Done! Your EmDash project is ready at my-emdash-site
```

![image.png](/images/try-emdash-cms/image%201.png)  

なお、`npm create emdash@latest` の時点で `AGENTS.md` や `CLAUDE.md` が作成されていたのは興味深かった。"EmDash is designed to be managed programmatically by your AI agents.”という通り、AI エージェントや開発支援ツールとの連携をかなり意識した設計に見える。

## 初期画面と管理画面の印象

Starter テンプレートを起動すると、まずはこのような画面が表示される。

![image.png](/images/try-emdash-cms/image%202.png)  

`Open Admin` を選択するとサイトセットアップ画面に進む。
![image.png](/images/try-emdash-cms/image.png)  

ここで印象的だったのは、**かなり WordPress っぽい UI/UX になっている**こと。既存の WordPress ユーザーでも比較的入りやすそうだと感じた。

一方で、セットアップ段階から Passkey を求められる。

![image.png](/images/try-emdash-cms/image%203.png)  

サインイン画面もこのような形。

![image.png](/images/try-emdash-cms/image%204.png)  

Passkey が初期状態で使えるのはセキュアでかなり好印象だった。その反面、WordPress に慣れた層の中には戸惑う人もいそうで、導入時のサポート設計は少し気になった。

## 投稿作成 UI はかなり WordPress ライク

投稿の作成画面はこのような見た目だった。

![image.png](/images/try-emdash-cms/image%205.png)  

このあたりはかなり WordPress 的で、既存顧客にとってもとっつきやすそう。

実際にWebサイトに投稿する担当者が迷いにくいという意味では、移行先 CMS としてのポテンシャルは高そうに見えた。

## AI 連携ネイティブ

初期状態で `SKILLS` が入っているのも特徴的だった。

![image.png](/images/try-emdash-cms/image%206.png)  

試しに `新商品のPRをする記事を作成したいです。` と Claude Code 経由で依頼してみると、いい感じに `SKILLS` を読み込んで記事生成を進めてくれた。

![image.png](/images/try-emdash-cms/image%207.png)  

AI を前提にした CMS 体験としてはかなり良く、今後伸びそうなポイントだと感じた。

## 記事データは JSON で管理される

生成された記事データは JSON で管理されていた。

```json
{
	"$schema": "https://emdashcms.com/seed.schema.json",
	"version": "1",
	"meta": {
		"name": "Marketing Starter",
		"description": "A conversion-focused marketing site with landing pages",
		"author": "EmDash"
	},

	"settings": {
		"title": "Acme",
		"tagline": "Build products people actually want"
	},

	"collections": [
		{
			"slug": "pages",
			"label": "Pages",
			"labelSingular": "Page",
			"supports": ["drafts", "revisions", "seo"],
			"fields": [
				{
					"slug": "title",
					"label": "Title",
					"type": "string",
					"required": true
				},
				{
					"slug": "content",
					"label": "Content",
					"type": "portableText"
				}
			]
		}
	],

	"menus": [
		{
			"name": "primary",
			"label": "Primary Navigation",
			"items": [
				{ "type": "custom", "label": "Features", "url": "/#features" },
				{ "type": "custom", "label": "Pricing", "url": "/pricing" },
				{ "type": "custom", "label": "Contact", "url": "/contact" }
			]
		}
	],

	"content": {
		"pages": [
			{
				"id": "home",
				"slug": "home",
				"status": "published",
				"data": {
					"title": "Home",
					"content": [
						{
							"_type": "marketing.hero",
							"_key": "hero",
							"headline": "Build products people actually want",
							"subheadline": "The all-in-one platform for modern teams. Ship faster, collaborate better, and focus on what matters.",
							"primaryCta": { "label": "Start Free Trial", "url": "/signup" },
							"secondaryCta": { "label": "Watch Demo", "url": "/demo" }
						},
						{
							"_type": "marketing.features",
							"_key": "features",
							"headline": "Everything you need to ship",
							"subheadline": "Powerful features that help your team move faster without sacrificing quality.",
							"features": [
								{
									"icon": "zap",
									"title": "Lightning Fast",
									"description": "Built for speed from the ground up. Your team will notice the difference from day one."
								},
								{
									"icon": "shield",
									"title": "Enterprise Security",
									"description": "SOC 2 compliant with end-to-end encryption. Your data stays yours."
								},
								{
									"icon": "users",
									"title": "Team Collaboration",
									"description": "Real-time collaboration features that make working together feel effortless."
								},
								{
									"icon": "chart",
									"title": "Powerful Analytics",
									"description": "Understand how your team works with detailed insights and reporting."
								},
								{
									"icon": "code",
									"title": "Developer Friendly",
									"description": "A robust API and CLI tools that integrate with your existing workflow."
								},
								{
									"icon": "globe",
									"title": "Global Scale",
									"description": "Deployed to edge locations worldwide. Fast for everyone, everywhere."
								}
							]
						}
					]
				}
			}
		]
	}
}
```

この構成を見る限り、**コンテンツは管理画面で編集しつつ、最終的な出力やテーマはコードベースで強くコントロールする思想**が見える。

WordPress の「テーマ + 管理画面」の考え方に近いが、データ構造はより開発者フレンドリーだった。

## ハマりどころ1: 記事を追加してもすぐには反映されない

ここで少しハマった。

記事を追加しても、**すぐにはサイト表示に反映されなかった**。

![image.png](/images/try-emdash-cms/image%208.png)  

確認してみると、記事追加後に `npx emdash seed seed/seed.json` を実行して CMS 側へ反映させる必要があるようだった。

## ハマりどころ2: GUI から作成した記事をそのままでは公開できなかった

GUI から記事を作成してみたところ、そのままでは公開できなかった。

一方で、コードベースで Astro ファイルを実装すると表示はできた。

直感に反する仕様だから、なんか手元環境の動作が怪しかっただけ、もしくはアルファ版のバグでそのうち修正されるとかなのではないかと感じている。

## ページ構成は Astro で作られている

フロント側は Astro で構成されている。

この点はかなり好印象で、テーマカスタマイズやレイアウト調整はやりやすそうだった。

```jsx
---index.asrto
import { getEmDashEntry } from "emdash";
import Base from "../layouts/Base.astro";
import MarketingBlocks from "../components/MarketingBlocks.astro";

const { entry: page, cacheHint } = await getEmDashEntry("pages", "home");

try {
	Astro.cache.set(cacheHint);
} catch {}

const pageTitle = page?.data.title;
const pageContent = page?.data.content;
---

<Base
	title={pageTitle !== "Home" ? pageTitle : undefined}
	description="Build products people actually want. The all-in-one platform for modern teams."
>
	{
		pageContent ? (
			<MarketingBlocks value={pageContent} />
		) : (
			<div class="empty-state">
				<h1>Welcome to Acme</h1>
				<p>Edit the home page content in the admin to get started.</p>
				<a href="/_emdash/admin" class="btn btn-primary">
					Open Admin
				</a>
			</div>
		)
	}
</Base>

<style>
	.empty-state {
		text-align: center;
		padding: var(--spacing-5xl) var(--spacing-lg);
		max-width: var(--max-width);
		margin: 0 auto;
	}

	.empty-state h1 {
		margin-bottom: var(--spacing-md);
	}

	.empty-state p {
		color: var(--color-muted);
		margin-bottom: var(--spacing-xl);
	}
</style>
```

WordPressをテーマをデザインカンプ通りに再現する作業は禿げ上がるので、こんな感じでAstroベースでカスタマイズできるのはとてもありがたい。いやまじで。

## デプロイは Cloudflare Workers ベースで進めやすい

デプロイ自体は分かりやすく、基本的には `wrangler` を使ったいつもの流れで進められた。楽。

```bash
npx wrangler login
npx wrangler d1 create my-marketing-site
npx wrangler r2 bucket create my-marketing-media

npm run build && npm run deploy
```

![image.png](/images/try-emdash-cms/image%209.png)  

注意点として、何も考えずにClaude Codeにお願いしていると、非対話モードで `wrangler` コマンドを叩くせいで対話的に埋まる想定の ID などが `wrangler.jsonc` に入らず余計な手間が増える。ここらへんの決定的なコマンドは無精せずちゃんとやろうね。（一敗）

## 触ってみた感想

ここまで触ってみて感じたポイントを整理すると、次の通り。

- 管理画面はかなり WordPress ライクで、編集者に優しい
- 実装基盤は Astro / Cloudflare Workers ベースでかなりモダン
- Passkey 対応など、セキュリティ志向がちゃんとしている（WordPress比）
- プラグインがサンドボックスに隔離実行されるらしくてとてもちゃんとしている（WordPress比）
    - ただし、 Cloudflare Workers デプロイする場合はWorkerのPaidPlanが必要みたい
- AI エージェントや `SKILLS` など、今っぽい拡張性を感じる

編集体験を見るとWordPressに代わるWebサイトとして魅力的で、サイト構築自体も開発者フレンドリーで期待を持てるという感想。

## まとめ

EmDash は、WordPress の後継候補として面白そうなプロダクトだった。

現状はまだアルファ版らしい荒さも感じるが、そのぶん今後の伸びしろはかなり大きい。

ぜひWordPressを駆逐してセキュアなWebサイトが増えてほしい。

## 参考文献

https://blog.cloudflare.com/emdash-wordpress/
https://github.com/emdash-cms/emdash/