**Exercício 1: Realimentação e Feedforward juntos**
```mermaid
graph LR
    R["R(s)"] --> |+| S1(("+"))
    S1 --> G1["G1(s)"]
    G1 --> A(("A"))
    
    A --> |+| S2(("+"))
    A --> G2["G2(s)"]
    G2 --> |+| S2
    
    S2 --> Y["Y(s)"]
    
    Y --> H["H(s)"]
    H --> |-| S1
```

**Exercício 2: Malhas em Cascata Aninhadas**
```mermaid
graph LR
    U["U(s)"] --> |+| S1(("+"))
    
    S1 --> G1["G1(s)"]
    G1 --> B(("B"))
    
    B --> |+| S2(("+"))
    S2 --> G2["G2(s)"]
    G2 --> Y["Y(s)"]
    
    B --> H1["H1(s)"]
    H1 --> |-| S1
    
    Y --> |-| S2
```

**Exercício 3: Malhas Concêntricas (Mesmo Nível)**
```mermaid
graph LR
    R["R(s)"] --> |+| S1(("+"))
    S1 --> |+| S2(("+"))
    S2 --> G1["G1(s)"]
    G1 --> A(("A"))
    A --> G2["G2(s)"]
    G2 --> Y["Y(s)"]
    
    %% Realimentação Interna
    A --> H1["H1(s)"]
    H1 --> |-| S2
    
    %% Realimentação Externa
    Y --> H2["H2(s)"]
    H2 --> |-| S1
```

**Exercício 4: Malhas Sobrepostas (Mais Complexo)**
```mermaid
graph LR
    R["R(s)"] --> |+| S1(("+"))
    S1 --> G1["G1(s)"]
    G1 --> |+| S2(("+"))
    S2 --> G2["G2(s)"]
    G2 --> A(("A"))
    A --> G3["G3(s)"]
    G3 --> Y["Y(s)"]
    
    %% Realimentação 1 (Cruza por cima do Somador 2)
    A --> H1["H1(s)"]
    H1 --> |-| S1
    
    %% Realimentação 2
    Y --> H2["H2(s)"]
    H2 --> |-| S2
```
