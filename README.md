# 📊 KDL API AFERIÇÃO

[![Version](https://img.shields.io/badge/version-5.0.0-blue.svg)](https://github.com/kdlti/kdl_api_afericao)
[![API Status](https://img.shields.io/badge/API-Active-green.svg)](https://simcidadesinteligentes.com.br:44300)
[![Auth](https://img.shields.io/badge/Auth-JWT_Bearer-orange.svg)]()

## 🚀 Visão Geral

A **API de Aferição KDL** é um sistema robusto de telegestão para monitoramento e aferição de indicadores de iluminação pública inteligente. Esta API REST fornece dados precisos para análise de desempenho, tempo de funcionamento e status operacional de luminárias conectadas.

### 🌐 URL Base
```
https://simcidadesinteligentes.com.br:44300
```

## 🔐 Autenticação

A API utiliza autenticação JWT (JSON Web Token) com Bearer Token. Todas as requisições aos endpoints protegidos devem incluir o token no cabeçalho de autorização.

👉 **[📖 Guia Completo de Autenticação](autenticacao.md)**

### 🔑 Endpoint de Login

**POST** `/login`

#### Payload de Requisição:
```json
{
  "username": "teste",
  "password": "teste"
}
```

#### Resposta de Sucesso:
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJiYW5rX2luZGV4IjoxLCJjb2R1c3IiOjEsImRhdGFiYXNlIjoiZGF0YWJhc2UxIiwiZXhwIjoxNzU1MTEyNjg3LCJpc19zdWJfdXNlciI6ZmFsc2UsInVzZXJuYW1lIjoidGVzdGUifQ.ifoZ0MiVuVbzdu925S8GJwAdTYjhifJtleTa7qDA9-A"
}
```

### 🛡️ Uso do Token

Inclua o token em todas as requisições no cabeçalho Authorization:

```bash
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

#### Exemplo com cURL:
```bash
curl -X GET "https://simcidadesinteligentes.com.br:44300/fdib/v7/01/01/2023" \
     -H "Authorization: Bearer SEU_TOKEN_AQUI" \
     -H "Content-Type: application/json"
```

## 📋 Endpoints Disponíveis

### 📊 Indicadores Principais

| Indicador | Descrição | Documentação |
|-----------|-----------|--------------|
| **FDI-b** | Funcionamento Diário Individual - Básico | [📖 Ver Documentação](fdi-b.md) |
| **A2** | Indicador de Aferição A2 | [📖 Ver Documentação](a2.md) |
| **F1** | Indicador de Funcionalidade F1 | [📖 Ver Documentação](f1.md) |
| **F2** | Indicador de Funcionalidade F2 | [📖 Ver Documentação](f2.md) |
| **F3** | Indicador de Funcionalidade F3 | [📖 Ver Documentação](f3.md) |
| **Pontos Comissionados** | Lista de pontos ativos | [📖 Ver Documentação](pontos-comissionados.md) |

## 💻 Exemplos Práticos

👉 **[🚀 Exemplos de Código e Implementações](exemplos.md)**

Exemplos completos em JavaScript, Python e HTML para:
- Autenticação e uso básico
- Geração de relatórios
- Monitoramento contínuo
- Dashboard em tempo real
- Tratamento de erros avançado

## 🚨 Importante

- ✅ **Autenticação Obrigatória**: Todos os endpoints requerem token de acesso válido
- 🕐 **Tokens Temporários**: Os tokens JWT têm validade limitada
- 🔒 **HTTPS**: Todas as comunicações são criptografadas via HTTPS
- 📊 **Rate Limiting**: A API pode ter limites de requisições por minuto
- 🧩 **Versionamento**: Use sempre a versão mais recente dos endpoints (v7)

## 🛠️ Status Codes

| Código | Descrição |
|--------|-----------|
| `200` | Sucesso |
| `401` | Não autorizado - Token inválido ou expirado |
| `403` | Acesso negado |
| `404` | Endpoint não encontrado |
| `429` | Muitas requisições - Rate limit excedido |
| `500` | Erro interno do servidor |

## 🆘 Suporte

Para suporte técnico ou dúvidas sobre a API:
- 📧 E-mail: suporte@kdltelegestao.com
- 📞 Telefone: +55 (11) 99999-9999
- 🌐 Website: [kdltelegestao.com](https://kdltelegestao.com)

## 📋 Documentação Adicional

- 🔐 **[Guia de Autenticação](autenticacao.md)** - Autenticação JWT detalhada
- 💻 **[Exemplos Práticos](exemplos.md)** - Código de exemplo em várias linguagens
- 🚀 **[Changelog](CHANGELOG.md)** - Guia de migração e mudanças na API

---

*Desenvolvido por **KDL Telegestão** - Iluminação Pública Inteligente*