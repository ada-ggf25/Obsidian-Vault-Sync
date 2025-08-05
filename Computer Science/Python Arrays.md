#computer_science #python

array = [1,  2,  3,  4, 5, 6, 7, 8, 9, 10]

|Objetivo|Sintaxe|Exemplo|
|---|---|---|
|Todos **menos** os x primeiros|`arr[x:]`|`arr[3:]` → `[4,5,…,10]`|
|Todos **menos** os x últimos|`arr[:-x]`|`arr[:-3]` → `[1,2,…,7]`|
|Um **intervalo** de elementos|`arr[start:end]`|`arr[2:5]` → `[3,4,5]`|
|Pegar **cada k-ésimo** elemento|`arr[::k]`|`arr[::2]` → `[1,3,5,7,9]`|
|Combinar intervalo + passo|`arr[start:end:step]`|`arr[1:8:2]` → `[2,4,6,8]`|


se eu tiver um array vazio posso pular o primeiro espaço e adicionar algo ao segundo espaço por exemplo?

### ChatGPT said:

Thought for a few seconds

Em Python não podes “saltar” posições num `list` (lista) sem antes garantir que essas posições existem — se fizeres:

```python
python
CopyEdit
lst = []
lst[1] = 'x'

```

recebes:

```
pgsql
CopyEdit
IndexError: list assignment index out of range

```

porque a lista está vazia (comprimento 0) e não há índice 1.

---

## Como “reservar” espaço até um índice

### 1. Estender com valores placeholder

Se realmente quiseres que a lista tenha, digamos, um “buraco” na posição 0, podes preencher até ao índice desejado com `None` (ou outro valor padrão):

```python
python
CopyEdit
lst = []
desired_index = 1

# Preenche posições 0 e 1 com None
lst.extend([None] * (desired_index + 1))

# Agora dá para atribuir em lst[1]
lst[1] = 'x'

print(lst)  # → [None, 'x']

```

### 2. Usar `insert` para deslocar

Se quisesse inserir um valor na posição 1 e “empurrar” tudo o que está depois (neste caso nada), podes usar:

```python
python
CopyEdit
lst = []
lst.insert(1, 'x')
print(lst)  # → ['x']

```

Observe que `insert(1, ...)` numa lista vazia insere na posição 0, porque não há posição 1 — o Python acaba por colocar o elemento no fim.

### 3. Construir até onde quiseres com `append`

Outra forma simples é garantir que a lista tem comprimento mínimo, por exemplo com `append`:

```python
python
CopyEdit
lst = []
lst.append(None)   # agora lst == [None]
lst.append('x')    # agora lst == [None, 'x']

```

---

## Quando usar um dicionário em vez de lista

Se precisas de um “array” esparso onde ocupas apenas certas chaves, pode ser mais adequado usar um `dict`:

```python
python
CopyEdit
d = {}
d[1] = 'x'
print(d)  # → {1: 'x'}

```

Aqui não existe “posição 0” se não a definires, e isso evita preencher manualmente “buracos”.

---

### Resumo

- **Listas (`list`)**: só podes atribuir a `lst[i]` se `0 ≤ i < len(lst)`. Para ir além, primeiro estende a lista (por exemplo com `extend([None]*(i+1))`) ou usa `append`/`insert`.
- **Dicionários (`dict`)**: permitem chaves inteiras arbitrárias sem precisar “reservar” posições intermédias.