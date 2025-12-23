## Processo de Venda

**Abrir Venda**
`POST /api/sales` 
Cria uma nova venda no sistema.

**Body:**
```json
{
  "referenceNumber": "123", // Número da mesa, cartão ou identificador
  "saleType": "TABLE", // TABLE, COUNTER, CARD, DELIVERY
  "peopleCount": 4 // Obrigatório quando tipo TABLE
}
```

**Retorno:** Dados completos da venda criada (ID, status, referência, tipo, data/hora de abertura)

---

**Adicionar Produto**
`POST /api/sales/{saleId}/items` 
Adiciona um ou mais produtos à venda.

**Body:**
```json
{
  "customerId": "opcional", // Identificação da pessoa/consumidor
  "productId": "123",
  "quantity": 1.5, // Unitário ou peso (Kg)
  "observation": "Sem cebola",
  "selections": [] // Array de seleções (combos, complementos, extras)
}
```

**Retorno:** Dados do item adicionado (ID do item, produto, quantidade, valor unitário, valor total, observações)

---

**Alterar Produto**
`PATCH /api/sales/{saleId}/items/{itemId}`
Modifica informações de um produto já adicionado.

**Body:**
```json
{
  "quantity": 2,
  "observation": "Bem passado"
}
```

**Retorno:** Dados atualizados do item
**Validação:** O Produto só será alterado com sucesso caso não esteja com status produzido 

---

**Remover Produto**
`DELETE /api/sales/{saleId}/items/{itemId}` 
Remove um produto da venda.

**Validação:** So remove com sucesso produtos com status em aberto 

---

**Mandar para Produção**
`POST /api/sales/{saleId}/production`
Envia itens para impressão/exibição nas áreas de produção.

**Body:**
```json
{
  "itemIds": ["1", "2", "3"] // Opcional - se vazio, envia todos os produtos em aberto
}
```

---

**Cancelar Produção**
`DELETE /api/sales/{saleId}/production`
Cancela itens que já foram enviados para produção.

**Body:**
```json
{
  "itemIds": ["1", "2"],
  "cancellationReasonId": "123" 
}
```

---

**Transferir Itens**
`POST /api/sales/{saleId}/transfer`
Move itens entre vendas/mesas/comandas.

**Body:**
```json
{
  "destinationType": "TABLE", // TABLE, COUNTER, CARD
  "destinationReference": "45", // Número da mesa/cartão de destino
  "items": [
    {
      "itemId": "1",
      "quantity": 2
    }
  ],
  "transferReasonId": "123"
}
```

**Retorno:** Dados da venda de destino com os itens transferidos ja adicionados 
**Validação:**  Venda só pode receber transferencias caso esteja com status em aberto

---
**Divisão de Conta**
`POST /api/sales/{saleId}/split`
Realiza a divisão matemática da conta.

**Body:**
```json
{
  "splitType": "EQUAL", // EQUAL, BY_ITEMS, BY_PERSON
  "splitCount": 4, // Número de divisões
  "items": [12,32,4] 
}
```

**Retorno:** Dados da venda

---

**Aplicar Desconto em Produto**
`POST /api/sales/{saleId}/items/discount`
Aplica desconto em produtos específicos.

**Body:**
```json
{
  "itemIds": ["1", "2"],
  "discountReasonId": "123",
  "discountPercentage": 10.5 // Opcional - usa o do motivo se não informado
}
```

---

**Definir Prioridade de Impressão**
`PATCH /api/sales/{saleId}/items/priority` 
Altera a prioridade de produção de itens.

**Body:**
```json
{
  "itemIds": ["1", "2"],
  "priority": "HIGH" // LOW, MEDIUM, HIGH
}
```

**Retorno:** Confirmação da alteração de prioridade

---

**Checkout**
`POST /api/sales/{saleId}/checkout`
Inicia o processo de fechamento da conta (bloqueia novas inserções).

**Retorno:** Dados da venda atualizado

---

**Gerar Pré-conta**
`POST /api/sales/{saleId}/pre-bill`
Gera a imagem/documento da pré-conta.

**Retorno:** base64 da impressão 

--- 

**Reabrir Venda**
`POST /api/sales/{saleId}/reopen`
Reabre uma venda que estava em checkout ou fechada.

**Retorno:** Status atualizado da venda

---

**Imprimir Pré-conta**
`POST /api/sales/{saleId}/pre-bill/print`
Envia comando de impressão da pré-conta.

**Body:**
```json
{
  "printerId": "opcional" // ID da impressora específica
}
```

---
### Aplicação de Taxas e Motivos

**Aplicar Covert**
`POST /api/sales/{saleId}/charges/covert`
Aplica taxa de covert artístico.

**Body:**
```json
{
  "covertId": "123",
  "value": 15.00, // Valor fixo OU
  "percentage": 10 // Percentual sobre o total
}
```

**Retorno:** Dados da venda atualizado

---

**Aplicar Taxa de Serviço**
`POST /api/sales/{saleId}/charges/service`
Aplica taxa de serviço (gorjeta).

**Body:**
```json
{
  "serviceFeeId": "123",
  "value": 25.00, // OU
  "percentage": 10
}
```

**Retorno:** Dados da venda atualizado

---

**Aplicar Taxa de Entrega**
`POST /api/sales/{saleId}/charges/delivery`
Aplica taxa de entrega para deliveries.

**Body:**

```json
{
  "deliveryFeeId": "123",
  "value": 8.00, // OU
  "percentage": 5
}
```

**Retorno:** Dados da venda atualizado

---

**Aplicar Desconto na Venda**
`POST /api/sales/{saleId}/discount`
Aplica desconto geral sobre toda a venda.

**Body:**

```json
{
  "discountReasonId": "123",
  "value": 50.00, // OU
  "percentage": 15
}
```

**Retorno:** Dados da venda atualizado

---
### Gestão de Consumidores

**Alterar Quantidade de Pessoas**
`PATCH /api/sales/{saleId}/people-count`
Atualiza o número de pessoas na mesa.

**Body:**
```json
{
  "peopleCount": 6 // Mínimo limitado pela quantidade de consumidores vinculados
}
```

**Retorno:** Dados da venda atualizado
**Validação:** Só pode ser aplicado a vendas do tipo mesa

---

**Adicionar/Alterar Consumidor**
`POST /api/sales/{saleId}/consumers`
`PATCH /api/sales/{saleId}/consumers/{consumerId}` 
Gerencia dados de consumidores individuais na venda.

**Body:**
```json
{
  "cardNumber": "123",
  "position": "1", // Posição da pessoa na mesa
  "name": "João Silva",
  "cpf": "12345678900",
  "phone": "11999999999",
  "birthDate": "1990-01-01",
  "address": {...},
  "permanentObservation": "Alérgico a frutos do mar",
  "observation": "Observação desta venda",
  "groupId": "opcional",
  "useCpfInInvoice": true
}
```

**Retorno:** Dados da venda atualizado
**Validação:** Criação de novos consumidores é restrito a mesas apenas

---

### Pagamentos

**Adicionar Pagamento** 
`POST /api/sales/{saleId}/payments`
Registra um pagamento na venda.

**Body:**
```json
{
  "paymentTypeId": "123",
  "cardId": "opcional", // ID da bandeira do cartão
  "value": 100.00,
  "installments": 3,
  "nsu": "123456",
  "authorization": "ABC123",
  "extra": {}, // JSON de retorno da integração
  "integrationType": "CIELO" // CIELO, REDE, STONE, etc
}
```

**Retorno:** Dados da venda atualizado

---

**Remover/Cancelar Pagamento**
`DELETE /api/sales/{saleId}/payments/{paymentId}`
Remove um pagamento previamente registrado.

**Retorno:** Dados da venda atualizado

---
### Finalização

**Emitir Nota Fiscal** 
`POST /api/sales/{saleId}/invoice`
Gera, assina e transmite a nota fiscal eletrônica.

**Retorno:** Dados da nota fiscal emitida (XML, chave de acesso, protocolo, eventuais erros de validação)

---

**Cancelar Nota Fiscal**
`DELETE /api/sales/{saleId}/invoice`
Cancela uma nota fiscal previamente emitida.

**Body:**
```json
{
  "cancellationReasonId": "123"
}
```

---

**Finalizar Venda sem Emissão**
`POST /api/sales/{saleId}/finalize`
Finaliza a venda sem emitir cupom fiscal (quando permitido).

**Retorno:** Dados da venda

---
### Consultas de Vendas

**Buscar Venda Específica**
`GET /api/sales/{saleId}`
Recupera todos os dados de uma venda.

**Retorno:** Dados completos da venda (itens, pagamentos, consumidores, taxas aplicadas, descontos, status, histórico)

---

**Listar Vendas**
`GET /api/sales`
Lista vendas com filtros.

**Query Params:**

- `saleId`: ID da venda
- `showReservarions`: Se mostra as vendas de reserva 
- `reference`: Número de referência (mesa/cartão)
- `saleType`: Tipo de venda (TABLE, COUNTER, etc)
- `createdFrom`: Data inicial de criação
- `createdTo`: Data final de criação
- `finalizedFrom`: Data inicial de finalização
- `finalizedTo`: Data final de finalização
- `status`: Status da venda

**Retorno:** Lista de vendas com dados resumidos

---

### CRM e Clientes

**Criar/Atualizar Cliente**
`POST /api/crm/customers` 
`PATCH /api/crm/customers/{customerId}`
Gerencia cadastro de clientes.

**Body:**
```json
{
  "name": "João Silva",
  "cpf": "12345678900",
  "phone": "11999999999",
  "email": "joao@example.com",
  "birthDate": "1990-01-01",
  "address": {...},
  "observations": "Cliente VIP"
}
```

**Retorno:** Dados completos do cliente cadastrado/atualizado

---

**Buscar Cliente**
`GET /api/crm/customers/search`
Busca clientes por múltiplos critérios.

**Query Params:**

- `cpf`: CPF do cliente
- `name`: Nome (busca parcial)
- `phone`: Telefone
- `email`: Email

**Retorno:** Lista de clientes que correspondem aos critérios

---

**Check-in de Cliente**
`POST /api/sales/{saleId}/checkin`
Vincula um cliente à venda atual.

**Body:**
```json
{
  "customerId": "123"
}
```

**Retorno:** Dados da venda

---

**Extrato de Cliente**
`GET /api/crm/customers/{customerId}/statement`
Gera extrato de conta corrente do cliente.

**Query Params:**

- `startDate`: Data inicial
- `endDate`: Data final

**Retorno:** Lista de movimentações (vendas, créditos, débitos) e saldo atual

---

**Saldo de Cliente**
`GET /api/crm/customers/{customerId}/balance`
Consulta saldo atual da conta corrente.

**Retorno:** Saldo atual (positivo = crédito, negativo = débito)

---

**Registrar Crédito/Débito**
`POST /api/crm/customers/{customerId}/transactions`
Adiciona movimentação na conta corrente.

**Body:**
```json
{
  "type": "CREDIT", // CREDIT ou DEBIT
  "value": 100.00,
  "description": "Recarga de créditos",
  "transferToCustomerId": "opcional" // Para transferências entre clientes
}
```

---

**Consultar Cashback (EuFalo)**
`GET /api/crm/customers/{customerId}/cashback`
Consulta saldo de cashback disponível.

**Query Params:**

- `cpf`: CPF do cliente (alternativa ao ID)

**Retorno:** Saldo de cashback disponível

---

### Configurações e Parâmetros

**Obter Parâmetro**
`GET /api/settings/parameters/{parameterName}`
Consulta valor de um parâmetro específico.

**Retorno:** Nome e valor do parâmetro

---

**Alterar Parâmetro**
`PATCH /api/settings/parameters/{parameterName}`
Atualiza valor de um parâmetro.

**Body:**
```json
{
  "value": "novo valor"
}
```

**Retorno:** Parâmetro atualizado

---

**Dados do Estabelecimento**
`GET /api/settings/establishment`
Recupera informações do estabelecimento.

**Retorno:** Dados completos (razão social, CNPJ, endereço, configurações fiscais, licença)

---

**Lista de Impressoras**
`GET /api/settings/printers`
Lista impressoras configuradas.

**Retorno:** Lista de impressoras com tipo, nome, endereço e status

---

### Caixa e Movimento

**Status do Movimento**
`GET /api/cash/movement/status`
Verifica status do movimento atual (Aberto, Bloqueado, Vencido).

**Retorno:** Status do movimento, data de abertura, operador, necessidade de fechamento

---

**Abrir Movimento**
`POST /api/cash/movement`
Abre um novo turno de trabalho.

**Body:**
```json
{
  "operatorId": "123",
  "initialValue": 200.00 // Valor inicial em caixa
}
```

**Retorno:** Dados do movimento aberto

---

**Suprimento/Sangria**
`POST /api/cash/movement/transactions`
Registra entrada (suprimento) ou saída (sangria) de valores.

**Body:**
```json
{
  "type": "SUPPLY", // SUPPLY ou WITHDRAWAL
  "value": 100.00,
  "reason": "Troco inicial"
}
```

**Retorno:** Comprovante da operação bitmap 

---

**Dados do Movimento do Operador**
`GET /api/cash/movement/operator/{operatorId}`
Consolida dados financeiros do operador.

**Retorno:** Vendas, cancelamentos, descontos, formas de pagamento, ticket médio, totalizadores

---

**Fechamento de Caixa**
`POST /api/cash/movement/operator/{operatorId}/close`
Fecha o movimento atual.

**Body:**
```json
{
 
 "saleIds": [12,13,14], // Lista das vendas referentes aos valores declarados
  "declaredValues": {
    // Valores declarados por forma de pagamento
    "money": 500.00,
    "creditCard": 1200.00,
    // ...
  }
}
```

**Retorno:** Relatório de fechamento (vendas, pagamentos, diferenças, totalizadores)

---

**Relatório de Fechamento**
`GET /api/reports/cash-closure`
Gera relatório de fechamento (simplificado ou completo).

**Query Params:**

- `movementId`: ID do movimento
- `type`: SIMPLE ou COMPLETE

**Retorno:** Dados estruturados do relatório (vendas, cancelamentos, descontos, formas de pagamento, totalizadores)

---

### Relatórios Financeiros

**Apuração de Pagamentos**
`GET /api/reports/payment-summary` 
Agrupa e totaliza pagamentos por tipo e bandeira.

**Query Params:**

- `startDate`: Data inicial
- `endDate`: Data final
- `blind`: true/false (conferência cega ou informada)

**Retorno:** Totalizadores por forma de pagamento e bandeira de cartão

---

**Descontos e Cancelamentos**
`GET /api/reports/discounts-cancellations`
Consolida descontos aplicados e itens cancelados.

**Query Params:**

- `startDate`: Data inicial
- `endDate`: Data final

**Retorno:** Quantitativos e valores de descontos e cancelamentos com detalhamento

---

**Total de Gorjetas**
`GET /api/reports/tips-summary`
Soma taxas de serviço e gorjetas.

**Query Params:**

- `startDate`: Data inicial
- `endDate`: Data final

**Retorno:** Total de gorjetas recebidas no período

---

## Logos Admin (Computador Local)

**Status do Servidor**
`GET /api/admin/server/status`
Verifica status de conexão e sincronização.

**Retorno:** Status do banco de dados, usuário conectado, status de sincronização, data da última sync

---

**Configurar Banco de Dados**
`POST /api/admin/database/configure`
Define configurações de conexão com o banco.

**Body:**
```json
{
  "host": "localhost",
  "port": 3306,
  "user": "root",
  "password": "senha123"
}
```

**Retorno:** Confirmação da configuração e teste de conexão

---

**Dados do Container/Estabelecimento**
`GET /api/admin/setup/{cnpj}`
Busca informações do container para iniciar o setup baseado no cnpj 

**Retorno:** Dados do container, loja, CNPJ, configurações de estoque

---

## Integrações e Processos Assíncronos Periodicos

> [!info] As funcionalidades abaixo **não são endpoints REST** acessíveis pelo frontend. São processos de background que rodam continuamente no servidor através de polling, timers ou event listeners.

### Runners Periódiodicos

Processo que controla intervalos de execução de todas as tarefa periódicas  

- Renovação de licença
- Atualização de schema do banco
- Download de dados mestres (produtos, usuários, configurações)
- Upload de vendas e fechamentos de caixa
- Auditoria de integridade
- Download de novas versões do software
- Backup automático

**Frequência:** Configurável por tipo de tarefa (ex: vendas a cada 5min, produtos a cada 30min)
**Logs:** Todas as operações geram logs de sucesso/falha para monitoramento

---

### Integrações de Delivery 

As seguintes plataformas são monitoradas continuamente:

**iFood (Classe iFood)**

- Busca novos pedidos via API
- Monitora eventos (cancelamentos, atualizações)
- Sincroniza catálogo
- Gerencia autenticação OAuth 2.0
- Envia atualizações de status (confirmado, pronto, despachado)

**Goomer Delivery (Classe GoomerDelivery)**

- Polling de eventos e novos pedidos
- Confirmação de recebimento de eventos
- Conversão de dados para formato interno
- Envio de ações (aceitar, preparar, despachar, cancelar)
- Gestão de status da loja (aberta/pausada)

**Keeta (Classe Keeta)**

- Renovação automática de tokens
- Descriptografia de dados sensíveis (LGPD)
- Geração de assinaturas digitais
- Polling de eventos
- Onboarding de novos estabelecimentos

**CADIS (Classe Cadis)**

- Resolução de IDs de lojas
- Mapeamento de itens e sub-itens
- Notificação de mudanças de status
- Conversão de IDs de plataformas parceiras

**Foodie (Classe Foodie)**

- Loop de processamento (foodTick)
- Importação de pedidos PLACED
- Gestão de pausas e reaberturas
- Sincronização de catálogo

**Anota AI (Classe AnotaAi)**

- Verificações periódicas de novos pedidos
- Aceitação e finalização de pedidos
- Sincronização de catálogo
- Gestão de status operacional

**Abrahão (Classe Abrahao)**

- Processamento de pedidos de autoatendimento
- Eventos de serviço (chamar garçom, pedir conta)
- Atualização de status de pedidos e eventos
- Sincronização de cardápio

**Comportamento Geral:**

- Todos os pedidos são convertidos para o formato interno padronizado
- Salvos no banco local
- Impressos automaticamente nas áreas de produção
- Status atualizado nas plataformas conforme progresso

---
### Integrações de Pagamento

**TEF Socket/VBI (Classe Destaxa)** Comunicação direta com terminal de pagamento (Pinpad):

- Processamento de vendas (crédito/débito/PIX)
- Cancelamentos administrativos
- Consultas e reimpressões
- Recuperação de comprovantes

**PayGo/ControlPay (Classe ControlPay)** Gateway API REST com monitoramento:

- Varredura de pagamentos pendentes no banco
- Criação de intenções de venda
- Monitoramento de status de transações
- Tradução de códigos de status
- Geração de comprovantes

**PIX Dinâmico (Classe Paggi)**

- Geração de cobranças com QR Code
- Consulta de status (pendente/pago/expirado)
- Cancelamento de cobranças
- Conversão de strings PIX em imagens

**Cashback EuFalo (Classe PayAux e EuFalo)** Processos de fidelização:

- Sincronização de vendas para acúmulo
- Consulta de saldos
- Processamento de resgates
- Estornos em caso de cancelamento
- Sync de cadastros mestres

---

### Fila de Impressão (Classe Queue)

**Processamento de Jobs:**
- Monitor contínuo da tabela de jobs
- Separação por tipo de dispositivo (Impressora, Fiscal, Comando)
- Execução distribuída entre terminais
- Atualização automática de status
- Retry em caso de falhas

**Tipos de Jobs:**

- Impressão de produção.
- Comandas e pré-contas
- Comprovantes de pagamento
- Relatórios

---

### Arcade/Entretenimento (Classe SPIX)

**Sincronização de Créditos**

- Processamento de vendas de fichas
- Ativação de cartões magnéticos/RFID
- Recarga de créditos
- Consultas de saldo
- Integração com sistema externo de gestão

---

### Comunicação Entre Terminais


**Canal Socket:**

 Notificações em tempo real 
 - Venda criada
 - Solicitação de Impressão
 - Notificações
 - Etc

---

### Atualização de Software

**Processo Automático:**

- Verificação periódica de novas versões
- Download automático de executáveis
- Instalação programada
- Garantia de que todos os terminais executem a mesma versão


---

### Backup e Contingência

**Backup Local:**

- Execução periódica (diária/semanal)
- Proteção contra perda de dados
- Preparação para recuperação

**Verificação de Integridade:**

- Reconciliação entre dados locais e nuvem
- Identificação de discrepâncias
- Correção automática quando possível

---

### Relatórios de Shopping

**Geração GeraShoppingMais:**
- Exportação formatada para sistemas de shopping
- Dados de vendas para cálculo de aluguel
- Processamento automático em horários definidos