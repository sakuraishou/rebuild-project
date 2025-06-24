# ベースとなるNode.jsの環境を指定
FROM node:22-alpine

# 作業ディレクトリを設定
WORKDIR /app

# まずpackage.jsonなどをコピーして、依存パッケージをインストール
COPY ./package.json ./
COPY ./package-lock.json ./
RUN npm install

# プロジェクトのすべてのファイルをコピー
COPY . .

# 本番用にアプリをビルド
RUN npm run build

# 1337番ポートを開ける
EXPOSE 1337

# コンテナが起動した時に、このコマンドを実行する
CMD ["npm", "start"]