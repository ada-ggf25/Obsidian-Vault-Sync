#computer_science #python 

Em Python, um **dicionário** (tipo `dict`) é uma coleção não ordenada de pares **chave–valor**, onde:

- **Chaves** podem ser de tipos imutáveis (strings, números, tuplas imutáveis, etc.).
- **Valores** podem ser de qualquer tipo (inclusive outros dicionários).

Por baixo dos panos, o `dict` usa uma **tabela de hash** para armazenar e procurar itens de forma muito eficiente (em média, acesso em tempo constante, O(1)).

---

## 1. Criação de um dicionário

```python
python
CopyEdit
# 1.1. Dicionário vazio
meu_dict = {}

# 1.2. Com valores iniciais
pessoa = {
    'nome': 'Ana',
    'idade': 28,
    'cidade': 'Porto'
}

# 1.3. Usando o construtor dict()
cores = dict(vermelho='#FF0000', verde='#00FF00', azul='#0000FF')

```

---

## 2. Acesso e atualização

```python
python
CopyEdit
# Acesso pelo índice (chave)
print(pessoa['nome'])   # → 'Ana'

# Se a chave não existir, KeyError:
# pessoa['sexo']  # KeyError

# Método seguro: get()
print(pessoa.get('sexo'))         # → None
print(pessoa.get('sexo', 'ND'))   # → 'ND' (valor padrão)

# Atualizar ou adicionar
pessoa['idade'] = 29        # atualiza
pessoa['sexo'] = 'Feminino' # adiciona

```

---

## 3. Remoção de itens

```python
python
CopyEdit
# pop(): remove e retorna o valor
idade = pessoa.pop('idade')    # idade == 29

# del: remove sem retornar
del pessoa['cidade']

# clear(): esvazia o dicionário
pessoa.clear()  # {}

```

---

## 4. Iteração

```python
python
CopyEdit
meu_dict = {'a': 1, 'b': 2, 'c': 3}

# 4.1. Percorrer chaves
for chave in meu_dict:
    print(chave)

# 4.2. Percorrer valores
for valor in meu_dict.values():
    print(valor)

# 4.3. Percorrer pares chave–valor
for chave, valor in meu_dict.items():
    print(f"{chave!r} → {valor}")

```

---

## 5. Principais métodos de dicionário

|Método|O que faz|
|---|---|
|`d.keys()`|Retorna uma _view_ das chaves|
|`d.values()`|Retorna uma _view_ dos valores|
|`d.items()`|Retorna uma _view_ de tuplas (chave, valor)|
|`d.get(chave[, padrão])`|Busca valor ou retorna padrão se não existir|
|`d.pop(chave[, padrão])`|Remove item e retorna valor ou padrão|
|`d.popitem()`|Remove e retorna o último par inserido (em 3.7+ é determinístico)|
|`d.clear()`|Remove todos os itens|
|`d.update(outro_dict)`|Atualiza com pares de outro dicionário ou iterável de pares|
|`d.copy()`|Cópia _shallow_ do dicionário|

---

## 6. Como funciona internamente (visão simplificada)

1. **Hashing**: a chave é passada por uma função de hash (`__hash__`), que gera um inteiro.
2. **Índice**: esse inteiro é mapeado a um índice na tabela interna.
3. **Colisões**: se duas chaves diferentes mapearem ao mesmo slot, Python usa sondagem linear ou encadeamento aberto para encontrar outro slot.
4. **Redimensionamento**: conforme o dicionário cresce (mais de ~2/3 cheio), ele realoca uma tabela maior e remapeia os itens para manter a eficiência.

---

### Exemplo completo

```python
python
CopyEdit
# Criar um inventário de loja
inventario = {
    'maçã': 50,
    'banana': 120,
    'laranja': 75
}

# Venda de 10 bananas
inventario['banana'] -= 10

# Adicionar um novo produto
inventario['kiwi'] = 40

# Mostrar estoque
for produto, qt in inventario.items():
    print(f"{produto.capitalize()}: {qt} unidades")

```

**Saída:**

```
makefile
CopyEdit
Maçã: 50 unidades
Banana: 110 unidades
Laranja: 75 unidades
Kiwi: 40 unidades

```

---

## 7. Dicas finais

- Use **tuplas** como chaves quando precisar de chaves compostas, por exemplo `d[(x, y)]`.
- Para operações de conjunto (união, interseção), você pode usar `d1.keys() & d2.keys()`, etc.
- Lembre-se de que `dict` em Python 3.7+ mantém a **ordem de inserção**, mas não use isso para lógica crítica de ordenação (prefira `collections.OrderedDict` se precisar de compatibilidade com versões anteriores).

Com isso, você tem as bases para usar e entender dicionários em Python!


```python
cores = dict(vermelho='#FF0000', verde='#00FF00', azul='#0000FF')

# 1) Usando colchetes (KeyError se a chave não existir)
print(cores['vermelho'])  # Saída: #FF0000

# 2) Usando get() (retorna None ou valor-padrão se a chave faltar)
print(cores.get('vermelho'))        # Saída: #FF0000
print(cores.get('amarelo', 'ND'))   # Saída: ND>)
```