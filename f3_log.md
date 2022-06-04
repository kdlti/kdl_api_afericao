# Indicador F3 (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador F3.
##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/a2/log/04/2022',
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

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f3/log/0/0000` | api-afericao.kdltelegestao.com.br/f3/log/4/2022 |

##### Parâmetros opcionais:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
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
    "type": "F3 (LOG)",
    "result": [
      {
        "etiqueta": "IP00000002A",
        "subPrefeitura": "LAPA",
        "comissionadoEm": "15/04/2022",
        "latLng": [0.000000, 0.000000],
        "conectadoEm": "2022-05-07T11:10:17.000+00:00",
      }
    ],
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
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `array<float>` | Latitude e Longitude                            | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| conectadoEm         | `string` | Data e Hora em que a peça foi conectada           | 