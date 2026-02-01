Criar pedido
publicar
/v1/finalizar compra/pedidos
Experimente
Cria um pedido.

Segurança
OAuth2
Solicitar
Parâmetros do cabeçalho
ID de atribuição de parceiro do PayPal
corda
Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
obrigatório
intenção
corda
A intenção.

Valor de enumeração	Descrição
OFERTA	Uma venda.
AUTORIZAR	Uma autorização.

unidades_de_compra
obrigatório
Matriz de objetos não vazia
Um conjunto de unidades de compra. Cada unidade de compra estabelece um contrato entre um cliente e um comerciante.


detalhes_de_pagamento
objeto
Detalhes do pagamento do pedido.


contexto_de_aplicação
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento com o PayPal.

Observação: a Plataforma de Comércio do PayPal pode configurar brand_nameparâmetros shipping_preferencedurante a configuração da conta do parceiro, que substituem os valores da solicitação.

redirecionar_urls
obrigatório
objeto
URLs de redirecionamento. Necessário apenas para o método de pagamento PayPal. As configurações suportadas são URLs de retorno e cancelamento.


valor_total_bruto
objeto
A moeda e o valor total calculado pelo PayPal para todas as unidades de compra.


metadados
objeto
Os pares nome-valor que contêm dados externos, como usuário, feedback do usuário, pontuação e assim por diante.

Respostas
200Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP e um corpo de resposta JSON que inclui o ID do pedido gerado pelo PayPal, uma matriz de objetos de unidade de compra, detalhes do pagamento, informações do cliente, metadados e status do pedido.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v1/checkout/orders \ 
 \ 
-d  '{
  "contexto_de_aplicação": {
    "locale": "en-US",
    "marca": "walmart",
    "landing_page": "faturamento",
    "user_action": "continuar",
    "dados_suplementares": [
      {
        "nome": "id_de_correlação_de_risco",
        "valor": "9N8554567F903282T"
      },
      {
        "nome": "endereço_ip_do_comprador",
        "valor": "109.20.212.116"
      },
      {
        "nome": "canal_externo",
        "valor": "WEB"
      }
    ]
  },
  "purchase_units": [
    {
      "reference_id": "store_mobile_world_order_1234",
      "descrição": "Pedido da Loja Mobile World - 1234",
      "quantia": {
        "moeda": "USD",
        "detalhes": {
          "subtotal": "1,09"
          "frete": "0,02",
          "imposto": "0,33"
        },
        "total": "1,44"
      },
      "beneficiário": {
        "e-mail": "seller@example.com"
      },
      "Unid": [
        {
          "nome": "NeoPhone",
          "sku": "sku03",
          "preço": "0,54",
          "moeda": "USD",
          "quantidade": "1"
        },
        {
          "nome": "Relógio Fitness",
          "sku": "sku04",
          "preço": "0,55"
          "moeda": "USD",
          "quantidade": "1"
        }
      ],
      "Endereço para envio": {
        "linha 1": "2211 N First Street",
        "linha2": "Edifício 17",
        "cidade": "San José",
        "código_do_país": "EUA",
        "código postal": "95131",
        "estado": "CA",
        "telefone": "(123) 456-7890"
      },
      "Método de envio": "Serviço Postal dos Estados Unidos"
      "partner_fee_details": {
        "receptor": {
          "e-mail": "partner@example.com"
        },
        "quantia": {
          "valor": "0,01",
          "moeda": "USD"
        }
      },
      "grupo_vinculado_ao_pagamento": 1,
      "custom": "custom_value_2388",
      "número_da_fatura": "número_da_fatura_2388",
      "descritor_de_pagamento": "Pagamento Mundo Móvel"
    }
  ],
  "redirect_urls": {
    "return_url": "https://example.com/return",
    "cancel_url": "https://example.com/cancel"
  }
}' 
Amostras de resposta
200
aplicativo/json

Exemplo 1 - 200 - Criar pedido com contexto de aplicação
Exemplo 1 - 200 - Criar pedido com contexto de aplicação
CópiaExpandir tudoRecolher tudo
{
"id": "8RU61172JS455403V",
"gross_total_amount": {
"value": "1.44",
"currency": "USD"
},
"application_context": {
"locale": "en-US",
"brand_name": "walmart",
"landing_page": "billing",
"user_action": "continue",
"supplementary_data": [
{
"name": "risk_correlation_id",
"value": "9N8554567F903282T"
},
{
"name": "buyer_ipaddress",
"value": "109.20.212.116"
},
{
"name": "external_channel",
"value": "WEB"
}
]
},
"purchase_units": [
{
"reference_id": "store_mobile_world_order_1234",
"description": "Mobile World Store order-1234",
"amount": {
"currency": "USD",
"details": {
"subtotal": "1.09",
"shipping": "0.02",
"tax": "0.33"
},
"total": "1.44"
},
"payee": {
"email": "[email protected]"
},
"items": [
{
"name": "NeoPhone",
"sku": "sku03",
"price": "0.54",
"currency": "USD",
"quantity": "1"
},
{
"name": "Fitness Watch",
"sku": "sku04",
"price": "0.55",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "John Doe",
"default_address": false,
"preferred_address": false,
"primary_address": false,
"disable_for_transaction": false,
"line1": "2211 N First Street",
"line2": "Building 17",
"city": "San Jose",
"country_code": "US",
"postal_code": "95131",
"state": "CA",
"phone": "(123) 456-7890"
},
"shipping_method": "United Postal Service",
"partner_fee_details": {
"receiver": {
"email": "[email protected]"
},
"amount": {
"value": "0.01",
"currency": "USD"
}
},
"payment_linked_group": 1,
"custom": "custom_value_2388",
"invoice_number": "invoice_number_2388",
"payment_descriptor": "Payment Mobile World",
"status": "CAPTURED"
}
],
"redirect_urls": {
"return_url": "https://example.com/return",
"cancel_url": "https://example.com/cancel"
},
"create_time": "2017-04-26T21:18:49Z",
"links": [
{
"href": "https://api-m.paypal.com/v1/checkout/orders/8RU61172JS455403V",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/webapps/hermes?token=8RU61172JS455403V",
"rel": "approval_url",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v1/checkout/orders/8RU61172JS455403V",
"rel": "cancel",
"method": "DELETE"
}
],
"status": "CREATED"
}
Mostrar detalhes do pedido
pegar
/v1/finalizar compra/pedidos/{id_do_pedido}
Experimente
Exibe os detalhes de um pedido, por ID.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
id_do_pedido
obrigatório
corda
O ID do pedido para o qual deseja exibir os detalhes.

Respostas
200Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP e um corpo de resposta JSON que mostra os detalhes do pedido.
Solicitar amostras
cURL
Cópia
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/checkout/orders/8SC68793353299025 \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Amostras de resposta
200
aplicativo/json

Exemplo 1 - 200 - Mostrar detalhes do pedido - 200 OK Criado
Exemplo 1 - 200 - Mostrar detalhes do pedido - 200 OK Criado
CópiaExpandir tudoRecolher tudo
{
"id": "8SC68793353299025",
"status": "CREATED",
"gross_total_amount": {
"value": "1.44",
"currency": "USD"
},
"application_context": { },
"purchase_units": [
{
"reference_id": "store_mobile_world_order_1234",
"description": "Mobile World Store order-1234",
"amount": {
"currency": "USD",
"details": {
"subtotal": "1.09",
"shipping": "0.02",
"tax": "0.33"
},
"total": "1.44"
},
"payee": {
"email": "[email protected]"
},
"items": [
{
"name": "NeoPhone",
"sku": "sku03",
"price": "0.54",
"currency": "USD",
"quantity": "1"
},
{
"name": "Fitness Watch",
"sku": "sku04",
"price": "0.55",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"line1": "2211 N First Street",
"line2": "Building 17",
"city": "San Jose",
"country_code": "US",
"postal_code": "95131",
"state": "CA",
"phone": "(123) 456-7890"
},
"shipping_method": "United Postal Service",
"partner_fee_details": {
"receiver": {
"email": "[email protected]"
},
"amount": {
"value": "0.01",
"currency": "USD"
}
},
"payment_linked_group": 1,
"custom": "custom_value_2388",
"invoice_number": "invoice_number_2388",
"payment_descriptor": "Payment Mobile World",
"status": "NOT_PROCESSED"
}
],
"redirect_urls": {
"return_url": "https://example.com/return",
"cancel_url": "https://example.com/cancel"
},
"links": [
{
"href": "https://api-m.paypal.com/v1/checkout/orders/8SC68793353299025",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.paypal.com/checkoutnow?token=8SC68793353299025",
"rel": "approval_url",
"method": "POST"
}
],
"create_time": "2018-09-21T17:22:45Z"
}
Cancelar pedido
excluir
/v1/finalizar compra/pedidos/{id_do_pedido}
Experimente
Cancela um pedido, por ID, e exclui o pedido. Para chamar este método, o status do pedido deve ser CREATEDou APPROVED.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
id_do_pedido
obrigatório
corda
O ID do pedido a ser cancelado.

Respostas
204Uma solicitação bem-sucedida retorna o 204 No Contentcódigo de status HTTP sem corpo de resposta JSON. Se o pedido já foi pago, ele não pode ser cancelado e a solicitação retorna o 422 Unprocessable Entitycódigo de status HTTP com a mensagem This order is in progress.
Solicitar amostras
cURL
Cópia
curl  -v  -X DELETE https://api-m.sandbox.paypal.com/v1/checkout/orders/8SC68793353299025 \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Amostras de resposta
204
aplicativo/json

Exemplo 1 - 204 - Cancelar Pedido - 204 Sem Conteúdo
Exemplo 1 - 204 - Cancelar Pedido - 204 Sem Conteúdo
Cópia
{ }
Pagar pelo pedido
publicar
/v1/finalizar compra/pedidos/{id_do_pedido}/pagamento
Experimente
Inicia um pagamento do PayPal que foi aprovado pelo comprador.

Observação: Para casos de uso de parceiros, use o parâmetro disbursement_modepara indicar se os fundos devem ser liberados para as contas do vendedor e do parceiro imediatamente ou posteriormente. Se você adiar a liberação, deverá chamar o método de liberação de fundos para liberar os fundos para o comerciante e o parceiro.
Segurança
OAuth2
Solicitar
Parâmetros do caminho
id_do_pedido
obrigatório
corda
O ID da ordem para a qual o pagamento deve ser efetuado.

Parâmetros do cabeçalho
ID de atribuição de parceiro do PayPal
corda
ID da solicitação do PayPal
corda
O servidor armazena as chaves permanentemente.

Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
obrigatório
modo_de_desembolso
obrigatório
corda
Indica se o dinheiro deve ser liberado imediatamente ou posteriormente.

Valor de enumeração	Descrição
INSTANT	O dinheiro é liberado instantaneamente.
ATRASADO	O dinheiro será liberado posteriormente.

pagador
objeto
A origem dos fundos para este pagamento. Pode ser uma conta PayPal ou um cartão de crédito.

Respostas
200Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP e um corpo de resposta JSON que mostra os detalhes do pedido e do pagamento.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v1/checkout/orders/8SC68793353299025/pay \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{}' 
Amostras de resposta
200
aplicativo/json

Exemplo 1 - 200 - Ordem de Pagamento - 200 OK
Exemplo 1 - 200 - Ordem de Pagamento - 200 OK
CópiaExpandir tudoRecolher tudo
{
"order_id": "76T08342AW176740Y",
"status": "APPROVED",
"payer_info": {
"email": "[email protected]",
"first_name": "Robert",
"last_name": "Lesley",
"payer_id": "HPQAUP5WC3J8U",
"phone": "7028530329",
"country_code": "US",
"shipping_address": {
"recipient_name": "Robert Lesley",
"line1": "2211 N First Street",
"line2": "Building 17",
"city": "San Jose",
"country_code": "US",
"postal_code": "95131",
"state": "CA"
}
},
"create_time": "2018-09-21T17:33:18Z",
"update_time": "2018-09-21T17:33:18Z",
"links": [
{
"href": "https://api-m.paypal.com/v1/checkout/orders/76T08342AW176740Y",
"rel": "self",
"method": "GET"
}
]
}
Erros
CONTA_NÃO_PODE_SER_RECUPERADA
Mensagem:
Não foi possível recuperar a conta.

Descrição:
O servidor não consegue obter as informações da conta do parceiro ou vendedor. A conta não está disponível ou está incorreta. A API retorna esse erro para contas de vendedores quando você não habilita o onboarding após os compradores realizarem compras para o parceiro associado.

ERRO DO SERVIDOR INTERNO
Mensagem:
Ocorreu um erro interno do servidor.

Descrição:
Ocorreu um erro de sistema ou de aplicação. Embora o cliente pareça ter enviado uma solicitação correta, algo inesperado aconteceu no servidor. A 500resposta indica uma falha de software no servidor ou uma indisponibilidade do site.

SOLICITAÇÃO_INVÁLIDA
Mensagem:
A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.

Descrição:
O servidor não entendeu a solicitação porque a API não conseguiu converter os dados da carga útil para o tipo de dados subjacente, os dados não estavam no formato esperado, o campo obrigatório não estava disponível ou ocorreu um erro simples de validação de dados. Consulte Erros de validação .

NÃO AUTORIZADO
Mensagem:
A autorização falhou devido a permissões insuficientes.

Descrição:
O cliente não está autorizado a acessar este recurso, embora possa ter credenciais válidas. Por exemplo, o cliente não possui o escopo OAuth2 correto. Além disso, pode ter ocorrido um erro de autorização em nível de negócios. Por exemplo, o titular da conta não possui fundos suficientes.

PAGAMENTO NÃO APROVADO PARA EXECUÇÃO
Mensagem:
O pagador não aprovou o pagamento.

Descrição:
A API criou o pedido. No entanto, o cliente não aprovou o pagamento.

RECURSO NÃO ENCONTRADO
Mensagem:
O recurso especificado não existe.

Descrição:
O servidor não encontrou nada que corresponda ao URI da solicitação. O URI pode estar incorreto ou o recurso pode não estar disponível. Por exemplo, não existem dados no banco de dados com essa chave.

NÃO AUTORIZADO
Mensagem:
A autenticação falhou devido a credenciais de autenticação inválidas.

Descrição:
A solicitação requer autenticação e o solicitante não forneceu credenciais de autenticação válidas. Observe a diferença entre este erro e o NOT_AUTHORIZEDerro descrito anteriormente. Consulte Erros de autenticação .

ERRO_DE_VALIDAÇÃO
Mensagem:
Campo obrigatório ausente.

Descrição:
O servidor não entendeu a solicitação porque a API não conseguiu converter os dados da carga útil para o tipo de dados subjacente, os dados não estavam no formato esperado, o campo obrigatório não estava disponível ou ocorreu um erro simples de validação de dados. Consulte Erros de validação .

Definições
Endereço
O endereço de cobrança ou de entrega para o pagamento.

linha1
obrigatório
corda
A primeira linha do endereço. Por exemplo, número, rua, etc.

linha2
corda
A segunda linha do endereço. Por exemplo, o número do apartamento ou da sala.

cidade
corda
O nome da cidade.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código ISO 3166-1 de dois caracteres que identifica o país ou região.

Nota: O código de país para a Grã-Bretanha é 0gg GBe não UKo utilizado nos domínios de nível superior desse país. Utilize o C2código de país para a China em todo o mundo para o método de preços comparáveis ​​não controlados (CUP), cartões bancários e transações internacionais.
código postal
corda
O código postal, que é o CEP ou equivalente. Normalmente exigido para países que possuem código postal ou equivalente. Veja código postal .

estado
corda
O código de um estado dos EUA ou o equivalente em outros países. Obrigatório para transações se o endereço for em um destes países: Argentina , Brasil , Canadá , Índia , Itália , Japão , México , Tailândia ou Estados Unidos . O comprimento máximo é de 40 caracteres de um byte.

telefone
corda
O número de telefone, no formato E.123 . O comprimento máximo é de 50 caracteres.

status_de_normalização
corda
Status da normalização de endereço. Retornado apenas para pagadores do Brasil.

Enum : 
"DESCONHECIDO"
 
"PREFERÊNCIA DO USUÁRIO NÃO NORMALIZADO"
 
"NORMALIZADO"
 
"NÃO NORMALIZADO"
tipo
corda
O tipo de endereço. Por exemplo, HOME_OR_WORK, GIFT, e assim por diante.

Cópia
{
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
}
Quantia
O valor do pagamento, com detalhes.

moeda
obrigatório
corda
O código de moeda ISO-4217 de três caracteres . O PayPal não aceita todas as moedas.

total
obrigatório
corda
O valor total cobrado ao beneficiário pelo pagador. Para reembolsos, representa o valor que o beneficiário devolve ao pagador original. O comprimento máximo é de 10 caracteres, incluindo:

Sete dígitos antes da vírgula decimal.
O ponto decimal.
Dois dígitos após a vírgula decimal.

detalhes
objeto
Detalhes adicionais sobre o valor do pagamento.

CópiaExpandir tudoRecolher tudo
{
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
}
Contexto da aplicação
Personaliza a experiência do pagador durante o processo de aprovação do pagamento com o PayPal.

Observação: a Plataforma de Comércio do PayPal pode configurar brand_nameparâmetros shipping_preferencedurante a configuração da conta do parceiro, que substituem os valores da solicitação.
nome_da_marca
string <= 127 caracteres
O rótulo que substitui o nome da empresa na conta do PayPal nas páginas do PayPal.

localidade
string [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}))...( linguagem ) Mostrar padrão
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 .

preferência de envio
corda
As preferências de envio.

Valor de enumeração	Descrição
SEM ENVIO	Oculte os campos de endereço de entrega das páginas do PayPal. Recomendado para produtos digitais.
OBTER_DO_ARQUIVO	Utilize o endereço de entrega selecionado pelo comprador.
DEFINIR_ENDEREÇO_FORNECIDO	Utilize o endereço fornecido pelo vendedor. O comprador não pode alterar o endereço nas páginas do PayPal. Caso o vendedor não forneça um endereço, o comprador poderá escolher o endereço nas páginas do PayPal.
ação_do_usuário
corda
Define se o cliente deve ser apresentado com o fluxo de finalização de compra " Continuar" ou "Pagar agora" . Para apresentar aos compradores o fluxo de finalização de compra "Pagar agora" , defina como useraction=commit. O padrão é o fluxo de finalização de compra " Continuar" .

Fluxo de finalização da compra	Escolha quando	Descrição
Continuar	Você não sabe o valor final do pagamento ao iniciar o processo de finalização da compra.	Fluxo padrão. Redireciona o cliente para a página de pagamento do PayPal, que exibe o botão Continuar . Ao clicar em Continuar , o cliente pode alterar o valor do pagamento.
Pagar agora	Você fica sabendo o valor final do pagamento ao iniciar o processo de finalização da compra.	Configurar user_action=commit. Redireciona o cliente para a página de pagamento do PayPal, que exibe o botão Pagar agora . Quando o cliente clica em Pagar agora , o pagamento é processado imediatamente.

dados_suplementares
Conjunto de objetos
Uma matriz de pares nome-valor que contém dados suplementares exigidos pelo PayPal para o processamento de transações.

CópiaExpandir tudoRecolher tudo
{
"brand_name": "string",
"locale": "string",
"shipping_preference": "NO_SHIPPING",
"user_action": "string",
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
}
Capturar
Uma transação de captura.

eu ia
corda
O ID da transação de captura.

status
corda
O status da transação de captura.

Valor de enumeração	Descrição
PENDENTE	O status da unidade de compra é [status] CAPTUREDe o status de captura é [status] pendente.
CONCLUÍDO	O status da unidade de compra é [status] CAPTUREDe o pagamento foi registrado com sucesso.
REEMBOLSADO	O status da unidade de compra é [informação ausente] CAPTUREDe a decisão da disputa foi favorável ao cliente. O reembolso integral do pagamento efetuado foi realizado com sucesso para o cliente.
REEMBOLSADO PARCIALMENTE	O status da unidade de compra é [informação ausente] CAPTUREDe a decisão da disputa foi favorável ao cliente. Um reembolso parcial do pagamento efetuado foi realizado com sucesso para o cliente.
NEGADO	O status da unidade de compra é [status] CAPTUREDe a captura foi negada.
código_motivo
corda
Um código de motivo que indica a razão para o estado da transação de PENDINGou REVERSED.

Valor de enumeração	Descrição
ESTORNO	O estado da transação é PENDINGou REVERSEDdevido a um estorno.
GARANTIA	O estado da transação é PENDINGou REVERSEDdevido a uma garantia.
RECLAMAÇÃO DO COMPRADOR	O estado da transação é PENDINGou REVERSEDdevido a uma reclamação do comprador.
REEMBOLSO	O estado da transação é PENDINGou REVERSEDdevido a um reembolso.
ENDEREÇO ​​DE ENTREGA NÃO CONFIRMADO	O estado da transação é PENDING"ou" REVERSEDdevido a um endereço de entrega não confirmado.
Cheque eletrônico	O estado da transação é PENDING"ou REVERSEDdevido a um cheque eletrônico".
RETIRADA INTERNACIONAL	O estado da transação é PENDINGou REVERSEDdevido a uma retirada internacional.
RECEBIMENTO DE MANDATOS DE PREFERÊNCIA - AÇÃO MANUAL	O estado da transação é PENDING"ou" REVERSEDdevido a uma preferência de recebimento que exige ação manual.
REVISÃO DE PAGAMENTO	O status da transação é " em análise" PENDINGou " REVERSEDaguardando revisão de pagamento".
REVISÃO REGULATÓRIA	O estado da transação é PENDINGou REVERSEDestá em análise regulatória.
UNILATERAL	O estado da transação é PENDINGou REVERSEDdevido a um motivo unilateral.
VERIFICAÇÃO NECESSÁRIA	O estado da transação é PENDING"ou" REVERSEDporque é necessária verificação.
DESEMBOLSO_ATRASADO	O estado da transação é PENDINGou REVERSEDdevido a um desembolso atrasado.

links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .


quantia
objeto
O valor a ser capturado. O padrão é o valor autorizado. Se esse valor for igual ao valor autorizado, o estado de autorização muda para CAPTURED. Caso contrário, o estado de autorização muda para PARTIALLY_CAPTURED. Para indicar que esta é a captura final, defina is_final_capturecomo true.


taxa_de_transação
objeto
A moeda e o valor da taxa de transação.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"status": "PENDING",
"reason_code": "CHARGEBACK",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
código_do_país
O código ISO 3166-1 de dois caracteres que identifica o país ou região.

Nota: O código de país para a Grã-Bretanha é 0gg GBe não UKo utilizado nos domínios de nível superior desse país. Utilize o C2código de país para a China em todo o mundo para o método de preços comparáveis ​​não controlados (CUP), cartões bancários e transações internacionais.
string ( country_code ) = 2 caracteres ^([AZ]{2}|C2)$ < ppaas_common_country_code_v2 >
O código ISO 3166-1 de dois caracteres que identifica o país ou região.

Nota: O código de país para a Grã-Bretanha é 0gg GBe não UKo utilizado nos domínios de nível superior desse país. Utilize o C2código de país para a China em todo o mundo para o método de preços comparáveis ​​não controlados (CUP), cartões bancários e transações internacionais.
Cópia
"st"
Cartão de crédito
Obsoleto. Dados do cartão de crédito. Você pode usar este instrumento para efetuar um pagamento. Use um cartão de pagamento em vez disso.

número
obrigatório
corda
Número do cartão de crédito. O valor deve conter apenas caracteres numéricos, sem espaços ou pontuação. Deve estar em conformidade com o módulo e o comprimento exigidos por cada tipo de cartão de crédito. (Informação omitida nas respostas.)

tipo
obrigatório
corda
Tipo de cartão de crédito. O valor é visa, mastercard, discover, ou amex. Não utilize esses valores em minúsculas para exibição.

mês de expiração
obrigatório
inteiro
O mês de vencimento sem zero à esquerda. O valor é de 1a 12.

ano_de_expiração
obrigatório
inteiro
O ano de validade de quatro dígitos.

cvv2
corda
O código de validação do cartão, que geralmente tem entre três e quatro dígitos.

primeiro nome
corda
O primeiro nome do titular do cartão.

sobrenome
corda
Sobrenome do titular do cartão.


links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .


Endereço de Cobrança
objeto
O endereço de cobrança ou de entrega para o pagamento.

CópiaExpandir tudoRecolher tudo
{
"number": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0,
"cvv2": "string",
"first_name": "string",
"last_name": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
}
}
Token de cartão de crédito
Os dados do cartão de crédito tokenizados. Você pode usar esse instrumento para efetuar um pagamento.

id_do_cartão_de_crédito
obrigatório
corda
O número de identificação do cartão de crédito que está armazenado no cofre do PayPal.

id_do_pagador
corda
Um ID exclusivo que você pode atribuir e rastrear ao armazenar um cartão de crédito no cofre ou ao usar um cartão de crédito armazenado no cofre. Esse ID pode ajudar a evitar o uso indevido ou não intencional de cartões de crédito e pode ser qualquer valor, como um UUID, nome de usuário ou endereço de e-mail. É necessário ao usar um cartão de crédito armazenado no cofre e se um ID payer_idfoi fornecido originalmente quando você armazenou o cartão de crédito no cofre.

últimos4
corda
Os últimos quatro dígitos do número do cartão de crédito armazenado.

tipo
corda
Tipo de cartão de crédito. O valor é visa, mastercard, discover, ou amex. Não utilize esses valores em minúsculas para exibição.

mês de expiração
inteiro
O mês de vencimento sem zero à esquerda. O valor é de 1a 12.

ano_de_expiração
inteiro
O ano de validade de quatro dígitos.

Cópia
{
"credit_card_id": "string",
"payer_id": "string",
"last4": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0
}
Moeda
A moeda e o valor da transação.

moeda
obrigatório
corda
O código de moeda ISO-4217 de três caracteres . O PayPal não aceita todas as moedas.

valor
obrigatório
corda
O valor. Inclui o número especificado de dígitos após o separador decimal para o código de moeda ISO-4217 .

Cópia
{
"currency": "string",
"value": "string"
}
código_da_moeda
O código de moeda ISO-4217 de três caracteres que identifica a moeda.

string ( código_da_moeda ) = 3 caracteres < ppaas_common_currency_code_v2 >
O código de moeda ISO-4217 de três caracteres que identifica a moeda.

Cópia
"str"
Erro
Detalhes do erro.

nome
obrigatório
corda
O nome do erro, legível por humanos e único.

mensagem
obrigatório
corda
A mensagem que descreve o erro.

debug_id
obrigatório
corda
O ID interno do PayPal usado para fins de correlação.

link_de_informação
corda
O link informativo, ou URI, que exibe informações detalhadas sobre esse erro para o desenvolvedor.


detalhes
Conjunto de objetos
Uma série de detalhes adicionais sobre o erro.


links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"message": "string",
"debug_id": "string",
"information_link": "string",
"details": [
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"description": "string"
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
]
}
Detalhes do erro
Detalhes do erro. Necessário para 4XXerros do lado do cliente.

campo
corda
O campo que causou o erro. Se o campo estiver no corpo da requisição, defina este valor como o ponteiro JSON para esse campo. Obrigatório para erros do lado do cliente.

valor
corda
O valor do campo que causou o erro.

localização
corda
Padrão: 
"corpo"
Localização do campo que causou o erro. O valor é body, path, ou query.

emitir
obrigatório
corda
O código de erro específico e detalhado da aplicação.

descrição
corda
Descrição legível para humanos de um problema. A descrição PODE mudar ao longo da vida útil de uma API, portanto, os clientes NÃO DEVEM depender desse valor.

Cópia
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"description": "string"
}
Executar ordem
Uma solicitação de execução de ordem.

modo_de_desembolso
obrigatório
corda
Indica se o dinheiro deve ser liberado imediatamente ou posteriormente.

Valor de enumeração	Descrição
INSTANT	O dinheiro é liberado instantaneamente.
ATRASADO	O dinheiro será liberado posteriormente.

pagador
objeto
A origem dos fundos para este pagamento. Pode ser uma conta PayPal ou um cartão de crédito.

CópiaExpandir tudoRecolher tudo
{
"disbursement_mode": "INSTANT",
"payer": {
"payment_method": "credit_card",
"status": "VERIFIED",
"funding_instruments": [
{
"credit_card": {
"number": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0,
"cvv2": "string",
"first_name": "string",
"last_name": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
}
},
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "string",
"last4": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0
}
}
],
"payer_info": {
"email": "[email protected]",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"country_code": "string",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
}
}
}
}
Instrumento de Financiamento
Detalhes do instrumento de financiamento. Uma instância deste esquema é válida se, e somente se, for validada em relação a exatamente uma destas propriedades suportadas.


Cartão de crédito
objeto
Obsoleto. Dados do cartão de crédito. Você pode usar este instrumento para efetuar um pagamento. Use um cartão de pagamento em vez disso.


token_do_cartão_de_crédito
objeto
Os dados do cartão de crédito tokenizados. Você pode usar esse instrumento para efetuar um pagamento.

CópiaExpandir tudoRecolher tudo
{
"credit_card": {
"number": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0,
"cvv2": "string",
"first_name": "string",
"last_name": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
}
},
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "string",
"last4": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0
}
}
Opção de parcelamento
Detalhes da opção de parcelamento.

prazo
obrigatório
inteiro
O número de parcelas.


pagamento_mensal
obrigatório
objeto
A moeda e o valor da transação.


valor_do_desconto
objeto
A moeda e o valor da transação.

porcentagem_de_desconto
string ^((-?[0-9]+)|(-?([0-9]+)?[.][0-9]+))$( Porcentagem )
A porcentagem é expressa como um valor decimal com sinal e ponto fixo. Use para todas as taxas de juros. Por exemplo, especifique uma taxa de juros de 19,99% como 19.99. Os formatos de número permitidos são números decimais e números inteiros.

CópiaExpandir tudoRecolher tudo
{
"term": 0,
"monthly_payment": {
"currency": "string",
"value": "string"
},
"discount_amount": {
"currency": "string",
"value": "string"
},
"discount_percentage": "string"
}
Item
Detalhes do item.

SKU
string <= 127 caracteres
A unidade de manutenção de estoque (SKU) do item.

nome
obrigatório
corda
Nome do item. O comprimento máximo é de 127 caracteres.

descrição
string <= 127 caracteres
Descrição do item. Pagamento somente via PayPal.

quantidade
obrigatório
string <= 10 caracteres ^[0-9]{0,10}$
Quantidade de itens. Deve ser um número inteiro.

preço
obrigatório
string <= 10 caracteres ^[0-9]{0,10}(\.[0-9]{0,2})?$
O custo do item. Suporta duas casas decimais.

moeda
obrigatório
corda
O código monetário ISO-4217 de três caracteres .

imposto
corda
Imposto sobre o item. Disponível apenas para pagamento via PayPal.

URL
corda
O URL com as informações do item. Disponível para o pagador no histórico de transações.

Cópia
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "string",
"tax": "string",
"url": "http://example.com"
}
linguagem
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 .

string ( idioma ) [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}))... < ppaas_common_language_v3 > Mostrar padrão
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 .

Cópia
"string"
Descrição do link
Informações sobre o link HATEOAS relacionado à solicitação .

href
obrigatório
corda
A URL de destino completa. Para fazer a chamada correspondente, combine o método com este link formatado em modelo URI . Para pré-processamento, inclua os caracteres $`<a>` (, `<b>` e ` <i>`. Este é o componente HATEOAS essencial que vincula uma chamada concluída a uma chamada subsequente.)href

rel
obrigatório
corda
O tipo de relação de link , que serve como um ID para um link que descreve sem ambiguidade a semântica do link. Consulte Relações de Link .

método
corda
O método HTTP necessário para fazer a chamada correspondente.

Enum : 
"PEGAR"
 
"PUBLICAR"
 
"COLOCAR"
 
"EXCLUIR"
 
"CABEÇA"
 
"CONECTAR"
 
"OPÇÕES"
 
"CORREÇÃO"
Cópia
{
"href": "string",
"rel": "string",
"method": "GET"
}
Metadados
Os pares nome-valor que contêm dados externos, como usuário, feedback do usuário, pontuação e assim por diante.


dados_suplementares
Conjunto de objetos
Uma matriz de pares nome-valor que contém os dados necessários para o processamento de transações pelo PayPal.

CópiaExpandir tudoRecolher tudo
{
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
}
Par Nome-Valor
Detalhes do par nome-valor.

nome
obrigatório
corda
A chave para o par nome-valor. Você deve correlacionar os tipos de valor e nome.

valor
obrigatório
corda
O valor do par nome-valor.

Cópia
{
"name": "string",
"value": "string"
}
Ordem
Uma ordem.

eu ia
corda
O ID do pedido.

intenção
corda
A intenção.

Valor de enumeração	Descrição
OFERTA	Uma venda.
AUTORIZAR	Uma autorização.

unidades_de_compra
obrigatório
Matriz de objetos não vazia
Um conjunto de unidades de compra. Cada unidade de compra estabelece um contrato entre um cliente e um comerciante.


detalhes_de_pagamento
objeto
Detalhes do pagamento do pedido.


contexto_de_aplicação
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento com o PayPal.

Observação: a Plataforma de Comércio do PayPal pode configurar brand_nameparâmetros shipping_preferencedurante a configuração da conta do parceiro, que substituem os valores da solicitação.
status
corda
O status do pedido. Após o cliente aprovar o pedido, o status é APPROVED. Após o pagamento ser efetuado e o pedido ser concluído, o status é COMPLETED.

Valor de enumeração	Descrição
CRIADO	A POST /v1/checkout/orderschamada foi bem-sucedida e o pedido foi criado.
APROVADO	O cliente aprovou o pedido.
CONCLUÍDO	A POST /v1/checkout/orders/{order_id}/paychamada foi bem-sucedida, o pedido foi pago e está concluído.
FRACASSADO	O pedido falhou.

redirecionar_urls
obrigatório
objeto
URLs de redirecionamento. Necessário apenas para o método de pagamento PayPal. As configurações suportadas são URLs de retorno e cancelamento.

tempo_de_criação
corda
A data e a hora em que o recurso foi criado, no formato de data e hora da Internet .

hora_de_atualização
corda
A data e a hora da última atualização do recurso, no formato de data e hora da Internet .


links
Conjunto de objetos
Uma série de links HATEOAS relacionados à solicitação . Para concluir a aprovação do comprador, use o approval_urllink com o GETmétodo e não use o link que mostra o REDIRECTmétodo.


valor_total_bruto
objeto
A moeda e o valor total calculado pelo PayPal para todas as unidades de compra.


metadados
objeto
Os pares nome-valor que contêm dados externos, como usuário, feedback do usuário, pontuação e assim por diante.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"intent": "SALE",
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom": "string",
"invoice_number": "string",
"payment_descriptor": "string",
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "string",
"tax": "string",
"url": "http://example.com"
}
],
"notify_url": "http://example.com",
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
},
"shipping_method": "string",
"partner_fee_details": {
"receiver": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
},
"amount": {
"currency": "string",
"value": "string"
}
},
"payment_linked_group": 1,
"metadata": {
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
},
"payment_summary": {
"captures": [
{
"id": "string",
"status": "PENDING",
"reason_code": "CHARGEBACK",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"refunds": [
{
"id": "string",
"capture_id": "string",
"sale_id": "string",
"status": "PENDING",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
}
}
],
"sales": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"authorizations": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
]
},
"status": "NOT_PROCESSED",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"payee": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
}
}
],
"payment_details": {
"payment_id": "string",
"disbursement_mode": "INSTANT"
},
"application_context": {
"brand_name": "string",
"locale": "string",
"shipping_preference": "NO_SHIPPING",
"user_action": "string",
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
},
"status": "CREATED",
"redirect_urls": {
"return_url": "http://example.com",
"cancel_url": "http://example.com"
},
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"gross_total_amount": {
"currency": "string",
"value": "string"
},
"metadata": {
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
}
}
Detalhes da taxa de parceria
A taxa de parceria cobrada pela transação original.


receptor
obrigatório
objeto
O sócio que recebe a taxa de sócio.


quantia
obrigatório
objeto
O valor e a moeda da taxa de parceria.

CópiaExpandir tudoRecolher tudo
{
"receiver": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
},
"amount": {
"currency": "string",
"value": "string"
}
}
Resposta da Ordem de Pagamento
Resposta a uma ordem de pagamento.

id_do_pedido
corda
O ID do pedido.

status
corda
O status do pedido.

Valor de enumeração	Descrição
APROVADO	O pedido foi aprovado.
CANCELADO	O pedido foi cancelado.
CONCLUÍDO	O pedido foi concluído.
CRIADO	O pedido foi criado.
EXPIRADO	O pedido expirou.
FRACASSADO	O pedido FALHOU.
EM ANDAMENTO	O pedido está em andamento.
PARCIALMENTE CONCLUÍDO	O pedido foi parcialmente concluído.
SUBMETIDO	O pedido foi submetido.
intenção
corda
A intenção.

Valor de enumeração	Descrição
OFERTA	Uma venda.
AUTORIZAR	Uma autorização.

unidades_de_compra
Matriz de objetos não vazia
Um conjunto de unidades de compra. Cada unidade de compra estabelece um contrato entre um cliente e um comerciante.

tempo_de_criação
corda
A data e a hora em que o recurso foi criado, no formato de data e hora da Internet .

hora_de_atualização
corda
A data e a hora da última atualização do recurso, no formato de data e hora da Internet .


links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .


informações_do_pagador
objeto
Informações do pagador.

CópiaExpandir tudoRecolher tudo
{
"order_id": "string",
"status": "APPROVED",
"intent": "SALE",
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom": "string",
"invoice_number": "string",
"payment_descriptor": "string",
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "string",
"tax": "string",
"url": "http://example.com"
}
],
"notify_url": "http://example.com",
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
},
"shipping_method": "string",
"partner_fee_details": {
"receiver": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
},
"amount": {
"currency": "string",
"value": "string"
}
},
"payment_linked_group": 1,
"metadata": {
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
},
"payment_summary": {
"captures": [
{
"id": "string",
"status": "PENDING",
"reason_code": "CHARGEBACK",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"refunds": [
{
"id": "string",
"capture_id": "string",
"sale_id": "string",
"status": "PENDING",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
}
}
],
"sales": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"authorizations": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": null,
"shipping": null,
"tax": null,
"handling_fee": null,
"shipping_discount": null,
"insurance": null,
"gift_wrap": null
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
]
},
"status": "NOT_PROCESSED",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"payee": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
}
}
],
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"payer_info": {
"email": "[email protected]",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"country_code": "string",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
}
}
}
Beneficiário
O beneficiário é quem recebe os fundos e cumpre o pedido.

e-mail
corda
O endereço de e-mail associado à conta PayPal do beneficiário. Para fins de autorização ou pedido, o endereço de e-mail deve estar associado a uma conta comercial PayPal confirmada. Para fins de venda, o e-mail pode ser:

Associe-se a uma conta pessoal ou comercial do PayPal confirmada.
Não estar associado a uma conta PayPal.
id_do_comerciante
corda
O ID da conta PayPal criptografada do beneficiário.


metadados_exibidos_do_pagador
objeto
Metadados somente para exibição do beneficiário.

CópiaExpandir tudoRecolher tudo
{
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
}
Metadados de exibição do beneficiário
Metadados somente para exibição do beneficiário.

e-mail
corda
Endereço de e-mail do pagador. O comprimento máximo é de 127 caracteres.


exibir_telefone
objeto
O número de telefone do beneficiário.

nome_da_marca
corda
Nome comercial do pagador.

CópiaExpandir tudoRecolher tudo
{
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
Pagador
O pagador. O pagador efetua o pagamento.

método_de_pagamento
corda
Método de pagamento. O valor pode ser pago via PayPal Wallet, débito direto em conta bancária ou cartão de crédito.

Enum : 
"Cartão de crédito"
 
"paypal"
status
corda
O status da conta PayPal do pagador.

Enum : 
"VERIFICADO"
 
"NÃO VERIFICADO"

instrumentos_de_financiamento
Conjunto de objetos
Uma matriz contendo um único instrumento de pagamento para a transação atual. Válida e obrigatória apenas para o método de pagamento com cartão de crédito. A matriz deve incluir um objeto `input` credit_cardou ` credit_card_tokenpayment`. Se a matriz contiver mais de um instrumento, o pagamento será recusado.


informações_do_pagador
objeto
Informações do pagador.

CópiaExpandir tudoRecolher tudo
{
"payment_method": "credit_card",
"status": "VERIFIED",
"funding_instruments": [
{
"credit_card": {
"number": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0,
"cvv2": "string",
"first_name": "string",
"last_name": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
}
},
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "string",
"last4": "string",
"type": "string",
"expire_month": 0,
"expire_year": 0
}
}
],
"payer_info": {
"email": "[email protected]",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"country_code": "string",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
}
}
}
Informações do pagador
Informações do pagador.

e-mail
corda
Endereço de e-mail do pagador. O comprimento máximo é de 127 caracteres.

saudação
corda
Saudação do pagador.

primeiro nome
corda
O primeiro nome do pagador.

nome_do_meio
corda
O nome do meio de quem paga.

sobrenome
corda
Sobrenome do pagador.

sufixo
corda
Sufixo do pagador.

id_do_pagador
corda
O ID de pagador criptografado atribuído pelo PayPal.

data_de_nascimento
corda
A data de nascimento do pagador, no formato de data da Internet . Por exemplo, 1990-04-12.

ID_imposto
string <= 14 caracteres
Número de identificação fiscal do pagador. Disponível apenas para pagamentos via PayPal.

tipo_de_identificação_de_imposto
corda
Tipo de identificação fiscal do pagador. Compatível apenas com o método de pagamento PayPal.

Enum : 
"BR_CPF"
 
"BR_CNPJ"
código_do_país
corda
O código de país ISO-3166-1 de dois caracteres do pagador .


Endereço de Cobrança
objeto
O endereço de cobrança ou de entrega para o pagamento.


Endereço para envio
objeto
Detalhes do endereço de entrega.

CópiaExpandir tudoRecolher tudo
{
"email": "[email protected]",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"country_code": "string",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
}
}
Detalhes do pagamento
Detalhes do pagamento do pedido.

id_do_pagamento
corda
O ID de pagamento do pedido.

modo_de_desembolso
corda
Indica se o pagamento deve ser efetuado imediatamente ou se deve ser adiado.

Valor de enumeração	Descrição
INSTANT	O pagamento é efetuado instantaneamente.
ATRASADO	O pagamento está atrasado.
Cópia
{
"payment_id": "string",
"disbursement_mode": "INSTANT"
}
Resumo de Pagamentos
Resumo do pagamento.


capturas
Matriz de objetos não vazia
Uma matriz de capturas para uma unidade de compra. Uma unidade de compra pode ter zero ou mais capturas.


reembolsos
Matriz de objetos não vazia
Uma série de reembolsos para uma unidade comprada. Uma unidade comprada pode ter zero ou mais reembolsos.


vendas
Matriz de objetos não vazia
Um conjunto de vendas para uma unidade de compra. Uma unidade de compra pode ter zero ou mais vendas.


autorizações
Matriz de objetos não vazia
Um conjunto de autorizações para uma unidade de compra. Uma unidade de compra pode ter zero ou mais autorizações.

CópiaExpandir tudoRecolher tudo
{
"captures": [
{
"id": "string",
"status": "PENDING",
"reason_code": "CHARGEBACK",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"refunds": [
{
"id": "string",
"capture_id": "string",
"sale_id": "string",
"status": "PENDING",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
}
}
],
"sales": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"authorizations": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
]
}
Percentagem
A porcentagem é expressa como um valor decimal com sinal e ponto fixo. Use para todas as taxas de juros. Por exemplo, especifique uma taxa de juros de 19,99% como 19.99. Os formatos de número permitidos são números decimais e números inteiros.

string ( Porcentagem ) ^((-?[0-9]+)|(-?([0-9]+)?[.][0-9]+))$ < ppaas_common_percentage_v1 >
A porcentagem é expressa como um valor decimal com sinal e ponto fixo. Use para todas as taxas de juros. Por exemplo, especifique uma taxa de juros de 19,99% como 19.99. Os formatos de número permitidos são números decimais e números inteiros.

Cópia
"string"
Unidade de compra
Uma unidade de compra. Estabelece um contrato entre o pagador e o beneficiário.

id_de_referência
obrigatório
string <= 256 caracteres
O ID do comerciante referente à unidade de compra.

descrição
string <= 127 caracteres
Descrição da compra.

personalizado
string <= 127 caracteres
O ID externo fornecido pelo cliente. Usado para conciliar as transações do cliente com as transações do PayPal. Retornado nos relatórios de transação e liquidação. Compatível apenas com o método de pagamento PayPal.

número_da_fatura
string <= 256 caracteres
O ID da fatura externa fornecido pelo solicitante da API para este pedido. Compatível apenas com o método de pagamento PayPal.

descritor_de_pagamento
string com menos de 22 caracteres
O descritor de pagamento no extrato da conta do cartão de crédito do comprador.


Unid
Conjunto de objetos
Uma variedade de itens que o cliente está comprando do comerciante.

notify_url
string <= 2048 caracteres
O URL de notificações de pagamento.


Endereço para envio
objeto
Detalhes do endereço de entrega.

método de envio
corda
O método de envio. Por exemplo, USPSParcel.


detalhes_da_taxa_de_parceiro
objeto
A taxa de parceria cobrada pela transação original.

grupo_vinculado_ao_pagamento
inteiro [ 1 .. 100 ]
Um ID que agrupa várias unidades de compra vinculadas. As transações de compra são vinculadas apenas para o pagamento e não para o reembolso. Um reembolso é processado apenas para a transação específica dentro do mesmo grupo vinculado.


metadados
objeto
Os pares nome-valor que contêm dados externos, como usuário, feedback do usuário, pontuação e assim por diante.


resumo_de_pagamento
objeto
Resumo do pagamento.

status
corda
O estado da transação.

Valor de enumeração	Descrição
NÃO PROCESSADO	A transação não foi processada.
PENDENTE	A transação está pendente.
ANULADO	A transação foi recusada e anulada.
AUTORIZADO	O pagamento da transação não foi autorizado.
CAPTURADO	O pagamento da transação foi capturado ou está pendente de captura.
código_motivo
corda
O código de motivo para um status de transação de "Não resolvido" PENDINGou " REVERSEDNão resolvido". Eventualmente, este campo substituirá o anterior pending_reason. Compatível apenas com o método de pagamento PayPal.

Valor de enumeração	Descrição
PAGADOR_ENVIO_NÃO_CONFIRMADO	O estado da transação é PENDING"ou" REVERSEDdevido a um endereço de entrega do pagador não confirmado.
MÚLTIPLAS MOEDAS	O estado da transação é PENDING"ou" REVERSEDporque se trata de uma transação em múltiplas moedas.
ANÁLISE DE RISCO	O estado da transação é " PENDINGou REVERSEDestá em análise de risco".
REVISÃO REGULATÓRIA	O estado da transação é PENDINGou REVERSEDestá em análise regulatória.
VERIFICAÇÃO NECESSÁRIA	O estado da transação é PENDING"ou" REVERSEDporque é necessária verificação.
ORDEM	O estado da transação é PENDING"ou" REVERSEDporque a transação é uma ordem.
OUTRO	O estado da transação é PENDINGou REVERSEDdevido a outro motivo.
RECUSADO_POR_POLÍTICA	O estado da transação é PENDING"ou" REVERSEDporque foi recusada por uma política.

quantia
obrigatório
objeto
O valor a ser cobrado.


beneficiário
objeto
O destinatário dos fundos referentes a esta transação.

CópiaExpandir tudoRecolher tudo
{
"reference_id": "string",
"description": "string",
"custom": "string",
"invoice_number": "string",
"payment_descriptor": "string",
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "string",
"tax": "string",
"url": "http://example.com"
}
],
"notify_url": "http://example.com",
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"country_code": "st",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"recipient_name": "string"
},
"shipping_method": "string",
"partner_fee_details": {
"receiver": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
},
"amount": {
"currency": "string",
"value": "string"
}
},
"payment_linked_group": 1,
"metadata": {
"supplementary_data": [
{
"name": "string",
"value": "string"
}
]
},
"payment_summary": {
"captures": [
{
"id": "string",
"status": "PENDING",
"reason_code": "CHARGEBACK",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"refunds": [
{
"id": "string",
"capture_id": "string",
"sale_id": "string",
"status": "PENDING",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
}
}
],
"sales": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
],
"authorizations": [
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
]
},
"status": "NOT_PROCESSED",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"payee": {
"email": "[email protected]",
"merchant_id": "string",
"payee_display_metadata": {
"email": "[email protected]",
"display_phone": {
"country_code": "string",
"number": "string"
},
"brand_name": "string"
}
}
}
Reembolso
Uma transação de reembolso.

eu ia
corda
O ID da transação de reembolso. O comprimento máximo é de 17 caracteres.

id_de_captura
corda
O ID da transação de venda a ser reembolsada.

id_da_venda
corda
O ID da transação de venda a ser reembolsada.

status
corda
O status do reembolso.

Valor de enumeração	Descrição
PENDENTE	O reembolso está pendente.
CONCLUÍDO	O reembolso foi concluído.
FRACASSADO	O reembolso falhou.

links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .


quantia
objeto
O valor reembolsado ao pagador e o valor reembolsado ao beneficiário. O comprimento máximo é de 10 caracteres, incluindo:

Sete dígitos antes da vírgula decimal.
O ponto decimal.
Dois dígitos após a vírgula decimal.
CópiaExpandir tudoRecolher tudo
{
"id": "string",
"capture_id": "string",
"sale_id": "string",
"status": "PENDING",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
}
}
Oferta
Uma transação de venda.

eu ia
corda
O ID da transação de venda.

status
corda
O status da transação de venda.

Valor de enumeração	Descrição
CONCLUÍDO	A venda foi concluída.
REEMBOLSADO PARCIALMENTE	A venda foi parcialmente reembolsada.
PENDENTE	A venda está pendente.
REEMBOLSADO	A venda foi reembolsada.
NEGADO	A venda foi negada.
código_motivo
corda
Um código de motivo que indica a razão para o estado da transação de PENDINGou REVERSED.

Valor de enumeração	Descrição
ESTORNO	O estado da transação se REVERSEDdeve a um estorno.
GARANTIA	O estado da transação REVERSEDdeve-se a uma garantia.
RECLAMAÇÃO DO COMPRADOR	O status da transação se REVERSEDdeve a uma reclamação do comprador.
REEMBOLSO	O status da transação é " REVERSEDdevido a um reembolso".
ENDEREÇO ​​DE ENTREGA NÃO CONFIRMADO	O estado da transação é PENDING"ou" REVERSEDdevido a um endereço de entrega não confirmado.
Cheque eletrônico	O estado da transação PENDINGdeve-se a um cheque eletrônico.
RETIRADA INTERNACIONAL	O status da transação se PENDINGdeve a um saque internacional.
RECEBIMENTO DE MANDATOS DE PREFERÊNCIA - AÇÃO MANUAL	O estado da transação PENDINGdeve-se a uma preferência de recebimento que exige ação manual.
REVISÃO DE PAGAMENTO	O status da transação é " PENDINGDevido a uma análise de pagamento".
REVISÃO REGULATÓRIA	O status da transação se PENDINGdeve a uma revisão regulatória.
UNILATERAL	O estado da transação PENDINGdeve-se a uma razão unilateral.
VERIFICAÇÃO NECESSÁRIA	O estado da transação é PENDING"is verification is required".
DESEMBOLSO_ATRASADO	O estado da transação PENDINGdeve-se a um atraso no desembolso.
tempo_de_criação
corda
A data e a hora em que o recurso foi criado, no formato de data e hora da Internet .

hora_de_atualização
corda
A data e a hora da última atualização do recurso, no formato de data e hora da Internet .


links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações .


quantia
objeto
O valor a ser coletado. O comprimento máximo é de 10 caracteres, incluindo:

Sete dígitos antes da vírgula decimal.
O ponto decimal.
Dois dígitos após a vírgula decimal.

taxa_de_transação
objeto
A moeda e o valor da taxa de transação. O comprimento máximo é de 10 caracteres, incluindo:

Sete dígitos antes da vírgula decimal.
O ponto decimal.
Dois dígitos após a vírgula decimal.
CópiaExpandir tudoRecolher tudo
{
"id": "string",
"status": "COMPLETED",
"reason_code": "CHARGEBACK",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency": "string",
"total": "string",
"details": {
"subtotal": "string",
"shipping": "string",
"tax": "string",
"handling_fee": "string",
"shipping_discount": "string",
"insurance": "string",
"gift_wrap": "string"
}
},
"transaction_fee": {
"currency": "string",
"value": "string"
}
}
Endereço para envio
Detalhes do endereço de entrega.

linha1
obrigatório
corda
A primeira linha do endereço. Por exemplo, número, rua, etc.

linha2
corda
A segunda linha do endereço. Por exemplo, o número do apartamento ou da sala.

cidade
corda
O nome da cidade.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código ISO 3166-1 de dois caracteres que identifica o país ou região.

Nota: O código de país para a Grã-Bretanha é 0gg GBe não UKo utilizado nos domínios de nível superior desse país. Utilize o C2código de país para a China em todo o mundo para o método de preços comparáveis ​​não controlados (CUP), cartões bancários e transações internacionais.
código postal
corda
O código postal, que é o CEP ou equivalente. Normalmente exigido para países que possuem código postal ou equivalente. Veja código postal .

estado
corda
O código de um estado dos EUA ou o equivalente em outros países. Obrigatório para transações se o endereço for em um destes países: Argentina , Brasil , Canadá , Índia , Itália , Japão , México , Tailândia ou Estados Unidos . O comprimento máximo é de 40 caracteres de um byte.

telefone
corda
O número de telefone, no formato E.123 . O comprimento máximo é de 50 caracteres.

status_de_normalização
corda
Status da normalização de endereço. Retornado apenas para pagadores do Brasil.

Enum : 
"DESCONHECIDO"
 
"PREFERÊNCIA DO USUÁRIO NÃO NORMALIZADO"
 
"NORMALIZADO"
 
"NÃO NORMALIZADO"
tipo
corda
O tipo de endereço. Por exemplo, HOME_OR_WORK, GIFT, e assim por diante.

nome_do_destinatário
string <= 127 caracteres
Nome do destinatário neste endereço.

