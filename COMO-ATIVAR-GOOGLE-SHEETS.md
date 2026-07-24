# 📊 Como ativar a captura de leads na sua Planilha Google

> **Sua planilha oficial:** [Formação Terapeutas Cristãs — Leads](https://docs.google.com/spreadsheets/d/1HDj4zBbvabupnw-6tSONPpjZkpLTDl1U1_3Xp8skU6o/edit)
> **Tempo estimado:** 5 minutos ⏱️

---

## ✅ O que já está pronto na página

- ✅ Formulário funcional (nome, WhatsApp com máscara automática, e-mail opcional)
- ✅ **Entrega automática do PDF "Desafio de 30 dias de oração"**
- ✅ Redirecionamento automático para o grupo do WhatsApp em 3 segundos
- ✅ Backup local de todos os leads (exportável em CSV pelo painel)
- ✅ Tracking com Meta Pixel e Google Analytics
- ✅ Estrutura pronta para receber a URL do Apps Script

## ⏳ O que falta

Apenas **colar 1 URL** no painel de configuração da página. Só isso.

---

## 🚀 Passo a passo (5 minutos)

### 1️⃣ Preparar a planilha

1. Abra sua planilha: [clique aqui](https://docs.google.com/spreadsheets/d/1HDj4zBbvabupnw-6tSONPpjZkpLTDl1U1_3Xp8skU6o/edit)
2. Na primeira linha, coloque os cabeçalhos (se ainda não tiver):

| A | B | C | D | E | F |
|---|---|---|---|---|---|
| Data/Hora | Nome | WhatsApp | E-mail | Origem | User Agent |

3. **Dica de formatação:** selecione a linha 1, deixe em negrito, com fundo roxo `#5B2A8B` e texto branco. Fica mais elegante e fácil de identificar.

### 2️⃣ Abrir o Apps Script

1. Na sua planilha, clique em **Extensões → Apps Script**
2. Uma nova aba abrirá com o editor de código
3. **Apague** todo o código que já estiver lá (a função `myFunction`)

### 3️⃣ Colar o código já customizado para SUA planilha

Copie **exatamente** este código (já vem com o ID da sua planilha configurado):

```javascript
// ============================================
// Webhook — Formação Terapeutas Cristãs
// Captura leads do formulário do Link da Bio
// ============================================

const SPREADSHEET_ID = '1HDj4zBbvabupnw-6tSONPpjZkpLTDl1U1_3Xp8skU6o';
const SHEET_NAME = 'Página1'; // Ajuste se o nome da aba for diferente

function doPost(e) {
  try {
    const sheet = SpreadsheetApp
      .openById(SPREADSHEET_ID)
      .getSheetByName(SHEET_NAME) || SpreadsheetApp.openById(SPREADSHEET_ID).getSheets()[0];

    const data = JSON.parse(e.postData.contents);

    // Adiciona nova linha com os dados do lead
    sheet.appendRow([
      new Date(),                    // A - Data/Hora
      data.nome || '',               // B - Nome
      data.whatsapp || '',           // C - WhatsApp
      data.email || '',              // D - E-mail
      data.origem || 'Link da Bio',  // E - Origem
      data.userAgent || ''           // F - User Agent
    ]);

    return ContentService
      .createTextOutput(JSON.stringify({ success: true, message: 'Lead capturado' }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (err) {
    // Registra erro em caso de falha
    console.error('Erro:', err);
    return ContentService
      .createTextOutput(JSON.stringify({ success: false, error: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput('Webhook Formação Terapeutas Cristãs — ativo ✓')
    .setMimeType(ContentService.MimeType.TEXT);
}

// Função de teste — execute uma vez para autorizar acesso à planilha
function testarConexao() {
  const sheet = SpreadsheetApp.openById(SPREADSHEET_ID).getSheets()[0];
  Logger.log('Planilha conectada: ' + sheet.getName());
  sheet.appendRow([
    new Date(), 'TESTE', '(00) 00000-0000', 'teste@teste.com', 'Teste manual', 'Apps Script'
  ]);
  Logger.log('Linha de teste adicionada ✓');
}
```

### 4️⃣ Salvar e testar

1. Pressione **Ctrl+S** (ou clique em 💾) e dê o nome: **"Webhook FTC"**
2. No topo, ao lado do botão "Executar", escolha a função **`testarConexao`**
3. Clique em **▶️ Executar**
4. Vai pedir autorização:
   - Clique em **"Revisar permissões"**
   - Escolha sua conta Google
   - Se aparecer aviso "App não verificado" → **"Avançado" → "Acessar Webhook FTC (não seguro)"** → **"Permitir"**
   > ⚠️ *É seu próprio script. Esse aviso é padrão para scripts pessoais.*
5. **Volte na planilha** — deve aparecer uma linha de teste com "TESTE" no nome
6. Se apareceu, pode apagar essa linha de teste. Se não apareceu, revise o passo 3.

### 5️⃣ Publicar como Web App

1. No editor do Apps Script, clique em **"Implantar" → "Nova implantação"** (canto superior direito)
2. Clique no ⚙️ **ícone de engrenagem** ao lado de "Selecionar tipo"
3. Escolha **"App da Web"**
4. Preencha:
   - **Descrição:** `Webhook FTC v1`
   - **Executar como:** `Eu (seu-email@gmail.com)`
   - **Quem pode acessar:** `Qualquer pessoa` ⚠️ **(muito importante!)**
5. Clique em **"Implantar"**
6. **Copie a URL do App da Web** que aparecer
   > Formato: `https://script.google.com/macros/s/AKfyc.../exec`

### 6️⃣ Colar no painel da página

1. Abra o arquivo `painel-configuracao.html` do seu projeto
2. Encontre o campo **"URL do Google Apps Script (Webhook do Sheets)"**
3. **Cole a URL copiada**
4. Clique em **💾 Salvar Configurações**

**Pronto! ✨** A partir de agora, cada visitante que preencher o formulário terá seus dados salvos automaticamente na sua planilha em tempo real.

---

## 🧪 Como testar após ativar

1. Abra sua página no navegador
2. Clique no botão do presente (🎁)
3. Preencha:
   - Nome: `Teste FTC`
   - WhatsApp: `(11) 99999-9999`
   - E-mail: `teste@ftc.com`
4. Envie
5. Volte na planilha — o lead deve aparecer em segundos

Se **não aparecer**, verifique:
- ✅ A URL do Web App começa com `https://script.google.com/macros/s/` e termina com `/exec`
- ✅ Você escolheu **"Qualquer pessoa"** em "Quem pode acessar" (⚠️ não "Qualquer pessoa com uma conta Google")
- ✅ Autorizou o acesso à planilha na função `testarConexao`
- ✅ Salvou as configurações no painel após colar a URL
- ✅ O nome da aba da planilha bate com `SHEET_NAME` no código (padrão: "Página1")

---

## 🔄 Como atualizar o código no futuro

Se precisar modificar algo (adicionar campos, ajustar formatação, etc.):

1. Volte no Apps Script
2. Edite o código
3. Clique em **"Implantar" → "Gerenciar implantações"**
4. Clique no ✏️ **lápis** ao lado da sua implantação
5. Em **"Versão"** escolha **"Nova versão"**
6. Clique em **"Implantar"**

⚠️ **A URL permanece a mesma** — não precisa mexer no painel novamente.

---

## 📥 Backup local (ativo desde já)

Enquanto o Google Sheets não estava ativo, todos os leads foram salvos no navegador. Para baixar:

1. Abra `painel-configuracao.html`
2. Clique em **"📥 Exportar Leads (CSV)"**
3. Baixa um arquivo `.csv` com todo o histórico

⚠️ **Depois de ativar o Sheets**, os novos leads vão para os dois lugares (planilha + backup local). Isso serve como redundância caso a internet falhe.

---

## 💡 Automações extras que podemos adicionar depois

Quando quiser evoluir:

- 📧 **Envio automático de e-mail** com o PDF anexado (via Gmail + Apps Script)
- 🤖 **Envio automático de WhatsApp** com mensagem de boas-vindas (via Z-API, WhatsApp Business API)
- 🔔 **Notificação instantânea** para você toda vez que houver novo lead (Telegram, Discord, e-mail)
- 📊 **Dashboard de conversão** com gráficos em tempo real (Looker Studio conectado à planilha)
- 🔗 **Integração com CRM** (RD Station, ActiveCampaign, HubSpot, Mailchimp)
- 🎯 **Segmentação por origem** (Instagram, YouTube, Facebook = tags diferentes)
- 📅 **Agendamento automático** de mensagens no dia da aula ao vivo

É só pedir. 🙏
