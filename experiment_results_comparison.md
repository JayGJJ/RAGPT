# RAGPT / Baseline 实验结果与论文结果对比

本文档整理当前 `checkpoints/` 下已有实验日志，并与论文 `Retrieval-Augmented Dynamic Prompt Tuning for Incomplete.pdf` 中 Table 1 的 70% missing rate 主实验结果进行对比。

## 口径说明

- 论文结果来源：论文 Table 1。
- 本地结果来源：`checkpoints/train_*/log.txt` 中的 `Final Tesing ...` 指标。
- 数值单位：为便于和论文一致，所有本地指标均已从 `0.x` 转为百分制。
- 差值定义：`Δ = 本地结果 - 论文结果`，单位为百分点。
- MM-IMDb 指标：F1-Micro (`F1-M`) 与 F1-Sample (`F1-S`)。
- HateMemes 指标：AUROC。
- Food101 未列入：当前 `checkpoints/` 中没有对应本地实验。
- RAGPT 的 `K` 设置：论文说明 MM-IMDb image missing 使用 `K=3`，其他场景使用 `K=5`。因此主对比表中 RAGPT 的 MM-IMDb image missing 默认使用本地 `K=3` 结果；本地额外训练过的 `K=5` 结果单独列出。

## MM-IMDb 对比表

| Method | Source / Setting | Text F1-M | Text F1-S | Image F1-M | Image F1-S | Both F1-M | Both F1-S |
|---|---:|---:|---:|---:|---:|---:|---:|
| MAPs | Paper | 46.12 | 45.47 | 44.86 | 43.19 | 45.48 | 44.30 |
| MAPs | Ours | 42.20 | 39.90 | 40.35 | 37.73 | 40.55 | 38.04 |
| MAPs | Δ | -3.92 | -5.57 | -4.51 | -5.46 | -4.93 | -6.26 |
| MSPs | Paper | 49.16 | 48.81 | 44.62 | 43.06 | 48.28 | 46.71 |
| MSPs | Ours | 41.77 | 39.30 | 40.00 | 37.06 | 40.29 | 37.48 |
| MSPs | Δ | -7.39 | -9.51 | -4.62 | -6.00 | -7.99 | -9.23 |
| RAGPT | Paper | 55.16 | 55.00 | 46.44 | 45.12 | 50.89 | 50.22 |
| RAGPT | Ours, paper K rule | 58.52 | 58.24 | 39.48 | 39.74 | 50.77 | 49.95 |
| RAGPT | Δ, paper K rule | +3.36 | +3.24 | -6.96 | -5.38 | -0.12 | -0.27 |
| RAGPT | Ours, image K=5 extra | - | - | 45.31 | 42.69 | - | - |
| RAGPT | Δ, image K=5 extra | - | - | -1.13 | -2.43 | - | - |

## HateMemes 对比表

| Method | Source / Setting | Text Missing AUROC |
|---|---:|---:|
| MAPs | Paper | 58.62 |
| MAPs | Ours | 59.03 |
| MAPs | Δ | +0.41 |
| MSPs | Paper | 59.60 |
| MSPs | Ours | 57.25 |
| MSPs | Δ | -2.35 |
| RAGPT | Paper | 64.10 |
| RAGPT | Ours, K=5 | 68.02 |
| RAGPT | Δ | +3.92 |

## 本地实验明细

| Dataset | Missing type | Method | Key setting | Best valid primary metric | Final test metric | Checkpoint |
|---|---|---|---|---:|---:|---|
| MM-IMDb | text | MAPs | prompt_num=6 | F1-M 42.87 @ epoch 20 | F1-M 42.20 / F1-S 39.90 | `checkpoints/train_maps_mmimdb_text_0.7_2026-06-30_20-40-55` |
| MM-IMDb | text | MSPs | prompt_num=6 | F1-M 43.10 @ epoch 18 | F1-M 41.77 / F1-S 39.30 | `checkpoints/train_msps_mmimdb_text_0.7_2026-06-30_21-30-18` |
| MM-IMDb | text | RAGPT | K=5 | F1-M 62.03 @ epoch 7 | F1-M 58.52 / F1-S 58.24 | `checkpoints/train_ragpt_mmimdb_text_0.7_2026-06-30_18-12-12` |
| MM-IMDb | image | MAPs | prompt_num=6 | F1-M 40.90 @ epoch 18 | F1-M 40.35 / F1-S 37.73 | `checkpoints/train_maps_mmimdb_image_0.7_2026-07-01_14-58-28` |
| MM-IMDb | image | MSPs | prompt_num=6 | F1-M 40.46 @ epoch 18 | F1-M 40.00 / F1-S 37.06 | `checkpoints/train_msps_mmimdb_image_0.7_2026-07-01_15-39-55` |
| MM-IMDb | image | RAGPT | K=3 | F1-M 42.57 @ epoch 1 | F1-M 39.48 / F1-S 39.74 | `checkpoints/train_ragpt_mmimdb_image_0.7_2026-07-01_14-14-27` |
| MM-IMDb | image | RAGPT | K=5 extra | F1-M 46.90 @ epoch 11 | F1-M 45.31 / F1-S 42.69 | `checkpoints/train_ragpt_mmimdb_image_0.7_2026-07-01_16-33-59` |
| MM-IMDb | both | MAPs | prompt_num=6 | F1-M 40.14 @ epoch 15 | F1-M 40.55 / F1-S 38.04 | `checkpoints/train_maps_mmimdb_both_0.7_2026-07-01_12-12-50` |
| MM-IMDb | both | MSPs | prompt_num=6 | F1-M 41.09 @ epoch 20 | F1-M 40.29 / F1-S 37.48 | `checkpoints/train_msps_mmimdb_both_0.7_2026-07-01_12-54-29` |
| MM-IMDb | both | RAGPT | K=5 | F1-M 53.74 @ epoch 3 | F1-M 50.77 / F1-S 49.95 | `checkpoints/train_ragpt_mmimdb_both_0.7_2026-07-01_10-41-33` |
| HateMemes | text | MAPs | prompt_num=6 | AUROC 60.19 @ epoch 11 | AUROC 59.03 | `checkpoints/train_maps_hatememes_text_0.7_2026-07-01_20-05-07` |
| HateMemes | text | MSPs | prompt_num=6 | AUROC 59.83 @ epoch 17 | AUROC 57.25 | `checkpoints/train_msps_hatememes_text_0.7_2026-07-01_20-23-10` |
| HateMemes | text | RAGPT | K=5 | AUROC 74.96 @ epoch 2 | AUROC 68.02 | `checkpoints/train_ragpt_hatememes_text_0.7_2026-07-01_19-25-59` |

## 结果总结

1. MM-IMDb text missing 场景下，本地 RAGPT 明显高于论文结果：F1-M 提升 3.36，F1-S 提升 3.24；同时也显著高于本地 MAPs/MSPs。
2. MM-IMDb both missing 场景下，本地 RAGPT 与论文基本一致：F1-M 低 0.12，F1-S 低 0.27，属于很接近的复现实验结果；但本地 MAPs/MSPs 明显低于论文。
3. MM-IMDb image missing 是主要差距来源。按论文设置 `K=3` 时，本地 RAGPT 比论文低 6.96/5.38；改为 `K=5` 后提升到 45.31/42.69，仍低于论文 1.13/2.43，但明显优于 `K=3`。
4. HateMemes text missing 场景下，本地 RAGPT AUROC 为 68.02，比论文高 3.92；MAPs 与论文基本持平，MSPs 低 2.35。
5. 从本地结果看，RAGPT 在 MM-IMDb text、MM-IMDb both、HateMemes text 上保持明显优势；baseline 在 MM-IMDb 三个缺失场景中整体低于论文复现值，尤其 MSPs 的 text/both 缺失差距较大。
6. 若后续需要写论文式报告，建议把 MM-IMDb image missing 同时报告 `K=3` 与 `K=5`：`K=3` 是论文设置可比结果，`K=5` 是本地更优的补充结果。

