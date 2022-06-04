# Indicador FDI-b (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador FDI-b.

Como parte da URI se faz necessário definir o mês e ano a ser consultado.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/fdi-b/log/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022 |

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022',
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
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| offset     | `int`  |  **0**    | O número de documentos a serem ignorados no conjunto de resultados.                                           |

#### Filtro por Subprefeitura
Para recuperar informações consolidadas das peças comissionadas para este indicador agrupadas por subprefeituras se faz necessário definir como parte da URI o mês e ano, incluíndo o parâmetro `subPrefeitura=LAPA`.

| Método | URI | Parâmetro | Exemplo      | 
| --- | --- | ----------| :----------- | 
| GET | `/fdi-b/log/00/0000` | ?subPrefeitura=LAPA | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022?subPrefeitura=LAPA |

#### Detalhamento de uma única peça
Para recuperar informações detalhadas de uma única peça comissionada para este indicador se faz necessário definir como parte da URI o mês, ano e a etiqueta de identificação da peça.

| Método | URI |  Exemplo      | 
| --- | --- |  :----------- | 
| GET | `/fdi-b/log/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022/IP00000A | 

#### Consulta de amostras
Para recuperar informações detalhadas de um conjunto de amostras de peças comissionadas para este indicador se faz necessário passar como body da consulta a lista de identificadores de etiqueta das peças a serem consultadas.

| Método | URI |  Exemplo      | 
| --- | --- |  :----------- | 
| POST | `/fdi-b/log/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022 | 

```javascript
var request = require('request');
var listaAmostras = ["IP00000A", "IP00000B"]; // <--Array-Json

request({
    url: "https://api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022",
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
    "type": "FDI-b/log",
    "result": [{
      "etiqueta": "IP00001",
      "subPrefeitura": "LAPA",
      "comissionadoEm": "15/04/2022",
      "latLng": [0.000000,0.000000],
      "registradoEm": "2022-05-31T15:51:21.728+00:00",
      "historico": {
        "status": 1,
        "luminosidade": 0,
      }
    }
  ],
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
| :------ | ---------| :-----------------------------------------                  | 
| data   | `object` | Resultado da consulta                                        | 
| total  | `int`    | Quantidade de itens encontrados                              | 
| limit  | `int`    | Quantidade de itens retornados por página                    | 
| offset | `int`    | O número de documentos a serem ignorados no conjunto de resultados.  |
| success| `bool`   | Status de sucesso/falha da interação                         | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| :------ | ---------| :------------------------------------------                 | 
| type   | `string` | Identifica o tipo de indicador consultado                    | 
| result| `array<object>` | Lista de peças encontradas                             | 

##### Bloco RESULT:
| Indentificador | Tipo | Descrição | 
| :------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `string` | Latitude e Longitude                            | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| historico           | `object` | Registra o histórico diário de interações da peça    |

##### Bloco HISTORICO:
| Indentificador | Tipo     | Descrição | 
| :-------------- | ---------| :------------------------------------------          | 
| status         | `float`  | 0 = Desligado, 1 = Ligado                            |
| luminosidade   | `float`  | Entre 0 e 1 - Sendo que, quanto mais próximo do valor 0 deve ser considerado escuro e mais próximo do valor 1 deve ser condiderado claro. |
