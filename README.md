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
4. Criar um diretório para executar as análises
  ```
  !mkdir somatico                         # criar diretório
  %cd somatico                            # entrar definitivamente no diretório
  ```
5. Para verificar se está dentro do diretório
  ```
  %%bash
  pwd
  ```
6. Instalar o VEP
  * Instalar as dependências
  ```
  %%bash
  sudo apt install unzip curl git libmodule-build-perl libdbi-perl libdbd-mysql-perl build-essential zlib1g-dev
  ```
  * Download do release esembl-vep 105.0
  ```
  %%bash
  wget -c https://github.com/Ensembl/ensembl-vep/archive/refs/tags/105.0.tar.gz
  ```
  * Descompactar o releaase
  ```
  %%bash
  tar -zxvf 105.0.tar.gz
  ```
  * Entrar no diretório e rodar a instalação
  ```
  %%bash
  cd ensembl-vep-105.0
  ./INSTALL.pl --NO_UPDATE  
  ```
7. Testar a instalação
  ```
  %%bash
  cd ensembl-vep-105.0
  ./vep 
  ```
  O resultado esperado é:
  ```
  #----------------------------------#
# ENSEMBL VARIANT EFFECT PREDICTOR #
#----------------------------------#

Versions:
  ensembl              : 105.525fbcb
  ensembl-funcgen      : 105.660df8f
  ensembl-io           : 105.2a0a40c
  ensembl-variation    : 105.ac8178e
  ensembl-vep          : 105.0

Help: dev@ensembl.org , helpdesk@ensembl.org
Twitter: @ensembl

http://www.ensembl.org/info/docs/tools/vep/script/index.html

Usage:
./vep [--cache|--offline|--database] [arguments]

Basic options
=============

--help                 Display this message and quit

-i | --input_file      Input file
-o | --output_file     Output file
--force_overwrite      Force overwriting of output file
--species [species]    Species to use [default: "human"]
                       
--everything           Shortcut switch to turn on commonly used options. See web
                       documentation for details [default: off]                       
--fork [num_forks]     Use forking to improve script runtime

For full option documentation see:
http://www.ensembl.org/info/docs/tools/vep/script/vep_options.html
  ```
  
8. Anotação do arquivo VCF
  * A documentação pode ser consultada em https://www.ensembl.org/info/docs/tools/vep/script/index.html
  * Rodar o vep
    * as flags utilizadas são:
    
| Flag                  | Descrição                                                  |
| --------------------- |:----------------------------------------------------------:|
| -i                    | arquivo CF input                                           | 
| -o                    | arquivo formato .tsv output                                |   
| --fork                | número de threads                                          |
| --dir_cache           | diretório de cache                                         |
| --fasta               | sequência referência                                       |
| --cache               | habilita uso do cache                                      |
| --offline             | roda sem uso de conexão com internet                       |
| --assembly            | versão da montagem                                         |
| --refseq              | esepcificque esta opção caso tenha instalado refseq cache |
| --pick                |                                                           |
| --pick_allele         |                                                           |
| --force_overwrite     |                                                           |
| --tab                 |                                                           |
| --symbol              |                                                           |
| --check_existing      |                                                           |
| --variant_class       |                                                           |
| --everything          |                                                           |
| --filter_common       |                                                           |
  ```
  %%bash
  ./ensembl-vep-105.0/vep  \
    --fork 3 \
    -i /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/105_GRCh37/WP312.filtered.vcf.gz \
    -o /content/drive/Shareddrives/T4-2022/FabioLima/somatico/WP312.filtered.vcf.tsv \
    --dir_cache /content/drive/Shareddrives/T4-2022/ \
    --fasta /content/drive/Shareddrives/T4-2022/homo_sapiens_refseq/Homo_sapiens_assembly19.fasta \
    --cache --offline --assembly GRCh37 --refseq  \
    --pick --pick_allele --force_overwrite --tab --symbol --check_existing --variant_class --everything --filter_common \
    --fields  "Uploaded_variation,Location,Allele,Existing_variation,HGVSc,HGVSp,SYMBOL,Consequence,IND,ZYG,Amino_acids,CLIN_SIG,PolyPhen,SIFT,VARIANT_CLASS,FREQS" \
    --individual all

  ```
    * Visualizar rapidamente o arquivo gerado
