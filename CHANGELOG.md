# 🚀 CHANGELOG - Migração para Nova API

## 📝 Resumo das Mudanças

Esta documentação foi atualizada para refletir as mudanças na API KDL Aferição. Aqui está um resumo completo das alterações:

## 🌐 Nova URL Base

### ❌ Antes:
```
api-afericao.kdltelegestao.com
```

### ✅ Agora:
```
https://simcidadesinteligentes.com.br:44300
```

## 🔐 Nova Autenticação JWT

### 🆕 Endpoint de Login
- **URL**: `https://simcidadesinteligentes.com.br:44300/login`
- **Método**: POST
- **Payload**:
```json
{
  "username": "teste",
  "password": "teste"
}
```

### 🆕 Token Bearer Obrigatório
Todos os endpoints agora requerem autenticação:
```bash
Authorization: Bearer SEU_TOKEN_JWT
```

## 📋 URLs Atualizadas por Endpoint

### FDI-b
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Por data | `api-afericao.kdltelegestao.com/fdib/v5/01/01/2023` | `https://simcidadesinteligentes.com.br:44300/fdib/v5/01/01/2023` |
| Por etiqueta | `api-afericao.kdltelegestao.com/fdib/v5/IP0322471` | `https://simcidadesinteligentes.com.br:44300/fdib/v5/IP0322471` |

### A2
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Por data | `api-afericao.kdltelegestao.com/a2/v5/01/01/2023` | `https://simcidadesinteligentes.com.br:44300/a2/v5/01/01/2023` |
| Por etiqueta | `api-afericao.kdltelegestao.com/a2/v5/IP0322471` | `https://simcidadesinteligentes.com.br:44300/a2/v5/IP0322471` |
| Changes | `api-afericao.kdltelegestao.com/changes/a2/v5/01/01/2023` | `https://simcidadesinteligentes.com.br:44300/changes/a2/v5/01/01/2023` |

### F1
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Lista | `http://api-afericao.kdltelegestao.com/f1/v5?limit=1000&offset=0` | `https://simcidadesinteligentes.com.br:44300/f1/v5?limit=1000&offset=0` |

### F2
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Por data | `api-afericao.kdltelegestao.com/f2/v5/01/01/2023` | `https://simcidadesinteligentes.com.br:44300/f2/v5/01/01/2023` |
| Por etiqueta | `api-afericao.kdltelegestao.com/f2/v5/IP0322471` | `https://simcidadesinteligentes.com.br:44300/f2/v5/IP0322471` |

### F3
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Por data | `api-afericao.kdltelegestao.com/f3/v5/01/01/2023` | `https://simcidadesinteligentes.com.br:44300/f3/v5/01/01/2023` |
| Por etiqueta | `api-afericao.kdltelegestao.com/f3/v5/IP0322471` | `https://simcidadesinteligentes.com.br:44300/f3/v5/IP0322471` |

### Pontos Comissionados
| Endpoint | URL Anterior | URL Nova |
|----------|-------------|----------|
| Lista | `http://api-afericao.kdltelegestao.com/pontos-comissionados?limit=1000&offset=0` | `https://simcidadesinteligentes.com.br:44300/pontos-comissionados?limit=1000&offset=0` |

## 🛠️ Guia de Migração Passo a Passo

### 1. ⚠️ O que DEVE ser alterado:

1. **URL Base**: Altere todas as URLs de `api-afericao.kdltelegestao.com` para `https://simcidadesinteligentes.com.br:44300`

2. **Adicione Autenticação**: 
   - Implemente chamada para `/login` antes das requisições
   - Adicione header `Authorization: Bearer TOKEN` em todas as requisições

3. **Protocolo HTTPS**: Certifique-se de usar HTTPS (já incluído na nova URL)

### 2. ✅ O que NÃO muda:

1. **Estrutura dos dados**: Os JSONs de resposta continuam os mesmos
2. **Parâmetros**: `limit`, `offset`, `count` funcionam igual
3. **Versionamento**: Continua sendo v5
4. **Códigos de status**: 200, 404, etc. funcionam igual

### 3. 🔄 Exemplo de Migração

#### ❌ Código Antigo:
```javascript
const response = await fetch('api-afericao.kdltelegestao.com/fdib/v5/01/01/2023');
const data = await response.json();
```

#### ✅ Código Novo:
```javascript
// 1. Fazer login primeiro
const loginResponse = await fetch('https://simcidadesinteligentes.com.br:44300/login', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ username: 'teste', password: 'teste' })
});
const loginData = await loginResponse.json();
const token = loginData.token;

// 2. Usar token nas requisições
const response = await fetch('https://simcidadesinteligentes.com.br:44300/fdib/v5/01/01/2023', {
  headers: {
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  }
});
const data = await response.json();
```

## 🚨 Pontos de Atenção

### 1. Tokens JWT têm Validade
- Monitor códigos de status 401 (Unauthorized)
- Implemente renovação automática de token
- Armazene tokens de forma segura

### 2. Rate Limiting
- A nova API pode ter limites de requisições
- Implemente backoff exponencial para erro 429

### 3. HTTPS Obrigatório
- Todas as comunicações devem usar HTTPS
- Verifique certificados SSL

### 4. Gestão de Erros
- Trate erro 401 (token expirado)
- Trate erro 403 (acesso negado)
- Trate erro 429 (rate limit)

## 📋 Checklist de Migração

### Antes do Deploy:
- [ ] Atualizar todas as URLs base
- [ ] Implementar autenticação JWT
- [ ] Adicionar headers Authorization em todas as requisições
- [ ] Implementar renovação automática de token
- [ ] Testar todos os endpoints
- [ ] Implementar tratamento de erro 401/403
- [ ] Verificar logs de erro
- [ ] Documentar novas credenciais

### Após o Deploy:
- [ ] Monitor logs de aplicação
- [ ] Verificar tempo de resposta
- [ ] Validar funcionalidades críticas
- [ ] Confirmar autenticação funcionando
- [ ] Testar renovação de token
- [ ] Verificar rate limiting

## 📞 Suporte

Se encontrar problemas durante a migração:

- 📧 **E-mail**: suporte@kdltelegestao.com
- 📞 **Telefone**: +55 (11) 99999-9999
- 📖 **Documentação**: [README.md](README.md)
- 💻 **Exemplos**: [exemplos.md](exemplos.md)
- 🔐 **Autenticação**: [autenticacao.md](autenticacao.md)

## 📅 Cronograma Sugerido

### Semana 1: Preparação
- Revisar código existente
- Identificar todos os pontos que fazem chamadas à API
- Preparar ambiente de teste

### Semana 2: Implementação
- Implementar autenticação JWT
- Atualizar URLs
- Implementar tratamento de erros

### Semana 3: Testes
- Testar todos os fluxos
- Validar performance
- Preparar rollback se necessário

### Semana 4: Deploy
- Deploy em produção
- Monitor logs
- Suporte intensivo

---

*Migração concluída com sucesso! 🎉*
