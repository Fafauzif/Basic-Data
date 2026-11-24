# PT Basic — Indonesian nitrile examination glove market-entry: public research data package

Collected **2026-11-24** (Asia/Jakarta) for the **Bosshire × PPIT Xiamen Business Case Competition 2026,
East Topic 1: Finance & Business Innovation** — the Indonesian nitrile examination glove market-entry
case for **PT Basic International Sumatera** (SYNGUARD nitrile examination gloves, AKD 10903620817).

This package is **raw public source material only**: collected files, basic format conversions and
integrity checks. It contains **no analysis, no recommendations, no charts and no market estimates** —
those are the competition team's job, and every number in the package is a verbatim copy of what the
publisher served.

* **Package root:** `PT_Basic_Research_Data/`
* **Collection date:** 2026-11-24 (all snapshots are date-stamped in their file names and/or in `MANIFEST.csv`)
* **Contents:** 179 payload files + 5 package-metadata files, **710.5 MiB** total
* **Largest file:** 45 MiB (the two big archives are split into numbered parts — see below)
* **Manifest:** `MANIFEST.csv` (one row per payload: publisher, exact source URL, collection date,
  format, coverage, byte size, SHA-256, original/derived/transcription, licence/attribution)
* **Checksums:** `SHA256SUMS`
* **Gaps & limitations:** `MISSING_SOURCES.md`
* **Split files:** `SPLITS.csv`

---

## 1. What is in the package

| Directory | Files | Size (MiB) | What it holds |
|---|---:|---:|---|
| `procurement/opentender/` | 14 | 359.3 | ICW **OpenTender** OCDS export (all-years `full.csv.tar.gz` in 7 parts + 2023 annual XLSX + 2023 annual CSV + registry/coverage snapshot + schema reports) |
| `procurement/sirup/` | 33 | 5.1 | **SiRUP** (LKPP procurement-plan) 2026 recap aggregates + package records for `nitril`, `nitrile`, `handscoon`, `sarung tangan pemeriksaan`, `examination glove`, `BMHP` and variants |
| `procurement/hospital-docs/` | 3 | 6.1 | Magetan hospital **BMHP consolidation negotiation** PDF + extracted text + `HANDSCOON NITRIL` transcription |
| `procurement/pricedocs/` | 7 | 15.6 | **Purwakarta FY2026 standard-price** regulation (8 pp, scanned) + item-level 191-page schedule PDF + glove-line transcription + JDIH/BPK records |
| `procurement/katalog/` | 5 | 0.1 | **INAPROC Katalog Elektronik** MEDDIS nitrile glove product page + structured offer snapshot + public purchase-history rows + official guide |
| `facilities/` | 9 | 17.2 | **Kemenkes RS Online** national facility feed + CSV/GeoJSON conversions, **Indonesia Health Profile 2025** PDF, **First REIT Annual Report 2025** PDF |
| `geospatial/` | 14 | 270.9 | **OSM Sumatra PBF** (snapshot 2026-11-22, 7 parts), bounded **Overpass** hospital extract for North Sumatra + query + GeoJSON, Kemenkes province GeoJSON, OSM/Geofabrik attribution |
| `trade/` | 13 | 16.9 | **UN Comtrade** imports HS 401512 for 2022-2025 (partner-level), **BPS** import bulletins June 2024 + July 2025 (official PDFs), **WCO** HS 2022 Chapter 40 |
| `regulatory/` | 79 | 19.1 | **Kemenkes Info Alkes** lookups/exports (product, CPAKB, IDAK), **Kemenperin TKDN** scoped exports, **CBP** ruling N354627 PDF, **FDA** CDRH LZA listing snapshots + transcriptions |
| `licenses/` | 2 | 0.1 | ODbL 1.0 text + OpenTender attribution note |
| *(root)* | 5 | 0.1 | `README.md`, `MANIFEST.csv`, `MISSING_SOURCES.md`, `SPLITS.csv`, `SHA256SUMS` |

### Headline facts preserved by the sources (as published, not interpreted)

* **PT Basic / SYNGUARD** — Info Alkes shows one product record: SYNGUARD Nitrile Exam Gloves,
  **AKD 10903620817**, issued 2026-08-19, valid to 2031-08-13, class 1-A, non-electromedical/non-sterile,
  status **Berlaku** (valid), manufacturer PT Basic International Sumatera (KEK Sei Mangkei).
* **FDA** — establishment/registration **FEI 3038284237**, status **Active**, registration-status year 2027,
  proprietary name "Disposable Nitrile gloves", product code **LZA**, class 1, regulation 880.6250;
  13 listings captured.
* **CBP ruling N354627** — "The country of origin of disposable nitrile rubber gloves" (official PDF, 4 pp).
* **Purwakarta FY2026 standard prices** — item-level line on p.85:
  `196 1.1.12.01.02.0003 Sarung Tangan Nitrile CHRONA CARE Nitrile Powder Free Examination Gloves S, M  Box  82.000  25,00`.
* **Magetan hospital negotiation** — p.24 `HANDSCOON NITRIL` rows M and S (qty, catalogue price, negotiated
  price, supplier PT INTISUMBER HASIL SEMPURNA) transcribed verbatim with page/size/pack fields.
* **Indonesia imports, HS 401512** (Comtrade, partner-level, World row preserved): 2022 = 17,562,176;
  2023 = 8,787,767; 2024 = 14,518,325; 2025 = 17,541,431.448 (USD, as returned).
* **BPS national tariff lines**, June 2024 and July 2025: `40151210` and `40151290` transcribed exactly as
  printed (kg and US$ CIF, month + year-to-date), distinct from the HS6 code `401512`.

---

## 2. Large files are split into parts (GitHub-friendly)

Every file in the package is **at most 45 MiB**, so the whole tree can be committed without Git LFS.
Two source archives exceeded that and were split **losslessly** into numbered parts (no recompression):

| Original file | Size | Parts |
|---|---:|---|
| `procurement/opentender/full.csv.tar.gz` | 308,296,024 B | `.001` … `.007` |
| `geospatial/sumatra-261122.osm.pbf` | 283,411,896 B | `.001` … `.007` |

`SPLITS.csv` records, for each split file: original path, original byte size, **original SHA-256**,
part count, per-part limit and the part file names.

### Reassembling on Windows (PowerShell / cmd)

```bat
cd procurement\opentender
cmd /c copy /b full.csv.tar.gz.001+full.csv.tar.gz.002+full.csv.tar.gz.003+full.csv.tar.gz.004+full.csv.tar.gz.005+full.csv.tar.gz.006+full.csv.tar.gz.007 full.csv.tar.gz
certutil -hashfile full.csv.tar.gz SHA256

cd ..\..\geospatial
cmd /c copy /b sumatra-261122.osm.pbf.001+sumatra-261122.osm.pbf.002+sumatra-261122.osm.pbf.003+sumatra-261122.osm.pbf.004+sumatra-261122.osm.pbf.005+sumatra-261122.osm.pbf.006+sumatra-261122.osm.pbf.007 sumatra-261122.osm.pbf
certutil -hashfile sumatra-261122.osm.pbf SHA256
```

### Reassembling on Linux / macOS / Git Bash

```bash
cd procurement/opentender && cat full.csv.tar.gz.00{1,2,3,4,5,6,7} > full.csv.tar.gz && sha256sum full.csv.tar.gz
cd ../../geospatial    && cat sumatra-261122.osm.pbf.00{1,2,3,4,5,6,7} > sumatra-261122.osm.pbf && sha256sum sumatra-261122.osm.pbf
```

Expected whole-file digests (must match `SPLITS.csv`):

```
48aa241ac4ef4344a74404d0f7b53063439bd753b1d110b9537c97d8c2ba67e5  full.csv.tar.gz
17ac853a0efa6172bb55ca4bd68776691b8d19ab833b3aa4cbc9146d9db74612  sumatra-261122.osm.pbf
```

The split was verified before the originals were removed: the concatenated SHA-256 of the parts equals
the original file digest, and the reconstructed byte length equals the published Content-Length.

---

## 3. How the data was collected and verified

* **Transport.** Public CLI/HTTP only (`curl.exe` 8.23, Python 3.14 + `requests`); documented public APIs
  and the publishers' own export/download endpoints. Large transfers were resumable; the two files above
  were fetched with parallel HTTP Range connections because single connections were throughput-capped.
* **Validation performed on the actual bytes** (not on URLs): gzip CRC + tar member listing for the
  OpenTender archives (`full_tar_listing.txt`, 11 members, 1,775,172,100 uncompressed bytes, CRC OK);
  ZIP/CRC + workbook structure for the 2023 XLSX; OSM PBF blob-header check
  (`sumatra_pbf_validation.txt`); `%PDF` magic + page counts for every PDF; JSON parse + schema/row-count
  checks for Comtrade, SiRUP, Info Alkes, RS Online, Overpass and RS Online GeoJSON; CSV column-count and
  row-count checks for every transcription; XLSX/HTML-in-XLS exports identified as such.
* **Cross-checks worth noting.** The July-2025 BPS mirror PDF is **byte-identical** to the official PDF
  (same SHA-256) — independent mirror confirmation. Purwakarta's 8-page regulation is scanned/image-only
  (0 extractable characters), so it was *not* claimed to contain item-level prices; the separate 191-page
  schedule is the one with line items. The OpenTender `main.csv` hub table has no `main_ocid`; the ten
  child tables do (verified in `table_headers.txt`).
* **Bulk vs lookup.** Aggregate tables are labelled **aggregate**, sales/package bundles **bundle
  records**, and hand-extracted values **transcription** — see the `kind` column in `MANIFEST.csv`.
* **No invented data.** Where a public interface cannot return records (TKDN certificate search,
  SiRUP hospital filter) the limitation and the documented fallback are recorded in
  `MISSING_SOURCES.md` instead of guessing values.

### `kind` column meaning

| kind | meaning |
|---|---|
| `original` | the publisher's file, retained as served (or split losslessly, see `SPLITS.csv`) |
| `derived` | produced from an original by format conversion, listing, validation or extraction (CSV/GeoJSON/text) |
| `transcription` | values hand-transcribed from a source page/document, kept verbatim (no recalculation) |

`MANIFEST.csv` includes a `stream` column (collection stream) and a `validation`/`note` column recording
the check that was actually run on each file.

---

## 4. Licences and attribution

| Dataset | Licence / attribution |
|---|---|
| **ICW OpenTender** (via Open Contracting Partnership, publication 101) | **ODbL 1.0** — attribution to ICW/OpenTender and OCP; ODbL text in `licenses/ODbL-1.0.html`, note in `licenses/ATTRIBUTION_opentender.txt` |
| **OpenStreetMap / Geofabrik** | © OpenStreetMap contributors, **ODbL 1.0**; extract © Geofabrik GmbH; `geospatial/OSM_copyright.html` |
| **UN Comtrade** | public API preview data, © United Nations — cite UN Comtrade |
| **BPS (Statistics Indonesia)** | official BPS publication PDFs, quoted with bulletin title and page |
| **WCO** | HS Nomenclature 2022, Chapter 40 (WCO publication) |
| **Kemenkes / Kemenperin / LKPP / SiRUP / INAPROC / Pemkab Purwakarta / Pemkab Magetan** | Indonesian government public information; publisher named per file in `MANIFEST.csv` |
| **U.S. CBP ruling N354627, U.S. FDA CDRH listing** | U.S. Government works / public agency records |

Redistribution is for the competition's research use; respect each publisher's terms. No credentials,
cookies, session tokens or personal contact details are included (RS Online phone fields were dropped).

---

## 5. Gaps, limitations and caveats — read this before using the data

`MISSING_SOURCES.md` is the authoritative list, with real HTTP errors and the fallback used. Summary:

* **LKPP's aggregate "Data SiRUP" XLSX dataset is gone** (portal migrated off CKAN; 404). SiRUP's own
  2026 recap **aggregates** and the closest published LKPP aggregate CSV were collected instead.
* **SiRUP cannot be scoped to hospitals** (no hospital filter; `kldi` filter returns 0 rows). Records are
  national keyword hits with hospital names visible in the `satuanKerja` column.
* **TKDN certificate search is client-side (WASM)** — no public data route. A scoped **Unduh TKDN** export
  (17,110 rows / 4,532 rows) was collected instead; it carries **no certificate numbers**, so no TKDN
  certificate number is claimed.
* **Purwakarta appendix host `news.purwakartakab.go.id` no longer resolves** (DNS NXDOMAIN); the same file
  was retrieved from the main host `purwakartakab.go.id` (HTTP 200, 191 pp, line items confirmed).
* **Comtrade preview** rows have `null` description strings (codes only); 2025 is a partial/current year.
* **RS Online feed** is internally inconsistent (`provinsi.jumlah` 3,700 vs 3,295 `rs` rows; 16 rows with
  corrupt coordinates, kept in CSV but excluded from GeoJSON); coordinate field is `[lat, lon]`.
* **Magetan 403 / FDA gzip short-read / TKDN short-read** were transient and resolved on retry — the files
  are present and complete.
* **Nothing was dropped for size reasons** — the 308 MB OpenTender archive and the 283 MB Sumatra PBF are
  both here (split only). See `MISSING_SOURCES.md` §E.

---

## 6. Reproducing or extending the collection

Every payload row in `MANIFEST.csv` carries the **exact source URL** and collection date, so any item can
be re-fetched or re-checked. `SHA256SUMS` uses the standard `sha256sum` format:

```bash
sha256sum -c SHA256SUMS        # Linux / macOS / Git Bash
```
```powershell
Get-Content SHA256SUMS | ForEach-Object { $h,$p = $_ -split '  ',2; if ((Get-FileHash $p -Algorithm SHA256).Hash.ToLower() -ne $h) { "FAIL $p" } }
```

---

## 7. Pushing this package to GitHub

All files are ≤45 MiB, so a normal push works (no Git LFS needed). The repository is ~710 MiB, so the
initial push takes a while on a slow uplink.

```bash
git init
git add PT_Basic_Research_Data
git commit -m "PT Basic nitrile glove market-entry public data package (2026-11-24)"
git branch -M main
git remote add origin https://github.com/<you>/<repo>.git
git push -u origin main
```

Optional: `.gitattributes` with `*.001 binary` … `*.007 binary` (and `*.pbf.00*`) prevents line-ending
mangling if you ever rewrite text files. The verification commands in §2 confirm reassembly integrity.

---

*Compiled by the data-collection agent, 2026-11-24. Sources are public; see `MISSING_SOURCES.md` for
everything that is partial, limited or replaced by a documented fallback.*
