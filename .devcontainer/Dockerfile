FROM node:20

# 基本的な開発ツールをインストール
RUN apt update && apt install -y \
  git \
  less \
  procps \
  sudo \
  jq

# nodeユーザーにsudo権限を付与
RUN echo "node ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# デフォルトのnodeユーザーが/usr/local/shareにアクセスできるようにする
RUN mkdir -p /usr/local/share/npm-global && \
  chown -R node:node /usr/local/share

# npmキャッシュディレクトリを作成し、権限を設定
RUN mkdir -p /home/node/.npm && \
  chown -R node:node /home/node/.npm

# `DEVCONTAINER`環境変数を設定（VS Code自体が必要とするものではない）
# アプリケーションが開発コンテナ内で動作していることを検出し、必要に応じて特別な設定や動作を適用するためのフラグ
ENV DEVCONTAINER=true

# ワークスペースと設定ディレクトリを作成
RUN mkdir -p /workspace /home/node/.claude && \
  chown -R node:node /workspace /home/node/.claude

WORKDIR /workspace

# node（非root）ユーザーを設定
USER node

# npmグローバルパッケージを設定
ENV NPM_CONFIG_PREFIX=/usr/local/share/npm-global
ENV PATH=$PATH:/usr/local/share/npm-global/bin

# npm設定でuid/gid問題を回避
ENV npm_config_unsafe_perm=true

# Claudeをインストール（エラー時の詳細表示のため--verboseを追加）
RUN npm install -g @anthropic-ai/claude-code --verbose