# Guia
Esse é um guia simples para instalar e rodar a ferramenta de tipagem de HLA [OptiType](https://github.com/FRED-2/OptiType) a partir da linha de comando. 

Referência: [SZOLEK, et al. OptiType: precision HLA typing from next-generation sequencing data. 2014](https://pubmed.ncbi.nlm.nih.gov/25143287/).

## Dependências
Apesar do OptiType utilizar uma série de programas (Python, RazerS, SAMtools, HDF5, CPLEX) e módulos do Python (NumPy, Pyomo, PyTables, Pandas, Pysam, Matplotlib, Future) para funcionar, a instalação de cada uma das ferramentas em seu computador não é obrigatória.

A ferramenta está disponibilizada via Docker, uma plataforma que possibilita o empacotamento de uma aplicação ou ambiente dentro de um container, permitindo que qualquer pessoa possa executar o programa, desde que o Docker esteja instalado em seu computador. Para instalar o Docker, basta acessar a [página](https://www.docker.com/) deles e baixar o executável, e seguir o [passo a passo de instalação](https://docs.docker.com/engine/install/) (Docker está disponível para todos os sistemas operacionais).

## Instalando o OptiType
Com o docker instalado, faremos a instalação do OptiType. Para isso, basta usar o comando ```docker pull fred2/optitype```.

## Como rodar o OptiType
Após baixar a imagem do Docker, basta usar a linha de comando abaixo para rodar o OptiType:

```
mkdir /caminho/para/o/seu/diretório/resultado/
chmod 777 /caminho/para/o/seu/diretório/resultado/
docker run -v /caminho/para/o/seu/diretório/:/data/ -t fred2/optitype -i input1 [input2] (-r|-d) -o /data/resultado/
```
Sendo as flags:

```
Verbose (-v)-  Caminho para o diretório em seu computador onde o dado que terá o HLA tipado pelo Optitype. Esse caminho deve ser direcionado para o diretório /data/ dentro da imagem do docker através da utilização de (:) (Exemplo: /caminho/para/o/seu/diretório/:/data/)
Image tag (-t fred2/optitype)- Imagem que está sendo chamada para excução pelo docker 
Input (-i)- Arquivo(s) .fastq ou arquivos .bam gerados anteriormente pelo próprio OptiType, e guardados para reanálise. Um arquivo: modo single-end, dois arquivos: modo paired-end.
RNA/DNA (-r|-d)- Indica se o dado de sequenciamento provém de sequenciamento de DNA ou RNA 
Output (-o)- Diretório onde os resultados serão depositados. O output deve ser o diretório /data/resultado/ da imagem do docker.
```

## Rodando o teste do OptiType
Para rodar o teste do OptiType, siga o seguinte passo a passo:
  1. Copie o diretório do github do OptiType para seu computador: ```git clone https://github.com/FRED-2/OptiType.git```;
  2. Copie os arquivos testes para o seu diretório de análise:
```
## Para o teste com dados de DNA:
cp OptiType/test/exome/* /caminho/para/o/seu/diretório/

## Para o teste com dados de RNA:
cp OptiType/test/rna/* /caminho/para/o/seu/diretório/
```
  3. Crie uma pasta de resultado no seu diretório e de permissões globais para a pasta:
```
## Criar o diretório de resultado:
mkdir /caminho/para/o/seu/diretório/resultado/

## Dê permissões globais para o diretório de resultado:
chmod 777 /caminho/para/o/seu/diretório/resultado/
```
  4. Rode o comando:
```
## Comando para o teste com dado de DNA:
docker run --name test -v /caminho/para/o/seu/diretório/:/data/ -t fred2/optitype -i NA11995_SRR766010_1_fished.fastq NA11995_SRR766010_2_fished.fastq -d -o /data/result/ -v

## Comando para o teste com dado de RNA:
docker run --name test -v /caminho/para/o/seu/diretório/:/data/ -t fred2/optitype -i CRC_81_N_1_fished.fastq CRC_81_N_2_fished.fastq -r -o /data/result/ -v
```
  5. Para visualizar o resultado basta utilizar o comando abaixo:

```
less -S /caminho/para/o/seu/diretório/resultado/data_hora/data_hora_result.tsv
```
O resultado deve ser igual a este: 

| A1	| A2	| B1	| B2	| C1	| C2	| Reads	| Objective |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| A*01:01	| A*01:01	| B*08:01	| B*57:01	| C*07:01	| C*06:02	| 1156.0	| 1135.192 |


