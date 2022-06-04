# Indicador FDI-b
### KDL API REST para Aferição de Indicadores
######*versão 1.0.0*

Este endpoint é resposável pela entrega de informações da telegestão do indicador FDI-b.

Como parte da URI se faz necessário definir o mês e ano a ser consultado.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/fdi-b/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/04/2022 |

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/fdi-b/04/2022',
  headers: {
     'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c'
  },
  rejectUnauthorized: false
}, function(err, res) {
      if(err) {
        console.error(err);
      } else {
        console.log(res.body);
      }
});
```

##### Parâmetros opcionais:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| resumo   | `bool`  |  **true** | Indica se deve ou não retornar os dados resumidos dos indicadores encontrados  |
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| offset     | `int`  |  **0**    | O número de documentos a serem ignorados no conjunto de resultados.                                           |

#### Filtro por Subprefeitura
Para recuperar informações consolidadas das peças comissionadas para este indicador agrupadas por subprefeituras se faz necessário definir como parte da URI o mês e ano, incluíndo o parâmetro `subPrefeitura=LAPA`.

| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/00/0000` | ?subPrefeitura=LAPA | api-afericao.kdltelegestao.com.br/fdi-b/04/2022?subPrefeitura=LAPA |

#### Detalhamento de uma única peça
Para recuperar informações detalhadas de uma única peça comissionada para este indicador se faz necessário definir como parte da URI o mês, ano e a etiqueta de identificação da peça.

| Método | URI |  Exemplo      | 
| --- | --- |  :----------- | 
| GET | `/fdi-b/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/fdi-b/04/2022/IP00000A | 

#### Consulta de amostras
Para recuperar informações detalhadas de um conjunto de amostras de peças comissionadas para este indicador se faz necessário passar como body da consulta a lista de identificadores de etiqueta das peças a serem consultadas.

| Método | URI |  Exemplo      | 
| --- | --- |  :----------- | 
| POST | `/fdi-b/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/04/2022 | 

```javascript
var request = require('request');
var listaAmostras = ["IP00000A", "IP00000B"]; // <--Array-Json

request({
    url: "https://api-afericao.kdltelegestao.com.br/fdi-b/04/2022",
    headers: {
     'Authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c'
    },
    method: "POST",
    json: true,   // <--Muito Importante
    body: listaAmostras
}, function (error, response, body){
    console.log(response);
});
```

### Exemplo do Resultado:
``` json
{
    "data": {
        "tipo": "FDI-b",
        "consolidados": [
            {
                "etiqueta": "IP00000002A",
                "subPrefeitura": "LAPA",
                "comissionadoEm": "15/04/2022",
                "latLng": [0.000000, 0.000000],
                "consolidadoEm": "2022-05-31T14:51:21.728+00:00",
                "minutosNecessarios": 18000,
                "minutosLigado": 18000,
                "diasMonitorado": 31,
                "resultado": "100%",
                "status": 1,
                "dias": {
                    "01": {
                        "horas":{
                            "18":60,
                            "19":60
                        },
                        "historico": {
                            "18:15": {
                                "status": 1,
                                "luminosidade": 0.35
                            },
                            "18:25": {
                                "status": 1,
                                "luminosidade": 0
                            },
                            "19:10": {
                                "status": 1,
                                "luminosidade": 0
                            }
                        },
                        "minutosLigados": 600
                    }
                }
            },
        ],
        "resumo": {
            "ano": 2022,
            "mes": 5,
            "pontosComissionados": 3,
            "minutosNecessarios": 18600,
            "minutosLigado": 600,
            "diasMonitorado": 31,
            "resultado": "0.19%",
            "status": 1
        }
    },
    "total": 2,
    "offset": 0,
    "limit": 1000,
    "success": true,
    "elapsedTime": "123.18s"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Indentificador | Tipo | Descrição | 
| ------ | ---------| :------------------------------------------ | 
| data   | `object` | Resultado da consulta                       | 
| total  | `int`    | Quantidade de itens encontrados             | 
| limit  | `int`    | Quantidade de itens retornados por página   | 
| offset | `int`    | O número de documentos a serem ignorados no conjunto de resultados.  |
| success| `bool`   | Status de sucesso/falha da interação        | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| ------ | ---------| :------------------------------------------                  | 
| tipo   | `string` | Identifica o tipo de indicador consultado                    | 
| consolidados| `array<object>` | Lista de peças encontradas                       | 
| resumo | `object` | Resumo da query e resultados retornados                      | 

##### Bloco CONSOLIDADOS:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `string` | Latitude e Longitude                            | 
| consolidadoEm       | `string` | Data e Hora da consolidação dos dados           | 
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
| historico      | `object` | Registra o histórico diário de interações da peça    |
| minutosLigado  | `int`    | Soma de todos os minutos ligados no dia              |

##### Bloco HORAS:
| Indentificador | Tipo     | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| 0~23           | `int` | Quantidade de minutos ligado durante a noite            |

##### Bloco HISTORICO:
| Indentificador | Tipo     | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| status         | `float`  | 0 = Desligado, 1 = Ligado                            |
| luminosidade   | `float`  | Entre 0 e 1 - Sendo que, quanto mais próximo do valor 0 deve ser considerado escuro e mais próximo do valor 1 deve ser condiderado claro. |

##### Bloco RESUMO:
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
