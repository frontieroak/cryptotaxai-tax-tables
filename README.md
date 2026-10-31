# CryptoTaxAI Tax Tables — US federal figures for tax year 2026

The public tax tables and calculation methods behind cryptotaxai.ai's free
crypto tax calculators — published so traders, developers, and researchers
can see exactly which federal figures the calculators use, and check them
against the IRS source documents.

> **Tax year:** 2026 · **Verified:** 2026-10-31, checked line by line against
> the IRS source — [Rev. Proc. 2025-32](https://www.irs.gov/pub/irs-drop/rp-25-32.pdf)
> · **Next update:** after the IRS publishes the tax year 2027 parameters
> (released each Oct–Nov), this repository will be refreshed to the new year.

The live tools that run on these tables:

- [Crypto Tax Calculator](https://cryptotaxai.ai/tools/crypto-tax-calculator) — gain/loss and estimated federal tax on a sale
- [Tax-Loss Harvesting Estimator](https://cryptotaxai.ai/tools/tax-loss-harvesting) — what a harvested loss is worth
- [Staking Income Tracker](https://cryptotaxai.ai/tools/staking-income-tracker) — staking, mining, and airdrop income at receipt
- [Guide: Crypto Capital Gains Tax 2026](https://cryptotaxai.ai/guides/crypto-capital-gains-tax-2026) — the same figures explained in prose

## What's inside (`data/`)

| File | What it is |
| --- | --- |
| `federal-income-tax-brackets-2026.json` / `.csv` | The 2026 ordinary-income brackets (10%–37%) for single, married filing jointly, married filing separately, and head of household. Short-term crypto gains are taxed through these. |
| `capital-gains-thresholds-2026.json` | The 2026 long-term capital-gains thresholds per filing status: the taxable-income ceilings for the 0% and 15% rates (20% above). |
| `standard-deduction-2026.json` | The 2026 standard deduction per filing status — the reference figure that turns gross income into the taxable income every threshold above is measured against. |
| `key-parameters-2026.json` | The other parameters the calculators apply: the 3.8% Net Investment Income Tax thresholds, the $3,000 / $1,500 capital-loss deduction limit and carryforward, and the holding-period rule. |

[`docs/calculation-methods.md`](docs/calculation-methods.md) walks through
the full method — stacking order, the loss-netting order, the NIIT estimate,
and a worked example — including what the estimators deliberately simplify.

## Tax year and sources

All figures are for **tax year 2026** (returns filed in 2027), taken from
the IRS's own publications:

- Brackets, capital-gains thresholds, standard deduction: **IRS Revenue
  Procedure 2025-32**, §§ 4.01, 4.03, 4.14 —
  [irs.gov/pub/irs-drop/rp-25-32.pdf](https://www.irs.gov/pub/irs-drop/rp-25-32.pdf),
  announced in
  [IR-2025-103](https://www.irs.gov/newsroom/irs-releases-tax-inflation-adjustments-for-tax-year-2026-including-amendments-from-the-one-big-beautiful-bill)
- Holding period and capital-loss limit: **IRS Topic No. 409, Capital Gains
  and Losses** — [irs.gov/taxtopics/tc409](https://www.irs.gov/taxtopics/tc409)
- NIIT: **26 U.S.C. § 1411** (statutory thresholds, not inflation-adjusted)

Every JSON file also carries its own `source` and `source_url` fields.

## Disclaimer

Reference data only — **not tax advice**. These tables reproduce
IRS-published figures as used by the CryptoTaxAI estimators on
2026-10-31. If anything in this repository differs from the IRS's official
publications, **the IRS figures control**. Confirm your own situation with
the IRS or a qualified tax professional.

## License

MIT — see [LICENSE](LICENSE). The underlying tax figures are US government
publications.
