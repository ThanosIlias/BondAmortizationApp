# Bond Analyzer Pro

**An IFRS 9-compliant bond portfolio tracker** — event-sourced amortized-cost accounting, lot-level effective interest rates, and multi-currency support, built as a Streamlit web app with a SQLite backend.

> Built to replace a personal, error-prone Excel workbook with a system whose accounting logic is actually tested, auditable, and correct — reviewed line-by-line by a chartered auditor.

---

## Why this exists

Tracking a bond portfolio at amortized cost under IFRS 9 by hand in Excel works — until it doesn't. A single wrong day-count, a settlement date typed before the trade date, a coupon frequency that silently breaks on a 29th-of-February anniversary, or a blended "average EIR" that quietly stops meaning anything once you top up a position — and the numbers are wrong in a way that's invisible until someone checks by hand.

Bond Analyzer Pro exists to make that class of error structurally impossible: every accounting decision is encoded as an explicit rule, every input is validated before it touches a calculation, and every non-obvious behavior is covered by a test that names the real-world mistake it prevents.

## What it does

- **Full amortized-cost / effective-interest accounting (IFRS 9)** — EIR computed via XIRR (compound discounting on actual dates, not a simplified straight-line approximation), frozen at initial recognition and never silently recalculated.
- **Event-sourced portfolio model** — a bond's history is a sequence of immutable events (purchase, top-up, sale, cash flow revision), not a single mutable row. The current position is always *derived*, never stored, so it can never drift out of sync with its own history.
- **Lot-level accounting** — every acquisition is its own lot with its own frozen EIR (IFRS 9 §5.1.1). A top-up never restates the interest income already recognized on an earlier lot. Disposals are allocated across open lots pro-rata (or to a specific lot on request).
- **IFRS 9 B5.4.6 catch-up mechanism** — when the *estimated* future cash flows of an existing instrument change (e.g. revising a perpetual bond's assumed call date), the carrying amount is recalculated as the present value of the revised cash flows at the *original* frozen EIR, with the adjustment recognized immediately in P&L — modeled as its own explicit event type, distinct from a new acquisition.
- **Separated P&L components** — interest income, realized gain/loss on disposal, and B5.4.6 catch-up P&L are tracked as three genuinely distinct figures, never blended into one "plug" number the way a flat spreadsheet typically does.
- **Multi-currency bonds** — a bond can be denominated in any currency while the EIR engine stays entirely currency-agnostic; a separate translation layer converts to EUR using manually-entered spot rates (no external API — every rate is a deliberate, attributable entry). The projected schedule beyond today assumes a **frozen** rate rather than forecasting one, with interest and pure FX movement reported as two distinct, always-reconciling figures.
- **Full audit trail** — every insert/update/delete on a bond or event is logged with who, when, and the before/after state.
- **Live-formula Excel export** — the entire portfolio exports to a single `.xlsx` with real Excel formulas (not pasted values), including the multi-currency translation columns, so a reviewer can change an assumption in Excel and watch the whole schedule recompute.
- **Portable desktop build** — packaged with PyInstaller into a self-contained Windows executable for non-technical users, with no Python installation required.

## Engineering approach

- **Explicit failure over silent guessing.** A malformed input, a missing exchange rate, an XIRR that fails to converge, an unsupported instrument feature (floating-rate coupons, non-ACT/365 day counts) — every one of these raises a specific, readable error naming exactly what's wrong, instead of falling back to a default value that quietly corrupts a downstream number.
- **Tests document real bugs, not just behavior.** The test suite doesn't just check "does this function return the right number" — each test name states the real-world mistake it exists to catch (a legacy settlement-before-trade-date data error, a leap-day coupon convention that silently dropped payments, a stale UI widget key masking the correct default), so the reasoning survives long after the original bug is forgotten.
- **Domain correctness reviewed by a professional.** Every accounting judgment call — lot vs. blended EIR, catch-up vs. new recognition, how a disposal's realized P&L is derived — was checked against real transaction data by a chartered auditor, not just against my own intuition of "looks right."

## Tech stack

| Layer | Choice |
|---|---|
| UI | Streamlit |
| Data manipulation | pandas |
| Financial calculations | `pyxirr` (XIRR / compound discounting) |
| Persistence | SQLite, hand-written schema with `CHECK` constraints and an idempotent migration path |
| Excel export | XlsxWriter (live formulas, conditional formatting) |
| Charting | Plotly |
| Testing | pytest, 95+ tests covering the accounting engine, database layer, and export logic |
| Packaging | PyInstaller (portable Windows executable) |

## A note on scope

This repository contains the project write-up only — the source lives in a private repository while the accounting logic keeps evolving. If you'd like to see the code (architecture, tests, or a walkthrough), reach out — happy to share.
