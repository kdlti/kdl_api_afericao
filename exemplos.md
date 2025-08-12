# 💡 Exemplos Práticos de Uso

Este arquivo contém exemplos práticos de como usar a API KDL Aferição em diferentes linguagens e cenários.

## 🌐 Configuração Base

```
URL Base: https://simcidadesinteligentes.com.br:44300
Autenticação: JWT Bearer Token
```

## 🔐 1. Autenticação e Primeiro Request

### JavaScript/Node.js

```javascript
const API_BASE = 'https://simcidadesinteligentes.com.br:44300';

// Função para fazer login
async function login() {
  const response = await fetch(`${API_BASE}/login`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      username: 'teste',
      password: 'teste'
    })
  });
  
  if (!response.ok) {
    throw new Error('Erro no login');
  }
  
  const data = await response.json();
  return data.token;
}

// Função para fazer requisições autenticadas
async function apiRequest(endpoint, token) {
  const response = await fetch(`${API_BASE}${endpoint}`, {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    }
  });
  
  if (!response.ok) {
    throw new Error(`Erro na API: ${response.status}`);
  }
  
  return response.json();
}

// Exemplo de uso
async function exemploCompleto() {
  try {
    // 1. Fazer login
    const token = await login();
    console.log('✅ Login realizado com sucesso');
    
    // 2. Buscar dados FDI-b do dia 01/01/2023
    const fdibData = await apiRequest('/fdib/v5/01/01/2023', token);
    console.log(`📊 FDI-b: ${fdibData.total} registros encontrados`);
    
    // 3. Buscar uma etiqueta específica
    const etiquetaData = await apiRequest('/fdib/v5/IP0322471', token);
    console.log(`🏷️ Dados da etiqueta IP0322471:`, etiquetaData.data.result[0]);
    
  } catch (error) {
    console.error('❌ Erro:', error.message);
  }
}

// Executar exemplo
exemploCompleto();
```

### Python

```python
import requests
import json
from datetime import datetime

class KDLApi:
    def __init__(self):
        self.base_url = "https://simcidadesinteligentes.com.br:44300"
        self.token = None
    
    def login(self):
        """Realiza login e obtém token"""
        url = f"{self.base_url}/login"
        data = {
            "username": "teste",
            "password": "teste"
        }
        
        response = requests.post(url, json=data)
        response.raise_for_status()
        
        self.token = response.json()["token"]
        print("✅ Login realizado com sucesso")
        return self.token
    
    def get_headers(self):
        """Retorna headers com token de autorização"""
        if not self.token:
            self.login()
        
        return {
            "Authorization": f"Bearer {self.token}",
            "Content-Type": "application/json"
        }
    
    def get_fdib(self, dia, mes, ano, limit=1000, offset=0):
        """Busca dados FDI-b para uma data específica"""
        url = f"{self.base_url}/fdib/v5/{dia:02d}/{mes:02d}/{ano}"
        params = {"limit": limit, "offset": offset}
        
        response = requests.get(url, headers=self.get_headers(), params=params)
        response.raise_for_status()
        
        return response.json()
    
    def get_etiqueta_fdib(self, etiqueta, dia=None, mes=None, ano=None):
        """Busca dados de uma etiqueta específica"""
        if dia and mes and ano:
            url = f"{self.base_url}/fdib/v5/{etiqueta}/{dia:02d}/{mes:02d}/{ano}"
        else:
            url = f"{self.base_url}/fdib/v5/{etiqueta}"
        
        response = requests.get(url, headers=self.get_headers())
        response.raise_for_status()
        
        return response.json()
    
    def get_pontos_comissionados(self, limit=1000, offset=0):
        """Busca pontos comissionados"""
        url = f"{self.base_url}/pontos-comissionados"
        params = {"limit": limit, "offset": offset}
        
        response = requests.get(url, headers=self.get_headers(), params=params)
        response.raise_for_status()
        
        return response.json()

# Exemplo de uso
if __name__ == "__main__":
    # Inicializar API
    api = KDLApi()
    
    try:
        # Fazer login
        api.login()
        
        # Buscar dados FDI-b do dia 01/01/2023
        fdib_data = api.get_fdib(1, 1, 2023, limit=10)
        print(f"📊 FDI-b: {fdib_data['total']} registros encontrados")
        print(f"⏱️ Tempo de consulta: {fdib_data['elapsedTime']}")
        
        # Buscar etiqueta específica
        etiqueta_data = api.get_etiqueta_fdib("IP0322471")
        if etiqueta_data['data']['result']:
            peca = etiqueta_data['data']['result'][0]
            print(f"🏷️ Etiqueta {peca['etiqueta']}:")
            print(f"   📍 Localização: {peca['subPrefeitura']}")
            print(f"   🔛 Status: {'Ligado' if peca['status'] else 'Desligado'}")
        
        # Buscar pontos comissionados
        pontos = api.get_pontos_comissionados(limit=5)
        print(f"📍 {pontos['total']} pontos comissionados encontrados")
        
    except requests.exceptions.RequestException as e:
        print(f"❌ Erro na requisição: {e}")
    except Exception as e:
        print(f"❌ Erro: {e}")
```

## 📊 2. Análises e Relatórios

### Exemplo: Relatório de Status das Luminárias

```javascript
class RelatorioLuminariaes {
  constructor(apiToken) {
    this.token = apiToken;
    this.baseUrl = 'https://simcidadesinteligentes.com.br:44300';
  }

  async gerarRelatorioStatus(dia, mes, ano) {
    const endpoint = `/fdib/v5/${String(dia).padStart(2, '0')}/${String(mes).padStart(2, '0')}/${ano}`;
    
    const response = await fetch(`${this.baseUrl}${endpoint}?count=true`, {
      headers: {
        'Authorization': `Bearer ${this.token}`,
        'Content-Type': 'application/json'
      }
    });
    
    const data = await response.json();
    
    // Análise dos dados
    const ligadas = data.data.result.filter(item => item.status === 1).length;
    const desligadas = data.data.result.filter(item => item.status === 0).length;
    const total = data.data.result.length;
    
    const relatorio = {
      data: `${dia}/${mes}/${ano}`,
      total_luminarias: total,
      ligadas: ligadas,
      desligadas: desligadas,
      percentual_funcionamento: total > 0 ? ((ligadas / total) * 100).toFixed(2) : 0,
      tempo_consulta: data.elapsedTime,
      subprefeituras: this.analisarPorSubprefeitura(data.data.result)
    };
    
    return relatorio;
  }
  
  analisarPorSubprefeitura(dados) {
    const grupos = {};
    
    dados.forEach(item => {
      if (!grupos[item.subPrefeitura]) {
        grupos[item.subPrefeitura] = { total: 0, ligadas: 0, desligadas: 0 };
      }
      
      grupos[item.subPrefeitura].total++;
      if (item.status === 1) {
        grupos[item.subPrefeitura].ligadas++;
      } else {
        grupos[item.subPrefeitura].desligadas++;
      }
    });
    
    // Calcular percentuais
    Object.keys(grupos).forEach(sub => {
      const grupo = grupos[sub];
      grupo.percentual = grupo.total > 0 ? 
        ((grupo.ligadas / grupo.total) * 100).toFixed(2) : 0;
    });
    
    return grupos;
  }
}

// Uso do relatório
async function exemploRelatorio() {
  const token = await login(); // função de login do exemplo anterior
  const relatorio = new RelatorioLuminariaes(token);
  
  const resultado = await relatorio.gerarRelatorioStatus(1, 1, 2023);
  
  console.log('📈 RELATÓRIO DE STATUS DAS LUMINÁRIAS');
  console.log('=====================================');
  console.log(`📅 Data: ${resultado.data}`);
  console.log(`📊 Total de Luminárias: ${resultado.total_luminarias}`);
  console.log(`🟢 Ligadas: ${resultado.ligadas} (${resultado.percentual_funcionamento}%)`);
  console.log(`🔴 Desligadas: ${resultado.desligadas}`);
  console.log(`⏱️ Tempo de Consulta: ${resultado.tempo_consulta}`);
  console.log('\n🏢 POR SUBPREFEITURA:');
  
  Object.entries(resultado.subprefeituras).forEach(([nome, dados]) => {
    console.log(`  ${nome}: ${dados.ligadas}/${dados.total} (${dados.percentual}%)`);
  });
}
```

## 🔄 3. Monitoramento Contínuo

### Script de Monitoramento (Python)

```python
import time
import schedule
from datetime import datetime, timedelta

def monitoramento_continuo():
    """Monitora o status da API continuamente"""
    api = KDLApi()
    
    def verificar_status():
        try:
            # Verificar se a API está respondendo
            data_hoje = datetime.now()
            dados = api.get_fdib(
                data_hoje.day, 
                data_hoje.month, 
                data_hoje.year, 
                limit=1
            )
            
            print(f"✅ [{datetime.now().strftime('%H:%M:%S')}] API funcionando - {dados['total']} registros disponíveis")
            
            # Verificar se há problemas
            if dados['total'] == 0:
                print("⚠️ ALERTA: Nenhum registro encontrado para hoje")
            
            # Log do tempo de resposta
            elapsed = dados.get('elapsedTime', 'N/A')
            print(f"⏱️ Tempo de resposta: {elapsed}")
            
        except Exception as e:
            print(f"❌ [{datetime.now().strftime('%H:%M:%S')}] Erro na API: {e}")
    
    # Agendar verificações a cada 5 minutos
    schedule.every(5).minutes.do(verificar_status)
    
    print("🚀 Iniciando monitoramento contínuo...")
    print("📅 Verificações a cada 5 minutos")
    
    # Fazer primeira verificação
    verificar_status()
    
    # Loop principal
    while True:
        schedule.run_pending()
        time.sleep(60)  # Verificar a cada minuto se há tarefas agendadas

# Executar monitoramento
if __name__ == "__main__":
    monitoramento_continuo()
```

## 🚨 4. Tratamento de Erros Avançado

### Classe de Cliente Robusto (JavaScript)

```javascript
class KDLApiClient {
  constructor() {
    this.baseUrl = 'https://simcidadesinteligentes.com.br:44300';
    this.token = null;
    this.maxRetries = 3;
    this.retryDelay = 1000; // 1 segundo
  }

  async sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  async makeRequestWithRetry(endpoint, options = {}, retryCount = 0) {
    try {
      const response = await fetch(`${this.baseUrl}${endpoint}`, {
        ...options,
        headers: {
          'Authorization': `Bearer ${this.token}`,
          'Content-Type': 'application/json',
          ...options.headers
        }
      });

      // Token expirado - renovar e tentar novamente
      if (response.status === 401 && retryCount === 0) {
        console.log('🔄 Token expirado, renovando...');
        await this.login();
        return this.makeRequestWithRetry(endpoint, options, retryCount + 1);
      }

      // Rate limit - aguardar e tentar novamente
      if (response.status === 429 && retryCount < this.maxRetries) {
        const waitTime = Math.pow(2, retryCount) * this.retryDelay;
        console.log(`⏳ Rate limit atingido, aguardando ${waitTime}ms...`);
        await this.sleep(waitTime);
        return this.makeRequestWithRetry(endpoint, options, retryCount + 1);
      }

      // Erro de rede - tentar novamente
      if (response.status >= 500 && retryCount < this.maxRetries) {
        const waitTime = Math.pow(2, retryCount) * this.retryDelay;
        console.log(`🔄 Erro do servidor (${response.status}), tentando novamente em ${waitTime}ms...`);
        await this.sleep(waitTime);
        return this.makeRequestWithRetry(endpoint, options, retryCount + 1);
      }

      if (!response.ok) {
        throw new Error(`API Error: ${response.status} - ${response.statusText}`);
      }

      return response.json();

    } catch (error) {
      // Erro de rede/conexão - tentar novamente
      if (retryCount < this.maxRetries && (error.name === 'TypeError' || error.code === 'ECONNRESET')) {
        const waitTime = Math.pow(2, retryCount) * this.retryDelay;
        console.log(`🔄 Erro de conexão, tentando novamente em ${waitTime}ms...`);
        await this.sleep(waitTime);
        return this.makeRequestWithRetry(endpoint, options, retryCount + 1);
      }

      throw error;
    }
  }

  async login() {
    const response = await fetch(`${this.baseUrl}/login`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ username: 'teste', password: 'teste' })
    });

    if (!response.ok) {
      throw new Error(`Login failed: ${response.status}`);
    }

    const data = await response.json();
    this.token = data.token;
    console.log('✅ Login realizado com sucesso');
  }

  // Métodos de conveniência
  async getFdib(dia, mes, ano, options = {}) {
    const endpoint = `/fdib/v5/${String(dia).padStart(2, '0')}/${String(mes).padStart(2, '0')}/${ano}`;
    const params = new URLSearchParams(options);
    return this.makeRequestWithRetry(`${endpoint}?${params}`);
  }

  async getEtiqueta(etiqueta, tipo = 'fdib') {
    return this.makeRequestWithRetry(`/${tipo}/v5/${etiqueta}`);
  }
}
```

## 📈 5. Dashboard em Tempo Real

### Exemplo com HTML + JavaScript

```html
<!DOCTYPE html>
<html>
<head>
    <title>Dashboard KDL API</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .card { border: 1px solid #ddd; padding: 20px; margin: 10px 0; border-radius: 8px; }
        .success { background-color: #d4edda; border-color: #c3e6cb; }
        .error { background-color: #f8d7da; border-color: #f5c6cb; }
        .loading { background-color: #fff3cd; border-color: #ffeaa7; }
        .stats { display: flex; gap: 20px; flex-wrap: wrap; }
        .stat { background: #f8f9fa; padding: 15px; border-radius: 8px; min-width: 150px; }
    </style>
</head>
<body>
    <h1>📊 Dashboard KDL API</h1>
    
    <div id="status" class="card loading">
        <h2>🔄 Carregando...</h2>
    </div>
    
    <div class="stats">
        <div class="stat">
            <h3>Total de Luminárias</h3>
            <div id="totalLuminaras">-</div>
        </div>
        <div class="stat">
            <h3>Luminárias Ligadas</h3>
            <div id="luminariasLigadas">-</div>
        </div>
        <div class="stat">
            <h3>% Funcionamento</h3>
            <div id="percentualFuncionamento">-</div>
        </div>
        <div class="stat">
            <h3>Tempo de Resposta</h3>
            <div id="tempoResposta">-</div>
        </div>
    </div>
    
    <div id="subprefeituras" class="card">
        <h2>🏢 Status por Subprefeitura</h2>
        <div id="listaSubprefeituras">Carregando...</div>
    </div>

    <script>
        const client = new KDLApiClient(); // Usando a classe do exemplo anterior
        
        async function atualizarDashboard() {
            try {
                document.getElementById('status').className = 'card loading';
                document.getElementById('status').innerHTML = '<h2>🔄 Atualizando dados...</h2>';
                
                const hoje = new Date();
                const dados = await client.getFdib(hoje.getDate(), hoje.getMonth() + 1, hoje.getFullYear(), {
                    limit: 1000,
                    count: 'true'
                });
                
                // Calcular estatísticas
                const total = dados.data.result.length;
                const ligadas = dados.data.result.filter(item => item.status === 1).length;
                const percentual = total > 0 ? ((ligadas / total) * 100).toFixed(1) : 0;
                
                // Atualizar interface
                document.getElementById('status').className = 'card success';
                document.getElementById('status').innerHTML = '<h2>✅ Conectado</h2><p>Última atualização: ' + new Date().toLocaleTimeString() + '</p>';
                
                document.getElementById('totalLuminaras').textContent = total;
                document.getElementById('luminariasLigadas').textContent = ligadas;
                document.getElementById('percentualFuncionamento').textContent = percentual + '%';
                document.getElementById('tempoResposta').textContent = dados.elapsedTime;
                
                // Agrupar por subprefeitura
                const grupos = {};
                dados.data.result.forEach(item => {
                    if (!grupos[item.subPrefeitura]) {
                        grupos[item.subPrefeitura] = { total: 0, ligadas: 0 };
                    }
                    grupos[item.subPrefeitura].total++;
                    if (item.status === 1) grupos[item.subPrefeitura].ligadas++;
                });
                
                // Mostrar subprefeituras
                const listaHtml = Object.entries(grupos)
                    .map(([nome, stats]) => {
                        const perc = ((stats.ligadas / stats.total) * 100).toFixed(1);
                        return `<div style="margin: 5px 0;">${nome}: ${stats.ligadas}/${stats.total} (${perc}%)</div>`;
                    })
                    .join('');
                
                document.getElementById('listaSubprefeituras').innerHTML = listaHtml;
                
            } catch (error) {
                document.getElementById('status').className = 'card error';
                document.getElementById('status').innerHTML = '<h2>❌ Erro</h2><p>' + error.message + '</p>';
                console.error('Erro no dashboard:', error);
            }
        }
        
        // Atualizar a cada 30 segundos
        atualizarDashboard();
        setInterval(atualizarDashboard, 30000);
    </script>
</body>
</html>
```

---

*Para mais informações, consulte a [documentação principal](README.md) e o [guia de autenticação](autenticacao.md)*
