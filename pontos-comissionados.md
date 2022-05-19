# Indicador Pontos Comissionados
### KDL API REST para Aferição de Indicadores

Este endpoint é resposável pela entrega de informações da telegestão dos pontos comissionados.

Como parte da URI se faz necessário definir o mês e ano a ser consultado.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/pontos-comissionados/0/0000` | api-afericao.kdltelegestao.com.br/pontos-comissionados/4/2022 |

##### Parametros:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| consolidado    | `bool` |  **true** | Indica se deve ou não retornar os dados consolidados dos indicadores encontrados  |


#### Dados consolidado
Por default o resultado da consulta retorna as informações **consolidadas** dos pontos comissionados. Esta informação pode ser retirada do resultado da consulta através da passagem do parâmetro `consolidado=false`.
| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/pontos-comissionados/0/0000` | ?consolidado=false | api-afericao.kdltelegestao.com.br/pontos-comissionados/4/2022?consolidado=false |

### Exemplo do Resultado:
``` json
{
  "data": {
    "pontosComissionados": [
      {
        "etiqueta": "IP00000002A",
        "subPrefeitura": "LAPA",
        "comissionadoEm": "15/04/2022",
        "latLng": [0.000000, 0.000000]
      }
    ],
    "consolidado": {
      "ano": 2022,
      "mes": 4
    }
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
| pontosComissionados| `object` | Informações sobre os pontos comissionados        | 
| consolidado* | `object` | Consolidação do resultado dos indicadores encontrados  | 

**Para reduzir o tempo necessários para obter respostas do serviço da API, o bloco de resultados `consolidado` pode opcionalmente ser removido da consulta através da passagem de parâmetros*

##### Bloco CONSOLIDADO:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------| :------------------------------------------        | 
| ano                 | `int`    | Ano                                             | 
| mes                 | `int`    | Mês                                             | 


##### Bloco PONTOS COMISSIONADOS:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm | `date`    | Data do comissionamento da luminária                | 
| latLng  | `array<float>`    | Posição geográfica da lumiária                     | 
