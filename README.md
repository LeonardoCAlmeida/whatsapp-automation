# Automação de Atendimento WhatsApp para Pequenos Negócios

Sistema automatizado de atendimento via WhatsApp que qualifica leads com IA e gerencia contatos comerciais.

## Funcionalidades

- Resposta automática a novos contatos via WhatsApp
- Classificação inteligente de leads como "quente", "morno" ou "frio" usando GPT-4
- Armazenamento de todo o histórico de conversas no Google Sheets
- Notificação para equipe comercial apenas quando leads são "quentes"

## Requisitos

- Docker e Docker Compose
- Conta na Twilio para WhatsApp API
- Conta na OpenAI para acesso ao GPT-4
- Conta no Google (para Google Sheets)
- Slack ou email para notificações

## Instalação

1. Clone este repositório:
   ```bash
   git clone https://github.com/LeonardoCAlmeida/whatsapp-automation.git
   cd whatsapp-automation
   ```

2. Configure as variáveis de ambiente copiando o arquivo de exemplo:
   ```bash
   cp .env.example .env
   ```

3. Edite o arquivo `.env` com suas credenciais:
   - Chave da API OpenAI
   - Credenciais da Twilio
   - Credenciais do Google Sheets
   - Webhook do Slack ou configuração de email

4. Inicie os contêineres:
   ```bash
   docker-compose up -d
   ```

5. Acesse o n8n em `http://localhost:5678` e importe o workflow do arquivo `whatsapp_automation_workflow.json`

## Configuração do WhatsApp API (Twilio)

1. Crie uma conta na [Twilio](https://www.twilio.com/)
2. Configure o WhatsApp Business API
3. Configure o webhook para apontar para sua instância n8n: `http://seu-dominio/webhook/whatsapp`

## Configuração do Google Sheets

1. Crie uma nova planilha no Google Sheets
2. Configure com as seguintes colunas:
   - Telefone
   - Nome
   - Mensagem
   - Classificação (quente/morno/frio)
   - Interesse
   - Data/Hora
   - Histórico

3. Configure as credenciais de API do Google conforme documentação do n8n

## Uso

O sistema funciona automaticamente após a configuração:

1. Cliente envia mensagem para seu número WhatsApp
2. Sistema responde automaticamente com perguntas estratégicas
3. GPT-4 analisa as respostas e classifica o lead
4. Dados são armazenados no Google Sheets
5. Se o lead for classificado como "quente", sua equipe é notificada

## Personalização

Você pode personalizar:
- Prompts do GPT-4 no arquivo `prompts/gpt4_classification.txt`
- Mensagens de resposta em `messages/responses.json`
- Fluxo de trabalho diretamente na interface do n8n

## Fluxo do Processo

1. **Recebimento de Mensagem**: Webhook recebe mensagens do WhatsApp via Twilio
2. **Classificação**: GPT-4 analisa a mensagem e classifica o lead
3. **Resposta Automática**: Sistema envia resposta personalizada conforme classificação
4. **Registro**: Todas as interações são registradas no Google Sheets
5. **Notificação**: Equipe comercial é notificada via Slack quando um lead "quente" é identificado

## Manutenção

### Backups
Recomenda-se fazer backups periódicos dos seguintes elementos:
- Volume do Docker contendo os dados do n8n
- Planilha do Google Sheets

### Atualizações
Para atualizar o sistema:
```bash
docker-compose down
git pull
docker-compose up -d
```

## Solução de Problemas

### Mensagens não estão sendo recebidas
- Verifique se o webhook está corretamente configurado na Twilio
- Confirme que sua instância n8n está acessível na internet
- Verifique os logs do n8n: `docker-compose logs -f n8n`

### Classificação inadequada de leads
- Ajuste o prompt do GPT-4 em `prompts/gpt4_classification.txt`
- Teste diferentes temperaturas de resposta no nó OpenAI do n8n

### Problemas com o Google Sheets
- Verifique as permissões da conta de serviço
- Confirme que o ID da planilha está correto no arquivo `.env`

## Suporte

Para suporte, entre em contato em [seu-email@exemplo.com]

## Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo LICENSE para detalhes.
