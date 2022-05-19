# Indicador Pontos Status
### KDL API REST para Aferição de Indicadores

Este endpoint é resposável pela entrega de informações da telegestão informando o status da luminária no momento solicitado.

Como parte da URI se faz necessário definir o dia, mês e ano a ser consultado, opcionalmente pode ser informado através de parâmetro a hora desejada.

| URI | Exemplo                                                    | 
| --- | :-----------                                               | 
| `/status/00/00/0000` | api-afericao.kdltelegestao.com.br/status/4/2022 |

##### Body:
| Indentificador | Tipo     | Descrição                                                                        | 
| -------------- | -------  | :------------------------------------------------------------------------------   | 
| body    | `Array<string>` | Lista de identificadores de etiquetas (idEtiqueta) que deverá ser utilizadas no filtro de busca |


##### Parametros:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| hora           | `int` |  **Todas** | Hora para realizar consulta de informações dos pontos comissionados               |
| consolidado    | `bool` |  **true** | Indica se deve ou não retornar os dados consolidados dos indicadores encontrados  |


#### Dados consolidado
Por default o resultado da consulta retorna as informações **consolidadas** dos pontos comissionados. Esta informação pode ser retirada do resultado da consulta através da passagem do parâmetro `consolidado=false`.
| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| POST | `/status/00/00/0000` | ?hora=15&consolidado=false | api-afericao.kdltelegestao.com.br/status/4/2022?consolidado=false |

### Exemplo do Resultado:
``` json
{
  "data": {
    "ultimoStatus": [
      {
        "etiqueta": "IP00000002A",
        "subPrefeitura": "LAPA",
        "comissionadoEm": "15/04/2022",
        "latLng": [0.000000, 0.000000],
        "status": 1,
        "createdAt": "2022-04-25T02:49:02.781+00:00"
      }
    ]
  },
  "total": 1,
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
| elapsedTime   | `string` | Tempo de duração da consulta         |  

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| ------ | ---------| :------------------------------------------                  | 
| ultimoStatus | `object` | Consolidação do resultado dos indicadores encontrados  | 

**Para reduzir o tempo necessários para obter respostas do serviço da API, o bloco de resultados `consolidado` pode opcionalmente ser removido da consulta através da passagem de parâmetros*

##### Bloco CONSOLIDADO:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------| :------------------------------------------        | 
| dia                 | `int`    | Dia                                             | 
| mes                 | `int`    | Mês                                             | 
| ano                 | `int`    | Ano                                             | 
| hora                | `int`    | Hora (opcional)                                 | 


##### Bloco PONTOS COMISSIONADOS:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm | `date`    | Data do comissionamento da luminária                | 
| latLng  | `array<float>`    | Posição geográfica da lumiária                     | 
| status  | `int`    | Status atual da luminária no momento solicitado (0 = Desligado / 1 = Ligado)| 
| createdAt  | `date`    | Data e hora da criação do registro de aferição          | 
