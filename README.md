# Climate Physical Risk Quantification

> **Track:** Resume A - Climate & ESG Finance | **Environment:** Python 3.12 (`resume-a`) | **Updated:** 2026-05

Quantifies the physical climate risk exposure of a 10-stock Indian equity portfolio using a TCFD-aligned expected loss framework. Asset locations are mapped to district-level hazard scores, combined with sector-specific vulnerability factors, and aggregated to a portfolio-level Climate VaR under two NGFS warming scenarios.

---

## Methodology

**Framework:** TCFD physical risk, three-factor expected loss model.

```
Expected Loss = Asset Value x Hazard Probability x Vulnerability Factor
```

Transition risk is excluded from scope.

### Portfolio

Ten Indian equities across six sectors: agriculture inputs (UPL, Coromandel International), power generation (NTPC, Adani Green Energy), infrastructure (L&T, IRB Infrastructure), banking with agricultural credit exposure (SBI, Bank of Baroda), irrigation (Jain Irrigation), and coastal real estate (DLF). Covers 25 districts across 10 Indian states.

### Asset Location Mapping

Primary asset locations are estimated based on publicly known operational geographies (headquarters, major plant locations, project sites) and sector-level priors. Locations are not verified against annual report disclosures. Companies with geographically distributed assets are represented by multiple district entries weighted by estimated revenue or asset share.

### Hazard Scoring

District-level ratings (river flood, drought, tropical cyclone, heat stress) sourced from ThinkHazard (GFDRR/World Bank). Qualitative ratings converted to annual exceedance probability proxies:

| Rating | Probability |
|--------|------------|
| High | 0.20 |
| Medium | 0.10 |
| Low | 0.05 |
| Very Low | 0.01 |

### Scenario Adjustment

Under NGFS Current Policies (4 degrees C), hazard probabilities are scaled 1.5x relative to the Orderly Transition (1.5 degrees C) baseline, consistent with NGFS documentation on South Asian extreme event frequency under high-emission pathways.

### Aggregation

Expected loss aggregated from location to company (weighted by asset share) to portfolio (equal-weighted, 10% per company).

---

## Results

<div align="center">

| Scenario | Portfolio Climate VaR |
|----------|----------------------|
| Orderly Transition (1.5C) | **5.97%** of portfolio value |
| Current Policies (4C) | **8.96%** of portfolio value |
| Incremental risk (scenario gap) | **2.99 pp** |

</div>

![Sector Heatmap](outputs/output2_sector_heatmap.png)

![Portfolio Summary](outputs/output3_portfolio_summary.png)

### Company-Level Breakdown

| Company | Sector | Orderly Transition EL (%) | Current Policies EL (%) | Increment (pp) |
|---------|--------|:-------------------------:|:-----------------------:|:--------------:|
| Coromandel International | Agriculture Inputs | 8.96 | 13.44 | 4.48 |
| Adani Green Energy | Power Generation | 7.41 | 11.12 | 3.71 |
| Larsen & Toubro | Infrastructure | 7.04 | 10.56 | 3.52 |
| UPL Ltd | Agriculture Inputs | 6.95 | 10.43 | 3.48 |
| Jain Irrigation Systems | Irrigation / Water | 6.72 | 10.08 | 3.36 |
| DLF Ltd | Real Estate (Coastal) | 6.13 | 9.19 | 3.06 |
| NTPC Ltd | Power Generation | 5.64 | 8.45 | 2.82 |
| IRB Infrastructure | Infrastructure | 4.57 | 6.85 | 2.28 |
| State Bank of India | Banking (Agri Credit) | 3.34 | 5.02 | 1.68 |
| Bank of Baroda | Banking (Agri Credit) | 2.98 | 4.46 | 1.49 |

**Key findings:**
- Agriculture inputs sector carries the highest exposure, driven by coastal Andhra Pradesh location and combined drought-cyclone hazard profile
- Adani Green Energy ranks second; renewable energy assets in Gujarat's cyclone belt are not inherently low physical risk
- Banking sector exposures are lowest, consistent with second-order credit channel rather than direct asset damage
- The 2.99 pp scenario gap represents the quantifiable cost of divergence between a managed transition and a high-emission pathway

---

## Data Sources

| Dataset | Source | Coverage |
|---------|--------|----------|
| ThinkHazard | [thinkhazard.org](https://thinkhazard.org/en/) | District-level, India - flood, drought, cyclone, heat stress |
| NGFS Scenarios | [ngfs.net](https://www.ngfs.net) | Global, 2022 - Orderly Transition and Current Policies pathways |
| IPCC AR6 WG2 | [ipcc.ch](https://www.ipcc.ch/report/ar6/wg2/) | Global - vulnerability coefficients by sector and hazard |
| NSE Annual Reports | [nseindia.com](https://www.nseindia.com) | Company-level - not directly verified; used as reference only |
| CLIMADA ETH | [climada-python.readthedocs.io](https://climada-python.readthedocs.io) | Global - supporting reference for vulnerability ranges |

---

## Limitations

<details>
<summary>Expand</summary>

- **Hazard data:** ThinkHazard ratings are qualitative district-level classes; a rigorous implementation would use return-period curves from CMIP6 or IMD gridded data
- **Asset locations:** Estimated from publicly known operational geographies, not verified against annual report disclosures; actual asset distribution may differ
- **Vulnerability:** Factors are point estimates, not continuous damage functions mapping hazard intensity to loss fraction
- **Scenario scalar:** The 1.5x multiplier is a uniform approximation; CMIP6 outputs show hazard-specific and region-specific multipliers that vary from this estimate
- **Loss correlation:** Company losses are treated as independent; correlation during systemic climate events would increase portfolio tail losses above expected loss estimates
- **Scope:** Transition risk is excluded; a complete TCFD assessment requires both physical and transition risk components
- **Asset value proxy:** Market capitalisation used; replacement or insured value would be more appropriate for physical damage estimation

</details>

---

## References

- TCFD Technical Supplement on Climate Scenario Analysis (2017): [PDF](https://assets.bbhub.io/company/sites/60/2021/03/FINAL-TCFD-Technical-Supplement-062917.pdf)
- NGFS Scenarios for Central Banks and Supervisors (2022): [PDF](https://www.ngfs.net/sites/default/files/medias/documents/ngfs_scenarios_for_central_banks_and_supervisors.pdf)
- IPCC AR6 Working Group II (2022): [ipcc.ch](https://www.ipcc.ch/report/ar6/wg2/)
- ThinkHazard, GFDRR/World Bank: [thinkhazard.org](https://thinkhazard.org/en/)
- CLIMADA Platform, ETH Zurich: [climada-python.readthedocs.io](https://climada-python.readthedocs.io)
