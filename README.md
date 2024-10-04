# apresentacao-bim1-2024b-MiguelMironSilva
> **Disciplina:** ELC117 - Paradigmas de Programação - 2024b  
> **Aluno:** Miguel Miron Silva
> **Apresentação:** Função 6

## Contexto

A visão computacional é um campo da inteligência artificial que utiliza algoritmos de aprendizagem de máquina para interpretar e entender o conteúdo de imagens e vídeos. Uma das tarefas fundamentais na visão computacional é a detecção de objetos, que envolve localizar e identificar objetos de interesse dentro de uma imagem ou um vídeo.

Durante o processo de detecção de objetos, três informações principais são geradas:

- Bounding Boxes (BBoxes): São retângulos ou caixas delimitadoras que representam a localização de objetos detectados em uma imagem. Cada bounding box é definida por quatro coordenadas (x1,y1,x2,y2), que correspondem aos cantos superior esquerdo e inferior direito da caixa. As bounding boxes ajudam a indicar onde os objetos estão localizados na imagem.
- Scores: Para cada bounding box, um score é atribuído, representando a confiança do modelo na detecção do objeto. Esse score é um valor entre 0 e 1, onde valores mais altos indicam maior confiança de que um objeto foi detectado corretamente. O score é fundamental para filtrar detecções irrelevantes ou errôneas.
- Classes: Cada objeto detectado é classificado em uma de várias categorias (ou classes) pré-definidas. Por exemplo, em um modelo treinado para detectar animais pode incluir "gato" e "cachorro".

A imagem abaixo ilustra as bounding boxes, classes e scores.
[![Bounding boxes classification of a dog and a cat](http://d2l.ai/_images/output_anchor_f592d1_192_0.svg 'Codey the Codecademy mascot')](http://d2l.ai/chapter_computer-vision/anchor.html)

## Dados

Em Haskell, podemos usar listas para representar bounding boxes, scores e classes de objetos. Essas listas têm o mesmo número de elementos, sendo que o primeiro elemento em cada uma delas refere-se ao primeiro objeto, e assim por diante. Por exemplo, no código abaixo, o primeiro objeto é da classe 0, com score 0.95 e com bounding box (34.0, 60.0, 200.0, 320.0).

```Haskell
-- Bounding boxes: (xmin, ymin, xmax, ymax)
boundingBoxes :: [(Float, Float, Float, Float)]
boundingBoxes = [ (34.0, 60.0, 200.0, 320.0),
              	(100.0, 150.0, 250.0, 380.0),
              	(300.0, 220.0, 450.0, 450.0) ]

-- Scores: Confidence scores for each detection
scores :: [Float]
scores = [0.95, 0.80, 0.60]

-- Classes: Class 0 or 1 representing two object types
classes :: [Int]
classes = [0, 1, 0]
```

## Descrição do problema
Devemos resolver o exercíco 6 que segue:

### Função 6
Resolva o exercício 5 usando lambda.

### Função 5
Crie uma função para converter a lista de bounding boxes para outro formato: em vez de representar cada bounding box como (xmin, ymin, xmax, ymax), usar a representação (xmin, ymin, width, height), sendo width = xmax - xmin e height = ymax - ymin
Resolva esta função sem usar lambda.

Exemplo de uso:
ghci> convertBoundingBoxes boundingBoxes
[(34.0,60.0,166.0,260.0),(100.0,150.0,150.0,230.0),(300.0,220.0,150.0,230.0)]

Nome e tipo da função:
convertBoundingBoxes :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]

## Resolução do problema
No haskell, podemos facilmente fazer o tipo de operação de conversão que queremos usando a função **map**. O **map** é uma função de alta ordem que aplica uma função em todos os elementos em uma dada lista - nesse caso a lista que receberemos se denomina ***boundingBoxes**, e nós queremos mudar sua formatação de ```(x1, x2, y1, y2)``` para ```(x1, y1, x2 - x1, y2 - y1)```. Ao invés de darmos as informações sobre os pontos mínimos e máxmimos horizontais e verticais do *boundingBox*, nós vamos repassar os pontos mínimos verticais e horizontais, e a largura e altura do *boundingBox* a partir desses.

Normalmente, escreveríamos a função que usaríamos com **map**  como uma função separada que é entâo aplicada em cada membro da lista, como por exemplo:
````Haskell
changeFormat :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]
changeFormat (x1, x2, y1, y2) -> (x1, y1, x2 - x1, y2 - y1)

convertBoundingBoxes :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]
converboundingBoxes -> map changeFormat boundingBoxes
````

Porém, como nosso objetivo é utilisar funções lambda para resolver esse problema, podemos utilisar características **tuplas** e **map** para resolvê-lo em apenas uma linha de código.

### Aprendizado 

A solução foi principalmente inventada usando-se um código lambda com tuplas já apresentado nas aulas:
```Haskell
(\(n,lat,long) -> long) ("Reitoria",-29.72083,-53.71479)
```

Caso rodarmos essa função, vamos perceber que ela só retorna ```-53.71479``` como argumento. Para fazermos ela retornar todos os três elementos, devemos fazer que o argumento de retorno também seja uma tupla, como tal:

```Haskell
(\(n,lat,long) -> (n,lat,long)) ("Reitoria",-29.72083,-53.71479)
```

Nota-se que poderíamos representar os argumentos à serem enviados e recebidos de maneira arbitrária - poderíamos substituir ```(n,lat,long)``` por ```(x1,x2,x3)``` e teríamos os mesmos resultados. Porém, a tupla de saída deve possuir as mesmas variáveis que a tupla de entrada para o programa funcionar de maneira se for o caso.

```Haskell
(\(x1,x2,x3) -> (x1,x2,x3)) ("Reitoria",-29.72083,-53.71479)
```

Os retornos das tuplas, caso nomeados corretamente, podem ter operações e funções aplicadas dentro deles para mudar o seu conteúdo, desde que os seus **tipos de dados** sejam respeitados. Por exemplo, podemos retornar uma operação usando lat e long:

```Haskell
(\(n,lat,long) -> (n,long + lat)) ("Reitoria",-29.72083,-53.71479)
```

Isso nos retorna o nome ```Reitoria``` e a soma de ```lat``` e ```long```. Note que assinalar valores, como, por exemplo ```(\(n,lat,long) -> (n,num = long + lat))``` vai dar erro no processo de compilação - o símbolo ```=``` é ilegal dentro dessas operações.

Para reformatar o conteúdo que queremos, utilizamos em cada elemento *boundingBox* a seguinte função:
```Haskell
(\(x1, x2, y1, y2) = (x1, y1, x2 - x1, y2 - y1))
```

Isso nos retorna uma função ```(Float, Float, Float, Float) -> (Float, Float, Float, Float)```. Porém, nós devemos lidar não com uma, mas três tuplas, no formato ```[(Float, Float, Float, Float) -> (Float, Float, Float, Float)]```. Para fazer isso, simplesmente adicionamos a função **map** junto à função lambda (não dentro dela!).

```Haskell
map (\(x1, x2, y1, y2) = (x1, y1, x2 - x1, y2 - y1))
```

Isso faz possível percorrer toda a lista de **boundingBoxes** e aplicar a operação em todas as suas tuplas.

## Código

```Haskell
--Solução
convertBoundingBoxes :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]
convertBoundingBoxes = map (\(x1, x2, y1, y2) -> (x1, y1, x2 - x1, y2 - y1))
```

Também há uma versão mais elaborada do programa, que aplica funções dentro da linha:

```Haskell
width :: Float -> Float -> Float
width x1 x2 = x1 - x2

height :: Float -> Float -> Float
height y1 y2 = y1 - y2

convertBoundingBoxes :: [(Float, Float, Float, Float)] -> [(Float, Float, Float, Float)]
convertBoundingBoxes = map (\(x1, x2, y1, y2) -> (x1, y1, width x2 x1, height y2 y1))
```

Rodamos ele com o seguinte comando:
```
convertBoundingBoxes boundingBoxes
```

Dado o código apresentado, o resultado deve ser:
```
[(34.0,200.0,26.0,120.0),(100.0,250.0,50.0,130.0),(300.0,450.0,-80.0,0.0)]
```

## Bibliografia
[https://pt.wikibooks.org/wiki/Haskell/Lambdas_e_operadores]  
[https://wiki.haskell.org/Lambda_abstraction]  
[https://wiki.haskell.org/Higher_order_function]
[https://www.codecademy.com/resources/docs/markdown/images]  

A LLM **Claude** foi extensivamente utilizada para tirar dúvidas e explicar a funcionalidade de elementos das funções do Haskell:
[claude.ai]  
