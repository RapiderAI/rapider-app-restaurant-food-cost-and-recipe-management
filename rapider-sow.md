<!-- Rapider Official App SOW & Specification | App #175: Restaurant Food Cost & Recipe Management (restaurant-food-cost-and-recipe-management) | Domain: HSP -->

# #175 Restaurant Food Cost & Recipe Management (`restaurant-food-cost-and-recipe-management`) — Scope of Work (SOW)

- **App Number & Name:** `#175` — Restaurant Food Cost & Recipe Management
- **Proposed App Key:** `restaurant-food-cost-and-recipe-management`
- **Suite / Domain Key & Model Prefix:** `HSP` (`Hsp*`)
- **Function Category:** 18. Hospitality, Restaurants & Travel
- **Representative Product (`P1`):** MarginEdge
- **Combined Feature Catalog:** [../combined-features/175-restaurant-food-cost-and-recipe-management-features.md](../combined-features/175-restaurant-food-cost-and-recipe-management-features.md)
- **Target Release Posture:** Culinary Sub-Recipe & Yield Costing, Distributor Invoice Price-Creep Engine, Shelf-to-Sheet Physical Counts, Theoretical vs. Actual (`TvA`) Variance & Menu Engineering Microapplication

---

## 1. Executive Outcome & Target Audience

### 1.1 Business Problem Solved
Restaurants operate on razor-thin 4%–8% net profit margins, yet most chefs and GMs rely on stale static spreadsheets created months ago—missing weekly 12% Sysco/US Foods beef or dairy price spikes, ignoring 30% fish butchery trim loss, counting walk-in coolers on clipboards out of shelf order, and discovering runaway Prime Cost 25 days after month-end. **Rapider Restaurant Food Cost & Recipe Management (`restaurant-food-cost-and-recipe-management`)** connects live distributor invoice line prices (`FinSupplierBill`) to cycle-free nested sub-recipes with exact trim/cook yields, pairs `#170` itemized `PMIX` sales & kitchen waste with blind shelf-to-sheet physical counts to isolate Theoretical vs. Actual (`TvA`) shrinkage down to the ounce, and plots live *Stars/Plowhorses/Puzzles/Dogs* menu engineering matrices natively across Rapider `HSP`, `OPS`, `FIN`, and `HR`.

### 1.2 Target Personas & Progressive Disclosure
- **SME Fast-Path Defaults (Independent Restaurants, Bakeries, Craft Cocktail Bars, Pizzerias):** Start immediately by uploading/ingesting vendor invoices to track live ingredient prices & daily Food/Bev spend %, build core top-20 dish & cocktail recipes with automatic plate-cost % badges, and run 20-minute shelf-to-sheet walk-in/bar mobile counts.
- **Enterprise Progressive Disclosure (Multi-Unit Restaurant Groups, Central Commissaries, Hotel Executive Chefs, F&B Controllers):** Unlock multi-tier nested commissary sub-recipes with raw-to-trimmed-to-cooked yield cascades, automated allergen/nutrition inheritance pushed to `#170`, cheapest-vendor split `OpsPurchaseOrder` generation, blind count high-variance recount gates via Home `WorkItem`s, modifier-aware `TvA` variance decomposition, daily Prime Cost (COGS + Labor %) flash reports, and automated USALI inventory valuation `FinJournalEntry` postings.

### 1.3 Measurable Business Outcomes
- **2.5 to 5.0 Percentage-Point Food & Pour Cost Reduction:** Real-time vendor price-creep alerts + ounce-level `TvA` variance expose over-portioning, unrecorded bar pours, and supplier overcharges within 24–48 hours.
- **Instant Sub-Recipe Cost Cascading:** Updating a single butter, flour, or tenderloin invoice line recalculates every dependent sub-recipe, modifier, and plated dish cost in `< 500ms` with `Decimal128` 6-decimal-place precision.
- **50% Faster Period-End Kitchen & Bar Counts:** Physical walk-path (`Shelf-to-Sheet`) ordering with dual Case + Loose-Unit inputs cuts Sunday night inventory labor in half while feeding `inventory` (`OpsInventoryPosition`) and `acc` (`FinJournalEntry`).

### 1.4 Benchmark Summary (`P1`–`P6`)
Combines the frictionless invoice-to-plate cost cascade and *Stars/Plowhorses* menu engineering of **`P1` MarginEdge**, enterprise `TvA` variance, commissary batching, and daily Prime Cost flash of **`P2` Restaurant365**, multi-vendor usable-unit par ordering and credit-memo tracking of **`P3` MarketMan**, multi-tier trim/cook yield math, batch scaling, and allergen/nutrition inheritance of **`P4` Apicbase**, modifier-aware POS theoretical depletion of **`P5` Oracle MICROS Inventory**, and ERP-native BOM/PO/AP/GL integration of **`P6` Odoo Enterprise**.

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### 2.1 In-Scope Capabilities
1. Culinary ingredient specifications (`HspIngredientSpec`) mapping multiple distributor pack SKUs (`OpsSupplierProfile`) to culinary UoMs with volume-to-weight density and raw-to-usable `trimYieldPct`.
2. Cycle-free nested sub-recipes, modifier micro-recipes, and plated dishes (`HspRecipe`, `HspRecipeComponent`) with cooking shrinkage %, dynamic batch scaling (`0.5x`–`50x`), digital line-cook spec cards, and bottom-up allergen + nutrition rollup.
3. Vendor delivery line-item price capture (`HspVendorPriceObservation`), moving-average/contract price-creep alerts, credit-memo dispute triggers, cheapest-vendor par order guides (`OpsPurchaseOrder`), and verified `FinSupplierBill` handoff.
4. Walk-path ordered Shelf-to-Sheet physical count sheets (`HspKitchenCountSheet`) with blind-count variance gates and prep batch production logging.
5. Period Theoretical vs. Actual (`TvA`) food & pour cost variance (`HspFoodCostVariance`), live *Stars/Plowhorses/Puzzles/Dogs* menu engineering matrix, daily controllable Prime Cost flash, and inventory valuation GL journal export.

### 2.2 Out-of-Scope Capabilities (Delegated to Adjacent Rapider Apps)
- **Live dining-room check entry, multi-station KDS ticket bumping, 3rd-party delivery throttling, and tip-pool calculations:** Owned by `#170 Restaurant Management & Kitchen Operations` (which emits `PMIX` & post-fire waste events to `#175`).
- **Online diner reservations, table floor plans, and SMS waitlists:** Owned by `#171 Restaurant Reservations & Table Management`.
- **Generic warehouse bin/pallet WMS picking, vendor AP ACH check runs, and employee payroll tax filings:** Owned by built `warehouse`, `inventory`, `procurement`, `accpayable`, `acc`, and `payroll`.

### 2.3 Cross-Suite Ownership & Integration Contract

| Domain / App | Owns (Canonical Models) | Integration Contract with `restaurant-food-cost-and-recipe-management` |
| :--- | :--- | :--- |
| **`restaurant-food-cost-and-recipe-management` (`#175` — This App)** | `HspIngredientSpec`, `HspRecipe`, `HspRecipeComponent`, `HspVendorPriceObservation`, `HspKitchenCountSheet`, `HspFoodCostVariance` | Authoritative source of truth for culinary yields, nested recipe costing, vendor price-creep history, shelf-to-sheet counts, `TvA` variance, and menu engineering. |
| **`restaurant-management-and-kitchen-operations` (`#170` — HSP)** | `HspRestaurantOutlet`, `HspMenuItem`, `HspModifierGroup`, `HspDiningCheck` | `#175` consumes `hsp.restaurant.pmix_recorded` & `hsp.restaurant.waste_logged` for `TvA`; `#175` pushes cascaded allergen tags & margin-breach alerts (`hsp.recipe.margin_alert_raised`) to `HspMenuItem`. |
| **`inventory`, `procurement`, `vendors` (`OPS` Suite)** | `OpsInventoryItem`, `OpsInventoryPosition`, `OpsPurchaseOrder`, `OpsSupplierProfile` | `HspIngredientSpec` extends `OpsInventoryItem` and maps `OpsSupplierProfile` packs; par guides create draft `OpsPurchaseOrder`s; finalized counts update `OpsInventoryPosition`. |
| **`accpayable` & `acc` (`FIN` Suite)** | `FinSupplierBill`, `FinJournalEntry` | Verified vendor invoice lines stage `FinSupplierBill` in `accpayable`; approved period counts post inventory valuation/COGS `FinJournalEntry` to `acc`. |
| **`attendance` & `payroll` (`HR` Suite) + Platform** | `HrAttendancePunch`, `HrPayrollRun`, Home `WorkItem`, `projectChannelMessage` | Reads daily labor cost totals for Prime Cost % flash; routes vendor overcharge disputes & blind-count recount gates via Home `WorkItem`s; hosts AI margin playbooks in `projectChannelMessage`. |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture

To comply strictly with the **Rapider Minimum 3 Items Sidebar Rule**, all visible pages are organized into **3 sidebar sections (`Culinary & Recipes`, `Purchasing & Counts`, `Margin & Variance`) containing 3 visible items each (9 visible menu items total)**. Detail views and creation/edit drawers set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | `dashboard` | Prime Cost & Margin Pulse | `Culinary & Recipes` | `true` (Order 1) | KPI cards (`countSWR`): Theoretical vs. Actual Food Cost %, Pour Cost %, Daily Prime Cost %, Active Price-Creep Alerts, Margin-Breached Dishes. |
| 2 | `recipes` | Recipe & Sub-Recipe Book | `Culinary & Recipes` | `true` (Order 2) | Searchable `HspRecipe` catalog (`PLATED_DISH`, `SUB_RECIPE_BATCH`, `MODIFIER_SPEC`, `COCKTAIL`) via `findSWR`, `#activeFilterTagsBar` with `Clear all`, live food-cost % vs. target badge, clickable recipe name -> `recipe-detail`. |
| 3 | `ingredients` | Ingredients, Packs & Yields | `Culinary & Recipes` | `true` (Order 3) | `HspIngredientSpec` table showing AP pack price, `trimYieldPct`, usable EP cost/oz (`Decimal128`), allergen badges, 30-day price trend sparkline. |
| 4 | `vendor-prices` | Invoice Lines & Price Creep | `Purchasing & Counts` | `true` (Order 4) | `<rapider-kanban>` workflow (`EXTRACTED_NEEDS_MAP`, `PRICE_SPIKE_REVIEW`, `CREDIT_DISPUTE_OPEN`, `VERIFIED_PUSHED_AP`) + line ledger (`HspVendorPriceObservation`). |
| 5 | `par-orders` | Smart Par Order Guide | `Purchasing & Counts` | `true` (Order 5) | Multi-vendor usable-unit price comparison matrix (`OpsSupplierProfile` with `<rapider-phone-input>` rep contact), par shortage quantities, one-click draft `OpsPurchaseOrder` push. |
| 6 | `count-sheets` | Shelf-to-Sheet Counts | `Purchasing & Counts` | `true` (Order 6) | `HspKitchenCountSheet` directory (`WALK_IN_COOLER`, `FREEZER`, `DRY_STORAGE`, `BAR_RAIL`), blind-count status, clickable `#CNT-...` -> `count-sheet-detail`. |
| 7 | `tva-variance` | Theoretical vs. Actual (TvA) | `Margin & Variance` | `true` (Order 7) | `HspFoodCostVariance` breakdown by category & top dollar leak ingredients (Actual vs. `#170` `PMIX` Theoretical vs. Logged Waste = Unexplained Shrinkage `\$`). |
| 8 | `menu-engineering` | Menu Engineering Matrix | `Margin & Variance` | `true` (Order 8) | Interactive 4-quadrant matrix (`STAR`, `PLOWHORSE`, `PUZZLE`, `DOG`) + sortable contribution-margin table with What-If price simulator. |
| 9 | `cogs-gl-close` | COGS Valuation & GL Sync | `Margin & Variance` | `true` (Order 9) | Beginning + Purchases - Ending COGS summary by USALI category (`Food`, `Liquor`, `Wine`, `Beer`), `FinSupplierBill` & `FinJournalEntry` sync audit. |
| 10 | `recipe-detail` | Culinary Spec Card & Cost 360 | *(Hidden)* | `false` | Full `HspRecipe` workspace (`?id=...`): interactive batch scaler (`1x`, `4x`, `10x`), nested `HspRecipeComponent` tree, AP/EP yield breakdown, plating instructions, inherited allergens/kcal, AI *Plowhorse* tips (`projectChannelMessage`), `Back to Recipes`. |
| 11 | `count-sheet-detail` | Mobile Walk-Path Counter | *(Hidden)* | `false` | Ordered storage-zone entry grid (`?id=...`) accepting `Cases + Loose Units`, blind-audit recount warnings, supervisor sign-off & `Back to Count Sheets`. |
| 12 | `recipe-component-drawer` | Create Recipe / Add Ingredient | *(Hidden)* | `false` | Sliding drawer enforcing cycle-free sub-recipe selection, UoM conversion preview, `Decimal128` EP cost calculation, `try/catch/finally` save reset, auto-close. |
| 13 | `invoice-line-drawer` | Map SKU / Flag Price Dispute | *(Hidden)* | `false` | Sliding drawer mapping vendor invoice SKU to `HspIngredientSpec` or raising a short-ship/price-spike Home `WorkItem` dispute before `FinSupplierBill` posting. |

---

## 4. Canonical Data Models (`Hsp*` — 6 Models)

All models omit `projectId`, use platform UUIDs (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx` in seed data), enforce optimistic `version`, and store all unit costs (6-decimal precision), yield ratios, valuations, and variances as `Decimal128`.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`HspIngredientSpec`** | `code` (String, unique), `name` (String), `usaliCategory` (`FOOD_PROTEIN`, `FOOD_PRODUCE`, `FOOD_DAIRY`, `FOOD_DRY`, `LIQUOR`, `WINE`, `BEER`, `NA_BEV`, `PACKAGING`), `baseRecipeUom` (`GRAM`, `ML`, `EACH`), `trimYieldPct` (`Decimal128`, `1..100`), `latestApCostPerBaseUom` (`Decimal128`), `effectiveEpCostPerBaseUom` (`Decimal128`), `movingAvg30dApCost` (`Decimal128`), `priceSpikeThresholdPct` (`Decimal128`), `allergens` (String[]), `kcalPer100g` (`Decimal128`), `vendorPacks` (JSON array of `{ supplierId, vendorSku, packDesc, baseUomQtyPerPack: Decimal128, lastPackPrice: Decimal128, repPhone }`) | `inventoryItemId` -> `OpsInventoryItem.id` (1:1), `preferredSupplierId` -> `OpsSupplierProfile.id` | `effectiveEpCostPerBaseUom == latestApCostPerBaseUom / (trimYieldPct / 100)`; `0 < trimYieldPct <= 100`. |
| **`HspRecipe`** | `recipeCode` (String, unique), `name` (String), `recipeKind` (`PLATED_DISH`, `SUB_RECIPE_BATCH`, `MODIFIER_SPEC`, `COCKTAIL`), `batchYieldQty` (`Decimal128`), `batchYieldUom` (String), `portionsPerBatch` (`Decimal128`), `cookShrinkagePct` (`Decimal128`, `0..99`), `totalBatchCost` (`Decimal128`), `costPerPortion` (`Decimal128`), `targetFoodCostPct` (`Decimal128`), `currentMenuPrice` (`Decimal128`, nullable), `actualFoodCostPct` (`Decimal128`, nullable), `contributionMargin` (`Decimal128`, nullable), `engineeringQuadrant` (`STAR`, `PLOWHORSE`, `PUZZLE`, `DOG`, `UNRANKED`), `inheritedAllergens` (String[]), `prepStepsMarkdown` (Text), `shelfLifeHours` (Int), `status` (`DRAFT`, `ACTIVE_PUBLISHED`, `SANDBOX_SIMULATION`, `ARCHIVED`) | `outletId` -> `HspRestaurantOutlet.id` (`#170`), `linkedMenuItemId` -> `HspMenuItem.id` (`#170`, optional) | **Cycle-Free DAG Invariant:** A sub-recipe may never directly or transitively reference itself; `costPerPortion == totalBatchCost / portionsPerBatch`; `inheritedAllergens` equals union of all child `HspRecipeComponent` allergens. |
| **`HspRecipeComponent`** | `componentType` (`RAW_INGREDIENT`, `NESTED_SUB_RECIPE`), `quantity` (`Decimal128`), `uom` (String), `overrideTrimYieldPct` (`Decimal128`, nullable), `extendedComponentCost` (`Decimal128`), `sortOrder` (Int), `prepNote` (String) | `parentRecipeId` -> `HspRecipe.id`, `ingredientSpecId` -> `HspIngredientSpec.id` (nullable), `childSubRecipeId` -> `HspRecipe.id` (nullable) | Exactly one of `ingredientSpecId` or `childSubRecipeId` is non-null; cost updates cascade bottom-up automatically. |
| **`HspVendorPriceObservation`** | `invoiceNumber` (String), `deliveryDate` (Date), `vendorSku` (String), `rawDescription` (String), `packsReceived` (`Decimal128`), `catchWeightTotal` (`Decimal128`, nullable), `packUnitPrice` (`Decimal128`), `computedBaseUomCost` (`Decimal128`), `priceDeltaPctVsAvg` (`Decimal128`), `disputeCreditRequestedAmount` (`Decimal128`), `status` (`EXTRACTED_NEEDS_MAP`, `PRICE_SPIKE_REVIEW`, `CREDIT_DISPUTE_OPEN`, `VERIFIED_PUSHED_AP`) | `outletId` -> `HspRestaurantOutlet.id`, `supplierId` -> `OpsSupplierProfile.id`, `mappedIngredientSpecId` -> `HspIngredientSpec.id`, `supplierBillId` -> `FinSupplierBill.id` (`accpayable`) | If `priceDeltaPctVsAvg > priceSpikeThresholdPct`, transitions to `PRICE_SPIKE_REVIEW` and alerts Chef/GM; `VERIFIED_PUSHED_AP` updates `HspIngredientSpec.latestApCostPerBaseUom` and cascades dependent `HspRecipe` costs. |
| **`HspKitchenCountSheet`** | `countCode` (String, unique), `countPeriodDate` (Date), `storageZone` (`WALK_IN_COOLER`, `FREEZER`, `DRY_STORAGE`, `BAR_SPEED_RAIL`, `WINE_CELLAR`, `ALL_ZONES`), `isBlindCount` (Boolean), `lines` (JSON array of `{ shelfOrder, ingredientSpecId, fullPacksCount: Decimal128, looseUomQty: Decimal128, unitCostSnapshot: Decimal128, extendedValue: Decimal128, recountFlagged: Boolean }`), `totalCountValuation` (`Decimal128`), `status` (`OPEN_COUNTING`, `RECOUNT_REQUIRED`, `APPROVED_LOCKED`) | `outletId` -> `HspRestaurantOutlet.id`, `countedByEmployeeId` -> `HrEmployee.id`, `glJournalEntryId` -> `FinJournalEntry.id` | When `isBlindCount == true`, API masks expected theoretical stock until line entry is submitted; immutable once `APPROVED_LOCKED`. |
| **`HspFoodCostVariance`** | `periodStartDate` (Date), `periodEndDate` (Date), `beginningInventoryValue` (`Decimal128`), `purchasesNetValue` (`Decimal128`), `endingInventoryValue` (`Decimal128`), `actualCogsAmount` (`Decimal128`), `theoreticalCogsAmount` (`Decimal128`), `loggedWasteAmount` (`Decimal128`), `unexplainedVarianceAmount` (`Decimal128`), `netFbSalesAmount` (`Decimal128`), `actualCogsPct` (`Decimal128`), `theoreticalCogsPct` (`Decimal128`), `topVarianceItems` (JSON array), `status` (`COMPUTED_DRAFT`, `PUBLISHED_CLOSED`) | `outletId` -> `HspRestaurantOutlet.id`, `openingCountSheetId` -> `HspKitchenCountSheet.id`, `closingCountSheetId` -> `HspKitchenCountSheet.id` | `actualCogsAmount == beginningInventoryValue + purchasesNetValue - endingInventoryValue`; `unexplainedVarianceAmount == actualCogsAmount - theoreticalCogsAmount - loggedWasteAmount`. |

---

## 5. Core End-to-End User Journeys

### Journey 1: Supplier Price-Creep Ingestion -> Instant Sub-Recipe & Plate Margin Alert
1. A Thursday delivery invoice from Sysco (`INV-99812`) arrives on `#/vendor-prices` showing Heavy Cream up `+16.4%` and Prime Beef Chuck up `+11.2%` (`HspVendorPriceObservation`).
2. Mapping and verifying the invoice updates `HspIngredientSpec.latestApCostPerBaseUom` (`Decimal128`), stages `FinSupplierBill` in `accpayable`, and cascades instantaneously through *House Burger Grind Batch* (`SUB_RECIPE_BATCH`) -> *Double Smash Burger* (`PLATED_DISH`), pushing its food-cost % from `28.4%` to `31.9%` (breaching its `30.0%` target).
3. `#175` highlights *Double Smash Burger* as a `PLOWHORSE` margin breach on `#/dashboard` and `#/menu-engineering`, allowing the Chef/GM to test a `+\$1.00` price adjustment or `0.5 oz` cheese tweak in the What-If sandbox before publishing to `#170`.

### Journey 2: Short-Ship & Contract Overcharge Dispute via Rapider Home `WorkItem`
1. Dockside receiving notices `INV-99812` billed `40 lbs` of Halibut at `\$19.50/lb` (above the `\$16.75/lb` contracted cap, and actual scale weight was `36.5 lbs`).
2. In `invoice-line-drawer`, the Sous-Chef logs `catchWeightTotal = 36.5` and requests a `\$168.75` vendor credit (`CREDIT_DISPUTE_OPEN`), creating a Rapider Home `WorkItem` for AP / Supplier Rep follow-up (`<rapider-phone-input>`) so `accpayable` withholds the disputed `\$168.75` until the distributor credit memo arrives.

### Journey 3: Sunday Night Blind Shelf-to-Sheet Count & High-Variance Recount Gate
1. At Sunday `22:30` close, the Kitchen Lead opens `#CNT-2026-1029` on `#/count-sheet-detail` (`isBlindCount = true`), walking Shelf 1 -> Shelf 8 in exact physical order and entering `3 Cases + 2 Loose Ribeyes` without seeing theoretical computer stock.
2. Upon submission, `#175` detects a `-\$490.00` discrepancy on Ribeyes (`> \$250.00` threshold), automatically flipping status to `RECOUNT_REQUIRED` and spawning a GM Home `WorkItem`.
3. Recount discovers `1 Case` stored in the secondary meat aging drawer; updating the count clears the variance, locks `APPROVED_LOCKED`, updates `OpsInventoryPosition` in `inventory`, and posts the USALI COGS `FinJournalEntry` to `acc`.

### Journey 4: Theoretical vs. Actual (`TvA`) Variance & Grounded AI Root-Cause Investigation
1. On `#/tva-variance`, the Bar & Kitchen Manager reviews weekly `HspFoodCostVariance`: `Actual Pour Cost = 22.8%` vs. `Theoretical Pour Cost = 18.9%`, with `-\$385.00` `unexplainedVarianceAmount` concentrated in Repo Tequila (`loggedWasteAmount = \$24.00` spilled).
2. In the linked `projectChannelMessage` thread, the AI Variance Investigator cites `#170` `PMIX` (`312 Spicy Margaritas sold` at `2.0 fl_oz` spec = `624 fl_oz` theoretical vs. `768 fl_oz` actual bottle depletion), pinpointing free-hand `2.5 fl_oz` over-pouring on the service bar well.

### Journey 5: Multi-Filter Recipe Search & Click-Through State Restoration
1. On `#/recipes`, the Executive Chef searches `Sauce` and filters `Kind: "SUB_RECIPE_BATCH"`, clicks *House Marinara Batch* to open `#/recipe-detail?id=...` (scaling the card to `4x` for morning prep and checking inherited allergens), and clicks **Back to Recipe Book**—returning cleanly to `#/recipes` with `#activeFilterTagsBar` chips preserved.

---

## 6. Consolidated Feature Priorities & Work Packages

### `P0` Foundation Work Packages (Mandatory MVP Launch)
- **WP-P0-1: Ingredients, Vendor Packs & Trim/Cook Yield Math (`RFC-001`, `RFC-002`):** `HspIngredientSpec` (`#/ingredients`), `OpsInventoryItem` / `OpsSupplierProfile` pack mapping, `Decimal128` AP-to-EP `trimYieldPct`, and volume-weight conversions.
- **WP-P0-2: Nested Sub-Recipes, Modifier Specs & Line-Cook Cards (`RFC-004`, `RFC-005`, `RFC-006`):** `HspRecipe`, `HspRecipeComponent` (`#/recipes`, `#/recipe-detail`, `recipe-component-drawer`), cycle-free DAG check, instant cost cascading, and interactive batch scaling.
- **WP-P0-3: Vendor Invoice Price-Creep Engine & AP Handoff (`RFC-008`, `RFC-009`):** `HspVendorPriceObservation` (`#/vendor-prices`, `invoice-line-drawer`), `<rapider-kanban>` review stages, price-spike alerts, Home `WorkItem` credit disputes, and `FinSupplierBill` staging.
- **WP-P0-4: Shelf-to-Sheet Counts, `TvA` Variance & Menu Engineering (`RFC-012`, `RFC-013`, `RFC-014`, `RFC-016`):** `HspKitchenCountSheet` (`#/count-sheets`, `#/count-sheet-detail`), `HspFoodCostVariance` (`#/tva-variance`) consuming `#170` `PMIX` + waste, and `#/menu-engineering` (*Stars/Plowhorses/Puzzles/Dogs*).

### `P1` Scale & Enterprise Work Packages
- **WP-P1-1: Allergen/Nutrition Rollup, Prep Batches & Par Split-POs (`RFC-003`, `RFC-007`, `RFC-010`, `RFC-011`):** Bottom-up allergen push to `#170`, commissary/prep batch stock moves, `#/par-orders` cheapest-vendor `OpsPurchaseOrder` builder, and dockside catch-weight/temp receiving.
- **WP-P1-2: Blind-Count Audits, Prime Cost Flash, What-If Sandbox & GL Close (`RFC-015`, `RFC-017`, `RFC-018`, `RFC-019`, `RFC-020`):** Blind count masking + Home `WorkItem` recount gate, target food-cost breach alerts, daily COGS + `HR` Labor Prime Cost %, recipe simulation clones, and `#/cogs-gl-close` `FinJournalEntry` sync.

### `P2` Adjacent & Grounded AI Work Packages
- **WP-P2-1: AI Invoice SKU Matcher & `TvA` Portioning Investigator (`RFC-021`, `RFC-022`):** Human-confirmed vendor SKU mapping suggestions and grounded `TvA` variance root-cause briefs in `projectChannelMessage`.
- **WP-P2-2: AI *Plowhorse*/*Puzzle* Playbooks & Zero-Waste Trim Ideation (`RFC-023`, `RFC-024`):** Margin optimization recommendations and trim cross-utilization ideas.

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **Line Cook / Prep Cook** | View `#/recipe-detail` digital spec cards, portion photos, allergens, and interactive batch scaler; enter assigned `HspKitchenCountSheet` shelf counts. | Blind counts hide expected stock quantities; cooks see prep specs without restaurant P&L profit margins, `HR` labor costs, or AP bank details. |
| **Sous-Chef / Bar Manager** | Create/edit draft `HspRecipe`s & `HspIngredientSpec`s; map `HspVendorPriceObservation` SKUs; flag short-ship disputes; generate draft `OpsPurchaseOrder`s. | Cannot bypass high-variance blind recount gates or publish `FinJournalEntry` COGS valuations without GM/Controller approval. |
| **Executive Chef / Restaurant GM** | Publish `ACTIVE_PUBLISHED` recipes & allergen cascades to `#170`; approve recount & vendor-dispute Home `WorkItem`s; run What-If menu engineering updates. | Cannot alter historical `APPROVED_LOCKED` count sheets or closed `PUBLISHED_CLOSED` `HspFoodCostVariance` snapshots. |
| **F&B Controller / Accountant** | Verify `FinSupplierBill` price tolerances, audit multi-outlet `TvA` & Prime Cost, lock period `HspFoodCostVariance`, and post `FinJournalEntry` to `acc`. | Full immutable audit trail across all cost overrides, inventory valuation adjustments, and GL postings. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### 8.1 Guarded Domain Commands
Every mutating command enforces `{ idempotencyKey, correlationId, expectedVersion }`:
- `hsp.foodcost.upsertRecipeAndCascade` — Validates cycle-free sub-recipe DAG, computes exact `Decimal128` AP-to-EP trim & cook shrinkage costs across all parent recipes, updates `inheritedAllergens`, and emits `hsp.recipe.margin_alert_raised` if `actualFoodCostPct > targetFoodCostPct`.
- `hsp.foodcost.verifyVendorInvoiceLines` — Updates `HspIngredientSpec.latestApCostPerBaseUom`, triggers recipe cost cascade, stages `FinSupplierBill` in `accpayable`, or creates a dispute Home `WorkItem` when `CREDIT_DISPUTE_OPEN`.
- `hsp.foodcost.submitAndAuditCountSheet` — Computes `Decimal128` extended values; if any item exceeds variance threshold under `isBlindCount`, sets `RECOUNT_REQUIRED` + creates Home `WorkItem`; upon approval, updates `OpsInventoryPosition`.
- `hsp.foodcost.computePeriodTvAAndPostGl` — Aggregates `#170` `PMIX` theoretical usage + logged waste against `Actual Usage`, persists `HspFoodCostVariance`, refreshes `engineeringQuadrant` (`STAR`/`PLOWHORSE`/`PUZZLE`/`DOG`), and emits `FinJournalEntry` to `acc`.

### 8.2 Published & Consumed Kafka Domain Events
- **Published:** `hsp.recipe.cost_cascaded`, `hsp.recipe.allergens_updated`, `hsp.recipe.margin_alert_raised`, `hsp.foodcost.price_spike_detected`, `hsp.foodcost.tva_published`.
- **Consumed:** `hsp.restaurant.pmix_recorded` & `hsp.restaurant.waste_logged` (from `#170`), `ops.purchase_order.received` (from `procurement`), `fin.supplier_bill.posted` (from `accpayable`).

### 8.3 `requiredWorkflows` (Declared in `app.manifest.json`)
1. `wf-hsp-invoice-price-cascade-and-spike-check` — Recomputes dependent sub-recipes/dishes on every verified `HspVendorPriceObservation` and raises price-spike/margin-breach alerts (`RFC-004`, `RFC-009`, `RFC-017`).
2. `wf-hsp-allergen-sync-to-menus` — Pushes updated `inheritedAllergens` onto linked `#170` `HspMenuItem`s (`RFC-003`).
3. `wf-hsp-blind-count-recount-workitem` — Routes high-dollar blind count discrepancies and vendor short-ship credit disputes via Rapider Home `WorkItem`s (`RFC-009`, `RFC-015`).
4. `wf-hsp-weekly-tva-and-menu-quadrant-refresh` — Computes weekly `TvA` variance, Prime Cost flash, and *Stars/Plowhorses/Puzzles/Dogs* classifications (`RFC-013`, `RFC-016`, `RFC-018`).

### 8.4 Grounded Rapider AI Guardrails
- AI Invoice SKU Matcher, `TvA` Investigator, and Menu Optimization Playbooks cite exact `HspIngredientSpec`, `HspRecipe`, `HspVendorPriceObservation`, and `HspFoodCostVariance` IDs within RBAC scope.
- AI **never** auto-approves disputed supplier invoices, never alters published `#170` customer menu prices, never changes allergen declarations without Chef verification, and never posts `FinJournalEntry` adjustments autonomously.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **Proprietary Culinary IP Protection:** Signature recipe ratios, house spice blends, and commissary formulas are strictly tenant-isolated and role-gated (preventing unauthorized bulk CSV export of proprietary culinary IP).
- **FDA Menu Labeling & EU 1169/2011 Allergen Traceability:** Every allergen/calorie rollup preserves versioned ingredient lineage for regulatory inspection.
- **GAAP / USALI Inventory Valuation Integrity:** Supports consistent FIFO / Latest-Invoice / Moving-Average valuation snapshots per `FinLegalEntity` policy, with append-only `APPROVED_LOCKED` count sheets.
- **Walk-In Cooler & Kitchen Tablet Ergonomics:** WCAG 2.2 AA compliant, large tap targets for cold-room gloved hands, and full dark/light theme parity across `1440px` controller desks, `768px` prep-line tablets, and `390px` count-sheet phones.

---

## 10. Deterministic Acceptance Criteria

1. **Sidebar Minimum-3-Items Compliance:** `app.manifest.json` defines 3 visible sidebar sections (`Culinary & Recipes`, `Purchasing & Counts`, `Margin & Variance`) containing 3 visible items each (`displayOnMenu: true`); detail and drawer routes set `displayOnMenu: false`.
2. **Sub-Recipe Cycle-Free DAG Guard:** Attempting to add Sub-Recipe `A` into Sub-Recipe `B` when `B` is already a direct or transitive component of `A` is deterministically rejected with a `422 Unprocessable Entity` cycle error.
3. **Exact `Decimal128` Trim Yield & Cascaded Costing:** An ingredient at `\$10.00/lb` AP with `trimYieldPct == 80.00%` computes `effectiveEpCost == \$12.50/lb` in `Decimal128`, and updating its invoice price instantaneously recalculates all parent sub-recipes and plated dishes.
4. **Bottom-Up Allergen Inheritance:** Adding `PEANUT` to a raw `HspIngredientSpec` automatically appends `PEANUT` to `inheritedAllergens` on every parent sub-recipe and plated `HspRecipe`.
5. **Blind Count Masking & High-Variance Recount Gate:** Querying an `OPEN_COUNTING` `HspKitchenCountSheet` with `isBlindCount == true` as a Line Cook omits expected stock quantities; submitting a discrepancy above threshold sets `RECOUNT_REQUIRED` and creates a Rapider Home `WorkItem`.
6. **Deterministic `TvA` Variance Conservation:** `HspFoodCostVariance` satisfies `actualCogsAmount == beginningInventoryValue + purchasesNetValue - endingInventoryValue` and `unexplainedVarianceAmount == actualCogsAmount - theoreticalCogsAmount - loggedWasteAmount` to `0.01` `Decimal128` precision.
7. **Menu Engineering Quadrant Classification:** Dishes above category mean popularity and above category mean `contributionMargin` classify deterministically as `STAR` (and correspondingly `PLOWHORSE`, `PUZZLE`, `DOG`).
8. **Custom Elements Adoption:** Supplier rep phone inputs use `<rapider-phone-input>` and `#/vendor-prices` renders invoice review stages via `<rapider-kanban>`.
9. **SWR Clean Query & Optimistic UI Standard:** Initial reads invoke `findSWR` / `countSWR` without `isDeleted` query filters, optimistically update local state + clear SWR cache on drawer save/delete, and reset save buttons inside `finally`.
10. **Table Action & Click-Through Navigation Standard:** Data tables omit standalone `View` buttons; clicking Recipe Name or `#CNT-...` navigates to `#/recipe-detail?id=...` or `#/count-sheet-detail?id=...` without `<base href>` 404 hijacking, and `Back` restores filter state.
11. **Search, `#activeFilterTagsBar` & `Clear all`:** Filtering recipes, ingredients, or invoice lines renders HTML-escaped chips in `#activeFilterTagsBar` with `✕` dismissal and `Clear all` reset.
12. **Theme & Button Contrast Compliance:** All 13 routes pass `validate_theme` in dark and light modes with `#ffffff !important` text on primary and destructive buttons and zero native `window.alert`/`confirm`/`prompt`.
