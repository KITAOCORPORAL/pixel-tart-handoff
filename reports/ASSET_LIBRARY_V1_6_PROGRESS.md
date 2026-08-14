# Asset Library V1.6 Development Preview

Branch: `feature/asset-library-v1`

## Acceptance fields

- `visual_analysis_ui_complete=false` — the real foreground evidence set is not complete yet.
- `analysis_pipeline_performance_verified=true`
- `color_management_reference_verified=false` — there is no certified non-sRGB ICC fixture with an independent converted-RGB oracle.
- `raw_visual_proxy_verified=false` — there is no reliable program-generated RAW/DNG embedded-preview fixture and the Preview decoder still rejects RAW.
- `visual_filter_complete=false` — pending final foreground behavior verification.
- `visual_smart_folder_complete=false` — pending final foreground behavior verification.
- `dominant_color_search_complete=false` — pending final foreground behavior verification.
- `palette_similarity_complete=false` — pending final foreground behavior verification.
- `visual_similarity_complete=false` — pending final foreground behavior verification.
- `similarity_score_explainable=false` — pending final foreground behavior verification.
- `batch_visual_analysis_complete=false` — pending final foreground behavior verification.
- `visual_query_100k_test=false` — pending final main-suite result.
- `similarity_100k_candidate_test=false` — pending final main-suite result.
- `visual_search_ui_review_complete=false`
- `P0Merged=false`
- `RCGenerated=false`
- `UserVerified=false`

## Phase 0 full-pipeline acceptance

The acceptance input contains 1,003 newly encoded synthetic JPEGs: three UI reference fixtures and 1,000 performance fixtures. It contains no customer media, copied EXIF/XMP/GPS metadata, source paths, ICC claim, or RAW claim. Every runtime path is an explicit child of the Windows system temporary directory and is not committed.

The full path under test is:

`generated JPEG → production WPF decoder → visual analysis → SQLite analysis cache → canonical visual feature rows → Inspector read`

The production decoder is internal. The isolated acceptance runner invokes only that boundary through reflection because acceptance tooling is not allowed to change Preview/Core visibility; all later stages call public production services directly.

| Cohort | Cold total | Cold decode | Cold analysis/cache/SQLite | Cold Inspector | Cold hit/miss | Warm total | Warm hit/miss | Feature rows | Reopened Inspector |
| ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| 100 | 5,040.32 ms | 183.35 ms | 4,784.91 ms | 71.50 ms | 0 / 100 | 81.01 ms | 100 / 0 | 100 | 100/100 in 20.83 ms |
| 1,000 | 38,564.68 ms | 546.35 ms | 37,776.28 ms | 241.45 ms | 0 / 1,000 | 638.89 ms | 1,000 / 0 | 1,000 | 1,000/1,000 in 118.15 ms |

The acceptance asserts complete counts, valid Inspector state, cache identity, persisted rows, and reload behavior. It records measured time without a machine-specific pass/fail threshold.

## Rapid selection cancellation

The A → B → C test uses three real generated JPEGs and the production WPF decoder.

- A entered decode while its token was not cancelled, then was cancelled by selection B; it did not publish.
- B entered decode while its token was not cancelled, then was cancelled by selection C; it did not publish.
- C completed, published exactly once, and its persisted Inspector state was `Valid`.
- Published asset count: `1`; published asset: C only.

## Evidence contract

The WPF evidence contract passes `13/13`. It maps exact filenames `07` through `14` to stable Preview automation targets, enforces synthetic-only evidence policy, rejects duplicate PNG bytes, rejects textual PNG metadata, and scans for local path/customer/token markers.

This contract does not make screenshots true. `visual_search_ui_review_complete` remains false until each requested scene is visibly reached in the real foreground Preview and its distinct window capture is saved.

## Final foreground attempt

The self-contained Preview was launched with a new temporary acceptance root and a newly generated synthetic fixture directory. The foreground window remained responsive, but the fixture import did not populate the AssetGrid; the visible import dialog exposed no completed asset selection or analysis state. No `07`–`14` capture was written, and no UI completion field was promoted from false.

## Explicitly unverified

- ICC detection or attempted conversion is not an independent color-management reference result. `color_management_reference_verified` stays false.
- A generated JPEG renamed to RAW is not a RAW fixture. `raw_visual_proxy_verified` stays false.
- Automated tests and Codex foreground operation do not set `UserVerified=true`.
