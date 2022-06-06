# Indicador FDI-b (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador FDI-b.

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

Como parte da URI é necessário definir o mês e ano a ser consultado.

| Método | Parâmetros | URI | Exemplo                                                    | 
| --- | :---: | --- |:-----------                                               | 
| GET | SIM |`/fdi-b/log/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022 |

##### Parâmetros opcionais:
| Identificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| limit          | `int`  |  **1000** | Quantidade de itens retornados na página de resultado                             |
| offset     | `int`  |  **0**    | O número de documentos a serem ignorados no conjunto de resultados.                                           |
| subPrefeitura     | `string`  |  -    | Identificador da subPrefeitura |

| Exemplo de uso dos parâmetros       | 
| :----------- | 
| ?limit=200&offset=500&subPrefeitura=LAPA |

#### Consulta de amostras
Para recuperar informações detalhadas de um conjunto de amostras de peças comissionadas para este indicador é necessário passar como body da consulta a lista de identificadores de etiqueta das peças a serem consultadas.

| Método | Parâmetros |  URI |  Exemplo      | 
| --- | :---: | --- |  :----------- | 
| POST | SIM |`/fdi-b/log/00/0000` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022 | 

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

#### Detalhamento de uma única peça
Para recuperar informações detalhadas de uma única peça comissionada para este indicador é necessário definir como parte da URI o mês, ano e a etiqueta de identificação da peça.

| Método |  Parâmetros | URI |  Exemplo      | 
| --- | :---: | --- | :----------- | 
| GET | NÃO |`/fdi-b/log/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/fdi-b/log/04/2022/IP00000A | 

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
    }],
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
| :------------------- | ------   | :------------------------------------------    | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `array<float>` | Latitude e Longitude                      | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| historico           | `object` | Registra o histórico diário de interações da peça    |

##### Bloco HISTORICO:
| Identificador | Tipo     | Descrição | 
| :-------------- | ---------| :------------------------------------------         | 
| status         | `int`  | 0 = Desligado, 1 = Ligado                              |
| luminosidade   | `float`  | Entre 0 e 1 - Sendo que, quanto mais próximo do valor 0 deve ser considerado escuro e mais próximo do valor 1 deve ser condiderado claro. |
