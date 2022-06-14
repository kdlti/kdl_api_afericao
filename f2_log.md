# Indicador F2 (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador F2.

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'http://api-afericao.kdltelegestao.com/f2/log/04/2022',
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

| Método |  Parâmetros | URI | Exemplo                                                    | 
| --- | :---: | --- |:-----------                         | 
| GET |SIM| `/f2/log/00/0000` | api-afericao.kdltelegestao.com/f2/log/04/2022 |

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
    "type": "F2 (LOG)",
    "result": [
      {
        "etiqueta": "IP00000002A",
        "subPrefeitura": "LAPA",
        "comissionadoEm": "15/04/2022",
        "latLng": [0.000000, 0.000000],
        "registradoEm": "2022-05-31T14:51:21.728+00:00",
        "ocorrencia": {
          "tipo": "Sem comunicação",
          "detectadoEm": "2022-05-07T11:10:17.000+00:00",
          "informadoEm": "2022-05-07T11:14:27.000+00:00",
          "tempo": "00:04:10",
          "status": 0
        },
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
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `array<float>` | Latitude e Longitude                            | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| ocorrencia          | `object` | Registra o histórico diário de interações da peça    |


##### Bloco OCORRENCIA:
| Identificador | Tipo     | Descrição | 
| :-------------- | ---------| :------------------------------------------          | 
| tipo           | `string` | Identificador do tipo de falha                       |
| detectadoEm    | `string`   | Momento em que o concentrador registra que uma falha foi observada|
| informadoEm    | `string`   | Momento em que a telegestão torna disponível a informação para utilização pelo CCO|
| tempo    | `string`   | Duração entre o tempo decorrido do momento em que o concentrador observou a falha e o momento em que a informação foi disponibilizada para uso do CCO|
| status           | `int` | Status indica se a falha está sendo informada dentro do tempo limite necessário (0 = Não está em conformidade / 1 = Está em conformidade)|