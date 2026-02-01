Mapa rápido de fluxos (para modelar checkout/pedidos/pagamentos)
Orders v2 (fluxo moderno de checkout)
Criar pedido: POST /v2/checkout/orders
Resultado típico: id + status (ex.: PAYER_ACTION_REQUIRED) + link para ação do pagador (payer-action/aprovação).
Consultar pedido: GET /v2/checkout/orders/{id}
Verifica estado e dados consolidados do pedido (o que o servidor “entendeu”).
Pagamento do pedido: dependendo da intenção (CAPTURE vs AUTHORIZE), o pedido evolui para captura (pagamento concluído) ou autorização (fundos reservados para capturar depois).
Pagamentos v2 (pós-checkout):
Detalhar captura: GET /v2/payments/captures/{capture_id}
Reembolsar captura: POST /v2/payments/captures/{capture_id}/refund (total com body vazio; parcial com amount)
Orders v1 / Payments v1 (legado)
Presente como referência/histórico; útil para entender variáveis, campos e padrões antigos (mas a própria doc sinaliza depreciações/migração).
Payment Links (NCPS / “no-code”)
CRUD do recurso: POST/GET/PUT/DELETE /v1/checkout/payment-resources
Fluxo mental: cria link → comprador paga numa página hospedada → comerciante consulta/acompanha transações (a doc cita notificações e acompanhamento em conta).
WPS (Website Payments Standard)
Modelo baseado em variáveis HTML (ex.: cmd, amount, return, cancel_return, notify_url) e redirecionamentos; útil para mapear riscos de integrações legadas.
NVP/SOAP
Integração legada e centrada em credenciais/certificados/assinaturas; o fluxo aqui é mais “plumbing” (autenticação, certificados, rotação/expiração) do que checkout moderno.
Checkpoints de teste (foco em falhas de lógica, sem “exploit passo-a-passo”)
1) Integridade de valor/moeda (o clássico do checkout)
Consistência de totais: 
i
t
e
m
_
t
o
t
a
l
+
s
h
i
p
p
i
n
g
+
t
a
x
+
f
e
e
s
=
t
o
t
a
l
item_total+shipping+tax+fees=total e arredondamentos.
Moeda: impedir misturas incoerentes (ex.: itens em USD e total “equivalente” divergente).
Campos duplicados/alternativos: quando há “breakdown” vs “total”, ver se o servidor privilegia sempre a fonte correta.
2) Máquina de estados (state machine) do pedido/pagamento
Transições inválidas: criar → aprovar → capturar/reembolsar deve respeitar status esperados.
Repetição de ações: capturar duas vezes, reembolsar acima do capturado, cancelar após concluído, etc.
Estados intermediários: lidar corretamente com PAYER_ACTION_REQUIRED, pendências, expiração.
3) Idempotência e replay (muito relevante em APIs de pagamento)
PayPal-Request-Id: garantir que reenvios do mesmo request id não causem duplicidade financeira.
Concorrência: duas requisições simultâneas para a mesma operação (captura/reembolso) devem ser serializadas/consistentes.
Preferências de resposta (Prefer: return=minimal/representation): confirmar que mudar “forma” da resposta não muda a lógica.
4) Autorização/escopo (quem pode fazer o quê)
Separação por “ator”: pagador vs comerciante vs parceiro (quando aplicável).
Acesso por ID: qualquer endpoint que recebe {id} é candidato a checagens de controle de acesso (sem assumir que é IDOR; validar limites corretamente).
PayPal-Auth-Assertion / contextos de partner (quando existirem): checar se o “comerciante alvo” é sempre o correto e não “vaza” para outro tenant.
5) Dados de risco/metadata e campos “auxiliares”
custom_id, invoice_id, reference_id: garantir unicidade/consistência e que não influenciem indevidamente reconciliação/estado.
Campos de “contexto de aplicação” (return/cancel, preferências de UX): devem afetar só UI/fluxo, não permitir desvio lógico.
6) Reembolsos e chargebacks (impacto financeiro direto)
Reembolso parcial vs total: validar limites, precisão, múltiplos reembolsos incrementais.
Motivo/nota: garantir que textos não afetem lógica (e que não gerem efeitos colaterais inesperados em logs/relatórios).
7) Integrações legadas (WPS / NVP/SOAP) — riscos comuns
Redirecionamentos de retorno/cancelamento: tratar como dados não confiáveis (evitar impactos além de UX).
Notificações/retornos assíncronos (ex.: notify_url/IPN no mundo WPS): foco em validação do evento, anti-replay, correlação com pedido real.
Certificados/assinaturas (NVP/SOAP): rotação, expiração, armazenamento seguro e permissões (evitar “quebrar” por erro operacional).
Modelo simples de “caso de teste” (para você ir executando e registrando)
Entrada: endpoint + headers relevantes + payload mínimo e payload “completo”.
Expectativa: estado antes/depois + invariantes (valores, moeda, permissões).
Observáveis: status, IDs correlatos (order_id, capture_id, refund_id), timestamps, links HATEOAS.
Critério de impacto: duplicidade financeira, manipulação de valor, bypass de estado, violação de tenant/conta.