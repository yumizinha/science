---
title: "Brain Decoding"
author: "Amanda Yumi"
highlight: true
date: "2018-11-28"
output: html_document
type: book
weight: 180
---

Inspirado no artigo na Nature: "Deep image reconstruction from human brain activity"", que realizou a análise baseada na aprendizagem por máquina dos padrões de ressonância magnética funcional humana (fMRI) que permitiu a visualização do conteúdo perceptivo de imagens através de um novo método de reconstrução de imagem, no qual os valores de pixel de uma imagem são otimizados para tornar suas características DNN semelhantes àquelas decodificadas da atividade do cérebro humano em múltiplas camadas. 

Aqui realizaremos um estudo simplificado, com base de dados de fNIRS:


```r
#Leitura de arquivos dos sujeitos
nomes = scan("./data/subjects.txt", what = "string")

#Taxa de amostragem
HZ = 7.81

#Numero de pontos (10 minutos de coleta) - Total de pontos no tempo
T = HZ*10*60

DADOS = NULL

for(i in 1:length(nomes)){

  #Leitura dos dados de cada individuo
  a = read.table(paste("./data/", nomes[i], sep = ""))[1:T,]
  
  #normalizacao dos dados para media zero
  #e variancia 1
  for(j in 1:ncol(a)){
     a[,j] = (a[, j]-mean(a[, j]))/sd(a[, j])
  } 
  
  #Coloca o identificador do individuo
  #na primeira coluna
  a = cbind(i, a)

  #Vetor de condicoes em cada ponto no tempo - 2: tarefa  1:repouso
  COND = rep(c(rep(2, 234), rep(1, 235)), 10)[1:T]

  # Concatena TUDO
  # Coluna 1: condicao
  # Coluna 2: Indice do individuo
  # Coluna 3:22 Sinais hemodinamicos 
  DADOS = rbind(DADOS, cbind(COND, a))

}#fecha o for do individuo
```

Na aprendizagem supervisionada, temos dois tipos de modelos: regressão e classificação. 
* Em um problema de regressão, o objetivo é prever os resultados em uma saída contínua, o que significa tentar mapear variáveis e entrada para alguma função contínua. 
* Em um problema de classificação, trata-se de prever os resultados em uma saída discreta. Em outras palavras, mapear variáveis de entrada em categorias distintas


# Classificador:

Encontrar uma função que satisfaz um critério de otimização.  - o
Um método conhecido e muito utilizado é o SVM - Support Vector Machine.

ESCREVER MAIS SOBRE DEEP LEARNING: INPUT Space e FEATURE SPACE


Matriz de variáveis preditoras:

X = linhas obs e colunasR com a variável
Y = Vetor de labels [R R R...]




```r
require(e1071)
```

```
## Carregando pacotes exigidos: e1071
```

```r
AcuraciaMedia = 0

for(semSUB in 1:length(nomes)){
  #Indice das linhas dos dados do individuo
  #semSUB
  IX = which(DADOS[, 2] == semSUB)

  #Procedimento leave-one-subject-out
  #matriz de sinais sem o individuo semSUB
  Xtreino = DADOS[-IX, 3:22]

  #matriz de sinais DO individuo semSUB
  Xteste = DADOS[IX, 3:22]

  #Labels dos dados de treino (sem o semSUB)
  Ytreino = factor(DADOS[-IX, 1])

  #labels dos dados de teste (labels DO semSUB)
  Yteste = factor(DADOS[IX, 1])

  #Treina o SVM - pode ser "linar", "radial", "poly"
  #FIT = svm(Xtreino, Ytreino, kernel="radial")
  FIT2 = svm(Xtreino, Ytreino, kernel="poly")
  #FIT3 = svm(Xtreino, Ytreino, kernel="linear")
  #Faz a predicao do label para o dado de teste
  
  PREDICAO = predict(FIT2, Xteste)
  #PREDICAO2 = predict(FIT2, Xteste)
  #PREDICAO3 = predict(FIT3, Xteste)

  #Comparar os labels PREDITOS com os reais
  #Matriz de confusao
  CONFUSAO = table(Yteste,PREDICAO)

  #ACURACIA
  acuracia = sum(diag(CONFUSAO))/sum(CONFUSAO)
  print(acuracia)

  AcuraciaMedia = AcuraciaMedia +acuracia/length(nomes)

}#fecha for do semSUB
```

```
## [1] 0.4054631
## [1] 0.5887751
## [1] 0.4605207
## [1] 0.7744345
## [1] 0.5172855
```

Cross-validation:

A validação cruzada fornece uma estimativa de desempenho pessimista porque a maioria dos modelos estatísticos irá melhorar se o conjunto de treinamento for ampliado. Isso significa que a validação cruzada k-fold estima o desempenho de um modelo treinado em um conjunto de dados 100 * (k-1) / k% dos dados disponíveis, em vez de 100% dele. 

Portanto, se você realizar a validação cruzada para estimar o desempenho e usar um modelo treinado em todos os dados para uso operacional, ele terá um desempenho um pouco melhor do que a estimativa de validação cruzada sugere. 

A validação cruzada leave-one-out é aproximadamente imparcial, porque a diferença de tamanho entre o conjunto de treinamento usado em cada dobra e o conjunto de dados inteiro é apenas um padrão único, mas é fácil de implementar.
