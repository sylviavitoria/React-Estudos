# Documentação Completa: React Básicos

---

## 🚀 Introdução ao React

React é uma biblioteca JavaScript para construir interfaces de usuário, especialmente aplicações single-page (SPA). Desenvolvido pelo Facebook, React utiliza uma abordagem declarativa e baseada em componentes.

### Principais conceitos:
- **Virtual DOM**: Representação virtual da interface que otimiza atualizações do DOM real
- **JSX**: Sintaxe que permite escrever HTML dentro do JavaScript
- **Unidirectional Data Flow**: Fluxo de dados unidirecional para maior previsibilidade
- **Component-Based**: Aplicação construída através de componentes reutilizáveis

---

## 🧱 Componentes

### O que são Componentes?

Componentes são blocos de construção fundamentais do React. Eles são como funções JavaScript que aceitam entradas (props) e retornam elementos React que descrevem o que deve aparecer na tela.

### Tipos de Componentes

#### 1. Componentes Funcionais (Recomendado)
```jsx
// Componente simples
function Welcome({ name }) {
  return <h1>Olá, {name}!</h1>;
}

// Arrow function
const Welcome = ({ name }) => <h1>Olá, {name}!</h1>;
```

#### 2. Componentes de Classe (Legacy)
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Olá, {this.props.name}!</h1>;
  }
}
```

### Props (Propriedades)

Props são argumentos passados para componentes React. Elas são como parâmetros de função.

```jsx
function Card({ title, description, isActive }) {
  return (
    <div className={isActive ? 'card active' : 'card'}>
      <h2>{title}</h2>
      <p>{description}</p>
    </div>
  );
}

// Uso do componente
<Card 
  title="Meu Card" 
  description="Descrição do card" 
  isActive={true} 
/>
```

### JSX (JavaScript XML)

JSX é uma extensão de sintaxe para JavaScript que permite escrever elementos HTML dentro do código JavaScript.

```jsx
// JSX
const element = <h1>Hello, World!</h1>;

// Equivalente em JavaScript puro
const element = React.createElement('h1', null, 'Hello, World!');
```

**Regras importantes do JSX:**
- Use `className` ao invés de `class`
- Use `htmlFor` ao invés de `for`
- Elementos devem ser fechados: `<input />` ou `<input></input>`
- Componentes devem retornar um único elemento pai

---

## 🧠 Conceito Geral dos Hooks

React Hooks são funções que permitem que componentes funcionais "lembrem" informações, realizem efeitos colaterais, e compartilhem lógica reutilizável, sem a necessidade de classes.

### Por que Hooks?
- **Reutilização de lógica**: Compartilhar lógica entre componentes sem HOCs ou render props
- **Simplicidade**: Componentes funcionais são mais simples que classes
- **Melhor performance**: Otimizações mais fáceis de implementar

---

## 1. `useState`

### O que é?

`useState` permite que um componente **"lembre" e gerencie informações** (estado) entre renderizações.  
Por exemplo: contadores, valores de input, status de loading, etc.

---

### Sintaxe básica

```tsx
const [state, setState] = useState(valorInicial);
```

- **state**: valor atual do estado
- **setState**: função usada para atualizar o estado

### Exemplo simples

```jsx
import { useState } from 'react';

function App() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(prevCount => prevCount + 1);
    console.log(count); // mostra o valor antigo porque a atualização é assíncrona
  };

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default App;
```

### Tipos comuns para useState

| Tipo | Exemplo |
|------|---------|
| Número | `useState(0)` |
| String | `useState('')` |
| Booleano | `useState(true)` |
| Objeto | `useState({ nome: '', idade: 0 })` |
| Array | `useState([])` |
| Null/Undefined | `useState(null)` ou `useState(undefined)` |

### ⚠️ Atualização de estado é assíncrona

```js
setCount(count + 1);
console.log(count); // ainda mostra o valor antigo!
```

O valor atualizado só estará disponível na próxima renderização.

---

## 2. useEffect

### O que é?

`useEffect` gerencia o ciclo de vida do componente, permitindo executar efeitos colaterais:

- Quando o componente é montado (criado)
- Quando o componente é atualizado (estado ou props mudam)
- Quando o componente é desmontado (destruído)

### Sintaxe básica

```jsx
useEffect(() => {
  // Código executado no ciclo de vida

  return () => {
    // Código para limpeza (destruição)
  };
}, [dependências]);
```

O array de dependências (`[]`) controla quando o efeito roda:

- `[]`: roda uma vez ao montar
- `[var]`: roda sempre que `var` mudar
- Sem array: roda em toda renderização (não recomendado)

### Exemplo com criação e destruição

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [value, setValue] = useState('valor inicial');
  const [checked, setChecked] = useState(false);

  useEffect(() => {
    console.log('useEffect executado');

    return () => {
      console.log('destruindo componente');
    };
  }, []); // executa só na montagem e desmontagem

  return (
    <div>
      <input
        type="text"
        value={value}
        onChange={e => setValue(e.target.value)}
      />

      <input
        type="checkbox"
        checked={checked}
        onChange={e => setChecked(e.target.checked)}
      />
    </div>
  );
}

export default App;
```

### Usos comuns

- Buscar dados (API) ao montar o componente
- Assinar eventos (e removê-los na limpeza)
- Configurar timers
- Sincronizar estado com armazenamento local

---

## 3. useContext

### O que é?

`useContext` permite compartilhar estado globalmente entre vários componentes, sem precisar passar props manualmente por vários níveis.

### Exemplo básico

```jsx
import React, { createContext, useContext, useState } from 'react';

const UserContext = createContext();

function App() {
  const [user, setUser] = useState({ nome: 'Sylvia' });

  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Profile />
    </UserContext.Provider>
  );
}

function Profile() {
  const { user } = useContext(UserContext);
  return <h1>Olá, {user.nome}!</h1>;
}

export default App;
```

---

## 🔄 Ciclo de Vida dos Componentes

### Com Hooks vs Classes

**Componentes de Classe (Antigo):**
```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    // Componente foi montado
  }
  
  componentDidUpdate() {
    // Componente foi atualizado
  }
  
  componentWillUnmount() {
    // Componente será desmontado
  }
}
```

**Com useEffect (Moderno):**
```jsx
function MyComponent() {
  useEffect(() => {
    // Equivale a componentDidMount e componentDidUpdate
    
    return () => {
      // Equivale a componentWillUnmount
    };
  }, []); // Array vazio = só na montagem
}
```

---

## 🎯 Boas Práticas

### 1. Nomenclatura de Componentes
- Sempre use PascalCase: `MyComponent`
- Nome descritivo: `UserProfile` ao invés de `Profile`

### 2. Estrutura de Arquivos
```
components/
├── UserProfile/
│   ├── index.jsx
│   ├── UserProfile.module.css
│   └── __tests__/
```

### 3. Props e Estado
- Prefira componentes controlados para inputs
- Mantenha o estado o mais próximo possível de onde é usado
- Use props para comunicação entre componentes

### 4. Performance
- Use React.memo para componentes que rerenderizam frequentemente
- Extraia funções que não dependem do estado para fora do componente

```jsx
// ❌ Ruim - função recriada a cada render
function MyComponent({ items }) {
  const handleClick = () => {
    console.log('clicked');
  };
  
  return <button onClick={handleClick}>Click</button>;
}

// ✅ Bom - função externa
const handleClick = () => {
  console.log('clicked');
};

function MyComponent({ items }) {
  return <button onClick={handleClick}>Click</button>;
}
```

---

## Resumo Geral

| Hook | Objetivo | Exemplo de uso |
|------|----------|----------------|
| `useState` | Gerenciar estado local (estado atômico) | Controlar inputs, contadores |
| `useEffect` | Gerenciar efeitos colaterais e ciclo de vida | Buscar dados, timers, limpeza |
| `useContext` | Compartilhar estado entre múltiplos componentes | Estado global, temas, auth |

---

## 📚 Próximos Passos

Após dominar estes hooks básicos, você pode explorar:
- **useReducer**: Para estado complexo
- **useMemo**: Para otimização de performance
- **useCallback**: Para memorização de funções
- **Custom Hooks**: Para lógica reutilizável
- **Libraries**: React Router, Redux Toolkit, React Query
