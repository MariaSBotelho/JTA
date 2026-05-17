# JTA - Resumo Rápido

## O que é JTA?

A :contentReference[oaicite:0]{index=0} é a especificação Java responsável por controlar transações.

Ela garante que:
- tudo seja salvo corretamente
- ou nada seja salvo em caso de erro

---

# Conceitos principais

## Transação

Conjunto de operações executadas como uma única unidade.

Exemplo:

```txt
Salvar pedido
Atualizar estoque
Gerar pagamento
```

Se algo falhar:
- tudo volta atrás

---

## Commit

Confirma alterações no banco.

```txt
Tudo deu certo → salva definitivamente
```

---

## Rollback

Desfaz alterações.

```txt
Erro aconteceu → cancela tudo
```

---

# Annotation mais importante

## `@Transactional`

Indica que o método deve rodar dentro de uma transação.

```java
@Transactional
public void salvar() {
    manager.persist(produto);
}
```

Fluxo automático:

```txt
inicia transação
      ↓
executa método
      ↓
erro?
 ↓       ↓
sim      não
 ↓         ↓
rollback  commit
```

---

# Container Managed Transaction

O servidor/framework controla a transação automaticamente.

Exemplo:
- :contentReference[oaicite:1]{index=1}
- :contentReference[oaicite:2]{index=2}
- :contentReference[oaicite:3]{index=3}

Mais usado no mercado.

---

# Bean Managed Transaction

Controle manual da transação.

```java
userTransaction.begin();

manager.persist(produto);

userTransaction.commit();
```

Também pode usar:

```java
userTransaction.rollback();
```

Usado apenas em casos específicos.

---

# Interceptor

Código automático executado antes/depois do método.

No `@Transactional`:

```txt
antes → abre transação
depois → commit ou rollback
```

---

# Tipos de transação

## REQUIRED (mais comum)

```txt
Se não existe transação → cria
Se existe → reutiliza
```

---

## REQUIRES_NEW

```txt
Sempre cria nova transação
```

---

## SUPPORTS

```txt
Pode executar com ou sem transação
```

---

## NEVER

```txt
Nunca executa em transação
```

---

# Rollback e Exceptions

## RuntimeException

Filha de:

```java
RuntimeException
```

Por padrão:
- faz rollback

```java
throw new RuntimeException();
```

---

## Checked Exception

Filha de:

```java
Exception
```

Por padrão:
- NÃO faz rollback

```java
class MinhaException extends Exception
```

---

# dontRollbackOn

Impede rollback para exceptions específicas.

```java
@Transactional(
   dontRollbackOn = {
      SemEstoqueException.class
   }
)
```

---

# Spring e JTA

O :contentReference[oaicite:4]{index=4} também usa:

```java
@Transactional
```

Mesmo fora de servidores Java EE.

---

# O que realmente preciso saber

## 1. `@Transactional`

Cria contexto transacional automaticamente.

---

## 2. Commit

Salva alterações.

---

## 3. Rollback

Desfaz alterações.

---

## 4. RuntimeException

Normalmente faz rollback.

---

## 5. Checked Exception

Normalmente NÃO faz rollback.

---

## 6. Container Managed Transaction

Framework controla tudo automaticamente.

---

# Resumo mental

```txt
@Transactional
       ↓
abre transação
       ↓
executa código
       ↓
erro?
 ↓        ↓
sim       não
 ↓          ↓
rollback   commit
```
