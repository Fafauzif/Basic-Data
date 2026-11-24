# Missing, partial and limited sources

Collection date: **2026-11-24** (Asia/Jakarta). Package root: `PT_Basic_Research_Data/`.

**Nothing in the briefed source list was intentionally excluded.** Every briefed source is
represented in the package. This file records, honestly and separately:

* **A.** canonical URLs that failed and the documented public fallback that was used,
* **B.** public interfaces that cannot return records to a non-JavaScript client,
* **C.** transient errors that were resolved on retry (the data is present),
* **D.** structural and format caveats that affect how the data may be used,
* **E.** confirmation that no payload was dropped for size reasons.

Raw machine log: the collection-side failure registry is summarised here; each affected item
also carries the same facts in `MANIFEST.csv` (`coverage` / `note` columns).

---

## A. Canonical URL failed -> documented fallback used (data present, scope reduced)

### A1. LKPP aggregate "Data SiRUP" dataset (brief section 1, SiRUP)

| field | value |
|---|---|
| briefed URL | https://data.lkpp.go.id/en/dataset/data-sirup-sistem-informasi-rencana-umum-pengadaan |
| result | **HTTP 404** (15,866-byte Next.js "Portal Data LKPP" shell) |
| also tried | CKAN API `/api/3/action/package_show?id=...`, `/package_search?q=sirup`, `/package_list` -> **HTTP 404** each; Wayback availability API -> no archived snapshot (one client timeout) |
| why | the portal has migrated off CKAN to a Next.js "Portal Satu Data" whose catalogue (45 datasets, all CSV) does not contain the SiRUP package dataset; `robots.txt` is `User-agent: * / Disallow: /`, so no further crawling was attempted |
| fallback collected | SiRUP's own public 2026 recap **aggregate**: `procurement/sirup/rekap_kldi_2026.json`, `rekap_kldi_2026.html`, `rekap_kldi_2026_total.html`; plus the closest published LKPP aggregate CSV `procurement/sirup/lkpp_portal_satu_data_nilai_perencanaan_dan_realisasi_pbj_2025.csv` (658 K/L/PD rows: RUP value vs realisation, portal resource `fact_dp_2421.csv`) — both labelled **aggregate** |
| status | **partial** — aggregate totals only; the per-package LKPP XLSX dataset was not retrievable from the LKPP data portal. Package-level records were instead collected from SiRUP itself (see A2/B2). |

### A2. Purwakarta FY2026 price-schedule appendix host is dead

| field | value |
|---|---|
| briefed URL | https://news.purwakartakab.go.id/uploads/download/1__PERBUP_SHS_2026.pdf |
| result | DNS **NXDOMAIN** — `getaddrinfo failed` ([Errno 11001]); `curl.exe` exit 6 "Could not resolve host"; https and http both fail |
| fallback collected | identical upload path on the reachable main host: https://purwakartakab.go.id/uploads/download/1__PERBUP_SHS_2026.pdf -> **HTTP 200 application/pdf, 1,350,331 bytes**, 191 pages with a text layer and item-level line items -> `procurement/pricedocs/purwakarta_shs_2026_appendix_candidate.pdf` |
| status | **resolved via fallback**; the brief's "check for actual line items before claiming the appendix" test **passes** (see `purwakarta_pdf_inspection.txt`) |

---

## B. Public interface cannot return records (documented limitation + alternative)

### B1. Kemenperin P3DN/TKDN certificate search (`/sertifikat`)

| field | value |
|---|---|
| URL | https://tkdn.kemenperin.go.id/sertifikat (and `?q=...`) |
| result | **HTTP 200 but `q` is not bound server-side**: the result region always renders the empty-query message; result rows are produced **client-side by a WASM module** (`/pkg/tkdn-web.<hash>.js` + `.wasm`). No public data route: probed `/sertifikat/data`, `/sertifikat.json` (404), `/api/sertifikat`, `/api/sertifikat/cari`, JSON/XHR/HX-Request headers. `/cari?q=` is the site-wide menu/news search only. |
| fallback collected | the site's own public **Unduh TKDN** export: server-rendered `/unduh_preview` (binds `q`/`kb`/`thn`, states exact match count) + `/export_excel` file link -> `regulatory/tkdn_rekapitulasi_tkdn_2026_all_months.csv` (**17,110 rows x 12 cols**) and `regulatory/tkdn_rekapitulasi_tkdn_2026_bahan_peralatan_kesehatan.csv` (**4,532 rows**); keyword lookups `regulatory/tkdn_lookup_*_2026.html` (PT Basic 0 rows; nitril 4; nitrile 3; sarung tangan 5) |
| status | **partial** — the export has **no certificate-number column**, so **no TKDN certificate number is claimed anywhere** in this package. The three `tkdn_cert_lookup_*.html` files are interface shells only and say so. Full detail: `regulatory/tkdn_sertifikat_limitations.txt`. |

### B2. SiRUP cannot scope to hospitals

| field | value |
|---|---|
| interface | https://sirup.inaproc.id/ "Cari Paket - Penyedia" (filters: jenis, metode, pagu, bulan, lokasi, kldi, pdn, ukm) |
| result | **no hospital-type filter exists**; the `kldi` filter returned **0 rows for every tested value/format** (`D109`, `Kab. Purwakarta`, `Purwakarta`), so K/L/D/I scoping is unusable. Hospital units appear only as free text in the `satuanKerja` column. |
| fallback collected | national keyword-scoped **bundle records** for all six briefed keywords plus spelling variants (`nitril` 23, `nitrile` 14, `handscoon` 29, `sarung tangan pemeriksaan` 1, `examination glove` 15, `BMHP` 5,164, `sarung tangan` 395, `glove` 74, `handscun` 1) and one `lokasi`-scoped query (Kab. Purwakarta, `idsLokasi=14766`, keyword `nitril`, 2 rows) -> `procurement/sirup/search_*` |
| status | **partial** — records are national keyword hits, not a hospital-scoped extract; bundle records are labelled as bundle records and hospital names are preserved verbatim in `satuanKerja`. |

---

## C. Transient errors resolved on retry (no data missing)

| item | transient error | resolution |
|---|---|---|
| `regulatory/fda_lza_994454.html` | `short read 31702/8455` (server gzip Content-Length vs decoded bytes) | retried with `Accept-Encoding: identity` -> HTTP 200, file complete |
| `regulatory/tkdn_rekapitulasi_tkdn_2026_all_months.csv` | same gzip/Content-Length short-read (`3482160/985588`) | identity retry -> complete (row count matches the site's stated count) |
| `regulatory/infoalkes_idak_result_baseline_empty_keyword.json` | `validation: zero bytes` (endpoint needs a warmed session) | captured through the warmed session; registered |
| `procurement/hospital-docs/magetan_konsolidasi_pengadaan_rs.pdf` | first GET **HTTP 403** | retried with `Referer` -> HTTP 200, 6,367,235 bytes, `%PDF` valid |
| `procurement/opentender/2023.xlsx` | single-stream transfer kept dropping | 8 concurrent HTTP ranges reassembled; exact Content-Length 42,652,150 and valid XLSX structure confirmed |
| `geospatial/sumatra-261122.osm.pbf` | single-stream transfer stalled; Geofabrik rolled `sumatra-latest` over to `sumatra-261123` mid-collection | pinned the dated snapshot URL `.../sumatra/sumatra-261122.osm.pbf` and downloaded with parallel ranges; byte length checked against the published 283,411,896 |

---

## D. Structural / format caveats

1. **UN Comtrade preview payloads** (`trade/comtrade_idn_*_401512.json`) carry codes but **null description strings**: `partnerDesc`, `reporterDesc`, `cmdDesc` are all `null` in the public preview response. Partner rows are therefore identified by numeric code only (partner `0` = World); no label text was invented. 2025 is the period the API returned at collection time (a partial/current year).
2. **SiRUP HTML fragments**: `rekap_kldi_2026.html`, `rekap_kldi_2026_total.html` and `rekap_instansi_2026_kabupaten_fragment.html` are server-returned table **fragments** (no `<html>` wrapper) — that is how the SiRUP app serves those views, not truncated files.
3. **Purwakarta**: `perbup_purwakarta_47_2025.pdf` is the **eight-page** regulation and is **image-only** (8 image XObjects, 0 extractable text characters), so it contains no item-level prices. The item-level schedule is the separate **191-page** `purwakarta_shs_2026_appendix_candidate.pdf` (text layer 607,515 chars; header `NO KODE BARANG NAMA BARANG SPESIFIKASI SATUAN HARGA (Rp) TKDN (%)`). The `nitril` hit on page 66 is `Acetonitrile HPLC` (a laboratory reagent) — flagged in the transcription.
4. **BPS July 2025 PDF** (`bps_bulletin_impor_juli_2025_official.pdf`) is **AES-encrypted with an empty user password**; pages are readable after `pypdf .decrypt("")`. The saved mirror (`..._mirror.pdf`) is **byte-identical** (sha256 `2946b2f76dab...`), which independently confirms mirror integrity.
5. **TKDN CSVs** carry a publisher encoding artifact (double-encoded en-dash, bytes `c3 a2 e2 82 ac e2 80 9c`); bytes are kept exactly as served, no re-encoding.
6. **INAPROC Katalog Elektronik**: the "Lihat Riwayat" purchase history **is public** (site's own GraphQL `getProductUnitSoldLogPdp`) and returned **7 rows** for the requested product/variant. This is a **scoped single-product collection**, not a national transaction export.
7. **Kemenkes RS Online fund**: the feed is internally inconsistent — `provinsi.jumlah` sums to 3,700 while the `rs` list has **3,295** rows; **16** rows carry corrupt coordinates (e.g. `[0,0]`, swapped lon/lat, a 14-digit value) and are excluded from the point layer but kept in the CSV. The coordinate field is **`[lat, lon]`** (3,279/3,295 pass Indonesia bounds under that reading; the lon/lat reading passes only 2/3,295); GeoJSON is emitted as `[lon, lat]`. `kode` is a 7-digit unique facility code (3,295/3,295 distinct, not the regional code). Detail: `facilities/rs_online_validation.txt`.
8. **Kemenkes Info Alkes** stored values differ by field: `PENDAFTAR = "PT. BASIC INTERNATIONAL SUMATERA"` (with a period) but `PRODUSEN = "PT BASIC INTERNATIONAL SUMATERA"` (no period) — this is why the literal registrant lookup returned 0 rows while the producer lookup returned the one SYNGUARD record (AKD 10903620817, status Berlaku). Recorded in `regulatory/infoalkes_request_log.txt`.
9. **OpenTender registry caveats**: the publication page lists per-year files for 2120, 2025-2012, 2010, 1970 and "date unknown" (**2011 is not listed**); the 2120 entry matches the page's own "date typos up to 3020" caveat. Stated all-time CSV size on the page is "294.0 MB" (rounded); the collected archive is 308,296,024 bytes. `main.csv` is the 26-column hub table with **no** `main_ocid`; all ten child tables carry `main_ocid` (+`main_id`) as the join key.
10. **All-years vs annual OpenTender files**: the all-years archive contains the same 10 tables as the 2023 annual archive, so `2023.xlsx` / `2023.csv.tar.gz` are a convenient annual working copy, not additional years.

---

## E. No payload was dropped for size reasons

An earlier intermediate instruction imposed a 90 MB package cap; that instruction was **withdrawn**
by the user ("do not leave any data out"). Consequences, verified in the final package:

* the **308,296,024-byte** OpenTender all-years archive and the **283,411,896-byte** Sumatra OSM
  PBF are **both present**;
* every other large file is present too (14.26 MB scanned Perbup, 11.35 MB Health Profile PDF,
  8.20 MB BPS bulletin, 42.65 MB 2023 XLSX, ...);
* to keep individual files pushable to GitHub, the two >45 MiB archives were **losslessly split
  into numbered parts** (`.001`, `.002`, ...). Nothing was re-compressed or removed. Reassembly
  instructions and whole-file SHA-256 values are in `SPLITS.csv` and `README.md`.
