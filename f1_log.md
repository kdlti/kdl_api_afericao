# Indicador F1 (LOG)
### KDL API REST para Aferição de Indicadores
######*versão 1.0.0*

Este endpoint é resposável pela entrega do log de registro da telegestão do indicador F1.

Como parte da URI se faz necessário definir o dia, mês, ano e a etiqueta de identificação da peça.

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f1/00/00/0000/IP00000A` | api-afericao.kdltelegestao.com.br/f1/10/05/2022/IP00001A |

##### Parâmetros opcionais:
| Indentificador | Tipo   | Default   |  Descrição                                                                        | 
| -------------- | -------| :--------:| :------------------------------------------------------------------------------   | 
| hora   | `string|int`  |  **00** | Se informado o parâmetro hora, será retornado somente os resultados encontrado no intervalo da hora do dia/mês/ano solicitado. Por default são retornas todos registro criados no dia/mês/ano solicitado. |
| resumo   | `bool`  |  **true** | Indica se deve ou não retornar os dados resumidos dos indicadores encontrados  |

| Método | URI | Exemplo                                                    | 
| --- | --- | :-----------                                               | 
| GET | `/f1/00/00/0000/IP00000A?hora=15` | api-afericao.kdltelegestao.com.br/f1/10/05/2022/IP00001A?hora=15 |

##### Autorização:
O token de autorização deve ser enviado no header da consulta.
```javascript
const request = require('request');

request({
  url: 'https://api-afericao.kdltelegestao.com.br/f1/08/04/2022/IP00000A?hora=15',
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
    "tipo": "F1",
    "log": ["2022-05-31T15:46:47.458+00:00"],
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
| :------ | ---------| :------------------------------------------                  | 
| data   | `object` | Resultado da consulta                                        | 
| total  | `int`    | Quantidade de itens encontrados                              | 
| success| `bool`   | Status de sucesso/falha da interação                         | 
| elapsedTime   | `string` | Tempo de duração da consulta                          | 

##### Bloco DATA:
| Indentificador | Tipo | Descrição                                                    | 
| :------ | ---------|  :------------------------------------------                | 
| tipo   | `string` | Identifica o tipo de indicador consultado                    | 
| log| `array<object>` | Lista de resultados - Data e hora do ping no servidor     | 
| resumo | `object` | Resumo da query e resultados retornados                      |  