# react-fastapi-postgres-neo4j-template

React、FastAPI、PostgreSQL、Neo4j、Traefik を組み合わせたフルスタック Web アプリケーションの構成テンプレートです。

## 構成

- **Frontend**: React (TypeScript) + Vite
- **Backend**: FastAPI (Python)
- **RDB**: PostgreSQL
- **GraphDB**: Neo4j
- **Proxy**: Traefik (ローカル開発および本番でのドメイン・SSL制御)

## ドキュメント

- [システムアーキテクチャ概要](docs/architecture.md): 全体構成図（Mermaid）とコンポーネント構成
- [開発ロードマップ](docs/roadmap.md): フェーズ別開発計画・タスク管理
- [Architecture Decision Records (ADR)](docs/adr/README.md): 技術選定・設計判断の記録

## クイックスタート

1. **環境変数の準備**
   各ディレクトリの `.env.sample` を参考に `.env` を作成します。
   ```bash
   cp .env.sample .env
   cp db/.env.sample db/.env
   cp proxy/.env.sample proxy/.env
   ```

2. **ネットワークの作成（初回のみ）**
   ```bash
   # エッジ用ネットワーク (Traefik, Frontend, Backend)
   docker network create edge-net

   # データベース専用の完全内部遮断ネットワーク (Backend, PostgreSQL, Neo4j)
   docker network create --internal internal-db-net
   ```

3. **コンテナの起動**
   ```bash
   # プロキシの起動 (Traefik + Docker Socket Proxy)
   docker compose -f proxy/compose.yaml up -d

   # データベースの起動 (PostgreSQL + Neo4j)
   docker compose -f db/compose.yaml up -d

   # アプリケーションの起動 (FastAPI + React/Nginx)
   docker compose up -d
   ```


## 便利なショートカットコマンド

各サービス（`backend`, `frontend`, `postgres`, `neo4j`）のコンテナ内シェルへ簡単にログインするためのスクリプトが用意されています。

### Makefile を使用する場合 (Linux / macOS / WSL)
```bash
# デフォルト (backend コンテナのシェルに入る)
make shell

# 対象サービスを指定してシェルに入る
make shell SERVICE=postgres
make shell SERVICE=neo4j
make shell SERVICE=frontend
```

### シェルスクリプトを使用する場合
- **Linux / macOS (`shell.sh`)**:
  ```bash
  ./shell.sh           # デフォルト: backend
  ./shell.sh postgres  # postgres コンテナ
  ./shell.sh neo4j     # neo4j コンテナ
  ./shell.sh frontend  # frontend コンテナ
  ```

- **Windows Command Prompt / PowerShell (`shell.bat`)**:
  ```cmd
  .\shell.bat          :: デフォルト: backend
  .\shell.bat postgres :: postgres コンテナ
  .\shell.bat neo4j    :: neo4j コンテナ
  .\shell.bat frontend :: frontend コンテナ
  ```
