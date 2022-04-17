---
title: "Análise de Coerência"
author: "Amanda Yumi"
highlight: true
date: "2018-11-26"
output: html_document
type: book
weight: 170
---

Autocovariância: `\(X_t\)`

$$ \lambda(h) = cov(X_t, X_t-h) $$


Covariância cruzada: 

$$ \lambda(h)_{XY} = cov(X_t, X_t-h) $$
Estimador - 

Estimador - 

$$ \hat{\lambda}(h)_{XY} =  $$

O espectro (ou densidade espectral) também pode ser obtido por meio da Transformada de Fourier da função de autocovariância.

O espectro cruzado é a TDF da função da covariância cruzada.


Calcularemos a coerência espectral usando os dados de eletrofisiologia:


```r
##################### Analise Espectral - Olhos Fechados
#leitura dos sinais
sinais = read.table("./data/OlhosFechados.txt", header = FALSE)

#leitura do nome dos canais
nomescanais = scan("./data/NOMEScanais.txt",what = "string")

dim(sinais)
```

```
## [1] 45315    32
```

```r
nomescanais 
```

```
##  [1] "Fp1"  "Fp2"  "F7"   "F3"   "Fz"   "F4"   "F8"   "FC5"  "FC1"  "FC2" 
## [11] "FC6"  "T7"   "C3"   "Cz"   "C4"   "T8"   "TP9"  "CP5"  "CP1"  "CP2" 
## [21] "CP6"  "TP10" "P7"   "P3"   "Pz"   "P4"   "P8"   "PO9"  "O1"   "Oz"  
## [31] "O2"   "PO10"
```

```r
#Taxa de amostragem
HZ = 250

#Tamanho da epoca (janela) em segundos
TAMsegundos = 5

#Tamanho da epoca (janela) em caselas do vetor de sinais
TAM = TAMsegundos*HZ

#Numero de epocas
Nepocas = floor(nrow(sinais)/TAM)

#Numero de canais
Ncanais = ncol(sinais)

# Por motivos didático, primeiramente vamos calcular
# a coerencia entre dois canais sem dividir em epocas
```

Por questões didáticas, calculemos a coerência entre dois canais sem dividir em épocas:
* F7 canal 3
* P7 canal 23
* O1 canal 29

Calculando a coerência entre O1 e F7 e vendo seu gráfico:

```r
coerencia = spectrum(sinais[, c(29,3)], spans = c(100, 100))
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-1.png" width="672" />

```r
#Grafico da coerencia
plot((HZ/2)*(1:nrow(coerencia$coh))/nrow(coerencia$coh), coerencia$coh,
 type="l", xlab="Frequencia (Hz)", ylab="Coerencia")
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-2.png" width="672" />

```r
#Coerencia entre F7 e P7
coerencia = spectrum(sinais[, c(3, 23)], spans = c(100, 100))
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-3.png" width="672" />

```r
#Grafico da coerencia
plot((HZ/2)*(1:nrow(coerencia$coh))/nrow(coerencia$coh),coerencia$coh,
 type ="l",xlab = "Frequencia (Hz)", ylab = "Coerencia")
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-4.png" width="672" />

```r
#### Calculo da coerencia media entre epocas
#1-Coerencia entre O1 e F7
#2-Coerencia entre O1 e P7
#3-Coerencia entre F7 e P7

  y1 = 0
  y2 = 0
  y3 = 0
  #Calcula a media entre épocas
  for(epoca in 1:Nepocas){
    #Descobrir caselas de inicio e fim de cada epoca
    INICIO = TAM*(epoca-1)+1
    FIM = epoca*TAM
    y1 = y1 +spectrum(sinais[INICIO:FIM, c(29, 3)], spans = c(15, 15))$coh
    y2 = y2 +spectrum(sinais[INICIO:FIM, c(29, 23)], spans = c(15, 15))$coh
    y3 = y3 +spectrum(sinais[INICIO:FIM, c(23, 3)], spans = c(15, 15))$coh

  }#for da epoca
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-5.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-6.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-7.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-8.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-9.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-10.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-11.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-12.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-13.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-14.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-15.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-16.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-17.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-18.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-19.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-20.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-21.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-22.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-23.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-24.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-25.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-26.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-27.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-28.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-29.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-30.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-31.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-32.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-33.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-34.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-35.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-36.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-37.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-38.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-39.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-40.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-41.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-42.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-43.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-44.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-45.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-46.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-47.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-48.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-49.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-50.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-51.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-52.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-53.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-54.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-55.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-56.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-57.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-58.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-59.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-60.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-61.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-62.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-63.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-64.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-65.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-66.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-67.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-68.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-69.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-70.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-71.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-72.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-73.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-74.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-75.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-76.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-77.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-78.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-79.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-80.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-81.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-82.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-83.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-84.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-85.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-86.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-87.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-88.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-89.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-90.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-91.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-92.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-93.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-94.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-95.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-96.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-97.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-98.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-99.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-100.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-101.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-102.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-103.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-104.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-105.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-106.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-107.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-108.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-109.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-110.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-111.png" width="672" /><img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-112.png" width="672" />

```r
   y1 = y1/Nepocas
   y2 = y2/Nepocas
   y3 = y3/Nepocas

#Grafico da coerencia media
plot((HZ/2)*(1:nrow(y1))/nrow(y1), y1,
 type="l", xlab = "Frequencia (Hz)", ylab = "Coerencia media")
lines((HZ/2)*(1:nrow(y2))/nrow(y2), y2, col = 2)
lines((HZ/2)*(1:nrow(y3))/nrow(y3), y3, col = 4)
legend("topright", c("O1-F7","O1-P7","F7-P7"), lty = c(1, 1, 1), col = c(1, 2, 4))
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-113.png" width="672" />

```r
#Grafico da coerencia media
plot((HZ/2)*(1:nrow(y1))/nrow(y1), y1,
 type = "l", xlab = "Frequencia (Hz)", ylab = "Coerencia media", ylim = c(0, 1))
lines((HZ/2)*(1:nrow(y2))/nrow(y2), y2, col = 2)
lines((HZ/2)*(1:nrow(y3))/nrow(y3), y3, col = 4)
legend("topright",c("O1-F7", "O1-P7", "F7-P7"), lty = c(1, 1, 1), col = c(1, 2, 4))
```

<img src="/courses/PSN/16_analise_de_coerencia_files/figure-html/unnamed-chunk-2-114.png" width="672" />
