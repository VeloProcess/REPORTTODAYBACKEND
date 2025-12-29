# 🚀 Configuração do Render - REPORTTODAY Backend

## 📋 Informações do Repositório

- **Repositório GitHub**: https://github.com/VeloProcess/REPORTTODAYBACKEND.git
- **Branch**: `main`
- **Tipo de Serviço**: Web Service

## ⚙️ Configurações no Render

### 1. Configurações Básicas

| Campo | Valor |
|-------|-------|
| **Name** | `reporttoday-backend` |
| **Environment** | `Node` |
| **Region** | `São Paulo (Brazil)` ou mais próxima |
| **Branch** | `main` |
| **Root Directory** | (deixe vazio) |
| **Build Command** | `npm install` |
| **Start Command** | `npm start` |

### 2. Variáveis de Ambiente (Environment Variables)

Adicione as seguintes variáveis no painel do Render:

```env
# SERVIDOR
PORT=3005

# API 55PBX
API_55_URL=https://reportapi02.55pbx.com:50500/api/pbx/reports/metrics
API_55_TOKEN=46a5e4fe-5e16-4131-8bda-d845f2fa2392-2025122946145
API_55_USERNAME=Gabriel_Validação Ligações

# WHATSAPP API
WHATSAPP_API_URL=https://baileys-api-relat-rios.onrender.com
WHATSAPP_DESTINATION=5511922048764

# AGENDAMENTO
REPORT_TIMES=18:00

# AMBIENTE
NODE_ENV=production
```

### 3. Configurações Avançadas (Opcional)

- **Auto-Deploy**: `Yes` (deploy automático ao fazer push)
- **Health Check Path**: `/health`
- **Dockerfile Path**: (deixe vazio)

## 📝 Passo a Passo no Render

1. Acesse https://render.com e faça login
2. Clique em **"New +"** → **"Web Service"**
3. Conecte o repositório:
   - Selecione **"Connect GitHub"**
   - Autorize o Render
   - Escolha o repositório: **`VeloProcess/REPORTTODAYBACKEND`**
4. Configure o serviço conforme a tabela acima
5. Adicione todas as variáveis de ambiente listadas
6. Clique em **"Create Web Service"**

## 🔗 Após o Deploy

Após o deploy, você receberá uma URL como:
- `https://reporttoday-backend.onrender.com`

**⚠️ IMPORTANTE**: Atualize o frontend para usar esta URL!

## 📊 Monitoramento

- **Logs**: Acesse a aba "Logs" no painel do Render
- **Metrics**: Acompanhe CPU, Memória e Requisições
- **Events**: Veja histórico de deploys

## 🔧 Troubleshooting

### Se o servidor não iniciar:
- Verifique os logs no Render
- Confirme que todas as variáveis de ambiente estão configuradas
- Verifique se o `package.json` tem o script `start` correto

### Se houver erro de porta:
- O Render define automaticamente a variável `PORT`
- O código já está configurado para usar `process.env.PORT || 3005`

### Se a API 55PBX não retornar dados:
- Verifique se o token está correto e atualizado
- Confirme que a URL da API está correta
- Veja os logs detalhados no Render

---

**55SYSTEM** © 2024

