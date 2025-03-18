# F1 Fantasy League Predictions

This repository contains a Jupyter Notebook (`F1_Analysis4.ipynb`) designed to predict Formula 1 race outcomes and maximize F1 Fantasy League points (max 20 per race) using pre-qualifying data. The project leverages the FastF1 API, data analysis, and predictive modeling to optimize team selections, focusing on accurate pre-qualifying predictions.

## Project Overview
- **Purpose**: Predict qualifying positions, race results, and fastest laps to achieve the highest Fantasy League points. Points are awarded as follows: pole (2), podium positions (up to 5), top 10 positions (up to 8.5), and fastest lap (1).
- **Data Source**: Practice session data (laps, telemetry, weather) from the FastF1 API, processed and stored in a SQLite database (`f1_fantasy.db`) and CSVs under `raw_data/{year}/R{round_number}/`.
- **Current Data**: Fully processed 2024 Rounds 1 (Bahrain), 2 (Saudi Arabia), 3 (Australia), 4 (Japan), and 24 (Abu Dhabi), covering practice sessions, predictions, and actual results.
- **Status**: Cells 1-13 are fully implemented, covering data fetching, cleaning, analysis, prediction, reporting, batch processing, and database consolidation. The focus is now on refining prediction accuracy, particularly tire degradation.

### Notebook Structure
Each step is contained in a numbered cell, where the step number matches the cell number (e.g., Step 13 is Cell 13).
- **Cell 1**: Sets up libraries (`pandas`, `fastf1`, `matplotlib`, `sqlite3`, `fpdf`, etc.) and configures the FastF1 cache at `fastf1_cache/`.
- **Cell 2**: `get_target_event(manual_year=None, manual_round=None, skip_timing_validation=False)` - Dynamically fetches event info or accepts manual input, returning `target_info`.
- **Cell 3**: `download_practice_data(target_info)` - Downloads practice session data (laps, weather, indicators), saved as CSVs.
- **Cell 4**: `clean_and_aggregate_data(target_info, session_data)` - Cleans lap data and generates summaries, saved as CSVs.
- **Cell 5**: `compute_driver_performance(cleaned_data, year, round_number)` - Calculates driver metrics (e.g., `FastestLapTime`, `BasePace`, `DegradationSlope`).
- **Cell 6**: `analyze_telemetry_metrics(config)` - Analyzes telemetry data (e.g., `MaxSpeed`, `ThrottleTime`).
- **Cell 7**: `investigate_sandbagging(config)` - Detects sandbagging via pace discrepancies.
- **Cell 8**: `evaluate_track_evolution(target_info, cleaned_data)` - Tracks session progression (e.g., lap time evolution).
- **Cell 9**: `analyze_track_characteristics(target_info, cleaned_data)` - Assesses driver fit to track (e.g., `OverallFit`).
- **Cell 10**: `predict_race_outcomes(config, ...)` - Predicts qualifying, race outcomes, and fastest lap.
- **Cell 10.5**: `generate_comprehensive_report(config, ...)` - Generates a pre-qualifying PDF report, saved in `reports/`.
- **Cell 11**: `generate_summary_report(config, ...)` - Compares predictions to actual results (e.g., `ActualDegradationSlope`) and calculates points.
- **Cell 12**: `batch_process_rounds(year=2024, rounds=None)` - Batch processes multiple rounds, saving CSVs and reports.
- **Cell 13**: `consolidate_data(rounds=None, force_reimport=False)` - Loads CSVs into `f1_fantasy.db`.

### Current Progress
- **Database**: Fully populated with tables: `events` (5 rows), `driver_performance`, `telemetry_metrics`, `sandbag_analysis`, `track_characteristics`, `predictions`, `results` (108 rows each), `track_evolution` (15 rows) for 2024 Rounds 1-4, 24.
- **Accuracy Concern**: The tire degradation metric (`DegradationSlope` in Cell 5) may not align with `ActualDegradationSlope` (Cell 11), potentially skewing race predictions. Analysis is needed to improve this before evaluating predictors.

### Next Steps
- **Cell 14**: Analyze the `results` table in `f1_fantasy.db` (e.g., `DegError`, `PaceError`, `PositionError`) to assess prediction accuracy, focusing on degradation. Use SQLite queries and statistical methods (e.g., mean error, scatter plots) to suggest improvements for `DegradationSlope`.
- **Update Cell 5**: Refine `DegradationSlope` calculation (e.g., non-linear fit, session weighting) based on Cell 14 findings.
- **Update Cell 10**: Adjust race strategy with the improved degradation metric.
- **Re-run Cells 12-13**: Reprocess rounds and reload the database with updated calculations.
- **Cell 15**: Evaluate predictors (e.g., `BasePace`, `SandbagFlag`, `OverallFit`) against points using correlation and regression analysis.

## Using with Grok
1. **Prompt**: See `Prompt.txt` for the latest Grok interaction prompt (updated as of March 18, 2025).
2. **Collaboration**: Start a chat with: “See `https://github.com/conrelma/F1/blob/main/Prompt.txt` for context,” and provide `https://github.com/conrelma/F1/blob/main/F1_Analysis4.ipynb` or upload the `.ipynb` file. Request an updated `Prompt.txt` after each session. Grok should analyze the notebook, infer its state from code/comments, and avoid relying on prior outputs.

## Setup
- **Requirements**: Python 3.8+, `fastf1`, `pandas`, `sqlite3`, `matplotlib`, `fpdf` (install via `pip install -r requirements.txt` if added).
- **Run**: Open `F1_Analysis4.ipynb` in Jupyter or PyCharm, execute Cells 1-13 to replicate the database and data.
- **Data**: CSVs in `raw_data/`, database in `f1_fantasy.db`. Use `.gitignore` to exclude `fastf1_cache/`, `raw_data/`, `reports/`, `*.db`, `*.csv`, `*.log`, `.idea/`.

## Contributing
Fork the repository, submit issues, or pull requests to enhance predictions or extend data coverage (e.g., 2025 rounds).

**Date**: March 18, 2025