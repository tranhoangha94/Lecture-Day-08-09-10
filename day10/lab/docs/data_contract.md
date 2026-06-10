# Data contract — Lab Day 10

> Bắt đầu từ `contracts/data_contract.yaml` — mở rộng và đồng bộ file này.

---

## 1. Nguồn dữ liệu (source map)

| Nguồn | Phương thức ingest | Failure mode chính | Metric / alert |
|-------|-------------------|-------------------|----------------|
| `data/raw/policy_export_dirty.csv` (export KB từ catalog nội bộ) | `etl_pipeline.py run` → `load_raw_csv()` | Duplicate chunk, `doc_id` lạ (`legacy_catalog_*`), ngày `DD/MM/YYYY` | `quarantine_records` tăng; alert nếu > 30% raw |
| `data/docs/*.txt` (canonical policy CS + IT) | Tham chiếu contract; không ingest trực tiếp trong Sprint 1 | Version conflict HR (10 vs 12 ngày), refund stale 14 ngày | `expectation[refund_no_stale_14d_window]` halt; `hits_forbidden` trên eval |

---

## 2. Schema cleaned

| Cột | Kiểu | Bắt buộc | Ghi chú |
|-----|------|----------|---------|
| chunk_id | string | Có | … |
| doc_id | string | Có | … |
| chunk_text | string | Có | … |
| effective_date | date | Có | … |
| exported_at | datetime | Có | … |

---

## 3. Quy tắc quarantine vs drop

> Record bị flag đi đâu? Ai approve merge lại?

---

## 4. Phiên bản & canonical

> Source of truth cho policy refund: file nào / version nào?
