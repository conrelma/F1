# F1 Fantasy League Predictions

This repository contains a Jupyter Notebook (`F1_Analysis4.ipynb`) designed to predict Formula 1 race outcomes and maximize F1 Fantasy League points (max 20 per race) using pre-qualifying data. The project leverages the FastF1 API, data analysis, and predictive modeling to optimize team selections for the F1 Fantasy League game.

## Project Overview
- **Purpose**: Predict qualifying positions, race results, and fastest laps to achieve the highest possible Fantasy League points based on pre-qualifying data analysis. 
- Points are awarded 
  - 1pt per finisher in top 10. 
  - Extra 1pt per exact podium position. 
  - 2pts podium bonus for picking all three podium finishers (position irrelevant)
  - Bonus 0.5 pts per exact position outside of podium in top 10
  - 1pt for correct fastest lap
- **Data Source**: Practice session data (laps, telemetry, weather) from the FastF1 API, processed and stored in a SQLite database (`f1_fantasy.db`) and CSVs (`raw_data/{year}/R{round_number}/`).
- **Current Data**: 2024 Rounds 1 (Bahrain), 2 (Saudi Arabia), 3 (Australia), 4 (Japan), and 24 (Abu Dhabi), covering practice, predictions, and actual results.
- **Status**: Cells 1-13 are fully implemented, covering data fetching, cleaning, analysis, prediction, reporting, batch processing, and database consolidation.

### Notebook Structure
- **Cell 1**: Sets up libraries (`pandas`, `fastf1`, `matplotlib`, `sqlite3`, `fpdf`, etc.) and configures FastF1 cache at `fastf1_cache/`.
- **Cell 2**: `get_target_event(manual_year=None, manual_round=None, skip_timing_validation=False)` - Fetches event info dynamically (next race) or manually, returning `target_info` (e.g., year, round, event_name).
- **Cell 3**: `download_practice_data(target_info)` - Downloads practice session data (laps, weather, indicators), saved as CSVs (e.g., `2024_R1_Practice_1_laps.csv`).
- **Cell 4**: `clean_and_aggregate_data(target_info, session_data)` - Cleans lap data (removes outliers, standardizes formats), generates `cleaned_data` and `driver_summary`, saved as CSVs (e.g., `2024_R1_practice_summary.csv`).
- **Cell 5**: `compute_driver_performance(cleaned_data, year, round_number)` - Calculates driver metrics (e.g., `FastestLapTime`, `BasePace`, `DegradationSlope`) from practice, saved as `2024_R{round}_driver_performance.csv`.
- **Cell 6**: `analyze_telemetry_metrics(config)` - Analyzes telemetry (e.g., `MaxSpeed`, `ThrottleTime`), saved as `2024_R{round}_telemetry_metrics.csv`.
- **Cell 7**: `investigate_sandbagging(config)` - Detects sandbagging via pace discrepancies, saved as `2024_R{round}_sandbag_analysis.csv`.
- **Cell 8**: `evaluate_track_evolution(target_info, cleaned_data)` - Tracks session progression (e.g., lap time evolution), saved as `2024_R{round}_track_evolution.csv`.
- **Cell 9**: `analyze_track_characteristics(target_info, cleaned_data)` - Assesses driver fit to track (e.g., `OverallFit`), saved as `2024_R{round}_track_characteristics_drivers.csv` and `track_characteristics.csv`.
- **Cell 10**: `predict_race_outcomes(config, ...)` - Predicts quali (`AdjustedLapTime`, `QualiPosition`), race (`AdjustedRaceTime`, `FinalPosition`), and fastest lap, saved as `2024_R{round}_quali_prediction.csv` and `race_prediction.csv`.
- **Cell 10.5**: `generate_comprehensive_report(config, ...)` - Generates a pre-qualifying PDF report, saved in `reports/2024/R{round}/`.
- **Cell 11**: `generate_summary_report(config, ...)` - Compares predictions to actual results (e.g., `ActualDegradationSlope`), calculates points, saved as `2024_R{round}_summary_results.csv`.
- **Cell 12**: `batch_process_rounds(year=2024, rounds=None)` - Batch processes rounds (default: all detected), runs Cells 2-11, saves CSVs and reports.
- **Cell 13**: `consolidate_data(rounds=None, force_reimport=False)` - Loads CSVs into `f1_fantasy.db`, auto-detecting rounds, with tables: `events` (5 rows), `driver_performance`, `telemetry_metrics`, `sandbag_analysis`, `track_characteristics`, `predictions`, `results` (108 rows each), `track_evolution` (15 rows).

### Current Progress
- **Data**: Fully processed 2024 Rounds 1-4, 24, with CSVs in `raw_data/2024/R{round_number}/` and data in `f1_fantasy.db`.
- **Accuracy Concern**: Tire degradation (`DegradationSlope` in Cell 5) may not align with `ActualDegradationSlope` (Cell 11), impacting race predictions. Analysis needed to refine calculations.

### Next Steps
- **Cell 14**: Analyze `results` table (e.g., `DegError`, `PaceError`, `PositionError`) to assess prediction accuracy, focusing on degradation. Use stats (mean error, scatter plots) to suggest improvements.
- **Update Cell 5**: Refine `DegradationSlope` (e.g., non-linear fit) based on Cell 14.
- **Update Cell 10**: Adjust race strategy with improved degradation.
- **Re-run Cells 12-13**: Reprocess rounds and reload database.
- **Cell 15**: Evaluate predictors (e.g., `BasePace`, `SandbagFlag`, `OverallFit`) against points using correlation/regression.


## Using with Grok
1. **Prompt**: See `Prompt.txt` for the latest Grok interaction prompt.
2. **Collaboration**: Start a chat with: “See `https://github.com/conrelma/F1/blob/main/Prompt.txt` for context,” followed by your question. Upload `F1_Analysis4.ipynb` and request an updated `Prompt.txt` after each session.

## Setup
- **Requirements**: Python 3.8+, `fastf1`, `pandas`, `sqlite3`, `matplotlib`, `fpdf` (install via `pip install -r requirements.txt` if added).
- **Run**: Open `F1_Analysis4.ipynb` in Jupyter, execute Cells 1-13 to replicate.
- **Data**: CSVs in `raw_data/`, database in `f1_fantasy.db`.

## Contributing
Fork, submit issues, or PRs to improve predictions or extend data coverage.

**Date**: March 18, 2025
