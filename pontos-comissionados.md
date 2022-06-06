# Indicador Pontos Comissionados
### KDL API REST para Aferição de Indicadores

Este endpoint é resposável pela entrega de informações da telegestão dos pontos comissionados.

Como parte da URI é necessário definir o mês e ano a ser consultado.

| Método |  Parâmetros | URI | Exemplo                                                    | 
| --- | :---: | --- | :-----------                                               | 
| GET | SIM | `/pontos-comissionados/0/0000` | api-afericao.kdltelegestao.com.br/pontos-comissionados/4/2022 |

##### Parâmetros opcionais:
| Identificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| offset     | `int`  |  **0**    | O número de documentos a serem ignorados no conjunto de resultados.                                           |
| subPrefeitura     | `string`  |  -    | Identificador da subPrefeitura |

| Exemplo de uso dos parâmetros       | 
| :----------- | 
| ?limit=200&offset=500&subPrefeitura=LAPA |

### Exemplo do Resultado:
``` json
{
  "data": {
    "type": "Pontos Comissionados",
    "result": [
      {
        "etiqueta": "IP00000002A",
        "subPrefeitura": "LAPA",
        "comissionadoEm": "15/04/2022",
        "latLng": [0.000000, 0.000000]
      }
    ],
  },
  "total": 1,
  "offset": 0,
  "limit": 1000,
  "success": true,
  "elapsedTime": "123.18s"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Identificador | Tipo | Descrição | 
| :------ | ---------| :-----------------------------------------                  | 
| data   | `object` | Resultado da consulta                                        | 
| total  | `int`    | Quantidade de itens encontrados                              | 
| limit  | `int`    | Quantidade de itens retornados por página                    | 
| offset | `int`    | O número de documentos a serem ignorados no conjunto de resultados.  |
| success| `bool`   | Status de sucesso/falha da interação                         | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Identificador | Tipo | Descrição                                                | 
| :------ | ---------| :------------------------------------------                 | 
| type   | `string` | Identifica o tipo de indicador consultado                    | 
| result| `array<object>` | Lista de peças encontradas                             |

##### Bloco RESULT:
| Identificador | Tipo | Descrição | 
| :------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm | `string`    | Data do comissionamento da luminária                | 
| latLng  | `array<float>`    | Posição geográfica da lumiária                     | 
