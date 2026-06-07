# NFL Strength of Schedule Analytics Platform

## Overview
This project is a data pipeline built in Databricks that figures out which NFL teams had the hardest schedules over the 2023, 2024, and 2025 seasons. Instead of just looking at one year, it combines all three seasons into a single leaderboard. It pulls messy, nested JSON game files from cloud storage and cleans them up to calculate a combined Strength of Schedule score for all 32 teams. 
I built this to get hands-on experience with big data architecture and cloud databases. The main goal was to figure out how to parse massive, deeply nested JSON files without using slow Python loops that drag down performance. It forced me to learn how to use distributed computing to flatten arrays and run complex database joins efficiently.

[Software Demo Video](http://youtube.link.goes.here)

# Cloud Database

For this project, I used a modern Lakehouse architecture built inside Databricks. Instead of a traditional standalone database engine, the data is managed using a local Catalog schema (workspace.nfl) where individual datasets are stored as Delta tables. The underlying storage uses cloud-managed Volumes, which lets the pipeline interact with raw file streams and structured tables inside the same environment.

The database structure follows the Medallion Architecture, splitting the data into distinct operational layers:

* **Bronze Layer (bronze_landing Volume):** This is the raw storage layer. It holds the original, unprocessed JSON weekly schedule files exactly as they came from the API.
* **Silver Layer (silver_nfl_teams Table):** A cleaned lookup table that maps unique team IDs to their official names (like mapping ID 11 to the "Indianapolis Colts").
* **Silver Layer (silver_team_season_records Table):** A processed table containing rows for each team's yearly performance. It breaks down the raw records into clean integer columns (wins, losses, games_played) and calculates a baseline win_percentage.
* **Gold Layer (Final Query Output):** The final analytical layer. It uses a SQL view to join the matchup web from the Bronze layer with the performance data from the Silver layers to calculate the final Strength of Schedule leaderboard.

# Development Environment
I used Databricks.

## Programming Languages & Libraries
I used python, SQL, and PySpark

# Useful Websites

* [Databricks Documentation](https://docs.databricks.com)
* [Spark SQL Built-in Functions](https://spark.apache.org/docs/latest/api/sql/index.html)
* [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

# Future Work

* **Add Automation:** I want to set up a workflow to automatically pull new weekly files so the pipeline updates itself without needing a manual refresh.
* **Optimize Queries:** I need to look into replacing the SQL array filters with native PySpark logic to see if it handles the triple-explosion data scale faster.
* **Build a Frontend Dashboard:** I would like to connect the final Gold layer leaderboard to a clean visualization tool like PowerBI or a Python web app to display the team rankings visually.
