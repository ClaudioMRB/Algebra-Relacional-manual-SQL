Dando continuidade à série de artigos para que o internauta entre no mundo dos bancos de dados, sugiro que você leia meus dois primeiros artigos (Conceitos Fundamentais de Banco de Dados – Parte I e II) e também os artigos do Reinaldo Viana (Banco de Dados e Modelagem de Dados – Parte I, II e Final), para que haja uma perfeita compreensão dos conceitos e metodologias de um projeto de BD.

Darei continuidade falando sobre Linguagem de Consulta Formal, abordando a Álgebra Relacional.

Linguagens de consulta formal são linguagens em que o usuário solicita informações à base de dados. Geralmente formam uma linguagem de mais alto nível que as [linguagens de programação](https://www.devmedia.com.br/introducao-as-linguagens-de-programacao/25111).

A Álgebra Relacional é uma linguagem de consulta formal, porém procedimental, ou seja, o usuário dá as instruções ao sistema para que o mesmo realize uma seqüência de operações na base de dados para calcular o resultado desejado.

A Álgebra Relacional define operadores para atuar nas tabelas (semelhante aos operadores +, -, etc. da álgebra que estamos acostumados) para chegar ao resultado desejado.

A forma de trabalho desta linguagem de consulta é a de pegar uma ou mais tabelas (conforme necessidade) como entrada de dados e produzirá uma nova tabela como resultado das operações.

### Funções da Álgebra Relacional

São definidas nove operações para se trabalhar com álgebra relacional:

- Union –União;
- Intersection– Intersecção;
- Difference– Diferença, Subtração;
- Product – Produto, Produto Cartesiano.

Estas quatro operações são provenientes da teoria de conjuntos, da matemática.

- Select– Seleção;
- Project– Projeção;
- Join– Junção;
- Divide – Divisão.

Aplicam-se especificamente ao modelo de dados relacional.

- Assignment– Designação, Atribuição.

É uma operação padrão das linguagens computacionais. Utilizaremos a seguinte tabela como estudo de caso para exemplificar nossas operações: **EMPREGADO**

![Atribuindo um valor a uma nova tabela](https://www.devmedia.com.br/imagens/sqlmagazine/mar2006/23-08pic01.JPG)

### Atribuindo um valor a uma nova tabela

O objetivo do operador de designação/atribuição é atribuir o resultado de uma operação a uma nova relação.

Simbologia: <-------- Ex.: R <----- AUB

Sintaxe: := Ex.: R := union(B, C)

### Operação de Seleção (Select)

É utilizada para selecionar um subconjunto de tuplas numa relação que satisfaça uma condição de seleção predefinida. Representação gráfica:

| Simbologia | σ                       |
| ---------- | ----------------------- |
| Sintaxe    | σ(Relação)              |
| Exemplo    | σ sal>=2500 (EMPREGADO) |

A seleção acima nos apresentará como resultado as informações abaixo:

![Operação de Seleção (Select)](https://www.devmedia.com.br/imagens/sqlmagazine/mar2006/23-08pic04.JPG)

### Operação de Projeção (Project)

A operação de projeção é utilizada para selecionar determinadas colunas de uma relação. A operação é executada em apenas uma relação e o resultado é uma nova relação contendo apenas os atributos selecionados, eliminando-se as duplicidades.

| Simbologia | π                                |
| ---------- | -------------------------------- |
| Sintaxe    | π(Relação)                       |
| π          | NOME, SOBRENOME, SAL (EMPREGADO) |

Teremos o seguinte resultado a partir da consulta acima:

| NOME    | SOBRENOME     | SAL  |
| ------- | ------------- | ---- |
| José    | da Silva      | 7000 |
| Cecília | Ortiz Rezende | 3200 |
| Pedro   | Silvestre     | 2800 |
| Felipe  | Guilhermino   | 1800 |
| Luciana | Feitosa       | 1500 |
| Fabio   | Santos Silva  | 1500 |
| Elaine  | Cristina      | 2500 |
| Cleiton | Fernandes     | 2200 |

### Aninhar de operações e renomear de atributos

Podemos aninhar as operações e produzir novos resultados sem a necessidade de sucessivas operações. Imaginem se nos interessa apenas o nome, sobrenome e salário dos funcionários do departamento número 3. Vejamos como ficaria a expressão em álgebra relacional:

- 1 => Relação = Tabela, entidade, na terminologia formal de banco de dados.
- 2 => Tupla = Linha da tabela, registro, na terminologia formal de banco de dados.

π NOME, SOBRENOME, SAL (σ DEPTO=3 (EMPREGADO)), que nos produziria o seguinte resultado:

| NOME    | SOBRENOME     | SAL  |
| ------- | ------------- | ---- |
| Cecília | Ortiz Rezende | 3200 |
| Felipe  | Guilhermino   | 1800 |
| Elaine  | Cristina      | 2500 |
| Cleiton | Fernandes     | 2200 |

Podemos ainda, criar relações intermediárias, dando um nome para cada uma delas e finalmente chegando ao resultado desejado:

```
R1 <---- σ DEPTO=3 (EMPREGADO)
```

E logo após:

RESULT <----- πNOME, SOBRENOME, SAL (R1), onde RESULT produziria o mesmo resultado da expressão aninhada.

E finalmente, ainda podemos renomear os atributos que aparecerão na relação resultante, para isso, basta identificar o novo nome para os atributos:

RESULT(Nome, Sobrenome, Salário) <----- πNOME, SOBRENOME, SAL (σ DEPTO=3 (EMPREGADO))

Resultado:

| NOME    | SOBRENOME     | SAL  |
| ------- | ------------- | ---- |
| Cecília | Ortiz Rezende | 3200 |
| Felipe  | Guilhermino   | 1800 |
| Elaine  | Cristina      | 2500 |
| Cleiton | Fernandes     | 2200 |

### Revendo a teoria dos conjuntos

Vamos descrever as funções da álgebra relacional pelas operações que vieram da teoria dos conjuntos:

#### União (Union)

O operador de união cria uma relação partindo de duas outras, levando as tuplas comuns e não comuns a ambas, desta forma aparecerão no resultado somente linhas únicas de uma ou outra relação e as informações duplicadas aparecerão somente uma vez.

Uma característica é que somente é possível utilizar este operador caso as tabelas de origem possuam compatibilidade de união, ou seja, as tabelas devem ser equivalentes e gerem o mesmo tipo de resultado.

Representação gráfica:

| Simbologia | **U**                       |
| ---------- | --------------------------- |
| Sintaxe    | (Relação 1)**U**(Relação 2) |

Exemplo: Imagine que precisemos recuperar A identificação de todos os empregados que trabalham no departamento 3 ou supervisione diretamente um empregado que trabalhe no departamento 3. Faremos as seguintes operações:

DEPTO3 <---- σ DEPTO=3 (EMPREGADO)

![DEPTO3](https://www.devmedia.com.br/imagens/sqlmagazine/mar2006/23-08pic05.JPG)DEPTO3

RESULT1 <---- π ID_EMP (DEPTO3)

RESULT1

| ID_EMP  |
| ------- |
| 12584-7 |
| 17987-5 |
| 16257-2 |
| 15234-1 |

RESULT2 <---- π ID_GER (DEPTO3)

RESULT2

| ID_GER  |
| ------- |
| 17206-2 |
| 12584-7 |

RESULT(ID) <----- (RESULT1)**U**(RESULT2)

RESULT

| ID      |
| ------- |
| 12584-7 |
| 17987-5 |
| 16257-2 |
| 15234-1 |
| 17206-2 |

Somente foi possível realizar a união entre RESULT1 e RESULT2, pois, apesar dos atributos serem diferentes, o número e o tipo de atributos são os mesmos, possibilitando uma compatibilidade de união.

#### Intersecção (Intersection)

A relação criada pela operação de intersecção será o resultado de todas as tuplas que pertençam a ambas as relações presentes na operação. Representação gráfica:

| Simbologia | ∩                       |
| ---------- | ----------------------- |
| Sintaxe    | (Relação 1)∩(Relação 2) |

Como exemplo, considere as seguintes relações:

ALUNOS

| NOME    | SOBRENOME     |
| ------- | ------------- |
| Cecília | Ortiz Rezende |
| João    | da Silva      |
| Laura   | Nogueira      |
| Elaine  | Cristina      |
| Paulo   | Vidigal       |
| Pedro   | Teodoro       |
| Sandra  | Oliveira      |
| Marcio  | de Souza      |

INSTRUTORES

| NOME     | SOBRENOME     |
| -------- | ------------- |
| Joel     | Nunes         |
| Marcio   | Santos        |
| Paula    | Andrade       |
| Reinaldo | Fagundes      |
| text     | text          |
| Cecília  | Ortiz Rezende |

Desta forma, uma operação de intersecção entre as duas relações, seria executada da seguinte forma:

RESULTADO <---- (ALUNOS)∩(INSTRUTORES) e produziria a seguinte relação:

RESULTADO

| NOME    | SOBRENOME     |
| ------- | ------------- |
| Marcio  | Santos        |
| Cecília | Ortiz Rezende |

Uma observação extremamente relevante a ser feita é que ambas as operações de união ou intersecção são:

- Comutativas, ou seja, A**U**B = B**U**A e A∩B = B∩A; Aplicadas a qualquer número de relações;
- Associativas, ou seja: A**U**U(B**U**UC) = (A**U**B)**U**C e A∩(B∩C) = (A∩B)∩C

É uma ferramenta bastante poderosa principalmente no auxílio à definição lógica de abordagem a dados em tabelas. Fica claro que muitos conceitos trazidos da matemática tradicional são extremamente aplicáveis a esta técnica.

Agora iremos abordar as duas últimas instruções provenientes da teoria de conjuntos, as operações Difference e Product, e também as duas operações restantes que interagem com o modelo relacional, Join e Divide.

O bom entendimento de como os operadores de álgebra relacional trabalham, facilitam muito a muito na construção de boas consultas em [linguagens como SQL](https://www.devmedia.com.br/guia/guia-completo-de-sql/38314), por exemplo. Aproveitem a navegação.

### Operação de Diferença (Difference)

A operação de diferença consiste em obter uma relação a partir da diferença da primeira pela segunda relação.

É importante salientar que a diferença entre a primeira e segunda relação não é o mesmo do inverso, ou seja, da segunda pela primeira. Com isso podemos dizer que a operação de diferença não é comutativa.

Exemplificando, poderíamos dizer que A – B é diferente de B – A.

| Simbologia | –                         |
| ---------- | ------------------------- |
| Sintaxe    | (Relação 1) – (Relação 2) |

Como exemplo, considere as relações vistas no artigo anterior:

ALUNOS

| NOME      | SOBRENOME     |
| --------- | ------------- |
| Cecília   | Ortiz Rezende |
| João      | da Silva      |
| Laura     | Nogueira      |
| Elaine    | Cristina      |
| Paulo     | Vidigal       |
| Pedro     | Teodoro       |
| Sandra    | Oliveira      |
| Marcio    | Santos        |
| Elisabeth | de Souza      |

INSTRUTORES

| NOME     | SOBRENOME     |
| -------- | ------------- |
| Joel     | Nunes         |
| Marcio   | Santos        |
| Paula    | Andrade       |
| Reinaldo | Fagundes      |
| Cecília  | Ortiz Rezende |

Exemplo 1:

Result1 <----- ALUNOS – INSTRUTORES

RESULT1

| NOME      | SOBRENOME |
| --------- | --------- |
| João      | da Silva  |
| Laura     | Nogueira  |
| Elaine    | Cristina  |
| Paulo     | Vidigal   |
| Pedro     | Teodoro   |
| Sandra    | Oliveira  |
| Elisabeth | de Souza  |

Exemplo 2:

Result2 <----- INSTRUTORES – ALUNOS

RESULT2

| NOME     | SOBRENOME |
| -------- | --------- |
| Joel     | Nunes     |
| Paula    | Andrade   |
| Reinaldo | Fagundes  |

### Operação de Produto Cartesiano (Product)

O Produto Cartesiano é a combinação de tuplas das duas relações em questão. O resultado é que, para cada tupla da primeira relação, haverá a combinação com todas as tuplas da segunda relação, e vice-versa.

| Simbologia | **x**                         |
| ---------- | ----------------------------- |
| Sintaxe    | (Relação 1) **x** (Relação 2) |

Como exemplo, considere as relações abaixo:

ALUNOS

| NOME    | SOBRENOME     |
| ------- | ------------- |
| Cecília | Ortiz Rezende |
| João    | da Silva      |
| Laura   | Nogueira      |
| Elaine  | Cristina      |

DISCIPLINA

| COD_DISC | DESCRICAO                            |
| -------- | ------------------------------------ |
| 1        | Fundamentos de Bando de Dados        |
| 2        | Linguagem de Programação             |
| 3        | Introdução aos Sistemas Operacionais |

RESULT <----- ALUNOS **x** INSTRUTORES

RESULT

| NOME    | SOBRENOME     | COD_DISC | DESCRICAO                            |
| ------- | ------------- | -------- | ------------------------------------ |
| Cecília | Ortiz Rezende | 1        | Fundamentos de Bando de Dados        |
| Cecília | Ortiz Rezende | 2        | Linguagem de Programação             |
| Cecília | Ortiz Rezende | 3        | Introdução aos Sistemas Operacionais |
| João    | da Silva      | 1        | Fundamentos de Bando de Dados        |
| João    | da Silva      | 2        | Linguagem de Programação             |
| João    | da Silva      | 3        | Introdução aos Sistemas Operacionais |
| Laura   | Nogueira      | 1        | Fundamentos de Bando de Dados        |
| Laura   | Nogueira      | 2        | Linguagem de Programação             |
| Laura   | Nogueira      | 3        | Introdução aos Sistemas Operacionais |
| Elaine  | Cristina      | 1        | Fundamentos de Bando de Dados        |
| Elaine  | Cristina      | 2        | Linguagem de Programação             |
| Elaine  | Cristina      | 3        | Introdução aos Sistemas Operacionais |

### Operação de Junção (Join)

Veremos agora as duas últimas operações que interagem com o modelo relacional.

A operação de junção é utilizada para combinar tuplas de duas relações partindo dos atributos comuns a ambas.

O resultado conterá as colunas das duas relações que estão participando da junção.

Esta operação é de extrema importância em bancos de dados relacionais, pois é através dela que nos é permitido fazer relacionamento.

| Simbologia | \|x\|                                              |
| ---------- | -------------------------------------------------- |
| Sintaxe    | (Relação 1) \|x\| <condição de junção> (Relação 2) |

Uma condição de junção pode ser formada por mais de uma condição simples, apenas aplicando os operadores relacionais AND ou OR.

Vejamos um exemplo, considerando as seguintes tabelas:

ALUNOS

| NOME    | SOBRENOME     | TURMA |
| ------- | ------------- | ----- |
| Cecília | Ortiz Rezende | 2TI   |
| João    | da Silva      | 1TI   |
| Laura   | Nogueira      | 2TI   |
| Elaine  | Cristina      | 2TI   |

TURMAS

| COD_TURMA | DESCRICAO                 |
| --------- | ------------------------- |
| 1TI       | 1º Módulo - Informática   |
| 2TI       | 2º Módulo - Informática   |
| 1TA       | 1º Módulo - Administração |
| 2TA       | 2º Módulo - Administração |

ALU_TUR <----- ALUNOS |x| TURMA=COD_TURMA TURMAS

ALU_TUR

| NOME    | SOBRENOME     | TURMA | COD_TURMA | DESCRICAO               |
| ------- | ------------- | ----- | --------- | ----------------------- |
| Cecília | Ortiz Rezende | 2TI   | 2TI       | 2º Módulo - Informática |
| João    | da Silva      | 1TI   | 1TI       | 1º Módulo - Informática |
| Laura   | Nogueira      | 2TI   | 2TI       | 2º Módulo - Informática |
| Elaine  | Cristina      | 2TI   | 2TI       | 2º Módulo - Informática |

E finalmente, podemos usar um conjunto de operações para trazer, por exemplo, apenas os alunos que cursam o 2º módulo de Informática:

RESULT <----- COD_TURMA>=2TI ( NOME, SOBRENOME, DESCRICAO (ALUNOS |x| TURMA=COD_TURMA TURMAS))

RESULT

| NOME    | SOBRENOME     | DESCRICAO               |
| ------- | ------------- | ----------------------- |
| Cecilia | Ortiz Rezende | 2º Módulo - Informática |
| Laura   | Nogueira      | 2º Módulo - Informática |
| Elaine  | Cristina      | 2º Módulo - Informática |

### Operação de Divisão (Divide)

É uma operação adicional que produz como resultado a projeção de todos os elementos da primeira relação que se relacionam com todos os elementos da segunda relação.

Não é um operador primitivo, mas pode ter o resultado obtido por uma combinação de operadores primitivos.

| Simbologia | ÷                         |
| ---------- | ------------------------- |
| Sintaxe    | (Relação 1) ÷ (Relação 2) |

Vejamos no exemplo abaixo:

EQUIPE

| ID_EMP  | COD_PROJ |
| ------- | -------- |
| 17206-2 | 001      |
| 12584-7 | 002      |
| 16764-6 | 001      |
| 17206-2 | 002      |
| 15698-3 | 003      |
| 17206-2 | 003      |

PROJETOS

| COD_PROJ | DESCRICAO     |
| -------- | ------------- |
| 001      | Sistema IRPF  |
| 002      | Sistema RH    |
| 003      | Sistema Banco |

FUNCIONARIO

| ID_EMP  | NOME      | CARGO        |
| ------- | --------- | ------------ |
| 17206-2 | Jorge     | Analista     |
| 12584-7 | Paula     | Programadora |
| 16764-6 | Frederico | DBA          |
| 15698-3 | Heloisa   | Web Master   |

Imagine a situação de querermos saber quais os funcionários que trabalham em todos os projetos:

RESULT <----- ( COD_PROJ (PROJETOS)) ÷ ( ID_EMP, COD_PROJ (EQUIPE))

RESULT

| ID_EMP  |
| ------- |
| 17206-2 |

| SÍMBOLO | OPERAÇÃO                     | SINTAXE                                            | TIPO      |
| ------- | ---------------------------- | -------------------------------------------------- | --------- |
| <-----  | Atribuição                   | Variável <----- Relação                            | Primitiva |
| σ       | Seleção (Select)             | <condicao de elecao>(Relação)                      | Primitiva |
| π       | Projeção (Project)           | <lista de atributos>(Relação)                      | Primitiva |
| ∪       | União (Union)                | (Relação 1) ∪ (Relação 2)                          | Primitiva |
| ∩       | Interseção (Intersection)    | (Relação 1) ∩ (Relação 2)                          | Adicional |
| –       | Diferença (Difference)       | (Relação 1) – (Relação 2)                          | Primitiva |
| X       | Produto Cartesiano (Product) | (Relação 1) X (Relação 2)                          | Primitiva |
| \|x\|   | Junção (Join)                | (Relação 1) \|x\| <condição de junção> (Relação 2) | Adicional |
| ÷       | Adicional                    | (Relação 1) ÷ (Relação 2)                          | Adicional |

#### Conclusões

Finalizamos aqui este artigo sobre **Álgebra Relacional**. Tivemos uma boa noção de como é trabalhada a lógica de consultas através desta linguagem formal e podemos concluir que é uma ótima metodologia de desenvolvimento de raciocínio por parte de quem está aplicando a técnica.

Este desenvolvimento de raciocínio lógico em consultas a bancos de dados será muito útil na construção de instruções que irão interagir com o [banco de dados](https://www.devmedia.com.br/conceitos-fundamentais-de-banco-de-dados/1649), porém construindo essas instruções de maneira performática, tirando o máximo de desempenho possível.



[Álgebra Relacional Tutorial (devmedia.com.br)](https://www.devmedia.com.br/algebra-relacional-tutorial/2663)
