#computer_science #frontend #deploy 

**React Hooks** são funções especiais introduzidas no React 16.8 que permitem **usar o estado e outras funcionalidades do React em componentes funcionais**, sem a necessidade de escrever componentes de classe.

---

## 🧠 Por que foram criados?

Antes dos Hooks, se quisesses usar **estado**, **ciclo de vida** ou **contexto**, tinhas de escrever componentes com `class`. Isso tornava o código mais complexo e menos reutilizável.

Os Hooks resolveram isso com uma abordagem **mais simples, funcional e modular**.

---

## 🎣 Os Hooks mais comuns

### 1. **`useState`**

Permite adicionar estado local a componentes funcionais.

```tsx

import { useState } from 'react';

function Contador() {
  const [contador, setContador] = useState(0);

  return (
    <button onClick={() => setContador(contador + 1)}>
      Clicaste {contador} vezes
    </button>
  );
}

```

---

### 2. **`useEffect`**

Executa efeitos colaterais (como `componentDidMount`, `componentDidUpdate`, etc.).

```tsx

import { useEffect } from 'react';

useEffect(() => {
  console.log('O componente montou!');
  return () => {
    console.log('O componente vai desmontar');
  };
}, []);

```

---

### 3. **`useContext`**

Acede a um contexto React.

```tsx

const TemaContext = createContext("claro");

function Botao() {
  const tema = useContext(TemaContext);
  return <button className={tema}>Botão</button>;
}

```

---

### 4. **`useRef`**

Cria uma referência mutável que não causa re-render quando muda.

```tsx

const inputRef = useRef<HTMLInputElement>(null);

<input ref={inputRef} />
<button onClick={() => inputRef.current?.focus()}>Focar</button>

```

---

### 5. **`useCallback` / `useMemo`**

Otimizações de performance: evitam re-renderizações desnecessárias.

```tsx

const memorizedCallback = useCallback(() => {
  fazerAlgo();
}, [dependencias]);

const valorMemo = useMemo(() => {
  return calcularCaro();
}, [dependencias]);

```

---

### 6. **`useReducer`**

Alternativa ao `useState` para lógica de estado mais complexa (como Redux).

---

## 🔁 Custom Hooks

Podes criar os teus próprios hooks reutilizáveis:

```tsx

function useContador(inicial = 0) {
  const [contador, setContador] = useState(inicial);
  const incrementar = () => setContador((c) => c + 1);
  return { contador, incrementar };
}

```

---

## ✅ Vantagens dos Hooks

- Código mais **limpo** e **modular**.
- Melhora a **reutilização de lógica de estado**.
- Elimina a necessidade de classes (`class`).
- Funciona melhor com **TypeScript** e novas bibliotecas (como React Server Components).

---

Se quiseres, posso dar-te uma lista de **hooks úteis para projetos com Next.js**, ou até criar **hooks personalizados para autenticação, fetch de dados ou manipulação de formulários.** Queres?