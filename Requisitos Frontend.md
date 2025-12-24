# Requisitos Frontend - Logos System

## Observações Gerais

### Arquitetura
- **Logos Cashier** e **Logos POS** compartilham a mesma base de código
- Diferenças de layout são tratadas por responsividade e adaptação de componentes
- Todas as operações de dados são realizadas via REST API com o backend Logos
- Comunicação em tempo real via WebSocket para notificações e atualizações
- Cada terminal conecta diretamente às impressoras locais para execução de impressões

### Padrões de Interface
- **Modal**: Usado em telas maiores (desktop/tablet landscape)
- **Tela**: Usado em dispositivos menores (mobile/tablet portrait) ou quando a operação requer mais espaço
- Teclados virtuais aparecem automaticamente em telas touch

---

## Logos Cashier / POS

### 1. Login
Autenticação básica com identificação do operador.
- Campos: Login/ID e Senha
- Teclado virtual visível em telas maiores
- Validação e seleção de terminal

### 2. Home (Mapa de Vendas)
Tela principal com visualização organizada por tipo de venda.
- **Tabs**: Indefinido, Balcão (F1), Mesas (F2), Cartão (F3), Delivery (F4), Reservas
- Cada tab mostra vendas do tipo correspondente
- Cards de venda exibem: número de referência, quantidade de pessoas, status visual
- Ações rápidas: Abrir nova venda, Gerencial, Movimento da Casa, Mapa (Kanban/Agenda)

### 3. Gerencial
Modal/tela com acesso a funcionalidades administrativas do caixa.
- **Operações**: Movimento da Casa, Logs do Sistema, NFCe Contingência, Inutilização NFCe
- **Impressão**: Seleção de impressora, teste de impressão, ver locais de impressão
- **TEF**: Operações e testes de integração com terminal de pagamento

### 4. Abrir Venda
Modal para criação de nova venda.
- Seleção do tipo (Mesa/Balcão/Cartão/Delivery)
- Campos específicos por tipo:
  - **Mesa**: Número da mesa, quantidade de pessoas
  - **Cartão**: Número do cartão
  - **Delivery**: Dados do cliente, endereço
- Numpad integrado para entrada rápida

### 5. Resumo da Venda
Painel lateral (desktop) ou tela completa (mobile) com detalhes da venda atual.
- Informações: Identificação da venda, status, horário, operador
- Lista de produtos com: nome, quantidade, valor unitário, valor total, observações, status
- Ações por produto: Alterar quantidade, Editar observação, Remover, Transferir, Prioridade
- Ações gerais: Desconto na venda, Taxa de serviço, Covert, Entrega
- Botões: Enviar para Produção, Checkout, Transferir Itens, Dividir Conta, Gerenciar Consumidores

### 6. Catálogo de Produtos
Área de seleção de produtos para adicionar à venda.
- Categorias organizadas visualmente (cores, ícones)
- Grid de produtos com imagem, nome e preço
- Busca rápida por nome/código
- Indicador visual de quantidade de itens por pessoa (quando aplicável)
- Seleção de complementos e extras ao adicionar produto

### 7. Consumidor
Modal/tela para gestão de dados individuais na venda.
- Campos: Nome, CPF, Telefone, E-mail, Data de nascimento
- Posição na mesa (para mesas)
- Número do cartão (para cartões)
- Endereço completo (para delivery)
- Observações permanentes e da venda
- Opção de usar CPF na nota fiscal
- Botão para buscar cliente existente

### 8. Gerenciamento de Consumidores
Modal/tela para visualizar e editar todas as pessoas na venda.
- Lista com todos os consumidores vinculados
- Opção de adicionar, editar ou remover
- Alteração da quantidade total de pessoas (apenas mesas)
- Vincular produtos específicos a cada consumidor

### 9. Transferência
Modal/tela para mover itens entre vendas.
- Seleção da venda de destino (tipo e referência)
- Lista de produtos da venda atual
- Campo para definir quantidade a transferir
- Seleção do motivo da transferência
- Validação: destino deve estar aberto

### 10. Motivo
Modal compacto para seleção de motivo com base no tipo de operação.
- Lista de motivos cadastrados filtrados por tipo
- Campo de observação adicional (opcional)
- Usado em: cancelamentos, descontos, transferências

### 11. Motivo com Valor
Extensão do modal de motivo incluindo campos financeiros.
- Campos adicionais: Valor fixo ou Percentual
- Cálculo automático e exibição do valor resultante
- Usado em: descontos, cobranças extras

### 12. Prioridade de Impressão
Modal para alterar urgência de produção de itens.
- Seleção entre: Baixa, Normal, Alta
- Aplicável a um ou múltiplos produtos
- Reflete visualmente na cozinha/bar

### 13. Pagamento
Modal/tela para fechamento financeiro da venda.
- **Resumo**: Subtotal, descontos, taxas, covert, entrega, total a pagar
- **Formas de pagamento**: Lista visual de tipos disponíveis
- **Pagamentos adicionados**: Lista com valor, tipo, parcelas, ações (remover)
- Campo CPF/CNPJ no cupom fiscal
- Botões: Reabrir Consumo, Pré-conta, Ver Processo Fiscal, Finalizar Venda

### 14. Adicionar Pagamento
Modal/tela em etapas para registro de pagamento.
- **Etapa 1**: Seleção do tipo (Dinheiro, Débito, Crédito, PIX, Voucher)
- **Etapa 2**: Confirmação com campos específicos:
  - Valor (sugestão do saldo restante)
  - Parcelas (quando aplicável)
  - Bandeira do cartão
  - Dados de autorização (NSU, código)

### 15. Buscar Mesa/Cartão
Modal para localizar venda existente por cartão.
- Campo de entrada com numpad
- Indicação da mesa/cartão atual
- Lista de resultados (se múltiplas vendas)
- Botão para ir para a venda encontrada

### 16. Processo Fiscal
Modal/tela para acompanhamento e gestão de documentos fiscais.
- Informações da NFCe/SAT: Chave, protocolo, status
- Eventos da SEFAZ com timestamps
- Erros de emissão (se houver)
- Opção de reimpressão do documento
- Cancelamento de nota (com motivo)

### 17. Movimentação
Tela para consulta e gestão de vendas realizadas.
- **Filtros**: Período, tipo de venda, status, operador, referência
- **Lista de vendas** com colunas:
  - ID, Tipo, Referência, Data/Hora, Consumo, Pagamento, Status, Documento Fiscal
- Ações por venda: Visualizar, Cancelar (quando permitido), Reemitir nota
- Estatísticas do período: Total de vendas, ticket médio, faturamento

### 18. Visualização de Venda
Modal/tela somente leitura para consulta de venda finalizada.
- Todas as informações da venda
- Linha do tempo de eventos (abertura, itens adicionados, produção, pagamentos, fechamento)
- Produtos com detalhes completos
- Consumidores vinculados
- Pagamentos realizados
- Documento fiscal emitido

### 19. Movimento da Casa
Tela de controle de turnos e fechamentos.
- **Lista de operadores** com colunas:
  - Nome, Status (Aberto/Encerrado/Bloqueado/Vencido), Data/Hora início, Tempo, Vendas em andamento, Vendas encerradas
- Ações por operador: Abrir movimento, Fechar caixa, Ver detalhes
- **Configuração**: Horário de virada do dia
- Status geral da casa: Aberta, Fechada para novos pedidos

### 20. Fechamento de Caixa
Tela para conferência e fechamento do turno do operador.
- **Valores declarados** por forma de pagamento (campos editáveis)
- **Valores do sistema** (automático do banco)
- **Diferenças** calculadas automaticamente
- Seleção das vendas a serem incluídas no fechamento
- Conferência cega (opcional - não mostra valores do sistema)
- Geração de relatório de fechamento
- Suprimentos e sangrias do período

### 21. Logs do Sistema
Janela para rastreamento e debug.
- **Filtros**: Data/hora, tipo de log, módulo
- Lista em tempo real de eventos
- Níveis: Info, Warning, Error
- Busca por texto
- Exportação de logs

### 22. Inutilização de NFCe
Tela para gerenciar numeração de notas fiscais.
- Campos: Série, número inicial, número final, motivo
- Lista de números com status:
  - Emitida (com venda vinculada)
  - Cancelada
  - Inutilizada
  - Sem operação
- Executar inutilização (envia para SEFAZ)

### 23. Locais de Impressão
Modal/tela de configuração de roteamento de produção.
- Lista de produtos cadastrados
- Praça de produção associada a cada produto
- Opção de alterar praça (arrasta e solta ou seleção)
- Agrupamento por categoria

### 24. Mapa da Casa - Kanban
Tela para gestão visual de deliveries e integrações.
- Colunas de status: Aberta, Em Produção, Pronto, Saiu para Entrega, Encerrada, Cancelada
- Cards de venda com: origem (iFood, Goomer, etc), horário, itens, valor
- Arrastar entre colunas para mudar status
- Ações rápidas: Ver detalhes, Ligar para cliente, Cancelar

### 25. Mapa da Casa - Agenda
Calendário horizontal para visualização de reservas e agendamentos.
- Visualização semanal por padrão
- Cada dia exibe vendas agendadas com: horário, nome, mesa, pessoas
- Indicadores visuais de status (confirmado, chegou, atrasado)
- Criar nova reserva
- Editar/cancelar reserva existente

### 26. Reservas
Tela completa para gerenciamento de reservas.
- **Formulário**: Telefone, E-mail, Nome, Data/Hora, Mesa, Pessoas, Observações
- **Lista de reservas** com filtros
- Status: Pendente, Confirmada, Cliente chegou, Cancelada
- Ações: Confirmar chegada (abre venda na mesa), Editar, Cancelar

### 27. Integrações
Tela para monitoramento de pedidos de plataformas externas.
- **Status das integrações**: iFood, Goomer, Keeta, Anota AI, etc. (online/offline)
- **Lista de pedidos** recebidos com: origem, horário, status
- Visualizar detalhes do JSON do pedido
- Histórico de eventos da integração
- **Tabs específicas**: EuFalo (cashback), Abrahão (autoatendimento)
- Ações: Aceitar pedido, Recusar (com motivo), Ver histórico

### 28. Detalhamento de Pedido Integrado
Modal/tela para visualização completa de pedido de delivery.
- JSON formatado do pedido original
- Dados do cliente
- Itens com complementos e observações
- Valores (subtotal, taxas, desconto, total)
- Histórico de eventos (recebido, aceito, produção, despachado)
- Ações disponíveis conforme status

### 29. Seleção de Impressora
Modal para escolha pontual de impressora.
- Lista de impressoras configuradas no terminal
- Filtro por tipo (fiscal, produção, geral)
- Status de cada impressora (online/offline/erro)
- Usado em: impressões manuais, testes, reimpressões

### 30. Configurações do Cashier/POS
Tela de preferências locais do terminal.
- Impressora padrão por tipo de operação
- Testes de impressão e conexão
- Layout da interface (tema, tamanho de fonte)
- Atalhos de teclado personalizados
- Parâmetros de comportamento (confirmações, timeouts)

---

## Logos Admin

### 1. Login
Autenticação administrativa com senha padrão do sistema.
- Campo de senha
- Validação de permissões

### 2. Dashboard
Tela principal com visão geral do sistema.
- **Status do Logos**: Versão, uptime, CPU, memória
- **Sincronização**: Última sync, próxima sync, status (ok/erro)
- **Banco de Dados**: Status da conexão, tamanho
- **Terminais conectados**: Lista de Cashiers/POS conectados via socket
- **Ações rápidas**:
  - Sincronizar agora (download/upload)
  - Status SEFAZ
  - Testes de impressão
  - Ver logs
  - Configurar sistema

### 3. Configuração do Banco de Dados
Tela para setup da conexão com MySQL.
- Campos: Host/IP, Porta, Usuário, Senha, Database
- Botão de testar conexão
- Salvar configuração
- Validação de acesso

### 4. Configuração do Sistema
Tela de parâmetros gerais do Logos.
- **Estabelecimento**: CNPJ, razão social, terminal
- **Dispositivos**:
  - Impressora padrão para documentos fiscais
  - Impressora para pré-conta e fechamento
  - Impressora padrão para produção
  - Impressora padrão para delivery
  - SAT padrão para autorização fiscal
- **Balança**: Porta e velocidade
- **Inutilização NFCe**: Série, número
- **Diretório base**: Caminho de instalação
- Botão salvar

### 5. Parâmetros
Tela com lista de configurações de comportamento.
- Tabela com: Nome do parâmetro, Descrição, Valor
- Edição inline de valores
- Busca por nome
- Parâmetros incluem impressões, regras de negócio, integrações
- **Observação**: Parâmetros marcados com (#) afetam apenas o terminal local

### 6. Sincronização Retaguarda
Interface de controle de dados com a nuvem.
- **Ações disponíveis**:
  - Baixar tudo (produtos, usuários, configurações, etc)
  - Baixar específico (seleção)
  - Enviar vendas
  - Enviar histórico de vendas
  - Enviar fechamentos
  - Verificar licença
  - Atualizar estrutura do banco
  - Backup local
  - Forçar reconstrução de produtos
  - Checar nova versão do programa
- Logs de cada operação com data/hora e status

### 7. Fila de Tarefas
Tela para monitoramento de jobs pendentes.
- **Filtros**: Data, tipo (impressão/fiscal/outros), status (processado/não coletado/com erro)
- **Tabela**: ID, Venda, Tipo, Referência, Evento, Dispositivo, Inserido, Coletado, Finalizado, Tentativas
- Ações: Apagar job, Reprocessar
- Atualização automática

### 8. Terminais (NextHubs)
Tela de gerenciamento dos terminais conectados.
- **Lista de hubs** com: ID, Terminal, Funções, Máquina, Porta, Data/Hora início, Log, Status
- **Detalhes do hub selecionado**:
  - ID, Terminal, Data/Hora, Máquina, Porta
  - Checkboxes de funções: Fila de tarefas, Sincronia retaguarda, Sincronia pagamentos, Sincronia Food
  - Impressoras gerenciadas por este hub
  - SATs gerenciados por este hub
- Acompanhar logs em tempo real
- Botão atualizar

### 9. Logs do Logos
Janela dedicada aos logs do backend.
- Visão mais completa que a do Cashier
- Filtros avançados por módulo, nível, período
- Busca textual
- Exportação
- Limpeza de logs antigos

### 10. Teste de Integrações
Tela para validar conexões e enviar comandos.
- **SPIX**: Teste de comunicação arcade/entretenimento
- **SEFAZ**: Status do serviço, última consulta
- Testes de impressão por dispositivo
- Teste de envio de produção
- Validação de certificado digital

---

## Funcionalidades Adicionais Identificadas

### Socket/Real-time
- Notificações de novas vendas (delivery/integração)
- Atualização de status de vendas em tempo real
- Alertas de impressão (quando terminal precisa buscar jobs)
- Sincronização de fechamento de movimento
- Alertas de sistema (erros, avisos)

### Impressões
- Cada terminal conecta diretamente à impressora
- Terminal busca jobs de impressão via REST quando recebe notificação socket
- Possibilidade de solicitar impressão para outro terminal específico

### Gestão de Estoque (Futura)
- Não detalhado neste documento, mas mencionado como integração possível

### Multi-loja/Multi-estabelecimento
- Admin pode gerenciar configurações de múltiplos estabelecimentos
- Identificação pelo CNPJ no setup inicial

---

## Observações Técnicas

### Responsividade
- Layouts adaptáveis: desktop (1920x1080), tablet (1024x768), mobile (768x1024)
- Touch-first em todos os dispositivos
- Teclados virtuais em telas touch

### Performance
- Lazy loading de imagens de produtos
- Virtualização de listas longas
- Cache local de dados frequentes
- Debounce em buscas

### Acessibilidade
- Atalhos de teclado (F1-F9, Ctrl+combinações)
- Indicações visuais claras de status
- Feedback sonoro opcional

### Offline-first
- Funcionamento mesmo sem conexão com retaguarda
- Sincronização automática quando conexão restabelecida
- Alertas de status de conectividade