# --- ステージ1: 依存パッケージのインストール ---
FROM node:22-alpine AS deps
# Alpine Linuxは軽量なLinuxディストリビューションです
WORKDIR /app

# package.jsonなどを先にコピーして、npm installを実行
COPY package.json ./
# package-lock.jsonもコピー
COPY package-lock.json* ./
RUN npm install --frozen-lockfile


# --- ステージ2: アプリケーションのビルド ---
FROM node:22-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

RUN npm run build


# --- ステージ3: 本番用の軽量なイメージを作成 ---
FROM node:22-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production

# Next.jsのStandalone Outputを利用するための設定
# 実行に必要なファイルだけをコピーして、イメージサイズを最小化します
COPY --from=builder /app/public ./public
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

# ポート3000番を開ける
EXPOSE 3000

# サーバーを起動する
ENV PORT 3000
CMD ["node", "server.js"]