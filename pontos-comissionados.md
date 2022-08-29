# Indicador Pontos Comissionados
### KDL API REST para Aferição de Indicadores

Este endpoint é resposável pela entrega de informações da telegestão dos pontos comissionados.

Como parte da URI é necessário definir o mês e ano a ser consultado.

| Método |  URI | Exemplo                                                                         | 
| --- |  --- |:--------------------------------------------------------------------------------| 
| GET |  `/pontos-comissionados?limit&offset` | http://api-afericao.kdltelegestao.com/pontos-comissionados?limit=1000&offset=0 |

##### Parâmetros opcionais:
| Identificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| offset     | `int`  |  **0**    | O número de documentos a serem ignorados no conjunto de resultados.                                           |

| Exemplo de uso dos parâmetros            | 
|:-----------------------------------------| 
| ?limit=200&offset=400 |

### Exemplo do Resultado:
``` json
{
    "data": {
        "type": "Pontos Comissionados",
        "result": [
            {
                "_id": "62fb8aabfc6177680f1c49d3",
                "etiqueta": "IP0505619",
                "nserlum": 2220000097,
                "subPrefeitura": "LAPA",
                "comissionadoEm": "2022-07-01",
                "latitude": "-23.52666",
                "longitude": "-46.70880"
            }
        ]
    },
    "total": 1,
    "offset": 0,
    "limit": 1,
    "success": true,
    "elapsedTime": "3.683228ms"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Identificador | Tipo     | Descrição                                                           | 
|:--------------|----------|:--------------------------------------------------------------------| 
| data          | `object` | Resultado da consulta                                               | 
| total         | `int`    | Quantidade de itens encontrados                                     |
| totalRetornado | `int64`  | Quantidade de itens retornados                                     |
| limit         | `int`    | Quantidade de itens retornados por página                           | 
| offset        | `int`    | O número de documentos a serem ignorados no conjunto de resultados. |
| success       | `bool`   | Status de sucesso/falha da interação                                | 
| elapsedTime   | `string` | Tempo de duração da consulta                                        | 

##### Bloco DATA:
| Identificador | Tipo            | Descrição                                             | 
|:--------------|-----------------|:------------------------------------------------------| 
| type          | `string`        | Identifica o tipo de indicador consultado             | 
| result        | `array<object>` | Lista de peças encontradas                            | 

##### Bloco RESULT:
| Identificador   | Tipo     | Descrição                             | 
|:----------------|----------|:--------------------------------------| 
| id              | `string` | Identificador do documento            |
| etiqueta        | `string` | Identificador universal da luminária  |
| nserlum         | `int`    | Identificador da peça no sistema KDL  |
| subPrefeitura   | `string` | Identificador da SubPrefeitura        | 
| comissionadoEm  | `string` | Dia do comissionamento da peça        | 
| latitude        | `string` | Latitude                              |
| longitude       | `string` | Longitude                             |


