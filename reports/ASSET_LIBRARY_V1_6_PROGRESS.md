# Asset Library V1.6 Development Preview

Branch: `feature/asset-library-v1`

## Acceptance fields

- `visual_analysis_ui_complete=true` — real foreground captures `07`–`09` show the selected synthetic JPEG in the Preview Inspector with distinct Palette, Histogram, and Tone tabs.
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

The real foreground captures now present are:

- `07_visual_analysis_color.png` — SHA-256 `89E23E6DA07C96C4742605E5F2CC972D0A09723CE62F2B07C287A17AA047FCB9`
- `08_visual_analysis_histogram.png` — SHA-256 `02A60C9EDE9DDAD65844CA85F41AE7E6D58BDF90B8E4561AB619EFD5BA95E321`
- `09_visual_analysis_tone.png` — SHA-256 `465D540748BF2B507F27D24EE52F054A26AAFB9FF65DA0D70EFF2F4DF8C5FB6E`

They are distinct full-window captures of the real Preview using generated JPEGs only. Captures `10`–`14` were not produced, so `visual_search_ui_review_complete`, visual filter/similarity, Smart Folder, and batch UI completion remain false.

## Foreground import recovery

The first failure was before the repository: `ImportDemoDirectoryAsync` scanned only the selected root with `SearchOption.TopDirectoryOnly`, while the generated JPEGs are stored under nested `phase0` and `performance` directories. It therefore produced an empty file list, never entered the repository transaction, and left the AssetGrid empty.

The Preview-only recovery now recursively scans the selected synthetic root, filters to supported image extensions, imports references transactionally, and actively refreshes the current query. This recovery is committed in asset HEAD `3da45d53e4628a743a2809f908cbdfd60d706c43`. The foreground file picker remains a multi-file picker, not a folder picker. It advertises JPG/JPEG/PNG/TIFF/WEBP plus only the existing RAW extensions; this does not claim RAW visual decoding support.

Latest isolated Dev Preview diagnostics:

| Field | Result |
| --- | ---: |
| PickerAccepted | true |
| SelectedFileCount | 33 |
| ImportCommandEntered | true |
| ImportServiceEntered | true |
| ImportedCount / SkippedCount / FailedCount | 33 / 0 / 0 |
| RepositoryAssetCountBefore / After | 0 / 33 |
| CurrentQueryCount | 33 |
| ViewModelItemCount | 33 |
| AssetGridItemCount | 33 |
| ItemsSource | `AssetCards`, same ViewModel collection |
| CollectionChangedCount | 35 |
| DataContext | `AssetLibraryPreviewViewModel` |
| SelectedCollection | `AllAssets` |
| ThumbnailQueueCount / ThumbnailFailureCount | 20 / 0 |

Diagnostics contain only counts, safe type/collection tokens, extension totals, and state tokens. They contain no absolute path, customer filename, or image contents. Reference mode does not move, rename, delete, or write the source file. Managed-copy records use their managed path for thumbnails when present, with source-path fallback.

The self-contained Preview used for this recovery is:

`artifacts/asset-library-v16/preview/PixelTart_AssetLibrary_V1_6_Preview_ImportRecovery2/PixelTart_AssetLibrary_V1_6_Preview.exe`

SHA-256: `D4ACB1E351397C9194B7D5771D06BA79236B7AFDC29C2938713AC4699C0B4963`

## Current verification

- Preview Debug and Release builds: 0 warnings, 0 errors.
- Core focused set (`AssetLibraryV16Tests`, `VisualAnalysisTests`, `AssetLibraryV16Phase0AcceptanceTests`): 27 passed, 0 failed, 0 skipped.
- WPF focused set (`AssetLibraryPreviewWpfTests`, `AssetLibraryV16EvidenceContractTests`): 13 passed, 0 failed, 0 skipped.
- Current counted total: 40 passed, 0 failed, 0 skipped.

## Explicitly unverified

- ICC detection or attempted conversion is not an independent color-management reference result. `color_management_reference_verified` stays false.
- A generated JPEG renamed to RAW is not a RAW fixture. `raw_visual_proxy_verified` stays false.
- Automated tests and Codex foreground operation do not set `UserVerified=true`.
