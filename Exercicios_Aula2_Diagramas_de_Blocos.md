# Exercícios – Diagramas de Blocos
### ECAC07 – Modelagem de Sistemas Dinâmicos (UNIFEI)

---

## Exercício 1 — Série e Paralelo (aquecimento)

Considere um sistema com entrada R(s) e saída Y(s) formado por três blocos em cascata:
G₁(s) = 1/(s+1), G₂(s) = 2, G₃(s) = 1/s

**a)** Escreva a função de transferência equivalente Y(s)/R(s) se os blocos estiverem em **série**.

**b)** Escreva Y(s)/R(s) se os blocos estiverem em **paralelo**, todos somados na saída (todos com sinal +).

---

## Exercício 2 — Malha de realimentação simples

Um sistema tem um bloco direto G(s) = K/(s+2) e realimentação negativa unitária (H(s) = 1).

**a)** Desenhe o diagrama de blocos (com o somador, G(s) e a realimentação).

**b)** Encontre a função de transferência de malha fechada Y(s)/R(s).

**c)** Repita para realimentação **positiva**.

---

## Exercício 3 — Realimentação com H(s) ≠ 1

Sistema com bloco direto G(s) = 10/(s(s+5)) e realimentação negativa H(s) = (s+1).

Encontre Y(s)/R(s) na forma de uma única fração (numerador e denominador expandidos).

---

## Exercício 4 — Diagrama com múltiplos somadores (estrutura tipo "Aula 2")

Considere o seguinte diagrama, descrito passo a passo:

- Entrada U(s) entra em um somador **S1** com sinal **+**.
- A saída de S1 passa por **G1(s)** e depois por **G2(s)**, chegando a um segundo somador **S2** (sinal **+**).
- A saída de S2 passa por **G3(s)**, produzindo a saída **Y(s)**.
- **Realimentação 1**: a saída de G1(s) (antes de G2) é levada por um bloco **H1(s)** de volta ao somador **S1** com sinal **−**.
- **Realimentação 2**: a saída Y(s) é levada diretamente (ganho unitário) de volta ao somador **S2** com sinal **+**.

**a)** Desenhe o diagrama de blocos completo.

**b)** Reduza o diagrama passo a passo (indicando cada movimentação de bloco ou combinação série/paralelo/realimentação) até obter Y(s)/U(s).

*Dica: essa estrutura é semelhante (mas não idêntica) ao exemplo resolvido na aula — tente resolver sozinho antes de comparar com o exemplo do slide 14-19.*

---

## Exercício 5 — Sistema de controle com dois laços de realimentação (tipo "ônibus espacial")

Um sistema de controle de atitude tem a seguinte estrutura:

- Erro E(s) = R(s) − (realimentação principal) passa por um ganho **K₁**.
- Resultado entra em um segundo somador, subtraindo uma realimentação de **taxa** (bloco **s**, um "derivador"), e o resultado passa por um ganho **K₂**.
- Esse sinal entra em um terceiro somador, subtraindo uma realimentação de **aceleração** (bloco **s²**), e o resultado passa pelo controlador **C(s)** e pela planta **G(s)**, gerando a saída Y(s).
- A saída Y(s) realimenta:
  - diretamente (ganho 1) para o primeiro somador;
  - através do bloco **s** para o segundo somador;
  - através do bloco **s²** para o terceiro somador.

**a)** Desenhe esse diagrama (compare com a Aula 2, slide 5 — "Sistema de controle de arfagem do ônibus espacial").

**b)** Encontre a função de transferência de malha fechada Y(s)/R(s) em função de K₁, K₂, C(s) e G(s).

*Dica: resolva de dentro para fora — primeiro a malha mais interna (com s²), depois a próxima (com s), depois a externa.*

---

## Exercício 6 — Movendo um ponto de ramificação

Dado o diagrama:

- Entrada R(s) passa por **G(s)**.
- Depois de G(s), há um ponto de ramificação que gera **três saídas**: uma vai direto para fora (saída 1), outra também sai direto (saída 2), e uma terceira sai direto (saída 3) — ou seja, o ponto de ramificação está **depois** de G(s).

**a)** Redesenhe o diagrama de forma equivalente movendo o **ponto de ramificação para antes** de G(s) (ou seja, a ramificação ocorre em R(s), antes do bloco).

**b)** Que blocos adicionais são necessários nos ramos para manter a equivalência? Justifique com base na regra vista na Aula 2 (slides 12-13).

---

## Exercício 7 — Movendo um bloco através de um somador

Dado o diagrama:

- R(s) entra em um somador (+) junto com um sinal X(s) (sinal ± ), resultando em um sinal intermediário.
- Esse sinal intermediário passa por **G(s)**, gerando Y(s).

**a)** Redesenhe o diagrama de forma equivalente, movendo o bloco G(s) para **antes** do somador (de forma que G(s) atue sobre R(s) antes de somar com X(s)).

**b)** Qual bloco extra deve ser inserido no ramo de X(s) para que o diagrama permaneça equivalente? Por quê?

---

## Exercício 8 — Diagrama de simulação (domínio do tempo)

Para cada equação diferencial abaixo, desenhe o **diagrama de simulação** (usando blocos de integrador, ganho e somador — não função de transferência):

**a)** ẏ = 10x

**b)** ẏ + ay = bx

**c)** ÿ + a₁ẏ + a₀y = bx

**d)** ẏ + ay = ẋ + bx *(cuidado: há uma derivada de x — pense em como representar isso sem usar um bloco "derivador" diretamente; uma forma comum é reorganizar variáveis de estado)*

---

## Exercício 9 — Desafio (síntese Aula 1 + Aula 2)

Um sistema massa-mola-amortecedor é descrito por:

m·ẍ + b·ẋ + k·x = F(t)

onde F(t) é a força aplicada (entrada) e x(t) é o deslocamento (saída).

**a)** Escreva esse sistema em **espaço de estados**, definindo x₁ = x e x₂ = ẋ (relacione com o conceito de "estado" da Aula 1 — por que é necessário conhecer x e ẋ, e não apenas x, para determinar o comportamento futuro?).

**b)** Aplique a Transformada de Laplace (condições iniciais nulas) e encontre a função de transferência X(s)/F(s).

**c)** Desenhe o diagrama de blocos correspondente a essa função de transferência.

**d)** Desenhe também o diagrama de simulação (domínio do tempo) para esse mesmo sistema, usando dois integradores em cascata (um para obter ẋ a partir de ẍ, outro para obter x a partir de ẋ).

---

## Gabarito rápido (apenas respostas-chave para conferência)

1a) Y/R = G₁G₂G₃ = 2/[s(s+1)]
1b) Y/R = G₁+G₂+G₃ = 1/(s+1) + 2 + 1/s

2b) Y/R = G/(1+G) = K/(s+2+K)
2c) Y/R = G/(1−G) = K/(s+2−K)

3) Y/R = 10/[s(s+5) + 10(s+1)] = 10/(s² + 15s + 10)

5b) Y/R = K₁K₂C(s)G(s) / [1 + K₁K₂C(s)G(s)·1 + K₂C(s)G(s)·s + C(s)G(s)·s²]
(monte de dentro para fora: malha com s² primeiro, depois s, depois a externa)

9b) X(s)/F(s) = 1/(ms² + bs + k)

Se quiser, posso conferir suas resoluções passo a passo ou dar dicas adicionais em qualquer um desses exercícios.
