# How the CryptoTaxAI calculators compute your estimate

This document describes, step by step, the exact method used by the free
calculators at [cryptotaxai.ai](https://cryptotaxai.ai). It is a description
of an **estimator**, not a tax return: the simplifications are listed at the
end, and IRS publications control over anything written here.

All figures are for **tax year 2026** (returns filed in 2027), per IRS
Revenue Procedure 2025-32.

## 1. Gain or loss on a sale

```
proceeds  = sell price × quantity
cost basis = buy price × quantity + fees
gain       = proceeds − cost basis
```

Fees are added to the cost basis, which is how the
[Crypto Tax Calculator](https://cryptotaxai.ai/tools/crypto-tax-calculator)
treats them.

## 2. Holding period: short-term vs long-term

Per IRS Topic No. 409, the holding period begins the day **after** the asset
was acquired. A sale is long-term only if it happens **more than one year**
after the purchase date — selling on the exact one-year anniversary is still
short-term.

- **Short-term** gains are taxed as ordinary income (Section 3).
- **Long-term** gains are taxed at 0% / 15% / 20% (Section 4).

## 3. Short-term gains: stacked through the ordinary brackets

A short-term gain is taxed as if it were the top slice of your income:

```
tax on gain = bracketTax(other income + gain) − bracketTax(other income)
```

`bracketTax` walks the ordinary-income table
([`federal-income-tax-brackets-2026.json`](../data/federal-income-tax-brackets-2026.json))
for your filing status: each dollar is taxed at the rate of the bracket it
falls in (10% up to the first ceiling, 12% up to the next, and so on).

## 4. Long-term gains: stacked through the 0% / 15% / 20% thresholds

Long-term gains sit on top of your other income. The gain fills three
buckets, in order:

1. The part of the gain that fits below the **0% threshold**
   ([`capital-gains-thresholds-2026.json`](../data/capital-gains-thresholds-2026.json))
   is taxed at **0%**.
2. The part below the **15% ceiling** is taxed at **15%**.
3. Everything above is taxed at **20%**.

If your other income alone already exceeds a threshold, that bucket is
already full and the gain flows straight into the next one.

## 5. Net Investment Income Tax (NIIT) estimate

If total income (other income + gain) exceeds the § 1411 threshold for your
filing status, the calculator adds:

```
NIIT = 3.8% × min(gain, total income − threshold)
```

Thresholds: $200,000 single / head of household, $250,000 married filing
jointly, $125,000 married filing separately. These are statutory and are
**not** adjusted for inflation. (The statute applies the tax against
modified adjusted gross income; the estimator uses total income as an
approximation.)

## 6. Losses and tax-loss harvesting

The [Tax-Loss Harvesting Estimator](https://cryptotaxai.ai/tools/tax-loss-harvesting)
nets positions in the IRS order:

1. Losses offset gains of the **same type first** (short-term against
   short-term, long-term against long-term).
2. Remaining losses offset gains of the **opposite type**.
3. A net loss left over can offset up to **$3,000** of ordinary income per
   year (**$1,500** if married filing separately); the rest **carries
   forward** to future years, keeping its short/long character.

The estimated saving is the tax computed with the losses minus the tax
computed without them, run through Sections 3–4 either way.

## 7. Staking, mining, and airdrop income

The [Staking Income Tracker](https://cryptotaxai.ai/tools/staking-income-tracker)
records each reward as ordinary income at receipt:

```
income = quantity received × fair market value per coin (USD) on that date
```

That amount is also your cost basis in the rewarded coins when you later
sell them.

## 8. Worked example (the calculator's built-in BTC preset)

Buy 0.5 BTC at $30,000 on 2023-03-10, sell at $97,500 on 2025-06-20, $40 in
fees, filing single, $85,000 of other income:

| Step | Amount |
| --- | --- |
| Proceeds (0.5 × $97,500) | $48,750 |
| Cost basis (0.5 × $30,000 + $40) | $15,040 |
| Gain | **$33,710** |
| Holding period | More than one year → long-term |
| Stacking | Other income ($85,000) already fills the 0% bucket (threshold $49,450); the whole gain falls in the 15% bucket (ceiling $545,500) |
| Long-term tax (15% × $33,710) | **$5,056.50** |
| NIIT | $0 (total income $118,710 < $200,000) |
| Effective rate on the gain | 15.0% |

The same gain held one year or less would be short-term and cost $7,676.40
under the 2026 ordinary brackets — the holding-period rule is worth real
money.

## 9. What the estimator does *not* do (simplifications)

- **No deduction is applied for you.** Thresholds and brackets apply to
  *taxable* income; the calculators stack your gain on the "other income"
  figure you type in. Enter your income **after** your standard or itemized
  deduction (2026 standard deduction:
  [`standard-deduction-2026.json`](../data/standard-deduction-2026.json))
  for the closest estimate.
- **Federal only.** State and local taxes are not included.
- No special rates (e.g., the 28% collectibles rate), no AMT, no wash-sale
  analysis, and no wallet-by-wallet cost-basis accounting — the calculators
  work one sale (or one set of totals) at a time.
- Estimates only. This is educational material, **not tax advice**.
