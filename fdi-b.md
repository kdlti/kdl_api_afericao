# Indicador FDI-b
### KDL API REST para Aferição de Indicadores
*versão 5.0.0*

🔐 **ATENÇÃO**: Este endpoint requer autenticação JWT. Inclua o token Bearer no cabeçalho Authorization.

Este endpoint é resposável pela entrega de informações da telegestão do indicador FDI-b.

Como parte da URI é necessário definir o dia/mês/ano a ser consultado.

## 🌐 URL Base
```
https://simcidadesinteligentes.com.br:44300
```

| Método | URI                            | Exemplo                                                  | 
| --- |--------------------------------|:---------------------------------------------------------| 
| GET | `/fdib/v7/00/00/0000`          | https://simcidadesinteligentes.com.br:44300/fdib/v7/01/01/2023       |
| GET | `/fdib/v7/etiqueta`            | https://simcidadesinteligentes.com.br:44300/fdib/v7/IP0322471            |
| GET | `/fdib/v7/etiqueta/00/00/0000` | https://simcidadesinteligentes.com.br:44300/fdib/v7/IP0322471/01/01/2023|

## 🔒 Autenticação

Todas as requisições devem incluir o token Bearer no cabeçalho:

```bash
Authorization: Bearer SEU_TOKEN_JWT
```

### Exemplo com cURL:
```bash
curl -X GET "https://simcidadesinteligentes.com.br:44300/fdib/v7/01/01/2023" \
     -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ..." \
     -H "Content-Type: application/json"
```

##### Parâmetros opcionais:
| Identificador | Tipo     |     Default     | Descrição                                                                                                                                                                | 
|---------------|----------|:---------------:|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
| limit         | `int`    |    **1000**     | Quantidade de itens retornados.                                                                                                                                          |
| offset        | `int`    |      **0**      | Paginação do resultado.                                                                                                                                                  |
| count         | `string` | **true/false** | Indica se deve retornar a contagem de documentos na mesma consulta. O tempo para retornar a contagem de documento pode ser 3 vezes MENOR sem a utilização desse recurso. |

| Exemplo de uso dos parâmetros    | 
|:---------------------------------| 
| ?limit=200&offset=400&count=true |

### Exemplo do Resultado:
``` json
{
    "data": {
        "type": "FDI-b",
        "result": [
            {
                "id": "58747476",
                "coletaRede": "2022-08-01T00:00:00Z",
                "coletaPeca": "2022-08-01T00:00:00Z",
                "comissionadoEm": "2022-07-01T00:00:00Z",
                "etiqueta": "IP0322471",
                "latitude": "-23.52501",
                "longitude": "-46.71013",
                "subPrefeitura": "LAPA",
                "lux": 0,
                "tempoLigadoParcial": 0,
                "tempoLigadoDecorrido": "00:00:00",
                "coletaPecaDecorrido": "00:00:00",
                "horaAcendeu": "00:00:00",
                "horaApagou": "",
                "status": 1
            }
        ]
    },
    "total": 100,
    "totalRetornado": 8,
    "offset": 0,
    "limit": 10,
    "success": true,
    "elapsedTime": "157.784032ms"
}
```
### Dicionário do Resultado:
##### Bloco PRINCIPAL:
| Identificador  | Tipo     | Descrição                                                           | 
|:---------------|----------|:--------------------------------------------------------------------| 
| data           | `object` | Resultado da consulta                                               | 
| total          | `int`    | Quantidade de itens encontrados                                     | 
| totalRetornado | `int64`  | Quantidade de itens retornados                                      |
| limit          | `int`    | Quantidade de itens retornados por página                           | 
| offset         | `int`    | O número de documentos a serem ignorados no conjunto de resultados. |
| success        | `bool`   | Status de sucesso/falha da interação                                | 
| elapsedTime    | `string` | Tempo de duração da consulta                                        | 

##### Bloco DATA:
| Identificador | Tipo            | Descrição                                             | 
|:--------------|-----------------|:------------------------------------------------------| 
| type          | `string`        | Identifica o tipo de indicador consultado             | 
| result        | `array<object>` | Lista de peças encontradas                            | 

##### Bloco RESULT:
| Identificador         | Tipo     | Descrição                                                       | 
|:----------------------|----------|:----------------------------------------------------------------| 
| id                    | `string` | Identificador do documento                                      |
| coletaRede            | `string` | Horário da leitura da rede                                      | 
| coletaPeca            | `string` | Horário da leitura da peça                                      |
| etiqueta              | `string` | Identificador universal da luminária                            |
| subPrefeitura         | `string` | Identificador da SubPrefeitura                                  | 
| comissionadoEm        | `string` | Dia do comissionamento da peça                                  | 
| latitude              | `string` | Latitude                                                        |
| longitude             | `string` | Longitude                                                       |
| lux                   | `string` | Luminosidade do ambiente 0~1 = 0:escuro/1:claro                 | 
| tempoLigadoParcial    | `string`    | Tempo ligado parcial (contador inicia 00:00:00hs)               | 
| tempoLigadoDecorrido  | `string` | Tempo ligado decorrido a partir das 00:00:00hs                  | 
| coletaPecaDecorrido   | `string` | Tempo decorrido entre as leituras da peça                       | 
| horaAcendeu           | `string` | Momento que a luminária acendeu dentro do intervalo da leitura  | 
| horaApagou            | `string` | Momento que a luminária apagou dentro do intervalo da leitura   | 
| status                | `bool`   | 1 = Ligado / 0 = Desligado                                      |

***Observação Importante***: 
Os campos 'coletaRede / coletaPeca / horaAcendeu e horaApagou' não tem precisão suficiente para ser 
utilizado em base de cálculo do tempo ligado da peça, essas informações servem somente para 
registros de leituras e eventos ocorridos durante os intervalos de leituras realizadas em cada peça.


Somente os campos ‘tempoLigadoParcial / tempoLigadoDecorrido‘ podem ser utilizados para definir o tempo total ligado da luminária
durante o dia, pois nele já estão sendo considerados:
1) Somente o tempo real que a peça ficou ligada.
2) Se a luminária está acesa durante o dia mas o lux está dentro do valor permitido (tempo nublado, embaixo de viadutos e outros)
3) Se a luminária está somente sem comunicação porém continua funcionando normalmente, está é registrada
como luminária apagada(sem registro no F2), e nenhum evento de `horaApagou` é registrado por falta de comunicação com a peça.

Os valores de ‘tempoLigadoParcial / tempoLigadoDecorrido‘ são registrados pela peça a partir da 00:00:00hs de cada dia, 
independente das leituras realizadas, dessa forma sendo confiáveis o bastante para serem utilizados em cálculos do tempo 
ligado da peça.