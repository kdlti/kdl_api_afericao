# Indicador F2 (LOG)
### KDL API REST para Aferição de Indicadores
*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador F2.

Como parte da URI é necessário definir o dia, mês, ano e a etiqueta de identificação da peça.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f2/00/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/f2/10/05/2022/IP00001A |

##### Parâmetros opcionais:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| hora   | `string|int`  |  **00** | Se informado o parâmetro hora, será retornado somente os resultados encontrado no intervalo da hora do dia/mês/ano solicitado. Por default são retornas todos registro criados no dia/mês/ano solicitado. |
| resumo   | `bool`  |  **true** | Indica se deve ou não retornar os dados resumidos dos indicadores encontrados  |

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f2/00/00/0000/IP00000A?resumo=false&hora=15` | api-afericao.kdltelegestao.com.br/f2/10/05/2022/IP00001A?resumo=false&hora=15 |

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/f2/08/04/2022/IP00000A?hora=15',
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
    "tipo": "F2",
    "log": [
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
          "tempoDecorrido": "00:04:10",
          "status": 0
        },
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
| :------ | ---------| :------------------------------------------                 | 
| data   | `object` | Resultado da consulta                                        | 
| total  | `int`    | Quantidade de itens encontrados                              | 
| success| `bool`   | Status de sucesso/falha da interação                         | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                | 
| :------ | ---------|  :------------------------------------------                | 
| tipo    | `string` | Identifica o tipo de indicador consultado                   | 
| log     | `array<date>` | Lista de resultados                                    | 
| resumo  | `object` | Resumo da query e resultados retornados                     |  

##### Bloco LOG:
| Indentificador | Tipo | Descrição | 
| ------------------- | ------   | :------------------------------------------     | 
| etiqueta            | `string` | Identificador universal da luminária            | 
| subPrefeitura       | `string` | Identificador da SubPrefeitura                  | 
| comissionadoEm      | `string` | Dia do comissionamento da peça                  | 
| latLng              | `array<float>` | Latitude e Longitude                            | 
| registradoEm        | `string` | Data e Hora do registro dos dados               | 
| ocorrencia          | `object` | Registra o histórico diário de interações da peça    |


##### Bloco OCORRENCIA:
| Indentificador | Tipo     | Descrição | 
| :-------------- | ---------| :------------------------------------------          | 
| tipo           | `string` | Identificador do tipo de falha                       |
| detectadoEm    | `date`   | Momento em que o concentrador registra que uma falha foi observada|
| informadoEm    | `date`   | Momento em que a telegestão torna disponível a informação para utilização pelo CCO|
| tempoDecorrido    | `string`   | Duração entre o tempo decorrido do momento em que o concentrador observou a falha e o momento em que a informação foi disponibilizada para uso do CCO|
| status           | `bool` | Status indica se a falha está sendo informada dentro do tempo limite necessário (0 = Não está em conformidade / 1 = Está em conformidade)|