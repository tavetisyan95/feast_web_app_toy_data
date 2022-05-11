## Feature store description
This is a toy feature store for [this React.js web application](https://github.com/tavetisyan95/feast_web_app) built for the post [Creating a Feature Store with Feast Part 3: Building An API and React App for Feast](https://kedion.medium.com/feature-storage-for-ml-with-feast-a061899fc4a2). The feature store is based on the driver stats dataset used across many tutorials in the Feast docs. We've changed the dataset somewhat to better showcase the app's capabilities and simplify its use.

You can find the original dataset [here](https://github.com/feast-dev/feast-demo), or in the root directory of this project (`driver_stats_with_string.parquet`).

This feature store has two parquet files:

- `driver_stats/data/driver_stats_1.parquet` - contains timestamps, driver IDs, and the feature column `conv_rate`
- `driver_stats/data/driver_stats_2.parquet` - contains timestamps, driver IDs, and the feature columns `acc_rate` and `daily_avg_trips`.

A few things to keep in mind with the dataset:

- The timestamps in the dataset range from `2021-09-01 00:00:00+00:00` to `2021-09-15 00:00:00+00:00`. 
- The entity column name for the dataset is `driver_id` - it represents the unique drivers that the data was collected for.
- There are two feature views in the feature store – `driver_stats_fv_1` and `driver_stats_fv_2`. `driver_stats_fv_1` contains the feature column `conv_rate`, while `driver_stats_fv_2` contains the feature columns `acc_rate` and `avg_daily_trips`.
- There is one entity registered in the feature store – `driver_id`. It represents the unique drivers that the data was collected for.
- There are five driver IDs in the dataset - `1001`, `1002`, `1003`, `1004`, and `1005`.
- For each hour in the indicated timestamp range, there are feature values for each driver ID. This means that you have five feature rows (one per driver ID) for every hour.


## Differences from the original dataset
The original dataset comprises a single parquet file. It has the following feature columns:

1. `event_timestamp` - the timestamp at which the event was recorded.
2. `driver_id` - the ID of the driver who the data was collected for.
3. `conv_rate`, `acc_rate`, `avg_daily_trips` - feature columns that will most likely be used to train models.
4. `created` - the timestamp at which each record was created.
5. `string_feature`.


We split the dataset into two parts - one contains the feature `conv_rate`, while the other contains the features `acc_rate` and `avg_daily_trips`. The columns `event_timestamp`, `driver_id`, `created`, and `string_feature` were preserved in each of the halves.

Additionally, we've dropped the feature rows for the event timestamp `2021-04-12 07:00:00+00:00`. This is because this is the only timestamp for `2021-04-12`. After this timestamp, event timestamps jump to `2021-08-31 18:00:00+00:00`.

We've also dropped the feature rows for the day `2021-08-31` because they don't start at `00:00:00+00:00`. Similarly, we dropped the timestamps after `2021-09-15 00:00:00+00:00`. This is because the app only allows you to select timestamp ranges by entering the year, month, and day. Hours, minutes, and seconds are not supported.

You can find the code used to modify the original dataset in `data_preparation.ipynb`. In `data_exploration.ipynb`, you can learn how we explored the timestamp ranges in the dataset.
