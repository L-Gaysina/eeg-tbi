# Временные, сетевые и событийные признаки ЭЭГ

Этот подраздел относится ко второму этапу проекта — расчёту количественных ЭЭГ-признаков. Он содержит ноутбук, репрезентативные рисунки, агрегированные статистические таблицы и анонимизированные примеры таблиц для анализа временной структуры, функциональной связности и транзиентных событий ЭЭГ у пациентов с ЧМТ и контрольной группы.

## Место в проекте

1. [`01_preprocessing/`](../../01_preprocessing/) — предобработка ЭЭГ и контроль качества.
2. [`02_quantitative_eeg_features/`](../) — расчёт количественных признаков ЭЭГ.
   - [`01_spectral_features/`](../01_spectral_features/) — спектральные признаки ЭЭГ.
   - [`02_temporal_connectivity_events/`](./) — временные, сетевые и событийные признаки ЭЭГ.
3. [`03_machine_learning/`](../../03_machine_learning/) — модели классификации и оценка информативности признаков.

На вход этого этапа подаётся очищенный inventory после предобработки: `analysis_ready_inventory_cleaned.csv`.

## Ноутбук

Основной ноутбук анализа:

```text
notebooks/02_temporal_connectivity_events.ipynb
```
Перед запуском в конфигурационной ячейке необходимо указать локальный путь к рабочей папке с очищенными данными:
```text
CONFIG["work_dir"] = Path("/path/to/work-3")
```
## Структура подраздела
```text
02_temporal_connectivity_events/
│
├── README.md
│
├── notebooks/
│   └── 02_temporal_connectivity_events.ipynb
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
* расчёт DFA-подобных флуктуационных признаков амплитудной огибающей;
* анализ burst-эпизодов амплитудной огибающей;
* оценка функциональной связности ЭЭГ;
* построение матриц связности между областями интереса ROI×ROI;
* расчёт графовых метрик сетевой организации;
* анализ морфологических и пространственных характеристик транзиентных событий ЭЭГ;
* статистическое сравнение групп на уровне субъектов с коррекцией на множественные сравнения методом FDR.

 ## Основные рисунки
 | Файл                                                                                                                                           | Описание                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| [`figures/burst_metrics_envelope_by_roi_subject_level_horizontal.png`](./figures/burst_metrics_envelope_by_roi_subject_level_horizontal.png)   | Burst-характеристики амплитудной огибающей по ROI и диапазонам   |
| [`figures/connectivity_mean_strength_by_method_band_subject_level.png`](./figures/connectivity_mean_strength_by_method_band_subject_level.png) | Средняя сила функциональной связности по методам и диапазонам    |
| [`figures/connectivity_roi_matrix_wpli_alpha.png`](./figures/connectivity_roi_matrix_wpli_alpha.png)                                           | ROI×ROI матрицы wPLI в α-диапазоне: ЧМТ, контроль и разность     |
| [`figures/connectivity_roi_effect_hedges_g_wpli_alpha.png`](./figures/connectivity_roi_effect_hedges_g_wpli_alpha.png)                         | Размер эффекта Hedges’ g для ROI×ROI связности                   |
| [`figures/event_morphology_spatial_features_subject_level.png`](./figures/event_morphology_spatial_features_subject_level.png)                 | Морфологические и пространственные признаки транзиентных событий |
| [`figures/dfa_lrtc_envelope_by_roi_subject_level.png`](./figures/dfa_lrtc_envelope_by_roi_subject_level.png)                                   | DFA-подобные признаки амплитудной огибающей по ROI               |

 ## Основные таблицы
 | Файл                                                                                                                                     | Описание                                                                    |
| ---------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| [`tables/all_temporal_connectivity_event_stats_subject_level.csv`](./tables/all_temporal_connectivity_event_stats_subject_level.csv)     | Общая subject-level статистика по временным, сетевым и событийным признакам |
| [`tables/temporal_connectivity_event_stats_summary_by_block.csv`](./tables/temporal_connectivity_event_stats_summary_by_block.csv)       | Сводка статистики по блокам признаков                                       |
| [`tables/top_temporal_connectivity_event_effects_subject_level.csv`](./tables/top_temporal_connectivity_event_effects_subject_level.csv) | Топ признаков по абсолютному размеру эффекта                                |
| [`tables/dfa_stats_subject_level.csv`](./tables/dfa_stats_subject_level.csv)                                                             | Subject-level статистика DFA-подобных признаков                             |
| [`tables/burst_stats_subject_level.csv`](./tables/burst_stats_subject_level.csv)                                                         | Subject-level статистика burst-признаков                                    |
| [`tables/connectivity_stats_subject_level.csv`](./tables/connectivity_stats_subject_level.csv)                                           | Subject-level статистика признаков функциональной связности                 |
| [`tables/event_stats_subject_level.csv`](./tables/event_stats_subject_level.csv)                                                         | Subject-level статистика событийных признаков                               |
| [`tables/extended_graph_stats_subject_level.csv`](./tables/extended_graph_stats_subject_level.csv)                                       | Subject-level статистика расширенных графовых метрик                        |
## Статистический подход

Основная статистическая интерпретация выполнялась на уровне субъектов. Если у одного субъекта было несколько записей, признаки предварительно агрегировались внутри субъекта, чтобы избежать псевдорепликации.

Для межгрупповых сравнений использовались:

* критерий Манна–Уитни;
* размер эффекта Hedges’ g;
* rank-biserial correlation;
* коррекция на множественные сравнения методом Benjamini–Hochberg FDR.

Положительные значения Hedges’ g соответствуют большим значениям признака в группе ЧМТ, отрицательные — большим значениям в контрольной группе.

 ## Методические замечания

DFA-подобные признаки амплитудной огибающей следует интерпретировать осторожно, если они рассчитаны на коротких 4-секундных эпохах. В этом случае они рассматриваются как дополнительные флуктуационные признаки, а не как строгая оценка долговременных временных корреляций.

Результаты функциональной связности и графовых метрик могут зависеть от возраста и других межгрупповых факторов. Поэтому они интерпретируются как ассоциированные с групповой принадлежностью признаки и требуют дополнительной валидации на возрастно-сбалансированной выборке.

ROI-схема использовалась для укрупнённой топографической агрегации признаков. Результаты ROI-анализа интерпретируются как различия между топографическими областями скальпа, а не как прямое указание на активность конкретных корковых структур.

 ## Термины и обозначения
 | Обозначение             | Расшифровка                                           |
| ----------------------- | ----------------------------------------------------- |
| DFA                     | Анализ детрендированных флуктуаций                    |
| Burst                   | Кратковременный эпизод повышенной амплитуды огибающей |
| Functional connectivity | Функциональная связность                              |
| Graph metrics           | Графовые метрики сетевой организации                  |
| ROI                     | Область интереса                                      |
| wPLI                    | Взвешенный индекс фазовой задержки                    |
| PLV                     | Индекс фазовой синхронизации                          |
| FDR                     | Коррекция на долю ложных открытий                     |
