# 🔐 Guia de Autenticação

## Visão Geral

A API KDL Aferição utiliza autenticação baseada em **JSON Web Token (JWT)** com padrão Bearer Token. Este sistema garante segurança e controle de acesso aos dados da telegestão.

## 🚪 Endpoint de Login

### **POST** `/login`

**URL Completa:** `https://simcidadesinteligentes.com.br:44300/login`

#### Parâmetros de Entrada

```json
{
  "username": "teste",
  "password": "teste"
}
```

#### Resposta de Sucesso (200 OK)

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJiYW5rX2luZGV4IjoxLCJjb2R1c3IiOjEsImRhdGFiYXNlIjoiZGF0YWJhc2UxIiwiZXhwIjoxNzU1MTEyNjg3LCJpc19zdWJfdXNlciI6ZmFsc2UsInVzZXJuYW1lIjoidGVzdGUifQ.ifoZ0MiVuVbzdu925S8GJwAdTYjhifJtleTa7qDA9-A"
}
```

#### Exemplos de Requisição

##### cURL
```bash
curl -X POST "https://simcidadesinteligentes.com.br:44300/login" \
     -H "Content-Type: application/json" \
     -d '{"username":"teste","password":"teste"}'
```

##### JavaScript (Fetch)
```javascript
const loginResponse = await fetch('https://simcidadesinteligentes.com.br:44300/login', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    username: 'teste',
    password: 'teste'
  })
});

const loginData = await loginResponse.json();
const token = loginData.token;
```

##### Python (requests)
```python
import requests

login_url = "https://simcidadesinteligentes.com.br:44300/login"
login_data = {
    "username": "teste",
    "password": "teste"
}

response = requests.post(login_url, json=login_data)
token = response.json()["token"]
```

## 🛡️ Usando o Token de Acesso

Após obter o token, inclua-o no cabeçalho `Authorization` com o prefixo `Bearer`:

```
Authorization: Bearer SEU_TOKEN_AQUI
```

### Exemplos de Uso

##### cURL
```bash
curl -X GET "https://simcidadesinteligentes.com.br:44300/fdib/v5/01/01/2023" \
     -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
     -H "Content-Type: application/json"
```

##### JavaScript (Fetch)
```javascript
const apiResponse = await fetch('https://simcidadesinteligentes.com.br:44300/fdib/v5/01/01/2023', {
  method: 'GET',
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
});

const data = await apiResponse.json();
```

##### Python (requests)
```python
api_url = "https://simcidadesinteligentes.com.br:44300/fdib/v5/01/01/2023"
headers = {
    "Authorization": f"Bearer {token}",
    "Content-Type": "application/json"
}

response = requests.get(api_url, headers=headers)
data = response.json()
```

## ⚠️ Códigos de Erro de Autenticação

| Código | Descrição | Solução |
|--------|-----------|---------|
| **401** | Token inválido ou expirado | Refaça o login para obter novo token |
| **403** | Acesso negado | Verifique permissões do usuário |
| **422** | Credenciais inválidas no login | Verifique username e password |

## 🕐 Expiração do Token

- Os tokens JWT têm **validade limitada**
- Monitor o código de status `401` nas respostas
- Implemente renovação automática do token quando necessário
- O campo `exp` no payload do JWT indica o timestamp de expiração

## 🔄 Renovação Automática

Exemplo de implementação de renovação automática em JavaScript:

```javascript
class KDLApiClient {
  constructor() {
    this.token = null;
    this.baseUrl = 'https://simcidadesinteligentes.com.br:44300';
  }

  async login() {
    const response = await fetch(`${this.baseUrl}/login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username: 'teste', password: 'teste' })
    });
    
    const data = await response.json();
    this.token = data.token;
    return this.token;
  }

  async makeRequest(endpoint) {
    if (!this.token) {
      await this.login();
    }

    let response = await fetch(`${this.baseUrl}${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${this.token}`,
        'Content-Type': 'application/json'
      }
    });

    // Se token expirou, renova automaticamente
    if (response.status === 401) {
      await this.login();
      response = await fetch(`${this.baseUrl}${endpoint}`, {
        headers: {
          'Authorization': `Bearer ${this.token}`,
          'Content-Type': 'application/json'
        }
      });
    }

    return response.json();
  }
}

// Uso
const client = new KDLApiClient();
const data = await client.makeRequest('/fdib/v5/01/01/2023');
```

## 🔒 Boas Práticas de Segurança

1. **Nunca exponha tokens** em logs, URLs ou código frontend
2. **Armazene tokens com segurança** (variáveis de ambiente, vaults)
3. **Use HTTPS** sempre para comunicação
4. **Implemente timeout** nas requisições
5. **Monitor tentativas de login** para detectar ataques
6. **Renove tokens** regularmente mesmo antes da expiração

---

*Para mais informações, consulte a [documentação principal](README.md)*
