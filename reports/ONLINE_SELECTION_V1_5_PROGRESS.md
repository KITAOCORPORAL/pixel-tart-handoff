# Online Selection V1.5 LocalDev Preview

Protocol: `pixel-tart-handoff/v1`

Branch: `feature/online-selection-v1`

HEAD: `e30eac4762af7eff837645a8303c47eeb95c5fe2`

P0 baseline: `4dac5f8e4460b7a67309646b6133bd186c121fea`

## Verified

- `localdev_server_complete=true`
- `loopback_bind=127.0.0.1`
- `localdev_db_complete=true`
- `client_choice_persisted=true`
- `confirm_snapshot_complete=true`
- `reopen_version_complete=true`
- `matching_sync_contract_complete=true`
- `desktop_result_pull_single_session=true`
- `short_lived_media_session=true`
- `main_token_in_media_query=false`
- `multi_client_conflict_detection=true`
- `weak_network_injection_contract=true`
- `online_preview_build=true`
- `launcher_isolation_verified=true`
- `formal_release_provider=None`
- `P0Merged=false`
- `RCGenerated=false`
- `UserVerified=false`

## Validation

- Selection API: `10/10`
- Core focused including formal-release isolation: `46/46`
- WPF Online Selection: `23/23`
- Mini Program contract: passed
- Server Debug build: `0 warnings / 0 errors`
- Preview Debug/Release build: `0 warnings / 0 errors`
- Formal product Release build: `0 warnings / 0 errors`
- NuGet vulnerable package scan: no known vulnerable packages
- Shared `IAssetSelectionSource` contract SHA-256: `0CFF267202FE8B1715C20729077F7792F4A30CDAB795F81ECF356AD94F90868A`

One-click launcher: `tools/PixelTart_OnlineSelection_LocalDev_Preview.ps1`

Final isolated smoke: `127.0.0.1:5159` ready; Preview title `Pixel Tart Online Selection LocalDev`; only the recorded Server and Preview PIDs were stopped.

## Partial or Deferred

- `local_object_storage_complete=false` — upload authorization/concurrency and RAW rejection are tested; disk-delete failure cleanup journal is deferred.
- `desktop_publish_complete=true` — create/import/re-encode/upload/publish works in the isolated Preview; production cloud is not configured.
- `desktop_result_pull_complete=false` — current session pull works, but asset DTOs do not hydrate full remote selected/favorite/comment state after local storage loss.
- `mini_program_local_api_complete=false` — the five-page LocalDev adapter, persistent pending queue and media sessions exist, but no WeChat DevTools runtime acceptance was performed.
- `weak_network_test=false` — deterministic injection contracts exist; complete queue behavior under live DevTools weak network is deferred.
- `multi_client_conflict_test=false` — server returns version/revision conflict and client refreshes/replays bounded intent; field-level remote state merge is deferred.
- `selection_rules_enforced=false` — rules persist, but all min/max/favorite/comment rules are not yet enforced transactionally by the server.
- `media_token_auto_refresh=false` — short-lived token works; automatic renewal is deferred.
- `online_selection_ui_review_complete=false` — all seven requested screenshots are deferred rather than fabricated.
- `physical_phone_localdev=false` — loopback works in desktop/DevTools only; a phone cannot reach the computer through `127.0.0.1`.
- `production_deployment=false`

## Mini Program

Exactly five pages remain: Project, Gallery, Photo, Selected and Confirm. Pages do not call `wx.request` directly. Runtime configuration stays outside Git; committed LocalDev config is disabled and credential-free. `AppSecret` and `session_key` remain server-only boundaries.

## Safety

The standalone Server uses an isolated SQLite/object root and re-encoded JPEG variants. Formal Pixel Tart still uses Provider=None. No production key, token, database, customer image, RAW, server log, UI screenshot or P0 interaction file is included.
