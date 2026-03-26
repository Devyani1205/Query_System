# SAP O2C LangGraph Multi-Agent System

i have created LangGraph agent system for Graph-Based Data Modeling and Query System intelligent, interactive knowledge graph agent for analyzing SAP Order-to-Cash (O2C) processes using LangGraph Agentic Framework due to , Groq, and NetworkX.

##  Overview

This system transforms raw SAP O2C transactional data into an interactive knowledge graph, user can ask que in  natural language queries and agent give intelligent analysis of your order-to-cash process. this is multi-agent architecture that combines graph analytics, LLM reasoning, semantic search, conversation memory, and streaming responses.

## Problem Statement

In real-world SAP ERP environments, business-critical data is fragmented across 19 tables — sales orders, deliveries, invoices, payments  with no unified view of how a single transaction flows end-to-end. Analysts must manually join tables and trace FK chains to answer questions that should be trivial: "Was this order delivered and billed?" or "Which customer has the most cancelled invoices?"

This system solves that by unifying the fragmented SAP O2C (Order-to-Cash) data into a property graph and placing an LLM-powered natural language query interface on top of it, enabling business users to explore and interrogate the data without writing a single line of SQL.


##  Key Features

- **Streaming LLM Responses** – Real-time token streaming via Groq
- **Conversation Memory** – Remembers last 5 turns for contextual conversations
- **Semantic Search** – Vector-based node search using sentence-transformers
- **Graph Clustering** – Automatically segments complete vs broken O2C flows
- **Rich Visualizations** – Matplotlib + interactive Pyvis HTML graphs
- **Intelligent Routing** – Smart intent detection and tool selection
- **Comprehensive O2C Analytics**:
  - Full order tracing (SO → Delivery → Billing → Payment → Journal)
  - Broken flow detection
  - Customer revenue analysis
  - Product billing ranking
  - Financial & cancellation summaries
  - Dashboard with KPIs

## 🛠 Tech Stack

- **LLM**: `openai/gpt-oss-20b` via Groq
- **Framework**: LangGraph 
- **Graph**: NetworkX + Pyvis
- **Embeddings**: sentence-transformers (`all-MiniLM-L6-v2`)
- **Visualization**: Matplotlib + Pyvis
- **Data**: SAP O2C transactional tables (JSONL format)
