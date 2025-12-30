# 📚 Documentação Completa - API 55PBX

## 📋 Índice

1. [Informações Gerais](#informações-gerais)
2. [Configuração](#configuração)
3. [Autenticação](#autenticação)
4. [URL Base e Endpoints](#url-base-e-endpoints)
5. [Estrutura de URLs](#estrutura-de-urls)
6. [Parâmetros e Filtros](#parâmetros-e-filtros)
7. [Formato de Resposta](#formato-de-resposta)
8. [Funções Disponíveis](#funções-disponíveis)
9. [Rotas do Backend](#rotas-do-backend)
10. [Exemplos de Uso](#exemplos-de-uso)

---

## 🔧 Informações Gerais

### URL Base da API
```
https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics
```

### Método HTTP
- **GET** - Para consultas de dados

### Timeout
- **30 segundos** (30000ms)

### Content-Type
- **application/json**

---

## ⚙️ Configuração

### Variáveis de Ambiente

| Variável | Descrição | Obrigatório | Exemplo |
|----------|-----------|-------------|---------|
| `API_55_URL` | URL base da API | Não (tem padrão) | `https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics` |
| `API_55_TOKEN` | Token de autenticação | **SIM** | `46a5e4fe-5e16-4131-8bda-d845f2fa2392-2025122946145` |
| `API_55_USERNAME` | Username (opcional) | Não | `Gabriel_Validação Ligações` |
| `API_55_PASSWORD` | Password (opcional) | Não | (pode usar o token) |

### Arquivo de Configuração
- **Localização**: `BACKEND/ReportsDAY-Backend-main/API-55PBX/config.js`

---

## 🔐 Autenticação

### Headers de Autenticação

A API 55PBX usa headers customizados:

```javascript
{
  'key': 'SEU_TOKEN_AQUI',
  'Chave': 'SEU_TOKEN_AQUI',
  'Accept': 'application/json'
}
```

### Observações Importantes

⚠️ **ATENÇÃO**: O formato exato dos headers pode variar. O código atual usa:
- `key` (minúscula)
- `Chave` (com maiúscula)

**Possíveis variações a verificar:**
- `api_key` e `client_code` (com underscore)
- `api-key` e `client-code` (com hífen)
- Outro formato específico da documentação oficial

### Função de Autenticação

```javascript
// Localização: API-55PBX/config.js
export function getAuthHeaders() {
  return {
    'key': config.token,
    'Chave': config.token,
  };
}
```

---

## 🌐 URL Base e Endpoints

### URL Base Completa
```
https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics
```

### Endpoint Principal
```
GET /{dateStart}/{dateEnd}/{queue}/{number}/{agent}/{report}/{quiz_id}/{timezone}
```

---

## 📍 Estrutura de URLs

### Formato Completo da URL

```
{API_URL}/{dateStart}/{dateEnd}/{queue}/{number}/{agent}/{report}/{quiz_id}/{timezone}
```

### Exemplo Real

```
https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics/Fri%20May%2022%202020%2000%3A00%3A00%20GMT%20-0300/Fri%20May%2022%202020%2023%3A59%3A59%20GMT%20-0300/all_queues/all_numbers/all_agent/report_01/undefined/-3
```

### Formatação de Datas

As datas são formatadas como:
```
"Fri May 22 2020 00:00:00 GMT -0300"
```

E então codificadas para URL (URL encoding):
```
Fri%20May%2022%202020%2000%3A00%3A00%20GMT%20-0300
```

**Função de formatação:**
```javascript
function formatDateForAPI(date) {
  const formatted = date.toUTCString().replace('GMT', 'GMT -0300');
  return encodeURIComponent(formatted);
}
```

---

## 🎯 Parâmetros e Filtros

### Parâmetros da URL (Path Parameters)

| Posição | Parâmetro | Descrição | Valor Padrão | Exemplo |
|---------|-----------|-----------|--------------|---------|
| 1 | `dateStart` | Data/hora inicial | - | `Fri May 22 2020 00:00:00 GMT -0300` |
| 2 | `dateEnd` | Data/hora final | - | `Fri May 22 2020 23:59:59 GMT -0300` |
| 3 | `queue` | Fila de atendimento | `all_queues` | `all_queues` ou ID específico |
| 4 | `number` | Número telefônico | `all_numbers` | `all_numbers` ou número específico |
| 5 | `agent` | Agente/operador | `all_agent` | `all_agent` ou ID específico |
| 6 | `report` | Tipo de relatório | `report_01` | `report_01` |
| 7 | `quiz_id` | ID de pesquisa | `undefined` | `undefined` |
| 8 | `timezone` | Fuso horário | `-3` | `-3` (Brasil) |

### Filtros Padrão Configurados

```javascript
defaultFilters: {
  queue: 'all_queues',      // Todas as filas
  number: 'all_numbers',   // Todos os números
  agent: 'all_agent',      // Todos os agentes
  report: 'report_01',     // Relatório tipo 01
  quiz_id: 'undefined',    // Sem pesquisa
  interval: 'undefined',   // Sem intervalo específico
}
```

### Timezone

- **Padrão**: `-3` (Brasil - UTC-3)
- **Configuração**: `config.timezone = '-3'`

---

## 📦 Formato de Resposta

### Resposta Agregada (Report_01)

A API retorna dados agregados quando usa `report_01`:

```json
{
  "totalCallAttendedReceptive": 153,      // Ligações atendidas (receptivas)
  "totalCallAbandonedQueue": 0,          // Ligações abandonadas na fila
  "totalCallAbandonedURA": 27,           // Ligações retidas/abandonadas na URA
  "timeMediumWaitingAttendance": "00:00:06",  // Tempo médio de espera (HH:MM:SS)
  "timeMediumDuration": "00:03:45",      // Tempo médio de duração (HH:MM:SS)
  "sla_attendance": "85%",               // SLA de atendimento
  // ... outros campos
}
```

### Campos Principais

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `totalCallAttendedReceptive` | number | Total de ligações atendidas (receptivas) |
| `totalCallAbandonedQueue` | number | Total de ligações abandonadas na fila |
| `totalCallAbandonedURA` | number | Total de ligações retidas/abandonadas na URA |
| `timeMediumWaitingAttendance` | string | Tempo médio de espera (formato HH:MM:SS) |
| `timeMediumDuration` | string | Tempo médio de duração (formato HH:MM:SS) |
| `sla_attendance` | string | SLA de atendimento (formato "XX%") |

### Cálculo do Total

```javascript
const totalCalls = 
  parseInt(data.totalCallAttendedReceptive || 0) + 
  parseInt(data.totalCallAbandonedQueue || 0) + 
  parseInt(data.totalCallAbandonedURA || 0);
```

### Resposta como Array

Em alguns casos, a API pode retornar um array de ligações individuais:

```json
[
  {
    "call_id": "12345",
    "call_date": "2024-01-15T10:30:00Z",
    "call_status": "ANSWERED",
    "call_queue": "Fila_01",
    "call_time_waiting": 15,
    "call_duration": 180,
    // ... outros campos
  },
  // ... mais ligações
]
```

---

## 🔧 Funções Disponíveis

### 1. `fetchTodayCalls(date)`

Busca dados de ligações do dia atual (ou data específica).

**Parâmetros:**
- `date` (Date, opcional): Data de referência. Padrão: `new Date()`

**Retorno:**
- `Promise<Object>`: Dados agregados da API ou `null` em caso de erro

**Uso:**
```javascript
const data = await fetchTodayCalls();
// ou
const data = await fetchTodayCalls(new Date('2024-01-15'));
```

**Período:**
- Início: 00:00:00 do dia especificado
- Fim: Momento atual (ou 23:59:59 se for dia passado)

---

### 2. `fetchDayData(date)`

Busca dados de um dia específico (dia completo).

**Parâmetros:**
- `date` (Date): Data específica para buscar

**Retorno:**
```javascript
{
  date: "15/01/2024",
  atendidas: 153,
  abandonadas: 0,
  retidasURA: 27,
  total: 180
}
```

**Uso:**
```javascript
const dados = await fetchDayData(new Date('2024-01-15'));
```

**Período:**
- Início: 00:00:00 do dia
- Fim: 23:59:59 do dia

---

### 3. `fetchHistoricalData(days)`

Busca dados dos últimos N dias.

**Parâmetros:**
- `days` (number, opcional): Quantidade de dias. Padrão: `15`

**Retorno:**
```javascript
{
  dias: 15,
  historico: [
    { date: "14/01/2024", atendidas: 150, abandonadas: 2, retidasURA: 25, total: 177 },
    { date: "13/01/2024", atendidas: 145, abandonadas: 5, retidasURA: 30, total: 180 },
    // ... mais dias
  ],
  medias: {
    atendidas: 148,
    abandonadas: 3,
    retidasURA: 27,
    total: 178
  },
  lastUpdate: "2024-01-15T10:30:00.000Z"
}
```

**Uso:**
```javascript
const historico = await fetchHistoricalData(15);
```

**Observação:**
- Não inclui o dia atual
- Começa do dia anterior
- Delay de 300ms entre requisições para não sobrecarregar a API

---

### 4. `calculateDayKPIs()`

Calcula os KPIs do dia atual.

**Parâmetros:**
- Nenhum

**Retorno:**
```javascript
{
  totalCalls: 180,
  answered: 153,           // Atendidas
  abandoned: 0,            // Abandonadas na fila
  retainedURA: 27,         // Retidas na URA
  other: 0,
  peakHour: {              // Horário de pico (se disponível)
    hour: "14:00",
    count: 25
  },
  avgWaitTime: 6,          // Tempo médio de espera (segundos)
  lastUpdate: "2024-01-15T10:30:00.000Z",
  slaAttendance: "85%",
  timeMediumDuration: "00:03:45"
}
```

**Uso:**
```javascript
const kpis = await calculateDayKPIs();
```

---

### 5. `analisarDiaAtual()`

Analisa o dia atual comparando com histórico de 15 dias.

**Parâmetros:**
- Nenhum

**Retorno:**
```javascript
{
  hoje: {
    totalCalls: 180,
    answered: 153,
    abandoned: 0,
    retainedURA: 27,
    // ... outros KPIs
  },
  historico: {
    dias: 15,
    historico: [...],
    medias: {
      atendidas: 148,
      abandonadas: 3,
      retidasURA: 27,
      total: 178
    }
  },
  analise: {
    atendidas: {
      nivel: "Alto",
      emoji: "🟢",
      percentual: 103,
      descricao: "103% da média (esperado: 148)"
    },
    abandonadas: {
      nivel: "Abaixo do comum",
      emoji: "🔴",
      percentual: 0,
      descricao: "0% da média (esperado: 3)"
    },
    retidasURA: {
      nivel: "Médio",
      emoji: "🟡",
      percentual: 100,
      descricao: "100% da média (esperado: 27)"
    },
    total: {
      nivel: "Alto",
      emoji: "🟢",
      percentual: 101,
      descricao: "101% da média (esperado: 178)"
    }
  },
  resumo: "Dia 🟢 Alto - 101% do esperado"
}
```

**Uso:**
```javascript
const analise = await analisarDiaAtual();
```

**Classificações:**
- 🔴 **Abaixo do comum**: < 70% da média
- 🟡 **Médio**: 70% - 100% da média
- 🟢 **Alto**: 100% - 130% da média
- 🔥 **Altíssimo**: > 130% da média

---

### 6. `testConnection()`

Testa a conexão com a API.

**Parâmetros:**
- Nenhum

**Retorno:**
- `Promise<boolean>`: `true` se conectou, `false` caso contrário

**Uso:**
```javascript
const conectado = await testConnection();
```

---

### 7. `classifyCallStatus(callData)`

Classifica o status de uma chamada.

**Parâmetros:**
- `callData` (Object): Dados da chamada

**Retorno:**
- `'answered'` | `'abandoned'` | `'retained_ura'` | `'other'`

**Uso:**
```javascript
const status = classifyCallStatus({
  call_status: "ANSWERED",
  call_queue: "Fila_01"
});
```

---

### 8. `classificarNivel(valorAtual, media)`

Classifica o nível atual comparado com a média.

**Parâmetros:**
- `valorAtual` (number): Valor atual
- `media` (number): Média histórica

**Retorno:**
```javascript
{
  nivel: "Alto",
  emoji: "🟢",
  percentual: 103,
  descricao: "103% da média (esperado: 148)"
}
```

**Uso:**
```javascript
const nivel = classificarNivel(153, 148);
```

---

## 🛣️ Rotas do Backend

### Rotas que Usam a API 55PBX

#### 1. `GET /api/status`
Retorna status geral do sistema, incluindo configuração da API 55PBX.

**Resposta:**
```json
{
  "api55": {
    "configured": true
  },
  // ... outros status
}
```

---

#### 2. `GET /api/report/d0`
Retorna os KPIs do dia atual.

**Resposta:**
```json
{
  "totalCalls": 180,
  "answered": 153,
  "abandoned": 0,
  "retainedURA": 27,
  "other": 0,
  "peakHour": null,
  "avgWaitTime": 6,
  "lastUpdate": "2024-01-15T10:30:00.000Z"
}
```

**Função usada:** `calculateDayKPIs()`

---

#### 3. `GET /api/report/analise`
Retorna análise comparativa: hoje vs últimos 15 dias.

**Resposta:**
```json
{
  "hoje": { /* KPIs de hoje */ },
  "historico": { /* Histórico de 15 dias */ },
  "analise": { /* Análise comparativa */ },
  "resumo": "Dia 🟢 Alto - 101% do esperado"
}
```

**Função usada:** `analisarDiaAtual()`

---

#### 4. `GET /api/report/historico?dias=15`
Retorna histórico dos últimos N dias.

**Query Parameters:**
- `dias` (number, opcional): Quantidade de dias. Padrão: `15`

**Resposta:**
```json
{
  "dias": 15,
  "historico": [
    { "date": "14/01/2024", "atendidas": 150, "abandonadas": 2, "retidasURA": 25, "total": 177 },
    // ... mais dias
  ],
  "medias": {
    "atendidas": 148,
    "abandonadas": 3,
    "retidasURA": 27,
    "total": 178
  },
  "lastUpdate": "2024-01-15T10:30:00.000Z"
}
```

**Função usada:** `fetchHistoricalData(dias)`

---

#### 5. `POST /webhook/55pbx`
Recebe webhooks da 55PBX (modo legado - não usado atualmente).

**Headers:**
- `token` ou `authorization`: Token de validação

**Body:**
```json
{
  "call_id": "12345",
  "call_date": "2024-01-15T10:30:00Z",
  "call_status": "ANSWERED",
  // ... outros campos
}
```

**Observação:** Atualmente o sistema usa modo API (pull) ao invés de webhook (push).

---

## 💡 Exemplos de Uso

### Exemplo 1: Buscar KPIs do Dia Atual

```javascript
import api55Service from './API-55PBX/service.js';

const kpis = await api55Service.calculateDayKPIs();
console.log(`Total de ligações: ${kpis.totalCalls}`);
console.log(`Atendidas: ${kpis.answered}`);
console.log(`Abandonadas: ${kpis.abandoned}`);
```

---

### Exemplo 2: Buscar Dados de um Dia Específico

```javascript
import api55Service from './API-55PBX/service.js';

const data = new Date('2024-01-15');
const dadosDia = await api55Service.fetchDayData(data);

console.log(`Data: ${dadosDia.date}`);
console.log(`Total: ${dadosDia.total}`);
```

---

### Exemplo 3: Buscar Histórico de 30 Dias

```javascript
import api55Service from './API-55PBX/service.js';

const historico = await api55Service.fetchHistoricalData(30);
console.log(`Média de atendidas: ${historico.medias.atendidas}`);
```

---

### Exemplo 4: Análise Completa

```javascript
import api55Service from './API-55PBX/service.js';

const analise = await api55Service.analisarDiaAtual();
console.log(analise.resumo);
console.log(`Atendidas: ${analise.analise.atendidas.emoji} ${analise.analise.atendidas.nivel}`);
```

---

### Exemplo 5: Testar Conexão

```javascript
import api55Service from './API-55PBX/service.js';

const conectado = await api55Service.testConnection();
if (conectado) {
  console.log('✅ API 55PBX está acessível');
} else {
  console.log('❌ Erro ao conectar com API 55PBX');
}
```

---

### Exemplo 6: Requisição HTTP Direta (cURL)

```bash
curl -X GET \
  "https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics/Fri%20May%2022%202020%2000%3A00%3A00%20GMT%20-0300/Fri%20May%2022%202020%2023%3A59%3A59%20GMT%20-0300/all_queues/all_numbers/all_agent/report_01/undefined/-3" \
  -H "key: SEU_TOKEN_AQUI" \
  -H "Chave: SEU_TOKEN_AQUI" \
  -H "Accept: application/json"
```

---

## ⚠️ Tratamento de Erros

### Erros Comuns

#### 1. Erro 404 - Endpoint não encontrado
```
❌ API retornou status 404
⚠️ Endpoint não encontrado - verifique a URL da API
```

**Causas:**
- URL da API incorreta
- Formato de data incorreto
- Parâmetros inválidos

---

#### 2. Erro 417 - Expectation Failed
```
❌ API retornou status 417
⚠️ Erro 417 - verifique autenticação e formato da requisição
```

**Causas:**
- Header `Expect` presente (removido automaticamente pelo interceptor)
- Formato de autenticação incorreto
- Token inválido

**Solução:**
O código já remove automaticamente o header `Expect` via interceptor do Axios.

---

#### 3. Erro de Timeout
```
❌ API-55PBX: Sem resposta do servidor
💡 Verifique: URL da API, conexão de rede, firewall
```

**Causas:**
- API indisponível
- Problemas de rede
- Firewall bloqueando

---

#### 4. Dados Vazios
```
⚠️ API retornou dados, mas todos os valores estão zerados
💡 Possíveis causas:
   - Período sem ligações
   - Filtros muito restritivos
   - Token expirado ou inválido
```

---

## 📝 Observações Importantes

### 1. Formato de Autenticação

⚠️ **O formato exato dos headers de autenticação precisa ser verificado na documentação oficial da 55PBX.**

O código atual usa:
- `key` e `Chave` (ambos com o mesmo valor do token)

**Possíveis variações:**
- `api_key` e `client_code`
- `api-key` e `client-code`
- Outro formato específico

### 2. Formato de Data

As datas devem ser formatadas como:
```
"Fri May 22 2020 00:00:00 GMT -0300"
```

E codificadas para URL antes de enviar.

### 3. Rate Limiting

O código implementa delay de 300ms entre requisições ao buscar histórico para não sobrecarregar a API.

### 4. Timeout

O timeout padrão é de 30 segundos. Ajuste se necessário.

### 5. Validação de Status

O código aceita status HTTP < 500 como válido para capturar erros 4xx sem lançar exceção.

---

## 🔗 Arquivos Relacionados

- **Configuração**: `BACKEND/ReportsDAY-Backend-main/API-55PBX/config.js`
- **Serviço**: `BACKEND/ReportsDAY-Backend-main/API-55PBX/service.js`
- **Rotas**: `BACKEND/ReportsDAY-Backend-main/CORE/routes.js`
- **README**: `BACKEND/ReportsDAY-Backend-main/API-55PBX/README.md`

---

## 📞 Suporte

Para questões sobre a API 55PBX:
1. Consulte a documentação oficial da 55PBX
2. Verifique o painel de configurações da 55PBX
3. Entre em contato com o suporte da 55PBX

---

**Última atualização:** 2024-01-15  
**Versão do documento:** 1.0

