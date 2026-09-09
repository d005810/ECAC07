graph LR
    %% Definindo os nós e entradas
    R["R(s)"] -->|"+"| Sum1((&Sigma;))
    
    %% Caminho principal até o ponto A
    Sum1 --> G1["G1(s)"]
    G1 --> A((A))
    
    %% Ramificações a partir do ponto A
    A -->|"+"| Sum2((&Sigma;))
    A --> G2["G2(s)"]
    G2 -->|"+"| Sum2
    
    %% Saída
    Sum2 --> Y["Y(s)"]
    
    %% Malha de realimentação principal
    Y --> H["H(s)"]
    H -->|"-"| Sum1



graph LR
    %% Definindo a entrada
    U["U(s)"] -->|"+"| Sum1((&Sigma;))
    
    %% Primeira parte da cascata
    Sum1 --> G1["G1(s)"]
    G1 --> B((B))
    
    %% Segunda parte da cascata
    B -->|"+"| Sum2((&Sigma;))
    Sum2 --> G2["G2(s)"]
    G2 --> Y["Y(s)"]
    
    %% Malha de realimentação interna (sai de B)
    B --> H1["H1(s)"]
    H1 -->|"-"| Sum1
    
    %% Malha de realimentação externa (sai de Y direto para Sum2)
    Y -->|"-"| Sum2
