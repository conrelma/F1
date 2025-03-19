F1 Fantasy League Predictions
This repository contains a Jupyter Notebook (F1_Analysis4.ipynb) designed to predict Formula 1 race outcomes and maximize F1 Fantasy League points (max 20 per race) using pre-qualifying data. The project leverages the FastF1 API, data analysis, and predictive modeling to optimize team selections, focusing on accurate pre-qualifying predictions.

Project Overview
Purpose: Predict qualifying positions, race results, and fastest laps to achieve the highest Fantasy League points. Points are awarded as follows: pole (2), podium positions (up to 5), top 10 positions (up to 8.5), and fastest lap (1).
Data Source: Practice session data (laps, telemetry, weather) from the FastF1 API, processed and stored in a SQLite database (f1_fantasy.db) and CSVs under raw_data/{year}/R{round_number}/.
Status: Cells 1-14 are fully implemented, covering data fetching, cleaning, analysis, prediction, reporting, batch processing, database consolidation, and prediction trend analysis. The focus is now on resolving issues in Cell 12 to enable full processing of Rounds passed in the cell, followed by refining prediction accuracy, particularly tire degradation.

Notebook Structure
Each step is contained in a numbered cell, where the step number matches the cell number (e.g., Step 14 is Cell 14).

Cell 1: Sets up libraries (pandas, fastf1, matplotlib, sqlite3, fpdf, etc.) and configures the FastF1 cache at fastf1_cache/.
Cell 2: get_target_event(manual_year=None, manual_round=None, skip_timing_validation=False) - Dynamically fetches event info or accepts manual input, returning target_info.
Cell 3: download_practice_data(target_info) - Downloads practice session data (laps, weather, indicators), saved as CSVs.
Cell 4: clean_and_aggregate_data(target_info, session_data) - Cleans lap data and generates summaries, saved as CSVs.
Cell 5: compute_driver_performance(cleaned_data, year, round_number) - Calculates driver metrics (e.g., FastestLapTime, BasePace, DegradationSlope).
Cell 6: analyze_telemetry_metrics(config) - Analyzes telemetry data (e.g., MaxSpeed, ThrottleTime).
Cell 7: investigate_sandbagging(config) - Detects sandbagging via pace discrepancies.
Cell 8: evaluate_track_evolution(target_info, cleaned_data) - Tracks session progression (e.g., lap time evolution).
Cell 9: analyze_track_characteristics(target_info, cleaned_data) - Assesses driver fit to track (e.g., OverallFit).
Cell 10: predict_race_outcomes(config, ...) - Predicts qualifying, race outcomes, and fastest lap.
Cell 10.5: generate_comprehensive_report(config, ...) - Generates a pre-qualifying PDF report, saved in reports/.
Cell 11: generate_summary_report(config, ...) - Compares predictions to actual results (e.g., ActualDegradationSlope) and calculates points.
Cell 12: batch_process_rounds(year=2024, rounds=None) - Batch processes multiple rounds, saving CSVs and reports.
Cell 13: consolidate_data(rounds=None, force_reimport=False) - Loads CSVs into f1_fantasy.db.
Cell 14: analyze_prediction_trends(db_path="f1_fantasy.db", race_id=None) - Analyzes prediction trends by comparing predicted race outcomes with actual results, correlating errors with metrics like BasePace and DegradationSlope, and visualizing trends.

Current Progress
Cell 12 Issue: Batch processing for Rounds 1-9 fails due to two issues:
Error in Cell 4: load_existing_practice_data() in Cell 12 raises TypeError: load_existing_practice_data() got an unexpected keyword argument 'force_redownload' because Cell 4 does not yet include this parameter. Development was paused before applying the fix.
Missing Rounds 8 and 9: fastf1.get_event_schedule in Cell 2 returns an incomplete 2024 schedule (15 events instead of 24), missing Rounds 8 (Monaco Grand Prix) and 9 (Canadian Grand Prix). We need to investigate what caused this issue.
Cell 14 Added: Implemented analyze_prediction_trends to compare predicted race outcomes with actuals, providing detailed error analysis (e.g., position errors, fastest lap errors) and visualizations to correlate errors with analysis inputs (e.g., BasePace, DegradationSlope).
Next Steps
Fix Cell 2: Double check the schedule download functionality. Ensure the function can run standalone while preserving existing functionality (historical winners, pole sitters, track metadata).
Fix Cell 4: Update load_existing_practice_data() to include the force_redownload parameter and ensure it can run standalone. Preserve the existing clean_and_aggregate_data() functionality.
Run Cell 12: Process Rounds 1-9 with the updated Cells 2 and 4, confirming that all rounds are processed and time conversions are correct.  Cell 12 must be able to run without redownloading data to make it faster to adjust calculations and rerun the code.
Run Cells 13 and 14: Compare predictions with actuals for Rounds 1-9 using Cell 14.
Update cell 5 to 9:  Based on the comparison in cells 13 and 14 evaluate what is causing the biggest prediction errors
Update Cell 10: Adjust race strategy with the improved analysis.
Re-run Cells 12-13: Reprocess rounds and reload the database with updated calculations.
Cell 15: Evaluate predictors (e.g., BasePace, SandbagFlag, OverallFit) against points using correlation and regression analysis.

Using with Grok
Prompt: See Prompt.txt for the latest Grok interaction prompt (updated as of March 19, 2025).
Collaboration: Start a chat with: “See https://github.com/conrelma/F1/blob/main/Prompt.txt for context,” and provide https://github.com/conrelma/F1/blob/main/F1_Analysis4.ipynb or upload the .ipynb file. Grok should analyze the notebook, infer its state from code/comments, and avoid relying on prior outputs.

Setup
Requirements: Python 3.8+, fastf1, pandas, sqlite3, matplotlib, fpdf (install via pip install -r requirements.txt if added).
Run: Open F1_Analysis4.ipynb in Jupyter or PyCharm, execute Cells 1-14 to replicate the database and data.
Data: CSVs in raw_data/, database in f1_fantasy.db. Use .gitignore to exclude fastf1_cache/, raw_data/, reports/, *.db, *.csv, *.log, .idea/.


Date: March 19, 2025