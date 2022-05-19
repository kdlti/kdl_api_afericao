# Indicador FDI-b
### KDL API REST para Aferição de Indicadores

Este endpoint é resposável pela entrega de informações da telegestão do indicaor FDI-b.

Como parte da URI se faz necessário definir o mês e ano a ser consultado.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/fdi-b/0/0000` | api-afericao.kdltelegestao.com.br/fdi-b/4/2022 |

##### Parametros:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| indicadores    | `bool` | **false** | Indica se deve ou não retornar a lista de indicadores                             | 
| consolidado   | `bool`  |  **true** | Indica se deve ou não retornar os dados consolidados dos indicadores encontrados  |
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| numberPage     | `int`  |  **1**    | Número da página que deve ser retornado                                           |


#### Dados consolidado
Por default o resultado da consulta retorna as informações **consolidadas** das peças comissionadas para este indicador. Esta informação pode ser retirada do resultado da consulta através da passagem do parâmetro `consolidado=false`.
| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/0/0000` | ?consolidado=false | api-afericao.kdltelegestao.com.br/fdi-b/4/2022?consolidado=false |

#### Filtro por Subprefeitura
Para recuperar informações **consolidadas** das peças comissionadas para este indicador agrupadas por subprefeituras se faz necessário definir como parte da URI o mês e ano, incluíndo o parâmetro `subPrefeitura=LAPA`.

| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/0/0000` | ?subPrefeitura=LAPA | api-afericao.kdltelegestao.com.br/fdi-b/4/2022?subPrefeitura=LAPA |


#### Indicadores Detalhado 
Para recuperar informações `detalhadas` das peças comissionadas para este indicador se faz necessário definir como parte da URI o mês e ano, incluíndo o parâmetro `indicadores=true`.

| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/0/0000` | ?indicadores=true | api-afericao.kdltelegestao.com.br/fdi-b/4/2022?indicadores=true | 

#### Filtro por Subprefeitura
Para recuperar informações `detalhadas` das peças comissionadas para este indicador agrupadas por subprefeituras se faz necessário definir como parte da URI o mês e ano, incluíndo os parâmetros `indicadores=true` e `subPrefeitura=LAPA`.

| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/0/0000` | ?indicadores=true&subPrefeitura=LAPA | api-afericao.kdltelegestao.com.br/fdi-b/4/2022?indicadores=true&subPrefeitura=LAPA |

#### Detalhamento de indicadores para uma única peça
Para recuperar informações `detalhadas` de uma única peça comissionada para este indicador se faz necessário definir como parte da URI o mês, ano e a etiqueta de identificação da peça `idEtiqueta`.

| Método | URI |  Exemplo      | 
| --- | --- |  :----------- | 
| GET | `/fdi-b/0/0000/idEtiqueta` | api-afericao.kdltelegestao.com.br/fdi-b/4/2022/IP00000002A | 


### Exemplo do Resultado:
``` json
{
    "data": {
        "tipo": "FDI-b",
        "indicadores": [
            {
                "etiqueta": "IP00000002A",
                "subPrefeitura": "LAPA",
                "minutosNecessarios": 18000,
                "minutosLigado": 18000,
                "diasMonitorado": 30,
                "resultado": "100%",
                "status": 1,
                "dias": {
                    "1": {
                        "horas": {
                            "17": 60,
                            "18": 60,
                            "19": 60,
                            "...": 0,
                            "5": 60,
                            "6": 60,
                        },
                        "minutosLigado": 300
                    },
                    "31": {...},
                },
            },
        ],
        "consolidado": {
            "ano": 2022,
            "mes": 5,
            "pontosComissionados": 3,
            "minutosNecessarios": 54000,
            "minutosLigado": 54000,
            "diasMonitorado": 31,
            "resultado": "100%",
            "status": 1
        }
    },
    "total": 2,
    "pageNumber": 1,
    "limit": 1000,
    "success": true,
    "elapsedTime": "123.18722s"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Indentificador | Tipo | Descrição | 
| ------ | ---------| :------------------------------------------ | 
| data   | `object` | Resultado da consulta                       | 
| total  | `int`    | Quantidade de itens encontrados             | 
| pageNumber  | `int` | Número da página de resultado             | 
| limit  | `int`    | Quantidade de itens retornados por página   | 
| success| `bool`   | Status de sucesso/falha da interação        | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| ------ | ---------| :------------------------------------------                  | 
| tipo   | `string` | Identifica o tipo de indicador consultado                    | 
| indicadores*| `array<object>` | Lista de indicadores encontrados                 | 
| consolidado* | `object` | Consolidação do resultado dos indicadores encontrados  | 


**Para reduzir o tempo necessários para obter respostas do serviço da API, os blocos de resultados `indicadores` e `consolidado` podem opcionalmente ser removido da consulta através da passagem de parâmetros*

##### Bloco CONSOLIDADO:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------| :------------------------------------------        | 
| ano                 | `int`    | Ano                                             | 
| mes                 | `int`    | Mês                                             | 
| pontosComissionados | `int`    | Quantidade de pontos comissionados              | 
| minutosNecessarios  | `int`    | Quantidade de minutos necessários ligado        | 
| minutosLigado       | `int`    | Quantidade de minutos efetivamente ligado       | 
| diasMonitorado      | `int`    | Quantidade de dias monitorado                   | 
| resultado           | `string` | Resultado do cálculo da fórmula em porcentagem  | 
| status              | `float`  | Resultado do cálculo da fórmula em decimal      | 

##### Bloco INDICADORES:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| minutosNecessarios  | `int`    | Quantidade de minutos necessários ligado        | 
| minutosLigado       | `int`    | Quantidade de minutos efetivamente ligado       | 
| diasMonitorado      | `int`    | Quantidade de dias monitorado                   | 
| resultado           | `string` | Resultado do cálculo da fórmula em porcentagem  | 
| status              | `float`  | Resultado do cálculo da fórmula em decimal      | 
| dias                | `int`    | Envolve o bloco de dias monitorado              | 

##### Bloco DIAS:
| Indentificador | Tipo | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| 0~31           | `string` | Identificador do dia monitorado                      | 
| horas          | `string` | Identificador da hora monitorado                     |
| minutosLigado  | `int`    | Soma de todos os minutos ligados no dia              |

##### Bloco HORAS:
| Indentificador | Tipo     | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| 0~23           | `string` | Quantidade de minutos ligado                         |
