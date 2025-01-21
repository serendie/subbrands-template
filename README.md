# Serendie UI にブランド独自のトークンを追加する

Figma の local variables、または JSON ファイルに記述したデザイントークンを、PandaCSS のトークンや CSS 変数などに変換し、ブランド独自のトークンを serendie/ui を使ったプロジェクトに追加することができます。

## 初期設定

GitHub の「Use this template」から、リポジトリを作成しローカルにクローンします。

## パッケージのインストール

レポジトリのルートで以下のコマンドを実行します。

```bash
npm install
```

## 環境変数の設定

Figma の API トークンとファイルキーを環境変数に設定します。

```bash
cp .env.example .env
```

- PERSONAL_ACCESS_TOKEN
  - Figma の API トークン
- FILE_KEY
  - Figma のファイルキー

## Figma からデータを取得

Rest API を使って Figma のデータを取得し、JSON ファイルに保存します。

```bash
npm run sync-figma-to-json
```

## デザイントークンの生成

style-dictionary を使って、JSON ファイルに保存したデザイントークンを PandaCSS のトークンや CSS 変数などに変換します。

```bash
npm run generate-design-tokens
```

## JSON ファイルを Figma に同期

Rest API を使って手元の JSON ファイルを Figma に同期します。

```bash
npm run sync-figma-from-json
```

## 生成したトークンを serendie/ui を含むプロジェクトで使う

### プロジェクトにこのリポジトリをインストールします。

```bash
# GitHubリポジトリから直接インストール
npm install github:serendie/sub-brand-tokens
```

実際は以下のようなルールのパスになります。

```bash
npm install github:{owner}/{repo}
```

### PandaCSS のトークンをインポートします。

`panda.config.js` に以下のように記述し、生成したトークンをインポートします。

```ts
import subBrandTokens from "@serendie/sub-brand-tokens/panda";

// PandaCSSのtokensとtextStylesが混在しているので分離する
const { textStyles, ...tokens } = subBrandTokens;
// 必要に応じてtokenの内容を拡張する
const extendedTokens = { ...tokens, sizes: tokens.spacing };
```

`panda.config.js` の `presets` に以下のように記述し、SerendiePreset を拡張します。

```ts
export default defineConfig({
  //...snip
  presets: [
    SerendiePreset,
    {
      theme: {
        extend: {
          tokens: extendedTokens,
          textStyles,
        },
      },
    },
  ],
  //...snip
});
```

必要に応じて `panda codegen` を実行すると、ブランドのトークンが更新されプロジェクト内で利用することができます。
