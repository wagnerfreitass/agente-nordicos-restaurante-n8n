# Agente de IA para atendimento e delivery

Workflow de n8n para pizzarias que reúne atendimento via WhatsApp, Instagram e Facebook, consulta ao cardápio e processamento de pedidos. Três agentes dividem o trabalho: um conduz a conversa, outro consulta produtos e o terceiro avalia se vale retomar um atendimento pendente.

## Como funciona

- **Entrada e contexto.** Webhooks recebem as mensagens. O fluxo identifica o canal, agrupa mensagens consecutivas e prepara texto, áudio, imagem ou PDF para o agente. O Supabase registra dados do cliente e o PostgreSQL mantém a memória da conversa. A intervenção de um atendente pode pausar a IA.
- **Consulta com RAG.** A atualização de um arquivo no Google Drive dispara a extração e a divisão do conteúdo em trechos. Embeddings da OpenAI permitem buscar informações por significado na base vetorial do Supabase.
- **Entrega e cobrança.** Durante o pedido, o agente pode consultar o CEP, usar o Mapbox para calcular a entrega e acionar uma rota interna para enviar o pedido. A integração com o Asaas cria a cobrança e registra o link de pagamento.
- **Follow-up.** Um gatilho periódico consulta os atendimentos pendentes no Supabase. O agente usa o histórico para decidir se envia uma mensagem pelo canal de origem.

## Configuração no n8n

1. Importe o [workflow em JSON](workflows/agente-ia-restaurante.json). Os nós comunitários usados no arquivo são `@splainez/n8n-nodes-phonenumber-parser` e `n8n-nodes-evolution-api`.
2. Configure as credenciais dos serviços usados, as tabelas no Supabase e a pasta monitorada no Google Drive. Revise os nós **Fixed Credentials** e **Fixed Credentials1** para os canais de atendimento.
3. Em **Apontamentos**, informe o token do Mapbox e as coordenadas do restaurante. Ajuste o cardápio, os preços e as regras de entrega do exemplo.
4. Confira a URL base do n8n e os caminhos dos webhooks internos. Teste com um contato de demonstração e configure o ambiente de testes do Asaas antes de simular a cobrança.

## 👨‍💻 Autor

**Wagner Freitas**

Desenvolvedor de Automações e IA\
n8n • AI Agents • Integrações • Low-Code

🔗 LinkedIn:\
[https://www.linkedin.com/in/wagner-freitas-1b5a0211b](https://www.linkedin.com/in/wagner-freitas-1b5a0211b)
