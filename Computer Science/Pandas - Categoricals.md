#python #computer_science 

In pandas, **categoricals** are a special dtype (`category`) for columns with a **limited set of repeated values** (e.g., `"male"`, `"female"`, `"other"`; `"Yes"/"No"`; ZIP codes; product IDs).

### How they’re stored

Internally a categorical column =

- an **array of integer codes** (0, 1, 2, …) for each row, and
    
- a small **categories** list mapping codes → labels.
    
    This saves RAM vs storing thousands of duplicate Python strings.
    

### Why use them

- **Memory**: big savings for low-cardinality text.
- **Speed**: faster `groupby`, `value_counts`, `merge`, sorting, comparisons.
- **Order semantics**: you can define an **ordered** category for proper sorting/comparisons (e.g., `"low" < "med" < "high"`).

### Quick usage

```python
import pandas as pd

df["gender"] = df["gender"].astype("category")             # convert
df["size"] = pd.Categorical(df["size"],
                            categories=["S","M","L","XL"], # fix universe
                            ordered=True)                  # give order

df["gender"].cat.categories       # view categories
df["gender"].cat.codes            # underlying integer codes
df["gender"] = df["gender"].cat.add_categories(["unknown"])
df["gender"] = df["gender"].cat.remove_unused_categories()

```

### When it helps

- Few unique values compared to number of rows (low cardinality).
- Repeated joins/aggregations on that column.

### Caveats

- **High-cardinality** text (almost all unique) → little/no benefit.
- New unseen labels need adding (`add_categories`) before assignment.
- Some operations auto-cast back to `object` or may need `.astype("string")` when doing string methods heavily.
- Be mindful when saving/loading (pickles preserve categories; CSVs don’t).

### Not the same as one-hot

Categoricals **compress & label**; one-hot encoding turns categories into many binary columns (for ML models). You can one-hot later with `pd.get_dummies(df["cat"])`.

**Bottom line:** use `category` for repeated labels to cut memory and speed up group/merge operations, especially in large DataFrames.

---

Pretty much, yes — with a couple nuances:

- A **categorical** column stores your repeated strings as:
    1. an **integer code array** (one int per row: 0,1,2,…) and
    2. a **categories array** (the unique labels, e.g. `["female","male","other"]`).
- You can think of it like a compact mapping **codes → labels**. It’s not literally a Python dict per row, but functionally similar: the codes reference the label table.
- Benefits: big **memory savings** and often **faster** `groupby`, `merge`, `value_counts`. You can also make it **ordered** (e.g., `"low" < "med" < "high"`).

Mini example:

```python
s = pd.Series(["male","female","male","other"]).astype("category")
s.cat.codes        # →  [1, 0, 1, 2]
s.cat.categories   # →  Index(['female','male','other'], dtype='object')

```

So yes: categoricals turn many repeated strings into a small label table + integer codes.