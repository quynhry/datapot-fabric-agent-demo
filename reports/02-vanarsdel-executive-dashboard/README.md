# 02 — VanArsdel Executive Dashboard

> Dashboard điều hành theo dõi tình hình kinh doanh VanArsdel: doanh thu thực tế, ngân sách, forecast, và phân tích theo sản phẩm / khu vực / kênh.

| | |
|---|---|
| **Domain** | Retail Sales & Budget |
| **Status** | 🔴 Scoping |
| **Owner** | BA candidate |
| **Audience** | C-level / Sales leadership — theo dõi kinh doanh, ra quyết định phân bổ ngân sách |
| **Semantic model** | `pbip/VanArsdelExecutive.SemanticModel` (PBIP / TMDL, Import) |
| **Report** | `pbip/VanArsdelExecutive.Report` (PBIR) |
| **Refresh** | Manual (demo) |

---

## Thứ tự đọc tài liệu (Reading order)

```
1. dictionary/business-glossary.md        ← Hiểu ngôn ngữ nghiệp vụ VanArsdel
2. dictionary/data-dictionary.md          ← Hiểu cấu trúc & quan hệ dữ liệu
3. dictionary/data-gap-questions.md       ← Điểm mơ hồ cần làm rõ với stakeholder
4. docs/brd.md                            ← Yêu cầu nghiệp vụ, KPI, scope
5. docs/report-spec.md                    ← Đặc tả handoff cho DA
6. docs/wireframe.md                      ← Phác layout các page
```

## Cách tiếp cận

1. **Khám phá dữ liệu** — đọc schema, sample, và M function hints từ `Actuals_Path.txt` / `Budget_Path.txt`.
2. **Dịch sang ngôn ngữ nghiệp vụ** — lập glossary & data dictionary, xác định điểm mơ hồ.
3. **Xác định người dùng & câu hỏi** — từ vai trò → câu hỏi nghiệp vụ → KPI → metric definition.
4. **Đặc tả report** — mapping KPI → page → visual, định nghĩa measure bằng ngôn ngữ business (không DAX).
5. **Wireframe** — phác layout để DA hình dung trước khi code.

## Contents

- `dataset/` — [DATASET-CONTRACT.md](dataset/DATASET-CONTRACT.md)
- `dictionary/` — [business-glossary.md](dictionary/business-glossary.md) · [data-dictionary.md](dictionary/data-dictionary.md) · [data-gap-questions.md](dictionary/data-gap-questions.md)
- `docs/` — [brd.md](docs/brd.md) · [report-spec.md](docs/report-spec.md) · [wireframe.md](docs/wireframe.md)
- `pbip/` — Power BI project *(nice-to-have, scaffold only)*

## Status notes

- [ ] Business glossary — draft
- [ ] Data dictionary — draft
- [ ] Data gap & questions — draft
- [ ] BRD — draft
- [ ] Report spec — draft
- [ ] Wireframe — draft
- [ ] PBIP build — not started
