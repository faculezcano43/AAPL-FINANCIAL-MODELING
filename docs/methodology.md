# Methodology

3-statement operating model for Apple Inc. (NASDAQ: AAPL), built off the FY2025
Form 10-K (fiscal year ended September 27, 2025). Historical columns (2024A,
2025A) are hardcoded from the filing; 2026F–2030F are fully formula-driven.

## Revenue

Modeled by category rather than a single blended growth rate, consistent with
how Apple reports segment revenue:

- **iPhone, Mac, iPad, Wearables/Home/Accessories** — each grown off its own
  historical growth assumption (`ASSUMPTIONS!D7:H10`).
- **Services** — grown off its own assumption (`ASSUMPTIONS!D11:H11`), reflecting
  a structurally higher growth rate than hardware.
- Total Products Net Sales and Total Net Sales are sums of the above.

## Margins & Opex

- **Cost of Sales** is split Products vs. Services (different margin
  profiles), each driven by a COGS-as-%-of-category-revenue assumption.
- **R&D** and **SG&A** are each modeled as a % of Total Net Sales.
- **Other Income/(Expense), net** is modeled as a % of revenue rather than a
  computed interest schedule — Apple has not broken out interest expense
  separately on the income statement since FY2021.
- **Tax** is modeled as a flat % of pre-tax income.

## Working Capital

Driven off day-count assumptions in `ASSUMPTIONS!D23:H27`:

| Item | Basis |
|---|---|
| Accounts Receivable | DSO × Total Net Sales |
| Vendor Non-Trade Receivables | Days × Total Net Sales (amounts due from contract manufacturers — unique to Apple's supply chain) |
| Inventory | DIO × Total Cost of Sales |
| Accounts Payable | DPO × Total Cost of Sales |
| Deferred Revenue | Days × Services Revenue (bundled service value recognized over time) |

## PP&E & Depreciation

Roll-forward schedule (`SUPPORTING SCHEDULES` rows 15–19): Opening PP&E +
CapEx (from `ASSUMPTIONS`) − D&A (% of opening PP&E) = Closing PP&E.

## Debt

Roll-forward schedule (`SUPPORTING SCHEDULES` rows 21–24): Opening Debt +
Issuance/(Repayment), net = Closing Debt.

## Equity

- **Common Stock & APIC** rolls forward by adding Share-Based Compensation
  only.
- **Retained Earnings** rolls forward by adding Net Income and subtracting
  Dividends Paid and Share Repurchases (Apple pays a meaningful dividend in
  addition to very large buybacks, so both reduce retained earnings).
- **AOCI** is held flat at the 2025A level — FX translation and pension
  remeasurement effects are not separately forecasted.

## Balance Sheet Plug

`Other Non-current Assets` (row 17) is a balancing plug —
`Total L&E − all other modeled assets` — not an independently forecasted
line. It exists to absorb items that aren't explicitly modeled line-by-line
(e.g., goodwill/intangibles, other minor working-capital items).

**Known limitation:** because it's a plug, the Balance Sheet Check
(row 36) will read 0 even if an upstream formula is wrong — it forces
balance rather than proving the model is correct. Two issues were caught and
fixed during review specifically because of this:

1. The plug formula originally summed a subtotal row twice, silently
   absorbing an error equal to Total Current Assets each period.
2. The forecast's opening cash balance was anchored to the (simplified)
   historical Cash Flow Statement's own closing cash rather than to the
   actual reported cash balance on the Balance Sheet, understating projected
   cash by ~$8.3bn throughout the forecast.

## Cash Flow Statement

- **CFO** = Net Income + D&A + SBC + Δ Net Working Capital (working-capital
  items above only; other minor balance-sheet items are held flat and thus
  don't flow through here).
- **CFI** = CapEx only. Marketable securities (current and non-current) are
  held flat in the forecast, so no investing line is needed for them.
- **CFF** = Debt issuance/(repayment), net + Dividends Paid + Share
  Repurchases.
- Opening cash for the first forecast year is linked to the actual FY2025A
  Balance Sheet cash balance, not to the historical Cash Flow Statement's own
  (simplified) closing cash figure — see Balance Sheet Plug note above.
