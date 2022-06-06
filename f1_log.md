# Indicador F1 (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador F1.

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/f1/log/04/2022/IP00000A?hora=15',
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
Como parte da URI é necessário definir o dia, mês, ano e a etiqueta de identificação da peça.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f1/log/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/f1/log/05/2022/IP00001A |


### Exemplo do Resultado:
``` json
{
  "data": {
    "type": "F1 (LOG)",
    "result":{
      "transmissoesNecessarias": 4,
      "transmissoesRecebidas": 4,
      "diasMonitorado": 1,
      "resultado": "100%",
      "status": 0,
      "dias": {
        "01": {
          "horas": {
            "00": [
            "2022-05-31T15:46:47.458+00:00", 
            "2022-05-31T16:46:47.458+00:00"
            ],
            "01": [
            "2022-05-31T15:46:47.458+00:00", 
            "2022-05-31T16:46:47.458+00:00"
            ],
          },
          "transmissoesRecebidas": 60
        }
      }
    },
  },
  "total": 1,
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
| Identificador | Tipo | Descrição                              

##### Bloco RESULT:
| Identificador | Tipo | Descrição | 
| :------------------- | ------   | :------------------------------------------     | 
| transmissoesNecessarias | `int`    | Quantidade de transmissões necessárias      | 
| transmissoesRecebidas  | `int`    | Quantidade de transmissões recebidas         | 
| diasMonitorado      | `int`    | Quantidade de dias monitorado                   | 
| resultado           | `string` | Resultado do cálculo da fórmula em porcentagem  | 
| status              | `float`  | Resultado do cálculo da fórmula em decimal      | 
| dias                | `object`    | Envolve o bloco de dias monitorado              | 

##### Bloco DIAS:
| Identificador | Tipo | Descrição | 
| :-------------- | ---------| :------------------------------------------          | 
| 0~31           | `string` | Identificador do dia monitorado                      | 
| horas          | `object` | Identificador da hora monitorado                     |
| transmissoesRecebidas  | `int`    | Quantidade de transmissões recebidas no dia  |

##### Bloco HORAS:
| Identificador | Tipo     | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| 0~23           |  `array<string>`    | Data e hora do ping  |