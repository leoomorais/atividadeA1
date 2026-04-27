# Estudo de Caso A1 — O Problema da Entrega Inteligente

## Questão 1

### A)
O problema de roteamento da FastBite pertence à classe NP-Completo. Porque, em casos com apenas 1 ou 2 pedidos, pode parecer simples de encontrar uma solução, mas dentro de um cenário real com muitos pedidos e entregadores a escala de possibilidades a serem calculadas por um algoritmo foge do tempo polinomial, sendo praticamente impossível chegar a uma solução. E uma outra característica dos problemas NP-Completo é que, mesmo que cheguemos a uma solução, também seria praticamente impossível conferir se a solução apresentada é válida ou se é realmente a melhor.

### B)
Para reduzir o problema da FastBite para o Problema do Caixeiro Viajante devemos observar apenas 1 entregador e seu conjunto de pedidos, a partir dessa simplificação a localização atual do entregador, dos restaurantes e os endereços de entrega passam a ser representações das cidades. O objetivo então passa a ser encontrar a ordem de visita de cada ponto (cidades) para determinar a rota com a menor distância ou tempo total, com uma diferença que é necessário respeitar a ordem de coleta e entrega. Dessa forma, o problema de calcular a rota de entrega dos pedidos passa a ser uma representação típica do Problema do Caixeiro Viajante.

### C)
Um algoritmo de força bruta é inviável para a solução do problema da FastBite, porque o algoritmo necessário para fazer os cálculos seria de complexidade fatorial O(n!), o que escala de forma gigantesca ao adicionar um novo pedido ou entregador, podendo facilmente fugir do tempo polinomial, além de ser necessário refazer todo o cálculo. Considerando o cenário proposto de 8 pedidos e 3 entregadores, podemos primeiro reduzir a 2 entregadores com 3 pedidos e 1 entregador com 2 pedidos, vamos primeiro calcular o número de rotas possíveis:

2 pedidos = 2! = 2  
3 pedidos = 3! = 6  

juntando os entregadores:

6 x 6 x 2 = 72 possibilidades

Entretanto, esse cálculo foi feito levando em consideração uma atribuição prévia dos pedidos aos entregadores. Se incluirmos o cálculo combinatório, o número de possibilidades cresce exponencialmente, o que torna a abordagem da força bruta inviável na prática, ainda mais se levarmos em consideração o tempo de resposta de até 2 segundos.

## Questão 2

### A)
Para cada pedido não atribuído, o algoritmo percorre a lista de entregadores disponíveis e calcula a distância entre a posição atual de cada entregador e o restaurante. Em seguida, seleciona o entregador mais próximo e atribui o pedido a ele, respeitando a capacidade máxima de carga. Esse processo é repetido até que todos os pedidos estejam atribuídos.

Após a atribuição dos pedidos, o algoritmo define a rota individual de cada entregador. Partindo da posição atual do entregador e, a cada passo, escolhe o próximo ponto.

Processo a cada passo:

O entregador vai até o ponto mais próximo  
Atualiza sua posição para esse novo ponto  
Repete a escolha do próximo ponto mais próximo  

Dessa forma, o algoritmo toma decisões baseadas na localização atual do entregador.

### B)
O algoritmo é classificado como guloso (greedy) porque, a cada etapa, ele toma decisões baseadas apenas na melhor escolha local, sem considerar o impacto global da solução. Na fase de atribuição, ele escolhe, para cada pedido, o entregador mais próximo do restaurante naquele momento. Já na fase de roteamento, o entregador sempre se desloca para o próximo ponto mais próximo da sua posição atual.

### C)
Criando um cenário de 2 pedidos A e B onde os restaurantes (pontos de coleta) são próximos e o endereço de entrega do pedido B é no caminho para o endereço de entrega do pedido A. O algoritmo criaria a rota partindo do restaurante A para o endereço de entrega A, voltando para o restaurante B e então para o endereço de entrega do pedido B. Nesse caso, a rota não seria a mais otimizada, pois recolher os dois pedidos e então fazer a entrega em sequência seria o ideal, mas como o algoritmo guloso toma decisões a partir do local, essa seria uma otimização global que ele não analisaria.

### D)
A análise de complexidade desse algoritmo pode ser dividida em 2 partes. A primeira parte seria a atribuição dos pedidos, onde temos uma complexidade O(n x m), pois ele teria que iterar uma lista de pedidos com uma lista de entregadores para fazer a combinação de pedidos x entregadores. Na segunda parte, ele faria uma iteração entre os pontos ainda não visitados a partir da localização atual para verificar o mais próximo, que seria aproximadamente O(n^2). Entretanto, levando em consideração o número máximo de 3 pedidos por entregador, a complexidade O(n x m) seria predominante em um cenário real com vários entregadores.

## Questão 3

### A)
A Programação Dinâmica (PD) é aplicável ao problema de roteamento de um único entregador com k pedidos, pois esse problema pode ser modelado como uma variação do Problema do Caixeiro Viajante (TSP), que possui solução conhecida via PD. O subproblema consiste em determinar o menor custo para visitar um subconjunto de pontos (coletas e entregas) e terminar em um ponto específico. A solução final é construída a partir da combinação dessas soluções parciais. A complexidade de tempo dessa abordagem é O(2^k · k²) e o consumo de memória é O(2^k · k), o que ainda caracteriza um crescimento exponencial.

### B)
A abordagem de Divisão e Conquista pode ser parcialmente aplicada ao problema da FastBite ao dividir a cidade em regiões geográficas (como zonas ou quadrantes) e resolver o problema de atribuição e roteamento separadamente em cada região. Essa estratégia é eficaz quando os pedidos e entregadores estão bem distribuídos geograficamente, permitindo que cada subproblema seja tratado de forma relativamente independente, reduzindo o custo computacional. No entanto, essa independência não é total. Um dos principais desafios ocorre nas fronteiras entre regiões, onde um entregador de uma zona pode estar mais próximo de um pedido localizado em outra. Ao impor uma divisão rígida, o sistema pode deixar de considerar soluções globalmente melhores. Além disso, pode haver desequilíbrio na distribuição de pedidos entre regiões, tornando algumas mais sobrecarregadas do que outras. Portanto, embora a Divisão e Conquista ajude na escalabilidade, ela apresenta limitações que podem impactar a qualidade da solução final.

## Questão 4

| Critério | Greedy | Programação Dinâmica | Divisão e Conquista |
|----------|--------|---------------------|---------------------|
| Qualidade da solução | Boa (pode ser subótima) | Ótima | Boa |
| Complexidade de tempo | Média O(n x m) | Alta (exponencial) | Média |
| Complexidade de espaço | Baixa | Alta | Média |
| Viabilidade em tempo real (≤ 2s) | Alta | Baixa | Média |
| Escalabilidade com aumento de n | Boa | Ruim | Boa |
| Facilidade de adaptação a mudanças | Alta | Baixa | Média |

Entre as abordagens analisadas, a estratégia gulosa se destaca como a mais adequada para o contexto da FastBite. Embora não garanta a solução ótima, ela apresenta baixo custo computacional, alta escalabilidade e capacidade de resposta em tempo real, atendendo à restrição de decisões em até 2 segundos. A Programação Dinâmica, apesar de fornecer soluções ótimas, é inviável na prática devido ao seu custo exponencial. Já a Divisão e Conquista oferece um equilíbrio interessante, mas pode comprometer a qualidade da solução devido à perda de visão global do problema. Assim, em um ambiente de larga escala como o da FastBite, a escolha mais adequada é utilizar algoritmo guloso, combinado com técnicas de refinamento e heurística.

## Questão 5

### A)
Heurística pode ser interpretada como "suficientemente bom", é um "ponto de parada" para um algoritmo que identifica que a solução já está boa o suficiente para resolver o problema, o que pode reduzir consideravelmente o custo e a complexidade do algoritmo. Em sistemas como a FastBite, heurísticas são preferíveis porque o problema possui alta complexidade (NP-Completo), tornando inviável encontrar a solução ótima em tempo hábil. Como o sistema precisa tomar decisões em tempo real (até 2 segundos), é mais importante obter uma solução rápida e de boa qualidade do que a melhor solução possível.

### B)
1. Particionamento geográfico: Inicialmente, os pedidos são agrupados por região geográfica (como zonas ou quadrantes da cidade), buscando equilibrar a quantidade de pedidos por área e reduzir a complexidade do problema.

2. Solução inicial com heurística gulosa: Em cada região, aplica-se um algoritmo guloso para atribuir pedidos aos entregadores, considerando a proximidade entre entregador e restaurante. Em seguida, as rotas são definidas utilizando a estratégia de sempre escolher o próximo ponto mais próximo.

3. Refinamento local: Após a solução inicial, são aplicadas melhorias locais, como a troca de pedidos entre entregadores ou a reordenação de rotas, verificando se há redução no tempo total de entrega ou melhor atendimento às restrições (como pedidos urgentes).

4. Limite de tempo de processamento: Todo o processo é executado sob uma restrição de tempo (por exemplo, até 2 segundos). Caso esse limite seja atingido, o algoritmo interrompe a busca por melhorias e retorna a melhor solução encontrada até o momento.

Dessa forma, a solução combina rapidez e qualidade, sendo adequada para um ambiente de larga escala e tomada de decisão em tempo real.

### C)
Vale a pena buscar uma solução ótima em situações de pedidos urgentes, supondo um cenário que um entregador tem 3 pedidos urgentes que precisam ser entregues em menos de 20 minutos. A rota calculada para esse cenário deve ser a mais otimizada possível.

## Questão 6

Dentro do contexto computacional, existe uma relação entre custo e precisão, mas também existe um contexto. Cada problema deve ser analisado de forma individual e única, não existe regra de ouro ou generalização, cada problema pode exigir uma precisão que vai corresponder a um determinado custo.

A computação resolve problemas de todas as áreas da sociedade, portanto, se analisarmos dois cenários: primeiro, o cenário de laboratório de exames que gera um diagnóstico médico automático feito pela análise de resultados de exames de sangue do paciente, esse processo é repetido para todos os pacientes e pode escalar significativamente. Entretanto, estamos falando da área da saúde, e a precisão aqui é fundamental, portanto o custo computacional ou o tempo de execução devem ser relevados em função de obter o resultado mais preciso possível.

Já no segundo cenário, como o da FastBite, podemos buscar mais eficiência em troca de um pouco de precisão. O cálculo "bom o suficiente" de uma rota que leva 30 minutos, mas que foi calculada em 2 segundos, e o cálculo de uma rota de 27 minutos, mas que foi calculada em 5 minutos, parece se justificar no somatório final. Como a precisão nesse cenário não é algo extremamente necessário, o "bom o suficiente" pode, na verdade, custar menos e ser a melhor solução para determinada demanda.
