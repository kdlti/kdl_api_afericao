# Indicador A2 (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador A2.

Como parte da URI se faz necessário definir o dia, mês, ano e a etiqueta de identificação da peça.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/a2/00/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/a2/10/05/2022/IP00001A |

##### Parâmetros opcionais:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| hora   | `string|int`  |  **00** | Se informado o parâmetro hora, será retornado somente os resultados encontrado no intervalo da hora do dia/mês/ano solicitado. Por default são retornas todos registro criados no dia/mês/ano solicitado. |
| resumo   | `bool`  |  **true** | Indica se deve ou não retornar os dados resumidos dos indicadores encontrados  |

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/a2/00/00/0000/IP00000A?resumo=false&hora=15` | api-afericao.kdltelegestao.com.br/a2/10/05/2022/IP00001A?resumo=false&hora=15 |

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/a2/08/04/2022/IP00000A?hora=15',
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
### Exemplo do Resultado:
``` json
{
  "data": {
    "tipo": "A2",
    "log": [{
      "etiqueta": "IP00001",
      "subPrefeitura": "LAPA",
      "comissionadoEm": "15/04/2022",
      "latLng": [0.000000,0.000000],
      "registradoEm": "2022-05-31T15:51:21.728+00:00",
      "historico": {
        "status": 1,
        "luminosidade": 0
      }
    }
  ],
  "resumo": {
      "ano": 2022,
      "mes": 5,
      "dia": 31,
      "hora": 15
    }
  },
  "total": 1,
  "success": true,
  "elapsedTime": "123.18s"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Indentificador | Tipo | Descrição | 
| ------ | ---------| :------------------------------------------                  | 
| data   | `object` | Resultado da consulta                                        | 
| total  | `int`    | Quantidade de itens encontrados                              | 
| success| `bool`   | Status de sucesso/falha da interação                         | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| ------ | ---------| :------------------------------------------                  | 
| tipo   | `string` | Identifica o tipo de indicador consultado                    | 
| log| `array<object>` | Lista de resultados                                       | 
| resumo | `object` | Resumo da query e resultados retornados                      |  

##### Bloco LOG:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `string` | Latitude e Longitude                            | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| historico           | `object` | Registra o histórico diário de interações da peça    |

##### Bloco HISTORICO:
| Indentificador | Tipo     | Descrição | 
| -------------- | ---------| :------------------------------------------          | 
| status         | `float`  | 0 = Desligado, 1 = Ligado                            |
| luminosidade   | `float`  | Entre 0 e 1 - Sendo que, quanto mais próximo do valor 0 deve ser considerado escuro e mais próximo do valor 1 deve ser condiderado claro. |

##### Bloco RESUMO:
| Indentificador | Tipo | Descrição | 
| :------------------- | ------| :------------------------------------------       | 
| ano                 | `string|int`| Ano                                          | 
| mes                 | `string|int`| Mês                                          | 
| dia                 | `string|int`| Dia                                          | 
| hora                | `string|int`| Hora                                         | 
