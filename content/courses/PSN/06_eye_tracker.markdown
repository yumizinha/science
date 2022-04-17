---
title: "Eye Tracking"
author: "Amanda Yumi"
highlight: true
date: "2018-10-08"
output: html_document
type: book
weight: 70
---

A tecnologia de eye tracking (ET) permite o rastreamento ocular, o que possibilita sua aplicação em diversas áreas das ciências, como saúde, psicologia e até mesmo marketing. O aparelho realiza a gravação do comportamento ocular, ou seja, identificando a localização do olhar, a duração de tempo, a dilatação da pupila e o trajeto do olhar. 
A importância desta técnica se deve ao fato de que o olhar de um indivíduo está diretamente relacionado com a sua atenção, o que faz com que seja possível compreender, em parte, o processo cognitivo de um usuário. 

A distância entre o reflexo que sai da pupila e o reflexo da luz observado na córnea é o que permite identificar a localização do olhar de uma pessoa, esse ponto onde a pessoa fixa o olhar é chamado de “fixação”. A fixação geralmente dura entre 100 e 500 ms (milissegundos). O trajeto que se produz entre duas fixações é chamado de sacada (saccade).

A hipótese trabalhada nesse tópico é baseada na relação entre as substâncias noradrelina e norepinefrina que associadas ao núcleos cerúleos permitem realizar a seguinte associação: 
* Aumento da atividade cerebral `\(\rightarrow\)` dilatação da pupila
* Repouso o sistema parassimpático $\rightarrow a pupila se contrai.]

Conforme ocorre a dilatação do diâmetro da pupila, podemos fazer inferências sobre a atividade cerebral dos indivíduos, como por exemplo determinando uma média dos sinais referentes ao diâmetro da pupila e comparar esses valores entre as condições de repouso e tarefa.



```r
#leitura de dados
dados=read.table("./data/dadosEYE.txt",header=TRUE,sep=";")

dim(dados)
```

```
## [1] 9099   30
```

Para ler as primeiras linhas (só pra saber a cara da tabela sem olhar tudo):


```r
head(dados)
```

```
##   RecordingTime..ms. Time.of.Day..h.m.s.ms.    Trial              Stimulus
## 1           221104.5           21:41:01:349 Trial001 brian-1-recording.avi
## 2           221137.8           21:41:01:382 Trial001 brian-1-recording.avi
## 3           221170.9           21:41:01:415 Trial001 brian-1-recording.avi
## 4           221204.4           21:41:01:449 Trial001 brian-1-recording.avi
## 5           221237.5           21:41:01:482 Trial001 brian-1-recording.avi
## 6           221270.8           21:41:01:515 Trial001 brian-1-recording.avi
##   Export.Start.Trial.Time..ms. Export.End.Trial.Time..ms. Participant Color
## 1                            0                     302726       brian Coral
## 2                            0                     302726       brian Coral
## 3                            0                     302726       brian Coral
## 4                            0                     302726       brian Coral
## 5                            0                     302726       brian Coral
## 6                            0                     302726       brian Coral
##   Tracking.Ratio.... Category.Group Category.Binocular Index.Binocular
## 1                100            Eye                  -               -
## 2                100            Eye      Visual Intake               1
## 3                100            Eye      Visual Intake               1
## 4                100            Eye      Visual Intake               1
## 5                100            Eye      Visual Intake               1
## 6                100            Eye      Visual Intake               1
##   Pupil.Diameter.Right..mm. Point.of.Regard.Binocular.X..px.
## 1                       4.6                            693.0
## 2                       4.6                            690.1
## 3                       4.6                            692.4
## 4                       4.6                            692.2
## 5                       4.6                            694.2
## 6                       4.6                            691.5
##   Point.of.Regard.Binocular.Y..px. Point.of.Regard.Right.X..px.
## 1                            258.7                        739.9
## 2                            257.0                        739.4
## 3                            257.2                        742.4
## 4                            257.5                        743.3
## 5                            256.3                        743.8
## 6                            257.2                        741.0
##   Point.of.Regard.Right.Y..px. AOI.Name.Binocular Gaze.Vector.Right.X
## 1                        271.6                  -                   0
## 2                        269.7                  -                   0
## 3                        270.5                  -                   0
## 4                        269.9                  -                   0
## 5                        268.9                  -                   0
## 6                        270.5                  -                   0
##   Gaze.Vector.Right.Y Gaze.Vector.Right.Z Video.Time..h.m.s.ms. Annotation.Name
## 1                   0                   1          00:00:00:083               -
## 2                   0                   1          00:00:00:083               -
## 3                   0                   1          00:00:00:125               -
## 4                   0                   1          00:00:00:167               -
## 5                   0                   1          00:00:00:208               -
## 6                   0                   1          00:00:00:250               -
##   Annotation.Description Annotation.Tags Mouse.Position.X..px.
## 1                      -               -                     -
## 2                      -               -                     -
## 3                      -               -                     -
## 4                      -               -                     -
## 5                      -               -                     -
## 6                      -               -                     -
##   Mouse.Position.Y..px. Scroll.Direction.X Scroll.Direction.Y Content
## 1                     -                  -                  -       -
## 2                     -                  -                  -       -
## 3                     -                  -                  -       -
## 4                     -                  -                  -       -
## 5                     -                  -                  -       -
## 6                     -                  -                  -       -
```

(assim é possível verificar se está tudo ok e se não tem problema algum com os arquivos)

A função "colnames"" ajuda a identificar o nome das colunas.


```r
#Leia os nomes das colunas 
colnames(dados)
```

```
##  [1] "RecordingTime..ms."               "Time.of.Day..h.m.s.ms."          
##  [3] "Trial"                            "Stimulus"                        
##  [5] "Export.Start.Trial.Time..ms."     "Export.End.Trial.Time..ms."      
##  [7] "Participant"                      "Color"                           
##  [9] "Tracking.Ratio...."               "Category.Group"                  
## [11] "Category.Binocular"               "Index.Binocular"                 
## [13] "Pupil.Diameter.Right..mm."        "Point.of.Regard.Binocular.X..px."
## [15] "Point.of.Regard.Binocular.Y..px." "Point.of.Regard.Right.X..px."    
## [17] "Point.of.Regard.Right.Y..px."     "AOI.Name.Binocular"              
## [19] "Gaze.Vector.Right.X"              "Gaze.Vector.Right.Y"             
## [21] "Gaze.Vector.Right.Z"              "Video.Time..h.m.s.ms."           
## [23] "Annotation.Name"                  "Annotation.Description"          
## [25] "Annotation.Tags"                  "Mouse.Position.X..px."           
## [27] "Mouse.Position.Y..px."            "Scroll.Direction.X"              
## [29] "Scroll.Direction.Y"               "Content"
```

A primeira coluna se traz os "RecordingTime..ms."
Para saber a taxa de amostragem, precisamos avaliar a diferença entre elementos.
Entre um MILISEGUNDO e outro, temos um intervalo de sinais coletados.



```r
#Tomando os 10 primeiros números
dados[1:10,1]
```

```
##  [1] 221104.5 221137.8 221170.9 221204.4 221237.5 221270.8 221304.0 221337.5
##  [9] 221370.7 221403.9
```

Para conhecer a taxa de amostragem fazemos um elemento menos o anterior, para isso usamos o comando "diff" e tiramos a média:

* 1) Identificando a diferença dos elementos com o elemento anterior:


```r
#Identificamos a diferença entre um elemento menos o anterior, para isso usamos o comando "diff":
diff(dados[,1])
```

* 2) O cálculo da média proporciona a identificação do intervalo, e dessa forma é possível identificar a taxa de amostragem:


```r
DIFF = mean(diff(dados$RecordingTime..ms.))

#A taxa de amostragem em Hertz é dada por:
HZ=1000*1/DIFF
```

No caso do cálculo da frequência, HZ, está multiplicado por 1000 pois a coleta está em milisegundos e a frequência tem que considerar segundos.

Visualizando a informação:


```r
#Fazer o grafico
plot(dados$RecordingTime..ms.*1000,
  dados$Pupil.Diameter.Right..mm.,type="l",xlab="Tempo(s)",
  ylab="Diametro Pupilar Direito(mm)")
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-7-1.png" width="672" />


Neste gráfico pode-se identificar piscadas (que não necessariamente é um artefato, pois piscada está associada a estados cognitivos). 

Para essa análise, pretendemos identificar os picos de cima e os de baixo (muito sensíveis posso considerar como missing data).

Identificando o diâmetro da pupila direita e os dados que quero ignorar:


```r
#Sinal do diametro da pupila direita
pupilD = dados$Pupil.Diameter.Right..mm.

#Detectar outliers, piscadas e missing data
IX=which(pupilD<4 | pupilD>6)
pupilD[IX]=NA
```

Delineamento do experimento:
* 30 segundos em repouso seguidos de 30s de tarefa (repetindo ciclo), fazendo a subtração de 13 a partir do 2000 (e decrescendo!)
* Cada época de 30s se refere a HZ*30 caselas no vetor pupilD.
* Devemos criar um vetor indicando a condição para cada casela do vetor pupilD:
* O vetor CONDICAO armazenará os dois tipos de estímulos existentes, inicialmente marcado como 1 para todos os dados 


```r
CONDICAO = array(1, length(pupilD))
```

Durante os períodos de contagem de subtração mental (a cada 30s) há um repouso:


```r
# identificando o inicio do intervalo, com base nos 30s de trial:
inicio = ceiling(HZ*30)
# calculando o teto, que é o final do trial, 30s depois:
final = ceiling((HZ*30)+(HZ*30))
```

Calculando as condições (para aula fizemos de uma forma manual para ficar mais rápido). O recomendado mesmo seria reproduzir as linhas acima num laço e identificar as condições e intervalos.

Uma vez identificado os períodos do trial, atualizar o vetor CONDICAO para os parâmetros para 2 nos intervalos em que ocorre o estímulo:


```r
#Colocar 2 nas caselas durante os periodos de subtracao mental
CONDICAO[902:1803]=2
CONDICAO[2706:3607]=2
CONDICAO[4510:5411]=2
CONDICAO[6314:7215]=2
CONDICAO[8118:9019]=2
```

Analisando as duas condições (repouso e atividade), podemos visualizar com boxplot:


```r
#Boxplot do diametro de pupila nas duas condicoes
#os proximos 2 comandos sao equivalentes
boxplot(pupilD[which(CONDICAO==1)],pupilD[which(CONDICAO==2)])
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-12-1.png" width="672" />

```r
boxplot(pupilD~CONDICAO)
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-12-2.png" width="672" />

Além disso, queremos avaliar a transição repouso da tarefa para cada bloco em cada região.

Olhando apenas uma condição:


```r
plot(1:1803,pupilD[1:1803],type="l")
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-13-1.png" width="672" />

Olhando todas as condições:


```r
plot(1:1803,pupilD[1:1803],type="l")
lines(1:1804,pupilD[1804:3607],col=2)
lines(1:1804,pupilD[3608:5411],col=3)
lines(1:1804,pupilD[5412:7215],col=4)
lines(1:1804,pupilD[7216:9019],col=6)
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-14-1.png" width="672" />


É possível ver uma tendência dos dados. Para uma visualização melhor dos dados, analisamos a curva média da pupila nesses períodos:


```r
#calcular a curva media
MEDIA=(pupilD[1:1804]+pupilD[1804:3607]+pupilD[3608:5411]+pupilD[5412:7215]+pupilD[7216:9019])/5
```


No caso do plot, estamos multiplicando por 60 por se tratar de um ciclo de 30s de atividade e 30s de repouso:


```r
plot(60*(1:1804)/1804,MEDIA,type="l",xlab="Tempo(s)",ylab="Diametro(mm)")
abline(v=30,lty=3)
```

<img src="/courses/PSN/06_eye_tracker_files/figure-html/unnamed-chunk-16-1.png" width="672" />

A dificuldade nestes exercícios é a identificação das caselas a serem trabalhadas.







```r
mediarep = mean(pupilD[which(CONDICAO==1)])
# Desvio padrão da condição repouso:
desviorep = sd(pupilD[which(CONDICAO==1)])

# Media da condição tarefa:
media = mean(pupilD[which(CONDICAO==2)])
# Desvio padrão da condição tarefa:
desviopad = sd(pupilD[which(CONDICAO==2)])
```
