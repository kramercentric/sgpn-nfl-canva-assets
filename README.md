# sgpn-nfl-canva-assets

Temp public host for Canva-ingest recolor assets (`upload-asset-from-url` requires a public
https URL; Canva's own asset thumbnails are signed and can't be re-fetched full-res).

## grid_master_2x.png

The regenerated MASTER for the perspective grid graphic used on every team preview deck.

- **Source (first-generation jets-lineage grid):** `grid_jets_native3.png`
  @ commit `0ee8936e943d73badbe84943911bfd6069019e9f` ("jets grid: left border strip cropped",
  2026-07-10). Confirmed first-generation by git history — it's the last file in the
  original Jets-only build session, immediately preceding the first team-derived grid
  (`grid_lv.png`). Confirmed cleanest by AA color-depth sampling (unique-RGB count in a
  1920x60 top-band crop) against its own lineage siblings and early descendants:
  `grid_jets.png` 254, `grid_jets_helmet.png` 150, `grid_jets_greenwhite.png` 337,
  `grid_jets_native.png` 4,890, `grid_jets_native2.png` 14,805, **`grid_jets_native3.png`
  14,401** (functionally the ceiling of the lineage — native2's marginally higher count is
  residual page-border pixels, not grid-line AA), vs. fresh single-generation swaps
  `ten_grid.png` 10,897 / `cle_grid.png` 11,241, and the degraded current `min_grid.png`
  at only **337**.
- **Regeneration:** Lanczos 2x upscale (1920x312 -> 3840x624), then a line re-threshold
  (UnsharpMask radius 2 / percent 180 / threshold 2) to counteract the edge-softening the
  Lanczos interpolation introduces on thin lines, restoring crisp line/background
  transitions instead of a mushy blur.
- **Measured AA quality before/after:**
  - Top-band (0,0,W,60px-equivalent) unique-RGB count: native3 1x = 14,401 -> Lanczos-only
    2x (pre-rethreshold) = 39,863 -> final master (2x + rethreshold) = 82,636.
  - Single-edge transition steps (horizontal scan across one line crossing): native3 = 36
    steps -> master = 57 steps (proportionally sharper, not just more pixels).
- Every derived per-team grid should now be re-derived from `grid_master_2x.png` in ONE
  hue-swap pass (never from another team's already-tinted grid, a page export, or a
  crop-then-stretch) per the `sgpn-canva-replicate` skill's MASTER-DERIVATION RULE.

## min_grid_v2.png

MIN's grid re-derived from `grid_master_2x.png` in one pass: green family hue-swapped to
MIN gold `#EBB540` (hue+sat locked to the target, per-pixel V preserved so the original
AA/brightness gradient survives), output at the master's full 3840x624 resolution — full
height, no crop-then-stretch. Supersedes `min_grid.png` (292px crop-stretched, AA depth 337
in the same top-band metric — the exact degraded-chain case the MASTER-DERIVATION RULE
exists to prevent) and `min_grid_purple.png` (kept for reference; Ryan reverted to gold).
