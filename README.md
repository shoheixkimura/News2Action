# News2Action

## 概要

**News2Action** は、TechCrunchおよびThe VergeのRSSフィードから最新記事を取得し、それを自社ナレッジベースと照合することで、自社ビジネスへの影響を示唆するAIシステムの最小構成を構築することを目的としたPoCプロジェクトです。

## 主な機能

- **ニュース取得**：RSSフィードからTechCrunchとThe Vergeの最新記事を取得
- **記事整形・保存**：記事情報をJSONB形式でPostgreSQLに保存、embeddingも格納
- **ナレッジベース連携**：自社ナレッジとの意味検索・照合（LLM活用）
- **影響分析**：LLMによる要約・影響評価・アクション提案（将来的に追加）

## 対象メディア

- [TechCrunch](https://techcrunch.com/feed/)
- [The Verge](https://www.theverge.com/rss/index.xml)

## システム構成

```text
[ RSSフィード ]
      ↓
[ RSS取得・整形モジュール ]
      ↓
[ PostgreSQL(JSONB + pgvector) ]
      ↓
[ LLMによる要約・影響分析（例：OpenAI GPT-4）]
```

## 使用技術

- 言語: Python
- ライブラリ: feedparser, requests, psycopg2, openai, langchain（将来的に）
- DB: PostgreSQL + pgvector
- LLM: OpenAI GPT-4 (予定)

## セットアップ手順

1. リポジトリをクローン
```bash
git clone https://github.com/yourname/news2action.git
cd news2action
```

2. 必要なライブラリをインストール
```bash
pip install -r requirements.txt
```

3. PostgreSQLにテーブルを作成
```sql
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
```

4. RSS取得スクリプトの実行
```bash
python fetch_articles.py
```

5. Embedding生成・保存スクリプトの実行（OpenAI APIキーが必要）
```bash
python embed_articles.py
```

## 今後の拡張

- 自社ナレッジベースの構築と統合
- LangChainまたはLlamaIndexによるRAG検索
- 影響度評価・アクションプラン生成とタスク管理連携

## ライセンス

MIT License

---

INTEC USA / News2Action Project
