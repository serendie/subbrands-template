# How to use Sub Brand Tokens

## パッケージのインストール

```bash
npm install
```

## 環境変数の設定

```bash
cp .env.example .env
```

- PERSONAL_ACCESS_TOKEN
- FILE_KEY

## Figma からデータを取得

```bash
npm run sync-figma-to-json
```

## デザイントークンの生成

```bash
npm run generate-design-tokens
```

## JSON ファイルを Figma に同期

```bash
npm run sync-figma-from-json
```

## 生成したトークンを serendie/ui を含むプロジェクトで使う

TBD
