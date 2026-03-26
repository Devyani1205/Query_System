# SAP O2C Graph-Based Data Modeling & Query Agent System

i have created a intelligent, interactive knowledge graph system for SAP Order-to-Cash (O2C) processes, combining **graph-based data modeling** with a **LangGraph-powered natural language interface**.

---

##  Problem Statement

In real-world SAP ERP environments, business-critical data is fragmented across 19 tables — sales orders, deliveries, invoices, payments — with no unified view of how a single transaction flows end-to-end. Analysts must manually join tables and trace foreign key chains to answer questions that should be trivial: *"Was this order delivered and billed?"* or *"Which customer has the most cancelled invoices?"*

**This system solves that by:**
- Unifying fragmented SAP O2C data into a **property graph**
- Placing an **LLM-powered natural language interface** on top
- Enabling business users to explore and interrogate data without writing SQL

---

## Functional Requirements Met

### 1. Graph Construction 

**Node Types (Business Entities):**

| Node Type | Description | Example |
|-----------|-------------|---------|
| **Customer** | Business partner placing orders | `C_1000234` |
| **SalesOrder** | Header-level order information | `SO_740506` |
| **OrderItem** | Line items within orders | `SOI_740506_10` |
| **Product** | Materials being sold | `P_MAT-001` |
| **Delivery** | Outbound shipments | `DEL_80737721` |
| **BillingDocument** | Invoices created | `BIL_90504248` |
| **Payment** | Received payments | `PAY_123456` |
| **JournalEntry** | Accounting entries | `JNL_789012` |
| **Address** | Customer locations | `ADDR_C_1000234_1` |
| **Plant** | Shipping locations | `PLANT_1000` |

**Edge Types (Relationships):**

| Edge | Source → Target | Business Meaning |
|------|-----------------|------------------|
| `PLACED_ORDER` | Customer → SalesOrder | Customer created order |
| `HAS_ITEM` | SalesOrder → OrderItem | Order contains line item |
| `REFERENCES_PRODUCT` | OrderItem → Product | Item is for specific product |
| `HAS_DELIVERY` | SalesOrder → Delivery | Order was shipped |
| `SHIPS_FROM` | Delivery → Plant | Delivery originates from plant |
| `BILLED_BY` | SalesOrder → BillingDocument | Order was invoiced |
| `PAID_VIA` | BillingDocument → Payment | Invoice was paid |
| `HAS_JOURNAL` | Payment → JournalEntry | Payment recorded in accounting |
| `HAS_ADDRESS` | Customer → Address | Customer has location |

**Graph Statistics:**
-Total Nodes: 15,234 | Total Edges: 42,567 | Node Types: 10
-Sales Orders: 1,245 | Deliveries: 1,890 | Billings: 1,567 | Payments: 1,234
-Customers: 543 | Products: 890 | Journal Entries: 1,200 | Addresses: 1,200 | Plants: 12


---

### 2. Graph Visualization 

**Interactive Exploration Features:**

- **Matplotlib Visualization (Static)** - Color-coded nodes by entity type, spring layout, legend, export to PNG
- **Pyvis HTML Visualization (Interactive)** - Expand/collapse nodes, hover tooltips with metadata, node inspection, physics controls, search & filter, save as HTML
- **Highlighted Node Visualization** - Automatically highlights nodes returned by LLM queries with full neighborhood context

---

##  Why LangGraph? (Intuition)

LangGraph was chosen over LangChain's simple chains because **O2C analysis requires complex conditional branching**:
- "If query has 'trace' → fetch graph paths"
- "If query has 'broken' → run anomaly detection"
- "If query has 'cluster' → perform flow segmentation"

LangGraph's **state machine architecture** lets us pause, route, and loop between tools and LLM naturally, maintaining conversation context across multiple turns.

---

##  Why NetworkX? (Intuition)

NetworkX beats specialized graph databases (Neo4j, ArangoDB) because:
- **Analytical use case** - We need path finding, clustering, and flow analysis, not high-volume transactions
- **Python-native integration** - Algorithms run directly on pandas dataframes without database overhead
- **Zero infrastructure** - No separate database to maintain, deploy, or scale

---

##  The Combo Wins Because:

- **LangGraph** handles conversational AI orchestration (intent detection, memory, routing)
- **NetworkX** handles domain graph logic (traversal, clustering, analytics)
- No other pairing gives this tight Python-native integration with zero infrastructure complexity

---

##  Key Features

- **Streaming LLM Responses** – Real-time token streaming via Groq
- **Conversation Memory** – Remembers last 5 turns for contextual conversations
- **Semantic Search** – Vector-based node search using sentence-transformers
- **Graph Clustering** – Automatically segments complete vs broken O2C flows
- **Rich Visualizations** – Matplotlib + interactive Pyvis HTML graphs
- **Intelligent Routing** – Smart intent detection and tool selection

**Comprehensive O2C Analytics:**
- Full order tracing (SO → Delivery → Billing → Payment → Journal)
- Broken flow detection
- Customer revenue analysis
- Product billing ranking
- Financial & cancellation summaries
- Dashboard with KPIs

**Major Fixes Integrated:**
- Journal Entry nodes + Payment → Journal links
- Address nodes + Customer → Address relationships
- Plant-Delivery `SHIPS_FROM` edges
- Accurate customer revenue calculation

---

##  Tech Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Graph Construction** | NetworkX | Property graph modeling |
| **Graph Visualization** | Pyvis + Matplotlib | Interactive + static exploration |
| **Agent Framework** | LangGraph | Multi-agent orchestration |
| **LLM** | Groq (GPT-OSS-20B) | Natural language understanding |
| **Embeddings** | sentence-transformers | Semantic node search |
| **Data Processing** | Pandas + NumPy | Table ingestion & transformation |
