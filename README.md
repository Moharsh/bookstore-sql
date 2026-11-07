# Bookstore SQL Project

A small SQLite and Python project for practicing relational database design and querying. The notebook creates a bookstore schema, inserts sample records, checks several integrity constraints, and runs exploratory and business analysis queries.

## Project files

- `sql-query.ipynb` — database setup, sample data, integrity checks, and SQL analysis.
- `requirements.txt` — Python packages used by the notebook.
- `bookstore.db` — local SQLite database created by the notebook. It is intentionally excluded from Git.

## Database design

The schema contains six tables:

| Table | Purpose |
| --- | --- |
| `Authors` | Author records. |
| `Books` | Book title, publication year, catalogue price in paise, stock, and ISBN. |
| `BookAuthors` | Junction table linking books and authors; its composite primary key prevents duplicate pairings. |
| `Customers` | Customer contact details. |
| `Orders` | Customer orders, dates, and status (`pending`, `completed`, or `cancelled`). |
| `OrderItems` | Books and quantities on each order, with the unit price recorded for that order. |

`BookAuthors` supports a many-to-many relationship: a book can have multiple authors and an author can be linked to multiple books. Each order belongs to one customer and can have multiple order items. Each order item refers to one book. Foreign keys connect these relationships.

The schema uses primary keys, `NOT NULL`, `UNIQUE`, and `CHECK` constraints. Prices are stored as integer paise to avoid storing fractional currency values; the notebook converts them to rupees for some displayed results. The notebook enables SQLite foreign-key enforcement on its connection.

## Analysis and observed results

The notebook's saved outputs report 8 authors, 10 books, 8 customers, 12 orders, 20 order items, and 13 book-author links. Its foreign-key check reported no violations. Constraint examples rejected a duplicate ISBN, negative stock, zero order-item quantity, and an order referencing a missing customer.

The saved completed-order revenue query reports **₹13,429**. The monthly query reports ₹7,488 for January 2026, ₹3,145 for February, and ₹2,796 for March. Its average completed-order value query reports **₹1,678.625**. The inventory query reports 227 copies across the catalogue, with `The Quantum Frontier` (12 copies) and `Intelligence Unbound` (10 copies) below the notebook's 15-copy restocking threshold.

These figures describe the notebook's sample data and saved outputs, not real bookstore performance. Some other analysis queries need correction before their results can support business conclusions: the customer-spend query does not multiply unit price by quantity; the “no completed order” condition is malformed; some book-sales queries do not filter to completed orders; and the book revenue query's displayed `copies_sold` value is not aggregated correctly. The author revenue query allocates the full revenue of co-authored books to each author, so author totals overlap. Treat those outputs as practice results, not verified rankings or customer metrics.

## Setup and run

1. Install Python 3 and create/activate a virtual environment.
2. Install the listed packages with `python -m pip install -r requirements.txt`.
3. Open `sql-query.ipynb` in Jupyter Notebook, JupyterLab, or an editor with notebook support and run the cells in order.

The notebook connects to `bookstore.db` in its current working directory and creates tables with `IF NOT EXISTS`. The seed-data cell uses ordinary `INSERT` statements, so it is intended for a fresh database and is not safe to rerun against an already populated database. To rebuild from scratch, close the notebook connection and remove the local `bookstore.db` file before running all cells again. This database is ignored by Git; a fresh clone therefore needs to run the notebook to create it.

## Limitations and next steps

- The data is a small, hand-authored sample and cannot establish real customer or sales trends.
- Correct the noted aggregation and filtering issues before presenting those analyses as findings.
- The notebook's setup depends on starting with a fresh database because sample inserts are not idempotent.
- The inventory valuation uses current catalogue prices, not cost basis, and is not a measure of profit.
- The project does not include automated tests or a separate production database layer.

