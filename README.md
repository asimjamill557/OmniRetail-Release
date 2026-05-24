# OmniRetail Product Tiers & Feature Details

This document provides a highly detailed mapping of each product feature, its current implementation status, core files, and standard plan access rules across the active tiers of **OmniRetail Desktop POS**.

---

## 1. Product Tiers Overview

We operate three main subscription tiers natively normalized across client-side Electron structures and PostgreSQL admin portal models:

| Tier | Target Merchant Segment | Active User Seats | Cloud Backups | Advanced Analytics & Tax |
| :--- | :--- | :---: | :---: | :---: |
| **Starter** | Small retail shops, kiosks, and single-register stores | 1 User | ❌ Local Only | ❌ Daily Simple |
| **Pro** | Growing retail, grocery, and wholesaling outlets | 3 Users | ❌ Local Only | ✅ Advanced |
| **Enterprise** | Large retailers, multi-branch operations, and B2B corporate groups | Unlimited | ✅ Automatic | ✅ Full Compliance (FBR/ZATCA) |

> [!NOTE]
> **Legacy "Business" Tier Normalization:** All legacy database records or local SQLite configurations referencing the `business` tier are automatically repaired and self-healed in-memory and in SQLite to `pro` dynamically. They inherit all Pro capabilities seamlessly.

---

## 2. Feature Entitlements Matrix

| Feature Module | Starter Tier | Pro Tier | Enterprise Tier | Core React Screen Files | Licensing Feature Flag |
| :--- | :---: | :---: | :---: | :--- | :--- |
| **Standard POS Billing** | ✅ | ✅ | ✅ | `src/pages/POS.tsx` | `enable_basic_inventory` |
| **Cash Registers & Sales History**| ✅ | ✅ | ✅ | `src/pages/SalesHistory.tsx` | `enable_basic_inventory` |
| **Customer Khata Book (Credit)** | ✅ | ✅ | ✅ | `src/pages/Khata.tsx` | `enable_credit_sales` |
| **Expense Recording** | ✅ | ✅ | ✅ | `src/pages/Expenses.tsx` | `enable_basic_inventory` |
| **Daily Sales Reports (Simple)** | ✅ | ✅ | ✅ | `src/pages/SalesHistory.tsx` | `enable_daily_reports` |
| **Product Master Catalog** | ✅ | ✅ | ✅ | `src/pages/ProductCatalog.tsx` | `enable_basic_inventory` |
| **Local Manual Backups** | ✅ | ✅ | ✅ | `src/pages/settings/Backup.tsx` | Core Capability |
| **B2B Invoicing & Wholesale Rates** | ❌ Gated | ✅ | ✅ | `src/pages/B2BInvoice.tsx` | `enable_wholesale_pricing` |
| **Multi-Unit Inventory Packaging** | ❌ Gated | ✅ | ✅ | `src/pages/ProductCatalog.tsx` | `enable_multi_unit` |
| **Supplier Balances & Procurement** | ❌ Gated | ✅ | ✅ | `src/pages/Suppliers.tsx` | `enable_wholesale_pricing` |
| **Purchase Invoices & History** | ❌ Gated | ✅ | ✅ | `src/pages/Purchases.tsx` | `enable_wholesale_pricing` |
| **Multi-Terminal LAN Sync** | ❌ Gated | ✅ | ✅ | `src/pages/settings/Network.tsx` | `enable_lan_sync` |
| **Advanced Reports & P&L** | ❌ Gated | ✅ | ✅ | `src/pages/Reports.tsx` | `enable_advanced_reports` |
| **Financial Cash & Bank Registers**| ❌ Gated | ✅ | ✅ | `src/pages/CashBank.tsx` | `enable_wholesale_pricing` |
| **Audit Logs Ledger** | ❌ Gated | ✅ | ✅ | `src/pages/settings/AuditLog.tsx` | `enable_advanced_reports` |
| **Automatic Google Drive Sync** | ❌ Gated | ❌ Gated | ✅ | `src/pages/settings/Backup.tsx` | `enable_cloud_sync` |
| **Custom E-Commerce Android App** | ❌ Gated | ❌ Gated | ✅ | *Custom Storefront Build* | `enable_customer_app` |
| **Multi-Branch Analytics Ledger** | ❌ Gated | ❌ Gated | ✅ | *Head Office Dashboard* | `enable_multi_branch` |

---

## 3. Deep-Dive Module Details & Status

### 🛒 Sales & POS Billing
* **Current Status:** **Stable**
* **Capabilities:** Real-time search of products via barcode or name, customer selection, credit payment integration, multiple cash drawers, receipt printing (thermal & standard), cash register tracking, and void orders.
* **Tier Gating Rules:**
  * **Starter:** Standard point-of-sale retail billing only. Single active user session.
  * **Pro:** Adds **Wholesale/Trade rate overrides** in cart, discount overrides, and custom tax status entries.
  * **Enterprise:** Unlocks multidevice POS lane registers syncing instantly over local networks.

### 📝 Customer Khata Book (Credit Sales)
* **Current Status:** **Stable**
* **Capabilities:** Individual customer ledger tracking, automated credit calculation, payment receiving logs, WhatsApp outreach reminders, and aging accounts ledger.
* **Tier Gating Rules:**
  * **All Tiers:** Active across all subscriptions. Basic credit terms and Urdu/English payment reminders are enabled natively for Starter, Pro, and Enterprise.

### 📦 Logistics & Inventory Control
* **Current Status:** **Stable**
* **Capabilities:** Barcode generating, product catalog entry, manual stock adjusting, categories mapping, bulk imports from Excel files, low stock notifications, and barcode-based inventory audits.
* **Tier Gating Rules:**
  * **Starter:** Standard inventory records and daily low-stock counters.
  * **Pro:** Adds **Multi-Unit packaging configurations** (e.g., selling by Carton, Pack, or Piece), bulk CSV imports, and advanced inventory logs.
  * **Enterprise:** Multi-branch stock inventory balancing.

### 🚚 Supplier Management & Procurement
* **Current Status:** **Stable**
* **Capabilities:** Supplier profiles, purchasing invoice entry, automated supplier balance updates, purchase returns tracking, and cost of goods calculations.
* **Tier Gating Rules:**
  * **Starter:** **Locked entirely**. Starter users do not have procurement ledgers or purchase billing enabled.
  * **Pro / Enterprise:** Full procurement system active. Purchases update standard cash/bank ledgers and automatically adjust product average unit cost.

### 📊 Intelligence & Finance (Registers, Expenses & Reports)
* **Current Status:** **Stable**
* **Capabilities:** Cash & bank account books, expense categorizing, profit & loss, sales velocity charts, dead stock logs, low-margin warnings, and tax reports.
* **Tier Gating Rules:**
  * **Starter:** Can record raw expenses, track simple cash registers, and view daily sales history.
  * **Pro / Enterprise:** Full financial accounting active. Unlocks:
    * Cash & Bank transfers.
    * P&L Statements.
    * Dead stock and sales analytics graphs in the Reports tab.
    * User audit trails under the settings menu.

### 💾 Backups & Google Drive Cloud Sync
* **Current Status:** **Stable**
* **Capabilities:** Full database compression via AdmZip, SQLite integrity checks, historical execution logging, and automated cloud syncs.
* **Tier Gating Rules:**
  * **Starter / Pro:** Full access to manual local backups, schedule pickers (daily/weekly/monthly), browse button folder allocations, and manual recovery. The right-hand **Google Drive Cloud Sync** block is masked under a fuchsia lock card.
  * **Enterprise:** Unlocks automated background Google Drive syncing.

---

## 4. Current Enforcement Mechanism

Gating is enforced robustly via a secure local-first validation stack:

1. **Preload IPC Bridge:** The Electron main process parses features from local cached SQLite credentials (`status.features`) and returns them securely to the Vite React frontend on request.
2. **Dynamic Badging:** Sidebars render colored badges (`PRO` or `ENT`) and intercept navigation on restricted nodes to open the **Feature Showcase Modal** containing live preview screenshots and sales WhatsApp pre-filled templates.
3. **`FeatureGuard` Route Shields:** Gated pages are wrapped inside `<FeatureGuard>` components in `App.tsx`, immediately redirecting manual address bar URL hacks back to home.
4. **Offline Resilience:** If internet connection fails, Electron uses the last-validated local SQLite cache, preventing accidental lockouts.

---

*This document is active. Please review the matrix above. Let me know if you would like to shift any specific feature keys between tiers, adjust seat constraints, or refine the locked overlays!*
