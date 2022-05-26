# *DNA Profiler*
<sup>Última atualização: 02/05/2022</sup>

## Introdução
O DNA, abreviação em inglês de *deoxyribonucleic acid* (ácido desoxirribonucleico), é um composto orgânico formado por duas cadeias cujas moléculas, chamadas nucleotídeos, contêm as instruções genéticas que coordenam o desenvolvimento e funcionamento de todos os seres vivos e que transmitem as suas características hereditárias. As cadeias do DNA apresentam-se como uma dupla hélice constituída de repetidos pares de nucleotídeos firmemente associados por ligações de hidrogênio. Cada nucleotídeo é composto de uma de quatro bases nitrogenadas: *adenina* (A), a *citosina* (C), a *guanina* (G) e a *timina* (T).

Cada célula humana tem milhões desses nucleotídeos agrupados em sequência. Algumas porções dessa sequencia (por exemplo, o genoma) são iguais, ou ao menos muito similares, entre todos os humanos, mas outras partes tem uma alta diversidade genética, logo variando entre as pessoas. Um ponto onde o DNA tende a ser muito diverso é nos chamados [*Short Tandem Repeats* (STRs)](https://en.wikipedia.org/wiki/Microsatellite), também chamados de microsatélites. Um STR é uma sequencia curta de DNA que se repete uma determinada quantidade de vezes em lugares específicos do DNA. O número de vezes que um STR se repete (geralmente entre 5 e 50 vezes) varia muito entre as pessoas, promovendo assim alta diversidade genética. No exemplo abaixo, o STR `AGAT`é repetido quatro vezes no DNA de uma pessoa (Alice), enquanto outra pessoa (Bob) tem o mesmo STR repetido seis vezes.

<img src="img/ALICE_BOB_DNA" />

A comparação dos STRs é utilizada principalmente em um processo chamado [*DNA profiling*](https://en.wikipedia.org/wiki/DNA_profiling), também chamado de impressão genética, desenvolvido em 1984 pelo geneticista britânico [Sir Alec Jeffreys](https://pt.wikipedia.org/wiki/Alec_Jeffreys). Através desse processo, cientistas forenses comparam amostras de DNA encontradas, por exemplo, em uma cena de crime com o DNA do suposto criminoso a fim de obter evidência acerca da probabilidade de esse indivíduo estar envolvido no crime. O *DNA profiling* é utilizado também em testes de paternidade para determinar se um indivíduo é o pai ou a mãe biológica de uma pessoa, com probabilidade de acerto de 99.99%, tornando o método altamente confiável para realizar esse tipo de detecção.

Se múltimos STRs forem usados, ao invés de apenas um único, é possível aumentar a acurácia do processo de *DNA profiling*. Por exemplo, se a probabilidade de duas pessoas terem o mesmo número de repetições de um STR é de 5% e os analistas usam dez diferentes STRs, a problabilidade de duas amostras de DNA serem iguais, apenas por sorte é de um em um quadrilhão (assumindo que cada STR é independente um do outro). Assim, se duas amostras de DNA têm o mesmo número de repetições de STRs, há alta confiabilidade na detecção de que aquele DNA analisado é proveniente da mesma pessoa. O [CODIS](https://www.fbi.gov/services/laboratory/biometric-analysis/codis/codis-and-ndis-fact-sheet), a base de dados do *Federal Bureau of Investigation* (FBI) dos EUA, usa 20 diferentes STRs como parte da análise de amostras de DNA.

Na forma mais simples, uma base de dados de DNA pode ser formatada em um arquivo de texto [CSV (valores separados por vírgula)](https://en.wikipedia.org/wiki/Comma-separated_values) onde cada linha corresponde a um indivíduo e cada coluna corresponde a um STR particular. Um exemplo de conteúdo desse tipo de arquivo, cuja primeira linha contém cabeçalhos para cada uma das colunas, seria:

```
name,AGAT,AATG,TATC
Alice,28,42,14
Bob,17,22,19
Charlie,36,18,25
```

Nesse exemplo, Alice tem a sequencia `AGAT` repetida 28 vezes em algum lugar de seu DNA, bem como as sequências `AATG` e `TATC` repetidas 42 e 14 vezes, respectivamente. De forma similar, essas sequências encontram-se repetidas 17, 22 e 19 vezes no DNA de Bob e 36, 18 e 25 vezes no DNA de Charlie.

Dada uma sequencia de DNA, como um investigador poderia, através do *DNA profiling* identificar quem é o seu dono? Suponha que se busque na sequencia de DNA pela sequencia mais longa de `AGAT` e se encontre que ela tem tamanho 17, ou seja, uma sequência de 17 ocorrências de `AGAT` consecutivas. O processo é então repetido para `AATG` e para `TATC` e se descobre que elas se repetem 22 e 19 vezes. Com isso, conclui-se pela probabilidade de que o DNA analisado pertença a Bob.

## Tarefas
Escreva um programa chamado *dnaprofiler* que realiza o processamento de DNA e executa o processo de *DNA profiling*. O programa deverá receber via linha de comando duas entradas representando dois arquivos. O primeiro arquivo, no formato CSV, contém uma base de dados de DNA, e o segundo é um arquivo de texto contendo a sequência de DNA que deve ser verificada. O programa deverá então processar as entradas, realizar o *profiling* (isto é, procurar pela sequencia mais longa com consecutivos STRs) e comparar com a base de dados fornecida como entrada, procurando se o perfil resultante do processamento está presente na base de dados. Caso o perfil seja encontrado, o programa deverá imprimir na saída padrão o nome da pessoa identificada e, em caso negativo, o programa deverá imprimir a mensagem *no match found*, indicando que não foi encontrado nenhuma pessoa na base de dados correspondendo às características encontradas.

O programa deverá ser executado (inclusive com os argumentos adicionais `-d` e `-s`) da seguinte forma:

```bash
$ ./dna_profiler -d <database_file> -s <dna_sequence_file>
```

sendo `database_file` o arquivo CSV correspondente à base de dados de DNA e `dna_sequence_file` o arquivo de texto com a sequência de DNA que se deseja identificar. Caso os argumentos não sejam fornecidos corretamente, o usuário deverá ser informado acerca do erro e como o programa deve ser executado corretamente.

Uma possível sequência de passos a serem seguidos na implementação do programa *dnaprofiler* seria:
1. Ler a base de dados de DNA e armazenar as informações em uma variável/objeto
2. Ler a sequência de DNA a ser identificada e armazenar em outra variável/objeto
3. Gerar o perfil da sequencia de DNA carregada
4. Procurar pelo pefil gerado na base de dados de DNA
     1. Se o indivíduo for encontrado, imprimir na saída padrão seu nome e exibir os STRs
     2. Se o indivíduo não for encontrado, imprimir a mensagem *no match found*

## Arquivos de entrada
A base de dados de DNA, fornecida como um arquivo no formato CSV, é basicamente uma tabela em que:

1. a primeira linha contém os nomes das colunas separadas por `,` (vírgula);
2. na primeira linha, a primeira coluna é sempre `name` seguida por uma quantidade *n* de STRs que devem ser considerados ao analisar as sequencias de entrada, e;
3. as linhas seguintes, **em quantidade não determinada**, contêm cada uma o nome de um indivíduo e seu perfil de DNA correspondente, na forma do número máximo que uma sequencia de STRs repetidos aparecem em seu DNA.

Para o exemplo anteriormente apresentado, o arquivo CSV fornecido como entrada representa a tabela abaixo:

| name | `AGAT` | `AATG` | `TATC`
|:--------:| -------------:|-------------:|-------------:|
| Alice | 28 | 42 | 14
| Bob | 17 | 22 | 19
| Charlie | 36 | 18 | 25

O segundo arquivo de texto representa o segmento de DNA do indivíduo que deve ser analisado.

Este é o segmento do DNA de Alice:
> AGACGGGTTACCATGACTATCTATCTATCTATCTATCTATCTATCTATCACGTACGTACGTATCGAGATAGATAGATAGATAGATCCTCGACTTCGATCGCAATGAATGCCAATAGACAAAA

Este é o segmento do DNA de Bob:
> AACCCTGCGCGCGCGCGATCTATCTATCTATCTATCCAGCATTAGCTAGCATCAAGATAGATAGATGAATTTCGAAATGAATGAATGAATGAATGAATGAATG

Este é o segmento do DNA de Charlie:
> CCAGATAGATAGATAGATAGATAGATGTCACAGGGATGCTGAGGGCTGCTTCGTACGTACTCCTGATTTCGGGGATCGCTGACACTAATGCGTGCGAGCGGATCGATCTCTATCTATCTATCTATCTATCCTATAGCATAGACATCCAGATAGATAGATC

Os arquivos referentes à base de dados e aos segmentos de DNA a serem analisados estão disponíveis no diretório [`data`](data).


## Modelagem
**Pelo menos três classes** devem ser modeladas para o programa *dnaprofiler*:
1. uma classe para armazernar a base de dados e realizar busca por perfis;
2. uma classe para armazenar a informação de DNA de um indivíduo, bem como realizar o perfil com base em algum STR (ou conjunto de STRs), e;
3. uma classe para centralizar as saídas para o usuário.

## Estrutura do projeto
Primando pela modularização, a definição e a implementação das classes deverá ser separada em arquivos cabeçalho (`.h`) e de corpo (`cpp`). **Necessariamente**, os arquivos de cabeçalho deverão estar presentes no diretório `include` e os arquivos de corpo no diretório `src`. O arquivo `main.cpp` correspondente à implementação da função principal do programa deverá **necessariamente** estar também presente no diretório `src`. Dessa forma, os arquivos deste projeto deverão estar organizado de acordo com a seguinte estrutura:

```
+─casa_rua            ---> Nome do diretório do projeto
  ├─── CMakeLists.txt ---> Script de configuração do cmake
  ├─── build          ---> Diretório onde os arquivos executáveis serão gerados
  ├─── data_expected  ---> Diretório que contém os arquivos de saída com as respostas corretas
  ├─── data_in        ---> Diretório que contém os arquivos de entrada para os testes
  ├─── include        ---> Diretório que contém os arquivos cabeçalho (.h)
       └─── casa.h    ---> Arquivo cabeçalho referente à definição da classe Casa
       └─── rua.h     ---> Arquivo cabeçalho referente à definição da classe Rua
  └─── src            ---> Diretório que contém os arquivos corpo (.cpp)
       └─── casa.cpp  ---> Arquivo fonte referente à implementação da classe Casa
       └─── rua.cpp   ---> Arquivo fonte referente à implementação da classe Rua
       └─── main.cpp  ---> Arquivo fonte contendo a implementação da função principal do programa
```

## Execução de testes automatizados
A execução dos testes automatizados para este programa deve ser feita a partir do presente diretório, executando os seguintes comandos:

```
mkdir build
cd build
cmake ..
cmake --build . --target verify
```

Caso haja erro de compilação ou, ao se observar os resultados do testes, identifique-se alguma falha, deve-se corrigir o problema e compilar o projeto novamente conforme descrito anteriormente.

## Conhecimentos necessários
- Entrada/saída padrão
- Laços
- Estruturas condicionais e expressões lógicas
- Passagem de parâmetros por valor e por referência
- Classes e objetos
- Visibilidade de membros de classes
- Construtores
- *Containers* da [*Standard Template Library* (STL)](https://www.cplusplus.com/reference/stl/)
- Classe [`std::pair`](https://www.cplusplus.com/reference/utility/pair/pair/), definida na biblioteca [`utility`](https://www.cplusplus.com/reference/utility/)
