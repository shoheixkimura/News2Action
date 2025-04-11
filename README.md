# News2Action

## 概要

**News2Action** は、TechCrunchおよびThe VergeのRSSフィードから最新記事を取得し、それを自社ナレッジベースと照合することで、自社ビジネスへの影響を示唆するAIシステムの最小構成PoCです。

---

## 主な機能

- **ニュース取得**：RSSフィードから最新記事を取得
- **記事整形・保存**：PostgreSQL(JSONB + embedding) に保存
- **自社ナレッジ構築**：Docx / PDF / Markdown などから自社情報を構造化して保存
- **意味検索・照合**：ニュースと自社ナレッジの間でベクトル類似検索
- **影響分析**：LLMによる要約・影響評価・アクション提案

---

## 対象メディア

- [TechCrunch](https://techcrunch.com/feed/)
- [The Verge](https://www.theverge.com/rss/index.xml)

---

## システム構成

```text
[ RSSフィード ]
      ↓
[ RSS取得・整形モジュール ]
      ↓
[ PostgreSQL (JSONB + pgvector) ]
      ↑               ↓
[ 自社ナレッジベース登録 ]     [ LLMによる影響分析 ]
```

---

## 使用技術

- 言語: Python
- ライブラリ: feedparser, requests, psycopg2, openai, langchain
- DB: PostgreSQL + pgvector
- LLM: OpenAI GPT-4 (予定)
- 文書抽出: unstructured, pdfplumber, python-docx, markdown

---

## セットアップ手順

### 1. リポジトリをクローン

```bash
git clone https://github.com/yourname/news2action.git
cd news2action
```

### 2. 必要なライブラリをインストール

```bash
pip install -r requirements.txt
```

### 3. PostgreSQLにテーブルを作成

```sql
-- ニュース記事格納
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE articles (
    id SERIAL PRIMARY KEY,
    title TEXT,
    url TEXT,
    published TIMESTAMP,
    source TEXT,
    content JSONB,
    embedding VECTOR(1536)
);

-- 自社ナレッジベース
CREATE TABLE knowledge_chunks (
    id SERIAL PRIMARY KEY,
    title TEXT,
    category TEXT,
    content TEXT,
    embedding VECTOR(1536)
);
```

### 4. RSS取得スクリプトの実行

```bash
python fetch_articles.py
```

### 5. 自社ナレッジ文書の読み込み・保存（雛形スクリプトあり）

```bash
python import_knowledge.py --path ./docs/
```

このスクリプトは `.pdf`, `.docx`, `.md` を自動で抽出・チャンク化し、embeddingを生成してDBに登録します。

---

## オンプレミス運用の提案

- **LLM**: オープンソース LLM（例：LLaMA3, Mistral） をローカルGPU/CPUサーバ上で起動
- **RAG処理**: LangChain or LlamaIndex をPythonで実行
- **ベクトルDB**: pgvector またはオンプレ対応ベクトルDB（Weaviate / Qdrant）をDockerで構築
- **構成管理**: docker-compose で全体を自社サーバ上に展開可能
- **セキュリティ**: VPN + Role-Based Access Control（RBAC）で外部遮断された安全な構成

---

## 今後の拡張

- アクションプランの生成とJIRA連携
- 類似ナレッジとのマッチ度評価スコアの表示
- Slack通知・レポート自動生成

---

## ライセンス

MIT License

---

shoheixkimura / News2Action Project
