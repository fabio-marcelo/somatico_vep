# somatico_vep
Passo a passo de como anotar um arquivo VCF utilizando o Ensembl Variant Effect Predictor (VEP) através do google colab e google drive.

## Obtenção do arquivo VCF
Realizar as análises de acordo com o tutorial disponibilizado em https://github.com/renatopuga/somatico.git

## Anotação do arquivo VCF
1. Fazer log in em sua conta google
2. Abrir um novo notebook no google colab (https://colab.research.google.com/)
3. Montar o google drive
  ```
  from google.colab import drive          # importar a biblioteca
  drive.mount('/content/drive')           # montar o drive
  ```
