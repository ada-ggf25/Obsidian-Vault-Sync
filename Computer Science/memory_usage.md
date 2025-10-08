#python #computer_theory #computer_science 

![b4fa292a-84d4-4a5b-8013-736d6901c22c.png](attachment:e33296f5-a58d-4abd-bce5-bdea466c057b:b4fa292a-84d4-4a5b-8013-736d6901c22c.png)

That’s the output of **`pandas.DataFrame.memory_usage(deep=True)`** for your DataFrame `sh`.

- It lists **how many bytes of RAM each column (and the Index) uses**.
- With **`deep=True`**, pandas also walks into **object-dtype** columns (e.g., strings, Python objects) and counts the memory of the Python objects themselves. Without `deep=True`, those would be under-counted.

How to read your numbers (they’re **bytes**):

- Columns with **80,000** bytes are almost certainly **numeric `int64`/`float64`** with **10,000 rows**:
    
    10,000 rows × 8 bytes/value = **80,000 bytes**.
    
- `gender` (540,037 B), `return_likelihood` (580,283 B), `complained` (514,950 B) are likely **object/string** columns. Strings have Python object overhead, so they use **much more** memory than pure numerics even with the same row count.
    
- `Index 132` is small because you probably have a **RangeIndex** (lightweight).
    

Quick conversions:

- 80,000 B ≈ 78.1 KB
- 580,283 B ≈ 567 KB

### Why some columns are much bigger

- **Numeric dtypes**: fixed size (usually 8 bytes per value for int64/float64).
- **Object dtypes** (strings like “male”/“female”, “Yes”/“No”): each entry is a separate Python object → extra overhead + variable string length.

### Tips to reduce memory

- Convert low-cardinality strings to **`category`**:
    
    ```python
    sh["gender"] = sh["gender"].astype("category")
    sh["complained"] = sh["complained"].astype("category")
    
    ```
    
- Downcast numerics when safe:
    
    ```python
    sh["age"] = pd.to_numeric(sh["age"], downcast="integer")
    sh["spent"] = pd.to_numeric(sh["spent"], downcast="float")
    
    ```
    
- Use **boolean** dtype for true/false data:
    
    ```python
    sh["complained"] = sh["complained"].map({"Yes": True, "No": False}).astype("boolean")
    
    ```
    

Re-run `sh.memory_usage(deep=True)` (or `sh.memory_usage(deep=True).sum()` for total) to see the savings.

---

You’re converting a string column to a **categorical** dtype to save RAM.

1. `sh.dtypes` shows current dtypes: several `object` columns (`gender`, `return_likelihood`, `complained`) plus numeric ones.
2. `sh.gender.value_counts()` confirms `gender` has only 3 distinct values (`female/other/male`). Low-cardinality strings are great candidates for categoricals.

```python
sh = sh.assign(
    gender=lambda df_: df_.gender.astype("category"),
)

```

replaces `gender` with a **Categorical** column. Internally pandas stores:

- a small integer array of **codes** (0,1,2, …) for each row
- a small **categories** table holding the unique strings once

This is much smaller than storing a separate Python string per row.

1. The `print(...)` compares memory before vs after:

```
Size: 1.43 MB compared to 1.96 MB

```

So converting `gender` to categorical cut ~0.53 MB. You could do the same for other low-cardinality `object` columns (`complained`, maybe `return_likelihood`) for more savings.

Handy extras:

- Access integer codes: `sh["gender"].cat.codes`
- Set category order: `sh["gender"] = sh["gender"].cat.set_categories(["male","female","other"], ordered=True)`
- Categoricals usually speed up `groupby`, `value_counts`, and `merge` on that column.

Caveats:

- Don’t use for **high-cardinality** text (nearly unique strings) — memory may not improve.
- If you later add new unseen values, you may need `add_categories` or `astype('string')` first.

---

![c9208c34-d1b0-4dd7-b608-8fca9f6d3707.png](attachment:f5ff9b72-7af3-4669-a230-43e35c48a68d:c9208c34-d1b0-4dd7-b608-8fca9f6d3707.png)

You’re converting a string column to a **categorical** dtype to save RAM.

1. `sh.dtypes` shows current dtypes: several `object` columns (`gender`, `return_likelihood`, `complained`) plus numeric ones.
2. `sh.gender.value_counts()` confirms `gender` has only 3 distinct values (`female/other/male`). Low-cardinality strings are great candidates for categoricals.

```python
sh = sh.assign(
    gender=lambda df_: df_.gender.astype("category"),
)

```

replaces `gender` with a **Categorical** column. Internally pandas stores:

- a small integer array of **codes** (0,1,2, …) for each row
- a small **categories** table holding the unique strings once

This is much smaller than storing a separate Python string per row.

1. The `print(...)` compares memory before vs after:

```
Size: 1.43 MB compared to 1.96 MB

```

So converting `gender` to categorical cut ~0.53 MB. You could do the same for other low-cardinality `object` columns (`complained`, maybe `return_likelihood`) for more savings.

Handy extras:

- Access integer codes: `sh["gender"].cat.codes`
- Set category order: `sh["gender"] = sh["gender"].cat.set_categories(["male","female","other"], ordered=True)`
- Categoricals usually speed up `groupby`, `value_counts`, and `merge` on that column.

Caveats:

- Don’t use for **high-cardinality** text (nearly unique strings) — memory may not improve.
- If you later add new unseen values, you may need `add_categories` or `astype('string')` first.