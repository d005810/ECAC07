# Aulas 3 e 4 – Modelagem de Circuitos Elétricos
### ECAC07 – Modelagem de Sistemas Dinâmicos (UNIFEI)

---

# PARTE 1 — Resumo explicado

## Aula 3 — Modelagem de Circuitos Elétricos (I)

### Por que resistores, capacitores e indutores se comportam de forma tão diferente?

A diferença fundamental é **energia**:

- O **resistor** não armazena energia — ele só a dissipa como calor. Por isso sua relação tensão-corrente é *instantânea* e *algébrica*: R = v(t)/i(t). Não existe "memória" no resistor: a tensão em um instante depende só da corrente naquele mesmo instante.
- O **capacitor** armazena energia em um campo elétrico (entre suas placas). Como a carga acumulada é q(t) = C·v(t) e a corrente é a taxa de variação da carga (i = dq/dt), a relação corrente-tensão do capacitor envolve uma **derivada**: i(t) = C·dv(t)/dt. Isso significa que a tensão no capacitor "lembra" do que aconteceu no passado (é a integral da corrente).
- O **indutor** armazena energia em um campo magnético. A tensão induzida é proporcional à *taxa de variação* da corrente: v(t) = L·di(t)/dt.

**Essa é exatamente a razão pela qual circuitos com C e/ou L são *sistemas dinâmicos* (Aula 1):** a presença de derivadas (ou integrais) na relação entrada-saída significa que a saída depende da história passada do sinal, não só do seu valor instantâneo — e por isso o **estado** de um circuito RLC é justamente o conjunto de tensões nos capacitores e correntes nos indutores (veremos isso formalmente na Aula 4).

| Elemento | Equação (tempo) | Armazena energia? |
|---|---|---|
| Resistor | v = Ri | Não (dissipa) |
| Capacitor | i = C·dv/dt , ou v = (1/C)∫i dτ | Sim (campo elétrico) |
| Indutor | v = L·di/dt , ou i = (1/L)∫v dτ | Sim (campo magnético) |

### Leis de Kirchhoff

Toda a modelagem de circuitos parte de apenas duas leis:

- **Lei das Correntes (LKC / nós):** a soma algébrica das correntes que entram e saem de um nó é zero. (Fisicamente: carga não se acumula em um fio.)
- **Lei das Tensões (LKT / malhas):** a soma algébrica das tensões ao longo de qualquer malha fechada é zero. (Fisicamente: energia potencial é conservativa.)

**Receita geral:** escreva a(s) equação(ões) de malha ou de nó → substitua as relações de cada elemento (R, L, C) → obtenha uma equação diferencial → aplique Laplace (condições iniciais nulas) → isole a razão saída/entrada = função de transferência.

**Padrão que se repete:** quanto mais elementos armazenadores de energia (L's e C's) *independentes* existem no circuito, maior a ordem da equação diferencial resultante (e do denominador da função de transferência). Um circuito RC de 1ª ordem dá uma função de transferência com denominador de grau 1; um circuito RLC de 2ª ordem dá grau 2.

### Impedância complexa — a grande simplificação

Aplicando Laplace diretamente nas equações dos elementos (com condições iniciais nulas):

| Elemento | Impedância Z(s) = V(s)/I(s) |
|---|---|
| Resistor | Z(s) = R |
| Capacitor | Z(s) = 1/(Cs) |
| Indutor | Z(s) = Ls |

**Por que isso é poderoso:** no domínio de Laplace, *todo* elemento passa a se comportar como uma "resistência generalizada". Isso significa que **todas as regras que você já conhece de circuitos puramente resistivos** (série, paralelo, divisor de tensão, divisor de corrente) **continuam válidas**, bastando substituir R por Z(s):

- Série: Z_eq(s) = Z1(s) + Z2(s) + Z3(s) + ⋯
- Paralelo: 1/Z_eq(s) = 1/Z1(s) + 1/Z2(s) + 1/Z3(s) + ⋯

Ou seja: você **desenha o "circuito transformado"** (troca R por R, L por Ls, C por 1/(Cs)) e resolve como se fosse um circuito resistivo comum — sem precisar montar e resolver equações diferenciais no domínio do tempo. Essa é a estratégia usada para resolver o mesmo exemplo RLC series muito mais rápido do que pela via "equação diferencial → Laplace".

### Circuitos complexos (múltiplas malhas/nós)

Para circuitos com mais de uma malha ou nó, dois métodos sistemáticos:

**Método das malhas** (usa impedâncias e a LKT):
1. Substituir elementos por impedâncias e fontes/variáveis por suas transformadas de Laplace.
2. Admitir uma corrente de malha (com sentido definido) em cada malha.
3. Escrever a LKT para cada malha.
4. Resolver o sistema de equações simultâneas (tipicamente com a **regra de Cramer**).
5. Formar a função de transferência.

Um padrão útil emerge nas equações (compare com um sistema resistivo): cada equação de malha tem a forma

```
[Soma das impedâncias da malha k] · I_k(s) − [Soma das impedâncias compartilhadas com a malha adjacente] · I_adjacente(s) = [Soma das tensões de alimentação da malha k]
```

**Método dos nós** (usa admitâncias Y(s) = 1/Z(s) e a LKC): mesma lógica, mas trocando impedância por admitância, corrente de malha por tensão de nó, e LKT por LKC. As equações de nó seguem o padrão análogo:

```
[Soma das admitâncias no nó k] · V_k(s) − [Soma das admitâncias compartilhadas com o nó adjacente] · V_adjacente(s) = [Soma das correntes aplicadas ao nó k]
```

---

## Aula 4 — Modelagem de Circuitos Elétricos (II): Espaço de Estados

### Escolhendo o vetor de estados

Lembrando da Aula 1: o **estado** é a informação mínima que, junto com a entrada futura, determina univocamente a saída futura. Em circuitos elétricos, essa informação mínima é justamente **as variáveis associadas aos elementos que armazenam energia**:

- **v_C** (tensão no capacitor) — porque ela está associada à energia armazenada no campo elétrico: E = ½Cv_C²
- **i_L** (corrente no indutor) — porque ela está associada à energia armazenada no campo magnético: E = ½Li_L²

Duas regras orientam a escolha:

1. **Número mínimo:** normalmente, o número de variáveis de estado = número de elementos armazenadores de energia *independentes* = ordem da equação diferencial do sistema = grau do denominador da função de transferência.
2. **Independência linear:** se uma variável candidata pode ser escrita como combinação linear de outras já escolhidas (ex.: x₃ = 5x₁ + 4x₂), ela é redundante e a escolha deve ser refeita.

**Isso explica por que, por exemplo, dois indutores em série contam como *uma* variável de estado (a mesma corrente passa pelos dois — não são independentes), enquanto dois indutores em ramos diferentes do circuito (por onde passam correntes diferentes) contam como duas.** O mesmo vale para capacitores em paralelo (mesma tensão) versus capacitores em ramos diferentes.

### O procedimento de 5 passos

1. **Nomear todas as correntes** dos ramos do circuito.
2. **Escrever as equações diferenciais** para cada elemento armazenador de energia (L e C) — as variáveis que aparecem *derivadas* nessas equações são as variáveis de estado escolhidas.
3. **Expressar todas as outras variáveis** que aparecem nessas equações (tensões, correntes de outros ramos) **em função apenas das variáveis de estado e da entrada**, usando LKC/LKT.
4. **Escrever as equações de estado** substituindo o resultado do passo 3 nas equações do passo 2.
5. **Escrever a equação de saída** como combinação linear das variáveis de estado e da entrada.

O resultado final é sempre escrito na forma matricial:

```
ẋ = Ax + Bu
y  = Cx + Du
```

**Importante (mencionado no slide 9 da Aula 4): a representação em espaço de estados não é única** — variáveis de estado diferentes (mas igualmente válidas) levam a matrizes A, B, C, D diferentes, mas que descrevem o mesmo comportamento entrada-saída.

---

# PARTE 2 — Exercícios

## Exercício 1 — Circuito RC: tensão no resistor (variação do exemplo da Aula 3)

Na Aula 3 (slides 6-8), foi obtida a função de transferência V_C(s)/V(s) para este circuito RC série. Agora, obtenha **V_R(s)/V(s)** (tensão no *resistor*, não no capacitor).

**Diagrama do problema:**

```
       R
  ┌───[R]───┐
  │  i(t) → │
 v(t)      [C]  v_C(t)
  │         │
  └─────────┘
```

**Passo a passo:**

1. Já sabemos da Aula 3 que, pela LKT: v(t) − Ri(t) − v_C(t) = 0, e que, após aplicar Laplace com condições iniciais nulas:

V_C(s)/V(s) = 1/(RCs + 1)

2. Como v_R(t) = v(t) − v_C(t) (a mesma malha), no domínio de Laplace: **V_R(s) = V(s) − V_C(s)**

3. Substituindo:

V_R(s)/V(s) = 1 − V_C(s)/V(s) = 1 − 1/(RCs+1) = [(RCs+1) − 1]/(RCs+1)

**Resultado:**

V_R(s)/V(s) = **RCs / (RCs + 1)**

**Diagrama do resultado (circuito transformado / função de transferência):**

```
       R                          RCs
  ┌───[R]───┐            V(s) ──────────── V_R(s)
  │         │                     RCs+1
 V(s)      1/Cs   V_C(s)
  │         │
  └─────────┘
```

*Observação:* repare que esse resultado é um **filtro passa-alta** (para s→0, ou seja, frequências baixas/DC, o ganho tende a 0; para s→∞, tende a 1), enquanto V_C(s)/V(s) da Aula 3 é um passa-baixa — comportamento complementar, como esperado, já que V_R + V_C = V sempre.

---

## Exercício 2 — Circuito RL série: corrente pelo circuito (método da impedância)

**Diagrama do problema (circuito original, domínio do tempo):**

```
        R            L
  ┌────[R]────┬─────[L]────┐
  │        i(t) →           │
 v(t)                       │
  │                         │
  └─────────────────────────┘
```

**Diagrama do circuito transformado (impedâncias):**

```
        R            Ls
  ┌────[R]────┬─────[L]────┐
  │        I(s) →           │
 V(s)                       │
  │                         │
  └─────────────────────────┘
```

**Passo a passo:**

1. Como é um circuito série, a impedância equivalente é a **soma** das impedâncias: Z_eq(s) = R + Ls
2. Pela "lei de Ohm generalizada": V(s) = Z_eq(s)·I(s) = (R + Ls)·I(s)
3. Isolando:

**Resultado:**

I(s)/V(s) = **1 / (Ls + R)**

Note que chegamos direto nesse resultado **sem escrever nenhuma equação diferencial** — essa é a vantagem prática do método da impedância descrito na Aula 3.

---

## Exercício 3 — Circuito de duas malhas: tensão no capacitor (usa o exemplo da Aula 3)

Este é o **mesmo circuito** do exemplo de "Circuitos complexos" da Aula 3 (slides 19-23 e 26-29), com R1, R2, L e C. Já temos dois resultados prontos da Aula 3 para reaproveitar:

- Pelo **método das malhas** (slides 19-23): I2(s)/V(s) = LCs² / [(R1+R2)LCs² + (R1R2C+L)s + R1]
- Pelo **método dos nós** (slides 26-29): V_C(s)/V(s) = [(1/(R1R2C))s] / [(1/R1+1/R2)s² + ((L/(R1R2)+C)/(LC))s + 1/(R2LC)]

**Sua tarefa: obtenha V_C(s)/V(s) partindo do resultado de I2(s)/V(s) do método das malhas** (sem refazer todo o método dos nós), e confirme que bate com o resultado do método dos nós.

**Diagrama do problema:**

```
        R1                R2
  ┌────[R1]────┬────────[R2]────┐
  │          i1(t)→ │  i2(t)→    │
 v(t)             [L]           [C]  v_C(t)
  │                 │             │
  └─────────────────┴─────────────┘
```

**Passo a passo:**

1. A corrente I2(s) passa pelo ramo série de R2 e C. A tensão no capacitor é, pela lei do capacitor em impedância:

V_C(s) = I2(s) · [1/(Cs)]

2. Substituindo o I2(s)/V(s) já conhecido:

V_C(s)/V(s) = {I2(s)/V(s)} · {1/(Cs)} = {LCs² / [(R1+R2)LCs²+(R1R2C+L)s+R1]} · {1/(Cs)}

3. Simplificando (o Cs do denominador cancela um C e um s do numerador LCs²):

**Resultado:**

V_C(s)/V(s) = **Ls / [(R1+R2)LCs² + (R1R2C+L)s + R1]**

**Verificação:** multiplicando o resultado do método dos nós (dado acima) por R1R2LC no numerador e denominador, chega-se exatamente à mesma expressão — **os dois métodos concordam**, como deveria ser (a função de transferência de um circuito não depende de qual método você usa para calculá-la).

**Diagrama do resultado:**

```
                          Ls
         V(s) ──────────────────────────── V_C(s)
              (R1+R2)LCs² + (R1R2C+L)s + R1
```

---

## Exercício 4 — Circuito RLC série: representação em espaço de estados com saída v_L(t)

Use o mesmo circuito RLC série da Aula 3 (slides 9-11), mas agora obtenha a representação em **espaço de estados** (Aula 4), com saída sendo a **tensão no indutor** v_L(t) (diferente do exemplo da Aula 4, que usava saída i_R).

**Diagrama do problema:**

```
        L             R
  ┌───[L]────┬───────[R]────┐
  │    i(t) →│               │
 v(t)         │              [C]  v_C(t)
  │           │               │
  └───────────┴───────────────┘
```

**Passo a passo (seguindo os 5 passos da Aula 4):**

**1. Nomear correntes:** neste circuito série, só existe uma corrente, i(t), que passa por L, R e "carrega" C.

**2. Equações diferenciais dos elementos armazenadores de energia** (definem as variáveis de estado):

L·di/dt = v_L  →  variável de estado: **i** (corrente no indutor)

C·dv_C/dt = i  →  variável de estado: **v_C** (tensão no capacitor)

**3. Expressar as outras variáveis (aqui, v_L) em função dos estados e da entrada.** Pela LKT ao longo da malha:

v(t) − v_L − Ri − v_C = 0  →  **v_L = v(t) − Ri − v_C**

**4. Escrever as equações de estado**, substituindo v_L no passo 2:

L·(di/dt) = v(t) − Ri − v_C  →  **di/dt = −(R/L)i − (1/L)v_C + (1/L)v(t)**

C·(dv_C/dt) = i  →  **dv_C/dt = (1/C)i**

**5. Equação de saída** (y = v_L, já obtida no passo 3):

**y = v_L = −Ri − v_C + v(t)**

**Resultado final (forma matricial):**

```
⌈ i̇  ⌉   ⌈ −R/L   −1/L ⌉ ⌈ i  ⌉   ⌈ 1/L ⌉
⌊ v̇_C ⌋ = ⌊  1/C    0   ⌋ ⌊ v_C ⌋ + ⌊  0  ⌋ v(t)

              y = [ −R   −1 ] ⌈ i  ⌉ + [1] v(t)
                              ⌊ v_C ⌋
```

*Observação para conferir:* se você somar as três "tensões" que aparecem na equação de saída com as equações de estado, deve recuperar exatamente a equação diferencial de 2ª ordem do circuito RLC série obtida na Aula 3 (LC·d²v_C/dt² + RC·dv_C/dt + v_C = v(t)) — é um bom teste de consistência.

---

## Exercício 5 — Quantas variáveis de estado? (exercício conceitual)

Para cada circuito abaixo, **determine o número mínimo de variáveis de estado** e **justifique** (relacionando com os elementos armazenadores de energia independentes, como discutido na Aula 4, slides 5-8).

**a)**

```
        L1            L2
  ┌───[L1]────┬─────[L2]────┐
  │        i(t) →            │
 v(t)                       [R]
  │                          │
  └──────────────────────────┘
```

**b)**

```
        R             C1
  ┌────[R]────┬──────||──────┐
  │            │              │
 v(t)         [L]            [C2]
  │            │              │
  └────────────┴──────────────┘
```

**c)**

```
        R1                        R2
  ┌────[R1]────┬────[L]────┬────[R2]────┐
  │             │            │            │
 v(t)          [C1]         [C2]         [C3]
  │             │            │            │
  └─────────────┴────────────┴────────────┘
```

**Respostas e justificativas:**

**a)** L1 e L2 estão em **série** — a mesma corrente i(t) passa pelos dois, então i_L1 = i_L2 (não são linearmente independentes). **Número de variáveis de estado: 1** (basta i(t), a corrente comum).

**b)** Aqui temos um indutor L e dois capacitores C1, C2 — mas C1 e C2 estão em **ramos diferentes**, ou seja, com tensões potencialmente diferentes (não são forçados a ter a mesma tensão, pois L está entre o nó de C1 e o nó de C2). **Número de variáveis de estado: 3** (i_L, v_C1, v_C2), desde que se confirme que nenhuma é combinação linear das outras.

**c)** Três capacitores em ramos diferentes (cada um "vê" uma tensão de nó potencialmente distinta, separados pelo resistor R1, o indutor L, e o resistor R2) e um indutor. Nenhum elemento está em série ou paralelo direto com outro do mesmo tipo. **Número de variáveis de estado: 4** (v_C1, v_C2, v_C3, i_L).

*Dica geral:* a regra prática mais rápida é contar elementos armazenadores de energia (L's e C's) e depois verificar se algum par está em **série** (indutores) ou **paralelo** (capacitores) direto — esses pares "colapsam" em uma única variável de estado.

---

## Gabarito rápido

| Exercício | Resultado |
|---|---|
| 1 | V_R(s)/V(s) = RCs / (RCs+1) |
| 2 | I(s)/V(s) = 1 / (Ls+R) |
| 3 | V_C(s)/V(s) = Ls / [(R1+R2)LCs² + (R1R2C+L)s + R1] |
| 4 | ẋ = [[-R/L,-1/L],[1/C,0]]x + [1/L,0]ᵀv(t) ; y = [-R,-1]x + v(t) |
| 5a | 1 variável de estado (L's em série) |
| 5b | 3 variáveis de estado |
| 5c | 4 variáveis de estado |

Qualquer dúvida em algum passo da álgebra ou na leitura dos esquemáticos, me chame que resolvemos juntos.
