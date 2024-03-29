# Rotação de Culturas

  É uma prática agrícola que consiste em alternar em uma mesma área diferentes culturas vegetais seguindo um plano definido anteriormente. Esta técnica visa tornar o sistema mais produtivo e ambientalmente mais sustentável, restabelecendo o equilíbrio biológico. Por outro lado, a monocultura é prejudicial ao solo, pois causa o empobrecimento nutricional devido à produção contínua de uma mesma planta, que absorve sempre os mesmos nutrientes, além de levar à ocorrência descontrolada de doenças, pragas e plantas daninhas.

  As espécies destinadas à rotação de culturas devem ter a finalidade de recuperar e preservar a qualidade do solo, mas também se pode levar em consideração a importância econômica dessas espécies. A escolha das espécies para um sistema de rotação depende das condições climáticas de cada região, das condições do solo, da época da semeadura, da finalidade da produção, entre outros fatores. Um sistema de rotação adotado em uma região pode ser inviável em outras regiões.

  Para garantir a eficiência de um sistema de rotação de culturas existem alguns princípios básicos, como: fazer a alternância entre espécies vegetais que apresentem exigências nutricionais distintas e que não apresentem suscetibilidade aos mesmos tipos de pragas; alternância entre espécies que apresentem sistemas radiculares diferentes quanto à arquitetura, distribuição e profundidade de exploração do solo; uso de pelo menos uma espécie com alta capacidade de produção de resíduos vegetais, os quais promovem a proteção do solo.

  NESSE TRABALHO, desenvolvido durante a disciplina de Tópicos Especiais em Otimização Linear na Universidade Federal Rural do Rio de Janeiro, é usado um algorítmo construtor e uma metaheurística (Simulated Annealing) para otimizar o problema combinatório de como fazer rotação de culturas levando em conta suas variáveis exponencias conforme o tamanho aumenta, de forma a maximizar o Lucro do agricultor.

# Construtor - Implementado

Importa uma planilha Excel xlsx com nome "RotacaoCulturas" e é exportada uma planilha excel "CulturaConstruida".

Esta heurística de construção tem o seguinte funcionamento conforme o artigo:
1. Uma permutação aleatória (cíclica) das (N + 1) culturas é selecionada. 
2. Para construir uma rotação em um lote, toma-se, na permutação, a primeira cultura que puder ser plantada no primeiro período. Uma vez que esta é encontrada, insere-se na programação e é escrita na matriz tantas vezes quanto for o seu ciclo de vida.
3. Assim, a q-ésima cultura entrará na programação deste lote, se puder ser plantada no período "1 + Somatório ti, com i=1 e máximo q-1" e não for da mesma família botânica que a cultura q − 1, com excessão da cultura N + 1, que pode ser “cultivada” consecutivamente (pousio).
4. Este processo se repete até que os M períodos sejam preenchidos.
5. Finalmente, uma solução consistirá em repetir este processo L vezes.

# Penalizador
  
  De modo a fazer os métodos convergirem para soluções factíveis, adotou-se uma metodologia de penalização de soluções infactíveis. Esta penalização, por sua vez, depende da quantidade de restriçõess que a programação em análise possui.Pretende-se fazer com que as soluções
com poucas infactibilidades tenham seu lucro menos reduzido em comparação com uma que possui um elevado número de violações.

1. P.V - Penalização de Vizinhos: lotes adjacentes há plantio de culturas da mesma família botânica.
2. P.A.V - Penalização de Adubação Verde: Uma solução que possui r penalizações de adubaçao verde é aquela que não contemplou adubação verde em r lotes durante todo o período considerado.
3. P.P - Penalização de Pousio: Similiar a P.A.V.
4. P.A.D. - Penalização de Demanda: São computadas determinando-se a quantidade de culturas que não atenderam à demanda solicitada.
5. P.A.L Precisa??

# Gerador de Vizinhança - Implementado

Dada uma solução qualquer S (com L linhas), um vizinho de S será uma matriz que possui L − 1 linhas de S. A diferença entre elas está somente na linha faltante.

# Simulated Annealing - Implementado

Algoritmo de minimização: 

```
Algoritmo 5 SA(f(.), V (.), α, SAmax, T0, Tf, s)
1 s∗ ← s {Melhor solução até então obtida};
2 iterT ← 0 {Contador de iterações numa isoterma T};
3 T ← T0 {Temperatura corrente};
4 enquanto (T > Tf ) faça;
5   enquanto (iterT < SAmax) faça;
6     iterT ← iterT + 1;
7     gere uma vizinhança em torno de V (s);
8     escolha aleatoriamente v ∈ V (s);
9     Δ= f(v) − f(s);
10    se (Δ < 0) ent˜ao
11      s ← v;
12      se (f(v) < f(s∗)) então s∗ ← v;
13    senão;
14      tome r ∈ [0, 1];
15      se r < e−Δ/T então s ← v;
16    fim-se;
17  fim-enquanto;
18  T = α(T) {A temperatura é atualizada segundo uma regra α};
19  iterT ← 0;
20 fim-enquanto;
21 s ← s∗;
22 retorne s;
fim SA
```
Porém no código implementado usa-se Δ > 0, pois é um problema de maximização de lucro!
