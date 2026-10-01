# US Tariff Data — HTS, additional duties by country, EU import duties

Clean, machine-readable copies of the **US Harmonized Tariff Schedule (HTS)**, the **chapter 99 additional duties that name a country** (Section 301, 232, 338, the forced-labor duty and the ended IEEPA and Section 122 measures), and **EU import duties by country of origin** — refreshed with every HTS revision.

The official sources publish this as PDFs, a web app and nested JSON. Here it is as flat CSV you can load in one line.

**Browse it online:** every code, rate and country is a page on **[checkdutyrates.com](https://checkdutyrates.com/)** — for example [8507.60 lithium-ion batteries](https://checkdutyrates.com/hs/85076000), [additional duties on goods from China](https://checkdutyrates.com/tariffs/china) or [EU import duties on goods from Japan](https://checkdutyrates.com/eu/from/japan).

Current data: see [`data/VERSION.json`](data/VERSION.json) (HTS revision and the date the EU data was checked).

## Files

| File | Rows | What it is |
|---|---|---|
| [`data/hts.csv`](data/hts.csv) | ~30,000 | Every coded line of the HTS: heading (4 digits), subheading (6), tariff line (8) and statistical line (10) |
| [`data/hts.json`](data/hts.json) | same | The same rows as JSON, with the revision name |
| [`data/additional_duties.csv`](data/additional_duties.csv) | ~380 | Chapter 99 provisions that name a country, with the added percentage and whether they are still in force |
| [`data/eu_duty_by_origin.csv`](data/eu_duty_by_origin.csv) | ~89,000 | EU import duty for each HS subheading, for all countries (MFN) and for 15 major origins |
| [`data/eu_duty_by_country.csv`](data/eu_duty_by_country.csv) | ~90 | Per origin: share of EU tariff codes it ships duty-free, average duty, arrangements used |
| [`datapackage.json`](datapackage.json) | | [Frictionless](https://frictionlessdata.io/) descriptor |

## Columns

### `hts.csv`

| Column | Example | Notes |
|---|---|---|
| `hts_code` | `8507.60.00` | Dotted form as printed in the schedule |
| `digits` | `85076000` | Digits only |
| `chapter` | `85` | |
| `level` | `8` | 4 heading, 6 subheading, 8 tariff line, 10 statistical line |
| `indent` | `1` | Indent in the printed schedule |
| `description` | `Lithium-ion batteries` | The line's own text |
| `full_description` | `Electric storage batteries … > Lithium-ion batteries` | The line's text with every parent, so `Other` lines are readable |
| `unit` | `No.;kg` | Units of quantity, `;`-separated |
| `general_rate` | `3.4%` | Column 1 general (normal trade relations) rate |
| `special_rate` | `Free (A,AU,B,…)` | Rates under trade agreements and preference programs, with program symbols |
| `column2_rate` | `40%` | Column 2 rate |
| `rate_source` | `8507.60.00` | The line the rates are printed on. 10-digit lines inherit from their 8-digit line |

### `additional_duties.csv`

| Column | Notes |
|---|---|
| `country`, `country_slug` | Country named by the provision |
| `heading` | Chapter 99 heading, e.g. `9903.91.03` |
| `added_percent` | Percentage added to the ordinary duty, when the provision states one |
| `rate_text` | The rate as printed, e.g. `The duty provided in the applicable subheading + 25%` |
| `status` | `in force`, or when and why it ended (IEEPA tariffs ended 2026-02-20; Section 122 expired 2026-07-23) |
| `description` | The provision's legal text |

Which products a provision covers is defined in the chapter 99 U.S. notes it cites; [checkdutyrates.com](https://checkdutyrates.com/) resolves that per tariff line and estimates the total duty by origin.

### `eu_duty_by_origin.csv`

| Column | Notes |
|---|---|
| `hs6` | HS subheading, e.g. `8507.60` |
| `origin` | ISO country code, or `ALL (MFN)` for the standard third-country duty |
| `cn_codes` | Number of 8-digit EU Combined Nomenclature codes under the subheading |
| `duty_min`, `duty_max` | Lowest and highest duty across those CN codes, as published (`2.7 %`, `12.8 % + 303.4 EUR / 100 kg`) |
| `basis` | `mfn` standard rate, `pref` a trade agreement or preference scheme, `union` customs union |
| `anti_dumping_possible` | `yes` when an anti-dumping or countervailing duty exists for that origin under the subheading (producer-specific) |
| `additional_duty` | `yes` when an additional duty applies (e.g. 50 % on Russian and Belarusian goods) |

Origins: CN, US, GB, CH, TR, JP, KR, IN, VN, TW, TH, BR, CA, MX, NO.

## Sources and licence

- **US data** comes from the [U.S. International Trade Commission](https://hts.usitc.gov/). Works of the US government are in the public domain (17 U.S.C. § 105); this compilation is released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
- **EU data** is the EU Common Customs Tariff and TARIC as published through the [UK Trade Tariff (XI) service](https://www.trade-tariff.service.gov.uk/xi/find_commodity), under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/). Reuse requires attribution: *Contains public sector information licensed under the Open Government Licence v3.0.*

This is a convenience copy, not a legal reference. Classification and the duty actually owed are decided by customs authorities — confirm with CBP, your national customs office or a licensed customs broker.

## Citation

If you use this data, please link to [checkdutyrates.com](https://checkdutyrates.com/), where it is maintained and browsable:

```
CheckDutyRates (2026). US Tariff Data: HTS, additional duties by country and EU import duties. https://checkdutyrates.com/
```

## Updates

Regenerated from the same pipeline that builds checkdutyrates.com, after each HTS revision (usually monthly) and each refresh of the EU data. Issues and corrections are welcome.
