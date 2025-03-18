# F1
F1 Analysis and Fantasy predictions

# F1 Fantasy League Predictions

This repository contains a Jupyter Notebook (`F1_Fantasy_2024.ipynb`) for predicting Formula 1 race outcomes and maximizing F1 Fantasy League points (max 20 per race) based on pre-qualifying data. The project uses the FastF1 API, data analysis, and predictive modeling to optimize team selections.

## Project Overview
- **Goal**: Predict quali positions, race outcomes, and fastest laps to maximize Fantasy League points.
- **Data**: Practice, telemetry, and historical F1 data for 2024 Rounds 1-4, 24, stored in a SQLite database (`f1_fantasy.db`).
- **Status**: Cells 1-13 completed, covering data fetching, cleaning, analysis, predictions, and database consolidation.

### Notebook Structure
- **Cell 1**: Library imports and FastF1 setup.
- **Cell 2**: `get_target_event` - Fetches event info.
- **Cell 3**: `download_practice_data` - Downloads practice data.
- **Cell 4**: `clean_and_aggregate_data` - Cleans lap data.
- **Cell 5**: `compute_driver_performance` - Calculates pace and degradation.
- **Cell 6**: `analyze_telemetry_metrics` - Telemetry analysis.
- **Cell 7**: `investigate_sandbagging` - Detects sandbagging.
- **Cell 8**: `evaluate_track_evolution` - Tracks session progression.
- **Cell 9**: `analyze_track_characteristics` - Assesses track fit.
- **Cell 10**: `predict_race_outcomes` - Predicts quali, race, fastest lap.
- **Cell 10.5**: `generate_comprehensive_report` - Creates PDF report.
- **Cell 11**: `generate_summary_report` - Compares predictions to actuals.
- **Cell 12**: `batch_process_rounds` - Processes multiple rounds.
- **Cell 13**: `consolidate_data` - Loads CSVs into SQLite.

### Current Progress
- **Database**: Fully populated with tables: `events` (5 rows), `driver_performance`, `telemetry_metrics`, `sandbag_analysis`, `track_characteristics`, `predictions`, `results` (108 rows each), `track_evolution` (15 rows).
- **Data**: 2024 Rounds 1-4, 24 processed and stored in `raw_data/2024/R{round_number}/`.

### Next Steps
- **Cell 14**: Analyze prediction accuracy (e.g., `DegError`, `PaceError`) in `results` table to refine calculations like tire degradation (`DegradationSlope`).
- **Update Cells 5, 10**: Improve degradation and race predictions based on Cell 14 findings.
- **Cell 15**: Evaluate predictors (e.g., `BasePace`, `SandbagFlag`) against points.
- **Cell 16+**: Modularize code and extend to all 2024/2025 rounds.

## Using with Grok
1. **Prompt**: See `Prompt.txt` for the latest interaction prompt with Grok, updated as of March 18, 2025.
2. **Collaboration**: Provide Grok with `https://github.com/{your-username}/F1-Fantasy-League-Predictions/blob/main/Prompt.txt` followed by your question. Request an updated `Prompt.txt` after each session.

## Setup
- **Requirements**: Python 3.8+, `fastf1`, `pandas`, `sqlite3`, `matplotlib`, `fpdf`.
- **Run**: Open `F1_Fantasy_2024.ipynb` in Jupyter, execute Cells 1-13 to replicate the database.
- **Data**: CSVs in `raw_data/`, database in `f1_fantasy.db`.

## Contributing
Feel free to fork, submit issues, or PRs to enhance predictions or add 2025 data!

**Date**: March 18, 2025
