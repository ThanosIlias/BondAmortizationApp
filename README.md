# Bond Analyzer Pro 

A web application for managing a portfolio of bonds, calculating amortization schedules using the Effective Interest Rate (EIR) method, and visualizing financial data. The application is built with Streamlit thus run on your Browser.

## Description

This tool is designed for individuals or small firms that need to track bond investments according to accounting standards (like IFRS 9). It allows users to input bond purchase details, automatically calculates the EIR, generates a full amortization table, and provides insightful visualizations. The entire portfolio can be exported to a single Excel file with dynamic, live formulas, allowing for further analysis or auditing outside the app. It is a demo that is easily scalable to become an app that support a lot of companies. 

## ✨ Features

- **Bond Entry**: Add standard (fixed-maturity) and perpetual bonds to the portfolio.
- **EIR Calculation**: Automatically calculates the Effective Interest Rate (EIR) using the XIRR method, which is crucial for amortized cost accounting.
- **Amortization Schedule**: Generates a detailed amortization table for each bond, showing:
  - Opening Balance
  - P/L (Interest Income calculated via EIR)
  - Cash Flow (Coupon payments)
  - Closing Balance
- **Portfolio Management**: View all bonds in a central dashboard. Each bond can be deleted individually.
- **Data Persistence**: The bond portfolio is saved locally in a SQLite database, so your data persists between sessions.
- **Interactive Charts**:
  - **Amortization Curve**: Visualizes the change in the bond's carrying amount over its life.
  - **Income vs. Cash Flow**: A dual-axis chart comparing the accounting income (P/L) with actual cash receipts.
  - **Portfolio Summary**: High-level charts summarizing the portfolio composition.
- **Dynamic Excel Export**: Export the entire portfolio to a single `.xlsx` file. This is not a static export; the Excel sheet contains **live formulas**, allowing you to change parameters (e.g., nominal value, coupon) and see the schedule update instantly within Excel.

## 🛠️ Tech Stack

- **Framework**: Streamlit
- **Data Manipulation**: Pandas
- **Financial Calculation**: PyXIRR
- **Database**: SQLite (via Python's built-in `sqlite3`)
- **Charting**: Plotly
- **Excel Export**: XlsxWriter

##  Usage

The application is organized into four main sections, accessible from the sidebar menu:

1.  **📝 Εισαγωγή Ομολόγου (Add Bond)**: Fill in the form with the bond's details (name, ISIN, nominal value, price, dates, etc.). There are separate tabs for standard and perpetual bonds.
2.  **📋 Χαρτοφυλάκιο (Portfolio)**: View a summary and a detailed list of all bonds you have added. You can delete bonds from here.
3.  **📈 Γραφήματα (Charts)**: See visual analyses for the entire portfolio and for each individual bond.
4.  **📥 Εξαγωγή Excel (Export Excel)**: Preview the amortization tables and download the complete, formula-driven Excel report.

## 📁 File Structure

- `app.py`: The main Streamlit application file; handles navigation and UI layout.
- `database.py`: Manages all interactions with the SQLite database.
- `bond_math.py`: Contains the core logic for calculating EIR and the amortization schedule.
- `bond_dates.py`: Generates the date sequences for coupon and reporting dates.
- `excel_export.py`: Logic for creating the dynamic Excel file with `xlsxwriter`.
- `bond_ui.py`, `bond_portofolio.py`, `bond_charts.py`: UI components for different pages/sections of the app.
- `test_bonds.py`: Unit tests for the core calculation and data handling logic.
- `setup_app.py`: An optional script for running the app, potentially for a packaged executable.

## Full Code
If you are interested in using the app please contact me. 
