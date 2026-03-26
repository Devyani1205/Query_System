# SAP O2C Graph-Based Data Modeling & Query System

An intelligent, interactive knowledge graph system for SAP Order-to-Cash (O2C) processes, combining **graph-based data modeling** with a **LangGraph-powered natural language interface**.

##  Problem Statement

In real-world SAP ERP environments, business-critical data is fragmented across 19+ tables — sales orders, deliveries, invoices, payments — with no unified view of how a single transaction flows end-to-end. Analysts must manually join tables and trace foreign key chains to answer questions that should be trivial: *"Was this order delivered and billed?"* or *"Which customer has the most cancelled invoices?"*

**This system solves that by:**
- Unifying fragmented SAP O2C data into a **property graph**
- Placing an **LLM-powered natural language interface** on top
- Enabling business users to explore and interrogate data without writing SQL

---

##  Functional Requirements Met

### 1. Graph Construction ✓

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

