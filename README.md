Movie Recommendation System — Big Data (Hadoop + Spark) + R

Overview

This project demonstrates a movie recommendation system built with R and big-data tooling (Hadoop + Spark). It uses the MovieLens dataset and integrates SparkR / RHadoop for large-scale preprocessing, and `recommenderlab` for model training. A Shiny dashboard provides an interactive UI (inspired by apps like taste.io).

Key components

- Data: MovieLens (ml-latest-small by default) stored on HDFS (CSV/Parquet)
- Big-data processing: SparkR scripts to run on a Spark cluster reading/writing HDFS
- Model training: `recommenderlab` to build collaborative filtering models (UBCF/IBCF)
- Serving: R Shiny app to query the saved model and show recommendations and visualizations

Repository layout

- README.md (this file)
- .gitignore
- scripts/
  - install_packages.R         # Installs required R packages
  - download_movielens.R      # Download and extract MovieLens (local)
  - upload_to_hdfs.ps1        # PowerShell snippet to upload data to HDFS
  - train_recommender.R       # Train recommender locally (or on cluster head)
- spark/
  - preprocess_sparkr.R       # SparkR preprocessing (read raw CSV from HDFS, write cleaned data)
- shiny/
  - app.R                     # Shiny dashboard
- models/
  - (saved models go here)
- data/
  - raw/                      # Local raw downloads
  - cleaned/                  # Local cleaned CSV (optional)

Quick start (local, minimal)

1. Install R (>=4.0) and Rtools (Windows). Install Java and Spark if you want to use SparkR.
2. From R, run:
   - scripts/install_packages.R
3. Download MovieLens (ml-latest-small) locally:
   - Run `scripts/download_movielens.R` in R. It writes to `data/raw/`.
4. Train a small recommender locally (no Spark) for development:
   - Run `Rscript scripts/train_recommender.R --input data/raw/ratings.csv --movies data/raw/movies.csv --out models/recommender_ubcf.rds`
  - To use IBCF: `Rscript scripts/train_recommender.R --algo IBCF --n_neighbors 50 --out models/recommender_ibcf.rds`
5. Launch the Shiny app:
   - Run `R -e "shiny::runApp('shiny', port=3838, launch.browser=TRUE)"`

HDFS + Spark workflow (cluster)

1. Upload raw files to HDFS (example PowerShell snippet provided in `scripts/upload_to_hdfs.ps1`).
2. Run Spark preprocessing on the cluster using `spark/preprocess_sparkr.R` (use `spark-submit` or run in interactive SparkR session configured for your cluster). The script will read the HDFS CSVs, perform joins & light cleaning, and write cleaned Parquet/CSV back to HDFS.

Example spark-submit (adjust paths and master):

spark-submit --master yarn --deploy-mode cluster spark/preprocess_sparkr.R hdfs:///user/you/movielens hdfs:///user/you/movielens/cleaned

3. Copy cleaned CSV from HDFS to the cluster head or use RHadoop/SparkR to train directly on cluster.
4. Train with `scripts/train_recommender.R` (can run on cluster head) and save model artifact to HDFS or a storage accessible to the Shiny server.

Notes and references

- MovieLens datasets: https://grouplens.org/datasets/movielens/
- SparkR documentation: https://spark.apache.org/docs/latest/sparkr.html
- recommenderlab: https://cran.r-project.org/package=recommenderlab
- For inspiration UI/UX: taste.io (reference app only)

Next steps

- Run the setup and data download scripts.
- Upload to HDFS and run SparkR preprocessing.
- Train model and test the Shiny dashboard locally.

If you want, I can run through the next steps: create the SparkR and training scripts (I will), and add a runnable Shiny app scaffold that loads the trained model and shows recommendations.