| Task                | Vectorized? | Tool              |
| ------------------- | ----------- | ----------------- |
| Column math         | ✅           | `+ - * /`         |
| If-else             | ✅           | `np.where`        |
| Multiple conditions | ✅           | `np.select`       |
| String ops          | ✅           | `.str`            |
| Date ops            | ✅           | `.dt`             |
| Group aggregation   | ✅           | `groupby().agg()` |
| Cross-column logic  | ❌           | `apply(axis=1)`   |
| Custom function     | ❌           | `apply()`         |
