# Спектральные признаки ЭЭГ

Этот подраздел относится ко второму этапу проекта — расчёту количественных ЭЭГ-признаков. Он содержит ноутбук, репрезентативные рисунки, агрегированные статистические таблицы и анонимизированные примеры таблиц, полученные при спектральном анализе ЭЭГ у пациентов с ЧМТ и контрольной группы.

## Место в проекте

1. [`01_preprocessing/`](../../01_preprocessing/) — предобработка ЭЭГ и контроль качества.
2. [`02_quantitative_eeg_features/`](../) — расчёт количественных признаков ЭЭГ.
   - [`01_spectral_features/`](./) — спектральные признаки ЭЭГ.
   - [`02_temporal_connectivity_events/`](../02_temporal_connectivity_events/) — временные, сетевые и событийные признаки ЭЭГ.
3. [`03_machine_learning/`](../../03_machine_learning/) — модели классификации и оценка информативности признаков.

На вход этого этапа подаётся очищенный inventory после предобработки: `analysis_ready_inventory_cleaned.csv`.

## Ноутбук

Основной ноутбук анализа:

```text
notebooks/02_spectral_features.ipynb
```
Перед запуском в конфигурационной ячейке необходимо указать локальный путь к рабочей папке с очищенными данными:
```text
CONFIG["work_dir"] = Path("/path/to/work-3")
```
Ожидаемый входной файл:
```text
work-3/output/tables/analysis_ready_inventory_cleaned.csv
```
## Структура подраздела
```text
01_spectral_features/
│
├── README.md
├── README_spectral_features_ru.md
│
├── notebooks/
│   └── 02_spectral_features.ipynb
│
├── figures/
│   └── *.png
│
├── tables/
│   └── *.csv
│
└── tables_anonymized_samples/
    └── *_sample_anonymized.csv
```
## Основные этапы анализа
* оценка спектральной плотности мощности (power spectral density, PSD) методом multitaper;
* расчёт абсолютной и относительной мощности стандартных частотных диапазонов ЭЭГ;
* расчёт индексов спектрального замедления;
* оценка спектральной граничной частоты SEF50 и SEF95;
* расчёт нормированной спектральной энтропии и спектральной плоскостности;
* оценка апериодической 1/f-компоненты спектра;
* анализ индивидуального α-пика;
* анализ остаточного спектра после удаления апериодической компоненты;
* статистическое сравнение групп на уровне субъектов с коррекцией на множественные сравнения методом FDR;

## Основные рисунки
| Файл                                                                                                                                         | Описание                                                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [`figures/global_psd_group_mean.png`](./figures/global_psd_group_mean.png)                                                                   | Средняя глобальная спектральная плотность мощности в группах ЧМТ и контроля |
| [`figures/relative_bandpower_heatmap_roi_combined.png`](./figures/relative_bandpower_heatmap_roi_combined.png)                               | Относительная мощность частотных диапазонов по ROI                          |
| [`figures/relative_bandpower_boxplots_by_band_roi.png`](./figures/relative_bandpower_boxplots_by_band_roi.png)                               | Распределения относительной мощности по диапазонам и ROI                    |
| [`figures/statistics_hedges_g_relative_bandpower_heatmap.png`](./figures/statistics_hedges_g_relative_bandpower_heatmap.png)                 | Размеры эффекта Hedges’ g и FDR-значимость для относительной мощности       |
| [`figures/statistics_hedges_g_all_roi_features_heatmap.png`](./figures/statistics_hedges_g_all_roi_features_heatmap.png)                     | Размеры эффекта для всех ROI-спектральных признаков                         |
| [`figures/statistics_top_roi_features_by_hedges_g.png`](./figures/statistics_top_roi_features_by_hedges_g.png)                               | Топ ROI-признаков по абсолютному размеру эффекта                            |
| [`figures/spectral_shape_features_by_roi.png`](./figures/spectral_shape_features_by_roi.png)                                                 | SEF50, SEF95, спектральная энтропия и спектральная плоскостность            |
| [`figures/aperiodic_1f_features_by_roi.png`](./figures/aperiodic_1f_features_by_roi.png)                                                     | Апериодический показатель 1/f и offset                                      |
| [`figures/residual_spectrum_by_roi.png`](./figures/residual_spectrum_by_roi.png)                                                             | Остаточный спектр после удаления апериодической компоненты                  |
| [`figures/residual_spectrum_difference_heatmap_tbi_minus_control.png`](./figures/residual_spectrum_difference_heatmap_tbi_minus_control.png) | Межгрупповая разность остаточного спектра                                   |
| [`figures/residual_global_frequency_cluster_analysis.png`](./figures/residual_global_frequency_cluster_analysis.png)                         | Частотно-кластерный анализ глобального остаточного спектра                  |
| [`figures/topomap_hedges_g_alpha.png`](./figures/topomap_hedges_g_alpha.png)                                                                 | Топографическая карта размера эффекта для α-диапазона                       |
| [`figures/topomap_hedges_g_beta.png`](./figures/topomap_hedges_g_beta.png)                                                                   | Топографическая карта размера эффекта для β-диапазона                       |

## Основные таблицы
| Файл                                                                                                             | Описание                                                                        |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| [`tables/spectral_stats_roi_subject_level.csv`](./tables/spectral_stats_roi_subject_level.csv)                   | Основная subject-level статистика по ROI-спектральным признакам                 |
| [`tables/spectral_stats_roi.csv`](./tables/spectral_stats_roi.csv)                                               | Record-level статистика, сохранённая как дополнительная диагностическая таблица |
| [`tables/spectral_feature_map.csv`](./tables/spectral_feature_map.csv)                                           | Карта рассчитанных спектральных признаков                                       |
| [`tables/spectral_feature_missingness.csv`](./tables/spectral_feature_missingness.csv)                           | Сводка пропусков по спектральным признакам                                      |
| [`tables/channel_bandpower_stats_fdr.csv`](./tables/channel_bandpower_stats_fdr.csv)                             | Channel-level статистика мощности диапазонов с FDR-коррекцией                   |
| [`tables/residual_global_frequency_cluster_results.csv`](./tables/residual_global_frequency_cluster_results.csv) | Результаты частотно-кластерного анализа остаточного спектра                     |
