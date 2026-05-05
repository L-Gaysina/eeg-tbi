# outputs_example

Эта папка содержит безопасные примерные выходные файлы пайплайна предобработки ЭЭГ.

В репозиторий не включаются сырые EEG-данные, очищенные FIF-файлы и таблицы с локальными путями к данным.

## tables

- `analysis_ready_inventory_cleaned_summary.csv` — агрегированная сводка финального analysis-ready набора.
- `analysis_ready_inventory_cleaned_public.csv` — построчная версия финального inventory без локальных путей.
- `final_cleaned_qc_summary.csv` — финальная сводка после очистки и унификации.
- `common_channels.txt` — список общих каналов, использованных в анализе.
- `channel_availability.csv` — доступность каналов по группам.
- `control_cleaning_qc_summary.csv` — QC-сводка для контрольной группы.
- `tbi_cleaning_qc_summary.csv` — QC-сводка для группы ЧМТ.
- `combined_cleaning_qc_summary_analysis_ready.csv` — объединённая QC-сводка после финального cleaning-QC.
- `amplitude_qc_summary_by_group.csv` — агрегированная сводка амплитудного контроля качества.
- `high_artifact_records_amplitude_qc_after_cleaning.csv` — записи с высокой долей эпох выше p2p-порога, если такие были обнаружены.

## qc

Папка содержит PNG-графики для визуального контроля качества:
состав выборки, распределение числа эпох, параметры очистки, амплитудный QC и проверку унификации каналов.
