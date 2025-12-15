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


Previous-row logic does NOT force apply or apply + lambda.
Use shift, diff, rolling, expanding whenever available


How to know in future (Decision Rule)

Ask:


Can I express this using:

shift()
diff()
rolling()
cumsum()
np.where()

If YES → Vectorize
If NO → apply
