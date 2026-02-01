

/v2/finalizar compra/pedidos
Experimente
Cria um pedido. Lojistas e parceiros podem adicionar dados de Nível 2 e 3 aos pagamentos para reduzir riscos e custos de processamento. Para mais informações sobre processamento de pagamentos, consulte finalização da compra ou finalização da compra com várias partes .

Nota: Para tratamento de erros e resolução de problemas, consulte Erros de pedidos v2 .
Segurança
OAuth2
Solicitar
Parâmetros do cabeçalho
ID da solicitação do PayPal
string [ 1 .. 108 ] caracteres
O servidor armazena as chaves por 6 horas. Os usuários da API podem solicitar que o período de armazenamento seja estendido para até 72 horas, entrando em contato com seu Gerente de Contas. Essa configuração é obrigatória para todas as solicitações de criação de pedidos em uma única etapa (por exemplo, solicitação de criação de pedido com informações da fonte de pagamento, como cartão, PayPal.vault_id, PayPal.billing_agreement_id etc.).

ID de atribuição de parceiro do PayPal
string [ 1 .. 36 ] caracteres
ID de metadados do cliente PayPal
string [ 1 .. 36 ] caracteres
Preferência
string [ 1 .. 25 ] caracteres ^[a-zA-Z=,-]*$
Padrão: 
retorno=mínimo
Resposta preferencial do servidor após a conclusão bem-sucedida da solicitação. O valor é:

return=minimalO servidor retorna uma resposta mínima para otimizar a comunicação entre o chamador da API e o servidor. Uma resposta mínima inclui os links `<username> id`, ` status<username>` e `<username>`.
return=representationO servidor retorna uma representação completa do recurso, incluindo o estado atual do recurso.
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
obrigatório

unidades_de_compra
obrigatório
Matriz de objetos [ 1 .. 10 ] itens
Um conjunto de unidades de compra. Cada unidade de compra estabelece um contrato entre o pagador e o beneficiário. Cada unidade de compra representa um pedido, total ou parcial, que o pagador pretende adquirir do beneficiário.

intenção
obrigatório
corda
A intenção é capturar o pagamento imediatamente ou autorizar um pagamento para um pedido após a sua criação.

Valor de enumeração	Descrição
CAPTURAR	O comerciante pretende capturar o pagamento imediatamente após o cliente efetuá-lo.
AUTORIZAR	O comerciante pretende autorizar um pagamento e reter os fundos após o cliente efetuar o pagamento. Os pagamentos autorizados são melhor capturados dentro de três dias após a autorização, mas podem ser capturados por até 29 dias. Após o período de três dias, o pagamento autorizado original expira e você deve reautorizá-lo. Você deve fazer uma solicitação separada para capturar pagamentos sob demanda. Essa opção não é compatível quando há mais de uma opção de captura de pagamento purchase_unitem seu pedido.

pagador
objeto
OBSOLETO. O cliente também é conhecido como pagador. O objeto Pagador foi projetado para ser usado apenas com o payment_source.paypalobjeto. Para tornar esse design mais claro, os detalhes do payerobjeto agora estão disponíveis em payment_source.paypal. Por favor, use payment_source.paypal.


fonte_de_pagamento
objeto
Definição da fonte de pagamento.


contexto_de_aplicação
objeto
Personalize a experiência do pagador durante o processo de aprovação do pagamento com o PayPal.

Respostas
200Uma resposta bem-sucedida a uma solicitação idempotente retorna o 200 OKcódigo de status HTTP com um corpo de resposta JSON que exibe os detalhes do pedido.
201Uma solicitação bem-sucedida retorna o 201 Createdcódigo de status HTTP e um corpo de resposta JSON que inclui, por padrão, uma resposta mínima com o ID, o status e os links HATEOAS. Se você precisar da representação completa do recurso do pedido, deverá passar o Prefer: return=representationcabeçalho de solicitação . O valor desse cabeçalho não é o padrão.
400A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 200 - Criar Pedido - Carteira PayPal como Forma de Pagamento, resultando na resposta PAYER_ACTION_REQUIRED
Exemplo 1 - 200 - Criar Pedido - Carteira PayPal como Forma de Pagamento, resultando na resposta PAYER_ACTION_REQUIRED
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders \ 
-H  'Content-Type: application/json'  \ 
-H  'PayPal-Request-Id: 7b92603e-77ed-4896-8e78-5dea2050476a'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{
  "intenção": "CAPTURA",
  "fonte_de_pagamento": {
    "paypal": {
      "contexto_de_experiência": {
        "payment_method_preference": "IMMEDIATE_PAYMENT_REQUIRED",
        "landing_page": "LOGIN",
        "preferência_de_envio": "OBTER_DO_ARQUIVO",
        "user_action": "PAGAR_AGORA",
        "return_url": "https://example.com/returnUrl",
        "cancel_url": "https://example.com/cancelUrl"
      }
    }
  },
  "purchase_units": [
    {
      "id_da_fatura": "90210",
      "quantia": {
        "currency_code": "USD",
        "valor": "230,00"
        "discriminação": {
          "total_do_item": {
            "currency_code": "USD",
            "Valor": "220,00"
          },
          "envio": {
            "currency_code": "USD",
            "Valor": "10,00"
          }
        }
      },
      "Unid": [
        {
          "nome": "Camiseta",
          "descrição": "Camisa Super Fresca",
          "unidade_quantidade": {
            "currency_code": "USD",
            "Valor": "20,00"
          },
          "quantidade": "1",
          "categoria": "BENS_FÍSICOS",
          "sku": "sku01",
          "image_url": "https://example.com/static/images/items/1/tshirt_green.jpg",
          "url": "https://example.com/url-to-the-item-being-purchased-1",
          "upc": {
            "tipo": "UPC-A",
            "código": "123456789012"
          }
        },
        {
          "nome": "Sapatos",
          "descrição": "Corrida, Tamanho 10,5"
          "sku": "sku02",
          "unidade_quantidade": {
            "currency_code": "USD",
            "Valor": "100,00"
          },
          "quantidade": "2",
          "categoria": "BENS_FÍSICOS",
          "image_url": "https://example.com/static/images/items/1/shoes_running.jpg",
          "url": "https://example.com/url-to-the-item-being-purchased-2",
          "upc": {
            "tipo": "UPC-A",
            "código": "987654321012"
          }
        }
      ]
    }
  ]
}' 
Amostras de resposta
200
201
400
422
aplicativo/json

Exemplo 1 - 200 - Criar Pedido - Carteira PayPal como Forma de Pagamento, resultando na resposta PAYER_ACTION_REQUIRED
Exemplo 1 - 200 - Criar Pedido - Carteira PayPal como Forma de Pagamento, resultando na resposta PAYER_ACTION_REQUIRED
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "PAYER_ACTION_REQUIRED",
"payment_source": {
"paypal": { }
},
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.paypal.com/checkoutnow?token=5O190127TN364715T",
"rel": "payer-action",
"method": "GET"
}
]
}
Mostrar detalhes do pedido
pegar
/v2/finalizar compra/pedidos/{id}
Experimente
Exibe os detalhes de um pedido, por ID.

Nota: Para tratamento de erros e resolução de problemas, consulte Erros de pedidos v2 .
Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido para o qual deseja exibir os detalhes.

Parâmetros de consulta
campos
corda
Uma lista de campos separados por vírgula que devem ser retornados para o pedido. O campo de filtro válido é payment_source.

Parâmetros do cabeçalho
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Respostas
200Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP e um corpo de resposta JSON que mostra os detalhes do pedido.
404O recurso especificado não existe.
Solicitar amostras
cURL

Exemplo 1 - 200 - Obter Pedido - Mostrar Detalhes do Pedido Após Aprovação do Cliente
Exemplo 1 - 200 - Obter Pedido - Mostrar Detalhes do Pedido Após Aprovação do Cliente
Cópia
curl  -v  -X GET https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Amostras de resposta
200
404
aplicativo/json

Exemplo 1 - 200 - Obter Pedido - Mostrar Detalhes do Pedido Após Aprovação do Cliente
Exemplo 1 - 200 - Obter Pedido - Mostrar Detalhes do Pedido Após Aprovação do Cliente
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "APPROVED",
"intent": "CAPTURE",
"payment_source": {
"paypal": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "[email protected]",
"account_id": "QYR5Z8XDVJNXQ"
}
},
"purchase_units": [
{
"reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
"amount": {
"currency_code": "USD",
"value": "100.00"
}
}
],
"payer": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "[email protected]",
"payer_id": "QYR5Z8XDVJNXQ"
},
"create_time": "2018-04-01T21:18:49Z",
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.paypal.com/checkoutnow?token=5O190127TN364715T",
"rel": "approve",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "update",
"method": "PATCH"
},
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T/capture",
"rel": "capture",
"method": "POST"
}
]
}
Atualizar pedido
correção
/v2/finalizar compra/pedidos/{id}
Experimente
Atualiza um pedido com um status CREATEDou APPROVEDstatus. Não é possível atualizar um pedido com o COMPLETEDstatus .

Para fazer uma atualização, você deve fornecer um reference_id. Se você omitir esse valor em um pedido que contenha apenas uma unidade de compra, o PayPal definirá o valor como , defaulto que permite usar o caminho: "/purchase_units/@reference_id=='default'/{attribute-or-object}". Comerciantes e parceiros podem adicionar dados de Nível 2 e 3 aos pagamentos para reduzir riscos e custos de processamento. Para obter mais informações sobre o processamento de pagamentos, consulte a finalização da compra ou a finalização da compra com várias partes .

Nota: Para tratamento de erros e resolução de problemas, consulte Erros de pedidos v2 .
Atributos ou objetos que podem ser modificados:

Atributo	Op	Notas
intent	substituir	
payer	substituir, adicionar	Usar a operação de substituição (replace op) payersubstituirá o payerobjeto inteiro pelo valor enviado na solicitação.
purchase_units	substituir, adicionar	
purchase_units[].custom_id	substituir, adicionar, remover	
purchase_units[].description	substituir, adicionar, remover	
purchase_units[].payee.email	substituir	
purchase_units[].shipping.name	substituir, adicionar	
purchase_units[].shipping.email_address	substituir, adicionar	
purchase_units[].shipping.phone_number	substituir, adicionar	
purchase_units[].shipping.options	substituir, adicionar	
purchase_units[].shipping.address	substituir, adicionar	
purchase_units[].shipping.type	substituir, adicionar	
purchase_units[].soft_descriptor	substituir, remover	
purchase_units[].amount	substituir	
purchase_units[].items	substituir, adicionar, remover	
purchase_units[].invoice_id	substituir, adicionar, remover	
purchase_units[].payment_instruction	substituir	
purchase_units[].payment_instruction.disbursement_mode	substituir	Por padrão, disbursement_modeé INSTANT.
purchase_units[].payment_instruction.payee_receivable_fx_rate_id	substituir, adicionar, remover	
purchase_units[].payment_instruction.platform_fees	substituir, adicionar, remover	
purchase_units[].supplementary_data.airline	substituir, adicionar, remover	
purchase_units[].supplementary_data.card	substituir, adicionar, remover	
application_context.client_configuration	substituir, adicionar	
Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido a ser atualizado.

Parâmetros do cabeçalho
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição: application/json
Array ([ 0 .. 32767 ] itens)
op
obrigatório
corda
A operação.

Valor de enumeração	Descrição
adicionar	Dependendo da referência de localização de destino, executa uma destas funções:
O local de destino é um índice de matriz . Insere um novo valor na matriz no índice especificado.
O local de destino é um parâmetro de objeto que ainda não existe . Adiciona um novo parâmetro ao objeto.
O local de destino é um parâmetro de objeto que existe . Substitui o valor desse parâmetro.
O valueparâmetro define o valor a ser adicionado. Para obter mais informações, consulte 4.1. adicionar .
remover	Remove o valor no local de destino. Para que a operação seja bem-sucedida, o local de destino deve existir. Para obter mais informações, consulte 4.2. remover .
substituir	Substitui o valor no local de destino por um novo valor. O objeto de operação deve conter um valueparâmetro que define o valor de substituição. Para que a operação seja bem-sucedida, o local de destino deve existir. Para obter mais informações, consulte 4.3. substituir .
mover	Remove o valor em um local especificado e o adiciona ao local de destino. O objeto de operação deve conter um fromparâmetro, que é uma string contendo um ponteiro JSON que referencia o local no documento de destino de onde o valor será movido. Para que a operação seja bem-sucedida, o fromlocal deve existir. Para obter mais informações, consulte 4.4. mover .
cópia	Copia o valor de um local especificado para o local de destino. O objeto de operação deve conter um fromparâmetro, que é uma string contendo um ponteiro JSON que referencia o local no documento de destino do qual o valor será copiado. Para que a operação seja bem-sucedida, o fromlocal deve existir. Para obter mais informações, consulte 4.5. copy .
teste	Testa se um valor no local de destino é igual a um valor especificado. O objeto de operação deve conter um valueparâmetro que define o valor a ser comparado com o valor do local de destino. Para que a operação seja bem-sucedida, o valor no local de destino deve ser igual ao valuevalor especificado. Por exemplo, o parâmetro equalindica que o valor no local de destino e o valor valueespecificado são do mesmo tipo JSON. O tipo de dados do valor determina como a igualdade é definida.
Tipo	Considera-se que ambos os valores são iguais se ambos os valores
cordas	Contêm o mesmo número de caracteres Unicode e seus pontos de código são iguais byte a byte.
números	São numericamente iguais.
matrizes	Contêm o mesmo número de valores, e cada valor é igual ao valor na posição correspondente na outra matriz, utilizando estas regras específicas do tipo.
objetos	Contêm o mesmo número de parâmetros, e cada parâmetro é igual a um parâmetro no outro objeto, comparando suas chaves (como strings) e seus valores (usando estas regras específicas do tipo).
literais ( false, true, e null)	São iguais. A comparação é lógica. Por exemplo, espaços em branco entre os valores dos parâmetros de um array não são relevantes. Da mesma forma, a ordem de serialização dos parâmetros do objeto também não é relevante.
Para obter mais informações, consulte 4.6. teste .
caminho
corda
O ponteiro JSON para a localização do documento de destino onde a operação será concluída.

valor
qualquer
O valor a ser aplicado. As operações remove`__init__` copy, `__replace__` e `__replace__` movenão exigem um valor. Como o JSON Patch permite qualquer tipo para `__init__` value, a typepropriedade não é especificada.

de
corda
O ponteiro JSON para o local do documento de destino a partir do qual o valor será movido. Necessário para a moveoperação.

Respostas
204Uma solicitação bem-sucedida retorna o 204 No Contentcódigo de status HTTP com um objeto vazio no corpo da resposta JSON.
400A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
Solicitar amostras
cURL

Amostra 1 - 204 - Pedido de Patch - Adicionar Endereço de Entrega
Amostra 1 - 204 - Pedido de Patch - Adicionar Endereço de Entrega
Cópia
curl  -v  -X PATCH https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '[
  {
    "op": "substituir",
    "caminho": "/purchase_units/@reference_id==' PUHF '/shipping/address",
    "valor": {
      "address_line_1": "2211 N First Street",
      "address_line_2": "Edifício 17",
      "admin_area_2": "San Jose",
      "admin_area_1": "CA",
      "código postal": "95131",
      "código_do_país": "EUA"
    }
  }
]' 
Amostras de resposta
204
400
422
aplicativo/json

Amostra 1 - 204 - Pedido de Patch - Adicionar Endereço de Entrega
Amostra 1 - 204 - Pedido de Patch - Adicionar Endereço de Entrega
Cópia
{ }
Confirme o pedido
publicar
/v2/finalizar compra/pedidos/{id}/confirmar-fonte-de-pagamento
Experimente
O pagador confirma sua intenção de pagar pelo pedido com a forma de pagamento informada.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido para o qual o pagador confirma sua intenção de pagar.

Parâmetros do cabeçalho
ID de metadados do cliente PayPal
string [ 1 .. 36 ] caracteres
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Preferência
string [ 1 .. 25 ] caracteres ^[a-zA-Z=]*$
Padrão: 
retorno=mínimo
Resposta preferencial do servidor após a conclusão bem-sucedida da solicitação. O valor é:

return=minimalO servidor retorna uma resposta mínima para otimizar a comunicação entre o chamador da API e o servidor. Uma resposta mínima inclui os links `<username> id`, ` status<username>` e `<username>`.
return=representationO servidor retorna uma representação completa do recurso, incluindo o estado atual do recurso.
Esquema do corpo da requisição:
aplicativo/json
aplicativo/json

contexto_de_aplicação
objeto
Personaliza a experiência de confirmação do pagador.


fonte_de_pagamento
obrigatório
objeto
Definição da fonte de pagamento.

Respostas
200Uma solicitação bem-sucedida indica que a forma de pagamento foi adicionada ao pedido. Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP com um corpo de resposta JSON que exibe os detalhes do pedido.
400A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.
403A autorização falhou devido a permissões insuficientes.
404Descrição padrão
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
500Ocorreu um erro interno no servidor.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 200 - Confirmar Pedido - Carteira PayPal resultando em ação do pagador necessária
Exemplo 1 - 200 - Confirmar Pedido - Carteira PayPal resultando em ação do pagador necessária
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T/confirm-payment-source \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{
  "fonte_de_pagamento": {
    "paypal": {
      "nome": {
        "nome_próprio": "John",
        Sobrenome: "Doe"
      },
      "endereço_de_email": "customer@example.com",
      "contexto_de_experiência": {
        "payment_method_preference": "IMMEDIATE_PAYMENT_REQUIRED",
        "nome_da_marca": "EXAMPLE INC",
        "locale": "en-US",
        "landing_page": "LOGIN",
        "preferência_de_envio": "DEFINIR_ENDEREÇO_FORNECIDO",
        "user_action": "PAGAR_AGORA",
        "return_url": "https://example.com/returnUrl",
        "cancel_url": "https://example.com/cancelUrl"
      }
    }
  }
}' 
Amostras de resposta
200
400
403
404
422

mais 1
mais 1
aplicativo/json

Exemplo 1 - 200 - Confirmar Pedido - Carteira PayPal resultando em ação do pagador necessária
Exemplo 1 - 200 - Confirmar Pedido - Carteira PayPal resultando em ação do pagador necessária
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "PAYER_ACTION_REQUIRED",
"payment_source": {
"paypal": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "[email protected]"
}
},
"payer": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "[email protected]"
},
"links": [
{
"href": "https://api.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.paypal.com/checkoutnow?token=5O190127TN364715T",
"rel": "payer-action",
"method": "GET"
}
]
}
Autorizar pagamento do pedido
publicar
/v2/finalizar compra/pedidos/{id}/autorizar
Experimente
Autoriza o pagamento de um pedido. Para autorizar o pagamento de um pedido com sucesso, o comprador deve primeiro aprovar o pedido ou fornecer uma fonte de pagamento válida na solicitação. O comprador pode aprovar o pedido ao ser redirecionado para a URL rel:approve retornada nos links HATEOAS na resposta de criação do pedido.

Nota: Para tratamento de erros e resolução de problemas, consulte Erros de pedidos v2 .
Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido que deseja autorizar.

Parâmetros do cabeçalho
ID da solicitação do PayPal
string [ 1 .. 108 ] caracteres
O servidor armazena as chaves por 6 horas. Os usuários da API podem solicitar que o período de armazenamento seja estendido para até 72 horas, entrando em contato com seu Gerente de Contas. Essa configuração é obrigatória para todas as solicitações de criação de pedidos em uma única etapa (por exemplo, solicitação de criação de pedido com informações da fonte de pagamento, como cartão, PayPal.vault_id, PayPal.billing_agreement_id etc.).

Preferência
string [ 1 .. 25 ] caracteres ^[a-zA-Z=,-]*$
Padrão: 
retorno=mínimo
Resposta preferencial do servidor após a conclusão bem-sucedida da solicitação. O valor é:

return=minimalO servidor retorna uma resposta mínima para otimizar a comunicação entre o chamador da API e o servidor. Uma resposta mínima inclui os links `<username> id`, ` status<username>` e `<username>`.
return=representationO servidor retorna uma representação completa do recurso, incluindo o estado atual do recurso.
ID de metadados do cliente PayPal
string [ 1 .. 36 ] caracteres
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição:
aplicativo/json
aplicativo/json

fonte_de_pagamento
objeto
A fonte de pagamento do pedido, que pode ser um token ou um cartão. Use este objeto somente se você não tiver redirecionado o usuário após a criação do pedido para aprovar o pagamento. Nesses casos, o método de pagamento selecionado pelo usuário no fluxo do PayPal é usado implicitamente.

Respostas
200Uma resposta bem-sucedida a uma solicitação idempotente retorna o 200 OKcódigo de status HTTP com um corpo de resposta JSON que mostra os detalhes do pagamento autorizado.
201Uma resposta bem-sucedida a uma solicitação não idempotente retorna o 201 Createdcódigo de status HTTP com um corpo de resposta JSON que mostra os detalhes de pagamento autorizados. Se uma resposta duplicada for reenviada, o 200 OKcódigo de status HTTP retornado também será retornado. Por padrão, a resposta é mínima. Se você precisar da representação completa do recurso, deverá passar o Prefer: return=representationcabeçalho de solicitação .
403O pagamento autorizado falhou devido a permissões insuficientes.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 201 - Autorizar Pedido - Carteira PayPal como Forma de Pagamento
Exemplo 1 - 201 - Autorizar Pedido - Carteira PayPal como Forma de Pagamento
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T/authorize \ 
-H  'Content-Type: application/json'  \ 
-H  'PayPal-Request-Id: 7b92603e-77ed-4896-8e78-5dea2050476a'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{}' 
Amostras de resposta
200
201
403
422
aplicativo/json

Exemplo 1 - 201 - Autorizar Pedido - Carteira PayPal como Forma de Pagamento
Exemplo 1 - 201 - Autorizar Pedido - Carteira PayPal como Forma de Pagamento
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "COMPLETED",
"payment_source": {
"paypal": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "customer@example.com",
"account_id": "QYR5Z8XDVJNXQ"
}
},
"purchase_units": [
{
"reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
"payments": {
"authorizations": [
{
"id": "3C679366HH908993F",
"status": "CREATED",
"amount": {
"currency_code": "USD",
"value": "100.00"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"ITEM_NOT_RECEIVED",
"UNAUTHORIZED_TRANSACTION"
]
},
"expiration_time": "2021-10-08T23:37:39Z",
"links": [
{
"href": "https://api-m.paypal.com/v2/payments/authorizations/5O190127TN364715T",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v2/payments/authorizations/5O190127TN364715T/capture",
"rel": "capture",
"method": "POST"
},
{
"href": "https://api-m.paypal.com/v2/payments/authorizations/5O190127TN364715T/void",
"rel": "void",
"method": "POST"
},
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "up",
"method": "GET"
}
]
}
]
}
}
],
"payer": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "customer@example.com",
"payer_id": "QYR5Z8XDVJNXQ"
},
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
}
]
}
Capturar pagamento do pedido
publicar
/v2/finalizar compra/pedidos/{id}/captura
Experimente
Captura o pagamento de um pedido. Para capturar o pagamento de um pedido com sucesso, o comprador deve primeiro aprovar o pedido ou fornecer uma fonte de pagamento válida na solicitação. O comprador pode aprovar o pedido ao ser redirecionado para a URL rel:approve retornada nos links HATEOAS na resposta de criação do pedido.

Nota: Para tratamento de erros e resolução de problemas, consulte Erros de pedidos v2 .
Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido para o qual o pagamento deve ser registrado.

Parâmetros do cabeçalho
ID da solicitação do PayPal
string [ 1 .. 108 ] caracteres
O servidor armazena as chaves por 6 horas. Os usuários da API podem solicitar que o período de armazenamento seja estendido para até 72 horas, entrando em contato com seu Gerente de Contas. Essa configuração é obrigatória para todas as solicitações de criação de pedidos em uma única etapa (por exemplo, solicitação de criação de pedido com informações da fonte de pagamento, como cartão, PayPal.vault_id, PayPal.billing_agreement_id etc.).

Preferência
string [ 1 .. 25 ] caracteres ^[a-zA-Z=,-]*$
Padrão: 
retorno=mínimo
Resposta preferencial do servidor após a conclusão bem-sucedida da solicitação. O valor é:

return=minimalO servidor retorna uma resposta mínima para otimizar a comunicação entre o chamador da API e o servidor. Uma resposta mínima inclui os links `<username> id`, ` status<username>` e `<username>`.
return=representationO servidor retorna uma representação completa do recurso, incluindo o estado atual do recurso.
ID de metadados do cliente PayPal
string [ 1 .. 36 ] caracteres
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição:
aplicativo/json
aplicativo/json

fonte_de_pagamento
objeto
Definição da fonte de pagamento.

Respostas
200Uma resposta bem-sucedida a uma solicitação idempotente retorna o 200 OKcódigo de status HTTP com um corpo de resposta JSON que mostra os detalhes do pagamento capturados.
201Uma resposta bem-sucedida a uma solicitação não idempotente retorna o 201 Createdcódigo de status HTTP com um corpo de resposta JSON que mostra os detalhes do pagamento capturados. Se uma resposta duplicada for reenviada, o 200 OKcódigo de status HTTP retornado também será retornado. Por padrão, a resposta é mínima. Se você precisar da representação completa do recurso, passe o Prefer: return=representationcabeçalho de solicitação .
403O pagamento autorizado falhou devido a permissões insuficientes.
404O recurso especificado não existe.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 201 - Capturar Pedido - Carteira PayPal como Fonte de Pagamento
Exemplo 1 - 201 - Capturar Pedido - Carteira PayPal como Fonte de Pagamento
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T/capture \ 
-H  'Content-Type: application/json'  \ 
-H  'PayPal-Request-Id: 7b92603e-77ed-4896-8e78-5dea2050476a'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{}' 
Amostras de resposta
200
201
403
404
422
aplicativo/json

Exemplo 1 - 201 - Capturar Pedido - Carteira PayPal como Fonte de Pagamento
Exemplo 1 - 201 - Capturar Pedido - Carteira PayPal como Fonte de Pagamento
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "COMPLETED",
"payment_source": {
"paypal": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "customer@example.com",
"account_id": "QYR5Z8XDVJNXQ"
}
},
"purchase_units": [
{
"reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
"shipping": {
"address": {
"address_line_1": "2211 N First Street",
"address_line_2": "Building 17",
"admin_area_2": "San Jose",
"admin_area_1": "CA",
"postal_code": "95131",
"country_code": "US"
}
},
"payments": {
"captures": [
{
"id": "3C679366HH908993F",
"status": "COMPLETED",
"amount": {
"currency_code": "USD",
"value": "100.00"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"ITEM_NOT_RECEIVED",
"UNAUTHORIZED_TRANSACTION"
]
},
"final_capture": true,
"disbursement_mode": "INSTANT",
"seller_receivable_breakdown": {
"gross_amount": {
"currency_code": "USD",
"value": "100.00"
},
"paypal_fee": {
"currency_code": "USD",
"value": "3.00"
},
"net_amount": {
"currency_code": "USD",
"value": "97.00"
}
},
"create_time": "2018-04-01T21:20:49Z",
"update_time": "2018-04-01T21:20:49Z",
"links": [
{
"href": "https://api-m.paypal.com/v2/payments/captures/3C679366HH908993F",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v2/payments/captures/3C679366HH908993F/refund",
"rel": "refund",
"method": "POST"
}
]
}
]
}
}
],
"payer": {
"name": {
"given_name": "John",
"surname": "Doe"
},
"email_address": "customer@example.com",
"payer_id": "QYR5Z8XDVJNXQ"
},
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
}
]
}
Adicionar informações de rastreamento para um pedido.
publicar
/v2/finalizar compra/pedidos/{id}/rastrear
Experimente
Adiciona informações de rastreamento para um pedido.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido ao qual as informações de rastreamento estão associadas.

Parâmetros do cabeçalho
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
obrigatório
número_de_rastreamento
obrigatório
string [ 1 .. 64 ] caracteres
O número de rastreamento da remessa. Esta propriedade é compatível com Unicode.

nome_da_operadora_outra
string [ 1 .. 64 ] caracteres
Nome da transportadora para a remessa. Forneça este valor somente se o parâmetro da transportadora for OUTRA. Esta propriedade aceita Unicode.

operadora
obrigatório
string [ 1 .. 64 ] caracteres ^[0-9A-Z_]+$
A transportadora para a remessa. Algumas transportadoras têm uma versão global, bem como subsidiárias locais. As subsidiárias se repetem em vários países e também podem ter uma entrada na lista global. Escolha a transportadora para o seu país. Se a transportadora não estiver disponível para o seu país, escolha a versão global da transportadora. Se o nome da sua transportadora não estiver na lista, defina carriercomo OTHERe insira o nome da transportadora em carrier_name_other. Para valores permitidos, consulte Transportadoras .

Valor de enumeração	Descrição
DPD_RU	DPD Rússia .
BG_POSTAGEM_BÚLGARA	Correios búlgaros .
KR_KOREA_POST	Koreapost (www.koreapost.go.kr) .
ZA_COURIERIT	Courier IT .
FR_EXAPAQ	DPD França (antiga Exapaq) .
SÃO_EMIRATES_POST	Correios dos Emirados .
GAC	GAC .
GEIS	Geis CZ .
SF_EX	SF Express .
PAGO	Pago Logistics .
MYHERMES	MyHermes Reino Unido .
DIAMANTE_EUROGÍSTICOS	Diamond Eurogistics Limitada .
CORPORATECOURIERS_WEBHOOK	Serviços de entrega corporativa .
LIGAÇÃO	Correio de títulos .
OMNIPARCEL	Omni Parcel .
SK_POSTA	Slovenska pošta .
PUROLATOR	purolator .
FETCHR_WEBHOOK	Mena 360 (Fetchr) .
THEDELIVERYGROUP	TDG – O Grupo de Entregas .
CELLO_SQUARE	Quadrado de violoncelo .
TARIFA	TONDA GLOBAL .
ENTREGA	MDS Collivery Pty (Ltd) .
FRETE PRINCIPAL	Frete principal .
IND_FIRSTFLIGHT	First Flight Couriers .
ACSWORLDWIDE	ACS Worldwide Express .
AMSTÃO	Logística de Amsterdã .
OKPARCEL	OkayParcel .
ENVIALIA_REFERÊNCIA	Referência Envialia .
SEUR_ES	Seu Espanha .
CONTINENTAL	Continental .
FDSEXPRESS	FDSEXPRESS .
AMAZON_FBA_SWISHIP	Swiship Reino Unido .
WYNGS	Wyngs .
DHL_RASTREAMENTO_ATIVO	Rastreamento ativo da DHL .
ZILETO	Zyllem .
RUSTON	Ruston .
XPOST	Xpost.ph .
CORREOS_ES	Correos Express (www.correos.es) .
DHL_FR	DHL França (www.dhl.com) .
PAN_ÁSIA	Pan-Ásia Internacional .
BRT_IT	BRT Couriers Itália (www.brt.it) .
SRE_COREIA	SRE Coreia (www.srekorea.co.kr) .
VELOCIDADE	Entrega Spee-Dee .
TNT_Reino Unido	TNT UK Limited (www.tnt.com) .
VENIPAK	Venipak .
SHREENANDANCOURIER	SHREE NANDAN COURIER .
CROSHOT	Croshot .
NIPOST_NG	NIpost (www.nipost.gov.ng) .
EPST_GLBL	ePost Global .
NEWGISTICS	Newgistics .
POST_ESLOVÊNIA	Post of Slovenia.
JERSEY_POST	Jersey Post.
BOMBINOEXP	Bombino Express Pvt.
WMG	WMG Delivery.
XQ_EXPRESS	XQ Express.
FURDECO	Furdeco.
LHT_EXPRESS	LHT Express.
SOUTH_AFRICAN_POST_OFFICE	South African Post Office.
SPOTON	SPOTON Logistics Pvt Ltd.
DIMERCO	Dimerco Express Group.
CYPRUS_POST_CYP	cyprus post.
ABCUSTOM	AB Custom Group.
IND_DELIVREE	deliverE.
CN_BESTEXPRESS	Best Express.
DX_SFTP	DX (SFTP).
PICKUPP_MYS	PICK UPP.
FMX	FMX.
HELLMANN	Hellmann Worldwide Logistics.
SHIP_IT_ASIA	Ship It Asia.
KERRY_ECOMMERCE	Kerry eCommerce.
FRETERAPIDO	Frete Rapido.
PITNEY_BOWES	Pitney Bowes.
XPRESSEN_DK	Xpressen courier.
SEUR_SP_API	Spanish Seur API.
DELIVERYONTIME	DELIVERYONTIME LOGISTICS PVT LTD.
JINSUNG	JINSUNG TRADING.
TRANS_KARGO	Trans Kargo Internasional.
SWISHIP_DE	Swiship DE.
IVOY_WEBHOOK	Ivoy courier.
AIRMEE_WEBHOOK	Airmee couriers.
DHL_BENELUX	dhl benelux.
FIRSTMILE	FirstMile.
FASTWAY_IR	Fastway Ireland.
HH_EXP	Hua Han Logistics.
MYS_MYPOST_ONLINE	Mypostonline.
TNT_NL	THT Netherland.
TIPSA	TIPSA courier.
TAQBIN_MY	TAQBIN Malaysia.
KGMHUB	KGM Hub.
INTEXPRESS	Internet Express.
OVERSE_EXP	Overseas Express.
ONECLICK	One click delivery services.
ROADRUNNER_FREIGHT	Roadbull Logistics.
GLS_CROTIA	GLS Croatia.
MRW_FTP	MRW courier.
BLUEX	Blue Express.
DYLT	Daylight Transport.
DPD_IR	DPD Ireland.
SIN_GLBL	Sin Global Express.
TUFFNELLS_REFERENCE	Tuffnells Parcels Express- Reference.
CJPACKET	CJ Packet.
MILKMAN	Milkman courier.
ASIGNA	ASIGNA courier.
ONEWORLDEXPRESS	One World Express.
ROYAL_MAIL	RoyalShipments.
VIA_EXPRESS	Viaxpress.
TIGFREIGHT	TIG Freight.
ZTO_EXPRESS	ZTO Express.
TWO_GO	2GO Courier.
IML	IML courier.
INTEL_VALLEY	Intel-Valley Supply chain (ShenZhen) Co. Ltd.
EFS	EFS (E-commerce Fulfillment Service).
UK_UK_MAIL	UK mail (ukmail.com).
RAM	RAM courier.
ALLIEDEXPRESS	Allied Express.
APC_OVERNIGHT	APC overnight (apc-overnight.com).
SHIPPIT	Shippit.
TFM	TFM Xpress.
M_XPRESS	M Xpress Sdn Bhd.
HDB_BOX	Haidaibao (BOX).
CLEVY_LINKS	Clevy Links.
IBEONE	Beone Logistics.
FIEGE_NL	Fiege Netherlands.
KWE_GLOBAL	KWE Global.
CTC_EXPRESS	CTC Express.
AMAZON	Amazon Shipping.
MORE_LINK	Morelink.
JX	JX courier.
EASY_MAIL	Easy Mail.
ADUIEPYLE	A Duie Pyle.
GB_PANTHER	Panther.
EXPRESSSALE	Expresssale.
SG_DETRACK	Detrack.
TRUNKRS_WEBHOOK	Trunkrs courier.
MATDESPATCH	Matdespatch.
DICOM	GLS Logistic Systems Canada Ltd./Dicom.
MBW	MBW Courier Inc..
KHM_CAMBODIA_POST	Cambodia Post.
SINOTRANS	Sinotrans.
BRT_IT_PARCELID	BRT Bartolini(Parcel ID).
DHL_SUPPLY_CHAIN	DHL Supply Chain APAC.
DHL_PL	DHL Poland.
TOPYOU	TopYou.
PALEXPRESS	PAL Express Limited.
DHL_SG	dhl Singapore.
CN_WEDO	WeDo Logistics.
FULFILLME	Fulfillme.
DPD_DELISTRACK	DPD delistrack.
UPS_REFERENCE	UPS Reference.
CARIBOU	Caribou.
LOCUS_WEBHOOK	Locus courier.
DSV	DSV courier.
P2P_TRC	P2P TrakPak.
DIRECTPARCELS	Direct Parcels.
NOVA_POSHTA_INT	Nova Poshta (International).
FEDEX_POLAND	FedEx® Poland Domestic.
CN_JCEX	JCEX courier.
FAR_INTERNATIONAL	FAR international.
IDEXPRESS	IDEX courier.
GANGBAO	GANGBAO Supplychain.
NEWAY	Neway Transport.
POSTNL_INT_3_S	PostNL International.
RPX_ID	RPX Indonesia.
DESIGNERTRANSPORT_WEBHOOK	Designer Transport.
GLS_SLOVEN	GLS Slovenia.
PARCELLED_IN	Parcelled.in.
GSI_EXPRESS	GSI EXPRESS.
CON_WAY	Con-way Freight.
BROUWER_TRANSPORT	Brouwer Transport en Logistiek.
CPEX	Captain Express International.
ISRAEL_POST	Israel Post.
DTDC_IN	DTDC India.
PTT_POST	PTT Post.
XDE_WEBHOOK	Ximex Delivery Express.
TOLOS	Tolos courier.
GIAO_HANG	Giao hàng nhanh.
GEODIS_ESPACE	Geodis E-space.
MAGYAR_HU	Magyar Post.
DOORDASH_WEBHOOK	DoorDash.
TIKI_ID	Tiki shipment.
CJ_HK_INTERNATIONAL	CJ Logistics International(Hong Kong).
STAR_TRACK_EXPRESS	Star Track Express.
HELTHJEM	Helthjem.
SFB2C	SF International.
FREIGHTQUOTE	Freightquote by C.H. Robinson.
LANDMARK_GLOBAL_REFERENCE	Landmark Global Reference.
PARCEL2GO	Parcel2Go.
DELNEXT	Delnext.
RCL	Red Carpet Logistics.
CGS_EXPRESS	CGS Express.
HK_POST	Hongkong Post (www.hongkongpost.hk).
SAP_EXPRESS	SAP EXPRESS.
PARCELPOST_SG	Parcel Post Singapore.
HERMES	HermesWorld UK.
IND_SAFEEXPRESS	Safexpress.
TOPHATTEREXPRESS	Tophatter Express.
MGLOBAL	PT MGLOBAL LOGISTICS INDONESIA.
AVERITT	Averitt Express.
LEADER	leader.
_2EBOX	2ebox courier.
SG_SPEEDPOST	Singapore Speedpost.
DBSCHENKER_SE	DB Schenker (www.dbschenker.com).
ISR_POST_DOMESTIC	Israel Post Domestic.
BESTWAYPARCEL	Best Way Parcel.
ASENDIA_DE	asendia_de.
NIGHTLINE_UK	nightline_uk.
TAQBIN_SG	taqbin_sg.
TCK_EXPRESS	TCK Express.
ENDEAVOUR_DELIVERY	Endeavour Delivery.
NANJINGWOYUAN	Nanjing Woyuan.
HEPPNER_FR	Heppner France.
EMPS_CN	EMPS Express.
FONSEN	Fonsen Logistics.
PICKRR	Pickrr.
APC_OVERNIGHT_CONNUM	APC Overnight Consignment.
STAR_TRACK_NEXT_FLIGHT	Star Track Next Flight.
DAJIN	Shanghai Aqrum Chemical Logistics Co.Ltd.
UPS_FREIGHT	UPS Freight.
POSTA_PLUS	Posta Plus.
CEVA	CEVA LOGISTICS.
ANSERX	ANSERX courier.
JS_EXPRESS	JS EXPRESS.
PADTF	padtf.com.
UPS_MAIL_INNOVATIONS	UPS Mail Innovations.
SYPOST	Sunyou Post.
AMAZON_SHIP_MCF	Amazon Shipping + Amazon MCF.
YUSEN	Yusen Logistics.
BRING	Bring.
SDA_IT	SDA Italy.
GBA	GBA Services Ltd.
NEWEGGEXPRESS	Newegg Express.
SPEEDCOURIERS_GR	Speed Couriers.
FORRUN	forrun Pvt Ltd (Arpatech Venture).
PICKUP	Pickupp.
ECMS	ECMS International Logistics Co..
INTELIPOST	Intelipost (TMS for LATAM).
FLASHEXPRESS	Flash Express.
CN_STO	STO Express.
SEKO_SFTP	SEKO Worldwide.
HOME_DELIVERY_SOLUTIONS	Home Delivery Solutions Ltd.
DPD_HGRY	DPD Hungary.
KERRYTTC_VN	Kerry Express (Vietnam) Co Ltd.
JOYING_BOX	Joying Box.
TOTAL_EXPRESS	Total Express.
ZJS_EXPRESS	ZJS International.
STARKEN	STARKEN couriers.
DEMANDSHIP	DemandShip.
CN_DPEX	DPEX.
AUPOST_CN	AuPost China.
LOGISTERS	Logisters.
GOGLOBALPOST	Global Post.
GLS_CZ	GLS Czech Republic.
PAACK_WEBHOOK	Paack courier.
GRAB_WEBHOOK	Grab courier.
PARCELPOINT	Parcelpoint.
ICUMULUS	iCumulus.
DAIGLOBALTRACK	DAI Post.
GLOBAL_IPARCEL	i-parcel.
YURTICI_KARGO	Yurtici Kargo.
CN_PAYPAL_PACKAGE	PayPal Package.
PARCEL_2_POST	Parcel To Post.
GLS_IT	GLS Italy.
PIL_LOGISTICS	PIL Logistics (China) Co..
HEPPNER	Heppner Internationale Spedition GmbH & Co..
GENERAL_OVERNIGHT	Go!Express and logistics.
HAPPY2POINT	Happy 2ThePoint.
CHITCHATS	Chit Chats.
SMOOTH	Smooth Couriers.
CLE_LOGISTICS	CL E-Logistics Solutions Limited.
FIEGE	Fiege Logistics.
MX_CARGO	M&X cargo.
ZIINGFINALMILE	Ziing Final Mile Inc.
DAYTON_FREIGHT	Dayton Freight.
TCS	TCS courier.
AEX	AEX Group.
HERMES_DE	Hermes Germany.
ROUTIFIC_WEBHOOK	Routific.
GLOBAVEND	Globavend.
CJ_LOGISTICS	CJ Logistics International.
PALLET_NETWORK	The Pallet Network.
RAF_PH	RAF Philippines.
UK_XDP	XDP Express.
PAPER_EXPRESS	Paper Express.
LA_POSTE_SUIVI	La Poste.
PAQUETEXPRESS	Paquetexpress.
LIEFERY	liefery.
STRECK_TRANSPORT	Streck Transport.
PONY_EXPRESS	Pony express.
ALWAYS_EXPRESS	Always Express.
GBS_BROKER	GBS-Broker.
CITYLINK_MY	City-Link Express.
ALLJOY	ALLJOY SUPPLY CHAIN.
YODEL	yodel.
YODEL_DIR	Yodel Direct.
STONE3PL	STONE3PL.
PARCELPAL_WEBHOOK	ParcelPal.
DHL_ECOMERCE_ASA	DHL eCommerce Asia (API).
SIMPLYPOST	J&T Express Singapore.
KY_EXPRESS	Kua Yue Express.
SHENZHEN	shenzhen 1st International Logistics(Group)Co.
US_LASERSHIP	LaserShip.
UC_EXPRE	ucexpress.
DIDADI	DIDADI Logistics tech.
CJ_KR	CJ Korea Express.
DBSCHENKER_B2B	DB Schenker B2B.
MXE	MXE Express.
CAE_DELIVERS	CAE Delivers.
PFCEXPRESS	PFC Express.
WHISTL	Whistl.
WEPOST	WePost Sdn Bhd.
DHL_PARCEL_ES	DHL parcel Spain(www.dhl.com).
DDEXPRESS	DD Express Courier.
ARAMEX_AU	Aramex Australia (formerly Fastway AU).
BNEED	Bneed courier.
HK_TGX	Kerry Express Hong Kong.
LATVIJAS_PASTS	Latvijas Pasts.
VIAEUROPE	ViaEurope.
CORREO_UY	Correo Uruguayo.
CHRONOPOST_FR	Chronopost france (www.chronopost.fr).
J_NET	J-Net.
_6LS	6ls.com.
BLR_BELPOST	Belpost.
BIRDSYSTEM	BirdSystem.
DOBROPOST	DobroPost.
WAHANA_ID	Wahana express (www.wahana.com).
WEASHIP	Weaship.
SONICTL	Sonic Transportation & Logistics.
KWT	Shenzhen Jinghuada Logistics Co..
AFLLOG_FTP	AFL LOGISTICS.
SKYNET_WORLDWIDE	SkyNet Worldwide Express.
NOVA_POSHTA	Nova Poshta (novaposhta.ua).
SEINO	Seino.
SZENDEX	SZENDEX.
BPOST_INT	Bpost international.
DBSCHENKER_SV	DB Schenker Sweden.
AO_DEUTSCHLAND	AO Deutschland.
EU_FLEET_SOLUTIONS	EU Fleet Solutions.
PCFCORP	PCF Final Mile.
LINKBRIDGE	Link Bridge(BeiJing)international logistics co..
PRIMAMULTICIPTA	PT Prima Multi Cipta.
COUREX	Urbanfox.
ZAJIL_EXPRESS	Zajil Express Company.
COLLECTCO	CollectCo.
JTEXPRESS	J&T EXPRESS MALAYSIA.
FEDEX_UK	FedEx® UK.
USHIP	uShip courier.
PIXSELL	PIXSELL LOGISTICS.
SHIPTOR	Shiptor.
CDEK	CDEK courier.
VNM_VIETTELPOST	ViettelPost.
CJ_CENTURY	CJ Century.
GSO	GSO(GLS-USA).
VIWO	VIWO IoT.
SKYBOX	SKYBOX.
KERRYTJ	Kerry TJ Logistics.
NTLOGISTICS_VN	Nhat Tin Logistics.
SDH_SCM	lightning monkey.
ZINC	Zinc courier.
DPE_SOUTH_AFRC	DPE South Africa.
CESKA_CZ	Czech Post.
ACS_GR	ACS Courier.
DEALERSEND	DealerSend.
JOCOM	Jocom.
CSE	CSE courier.
TFORCE_FINALMILE	TForce Final Mile.
SHIP_GATE	ShipGate.
SHIPTER	SHIPTER.
NATIONAL_SAMEDAY	National Sameday.
YUNEXPRESS	YunExpress.
CAINIAO	AliExpress Standard Shipping.
DMS_MATRIX	DMSMatrix.
DIRECTLOG	Directlog (www.directlog.com.br).
ASENDIA_US	Asendia USA.
_3JMSLOGISTICS	3JMS Logistics.
LICCARDI_EXPRESS	LICCARDI EXPRESS COURIER.
SKY_POSTAL	SkyPostal.
CNWANGTONG	cnwangtong.
POSTNORD_LOGISTICS_DK	ostnord denmark.
LOGISTIKA	Logistika.
CELERITAS	Celeritas Transporte.
PRESSIODE	Pressio.
SHREE_MARUTI	Shree Maruti Courier Services Pvt Ltd.
LOGISTICSWORLDWIDE_HK	Logistic Worldwide Express (LWE Honkong).
EFEX	eFEx (E-Commerce Fulfillment & Express).
LOTTE	Lotte Global Logistics.
LONESTAR	Lone Star Overnight.
APRISAEXPRESS	Aprisa Express.
BEL_RS	BEL North Russia.
OSM_WORLDWIDE	OSM Worldwide.
WESTGATE_GL	Westgate Global.
FASTRACK	Fasttrack.
DTD_EXPR	DTD Express.
ALFATREX	AlfaTrex.
PROMEDDELIVERY	ProMed Delivery.
THABIT_LOGISTICS	Thabit Logistics.
HCT_LOGISTICS	HCT LOGISTICS CO.LTD..
CARRY_FLAP	Carry-Flap Co..
US_OLD_DOMINION	Old Dominion Freight Line.
ANICAM_BOX	ANICAM BOX EXPRESS.
WANBEXPRESS	WanbExpress.
AN_POST	An Post.
DPD_LOCAL	DPD Local.
STALLIONEXPRESS	Stallion Express.
RAIDEREX	RaidereX.
SHOPFANS	ShopfansRU LLC.
KYUNGDONG_PARCEL	Kyungdong Parcel.
CHAMPION_LOGISTICS	Champion Logistics.
PICKUPP_SGP	PICK UPP (Singapore).
MORNING_EXPRESS	Morning Express.
NACEX	NACEX.
THENILE_WEBHOOK	SortHub courier.
HOLISOL	Holisol.
LBCEXPRESS_FTP	LBC EXPRESS INC..
KURASI	KURASI.
USF_REDDAWAY	USF Reddaway.
APG	APG eCommerce Solutions.
CN_BOXC	BoxC courier.
ECOSCOOTING	ECOSCOOTING.
MAINWAY	Mainway.
PAPERFLY	Paperfly Private Limited.
HOUNDEXPRESS	Hound Express.
BOX_BERRY	Boxberry courier.
EP_BOX	EP-Box courier.
PLUS_LOG_UK	Plus UK Logistics.
FULFILLA	Fulfilla.
ASE	ASE KARGO.
MAIL_PLUS	MailPlus.
XPO_LOGISTICS	XPO logistics.
WNDIRECT	wnDirect.
CLOUDWISH_ASIA	Cloudwish Asia.
ZELERIS	Zeleris.
GIO_EXPRESS	Gio Express.
OCS_WORLDWIDE	OCS WORLDWIDE.
ARK_LOGISTICS	ARK Logistics.
AQUILINE	Aquiline.
PILOT_FREIGHT	Pilot Freight Services.
QWINTRY	Qwintry Logistics.
DANSKE_FRAGT	Danske Fragtaend.
CARRIERS	Carriers courier.
AIR_CANADA_GLOBAL	Rivo (Air canada).
PRESIDENT_TRANS	PRESIDENT TRANSNET CORP.
STEPFORWARDFS	STEP FORWARD FREIGHT SERVICE CO LTD.
SKYNET_UK	Skynet UK.
PITTOHIO	PITT OHIO.
CORREOS_EXPRESS	Correos Express.
RL_US	RL Carriers.
DESTINY	Destiny Transportation.
UK_YODEL	Yodel (www.yodel.co.uk).
COMET_TECH	CometTech.
DHL_PARCEL_RU	DHL Parcel Russia.
TNT_REFR	TNT Reference.
SHREE_ANJANI_COURIER	Shree Anjani Courier.
MIKROPAKKET_BE	Mikropakket Belgium.
ETS_EXPRESS	RETS express.
COLIS_PRIVE	Colis Privé.
CN_YUNDA	Yunda Express.
AAA_COOPER	AAA Cooper.
ROCKET_PARCEL	Rocket Parcel International.
_360LION	360 Lion Express.
PANDU	PANDU.
PROFESSIONAL_COURIERS	PROFESSIONAL COURIERS.
FLYTEXPRESS	FLYTEXPRESS.
LOGISTICSWORLDWIDE_MY	LOGISTICSWORLDWIDE MY.
CORREOS_DE_ESPANA	CORREOS DE ESPANA.
IMX	IMX.
FOUR_PX_EXPRESS	FOUR PX EXPRESS.
XPRESSBEES	XPRESSBEES.
PICKUPP_VNM	pickupp_vnm.
STARTRACK_EXPRESS	startrack_express.
FR_COLISSIMO	fr_colissimo.
NACEX_SPAIN_REFERENCE	nacex_spain_reference.
DHL_SUPPLY_CHAIN_AU	dhl_supply_chain_au.
ESHIPPING	Eshipping.
SHREETIRUPATI	SHREE TIRUPATI COURIER SERVICES PVT. LTD..
HX_EXPRESS	HX Express.
INDOPAKET	INDOPAKET.
CN_17POST	17 Post Service.
K1_EXPRESS	K1 Express.
CJ_GLS	CJ GLS.
MYS_GDEX	GDEX courier.
NATIONEX	Nationex courier.
ANJUN	Anjun couriers.
FARGOOD	FarGood.
SMG_EXPRESS	SMG Direct.
RZYEXPRESS	RZY Express.
SEFL	Southeastern Freight Lines.
TNT_CLICK_IT	TNT-Click Italy.
HDB	Haidaibao.
HIPSHIPPER	Hipshipper.
RPXLOGISTICS	RPX Logistics.
KUEHNE	Kuehne + Nagel.
IT_NEXIVE	Nexive (TNT Post Italy).
PTS	PTS courier.
SWISS_POST_FTP	Swiss Post FTP.
FASTRK_SERV	Fastrak Services.
_4_72	4-72 Entregando.
US_YRC	YRC courier.
POSTNL_INTL_3S	PostNL International 3S.
ELIAN_POST	Yilian (Elian) Supply Chain.
CUBYN	Cubyn.
SAU_SAUDI_POST	Saudi Post.
ABXEXPRESS_MY	ABX Express.
HUAHAN_EXPRESS	HUAHANG EXPRESS.
ZES_EXPRESS	Eshun international Logistic.
ZEPTO_EXPRESS	ZeptoExpress.
SKYNET_ZA	Skynet World Wide Express South Africa.
ZEEK_2_DOOR	Zeek2Door.
BLINKLASTMILE	Blink.
POSTA_UKR	UkrPoshta.
CHROBINSON	C.H. Robinson Worldwide.
CN_POST56	Post56.
COURANT_PLUS	Courant Plus.
SCUDEX_EXPRESS	Scudex Express.
SHIPENTEGRA	ShipEntegra.
B_TWO_C_EUROPE	B2C courier Europe.
COPE	Cope Sensitive Freight.
IND_GATI	Gati-KWE.
CN_WISHPOST	WishPost.
NACEX_ES	NACEX Spain.
TAQBIN_HK	TAQBIN Hong Kong.
GLOBALTRANZ	GlobalTranz.
HKD	Qingdao HKD International Logistics.
BJSHOMEDELIVERY	BJS Distribution courier.
OMNIVA	Omniva.
SUTTON	Sutton Transport.
PANTHER_REFERENCE	Panther Reference.
SFCSERVICE	SFC Service.
LTL	LTL COURIER.
PARKNPARCEL	Park N Parcel.
SPRING_GDS	Spring GDS.
ECEXPRESS	ECexpress.
INTERPARCEL_AU	Interparcel Australia.
AGILITY	Agility.
XL_EXPRESS	XL Express.
ADERONLINE	Ader couriers.
DIRECTCOURIERS	Direct Couriers.
PLANZER	Planzer Group.
SENDING	Sending Transporte Urgente y Comunicacion.
NINJAVAN_WB	Ninjavan Webhook.
NATIONWIDE_MY	Nationwide Express Courier Services Bhd (www.nationwide.com.my).
SENDIT	Sendit.
GB_ARROW	Arrow XL.
IND_GOJAVAS	GoJavas.
KPOST	Korea Post.
DHL_FREIGHT	DHL Freight.
BLUECARE	Bluecare Express Ltd.
JINDOUYUN	jindouyun courier.
TRACKON	Trackon Couriers Pvt. Ltd.
GB_TUFFNELLS	Tuffnells Parcels Express.
TRUMPCARD	TRUMPCARD LLC.
ETOTAL	eTotal Solution Limited.
SFPLUS_WEBHOOK	Zeek courier.
SEKOLOGISTICS	SEKO Logistics.
HERMES_2MANN_HANDLING	Hermes Einrichtungs Service GmbH & Co. KG.
DPD_LOCAL_REF	DPD Local reference.
UDS	United Delivery Service.
ZA_SPECIALISED_FREIGHT	Specialised Freight.
THA_KERRY	Kerry Express Thailand.
PRT_INT_SEUR	SEUR International.
BRA_CORREIOS	Correios Brazil.
NZ_NZ_POST	New Zealand Post.
CN_EQUICK	Equick China.
MYS_EMS	Malaysia Post EMS / Pos Laju.
GB_NORSK	Norsk Global.
ESP_MRW	MRW spain.
ESP_PACKLINK	Packlink.
KANGAROO_MY	Kangaroo Worldwide Express.
RPX	RPX Online.
XDP_UK_REFERENCE	XDP Express Reference.
NINJAVAN_MY	ninja van (www.ninjavan.co).
ADICIONAL	Adicional Logistics.
ROADBULL	Red Carpet Logistics.
YAKIT	Yakit courier.
MAILAMERICAS	MailAmericas.
MIKROPAKKET	Mikropakket.
DYNALOGIC	Dynamic Logistics.
DHL_ES	DHL Spain(www.dhl.com).
DHL_PARCEL_NL	DHL Parcel NL.
DHL_GLOBAL_MAIL_ASIA	DHL Global Mail Asia (www.dhl.com).
DAWN_WING	Dawn Wing.
GENIKI_GR	Geniki Taxydromiki.
HERMESWORLD_UK	hermesworld_uk.
ALPHAFAST	Alphafast (www.alphafast.com).
BUYLOGIC	buylogic.
EKART	Ekart logistics (ekartlogistics.com).
MEX_SENDA	mexico senda express.
SFC_LOGISTICS	SFC.
POST_SERBIA	Posta Serbia.
IND_DELHIVERY	Delhivery India.
DE_DPD_DELISTRACK	DPD Germany.
RPD2MAN	RPD2man Deliveries.
CN_SF_EXPRESS	SF Express (www.sf-express.com).
YANWEN	Yanwen Logistics.
MYS_SKYNET	Skynet Malaysia.
CORREOS_DE_MEXICO	correos mexico.
CBL_LOGISTICA	CBL Logistica.
MEX_ESTAFETA	Estafeta (www.estafeta.com).
AU_AUSTRIAN_POST	Austrian Post (Registered).
RINCOS	Rincos.
NLD_DHL	DHL Netherland.
RUSSIAN_POST	Russian post.
COURIERS_PLEASE	CouriersPlease (couriersplease.com.au).
POSTNORD_LOGISTICS	PostNord Logistics.
FEDEX	Fedex.
DPE_EXPRESS	DPE Express.
DPD	DPD.
ADSONE	ADSone.
IDN_JNE	JNE Express (Jalur Nugraha Ekakurir).
THECOURIERGUY	The Courier Guy.
CNEXPS	CNE Express.
PRT_CHRONOPOST	Chronopost Portugal.
LANDMARK_GLOBAL	Landmark Global.
IT_DHL_ECOMMERCE	DHL International.
ESP_NACEX	NACEX Spain.
PRT_CTT	CTT Portugal.
BE_KIALA	Kiala.
ASENDIA_UK	Asendia UK.
GLOBAL_TNT	TNT global.
POSTUR_IS	Iceland Post.
EPARCEL_KR	eParcel Korea.
INPOST_PACZKOMATY	InPost Paczkomaty.
IT_POSTE_ITALIA	Poste italiane (www.poste.it).
BE_BPOST	Bpost (www.bpost.be).
PL_POCZTA_POLSKA	Poczta Polska (www.poczta-polska.pl).
MYS_MYS_POST	Malaysia Post.
SG_SG_POST	Singapore Post.
THA_THAILAND_POST	Thailand Post (www.thailandpost.co.th).
LEXSHIP	LexShip.
FASTWAY_NZ	Fastway New Zealand.
DHL_AU	DHL Supply Chain Australia.
COSTMETICSNOW	Cosmetics Now.
PFLOGISTICS	PFL.
LOOMIS_EXPRESS	Loomis Express.
GLS_ITALY	GLS Italy.
LINE	Line Clear Express & Logistics Sdn Bhd.
GEL_EXPRESS	Gel Express Logistik.
HUODULL	Huodull.
NINJAVAN_SG	Ninja van Singapore.
JANIO	Janio Asia.
AO_COURIER	AO Logistics.
BRT_IT_SENDER_REF	BRT Bartolini(Sender Reference).
SAILPOST	SAILPOST.
LALAMOVE	Lalamove.
NEWZEALAND_COURIERS	NEW ZEALAND COURIERS.
ETOMARS	Etomars.
VIRTRANSPORT	VIR Transport.
WIZMO	Wizmo.
PALLETWAYS	Palletways.
I_DIKA	i-dika.
CFL_LOGISTICS	CFL Logistics.
GEMWORLDWIDE	GEM Worldwide.
GLOBAL_EXPRESS	Tai Wan Global Business.
LOGISTYX_TRANSGROUP	Transgroup courier.
WESTBANK_COURIER	West Bank Courier.
ARCO_SPEDIZIONI	Arco Spedizioni SP.
YDH_EXPRESS	YDH express.
PARCELINKLOGISTICS	Parcelink Logistics.
CNDEXPRESS	CND Express.
NOX_NIGHT_TIME_EXPRESS	NOX NightTimeExpress.
AERONET	Aeronet couriers.
LTIANEXP	LTIAN EXP.
INTEGRA2_FTP	Integra2.
PARCELONE	PARCEL ONE.
NOX_NACHTEXPRESS	Innight Express Germany GmbH (nox NachtExpress).
CN_CHINA_POST_EMS	China Post.
CHUKOU1	Chukou1.
GLS_SLOV	GLS General Logistics Systems Slovakia s.r.o..
ORANGE_DS	OrangeDS (Orange Distribution Solutions Inc).
JOOM_LOGIS	Joom Logistics.
AUS_STARTRACK	StarTrack (startrack.com.au).
DHL	dhl Global.
GB_APC	APC postal logistics germany.
BONDSCOURIERS	Bonds Courier Service (bondscouriers.com.au).
JPN_JAPAN_POST	Japan Post.
USPS	United States Postal Service.
WINIT	WinIt.
ARG_OCA	OCA Argentina.
TW_TAIWAN_POST	Taiwan Post.
DMM_NETWORK	DMM Network.
TNT	TNT Express.
BH_POSTA	BH Posta (www.posta.ba).
SWE_POSTNORD	Postnord sweden.
CA_CANADA_POST	Canada Post.
WISELOADS	Wiseloads.
ASENDIA_HK	Asendia HonKong.
NLD_GLS	GLS Netherland.
MEX_REDPACK	Redpack.
JET_SHIP	Jet-Ship Worldwide.
DE_DHL_EXPRESS	DHL Express.
NINJAVAN_THAI	Ninja van Thai.
RABEN_GROUP	Raben Group.
ESP_ASM	ASM(GLS Spain).
HRV_HRVATSKA	Hrvatska posta.
GLOBAL_ESTES	Estes Express Lines.
LTU_LIETUVOS	Lietuvos pastas.
BEL_DHL	DHL Benelux.
AU_AU_POST	Australia Post.
SPEEDEXCOURIER	SPEEDEX couriers.
FR_COLIS	Colissimo.
ARAMEX	Aramex.
DPEX	DPEX (www.dpex.com).
MYS_AIRPAK	Airpak Express.
CUCKOOEXPRESS	Cuckoo Express.
DPD_POLAND	DPD Poland.
NLD_POSTNL	PostNL International.
NIM_EXPRESS	Nim Express.
QUANTIUM	Quantium.
SENDLE	Sendle.
ESP_REDUR	Redur Spain.
MATKAHUOLTO	Matkahuolto.
CPACKET	Cpacket couriers.
POSTI	Posti courier.
HUNTER_EXPRESS	Hunter Express.
CHOIR_EXP	Choir Express Indonesia.
LEGION_EXPRESS	Legion Express.
AUSTRIAN_POST_EXPRESS	austrian post.
GRUPO	Grupo ampm.
POSTA_RO	Post Roman (www.posta-romana.ro).
INTERPARCEL_UK	Interparcel UK.
GLOBAL_ABF	ABF Freight.
POSTEN_NORGE	Posten Norge (www.posten.no).
XPERT_DELIVERY	Xpert Delivery.
DHL_REFR	DHl (Reference number).
DHL_HK	DHL HonKong.
SKYNET_UAE	SKYNET UAE.
GOJEK	Gojek.
YODEL_INTNL	Yodel International.
JANCO	Janco Ecommerce.
YTO	YTO Express.
WISE_EXPRESS	Wise Express.
JTEXPRESS_VN	J&T Express Vietnam.
FEDEX_INTL_MLSERV	FedEx International MailService.
VAMOX	VAMOX.
AMS_GRP	AMS Group.
DHL_JP	DHL Japan.
HRPARCEL	HR Parcel.
GESWL	GESWL Express.
BLUESTAR	Blue Star.
CDEK_TR	CDEK TR.
DESCARTES	Innovel courier.
DELTEC_UK	Deltec Courier.
DTDC_EXPRESS	DTDC express.
TOURLINE	tourline.
BH_WORLDWIDE	B&H Worldwide.
OCS	OCS ANA Group.
YINGNUO_LOGISTICS	yingnuo logistics.
UPS	United Parcel Service.
TOLL	Toll IPEC.
PRT_SEUR	SEUR portugal.
DTDC_AU	DTDC Australia.
THA_DYNAMIC_LOGISTICS	Dynamic Logistics.
UBI_LOGISTICS	UBI Smart Parcel.
FEDEX_CROSSBORDER	FedEx Cross Border.
A1POST	A1Post.
TAZMANIAN_FREIGHT	Tazmanian Freight Systems.
CJ_INT_MY	CJ International malaysia.
SAIA_FREIGHT	Saia LTL Freight.
SG_QXPRESS	Qxpress.
NHANS_SOLUTIONS	Nhans Solutions.
DPD_FR	DPD France.
COORDINADORA	Coordinadora.
ANDREANI	Grupo logistico Andreani.
DOORA	Doora Logistics.
INTERPARCEL_NZ	Interparcel New Zealand.
PHL_JAMEXPRESS	Jam Express Philippines.
BEL_BELGIUM_POST	bel_belgium_post.
US_APC	us_apc.
IDN_POS	idn_pos.
FR_MONDIAL	fr_mondial.
DE_DHL	DE DHL.
HK_RPX	hk_rpx.
DHL_PIECEID	dhl_pieceid.
VNPOST_EMS	vnpost_ems.
RRDONNELLEY	rrdonnelley.
DPD_DE	dpd_de.
DELCART_IN	delcart_in.
IMEXGLOBALSOLUTIONS	imexglobalsolutions.
ACOMMERCE	ACOMMERCE.
EURODIS	eurodis.
CANPAR	CANPAR.
GLS	GLS.
IND_ECOM	Ecom Express.
ESP_ENVIALIA	Envialia.
DHL_UK	dhl UK.
SMSA_EXPRESS	SMSA Express.
TNT_FR	TNT France.
DEX_I	DEX-I courier.
BUDBEE_WEBHOOK	Budbee courier.
COPA_COURIER	Copa Airlines Courier.
VNM_VIETNAM_POST	Vietnam Post.
DPD_HK	DPD HongKong.
TOLL_NZ	Toll New Zealand.
ECHO	Echo courier.
FEDEX_FR	FedEx® Freight.
BORDEREXPRESS	Border Express.
MAILPLUS_JPN	MailPlus (Japan).
TNT_UK_REFR	TNT UK Reference.
KEC	KEC courier.
DPD_RO	DPD Romania.
TNT_JP	TNT_JP.
TH_CJ	TH_CJ.
EC_CN	EC_CN.
FASTWAY_UK	FASTWAY_UK.
FASTWAY_US	FASTWAY_US.
GLS_DE	GLS_DE.
GLS_ES	GLS_ES.
GLS_FR	GLS_FR.
MONDIAL_BE	MONDIAL_BE.
SGT_IT	SGT_IT.
TNT_CN	TNT_CN.
TNT_DE	TNT_DE.
TNT_ES	TNT_ES.
TNT_PL	TNT_PL.
PARCELFORCE	PARCELFORCE.
SWISS_POST	SWISS POST.
TOLL_IPEC	TOLL IPEC.
AIR_21	AIR 21.
AIRSPEED	AIRSPEED.
BERT	BERT.
BLUEDART	BLUEDART.
COLLECTPLUS	COLLECTPLUS.
COURIERPLUS	COURIERPLUS.
COURIER_POST	COURIER POST.
DHL_GLOBAL_MAIL	dhl_global_mail.
DPD_UK	dpd_uk.
DELTEC_DE	DELTEC DE.
DEUTSCHE_DE	deutsche_de.
DOTZOT	DOTZOT.
ELTA_GR	elta_gr.
EMS_CN	ems_cn.
ECARGO	ECARGO.
ENSENDA	ENSENDA.
FERCAM_IT	fercam_it.
FASTWAY_ZA	fastway_za.
FASTWAY_AU	fastway_au.
FIRST_LOGISITCS	first_logisitcs.
GEODIS	GEODIS.
GLOBEGISTICS	GLOBEGISTICS.
GREYHOUND	GREYHOUND.
JETSHIP_MY	jetship_my.
LION_PARCEL	LION PARCEL.
AEROFLASH	AEROFLASH.
ONTRAC	ONTRAC.
SAGAWA	SAGAWA.
SIODEMKA	SIODEMKA.
STARTRACK	startrack.
TNT_AU	tnt_au.
TNT_IT	tnt_it.
TRANSMISSION	TRANSMISSION.
YAMATO	YAMATO.
DHL_IT	dhl_it.
DHL_AT	dhl_at.
LOGISTICSWORLDWIDE_KR	LOGISTICSWORLDWIDE KR.
GLS_SPAIN	gls_spain.
AMAZON_UK_API	amazon_uk_api.
DPD_FR_REFERENCE	dpd_fr_reference.
DHLPARCEL_UK	dhlparcel_uk.
MEGASAVE	megasave.
QUALITYPOST	qualitypost.
IDS_LOGISTICS	ids_logistics.
JOYINGBOX	joyingbox.
PANTHER_ORDER_NUMBER	panther_order_number.
WATKINS_SHEPARD	watkins_shepard.
FASTTRACK	fasttrack.
UP_EXPRESS	up_express.
ELOGISTICA	elogistica.
ECOURIER	ecourier.
CJ_PHILIPPINES	cj_philippines.
SPEEDEX	speedex.
ORANGECONNEX	orangeconnex.
TECOR	tecor.
SAEE	saee.
GLS_ITALY_FTP	gls_italy_ftp.
DELIVERE	delivere.
YYCOM	yycom.
ADICIONAL_PT	Adicional Logistics.
DKSH	DKSH.
NIPPON_EXPRESS_FTP	Nippon Express.
GOLS	GO Logistics & Storage.
FUJEXP	FUJIE EXPRESS.
QTRACK	QTrack.
OMLOGISTICS_API	OM LOGISTICS LTD.
GDPHARM	GDPharm Logistics.
MISUMI_CN	MISUMI Group Inc..
AIR_CANADA	Rivo.
CITY56_WEBHOOK	City Express.
SAGAWA_API	Sagawa.
KEDAEX	KedaEX.
PGEON_API	Pgeon.
WEWORLDEXPRESS	We World Express.
JT_LOGISTICS	J&T International logistics.
TRUSK	Trusk France.
VIAXPRESS	ViaXpress.
DHL_SUPPLYCHAIN_ID	DHL Supply Chain Indonesia.
ZUELLIGPHARMA_SFTP	Zuellig Pharma Korea.
MEEST	Meest.
TOLL_PRIORITY	Toll Priority.
MOTHERSHIP_API	Mothership.
CAPITAL	Capital Transport.
EUROPAKET_API	Europacket+.
HFD	HFD.
TOURLINE_REFERENCE	Tourline Express.
GIO_ECOURIER	GIO Express Inc.
CN_LOGISTICS	CN Logistics.
PANDION	Pandion.
BPOST_API	Bpost API.
PASSPORTSHIPPING	Passport Shipping.
PAKAJO	Pakajo World.
DACHSER	DACHSER.
YUSEN_SFTP	Yusen Logistics.
SHYPLITE	Shypmax.
XYY	Xingyunyi Logistics.
MWD	Metropolitan Warehouse & Delivery.
FAXECARGO	Faxe Cargo.
MAZET	Groupe Mazet.
FIRST_LOGISTICS_API	First Logistics.
SPRINT_PACK	SPRINT PACK.
HERMES_DE_FTP	Hermes Germany.
CONCISE	Concise.
KERRY_EXPRESS_TW_API	Kerry Express TaiWan.
EWE	EWE Global Express.
FASTDESPATCH	Fast Despatch Logistics Limited.
ABCUSTOM_SFTP	AB Custom Group.
CHAZKI	Chazki.
SHIPPIE	Shippie.
GEODIS_API	GEODIS - Distribution & Express.
NAQEL_EXPRESS	Naqel Express.
PAPA_WEBHOOK	Papa.
FORWARDAIR	Forward Air.
DIALOGO_LOGISTICA_API	Dialogo Logistica.
LALAMOVE_API	Lalamove.
TOMYDOOR	Tomydoor.
KRONOS_WEBHOOK	Kronos Express.
JTCARGO	J&T CARGO.
T_CAT	T-cat.
CONCISE_WEBHOOK	Concise.
TELEPORT_WEBHOOK	Teleport.
CUSTOMCO_API	The Custom Companies.
SPX_TH	Shopee Xpress.
BOLLORE_LOGISTICS	Bollore Logistics.
CLICKLINK_SFTP	ClickLink.
M3LOGISTICS	M3 Logistics.
VNPOST_API	Vietnam Post.
AXLEHIRE_FTP	Axlehire.
SHADOWFAX	Shadowfax.
MYHERMES_UK_API	EVRi.
DAIICHI	Daiichi Freight System Inc.
MENSAJEROSURBANOS_API	Mensajeros Urbanos.
POLARSPEED	PolarSpeed Inc.
IDEXPRESS_ID	iDexpress Indonesia.
PAYO	Payo.
WHISTL_SFTP	Whistl.
INTEX_DE	INTEX Paketdienst GmbH.
TRANS2U	Trans2u.
PRODUCTCAREGROUP_SFTP	Product Care Services Limited.
BIGSMART	Big Smart.
EXPEDITORS_API_REF	Expeditors API Reference.
AITWORLDWIDE_API	AIT.
WORLDCOURIER	World Courier.
QUIQUP	Quiqup.
AGEDISS_SFTP	Agediss.
ANDREANI_API	Andreani.
CRLEXPRESS	CRL Express.
SMARTCAT	SMARTCAT.
CROSSFLIGHT	Crossflight Limited.
PROCARRIER	Pro Carrier.
DHL_REFERENCE_API	DHL (Reference number).
SEINO_API	Seino.
WSPEXPRESS	WSP Express.
KRONOS	Kronos Express.
TOTAL_EXPRESS_API	Total Express.
PARCLL	PARCLL.
XPEDIGO	Xpedigo.
STAR_TRACK_WEBHOOK	StarTrack.
GPOST	Georgian Post.
UCS	UCS.
DMFGROUP	DMF.
COORDINADORA_API	Coordinadora.
MARKEN	Marken.
NTL	NTL logistics.
REDJEPAKKETJE	Red je Pakketje.
ALLIED_EXPRESS_FTP	Allied Express (FTP).
MONDIALRELAY_ES	Mondial Relay Spain(Punto Pack).
NAEKO_FTP	Naeko Logistics.
MHI	Mhi.
SHIPPIFY	Shippify, Inc.
MALCA_AMIT_API	Malca Amit.
JTEXPRESS_SG_API	J&T Express Singapore.
DACHSER_WEB	DACHSER.
FLIGHTLG	Flight Logistics Group.
CAGO	Cago.
COM1EXPRESS	ComOne Express.
TONAMI_FTP	Tonami.
PACKFLEET	PACKFLEET.
PUROLATOR_INTERNATIONAL	Purolator International.
WINESHIPPING_WEBHOOK	Wineshipping.
DHL_ES_SFTP	DHL Spain Domestic.
PCHOME_API	網家速配股份有限公司.
CESKAPOSTA_API	Czech Post.
GORUSH	Go Rush.
HOMERUNNER	HomeRunner.
AMAZON_ORDER	Amazon order.
EFWNOW_API	Estes Forwarding Worldwide.
CBL_LOGISTICA_API	CBL Logistica (API).
NIMBUSPOST	NimbusPost.
LOGWIN_LOGISTICS	Logwin Logistics.
NOWLOG_API	Sequoialog.
DPD_NL	DPD Netherlands.
GODEPENDABLE	Dependable Supply Chain Services.
ESDEX	Top Ideal Express.
LOGISYSTEMS_SFTP	Kiitäjät.
EXPEDITORS	Expeditors.
SNTGLOBAL_API	Snt Global Etrax.
SHIPX	ShipX.
QINTL_API	Quickstat Courier LLC.
PACKS	Packs.
POSTNL_INTERNATIONAL	PostNL International.
AMAZON_EMAIL_PUSH	Amazon.
DHL_API	DHL.
SPX	Shopee Express.
AXLEHIRE	AxleHire.
ICSCOURIER	ICS COURIER.
DIALOGO_LOGISTICA	Dialogo Logistica.
SHUNBANG_EXPRESS	ShunBang Express.
TCS_API	TCS.
SF_EXPRESS_CN	SF Express China.
PACKETA	Packeta.
SIC_TELIWAY	Teliway SIC Express.
MONDIALRELAY_FR	Mondial Relay France.
INTIME_FTP	InTime.
JD_EXPRESS	京东物流.
FASTBOX	Fastbox.
PATHEON	Patheon Logistics.
INDIA_POST	India Post Domestic.
TIPSA_REF	Tipsa Reference.
ECOFREIGHT	Eco Freight.
VOX	VOX SOLUCION EMPRESARIAL SRL.
DIRECTFREIGHT_AU_REF	Direct Freight Express.
BESTTRANSPORT_SFTP	Best Transport.
AUSTRALIA_POST_API	Australia Post.
FRAGILEPAK_SFTP	FragilePAK.
FLIPXP	FlipXpress.
VALUE_WEBHOOK	Value Logistics.
DAESHIN	Daeshin.
SHERPA	Sherpa.
MWD_API	Metropolitan Warehouse & Delivery.
SMARTKARGO	SmartKargo.
DNJ_EXPRESS	DNJ Express.
GOPEOPLE	Go People.
MYSENDLE_API	mySendle.
ARAMEX_API	Aramex.
PIDGE	Pidge.
THAIPARCELS	TP Logistic.
PANTHER_REFERENCE_API	Panther Reference.
POSTAPLUS	Posta Plus.
BUFFALO	BUFFALO.
U_ENVIOS	U-ENVIOS.
ELITE_CO	Elite Express.
ROCHE_INTERNAL_SFTP	Roche Internal Courier.
DBSCHENKER_ICELAND	DB Schenker Iceland.
TNT_FR_REFERENCE	TNT France Reference.
NEWGISTICSAPI	Newgistics API.
GLOVO	Glovo.
GWLOGIS_API	G.I.G.
SPREETAIL_API	Spreetail.
MOOVA	Moova.
PLYCONGROUP	Plycon Transportation Group.
USPS_WEBHOOK	USPS Informed Visibility - Webhook.
REIMAGINEDELIVERY	maergo.
EDF_FTP	Eurodifarm.
DAO365	DAO365.
BIOCAIR_FTP	BioCair.
RANSA_WEBHOOK	Ransa.
SHIPXPRES	SHIPXPRESS.
COURANT_PLUS_API	Courant Plus.
SHIPA	SHIPA.
HOMELOGISTICS	Home Logistics.
DX	DX.
POSTE_ITALIANE_PACCOCELERE	Poste Italiane Paccocelere.
TOLL_WEBHOOK	Toll Group.
LCTBR_API	LCT do Brasil.
DX_FREIGHT	DX Freight.
DHL_SFTP	DHL Express.
SHIPROCKET	Shiprocket X.
UBER_WEBHOOK	Uber.
STATOVERNIGHT	Stat Overnight.
BURD	Burd Delivery.
FASTSHIP	Fastship Express.
IBVENTURE_WEBHOOK	IB Venture.
GATI_KWE_API	Gati-KWE.
CRYOPDP_FTP	CryoPDP.
HUBBED	HUBBED.
TIPSA_API	Tipsa API.
ARASKARGO	Aras Cargo.
THIJS_NL	Thijs Logistiek.
ATSHEALTHCARE_REFERENCE	ATS Healthcare.
99MINUTOS	99minutos.
HELLENIC_POST	Hellenic (Greece) Post.
HSM_GLOBAL	HSM Global.
MNX	MNX.
NMTRANSFER	N&M Transfer Co., Inc..
LOGYSTO	Logysto.
INDIA_POST_INT	India Post International.
AMAZON_FBA_SWISHIP_IN	Swiship IN.
SRT_TRANSPORT	SRT Transport.
BOMI	Bomi Group.
DELIVERR_SFTP	Deliverr.
HSDEXPRESS	HSDEXPRESS.
SIMPLETIRE_WEBHOOK	SimpleTire.
HUNTER_EXPRESS_SFTP	Hunter Express.
UPS_API	UPS.
WOOYOUNG_LOGISTICS_SFTP	WOO YOUNG LOGISTICS CO.,LTD..
PHSE_API	PHSE.
WISH_EMAIL_PUSH	Wish.
NORTHLINE	Northline.
MEDAFRICA	Med Africa Logistics.
DPD_AT_SFTP	DPD Austria.
ANTERAJA	Anteraja.
DHL_GLOBAL_FORWARDING_API	DHL Global Forwarding API.
LBCEXPRESS_API	LBC EXPRESS INC..
SIMSGLOBAL	Sims Global.
CDLDELIVERS	CDL Last Mile.
TYP	TYP.
TESTING_COURIER_WEBHOOK	Testing Courier.
PANDAGO_API	Pandago.
ROYAL_MAIL_FTP	Royal Mail.
THUNDEREXPRESS	Thunder Express Australia.
SECRETLAB_WEBHOOK	Secretlab.
SETEL	Setel Express.
JD_WORLDWIDE	JD Worldwide.
DPD_RU_API	DPD Russia.
ARGENTS_WEBHOOK	Argents Express Group.
POSTONE	Post ONE.
TUSKLOGISTICS	Tusk Logistics.
RHENUS_UK_API	Rhenus Logistics UK.
TAQBIN_SG_API	Yamato Singapore.
INNTRALOG_SFTP	Inntralog GmbH.
DAYROSS	Day & Ross.
CORREOSEXPRESS_API	Correos Express (API).
INTERNATIONAL_SEUR_API	International Seur API.
YODEL_API	Yodel API.
HEROEXPRESS	Hero Express.
DHL_SUPPLYCHAIN_IN	DHL supply chain India.
URGENT_CARGUS	Urgent Cargus.
FRONTDOORCORP	FRONTdoor Collective.
JTEXPRESS_PH	J&T Express Philippines.
PARCELSTARS_WEBHOOK	Parcelstars.
DPD_SK_SFTP	DPD Slovakia.
MOVIANTO	Movianto.
OZEPARTS_SHIPPING	Ozeparts Shipping.
KARGOMKOLAY	KargomKolay (CargoMini).
TRUNKRS	Trunkrs.
OMNIRPS_WEBHOOK	Omni Returns.
CHILEXPRESS	Chile Express.
TESTING_COURIER	Testing Courier.
JNE_API	JNE (API).
BJSHOMEDELIVERY_FTP	BJS Distribution, Storage & Couriers - FTP.
DEXPRESS_WEBHOOK	D Express.
USPS_API	USPS API.
TRANSVIRTUAL	TransVirtual.
SOLISTICA_API	solistica.
CHIENVENTURE_WEBHOOK	Chienventure.
DPD_UK_SFTP	DPD UK.
INPOST_UK	InPost.
JAVIT	Javit.
ZTO_DOMESTIC	ZTO Express China.
DHL_GT_API	DHL Global Forwarding Guatemala.
CEVA_TRACKING	CEVA Package.
KOMON_EXPRESS	Komon Express.
EASTWESTCOURIER_FTP	East West Courier Pte Ltd.
DANNIAO	Danniao.
SPECTRAN	Spectran.
DELIVER_IT	Deliver-iT.
RELAISCOLIS	Relais Colis.
GLS_SPAIN_API	GLS Spain.
POSTPLUS	PostPlus.
AIRTERRA	Airterra.
GIO_ECOURIER_API	GIO Express Ecourier.
DPD_CH_SFTP	DPD Switzerland.
FEDEX_API	FedEx®.
INTERSMARTTRANS	INTERSMARTTRANS & SOLUTIONS SL.
HERMES_UK_SFTP	Hermes UK.
EXELOT_FTP	Exelot Ltd..
DHL_PA_API	DHL GLOBAL FORWARDING PANAMÁ.
VIRTRANSPORT_SFTP	Vir Transport.
WORLDNET	Worldnet Logistics.
INSTABOX_WEBHOOK	Instabox.
KNG	Keuhne + Nagel Global.
FLASHEXPRESS_WEBHOOK	Flash Express.
MAGYAR_POSTA_API	Magyar Posta.
WESHIP_API	WeShip.
OHI_WEBHOOK	Ohi.
MUDITA	MUDITA.
BLUEDART_API	Bluedart.
T_CAT_API	T-cat.
ADS	ADS Express.
HERMES_IT	HR Parcel.
FITZMARK_API	FitzMark.
POSTI_API	Posti API.
SMSA_EXPRESS_WEBHOOK	SMSA Express.
TAMERGROUP_WEBHOOK	Tamer Logistics.
LIVRAPIDE	Livrapide.
NIPPON_EXPRESS	Nippon Express.
BETTERTRUCKS	Better Trucks.
FAN	FAN COURIER EXPRESS.
PB_USPSFLATS_FTP	USPS Flats (Pitney Bowes).
PARCELRIGHT	Parcel Right.
ITHINKLOGISTICS	iThink Logistics.
KERRY_EXPRESS_TH_WEBHOOK	Kerry Logistics.
ECOUTIER	eCoutier.
SHOWL	SENHONG INTERNATIONAL LOGISTICS.
BRT_IT_API	BRT Bartolini API.
RIXONHK_API	Rixon Logistics.
DBSCHENKER_API	DB Schenker.
ILYANGLOGIS	Ilyang logistics.
MAIL_BOX_ETC	Mail Boxes Etc..
WESHIP	WeShip.
DHL_GLOBAL_MAIL_API	DHL eCommerce Solutions.
ACTIVOS24_API	Activos24.
ATSHEALTHCARE	ATS Healthcare.
LUWJISTIK	Luwjistik.
GW_WORLD	Gebrüder Weiss.
FAIRSENDEN_API	fairsenden.
SERVIP_WEBHOOK	SerVIP.
SWISHIP	Swiship.
TANET	Transport Ambientales.
HOTSIN_CARGO	SHENZHEN HOTSIN CARGO INT'L FORWARDING CO.,LTD.
DIREX	Direx.
HUANTONG	HuanTong.
IMILE_API	iMile.
AUEXPRESS	Au Express.
NYTLOGISTICS	NYT SUPPLY CHAIN LOGISTICS Co.,LTD.
DSV_REFERENCE	DSV Futurewave.
NOVOFARMA_WEBHOOK	Novofarma.
AITWORLDWIDE_SFTP	AIT.
SHOPOLIVE	Olive.
FNF_ZA	Fast & Furious.
DHL_ECOMMERCE_GC	DHL eCommerce Greater China.
FETCHR	Fetchr.
STARLINKS_API	Starlinks Global.
YYEXPRESS	YYEXPRESS.
SERVIENTREGA	Servientrega.
HANJIN	HanJin.
SPANISH_SEUR_FTP	Spanish Seur.
DX_B2B_CONNUM	DX (B2B).
HELTHJEM_API	Helthjem.
INEXPOST	Inexpost.
A2B_BA	A2B Express Logistics.
RHENUS_GROUP	Rhenus Logistics.
SBERLOGISTICS_RU	Sber Logistics.
MALCA_AMIT	Malca-Amit.
PPL	Professional Parcel Logistics.
OSM_WORLDWIDE_SFTP	OSM Worldwide.
ACILOGISTIX	ACI Logistix.
OPTIMACOURIER	Optima Courier.
NOVA_POSHTA_API	Nova Poshta API.
LOGGI	Loggi.
YIFAN	YiFan Express.
MYDYNALOGIC	My DynaLogic.
MORNINGLOBAL	Morning Global.
CONCISE_API	Concise.
FXTRAN	Falcon Express.
DELIVERYOURPARCEL_ZA	Deliver Your Parcel.
UPARCEL	uParcel.
MOBI_BR	Mobi Logistica.
LOGINEXT_WEBHOOK	T&W Delivery.
EMS	EMS.
SPEEDY	Speedy.
ZOOM_RED	Zoom.
NAVLUNGO	Navlungo.
CASTLEPARCELS	Castle Parcels.
WEEE	Weee.
PACKALY	Packaly.
YUNHUIPOST	Yunhuipost.
YOUPARCEL	YouParcel.
LEMAN	Leman.
MOOVIN	Moovin.
URB_IT	Urb-it.
MULTIENTREGAPANAMA	Multientrega.
JUSDASR	Jusdasr.
DISCOUNTPOST	Discount Post.
RHENUS_UK	Rhenus Logistics UK.
SWISHIP_JP	Swiship JP.
GLS_US	GLS USA.
SMTL	Southwestern Motor Transport. Inc.
EMEGA	Discount Post Emega.
EXPRESSONE_SV	EXPRESSONE Slovenia.
HEPSIJET	hepsiJET.
WELIVERY	Welivery.
BRINGER	Bringer Parcel Services.
EASYROUTES	EasyRoutes.
MRW	MRW.
RPM	RPM.
DPD_PRT	DPD Portugal.
GLS_ROMANIA	GLS Romania.
LMPARCEL	LM Parcel.
GTAGSM	GTA GSM.
DOMINO	DOMINO.
ESHIPPER	eShipper.
TRANSPAK	Transpak Inc..
XINDUS	Xindus.
AOYUE	Aoyue.
EASYPARCEL	Easyparcel.
EXPRESSONE	EXPRESSONE.
SENDEO_KARGO	Sendeo Kargo.
SPEEDAF	Speedaf Express.
ETOWER	eTower.
GCX	GC Express.
NINJAVAN_VN	Ninjavan Vietnam.
ALLEGRO	Allegro.
JUMPPOINT	Jumppoint.
SHIPGLOBAL_US	ShipGlobal.
KINISI	Kinisi Transport Pty Ltd.
OAKH	Oakh Harbour Freight Lines.
AWEST	American West.
BARSAN	Barsan Global Lojistik.
ENERGOLOGISTIC	Energo Logistic.
MADROOEX	Madrooex.
GOBOLT	GoBolt.
SWISS_UNIVERSAL_EXPRESS	Swiss Universal Express.
IORDIRECT	IOR Direct Solutions.
XMSZM	xmszm.
GLS_HUN	GLS Hungary.
SENDY	Sendy Express.
BRAUNSEXPRESS	Brauns Express.
GRANDSLAMEXPRESS	Grand Slam Express.
XGS	XGS.
OTSCHILE	OTS.
PACK_UP	Pack-Up.
PARCELSTARS	Parcelstars.
TEAMEXPRESSLLC	Team Express Service LLC.
ASYADEXPRESS	Asyad Express.
TDN	TDN.
EARLYBIRD	Early Bird.
CACESA	Cacesa.
PARCELJET	Parceljet.
MNG_KARGO	MNG Kargo.
SUPERPACKLINE	Super Pac Line.
SPEEDX	SpeedX.
VESYL	Vesyl.
SKYKING	Sky King.
DIRMENSAJERIA	DIR.
NETLOGIXGROUP	Netlogix.
ZYOU	ZYEX.
JAWAR	Jawar.
AGSYSTEMS	Associate Global Systems.
GPS	GPS.
PTT_KARGO	PTT Kargo.
MAERGO	Maergo.
ARIHANTCOURIER	AICS.
VTFE	VicTas Freight Express.
YUNANT	Yunant.
URBIFY	Urbify .
PACK_MAN	homem-pacote .
LIEFERGRUN	LIEFERGRUN .
OBIBOX	Obibox .
PAIKEDA	Paikeda .
SCOTTY	Scotty .
INTELCOM_CA	Intelcom .
SWE	sueca .
ASÊNDIA	Asendia Global .
DPD_AT	DPD Áustria .
REVEZAMENTO	Revezamento .
ATA	ATA .
SKYEXPRESS_INTERNATIONAL	SkyExpress Internacional .
SURAT_KARGO	Surat Kargo .
SGLINK	SG LINK .
FLEETOPTICSINC	FleetOptics .
COMPRAR ONLINE	loja online .
PORQUINHA	PIGGYSHIP .
LOGOIX	LogoiX .
KOLAY_GELSIN	Geleia Kolay .
ASSOCIATED_COURIERS	Correios Associados .
UPS_CHECKER	verificador-ups .
ENVIO DE VINHO	Envio de vinhos .
ESPECIAL	Spedisci online .
QUATRO PISTAS	Quatro pipas .
ETONAS	Etonas .
FINMILE	Fin Mile .
UNIUNI	Uniuni .
RODONAVES	Rodonaves .
INPOST_IT	Inpost Itália .
TFORCE_FRETE	Tforce Freight .
RICHMOM	Mãe rica .
FRANCO	Corriere Franco .
ECPARCEL	Ecparcel .
FEDEX_CHINA	Fedex China .
GOFO_EXPRESS	Gofo Express .
SHIPBOB	Shipbob .
JERSEYPOST_ATLAS	Grupo Jersey Post .
CORETRAILS	Coretrails .
RHENUS_ITÁLIA	Rhenus Logistics Itália .
JADLOG	Jadlog .
JITSU	Jiu-jitsu .
YANWEN_EXPRESS	Yanwen Express .
DASHLINK	Dashlink .
SEINO_SUPER_EXPRESS	Seino Super Express .
FLOSHIP	Floship .
METROSCG	Cadeia de suprimentos metropolitana .
ENVIAR PACOTE	Sendparcel .
P2P	P2p .
CN_EXPRESS	Cn Express .
CIRROTRACK	Pista Cirro .
LOGÍSTICA TERRESTRE	Logística terrestre .
VEHO	Veho .
MEDLINE	Medline .
VDTRACK	Vdtrack .
SINO_SCM	Sino Scm .
3PE_EXPRESS	3pe Express .
SWIFTX	Swiftx .
SFYDEXPRESS	Sfyd Express .
TOPTRANS	Toptrans .
id_de_captura
obrigatório
string [ 1 .. 50 ] caracteres ^[a-zA-Z0-9]*$
O ID de captura do PayPal.

notificar_pagador
booleano
Padrão: 
falso
Se verdadeiro, o PayPal enviará uma notificação por e-mail ao pagador da transação. O e-mail contém os detalhes de rastreamento fornecidos pela solicitação da API de rastreamento de pedidos. Independentemente de qualquer valor passado para notify_payer, o pagador poderá receber notificações de rastreamento no aplicativo do PayPal, com base nas preferências de notificação do usuário.


Unid
Conjunto de objetos
Uma série de detalhes dos itens na remessa.

Respostas
200Uma resposta bem-sucedida a uma solicitação idempotente retorna o 200 OKcódigo de status HTTP com um corpo de resposta JSON que exibe os detalhes do rastreador.
201Uma resposta bem-sucedida a uma solicitação não idempotente retorna o 201 Createdcódigo de status HTTP com um corpo de resposta JSON que exibe detalhes do rastreador. Se uma resposta duplicada for reenviada, o 200 OKcódigo de status HTTP retornado também será retornado.
400A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.
403A autorização falhou devido a permissões insuficientes.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
500Ocorreu um erro interno no servidor.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 201 - Adicionar rastreamento ao pedido - Informações da transportadora e detalhamento dos itens
Exemplo 1 - 201 - Adicionar rastreamento ao pedido - Informações da transportadora e detalhamento dos itens
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T/track \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{
  "capture_id": "8MC585209K746392H",
  "número_de_rastreamento": "443844607820",
  "transportadora": "FEDEX",
  "notificar_pagador": falso,
  "Unid": [
    {
      "nome": "Camiseta",
      "sku": "sku02",
      "quantidade": "1",
      "upc": {
        "tipo": "UPC-A",
        "código": "upc001"
      },
      "image_url": "https://www.example.com/example.jpg",
      "url": "https://www.example.com/example"
    }
  ]
}' 
Amostras de resposta
200
201
400
403
422

mais 1
mais 1
aplicativo/json

Exemplo 1 - 201 - Adicionar rastreamento ao pedido - Informações da transportadora e detalhamento dos itens
Exemplo 1 - 201 - Adicionar rastreamento ao pedido - Informações da transportadora e detalhamento dos itens
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"status": "COMPLETED",
"purchase_units": [
{
"reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
"items": [
{
"name": "Air Jordan Shoe",
"sku": "sku01",
"quantity": "1"
},
{
"name": "T-Shirt",
"sku": "sku02",
"quantity": "1"
}
],
"shipping": {
"trackers": [
{
"id": "8MC585209K746392H-443844607820",
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "up",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T/trackers/8MC585209K746392H-443844607820",
"rel": "update",
"method": "PATCH"
}
],
"create_time": "2022-08-12T21:20:49Z",
"update_time": "2022-08-12T21:20:49Z"
}
]
},
"payments": {
"captures": [
{
"id": "8MC585209K746392H",
"status": "COMPLETED",
"amount": {
"currency_code": "USD",
"value": "100.00"
},
"seller_protection": {
"status": "NOT_ELIGIBLE"
},
"final_capture": true,
"seller_receivable_breakdown": {
"gross_amount": {
"currency_code": "USD",
"value": "100.00"
},
"paypal_fee": {
"currency_code": "USD",
"value": "3.00"
},
"net_amount": {
"currency_code": "USD",
"value": "97.00"
}
},
"create_time": "2018-04-01T21:20:49Z",
"update_time": "2018-04-01T21:20:49Z",
"links": [
{
"href": "https://api-m.paypal.com/v2/payments/captures/8MC585209K746392H",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v2/payments/captures/8MC585209K746392H/refund",
"rel": "refund",
"method": "POST"
}
]
}
]
}
}
],
"links": [
{
"href": "https://api-m.paypal.com/v2/checkout/orders/5O190127TN364715T",
"rel": "self",
"method": "GET"
}
]
}
Atualizar ou cancelar informações de rastreamento de um pedido
correção
/v2/finalizar compra/pedidos/{id}/rastreadores/{id_do_rastreador}
Experimente
Atualiza ou cancela as informações de rastreamento de um pedido do PayPal, por ID. Atributos ou objetos atualizáveis:


Atributo	Op	Notas
items	substituir	Usar a operação de substituição (replace op) itemssubstituirá o itemsobjeto inteiro pelo valor enviado na solicitação.
notify_payer	substituir, adicionar	
status	substituir	Atualmente, apenas a alteração do status de patch para CANCELADO é suportada.
Segurança
OAuth2
Solicitar
Parâmetros do caminho
eu ia
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O ID do pedido ao qual as informações de rastreamento estão associadas.

tracker_id
obrigatório
string [ 1 .. 36 ] caracteres ^[A-Z0-9]+$
O número de rastreamento do pedido.

Parâmetros do cabeçalho
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Esquema do corpo da requisição: application/json
Array ([ 0 .. 32767 ] itens)
op
obrigatório
corda
A operação.

Valor de enumeração	Descrição
adicionar	Dependendo da referência de localização de destino, executa uma destas funções:
O local de destino é um índice de matriz . Insere um novo valor na matriz no índice especificado.
O local de destino é um parâmetro de objeto que ainda não existe . Adiciona um novo parâmetro ao objeto.
O local de destino é um parâmetro de objeto que existe . Substitui o valor desse parâmetro.
O valueparâmetro define o valor a ser adicionado. Para obter mais informações, consulte 4.1. adicionar .
remover	Remove o valor no local de destino. Para que a operação seja bem-sucedida, o local de destino deve existir. Para obter mais informações, consulte 4.2. remover .
substituir	Substitui o valor no local de destino por um novo valor. O objeto de operação deve conter um valueparâmetro que define o valor de substituição. Para que a operação seja bem-sucedida, o local de destino deve existir. Para obter mais informações, consulte 4.3. substituir .
mover	Remove o valor em um local especificado e o adiciona ao local de destino. O objeto de operação deve conter um fromparâmetro, que é uma string contendo um ponteiro JSON que referencia o local no documento de destino de onde o valor será movido. Para que a operação seja bem-sucedida, o fromlocal deve existir. Para obter mais informações, consulte 4.4. mover .
cópia	Copia o valor de um local especificado para o local de destino. O objeto de operação deve conter um fromparâmetro, que é uma string contendo um ponteiro JSON que referencia o local no documento de destino do qual o valor será copiado. Para que a operação seja bem-sucedida, o fromlocal deve existir. Para obter mais informações, consulte 4.5. copy .
teste	Testa se um valor no local de destino é igual a um valor especificado. O objeto de operação deve conter um valueparâmetro que define o valor a ser comparado com o valor do local de destino. Para que a operação seja bem-sucedida, o valor no local de destino deve ser igual ao valuevalor especificado. Por exemplo, o parâmetro equalindica que o valor no local de destino e o valor valueespecificado são do mesmo tipo JSON. O tipo de dados do valor determina como a igualdade é definida.
Tipo	Considera-se que ambos os valores são iguais se ambos os valores
cordas	Contêm o mesmo número de caracteres Unicode e seus pontos de código são iguais byte a byte.
números	São numericamente iguais.
matrizes	Contêm o mesmo número de valores, e cada valor é igual ao valor na posição correspondente na outra matriz, utilizando estas regras específicas do tipo.
objetos	Contêm o mesmo número de parâmetros, e cada parâmetro é igual a um parâmetro no outro objeto, comparando suas chaves (como strings) e seus valores (usando estas regras específicas do tipo).
literais ( false, true, e null)	São iguais. A comparação é lógica. Por exemplo, espaços em branco entre os valores dos parâmetros de um array não são relevantes. Da mesma forma, a ordem de serialização dos parâmetros do objeto também não é relevante.
Para obter mais informações, consulte 4.6. teste .
caminho
corda
O ponteiro JSON para a localização do documento de destino onde a operação será concluída.

valor
qualquer
O valor a ser aplicado. As operações remove`__init__` copy, `__replace__` e `__replace__` movenão exigem um valor. Como o JSON Patch permite qualquer tipo para `__init__` value, a typepropriedade não é especificada.

de
corda
O ponteiro JSON para o local do documento de destino a partir do qual o valor será movido. Necessário para a moveoperação.

Respostas
204Uma solicitação bem-sucedida retorna o 204 No Contentcódigo de status HTTP com um objeto vazio no corpo da resposta JSON.
400A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.
403A autorização falhou devido a permissões insuficientes.
404O recurso especificado não existe.
422A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.
500Ocorreu um erro interno no servidor.
Solicitar amostras
cURL

Exemplo 1 - 204 - Informações de rastreamento de patches - Substituir o status do rastreador.
Exemplo 1 - 204 - Informações de rastreamento de patches - Substituir o status do rastreador.
Cópia
curl  -v  -X PATCH https://api-m.sandbox.paypal.com/v2/checkout/orders/5O190127TN364715T/trackers/8MC585209K746392H-443844607820 \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer 6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '[
  {
    "op": "substituir",
    "caminho": "/status",
    "valor": "CANCELADO"
  }
]' 
Amostras de resposta
204
400
403
404
422

mais 1
mais 1
aplicativo/json

Exemplo 1 - 204 - Informações de rastreamento de patches - Substituir o status do rastreador.
Exemplo 1 - 204 - Informações de rastreamento de patches - Substituir o status do rastreador.
Cópia
{ }
Receba informações atualizadas sobre seu pedido por meio de um URL de retorno de chamada.
publicar
/v2/finalizar compra/pedidos/callback de atualização de pedidos
Experimente
A documentação para este 'endpoint' é diferente dos outros endpoints em Pedidos v2. Para este endpoint, os papéis de cliente e servidor são invertidos. O cliente que envia a solicitação é o PayPal, e o servidor que envia a resposta é o comerciante. Na solicitação, o PayPal enviará o endereço de entrega oculto do comprador e a opção de envio selecionada para a URL de retorno de chamada definida na solicitação de criação do pedido. A resposta do comerciante atualizará o recurso Pedidos.

Solicitar
Esquema do corpo da requisição:
aplicativo/json
aplicativo/json

unidades_de_compra
obrigatório
Array de objetos = 1 item
Um conjunto de unidades de compra. Atualmente, apenas uma unidade de compra é suportada. Cada unidade de compra estabelece um contrato entre o pagador e o beneficiário. Cada unidade de compra representa um pedido, total ou parcial, que o pagador pretende adquirir do beneficiário.


Endereço para envio
objeto
Endereço de entrega ocultado para ser usado nas opções de envio e no cálculo de impostos.


opção_de_envio
objeto
Opção de envio selecionada pelo comprador.

Respostas
200A ligação de retorno para o comerciante foi bem-sucedida.
Solicitar amostras
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 200 - Solicitação de retorno de chamada para código de desconto - Adicionando um código de desconto - Sem endereço
Exemplo 1 - 200 - Solicitação de retorno de chamada para código de desconto - Adicionando um código de desconto - Sem endereço
Cópia
curl  -v  -X POST https://api-m.sandbox.paypal.com/v2/checkout/orders/order-update-callback \ 
-H  'Content-Type: application/json'  \ 
-H  'Content-Language: en_US'  \ 
-d  '{
  "id": "5O190127TN364715T",
  "descontos": [
    {
      "operação": "ADICIONAR",
      "código": "LIQUIDAÇÃO_DE_VERÃO"
    }
  ],
  "purchase_units": [
    {
      "reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
      "quantia": {
        "currency_code": "USD",
        "valor": "100,00"
        "discriminação": {
          "total_do_item": {
            "currency_code": "USD",
            "Valor": "90,00"
          },
          "tax_total": {
            "currency_code": "USD",
            "Valor": "10,00"
          },
          "envio": {
            "currency_code": "USD",
            "valor": "0,00"
          }
        }
      },
      "Unid": [
        {
          "nome": "E-book",
          "descrição": "E-book digital",
          "sku": "sku01",
          "unidade_quantidade": {
            "currency_code": "USD",
            "Valor": "90,00"
          },
          "imposto": {
            "currency_code": "USD",
            "Valor": "10,00"
          },
          "quantidade": "1",
          "categoria": "PRODUTOS_DIGITAIS"
        }
      ]
    }
  ]
}' 
Amostras de resposta
200
aplicativo/json

Exemplo 1 - 200 - Solicitação de retorno de chamada para código de desconto - Adicionando um código de desconto - Sem endereço
Exemplo 1 - 200 - Solicitação de retorno de chamada para código de desconto - Adicionando um código de desconto - Sem endereço
CópiaExpandir tudoRecolher tudo
{
"id": "5O190127TN364715T",
"purchase_units": [
{
"reference_id": "d9f80740-38f0-11e8-b467-0ed5f89f718b",
"amount": {
"currency_code": "USD",
"value": "90.00",
"breakdown": {
"item_total": {
"currency_code": "USD",
"value": "90.00"
},
"tax_total": {
"currency_code": "USD",
"value": "10.00"
},
"shipping": {
"currency_code": "USD",
"value": "0.00"
},
"discount": {
"currency_code": "USD",
"value": "10.00",
"breakdown": [
{
"value": "10.00",
"currency_code": "USD",
"discount_code": "SUMMER_SALE",
"description": "$10 off for summer sale"
}
]
}
}
},
"items": [
{
"name": "E-book",
"description": "Digital E-book",
"sku": "sku01",
"unit_amount": {
"currency_code": "USD",
"value": "90.00"
},
"tax": {
"currency_code": "USD",
"value": "10.00"
},
"quantity": "1",
"category": "DIGITAL_GOODS"
}
]
}
]
}
Definições
id_da_conta
O ID do pagador do PayPal é uma versão mascarada do número da conta do PayPal destinada ao uso com terceiros. O número da conta é criptografado de forma reversível e uma variante proprietária do Base32 é usada para codificar o resultado.

string ( account_id ) = 13 caracteres ^[2-9A-HJ-NP-Z]{13}$ < ppaas_payer_id_v3 >
O ID do pagador do PayPal é uma versão mascarada do número da conta do PayPal destinada ao uso com terceiros. O número da conta é criptografado de forma reversível e uma variante proprietária do Base32 é usada para codificar o resultado.

Cópia
"stringstrings"
carimbos_de_data_da_atividade
Os registros de data e hora que são comuns às transações de pagamento autorizado, pagamento capturado e reembolso.

tempo_de_criação
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que a transação ocorreu, no formato de data e hora da Internet .

hora_de_atualização
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora da última atualização da transação, no formato de data e hora da Internet .

Cópia
{
"create_time": "string",
"update_time": "string"
}
cliente_altpay
Informações para clientes sobre Métodos de Pagamento Alternativos (APM).

id_do_cliente_do_comerciante
string [ 1 .. 64 ] caracteres ^[0-9a-zA-Z-_.^*$@#]+$
Comerciantes e parceiros podem já ter um banco de dados onde as informações de seus clientes são armazenadas. Use merchant_customer_id para obter o ID do cliente do comerciante.

Cópia
{
"merchant_customer_id": "string"
}
detalhamento_do_valor
A discriminação do valor. A discriminação fornece detalhes como o valor total dos itens, o valor total dos impostos, o frete, o manuseio, o seguro e os descontos, se houver.


total_do_item
objeto
O subtotal de todos os itens. Obrigatório se o pedido incluir itens purchase_units[].items[].unit_amount. Deve ser igual à soma de (items[].unit_amount * items[].quantity)todos os itens. item_total.valueNão pode ser um número negativo.


envio
objeto
A taxa de envio para todos os itens dentro de um determinado local purchase_unitnão shipping.valuepode ser um número negativo.


manuseio
objeto
A taxa de manuseio para todos os itens dentro de um determinado intervalo purchase_unitnão handling.valuepode ser um número negativo.


total_de_impostos
objeto
O imposto total para todos os itens. Obrigatório se o pedido incluir [itens adicionais] purchase_units.items.tax. Deve ser igual à soma de [itens adicionais] (items[].tax * items[].quantity)para todos os itens. tax_total.valueNão pode ser um número negativo.


seguro
objeto
A taxa de seguro para todos os itens dentro de um determinado local purchase_unitnão insurance.valuepode ser um número negativo.


desconto no frete
objeto
O desconto no frete para todos os itens dentro de um determinado período purchase_unitnão shipping_discount.valuepode ser um número negativo.


desconto
objeto
O desconto para todos os itens dentro de um determinado intervalo purchase_unitnão discount.valuepode ser um número negativo.

CópiaExpandir tudoRecolher tudo
{
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
valor_com_detalhamento
O valor total do pedido, com um detalhamento opcional que fornece informações como o valor total dos itens, o valor total dos impostos, o frete, o manuseio, o seguro e os descontos, se houver.
Se você especificar amount.breakdown, o valor será igual item_totala mais tax_totalmais shippingmais handlingmais insurancemenos shipping_discountmenos o desconto.
O valor deve ser um número positivo. Para obter uma lista das moedas suportadas e a precisão decimal, consulte os Códigos de Moeda das APIs REST do PayPal .

código_da_moeda
obrigatório
string = 3 caracteres( código_da_moeda )
O código de moeda ISO-4217 de três caracteres que identifica a moeda.

valor
obrigatório
string com menos de 32 caracteres ^((-?[0-9]+)|(-?([0-9]+)?[.][0-9]+))$
O valor, que pode ser:

Números inteiros para moedas desse tipo JPYnormalmente não são fracionários.
Uma fração decimal para moedas TNDdesse tipo é subdividida em milésimos.
Para saber o número necessário de casas decimais para um código de moeda, consulte Códigos de Moeda .

discriminação
objeto
A discriminação do valor. A discriminação fornece detalhes como o valor total dos itens, o valor total dos impostos, o frete, o manuseio, o seguro e os descontos, se houver.

CópiaExpandir tudoRecolher tudo
{
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
}
Método de autenticação APM
Enumeração dos métodos de autenticação para aprovação de pagamentos.

string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$( Método de autenticação APM )
Padrão: 
"REDIRECIONAR"
Enumeração dos métodos de autenticação para aprovação de pagamentos.

Valor de enumeração	Descrição
REDIRECIONAR	Fluxo de redirecionamento baseado em navegador, onde o pagador é direcionado para uma página específica.
VERIFICAÇÃO DE CÓDIGO	O código é apresentado ao pagador como método de autenticação.
NOTIFICAÇÃO_DO_APLICATIVO	O pagador recebe uma notificação push em seu dispositivo móvel, solicitando que ele autentique o pagamento em seu aplicativo bancário ou de pagamento.
Cópia
"REDIRECT"
Preferências de troca de aplicativos no aplicativo nativo
O comerciante configurou as preferências do aplicativo nativo do comprador para alternar para o aplicativo PayPal para consumidores.

os_type
string [ 1 .. 7 ] caracteres ^[A-Z_]+$
Tipo de sistema operacional do dispositivo que o comprador está utilizando.

Valor de enumeração	Descrição
ANDROID	Sistema operacional Android do Google.
iOS	O sistema operacional da Apple é normalmente encontrado em dispositivos móveis da Apple.
OUTRO	Qualquer outro tipo de sistema operacional.
versão_do_sistema_operacional
string [ 1 .. 64 ] caracteres ^.*$
Versão do sistema operacional do dispositivo que o comprador está utilizando.

Cópia
{
"os_type": "ANDROID",
"os_version": "string"
}
apple_pay
Informações necessárias para pagar com Apple Pay.

eu ia
string [ 1 .. 250 ] caracteres ^.*$
Identificador de transação do Apple Pay: este será o identificador exclusivo desta transação, fornecido pela Apple. O padrão é definido por terceiros e é compatível com Unicode.

token
string [ 1 .. 10000 ] caracteres ^.*$
Token Apple Pay criptografado, contendo informações do cartão. Este token seria codificado em base64. O padrão é definido por terceiros e é compatível com Unicode.


credencial armazenada
objeto
Fornece detalhes adicionais para processar um pagamento usando uma credencial cardque foi armazenada ou que se pretende armazenar (também conhecida como credencial armazenada ou cartão em arquivo).
Compatibilidade de parâmetros:

payment_type=ONE_TIMEé compatível apenas com payment_initiator=CUSTOMER.
usage=FIRSTé compatível apenas com payment_initiator=CUSTOMER.
previous_transaction_referenceou previous_network_transaction_referenceé compatível apenas com payment_initiator=MERCHANT.
Apenas um dos parâmetros - previous_transaction_referencee previous_network_transaction_reference- pode estar presente na solicitação.
nome
string [ 3 .. 300 ] caracteres
Nome na carteira.

endereço de email
string [ 3 .. 254 ] caracteres ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...( endereço de email ) Mostrar padrão
O endereço de e-mail do titular da conta associada a este método de pagamento.


número_de_telefone
objeto
O número de telefone, em seu formato canônico internacional de numeração E.164 . Suporta apenas a national_numberpropriedade.


cartão
objeto
Informações do cartão de pagamento.


atributos
objeto
Atributos adicionais associados ao Apple Pay.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"token": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
}
atributos_do_apple_pay
Atributos adicionais associados ao Apple Pay.


cliente
objeto
Este objeto representa o cliente de um comerciante, permitindo-lhe armazenar detalhes de contato e rastrear todos os pagamentos associados ao mesmo cliente.


cofre
objeto
Especificação básica de armazenamento em cofre. O objeto pode ser estendido para casos de uso específicos em cada `payment_source` que suporte armazenamento em cofre.

CópiaExpandir tudoRecolher tudo
{
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
}
apple_pay_attributes_response
Atributos adicionais associados ao uso do Apple Pay.


cofre
objeto
Detalhes sobre uma forma de pagamento salva.

CópiaExpandir tudoRecolher tudo
{
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
resposta_do_cartão_pay_apple
O cartão utilizado para efetuar o pagamento foi proveniente da Carteira Apple Pay.

nome
string [ 2 .. 300 ] caracteres
O nome do titular do cartão, tal como aparece no cartão.

últimos_dígitos
corda
Os últimos dígitos do cartão de pagamento.

redes_disponíveis
Array de strings [ 1 .. 256 ] itens
Conjunto de marcas ou redes associadas ao cartão.

Valor do Enum de Itens	Descrição
VISA	Cartão Visa.
MASTERCARD	Cartão Mastercard.
DESCOBRIR	Cartão Discover.
AMEX	Cartão American Express.
SOLO	Cartão de débito Solo.
JCB	Cartão do Japan Credit Bureau.
ESTRELA	Cartão Estrela Militar.
DELTA	Cartão da Delta Airlines.
TROCAR	Trocar de cartão de crédito.
MAESTRO	Cartão de crédito Maestro.
CB_NACIONAL	Cartão de crédito Carte Bancaire (CB).
CONFIGURAÇÃO	Cartão de crédito Configoga.
CONFIDIS	Cartão de crédito Confidis.
ELÉTRON	Cartão de crédito Visa Electron.
CETELEM	Cartão de crédito Cetelem.
CHINA_UNIÃO_PAGAMENTO	Cartão de crédito China Union Pay.
RESTAURANTES	A rede de serviços bancários e de pagamento da Diners Club International pertence à Discover Financial Services (DFS), uma das marcas mais reconhecidas no setor de serviços financeiros dos EUA.
ELO	A rede brasileira de pagamentos com cartão Elo.
HIPER	A rede de pagamentos eletrônicos Hiper-Ingenico.
HIPERCARD	O Hipercard é a rede de pagamentos brasileira amplamente aceita no mercado varejista.
Rúpia	A rede de pagamentos RuPay.
GE	A rede de pagamentos com cartão 3Point da GE Credit Union.
SINCRONIA	A rede de pagamentos Synchrony Financial (SYF).
EFTPOS	A rede de pagamento com cartão de débito EFTPOS (Transferência Eletrônica de Fundos no Ponto de Venda).
CARTÃO_BANCÁRIO	A rede de pagamento Carte Bancaire.
ACESSO ÀS ESTRELAS	A rede de pagamentos Star Access.
PULSO	A rede de pagamentos Pulse.
NYCE	A rede de pagamentos NYCE.
ACELERAÇÃO	A rede de pagamentos Accel.
DESCONHECIDO	Rede de pagamento desconhecida.

da solicitação
objeto
Representação dos dados do cartão conforme recebidos na solicitação.


credencial armazenada
objeto
Fornece detalhes adicionais para processar um pagamento usando uma credencial cardque foi armazenada ou que se pretende armazenar (também conhecida como credencial armazenada ou cartão em arquivo).
Compatibilidade de parâmetros:

payment_type=ONE_TIMEé compatível apenas com payment_initiator=CUSTOMER.
usage=FIRSTé compatível apenas com payment_initiator=CUSTOMER.
previous_transaction_referenceou previous_network_transaction_referenceé compatível apenas com payment_initiator=MERCHANT.
Apenas um dos parâmetros - previous_transaction_referencee previous_network_transaction_reference- pode estar presente na solicitação.
marca
string [ 1 .. 255 ] caracteres ^[A-Z_]+$
A marca ou a rede do cartão. Normalmente usada na resposta.

Valor de enumeração	Descrição
VISA	Cartão Visa.
MASTERCARD	Cartão Mastercard.
DESCOBRIR	Cartão Discover.
AMEX	Cartão American Express.
SOLO	Cartão de débito Solo.
JCB	Cartão do Japan Credit Bureau.
ESTRELA	Cartão Estrela Militar.
DELTA	Cartão da Delta Airlines.
TROCAR	Trocar de cartão de crédito.
MAESTRO	Cartão de crédito Maestro.
CB_NACIONAL	Cartão de crédito Carte Bancaire (CB).
CONFIGURAÇÃO	Cartão de crédito Configoga.
CONFIDIS	Cartão de crédito Confidis.
ELÉTRON	Cartão de crédito Visa Electron.
CETELEM	Cartão de crédito Cetelem.
CHINA_UNIÃO_PAGAMENTO	Cartão de crédito China Union Pay.
RESTAURANTES	A rede de serviços bancários e de pagamento da Diners Club International pertence à Discover Financial Services (DFS), uma das marcas mais reconhecidas no setor de serviços financeiros dos EUA.
ELO	A rede brasileira de pagamentos com cartão Elo.
HIPER	A rede de pagamentos eletrônicos Hiper-Ingenico.
HIPERCARD	O Hipercard é a rede de pagamentos brasileira amplamente aceita no mercado varejista.
Rúpia	A rede de pagamentos RuPay.
GE	A rede de pagamentos com cartão 3Point da GE Credit Union.
SINCRONIA	A rede de pagamentos Synchrony Financial (SYF).
EFTPOS	A rede de pagamento com cartão de débito EFTPOS (Transferência Eletrônica de Fundos no Ponto de Venda).
CARTÃO_BANCÁRIO	A rede de pagamento Carte Bancaire.
ACESSO ÀS ESTRELAS	A rede de pagamentos Star Access.
PULSO	A rede de pagamentos Pulse.
NYCE	A rede de pagamentos NYCE.
ACELERAÇÃO	A rede de pagamentos Accel.
DESCONHECIDO	Rede de pagamento desconhecida.
tipo
string [ 1 .. 255 ] caracteres ^[A-Z_]+$
O tipo de cartão de pagamento.

Valor de enumeração	Descrição
CRÉDITO	Um cartão de crédito.
DÉBITO	Um cartão de débito.
PRÉ-PAGO	Um cartão pré-pago.
LOJA	Um cartão da loja.
DESCONHECIDO	Não foi possível determinar o tipo de cartão.

resultado_da_autenticação
objeto
Resultados de autenticação, como o 3D Secure.


atributos
objeto
Atributos adicionais associados ao uso deste cartão.

termo
string = 7 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])$
O ano e mês de validade do cartão, em formato de data da Internet .


detalhes_do_bin
objeto
Detalhes do Número de Identificação Bancária (BIN) utilizados para efetuar um pagamento.


Endereço de Cobrança
objeto
Endereço postal internacional portátil. Mapeia para AddressValidationMetadata e controles de formulário de preenchimento automático HTML 5.1 : o atributo autocomplete .

código_do_país
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código ISO 3166-1 de dois caracteres que identifica o país ou região.

Nota: O código de país para a Grã-Bretanha é 0gg GBe não UKo utilizado nos domínios de nível superior desse país. Utilize o C2código de país para a China em todo o mundo para o método de preços comparáveis ​​não controlados (CUP), cartões bancários e transações internacionais.
CópiaExpandir tudoRecolher tudo
{
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
}
dados_de_token_descriptografados_do_apple_pay
Informações sobre os dados de pagamento obtidos pela descriptografia do token do Apple Pay.

ID do fabricante do dispositivo
string [ 1 .. 2000 ] caracteres ^.*$
Identificador do fabricante do dispositivo Apple Pay codificado em hexadecimal. O padrão é definido por terceiros e é compatível com Unicode.

tipo_de_dados_de_pagamento
string [ 1 .. 16 ] caracteres ^[0-9A-Z_]+$
Indica o tipo de dados de pagamento transmitidos; para pagamentos fora da China, os dados são criptografados com 3DSECURE e, para a China, com EMV.

Valor de enumeração	Descrição
3DSECURE	O cartão foi autenticado usando o esquema de autenticação 3D Secure (3DS). Ao usar esse valor, certifique-se de preencher o criptograma e o indicador ECI como parte dos dados de pagamento.
EMV	O cartão foi autenticado usando o método EMV, que é aplicável na China. Ao usar esse valor, certifique-se de incluir os dados EMV e o PIN como parte dos dados de pagamento.

valor_da_transação
objeto
O valor da transação referente ao pagamento que o pagador aprovou na plataforma da Apple.


cartão_tokenizado
obrigatório
objeto
O Apple Pay utilizou um cartão de crédito tokenizado para efetuar o pagamento.


dados_de_pagamento
objeto
Objeto de dados de pagamento do Apple Pay que contém o criptograma, o indicador ECI e outros dados.

CópiaExpandir tudoRecolher tudo
{
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
}
apple_pay_experience_context
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.

URL de retorno
obrigatório
corda
O URL para onde o cliente é redirecionado após aprovar o pagamento.

url_de_cancelamento
obrigatório
corda
O URL para onde o cliente é redirecionado após o cancelamento do pagamento.

Cópia
{
"return_url": "string",
"cancel_url": "string"
}
dados de pagamento apple_pay
Informações sobre os dados de pagamento descriptografados do Apple Pay para o token, como criptograma e indicador ECI.

criptograma
string [ 1 .. 2000 ] caracteres ^.*$
Criptograma de pagamento online, conforme definido pelo 3D Secure. O padrão é definido por uma entidade externa e é compatível com Unicode.

indicador eci
string [ 1 .. 256 ] caracteres ^.*$
Indicador ECI, conforme definido pela 3-Secure. O padrão é definido por uma entidade externa e é compatível com Unicode.

dados_emv
string [ 1 .. 2000 ] caracteres ^.*$
Estrutura de pagamento EMV codificada do Apple Pay usada para pagamentos na China. O padrão é definido por uma entidade externa e é compatível com Unicode.

alfinete
string [ 1 .. 2000 ] caracteres ^.*$
PIN do Apple Pay criptografado com chave bancária. O padrão é definido por terceiros e é compatível com Unicode.

Cópia
{
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
solicitação de pagamento da Apple
Informações necessárias para pagar com Apple Pay.

eu ia
string [ 1 .. 250 ] caracteres ^.*$
Identificador de transação do Apple Pay: este será o identificador exclusivo desta transação, fornecido pela Apple. O padrão é definido por terceiros e é compatível com Unicode.


credencial armazenada
objeto
Fornece detalhes adicionais para processar um pagamento usando uma credencial cardque foi armazenada ou que se pretende armazenar (também conhecida como credencial armazenada ou cartão em arquivo).
Compatibilidade de parâmetros:

payment_type=ONE_TIMEé compatível apenas com payment_initiator=CUSTOMER.
usage=FIRSTé compatível apenas com payment_initiator=CUSTOMER.
previous_transaction_referenceou previous_network_transaction_referenceé compatível apenas com payment_initiator=MERCHANT.
Apenas um dos parâmetros - previous_transaction_referencee previous_network_transaction_reference- pode estar presente na solicitação.

atributos
objeto
Atributos adicionais associados ao Apple Pay.

nome
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada ao Apple Pay.

endereço de email
string [ 3 .. 254 ] caracteres ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...( endereço de email ) Mostrar padrão
O endereço de e-mail do titular da conta associada ao Apple Pay.


número_de_telefone
objeto
O número de telefone, em seu formato canônico internacional de numeração E.164 . Suporta apenas a national_numberpropriedade.


token_descriptografado
objeto
Detalhes da carga útil descriptografada do token do Apple Pay.

id_do_cofre
string [ 1 .. 255 ] caracteres ^[0-9a-zA-Z_-]+$
O ID gerado pelo PayPal para a forma de pagamento salva do Apple Pay. Esse ID deve ser armazenado no servidor do comerciante para que a forma de pagamento salva possa ser usada em transações futuras.


contexto_de_experiência
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
}
contexto_de_aplicação
Personaliza a experiência do pagador durante o processo de aprovação do pagamento com o PayPal.

Observação: Parceiros e marketplaces podem configurar brand_nameopções shipping_preferencedurante a configuração da conta do parceiro, que substituem os valores da solicitação.
nome_da_marca
string [ 1 .. 127 ] caracteres
OBSOLETO. O rótulo que substitui o nome da empresa na conta do PayPal no site do PayPal. Os campos em `<nome_da_empresa>` application_contextagora estão disponíveis no experience_contextobjeto `<nome_da_empresa> payment_source` que os suporta (por exemplo, `<nome_da_empresa> payment_source.paypal.experience_context.brand_name`). Especifique este campo no experience_contextobjeto `<nome_da_empresa>` em vez do application_contextobjeto `<nome_da_empresa>`.

página_de_destino
string [ 1 .. 13 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"SEM PREFERÊNCIA"
OBSOLETO. OBSOLETO. O tipo de página de destino a ser exibida no site do PayPal para a finalização da compra do cliente. Os campos em `landingpage` application_contextagora estão disponíveis no experience_contextobjeto `canvas` payment_sourceque os suporta (por exemplo, `canvas` payment_source.paypal.experience_context.landing_page). Especifique este campo no experience_contextobjeto `canvas` em vez do application_contextobjeto `canvas`.

Valor de enumeração	Descrição
CONECTE-SE	Quando o cliente clica em "Finalizar compra com PayPal" , ele é redirecionado para uma página onde deve fazer login em sua conta PayPal e aprovar o pagamento.
COBRANÇA	Quando o cliente clica em "Finalizar compra com PayPal" , ele é redirecionado para uma página onde deve inserir os dados do cartão de crédito ou débito e outras informações de faturamento necessárias para concluir a compra.
SEM PREFERÊNCIA	Quando o cliente clica em "Finalizar compra com PayPal" , ele é redirecionado para uma página onde deve fazer login no PayPal e aprovar o pagamento, ou para uma página onde deve inserir os dados do cartão de crédito ou débito e outras informações de faturamento necessárias para concluir a compra, dependendo de sua interação anterior com o PayPal.
preferência de envio
string [ 1 .. 20 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"OBTER_DO_ARQUIVO"
OBSOLETO. OBSOLETO. Preferência de envio:

Exibe o endereço de entrega para o cliente.
Permite ao cliente escolher um endereço no site do PayPal.
Impede que o cliente altere o endereço durante o processo de aprovação do pagamento.
Os campos em `<field>` application_contextagora estão disponíveis no experience_contextobjeto sob o `<field> payment_source` que os suporta (ex.: `<field> payment_source.paypal.experience_context.shipping_preference`). Por favor, especifique este campo no experience_contextobjeto em vez do application_contextobjeto `<field>`.
Valor de enumeração	Descrição
OBTER_DO_ARQUIVO	Utilize o endereço de entrega fornecido pelo cliente no site do PayPal.
SEM ENVIO	Oculte o endereço de entrega no site do PayPal. Recomendado para produtos digitais.
DEFINIR_ENDEREÇO_FORNECIDO	Utilize o endereço fornecido pelo comerciante. O cliente não pode alterar esse endereço no site do PayPal.
ação_do_usuário
string [ 1 .. 8 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"CONTINUAR"
OBSOLETO. Configura um fluxo de finalização de compra " Continuar" ou "Pagar agora" . Os campos em `checkout` application_contextagora estão disponíveis no experience_contextobjeto `checkout` sob o payment_source`checkout` que os suporta (por exemplo, `checkout` payment_source.paypal.experience_context.user_action). Especifique este campo no experience_contextobjeto `checkout` em vez do application_contextobjeto `checkout`.

Valor de enumeração	Descrição
CONTINUAR	Após redirecionar o cliente para a página de pagamento do PayPal, um botão "Continuar" será exibido. Use esta opção quando o valor final não for conhecido no momento do início do processo de finalização da compra e você quiser redirecionar o cliente para a página do comerciante sem processar o pagamento.
PAGAR AGORA	Após redirecionar o cliente para a página de pagamento do PayPal, um botão "Pagar agora" será exibido. Utilize esta opção quando o valor final for conhecido no momento do início do processo de finalização da compra e você desejar processar o pagamento imediatamente após o cliente clicar em " Pagar agora" .
URL de retorno
corda
OBSOLETO. A URL para onde o cliente é redirecionado após aprovar o pagamento. Os campos em `$_URL` application_contextagora estão disponíveis no experience_contextobjeto `$_URL` payment_sourceque os suporta (ex.: `$_URL` payment_source.paypal.experience_context.return_url). Especifique este campo no experience_contextobjeto `$_URL` em vez do application_contextobjeto `$_URL`.

url_de_cancelamento
corda
OBSOLETO. A URL para onde o cliente é redirecionado após o cancelamento do pagamento. Os campos em `$_CANCEL_PAY` application_contextagora estão disponíveis no experience_contextobjeto `$_CANCEL_PAY` payment_sourceque os suporta (ex.: `$_CANCEL_PAY` payment_source.paypal.experience_context.cancel_url). Por favor, especifique este campo no experience_contextobjeto `$_CANCEL_PAY` em vez do application_contextobjeto `$_CANCEL_PAY`.

localidade
string [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}|[...( linguagem ) Mostrar padrão
OBSOLETO. O locale formatado em BCP 47 das páginas exibidas na experiência de pagamento do PayPal. O PayPal aceita um código de cinco caracteres. Por exemplo, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, ou zh-TW. Os campos em application_contextagora estão disponíveis no experience_contextobjeto sob o payment_sourceque os suporta (por exemplo, payment_source.paypal.experience_context.locale). Especifique este campo no experience_contextobjeto em vez do application_contextobjeto.


método_de_pagamento
objeto
OBSOLETO. Preferências de pagamento do cliente e do comerciante. Os campos em application_contextagora estão disponíveis no experience_contextobjeto sob o payment_sourceque os suporta (ex.: payment_source.paypal.experience_context.payment_method_selected). Especifique este campo no experience_contextobjeto em vez do application_contextobjeto.


fonte_de_pagamento_armazenada
objeto
OBSOLETO. Fornece detalhes adicionais para processar um pagamento usando uma payment_sourcecredencial que foi armazenada ou que se pretende armazenar (também conhecida como stored_credential ou card-on-file).
Compatibilidade de parâmetros:

payment_type=ONE_TIMEé compatível apenas com payment_initiator=CUSTOMER.
usage=FIRSTé compatível apenas com payment_initiator=CUSTOMER.
previous_transaction_referenceou previous_network_transaction_referenceé compatível apenas com payment_initiator=MERCHANT.
Apenas um dos parâmetros - previous_transaction_referencee previous_network_transaction_reference- pode estar presente na solicitação.
Os campos em `<field>` stored_payment_sourceagora estão disponíveis no stored_credentialobjeto sob o `<field> payment_source` que os suporta (ex.: `<field> payment_source.card.stored_credential.payment_initiator`). Por favor, especifique este campo no payment_sourceobjeto em vez do application_contextobjeto `<field>`.
CópiaExpandir tudoRecolher tudo
{
"brand_name": "string",
"landing_page": "LOGIN",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"return_url": "http://example.com",
"cancel_url": "http://example.com",
"locale": "string",
"payment_method": {
"standard_entry_class_code": "TEL",
"payee_preferred": "UNRESTRICTED"
},
"stored_payment_source": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
}
detalhes_da_garantia
Informações sobre validação de posse do cartão e identificação e verificação (ID&V) do titular do cartão.

conta_verificada
booleano
Padrão: 
falso
Se verdadeiro, indica que a validação de posse do titular do cartão foi realizada na credencial de pagamento devolvida.

titular do cartão autenticado
booleano
Padrão: 
falso
Se verdadeiro, indica que a identificação e verificação (ID&V) foram realizadas nas credenciais de pagamento devolvidas. Se falso, a mesma autenticação baseada em risco pode ser realizada como em transações com cartão. Essa autenticação baseada em risco pode incluir, entre outras medidas, a aplicação do protocolo 3D Secure, se aplicável.

Cópia
{
"account_verified": false,
"card_holder_authenticated": false
}
resposta_de_autenticação
Resultados de autenticação, como o 3D Secure.

transferência de responsabilidade
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
Indicador de transferência de responsabilidade. Resultado da autenticação do emissor.

Valor de enumeração	Descrição
NÃO	A responsabilidade é do comerciante.
POSSÍVEL	A responsabilidade pode ser transferida para a emissora do cartão.
DESCONHECIDO	O sistema de autenticação não está disponível.

três_d_seguro
objeto
Resultados da autenticação 3D Secure.

CópiaExpandir tudoRecolher tudo
{
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
autorização
A transação de pagamento foi autorizada.

status
corda
O status do pagamento autorizado.

Valor de enumeração	Descrição
CRIADO	O pagamento autorizado foi criado. Nenhum pagamento foi capturado para este pagamento autorizado.
CAPTURADO	O pagamento autorizado possui uma ou mais capturas associadas. A soma desses pagamentos capturados é maior que o valor do pagamento autorizado original.
NEGADO	O PayPal não pode autorizar fundos para este pagamento autorizado.
PARCIALMENTE_CAPTURADO	Foi realizado um pagamento capturado referente ao pagamento autorizado, porém em um valor inferior ao valor do pagamento autorizado original.
ANULADO	O pagamento autorizado foi cancelado. Não é possível efetuar mais pagamentos referentes a esta autorização.
PENDENTE	A autorização criada está pendente. Para obter mais informações, consulte status.details.

detalhes_de_status
objeto
Detalhes sobre o status do pedido autorizado pendente.

eu ia
corda
O ID gerado pelo PayPal para o pagamento autorizado.

id_da_fatura
corda
O número da fatura externa fornecido pelo solicitante da API para este pedido. Aparece tanto no histórico de transações do pagador quanto nos e-mails que ele recebe.

id_personalizado
string <= 255 caracteres
O ID externo fornecido pelo usuário da API. Usado para conciliar transações iniciadas pelo usuário da API com transações do PayPal. Aparece em relatórios de transações e liquidações.


links
Conjunto de objetos
Uma série de links relacionados ao HATEOAS .


quantia
objeto
O valor referente a este pagamento autorizado.


referência_de_transação_de_rede
objeto
Valores de referência usados ​​pela rede de cartões para identificar uma transação.


proteção_do_vendedor
objeto
O nível de proteção oferecido, conforme definido pela Proteção ao Vendedor do PayPal para Comerciantes .

tempo_de_expiração
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que o pagamento autorizado expira, no formato de data e hora da Internet .

tempo_de_criação
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que a transação ocorreu, no formato de data e hora da Internet .

hora_de_atualização
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora da última atualização da transação, no formato de data e hora da Internet .

CópiaExpandir tudoRecolher tudo
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string"
}
status_de_autorização
Os campos de status e os detalhes de status de um pagamento autorizado.

status
corda
O status do pagamento autorizado.

Valor de enumeração	Descrição
CRIADO	O pagamento autorizado foi criado. Nenhum pagamento foi capturado para este pagamento autorizado.
CAPTURADO	O pagamento autorizado possui uma ou mais capturas associadas. A soma desses pagamentos capturados é maior que o valor do pagamento autorizado original.
NEGADO	O PayPal não pode autorizar fundos para este pagamento autorizado.
PARCIALMENTE_CAPTURADO	Foi realizado um pagamento capturado referente ao pagamento autorizado, porém em um valor inferior ao valor do pagamento autorizado original.
ANULADO	O pagamento autorizado foi cancelado. Não é possível efetuar mais pagamentos referentes a esta autorização.
PENDENTE	A autorização criada está pendente. Para obter mais informações, consulte status.details.

detalhes_de_status
objeto
Detalhes sobre o status do pedido autorizado pendente.

CópiaExpandir tudoRecolher tudo
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
}
}
detalhes_do_status_da_autorização
Detalhes sobre o status do pagamento autorizado.

razão
string [ 1 .. 64 ] caracteres ^[A-Z_]+$
O motivo pelo qual o status autorizado é PENDING.

Valor de enumeração	Descrição
PENDENTE DE REVISÃO	A autorização está pendente de análise manual.
REJEITADO POR RISCO_FILTROS_DE_FRAUDE	O filtro de risco definido pelo beneficiário falhou para a transação.
Cópia
{
"reason": "PENDING_REVIEW"
}
autorização_com_dados_adicionais
A autorização inclui detalhes adicionais de pagamento, como avaliação de risco e resposta do processador. Esses detalhes são preenchidos apenas para determinados métodos de pagamento.

status
corda
O status do pagamento autorizado.

Valor de enumeração	Descrição
CRIADO	O pagamento autorizado foi criado. Nenhum pagamento foi capturado para este pagamento autorizado.
CAPTURADO	O pagamento autorizado possui uma ou mais capturas associadas. A soma desses pagamentos capturados é maior que o valor do pagamento autorizado original.
NEGADO	O PayPal não pode autorizar fundos para este pagamento autorizado.
PARCIALMENTE_CAPTURADO	Foi realizado um pagamento capturado referente ao pagamento autorizado, porém em um valor inferior ao valor do pagamento autorizado original.
ANULADO	O pagamento autorizado foi cancelado. Não é possível efetuar mais pagamentos referentes a esta autorização.
PENDENTE	A autorização criada está pendente. Para obter mais informações, consulte status.details.

detalhes_de_status
objeto
Detalhes sobre o status do pedido autorizado pendente.

eu ia
corda
O ID gerado pelo PayPal para o pagamento autorizado.

id_da_fatura
corda
O número da fatura externa fornecido pelo solicitante da API para este pedido. Aparece tanto no histórico de transações do pagador quanto nos e-mails que ele recebe.

id_personalizado
string <= 255 caracteres
O ID externo fornecido pelo usuário da API. Usado para conciliar transações iniciadas pelo usuário da API com transações do PayPal. Aparece em relatórios de transações e liquidações.


links
Conjunto de objetos
Uma série de links relacionados ao HATEOAS .


quantia
objeto
O valor referente a este pagamento autorizado.


referência_de_transação_de_rede
objeto
Valores de referência usados ​​pela rede de cartões para identificar uma transação.


proteção_do_vendedor
objeto
O nível de proteção oferecido, conforme definido pela Proteção ao Vendedor do PayPal para Comerciantes .

tempo_de_expiração
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que o pagamento autorizado expira, no formato de data e hora da Internet .

tempo_de_criação
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que a transação ocorreu, no formato de data e hora da Internet .

hora_de_atualização
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora da última atualização da transação, no formato de data e hora da Internet .


resposta_do_processador
objeto
As informações de resposta do processador para solicitações de pagamento, como transações diretas com cartão de crédito.

CópiaExpandir tudoRecolher tudo
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
bancontact
Informações utilizadas para efetuar o pagamento ao Bancontact.

últimos_dígitos_do_cartão
string = 4 caracteres [0-9]{4}
Os últimos dígitos do cartão usado para efetuar o pagamento Bancontact.

nome
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código de país ISO 3166-1 de dois caracteres.

bic
string [8 .. 11] caracteres ^[AZa-z0-9]{4}[AZaz]{2}[AZa-z0-9]{2}([... Mostrar padrão
O código de identificação bancária (BIC).

iban_últimos_caracteres
string [ 4 .. 34 ] caracteres [a-zA-Z0-9]{4}
Os últimos caracteres do IBAN usado para efetuar o pagamento.

Cópia
{
"card_last_digits": "stri",
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
}
solicitação de contato bancário
Informações necessárias para efetuar o pagamento utilizando o Bancontact.

nome
obrigatório
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código de país ISO 3166-1 de dois caracteres.


contexto_de_experiência
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
BIC
O código de identificação comercial (BIC, na sigla em inglês). Em sistemas de pagamento, um BIC é usado para identificar uma empresa específica, geralmente um banco.

string [8 .. 11] caracteres ^[AZa-z0-9]{4}[AZaz]{2}[AZa-z0-9]{2}([...( BIC ) Mostrar padrão
O código de identificação comercial (BIC, na sigla em inglês). Em sistemas de pagamento, um BIC é usado para identificar uma empresa específica, geralmente um banco.

Cópia
"stringst"
Ciclo de faturamento
O ciclo de faturamento fornece detalhes sobre a frequência, o valor, a duração da cobrança e se o ciclo de faturamento é gratuito, com desconto ou regular.

eu ia
string [ 1 .. 128 ] caracteres ^(?:[0-9A-Za-z][0-9A-Za-z_-]*){0,1}[0-9A-Za-z...Mostrar padrão
O identificador único para este ciclo de faturamento ou série recorrente.

período_atual
booleano
Indica se este período, com seus termos de assinatura, está atualmente ativo.

unidade_de_intervalo_de_faturamento
string [ 1 .. 24 ] caracteres ^[A-Z_]+$
A unidade de intervalo de faturamento para o ciclo de faturamento.

Valor de enumeração	Descrição
ANO	O intervalo de faturamento para o ciclo de faturamento é de um ano.
MÊS	O intervalo de faturamento do ciclo de faturamento é de um mês.
SEMESTRE	O intervalo de faturamento do ciclo de faturamento é de meio mês.
SEMANA	O intervalo de faturamento do ciclo de faturamento é de uma semana.
DIA	O intervalo de faturamento do ciclo de faturamento é de um dia.
frequência_de_faturamento
inteiro [ 1 .. 365 ]
Padrão: 
1
A frequência, ou seja, o número de unidades de intervalo de faturamento, na qual o usuário é cobrado. Por exemplo, se o billing_interval_unitvalor for DAY, com um valor billing_frequencyde 2, o usuário será cobrado uma vez a cada dois dias.

ciclos_de_faturamento_totais
inteiro [ 0 .. 999 ]
Padrão: 
1
O número de ciclos de faturamento no período. Isso representa, na prática, quantas vezes o usuário será cobrado durante o período.

ciclo_de_faturamento_atual
inteiro [ 0 .. 999 ]
Indica o índice do ciclo de faturamento atual para o período. Por exemplo, um valor de 3 significa que é o terceiro ciclo de faturamento do período.

período_regular
booleano
Indica se o período é um período regular. Um período regular teria os termos padrão do ciclo de faturamento, em oposição a alguns termos especiais que podem ser aplicáveis ​​a um período de teste ou promocional.

tempo_de_criação
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e hora de criação deste ciclo de faturamento ou série recorrente.

hora_de_atualização
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
Data e hora da última atualização deste ciclo de faturamento ou série recorrente.


valor_de_cobrança
objeto
O valor que será cobrado ao usuário em cada ciclo de faturamento.


valor_do_envio
objeto
Os custos de envio aplicáveis ​​a cada ciclo.


valor_do_imposto
objeto
O valor do imposto aplicável a cada ciclo.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"current_period": true,
"billing_interval_unit": "YEAR",
"billing_frequency": 1,
"total_billing_cycles": 1,
"current_billing_cycle": 999,
"regular_period": true,
"create_time": "string",
"update_time": "string",
"billing_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"tax_amount": {
"currency_code": "str",
"value": "string"
}
}
id_do_acordo_de_faturamento
O ID do contrato de cobrança do PayPal. Refere-se a um pagamento recorrente aprovado por bens ou serviços.

string [ 2 .. 128 ] caracteres ^[a-zA-Z0-9-]+$( id_do_acordo_de_faturamento )
O ID do contrato de cobrança do PayPal. Refere-se a um pagamento recorrente aprovado por bens ou serviços.

Cópia
"string"
ciclo_de_faturamento
O ciclo de faturamento fornece detalhes sobre a frequência, o valor, a duração da cobrança e se o ciclo é gratuito, com desconto ou regular. A sequência do ciclo de faturamento será na seguinte ordem: ciclo(s) de faturamento de teste gratuito, ciclo(s) de faturamento de teste com desconto, ciclo(s) de faturamento regular.

tipo_de_posse
obrigatório
string [ 1 .. 24 ] caracteres ^[A-Z_]+$
O tipo de duração do ciclo de faturamento identifica se o ciclo de faturamento é um período de teste (gratuito ou com desconto) ou um ciclo de faturamento regular.

Valor de enumeração	Descrição
REGULAR	Um ciclo de faturamento regular para identificar cobranças recorrentes referentes ao contrato de faturamento.
JULGAMENTO	Um ciclo de faturamento de teste serve para identificar cobranças gratuitas ou com desconto no contrato de faturamento. Os testes gratuitos não terão um objeto de preço no esquema de precificação, enquanto um teste com desconto terá um preço reduzido em comparação com o ciclo de faturamento regular.
ciclos_totais
inteiro [ 0 .. 999 ]
Padrão: 
1
O número de vezes que este ciclo de faturamento é executado. Ciclos de faturamento de teste podem ser executados apenas um número finito de vezes (valor entre 1e 999para total_cycles). Ciclos de faturamento regulares podem ser executados infinitas vezes (valor de 0para total_cycles) ou um número finito de vezes (valor entre 1e 999para total_cycles).

sequência
inteiro [ 1 .. 3 ]
Padrão: 
1
A ordem em que este ciclo deve ser executado em relação a outros ciclos de faturamento. Por exemplo, um ciclo de faturamento de teste tem um sequencevalor de 1 1, enquanto um ciclo de faturamento regular tem um sequencevalor de 2 2, de modo que o ciclo de teste seja executado antes do ciclo regular.


esquema_de_preços
objeto
O plano de preços ativo para este ciclo de faturamento. Um período de teste gratuito não requer um plano de preços.

data_inicial
string = 10 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_sem_hora ) Mostrar padrão
Data de início do ciclo de faturamento, no formato AAAA-MM-DD. Este campo não deve ser preenchido se o ciclo de faturamento começar no momento da finalização da compra. Quando este campo não for preenchido, o valor do ciclo de faturamento será incluído em quaisquer validações de dados que confirmem se o total fornecido pelo comerciante corresponde à soma dos itens individuais devidos no momento da finalização da compra. Apenas um ciclo de faturamento (com sequência igual a 1) pode não ter data de início.

CópiaExpandir tudoRecolher tudo
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
detalhes_do_bin
Detalhes do Número de Identificação Bancária (BIN) utilizados para efetuar um pagamento.

lixeira
string [ 1 .. 25 ] caracteres ^[0-9]+$
O Número de Identificação Bancária (BIN) é o número utilizado para identificar os detalhes específicos (exceto as informações de identificação pessoal) do cartão.

banco emissor
string [ 1 .. 64 ] caracteres
A entidade emissora do cartão.

produtos
Array de strings [ 1 .. 256 ] itens
O tipo de produto de cartão atribuído ao BIN pela emissora. Esses valores são definidos pela emissora e podem mudar ao longo do tempo. Alguns exemplos incluem: PREPAID_GIFT, CONSUMER, CORPORATE.

código_do_país_bin
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código de país ISO-3166-1 de dois caracteres do banco.

CópiaExpandir tudoRecolher tudo
{
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
blik
Informações utilizadas para efetuar o pagamento com BLIK.

nome
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código de país ISO 3166-1 de dois caracteres.

e-mail
string [ 3 .. 254 ] caracteres ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...( endereço de email ) Mostrar padrão
O endereço de e-mail do titular da conta associada a este método de pagamento.


um_clique
objeto
O objeto de fluxo de integração com um clique.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"country_code": "string",
"email": "string",
"one_click": {
"consumer_reference": "string"
}
}
blik_experience_context
Personaliza a experiência do pagador durante o processo de aprovação do pagamento BLIK.

nome_da_marca
string [ 1 .. 127 ] caracteres ^.*$
O rótulo que substitui o nome da empresa na conta do PayPal no site do PayPal. O padrão é definido por terceiros e é compatível com Unicode.

preferência de envio
string [ 1 .. 24 ] caracteres ^[A-Z_]+$
Padrão: 
"OBTER_DO_ARQUIVO"
A localização a partir da qual o endereço de entrega é derivado.

Valor de enumeração	Descrição
OBTER_DO_ARQUIVO	Obtenha o endereço de entrega fornecido pelo cliente no site do PayPal.
SEM ENVIO	Oculta o endereço de entrega do site do PayPal. Recomendado para produtos digitais.
DEFINIR_ENDEREÇO_FORNECIDO	O comerciante envia o endereço de entrega usando purchase_units.shipping.address. O cliente não pode alterar esse endereço no site do PayPal.
localidade
string [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}|[...( linguagem ) Mostrar padrão
O idioma formatado em BCP 47 das páginas que a experiência de pagamento do PayPal exibe. O PayPal aceita um código de cinco caracteres. Por exemplo, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HKou zh-TW.

URL de retorno
corda
O URL para onde o cliente é redirecionado após aprovar o pagamento.

url_de_cancelamento
corda
O URL para onde o cliente é redirecionado após o cancelamento do pagamento.

agente_do_usuário_consumidor
string [ 1 .. 256 ] caracteres ^.*$
O agente de usuário do pagador. Por exemplo, Mozilla/5.0 (Macintosh; Intel Mac OS X xy; rv:42.0).

IP do consumidor
string [ 7 .. 39 ] caracteres ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[...( Endereço IP ) Mostrar padrão
O endereço IP do consumidor. Pode ser IPv4 ou IPv6.

Cópia
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"consumer_user_agent": "string",
"consumer_ip": "string"
}
blik_level_0
Informações utilizadas para pagamento através do fluxo BLIK nível 0.

código_de_autorização
obrigatório
string = 6 caracteres ^[0-9]{6}$
Código de 6 dígitos usado para autenticar um consumidor no BLIK.

Cópia
{
"auth_code": "string"
}
blik_one_click
Informações utilizadas para efetuar o pagamento com um clique através do fluxo BLIK.

código_de_autorização
string = 6 caracteres ^[0-9]{6}$
Código de 6 dígitos usado para autenticar um consumidor no BLIK.

referência_do_consumidor
obrigatório
string [ 3 .. 64 ] caracteres ^[ -~]{3,64}$
A referência única gerada pelo comerciante serve como identificador principal para as contas conectadas entre o Blik e o comerciante.

alias_label
string [ 8 .. 35 ] caracteres ^[ -~]{8,35}$
Um identificador definido pelo banco, usado como nome de exibição para permitir que o pagador diferencie entre várias contas bancárias registradas.

chave_alias
string [ 1 .. 19 ] caracteres ^[0-9]+$
Um identificador definido pela Blik para uma conta bancária específica habilitada para Blik, associada a um determinado comerciante. Usado somente em conjunto com uma Referência do Consumidor.

Cópia
{
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
blik_one_click_response
Informações utilizadas para efetuar o pagamento com um clique através do fluxo BLIK.

referência_do_consumidor
string [ 3 .. 64 ] caracteres ^[ -~]{3,64}$
A referência única gerada pelo comerciante serve como identificador principal para as contas conectadas entre o Blik e o comerciante.

Cópia
{
"consumer_reference": "string"
}
blik_request
Informações necessárias para efetuar o pagamento com BLIK.

nome
obrigatório
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$( código_do_país )
O código de país ISO 3166-1 de dois caracteres.

e-mail
string [ 3 .. 254 ] caracteres ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...( endereço de email ) Mostrar padrão
O endereço de e-mail do titular da conta associada a este método de pagamento.


contexto_de_experiência
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.


nível_0
objeto
O objeto de fluxo de integração de nível 0.


um_clique
objeto
O objeto de fluxo de integração com um clique.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"consumer_user_agent": "string",
"consumer_ip": "string"
},
"level_0": {
"auth_code": "string"
},
"one_click": {
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
}
configuração_de_retorno
Configuração de retorno de chamada que o comerciante pode fornecer ao PayPal/Venmo.

eventos_de_retorno
obrigatório
Array de strings [ 1 .. 5 ] itens únicos
Uma série de eventos de retorno de chamada aos quais o comerciante pode se inscrever para o URL de retorno de chamada correspondente.

URL de retorno de chamada
obrigatório
string [ 10 .. 2040 ] caracteres ^.*$
O comerciante forneceu a URL de retorno de chamada. O PayPal/Venmo usará essa URL para contatar o comerciante quando os eventos ocorrerem. O PayPal/Venmo espera uma URL segura, geralmente no formato https. O comerciante pode adicionar o ID do carrinho ou outros parâmetros à URL como parâmetros de consulta ou de caminho.

CópiaExpandir tudoRecolher tudo
{
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
capturar
Pagamento capturado.

tempo_de_criação
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora em que a transação ocorreu, no formato de data e hora da Internet .

hora_de_atualização
string [ 20 .. 64 ] caracteres ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...( data_hora ) Mostrar padrão
A data e a hora da última atualização da transação, no formato de data e hora da Internet .

eu ia
corda
O ID gerado pelo PayPal para o pagamento capturado.

id_da_fatura
corda
O número da fatura externa fornecido pelo solicitante da API para este pedido. Aparece tanto no histórico de transações do pagador quanto nos e-mails que ele recebe.

id_personalizado
string <= 255 caracteres
O ID externo fornecido pelo usuário da API. Usado para conciliar transações iniciadas pelo usuário da API com transações do PayPal. Aparece em relatórios de transações e liquidações.

captura_final
booleano
Padrão: 
falso
Indica se você pode fazer capturas adicionais referentes ao pagamento autorizado. Defina como verdadeiro truese você não pretende capturar pagamentos adicionais referentes à autorização. Defina como falso falsese você pretende capturar pagamentos adicionais referentes à autorização.


links
Conjunto de objetos
Uma série de links relacionados ao HATEOAS .


quantia
objeto
O valor referente a este pagamento registrado.


referência_de_transação_de_rede
objeto
Valores de referência usados ​​pela rede de cartões para identificar uma transação.


proteção_do_vendedor
objeto
O nível de proteção oferecido, conforme definido pela Proteção ao Vendedor do PayPal para Comerciantes .


detalhamento_do_recebível_do_vendedor
objeto
A descrição detalhada da atividade de captura. Esta informação não está disponível para transações pendentes.

modo_de_desembolso
string [ 1 .. 16 ] caracteres ^[A-Z_]+$
Padrão: 
"INSTANTÂNEO"
Os fundos que são mantidos em nome do comerciante.

Valor de enumeração	Descrição
INSTANT	Os fundos são liberados para o comerciante imediatamente.
ATRASADO	Os fundos ficam retidos por um número limitado de dias. A duração exata depende da região e do tipo de integração. Você pode liberar os fundos por meio de um pagamento referenciado. Caso contrário, os fundos serão liberados automaticamente após o período especificado.

resposta_do_processador
objeto
Um objeto que fornece informações adicionais do processador para uma transação direta com cartão de crédito.

CópiaExpandir tudoRecolher tudo
{
"create_time": "string",
"update_time": "string",
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"final_capture": false,
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"seller_receivable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"receivable_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
},
"disbursement_mode": "INSTANT",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
status_de_captura
O status e os detalhes do status de um pagamento registrado.

status
corda
O status do pagamento capturado.

Valor de enumeração	Descrição
CONCLUÍDO	Os fundos referentes a esse pagamento registrado foram creditados na conta PayPal do beneficiário.
RECUSADO	Os fundos não puderam ser recuperados.
REEMBOLSADO PARCIALMENTE	Um valor inferior ao montante do pagamento registrado foi parcialmente reembolsado ao pagador.
PENDENTE	Os fundos referentes a este pagamento capturado ainda não foram creditados na conta PayPal do beneficiário. Para mais informações, consulte [link para a página de informações] status.details.
REEMBOLSADO	Um valor igual ou superior ao valor desse pagamento registrado foi reembolsado ao pagador.
FRACASSADO	Ocorreu um erro ao processar o pagamento.

detalhes_de_status
objeto
Detalhes do status do pagamento registrado.

CópiaExpandir tudoRecolher tudo
{
"status": "COMPLETED",
"status_details": {
"reason": "BUYER_COMPLAINT"
}
}
detalhes_do_status_da_captura
Detalhes do status do pagamento registrado.

razão
string [ 1 .. 64 ] caracteres ^[A-Z_]+$
O motivo pelo qual o status de pagamento capturado é PENDINGou DENIED.

Valor de enumeração	Descrição
RECLAMAÇÃO DO COMPRADOR	O pagador iniciou uma disputa referente a esse pagamento capturado junto ao PayPal.
ESTORNO	Os fundos retidos foram estornados em resposta à contestação do pagamento retido pelo pagador junto à instituição emissora do instrumento financeiro utilizado para efetuar o pagamento.
Cheque eletrônico	O pagador efetuou o pagamento por meio de um cheque eletrônico que ainda não foi compensado.
RETIRADA INTERNACIONAL	Acesse sua conta online. Na Visão Geral da Conta , aceite e recuse este pagamento.
OUTRO	Não é possível fornecer nenhuma outra razão específica. Para obter mais informações sobre este pagamento capturado, acesse sua conta online ou entre em contato com o PayPal.
PENDENTE DE REVISÃO	O pagamento capturado está pendente de análise manual.
RECEBIMENTO DE MANDATOS DE PREFERÊNCIA - AÇÃO MANUAL	O beneficiário ainda não configurou as preferências de recebimento adequadas para sua conta. Para obter mais informações sobre como aceitar ou recusar este pagamento, acesse sua conta online. Essa justificativa geralmente é apresentada em situações como quando a moeda do pagamento capturado é diferente da moeda principal do beneficiário.
REEMBOLSADO	Os fundos retidos foram reembolsados.
TRANSAÇÃO_APROVADA_AGUARDANDO_FINANCIAMENTO	O pagador deve enviar os fundos referentes a este pagamento registrado. Este código geralmente aparece para transferências eletrônicas manuais.
UNILATERAL	O destinatário do pagamento não possui uma conta PayPal.
VERIFICAÇÃO NECESSÁRIA	A conta PayPal do destinatário não está verificada.
REJEITADO POR RISCO_FILTROS_DE_FRAUDE	O filtro de risco definido pelo beneficiário falhou para a transação.
Cópia
{
"reason": "BUYER_COMPLAINT"
}
cartão
O cartão de pagamento utilizado para efetuar um pagamento. Pode ser um cartão de crédito ou de débito.

nome
string [ 1 .. 300 ] caracteres ^.{1,300}$
O nome do titular do cartão, tal como aparece no cartão.

número
string [ 13 .. 19 ] caracteres ^[0-9]{13,19}$
O número da conta principal (PAN) do cartão de pagamento.

código_de_segurança
string [ 3 .. 4 ] caracteres ^[0-9]{3,4}$
O código de segurança de três ou quatro dígitos do cartão. Também conhecido como CVV, CVC, CVN, CVE ou CID. Este parâmetro não pode estar presente na solicitação quando payment_initiator=MERCHANT.

termo
string = 7 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])$
O ano e mês de validade do cartão, no formato de data da Internet. Por exemplo: 2028-04


Endereço de Cobrança
objeto
Endereço de cobrança deste cartão. Suporta apenas as propriedades address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, e .country_code


atributos
objeto
Atributos adicionais associados ao uso deste cartão.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
}
}
cartão
O cartão de pagamento utilizado para efetuar um pagamento. Pode ser um cartão de crédito ou de débito.

nome
string [ 1 .. 300 ] caracteres ^.{1,300}$
O nome do titular do cartão, tal como aparece no cartão.

número
string [ 13 .. 19 ] caracteres ^[0-9]{13,19}$
O número da conta principal (PAN) do cartão de pagamento.

termo
string = 7 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])$
O ano e mês de validade do cartão, no formato de data da Internet. Por exemplo: 2028-04

tipo_de_cartão
string [ 1 .. 255 ] caracteres ^[A-Z_]+$
A marca ou a rede do cartão. Normalmente usada na resposta.

Valor de enumeração	Descrição
VISA	Cartão Visa.
MASTERCARD	Cartão Mastercard.
DESCOBRIR	Cartão Discover.
AMEX	Cartão American Express.
SOLO	Cartão de débito Solo.
JCB	Cartão do Japan Credit Bureau.
ESTRELA	Cartão Estrela Militar.
DELTA	Cartão da Delta Airlines.
TROCAR	Trocar de cartão de crédito.
MAESTRO	Cartão de crédito Maestro.
CB_NACIONAL	Cartão de crédito Carte Bancaire (CB).
CONFIGURAÇÃO	Cartão de crédito Configoga.
CONFIDIS	Cartão de crédito Confidis.
ELÉTRON	Cartão de crédito Visa Electron.
CETELEM	Cartão de crédito Cetelem.
CHINA_UNIÃO_PAGAMENTO	Cartão de crédito China Union Pay.
RESTAURANTES	A rede de serviços bancários e de pagamento da Diners Club International pertence à Discover Financial Services (DFS), uma das marcas mais reconhecidas no setor de serviços financeiros dos EUA.
ELO	A rede brasileira de pagamentos com cartão Elo.
HIPER	A rede de pagamentos eletrônicos Hiper-Ingenico.
HIPERCARD	O Hipercard é a rede de pagamentos brasileira amplamente aceita no mercado varejista.
Rúpia	A rede de pagamentos RuPay.
GE	A rede de pagamentos com cartão 3Point da GE Credit Union.
SINCRONIA	A rede de pagamentos Synchrony Financial (SYF).
EFTPOS	A rede de pagamento com cartão de débito EFTPOS (Transferência Eletrônica de Fundos no Ponto de Venda).
CARTÃO_BANCÁRIO	A rede de pagamento Carte Bancaire.
ACESSO ÀS ESTRELAS	A rede de pagamentos Star Access.
PULSO	A rede de pagamentos Pulse.
NYCE	A rede de pagamentos NYCE.
ACELERAÇÃO	A rede de pagamentos Accel.
DESCONHECIDO	Rede de pagamento desconhecida.
tipo
string [ 1 .. 255 ] caracteres ^[A-Z_]+$
O tipo de cartão de pagamento.

Valor de enumeração	Descrição
CRÉDITO	Um cartão de crédito.
DÉBITO	Um cartão de débito.
PRÉ-PAGO	Um cartão pré-pago.
LOJA	Um cartão da loja.
DESCONHECIDO	Não foi possível determinar o tipo de cartão.
marca
string [ 1 .. 255 ] caracteres ^[A-Z_]+$
A marca ou a rede do cartão. Normalmente usada na resposta.

Valor de enumeração	Descrição
VISA	Cartão Visa.
MASTERCARD	Cartão Mastercard.
DESCOBRIR	Cartão Discover.
AMEX	Cartão American Express.
SOLO	Cartão de débito Solo.
JCB	Cartão do Japan Credit Bureau.
ESTRELA	Cartão Estrela Militar.
DELTA	Cartão da Delta Airlines.
TROCAR	Trocar de cartão de crédito.
MAESTRO	Cartão de crédito Maestro.
CB_NACIONAL	Cartão de crédito Carte Bancaire (CB).
CONFIGURAÇÃO	Cartão de crédito Configoga.
CONFIDIS	Cartão de crédito Confidis.
ELÉTRON	Cartão de crédito Visa Electron.
CETELEM	Cartão de crédito Cetelem.
CHINA_UNIÃO_PAGAMENTO	Cartão de crédito China Union Pay.
RESTAURANTES	A rede de serviços bancários e de pagamento da Diners Club International pertence à Discover Financial Services (DFS), uma das marcas mais reconhecidas no setor de serviços financeiros dos EUA.
ELO	A rede brasileira de pagamentos com cartão Elo.
HIPER	A rede de pagamentos eletrônicos Hiper-Ingenico.
HIPERCARD	O Hipercard é a rede de pagamentos brasileira amplamente aceita no mercado varejista.
Rúpia	A rede de pagamentos RuPay.
GE	A rede de pagamentos com cartão 3Point da GE Credit Union.
SINCRONIA	A rede de pagamentos Synchrony Financial (SYF).
EFTPOS	A rede de pagamento com cartão de débito EFTPOS (Transferência Eletrônica de Fundos no Ponto de Venda).
CARTÃO_BANCÁRIO	A rede de pagamento Carte Bancaire.
ACESSO ÀS ESTRELAS	A rede de pagamentos Star Access.
PULSO	A rede de pagamentos Pulse.
NYCE	A rede de pagamentos NYCE.
ACELERAÇÃO	A rede de pagamentos Accel.
DESCONHECIDO	Rede de pagamento desconhecida.

Endereço de Cobrança
objeto
Endereço de cobrança deste cartão. Suporta apenas as propriedades address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, e .country_code

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
Verificação de cartão
O usuário que chama a API pode optar por verificar o cartão por meio dos serviços de verificação oferecidos pelo PayPal (por exemplo, Smart Dollar Auth, 3DS).

método
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"SCA_QUANDO_NECESSÁRIO"
O método utilizado para verificação do cartão.

Valor de enumeração	Descrição
SCA_SEMPRE	Selecionar esta opção tentará forçar uma autenticação forte do cliente para a autorização/transação. Em países onde a SCA (Autenticação Forte do Cliente) foi definida e implementada, isso resultará em uma resposta de contingência e um link HATEOAS. O solicitante da API deve redirecionar o pagador para esse link para que ele possa se autenticar junto ao seu banco emissor ou outra entidade. Conforme mencionado, o link HATEOAS está disponível apenas em todas as regiões onde a autenticação forte é suportada (por exemplo, em países europeus onde o 3DS está ativo). Os comerciantes podem usar essa configuração como uma camada adicional de segurança, se desejarem. Em todos os casos, quando uma autorização for solicitada, os resultados do AVS/CVV serão retornados na resposta.
SCA_QUANDO_NECESSÁRIO	Esta é a configuração padrão. Quando uma autorização ou transação é tentada, esta opção retornará um link de contingência e HATEOAS somente quando as regulamentações locais exigirem autenticação forte do cliente (por exemplo, 3DS em países e casos de uso onde é obrigatório). O usuário da API deve redirecionar o pagador para o link para que ele possa se autenticar. Em todos os casos, quando uma autorização for solicitada, os resultados do AVS/CVV serão retornados na resposta.
3D_SEGURO	Essa medida de contingência surgiu como uma camada adicional de segurança que ajuda a prevenir transações não autorizadas sem a presença do cartão e protege o comerciante contra fraudes.
AVS_CVV	Bloqueia temporariamente o cartão para garantir sua validade. Esse processo protege o comerciante contra fraudes. Esse método de verificação confirma se as informações de endereço ou o CVV correspondem aos dados registrados pelo banco emissor do cartão, garantindo que apenas usuários autorizados possam fazer compras em seu estabelecimento.
Cópia
{
"method": "SCA_ALWAYS"
}
atributos_do_cartão
Atributos adicionais associados ao uso deste cartão.


cliente
objeto
Os dados pessoais de um cliente registrados no sistema do PayPal.


cofre
objeto
Instruções para guardar a carta no cofre de acordo com a estratégia especificada.


verificação
objeto
Instruções para verificar opcionalmente o cartão com base na estratégia especificada.

CópiaExpandir tudoRecolher tudo
{
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
}
resposta_atributos_do_cartão
Atributos adicionais associados ao uso deste cartão.


cofre
objeto
Detalhes sobre uma forma de pagamento com cartão salva.

CópiaExpandir tudoRecolher tudo
{
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
}
marca_do_cartão
A rede ou bandeira do cartão. Aplica-se a cartões de crédito, débito, presente e pagamento.

string [ 1 .. 255 ] caracteres ^[A-Z_]+$( marca_do_cartão )
A rede ou bandeira do cartão. Aplica-se a cartões de crédito, débito, presente e pagamento.

Valor de enumeração	Descrição
VISA	Cartão Visa.
MASTERCARD	Cartão Mastercard.
DESCOBRIR	Cartão Discover.
AMEX	Cartão American Express.
SOLO	Cartão de débito Solo.
JCB	Cartão do Japan Credit Bureau.
ESTRELA	Cartão Estrela Militar.
DELTA	Cartão da Delta Airlines.
TROCAR	Trocar de cartão de crédito.
MAESTRO	Cartão de crédito Maestro.
CB_NACIONAL	Cartão de crédito Carte Bancaire (CB).
CONFIGURAÇÃO	Cartão de crédito Configoga.
CONFIDIS	Cartão de crédito Confidis.
ELÉTRON	Cartão de crédito Visa Electron.
CETELEM	Cartão de crédito Cetelem.
CHINA_UNIÃO_PAGAMENTO	Cartão de crédito China Union Pay.
RESTAURANTES	A rede de serviços bancários e de pagamento da Diners Club International pertence à Discover Financial Services (DFS), uma das marcas mais reconhecidas no setor de serviços financeiros dos EUA.
ELO	A rede brasileira de pagamentos com cartão Elo.
HIPER	A rede de pagamentos eletrônicos Hiper-Ingenico.
HIPERCARD	O Hipercard é a rede de pagamentos brasileira amplamente aceita no mercado varejista.
Rúpia	A rede de pagamentos RuPay.
GE	A rede de pagamentos com cartão 3Point da GE Credit Union.
SINCRONIA	A rede de pagamentos Synchrony Financial (SYF).
EFTPOS	A rede de pagamento com cartão de débito EFTPOS (Transferência Eletrônica de Fundos no Ponto de Venda).
CARTÃO_BANCÁRIO	A rede de pagamento Carte Bancaire.
ACESSO ÀS ESTRELAS	A rede de pagamentos Star Access.
PULSO	A rede de pagamentos Pulse.
NYCE	A rede de pagamentos NYCE.
ACELERAÇÃO	A rede de pagamentos Accel.
DESCONHECIDO	Rede de pagamento desconhecida.
Cópia
"VISA"
cliente_do_cartão
Os dados pessoais de um cliente registrados no sistema do PayPal.

eu ia
string [ 1 .. 22 ] caracteres ^[0-9a-zA-Z_-]+$
O ID exclusivo de um cliente gerado pelo PayPal.

endereço de email
string [ 3 .. 254 ] caracteres (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...( e-mail ) Mostrar padrão
Endereço de e-mail do cliente, conforme fornecido ao comerciante ou registrado em seus arquivos. O endereço de e-mail é obrigatório caso a transação seja processada por meio do Processamento de Convidados do PayPal, recurso oferecido a parceiros e comerciantes selecionados.


telefone
objeto
O número de telefone do cliente, conforme fornecido ao comerciante ou registrado em seus arquivos. phone.phone_numberApenas para suporte national_number.


nome
objeto
O nome completo do cliente, conforme fornecido ao comerciante ou registrado em arquivo pelo comerciante.

id_do_cliente_do_comerciante
string [ 1 .. 64 ] caracteres ^[0-9a-zA-Z-_.^*$@#]+$
Comerciantes e parceiros podem já ter um banco de dados onde as informações de seus clientes são armazenadas. Use o `merchant_customer_id` para associar o `customer.id` gerado pelo PayPal à sua representação do cliente.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
contexto_de_experiência_do_cartão
Personaliza a experiência do pagador durante a aprovação do pagamento no sistema 3DS.

URL de retorno
string [ 10 .. 4000 ] caracteres( url )
O URL para onde o cliente será redirecionado após concluir com sucesso o desafio 3DS.

url_de_cancelamento
string [ 10 .. 4000 ] caracteres( url )
O URL para onde o cliente será redirecionado após o cancelamento do desafio 3DS.

Cópia
{
"return_url": "string",
"cancel_url": "string"
}
cartão_de_pedido
Representação dos dados do cartão conforme recebidos na solicitação.

últimos_dígitos
string [ 2 .. 4 ] caracteres [0-9]{2,}
Os últimos dígitos do cartão de pagamento.

termo
string = 7 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])$
O ano e mês de validade do cartão, em formato de data da Internet .

Cópia
{
"last_digits": "stri",
"expiry": "string"
}
solicitação de cartão
O cartão de pagamento utilizado para efetuar um pagamento. Pode ser um cartão de crédito ou de débito.

Observação: o envio direto do número do cartão, CVV e data de validade via API exige conformidade com o PCI SAQ D. O
PayPal oferece um mecanismo que permite evitar essa exigência, utilizando campos hospedados. Consulte este Guia de Integração .
nome
string [ 1 .. 300 ] caracteres ^.{1,300}$
O nome do titular do cartão, tal como aparece no cartão.

número
string [ 13 .. 19 ] caracteres ^[0-9]{13,19}$
O número da conta principal (PAN) do cartão de pagamento.

código_de_segurança
string [ 3 .. 4 ] caracteres ^[0-9]{3,4}$
O código de segurança de três ou quatro dígitos do cartão. Também conhecido como CVV, CVC, CVN, CVE ou CID. Este parâmetro não pode estar presente na solicitação quando payment_initiator=MERCHANT.

termo
string = 7 caracteres ^[0-9]{4}-(0[1-9]|1[0-2])$
O ano e mês de validade do cartão, no formato de data da Internet. Por exemplo: 2028-04


Endereço de Cobrança
objeto
Endereço de cobrança deste cartão. Suporta apenas as propriedades address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, e .country_code


atributos
objeto
Atributos adicionais associados ao uso deste cartão.


credencial armazenada
objeto
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
vault_id	
string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated ID for the saved card payment source. Typically stored on the merchant's server.

single_use_token	
string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated, short-lived, one-time-use token, used to communicate payment information to PayPal for transaction processing.

network_token	
object
A 3rd party network token refers to a network token that the merchant provisions from and vaults with an external TSP (Token Service Provider) other than PayPal.

experience_context	
object
Customizes the payer experience during the 3DS Approval for payment.

CopyExpand allCollapse all
{
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
}
card_response
The payment card to use to fund a payment. Card can be a credit or debit card.

name	
string [ 2 .. 300 ] characters
The card holder's name as it appears on the card.

last_digits	
string
The last digits of the payment card.

available_networks	
Array of strings [ 1 .. 256 ] items
Array of brands or networks associated with the card.

Items Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
from_request	
object
Representation of card details as received in the request.

stored_credential	
object
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The payment card type.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
authentication_result	
object
Results of Authentication such as 3D Secure.

attributes	
object
Additional attributes associated with the use of this card.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

bin_details	
object
Bank Identification Number (BIN) details used to fund a payment.

CopyExpand allCollapse all
{
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
}
card_response
The payment card to use to fund a payment. Card can be a credit or debit card.

name	
string [ 2 .. 300 ] characters
The card holder's name as it appears on the card.

last_digits	
string
The last digits of the payment card.

available_networks	
Array of strings [ 1 .. 256 ] items
Array of brands or networks associated with the card.

Items Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
from_request	
object
Representation of card details as received in the request.

stored_credential	
object
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The payment card type.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
authentication_result	
object
Results of Authentication such as 3D Secure.

attributes	
object
Additional attributes associated with the use of this card.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

bin_details	
object
Bank Identification Number (BIN) details used to fund a payment.

CopyExpand allCollapse all
{
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
}
card_stored_credential
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
payment_initiator
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The person or party who initiated or triggered the payment.

Enum Value	Description
CUSTOMER	Payment is initiated with the active engagement of the customer. e.g. a customer checking out on a merchant website.
MERCHANT	Payment is initiated by merchant on behalf of the customer without the active engagement of customer. e.g. a merchant charging the monthly payment of a subscription to the customer.
payment_type
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Indicates the type of the stored payment_source payment.

Enum Value	Description
ONE_TIME	One Time payment such as online purchase or donation. (e.g. Checkout with one-click).
RECURRING	Payment which is part of a series of payments with fixed or variable amounts, following a fixed time interval. (e.g. Subscription payments).
UNSCHEDULED	Payment which is part of a series of payments that occur on a non-fixed schedule and/or have variable amounts. (e.g. Account Topup payments).
usage	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "DERIVED"
Indicates if this is a first or subsequent payment using a stored payment source (also referred to as stored credential or card on file).

Enum Value	Description
FIRST	Indicates the Initial/First payment with a payment_source that is intended to be stored upon successful processing of the payment.
SUBSEQUENT	Indicates a payment using a stored payment_source which has been successfully used previously for a payment.
DERIVED	Indicates that PayPal will derive the value of FIRST or SUBSEQUENT based on data available to PayPal.
previous_network_transaction_reference	
object
Reference values used by the card network to identify a transaction.

CopyExpand allCollapse all
{
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
card_supplementary_data
Merchants and partners can add Level 2 and 3 data to payments to reduce risk and payment processing costs. For more information about processing payments, see checkout or multiparty checkout.

level_2	
object
The level 2 card processing data collections. If your merchant account has been configured for Level 2 processing this field will be passed to the processor on your behalf. Please contact your PayPal Technical Account Manager to define level 2 data for your business.

level_3	
object
The level 3 card processing data collections, If your merchant account has been configured for Level 3 processing this field will be passed to the processor on your behalf. Please contact your PayPal Technical Account Manager to define level 3 data for your business.

CopyExpand allCollapse all
{
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": null,
"price": null,
"reload_threshold_amount": null
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
}
card_type
Type of card. i.e Credit, Debit and so on.

string [ 1 .. 255 ] characters ^[A-Z_]+$
Type of card. i.e Credit, Debit and so on.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
Copy
"CREDIT"
card_vault_response
The details about a saved Card payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The vault status.

Enum Value	Description
VAULTED	The payment source has been saved in your customer's vault. This vault status reflects /v3/vault status.
CREATED	DEPRECATED. The payment source has been saved in your customer's vault. This status applies to deprecated integration patterns and will not be returned for v3/vault integrations.
APPROVED	Customer has approved the action of saving the specified payment_source into their vault. Use v3/vault/payment-tokens with given setup_token to save the payment source in the vault
links	
Array of objects [ 1 .. 10 ] items
An array of request-related HATEOAS links.

customer	
object
The details about a customer in PayPal's system of record.

CopyExpand allCollapse all
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
carrier
The carrier for the shipment. Some carriers have a global version as well as local subsidiaries. The subsidiaries are repeated over many countries and might also have an entry in the global list. Choose the carrier for your country. If the carrier is not available for your country, choose the global version of the carrier. If your carrier name is not in the list, set carrier to OTHER and set carrier name in carrier_name_other. For allowed values, see Carriers.

string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The carrier for the shipment. Some carriers have a global version as well as local subsidiaries. The subsidiaries are repeated over many countries and might also have an entry in the global list. Choose the carrier for your country. If the carrier is not available for your country, choose the global version of the carrier. If your carrier name is not in the list, set carrier to OTHER and set carrier name in carrier_name_other. For allowed values, see Carriers.

Enum Value	Description
DPD_RU	DPD Russia.
BG_BULGARIAN_POST	Bulgarian Posts.
KR_KOREA_POST	Koreapost (www.koreapost.go.kr).
ZA_COURIERIT	Courier IT.
FR_EXAPAQ	DPD France (formerly exapaq).
ARE_EMIRATES_POST	Emirates Post.
GAC	GAC.
GEIS	Geis CZ.
SF_EX	SF Express.
PAGO	Pago Logistics.
MYHERMES	MyHermes UK.
DIAMOND_EUROGISTICS	Diamond Eurogistics Limited.
CORPORATECOURIERS_WEBHOOK	Corporate Couriers.
BOND	Bond courier.
OMNIPARCEL	Omni Parcel.
SK_POSTA	Slovenska pošta.
PUROLATOR	purolator.
FETCHR_WEBHOOK	Mena 360 (Fetchr).
THEDELIVERYGROUP	TDG – The Delivery Group.
CELLO_SQUARE	Cello Square.
TARRIVE	TONDA GLOBAL.
COLLIVERY	MDS Collivery Pty (Ltd).
MAINFREIGHT	Mainfreight.
IND_FIRSTFLIGHT	First Flight Couriers.
ACSWORLDWIDE	ACS Worldwide Express.
AMSTAN	Amstan Logistics.
OKAYPARCEL	OkayParcel.
ENVIALIA_REFERENCE	Envialia Reference.
SEUR_ES	Seur Spain.
CONTINENTAL	Continental.
FDSEXPRESS	FDSEXPRESS.
AMAZON_FBA_SWISHIP	Swiship UK.
WYNGS	Wyngs.
DHL_ACTIVE_TRACING	DHL Active Tracing.
ZYLLEM	Zyllem.
RUSTON	Ruston.
XPOST	Xpost.ph.
CORREOS_ES	correos Express (www.correos.es).
DHL_FR	DHL France (www.dhl.com).
PAN_ASIA	Pan-Asia International.
BRT_IT	BRT couriers Italy (www.brt.it).
SRE_KOREA	SRE Korea (www.srekorea.co.kr).
SPEEDEE	Spee-Dee Delivery.
TNT_UK	TNT UK Limited (www.tnt.com).
VENIPAK	Venipak.
SHREENANDANCOURIER	SHREE NANDAN COURIER.
CROSHOT	Croshot.
NIPOST_NG	NIpost (www.nipost.gov.ng).
EPST_GLBL	ePost Global.
NEWGISTICS	Newgistics.
POST_SLOVENIA	Post of Slovenia.
JERSEY_POST	Jersey Post.
BOMBINOEXP	Bombino Express Pvt.
WMG	WMG Delivery.
XQ_EXPRESS	XQ Express.
FURDECO	Furdeco.
LHT_EXPRESS	LHT Express.
SOUTH_AFRICAN_POST_OFFICE	South African Post Office.
SPOTON	SPOTON Logistics Pvt Ltd.
DIMERCO	Dimerco Express Group.
CYPRUS_POST_CYP	cyprus post.
ABCUSTOM	AB Custom Group.
IND_DELIVREE	deliverE.
CN_BESTEXPRESS	Best Express.
DX_SFTP	DX (SFTP).
PICKUPP_MYS	PICK UPP.
FMX	FMX.
HELLMANN	Hellmann Worldwide Logistics.
SHIP_IT_ASIA	Ship It Asia.
KERRY_ECOMMERCE	Kerry eCommerce.
FRETERAPIDO	Frete Rapido.
PITNEY_BOWES	Pitney Bowes.
XPRESSEN_DK	Xpressen courier.
SEUR_SP_API	Spanish Seur API.
DELIVERYONTIME	DELIVERYONTIME LOGISTICS PVT LTD.
JINSUNG	JINSUNG TRADING.
TRANS_KARGO	Trans Kargo Internasional.
SWISHIP_DE	Swiship DE.
IVOY_WEBHOOK	Ivoy courier.
AIRMEE_WEBHOOK	Airmee couriers.
DHL_BENELUX	dhl benelux.
FIRSTMILE	FirstMile.
FASTWAY_IR	Fastway Ireland.
HH_EXP	Hua Han Logistics.
MYS_MYPOST_ONLINE	Mypostonline.
TNT_NL	THT Netherland.
TIPSA	TIPSA courier.
TAQBIN_MY	TAQBIN Malaysia.
KGMHUB	KGM Hub.
INTEXPRESS	Internet Express.
OVERSE_EXP	Overseas Express.
ONECLICK	One click delivery services.
ROADRUNNER_FREIGHT	Roadbull Logistics.
GLS_CROTIA	GLS Croatia.
MRW_FTP	MRW courier.
BLUEX	Blue Express.
DYLT	Daylight Transport.
DPD_IR	DPD Ireland.
SIN_GLBL	Sin Global Express.
TUFFNELLS_REFERENCE	Tuffnells Parcels Express- Reference.
CJPACKET	CJ Packet.
MILKMAN	Milkman courier.
ASIGNA	ASIGNA courier.
ONEWORLDEXPRESS	One World Express.
ROYAL_MAIL	RoyalShipments.
VIA_EXPRESS	Viaxpress.
TIGFREIGHT	TIG Freight.
ZTO_EXPRESS	ZTO Express.
TWO_GO	2GO Courier.
IML	IML courier.
INTEL_VALLEY	Intel-Valley Supply chain (ShenZhen) Co. Ltd.
EFS	EFS (E-commerce Fulfillment Service).
UK_UK_MAIL	UK mail (ukmail.com).
RAM	RAM courier.
ALLIEDEXPRESS	Allied Express.
APC_OVERNIGHT	APC overnight (apc-overnight.com).
SHIPPIT	Shippit.
TFM	TFM Xpress.
M_XPRESS	M Xpress Sdn Bhd.
HDB_BOX	Haidaibao (BOX).
CLEVY_LINKS	Clevy Links.
IBEONE	Beone Logistics.
FIEGE_NL	Fiege Netherlands.
KWE_GLOBAL	KWE Global.
CTC_EXPRESS	CTC Express.
AMAZON	Amazon Shipping.
MORE_LINK	Morelink.
JX	JX courier.
EASY_MAIL	Easy Mail.
ADUIEPYLE	A Duie Pyle.
GB_PANTHER	Panther.
EXPRESSSALE	Expresssale.
SG_DETRACK	Detrack.
TRUNKRS_WEBHOOK	Trunkrs courier.
MATDESPATCH	Matdespatch.
DICOM	GLS Logistic Systems Canada Ltd./Dicom.
MBW	MBW Courier Inc..
KHM_CAMBODIA_POST	Cambodia Post.
SINOTRANS	Sinotrans.
BRT_IT_PARCELID	BRT Bartolini(Parcel ID).
DHL_SUPPLY_CHAIN	DHL Supply Chain APAC.
DHL_PL	DHL Poland.
TOPYOU	TopYou.
PALEXPRESS	PAL Express Limited.
DHL_SG	dhl Singapore.
CN_WEDO	WeDo Logistics.
FULFILLME	Fulfillme.
DPD_DELISTRACK	DPD delistrack.
UPS_REFERENCE	UPS Reference.
CARIBOU	Caribou.
LOCUS_WEBHOOK	Locus courier.
DSV	DSV courier.
P2P_TRC	P2P TrakPak.
DIRECTPARCELS	Direct Parcels.
NOVA_POSHTA_INT	Nova Poshta (International).
FEDEX_POLAND	FedEx® Poland Domestic.
CN_JCEX	JCEX courier.
FAR_INTERNATIONAL	FAR international.
IDEXPRESS	IDEX courier.
GANGBAO	GANGBAO Supplychain.
NEWAY	Neway Transport.
POSTNL_INT_3_S	PostNL International.
RPX_ID	RPX Indonesia.
DESIGNERTRANSPORT_WEBHOOK	Designer Transport.
GLS_SLOVEN	GLS Slovenia.
PARCELLED_IN	Parcelled.in.
GSI_EXPRESS	GSI EXPRESS.
CON_WAY	Con-way Freight.
BROUWER_TRANSPORT	Brouwer Transport en Logistiek.
CPEX	Captain Express International.
ISRAEL_POST	Israel Post.
DTDC_IN	DTDC India.
PTT_POST	PTT Post.
XDE_WEBHOOK	Ximex Delivery Express.
TOLOS	Tolos courier.
GIAO_HANG	Giao hàng nhanh.
GEODIS_ESPACE	Geodis E-space.
MAGYAR_HU	Magyar Post.
DOORDASH_WEBHOOK	DoorDash.
TIKI_ID	Tiki shipment.
CJ_HK_INTERNATIONAL	CJ Logistics International(Hong Kong).
STAR_TRACK_EXPRESS	Star Track Express.
HELTHJEM	Helthjem.
SFB2C	SF International.
FREIGHTQUOTE	Freightquote by C.H. Robinson.
LANDMARK_GLOBAL_REFERENCE	Landmark Global Reference.
PARCEL2GO	Parcel2Go.
DELNEXT	Delnext.
RCL	Red Carpet Logistics.
CGS_EXPRESS	CGS Express.
HK_POST	Hongkong Post (www.hongkongpost.hk).
SAP_EXPRESS	SAP EXPRESS.
PARCELPOST_SG	Parcel Post Singapore.
HERMES	HermesWorld UK.
IND_SAFEEXPRESS	Safexpress.
TOPHATTEREXPRESS	Tophatter Express.
MGLOBAL	PT MGLOBAL LOGISTICS INDONESIA.
AVERITT	Averitt Express.
LEADER	leader.
_2EBOX	2ebox courier.
SG_SPEEDPOST	Singapore Speedpost.
DBSCHENKER_SE	DB Schenker (www.dbschenker.com).
ISR_POST_DOMESTIC	Israel Post Domestic.
BESTWAYPARCEL	Best Way Parcel.
ASENDIA_DE	asendia_de.
NIGHTLINE_UK	nightline_uk.
TAQBIN_SG	taqbin_sg.
TCK_EXPRESS	TCK Express.
ENDEAVOUR_DELIVERY	Endeavour Delivery.
NANJINGWOYUAN	Nanjing Woyuan.
HEPPNER_FR	Heppner France.
EMPS_CN	EMPS Express.
FONSEN	Fonsen Logistics.
PICKRR	Pickrr.
APC_OVERNIGHT_CONNUM	APC Overnight Consignment.
STAR_TRACK_NEXT_FLIGHT	Star Track Next Flight.
DAJIN	Shanghai Aqrum Chemical Logistics Co.Ltd.
UPS_FREIGHT	UPS Freight.
POSTA_PLUS	Posta Plus.
CEVA	CEVA LOGISTICS.
ANSERX	ANSERX courier.
JS_EXPRESS	JS EXPRESS.
PADTF	padtf.com.
UPS_MAIL_INNOVATIONS	UPS Mail Innovations.
SYPOST	Sunyou Post.
AMAZON_SHIP_MCF	Amazon Shipping + Amazon MCF.
YUSEN	Yusen Logistics.
BRING	Bring.
SDA_IT	SDA Italy.
GBA	GBA Services Ltd.
NEWEGGEXPRESS	Newegg Express.
SPEEDCOURIERS_GR	Speed Couriers.
FORRUN	forrun Pvt Ltd (Arpatech Venture).
PICKUP	Pickupp.
ECMS	ECMS International Logistics Co..
INTELIPOST	Intelipost (TMS for LATAM).
FLASHEXPRESS	Flash Express.
CN_STO	STO Express.
SEKO_SFTP	SEKO Worldwide.
HOME_DELIVERY_SOLUTIONS	Home Delivery Solutions Ltd.
DPD_HGRY	DPD Hungary.
KERRYTTC_VN	Kerry Express (Vietnam) Co Ltd.
JOYING_BOX	Joying Box.
TOTAL_EXPRESS	Total Express.
ZJS_EXPRESS	ZJS International.
STARKEN	STARKEN couriers.
DEMANDSHIP	DemandShip.
CN_DPEX	DPEX.
AUPOST_CN	AuPost China.
LOGISTERS	Logisters.
GOGLOBALPOST	Global Post.
GLS_CZ	GLS Czech Republic.
PAACK_WEBHOOK	Paack courier.
GRAB_WEBHOOK	Grab courier.
PARCELPOINT	Parcelpoint.
ICUMULUS	iCumulus.
DAIGLOBALTRACK	DAI Post.
GLOBAL_IPARCEL	i-parcel.
YURTICI_KARGO	Yurtici Kargo.
CN_PAYPAL_PACKAGE	PayPal Package.
PARCEL_2_POST	Parcel To Post.
GLS_IT	GLS Italy.
PIL_LOGISTICS	PIL Logistics (China) Co..
HEPPNER	Heppner Internationale Spedition GmbH & Co..
GENERAL_OVERNIGHT	Go!Express and logistics.
HAPPY2POINT	Happy 2ThePoint.
CHITCHATS	Chit Chats.
SMOOTH	Smooth Couriers.
CLE_LOGISTICS	CL E-Logistics Solutions Limited.
FIEGE	Fiege Logistics.
MX_CARGO	M&X cargo.
ZIINGFINALMILE	Ziing Final Mile Inc.
DAYTON_FREIGHT	Dayton Freight.
TCS	TCS courier.
AEX	AEX Group.
HERMES_DE	Hermes Germany.
ROUTIFIC_WEBHOOK	Routific.
GLOBAVEND	Globavend.
CJ_LOGISTICS	CJ Logistics International.
PALLET_NETWORK	The Pallet Network.
RAF_PH	RAF Philippines.
UK_XDP	XDP Express.
PAPER_EXPRESS	Paper Express.
LA_POSTE_SUIVI	La Poste.
PAQUETEXPRESS	Paquetexpress.
LIEFERY	liefery.
STRECK_TRANSPORT	Streck Transport.
PONY_EXPRESS	Pony express.
ALWAYS_EXPRESS	Always Express.
GBS_BROKER	GBS-Broker.
CITYLINK_MY	City-Link Express.
ALLJOY	ALLJOY SUPPLY CHAIN.
YODEL	yodel.
YODEL_DIR	Yodel Direct.
STONE3PL	STONE3PL.
PARCELPAL_WEBHOOK	ParcelPal.
DHL_ECOMERCE_ASA	DHL eCommerce Asia (API).
SIMPLYPOST	J&T Express Singapore.
KY_EXPRESS	Kua Yue Express.
SHENZHEN	shenzhen 1st International Logistics(Group)Co.
US_LASERSHIP	LaserShip.
UC_EXPRE	ucexpress.
DIDADI	DIDADI Logistics tech.
CJ_KR	CJ Korea Express.
DBSCHENKER_B2B	DB Schenker B2B.
MXE	MXE Express.
CAE_DELIVERS	CAE Delivers.
PFCEXPRESS	PFC Express.
WHISTL	Whistl.
WEPOST	WePost Sdn Bhd.
DHL_PARCEL_ES	DHL parcel Spain(www.dhl.com).
DDEXPRESS	DD Express Courier.
ARAMEX_AU	Aramex Australia (formerly Fastway AU).
BNEED	Bneed courier.
HK_TGX	Kerry Express Hong Kong.
LATVIJAS_PASTS	Latvijas Pasts.
VIAEUROPE	ViaEurope.
CORREO_UY	Correo Uruguayo.
CHRONOPOST_FR	Chronopost france (www.chronopost.fr).
J_NET	J-Net.
_6LS	6ls.com.
BLR_BELPOST	Belpost.
BIRDSYSTEM	BirdSystem.
DOBROPOST	DobroPost.
WAHANA_ID	Wahana express (www.wahana.com).
WEASHIP	Weaship.
SONICTL	Sonic Transportation & Logistics.
KWT	Shenzhen Jinghuada Logistics Co..
AFLLOG_FTP	AFL LOGISTICS.
SKYNET_WORLDWIDE	SkyNet Worldwide Express.
NOVA_POSHTA	Nova Poshta (novaposhta.ua).
SEINO	Seino.
SZENDEX	SZENDEX.
BPOST_INT	Bpost international.
DBSCHENKER_SV	DB Schenker Sweden.
AO_DEUTSCHLAND	AO Deutschland.
EU_FLEET_SOLUTIONS	EU Fleet Solutions.
PCFCORP	PCF Final Mile.
LINKBRIDGE	Link Bridge(BeiJing)international logistics co..
PRIMAMULTICIPTA	PT Prima Multi Cipta.
COUREX	Urbanfox.
ZAJIL_EXPRESS	Zajil Express Company.
COLLECTCO	CollectCo.
JTEXPRESS	J&T EXPRESS MALAYSIA.
FEDEX_UK	FedEx® UK.
USHIP	uShip courier.
PIXSELL	PIXSELL LOGISTICS.
SHIPTOR	Shiptor.
CDEK	CDEK courier.
VNM_VIETTELPOST	ViettelPost.
CJ_CENTURY	CJ Century.
GSO	GSO(GLS-USA).
VIWO	VIWO IoT.
SKYBOX	SKYBOX.
KERRYTJ	Kerry TJ Logistics.
NTLOGISTICS_VN	Nhat Tin Logistics.
SDH_SCM	lightning monkey.
ZINC	Zinc courier.
DPE_SOUTH_AFRC	DPE South Africa.
CESKA_CZ	Czech Post.
ACS_GR	ACS Courier.
DEALERSEND	DealerSend.
JOCOM	Jocom.
CSE	CSE courier.
TFORCE_FINALMILE	TForce Final Mile.
SHIP_GATE	ShipGate.
SHIPTER	SHIPTER.
NATIONAL_SAMEDAY	National Sameday.
YUNEXPRESS	YunExpress.
CAINIAO	AliExpress Standard Shipping.
DMS_MATRIX	DMSMatrix.
DIRECTLOG	Directlog (www.directlog.com.br).
ASENDIA_US	Asendia USA.
_3JMSLOGISTICS	3JMS Logistics.
LICCARDI_EXPRESS	LICCARDI EXPRESS COURIER.
SKY_POSTAL	SkyPostal.
CNWANGTONG	cnwangtong.
POSTNORD_LOGISTICS_DK	ostnord denmark.
LOGISTIKA	Logistika.
CELERITAS	Celeritas Transporte.
PRESSIODE	Pressio.
SHREE_MARUTI	Shree Maruti Courier Services Pvt Ltd.
LOGISTICSWORLDWIDE_HK	Logistic Worldwide Express (LWE Honkong).
EFEX	eFEx (E-Commerce Fulfillment & Express).
LOTTE	Lotte Global Logistics.
LONESTAR	Lone Star Overnight.
APRISAEXPRESS	Aprisa Express.
BEL_RS	BEL North Russia.
OSM_WORLDWIDE	OSM Worldwide.
WESTGATE_GL	Westgate Global.
FASTRACK	Fasttrack.
DTD_EXPR	DTD Express.
ALFATREX	AlfaTrex.
PROMEDDELIVERY	ProMed Delivery.
THABIT_LOGISTICS	Thabit Logistics.
HCT_LOGISTICS	HCT LOGISTICS CO.LTD..
CARRY_FLAP	Carry-Flap Co..
US_OLD_DOMINION	Old Dominion Freight Line.
ANICAM_BOX	ANICAM BOX EXPRESS.
WANBEXPRESS	WanbExpress.
AN_POST	An Post.
DPD_LOCAL	DPD Local.
STALLIONEXPRESS	Stallion Express.
RAIDEREX	RaidereX.
SHOPFANS	ShopfansRU LLC.
KYUNGDONG_PARCEL	Kyungdong Parcel.
CHAMPION_LOGISTICS	Champion Logistics.
PICKUPP_SGP	PICK UPP (Singapore).
MORNING_EXPRESS	Morning Express.
NACEX	NACEX.
THENILE_WEBHOOK	SortHub courier.
HOLISOL	Holisol.
LBCEXPRESS_FTP	LBC EXPRESS INC..
KURASI	KURASI.
USF_REDDAWAY	USF Reddaway.
APG	APG eCommerce Solutions.
CN_BOXC	BoxC courier.
ECOSCOOTING	ECOSCOOTING.
MAINWAY	Mainway.
PAPERFLY	Paperfly Private Limited.
HOUNDEXPRESS	Hound Express.
BOX_BERRY	Boxberry courier.
EP_BOX	EP-Box courier.
PLUS_LOG_UK	Plus UK Logistics.
FULFILLA	Fulfilla.
ASE	ASE KARGO.
MAIL_PLUS	MailPlus.
XPO_LOGISTICS	XPO logistics.
WNDIRECT	wnDirect.
CLOUDWISH_ASIA	Cloudwish Asia.
ZELERIS	Zeleris.
GIO_EXPRESS	Gio Express.
OCS_WORLDWIDE	OCS WORLDWIDE.
ARK_LOGISTICS	ARK Logistics.
AQUILINE	Aquiline.
PILOT_FREIGHT	Pilot Freight Services.
QWINTRY	Qwintry Logistics.
DANSKE_FRAGT	Danske Fragtaend.
CARRIERS	Carriers courier.
AIR_CANADA_GLOBAL	Rivo (Air canada).
PRESIDENT_TRANS	PRESIDENT TRANSNET CORP.
STEPFORWARDFS	STEP FORWARD FREIGHT SERVICE CO LTD.
SKYNET_UK	Skynet UK.
PITTOHIO	PITT OHIO.
CORREOS_EXPRESS	Correos Express.
RL_US	RL Carriers.
DESTINY	Destiny Transportation.
UK_YODEL	Yodel (www.yodel.co.uk).
COMET_TECH	CometTech.
DHL_PARCEL_RU	DHL Parcel Russia.
TNT_REFR	TNT Reference.
SHREE_ANJANI_COURIER	Shree Anjani Courier.
MIKROPAKKET_BE	Mikropakket Belgium.
ETS_EXPRESS	RETS express.
COLIS_PRIVE	Colis Privé.
CN_YUNDA	Yunda Express.
AAA_COOPER	AAA Cooper.
ROCKET_PARCEL	Rocket Parcel International.
_360LION	360 Lion Express.
PANDU	PANDU.
PROFESSIONAL_COURIERS	PROFESSIONAL COURIERS.
FLYTEXPRESS	FLYTEXPRESS.
LOGISTICSWORLDWIDE_MY	LOGISTICSWORLDWIDE MY.
CORREOS_DE_ESPANA	CORREOS DE ESPANA.
IMX	IMX.
FOUR_PX_EXPRESS	FOUR PX EXPRESS.
XPRESSBEES	XPRESSBEES.
PICKUPP_VNM	pickupp_vnm.
STARTRACK_EXPRESS	startrack_express.
FR_COLISSIMO	fr_colissimo.
NACEX_SPAIN_REFERENCE	nacex_spain_reference.
DHL_SUPPLY_CHAIN_AU	dhl_supply_chain_au.
ESHIPPING	Eshipping.
SHREETIRUPATI	SHREE TIRUPATI COURIER SERVICES PVT. LTD..
HX_EXPRESS	HX Express.
INDOPAKET	INDOPAKET.
CN_17POST	17 Post Service.
K1_EXPRESS	K1 Express.
CJ_GLS	CJ GLS.
MYS_GDEX	GDEX courier.
NATIONEX	Nationex courier.
ANJUN	Anjun couriers.
FARGOOD	FarGood.
SMG_EXPRESS	SMG Direct.
RZYEXPRESS	RZY Express.
SEFL	Southeastern Freight Lines.
TNT_CLICK_IT	TNT-Click Italy.
HDB	Haidaibao.
HIPSHIPPER	Hipshipper.
RPXLOGISTICS	RPX Logistics.
KUEHNE	Kuehne + Nagel.
IT_NEXIVE	Nexive (TNT Post Italy).
PTS	PTS courier.
SWISS_POST_FTP	Swiss Post FTP.
FASTRK_SERV	Fastrak Services.
_4_72	4-72 Entregando.
US_YRC	YRC courier.
POSTNL_INTL_3S	PostNL International 3S.
ELIAN_POST	Yilian (Elian) Supply Chain.
CUBYN	Cubyn.
SAU_SAUDI_POST	Saudi Post.
ABXEXPRESS_MY	ABX Express.
HUAHAN_EXPRESS	HUAHANG EXPRESS.
ZES_EXPRESS	Eshun international Logistic.
ZEPTO_EXPRESS	ZeptoExpress.
SKYNET_ZA	Skynet World Wide Express South Africa.
ZEEK_2_DOOR	Zeek2Door.
BLINKLASTMILE	Blink.
POSTA_UKR	UkrPoshta.
CHROBINSON	C.H. Robinson Worldwide.
CN_POST56	Post56.
COURANT_PLUS	Courant Plus.
SCUDEX_EXPRESS	Scudex Express.
SHIPENTEGRA	ShipEntegra.
B_TWO_C_EUROPE	B2C courier Europe.
COPE	Cope Sensitive Freight.
IND_GATI	Gati-KWE.
CN_WISHPOST	WishPost.
NACEX_ES	NACEX Spain.
TAQBIN_HK	TAQBIN Hong Kong.
GLOBALTRANZ	GlobalTranz.
HKD	Qingdao HKD International Logistics.
BJSHOMEDELIVERY	BJS Distribution courier.
OMNIVA	Omniva.
SUTTON	Sutton Transport.
PANTHER_REFERENCE	Panther Reference.
SFCSERVICE	SFC Service.
LTL	LTL COURIER.
PARKNPARCEL	Park N Parcel.
SPRING_GDS	Spring GDS.
ECEXPRESS	ECexpress.
INTERPARCEL_AU	Interparcel Australia.
AGILITY	Agility.
XL_EXPRESS	XL Express.
ADERONLINE	Ader couriers.
DIRECTCOURIERS	Direct Couriers.
PLANZER	Planzer Group.
SENDING	Sending Transporte Urgente y Comunicacion.
NINJAVAN_WB	Ninjavan Webhook.
NATIONWIDE_MY	Nationwide Express Courier Services Bhd (www.nationwide.com.my).
SENDIT	Sendit.
GB_ARROW	Arrow XL.
IND_GOJAVAS	GoJavas.
KPOST	Korea Post.
DHL_FREIGHT	DHL Freight.
BLUECARE	Bluecare Express Ltd.
JINDOUYUN	jindouyun courier.
TRACKON	Trackon Couriers Pvt. Ltd.
GB_TUFFNELLS	Tuffnells Parcels Express.
TRUMPCARD	TRUMPCARD LLC.
ETOTAL	eTotal Solution Limited.
SFPLUS_WEBHOOK	Zeek courier.
SEKOLOGISTICS	SEKO Logistics.
HERMES_2MANN_HANDLING	Hermes Einrichtungs Service GmbH & Co. KG.
DPD_LOCAL_REF	DPD Local reference.
UDS	United Delivery Service.
ZA_SPECIALISED_FREIGHT	Specialised Freight.
THA_KERRY	Kerry Express Thailand.
PRT_INT_SEUR	SEUR International.
BRA_CORREIOS	Correios Brazil.
NZ_NZ_POST	New Zealand Post.
CN_EQUICK	Equick China.
MYS_EMS	Malaysia Post EMS / Pos Laju.
GB_NORSK	Norsk Global.
ESP_MRW	MRW spain.
ESP_PACKLINK	Packlink.
KANGAROO_MY	Kangaroo Worldwide Express.
RPX	RPX Online.
XDP_UK_REFERENCE	XDP Express Reference.
NINJAVAN_MY	ninja van (www.ninjavan.co).
ADICIONAL	Adicional Logistics.
ROADBULL	Red Carpet Logistics.
YAKIT	Yakit courier.
MAILAMERICAS	MailAmericas.
MIKROPAKKET	Mikropakket.
DYNALOGIC	Dynamic Logistics.
DHL_ES	DHL Spain(www.dhl.com).
DHL_PARCEL_NL	DHL Parcel NL.
DHL_GLOBAL_MAIL_ASIA	DHL Global Mail Asia (www.dhl.com).
DAWN_WING	Dawn Wing.
GENIKI_GR	Geniki Taxydromiki.
HERMESWORLD_UK	hermesworld_uk.
ALPHAFAST	Alphafast (www.alphafast.com).
BUYLOGIC	buylogic.
EKART	Ekart logistics (ekartlogistics.com).
MEX_SENDA	mexico senda express.
SFC_LOGISTICS	SFC.
POST_SERBIA	Posta Serbia.
IND_DELHIVERY	Delhivery India.
DE_DPD_DELISTRACK	DPD Germany.
RPD2MAN	RPD2man Deliveries.
CN_SF_EXPRESS	SF Express (www.sf-express.com).
YANWEN	Yanwen Logistics.
MYS_SKYNET	Skynet Malaysia.
CORREOS_DE_MEXICO	correos mexico.
CBL_LOGISTICA	CBL Logistica.
MEX_ESTAFETA	Estafeta (www.estafeta.com).
AU_AUSTRIAN_POST	Austrian Post (Registered).
RINCOS	Rincos.
NLD_DHL	DHL Netherland.
RUSSIAN_POST	Russian post.
COURIERS_PLEASE	CouriersPlease (couriersplease.com.au).
POSTNORD_LOGISTICS	PostNord Logistics.
FEDEX	Fedex.
DPE_EXPRESS	DPE Express.
DPD	DPD.
ADSONE	ADSone.
IDN_JNE	JNE Express (Jalur Nugraha Ekakurir).
THECOURIERGUY	The Courier Guy.
CNEXPS	CNE Express.
PRT_CHRONOPOST	Chronopost Portugal.
LANDMARK_GLOBAL	Landmark Global.
IT_DHL_ECOMMERCE	DHL International.
ESP_NACEX	NACEX Spain.
PRT_CTT	CTT Portugal.
BE_KIALA	Kiala.
ASENDIA_UK	Asendia UK.
GLOBAL_TNT	TNT global.
POSTUR_IS	Iceland Post.
EPARCEL_KR	eParcel Korea.
INPOST_PACZKOMATY	InPost Paczkomaty.
IT_POSTE_ITALIA	Poste italiane (www.poste.it).
BE_BPOST	Bpost (www.bpost.be).
PL_POCZTA_POLSKA	Poczta Polska (www.poczta-polska.pl).
MYS_MYS_POST	Malaysia Post.
SG_SG_POST	Singapore Post.
THA_THAILAND_POST	Thailand Post (www.thailandpost.co.th).
LEXSHIP	LexShip.
FASTWAY_NZ	Fastway New Zealand.
DHL_AU	DHL Supply Chain Australia.
COSTMETICSNOW	Cosmetics Now.
PFLOGISTICS	PFL.
LOOMIS_EXPRESS	Loomis Express.
GLS_ITALY	GLS Italy.
LINE	Line Clear Express & Logistics Sdn Bhd.
GEL_EXPRESS	Gel Express Logistik.
HUODULL	Huodull.
NINJAVAN_SG	Ninja van Singapore.
JANIO	Janio Asia.
AO_COURIER	AO Logistics.
BRT_IT_SENDER_REF	BRT Bartolini(Sender Reference).
SAILPOST	SAILPOST.
LALAMOVE	Lalamove.
NEWZEALAND_COURIERS	NEW ZEALAND COURIERS.
ETOMARS	Etomars.
VIRTRANSPORT	VIR Transport.
WIZMO	Wizmo.
PALLETWAYS	Palletways.
I_DIKA	i-dika.
CFL_LOGISTICS	CFL Logistics.
GEMWORLDWIDE	GEM Worldwide.
GLOBAL_EXPRESS	Tai Wan Global Business.
LOGISTYX_TRANSGROUP	Transgroup courier.
WESTBANK_COURIER	West Bank Courier.
ARCO_SPEDIZIONI	Arco Spedizioni SP.
YDH_EXPRESS	YDH express.
PARCELINKLOGISTICS	Parcelink Logistics.
CNDEXPRESS	CND Express.
NOX_NIGHT_TIME_EXPRESS	NOX NightTimeExpress.
AERONET	Aeronet couriers.
LTIANEXP	LTIAN EXP.
INTEGRA2_FTP	Integra2.
PARCELONE	PARCEL ONE.
NOX_NACHTEXPRESS	Innight Express Germany GmbH (nox NachtExpress).
CN_CHINA_POST_EMS	China Post.
CHUKOU1	Chukou1.
GLS_SLOV	GLS General Logistics Systems Slovakia s.r.o..
ORANGE_DS	OrangeDS (Orange Distribution Solutions Inc).
JOOM_LOGIS	Joom Logistics.
AUS_STARTRACK	StarTrack (startrack.com.au).
DHL	dhl Global.
GB_APC	APC postal logistics germany.
BONDSCOURIERS	Bonds Courier Service (bondscouriers.com.au).
JPN_JAPAN_POST	Japan Post.
USPS	United States Postal Service.
WINIT	WinIt.
ARG_OCA	OCA Argentina.
TW_TAIWAN_POST	Taiwan Post.
DMM_NETWORK	DMM Network.
TNT	TNT Express.
BH_POSTA	BH Posta (www.posta.ba).
SWE_POSTNORD	Postnord sweden.
CA_CANADA_POST	Canada Post.
WISELOADS	Wiseloads.
ASENDIA_HK	Asendia HonKong.
NLD_GLS	GLS Netherland.
MEX_REDPACK	Redpack.
JET_SHIP	Jet-Ship Worldwide.
DE_DHL_EXPRESS	DHL Express.
NINJAVAN_THAI	Ninja van Thai.
RABEN_GROUP	Raben Group.
ESP_ASM	ASM(GLS Spain).
HRV_HRVATSKA	Hrvatska posta.
GLOBAL_ESTES	Estes Express Lines.
LTU_LIETUVOS	Lietuvos pastas.
BEL_DHL	DHL Benelux.
AU_AU_POST	Australia Post.
SPEEDEXCOURIER	SPEEDEX couriers.
FR_COLIS	Colissimo.
ARAMEX	Aramex.
DPEX	DPEX (www.dpex.com).
MYS_AIRPAK	Airpak Express.
CUCKOOEXPRESS	Cuckoo Express.
DPD_POLAND	DPD Poland.
NLD_POSTNL	PostNL International.
NIM_EXPRESS	Nim Express.
QUANTIUM	Quantium.
SENDLE	Sendle.
ESP_REDUR	Redur Spain.
MATKAHUOLTO	Matkahuolto.
CPACKET	Cpacket couriers.
POSTI	Posti courier.
HUNTER_EXPRESS	Hunter Express.
CHOIR_EXP	Choir Express Indonesia.
LEGION_EXPRESS	Legion Express.
AUSTRIAN_POST_EXPRESS	austrian post.
GRUPO	Grupo ampm.
POSTA_RO	Post Roman (www.posta-romana.ro).
INTERPARCEL_UK	Interparcel UK.
GLOBAL_ABF	ABF Freight.
POSTEN_NORGE	Posten Norge (www.posten.no).
XPERT_DELIVERY	Xpert Delivery.
DHL_REFR	DHl (Reference number).
DHL_HK	DHL HonKong.
SKYNET_UAE	SKYNET UAE.
GOJEK	Gojek.
YODEL_INTNL	Yodel International.
JANCO	Janco Ecommerce.
YTO	YTO Express.
WISE_EXPRESS	Wise Express.
JTEXPRESS_VN	J&T Express Vietnam.
FEDEX_INTL_MLSERV	FedEx International MailService.
VAMOX	VAMOX.
AMS_GRP	AMS Group.
DHL_JP	DHL Japan.
HRPARCEL	HR Parcel.
GESWL	GESWL Express.
BLUESTAR	Blue Star.
CDEK_TR	CDEK TR.
DESCARTES	Innovel courier.
DELTEC_UK	Deltec Courier.
DTDC_EXPRESS	DTDC express.
TOURLINE	tourline.
BH_WORLDWIDE	B&H Worldwide.
OCS	OCS ANA Group.
YINGNUO_LOGISTICS	yingnuo logistics.
UPS	United Parcel Service.
TOLL	Toll IPEC.
PRT_SEUR	SEUR portugal.
DTDC_AU	DTDC Australia.
THA_DYNAMIC_LOGISTICS	Dynamic Logistics.
UBI_LOGISTICS	UBI Smart Parcel.
FEDEX_CROSSBORDER	FedEx Cross Border.
A1POST	A1Post.
TAZMANIAN_FREIGHT	Tazmanian Freight Systems.
CJ_INT_MY	CJ International malaysia.
SAIA_FREIGHT	Saia LTL Freight.
SG_QXPRESS	Qxpress.
NHANS_SOLUTIONS	Nhans Solutions.
DPD_FR	DPD France.
COORDINADORA	Coordinadora.
ANDREANI	Grupo logistico Andreani.
DOORA	Doora Logistics.
INTERPARCEL_NZ	Interparcel New Zealand.
PHL_JAMEXPRESS	Jam Express Philippines.
BEL_BELGIUM_POST	bel_belgium_post.
US_APC	us_apc.
IDN_POS	idn_pos.
FR_MONDIAL	fr_mondial.
DE_DHL	DE DHL.
HK_RPX	hk_rpx.
DHL_PIECEID	dhl_pieceid.
VNPOST_EMS	vnpost_ems.
RRDONNELLEY	rrdonnelley.
DPD_DE	dpd_de.
DELCART_IN	delcart_in.
IMEXGLOBALSOLUTIONS	imexglobalsolutions.
ACOMMERCE	ACOMMERCE.
EURODIS	eurodis.
CANPAR	CANPAR.
GLS	GLS.
IND_ECOM	Ecom Express.
ESP_ENVIALIA	Envialia.
DHL_UK	dhl UK.
SMSA_EXPRESS	SMSA Express.
TNT_FR	TNT France.
DEX_I	DEX-I courier.
BUDBEE_WEBHOOK	Budbee courier.
COPA_COURIER	Copa Airlines Courier.
VNM_VIETNAM_POST	Vietnam Post.
DPD_HK	DPD HongKong.
TOLL_NZ	Toll New Zealand.
ECHO	Echo courier.
FEDEX_FR	FedEx® Freight.
BORDEREXPRESS	Border Express.
MAILPLUS_JPN	MailPlus (Japan).
TNT_UK_REFR	TNT UK Reference.
KEC	KEC courier.
DPD_RO	DPD Romania.
TNT_JP	TNT_JP.
TH_CJ	TH_CJ.
EC_CN	EC_CN.
FASTWAY_UK	FASTWAY_UK.
FASTWAY_US	FASTWAY_US.
GLS_DE	GLS_DE.
GLS_ES	GLS_ES.
GLS_FR	GLS_FR.
MONDIAL_BE	MONDIAL_BE.
SGT_IT	SGT_IT.
TNT_CN	TNT_CN.
TNT_DE	TNT_DE.
TNT_ES	TNT_ES.
TNT_PL	TNT_PL.
PARCELFORCE	PARCELFORCE.
SWISS_POST	SWISS POST.
TOLL_IPEC	TOLL IPEC.
AIR_21	AIR 21.
AIRSPEED	AIRSPEED.
BERT	BERT.
BLUEDART	BLUEDART.
COLLECTPLUS	COLLECTPLUS.
COURIERPLUS	COURIERPLUS.
COURIER_POST	COURIER POST.
DHL_GLOBAL_MAIL	dhl_global_mail.
DPD_UK	dpd_uk.
DELTEC_DE	DELTEC DE.
DEUTSCHE_DE	deutsche_de.
DOTZOT	DOTZOT.
ELTA_GR	elta_gr.
EMS_CN	ems_cn.
ECARGO	ECARGO.
ENSENDA	ENSENDA.
FERCAM_IT	fercam_it.
FASTWAY_ZA	fastway_za.
FASTWAY_AU	fastway_au.
FIRST_LOGISITCS	first_logisitcs.
GEODIS	GEODIS.
GLOBEGISTICS	GLOBEGISTICS.
GREYHOUND	GREYHOUND.
JETSHIP_MY	jetship_my.
LION_PARCEL	LION PARCEL.
AEROFLASH	AEROFLASH.
ONTRAC	ONTRAC.
SAGAWA	SAGAWA.
SIODEMKA	SIODEMKA.
STARTRACK	startrack.
TNT_AU	tnt_au.
TNT_IT	tnt_it.
TRANSMISSION	TRANSMISSION.
YAMATO	YAMATO.
DHL_IT	dhl_it.
DHL_AT	dhl_at.
LOGISTICSWORLDWIDE_KR	LOGISTICSWORLDWIDE KR.
GLS_SPAIN	gls_spain.
AMAZON_UK_API	amazon_uk_api.
DPD_FR_REFERENCE	dpd_fr_reference.
DHLPARCEL_UK	dhlparcel_uk.
MEGASAVE	megasave.
QUALITYPOST	qualitypost.
IDS_LOGISTICS	ids_logistics.
JOYINGBOX	joyingbox.
PANTHER_ORDER_NUMBER	panther_order_number.
WATKINS_SHEPARD	watkins_shepard.
FASTTRACK	fasttrack.
UP_EXPRESS	up_express.
ELOGISTICA	elogistica.
ECOURIER	ecourier.
CJ_PHILIPPINES	cj_philippines.
SPEEDEX	speedex.
ORANGECONNEX	orangeconnex.
TECOR	tecor.
SAEE	saee.
GLS_ITALY_FTP	gls_italy_ftp.
DELIVERE	delivere.
YYCOM	yycom.
ADICIONAL_PT	Adicional Logistics.
DKSH	DKSH.
NIPPON_EXPRESS_FTP	Nippon Express.
GOLS	GO Logistics & Storage.
FUJEXP	FUJIE EXPRESS.
QTRACK	QTrack.
OMLOGISTICS_API	OM LOGISTICS LTD.
GDPHARM	GDPharm Logistics.
MISUMI_CN	MISUMI Group Inc..
AIR_CANADA	Rivo.
CITY56_WEBHOOK	City Express.
SAGAWA_API	Sagawa.
KEDAEX	KedaEX.
PGEON_API	Pgeon.
WEWORLDEXPRESS	We World Express.
JT_LOGISTICS	J&T International logistics.
TRUSK	Trusk France.
VIAXPRESS	ViaXpress.
DHL_SUPPLYCHAIN_ID	DHL Supply Chain Indonesia.
ZUELLIGPHARMA_SFTP	Zuellig Pharma Korea.
MEEST	Meest.
TOLL_PRIORITY	Toll Priority.
MOTHERSHIP_API	Mothership.
CAPITAL	Capital Transport.
EUROPAKET_API	Europacket+.
HFD	HFD.
TOURLINE_REFERENCE	Tourline Express.
GIO_ECOURIER	GIO Express Inc.
CN_LOGISTICS	CN Logistics.
PANDION	Pandion.
BPOST_API	Bpost API.
PASSPORTSHIPPING	Passport Shipping.
PAKAJO	Pakajo World.
DACHSER	DACHSER.
YUSEN_SFTP	Yusen Logistics.
SHYPLITE	Shypmax.
XYY	Xingyunyi Logistics.
MWD	Metropolitan Warehouse & Delivery.
FAXECARGO	Faxe Cargo.
MAZET	Groupe Mazet.
FIRST_LOGISTICS_API	First Logistics.
SPRINT_PACK	SPRINT PACK.
HERMES_DE_FTP	Hermes Germany.
CONCISE	Concise.
KERRY_EXPRESS_TW_API	Kerry Express TaiWan.
EWE	EWE Global Express.
FASTDESPATCH	Fast Despatch Logistics Limited.
ABCUSTOM_SFTP	AB Custom Group.
CHAZKI	Chazki.
SHIPPIE	Shippie.
GEODIS_API	GEODIS - Distribution & Express.
NAQEL_EXPRESS	Naqel Express.
PAPA_WEBHOOK	Papa.
FORWARDAIR	Forward Air.
DIALOGO_LOGISTICA_API	Dialogo Logistica.
LALAMOVE_API	Lalamove.
TOMYDOOR	Tomydoor.
KRONOS_WEBHOOK	Kronos Express.
JTCARGO	J&T CARGO.
T_CAT	T-cat.
CONCISE_WEBHOOK	Concise.
TELEPORT_WEBHOOK	Teleport.
CUSTOMCO_API	The Custom Companies.
SPX_TH	Shopee Xpress.
BOLLORE_LOGISTICS	Bollore Logistics.
CLICKLINK_SFTP	ClickLink.
M3LOGISTICS	M3 Logistics.
VNPOST_API	Vietnam Post.
AXLEHIRE_FTP	Axlehire.
SHADOWFAX	Shadowfax.
MYHERMES_UK_API	EVRi.
DAIICHI	Daiichi Freight System Inc.
MENSAJEROSURBANOS_API	Mensajeros Urbanos.
POLARSPEED	PolarSpeed Inc.
IDEXPRESS_ID	iDexpress Indonesia.
PAYO	Payo.
WHISTL_SFTP	Whistl.
INTEX_DE	INTEX Paketdienst GmbH.
TRANS2U	Trans2u.
PRODUCTCAREGROUP_SFTP	Product Care Services Limited.
BIGSMART	Big Smart.
EXPEDITORS_API_REF	Expeditors API Reference.
AITWORLDWIDE_API	AIT.
WORLDCOURIER	World Courier.
QUIQUP	Quiqup.
AGEDISS_SFTP	Agediss.
ANDREANI_API	Andreani.
CRLEXPRESS	CRL Express.
SMARTCAT	SMARTCAT.
CROSSFLIGHT	Crossflight Limited.
PROCARRIER	Pro Carrier.
DHL_REFERENCE_API	DHL (Reference number).
SEINO_API	Seino.
WSPEXPRESS	WSP Express.
KRONOS	Kronos Express.
TOTAL_EXPRESS_API	Total Express.
PARCLL	PARCLL.
XPEDIGO	Xpedigo.
STAR_TRACK_WEBHOOK	StarTrack.
GPOST	Georgian Post.
UCS	UCS.
DMFGROUP	DMF.
COORDINADORA_API	Coordinadora.
MARKEN	Marken.
NTL	NTL logistics.
REDJEPAKKETJE	Red je Pakketje.
ALLIED_EXPRESS_FTP	Allied Express (FTP).
MONDIALRELAY_ES	Mondial Relay Spain(Punto Pack).
NAEKO_FTP	Naeko Logistics.
MHI	Mhi.
SHIPPIFY	Shippify, Inc.
MALCA_AMIT_API	Malca Amit.
JTEXPRESS_SG_API	J&T Express Singapore.
DACHSER_WEB	DACHSER.
FLIGHTLG	Flight Logistics Group.
CAGO	Cago.
COM1EXPRESS	ComOne Express.
TONAMI_FTP	Tonami.
PACKFLEET	PACKFLEET.
PUROLATOR_INTERNATIONAL	Purolator International.
WINESHIPPING_WEBHOOK	Wineshipping.
DHL_ES_SFTP	DHL Spain Domestic.
PCHOME_API	網家速配股份有限公司.
CESKAPOSTA_API	Czech Post.
GORUSH	Go Rush.
HOMERUNNER	HomeRunner.
AMAZON_ORDER	Amazon order.
EFWNOW_API	Estes Forwarding Worldwide.
CBL_LOGISTICA_API	CBL Logistica (API).
NIMBUSPOST	NimbusPost.
LOGWIN_LOGISTICS	Logwin Logistics.
NOWLOG_API	Sequoialog.
DPD_NL	DPD Netherlands.
GODEPENDABLE	Dependable Supply Chain Services.
ESDEX	Top Ideal Express.
LOGISYSTEMS_SFTP	Kiitäjät.
EXPEDITORS	Expeditors.
SNTGLOBAL_API	Snt Global Etrax.
SHIPX	ShipX.
QINTL_API	Quickstat Courier LLC.
PACKS	Packs.
POSTNL_INTERNATIONAL	PostNL International.
AMAZON_EMAIL_PUSH	Amazon.
DHL_API	DHL.
SPX	Shopee Express.
AXLEHIRE	AxleHire.
ICSCOURIER	ICS COURIER.
DIALOGO_LOGISTICA	Dialogo Logistica.
SHUNBANG_EXPRESS	ShunBang Express.
TCS_API	TCS.
SF_EXPRESS_CN	SF Express China.
PACKETA	Packeta.
SIC_TELIWAY	Teliway SIC Express.
MONDIALRELAY_FR	Mondial Relay France.
INTIME_FTP	InTime.
JD_EXPRESS	京东物流.
FASTBOX	Fastbox.
PATHEON	Patheon Logistics.
INDIA_POST	India Post Domestic.
TIPSA_REF	Tipsa Reference.
ECOFREIGHT	Eco Freight.
VOX	VOX SOLUCION EMPRESARIAL SRL.
DIRECTFREIGHT_AU_REF	Direct Freight Express.
BESTTRANSPORT_SFTP	Best Transport.
AUSTRALIA_POST_API	Australia Post.
FRAGILEPAK_SFTP	FragilePAK.
FLIPXP	FlipXpress.
VALUE_WEBHOOK	Value Logistics.
DAESHIN	Daeshin.
SHERPA	Sherpa.
MWD_API	Metropolitan Warehouse & Delivery.
SMARTKARGO	SmartKargo.
DNJ_EXPRESS	DNJ Express.
GOPEOPLE	Go People.
MYSENDLE_API	mySendle.
ARAMEX_API	Aramex.
PIDGE	Pidge.
THAIPARCELS	TP Logistic.
PANTHER_REFERENCE_API	Panther Reference.
POSTAPLUS	Posta Plus.
BUFFALO	BUFFALO.
U_ENVIOS	U-ENVIOS.
ELITE_CO	Elite Express.
ROCHE_INTERNAL_SFTP	Roche Internal Courier.
DBSCHENKER_ICELAND	DB Schenker Iceland.
TNT_FR_REFERENCE	TNT France Reference.
NEWGISTICSAPI	Newgistics API.
GLOVO	Glovo.
GWLOGIS_API	G.I.G.
SPREETAIL_API	Spreetail.
MOOVA	Moova.
PLYCONGROUP	Plycon Transportation Group.
USPS_WEBHOOK	USPS Informed Visibility - Webhook.
REIMAGINEDELIVERY	maergo.
EDF_FTP	Eurodifarm.
DAO365	DAO365.
BIOCAIR_FTP	BioCair.
RANSA_WEBHOOK	Ransa.
SHIPXPRES	SHIPXPRESS.
COURANT_PLUS_API	Courant Plus.
SHIPA	SHIPA.
HOMELOGISTICS	Home Logistics.
DX	DX.
POSTE_ITALIANE_PACCOCELERE	Poste Italiane Paccocelere.
TOLL_WEBHOOK	Toll Group.
LCTBR_API	LCT do Brasil.
DX_FREIGHT	DX Freight.
DHL_SFTP	DHL Express.
SHIPROCKET	Shiprocket X.
UBER_WEBHOOK	Uber.
STATOVERNIGHT	Stat Overnight.
BURD	Burd Delivery.
FASTSHIP	Fastship Express.
IBVENTURE_WEBHOOK	IB Venture.
GATI_KWE_API	Gati-KWE.
CRYOPDP_FTP	CryoPDP.
HUBBED	HUBBED.
TIPSA_API	Tipsa API.
ARASKARGO	Aras Cargo.
THIJS_NL	Thijs Logistiek.
ATSHEALTHCARE_REFERENCE	ATS Healthcare.
99MINUTOS	99minutos.
HELLENIC_POST	Hellenic (Greece) Post.
HSM_GLOBAL	HSM Global.
MNX	MNX.
NMTRANSFER	N&M Transfer Co., Inc..
LOGYSTO	Logysto.
INDIA_POST_INT	India Post International.
AMAZON_FBA_SWISHIP_IN	Swiship IN.
SRT_TRANSPORT	SRT Transport.
BOMI	Bomi Group.
DELIVERR_SFTP	Deliverr.
HSDEXPRESS	HSDEXPRESS.
SIMPLETIRE_WEBHOOK	SimpleTire.
HUNTER_EXPRESS_SFTP	Hunter Express.
UPS_API	UPS.
WOOYOUNG_LOGISTICS_SFTP	WOO YOUNG LOGISTICS CO.,LTD..
PHSE_API	PHSE.
WISH_EMAIL_PUSH	Wish.
NORTHLINE	Northline.
MEDAFRICA	Med Africa Logistics.
DPD_AT_SFTP	DPD Austria.
ANTERAJA	Anteraja.
DHL_GLOBAL_FORWARDING_API	DHL Global Forwarding API.
LBCEXPRESS_API	LBC EXPRESS INC..
SIMSGLOBAL	Sims Global.
CDLDELIVERS	CDL Last Mile.
TYP	TYP.
TESTING_COURIER_WEBHOOK	Testing Courier.
PANDAGO_API	Pandago.
ROYAL_MAIL_FTP	Royal Mail.
THUNDEREXPRESS	Thunder Express Australia.
SECRETLAB_WEBHOOK	Secretlab.
SETEL	Setel Express.
JD_WORLDWIDE	JD Worldwide.
DPD_RU_API	DPD Russia.
ARGENTS_WEBHOOK	Argents Express Group.
POSTONE	Post ONE.
TUSKLOGISTICS	Tusk Logistics.
RHENUS_UK_API	Rhenus Logistics UK.
TAQBIN_SG_API	Yamato Singapore.
INNTRALOG_SFTP	Inntralog GmbH.
DAYROSS	Day & Ross.
CORREOSEXPRESS_API	Correos Express (API).
INTERNATIONAL_SEUR_API	International Seur API.
YODEL_API	Yodel API.
HEROEXPRESS	Hero Express.
DHL_SUPPLYCHAIN_IN	DHL supply chain India.
URGENT_CARGUS	Urgent Cargus.
FRONTDOORCORP	FRONTdoor Collective.
JTEXPRESS_PH	J&T Express Philippines.
PARCELSTARS_WEBHOOK	Parcelstars.
DPD_SK_SFTP	DPD Slovakia.
MOVIANTO	Movianto.
OZEPARTS_SHIPPING	Ozeparts Shipping.
KARGOMKOLAY	KargomKolay (CargoMini).
TRUNKRS	Trunkrs.
OMNIRPS_WEBHOOK	Omni Returns.
CHILEXPRESS	Chile Express.
TESTING_COURIER	Testing Courier.
JNE_API	JNE (API).
BJSHOMEDELIVERY_FTP	BJS Distribution, Storage & Couriers - FTP.
DEXPRESS_WEBHOOK	D Express.
USPS_API	USPS API.
TRANSVIRTUAL	TransVirtual.
SOLISTICA_API	solistica.
CHIENVENTURE_WEBHOOK	Chienventure.
DPD_UK_SFTP	DPD UK.
INPOST_UK	InPost.
JAVIT	Javit.
ZTO_DOMESTIC	ZTO Express China.
DHL_GT_API	DHL Global Forwarding Guatemala.
CEVA_TRACKING	CEVA Package.
KOMON_EXPRESS	Komon Express.
EASTWESTCOURIER_FTP	East West Courier Pte Ltd.
DANNIAO	Danniao.
SPECTRAN	Spectran.
DELIVER_IT	Deliver-iT.
RELAISCOLIS	Relais Colis.
GLS_SPAIN_API	GLS Spain.
POSTPLUS	PostPlus.
AIRTERRA	Airterra.
GIO_ECOURIER_API	GIO Express Ecourier.
DPD_CH_SFTP	DPD Switzerland.
FEDEX_API	FedEx®.
INTERSMARTTRANS	INTERSMARTTRANS & SOLUTIONS SL.
HERMES_UK_SFTP	Hermes UK.
EXELOT_FTP	Exelot Ltd..
DHL_PA_API	DHL GLOBAL FORWARDING PANAMÁ.
VIRTRANSPORT_SFTP	Vir Transport.
WORLDNET	Worldnet Logistics.
INSTABOX_WEBHOOK	Instabox.
KNG	Keuhne + Nagel Global.
FLASHEXPRESS_WEBHOOK	Flash Express.
MAGYAR_POSTA_API	Magyar Posta.
WESHIP_API	WeShip.
OHI_WEBHOOK	Ohi.
MUDITA	MUDITA.
BLUEDART_API	Bluedart.
T_CAT_API	T-cat.
ADS	ADS Express.
HERMES_IT	HR Parcel.
FITZMARK_API	FitzMark.
POSTI_API	Posti API.
SMSA_EXPRESS_WEBHOOK	SMSA Express.
TAMERGROUP_WEBHOOK	Tamer Logistics.
LIVRAPIDE	Livrapide.
NIPPON_EXPRESS	Nippon Express.
BETTERTRUCKS	Better Trucks.
FAN	FAN COURIER EXPRESS.
PB_USPSFLATS_FTP	USPS Flats (Pitney Bowes).
PARCELRIGHT	Parcel Right.
ITHINKLOGISTICS	iThink Logistics.
KERRY_EXPRESS_TH_WEBHOOK	Kerry Logistics.
ECOUTIER	eCoutier.
SHOWL	SENHONG INTERNATIONAL LOGISTICS.
BRT_IT_API	BRT Bartolini API.
RIXONHK_API	Rixon Logistics.
DBSCHENKER_API	DB Schenker.
ILYANGLOGIS	Ilyang logistics.
MAIL_BOX_ETC	Mail Boxes Etc..
WESHIP	WeShip.
DHL_GLOBAL_MAIL_API	DHL eCommerce Solutions.
ACTIVOS24_API	Activos24.
ATSHEALTHCARE	ATS Healthcare.
LUWJISTIK	Luwjistik.
GW_WORLD	Gebrüder Weiss.
FAIRSENDEN_API	fairsenden.
SERVIP_WEBHOOK	SerVIP.
SWISHIP	Swiship.
TANET	Transport Ambientales.
HOTSIN_CARGO	SHENZHEN HOTSIN CARGO INT'L FORWARDING CO.,LTD.
DIREX	Direx.
HUANTONG	HuanTong.
IMILE_API	iMile.
AUEXPRESS	Au Express.
NYTLOGISTICS	NYT SUPPLY CHAIN LOGISTICS Co.,LTD.
DSV_REFERENCE	DSV Futurewave.
NOVOFARMA_WEBHOOK	Novofarma.
AITWORLDWIDE_SFTP	AIT.
SHOPOLIVE	Olive.
FNF_ZA	Fast & Furious.
DHL_ECOMMERCE_GC	DHL eCommerce Greater China.
FETCHR	Fetchr.
STARLINKS_API	Starlinks Global.
YYEXPRESS	YYEXPRESS.
SERVIENTREGA	Servientrega.
HANJIN	HanJin.
SPANISH_SEUR_FTP	Spanish Seur.
DX_B2B_CONNUM	DX (B2B).
HELTHJEM_API	Helthjem.
INEXPOST	Inexpost.
A2B_BA	A2B Express Logistics.
RHENUS_GROUP	Rhenus Logistics.
SBERLOGISTICS_RU	Sber Logistics.
MALCA_AMIT	Malca-Amit.
PPL	Professional Parcel Logistics.
OSM_WORLDWIDE_SFTP	OSM Worldwide.
ACILOGISTIX	ACI Logistix.
OPTIMACOURIER	Optima Courier.
NOVA_POSHTA_API	Nova Poshta API.
LOGGI	Loggi.
YIFAN	YiFan Express.
MYDYNALOGIC	My DynaLogic.
MORNINGLOBAL	Morning Global.
CONCISE_API	Concise.
FXTRAN	Falcon Express.
DELIVERYOURPARCEL_ZA	Deliver Your Parcel.
UPARCEL	uParcel.
MOBI_BR	Mobi Logistica.
LOGINEXT_WEBHOOK	T&W Delivery.
EMS	EMS.
SPEEDY	Speedy.
ZOOM_RED	Zoom.
NAVLUNGO	Navlungo.
CASTLEPARCELS	Castle Parcels.
WEEE	Weee.
PACKALY	Packaly.
YUNHUIPOST	Yunhuipost.
YOUPARCEL	YouParcel.
LEMAN	Leman.
MOOVIN	Moovin.
URB_IT	Urb-it.
MULTIENTREGAPANAMA	Multientrega.
JUSDASR	Jusdasr.
DISCOUNTPOST	Discount Post.
RHENUS_UK	Rhenus Logistics UK.
SWISHIP_JP	Swiship JP.
GLS_US	GLS USA.
SMTL	Southwestern Motor Transport. Inc.
EMEGA	Discount Post Emega.
EXPRESSONE_SV	EXPRESSONE Slovenia.
HEPSIJET	hepsiJET.
WELIVERY	Welivery.
BRINGER	Bringer Parcel Services.
EASYROUTES	EasyRoutes.
MRW	MRW.
RPM	RPM.
DPD_PRT	DPD Portugal.
GLS_ROMANIA	GLS Romania.
LMPARCEL	LM Parcel.
GTAGSM	GTA GSM.
DOMINO	DOMINO.
ESHIPPER	eShipper.
TRANSPAK	Transpak Inc..
XINDUS	Xindus.
AOYUE	Aoyue.
EASYPARCEL	Easyparcel.
EXPRESSONE	EXPRESSONE.
SENDEO_KARGO	Sendeo Kargo.
SPEEDAF	Speedaf Express.
ETOWER	eTower.
GCX	GC Express.
NINJAVAN_VN	Ninjavan Vietnam.
ALLEGRO	Allegro.
JUMPPOINT	Jumppoint.
SHIPGLOBAL_US	ShipGlobal.
KINISI	Kinisi Transport Pty Ltd.
OAKH	Oakh Harbour Freight Lines.
AWEST	American West.
BARSAN	Barsan Global Lojistik.
ENERGOLOGISTIC	Energo Logistic.
MADROOEX	Madrooex.
GOBOLT	GoBolt.
SWISS_UNIVERSAL_EXPRESS	Swiss Universal Express.
IORDIRECT	IOR Direct Solutions.
XMSZM	xmszm.
GLS_HUN	GLS Hungary.
SENDY	Sendy Express.
BRAUNSEXPRESS	Brauns Express.
GRANDSLAMEXPRESS	Grand Slam Express.
XGS	XGS.
OTSCHILE	OTS.
PACK_UP	Pack-Up.
PARCELSTARS	Parcelstars.
TEAMEXPRESSLLC	Team Express Service LLC.
ASYADEXPRESS	Asyad Express.
TDN	TDN.
EARLYBIRD	Early Bird.
CACESA	Cacesa.
PARCELJET	Parceljet.
MNG_KARGO	MNG Kargo.
SUPERPACKLINE	Super Pac Line.
SPEEDX	SpeedX.
VESYL	Vesyl.
SKYKING	Sky King.
DIRMENSAJERIA	DIR.
NETLOGIXGROUP	Netlogix.
ZYOU	ZYEX.
JAWAR	Jawar.
AGSYSTEMS	Associate Global Systems.
GPS	GPS.
PTT_KARGO	PTT Kargo.
MAERGO	Maergo.
ARIHANTCOURIER	AICS.
VTFE	VicTas Freight Express.
YUNANT	Yunant.
URBIFY	Urbify.
PACK_MAN	pack-man.
LIEFERGRUN	LIEFERGRUN.
OBIBOX	Obibox.
PAIKEDA	Paikeda.
SCOTTY	Scotty.
INTELCOM_CA	Intelcom.
SWE	swe.
ASENDIA	Asendia Global.
DPD_AT	DPD Austria.
RELAY	Relay.
ATA	ATA.
SKYEXPRESS_INTERNATIONAL	SkyExpress Internationals.
SURAT_KARGO	Surat Kargo.
SGLINK	SG LINK.
FLEETOPTICSINC	FleetOptics.
SHOPLINE	shopline.
PIGGYSHIP	PIGGYSHIP.
LOGOIX	LogoiX.
KOLAY_GELSIN	Kolay Gelsin.
ASSOCIATED_COURIERS	Associated Couriers.
UPS_CHECKER	ups-checker.
WINESHIPPING	Wineshipping.
SPEDISCI	Spedisci online.
FOURKITES	Fourkites.
ETONAS	Etonas.
FINMILE	Fin Mile.
UNIUNI	Uniuni.
RODONAVES	Rodonaves.
INPOST_IT	Inpost Italy.
TFORCE_FREIGHT	Tforce Freight.
RICHMOM	Rich Mom.
FRANCO	Corriere Franco.
ECPARCEL	Ecparcel.
FEDEX_CHINA	Fedex China.
GOFO_EXPRESS	Gofo Express.
SHIPBOB	Shipbob.
JERSEYPOST_ATLAS	Jersey Post Group.
CORETRAILS	Coretrails.
RHENUS_ITALY	Rhenus Logistics Italy.
JADLOG	Jadlog.
JITSU	Jitsu.
YANWEN_EXPRESS	Yanwen Express.
DASHLINK	Dashlink.
SEINO_SUPER_EXPRESS	Seino Super Express.
FLOSHIP	Floship.
METROSCG	Metro Supply Chain.
SENDPARCEL	Sendparcel.
P2P	P2p.
CN_EXPRESS	Cn Express.
CIRROTRACK	Cirro Track.
LAND_LOGISTICS	Land Logistics.
VEHO	Veho.
MEDLINE	Medline.
VDTRACK	Vdtrack.
SINO_SCM	Sino Scm.
3PE_EXPRESS	3pe Express.
SWIFTX	Swiftx.
SFYDEXPRESS	Sfyd Express.
TOPTRANS	Toptrans.
Copy
"DPD_RU"
charge_pattern
Expected business/pricing model for the billing agreement.

string [ 1 .. 30 ] characters ^[A-Z0-9_]+$
Expected business/pricing model for the billing agreement.

Enum Value	Description
IMMEDIATE	On-demand instant payments – non-recurring, pre-paid, variable amount, variable frequency.
DEFERRED	Pay after use, non-recurring post-paid, variable amount, irregular frequency.
RECURRING_PREPAID	Pay upfront fixed or variable amount on a fixed date before the goods/service is delivered.
RECURRING_POSTPAID	Pay on a fixed date based on usage or consumption after the goods/service is delivered.
THRESHOLD_PREPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, before the goods/service is delivered.
THRESHOLD_POSTPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, after the goods/service is delivered.
SUBSCRIPTION_PREPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due before the good/service is delivered.
SUBSCRIPTION_POSTPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due after the goods/services are delivered.
UNSCHEDULED_PREPAID	Unscheduled card on file plan where the merchant can bill buyer upfront based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
UNSCHEDULED_POSTPAID	Unscheduled card on file plan where the merchant can bill buyer based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
INSTALLMENT_PREPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due before the good/service is delivered.
INSTALLMENT_POSTPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due after the goods/services are delivered.
Copy
"IMMEDIATE"
checkout_payment_intent
The intent to either capture payment immediately or authorize a payment for an order after order creation.

string
The intent to either capture payment immediately or authorize a payment for an order after order creation.

Enum Value	Description
CAPTURE	The merchant intends to capture payment immediately after the customer makes a payment.
AUTHORIZE	The merchant intends to authorize a payment and place funds on hold after the customer makes a payment. Authorized payments are best captured within three days of authorization but are available to capture for up to 29 days. After the three-day honor period, the original authorized payment expires and you must re-authorize the payment. You must make a separate request to capture payments on demand. This intent is not supported when you have more than one purchase_unit within your order.
Copy
"CAPTURE"
cobranded_card
Details about the merchant cobranded card used for order purchase.

labels	
Array of strings [ 1 .. 25 ] items
Array of labels for the cobranded card.

payee	
object
Merchant associated with the purchase.

amount	
object
Amount that was charged to the cobranded card.

CopyExpand allCollapse all
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
confirm_order_request
Payer confirms the intent to pay for the Order using the provided payment source.

application_context	
object
Customizes the payer confirmation experience.

payment_source
required
object
The payment source definition.

CopyExpand allCollapse all
{
"application_context": {
"brand_name": "string",
"return_url": "http://example.com",
"cancel_url": "http://example.com",
"locale": "string",
"stored_payment_source": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
},
"payment_source": {
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"bancontact": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"blik": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"consumer_user_agent": "string",
"consumer_ip": "string"
},
"level_0": {
"auth_code": "string"
},
"one_click": {
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
},
"eps": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"giropay": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"ideal": {
"name": "string",
"country_code": "string",
"bic": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"mybank": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"p24": {
"name": "string",
"email": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"sofort": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"trustly": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
}
country_code
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
string (country_code) = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
"st"
country_code
The two-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
string (country_code) = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
"st"
country_code
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
string (country_code) = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
"st"
currency_code
The three-character ISO-4217 currency code that identifies the currency.

string (currency_code) = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

Copy
"str"
currency_code
The 3-character ISO-4217 currency code that identifies the currency.

string (currency_code) = 3 characters
The 3-character ISO-4217 currency code that identifies the currency.

Copy
"str"
customer
This object represents a merchant’s customer, allowing them to store contact details, and track all payments associated with the same customer.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

name	
object
The full name of the customer as provided to the merchant or on file with the merchant.

CopyExpand allCollapse all
{
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
customer
This object represents a merchant’s customer, allowing them to store contact details, and track all payments associated with the same customer.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
Email address of the customer as provided to the merchant or on file with the merchant. Email Address is required if you are processing the transaction using PayPal Guest Processing which is offered to select partners and merchants.

phone	
object
The phone number of the customer as provided to the merchant or on file with the merchant. The phone.phone_number supports only national_number.

name	
object
The full name of the customer as provided to the merchant or on file with the merchant.

CopyExpand allCollapse all
{
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
date_no_time
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

string (date_no_time) = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

Copy
"stringstri"
date_no_time
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

string (date_no_time) = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

Copy
"stringstri"
date_no_time
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

string (date_no_time) = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

Copy
"stringstri"
date_no_time
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

string (date_no_time) = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

Copy
"stringstri"
date_time
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
string (date_time) [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
Copy
"stringstringstringst"
date_time
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
string (date_time) [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
Copy
"stringstringstringst"
date_time
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
string (date_time) [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time, in Internet date and time format. Seconds are required while fractional seconds are optional.

Note: The regular expression provides guidance but does not reject all invalid dates.
Copy
"stringstringstringst"
date_year_month
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

Copy
"strings"
date_year_month
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

Copy
"strings"
disbursement_mode
The funds that are held on behalf of the merchant.

string [ 1 .. 16 ] characters ^[A-Z_]+$
Default: "INSTANT"
The funds that are held on behalf of the merchant.

Enum Value	Description
INSTANT	The funds are released to the merchant immediately.
DELAYED	The funds are held for a finite number of days. The actual duration depends on the region and type of integration. You can release the funds through a referenced payout. Otherwise, the funds disbursed automatically after the specified duration.
Copy
"INSTANT"
discount_with_breakdown
The discount amount and currency code. For list of supported currencies and decimal precision, see the PayPal REST APIs Currency Codes.

currency_code
required
string = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

value
required
string <= 32 characters
The value, which might be:

An integer for currencies like JPY that are not typically fractional.
A decimal fraction for currencies like TND that are subdivided into thousandths.
For the required number of decimal places for a currency code, see Currency Codes.
Copy
{
"currency_code": "str",
"value": "string"
}
eci_flag
Electronic Commerce Indicator (ECI). The ECI value is part of the 2 data elements that indicate the transaction was processed electronically. This should be passed on the authorization transaction to the Gateway/Processor.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Electronic Commerce Indicator (ECI). The ECI value is part of the 2 data elements that indicate the transaction was processed electronically. This should be passed on the authorization transaction to the Gateway/Processor.

Enum Value	Description
MASTERCARD_NON_3D_SECURE_TRANSACTION	Mastercard non-3-D Secure transaction.
MASTERCARD_ATTEMPTED_AUTHENTICATION_TRANSACTION	Mastercard attempted authentication transaction.
MASTERCARD_FULLY_AUTHENTICATED_TRANSACTION	Mastercard fully authenticated transaction.
FULLY_AUTHENTICATED_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB fully authenticated transaction.
ATTEMPTED_AUTHENTICATION_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB attempted authentication transaction.
NON_3D_SECURE_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB non-3-D Secure transaction.
Copy
"MASTERCARD_NON_3D_SECURE_TRANSACTION"
email
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
string (email) [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
Copy
"string"
email_address
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
string (email_address) [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
Copy
"string"
email_address
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
string (email_address) [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The internationalized email address.

Note: Up to 64 characters are allowed before and 255 characters are allowed after the @ sign. However, the generally accepted maximum length for an email address is 254 characters. The pattern verifies that an unquoted @ sign exists.
Copy
"string"
enrolled
Status of Authentication eligibility.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Status of Authentication eligibility.

Enum Value	Description
Y	Yes. The bank is participating in 3-D Secure protocol and will return the ACSUrl.
N	No. The bank is not participating in 3-D Secure protocol.
U	Unavailable. The DS or ACS is not available for authentication at the time of the request.
B	Bypass. The merchant authentication rule is triggered to bypass authentication.
Copy
"Y"
eps
Information used to pay using eps.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

bic	
string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The bank identification code (BIC).

Copy
{
"name": "string",
"country_code": "string",
"bic": "string"
}
eps_request
Information needed to pay using eps.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
Error
The error details.

name
required
string
The human-readable, unique name of the error.

message
required
string
The message that describes the error.

debug_id
required
string
The PayPal internal ID. Used for correlation purposes.

details	
Array of objects
An array of additional details about the error.

links	
Array of objects
An array of request-related HATEOAS links.

CopyExpand allCollapse all
{
"name": "string",
"message": "string",
"debug_id": "string",
"details": [
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
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
Error Details
The error details. Required for client-side 4XX errors.

field	
string
The field that caused the error. If this field is in the body, set this value to the field's JSON pointer value. Required for client-side errors.

value	
string
The value of the field that caused the error.

location	
string
Default: "body"
The location of the field that caused the error. Value is body, path, or query.

issue
required
string
The unique, fine-grained application-level error code.

links	
Array of objects [ 1 .. 4 ] items
An array of request-related HATEOAS links that are either relevant to the issue by providing additional information or offering potential resolutions.

description	
string
The human-readable description for an issue. The description can change over the lifetime of an API, so clients must not depend on this value.

CopyExpand allCollapse all
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"description": "string"
}
exchange_rate
The exchange rate that determines the amount to convert from one currency to another currency.

value	
string
The target currency amount. Equivalent to one unit of the source currency. Formatted as integer or decimal value with one to 15 digits to the right of the decimal point.

source_currency	
string = 3 characters
The source currency from which to convert an amount.

target_currency	
string = 3 characters
The target currency to which to convert an amount.

Copy
{
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
experience_context_base
Customizes the payer experience during the approval process for the payment.

brand_name	
string [ 1 .. 127 ] characters
The label that overrides the business name in the PayPal account on the PayPal site. The pattern is defined by an external party and supports Unicode.

shipping_preference	
string [ 1 .. 24 ] characters
Default: "GET_FROM_FILE"
The location from which the shipping address is derived.

Enum Value	Description
GET_FROM_FILE	Get the customer-provided shipping address on the PayPal site.
NO_SHIPPING	Redacts the shipping address from the PayPal site. Recommended for digital goods.
SET_PROVIDED_ADDRESS	Merchant sends the shipping address using purchase_units.shipping.address. The customer cannot change this address on the PayPal site.
locale	
string [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The BCP 47-formatted locale of pages that the PayPal payment experience shows. PayPal supports a five-character code. For example, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, or zh-TW.

return_url	
string
The URL where the customer is redirected after the customer approves the payment.

cancel_url	
string
The URL where the customer is redirected after the customer cancels the payment.

Copy
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
experience_context_base
Customizes the payer experience during the approval process for the payment.

locale	
string [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The BCP 47-formatted locale of pages that the PayPal payment experience shows. PayPal supports a five-character code. For example, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, or zh-TW.

return_url	
string
The URL where the customer is redirected after the customer approves the payment.

cancel_url	
string
The URL where the customer is redirected after the customer cancels the payment.

Copy
{
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
giropay
Information needed to pay using giropay.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

bic	
string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The bank identification code (BIC).

Copy
{
"name": "string",
"country_code": "string",
"bic": "string"
}
giropay_request
Information needed to pay using giropay.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
google_pay
Google Pay Wallet payment data.

name	
string [ 3 .. 300 ] characters
Name on the account holder associated with Google Pay.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with Google Pay.

phone_number	
object
The phone number of account holder, in its canonical international E.164 numbering plan format. Supports only the national_number property.

card	
object
The Card from Google Pay Wallet used to fund the payment.

CopyExpand allCollapse all
{
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
}
google_pay_card
The payment card used to fund a Google Pay payment. Can be a credit or debit card.

name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The payment card type.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

CopyExpand allCollapse all
{
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
google_pay_card
The payment card used to fund a Google Pay payment. Can be a credit or debit card.

name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

number
required
string [ 13 .. 19 ] characters
The primary account number (PAN) for the payment card.

last_digits	
string [ 2 .. 4 ] characters
The last digits of the payment card.

expiry
required
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The payment card type.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

CopyExpand allCollapse all
{
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
google_pay_card_response
The payment card to use to fund a Google Pay payment response. Can be a credit or debit card.

name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

last_digits	
string [ 2 .. 4 ] characters
The last digits of the payment card.

type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The payment card type.

Enum Value	Description
CREDIT	A credit card.
DEBIT	A debit card.
PREPAID	A Prepaid card.
STORE	A store card.
UNKNOWN	Card type cannot be determined.
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

authentication_result	
object
Results of Authentication such as 3D Secure.

CopyExpand allCollapse all
{
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
google_pay_decrypted_token_data
Details shared by Google for the merchant to be shared with PayPal. This is required to process the transaction using the Google Pay payment method.

message_id	
string [ 1 .. 250 ] characters
A unique ID that identifies the message in case it needs to be revoked or located at a later time.

message_expiration	
string = 13 characters
Date and time at which the message expires as UTC milliseconds since epoch. Integrators should reject any message that's expired.

payment_method
required
string = 4 characters
The type of the payment credential. Currently, only CARD is supported.

Value	Description
CARD	CARD is the only value that Google Pay accepts.
authentication_method
required
string [ 1 .. 50 ] characters
Authentication Method which is used for the card transaction.

Enum Value	Description
PAN_ONLY	This authentication method is associated with payment cards stored on file with the user's Google Account. Returned payment data includes primary account number (PAN) with the expiration month and the expiration year.
CRYPTOGRAM_3DS	Returned payment data includes a 3-D Secure (3DS) cryptogram generated on the device. -> If authentication_method=CRYPTOGRAM, it is required that 'cryptogram' parameter in the request has a valid 3-D Secure (3DS) cryptogram generated on the device.
cryptogram	
string [ 1 .. 2000 ] characters
Base-64 cryptographic identifier used by card schemes to validate the token verification result. This is a conditionally required field if authentication_method is CRYPTOGRAM_3DS.

eci_indicator	
string [ 1 .. 256 ] characters
Electronic Commerce Indicator may not always be present. It is only returned for tokens on the Visa card network. This value is passed through in the payment authorization request.

card
required
object
Google Pay tokenized credit card used to pay.

CopyExpand allCollapse all
{
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
}
google_pay_experience_context
Customizes the payer experience during the approval process for the payment.

return_url
required
string
The URL where the customer is redirected after the customer approves the payment.

cancel_url
required
string
The URL where the customer is redirected after the customer cancels the payment.

Copy
{
"return_url": "string",
"cancel_url": "string"
}
google_pay_request
Information needed to pay using Google Pay.

name	
string [ 3 .. 300 ] characters
Name on the account holder associated with Google Pay.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with Google Pay.

phone_number	
object
The phone number of account holder, in its canonical international E.164 numbering plan format. Supports only the national_number property.

card	
object
The payment card information.

decrypted_token	
object
The decrypted payload details for the Google Pay token.

assurance_details	
object
Information about what validation has been performed on the returned payment credentials.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
}
iban_last_chars
The last characters of the IBAN used to pay.

string [ 4 .. 34 ] characters [a-zA-Z0-9]{4}
The last characters of the IBAN used to pay.

Copy
"string"
ideal
Information used to pay using iDEAL.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

bic	
string [8 .. 11] caracteres ^[AZa-z0-9]{4}[AZaz]{2}[AZa-z0-9]{2}([... Mostrar padrão
O código de identificação bancária (BIC).

iban_últimos_caracteres
string [ 4 .. 34 ] caracteres [a-zA-Z0-9]{4}
Os últimos caracteres do IBAN usado para efetuar o pagamento.

Cópia
{
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
}
pedido ideal
Informações necessárias para efetuar o pagamento com iDEAL.

nome
obrigatório
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$ (country_code)
O código de país ISO 3166-1 de dois caracteres.

bic
string [8 .. 11] caracteres ^[AZa-z0-9]{4}[AZaz]{2}[AZa-z0-9]{2}([... Mostrar padrão
O código de identificação bancária (BIC).


contexto_de_experiência
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"country_code": "string",
"bic": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
id_do_instrumento
O identificador do instrumento.

string [ 1 .. 256 ] caracteres ^[A-Za-z0-9-_.+=]+$ (instrument_id)
O identificador do instrumento.

Cópia
"string"
Endereço IP
Um endereço de Protocolo de Internet (endereço IP). Este endereço atribui um rótulo numérico a cada dispositivo conectado a uma rede de computadores através do Protocolo de Internet. Suporta endereços IPv4 e IPv6.

string ( Endereço IP ) [ 7 .. 39 ] caracteres ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[... <ppaas_ip_address_v1> Mostrar padrão
Um endereço de Protocolo de Internet (endereço IP). Este endereço atribui um rótulo numérico a cada dispositivo conectado a uma rede de computadores através do Protocolo de Internet. Suporta endereços IPv4 e IPv6.

Cópia
"strings"
item
Detalhes dos itens a serem adquiridos.

nome
obrigatório
string [ 1 .. 127 ] caracteres
O nome ou título do item.

quantidade
obrigatório
string <= 10 caracteres ^[1-9][0-9]{0,9}$
Quantidade de itens. Deve ser um número inteiro.

descrição
string <= 2048 caracteres
Descrição detalhada do item.

SKU
string <= 127 caracteres
A unidade de manutenção de estoque (SKU) do item.

URL
string [ 1 .. 2048 ] caracteres
O URL do item que está sendo comprado. Visível para o comprador e usado nas experiências de compra.

categoria
string [ 1 .. 20 ] caracteres
O tipo de categoria do item.

Valor de enumeração	Descrição
PRODUTOS DIGITAIS	Bens armazenados, entregues e utilizados em formato eletrônico. Este valor não é atualmente suportado para chamadas de API que utilizam a plataforma PayPal for Commerce .
BENS FÍSICOS	Um item tangível que pode ser enviado com comprovante de entrega.
DOAÇÃO	Uma contribuição ou doação sem troca de bens ou serviços, geralmente destinada a uma organização sem fins lucrativos.
URL da imagem
string [ 1 .. 2048 ] caracteres ^(https:)([/|.|\w|\s|-])*\.(?:jpg|gif|png|jpe... Mostrar padrão
O URL da imagem do item. Restrições de tipo e tamanho de arquivo se aplicam. Imagens que violarem essas restrições não serão aceitas.


valor_unitário
obrigatório
objeto
O preço ou taxa por unidade do item. Se você especificar unit_amount, purchase_units[].amount.breakdown.item_totalé obrigatório. Deve ser igual unit_amount * quantitypara todos os itens. unit_amount.valueNão pode ser um número negativo.


imposto
objeto
O imposto por item é calculado para cada unidade. Se taxespecificado, purchase_units[].amount.breakdown.tax_totalé obrigatório. Deve ser igual tax * quantitypara todos os itens. tax.valueNão pode ser um número negativo.


upc
objeto
O Código Universal do Produto (UPC) do item.


plano de faturamento
objeto
Metadados para planos de cobrança recorrente gerenciados pelo comerciante. Válido somente durante a criação do token do método de pagamento salvo ou do contrato de cobrança.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
item
Detalhes dos itens a serem adquiridos.

nome
obrigatório
string [ 1 .. 127 ] caracteres
O nome ou título do item.

quantidade
obrigatório
string <= 10 caracteres ^[1-9][0-9]{0,9}$
Quantidade de itens. Deve ser um número inteiro.

descrição
string <= 2048 caracteres
Descrição detalhada do item.

SKU
string <= 127 caracteres
A unidade de manutenção de estoque (SKU) do item.

URL
string [ 1 .. 2048 ] caracteres
O URL do item que está sendo comprado. Visível para o comprador e usado nas experiências de compra.

URL da imagem
string [ 1 .. 2048 ] caracteres ^(https:)([/|.|\w|\s|-])*\.(?:jpg|gif|png|jpe... Mostrar padrão
O URL da imagem do item. Restrições de tipo e tamanho de arquivo se aplicam. Imagens que violarem essas restrições não serão aceitas.


upc
objeto
O Código Universal do Produto (UPC) do item.


plano de faturamento
objeto
Metadados para planos de cobrança recorrente gerenciados pelo comerciante. Válido somente durante a criação do token do método de pagamento salvo ou do contrato de cobrança.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
solicitação_de_item
Detalhes dos itens a serem adquiridos.

nome
obrigatório
string [ 1 .. 127 ] caracteres
O nome ou título do item.

quantidade
obrigatório
string <= 10 caracteres ^[1-9][0-9]{0,9}$
Quantidade de itens. Deve ser um número inteiro.

descrição
string <= 4000 caracteres
Este campo suporta até 4000 caracteres, mas qualquer conteúdo que ultrapasse 2048 caracteres (incluindo espaços) será truncado. O limite de 2048 caracteres é refletido na representação da resposta deste campo.
.
SKU
string <= 127 caracteres
A unidade de manutenção de estoque (SKU) do item.

URL
string [ 1 .. 2048 ] caracteres
O URL do item que está sendo comprado. Visível para o comprador e usado nas experiências de compra.

categoria
string [ 1 .. 20 ] caracteres
O tipo de categoria do item.

Valor de enumeração	Descrição
PRODUTOS DIGITAIS	Bens armazenados, entregues e utilizados em formato eletrônico. Este valor não é atualmente suportado para chamadas de API que utilizam a plataforma PayPal for Commerce .
BENS FÍSICOS	Um item tangível que pode ser enviado com comprovante de entrega.
DOAÇÃO	Uma contribuição ou doação sem troca de bens ou serviços, geralmente destinada a uma organização sem fins lucrativos.
URL da imagem
string [ 1 .. 2048 ] caracteres ^(https:)([/|.|\w|\s|-])*\.(?:jpg|gif|png|jpe... Mostrar padrão
O URL da imagem do item. Restrições de tipo e tamanho de arquivo se aplicam. Imagens que violarem essas restrições não serão aceitas.


valor_unitário
obrigatório
objeto
O preço ou taxa por unidade do item. Se você especificar unit_amount, purchase_units[].amount.breakdown.item_totalé obrigatório. Deve ser igual unit_amount * quantitypara todos os itens. unit_amount.valueNão pode ser um número negativo.


imposto
objeto
O imposto por item é calculado para cada unidade. Se taxespecificado, purchase_units[].amount.breakdown.tax_totalé obrigatório. Deve ser igual tax * quantitypara todos os itens. tax.valueNão pode ser um número negativo.


upc
objeto
O Código Universal do Produto (UPC) do item.


plano de faturamento
objeto
Metadados para planos de cobrança recorrente gerenciados pelo comerciante. Válido somente durante a criação do token do método de pagamento salvo ou do contrato de cobrança.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
linguagem
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 ou código de região M49 .

string ( idioma ) [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}|[... <ppaas_common_language_v3> Mostrar padrão
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 ou código de região M49 .

Cópia
"string"
linguagem
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 ou código de região M49 .

string ( idioma ) [ 2 .. 10 ] caracteres ^[az]{2}(?:-[AZ][az]{3})?(?:-(?:[AZ]{2}|[... <ppaas_common_language_v3> Mostrar padrão
A etiqueta de idioma para o idioma no qual as strings relacionadas a erros, como mensagens, problemas e ações sugeridas, serão localizadas. A etiqueta é composta pelo código de idioma ISO 639-2 , pela etiqueta de script opcional ISO-15924 e pelo código de país ISO-3166 alpha-2 ou código de região M49 .

Cópia
"string"
nível_2
The level 2 card processing data collections. If your merchant account has been configured for Level 2 processing this field will be passed to the processor on your behalf. Please contact your PayPal Technical Account Manager to define level 2 data for your business.

invoice_id	
string [ 1 .. 127 ] characters
Use this field to pass a purchase identification value of up to 127 ASCII characters. The length of this field will be adjusted to meet network specifications (25chars for Visa and Mastercard, 17chars for Amex), and the original invoice ID will still be displayed in your existing reports.

tax_total	
object
Use this field to break down the amount of tax included in the total purchase amount. The value provided here will not add to the total purchase amount. The value can't be negative, and in most cases, it must be greater than zero in order to qualify for lower interchange rates. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
CopyExpand allCollapse all
{
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
}
level_3
The level 3 card processing data collections, If your merchant account has been configured for Level 3 processing this field will be passed to the processor on your behalf. Please contact your PayPal Technical Account Manager to define level 3 data for your business.

ships_from_postal_code	
string [ 1 .. 60 ] characters
Use this field to specify the postal code of the shipping location.

line_items	
Array of objects [ 1 .. 100 ] items
A list of the items that were purchased with this payment. If your merchant account has been configured for Level 3 processing this field will be passed to the processor on your behalf.

shipping_amount	
object
Use this field to break down the shipping cost included in the total purchase amount. The value provided here will not add to the total purchase amount. The value cannot be negative.

duty_amount	
object
Use this field to break down the duty amount included in the total purchase amount. The value provided here will not add to the total purchase amount. The value cannot be negative.

discount_amount	
object
Use this field to break down the discount amount included in the total purchase amount. The value provided here will not add to the total purchase amount. The value cannot be negative.

shipping_address	
object
The address of the person to whom to ship the items. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

CopyExpand allCollapse all
{
"ships_from_postal_code": "string",
"line_items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": null,
"value": null
},
"reload_threshold_amount": {
"currency_code": null,
"value": null
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
liability_shift
Liability shift indicator. The outcome of the issuer's authentication.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Liability shift indicator. The outcome of the issuer's authentication.

Enum Value	Description
NO	Liability is with the merchant.
POSSIBLE	Liability may shift to the card issuer.
UNKNOWN	The authentication system is not available.
Copy
"NO"
line_item
The line items for this purchase. If your merchant account has been configured for Level 3 processing this field will be passed to the processor on your behalf.

name
required
string [ 1 .. 127 ] characters
The item name or title.

quantity
required
string <= 10 characters
The item quantity. Must be a whole number.

description	
string <= 2048 characters
The detailed item description.

sku	
string <= 127 characters
The stock keeping unit (SKU) for the item.

url	
string [ 1 .. 2048 ] characters
The URL to the item being purchased. Visible to buyer and used in buyer experiences.

image_url	
string [ 1 .. 2048 ] characters ^(https:)([/|.|\w|\s|-])*\.(?:jpg|gif|png|jpe...Show pattern
The URL of the item's image. File type and size restrictions apply. An image that violates these restrictions will not be honored.

upc	
object
The Universal Product Code of the item.

billing_plan	
object
Metadata for merchant-managed recurring billing plans. Valid only during the saved payment method token or billing agreement creation.

commodity_code	
string [ 1 .. 12 ] characters
Code used to classify items purchased and track the total amount spent across various categories of products and services. Different corporate purchasing organizations may use different standards, but the United Nations Standard Products and Services Code (UNSPSC) is frequently used.

unit_of_measure	
string [ 1 .. 12 ] characters
Unit of measure is a standard used to express the magnitude of a quantity in international trade. Most commonly used (but not limited to) examples are: Acre (ACR), Ampere (AMP), Centigram (CGM), Centimetre (CMT), Cubic inch (INQ), Cubic metre (MTQ), Fluid ounce (OZA), Foot (FOT), Hour (HUR), Item (ITM), Kilogram (KGM), Kilometre (KMT), Kilowatt (KWT), Liquid gallon (GLL), Liter (LTR), Pounds (LBS), Square foot (FTK).

unit_amount
required
object
The item price or rate per unit. Must equal unit_amount * quantity for all items. unit_amount.value can not be a negative number.

tax	
object
The item tax for each unit. Must equal tax * quantity for all items. tax.value can not be a negative number.

discount_amount	
object
Use this field to break down the discount amount included in the total purchase amount. The value provided here will not add to the total purchase amount. The value cannot be negative.

total_amount	
object
The subtotal for all items. Must equal the sum of (items[].unit_amount * items[].quantity) for all items. item_total.value can not be a negative number.

CopyExpand allCollapse all
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
Link Description
The request-related HATEOAS link information.

href
required
string
The complete target URL. To make the related call, combine the method with this URI Template-formatted link. For pre-processing, include the $, (, and ) characters. The href is the key HATEOAS component that links a completed call with a subsequent call.

rel
required
string
The link relation type, which serves as an ID for a link that unambiguously describes the semantics of the link. See Link Relations.

method	
string
The HTTP method required to make the related call.

Enum Value	Description
GET	The HTTP GET method.
POST	The HTTP POST method.
PUT	The HTTP PUT method.
DELETE	The HTTP DELETE method.
HEAD	The HTTP HEAD method.
CONNECT	The HTTP CONNECT method.
OPTIONS	The HTTP OPTIONS method.
PATCH	The HTTP PATCH method.
Copy
{
"href": "string",
"rel": "string",
"method": "GET"
}
Link Description
The request-related HATEOAS link information.

href
required
string
The complete target URL. To make the related call, combine the method with this URI Template-formatted link. For pre-processing, include the $, (, and ) characters. The href is the key HATEOAS component that links a completed call with a subsequent call.

rel
required
string
The link relation type, which serves as an ID for a link that unambiguously describes the semantics of the link. See Link Relations.

method	
string
The HTTP method required to make the related call.

Enum Value	Description
GET	The HTTP GET method.
POST	The HTTP POST method.
PUT	The HTTP PUT method.
DELETE	The HTTP DELETE method.
HEAD	The HTTP HEAD method.
CONNECT	The HTTP CONNECT method.
OPTIONS	The HTTP OPTIONS method.
PATCH	The HTTP PATCH method.
Copy
{
"href": "string",
"rel": "string",
"method": "GET"
}
Link Description
The request-related HATEOAS link information.

href
required
string
The complete target URL. To make the related call, combine the method with this URI Template-formatted link. For pre-processing, include the $, (, and ) characters. The href is the key HATEOAS component that links a completed call with a subsequent call.

rel
required
string
The link relation type, which serves as an ID for a link that unambiguously describes the semantics of the link. See Link Relations.

method	
string
The HTTP method required to make the related call.

Enum Value	Description
GET	The HTTP GET method.
POST	The HTTP POST method.
PUT	The HTTP PUT method.
DELETE	The HTTP DELETE method.
HEAD	The HTTP HEAD method.
CONNECT	The HTTP CONNECT method.
OPTIONS	The HTTP OPTIONS method.
PATCH	The HTTP PATCH method.
Copy
{
"href": "string",
"rel": "string",
"method": "GET"
}
Merchant App Switch Details & Preferences
Merchant provided details of the native app or mobile web browser to facilitate buyer's app switch to the PayPal consumer app.

native_app	
object
Merchant provided, buyer's native app preferences to app switch to the PayPal consumer app.

mobile_web	
object
Buyer's mobile web browser context to app switch to the PayPal consumer app.

CopyExpand allCollapse all
{
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
}
merchant_partner_customer_id
The unique ID for a customer generated by PayPal.

string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

Copy
"string"
Mobile Web App Switch Context
Buyer's mobile web browser context to app switch to the PayPal consumer app.

return_flow	
string [ 1 .. 6 ] characters
Default: "AUTO"
Merchant preference on how the buyer can navigate back to merchant website post approving the transaction on the PayPal App.

Enum Value	Description
AUTO	After payment approval in the PayPal App, buyer will automatically be redirected to the merchant website.
MANUAL	After payment approval in the PayPal App, buyer will be asked to manually navigate back to the merchant website where they started the transaction from. The buyer is shown a message like 'Return to Merchant' to return to the source where the transaction actually started.
buyer_user_agent	
string [ 1 .. 512 ] characters
User agent from the request originating from the buyer's device. This will be used to identify the buyer's operating system and browser versions. NOTE: Merchants must not alter or modify the buyer's device user agent.

Copy
{
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
Money
The currency and amount for a financial transaction, such as a balance or payment due.

currency_code
required
string = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

value
required
string <= 32 characters
The value, which might be:

An integer for currencies like JPY that are not typically fractional.
A decimal fraction for currencies like TND that are subdivided into thousandths.
For the required number of decimal places for a currency code, see Currency Codes.
Copy
{
"currency_code": "str",
"value": "string"
}
Money
The currency and amount for a financial transaction, such as a balance or payment due.

currency_code
required
string = 3 characters
The 3-character ISO-4217 currency code that identifies the currency.

value
required
string <= 32 characters
The value, which might be:

An integer for currencies like JPY that are not typically fractional.
A decimal fraction for currencies like TND that are subdivided into thousandths.
For the required number of decimal places for a currency code, see Currency Codes.
Copy
{
"currency_code": "str",
"value": "string"
}
mybank
Information used to pay using MyBank.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

bic	
string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The bank identification code (BIC).

iban_last_chars	
string [ 4 .. 34 ] characters [a-zA-Z0-9]{4}
The last characters of the IBAN used to pay.

Copy
{
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
}
mybank_request
Information needed to pay using MyBank.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
name
The full name representation like Mr J Smith.

string [ 3 .. 300 ] characters
The full name representation like Mr J Smith.

Copy
"string"
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
Name
The name of the party.

full_name	
string <= 300 characters
When the party is a person, the party's full name.

Copy
{
"full_name": "string"
}
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
Name
The name of the party.

given_name	
string <= 140 characters
When the party is a person, the party's given, or first, name.

surname	
string <= 140 characters
When the party is a person, the party's surname or family name. Also known as the last name. Required when the party is a person. Use also to store multiple surnames including the matronymic, or mother's, surname.

Copy
{
"given_name": "string",
"surname": "string"
}
net_amount_breakdown
The net amount. Returned when the currency of the refund is different from the currency of the PayPal account where the merchant holds their funds.

payable_amount	
object
The net amount debited from the merchant's PayPal account.

converted_amount	
object
The converted payable amount.

exchange_rate	
object
The exchange rate that determines the amount that was debited from the merchant's PayPal account.

CopyExpand allCollapse all
{
"payable_amount": {
"currency_code": "str",
"value": "string"
},
"converted_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
}
network_token
The Third Party Network token used to fund a payment.

number
required
string [ 13 .. 19 ] characters
Third party network token number.

cryptogram	
string [ 28 .. 32 ] characters
An Encrypted one-time use value that's sent along with Network Token. This field is not required to be present for recurring transactions.

token_requestor_id	
string [ 1 .. 11 ] characters
A TRID, or a Token Requestor ID, is an identifier used by merchants to request network tokens from card networks. A TRID is a precursor to obtaining a network token for a credit card primary account number (PAN), and will aid in enabling secure card on file (COF) payments and reducing fraud.

expiry
required
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

eci_flag	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Electronic Commerce Indicator (ECI). The ECI value is part of the 2 data elements that indicate the transaction was processed electronically. This should be passed on the authorization transaction to the Gateway/Processor.

Enum Value	Description
MASTERCARD_NON_3D_SECURE_TRANSACTION	Mastercard non-3-D Secure transaction.
MASTERCARD_ATTEMPTED_AUTHENTICATION_TRANSACTION	Mastercard attempted authentication transaction.
MASTERCARD_FULLY_AUTHENTICATED_TRANSACTION	Mastercard fully authenticated transaction.
FULLY_AUTHENTICATED_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB fully authenticated transaction.
ATTEMPTED_AUTHENTICATION_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB attempted authentication transaction.
NON_3D_SECURE_TRANSACTION	VISA, AMEX, JCB, DINERS CLUB non-3-D Secure transaction.
Copy
{
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
}
network_transaction
Reference values used by the card network to identify a transaction.

id	
string [ 9 .. 36 ] characters
Transaction reference id returned by the scheme. For Visa and Amex, this is the "Tran id" field in response. For MasterCard, this is the "BankNet reference id" field in response. For Discover, this is the "NRID" field in response. The pattern we expect for this field from Visa/Amex/CB/Discover is numeric, Mastercard/BNPP is alphanumeric and Paysecure is alphanumeric with special character -.

date	
string = 4 characters
The date that the transaction was authorized by the scheme. This field may not be returned for all networks. MasterCard refers to this field as "BankNet reference date". For some specific networks, such as MasterCard and Discover, this date field is mandatory when the previous_network_transaction_reference_id is passed.

acquirer_reference_number	
string [ 1 .. 36 ] characters
Reference ID issued for the card transaction. This ID can be used to track the transaction across processors, card brands and issuing banks.

network	
string [ 1 .. 255 ] characters ^[A-Z_]+$
Name of the card network through which the transaction was routed.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
Copy
{
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
network_transaction_reference
Reference values used by the card network to identify a transaction.

id
required
string [ 9 .. 36 ] characters
Transaction reference id returned by the scheme. For Visa and Amex, this is the "Tran id" field in response. For MasterCard, this is the "BankNet reference id" field in response. For Discover, this is the "NRID" field in response. The pattern we expect for this field from Visa/Amex/CB/Discover is numeric, Mastercard/BNPP is alphanumeric and Paysecure is alphanumeric with special character -.

date	
string = 4 characters
The date that the transaction was authorized by the scheme. This field may not be returned for all networks. MasterCard refers to this field as "BankNet reference date". For some specific networks, such as MasterCard and Discover, this date field is mandatory when the previous_network_transaction_reference_id is passed.

acquirer_reference_number	
string [ 1 .. 36 ] characters
Reference ID issued for the card transaction. This ID can be used to track the transaction across processors, card brands and issuing banks.

network	
string [ 1 .. 255 ] characters ^[A-Z_]+$
Name of the card network through which the transaction was routed.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastercard card.
DISCOVER	Discover card.
AMEX	American Express card.
SOLO	Solo debit card.
JCB	Japan Credit Bureau card.
STAR	Military Star card.
DELTA	Delta Airlines card.
SWITCH	Switch credit card.
MAESTRO	Maestro credit card.
CB_NATIONALE	Carte Bancaire (CB) credit card.
CONFIGOGA	Configoga credit card.
CONFIDIS	Confidis credit card.
ELECTRON	Visa Electron credit card.
CETELEM	Cetelem credit card.
CHINA_UNION_PAY	China union pay credit card.
DINERS	The Diners Club International banking and payment services capability network owned by Discover Financial Services (DFS), one of the most recognized brands in US financial services.
ELO	The Brazilian Elo card payment network.
HIPER	The Hiper - Ingenico ePayment network.
HIPERCARD	The Brazilian Hipercard payment network that's widely accepted in the retail market.
RUPAY	The RuPay payment network.
GE	The GE Credit Union 3Point card payment network.
SYNCHRONY	The Synchrony Financial (SYF) payment network.
EFTPOS	The Electronic Fund Transfer At Point of Sale(EFTPOS) Debit card payment network.
CARTE_BANCAIRE	The Carte Bancaire payment network.
STAR_ACCESS	The Star Access payment network.
PULSE	The Pulse payment network.
NYCE	The NYCE payment network.
ACCEL	The Accel payment network.
UNKNOWN	UNKNOWN payment network.
Copy
{
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
Order
The order details.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

id	
string
The ID of the order.

purchase_units	
Array of objects [ 1 .. 10 ] items
An array of purchase units. Each purchase unit establishes a contract between a customer and merchant. Each purchase unit represents either a full or partial order that the customer intends to purchase from the merchant.

links	
Array of objects
An array of request-related HATEOAS links. To complete payer approval, use the approve link to redirect the payer. The API caller has 6 hours (default setting, this which can be changed by your account manager to 24/48/72 hours to accommodate your use case) from the time the order is created, to redirect your payer. Once redirected, the API caller has 6 hours for the payer to approve the order and either authorize or capture the order. If you are not using the PayPal JavaScript SDK to initiate PayPal Checkout (in context) ensure that you include application_context.return_url is specified or you will get "We're sorry, Things don't appear to be working at the moment" after the payer approves the payment.

payment_source	
object
The payment source used to fund the payment.

intent	
string
The intent to either capture payment immediately or authorize a payment for an order after order creation.

Enum Value	Description
CAPTURE	The merchant intends to capture payment immediately after the customer makes a payment.
AUTHORIZE	The merchant intends to authorize a payment and place funds on hold after the customer makes a payment. Authorized payments are best captured within three days of authorization but are available to capture for up to 29 days. After the three-day honor period, the original authorized payment expires and you must re-authorize the payment. You must make a separate request to capture payments on demand. This intent is not supported when you have more than one purchase_unit within your order.
payer	
object
The customer who approves and pays for the order. The customer is also known as the payer.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The order status.

Enum Value	Description
CREATED	The order was created with the specified context.
SAVED	The order was saved and persisted. The order status continues to be in progress until a capture is made with final_capture = true for all purchase units within the order.
APPROVED	The customer approved the payment through the PayPal wallet or another form of guest or unbranded payment. For example, a card, bank account, or so on.
VOIDED	All purchase units in the order are voided.
COMPLETED	The intent of the order was completed and a payments resource was created. Important: Check the payment status in purchase_units[].payments.captures[].status before fulfilling the order. A completed order can indicate a payment was authorized, an authorized payment was captured, or a payment was declined.
PAYER_ACTION_REQUIRED	The order requires an action from the payer (e.g. 3DS authentication). Redirect the payer to the "rel":"payer-action" HATEOAS link returned as part of the response prior to authorizing or capturing the order. Some payment sources may not return a payer-action HATEOAS link (eg. MB WAY). For these payment sources the payer-action is managed by the scheme itself (eg. through SMS, email, in-app notification, etc).
CopyExpand allCollapse all
{
"create_time": "string",
"update_time": "string",
"id": "string",
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": null,
"total_cycles": null,
"sequence": null,
"pricing_scheme": null,
"start_date": null
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"most_recent_errors": [
null
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"trackers": [
{
"id": "string",
"status": "CANCELLED",
"items": [
{
"name": null,
"quantity": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null
}
],
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"create_time": "string",
"update_time": "string"
}
]
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": null,
"quantity": null,
"description": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null,
"billing_plan": null,
"commodity_code": null,
"unit_of_measure": null,
"unit_amount": null,
"tax": null,
"discount_amount": null,
"total_amount": null
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
},
"payments": {
"authorizations": [
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
null
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"captures": [
{
"create_time": "string",
"update_time": "string",
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"final_capture": false,
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
null
]
},
"seller_receivable_breakdown": {
"platform_fees": [
null
],
"gross_amount": {
"currency_code": null,
"value": null
},
"paypal_fee": {
"currency_code": null,
"value": null
},
"paypal_fee_in_receivable_currency": {
"currency_code": null,
"value": null
},
"net_amount": {
"currency_code": null,
"value": null
},
"receivable_amount": {
"currency_code": null,
"value": null
},
"exchange_rate": {
"value": null,
"source_currency": null,
"target_currency": null
}
},
"disbursement_mode": "INSTANT",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"refunds": [
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"acquirer_reference_number": "string",
"note_to_payer": "string",
"seller_payable_breakdown": {
"platform_fees": [
null
],
"net_amount_breakdown": [
null
],
"gross_amount": {
"currency_code": null,
"value": null
},
"paypal_fee": {
"currency_code": null,
"value": null
},
"paypal_fee_in_receivable_currency": {
"currency_code": null,
"value": null
},
"net_amount": {
"currency_code": null,
"value": null
},
"net_amount_in_receivable_currency": {
"currency_code": null,
"value": null
},
"total_refunded_amount": {
"currency_code": null,
"value": null
}
},
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"payer": {
"email_address": "string",
"merchant_id": "string"
},
"create_time": "string",
"update_time": "string"
}
]
}
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"payment_source": {
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
},
"bancontact": {
"card_last_digits": "stri",
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"blik": {
"name": "string",
"country_code": "string",
"email": "string",
"one_click": {
"consumer_reference": "string"
}
},
"eps": {
"name": "string",
"country_code": "string",
"bic": "string"
},
"giropay": {
"name": "string",
"country_code": "string",
"bic": "string"
},
"ideal": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"mybank": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"p24": {
"payment_descriptor": "string",
"method_id": "string",
"method_description": "string",
"name": "string",
"email": "string",
"country_code": "string"
},
"sofort": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"trustly": {
"name": "string",
"country_code": "string",
"email": "string",
"bic": "string",
"iban_last_chars": "string"
},
"venmo": {
"user_name": "string",
"return_flow": "AUTO",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
},
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"paypal": {
"account_status": "VERIFIED",
"phone_type": "FAX",
"business_name": "string",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"experience_status": "NOT_STARTED",
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"apple_pay": {
"id": "string",
"token": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": null,
"phone_number": null
},
"name": {
"given_name": null,
"surname": null
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
}
},
"intent": "CAPTURE",
"payer": {
"email_address": "string",
"payer_id": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"status": "CREATED"
}
Order Authorize Request
The authorization of an order request.

payment_source	
object
The source of payment for the order, which can be a token or a card. Use this object only if you have not redirected the user after order creation to approve the payment. In such cases, the user-selected payment method in the PayPal flow is implicitly used.

CopyExpand allCollapse all
{
"payment_source": {
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
}
Order Authorize Response
The order authorize response.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

id	
string
The ID of the order.

purchase_units	
Array of objects [ 1 .. 10 ] items
An array of purchase units. Each purchase unit establishes a contract between a customer and merchant. Each purchase unit represents either a full or partial order that the customer intends to purchase from the merchant.

links	
Array of objects
An array of request-related HATEOAS links. To complete payer approval, use the approve link to redirect the payer. The API caller has 6 hours (default setting, this which can be changed by your account manager to 24/48/72 hours to accommodate your use case) from the time the order is created, to redirect your payer. Once redirected, the API caller has 6 hours for the payer to approve the order and either authorize or capture the order. If you are not using the PayPal JavaScript SDK to initiate PayPal Checkout (in context) ensure that you include application_context.return_url is specified or you will get "We're sorry, Things don't appear to be working at the moment" after the payer approves the payment.

payment_source	
object
The payment source used to fund the payment.

intent	
string
The intent to either capture payment immediately or authorize a payment for an order after order creation.

Enum Value	Description
CAPTURE	The merchant intends to capture payment immediately after the customer makes a payment.
AUTHORIZE	The merchant intends to authorize a payment and place funds on hold after the customer makes a payment. Authorized payments are best captured within three days of authorization but are available to capture for up to 29 days. After the three-day honor period, the original authorized payment expires and you must re-authorize the payment. You must make a separate request to capture payments on demand. This intent is not supported when you have more than one purchase_unit within your order.
payer	
object
The customer who approves and pays for the order. The customer is also known as the payer.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The order status.

Enum Value	Description
CREATED	The order was created with the specified context.
SAVED	The order was saved and persisted. The order status continues to be in progress until a capture is made with final_capture = true for all purchase units within the order.
APPROVED	The customer approved the payment through the PayPal wallet or another form of guest or unbranded payment. For example, a card, bank account, or so on.
VOIDED	All purchase units in the order are voided.
COMPLETED	The intent of the order was completed and a payments resource was created. Important: Check the payment status in purchase_units[].payments.captures[].status before fulfilling the order. A completed order can indicate a payment was authorized, an authorized payment was captured, or a payment was declined.
PAYER_ACTION_REQUIRED	The order requires an action from the payer (e.g. 3DS authentication). Redirect the payer to the "rel":"payer-action" HATEOAS link returned as part of the response prior to authorizing or capturing the order. Some payment sources may not return a payer-action HATEOAS link (eg. MB WAY). For these payment sources the payer-action is managed by the scheme itself (eg. through SMS, email, in-app notification, etc).
CopyExpand allCollapse all
{
"create_time": "string",
"update_time": "string",
"id": "string",
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": null,
"total_cycles": null,
"sequence": null,
"pricing_scheme": null,
"start_date": null
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"most_recent_errors": [
null
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"trackers": [
{
"id": "string",
"status": "CANCELLED",
"items": [
{
"name": null,
"quantity": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null
}
],
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"create_time": "string",
"update_time": "string"
}
]
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": null,
"quantity": null,
"description": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null,
"billing_plan": null,
"commodity_code": null,
"unit_of_measure": null,
"unit_amount": null,
"tax": null,
"discount_amount": null,
"total_amount": null
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
},
"payments": {
"authorizations": [
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
null
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"captures": [
{
"create_time": "string",
"update_time": "string",
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"final_capture": false,
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
null
]
},
"seller_receivable_breakdown": {
"platform_fees": [
null
],
"gross_amount": {
"currency_code": null,
"value": null
},
"paypal_fee": {
"currency_code": null,
"value": null
},
"paypal_fee_in_receivable_currency": {
"currency_code": null,
"value": null
},
"net_amount": {
"currency_code": null,
"value": null
},
"receivable_amount": {
"currency_code": null,
"value": null
},
"exchange_rate": {
"value": null,
"source_currency": null,
"target_currency": null
}
},
"disbursement_mode": "INSTANT",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"refunds": [
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"acquirer_reference_number": "string",
"note_to_payer": "string",
"seller_payable_breakdown": {
"platform_fees": [
null
],
"net_amount_breakdown": [
null
],
"gross_amount": {
"currency_code": null,
"value": null
},
"paypal_fee": {
"currency_code": null,
"value": null
},
"paypal_fee_in_receivable_currency": {
"currency_code": null,
"value": null
},
"net_amount": {
"currency_code": null,
"value": null
},
"net_amount_in_receivable_currency": {
"currency_code": null,
"value": null
},
"total_refunded_amount": {
"currency_code": null,
"value": null
}
},
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"payer": {
"email_address": "string",
"merchant_id": "string"
},
"create_time": "string",
"update_time": "string"
}
]
}
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"payment_source": {
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
},
"venmo": {
"user_name": "string",
"return_flow": "AUTO",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
},
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"paypal": {
"account_status": "VERIFIED",
"phone_type": "FAX",
"business_name": "string",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"experience_status": "NOT_STARTED",
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"apple_pay": {
"id": "string",
"token": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": null,
"rel": null,
"method": null
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": null,
"phone_number": null
},
"name": {
"given_name": null,
"surname": null
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
}
},
"intent": "CAPTURE",
"payer": {
"email_address": "string",
"payer_id": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"status": "CREATED"
}
Order Capture Request
Completes an capture payment for an order.

payment_source	
object
The payment source definition.

CopyExpand allCollapse all
{
"payment_source": {
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
}
Order Confirm Application Context
Customizes the payer confirmation experience.

brand_name	
string [ 1 .. 127 ] characters
Label to present to your payer as part of the PayPal hosted web experience.

return_url	
string [ 10 .. 4000 ] characters
The URL where the customer is redirected after the customer approves the payment.

cancel_url	
string [ 10 .. 4000 ] characters
The URL where the customer is redirected after the customer cancels the payment.

locale	
string [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The BCP 47-formatted locale of pages that the PayPal payment experience shows. PayPal supports a five-character code. For example, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, or zh-TW.

stored_payment_source	
object
Provides additional details to process a payment using a payment_source that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
CopyExpand allCollapse all
{
"brand_name": "string",
"return_url": "http://example.com",
"cancel_url": "http://example.com",
"locale": "string",
"stored_payment_source": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
}
Order Request
The order request details.

purchase_units
required
Array of objects [ 1 .. 10 ] items
An array of purchase units. Each purchase unit establishes a contract between a payer and the payee. Each purchase unit represents either a full or partial order that the payer intends to purchase from the payee.

intent
required
string
The intent to either capture payment immediately or authorize a payment for an order after order creation.

Enum Value	Description
CAPTURE	The merchant intends to capture payment immediately after the customer makes a payment.
AUTHORIZE	The merchant intends to authorize a payment and place funds on hold after the customer makes a payment. Authorized payments are best captured within three days of authorization but are available to capture for up to 29 days. After the three-day honor period, the original authorized payment expires and you must re-authorize the payment. You must make a separate request to capture payments on demand. This intent is not supported when you have more than one purchase_unit within your order.
payer	
object
DEPRECATED. The customer is also known as the payer. The Payer object was intended to only be used with the payment_source.paypal object. In order to make this design more clear, the details in the payer object are now available under payment_source.paypal. Please use payment_source.paypal.

payment_source	
object
The payment source definition.

application_context	
object
Customize the payer experience during the approval process for the payment with PayPal.

CopyExpand allCollapse all
{
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": null,
"total_cycles": null,
"sequence": null,
"pricing_scheme": null,
"start_date": null
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": null,
"quantity": null,
"description": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null,
"billing_plan": null,
"commodity_code": null,
"unit_of_measure": null,
"unit_amount": null,
"tax": null,
"discount_amount": null,
"total_amount": null
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
}
}
],
"intent": "CAPTURE",
"payer": {
"email_address": "string",
"payer_id": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_source": {
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"bancontact": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"blik": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"consumer_user_agent": "string",
"consumer_ip": "string"
},
"level_0": {
"auth_code": "string"
},
"one_click": {
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
},
"eps": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"giropay": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"ideal": {
"name": "string",
"country_code": "string",
"bic": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"mybank": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"p24": {
"name": "string",
"email": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"sofort": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"trustly": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
},
"application_context": {
"brand_name": "string",
"landing_page": "LOGIN",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"return_url": "http://example.com",
"cancel_url": "http://example.com",
"locale": "string",
"payment_method": {
"standard_entry_class_code": "TEL",
"payee_preferred": "UNRESTRICTED"
},
"stored_payment_source": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
}
}
Order Status
The order status.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The order status.

Enum Value	Description
CREATED	The order was created with the specified context.
SAVED	The order was saved and persisted. The order status continues to be in progress until a capture is made with final_capture = true for all purchase units within the order.
APPROVED	The customer approved the payment through the PayPal wallet or another form of guest or unbranded payment. For example, a card, bank account, or so on.
VOIDED	All purchase units in the order are voided.
COMPLETED	The intent of the order was completed and a payments resource was created. Important: Check the payment status in purchase_units[].payments.captures[].status before fulfilling the order. A completed order can indicate a payment was authorized, an authorized payment was captured, or a payment was declined.
PAYER_ACTION_REQUIRED	The order requires an action from the payer (e.g. 3DS authentication). Redirect the payer to the "rel":"payer-action" HATEOAS link returned as part of the response prior to authorizing or capturing the order. Some payment sources may not return a payer-action HATEOAS link (eg. MB WAY). For these payment sources the payer-action is managed by the scheme itself (eg. through SMS, email, in-app notification, etc).
Copy
"CREATED"
order_billing_plan
Metadata for merchant-managed recurring billing plans. Valid only during the saved payment method token or billing agreement creation.

billing_cycles
required
Array of objects [ 1 .. 3 ] items
An array of billing cycles for trial billing and regular billing. A plan can have at most two trial cycles and only one regular cycle.

name	
string [ 1 .. 127 ] characters
Name of the recurring plan.

setup_fee	
object
The setup fee for the recurring plan. Ensure its part of the item amount.

CopyExpand allCollapse all
{
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
OrderUpdateCallbackErrorResponse
The error details.

name
required
string [ 1 .. 256 ] characters
The human-readable, unique name of the error.

message	
string [ 1 .. 2048 ] characters
The message that describes the error.

details	
Array of objects [ 1 .. 100 ] items
An array of additional details about the error.

CopyExpand allCollapse all
{
"name": "string",
"message": "string",
"details": [
{
"field": "string",
"value": "string",
"issue": "string"
}
]
}
OrderUpdateCallbackErrorResponseDetails
The error details. Required for client-side 4XX errors.

field	
string [ 0 .. 256 ] characters
The field that caused the error. If this field is in the body, set this value to the field's JSON pointer value. Required for client-side errors.

value	
string [ 0 .. 1024 ] characters
The value of the field that caused the error.

issue
required
string [ 0 .. 256 ] characters
The unique, fine-grained application-level error code.

Copy
{
"field": "string",
"value": "string",
"issue": "string"
}
OrderUpdateCallbackRequest
Shipping Options Callback request. This will be implemented by the merchants.

id	
string [ 1 .. 36 ] characters
The ID of the order.

purchase_units
required
Array of objects = 1 items
An array of purchase units. At present only 1 purchase_unit is supported. Each purchase unit establishes a contract between a payer and the payee. Each purchase unit represents either a full or partial order that the payer intends to purchase from the payee.

shipping_address	
object
Redacted shipping address to be used for shipping options and tax calculations.

shipping_option	
object
Buyer selected shipping option.

CopyExpand allCollapse all
{
"id": "string",
"purchase_units": [
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": null,
"total_cycles": null,
"sequence": null,
"pricing_scheme": null,
"start_date": null
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": null,
"quantity": null,
"description": null,
"sku": null,
"url": null,
"image_url": null,
"upc": null,
"billing_plan": null,
"commodity_code": null,
"unit_of_measure": null,
"unit_amount": null,
"tax": null,
"discount_amount": null,
"total_amount": null
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
}
}
],
"shipping_address": {
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"shipping_option": {
"id": "string",
"label": "string",
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
}
OrderUpdateCallbackResponse
Returns the updated shipping options for an order.

id	
string [ 1 .. 36 ] characters
The ID of the order.

purchase_units	
object
This would contain shipping option and amount data at purchase unit level.

CopyExpand allCollapse all
{
"id": "string",
"purchase_units": {
"reference_id": "string",
"shipping_options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
}
}
}
Orthography Type
The orthography type based on the ISO 15924 names for scripts. Scipts are chosen based on most widely used writing systems.

string = 4 characters ^[A-Z][a-z]{3}$
The orthography type based on the ISO 15924 names for scripts. Scipts are chosen based on most widely used writing systems.

Enum Value	Description
Zyyy	The orthography cannot be determined.
Zzzz	The orthography is unknown.
Kana	An angular form of Japanese writing for words of foreign origin.
Cyrl	The Slavic languages alphabet. Used in eastern Europe.
Arab	The Arabic language alphabet.
Armn	The Armenian alphabet.
Beng	The Bengali alphabet. Used in eastern India.
Cans	The Unified Canadian Aboriginal Syllabics alphabet.
Deva	The Devanagari (Nagari) alphabet.
Ethi	The Ethiopic alphabet.
Geor	The Georgian (Mkhedruli and Mtavruli) alphabet.
Grek	The Greek alphabet.
Gujr	The Gujurati language alphabet. Used in western India.
Guru	The Gurmukhi alphabet. Used in the northern Indian state of Punjab.
Hani	The Han (Hanzi, Kanji, Hanja) alphabet.
Hebr	The Hebrew alphabet.
Java	The Javanese alphabet.
Jpan	The Japanese (alias for Han + Hiragana + Katakana) alphabet.
Khmr	The Khmer alphabet.
Knda	The Kannada alphabet. Used in the southern Indian state of Karnataka.
Kore	Korean (alias for Hangul + Han).
Laoo	The Lao alphabet.
Latn	The Latin alphabet.
Mlym	The Malayalam alphabet. Used in the southern Indian state of Kerala.
Mong	The Mongolian alphabet.
Mymr	The Myanmar (Burmese) alphabet.
Orya	The Oriya (Odia) alphabet. Used in the eastern Indian state of Odisha.
Sinh	The Sinhala alphabet.
Sund	The Sundanese alphabet.
Syrc	The Syriac alphabet.
Taml	The Tamil alphabet. Used in the southern Indian state of Tamilnadu.
Telu	The Telugu language alphabet. Used in the southern Indian state of Andhra pradesh.
Thaa	The Thaana (Maldivian) alphabet.
Thai	The Thai alphabet. Used in Thailand.
Tibt	The Tibetan alphabet.
Yiii	The Yi alphabet.
Copy
"Zyyy"
p24
Information used to pay using P24(Przelewy24).

payment_descriptor	
string [ 1 .. 2000 ] characters
P24 generated payment description.

method_id	
string [ 1 .. 300 ] characters
Numeric identifier of the payment scheme or bank used for the payment.

method_description	
string [ 1 .. 2000 ] characters
Friendly name of the payment scheme or bank used for the payment.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

email	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

Copy
{
"payment_descriptor": "string",
"method_id": "string",
"method_description": "string",
"name": "string",
"email": "string",
"country_code": "string"
}
p24_request
Information needed to pay using P24 (Przelewy24).

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

email
required
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"email": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
pares_status
Transactions status result identifier. The outcome of the issuer's authentication.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Transactions status result identifier. The outcome of the issuer's authentication.

Enum Value	Description
Y	Successful authentication.
N	Failed authentication / account not verified / transaction denied.
U	Unable to complete authentication.
A	Successful attempts transaction.
C	Challenge required for authentication.
R	Authentication rejected (merchant must not submit for authorization).
D	Challenge required; decoupled authentication confirmed.
I	Informational only; 3DS requestor challenge preference acknowledged.
Copy
"Y"
participant_metadata
Profile information of the sender or receiver.

ip_address	
string [ 7 .. 39 ] characters ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[...Show pattern
The consumer's IP address, which can be represented in either IPv4 or IPv6 format.

Copy
{
"ip_address": "string"
}
Patch
The JSON patch object to apply partial updates to resources.

op
required
string
The operation.

Enum Value	Description
add	Depending on the target location reference, completes one of these functions:
The target location is an array index. Inserts a new value into the array at the specified index.
The target location is an object parameter that does not already exist. Adds a new parameter to the object.
The target location is an object parameter that does exist. Replaces that parameter's value.
The value parameter defines the value to add. For more information, see 4.1. add.
remove	Removes the value at the target location. For the operation to succeed, the target location must exist. For more information, see 4.2. remove.
replace	Replaces the value at the target location with a new value. The operation object must contain a value parameter that defines the replacement value. For the operation to succeed, the target location must exist. For more information, see 4.3. replace.
move	Removes the value at a specified location and adds it to the target location. The operation object must contain a from parameter, which is a string that contains a JSON pointer value that references the location in the target document from which to move the value. For the operation to succeed, the from location must exist. For more information, see 4.4. move.
copy	Copies the value at a specified location to the target location. The operation object must contain a from parameter, which is a string that contains a JSON pointer value that references the location in the target document from which to copy the value. For the operation to succeed, the from location must exist. For more information, see 4.5. copy.
test	Tests that a value at the target location is equal to a specified value. The operation object must contain a value parameter that defines the value to compare to the target location's value. For the operation to succeed, the target location must be equal to the value value. For test, equal indicates that the value at the target location and the value that value defines are of the same JSON type. The data type of the value determines how equality is defined:
Type	Considered equal if both values
strings	Contain the same number of Unicode characters and their code points are byte-by-byte equal.
numbers	Are numerically equal.
arrays	Contain the same number of values, and each value is equal to the value at the corresponding position in the other array, by using these type-specific rules.
objects	Contain the same number of parameters, and each parameter is equal to a parameter in the other object, by comparing their keys (as strings) and their values (by using these type-specific rules).
literals (false, true, and null)	Are the same. The comparison is a logical comparison. For example, whitespace between the parameter values of an array is not significant. Also, ordering of the serialization of object parameters is not significant.
For more information, see 4.6. test.
path	
string
The JSON Pointer to the target document location at which to complete the operation.

value	
any
The value to apply. The remove, copy, and move operations do not require a value. Since JSON Patch allows any type for value, the type property is not specified.

from	
string
The JSON Pointer to the target document location from which to move the value. Required for the move operation.

Copy
{
"op": "add",
"path": "string",
"value": null,
"from": "string"
}
Patch Request
An array of JSON patch objects to apply partial updates to resources.

Array ([ 0 .. 32767 ] items)
op
required
string
The operation.

Enum Value	Description
add	Depending on the target location reference, completes one of these functions:
The target location is an array index. Inserts a new value into the array at the specified index.
The target location is an object parameter that does not already exist. Adds a new parameter to the object.
The target location is an object parameter that does exist. Replaces that parameter's value.
The value parameter defines the value to add. For more information, see 4.1. add.
remove	Removes the value at the target location. For the operation to succeed, the target location must exist. For more information, see 4.2. remove.
replace	Replaces the value at the target location with a new value. The operation object must contain a value parameter that defines the replacement value. For the operation to succeed, the target location must exist. For more information, see 4.3. replace.
move	Removes the value at a specified location and adds it to the target location. The operation object must contain a from parameter, which is a string that contains a JSON pointer value that references the location in the target document from which to move the value. For the operation to succeed, the from location must exist. For more information, see 4.4. move.
copy	Copies the value at a specified location to the target location. The operation object must contain a from parameter, which is a string that contains a JSON pointer value that references the location in the target document from which to copy the value. For the operation to succeed, the from location must exist. For more information, see 4.5. copy.
test	Tests that a value at the target location is equal to a specified value. The operation object must contain a value parameter that defines the value to compare to the target location's value. For the operation to succeed, the target location must be equal to the value value. For test, equal indicates that the value at the target location and the value that value defines are of the same JSON type. The data type of the value determines how equality is defined:
Type	Considered equal if both values
strings	Contain the same number of Unicode characters and their code points are byte-by-byte equal.
numbers	Are numerically equal.
arrays	Contain the same number of values, and each value is equal to the value at the corresponding position in the other array, by using these type-specific rules.
objects	Contain the same number of parameters, and each parameter is equal to a parameter in the other object, by comparing their keys (as strings) and their values (by using these type-specific rules).
literals (false, true, and null)	Are the same. The comparison is a logical comparison. For example, whitespace between the parameter values of an array is not significant. Also, ordering of the serialization of object parameters is not significant.
For more information, see 4.6. test.
path	
string
The JSON Pointer to the target document location at which to complete the operation.

value	
any
The value to apply. The remove, copy, and move operations do not require a value. Since JSON Patch allows any type for value, the type property is not specified.

from	
string
The JSON Pointer to the target document location from which to move the value. Required for the move operation.

CopyExpand allCollapse all
[
{
"op": "add",
"path": "string",
"value": null,
"from": "string"
}
]
payee
The merchant who receives the funds and fulfills the order. The merchant is also known as the payee.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of merchant.

merchant_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The encrypted PayPal account ID of the merchant.

Copy
{
"email_address": "string",
"merchant_id": "string"
}
payee_base
The details for the merchant who receives the funds and fulfills the order. The merchant is also known as the payee.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of merchant.

merchant_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The encrypted PayPal account ID of the merchant.

Copy
{
"email_address": "string",
"merchant_id": "string"
}
payee_base
The details for the merchant who receives the funds and fulfills the order. The merchant is also known as the payee.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of merchant.

merchant_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The encrypted PayPal account ID of the merchant.

Copy
{
"email_address": "string",
"merchant_id": "string"
}
payee_payment_method_preference
The merchant-preferred payment methods.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "UNRESTRICTED"
The merchant-preferred payment methods.

Enum Value	Description
UNRESTRICTED	Accepts any type of payment from the customer.
IMMEDIATE_PAYMENT_REQUIRED	Accepts only immediate payment from the customer. For example, credit card, PayPal balance, or instant ACH. Ensures that at the time of capture, the payment does not have the pending status.
Copy
"UNRESTRICTED"
payer
The customer who approves and pays for the order. The customer is also known as the payer.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of the payer.

payer_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The PayPal-assigned ID for the payer.

name	
object
The name of the payer. Supports only the given_name and surname properties.

phone	
object
The phone number of the customer. Available only when you enable the Contact Telephone Number option in the Profile & Settings for the merchant's PayPal account. The phone.phone_number supports only national_number.

birth_date	
string = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The birth date of the payer in YYYY-MM-DD format.

tax_info	
object
The tax information of the payer. Required only for Brazilian payer's. Both tax_id and tax_id_type are required.

address	
object
The address of the payer. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. Also referred to as the billing address of the customer.

CopyExpand allCollapse all
{
"email_address": "string",
"payer_id": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
payer_base
The customer who approves and pays for the order. The customer is also known as the payer.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of the payer.

payer_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The PayPal-assigned ID for the payer.

Copy
{
"email_address": "string",
"payer_id": "string"
}
Payment Collection
The collection of payments, or transactions, for a purchase unit in an order. For example, authorized payments, captured payments, and refunds.

authorizations	
Array of objects
An array of authorized payments for a purchase unit. A purchase unit can have zero or more authorized payments.

captures	
Array of objects
An array of captured payments for a purchase unit. A purchase unit can have zero or more captured payments.

refunds	
Array of objects
An array of refunds for a purchase unit. A purchase unit can have zero or more refunds.

CopyExpand allCollapse all
{
"authorizations": [
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"captures": [
{
"create_time": "string",
"update_time": "string",
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"final_capture": false,
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"seller_receivable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"receivable_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
},
"disbursement_mode": "INSTANT",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"refunds": [
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"acquirer_reference_number": "string",
"note_to_payer": "string",
"seller_payable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"net_amount_breakdown": [
{
"payable_amount": {
"currency_code": "str",
"value": "string"
},
"converted_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"net_amount_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"total_refunded_amount": {
"currency_code": "str",
"value": "string"
}
},
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"payer": {
"email_address": "string",
"merchant_id": "string"
},
"create_time": "string",
"update_time": "string"
}
]
}
payment_initiator
The person or party who initiated or triggered the payment.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The person or party who initiated or triggered the payment.

Enum Value	Description
CUSTOMER	Payment is initiated with the active engagement of the customer. e.g. a customer checking out on a merchant website.
MERCHANT	Payment is initiated by merchant on behalf of the customer without the active engagement of customer. e.g. a merchant charging the monthly payment of a subscription to the customer.
Copy
"CUSTOMER"
payment_instruction
Any additional payment instructions to be consider during payment processing. This processing instruction is applicable for Capturing an order or Authorizing an Order.

platform_fees	
Array of objects [ 0 .. 1 ] items
An array of various fees, commissions, tips, or donations. This field is only applicable to merchants that been enabled for PayPal Complete Payments Platform for Marketplaces and Platforms capability.

payee_pricing_tier_id	
string [ 1 .. 20 ] characters
This field is only enabled for selected merchants/partners to use and provides the ability to trigger a specific pricing rate/plan for a payment transaction. The list of eligible 'payee_pricing_tier_id' would be provided to you by your Account Manager. Specifying values other than the one provided to you by your account manager would result in an error.

payee_receivable_fx_rate_id	
string [ 1 .. 4000 ] characters
FX identifier generated returned by PayPal to be used for payment processing in order to honor FX rate (for eligible integrations) to be used when amount is settled/received into the payee account.

disbursement_mode	
string [ 1 .. 16 ] characters ^[A-Z_]+$
Default: "INSTANT"
The funds that are held payee by the marketplace/platform. This field is only applicable to merchants that been enabled for PayPal Complete Payments Platform for Marketplaces and Platforms capability.

Enum Value	Description
INSTANT	The funds are released to the merchant immediately.
DELAYED	The funds are held for a finite number of days. The actual duration depends on the region and type of integration. You can release the funds through a referenced payout. Otherwise, the funds disbursed automatically after the specified duration.
CopyExpand allCollapse all
{
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
}
payment_method
The customer and merchant payment preferences.

standard_entry_class_code	
string [ 3 .. 255 ] characters
Default: "WEB"
NACHA (the regulatory body governing the ACH network) requires that API callers (merchants, partners) obtain the consumer’s explicit authorization before initiating a transaction. To stay compliant, you’ll need to make sure that you retain a compliant authorization for each transaction that you originate to the ACH Network using this API. ACH transactions are categorized (using SEC codes) by how you capture authorization from the Receiver (the person whose bank account is being debited or credited). PayPal supports the following SEC codes.

Enum Value	Description
TEL	The API caller (merchant/partner) accepts authorization and payment information from a consumer over the telephone.
WEB	The API caller (merchant/partner) accepts Debit transactions from a consumer on their website.
CCD	Cash concentration and disbursement for corporate debit transaction. Used to disburse or consolidate funds. Entries are usually Optional high-dollar, low-volume, and time-critical. (e.g. intra-company transfers or invoice payments to suppliers).
PPD	Prearranged payment and deposit entries. Used for debit payments authorized by a consumer account holder, and usually initiated by a company. These are usually recurring debits (such as insurance premiums).
payee_preferred	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "UNRESTRICTED"
The merchant-preferred payment methods.

Enum Value	Description
UNRESTRICTED	Accepts any type of payment from the customer.
IMMEDIATE_PAYMENT_REQUIRED	Accepts only immediate payment from the customer. For example, credit card, PayPal balance, or instant ACH. Ensures that at the time of capture, the payment does not have the pending status.
Copy
{
"standard_entry_class_code": "TEL",
"payee_preferred": "UNRESTRICTED"
}
payment_source
The payment source definition.

card	
object
The payment card to use to fund a payment. Can be a credit or debit card.

Note: Passing card number, cvv and expiry directly via the API requires PCI SAQ D compliance.
PayPal offers a mechanism by which you do not have to take on the PCI SAQ D burden by using hosted fields - refer to this Integration Guide.
token	
object
The tokenized payment source to fund a payment.

paypal	
object
Indicates that PayPal Wallet is the payment source. Main use of this selection is to provide additional instructions associated with this choice like vaulting.

bancontact	
object
Bancontact is the most popular online payment in Belgium. More Details.

blik	
object
BLIK is a mobile payment system, created by Polish Payment Standard in order to allow millions of users to pay in shops, payout cash in ATMs and make online purchases and payments. More Details.

eps	
object
The eps transfer is an online payment method developed by many Austrian banks. More Details.

giropay	
object
Giropay is an Internet payment System in Germany, based on online banking. More Details.

ideal	
object
The Dutch payment method iDEAL is an online payment method that enables consumers to pay online through their own bank. More Details.

mybank	
object
MyBank is an e-authorisation solution which enables safe digital payments and identity authentication through a consumer’s own online banking portal or mobile application. More Details.

p24	
object
P24 (Przelewy24) is a secure and fast online bank transfer service linked to all the major banks in Poland. More Details.

sofort	
object
SOFORT Banking is a real-time bank transfer payment method that buyers use to transfer funds directly to merchants from their bank accounts. More Details.

trustly	
object
Trustly is a payment method that allows customers to shop and pay from their bank account. More Details.

apple_pay	
object
ApplePay payment source, allows buyer to pay using ApplePay, both on Web as well as on Native.

google_pay	
object
Google Pay payment source, allows buyer to pay using Google Pay.

venmo	
object
Information needed to indicate that Venmo is being used to fund the payment.

CopyExpand allCollapse all
{
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"bancontact": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"blik": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"consumer_user_agent": "string",
"consumer_ip": "string"
},
"level_0": {
"auth_code": "string"
},
"one_click": {
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
},
"eps": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"giropay": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"ideal": {
"name": "string",
"country_code": "string",
"bic": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"mybank": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"p24": {
"name": "string",
"email": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"sofort": {
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"trustly": {
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
payment_source
The payment source definition.

card	
object
The payment card to use to fund a payment. Can be a credit or debit card.

Note: Passing card number, cvv and expiry directly via the API requires PCI SAQ D compliance.
PayPal offers a mechanism by which you do not have to take on the PCI SAQ D burden by using hosted fields - refer to this Integration Guide.
token	
object
The tokenized payment source to fund a payment.

paypal	
object
Indicates that PayPal Wallet is the payment source. Main use of this selection is to provide additional instructions associated with this choice like vaulting.

apple_pay	
object
ApplePay payment source, allows buyer to pay using ApplePay, both on Web as well as on Native.

google_pay	
object
Google Pay payment source, allows buyer to pay using Google Pay.

venmo	
object
Information needed to indicate that Venmo is being used to fund the payment.

CopyExpand allCollapse all
{
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
payment_source
The payment source definition.

card	
object
The payment card to use to fund a payment. Can be a credit or debit card.

Note: Passing card number, cvv and expiry directly via the API requires PCI SAQ D compliance.
PayPal offers a mechanism by which you do not have to take on the PCI SAQ D burden by using hosted fields - refer to this Integration Guide.
token	
object
The tokenized payment source to fund a payment.

paypal	
object
Indicates that PayPal Wallet is the payment source. Main use of this selection is to provide additional instructions associated with this choice like vaulting.

apple_pay	
object
ApplePay payment source, allows buyer to pay using ApplePay, both on Web as well as on Native.

google_pay	
object
Google Pay payment source, allows buyer to pay using Google Pay.

venmo	
object
Information needed to indicate that Venmo is being used to fund the payment.

CopyExpand allCollapse all
{
"card": {
"name": "string",
"number": "stringstrings",
"security_code": "stri",
"expiry": "string",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"vault_id": "string",
"single_use_token": "string",
"network_token": {
"number": "stringstrings",
"cryptogram": "stringstringstringstringstri",
"token_requestor_id": "string",
"expiry": "string",
"eci_flag": "MASTERCARD_NON_3D_SECURE_TRANSACTION"
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
},
"apple_pay": {
"id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"decrypted_token": {
"device_manufacturer_id": "string",
"payment_data_type": "3DSECURE",
"transaction_amount": {
"currency_code": "str",
"value": "string"
},
"tokenized_card": {
"name": "string",
"number": "stringstrings",
"expiry": "string",
"card_type": "VISA",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"payment_data": {
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
},
"vault_id": "string",
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string",
"card": {
"name": "string",
"number": "stringstrings",
"last_digits": "stri",
"expiry": "string",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"assurance_details": {
"account_verified": false,
"card_holder_authenticated": false
},
"experience_context": {
"return_url": "string",
"cancel_url": "string"
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
}
payment_source_response
The payment source used to fund the payment.

card	
object
The payment card to use to fund a payment. Card can be a credit or debit card.

bancontact	
object
Information used to pay Bancontact.

blik	
object
Information used to pay using BLIK.

eps	
object
Information used to pay using eps.

giropay	
object
Information needed to pay using giropay.

ideal	
object
Information used to pay using iDEAL.

mybank	
object
Information used to pay using MyBank.

p24	
object
Information used to pay using P24(Przelewy24).

sofort	
object
Information used to pay using Sofort.

trustly	
object
Information needed to pay using Trustly.

venmo	
object
Venmo wallet response.

paypal	
object
The PayPal Wallet response.

apple_pay	
object
Information needed to pay using ApplePay.

google_pay	
object
Google Pay Wallet payment data.

CopyExpand allCollapse all
{
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
},
"bancontact": {
"card_last_digits": "stri",
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"blik": {
"name": "string",
"country_code": "string",
"email": "string",
"one_click": {
"consumer_reference": "string"
}
},
"eps": {
"name": "string",
"country_code": "string",
"bic": "string"
},
"giropay": {
"name": "string",
"country_code": "string",
"bic": "string"
},
"ideal": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"mybank": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"p24": {
"payment_descriptor": "string",
"method_id": "string",
"method_description": "string",
"name": "string",
"email": "string",
"country_code": "string"
},
"sofort": {
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
},
"trustly": {
"name": "string",
"country_code": "string",
"email": "string",
"bic": "string",
"iban_last_chars": "string"
},
"venmo": {
"user_name": "string",
"return_flow": "AUTO",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
},
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"paypal": {
"account_status": "VERIFIED",
"phone_type": "FAX",
"business_name": "string",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"experience_status": "NOT_STARTED",
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"apple_pay": {
"id": "string",
"token": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
}
}
payment_source_response
The payment source used to fund the payment.

card	
object
The payment card to use to fund a payment. Card can be a credit or debit card.

venmo	
object
Venmo wallet response.

paypal	
object
The PayPal Wallet response.

apple_pay	
object
Information needed to pay using ApplePay.

google_pay	
object
Google Pay Wallet payment data.

CopyExpand allCollapse all
{
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
},
"venmo": {
"user_name": "string",
"return_flow": "AUTO",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
},
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"paypal": {
"account_status": "VERIFIED",
"phone_type": "FAX",
"business_name": "string",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"experience_status": "NOT_STARTED",
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"apple_pay": {
"id": "string",
"token": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"name": "string",
"email_address": "string",
"phone_number": {
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "string",
"available_networks": [
"VISA"
],
"from_request": {
"last_digits": "stri",
"expiry": "string"
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
},
"brand": "VISA",
"type": "CREDIT",
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
},
"expiry": "string",
"bin_details": {
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
},
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"country_code": "string"
},
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": {
"name": "string",
"last_digits": "stri",
"type": "CREDIT",
"brand": "VISA",
"billing_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"authentication_result": {
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
}
}
}
PayPal Account Identifier
The account identifier for a PayPal account.

string (PayPal Account Identifier) = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The account identifier for a PayPal account.

Copy
"stringstrings"
paypal_wallet
A resource that identifies a PayPal Wallet is used for payment.

experience_context	
object
Customizes the payer experience during the approval process for payment with PayPal.

Note: Partners and Marketplaces might configure brand_name and shipping_preference during partner account setup, which overrides the request values.
billing_agreement_id	
string [ 2 .. 128 ] characters ^[a-zA-Z0-9-]+$
The PayPal billing agreement ID. References an approved recurring payment for goods or services.

stored_credential	
object
Provides additional details to process a payment using the PayPal wallet billing agreement or a vaulted payment method that has been stored or is intended to be stored.

vault_id	
string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated ID for the payment_source stored within the Vault.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of the PayPal account holder.

name	
object
The name of the PayPal account holder. Supports only the given_name and surname properties.

phone	
object
The phone number of the customer. Available only when you enable the Contact Telephone Number option in the Profile & Settings for the merchant's PayPal account. The phone.phone_number supports only national_number.

birth_date	
string = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The birth date of the PayPal account holder in YYYY-MM-DD format.

tax_info	
object
The tax information of the PayPal account holder. Required only for Brazilian PayPal account holder's. Both tax_id and tax_id_type are required.

address	
object
The address of the PayPal account holder. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. Also referred to as the billing address of the customer.

attributes	
object
Additional attributes associated with the use of this wallet.

CopyExpand allCollapse all
{
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"billing_agreement_id": "string",
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
paypal_wallet_attributes
Additional attributes associated with the use of this PayPal Wallet.

customer	
object
The details about a customer in PayPal's system of record.

vault	
object
Attributes used to provide the instructions during vaulting of the PayPal Wallet.

CopyExpand allCollapse all
{
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
paypal_wallet_attributes_response
Additional attributes associated with the use of a PayPal Wallet.

vault	
object
The details about a saved PayPal Wallet payment source.

cobranded_cards	
Array of objects [ 0 .. 25 ] items
An array of merchant cobranded cards used by buyer to complete an order. This array will be present if a merchant has onboarded their cobranded card with PayPal and provided corresponding label(s).

CopyExpand allCollapse all
{
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
}
paypal_wallet_customer
The details about a customer in PayPal's system of record.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
Email address of the customer as provided to the merchant or on file with the merchant. Email Address is required if you are processing the transaction using PayPal Guest Processing which is offered to select partners and merchants.

phone	
object
The phone number of the customer as provided to the merchant or on file with the merchant. The phone.phone_number supports only national_number.

name	
object
The full name of the customer as provided to the merchant or on file with the merchant.

merchant_customer_id	
string [ 1 .. 64 ] characters
Merchants and partners may already have a data-store where their customer information is persisted. Use merchant_customer_id to associate the PayPal-generated customer.id to your representation of a customer.

CopyExpand allCollapse all
{
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
paypal_wallet_experience_context
Customizes the payer experience during the approval process for payment with PayPal.

Note: Partners and Marketplaces might configure brand_name and shipping_preference during partner account setup, which overrides the request values.
brand_name	
string [ 1 .. 127 ] characters
The label that overrides the business name in the PayPal account on the PayPal site. The pattern is defined by an external party and supports Unicode.

shipping_preference	
string [ 1 .. 24 ] characters
Default: "GET_FROM_FILE"
The location from which the shipping address is derived.

Enum Value	Description
GET_FROM_FILE	Get the customer-provided shipping address on the PayPal site.
NO_SHIPPING	Removes the shipping address information from the API response and the Paypal site. However, the shipping.phone_number and shipping.email_address fields will still be returned to allow for digital goods delivery.
SET_PROVIDED_ADDRESS	Get the merchant-provided address. The customer cannot change this address on the PayPal site. If merchant does not pass an address, customer can choose the address on PayPal pages.
contact_preference	
string [ 1 .. 24 ] characters
Default: "NO_CONTACT_INFO"
The preference to display the contact information (buyer’s shipping email & phone number) on PayPal's checkout for easy merchant-buyer communication.

Enum Value	Description
NO_CONTACT_INFO	The merchant can opt out of showing buyer's contact information on PayPal checkout.
UPDATE_CONTACT_INFO	The merchant allows buyer to add or update shipping contact information on the PayPal checkout. Please ensure to use this updated information returned in shipping.email_address and shipping.phone_number to contact your buyers.
RETAIN_CONTACT_INFO	The buyer can only see but can not override merchant passed contact information (shipping.email_address and shipping.phone_number) on PayPal checkout. NOTE: If you don't pass the contact information, the behavior is the same as NO_CONTACT_INFO preference.
landing_page	
string [ 1 .. 13 ] characters
Default: "NO_PREFERENCE"
The type of landing page to show on the PayPal site for customer checkout.

Enum Value	Description
LOGIN	When the customer clicks PayPal Checkout, the customer is redirected to a page to log in to PayPal and approve the payment.
GUEST_CHECKOUT	When the customer clicks PayPal Checkout, the customer is redirected to a page to enter credit or debit card and other relevant billing information required to complete the purchase. This option has previously been also called as 'BILLING'
NO_PREFERENCE	When the customer clicks PayPal Checkout, the customer is redirected to either a page to log in to PayPal and approve the payment or to a page to enter credit or debit card and other relevant billing information required to complete the purchase, depending on their previous interaction with PayPal.
BILLING	DEPRECATED - please use GUEST_CHECKOUT. All implementations of 'BILLING' will be routed to 'GUEST_CHECKOUT'. When the customer clicks PayPal Checkout, the customer is redirected to a page to enter credit or debit card and other relevant billing information required to complete the purchase.
user_action	
string [ 1 .. 8 ] characters
Default: "CONTINUE"
Configures a Continue or Pay Now checkout flow.

Enum Value	Description
CONTINUE	After you redirect the customer to the PayPal payment page, a Continue button appears. Use this option when the final amount is not known when the checkout flow is initiated and you want to redirect the customer to the merchant page without processing the payment.
PAY_NOW	After you redirect the customer to the PayPal payment page, a Pay Now button appears. Use this option when the final amount is known when the checkout is initiated and you want to process the payment immediately when the customer clicks Pay Now.
payment_method_preference	
string [ 1 .. 255 ] characters
Default: "UNRESTRICTED"
The merchant-preferred payment methods.

Enum Value	Description
UNRESTRICTED	Accepts any type of payment from the customer.
IMMEDIATE_PAYMENT_REQUIRED	Accepts only immediate payment from the customer. For example, credit card, PayPal balance, or instant ACH. Ensures that at the time of capture, the payment does not have the pending status.
locale	
string [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The BCP 47-formatted locale of pages that the PayPal payment experience shows. PayPal supports a five-character code. For example, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, or zh-TW.

return_url	
string
The URL where the customer will be redirected upon approving a payment.

cancel_url	
string
The URL where the customer will be redirected upon cancelling the payment approval.

app_switch_context	
object
Merchants can use this to switch buyers from their website/application to the PayPal consumer app to review and approve the transaction.

order_update_callback_config	
object
Merchant provided Order Update callback configuration for PayPal Wallet.PayPal will call back merchant when the specified event occurs.we recommend merchants to pass both the shipping_options and shipping_address callback events. Not supported when shipping.type is specified or when 'application_context.shipping_preference' is set as 'NO_SHIPPING' or 'SET_PROVIDED_ADDRESS'.

CopyExpand allCollapse all
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"contact_preference": "NO_CONTACT_INFO",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string",
"app_switch_context": {
"native_app": {
"os_type": "ANDROID",
"os_version": "string"
},
"mobile_web": {
"return_flow": "AUTO",
"buyer_user_agent": "string"
}
},
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
}
paypal_wallet_response
The PayPal Wallet response.

account_status	
string [ 1 .. 255 ] characters
The account status indicates whether the buyer has verified the financial details associated with their PayPal account.

Enum Value	Description
VERIFIED	The buyer has completed the verification of the financial details associated with this PayPal account. For example: confirming their bank account.
UNVERIFIED	The buyer has not completed the verification of the financial details associated with this PayPal account. For example: confirming their bank account.
phone_type	
string
The phone type.

Enum Value	Description
FAX	Fax number.
HOME	Home phone number.
MOBILE	Mobile phone number.
OTHER	Other phone number.
PAGER	Pager number.
business_name	
string [ 0 .. 300 ] characters
The business name of the PayPal account holder (populated for business accounts only)

attributes	
object
Additional attributes associated with the use of a PayPal Wallet.

stored_credential	
object
Provides additional details to process a payment using the PayPal wallet billing agreement or a vaulted payment method that has been stored or is intended to be stored.

experience_status	
string [ 1 .. 255 ] characters
This field indicates the status of PayPal's Checkout experience throughout the order lifecycle. The values reflect the current stage of the checkout process.

Enum Value	Description
NOT_STARTED	PayPal checkout process has not yet begun.
IN_PROGRESS	PayPal checkout initiated. User is on the checkout page for order review before approval.
CANCELED	PayPal checkout is canceled (by closing the checkout window or clicking cancel) before the order approval.
APPROVED	Order is approved. User has completed the checkout process.
email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of the PayPal account holder.

account_id	
string = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The PayPal-assigned ID for the PayPal account holder.

name	
object
The name of the PayPal account holder. Supports only the given_name and surname properties.

phone_number	
object
The phone number, in its canonical international E.164 numbering plan format. Available only when you enable the Contact Telephone Number option in the Profile & Settings for the merchant's PayPal account. Supports only the national_number property.

birth_date	
string = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The birth date of the PayPal account holder in YYYY-MM-DD format.

tax_info	
object
The tax information of the PayPal account holder. Required only for Brazilian PayPal account holder's. Both tax_id and tax_id_type are required.

address	
object
The address of the PayPal account holder. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. Also referred to as the billing address of the customer.

CopyExpand allCollapse all
{
"account_status": "VERIFIED",
"phone_type": "FAX",
"business_name": "string",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
},
"cobranded_cards": [
{
"labels": [
"string"
],
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
},
"stored_credential": {
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
},
"experience_status": "NOT_STARTED",
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"birth_date": "stringstri",
"tax_info": {
"tax_id": "string",
"tax_id_type": "BR_CPF"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
paypal_wallet_stored_credential
Provides additional details to process a payment using the PayPal wallet billing agreement or a vaulted payment method that has been stored or is intended to be stored.

payment_initiator
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The person or party who initiated or triggered the payment.

Enum Value	Description
CUSTOMER	Payment is initiated with the active engagement of the customer. e.g. a customer checking out on a merchant website.
MERCHANT	Payment is initiated by merchant on behalf of the customer without the active engagement of customer. e.g. a merchant charging the monthly payment of a subscription to the customer.
charge_pattern	
string [ 1 .. 30 ] characters ^[A-Z0-9_]+$
DEPRECATED. Expected business/pricing model for the billing agreement, Please use usage_pattern instead.

Enum Value	Description
IMMEDIATE	On-demand instant payments – non-recurring, pre-paid, variable amount, variable frequency.
DEFERRED	Pay after use, non-recurring post-paid, variable amount, irregular frequency.
RECURRING_PREPAID	Pay upfront fixed or variable amount on a fixed date before the goods/service is delivered.
RECURRING_POSTPAID	Pay on a fixed date based on usage or consumption after the goods/service is delivered.
THRESHOLD_PREPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, before the goods/service is delivered.
THRESHOLD_POSTPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, after the goods/service is delivered.
SUBSCRIPTION_PREPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due before the good/service is delivered.
SUBSCRIPTION_POSTPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due after the goods/services are delivered.
UNSCHEDULED_PREPAID	Unscheduled card on file plan where the merchant can bill buyer upfront based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
UNSCHEDULED_POSTPAID	Unscheduled card on file plan where the merchant can bill buyer based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
INSTALLMENT_PREPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due before the good/service is delivered.
INSTALLMENT_POSTPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due after the goods/services are delivered.
usage_pattern	
string [ 1 .. 30 ] characters ^[A-Z0-9_]+$
Expected business/pricing model for the billing agreement.

Enum Value	Description
IMMEDIATE	On-demand instant payments – non-recurring, pre-paid, variable amount, variable frequency.
DEFERRED	Pay after use, non-recurring post-paid, variable amount, irregular frequency.
RECURRING_PREPAID	Pay upfront fixed or variable amount on a fixed date before the goods/service is delivered.
RECURRING_POSTPAID	Pay on a fixed date based on usage or consumption after the goods/service is delivered.
THRESHOLD_PREPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, before the goods/service is delivered.
THRESHOLD_POSTPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, after the goods/service is delivered.
SUBSCRIPTION_PREPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due before the good/service is delivered.
SUBSCRIPTION_POSTPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due after the goods/services are delivered.
UNSCHEDULED_PREPAID	Unscheduled card on file plan where the merchant can bill buyer upfront based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
UNSCHEDULED_POSTPAID	Unscheduled card on file plan where the merchant can bill buyer based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
INSTALLMENT_PREPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due before the good/service is delivered.
INSTALLMENT_POSTPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due after the goods/services are delivered.
usage	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "DERIVED"
Indicates if this is a first or subsequent payment using a stored payment source (also referred to as stored credential or card on file).

Enum Value	Description
FIRST	Indicates the Initial/First payment with a payment_source that is intended to be stored upon successful processing of the payment.
SUBSEQUENT	Indicates a payment using a stored payment_source which has been successfully used previously for a payment.
DERIVED	Indicates that PayPal will derive the value of FIRST or SUBSEQUENT based on data available to PayPal.
Copy
{
"payment_initiator": "CUSTOMER",
"charge_pattern": "IMMEDIATE",
"usage_pattern": "IMMEDIATE",
"usage": "FIRST"
}
paypal_wallet_vault_response
The details about a saved PayPal Wallet payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
O estado do cofre.

Valor de enumeração	Descrição
ABOBADADO	A forma de pagamento foi salva no cofre do seu cliente. O status deste cofre reflete /v3/vaulta situação atual.
CRIADO	OBSOLETO. A forma de pagamento foi salva no cofre do seu cliente. Este status se aplica a padrões de integração obsoletos e não será retornado para integrações v3/cofre.
APROVADO	O cliente aprovou a ação de salvar a fonte de pagamento especificada em seu cofre. Use v3/vault/payment-tokens com o setup_token fornecido para salvar a fonte de pagamento no cofre.

links
Matriz de objetos [ 1 .. 10 ] itens
Uma série de links HATEOAS relacionados a solicitações.


cliente
objeto
Os dados pessoais de um cliente registrados no sistema do PayPal.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
},
"merchant_customer_id": "string"
}
}
Telefone
O número de telefone, em seu formato canônico internacional de numeração E.164 .

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"national_number": "string"
}
Telefone
O número de telefone, em seu formato canônico internacional de numeração E.164 .

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"national_number": "string"
}
Telefone
O número de telefone em seu formato canônico internacional de numeração E.164 .

código_do_país
string [ 1 .. 3 ] caracteres ^[0-9]{1,3}?$
O código de chamada do país (CC), em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do CC e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"country_code": "str",
"national_number": "string"
}
Telefone
O número de telefone em seu formato canônico internacional de numeração E.164 .

código_do_país
string [ 1 .. 3 ] caracteres ^[0-9]{1,3}?$
O código de chamada do país (CC), em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do CC e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"country_code": "str",
"national_number": "string"
}
Telefone
O número de telefone, em seu formato canônico internacional de numeração E.164 .

código_do_país
obrigatório
string [ 1 .. 3 ] caracteres ^[0-9]{1,3}?$
O código de chamada do país (CC), em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do CC e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"country_code": "str",
"national_number": "string"
}
Telefone
O número de telefone, em seu formato canônico internacional de numeração E.164 .

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"national_number": "string"
}
Telefone
O número de telefone em seu formato canônico internacional de numeração E.164 .

código_do_país
string [ 1 .. 3 ] caracteres ^[0-9]{1,3}?$
O código de chamada do país (CC), em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do CC e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"country_code": "str",
"national_number": "string"
}
Número de telefone
O número de telefone em seu formato canônico internacional de numeração E.164 .

número_nacional
obrigatório
string [ 1 .. 14 ] caracteres ^[0-9]{1,14}?$
O número nacional, em seu formato canônico do plano de numeração internacional E.164 . O comprimento combinado do código de chamada do país (CC) e do número nacional não deve ser superior a 15 dígitos. O número nacional consiste em um código de destino nacional (NDC) e um número de assinante (SN).

Cópia
{
"national_number": "string"
}
Tipo de telefone
O tipo de telefone.

corda
O tipo de telefone.

Valor de enumeração	Descrição
FAX	Número de fax.
LAR	Número de telefone residencial.
MÓVEL	Número de telefone celular.
OUTRO	Outro número de telefone.
PAGER	Número do pager.
Cópia
"FAX"
telefone_com_tipo
As informações do telefone.

tipo_de_telefone
corda
O tipo de telefone.

Valor de enumeração	Descrição
FAX	Número de fax.
LAR	Número de telefone residencial.
MÓVEL	Número de telefone celular.
OUTRO	Outro número de telefone.
PAGER	Número do pager.

número_de_telefone
obrigatório
objeto
O número de telefone, em seu formato canônico internacional de numeração E.164 . Suporta apenas a national_numberpropriedade.

CópiaExpandir tudoRecolher tudo
{
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
}
taxa_da_plataforma
A taxa da plataforma ou do parceiro, comissão ou taxa de corretagem associada à transação. Não se trata de uma etapa de transação separada ou isolada do ponto de vista externo. A taxa da plataforma tem um escopo limitado e está sempre associada ao pagamento original da unidade adquirida.


quantia
obrigatório
objeto
A taxa para esta transação.


beneficiário
objeto
O destinatário da taxa referente a esta transação. Caso este valor seja omitido, o padrão será o solicitante da API.

CópiaExpandir tudoRecolher tudo
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
Endereço Postal Portátil (Granulação Média)
Endereço postal internacional portátil. Mapeia para AddressValidationMetadata e controles de formulário de preenchimento automático HTML 5.1 : o atributo autocomplete .

Endereço Linha 1
string <= 300 caracteres
A primeira linha do endereço, como número e rua, por exemplo 173 Drury Lane. Necessária para entrada de dados e verificações de conformidade e risco. Este campo deve conter o endereço completo.

endereço_linha_2
string <= 300 caracteres
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
Portable Postal Address (Medium-Grained)
The portable international postal address. Maps to AddressValidationMetadata and HTML 5.1 Autofilling form controls: the autocomplete attribute.

address_line_1	
string <= 300 characters
The first line of the address, such as number and street, for example, 173 Drury Lane. Needed for data entry, and Compliance and Risk checks. This field needs to pass the full address.

address_line_2	
string <= 300 characters
The second line of the address, for example, a suite or apartment number.

admin_area_2	
string <= 120 characters
A city, town, or village. Smaller than admin_area_level_1.

admin_area_1	
string <= 300 characters
The highest-level sub-division in a country, which is usually a province, state, or ISO-3166-2 subdivision. This data is formatted for postal delivery, for example, CA and not California. Value, by country, is:

UK. A county.
US. A state.
Canada. A province.
Japan. A prefecture.
Switzerland. A kanton.
postal_code	
string <= 60 characters
The postal code, which is the ZIP code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The 2-character ISO 3166-1 code that identifies the country or region.

Note: The country code for Great Britain is GB and not UK as used in the top-level domain names for that country. Use the C2 country code for China worldwide for comparable uncontrolled price (CUP) method, bank card, and cross-border transactions.
Copy
{
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
pricing_scheme
The pricing scheme details.

pricing_model
required
string [ 1 .. 24 ] characters
The pricing model for the billing cycle.

Enum Value	Description
FIXED	A fixed pricing scheme where the customer is charged a fixed amount.
VARIABLE	A variable pricing scheme where the customer is charged a variable amount.
AUTO_RELOAD	A auto-reload pricing scheme where the customer is charged a fixed amount for reload.
price	
object
The price the customer will be charged based on the pricing model

reload_threshold_amount	
object
The threshold amount on which the reload charge would be triggered. This will be associated with the account-balance where if the account-balance goes below this amount then customer would incur reload charge.

CopyExpand allCollapse all
{
"pricing_model": "FIXED",
"price": {
"currency_code": "str",
"value": "string"
},
"reload_threshold_amount": {
"currency_code": "str",
"value": "string"
}
}
processor_response
The processor response information for payment requests, such as direct credit card transactions.

avs_code	
string
The address verification code for Visa, Discover, Mastercard, or American Express transactions.

Enum Value	Description
0	For Maestro, all address information matches.
1	For Maestro, none of the address information matches.
2	For Maestro, part of the address information matches.
3	For Maestro, the merchant did not provide AVS information. It was not processed.
4	For Maestro, the address was not checked or the acquirer had no response. The service is not available.
A	For Visa, Mastercard, or Discover transactions, the address matches but the zip code does not match. For American Express transactions, the card holder address is correct.
B	For Visa, Mastercard, or Discover transactions, the address matches. International A.
C	For Visa, Mastercard, or Discover transactions, no values match. International N.
D	For Visa, Mastercard, or Discover transactions, the address and postal code match. International X.
E	For Visa, Mastercard, or Discover transactions, not allowed for Internet or phone transactions. For American Express card holder, the name is incorrect but the address and postal code match.
F	For Visa, Mastercard, or Discover transactions, the address and postal code match. UK-specific X. For American Express card holder, the name is incorrect but the address matches.
G	For Visa, Mastercard, or Discover transactions, global is unavailable. Nothing matches.
I	For Visa, Mastercard, or Discover transactions, international is unavailable. Not applicable.
M	For Visa, Mastercard, or Discover transactions, the address and postal code match. For American Express card holder, the name, address, and postal code match.
N	For Visa, Mastercard, or Discover transactions, nothing matches. For American Express card holder, the address and postal code are both incorrect.
P	For Visa, Mastercard, or Discover transactions, postal international Z. Postal code only.
R	For Visa, Mastercard, or Discover transactions, re-try the request. For American Express, the system is unavailable.
S	For Visa, Mastercard, Discover, or American Express, the service is not supported.
U	For Visa, Mastercard, or Discover transactions, the service is unavailable. For American Express, information is not available. For Maestro, the address is not checked or the acquirer had no response. The service is not available.
W	For Visa, Mastercard, or Discover transactions, whole ZIP code. For American Express, the card holder name, address, and postal code are all incorrect.
X	For Visa, Mastercard, or Discover transactions, exact match of the address and the nine-digit ZIP code. For American Express, the card holder name, address, and postal code are all incorrect.
Y	For Visa, Mastercard, or Discover transactions, the address and five-digit ZIP code match. For American Express, the card holder address and postal code are both correct.
Z	For Visa, Mastercard, or Discover transactions, the five-digit ZIP code matches but no address. For American Express, only the card holder postal code is correct.
Null	For Maestro, no AVS response was obtained.
cvv_code	
string
The card verification value code for for Visa, Discover, Mastercard, or American Express.

Enum Value	Description
0	For Maestro, the CVV2 matched.
1	For Maestro, the CVV2 did not match.
2	For Maestro, the merchant has not implemented CVV2 code handling.
3	For Maestro, the merchant has indicated that CVV2 is not present on card.
4	For Maestro, the service is not available.
E	For Visa, Mastercard, Discover, or American Express, error - unrecognized or unknown response.
I	For Visa, Mastercard, Discover, or American Express, invalid or null.
M	For Visa, Mastercard, Discover, or American Express, the CVV2/CSC matches.
N	For Visa, Mastercard, Discover, or American Express, the CVV2/CSC does not match.
P	For Visa, Mastercard, Discover, or American Express, it was not processed.
S	For Visa, Mastercard, Discover, or American Express, the service is not supported.
U	For Visa, Mastercard, Discover, or American Express, unknown - the issuer is not certified.
X	For Visa, Mastercard, Discover, or American Express, no response. For Maestro, the service is not available.
All others	For Visa, Mastercard, Discover, or American Express, error.
response_code	
string
Processor response code for the non-PayPal payment processor errors.

Enum Value	Description
1000	PARTIAL_AUTHORIZATION.
1300	INVALID_DATA_FORMAT.
1310	INVALID_AMOUNT.
1312	INVALID_TRANSACTION_CARD_ISSUER_ACQUIRER.
1317	INVALID_CAPTURE_DATE.
1320	INVALID_CURRENCY_CODE.
1330	INVALID_ACCOUNT.
1335	INVALID_ACCOUNT_RECURRING.
1340	INVALID_TERMINAL.
1350	INVALID_MERCHANT.
1352	RESTRICTED_OR_INACTIVE_ACCOUNT.
1360	BAD_PROCESSING_CODE.
1370	INVALID_MCC.
1380	INVALID_EXPIRATION.
1382	INVALID_CARD_VERIFICATION_VALUE.
1384	INVALID_LIFE_CYCLE_OF_TRANSACTION.
1390	INVALID_ORDER.
1393	TRANSACTION_CANNOT_BE_COMPLETED.
5100	GENERIC_DECLINE.
5110	CVV2_FAILURE.
5120	INSUFFICIENT_FUNDS.
5130	INVALID_PIN.
5135	DECLINED_PIN_TRY_EXCEEDED.
5140	CARD_CLOSED.
5150	PICKUP_CARD_SPECIAL_CONDITIONS. Try using another card. Do not retry the same card.
5160	UNAUTHORIZED_USER.
5170	AVS_FAILURE.
5180	INVALID_OR_RESTRICTED_CARD. Try using another card. Do not retry the same card.
5190	SOFT_AVS.
5200	DUPLICATE_TRANSACTION.
5210	INVALID_TRANSACTION.
5400	EXPIRED_CARD.
5500	INCORRECT_PIN_REENTER.
5650	DECLINED_SCA_REQUIRED.
5700	TRANSACTION_NOT_PERMITTED. Outside of scope of accepted business.
5710	TX_ATTEMPTS_EXCEED_LIMIT.
5800	REVERSAL_REJECTED.
5900	INVALID_ISSUE.
5910	ISSUER_NOT_AVAILABLE_NOT_RETRIABLE.
5920	ISSUER_NOT_AVAILABLE_RETRIABLE.
5930	CARD_NOT_ACTIVATED.
5950	DECLINED_DUE_TO_UPDATED_ACCOUNT. External decline as an updated card has been issued.
6300	ACCOUNT_NOT_ON_FILE.
7600	APPROVED_NON_CAPTURE.
7700	ERROR_3DS.
7710	AUTHENTICATION_FAILED.
7800	BIN_ERROR.
7900	PIN_ERROR.
8000	PROCESSOR_SYSTEM_ERROR.
8010	HOST_KEY_ERROR.
8020	CONFIGURATION_ERROR.
8030	UNSUPPORTED_OPERATION.
8100	FATAL_COMMUNICATION_ERROR.
8110	RETRIABLE_COMMUNICATION_ERROR.
8220	SYSTEM_UNAVAILABLE.
9100	DECLINED_PLEASE_RETRY. Retry.
9500	SUSPECTED_FRAUD. Try using another card. Do not retry the same card.
9510	SECURITY_VIOLATION.
9520	LOST_OR_STOLEN. Try using another card. Do not retry the same card.
9530	HOLD_CALL_CENTER. The merchant must call the number on the back of the card. POS scenario.
9540	REFUSED_CARD.
9600	UNRECOGNIZED_RESPONSE_CODE.
0000	APPROVED.
00N7	CVV2_FAILURE_POSSIBLE_RETRY_WITH_CVV.
0100	REFERRAL.
0390	ACCOUNT_NOT_FOUND.
0500	DO_NOT_HONOR.
0580	UNAUTHORIZED_TRANSACTION.
0800	BAD_RESPONSE_REVERSAL_REQUIRED.
0880	CRYPTOGRAPHIC_FAILURE.
0890	UNACCEPTABLE_PIN.
0960	SYSTEM_MALFUNCTION.
0R00	CANCELLED_PAYMENT.
10BR	ISSUER_REJECTED.
PCNR	CONTINGENCIES_NOT_RESOLVED.
PCVV	CVV_FAILURE.
PP06	ACCOUNT_CLOSED. A previously open account is now closed
PPRN	REATTEMPT_NOT_PERMITTED.
PPAD	BILLING_ADDRESS.
PPAB	ACCOUNT_BLOCKED_BY_ISSUER.
PPAE	AMEX_DISABLED.
PPAG	ADULT_GAMING_UNSUPPORTED.
PPAI	AMOUNT_INCOMPATIBLE.
PPAR	AUTH_RESULT.
PPAU	MCC_CODE.
PPAV	ARC_AVS.
PPAX	AMOUNT_EXCEEDED.
PPBG	BAD_GAMING.
PPC2	ARC_CVV.
PPCE	CE_REGISTRATION_INCOMPLETE.
PPCO	COUNTRY.
PPCR	CREDIT_ERROR.
PPCT	CARD_TYPE_UNSUPPORTED.
PPCU	CURRENCY_USED_INVALID.
PPD3	SECURE_ERROR_3DS.
PPDC	DCC_UNSUPPORTED.
PPDI	DINERS_REJECT.
PPDV	AUTH_MESSAGE.
PPDT	DECLINE_THRESHOLD_BREACH.
PPEF	EXPIRED_FUNDING_INSTRUMENT.
PPEL	EXCEEDS_FREQUENCY_LIMIT.
PPER	INTERNAL_SYSTEM_ERROR.
PPEX	EXPIRY_DATE.
PPFE	FUNDING_SOURCE_ALREADY_EXISTS.
PPFI	INVALID_FUNDING_INSTRUMENT.
PPFR	RESTRICTED_FUNDING_INSTRUMENT.
PPFV	FIELD_VALIDATION_FAILED.
PPGR	GAMING_REFUND_ERROR.
PPH1	H1_ERROR.
PPIF	IDEMPOTENCY_FAILURE.
PPII	INVALID_INPUT_FAILURE.
PPIM	ID_MISMATCH.
PPIT	INVALID_TRACE_ID.
PPLR	LATE_REVERSAL.
PPLS	LARGE_STATUS_CODE.
PPMB	MISSING_BUSINESS_RULE_OR_DATA.
PPMC	BLOCKED_Mastercard.
PPMD	DEPRECATED The PPMD value has been deprecated.
PPNC	NOT_SUPPORTED_NRC.
PPNL	EXCEEDS_NETWORK_FREQUENCY_LIMIT.
PPNM	NO_MID_FOUND.
PPNT	NETWORK_ERROR.
PPPH	NO_PHONE_FOR_DCC_TRANSACTION.
PPPI	INVALID_PRODUCT.
PPPM	INVALID_PAYMENT_METHOD.
PPQC	QUASI_CASH_UNSUPPORTED.
PPRE	UNSUPPORT_REFUND_ON_PENDING_BC.
PPRF	INVALID_PARENT_TRANSACTION_STATUS.
PPRR	MERCHANT_NOT_REGISTERED.
PPS0	BANKAUTH_ROW_MISMATCH.
PPS1	BANKAUTH_ROW_SETTLED.
PPS2	BANKAUTH_ROW_VOIDED.
PPS3	BANKAUTH_EXPIRED.
PPS4	CURRENCY_MISMATCH.
PPS5	CREDITCARD_MISMATCH.
PPS6	AMOUNT_MISMATCH.
PPSC	ARC_SCORE.
PPSD	STATUS_DESCRIPTION.
PPSE	AMEX_DENIED.
PPTE	VERIFICATION_TOKEN_EXPIRED.
PPTF	INVALID_TRACE_REFERENCE.
PPTI	INVALID_TRANSACTION_ID.
PPTR	VERIFICATION_TOKEN_REVOKED.
PPTT	TRANSACTION_TYPE_UNSUPPORTED.
PPTV	INVALID_VERIFICATION_TOKEN.
PPUA	USER_NOT_AUTHORIZED.
PPUC	CURRENCY_CODE_UNSUPPORTED.
PPUE	UNSUPPORT_ENTITY.
PPUI	UNSUPPORT_INSTALLMENT.
PPUP	UNSUPPORT_POS_FLAG.
PPUR	UNSUPPORTED_REVERSAL.
PPVC	VALIDATE_CURRENCY.
PPVE	VALIDATION_ERROR.
PPVT	VIRTUAL_TERMINAL_UNSUPPORTED.
payment_advice_code	
string
The declined payment transactions might have payment advice codes. The card networks, like Visa and Mastercard, return payment advice codes.

Enum Value	Description
21	For Mastercard, the card holder has been unsuccessful at canceling recurring payment through merchant. Stop recurring payment requests. For Visa, all recurring payments were canceled for the card number requested. Stop recurring payment requests.
22	For Mastercard, merchant does not qualify for product code.
24	For Mastercard, retry after 1 hour.
25	For Mastercard, retry after 24 hours.
26	For Mastercard, retry after 2 days.
27	For Mastercard, retry after 4 days.
28	For Mastercard, retry after 6 days.
29	For Mastercard, retry after 8 days.
30	For Mastercard, retry after 10 days .
40	For Mastercard, consumer non-reloadable prepaid card.
43	For Mastercard, consumer multi-use virtual card number.
01	For Mastercard, expired card account upgrade or portfolio sale conversion. Obtain new account information before next billing cycle.
02	For Mastercard, over credit limit or insufficient funds. Retry the transaction 72 hours later. For Visa, the card holder wants to stop only one specific payment in the recurring payment relationship. The merchant must NOT resubmit the same transaction. The merchant can continue the billing process in the subsequent billing period.
03	For Mastercard, account closed as fraudulent. Obtain another type of payment from customer due to account being closed or fraud. Possible reason: Account closed as fraudulent. For Visa, the card holder wants to stop all recurring payment transactions for a specific merchant. Stop recurring payment requests.
04	For Mastercard, token requirements not fulfilled for this token type.
Copy
{
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
Purchase Unit
The purchase unit details. Used to capture required information for the payment contract.

reference_id	
string [ 1 .. 256 ] characters
The API caller-provided external ID for the purchase unit. Required for multiple purchase units when you must update the order through PATCH. If you omit this value and the order contains only one purchase unit, PayPal sets this value to default.

Note: If there are multiple purchase units, reference_id is required for each purchase unit.
description	
string [ 1 .. 127 ] characters
The purchase description.

custom_id	
string [ 1 .. 255 ] characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice ID for this order.

id	
string [ 1 .. 19 ] characters
The PayPal-generated ID for the purchase unit. This ID appears in both the payer's transaction history and the emails that the payer receives. In addition, this ID is available in transaction and settlement reports that merchants and API callers can use to reconcile transactions. This ID is only available when an order is saved by calling v2/checkout/orders/id/save.

soft_descriptor	
string [ 1 .. 22 ] characters
The payment descriptor on account transactions on the customer's credit card statement, that PayPal sends to processors. The maximum length of the soft descriptor information that you can pass in the API field is 22 characters, in the following format:22 - len(PAYPAL * (8)) - len(Descriptor in Payment Receiving Preferences of Merchant account + 1)The PAYPAL prefix uses 8 characters.

The soft descriptor supports the following ASCII characters:

Alphanumeric characters
Dashes
Asterisks
Periods (.)
Spaces
For Wallet payments marketplace integrations:
The merchant descriptor in the Payment Receiving Preferences must be the marketplace name.
You can't use the remaining space to show the customer service number.
The remaining spaces can be a combination of seller name and country.

For unbranded payments (Direct Card) marketplace integrations, use a combination of the seller name and phone number.
items	
Array of objects
An array of items that the customer purchases from the merchant.

most_recent_errors	
Array of any [ 1 .. 10 ] items
The error reason code and description that are the reason for the most recent order decline.

amount	
object
The total order amount with an optional breakdown that provides details, such as the total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.
If you specify amount.breakdown, the amount equals item_total plus tax_total plus shipping plus handling plus insurance minus shipping_discount minus discount.
The amount must be a positive number. For listed of supported currencies and decimal precision, see the PayPal REST APIs Currency Codes.

payee	
object
The merchant who receives payment for this transaction.

payment_instruction	
object
Any additional payment instructions to be consider during payment processing. This processing instruction is applicable for Capturing an order or Authorizing an Order.

shipping	
object
The shipping address and method.

supplementary_data	
object
Supplementary data about this payment. Merchants and partners can add Level 2 and 3 data to payments to reduce risk and payment processing costs. For more information about processing payments, see checkout or multiparty checkout.

payments	
object
The comprehensive history of payments for the purchase unit.

CopyExpand allCollapse all
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": null,
"value": null
},
"reload_threshold_amount": {
"currency_code": null,
"value": null
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"most_recent_errors": [
null
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"trackers": [
{
"id": "string",
"status": "CANCELLED",
"items": [
{
"name": "string",
"quantity": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
}
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"create_time": "string",
"update_time": "string"
}
]
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
null
],
"name": "string",
"setup_fee": {
"currency_code": null,
"value": null
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
},
"payments": {
"authorizations": [
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"expiration_time": "string",
"create_time": "string",
"update_time": "string",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"captures": [
{
"create_time": "string",
"update_time": "string",
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"final_capture": false,
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
},
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
},
"seller_receivable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": null,
"value": null
},
"payee": {
"email_address": null,
"merchant_id": null
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"receivable_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
},
"disbursement_mode": "INSTANT",
"processor_response": {
"avs_code": "A",
"cvv_code": "E",
"response_code": "0000",
"payment_advice_code": "01"
}
}
],
"refunds": [
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"acquirer_reference_number": "string",
"note_to_payer": "string",
"seller_payable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": null,
"value": null
},
"payee": {
"email_address": null,
"merchant_id": null
}
}
],
"net_amount_breakdown": [
{
"payable_amount": {
"currency_code": null,
"value": null
},
"converted_amount": {
"currency_code": null,
"value": null
},
"exchange_rate": {
"value": null,
"source_currency": null,
"target_currency": null
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"net_amount_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"total_refunded_amount": {
"currency_code": "str",
"value": "string"
}
},
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"payer": {
"email_address": "string",
"merchant_id": "string"
},
"create_time": "string",
"update_time": "string"
}
]
}
}
Purchase Unit Request
The purchase unit request. Includes required information for the payment contract.

reference_id	
string [ 1 .. 256 ] characters
The API caller-provided external ID for the purchase unit. Required for multiple purchase units when you must update the order through PATCH. If you omit this value and the order contains only one purchase unit, PayPal sets this value to default.

description	
string [ 1 .. 3000 ] characters
This field supports up to 3,000 characters, but any content beyond 127 characters (including spaces) will be truncated. The 127 character limit is reflected in the response representation of this field.
The purchase description. The maximum length of the character is dependent on the type of characters used. The character length is specified assuming a US ASCII character. Depending on type of character; (e.g. accented character, Japanese characters) the number of characters that that can be specified as input might not equal the permissible max length.
custom_id	
string [ 1 .. 255 ] characters
The API caller-provided external ID. Used to reconcile client transactions with PayPal transactions. Appears in transaction and settlement reports but is not visible to the payer.

invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives. invoice_id values are required to be unique within each merchant account by default. Although the uniqueness validation is configurable, disabling this behavior will remove the account's ability to use invoice_id in other APIs as an identifier. It is highly recommended to keep a unique invoice_id for each Order.

soft_descriptor	
string [ 1 .. 1000 ] characters
This field supports up to 127 characters, but any content beyond 22 characters (including spaces) will be truncated. The 22 character limit is reflected in the response representation of this field.
The soft descriptor is the dynamic text used to construct the statement descriptor that appears on a payer's card statement.

If an Order is paid using the "PayPal Wallet", the statement descriptor will appear in following format on the payer's card statement: PAYPAL_prefix+(space)+merchant_descriptor+(space)+ soft_descriptor
Note: The merchant descriptor is the descriptor of the merchant’s payment receiving preferences which can be seen by logging into the merchant account https://www.sandbox.paypal.com/businessprofile/settings/info/edit
The PAYPAL prefix uses 8 characters. Only the first 22 characters will be displayed in the statement.
For example, if:
The PayPal prefix toggle is PAYPAL *.
The merchant descriptor in the profile is Janes Gift.
The soft descriptor is 800-123-1234.
Then, the statement descriptor on the card is PAYPAL * Janes Gift 80.
items	
Array of objects
An array of items that the customer purchases from the merchant.

amount
required
object
The total order amount with an optional breakdown that provides details, such as the total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.
If you specify amount.breakdown, the amount equals item_total plus tax_total plus shipping plus handling plus insurance minus shipping_discount minus discount.
The amount must be a positive number. The amount.value field supports up to 15 digits preceding the decimal. For a list of supported currencies, decimal precision, and maximum charge amount, see the PayPal REST APIs Currency Codes.

payee	
object
The merchant who receives payment for this transaction.

payment_instruction	
object
Any additional payment instructions to be consider during payment processing. This processing instruction is applicable for Capturing an order or Authorizing an Order.

shipping	
object
The name and address of the person to whom to ship the items.

supplementary_data	
object
Contains Supplementary Data.

CopyExpand allCollapse all
{
"reference_id": "string",
"description": "string",
"custom_id": "string",
"invoice_id": "string",
"soft_descriptor": "string",
"items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"category": "DIGITAL_GOODS",
"image_url": "http://example.com",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": "REGULAR",
"total_cycles": 1,
"sequence": 1,
"pricing_scheme": {
"pricing_model": "FIXED",
"price": {
"currency_code": null,
"value": null
},
"reload_threshold_amount": {
"currency_code": null,
"value": null
}
},
"start_date": "string"
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
}
}
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"payee_pricing_tier_id": "string",
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"shipping": {
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
},
"supplementary_data": {
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
null
],
"name": "string",
"setup_fee": {
"currency_code": null,
"value": null
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
}
}
refund
The refund information.

status	
string
The status of the refund.

Enum Value	Description
CANCELLED	The refund was cancelled.
FAILED	The refund could not be processed.
PENDING	The refund is pending. For more information, see status_details.reason.
COMPLETED	The funds for this transaction were debited to the customer's account.
status_details	
object
The details of the refund status.

id	
string
The PayPal-generated ID for the refund.

invoice_id	
string
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

custom_id	
string [ 1 .. 255 ] characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

acquirer_reference_number	
string [ 1 .. 36 ] characters
Reference ID issued for the card transaction. This ID can be used to track the transaction across processors, card brands and issuing banks.

note_to_payer	
string
The reason for the refund. Appears in both the payer's transaction history and the emails that the payer receives.

seller_payable_breakdown	
object
The breakdown of the refund.

links	
Array of objects
An array of related HATEOAS links.

amount	
object
The amount that the payee refunded to the payer.

payer	
object
The details associated with the merchant for this transaction.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

CopyExpand allCollapse all
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
},
"id": "string",
"invoice_id": "string",
"custom_id": "string",
"acquirer_reference_number": "string",
"note_to_payer": "string",
"seller_payable_breakdown": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"net_amount_breakdown": [
{
"payable_amount": {
"currency_code": "str",
"value": "string"
},
"converted_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"net_amount_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"total_refunded_amount": {
"currency_code": "str",
"value": "string"
}
},
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"amount": {
"currency_code": "str",
"value": "string"
},
"payer": {
"email_address": "string",
"merchant_id": "string"
},
"create_time": "string",
"update_time": "string"
}
refund_status
The refund status with details.

status	
string
The status of the refund.

Enum Value	Description
CANCELLED	The refund was cancelled.
FAILED	The refund could not be processed.
PENDING	The refund is pending. For more information, see status_details.reason.
COMPLETED	The funds for this transaction were debited to the customer's account.
status_details	
object
The details of the refund status.

CopyExpand allCollapse all
{
"status": "CANCELLED",
"status_details": {
"reason": "ECHECK"
}
}
refund_status_details
The details of the refund status.

reason	
string
The reason why the refund has the PENDING or FAILED status.

Value	Description
ECHECK	The customer's account is funded through an eCheck, which has not yet cleared.
Copy
{
"reason": "ECHECK"
}
Risk Assessment Status
The risk assessment status.

string [ 1 .. 255 ] characters ^[A-Z_]+$
The risk assessment status.

Enum Value	Description
ALLOW	Fraud Protection allowed this payment.
DENY	Fraud Protection denied this payment.
PENDING	Payment is in Fraud Protection Review Queue.
Copy
"ALLOW"
risk_supplementary_data
Additional information necessary to evaluate the risk profile of a transaction.

customer	
object
Profile information of the sender or receiver.

CopyExpand allCollapse all
{
"customer": {
"ip_address": "string"
}
}
Seller Receivable Breakdown
The detailed breakdown of the capture activity. This is not available for transactions that are in pending state.

platform_fees	
Array of objects [ 0 .. 1 ] items
An array of platform or partner fees, commissions, or brokerage fees that associated with the captured payment.

gross_amount
required
object
The amount for this captured payment in the currency of the transaction.

paypal_fee	
object
The applicable fee for this captured payment in the currency of the transaction.

paypal_fee_in_receivable_currency	
object
The applicable fee for this captured payment in the receivable currency. Returned only in cases the fee is charged in the receivable currency. Example 'CNY'.

net_amount	
object
The net amount that the payee receives for this captured payment in their PayPal account. The net amount is computed as gross_amount minus the paypal_fee minus the platform_fees.

receivable_amount	
object
The net amount that is credited to the payee's PayPal account. Returned only when the currency of the captured payment is different from the currency of the PayPal account where the payee wants to credit the funds. The amount is computed as net_amount times exchange_rate.

exchange_rate	
object
The exchange rate that determines the amount that is credited to the payee's PayPal account. Returned when the currency of the captured payment is different from the currency of the PayPal account where the payee wants to credit the funds.

CopyExpand allCollapse all
{
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
],
"gross_amount": {
"currency_code": "str",
"value": "string"
},
"paypal_fee": {
"currency_code": "str",
"value": "string"
},
"paypal_fee_in_receivable_currency": {
"currency_code": "str",
"value": "string"
},
"net_amount": {
"currency_code": "str",
"value": "string"
},
"receivable_amount": {
"currency_code": "str",
"value": "string"
},
"exchange_rate": {
"value": "string",
"source_currency": "str",
"target_currency": "str"
}
}
seller_protection
The level of protection offered as defined by PayPal Seller Protection for Merchants.

status	
string
Indicates whether the transaction is eligible for seller protection. For information, see PayPal Seller Protection for Merchants.

Enum Value	Description
ELIGIBLE	Your PayPal balance remains intact if the customer claims that they did not receive an item or the account holder claims that they did not authorize the payment.
PARTIALLY_ELIGIBLE	Your PayPal balance remains intact if the customer claims that they did not receive an item.
NOT_ELIGIBLE	This transaction is not eligible for seller protection.
dispute_categories	
Array of strings
An array of conditions that are covered for the transaction.

CopyExpand allCollapse all
{
"status": "ELIGIBLE",
"dispute_categories": [
"string"
]
}
Shipping Option and Amount data at Purchase Unit
This would contain shipping option and amount data at purchase unit level.

reference_id	
string [ 1 .. 256 ] characters
The API caller-provided external ID for the purchase unit. Required for multiple purchase units when you must update the order through PATCH. If you omit this value and the order contains only one purchase unit, PayPal sets this value to default.

Note: If there are multiple purchase units, reference_id is required for each purchase unit.
shipping_options	
Array of objects [ 1 .. 10 ] items
An array of shipping options that the payee or merchant offers to the payer to ship or pick up their items.

amount	
object
The total order amount with an optional breakdown that provides details, such as the total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.

CopyExpand allCollapse all
{
"reference_id": "string",
"shipping_options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"amount": {
"currency_code": "str",
"value": "string",
"breakdown": {
"item_total": {
"currency_code": "str",
"value": "string"
},
"shipping": {
"currency_code": "str",
"value": "string"
},
"handling": {
"currency_code": "str",
"value": "string"
},
"tax_total": {
"currency_code": "str",
"value": "string"
},
"insurance": {
"currency_code": "str",
"value": "string"
},
"shipping_discount": {
"currency_code": "str",
"value": "string"
},
"discount": {
"currency_code": "str",
"value": "string"
}
}
}
}
shipping_detail
The shipping details.

type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
A classification for the method of purchase fulfillment (e.g shipping, in-store pickup, etc). Either type or options may be present, but not both.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP_IN_PERSON	DEPRECATED. Please use "PICKUP_FROM_PERSON" instead.
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
options	
Array of objects [ 0 .. 10 ] items
An array of shipping options that the payee or merchant offers to the payer to ship or pick up their items.

name	
object
The name of the person to whom to ship the items. Supports only the full_name property.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery.

phone_number	
object
The phone number of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery. [Format - canonical international E.164 numbering plan]

address	
object
The address of the person to whom to ship the items. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. admin_area_1 is required for addresses located in Argentina, Brazil, China, Canada, India, Indonesia, Japan, Mexico, Thailand, and the United States.

CopyExpand allCollapse all
{
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
shipping_detail
The shipping details.

type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
A classification for the method of purchase fulfillment (e.g shipping, in-store pickup, etc). Either type or options may be present, but not both.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP_IN_PERSON	DEPRECATED. Please use "PICKUP_FROM_PERSON" instead.
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
name	
object
The name of the person to whom to ship the items. Supports only the full_name property.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery.

phone_number	
object
The phone number of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery. [Format - canonical international E.164 numbering plan]

address	
object
The address of the person to whom to ship the items. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. admin_area_1 is required for addresses located in Argentina, Brazil, China, Canada, India, Indonesia, Japan, Mexico, Thailand, and the United States.

CopyExpand allCollapse all
{
"type": "SHIPPING",
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
shipping_option
The options that the payee or merchant offers to the payer to ship or pick up their items.

id
required
string <= 127 characters
A unique ID that identifies a payer-selected shipping option.

label
required
string <= 127 characters
A description that the payer sees, which helps them choose an appropriate shipping option. For example, Free Shipping, USPS Priority Shipping, Expédition prioritaire USPS, or USPS yōuxiān fā huò. Localize this description to the payer's locale.

selected
required
boolean
If the API request sets selected = true, it represents the shipping option that the payee or merchant expects to be pre-selected for the payer when they first view the shipping.options in the PayPal Checkout experience. As part of the response if a shipping.option contains selected=true, it represents the shipping option that the payer selected during the course of checkout with PayPal. Only one shipping.option can be set to selected=true.

type	
string
A classification for the method of purchase fulfillment.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP	DEPRECATED. To ensure that seller protection is correctly assigned, please use 'PICKUP_IN_STORE' or 'PICKUP_FROM_PERSON' instead. Currently, this field indicates that the payer intends to pick up the items at a specified address (ie. a store address).
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
amount	
object
The shipping cost for the selected option.

CopyExpand allCollapse all
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
shipping_option
The options that the payee or merchant offers to the payer to ship or pick up their items.

id
required
string <= 127 characters
A unique ID that identifies a payer-selected shipping option.

label
required
string <= 127 characters
A description that the payer sees, which helps them choose an appropriate shipping option. For example, Free Shipping, USPS Priority Shipping, Expédition prioritaire USPS, or USPS yōuxiān fā huò. Localize this description to the payer's locale.

type	
string
A classification for the method of purchase fulfillment.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP	DEPRECATED. To ensure that seller protection is correctly assigned, please use 'PICKUP_IN_STORE' or 'PICKUP_FROM_PERSON' instead. Currently, this field indicates that the payer intends to pick up the items at a specified address (ie. a store address).
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
amount	
object
The shipping cost for the selected option.

CopyExpand allCollapse all
{
"id": "string",
"label": "string",
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
shipping_type
A classification for the method of purchase fulfillment.

string
A classification for the method of purchase fulfillment.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP	DEPRECATED. To ensure that seller protection is correctly assigned, please use 'PICKUP_IN_STORE' or 'PICKUP_FROM_PERSON' instead. Currently, this field indicates that the payer intends to pick up the items at a specified address (ie. a store address).
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
Copy
"SHIPPING"
shipping_with_tracking_details
The order shipping details.

type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
A classification for the method of purchase fulfillment (e.g shipping, in-store pickup, etc). Either type or options may be present, but not both.

Enum Value	Description
SHIPPING	The payer intends to receive the items at a specified address.
PICKUP_IN_PERSON	DEPRECATED. Please use "PICKUP_FROM_PERSON" instead.
PICKUP_IN_STORE	The payer intends to pick up the item(s) from the payee's physical store. Also termed as BOPIS, "Buy Online, Pick-up in Store". Seller protection is provided with this option.
PICKUP_FROM_PERSON	The payer intends to pick up the item(s) from the payee in person. Also termed as BOPIP, "Buy Online, Pick-up in Person". Seller protection is not available, since the payer is receiving the item from the payee in person, and can validate the item prior to payment.
options	
Array of objects [ 0 .. 10 ] items
An array of shipping options that the payee or merchant offers to the payer to ship or pick up their items.

name	
object
The name of the person to whom to ship the items. Supports only the full_name property.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery.

phone_number	
object
The phone number of the recipient of the shipped items, which may belong to either the payer, or an alternate contact, for delivery. [Format - canonical international E.164 numbering plan]

address	
object
The address of the person to whom to ship the items. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties. admin_area_1 is required for addresses located in Argentina, Brazil, China, Canada, India, Indonesia, Japan, Mexico, Thailand, and the United States.

trackers	
Array of objects
Uma série de rastreadores para uma transação.

CópiaExpandir tudoRecolher tudo
{
"type": "SHIPPING",
"options": [
{
"id": "string",
"label": "string",
"selected": true,
"type": "SHIPPING",
"amount": {
"currency_code": "str",
"value": "string"
}
}
],
"name": {
"full_name": "string"
},
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
},
"trackers": [
{
"id": "string",
"status": "CANCELLED",
"items": [
{
"name": "string",
"quantity": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
}
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"create_time": "string",
"update_time": "string"
}
]
}
token_de_uso_único
Token de uso único, de curta duração e gerado pelo PayPal, usado para comunicar informações de pagamento ao PayPal para processamento da transação.

string [ 1 .. 255 ] caracteres ^[0-9a-zA-Z_-]+$ (single_use_token)
Token de uso único, de curta duração e gerado pelo PayPal, usado para comunicar informações de pagamento ao PayPal para processamento da transação.

Cópia
"string"
imediatamente
Informações utilizadas para efetuar o pagamento com o Sofort.

nome
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
string = 2 caracteres ^([AZ]{2}|C2)$ (country_code)
O código de país ISO 3166-1 de dois caracteres.

bic
string [8 .. 11] caracteres ^[AZa-z0-9]{4}[AZaz]{2}[AZa-z0-9]{2}([... Mostrar padrão
O código de identificação bancária (BIC).

iban_últimos_caracteres
string [ 4 .. 34 ] caracteres [a-zA-Z0-9]{4}
Os últimos caracteres do IBAN usado para efetuar o pagamento.

Cópia
{
"name": "string",
"country_code": "string",
"bic": "string",
"iban_last_chars": "string"
}
solicitação imediata
Informações necessárias para efetuar o pagamento com Sofort.

nome
obrigatório
string [ 3 .. 300 ] caracteres
Nome do titular da conta associada a este método de pagamento.

código_do_país
obrigatório
string = 2 caracteres ^([AZ]{2}|C2)$ (country_code)
O código de país ISO 3166-1 de dois caracteres.


contexto_de_experiência
objeto
Personaliza a experiência do pagador durante o processo de aprovação do pagamento.

CópiaExpandir tudoRecolher tudo
{
"name": "string",
"country_code": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
instruções_de_armazenamento_no_cofre
Define como e quando a fonte de pagamento é armazenada em cofre.

string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$ (store_in_vault_instruction)
Define como e quando a fonte de pagamento é armazenada em cofre.

Valor	Descrição
EM CASO DE SUCESSO	Define que a origem do pagamento será armazenada no cofre somente quando pelo menos uma autorização ou captura usando essa origem de pagamento for bem-sucedida.
Cópia
"ON_SUCCESS"
fonte_de_pagamento_armazenada
Fornece detalhes adicionais para processar um pagamento usando uma credencial payment_sourceque foi armazenada ou que se pretende armazenar (também conhecida como credencial armazenada ou cartão em arquivo).
Compatibilidade de parâmetros:

payment_type=ONE_TIMEé compatível apenas com payment_initiator=CUSTOMER.
usage=FIRSTé compatível apenas com payment_initiator=CUSTOMER.
previous_transaction_referenceou previous_network_transaction_referenceé compatível apenas com payment_initiator=MERCHANT.
Apenas um dos parâmetros - previous_transaction_referencee previous_network_transaction_reference- pode estar presente na solicitação.
iniciador_de_pagamento
obrigatório
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
A pessoa ou entidade que iniciou ou desencadeou o pagamento.

Valor de enumeração	Descrição
CLIENTE	O pagamento é iniciado com a participação ativa do cliente, por exemplo, quando um cliente finaliza a compra em um site de um comerciante.
COMERCIANTE	O pagamento é iniciado pelo comerciante em nome do cliente, sem a participação ativa deste. Por exemplo, um comerciante que cobra do cliente o pagamento mensal de uma assinatura.
tipo_de_pagamento
obrigatório
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
Indica o tipo de pagamento armazenado em payment_source.

Valor de enumeração	Descrição
UMA VEZ	Pagamento único, como uma compra online ou uma doação (ex.: finalizar a compra com um clique).
RECORRENTE	Pagamento que faz parte de uma série de pagamentos com valores fixos ou variáveis, realizados em intervalos de tempo predefinidos (ex.: pagamentos de assinatura).
SEM PROGRAMAÇÃO	Pagamento que faz parte de uma série de pagamentos que ocorrem em um cronograma não fixo e/ou têm valores variáveis ​​(ex.: recargas de conta).
uso
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"DERIVADO"
Indica se este é um firstpagamento subsequentfeito através de uma fonte de pagamento armazenada (também conhecida como credencial armazenada ou cartão em arquivo).

Valor de enumeração	Descrição
PRIMEIRO	Indica o pagamento inicial/primeiro pagamento com uma `payment_source` que deverá ser armazenada após o processamento bem-sucedido do pagamento.
SUBSEQUENT	Indica um pagamento usando uma `payment_source` armazenada que já foi usada com sucesso anteriormente para um pagamento.
DERIVADO	Indica que o PayPal irá calcular o valor FIRSTcom SUBSEQUENTbase nos dados disponíveis para o PayPal.

referência_de_transação_de_rede_anterior
objeto
Valores de referência usados ​​pela rede de cartões para identificar uma transação.

CópiaExpandir tudoRecolher tudo
{
"payment_initiator": "CUSTOMER",
"payment_type": "ONE_TIME",
"usage": "FIRST",
"previous_network_transaction_reference": {
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
}
tipo_de_pagamento_fonte_de_pagamento_armazenado
Indica o tipo de pagamento armazenado em payment_source.

string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$ (stored_payment_source_payment_type)
Indica o tipo de pagamento armazenado em payment_source.

Valor de enumeração	Descrição
UMA VEZ	Pagamento único, como uma compra online ou uma doação (ex.: finalizar a compra com um clique).
RECORRENTE	Pagamento que faz parte de uma série de pagamentos com valores fixos ou variáveis, realizados em intervalos de tempo predefinidos (ex.: pagamentos de assinatura).
SEM PROGRAMAÇÃO	Pagamento que faz parte de uma série de pagamentos que ocorrem em um cronograma não fixo e/ou têm valores variáveis ​​(ex.: recargas de conta).
Cópia
"ONE_TIME"
tipo_de_uso_da_fonte_de_pagamento_armazenada
Indica se este é um firstpagamento subsequentfeito através de uma fonte de pagamento armazenada (também conhecida como credencial armazenada ou cartão em arquivo).

string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$ (stored_payment_source_usage_type)
Padrão: 
"DERIVADO"
Indica se este é um firstpagamento subsequentfeito através de uma fonte de pagamento armazenada (também conhecida como credencial armazenada ou cartão em arquivo).

Valor de enumeração	Descrição
PRIMEIRO	Indica o pagamento inicial/primeiro pagamento com uma `payment_source` que deverá ser armazenada após o processamento bem-sucedido do pagamento.
SUBSEQUENT	Indica um pagamento usando uma `payment_source` armazenada que já foi usada com sucesso anteriormente para um pagamento.
DERIVADO	Indica que o PayPal irá calcular o valor FIRSTcom SUBSEQUENTbase nos dados disponíveis para o PayPal.
Cópia
"FIRST"
Período de assinatura
Descreve um período de uma assinatura. Um período é um intervalo de tempo durante o qual um conjunto de termos de assinatura é aplicável.

período_atual
booleano
Indica se este período, com seus termos de assinatura, está atualmente ativo.

unidade_de_intervalo_de_faturamento
string [ 1 .. 24 ] caracteres ^[A-Z_]+$
A unidade de intervalo de faturamento para o período de assinatura.

Valor de enumeração	Descrição
ANO	O período de faturamento da assinatura é anual.
MÊS	O período de faturamento da assinatura é mensal.
SEMESTRE	O intervalo de faturamento para o período de assinatura é de meio mês.
SEMANA	O intervalo de faturamento para o período de assinatura é semanal.
DIA	O intervalo de faturamento para o período de assinatura é diário.
frequência_de_faturamento
inteiro [ 1 .. 365 ]
Padrão: 
1
A frequência, ou seja, o número de unidades de intervalo de faturamento, na qual o assinante é cobrado. Por exemplo, se o billing_interval_unitvalor for DAY, com um valor billing_frequencyde 2, o assinante será cobrado uma vez a cada dois dias.

ciclos_de_faturamento_totais
inteiro [ 0 .. 999 ]
Padrão: 
1
O número de ciclos de faturamento no período. Isso representa, na prática, quantas vezes o assinante será cobrado durante o período.

ciclo_de_faturamento_atual
inteiro [ 0 .. 999 ]
Indica o índice do ciclo de faturamento atual para o período. Por exemplo, um valor de 3 significa que é o terceiro ciclo de faturamento do período.

período_regular
booleano
Indica se o período é regular. Um período regular teria os termos de assinatura padrão, em oposição a termos especiais que poderiam ser aplicáveis ​​a um período de teste ou promocional.


valor_de_cobrança
objeto
O valor que será cobrado ao assinante em cada ciclo de faturamento.


valor_do_envio
objeto
Os custos de envio aplicáveis ​​a cada ciclo.


valor_do_imposto
objeto
O valor do imposto aplicável a cada ciclo.

CópiaExpandir tudoRecolher tudo
{
"current_period": true,
"billing_interval_unit": "YEAR",
"billing_frequency": 1,
"total_billing_cycles": 1,
"current_billing_cycle": 999,
"regular_period": true,
"billing_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"tax_amount": {
"currency_code": "str",
"value": "string"
}
}
dados_suplementares
Dados suplementares sobre um pagamento. Este objeto transmite informações que podem ser usadas para melhorar as avaliações de risco e os custos de processamento, por exemplo, fornecendo dados de pagamento de Nível 2 e Nível 3.


cartão
objeto
Comerciantes e parceiros podem adicionar dados de Nível 2 e 3 aos pagamentos para reduzir riscos e custos de processamento. Para mais informações sobre processamento de pagamentos, consulte a página de finalização da compra ou finalização da compra com múltiplos participantes .


risco
objeto
Comerciantes e parceiros podem adicionar parâmetros adicionais do cliente que podem ajudar a melhorar a proteção contra fraudes e reduzir o risco em pagamentos com cartões sem marca.

CópiaExpandir tudoRecolher tudo
{
"card": {
"level_2": {
"invoice_id": "string",
"tax_total": {
"currency_code": "str",
"value": "string"
}
},
"level_3": {
"ships_from_postal_code": "string",
"line_items": [
{
"name": "string",
"quantity": "string",
"description": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
},
"billing_plan": {
"billing_cycles": [
{
"tenure_type": null,
"total_cycles": null,
"sequence": null,
"pricing_scheme": null,
"start_date": null
}
],
"name": "string",
"setup_fee": {
"currency_code": "str",
"value": "string"
}
},
"commodity_code": "string",
"unit_of_measure": "string",
"unit_amount": {
"currency_code": "str",
"value": "string"
},
"tax": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"total_amount": {
"currency_code": "str",
"value": "string"
}
}
],
"shipping_amount": {
"currency_code": "str",
"value": "string"
},
"duty_amount": {
"currency_code": "str",
"value": "string"
},
"discount_amount": {
"currency_code": "str",
"value": "string"
},
"shipping_address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
},
"risk": {
"customer": {
"ip_address": "string"
}
}
}
informações_imposto
O número de identificação fiscal do cliente. O cliente também é conhecido como pagador. Ambos tax_idos dados tax_id_typesão obrigatórios.

ID_imposto
obrigatório
string [ 1 .. 14 ] caracteres ([a-zA-Z0-9])
O valor do número de identificação fiscal do cliente.

tipo_de_identificação_de_imposto
obrigatório
string [ 1 .. 14 ] caracteres ^[A-Z0-9_]+$
O tipo de identificação fiscal do cliente.

Valor de enumeração	Descrição
BR_CPF	O número de identificação fiscal individual geralmente possui 11 caracteres.
BR_CNPJ	O tipo de identificação fiscal da empresa geralmente possui 14 caracteres.
Cópia
{
"tax_id": "string",
"tax_id_type": "BR_CPF"
}
resposta_de_autenticação_segura_three_d
Resultados da autenticação 3D Secure.

status_de_autenticação
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
O resultado da autenticação do emissor.

Valor de enumeração	Descrição
Y	Autenticação bem-sucedida.
N	Autenticação falhou / conta não verificada / transação negada.
U	Não foi possível concluir a autenticação.
UM	Transação tentada com sucesso.
C	É necessário um desafio para autenticação.
R	Autenticação rejeitada (o comerciante não deve submeter o pedido de autorização).
D	Desafio necessário; autenticação desacoplada confirmada.
EU	Apenas para fins informativos; preferência do solicitante do 3DS reconhecida.
status_de_matrícula
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
Status da elegibilidade para autenticação.

Valor de enumeração	Descrição
Y	Sim. O banco participa do protocolo 3-D Secure e retornará o ACSUrl.
N	Não. O banco não participa do protocolo 3-D Secure.
U	Indisponível. O DS ou ACS não está disponível para autenticação no momento da solicitação.
B	Ignorar. A regra de autenticação do comerciante é acionada para ignorar a autenticação.
Cópia
{
"authentication_status": "Y",
"enrollment_status": "Y"
}
token
A fonte de pagamento tokenizada para financiar um pagamento.

eu ia
obrigatório
string [ 1 .. 255 ] caracteres ^[0-9a-zA-Z_-]+$
O ID do token gerado pelo PayPal.

tipo
obrigatório
string [ 1 .. 255 ] caracteres ^[0-9A-Z_-]+$
O método de tokenização que gerou o ID.

Valor	Descrição
CONTRATO DE COBRANÇA	O ID do contrato de cobrança do PayPal. Refere-se a um pagamento recorrente aprovado por bens ou serviços.
Cópia
{
"id": "string",
"type": "BILLING_AGREEMENT"
}
rastreador
A resposta de rastreamento na criação do rastreador.

eu ia
corda
O ID do rastreador.

status
string [ 1 .. 64 ] caracteres ^[0-9A-Z_]+$
O status do envio do item.

Valor de enumeração	Descrição
CANCELADO	O envio foi cancelado e o número de rastreamento não é mais válido.
ENVIADO	O vendedor atribuiu um número de rastreamento aos itens enviados do pedido. Este número não corresponde ao status real da entrega fornecido pela transportadora. O status mais recente da encomenda deve ser obtido junto à transportadora.

Unid
Conjunto de objetos
Uma série de detalhes dos itens na remessa.


links
Conjunto de objetos
Uma série de links HATEOAS relacionados a solicitações.

tempo_de_criação
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

CopyExpand allCollapse all
{
"id": "string",
"status": "CANCELLED",
"items": [
{
"name": "string",
"quantity": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
}
}
],
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"create_time": "string",
"update_time": "string"
}
tracker
The tracking information for a shipment.

tracking_number
required
string [ 1 .. 64 ] characters
The tracking number for the shipment. This property supports Unicode.

carrier_name_other	
string [ 1 .. 64 ] characters
The name of the carrier for the shipment. Provide this value only if the carrier parameter is OTHER. This property supports Unicode.

carrier
required
string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The carrier for the shipment. Some carriers have a global version as well as local subsidiaries. The subsidiaries are repeated over many countries and might also have an entry in the global list. Choose the carrier for your country. If the carrier is not available for your country, choose the global version of the carrier. If your carrier name is not in the list, set carrier to OTHER and set carrier name in carrier_name_other. For allowed values, see Carriers.

Enum Value	Description
DPD_RU	DPD Russia.
BG_BULGARIAN_POST	Bulgarian Posts.
KR_KOREA_POST	Koreapost (www.koreapost.go.kr).
ZA_COURIERIT	Courier IT.
FR_EXAPAQ	DPD France (formerly exapaq).
ARE_EMIRATES_POST	Emirates Post.
GAC	GAC.
GEIS	Geis CZ.
SF_EX	SF Express.
PAGO	Pago Logistics.
MYHERMES	MyHermes UK.
DIAMOND_EUROGISTICS	Diamond Eurogistics Limited.
CORPORATECOURIERS_WEBHOOK	Corporate Couriers.
BOND	Bond courier.
OMNIPARCEL	Omni Parcel.
SK_POSTA	Slovenska pošta.
PUROLATOR	purolator.
FETCHR_WEBHOOK	Mena 360 (Fetchr).
THEDELIVERYGROUP	TDG – The Delivery Group.
CELLO_SQUARE	Cello Square.
TARRIVE	TONDA GLOBAL.
COLLIVERY	MDS Collivery Pty (Ltd).
MAINFREIGHT	Mainfreight.
IND_FIRSTFLIGHT	First Flight Couriers.
ACSWORLDWIDE	ACS Worldwide Express.
AMSTAN	Amstan Logistics.
OKAYPARCEL	OkayParcel.
ENVIALIA_REFERENCE	Envialia Reference.
SEUR_ES	Seur Spain.
CONTINENTAL	Continental.
FDSEXPRESS	FDSEXPRESS.
AMAZON_FBA_SWISHIP	Swiship UK.
WYNGS	Wyngs.
DHL_ACTIVE_TRACING	DHL Active Tracing.
ZYLLEM	Zyllem.
RUSTON	Ruston.
XPOST	Xpost.ph.
CORREOS_ES	correos Express (www.correos.es).
DHL_FR	DHL France (www.dhl.com).
PAN_ASIA	Pan-Asia International.
BRT_IT	BRT couriers Italy (www.brt.it).
SRE_KOREA	SRE Korea (www.srekorea.co.kr).
SPEEDEE	Spee-Dee Delivery.
TNT_UK	TNT UK Limited (www.tnt.com).
VENIPAK	Venipak.
SHREENANDANCOURIER	SHREE NANDAN COURIER.
CROSHOT	Croshot.
NIPOST_NG	NIpost (www.nipost.gov.ng).
EPST_GLBL	ePost Global.
NEWGISTICS	Newgistics.
POST_SLOVENIA	Post of Slovenia.
JERSEY_POST	Jersey Post.
BOMBINOEXP	Bombino Express Pvt.
WMG	WMG Delivery.
XQ_EXPRESS	XQ Express.
FURDECO	Furdeco.
LHT_EXPRESS	LHT Express.
SOUTH_AFRICAN_POST_OFFICE	South African Post Office.
SPOTON	SPOTON Logistics Pvt Ltd.
DIMERCO	Dimerco Express Group.
CYPRUS_POST_CYP	cyprus post.
ABCUSTOM	AB Custom Group.
IND_DELIVREE	deliverE.
CN_BESTEXPRESS	Best Express.
DX_SFTP	DX (SFTP).
PICKUPP_MYS	PICK UPP.
FMX	FMX.
HELLMANN	Hellmann Worldwide Logistics.
SHIP_IT_ASIA	Ship It Asia.
KERRY_ECOMMERCE	Kerry eCommerce.
FRETERAPIDO	Frete Rapido.
PITNEY_BOWES	Pitney Bowes.
XPRESSEN_DK	Xpressen courier.
SEUR_SP_API	Spanish Seur API.
DELIVERYONTIME	DELIVERYONTIME LOGISTICS PVT LTD.
JINSUNG	JINSUNG TRADING.
TRANS_KARGO	Trans Kargo Internasional.
SWISHIP_DE	Swiship DE.
IVOY_WEBHOOK	Ivoy courier.
AIRMEE_WEBHOOK	Airmee couriers.
DHL_BENELUX	dhl benelux.
FIRSTMILE	FirstMile.
FASTWAY_IR	Fastway Ireland.
HH_EXP	Hua Han Logistics.
MYS_MYPOST_ONLINE	Mypostonline.
TNT_NL	THT Netherland.
TIPSA	TIPSA courier.
TAQBIN_MY	TAQBIN Malaysia.
KGMHUB	KGM Hub.
INTEXPRESS	Internet Express.
OVERSE_EXP	Overseas Express.
ONECLICK	One click delivery services.
ROADRUNNER_FREIGHT	Roadbull Logistics.
GLS_CROTIA	GLS Croatia.
MRW_FTP	MRW courier.
BLUEX	Blue Express.
DYLT	Daylight Transport.
DPD_IR	DPD Ireland.
SIN_GLBL	Sin Global Express.
TUFFNELLS_REFERENCE	Tuffnells Parcels Express- Reference.
CJPACKET	CJ Packet.
MILKMAN	Milkman courier.
ASIGNA	ASIGNA courier.
ONEWORLDEXPRESS	One World Express.
ROYAL_MAIL	RoyalShipments.
VIA_EXPRESS	Viaxpress.
TIGFREIGHT	TIG Freight.
ZTO_EXPRESS	ZTO Express.
TWO_GO	2GO Courier.
IML	IML courier.
INTEL_VALLEY	Intel-Valley Supply chain (ShenZhen) Co. Ltd.
EFS	EFS (E-commerce Fulfillment Service).
UK_UK_MAIL	UK mail (ukmail.com).
RAM	RAM courier.
ALLIEDEXPRESS	Allied Express.
APC_OVERNIGHT	APC overnight (apc-overnight.com).
SHIPPIT	Shippit.
TFM	TFM Xpress.
M_XPRESS	M Xpress Sdn Bhd.
HDB_BOX	Haidaibao (BOX).
CLEVY_LINKS	Clevy Links.
IBEONE	Beone Logistics.
FIEGE_NL	Fiege Netherlands.
KWE_GLOBAL	KWE Global.
CTC_EXPRESS	CTC Express.
AMAZON	Amazon Shipping.
MORE_LINK	Morelink.
JX	JX courier.
EASY_MAIL	Easy Mail.
ADUIEPYLE	A Duie Pyle.
GB_PANTHER	Panther.
EXPRESSSALE	Expresssale.
SG_DETRACK	Detrack.
TRUNKRS_WEBHOOK	Trunkrs courier.
MATDESPATCH	Matdespatch.
DICOM	GLS Logistic Systems Canada Ltd./Dicom.
MBW	MBW Courier Inc..
KHM_CAMBODIA_POST	Cambodia Post.
SINOTRANS	Sinotrans.
BRT_IT_PARCELID	BRT Bartolini(Parcel ID).
DHL_SUPPLY_CHAIN	DHL Supply Chain APAC.
DHL_PL	DHL Poland.
TOPYOU	TopYou.
PALEXPRESS	PAL Express Limited.
DHL_SG	dhl Singapore.
CN_WEDO	WeDo Logistics.
FULFILLME	Fulfillme.
DPD_DELISTRACK	DPD delistrack.
UPS_REFERENCE	UPS Reference.
CARIBOU	Caribou.
LOCUS_WEBHOOK	Locus courier.
DSV	DSV courier.
P2P_TRC	P2P TrakPak.
DIRECTPARCELS	Direct Parcels.
NOVA_POSHTA_INT	Nova Poshta (International).
FEDEX_POLAND	FedEx® Poland Domestic.
CN_JCEX	JCEX courier.
FAR_INTERNATIONAL	FAR international.
IDEXPRESS	IDEX courier.
GANGBAO	GANGBAO Supplychain.
NEWAY	Neway Transport.
POSTNL_INT_3_S	PostNL International.
RPX_ID	RPX Indonesia.
DESIGNERTRANSPORT_WEBHOOK	Designer Transport.
GLS_SLOVEN	GLS Slovenia.
PARCELLED_IN	Parcelled.in.
GSI_EXPRESS	GSI EXPRESS.
CON_WAY	Con-way Freight.
BROUWER_TRANSPORT	Brouwer Transport en Logistiek.
CPEX	Captain Express International.
ISRAEL_POST	Israel Post.
DTDC_IN	DTDC India.
PTT_POST	PTT Post.
XDE_WEBHOOK	Ximex Delivery Express.
TOLOS	Tolos courier.
GIAO_HANG	Giao hàng nhanh.
GEODIS_ESPACE	Geodis E-space.
MAGYAR_HU	Magyar Post.
DOORDASH_WEBHOOK	DoorDash.
TIKI_ID	Tiki shipment.
CJ_HK_INTERNATIONAL	CJ Logistics International(Hong Kong).
STAR_TRACK_EXPRESS	Star Track Express.
HELTHJEM	Helthjem.
SFB2C	SF International.
FREIGHTQUOTE	Freightquote by C.H. Robinson.
LANDMARK_GLOBAL_REFERENCE	Landmark Global Reference.
PARCEL2GO	Parcel2Go.
DELNEXT	Delnext.
RCL	Red Carpet Logistics.
CGS_EXPRESS	CGS Express.
HK_POST	Hongkong Post (www.hongkongpost.hk).
SAP_EXPRESS	SAP EXPRESS.
PARCELPOST_SG	Parcel Post Singapore.
HERMES	HermesWorld UK.
IND_SAFEEXPRESS	Safexpress.
TOPHATTEREXPRESS	Tophatter Express.
MGLOBAL	PT MGLOBAL LOGISTICS INDONESIA.
AVERITT	Averitt Express.
LEADER	leader.
_2EBOX	2ebox courier.
SG_SPEEDPOST	Singapore Speedpost.
DBSCHENKER_SE	DB Schenker (www.dbschenker.com).
ISR_POST_DOMESTIC	Israel Post Domestic.
BESTWAYPARCEL	Best Way Parcel.
ASENDIA_DE	asendia_de.
NIGHTLINE_UK	nightline_uk.
TAQBIN_SG	taqbin_sg.
TCK_EXPRESS	TCK Express.
ENDEAVOUR_DELIVERY	Endeavour Delivery.
NANJINGWOYUAN	Nanjing Woyuan.
HEPPNER_FR	Heppner France.
EMPS_CN	EMPS Express.
FONSEN	Fonsen Logistics.
PICKRR	Pickrr.
APC_OVERNIGHT_CONNUM	APC Overnight Consignment.
STAR_TRACK_NEXT_FLIGHT	Star Track Next Flight.
DAJIN	Shanghai Aqrum Chemical Logistics Co.Ltd.
UPS_FREIGHT	UPS Freight.
POSTA_PLUS	Posta Plus.
CEVA	CEVA LOGISTICS.
ANSERX	ANSERX courier.
JS_EXPRESS	JS EXPRESS.
PADTF	padtf.com.
UPS_MAIL_INNOVATIONS	UPS Mail Innovations.
SYPOST	Sunyou Post.
AMAZON_SHIP_MCF	Amazon Shipping + Amazon MCF.
YUSEN	Yusen Logistics.
BRING	Bring.
SDA_IT	SDA Italy.
GBA	GBA Services Ltd.
NEWEGGEXPRESS	Newegg Express.
SPEEDCOURIERS_GR	Speed Couriers.
FORRUN	forrun Pvt Ltd (Arpatech Venture).
PICKUP	Pickupp.
ECMS	ECMS International Logistics Co..
INTELIPOST	Intelipost (TMS for LATAM).
FLASHEXPRESS	Flash Express.
CN_STO	STO Express.
SEKO_SFTP	SEKO Worldwide.
HOME_DELIVERY_SOLUTIONS	Home Delivery Solutions Ltd.
DPD_HGRY	DPD Hungary.
KERRYTTC_VN	Kerry Express (Vietnam) Co Ltd.
JOYING_BOX	Joying Box.
TOTAL_EXPRESS	Total Express.
ZJS_EXPRESS	ZJS International.
STARKEN	STARKEN couriers.
DEMANDSHIP	DemandShip.
CN_DPEX	DPEX.
AUPOST_CN	AuPost China.
LOGISTERS	Logisters.
GOGLOBALPOST	Global Post.
GLS_CZ	GLS Czech Republic.
PAACK_WEBHOOK	Paack courier.
GRAB_WEBHOOK	Grab courier.
PARCELPOINT	Parcelpoint.
ICUMULUS	iCumulus.
DAIGLOBALTRACK	DAI Post.
GLOBAL_IPARCEL	i-parcel.
YURTICI_KARGO	Yurtici Kargo.
CN_PAYPAL_PACKAGE	PayPal Package.
PARCEL_2_POST	Parcel To Post.
GLS_IT	GLS Italy.
PIL_LOGISTICS	PIL Logistics (China) Co..
HEPPNER	Heppner Internationale Spedition GmbH & Co..
GENERAL_OVERNIGHT	Go!Express and logistics.
HAPPY2POINT	Happy 2ThePoint.
CHITCHATS	Chit Chats.
SMOOTH	Smooth Couriers.
CLE_LOGISTICS	CL E-Logistics Solutions Limited.
FIEGE	Fiege Logistics.
MX_CARGO	M&X cargo.
ZIINGFINALMILE	Ziing Final Mile Inc.
DAYTON_FREIGHT	Dayton Freight.
TCS	TCS courier.
AEX	AEX Group.
HERMES_DE	Hermes Germany.
ROUTIFIC_WEBHOOK	Routific.
GLOBAVEND	Globavend.
CJ_LOGISTICS	CJ Logistics International.
PALLET_NETWORK	The Pallet Network.
RAF_PH	RAF Philippines.
UK_XDP	XDP Express.
PAPER_EXPRESS	Paper Express.
LA_POSTE_SUIVI	La Poste.
PAQUETEXPRESS	Paquetexpress.
LIEFERY	liefery.
STRECK_TRANSPORT	Streck Transport.
PONY_EXPRESS	Pony express.
ALWAYS_EXPRESS	Always Express.
GBS_BROKER	GBS-Broker.
CITYLINK_MY	City-Link Express.
ALLJOY	ALLJOY SUPPLY CHAIN.
YODEL	yodel.
YODEL_DIR	Yodel Direct.
STONE3PL	STONE3PL.
PARCELPAL_WEBHOOK	ParcelPal.
DHL_ECOMERCE_ASA	DHL eCommerce Asia (API).
SIMPLYPOST	J&T Express Singapore.
KY_EXPRESS	Kua Yue Express.
SHENZHEN	shenzhen 1st International Logistics(Group)Co.
US_LASERSHIP	LaserShip.
UC_EXPRE	ucexpress.
DIDADI	DIDADI Logistics tech.
CJ_KR	CJ Korea Express.
DBSCHENKER_B2B	DB Schenker B2B.
MXE	MXE Express.
CAE_DELIVERS	CAE Delivers.
PFCEXPRESS	PFC Express.
WHISTL	Whistl.
WEPOST	WePost Sdn Bhd.
DHL_PARCEL_ES	DHL parcel Spain(www.dhl.com).
DDEXPRESS	DD Express Courier.
ARAMEX_AU	Aramex Australia (formerly Fastway AU).
BNEED	Bneed courier.
HK_TGX	Kerry Express Hong Kong.
LATVIJAS_PASTS	Latvijas Pasts.
VIAEUROPE	ViaEurope.
CORREO_UY	Correo Uruguayo.
CHRONOPOST_FR	Chronopost france (www.chronopost.fr).
J_NET	J-Net.
_6LS	6ls.com.
BLR_BELPOST	Belpost.
BIRDSYSTEM	BirdSystem.
DOBROPOST	DobroPost.
WAHANA_ID	Wahana express (www.wahana.com).
WEASHIP	Weaship.
SONICTL	Sonic Transportation & Logistics.
KWT	Shenzhen Jinghuada Logistics Co..
AFLLOG_FTP	AFL LOGISTICS.
SKYNET_WORLDWIDE	SkyNet Worldwide Express.
NOVA_POSHTA	Nova Poshta (novaposhta.ua).
SEINO	Seino.
SZENDEX	SZENDEX.
BPOST_INT	Bpost international.
DBSCHENKER_SV	DB Schenker Sweden.
AO_DEUTSCHLAND	AO Deutschland.
EU_FLEET_SOLUTIONS	EU Fleet Solutions.
PCFCORP	PCF Final Mile.
LINKBRIDGE	Link Bridge(BeiJing)international logistics co..
PRIMAMULTICIPTA	PT Prima Multi Cipta.
COUREX	Urbanfox.
ZAJIL_EXPRESS	Zajil Express Company.
COLLECTCO	CollectCo.
JTEXPRESS	J&T EXPRESS MALAYSIA.
FEDEX_UK	FedEx® UK.
USHIP	uShip courier.
PIXSELL	PIXSELL LOGISTICS.
SHIPTOR	Shiptor.
CDEK	CDEK courier.
VNM_VIETTELPOST	ViettelPost.
CJ_CENTURY	CJ Century.
GSO	GSO(GLS-USA).
VIWO	VIWO IoT.
SKYBOX	SKYBOX.
KERRYTJ	Kerry TJ Logistics.
NTLOGISTICS_VN	Nhat Tin Logistics.
SDH_SCM	lightning monkey.
ZINC	Zinc courier.
DPE_SOUTH_AFRC	DPE South Africa.
CESKA_CZ	Czech Post.
ACS_GR	ACS Courier.
DEALERSEND	DealerSend.
JOCOM	Jocom.
CSE	CSE courier.
TFORCE_FINALMILE	TForce Final Mile.
SHIP_GATE	ShipGate.
SHIPTER	SHIPTER.
NATIONAL_SAMEDAY	National Sameday.
YUNEXPRESS	YunExpress.
CAINIAO	AliExpress Standard Shipping.
DMS_MATRIX	DMSMatrix.
DIRECTLOG	Directlog (www.directlog.com.br).
ASENDIA_US	Asendia USA.
_3JMSLOGISTICS	3JMS Logistics.
LICCARDI_EXPRESS	LICCARDI EXPRESS COURIER.
SKY_POSTAL	SkyPostal.
CNWANGTONG	cnwangtong.
POSTNORD_LOGISTICS_DK	ostnord denmark.
LOGISTIKA	Logistika.
CELERITAS	Celeritas Transporte.
PRESSIODE	Pressio.
SHREE_MARUTI	Shree Maruti Courier Services Pvt Ltd.
LOGISTICSWORLDWIDE_HK	Logistic Worldwide Express (LWE Honkong).
EFEX	eFEx (E-Commerce Fulfillment & Express).
LOTTE	Lotte Global Logistics.
LONESTAR	Lone Star Overnight.
APRISAEXPRESS	Aprisa Express.
BEL_RS	BEL North Russia.
OSM_WORLDWIDE	OSM Worldwide.
WESTGATE_GL	Westgate Global.
FASTRACK	Fasttrack.
DTD_EXPR	DTD Express.
ALFATREX	AlfaTrex.
PROMEDDELIVERY	ProMed Delivery.
THABIT_LOGISTICS	Thabit Logistics.
HCT_LOGISTICS	HCT LOGISTICS CO.LTD..
CARRY_FLAP	Carry-Flap Co..
US_OLD_DOMINION	Old Dominion Freight Line.
ANICAM_BOX	ANICAM BOX EXPRESS.
WANBEXPRESS	WanbExpress.
AN_POST	An Post.
DPD_LOCAL	DPD Local.
STALLIONEXPRESS	Stallion Express.
RAIDEREX	RaidereX.
SHOPFANS	ShopfansRU LLC.
KYUNGDONG_PARCEL	Kyungdong Parcel.
CHAMPION_LOGISTICS	Champion Logistics.
PICKUPP_SGP	PICK UPP (Singapore).
MORNING_EXPRESS	Morning Express.
NACEX	NACEX.
THENILE_WEBHOOK	SortHub courier.
HOLISOL	Holisol.
LBCEXPRESS_FTP	LBC EXPRESS INC..
KURASI	KURASI.
USF_REDDAWAY	USF Reddaway.
APG	APG eCommerce Solutions.
CN_BOXC	BoxC courier.
ECOSCOOTING	ECOSCOOTING.
MAINWAY	Mainway.
PAPERFLY	Paperfly Private Limited.
HOUNDEXPRESS	Hound Express.
BOX_BERRY	Boxberry courier.
EP_BOX	EP-Box courier.
PLUS_LOG_UK	Plus UK Logistics.
FULFILLA	Fulfilla.
ASE	ASE KARGO.
MAIL_PLUS	MailPlus.
XPO_LOGISTICS	XPO logistics.
WNDIRECT	wnDirect.
CLOUDWISH_ASIA	Cloudwish Asia.
ZELERIS	Zeleris.
GIO_EXPRESS	Gio Express.
OCS_WORLDWIDE	OCS WORLDWIDE.
ARK_LOGISTICS	ARK Logistics.
AQUILINE	Aquiline.
PILOT_FREIGHT	Pilot Freight Services.
QWINTRY	Qwintry Logistics.
DANSKE_FRAGT	Danske Fragtaend.
CARRIERS	Carriers courier.
AIR_CANADA_GLOBAL	Rivo (Air canada).
PRESIDENT_TRANS	PRESIDENT TRANSNET CORP.
STEPFORWARDFS	STEP FORWARD FREIGHT SERVICE CO LTD.
SKYNET_UK	Skynet UK.
PITTOHIO	PITT OHIO.
CORREOS_EXPRESS	Correos Express.
RL_US	RL Carriers.
DESTINY	Destiny Transportation.
UK_YODEL	Yodel (www.yodel.co.uk).
COMET_TECH	CometTech.
DHL_PARCEL_RU	DHL Parcel Russia.
TNT_REFR	TNT Reference.
SHREE_ANJANI_COURIER	Shree Anjani Courier.
MIKROPAKKET_BE	Mikropakket Belgium.
ETS_EXPRESS	RETS express.
COLIS_PRIVE	Colis Privé.
CN_YUNDA	Yunda Express.
AAA_COOPER	AAA Cooper.
ROCKET_PARCEL	Rocket Parcel International.
_360LION	360 Lion Express.
PANDU	PANDU.
PROFESSIONAL_COURIERS	PROFESSIONAL COURIERS.
FLYTEXPRESS	FLYTEXPRESS.
LOGISTICSWORLDWIDE_MY	LOGISTICSWORLDWIDE MY.
CORREOS_DE_ESPANA	CORREOS DE ESPANA.
IMX	IMX.
FOUR_PX_EXPRESS	FOUR PX EXPRESS.
XPRESSBEES	XPRESSBEES.
PICKUPP_VNM	pickupp_vnm.
STARTRACK_EXPRESS	startrack_express.
FR_COLISSIMO	fr_colissimo.
NACEX_SPAIN_REFERENCE	nacex_spain_reference.
DHL_SUPPLY_CHAIN_AU	dhl_supply_chain_au.
ESHIPPING	Eshipping.
SHREETIRUPATI	SHREE TIRUPATI COURIER SERVICES PVT. LTD..
HX_EXPRESS	HX Express.
INDOPAKET	INDOPAKET.
CN_17POST	17 Post Service.
K1_EXPRESS	K1 Express.
CJ_GLS	CJ GLS.
MYS_GDEX	GDEX courier.
NATIONEX	Nationex courier.
ANJUN	Anjun couriers.
FARGOOD	FarGood.
SMG_EXPRESS	SMG Direct.
RZYEXPRESS	RZY Express.
SEFL	Southeastern Freight Lines.
TNT_CLICK_IT	TNT-Click Italy.
HDB	Haidaibao.
HIPSHIPPER	Hipshipper.
RPXLOGISTICS	RPX Logistics.
KUEHNE	Kuehne + Nagel.
IT_NEXIVE	Nexive (TNT Post Italy).
PTS	PTS courier.
SWISS_POST_FTP	Swiss Post FTP.
FASTRK_SERV	Fastrak Services.
_4_72	4-72 Entregando.
US_YRC	YRC courier.
POSTNL_INTL_3S	PostNL International 3S.
ELIAN_POST	Yilian (Elian) Supply Chain.
CUBYN	Cubyn.
SAU_SAUDI_POST	Saudi Post.
ABXEXPRESS_MY	ABX Express.
HUAHAN_EXPRESS	HUAHANG EXPRESS.
ZES_EXPRESS	Eshun international Logistic.
ZEPTO_EXPRESS	ZeptoExpress.
SKYNET_ZA	Skynet World Wide Express South Africa.
ZEEK_2_DOOR	Zeek2Door.
BLINKLASTMILE	Blink.
POSTA_UKR	UkrPoshta.
CHROBINSON	C.H. Robinson Worldwide.
CN_POST56	Post56.
COURANT_PLUS	Courant Plus.
SCUDEX_EXPRESS	Scudex Express.
SHIPENTEGRA	ShipEntegra.
B_TWO_C_EUROPE	B2C courier Europe.
COPE	Cope Sensitive Freight.
IND_GATI	Gati-KWE.
CN_WISHPOST	WishPost.
NACEX_ES	NACEX Spain.
TAQBIN_HK	TAQBIN Hong Kong.
GLOBALTRANZ	GlobalTranz.
HKD	Qingdao HKD International Logistics.
BJSHOMEDELIVERY	BJS Distribution courier.
OMNIVA	Omniva.
SUTTON	Sutton Transport.
PANTHER_REFERENCE	Panther Reference.
SFCSERVICE	SFC Service.
LTL	LTL COURIER.
PARKNPARCEL	Park N Parcel.
SPRING_GDS	Spring GDS.
ECEXPRESS	ECexpress.
INTERPARCEL_AU	Interparcel Australia.
AGILITY	Agility.
XL_EXPRESS	XL Express.
ADERONLINE	Ader couriers.
DIRECTCOURIERS	Direct Couriers.
PLANZER	Planzer Group.
SENDING	Sending Transporte Urgente y Comunicacion.
NINJAVAN_WB	Ninjavan Webhook.
NATIONWIDE_MY	Nationwide Express Courier Services Bhd (www.nationwide.com.my).
SENDIT	Sendit.
GB_ARROW	Arrow XL.
IND_GOJAVAS	GoJavas.
KPOST	Korea Post.
DHL_FREIGHT	DHL Freight.
BLUECARE	Bluecare Express Ltd.
JINDOUYUN	jindouyun courier.
TRACKON	Trackon Couriers Pvt. Ltd.
GB_TUFFNELLS	Tuffnells Parcels Express.
TRUMPCARD	TRUMPCARD LLC.
ETOTAL	eTotal Solution Limited.
SFPLUS_WEBHOOK	Zeek courier.
SEKOLOGISTICS	SEKO Logistics.
HERMES_2MANN_HANDLING	Hermes Einrichtungs Service GmbH & Co. KG.
DPD_LOCAL_REF	DPD Local reference.
UDS	United Delivery Service.
ZA_SPECIALISED_FREIGHT	Specialised Freight.
THA_KERRY	Kerry Express Thailand.
PRT_INT_SEUR	SEUR International.
BRA_CORREIOS	Correios Brazil.
NZ_NZ_POST	New Zealand Post.
CN_EQUICK	Equick China.
MYS_EMS	Malaysia Post EMS / Pos Laju.
GB_NORSK	Norsk Global.
ESP_MRW	MRW spain.
ESP_PACKLINK	Packlink.
KANGAROO_MY	Kangaroo Worldwide Express.
RPX	RPX Online.
XDP_UK_REFERENCE	XDP Express Reference.
NINJAVAN_MY	ninja van (www.ninjavan.co).
ADICIONAL	Adicional Logistics.
ROADBULL	Red Carpet Logistics.
YAKIT	Yakit courier.
MAILAMERICAS	MailAmericas.
MIKROPAKKET	Mikropakket.
DYNALOGIC	Dynamic Logistics.
DHL_ES	DHL Spain(www.dhl.com).
DHL_PARCEL_NL	DHL Parcel NL.
DHL_GLOBAL_MAIL_ASIA	DHL Global Mail Asia (www.dhl.com).
DAWN_WING	Dawn Wing.
GENIKI_GR	Geniki Taxydromiki.
HERMESWORLD_UK	hermesworld_uk.
ALPHAFAST	Alphafast (www.alphafast.com).
BUYLOGIC	buylogic.
EKART	Ekart logistics (ekartlogistics.com).
MEX_SENDA	mexico senda express.
SFC_LOGISTICS	SFC.
POST_SERBIA	Posta Serbia.
IND_DELHIVERY	Delhivery India.
DE_DPD_DELISTRACK	DPD Germany.
RPD2MAN	RPD2man Deliveries.
CN_SF_EXPRESS	SF Express (www.sf-express.com).
YANWEN	Yanwen Logistics.
MYS_SKYNET	Skynet Malaysia.
CORREOS_DE_MEXICO	correos mexico.
CBL_LOGISTICA	CBL Logistica.
MEX_ESTAFETA	Estafeta (www.estafeta.com).
AU_AUSTRIAN_POST	Austrian Post (Registered).
RINCOS	Rincos.
NLD_DHL	DHL Netherland.
RUSSIAN_POST	Russian post.
COURIERS_PLEASE	CouriersPlease (couriersplease.com.au).
POSTNORD_LOGISTICS	PostNord Logistics.
FEDEX	Fedex.
DPE_EXPRESS	DPE Express.
DPD	DPD.
ADSONE	ADSone.
IDN_JNE	JNE Express (Jalur Nugraha Ekakurir).
THECOURIERGUY	The Courier Guy.
CNEXPS	CNE Express.
PRT_CHRONOPOST	Chronopost Portugal.
LANDMARK_GLOBAL	Landmark Global.
IT_DHL_ECOMMERCE	DHL International.
ESP_NACEX	NACEX Spain.
PRT_CTT	CTT Portugal.
BE_KIALA	Kiala.
ASENDIA_UK	Asendia UK.
GLOBAL_TNT	TNT global.
POSTUR_IS	Iceland Post.
EPARCEL_KR	eParcel Korea.
INPOST_PACZKOMATY	InPost Paczkomaty.
IT_POSTE_ITALIA	Poste italiane (www.poste.it).
BE_BPOST	Bpost (www.bpost.be).
PL_POCZTA_POLSKA	Poczta Polska (www.poczta-polska.pl).
MYS_MYS_POST	Malaysia Post.
SG_SG_POST	Singapore Post.
THA_THAILAND_POST	Thailand Post (www.thailandpost.co.th).
LEXSHIP	LexShip.
FASTWAY_NZ	Fastway New Zealand.
DHL_AU	DHL Supply Chain Australia.
COSTMETICSNOW	Cosmetics Now.
PFLOGISTICS	PFL.
LOOMIS_EXPRESS	Loomis Express.
GLS_ITALY	GLS Italy.
LINE	Line Clear Express & Logistics Sdn Bhd.
GEL_EXPRESS	Gel Express Logistik.
HUODULL	Huodull.
NINJAVAN_SG	Ninja van Singapore.
JANIO	Janio Asia.
AO_COURIER	AO Logistics.
BRT_IT_SENDER_REF	BRT Bartolini(Sender Reference).
SAILPOST	SAILPOST.
LALAMOVE	Lalamove.
NEWZEALAND_COURIERS	NEW ZEALAND COURIERS.
ETOMARS	Etomars.
VIRTRANSPORT	VIR Transport.
WIZMO	Wizmo.
PALLETWAYS	Palletways.
I_DIKA	i-dika.
CFL_LOGISTICS	CFL Logistics.
GEMWORLDWIDE	GEM Worldwide.
GLOBAL_EXPRESS	Tai Wan Global Business.
LOGISTYX_TRANSGROUP	Transgroup courier.
WESTBANK_COURIER	West Bank Courier.
ARCO_SPEDIZIONI	Arco Spedizioni SP.
YDH_EXPRESS	YDH express.
PARCELINKLOGISTICS	Parcelink Logistics.
CNDEXPRESS	CND Express.
NOX_NIGHT_TIME_EXPRESS	NOX NightTimeExpress.
AERONET	Aeronet couriers.
LTIANEXP	LTIAN EXP.
INTEGRA2_FTP	Integra2.
PARCELONE	PARCEL ONE.
NOX_NACHTEXPRESS	Innight Express Germany GmbH (nox NachtExpress).
CN_CHINA_POST_EMS	China Post.
CHUKOU1	Chukou1.
GLS_SLOV	GLS General Logistics Systems Slovakia s.r.o..
ORANGE_DS	OrangeDS (Orange Distribution Solutions Inc).
JOOM_LOGIS	Joom Logistics.
AUS_STARTRACK	StarTrack (startrack.com.au).
DHL	dhl Global.
GB_APC	APC postal logistics germany.
BONDSCOURIERS	Bonds Courier Service (bondscouriers.com.au).
JPN_JAPAN_POST	Japan Post.
USPS	United States Postal Service.
WINIT	WinIt.
ARG_OCA	OCA Argentina.
TW_TAIWAN_POST	Taiwan Post.
DMM_NETWORK	DMM Network.
TNT	TNT Express.
BH_POSTA	BH Posta (www.posta.ba).
SWE_POSTNORD	Postnord sweden.
CA_CANADA_POST	Canada Post.
WISELOADS	Wiseloads.
ASENDIA_HK	Asendia HonKong.
NLD_GLS	GLS Netherland.
MEX_REDPACK	Redpack.
JET_SHIP	Jet-Ship Worldwide.
DE_DHL_EXPRESS	DHL Express.
NINJAVAN_THAI	Ninja van Thai.
RABEN_GROUP	Raben Group.
ESP_ASM	ASM(GLS Spain).
HRV_HRVATSKA	Hrvatska posta.
GLOBAL_ESTES	Estes Express Lines.
LTU_LIETUVOS	Lietuvos pastas.
BEL_DHL	DHL Benelux.
AU_AU_POST	Australia Post.
SPEEDEXCOURIER	SPEEDEX couriers.
FR_COLIS	Colissimo.
ARAMEX	Aramex.
DPEX	DPEX (www.dpex.com).
MYS_AIRPAK	Airpak Express.
CUCKOOEXPRESS	Cuckoo Express.
DPD_POLAND	DPD Poland.
NLD_POSTNL	PostNL International.
NIM_EXPRESS	Nim Express.
QUANTIUM	Quantium.
SENDLE	Sendle.
ESP_REDUR	Redur Spain.
MATKAHUOLTO	Matkahuolto.
CPACKET	Cpacket couriers.
POSTI	Posti courier.
HUNTER_EXPRESS	Hunter Express.
CHOIR_EXP	Choir Express Indonesia.
LEGION_EXPRESS	Legion Express.
AUSTRIAN_POST_EXPRESS	austrian post.
GRUPO	Grupo ampm.
POSTA_RO	Post Roman (www.posta-romana.ro).
INTERPARCEL_UK	Interparcel UK.
GLOBAL_ABF	ABF Freight.
POSTEN_NORGE	Posten Norge (www.posten.no).
XPERT_DELIVERY	Xpert Delivery.
DHL_REFR	DHl (Reference number).
DHL_HK	DHL HonKong.
SKYNET_UAE	SKYNET UAE.
GOJEK	Gojek.
YODEL_INTNL	Yodel International.
JANCO	Janco Ecommerce.
YTO	YTO Express.
WISE_EXPRESS	Wise Express.
JTEXPRESS_VN	J&T Express Vietnam.
FEDEX_INTL_MLSERV	FedEx International MailService.
VAMOX	VAMOX.
AMS_GRP	AMS Group.
DHL_JP	DHL Japan.
HRPARCEL	HR Parcel.
GESWL	GESWL Express.
BLUESTAR	Blue Star.
CDEK_TR	CDEK TR.
DESCARTES	Innovel courier.
DELTEC_UK	Deltec Courier.
DTDC_EXPRESS	DTDC express.
TOURLINE	tourline.
BH_WORLDWIDE	B&H Worldwide.
OCS	OCS ANA Group.
YINGNUO_LOGISTICS	yingnuo logistics.
UPS	United Parcel Service.
TOLL	Toll IPEC.
PRT_SEUR	SEUR portugal.
DTDC_AU	DTDC Australia.
THA_DYNAMIC_LOGISTICS	Dynamic Logistics.
UBI_LOGISTICS	UBI Smart Parcel.
FEDEX_CROSSBORDER	FedEx Cross Border.
A1POST	A1Post.
TAZMANIAN_FREIGHT	Tazmanian Freight Systems.
CJ_INT_MY	CJ International malaysia.
SAIA_FREIGHT	Saia LTL Freight.
SG_QXPRESS	Qxpress.
NHANS_SOLUTIONS	Nhans Solutions.
DPD_FR	DPD France.
COORDINADORA	Coordinadora.
ANDREANI	Grupo logistico Andreani.
DOORA	Doora Logistics.
INTERPARCEL_NZ	Interparcel New Zealand.
PHL_JAMEXPRESS	Jam Express Philippines.
BEL_BELGIUM_POST	bel_belgium_post.
US_APC	us_apc.
IDN_POS	idn_pos.
FR_MONDIAL	fr_mondial.
DE_DHL	DE DHL.
HK_RPX	hk_rpx.
DHL_PIECEID	dhl_pieceid.
VNPOST_EMS	vnpost_ems.
RRDONNELLEY	rrdonnelley.
DPD_DE	dpd_de.
DELCART_IN	delcart_in.
IMEXGLOBALSOLUTIONS	imexglobalsolutions.
ACOMMERCE	ACOMMERCE.
EURODIS	eurodis.
CANPAR	CANPAR.
GLS	GLS.
IND_ECOM	Ecom Express.
ESP_ENVIALIA	Envialia.
DHL_UK	dhl UK.
SMSA_EXPRESS	SMSA Express.
TNT_FR	TNT France.
DEX_I	DEX-I courier.
BUDBEE_WEBHOOK	Budbee courier.
COPA_COURIER	Copa Airlines Courier.
VNM_VIETNAM_POST	Vietnam Post.
DPD_HK	DPD HongKong.
TOLL_NZ	Toll New Zealand.
ECHO	Echo courier.
FEDEX_FR	FedEx® Freight.
BORDEREXPRESS	Border Express.
MAILPLUS_JPN	MailPlus (Japan).
TNT_UK_REFR	TNT UK Reference.
KEC	KEC courier.
DPD_RO	DPD Romania.
TNT_JP	TNT_JP.
TH_CJ	TH_CJ.
EC_CN	EC_CN.
FASTWAY_UK	FASTWAY_UK.
FASTWAY_US	FASTWAY_US.
GLS_DE	GLS_DE.
GLS_ES	GLS_ES.
GLS_FR	GLS_FR.
MONDIAL_BE	MONDIAL_BE.
SGT_IT	SGT_IT.
TNT_CN	TNT_CN.
TNT_DE	TNT_DE.
TNT_ES	TNT_ES.
TNT_PL	TNT_PL.
PARCELFORCE	PARCELFORCE.
SWISS_POST	SWISS POST.
TOLL_IPEC	TOLL IPEC.
AIR_21	AIR 21.
AIRSPEED	AIRSPEED.
BERT	BERT.
BLUEDART	BLUEDART.
COLLECTPLUS	COLLECTPLUS.
COURIERPLUS	COURIERPLUS.
COURIER_POST	COURIER POST.
DHL_GLOBAL_MAIL	dhl_global_mail.
DPD_UK	dpd_uk.
DELTEC_DE	DELTEC DE.
DEUTSCHE_DE	deutsche_de.
DOTZOT	DOTZOT.
ELTA_GR	elta_gr.
EMS_CN	ems_cn.
ECARGO	ECARGO.
ENSENDA	ENSENDA.
FERCAM_IT	fercam_it.
FASTWAY_ZA	fastway_za.
FASTWAY_AU	fastway_au.
FIRST_LOGISITCS	first_logisitcs.
GEODIS	GEODIS.
GLOBEGISTICS	GLOBEGISTICS.
GREYHOUND	GREYHOUND.
JETSHIP_MY	jetship_my.
LION_PARCEL	LION PARCEL.
AEROFLASH	AEROFLASH.
ONTRAC	ONTRAC.
SAGAWA	SAGAWA.
SIODEMKA	SIODEMKA.
STARTRACK	startrack.
TNT_AU	tnt_au.
TNT_IT	tnt_it.
TRANSMISSION	TRANSMISSION.
YAMATO	YAMATO.
DHL_IT	dhl_it.
DHL_AT	dhl_at.
LOGISTICSWORLDWIDE_KR	LOGISTICSWORLDWIDE KR.
GLS_SPAIN	gls_spain.
AMAZON_UK_API	amazon_uk_api.
DPD_FR_REFERENCE	dpd_fr_reference.
DHLPARCEL_UK	dhlparcel_uk.
MEGASAVE	megasave.
QUALITYPOST	qualitypost.
IDS_LOGISTICS	ids_logistics.
JOYINGBOX	joyingbox.
PANTHER_ORDER_NUMBER	panther_order_number.
WATKINS_SHEPARD	watkins_shepard.
FASTTRACK	fasttrack.
UP_EXPRESS	up_express.
ELOGISTICA	elogistica.
ECOURIER	ecourier.
CJ_PHILIPPINES	cj_philippines.
SPEEDEX	speedex.
ORANGECONNEX	orangeconnex.
TECOR	tecor.
SAEE	saee.
GLS_ITALY_FTP	gls_italy_ftp.
DELIVERE	delivere.
YYCOM	yycom.
ADICIONAL_PT	Adicional Logistics.
DKSH	DKSH.
NIPPON_EXPRESS_FTP	Nippon Express.
GOLS	GO Logistics & Storage.
FUJEXP	FUJIE EXPRESS.
QTRACK	QTrack.
OMLOGISTICS_API	OM LOGISTICS LTD.
GDPHARM	GDPharm Logistics.
MISUMI_CN	MISUMI Group Inc..
AIR_CANADA	Rivo.
CITY56_WEBHOOK	City Express.
SAGAWA_API	Sagawa.
KEDAEX	KedaEX.
PGEON_API	Pgeon.
WEWORLDEXPRESS	We World Express.
JT_LOGISTICS	J&T International logistics.
TRUSK	Trusk France.
VIAXPRESS	ViaXpress.
DHL_SUPPLYCHAIN_ID	DHL Supply Chain Indonesia.
ZUELLIGPHARMA_SFTP	Zuellig Pharma Korea.
MEEST	Meest.
TOLL_PRIORITY	Toll Priority.
MOTHERSHIP_API	Mothership.
CAPITAL	Capital Transport.
EUROPAKET_API	Europacket+.
HFD	HFD.
TOURLINE_REFERENCE	Tourline Express.
GIO_ECOURIER	GIO Express Inc.
CN_LOGISTICS	CN Logistics.
PANDION	Pandion.
BPOST_API	Bpost API.
PASSPORTSHIPPING	Passport Shipping.
PAKAJO	Pakajo World.
DACHSER	DACHSER.
YUSEN_SFTP	Yusen Logistics.
SHYPLITE	Shypmax.
XYY	Xingyunyi Logistics.
MWD	Metropolitan Warehouse & Delivery.
FAXECARGO	Faxe Cargo.
MAZET	Groupe Mazet.
FIRST_LOGISTICS_API	First Logistics.
SPRINT_PACK	SPRINT PACK.
HERMES_DE_FTP	Hermes Germany.
CONCISE	Concise.
KERRY_EXPRESS_TW_API	Kerry Express TaiWan.
EWE	EWE Global Express.
FASTDESPATCH	Fast Despatch Logistics Limited.
ABCUSTOM_SFTP	AB Custom Group.
CHAZKI	Chazki.
SHIPPIE	Shippie.
GEODIS_API	GEODIS - Distribution & Express.
NAQEL_EXPRESS	Naqel Express.
PAPA_WEBHOOK	Papa.
FORWARDAIR	Forward Air.
DIALOGO_LOGISTICA_API	Dialogo Logistica.
LALAMOVE_API	Lalamove.
TOMYDOOR	Tomydoor.
KRONOS_WEBHOOK	Kronos Express.
JTCARGO	J&T CARGO.
T_CAT	T-cat.
CONCISE_WEBHOOK	Concise.
TELEPORT_WEBHOOK	Teleport.
CUSTOMCO_API	The Custom Companies.
SPX_TH	Shopee Xpress.
BOLLORE_LOGISTICS	Bollore Logistics.
CLICKLINK_SFTP	ClickLink.
M3LOGISTICS	M3 Logistics.
VNPOST_API	Vietnam Post.
AXLEHIRE_FTP	Axlehire.
SHADOWFAX	Shadowfax.
MYHERMES_UK_API	EVRi.
DAIICHI	Daiichi Freight System Inc.
MENSAJEROSURBANOS_API	Mensajeros Urbanos.
POLARSPEED	PolarSpeed Inc.
IDEXPRESS_ID	iDexpress Indonesia.
PAYO	Payo.
WHISTL_SFTP	Whistl.
INTEX_DE	INTEX Paketdienst GmbH.
TRANS2U	Trans2u.
PRODUCTCAREGROUP_SFTP	Product Care Services Limited.
BIGSMART	Big Smart.
EXPEDITORS_API_REF	Expeditors API Reference.
AITWORLDWIDE_API	AIT.
WORLDCOURIER	World Courier.
QUIQUP	Quiqup.
AGEDISS_SFTP	Agediss.
ANDREANI_API	Andreani.
CRLEXPRESS	CRL Express.
SMARTCAT	SMARTCAT.
CROSSFLIGHT	Crossflight Limited.
PROCARRIER	Pro Carrier.
DHL_REFERENCE_API	DHL (Reference number).
SEINO_API	Seino.
WSPEXPRESS	WSP Express.
KRONOS	Kronos Express.
TOTAL_EXPRESS_API	Total Express.
PARCLL	PARCLL.
XPEDIGO	Xpedigo.
STAR_TRACK_WEBHOOK	StarTrack.
GPOST	Georgian Post.
UCS	UCS.
DMFGROUP	DMF.
COORDINADORA_API	Coordinadora.
MARKEN	Marken.
NTL	NTL logistics.
REDJEPAKKETJE	Red je Pakketje.
ALLIED_EXPRESS_FTP	Allied Express (FTP).
MONDIALRELAY_ES	Mondial Relay Spain(Punto Pack).
NAEKO_FTP	Naeko Logistics.
MHI	Mhi.
SHIPPIFY	Shippify, Inc.
MALCA_AMIT_API	Malca Amit.
JTEXPRESS_SG_API	J&T Express Singapore.
DACHSER_WEB	DACHSER.
FLIGHTLG	Flight Logistics Group.
CAGO	Cago.
COM1EXPRESS	ComOne Express.
TONAMI_FTP	Tonami.
PACKFLEET	PACKFLEET.
PUROLATOR_INTERNATIONAL	Purolator International.
WINESHIPPING_WEBHOOK	Wineshipping.
DHL_ES_SFTP	DHL Spain Domestic.
PCHOME_API	網家速配股份有限公司.
CESKAPOSTA_API	Czech Post.
GORUSH	Go Rush.
HOMERUNNER	HomeRunner.
AMAZON_ORDER	Amazon order.
EFWNOW_API	Estes Forwarding Worldwide.
CBL_LOGISTICA_API	CBL Logistica (API).
NIMBUSPOST	NimbusPost.
LOGWIN_LOGISTICS	Logwin Logistics.
NOWLOG_API	Sequoialog.
DPD_NL	DPD Netherlands.
GODEPENDABLE	Dependable Supply Chain Services.
ESDEX	Top Ideal Express.
LOGISYSTEMS_SFTP	Kiitäjät.
EXPEDITORS	Expeditors.
SNTGLOBAL_API	Snt Global Etrax.
SHIPX	ShipX.
QINTL_API	Quickstat Courier LLC.
PACKS	Packs.
POSTNL_INTERNATIONAL	PostNL International.
AMAZON_EMAIL_PUSH	Amazon.
DHL_API	DHL.
SPX	Shopee Express.
AXLEHIRE	AxleHire.
ICSCOURIER	ICS COURIER.
DIALOGO_LOGISTICA	Dialogo Logistica.
SHUNBANG_EXPRESS	ShunBang Express.
TCS_API	TCS.
SF_EXPRESS_CN	SF Express China.
PACKETA	Packeta.
SIC_TELIWAY	Teliway SIC Express.
MONDIALRELAY_FR	Mondial Relay France.
INTIME_FTP	InTime.
JD_EXPRESS	京东物流.
FASTBOX	Fastbox.
PATHEON	Patheon Logistics.
INDIA_POST	India Post Domestic.
TIPSA_REF	Tipsa Reference.
ECOFREIGHT	Eco Freight.
VOX	VOX SOLUCION EMPRESARIAL SRL.
DIRECTFREIGHT_AU_REF	Direct Freight Express.
BESTTRANSPORT_SFTP	Best Transport.
AUSTRALIA_POST_API	Australia Post.
FRAGILEPAK_SFTP	FragilePAK.
FLIPXP	FlipXpress.
VALUE_WEBHOOK	Value Logistics.
DAESHIN	Daeshin.
SHERPA	Sherpa.
MWD_API	Metropolitan Warehouse & Delivery.
SMARTKARGO	SmartKargo.
DNJ_EXPRESS	DNJ Express.
GOPEOPLE	Go People.
MYSENDLE_API	mySendle.
ARAMEX_API	Aramex.
PIDGE	Pidge.
THAIPARCELS	TP Logistic.
PANTHER_REFERENCE_API	Panther Reference.
POSTAPLUS	Posta Plus.
BUFFALO	BUFFALO.
U_ENVIOS	U-ENVIOS.
ELITE_CO	Elite Express.
ROCHE_INTERNAL_SFTP	Roche Internal Courier.
DBSCHENKER_ICELAND	DB Schenker Iceland.
TNT_FR_REFERENCE	TNT France Reference.
NEWGISTICSAPI	Newgistics API.
GLOVO	Glovo.
GWLOGIS_API	G.I.G.
SPREETAIL_API	Spreetail.
MOOVA	Moova.
PLYCONGROUP	Plycon Transportation Group.
USPS_WEBHOOK	USPS Informed Visibility - Webhook.
REIMAGINEDELIVERY	maergo.
EDF_FTP	Eurodifarm.
DAO365	DAO365.
BIOCAIR_FTP	BioCair.
RANSA_WEBHOOK	Ransa.
SHIPXPRES	SHIPXPRESS.
COURANT_PLUS_API	Courant Plus.
SHIPA	SHIPA.
HOMELOGISTICS	Home Logistics.
DX	DX.
POSTE_ITALIANE_PACCOCELERE	Poste Italiane Paccocelere.
TOLL_WEBHOOK	Toll Group.
LCTBR_API	LCT do Brasil.
DX_FREIGHT	DX Freight.
DHL_SFTP	DHL Express.
SHIPROCKET	Shiprocket X.
UBER_WEBHOOK	Uber.
STATOVERNIGHT	Stat Overnight.
BURD	Burd Delivery.
FASTSHIP	Fastship Express.
IBVENTURE_WEBHOOK	IB Venture.
GATI_KWE_API	Gati-KWE.
CRYOPDP_FTP	CryoPDP.
HUBBED	HUBBED.
TIPSA_API	Tipsa API.
ARASKARGO	Aras Cargo.
THIJS_NL	Thijs Logistiek.
ATSHEALTHCARE_REFERENCE	ATS Healthcare.
99MINUTOS	99minutos.
HELLENIC_POST	Hellenic (Greece) Post.
HSM_GLOBAL	HSM Global.
MNX	MNX.
NMTRANSFER	N&M Transfer Co., Inc..
LOGYSTO	Logysto.
INDIA_POST_INT	India Post International.
AMAZON_FBA_SWISHIP_IN	Swiship IN.
SRT_TRANSPORT	SRT Transport.
BOMI	Bomi Group.
DELIVERR_SFTP	Deliverr.
HSDEXPRESS	HSDEXPRESS.
SIMPLETIRE_WEBHOOK	SimpleTire.
HUNTER_EXPRESS_SFTP	Hunter Express.
UPS_API	UPS.
WOOYOUNG_LOGISTICS_SFTP	WOO YOUNG LOGISTICS CO.,LTD..
PHSE_API	PHSE.
WISH_EMAIL_PUSH	Wish.
NORTHLINE	Northline.
MEDAFRICA	Med Africa Logistics.
DPD_AT_SFTP	DPD Austria.
ANTERAJA	Anteraja.
DHL_GLOBAL_FORWARDING_API	DHL Global Forwarding API.
LBCEXPRESS_API	LBC EXPRESS INC..
SIMSGLOBAL	Sims Global.
CDLDELIVERS	CDL Last Mile.
TYP	TYP.
TESTING_COURIER_WEBHOOK	Testing Courier.
PANDAGO_API	Pandago.
ROYAL_MAIL_FTP	Royal Mail.
THUNDEREXPRESS	Thunder Express Australia.
SECRETLAB_WEBHOOK	Secretlab.
SETEL	Setel Express.
JD_WORLDWIDE	JD Worldwide.
DPD_RU_API	DPD Russia.
ARGENTS_WEBHOOK	Argents Express Group.
POSTONE	Post ONE.
TUSKLOGISTICS	Tusk Logistics.
RHENUS_UK_API	Rhenus Logistics UK.
TAQBIN_SG_API	Yamato Singapore.
INNTRALOG_SFTP	Inntralog GmbH.
DAYROSS	Day & Ross.
CORREOSEXPRESS_API	Correos Express (API).
INTERNATIONAL_SEUR_API	International Seur API.
YODEL_API	Yodel API.
HEROEXPRESS	Hero Express.
DHL_SUPPLYCHAIN_IN	DHL supply chain India.
URGENT_CARGUS	Urgent Cargus.
FRONTDOORCORP	FRONTdoor Collective.
JTEXPRESS_PH	J&T Express Philippines.
PARCELSTARS_WEBHOOK	Parcelstars.
DPD_SK_SFTP	DPD Slovakia.
MOVIANTO	Movianto.
OZEPARTS_SHIPPING	Ozeparts Shipping.
KARGOMKOLAY	KargomKolay (CargoMini).
TRUNKRS	Trunkrs.
OMNIRPS_WEBHOOK	Omni Returns.
CHILEXPRESS	Chile Express.
TESTING_COURIER	Testing Courier.
JNE_API	JNE (API).
BJSHOMEDELIVERY_FTP	BJS Distribution, Storage & Couriers - FTP.
DEXPRESS_WEBHOOK	D Express.
USPS_API	USPS API.
TRANSVIRTUAL	TransVirtual.
SOLISTICA_API	solistica.
CHIENVENTURE_WEBHOOK	Chienventure.
DPD_UK_SFTP	DPD UK.
INPOST_UK	InPost.
JAVIT	Javit.
ZTO_DOMESTIC	ZTO Express China.
DHL_GT_API	DHL Global Forwarding Guatemala.
CEVA_TRACKING	CEVA Package.
KOMON_EXPRESS	Komon Express.
EASTWESTCOURIER_FTP	East West Courier Pte Ltd.
DANNIAO	Danniao.
SPECTRAN	Spectran.
DELIVER_IT	Deliver-iT.
RELAISCOLIS	Relais Colis.
GLS_SPAIN_API	GLS Spain.
POSTPLUS	PostPlus.
AIRTERRA	Airterra.
GIO_ECOURIER_API	GIO Express Ecourier.
DPD_CH_SFTP	DPD Switzerland.
FEDEX_API	FedEx®.
INTERSMARTTRANS	INTERSMARTTRANS & SOLUTIONS SL.
HERMES_UK_SFTP	Hermes UK.
EXELOT_FTP	Exelot Ltd..
DHL_PA_API	DHL GLOBAL FORWARDING PANAMÁ.
VIRTRANSPORT_SFTP	Vir Transport.
WORLDNET	Worldnet Logistics.
INSTABOX_WEBHOOK	Instabox.
KNG	Keuhne + Nagel Global.
FLASHEXPRESS_WEBHOOK	Flash Express.
MAGYAR_POSTA_API	Magyar Posta.
WESHIP_API	WeShip.
OHI_WEBHOOK	Ohi.
MUDITA	MUDITA.
BLUEDART_API	Bluedart.
T_CAT_API	T-cat.
ADS	ADS Express.
HERMES_IT	HR Parcel.
FITZMARK_API	FitzMark.
POSTI_API	Posti API.
SMSA_EXPRESS_WEBHOOK	SMSA Express.
TAMERGROUP_WEBHOOK	Tamer Logistics.
LIVRAPIDE	Livrapide.
NIPPON_EXPRESS	Nippon Express.
BETTERTRUCKS	Better Trucks.
FAN	FAN COURIER EXPRESS.
PB_USPSFLATS_FTP	USPS Flats (Pitney Bowes).
PARCELRIGHT	Parcel Right.
ITHINKLOGISTICS	iThink Logistics.
KERRY_EXPRESS_TH_WEBHOOK	Kerry Logistics.
ECOUTIER	eCoutier.
SHOWL	SENHONG INTERNATIONAL LOGISTICS.
BRT_IT_API	BRT Bartolini API.
RIXONHK_API	Rixon Logistics.
DBSCHENKER_API	DB Schenker.
ILYANGLOGIS	Ilyang logistics.
MAIL_BOX_ETC	Mail Boxes Etc..
WESHIP	WeShip.
DHL_GLOBAL_MAIL_API	DHL eCommerce Solutions.
ACTIVOS24_API	Activos24.
ATSHEALTHCARE	ATS Healthcare.
LUWJISTIK	Luwjistik.
GW_WORLD	Gebrüder Weiss.
FAIRSENDEN_API	fairsenden.
SERVIP_WEBHOOK	SerVIP.
SWISHIP	Swiship.
TANET	Transport Ambientales.
HOTSIN_CARGO	SHENZHEN HOTSIN CARGO INT'L FORWARDING CO.,LTD.
DIREX	Direx.
HUANTONG	HuanTong.
IMILE_API	iMile.
AUEXPRESS	Au Express.
NYTLOGISTICS	NYT SUPPLY CHAIN LOGISTICS Co.,LTD.
DSV_REFERENCE	DSV Futurewave.
NOVOFARMA_WEBHOOK	Novofarma.
AITWORLDWIDE_SFTP	AIT.
SHOPOLIVE	Olive.
FNF_ZA	Fast & Furious.
DHL_ECOMMERCE_GC	DHL eCommerce Greater China.
FETCHR	Fetchr.
STARLINKS_API	Starlinks Global.
YYEXPRESS	YYEXPRESS.
SERVIENTREGA	Servientrega.
HANJIN	HanJin.
SPANISH_SEUR_FTP	Spanish Seur.
DX_B2B_CONNUM	DX (B2B).
HELTHJEM_API	Helthjem.
INEXPOST	Inexpost.
A2B_BA	A2B Express Logistics.
RHENUS_GROUP	Rhenus Logistics.
SBERLOGISTICS_RU	Sber Logistics.
MALCA_AMIT	Malca-Amit.
PPL	Professional Parcel Logistics.
OSM_WORLDWIDE_SFTP	OSM Worldwide.
ACILOGISTIX	ACI Logistix.
OPTIMACOURIER	Optima Courier.
NOVA_POSHTA_API	Nova Poshta API.
LOGGI	Loggi.
YIFAN	YiFan Express.
MYDYNALOGIC	My DynaLogic.
MORNINGLOBAL	Morning Global.
CONCISE_API	Concise.
FXTRAN	Falcon Express.
DELIVERYOURPARCEL_ZA	Deliver Your Parcel.
UPARCEL	uParcel.
MOBI_BR	Mobi Logistica.
LOGINEXT_WEBHOOK	T&W Delivery.
EMS	EMS.
SPEEDY	Speedy.
ZOOM_RED	Zoom.
NAVLUNGO	Navlungo.
CASTLEPARCELS	Castle Parcels.
WEEE	Weee.
PACKALY	Packaly.
YUNHUIPOST	Yunhuipost.
YOUPARCEL	YouParcel.
LEMAN	Leman.
MOOVIN	Moovin.
URB_IT	Urb-it.
MULTIENTREGAPANAMA	Multientrega.
JUSDASR	Jusdasr.
DISCOUNTPOST	Discount Post.
RHENUS_UK	Rhenus Logistics UK.
SWISHIP_JP	Swiship JP.
GLS_US	GLS USA.
SMTL	Southwestern Motor Transport. Inc.
EMEGA	Discount Post Emega.
EXPRESSONE_SV	EXPRESSONE Slovenia.
HEPSIJET	hepsiJET.
WELIVERY	Welivery.
BRINGER	Bringer Parcel Services.
EASYROUTES	EasyRoutes.
MRW	MRW.
RPM	RPM.
DPD_PRT	DPD Portugal.
GLS_ROMANIA	GLS Romania.
LMPARCEL	LM Parcel.
GTAGSM	GTA GSM.
DOMINO	DOMINO.
ESHIPPER	eShipper.
TRANSPAK	Transpak Inc..
XINDUS	Xindus.
AOYUE	Aoyue.
EASYPARCEL	Easyparcel.
EXPRESSONE	EXPRESSONE.
SENDEO_KARGO	Sendeo Kargo.
SPEEDAF	Speedaf Express.
ETOWER	eTower.
GCX	GC Express.
NINJAVAN_VN	Ninjavan Vietnam.
ALLEGRO	Allegro.
JUMPPOINT	Jumppoint.
SHIPGLOBAL_US	ShipGlobal.
KINISI	Kinisi Transport Pty Ltd.
OAKH	Oakh Harbour Freight Lines.
AWEST	American West.
BARSAN	Barsan Global Lojistik.
ENERGOLOGISTIC	Energo Logistic.
MADROOEX	Madrooex.
GOBOLT	GoBolt.
SWISS_UNIVERSAL_EXPRESS	Swiss Universal Express.
IORDIRECT	IOR Direct Solutions.
XMSZM	xmszm.
GLS_HUN	GLS Hungary.
SENDY	Sendy Express.
BRAUNSEXPRESS	Brauns Express.
GRANDSLAMEXPRESS	Grand Slam Express.
XGS	XGS.
OTSCHILE	OTS.
PACK_UP	Pack-Up.
PARCELSTARS	Parcelstars.
TEAMEXPRESSLLC	Team Express Service LLC.
ASYADEXPRESS	Asyad Express.
TDN	TDN.
EARLYBIRD	Early Bird.
CACESA	Cacesa.
PARCELJET	Parceljet.
MNG_KARGO	MNG Kargo.
SUPERPACKLINE	Super Pac Line.
SPEEDX	SpeedX.
VESYL	Vesyl.
SKYKING	Sky King.
DIRMENSAJERIA	DIR.
NETLOGIXGROUP	Netlogix.
ZYOU	ZYEX.
JAWAR	Jawar.
AGSYSTEMS	Associate Global Systems.
GPS	GPS.
PTT_KARGO	PTT Kargo.
MAERGO	Maergo.
ARIHANTCOURIER	AICS.
VTFE	VicTas Freight Express.
YUNANT	Yunant.
URBIFY	Urbify.
PACK_MAN	pack-man.
LIEFERGRUN	LIEFERGRUN.
OBIBOX	Obibox.
PAIKEDA	Paikeda.
SCOTTY	Scotty.
INTELCOM_CA	Intelcom.
SWE	swe.
ASENDIA	Asendia Global.
DPD_AT	DPD Austria.
RELAY	Relay.
ATA	ATA.
SKYEXPRESS_INTERNATIONAL	SkyExpress Internationals.
SURAT_KARGO	Surat Kargo.
SGLINK	SG LINK.
FLEETOPTICSINC	FleetOptics.
SHOPLINE	shopline.
PIGGYSHIP	PIGGYSHIP.
LOGOIX	LogoiX.
KOLAY_GELSIN	Kolay Gelsin.
ASSOCIATED_COURIERS	Associated Couriers.
UPS_CHECKER	ups-checker.
WINESHIPPING	Wineshipping.
SPEDISCI	Spedisci online.
FOURKITES	Fourkites.
ETONAS	Etonas.
FINMILE	Fin Mile.
UNIUNI	Uniuni.
RODONAVES	Rodonaves.
INPOST_IT	Inpost Italy.
TFORCE_FREIGHT	Tforce Freight.
RICHMOM	Rich Mom.
FRANCO	Corriere Franco.
ECPARCEL	Ecparcel.
FEDEX_CHINA	Fedex China.
GOFO_EXPRESS	Gofo Express.
SHIPBOB	Shipbob.
JERSEYPOST_ATLAS	Jersey Post Group.
CORETRAILS	Coretrails.
RHENUS_ITALY	Rhenus Logistics Italy.
JADLOG	Jadlog.
JITSU	Jitsu.
YANWEN_EXPRESS	Yanwen Express.
DASHLINK	Dashlink.
SEINO_SUPER_EXPRESS	Seino Super Express.
FLOSHIP	Floship.
METROSCG	Metro Supply Chain.
SENDPARCEL	Sendparcel.
P2P	P2p.
CN_EXPRESS	Cn Express.
CIRROTRACK	Cirro Track.
LAND_LOGISTICS	Land Logistics.
VEHO	Veho.
MEDLINE	Medline.
VDTRACK	Vdtrack.
SINO_SCM	Sino Scm.
3PE_EXPRESS	3pe Express.
SWIFTX	Swiftx.
SFYDEXPRESS	Sfyd Express.
TOPTRANS	Toptrans.
Copy
{
"tracking_number": "string",
"carrier_name_other": "string",
"carrier": "DPD_RU"
}
Tracker Status
The status of the item shipment.

string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The status of the item shipment.

Enum Value	Description
CANCELLED	The shipment was cancelled and the tracking number no longer applies.
SHIPPED	The merchant has assigned a tracking number to the items being shipped from the Order. This does not correspond to the carrier's actual status for the shipment. The latest status of the parcel must be retrieved from the carrier.
Copy
"CANCELLED"
tracker_item
The details of the items in the shipment.

name	
string [ 1 .. 127 ] characters
The item name or title.

quantity	
string [ 1 .. 10 ] characters
The item quantity. Must be a whole number.

sku	
string [ 1 .. 127 ] characters
The stock keeping unit (SKU) for the item. This can contain unicode characters.

url	
string [ 1 .. 2048 ] characters
The URL to the item being purchased. Visible to buyer and used in buyer experiences.

image_url	
string [ 1 .. 2048 ] characters ^(https:)([/|.|\w|\s|-])*\.(?:jpg|gif|png|jpe...Show pattern
The URL of the item's image. File type and size restrictions apply. An image that violates these restrictions will not be honored.

upc	
object
The Universal Product Code of the item.

CopyExpand allCollapse all
{
"name": "string",
"quantity": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
}
}
tracker_request
The tracking details of an order.

tracking_number
required
string [ 1 .. 64 ] characters
The tracking number for the shipment. This property supports Unicode.

carrier_name_other	
string [ 1 .. 64 ] characters
The name of the carrier for the shipment. Provide this value only if the carrier parameter is OTHER. This property supports Unicode.

carrier
required
string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The carrier for the shipment. Some carriers have a global version as well as local subsidiaries. The subsidiaries are repeated over many countries and might also have an entry in the global list. Choose the carrier for your country. If the carrier is not available for your country, choose the global version of the carrier. If your carrier name is not in the list, set carrier to OTHER and set carrier name in carrier_name_other. For allowed values, see Carriers.

Enum Value	Description
DPD_RU	DPD Russia.
BG_BULGARIAN_POST	Bulgarian Posts.
KR_KOREA_POST	Koreapost (www.koreapost.go.kr).
ZA_COURIERIT	Courier IT.
FR_EXAPAQ	DPD France (formerly exapaq).
ARE_EMIRATES_POST	Emirates Post.
GAC	GAC.
GEIS	Geis CZ.
SF_EX	SF Express.
PAGO	Pago Logistics.
MYHERMES	MyHermes UK.
DIAMOND_EUROGISTICS	Diamond Eurogistics Limited.
CORPORATECOURIERS_WEBHOOK	Corporate Couriers.
BOND	Bond courier.
OMNIPARCEL	Omni Parcel.
SK_POSTA	Slovenska pošta.
PUROLATOR	purolator.
FETCHR_WEBHOOK	Mena 360 (Fetchr).
THEDELIVERYGROUP	TDG – The Delivery Group.
CELLO_SQUARE	Cello Square.
TARRIVE	TONDA GLOBAL.
COLLIVERY	MDS Collivery Pty (Ltd).
MAINFREIGHT	Mainfreight.
IND_FIRSTFLIGHT	First Flight Couriers.
ACSWORLDWIDE	ACS Worldwide Express.
AMSTAN	Amstan Logistics.
OKAYPARCEL	OkayParcel.
ENVIALIA_REFERENCE	Envialia Reference.
SEUR_ES	Seur Spain.
CONTINENTAL	Continental.
FDSEXPRESS	FDSEXPRESS.
AMAZON_FBA_SWISHIP	Swiship UK.
WYNGS	Wyngs.
DHL_ACTIVE_TRACING	DHL Active Tracing.
ZYLLEM	Zyllem.
RUSTON	Ruston.
XPOST	Xpost.ph.
CORREOS_ES	correos Express (www.correos.es).
DHL_FR	DHL France (www.dhl.com).
PAN_ASIA	Pan-Asia International.
BRT_IT	BRT couriers Italy (www.brt.it).
SRE_KOREA	SRE Korea (www.srekorea.co.kr).
SPEEDEE	Spee-Dee Delivery.
TNT_UK	TNT UK Limited (www.tnt.com).
VENIPAK	Venipak.
SHREENANDANCOURIER	SHREE NANDAN COURIER.
CROSHOT	Croshot.
NIPOST_NG	NIpost (www.nipost.gov.ng).
EPST_GLBL	ePost Global.
NEWGISTICS	Newgistics.
POST_SLOVENIA	Post of Slovenia.
JERSEY_POST	Jersey Post.
BOMBINOEXP	Bombino Express Pvt.
WMG	WMG Delivery.
XQ_EXPRESS	XQ Express.
FURDECO	Furdeco.
LHT_EXPRESS	LHT Express.
SOUTH_AFRICAN_POST_OFFICE	South African Post Office.
SPOTON	SPOTON Logistics Pvt Ltd.
DIMERCO	Dimerco Express Group.
CYPRUS_POST_CYP	cyprus post.
ABCUSTOM	AB Custom Group.
IND_DELIVREE	deliverE.
CN_BESTEXPRESS	Best Express.
DX_SFTP	DX (SFTP).
PICKUPP_MYS	PICK UPP.
FMX	FMX.
HELLMANN	Hellmann Worldwide Logistics.
SHIP_IT_ASIA	Ship It Asia.
KERRY_ECOMMERCE	Kerry eCommerce.
FRETERAPIDO	Frete Rapido.
PITNEY_BOWES	Pitney Bowes.
XPRESSEN_DK	Xpressen courier.
SEUR_SP_API	Spanish Seur API.
DELIVERYONTIME	DELIVERYONTIME LOGISTICS PVT LTD.
JINSUNG	JINSUNG TRADING.
TRANS_KARGO	Trans Kargo Internasional.
SWISHIP_DE	Swiship DE.
IVOY_WEBHOOK	Ivoy courier.
AIRMEE_WEBHOOK	Airmee couriers.
DHL_BENELUX	dhl benelux.
FIRSTMILE	FirstMile.
FASTWAY_IR	Fastway Ireland.
HH_EXP	Hua Han Logistics.
MYS_MYPOST_ONLINE	Mypostonline.
TNT_NL	THT Netherland.
TIPSA	TIPSA courier.
TAQBIN_MY	TAQBIN Malaysia.
KGMHUB	KGM Hub.
INTEXPRESS	Internet Express.
OVERSE_EXP	Overseas Express.
ONECLICK	One click delivery services.
ROADRUNNER_FREIGHT	Roadbull Logistics.
GLS_CROTIA	GLS Croatia.
MRW_FTP	MRW courier.
BLUEX	Blue Express.
DYLT	Daylight Transport.
DPD_IR	DPD Ireland.
SIN_GLBL	Sin Global Express.
TUFFNELLS_REFERENCE	Tuffnells Parcels Express- Reference.
CJPACKET	CJ Packet.
MILKMAN	Milkman courier.
ASIGNA	ASIGNA courier.
ONEWORLDEXPRESS	One World Express.
ROYAL_MAIL	RoyalShipments.
VIA_EXPRESS	Viaxpress.
TIGFREIGHT	TIG Freight.
ZTO_EXPRESS	ZTO Express.
TWO_GO	2GO Courier.
IML	IML courier.
INTEL_VALLEY	Intel-Valley Supply chain (ShenZhen) Co. Ltd.
EFS	EFS (E-commerce Fulfillment Service).
UK_UK_MAIL	UK mail (ukmail.com).
RAM	RAM courier.
ALLIEDEXPRESS	Allied Express.
APC_OVERNIGHT	APC overnight (apc-overnight.com).
SHIPPIT	Shippit.
TFM	TFM Xpress.
M_XPRESS	M Xpress Sdn Bhd.
HDB_BOX	Haidaibao (BOX).
CLEVY_LINKS	Clevy Links.
IBEONE	Beone Logistics.
FIEGE_NL	Fiege Netherlands.
KWE_GLOBAL	KWE Global.
CTC_EXPRESS	CTC Express.
AMAZON	Amazon Shipping.
MORE_LINK	Morelink.
JX	JX courier.
EASY_MAIL	Easy Mail.
ADUIEPYLE	A Duie Pyle.
GB_PANTHER	Panther.
EXPRESSSALE	Expresssale.
SG_DETRACK	Detrack.
TRUNKRS_WEBHOOK	Trunkrs courier.
MATDESPATCH	Matdespatch.
DICOM	GLS Logistic Systems Canada Ltd./Dicom.
MBW	MBW Courier Inc..
KHM_CAMBODIA_POST	Cambodia Post.
SINOTRANS	Sinotrans.
BRT_IT_PARCELID	BRT Bartolini(Parcel ID).
DHL_SUPPLY_CHAIN	DHL Supply Chain APAC.
DHL_PL	DHL Poland.
TOPYOU	TopYou.
PALEXPRESS	PAL Express Limited.
DHL_SG	dhl Singapore.
CN_WEDO	WeDo Logistics.
FULFILLME	Fulfillme.
DPD_DELISTRACK	DPD delistrack.
UPS_REFERENCE	UPS Reference.
CARIBOU	Caribou.
LOCUS_WEBHOOK	Locus courier.
DSV	DSV courier.
P2P_TRC	P2P TrakPak.
DIRECTPARCELS	Direct Parcels.
NOVA_POSHTA_INT	Nova Poshta (International).
FEDEX_POLAND	FedEx® Poland Domestic.
CN_JCEX	JCEX courier.
FAR_INTERNATIONAL	FAR international.
IDEXPRESS	IDEX courier.
GANGBAO	GANGBAO Supplychain.
NEWAY	Neway Transport.
POSTNL_INT_3_S	PostNL International.
RPX_ID	RPX Indonesia.
DESIGNERTRANSPORT_WEBHOOK	Designer Transport.
GLS_SLOVEN	GLS Slovenia.
PARCELLED_IN	Parcelled.in.
GSI_EXPRESS	GSI EXPRESS.
CON_WAY	Con-way Freight.
BROUWER_TRANSPORT	Brouwer Transport en Logistiek.
CPEX	Captain Express International.
ISRAEL_POST	Israel Post.
DTDC_IN	DTDC India.
PTT_POST	PTT Post.
XDE_WEBHOOK	Ximex Delivery Express.
TOLOS	Tolos courier.
GIAO_HANG	Giao hàng nhanh.
GEODIS_ESPACE	Geodis E-space.
MAGYAR_HU	Magyar Post.
DOORDASH_WEBHOOK	DoorDash.
TIKI_ID	Tiki shipment.
CJ_HK_INTERNATIONAL	CJ Logistics International(Hong Kong).
STAR_TRACK_EXPRESS	Star Track Express.
HELTHJEM	Helthjem.
SFB2C	SF International.
FREIGHTQUOTE	Freightquote by C.H. Robinson.
LANDMARK_GLOBAL_REFERENCE	Landmark Global Reference.
PARCEL2GO	Parcel2Go.
DELNEXT	Delnext.
RCL	Red Carpet Logistics.
CGS_EXPRESS	CGS Express.
HK_POST	Hongkong Post (www.hongkongpost.hk).
SAP_EXPRESS	SAP EXPRESS.
PARCELPOST_SG	Parcel Post Singapore.
HERMES	HermesWorld UK.
IND_SAFEEXPRESS	Safexpress.
TOPHATTEREXPRESS	Tophatter Express.
MGLOBAL	PT MGLOBAL LOGISTICS INDONESIA.
AVERITT	Averitt Express.
LEADER	leader.
_2EBOX	2ebox courier.
SG_SPEEDPOST	Singapore Speedpost.
DBSCHENKER_SE	DB Schenker (www.dbschenker.com).
ISR_POST_DOMESTIC	Israel Post Domestic.
BESTWAYPARCEL	Best Way Parcel.
ASENDIA_DE	asendia_de.
NIGHTLINE_UK	nightline_uk.
TAQBIN_SG	taqbin_sg.
TCK_EXPRESS	TCK Express.
ENDEAVOUR_DELIVERY	Endeavour Delivery.
NANJINGWOYUAN	Nanjing Woyuan.
HEPPNER_FR	Heppner France.
EMPS_CN	EMPS Express.
FONSEN	Fonsen Logistics.
PICKRR	Pickrr.
APC_OVERNIGHT_CONNUM	APC Overnight Consignment.
STAR_TRACK_NEXT_FLIGHT	Star Track Next Flight.
DAJIN	Shanghai Aqrum Chemical Logistics Co.Ltd.
UPS_FREIGHT	UPS Freight.
POSTA_PLUS	Posta Plus.
CEVA	CEVA LOGISTICS.
ANSERX	ANSERX courier.
JS_EXPRESS	JS EXPRESS.
PADTF	padtf.com.
UPS_MAIL_INNOVATIONS	UPS Mail Innovations.
SYPOST	Sunyou Post.
AMAZON_SHIP_MCF	Amazon Shipping + Amazon MCF.
YUSEN	Yusen Logistics.
BRING	Bring.
SDA_IT	SDA Italy.
GBA	GBA Services Ltd.
NEWEGGEXPRESS	Newegg Express.
SPEEDCOURIERS_GR	Speed Couriers.
FORRUN	forrun Pvt Ltd (Arpatech Venture).
PICKUP	Pickupp.
ECMS	ECMS International Logistics Co..
INTELIPOST	Intelipost (TMS for LATAM).
FLASHEXPRESS	Flash Express.
CN_STO	STO Express.
SEKO_SFTP	SEKO Worldwide.
HOME_DELIVERY_SOLUTIONS	Home Delivery Solutions Ltd.
DPD_HGRY	DPD Hungary.
KERRYTTC_VN	Kerry Express (Vietnam) Co Ltd.
JOYING_BOX	Joying Box.
TOTAL_EXPRESS	Total Express.
ZJS_EXPRESS	ZJS International.
STARKEN	STARKEN couriers.
DEMANDSHIP	DemandShip.
CN_DPEX	DPEX.
AUPOST_CN	AuPost China.
LOGISTERS	Logisters.
GOGLOBALPOST	Global Post.
GLS_CZ	GLS Czech Republic.
PAACK_WEBHOOK	Paack courier.
GRAB_WEBHOOK	Grab courier.
PARCELPOINT	Parcelpoint.
ICUMULUS	iCumulus.
DAIGLOBALTRACK	DAI Post.
GLOBAL_IPARCEL	i-parcel.
YURTICI_KARGO	Yurtici Kargo.
CN_PAYPAL_PACKAGE	PayPal Package.
PARCEL_2_POST	Parcel To Post.
GLS_IT	GLS Italy.
PIL_LOGISTICS	PIL Logistics (China) Co..
HEPPNER	Heppner Internationale Spedition GmbH & Co..
GENERAL_OVERNIGHT	Go!Express and logistics.
HAPPY2POINT	Happy 2ThePoint.
CHITCHATS	Chit Chats.
SMOOTH	Smooth Couriers.
CLE_LOGISTICS	CL E-Logistics Solutions Limited.
FIEGE	Fiege Logistics.
MX_CARGO	M&X cargo.
ZIINGFINALMILE	Ziing Final Mile Inc.
DAYTON_FREIGHT	Dayton Freight.
TCS	TCS courier.
AEX	AEX Group.
HERMES_DE	Hermes Germany.
ROUTIFIC_WEBHOOK	Routific.
GLOBAVEND	Globavend.
CJ_LOGISTICS	CJ Logistics International.
PALLET_NETWORK	The Pallet Network.
RAF_PH	RAF Philippines.
UK_XDP	XDP Express.
PAPER_EXPRESS	Paper Express.
LA_POSTE_SUIVI	La Poste.
PAQUETEXPRESS	Paquetexpress.
LIEFERY	liefery.
STRECK_TRANSPORT	Streck Transport.
PONY_EXPRESS	Pony express.
ALWAYS_EXPRESS	Always Express.
GBS_BROKER	GBS-Broker.
CITYLINK_MY	City-Link Express.
ALLJOY	ALLJOY SUPPLY CHAIN.
YODEL	yodel.
YODEL_DIR	Yodel Direct.
STONE3PL	STONE3PL.
PARCELPAL_WEBHOOK	ParcelPal.
DHL_ECOMERCE_ASA	DHL eCommerce Asia (API).
SIMPLYPOST	J&T Express Singapore.
KY_EXPRESS	Kua Yue Express.
SHENZHEN	shenzhen 1st International Logistics(Group)Co.
US_LASERSHIP	LaserShip.
UC_EXPRE	ucexpress.
DIDADI	DIDADI Logistics tech.
CJ_KR	CJ Korea Express.
DBSCHENKER_B2B	DB Schenker B2B.
MXE	MXE Express.
CAE_DELIVERS	CAE Delivers.
PFCEXPRESS	PFC Express.
WHISTL	Whistl.
WEPOST	WePost Sdn Bhd.
DHL_PARCEL_ES	DHL parcel Spain(www.dhl.com).
DDEXPRESS	DD Express Courier.
ARAMEX_AU	Aramex Australia (formerly Fastway AU).
BNEED	Bneed courier.
HK_TGX	Kerry Express Hong Kong.
LATVIJAS_PASTS	Latvijas Pasts.
VIAEUROPE	ViaEurope.
CORREO_UY	Correo Uruguayo.
CHRONOPOST_FR	Chronopost france (www.chronopost.fr).
J_NET	J-Net.
_6LS	6ls.com.
BLR_BELPOST	Belpost.
BIRDSYSTEM	BirdSystem.
DOBROPOST	DobroPost.
WAHANA_ID	Wahana express (www.wahana.com).
WEASHIP	Weaship.
SONICTL	Sonic Transportation & Logistics.
KWT	Shenzhen Jinghuada Logistics Co..
AFLLOG_FTP	AFL LOGISTICS.
SKYNET_WORLDWIDE	SkyNet Worldwide Express.
NOVA_POSHTA	Nova Poshta (novaposhta.ua).
SEINO	Seino.
SZENDEX	SZENDEX.
BPOST_INT	Bpost international.
DBSCHENKER_SV	DB Schenker Sweden.
AO_DEUTSCHLAND	AO Deutschland.
EU_FLEET_SOLUTIONS	EU Fleet Solutions.
PCFCORP	PCF Final Mile.
LINKBRIDGE	Link Bridge(BeiJing)international logistics co..
PRIMAMULTICIPTA	PT Prima Multi Cipta.
COUREX	Urbanfox.
ZAJIL_EXPRESS	Zajil Express Company.
COLLECTCO	CollectCo.
JTEXPRESS	J&T EXPRESS MALAYSIA.
FEDEX_UK	FedEx® UK.
USHIP	uShip courier.
PIXSELL	PIXSELL LOGISTICS.
SHIPTOR	Shiptor.
CDEK	CDEK courier.
VNM_VIETTELPOST	ViettelPost.
CJ_CENTURY	CJ Century.
GSO	GSO(GLS-USA).
VIWO	VIWO IoT.
SKYBOX	SKYBOX.
KERRYTJ	Kerry TJ Logistics.
NTLOGISTICS_VN	Nhat Tin Logistics.
SDH_SCM	lightning monkey.
ZINC	Zinc courier.
DPE_SOUTH_AFRC	DPE South Africa.
CESKA_CZ	Czech Post.
ACS_GR	ACS Courier.
DEALERSEND	DealerSend.
JOCOM	Jocom.
CSE	CSE courier.
TFORCE_FINALMILE	TForce Final Mile.
SHIP_GATE	ShipGate.
SHIPTER	SHIPTER.
NATIONAL_SAMEDAY	National Sameday.
YUNEXPRESS	YunExpress.
CAINIAO	AliExpress Standard Shipping.
DMS_MATRIX	DMSMatrix.
DIRECTLOG	Directlog (www.directlog.com.br).
ASENDIA_US	Asendia USA.
_3JMSLOGISTICS	3JMS Logistics.
LICCARDI_EXPRESS	LICCARDI EXPRESS COURIER.
SKY_POSTAL	SkyPostal.
CNWANGTONG	cnwangtong.
POSTNORD_LOGISTICS_DK	ostnord denmark.
LOGISTIKA	Logistika.
CELERITAS	Celeritas Transporte.
PRESSIODE	Pressio.
SHREE_MARUTI	Shree Maruti Courier Services Pvt Ltd.
LOGISTICSWORLDWIDE_HK	Logistic Worldwide Express (LWE Honkong).
EFEX	eFEx (E-Commerce Fulfillment & Express).
LOTTE	Lotte Global Logistics.
LONESTAR	Lone Star Overnight.
APRISAEXPRESS	Aprisa Express.
BEL_RS	BEL North Russia.
OSM_WORLDWIDE	OSM Worldwide.
WESTGATE_GL	Westgate Global.
FASTRACK	Fasttrack.
DTD_EXPR	DTD Express.
ALFATREX	AlfaTrex.
PROMEDDELIVERY	ProMed Delivery.
THABIT_LOGISTICS	Thabit Logistics.
HCT_LOGISTICS	HCT LOGISTICS CO.LTD..
CARRY_FLAP	Carry-Flap Co..
US_OLD_DOMINION	Old Dominion Freight Line.
ANICAM_BOX	ANICAM BOX EXPRESS.
WANBEXPRESS	WanbExpress.
AN_POST	An Post.
DPD_LOCAL	DPD Local.
STALLIONEXPRESS	Stallion Express.
RAIDEREX	RaidereX.
SHOPFANS	ShopfansRU LLC.
KYUNGDONG_PARCEL	Kyungdong Parcel.
CHAMPION_LOGISTICS	Champion Logistics.
PICKUPP_SGP	PICK UPP (Singapore).
MORNING_EXPRESS	Morning Express.
NACEX	NACEX.
THENILE_WEBHOOK	SortHub courier.
HOLISOL	Holisol.
LBCEXPRESS_FTP	LBC EXPRESS INC..
KURASI	KURASI.
USF_REDDAWAY	USF Reddaway.
APG	APG eCommerce Solutions.
CN_BOXC	BoxC courier.
ECOSCOOTING	ECOSCOOTING.
MAINWAY	Mainway.
PAPERFLY	Paperfly Private Limited.
HOUNDEXPRESS	Hound Express.
BOX_BERRY	Boxberry courier.
EP_BOX	EP-Box courier.
PLUS_LOG_UK	Plus UK Logistics.
FULFILLA	Fulfilla.
ASE	ASE KARGO.
MAIL_PLUS	MailPlus.
XPO_LOGISTICS	XPO logistics.
WNDIRECT	wnDirect.
CLOUDWISH_ASIA	Cloudwish Asia.
ZELERIS	Zeleris.
GIO_EXPRESS	Gio Express.
OCS_WORLDWIDE	OCS WORLDWIDE.
ARK_LOGISTICS	ARK Logistics.
AQUILINE	Aquiline.
PILOT_FREIGHT	Pilot Freight Services.
QWINTRY	Qwintry Logistics.
DANSKE_FRAGT	Danske Fragtaend.
CARRIERS	Carriers courier.
AIR_CANADA_GLOBAL	Rivo (Air canada).
PRESIDENT_TRANS	PRESIDENT TRANSNET CORP.
STEPFORWARDFS	STEP FORWARD FREIGHT SERVICE CO LTD.
SKYNET_UK	Skynet UK.
PITTOHIO	PITT OHIO.
CORREOS_EXPRESS	Correos Express.
RL_US	RL Carriers.
DESTINY	Destiny Transportation.
UK_YODEL	Yodel (www.yodel.co.uk).
COMET_TECH	CometTech.
DHL_PARCEL_RU	DHL Parcel Russia.
TNT_REFR	TNT Reference.
SHREE_ANJANI_COURIER	Shree Anjani Courier.
MIKROPAKKET_BE	Mikropakket Belgium.
ETS_EXPRESS	RETS express.
COLIS_PRIVE	Colis Privé.
CN_YUNDA	Yunda Express.
AAA_COOPER	AAA Cooper.
ROCKET_PARCEL	Rocket Parcel International.
_360LION	360 Lion Express.
PANDU	PANDU.
PROFESSIONAL_COURIERS	PROFESSIONAL COURIERS.
FLYTEXPRESS	FLYTEXPRESS.
LOGISTICSWORLDWIDE_MY	LOGISTICSWORLDWIDE MY.
CORREOS_DE_ESPANA	CORREOS DE ESPANA.
IMX	IMX.
FOUR_PX_EXPRESS	FOUR PX EXPRESS.
XPRESSBEES	XPRESSBEES.
PICKUPP_VNM	pickupp_vnm.
STARTRACK_EXPRESS	startrack_express.
FR_COLISSIMO	fr_colissimo.
NACEX_SPAIN_REFERENCE	nacex_spain_reference.
DHL_SUPPLY_CHAIN_AU	dhl_supply_chain_au.
ESHIPPING	Eshipping.
SHREETIRUPATI	SHREE TIRUPATI COURIER SERVICES PVT. LTD..
HX_EXPRESS	HX Express.
INDOPAKET	INDOPAKET.
CN_17POST	17 Post Service.
K1_EXPRESS	K1 Express.
CJ_GLS	CJ GLS.
MYS_GDEX	GDEX courier.
NATIONEX	Nationex courier.
ANJUN	Anjun couriers.
FARGOOD	FarGood.
SMG_EXPRESS	SMG Direct.
RZYEXPRESS	RZY Express.
SEFL	Southeastern Freight Lines.
TNT_CLICK_IT	TNT-Click Italy.
HDB	Haidaibao.
HIPSHIPPER	Hipshipper.
RPXLOGISTICS	RPX Logistics.
KUEHNE	Kuehne + Nagel.
IT_NEXIVE	Nexive (TNT Post Italy).
PTS	PTS courier.
SWISS_POST_FTP	Swiss Post FTP.
FASTRK_SERV	Fastrak Services.
_4_72	4-72 Entregando.
US_YRC	YRC courier.
POSTNL_INTL_3S	PostNL International 3S.
ELIAN_POST	Yilian (Elian) Supply Chain.
CUBYN	Cubyn.
SAU_SAUDI_POST	Saudi Post.
ABXEXPRESS_MY	ABX Express.
HUAHAN_EXPRESS	HUAHANG EXPRESS.
ZES_EXPRESS	Eshun international Logistic.
ZEPTO_EXPRESS	ZeptoExpress.
SKYNET_ZA	Skynet World Wide Express South Africa.
ZEEK_2_DOOR	Zeek2Door.
BLINKLASTMILE	Blink.
POSTA_UKR	UkrPoshta.
CHROBINSON	C.H. Robinson Worldwide.
CN_POST56	Post56.
COURANT_PLUS	Courant Plus.
SCUDEX_EXPRESS	Scudex Express.
SHIPENTEGRA	ShipEntegra.
B_TWO_C_EUROPE	B2C courier Europe.
COPE	Cope Sensitive Freight.
IND_GATI	Gati-KWE.
CN_WISHPOST	WishPost.
NACEX_ES	NACEX Spain.
TAQBIN_HK	TAQBIN Hong Kong.
GLOBALTRANZ	GlobalTranz.
HKD	Qingdao HKD International Logistics.
BJSHOMEDELIVERY	BJS Distribution courier.
OMNIVA	Omniva.
SUTTON	Sutton Transport.
PANTHER_REFERENCE	Panther Reference.
SFCSERVICE	SFC Service.
LTL	LTL COURIER.
PARKNPARCEL	Park N Parcel.
SPRING_GDS	Spring GDS.
ECEXPRESS	ECexpress.
INTERPARCEL_AU	Interparcel Australia.
AGILITY	Agility.
XL_EXPRESS	XL Express.
ADERONLINE	Ader couriers.
DIRECTCOURIERS	Direct Couriers.
PLANZER	Planzer Group.
SENDING	Sending Transporte Urgente y Comunicacion.
NINJAVAN_WB	Ninjavan Webhook.
NATIONWIDE_MY	Nationwide Express Courier Services Bhd (www.nationwide.com.my).
SENDIT	Sendit.
GB_ARROW	Arrow XL.
IND_GOJAVAS	GoJavas.
KPOST	Korea Post.
DHL_FREIGHT	DHL Freight.
BLUECARE	Bluecare Express Ltd.
JINDOUYUN	jindouyun courier.
TRACKON	Trackon Couriers Pvt. Ltd.
GB_TUFFNELLS	Tuffnells Parcels Express.
TRUMPCARD	TRUMPCARD LLC.
ETOTAL	eTotal Solution Limited.
SFPLUS_WEBHOOK	Zeek courier.
SEKOLOGISTICS	SEKO Logistics.
HERMES_2MANN_HANDLING	Hermes Einrichtungs Service GmbH & Co. KG.
DPD_LOCAL_REF	DPD Local reference.
UDS	United Delivery Service.
ZA_SPECIALISED_FREIGHT	Specialised Freight.
THA_KERRY	Kerry Express Thailand.
PRT_INT_SEUR	SEUR International.
BRA_CORREIOS	Correios Brazil.
NZ_NZ_POST	New Zealand Post.
CN_EQUICK	Equick China.
MYS_EMS	Malaysia Post EMS / Pos Laju.
GB_NORSK	Norsk Global.
ESP_MRW	MRW spain.
ESP_PACKLINK	Packlink.
KANGAROO_MY	Kangaroo Worldwide Express.
RPX	RPX Online.
XDP_UK_REFERENCE	XDP Express Reference.
NINJAVAN_MY	ninja van (www.ninjavan.co).
ADICIONAL	Adicional Logistics.
ROADBULL	Red Carpet Logistics.
YAKIT	Yakit courier.
MAILAMERICAS	MailAmericas.
MIKROPAKKET	Mikropakket.
DYNALOGIC	Dynamic Logistics.
DHL_ES	DHL Spain(www.dhl.com).
DHL_PARCEL_NL	DHL Parcel NL.
DHL_GLOBAL_MAIL_ASIA	DHL Global Mail Asia (www.dhl.com).
DAWN_WING	Dawn Wing.
GENIKI_GR	Geniki Taxydromiki.
HERMESWORLD_UK	hermesworld_uk.
ALPHAFAST	Alphafast (www.alphafast.com).
BUYLOGIC	buylogic.
EKART	Ekart logistics (ekartlogistics.com).
MEX_SENDA	mexico senda express.
SFC_LOGISTICS	SFC.
POST_SERBIA	Posta Serbia.
IND_DELHIVERY	Delhivery India.
DE_DPD_DELISTRACK	DPD Germany.
RPD2MAN	RPD2man Deliveries.
CN_SF_EXPRESS	SF Express (www.sf-express.com).
YANWEN	Yanwen Logistics.
MYS_SKYNET	Skynet Malaysia.
CORREOS_DE_MEXICO	correos mexico.
CBL_LOGISTICA	CBL Logistica.
MEX_ESTAFETA	Estafeta (www.estafeta.com).
AU_AUSTRIAN_POST	Austrian Post (Registered).
RINCOS	Rincos.
NLD_DHL	DHL Netherland.
RUSSIAN_POST	Russian post.
COURIERS_PLEASE	CouriersPlease (couriersplease.com.au).
POSTNORD_LOGISTICS	PostNord Logistics.
FEDEX	Fedex.
DPE_EXPRESS	DPE Express.
DPD	DPD.
ADSONE	ADSone.
IDN_JNE	JNE Express (Jalur Nugraha Ekakurir).
THECOURIERGUY	The Courier Guy.
CNEXPS	CNE Express.
PRT_CHRONOPOST	Chronopost Portugal.
LANDMARK_GLOBAL	Landmark Global.
IT_DHL_ECOMMERCE	DHL International.
ESP_NACEX	NACEX Spain.
PRT_CTT	CTT Portugal.
BE_KIALA	Kiala.
ASENDIA_UK	Asendia UK.
GLOBAL_TNT	TNT global.
POSTUR_IS	Iceland Post.
EPARCEL_KR	eParcel Korea.
INPOST_PACZKOMATY	InPost Paczkomaty.
IT_POSTE_ITALIA	Poste italiane (www.poste.it).
BE_BPOST	Bpost (www.bpost.be).
PL_POCZTA_POLSKA	Poczta Polska (www.poczta-polska.pl).
MYS_MYS_POST	Malaysia Post.
SG_SG_POST	Singapore Post.
THA_THAILAND_POST	Thailand Post (www.thailandpost.co.th).
LEXSHIP	LexShip.
FASTWAY_NZ	Fastway New Zealand.
DHL_AU	DHL Supply Chain Australia.
COSTMETICSNOW	Cosmetics Now.
PFLOGISTICS	PFL.
LOOMIS_EXPRESS	Loomis Express.
GLS_ITALY	GLS Italy.
LINE	Line Clear Express & Logistics Sdn Bhd.
GEL_EXPRESS	Gel Express Logistik.
HUODULL	Huodull.
NINJAVAN_SG	Ninja van Singapore.
JANIO	Janio Asia.
AO_COURIER	AO Logistics.
BRT_IT_SENDER_REF	BRT Bartolini(Sender Reference).
SAILPOST	SAILPOST.
LALAMOVE	Lalamove.
NEWZEALAND_COURIERS	NEW ZEALAND COURIERS.
ETOMARS	Etomars.
VIRTRANSPORT	VIR Transport.
WIZMO	Wizmo.
PALLETWAYS	Palletways.
I_DIKA	i-dika.
CFL_LOGISTICS	CFL Logistics.
GEMWORLDWIDE	GEM Worldwide.
GLOBAL_EXPRESS	Tai Wan Global Business.
LOGISTYX_TRANSGROUP	Transgroup courier.
WESTBANK_COURIER	West Bank Courier.
ARCO_SPEDIZIONI	Arco Spedizioni SP.
YDH_EXPRESS	YDH express.
PARCELINKLOGISTICS	Parcelink Logistics.
CNDEXPRESS	CND Express.
NOX_NIGHT_TIME_EXPRESS	NOX NightTimeExpress.
AERONET	Aeronet couriers.
LTIANEXP	LTIAN EXP.
INTEGRA2_FTP	Integra2.
PARCELONE	PARCEL ONE.
NOX_NACHTEXPRESS	Innight Express Germany GmbH (nox NachtExpress).
CN_CHINA_POST_EMS	China Post.
CHUKOU1	Chukou1.
GLS_SLOV	GLS General Logistics Systems Slovakia s.r.o..
ORANGE_DS	OrangeDS (Orange Distribution Solutions Inc).
JOOM_LOGIS	Joom Logistics.
AUS_STARTRACK	StarTrack (startrack.com.au).
DHL	dhl Global.
GB_APC	APC postal logistics germany.
BONDSCOURIERS	Bonds Courier Service (bondscouriers.com.au).
JPN_JAPAN_POST	Japan Post.
USPS	United States Postal Service.
WINIT	WinIt.
ARG_OCA	OCA Argentina.
TW_TAIWAN_POST	Taiwan Post.
DMM_NETWORK	DMM Network.
TNT	TNT Express.
BH_POSTA	BH Posta (www.posta.ba).
SWE_POSTNORD	Postnord sweden.
CA_CANADA_POST	Canada Post.
WISELOADS	Wiseloads.
ASENDIA_HK	Asendia HonKong.
NLD_GLS	GLS Netherland.
MEX_REDPACK	Redpack.
JET_SHIP	Jet-Ship Worldwide.
DE_DHL_EXPRESS	DHL Express.
NINJAVAN_THAI	Ninja van Thai.
RABEN_GROUP	Raben Group.
ESP_ASM	ASM(GLS Spain).
HRV_HRVATSKA	Hrvatska posta.
GLOBAL_ESTES	Estes Express Lines.
LTU_LIETUVOS	Lietuvos pastas.
BEL_DHL	DHL Benelux.
AU_AU_POST	Australia Post.
SPEEDEXCOURIER	SPEEDEX couriers.
FR_COLIS	Colissimo.
ARAMEX	Aramex.
DPEX	DPEX (www.dpex.com).
MYS_AIRPAK	Airpak Express.
CUCKOOEXPRESS	Cuckoo Express.
DPD_POLAND	DPD Poland.
NLD_POSTNL	PostNL International.
NIM_EXPRESS	Nim Express.
QUANTIUM	Quantium.
SENDLE	Sendle.
ESP_REDUR	Redur Spain.
MATKAHUOLTO	Matkahuolto.
CPACKET	Cpacket couriers.
POSTI	Posti courier.
HUNTER_EXPRESS	Hunter Express.
CHOIR_EXP	Choir Express Indonesia.
LEGION_EXPRESS	Legion Express.
AUSTRIAN_POST_EXPRESS	austrian post.
GRUPO	Grupo ampm.
POSTA_RO	Post Roman (www.posta-romana.ro).
INTERPARCEL_UK	Interparcel UK.
GLOBAL_ABF	ABF Freight.
POSTEN_NORGE	Posten Norge (www.posten.no).
XPERT_DELIVERY	Xpert Delivery.
DHL_REFR	DHl (Reference number).
DHL_HK	DHL HonKong.
SKYNET_UAE	SKYNET UAE.
GOJEK	Gojek.
YODEL_INTNL	Yodel International.
JANCO	Janco Ecommerce.
YTO	YTO Express.
WISE_EXPRESS	Wise Express.
JTEXPRESS_VN	J&T Express Vietnam.
FEDEX_INTL_MLSERV	FedEx International MailService.
VAMOX	VAMOX.
AMS_GRP	AMS Group.
DHL_JP	DHL Japan.
HRPARCEL	HR Parcel.
GESWL	GESWL Express.
BLUESTAR	Blue Star.
CDEK_TR	CDEK TR.
DESCARTES	Innovel courier.
DELTEC_UK	Deltec Courier.
DTDC_EXPRESS	DTDC express.
TOURLINE	tourline.
BH_WORLDWIDE	B&H Worldwide.
OCS	OCS ANA Group.
YINGNUO_LOGISTICS	yingnuo logistics.
UPS	United Parcel Service.
TOLL	Toll IPEC.
PRT_SEUR	SEUR portugal.
DTDC_AU	DTDC Australia.
THA_DYNAMIC_LOGISTICS	Dynamic Logistics.
UBI_LOGISTICS	UBI Smart Parcel.
FEDEX_CROSSBORDER	FedEx Cross Border.
A1POST	A1Post.
TAZMANIAN_FREIGHT	Tazmanian Freight Systems.
CJ_INT_MY	CJ International malaysia.
SAIA_FREIGHT	Saia LTL Freight.
SG_QXPRESS	Qxpress.
NHANS_SOLUTIONS	Nhans Solutions.
DPD_FR	DPD France.
COORDINADORA	Coordinadora.
ANDREANI	Grupo logistico Andreani.
DOORA	Doora Logistics.
INTERPARCEL_NZ	Interparcel New Zealand.
PHL_JAMEXPRESS	Jam Express Philippines.
BEL_BELGIUM_POST	bel_belgium_post.
US_APC	us_apc.
IDN_POS	idn_pos.
FR_MONDIAL	fr_mondial.
DE_DHL	DE DHL.
HK_RPX	hk_rpx.
DHL_PIECEID	dhl_pieceid.
VNPOST_EMS	vnpost_ems.
RRDONNELLEY	rrdonnelley.
DPD_DE	dpd_de.
DELCART_IN	delcart_in.
IMEXGLOBALSOLUTIONS	imexglobalsolutions.
ACOMMERCE	ACOMMERCE.
EURODIS	eurodis.
CANPAR	CANPAR.
GLS	GLS.
IND_ECOM	Ecom Express.
ESP_ENVIALIA	Envialia.
DHL_UK	dhl UK.
SMSA_EXPRESS	SMSA Express.
TNT_FR	TNT France.
DEX_I	DEX-I courier.
BUDBEE_WEBHOOK	Budbee courier.
COPA_COURIER	Copa Airlines Courier.
VNM_VIETNAM_POST	Vietnam Post.
DPD_HK	DPD HongKong.
TOLL_NZ	Toll New Zealand.
ECHO	Echo courier.
FEDEX_FR	FedEx® Freight.
BORDEREXPRESS	Border Express.
MAILPLUS_JPN	MailPlus (Japan).
TNT_UK_REFR	TNT UK Reference.
KEC	KEC courier.
DPD_RO	DPD Romania.
TNT_JP	TNT_JP.
TH_CJ	TH_CJ.
EC_CN	EC_CN.
FASTWAY_UK	FASTWAY_UK.
FASTWAY_US	FASTWAY_US.
GLS_DE	GLS_DE.
GLS_ES	GLS_ES.
GLS_FR	GLS_FR.
MONDIAL_BE	MONDIAL_BE.
SGT_IT	SGT_IT.
TNT_CN	TNT_CN.
TNT_DE	TNT_DE.
TNT_ES	TNT_ES.
TNT_PL	TNT_PL.
PARCELFORCE	PARCELFORCE.
SWISS_POST	SWISS POST.
TOLL_IPEC	TOLL IPEC.
AIR_21	AIR 21.
AIRSPEED	AIRSPEED.
BERT	BERT.
BLUEDART	BLUEDART.
COLLECTPLUS	COLLECTPLUS.
COURIERPLUS	COURIERPLUS.
COURIER_POST	COURIER POST.
DHL_GLOBAL_MAIL	dhl_global_mail.
DPD_UK	dpd_uk.
DELTEC_DE	DELTEC DE.
DEUTSCHE_DE	deutsche_de.
DOTZOT	DOTZOT.
ELTA_GR	elta_gr.
EMS_CN	ems_cn.
ECARGO	ECARGO.
ENSENDA	ENSENDA.
FERCAM_IT	fercam_it.
FASTWAY_ZA	fastway_za.
FASTWAY_AU	fastway_au.
FIRST_LOGISITCS	first_logisitcs.
GEODIS	GEODIS.
GLOBEGISTICS	GLOBEGISTICS.
GREYHOUND	GREYHOUND.
JETSHIP_MY	jetship_my.
LION_PARCEL	LION PARCEL.
AEROFLASH	AEROFLASH.
ONTRAC	ONTRAC.
SAGAWA	SAGAWA.
SIODEMKA	SIODEMKA.
STARTRACK	startrack.
TNT_AU	tnt_au.
TNT_IT	tnt_it.
TRANSMISSION	TRANSMISSION.
YAMATO	YAMATO.
DHL_IT	dhl_it.
DHL_AT	dhl_at.
LOGISTICSWORLDWIDE_KR	LOGISTICSWORLDWIDE KR.
GLS_SPAIN	gls_spain.
AMAZON_UK_API	amazon_uk_api.
DPD_FR_REFERENCE	dpd_fr_reference.
DHLPARCEL_UK	dhlparcel_uk.
MEGASAVE	megasave.
QUALITYPOST	qualitypost.
IDS_LOGISTICS	ids_logistics.
JOYINGBOX	joyingbox.
PANTHER_ORDER_NUMBER	panther_order_number.
WATKINS_SHEPARD	watkins_shepard.
FASTTRACK	fasttrack.
UP_EXPRESS	up_express.
ELOGISTICA	elogistica.
ECOURIER	ecourier.
CJ_PHILIPPINES	cj_philippines.
SPEEDEX	speedex.
ORANGECONNEX	orangeconnex.
TECOR	tecor.
SAEE	saee.
GLS_ITALY_FTP	gls_italy_ftp.
DELIVERE	delivere.
YYCOM	yycom.
ADICIONAL_PT	Adicional Logistics.
DKSH	DKSH.
NIPPON_EXPRESS_FTP	Nippon Express.
GOLS	GO Logistics & Storage.
FUJEXP	FUJIE EXPRESS.
QTRACK	QTrack.
OMLOGISTICS_API	OM LOGISTICS LTD.
GDPHARM	GDPharm Logistics.
MISUMI_CN	MISUMI Group Inc..
AIR_CANADA	Rivo.
CITY56_WEBHOOK	City Express.
SAGAWA_API	Sagawa.
KEDAEX	KedaEX.
PGEON_API	Pgeon.
WEWORLDEXPRESS	We World Express.
JT_LOGISTICS	J&T International logistics.
TRUSK	Trusk France.
VIAXPRESS	ViaXpress.
DHL_SUPPLYCHAIN_ID	DHL Supply Chain Indonesia.
ZUELLIGPHARMA_SFTP	Zuellig Pharma Korea.
MEEST	Meest.
TOLL_PRIORITY	Toll Priority.
MOTHERSHIP_API	Mothership.
CAPITAL	Capital Transport.
EUROPAKET_API	Europacket+.
HFD	HFD.
TOURLINE_REFERENCE	Tourline Express.
GIO_ECOURIER	GIO Express Inc.
CN_LOGISTICS	CN Logistics.
PANDION	Pandion.
BPOST_API	Bpost API.
PASSPORTSHIPPING	Passport Shipping.
PAKAJO	Pakajo World.
DACHSER	DACHSER.
YUSEN_SFTP	Yusen Logistics.
SHYPLITE	Shypmax.
XYY	Xingyunyi Logistics.
MWD	Metropolitan Warehouse & Delivery.
FAXECARGO	Faxe Cargo.
MAZET	Groupe Mazet.
FIRST_LOGISTICS_API	First Logistics.
SPRINT_PACK	SPRINT PACK.
HERMES_DE_FTP	Hermes Germany.
CONCISE	Concise.
KERRY_EXPRESS_TW_API	Kerry Express TaiWan.
EWE	EWE Global Express.
FASTDESPATCH	Fast Despatch Logistics Limited.
ABCUSTOM_SFTP	AB Custom Group.
CHAZKI	Chazki.
SHIPPIE	Shippie.
GEODIS_API	GEODIS - Distribution & Express.
NAQEL_EXPRESS	Naqel Express.
PAPA_WEBHOOK	Papa.
FORWARDAIR	Forward Air.
DIALOGO_LOGISTICA_API	Dialogo Logistica.
LALAMOVE_API	Lalamove.
TOMYDOOR	Tomydoor.
KRONOS_WEBHOOK	Kronos Express.
JTCARGO	J&T CARGO.
T_CAT	T-cat.
CONCISE_WEBHOOK	Concise.
TELEPORT_WEBHOOK	Teleport.
CUSTOMCO_API	The Custom Companies.
SPX_TH	Shopee Xpress.
BOLLORE_LOGISTICS	Bollore Logistics.
CLICKLINK_SFTP	ClickLink.
M3LOGISTICS	M3 Logistics.
VNPOST_API	Vietnam Post.
AXLEHIRE_FTP	Axlehire.
SHADOWFAX	Shadowfax.
MYHERMES_UK_API	EVRi.
DAIICHI	Daiichi Freight System Inc.
MENSAJEROSURBANOS_API	Mensajeros Urbanos.
POLARSPEED	PolarSpeed Inc.
IDEXPRESS_ID	iDexpress Indonesia.
PAYO	Payo.
WHISTL_SFTP	Whistl.
INTEX_DE	INTEX Paketdienst GmbH.
TRANS2U	Trans2u.
PRODUCTCAREGROUP_SFTP	Product Care Services Limited.
BIGSMART	Big Smart.
EXPEDITORS_API_REF	Expeditors API Reference.
AITWORLDWIDE_API	AIT.
WORLDCOURIER	World Courier.
QUIQUP	Quiqup.
AGEDISS_SFTP	Agediss.
ANDREANI_API	Andreani.
CRLEXPRESS	CRL Express.
SMARTCAT	SMARTCAT.
CROSSFLIGHT	Crossflight Limited.
PROCARRIER	Pro Carrier.
DHL_REFERENCE_API	DHL (Reference number).
SEINO_API	Seino.
WSPEXPRESS	WSP Express.
KRONOS	Kronos Express.
TOTAL_EXPRESS_API	Total Express.
PARCLL	PARCLL.
XPEDIGO	Xpedigo.
STAR_TRACK_WEBHOOK	StarTrack.
GPOST	Georgian Post.
UCS	UCS.
DMFGROUP	DMF.
COORDINADORA_API	Coordinadora.
MARKEN	Marken.
NTL	NTL logistics.
REDJEPAKKETJE	Red je Pakketje.
ALLIED_EXPRESS_FTP	Allied Express (FTP).
MONDIALRELAY_ES	Mondial Relay Spain(Punto Pack).
NAEKO_FTP	Naeko Logistics.
MHI	Mhi.
SHIPPIFY	Shippify, Inc.
MALCA_AMIT_API	Malca Amit.
JTEXPRESS_SG_API	J&T Express Singapore.
DACHSER_WEB	DACHSER.
FLIGHTLG	Flight Logistics Group.
CAGO	Cago.
COM1EXPRESS	ComOne Express.
TONAMI_FTP	Tonami.
PACKFLEET	PACKFLEET.
PUROLATOR_INTERNATIONAL	Purolator International.
WINESHIPPING_WEBHOOK	Wineshipping.
DHL_ES_SFTP	DHL Spain Domestic.
PCHOME_API	網家速配股份有限公司.
CESKAPOSTA_API	Czech Post.
GORUSH	Go Rush.
HOMERUNNER	HomeRunner.
AMAZON_ORDER	Amazon order.
EFWNOW_API	Estes Forwarding Worldwide.
CBL_LOGISTICA_API	CBL Logistica (API).
NIMBUSPOST	NimbusPost.
LOGWIN_LOGISTICS	Logwin Logistics.
NOWLOG_API	Sequoialog.
DPD_NL	DPD Netherlands.
GODEPENDABLE	Dependable Supply Chain Services.
ESDEX	Top Ideal Express.
LOGISYSTEMS_SFTP	Kiitäjät.
EXPEDITORS	Expeditors.
SNTGLOBAL_API	Snt Global Etrax.
SHIPX	ShipX.
QINTL_API	Quickstat Courier LLC.
PACKS	Packs.
POSTNL_INTERNATIONAL	PostNL International.
AMAZON_EMAIL_PUSH	Amazon.
DHL_API	DHL.
SPX	Shopee Express.
AXLEHIRE	AxleHire.
ICSCOURIER	ICS COURIER.
DIALOGO_LOGISTICA	Dialogo Logistica.
SHUNBANG_EXPRESS	ShunBang Express.
TCS_API	TCS.
SF_EXPRESS_CN	SF Express China.
PACKETA	Packeta.
SIC_TELIWAY	Teliway SIC Express.
MONDIALRELAY_FR	Mondial Relay France.
INTIME_FTP	InTime.
JD_EXPRESS	京东物流.
FASTBOX	Fastbox.
PATHEON	Patheon Logistics.
INDIA_POST	India Post Domestic.
TIPSA_REF	Tipsa Reference.
ECOFREIGHT	Eco Freight.
VOX	VOX SOLUCION EMPRESARIAL SRL.
DIRECTFREIGHT_AU_REF	Direct Freight Express.
BESTTRANSPORT_SFTP	Best Transport.
AUSTRALIA_POST_API	Australia Post.
FRAGILEPAK_SFTP	FragilePAK.
FLIPXP	FlipXpress.
VALUE_WEBHOOK	Value Logistics.
DAESHIN	Daeshin.
SHERPA	Sherpa.
MWD_API	Metropolitan Warehouse & Delivery.
SMARTKARGO	SmartKargo.
DNJ_EXPRESS	DNJ Express.
GOPEOPLE	Go People.
MYSENDLE_API	mySendle.
ARAMEX_API	Aramex.
PIDGE	Pidge.
THAIPARCELS	TP Logistic.
PANTHER_REFERENCE_API	Panther Reference.
POSTAPLUS	Posta Plus.
BUFFALO	BUFFALO.
U_ENVIOS	U-ENVIOS.
ELITE_CO	Elite Express.
ROCHE_INTERNAL_SFTP	Roche Internal Courier.
DBSCHENKER_ICELAND	DB Schenker Iceland.
TNT_FR_REFERENCE	TNT France Reference.
NEWGISTICSAPI	Newgistics API.
GLOVO	Glovo.
GWLOGIS_API	G.I.G.
SPREETAIL_API	Spreetail.
MOOVA	Moova.
PLYCONGROUP	Plycon Transportation Group.
USPS_WEBHOOK	USPS Informed Visibility - Webhook.
REIMAGINEDELIVERY	maergo.
EDF_FTP	Eurodifarm.
DAO365	DAO365.
BIOCAIR_FTP	BioCair.
RANSA_WEBHOOK	Ransa.
SHIPXPRES	SHIPXPRESS.
COURANT_PLUS_API	Courant Plus.
SHIPA	SHIPA.
HOMELOGISTICS	Home Logistics.
DX	DX.
POSTE_ITALIANE_PACCOCELERE	Poste Italiane Paccocelere.
TOLL_WEBHOOK	Toll Group.
LCTBR_API	LCT do Brasil.
DX_FREIGHT	DX Freight.
DHL_SFTP	DHL Express.
SHIPROCKET	Shiprocket X.
UBER_WEBHOOK	Uber.
STATOVERNIGHT	Stat Overnight.
BURD	Burd Delivery.
FASTSHIP	Fastship Express.
IBVENTURE_WEBHOOK	IB Venture.
GATI_KWE_API	Gati-KWE.
CRYOPDP_FTP	CryoPDP.
HUBBED	HUBBED.
TIPSA_API	Tipsa API.
ARASKARGO	Aras Cargo.
THIJS_NL	Thijs Logistiek.
ATSHEALTHCARE_REFERENCE	ATS Healthcare.
99MINUTOS	99minutos.
HELLENIC_POST	Hellenic (Greece) Post.
HSM_GLOBAL	HSM Global.
MNX	MNX.
NMTRANSFER	N&M Transfer Co., Inc..
LOGYSTO	Logysto.
INDIA_POST_INT	India Post International.
AMAZON_FBA_SWISHIP_IN	Swiship IN.
SRT_TRANSPORT	SRT Transport.
BOMI	Bomi Group.
DELIVERR_SFTP	Deliverr.
HSDEXPRESS	HSDEXPRESS.
SIMPLETIRE_WEBHOOK	SimpleTire.
HUNTER_EXPRESS_SFTP	Hunter Express.
UPS_API	UPS.
WOOYOUNG_LOGISTICS_SFTP	WOO YOUNG LOGISTICS CO.,LTD..
PHSE_API	PHSE.
WISH_EMAIL_PUSH	Wish.
NORTHLINE	Northline.
MEDAFRICA	Med Africa Logistics.
DPD_AT_SFTP	DPD Austria.
ANTERAJA	Anteraja.
DHL_GLOBAL_FORWARDING_API	DHL Global Forwarding API.
LBCEXPRESS_API	LBC EXPRESS INC..
SIMSGLOBAL	Sims Global.
CDLDELIVERS	CDL Last Mile.
TYP	TYP.
TESTING_COURIER_WEBHOOK	Testing Courier.
PANDAGO_API	Pandago.
ROYAL_MAIL_FTP	Royal Mail.
THUNDEREXPRESS	Thunder Express Australia.
SECRETLAB_WEBHOOK	Secretlab.
SETEL	Setel Express.
JD_WORLDWIDE	JD Worldwide.
DPD_RU_API	DPD Russia.
ARGENTS_WEBHOOK	Argents Express Group.
POSTONE	Post ONE.
TUSKLOGISTICS	Tusk Logistics.
RHENUS_UK_API	Rhenus Logistics UK.
TAQBIN_SG_API	Yamato Singapore.
INNTRALOG_SFTP	Inntralog GmbH.
DAYROSS	Day & Ross.
CORREOSEXPRESS_API	Correos Express (API).
INTERNATIONAL_SEUR_API	International Seur API.
YODEL_API	Yodel API.
HEROEXPRESS	Hero Express.
DHL_SUPPLYCHAIN_IN	DHL supply chain India.
URGENT_CARGUS	Urgent Cargus.
FRONTDOORCORP	FRONTdoor Collective.
JTEXPRESS_PH	J&T Express Philippines.
PARCELSTARS_WEBHOOK	Parcelstars.
DPD_SK_SFTP	DPD Slovakia.
MOVIANTO	Movianto.
OZEPARTS_SHIPPING	Ozeparts Shipping.
KARGOMKOLAY	KargomKolay (CargoMini).
TRUNKRS	Trunkrs.
OMNIRPS_WEBHOOK	Omni Returns.
CHILEXPRESS	Chile Express.
TESTING_COURIER	Testing Courier.
JNE_API	JNE (API).
BJSHOMEDELIVERY_FTP	BJS Distribution, Storage & Couriers - FTP.
DEXPRESS_WEBHOOK	D Express.
USPS_API	USPS API.
TRANSVIRTUAL	TransVirtual.
SOLISTICA_API	solistica.
CHIENVENTURE_WEBHOOK	Chienventure.
DPD_UK_SFTP	DPD UK.
INPOST_UK	InPost.
JAVIT	Javit.
ZTO_DOMESTIC	ZTO Express China.
DHL_GT_API	DHL Global Forwarding Guatemala.
CEVA_TRACKING	CEVA Package.
KOMON_EXPRESS	Komon Express.
EASTWESTCOURIER_FTP	East West Courier Pte Ltd.
DANNIAO	Danniao.
SPECTRAN	Spectran.
DELIVER_IT	Deliver-iT.
RELAISCOLIS	Relais Colis.
GLS_SPAIN_API	GLS Spain.
POSTPLUS	PostPlus.
AIRTERRA	Airterra.
GIO_ECOURIER_API	GIO Express Ecourier.
DPD_CH_SFTP	DPD Switzerland.
FEDEX_API	FedEx®.
INTERSMARTTRANS	INTERSMARTTRANS & SOLUTIONS SL.
HERMES_UK_SFTP	Hermes UK.
EXELOT_FTP	Exelot Ltd..
DHL_PA_API	DHL GLOBAL FORWARDING PANAMÁ.
VIRTRANSPORT_SFTP	Vir Transport.
WORLDNET	Worldnet Logistics.
INSTABOX_WEBHOOK	Instabox.
KNG	Keuhne + Nagel Global.
FLASHEXPRESS_WEBHOOK	Flash Express.
MAGYAR_POSTA_API	Magyar Posta.
WESHIP_API	WeShip.
OHI_WEBHOOK	Ohi.
MUDITA	MUDITA.
BLUEDART_API	Bluedart.
T_CAT_API	T-cat.
ADS	ADS Express.
HERMES_IT	HR Parcel.
FITZMARK_API	FitzMark.
POSTI_API	Posti API.
SMSA_EXPRESS_WEBHOOK	SMSA Express.
TAMERGROUP_WEBHOOK	Tamer Logistics.
LIVRAPIDE	Livrapide.
NIPPON_EXPRESS	Nippon Express.
BETTERTRUCKS	Better Trucks.
FAN	FAN COURIER EXPRESS.
PB_USPSFLATS_FTP	USPS Flats (Pitney Bowes).
PARCELRIGHT	Parcel Right.
ITHINKLOGISTICS	iThink Logistics.
KERRY_EXPRESS_TH_WEBHOOK	Kerry Logistics.
ECOUTIER	eCoutier.
SHOWL	SENHONG INTERNATIONAL LOGISTICS.
BRT_IT_API	BRT Bartolini API.
RIXONHK_API	Rixon Logistics.
DBSCHENKER_API	DB Schenker.
ILYANGLOGIS	Ilyang logistics.
MAIL_BOX_ETC	Mail Boxes Etc..
WESHIP	WeShip.
DHL_GLOBAL_MAIL_API	DHL eCommerce Solutions.
ACTIVOS24_API	Activos24.
ATSHEALTHCARE	ATS Healthcare.
LUWJISTIK	Luwjistik.
GW_WORLD	Gebrüder Weiss.
FAIRSENDEN_API	fairsenden.
SERVIP_WEBHOOK	SerVIP.
SWISHIP	Swiship.
TANET	Transport Ambientales.
HOTSIN_CARGO	SHENZHEN HOTSIN CARGO INT'L FORWARDING CO.,LTD.
DIREX	Direx.
HUANTONG	HuanTong.
IMILE_API	iMile.
AUEXPRESS	Au Express.
NYTLOGISTICS	NYT SUPPLY CHAIN LOGISTICS Co.,LTD.
DSV_REFERENCE	DSV Futurewave.
NOVOFARMA_WEBHOOK	Novofarma.
AITWORLDWIDE_SFTP	AIT.
SHOPOLIVE	Olive.
FNF_ZA	Fast & Furious.
DHL_ECOMMERCE_GC	DHL eCommerce Greater China.
FETCHR	Fetchr.
STARLINKS_API	Starlinks Global.
YYEXPRESS	YYEXPRESS.
SERVIENTREGA	Servientrega.
HANJIN	HanJin.
SPANISH_SEUR_FTP	Spanish Seur.
DX_B2B_CONNUM	DX (B2B).
HELTHJEM_API	Helthjem.
INEXPOST	Inexpost.
A2B_BA	A2B Express Logistics.
RHENUS_GROUP	Rhenus Logistics.
SBERLOGISTICS_RU	Sber Logistics.
MALCA_AMIT	Malca-Amit.
PPL	Professional Parcel Logistics.
OSM_WORLDWIDE_SFTP	OSM Worldwide.
ACILOGISTIX	ACI Logistix.
OPTIMACOURIER	Optima Courier.
NOVA_POSHTA_API	Nova Poshta API.
LOGGI	Loggi.
YIFAN	YiFan Express.
MYDYNALOGIC	My DynaLogic.
MORNINGLOBAL	Morning Global.
CONCISE_API	Concise.
FXTRAN	Falcon Express.
DELIVERYOURPARCEL_ZA	Deliver Your Parcel.
UPARCEL	uParcel.
MOBI_BR	Mobi Logistica.
LOGINEXT_WEBHOOK	T&W Delivery.
EMS	EMS.
SPEEDY	Speedy.
ZOOM_RED	Zoom.
NAVLUNGO	Navlungo.
CASTLEPARCELS	Castle Parcels.
WEEE	Weee.
PACKALY	Packaly.
YUNHUIPOST	Yunhuipost.
YOUPARCEL	YouParcel.
LEMAN	Leman.
MOOVIN	Moovin.
URB_IT	Urb-it.
MULTIENTREGAPANAMA	Multientrega.
JUSDASR	Jusdasr.
DISCOUNTPOST	Discount Post.
RHENUS_UK	Rhenus Logistics UK.
SWISHIP_JP	Swiship JP.
GLS_US	GLS USA.
SMTL	Southwestern Motor Transport. Inc.
EMEGA	Discount Post Emega.
EXPRESSONE_SV	EXPRESSONE Slovenia.
HEPSIJET	hepsiJET.
WELIVERY	Welivery.
BRINGER	Bringer Parcel Services.
EASYROUTES	EasyRoutes.
MRW	MRW.
RPM	RPM.
DPD_PRT	DPD Portugal.
GLS_ROMANIA	GLS Romania.
LMPARCEL	LM Parcel.
GTAGSM	GTA GSM.
DOMINO	DOMINO.
ESHIPPER	eShipper.
TRANSPAK	Transpak Inc..
XINDUS	Xindus.
AOYUE	Aoyue.
EASYPARCEL	Easyparcel.
EXPRESSONE	EXPRESSONE.
SENDEO_KARGO	Sendeo Kargo.
SPEEDAF	Speedaf Express.
ETOWER	eTower.
GCX	GC Express.
NINJAVAN_VN	Ninjavan Vietnam.
ALLEGRO	Allegro.
JUMPPOINT	Jumppoint.
SHIPGLOBAL_US	ShipGlobal.
KINISI	Kinisi Transport Pty Ltd.
OAKH	Oakh Harbour Freight Lines.
AWEST	American West.
BARSAN	Barsan Global Lojistik.
ENERGOLOGISTIC	Energo Logistic.
MADROOEX	Madrooex.
GOBOLT	GoBolt.
SWISS_UNIVERSAL_EXPRESS	Swiss Universal Express.
IORDIRECT	IOR Direct Solutions.
XMSZM	xmszm.
GLS_HUN	GLS Hungary.
SENDY	Sendy Express.
BRAUNSEXPRESS	Brauns Express.
GRANDSLAMEXPRESS	Grand Slam Express.
XGS	XGS.
OTSCHILE	OTS.
PACK_UP	Pack-Up.
PARCELSTARS	Parcelstars.
TEAMEXPRESSLLC	Team Express Service LLC.
ASYADEXPRESS	Asyad Express.
TDN	TDN.
EARLYBIRD	Early Bird.
CACESA	Cacesa.
PARCELJET	Parceljet.
MNG_KARGO	MNG Kargo.
SUPERPACKLINE	Super Pac Line.
SPEEDX	SpeedX.
VESYL	Vesyl.
SKYKING	Sky King.
DIRMENSAJERIA	DIR.
NETLOGIXGROUP	Netlogix.
ZYOU	ZYEX.
JAWAR	Jawar.
AGSYSTEMS	Associate Global Systems.
GPS	GPS.
PTT_KARGO	PTT Kargo.
MAERGO	Maergo.
ARIHANTCOURIER	AICS.
VTFE	VicTas Freight Express.
YUNANT	Yunant.
URBIFY	Urbify.
PACK_MAN	pack-man.
LIEFERGRUN	LIEFERGRUN.
OBIBOX	Obibox.
PAIKEDA	Paikeda.
SCOTTY	Scotty.
INTELCOM_CA	Intelcom.
SWE	swe.
ASENDIA	Asendia Global.
DPD_AT	DPD Austria.
RELAY	Relay.
ATA	ATA.
SKYEXPRESS_INTERNATIONAL	SkyExpress Internationals.
SURAT_KARGO	Surat Kargo.
SGLINK	SG LINK.
FLEETOPTICSINC	FleetOptics.
SHOPLINE	shopline.
PIGGYSHIP	PIGGYSHIP.
LOGOIX	LogoiX.
KOLAY_GELSIN	Kolay Gelsin.
ASSOCIATED_COURIERS	Associated Couriers.
UPS_CHECKER	ups-checker.
WINESHIPPING	Wineshipping.
SPEDISCI	Spedisci online.
FOURKITES	Fourkites.
ETONAS	Etonas.
FINMILE	Fin Mile.
UNIUNI	Uniuni.
RODONAVES	Rodonaves.
INPOST_IT	Inpost Italy.
TFORCE_FREIGHT	Tforce Freight.
RICHMOM	Rich Mom.
FRANCO	Corriere Franco.
ECPARCEL	Ecparcel.
FEDEX_CHINA	Fedex China.
GOFO_EXPRESS	Gofo Express.
SHIPBOB	Shipbob.
JERSEYPOST_ATLAS	Jersey Post Group.
CORETRAILS	Coretrails.
RHENUS_ITALY	Rhenus Logistics Italy.
JADLOG	Jadlog.
JITSU	Jitsu.
YANWEN_EXPRESS	Yanwen Express.
DASHLINK	Dashlink.
SEINO_SUPER_EXPRESS	Seino Super Express.
FLOSHIP	Floship.
METROSCG	Metro Supply Chain.
SENDPARCEL	Sendparcel.
P2P	P2p.
CN_EXPRESS	Cn Express.
CIRROTRACK	Cirro Track.
LAND_LOGISTICS	Land Logistics.
VEHO	Veho.
MEDLINE	Medline.
VDTRACK	Vdtrack.
SINO_SCM	Sino Scm.
3PE_EXPRESS	3pe Express.
SWIFTX	Swiftx.
SFYDEXPRESS	Sfyd Express.
TOPTRANS	Toptrans.
capture_id
required
string [ 1 .. 50 ] characters
The PayPal capture ID.

notify_payer	
boolean
Default: false
If true, PayPal will send an email notification to the payer of the PayPal transaction. The email contains the tracking details provided through the Orders tracking API request. Independent of any value passed for notify_payer, the payer may receive tracking notifications within the PayPal app, based on the user's notification preferences.

items	
Array of objects
An array of details of items in the shipment.

CopyExpand allCollapse all
{
"tracking_number": "string",
"carrier_name_other": "string",
"carrier": "DPD_RU",
"capture_id": "string",
"notify_payer": false,
"items": [
{
"name": "string",
"quantity": "string",
"sku": "string",
"url": "http://example.com",
"image_url": "http://example.com",
"upc": {
"type": "UPC-A",
"code": "string"
}
}
]
}
tracking_number_type
The tracking number type.

string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The tracking number type.

Enum Value	Description
CARRIER_PROVIDED	A carrier-provided tracking number.
E2E_PARTNER_PROVIDED	A marketplace-provided tracking number.
Copy
"CARRIER_PROVIDED"
tracking_status
The status of the item shipment. For allowed values, see Shipping Statuses.

string [ 1 .. 64 ] characters ^[0-9A-Z_]+$
The status of the item shipment. For allowed values, see Shipping Statuses.

Enum Value	Description
CANCELLED	The shipment was cancelled and the tracking number no longer applies.
DELIVERED	The item was already delivered when the tracking number was uploaded.
LOCAL_PICKUP	Either the buyer physically picked up the item or the seller delivered the item in person without involving any couriers or postal companies.
ON_HOLD	The item is on hold. Its shipment was temporarily stopped due to bad weather, a strike, customs, or another reason.
SHIPPED	The item was shipped and is on the way.
Copy
"CANCELLED"
trustly
Information needed to pay using Trustly.

name	
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

email	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with this payment method.

bic	
string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The bank identification code (BIC).

iban_last_chars	
string [ 4 .. 34 ] characters [a-zA-Z0-9]{4}
The last characters of the IBAN used to pay.

Copy
{
"name": "string",
"country_code": "string",
"email": "string",
"bic": "string",
"iban_last_chars": "string"
}
trustly_request
Information needed to pay using Trustly.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

email
required
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with this payment method.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
{
"name": "string",
"country_code": "string",
"email": "string",
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
}
universal_product_code
The Universal Product Code of the item.

type
required
string [ 1 .. 5 ] characters ^[0-9A-Z_-]+$
The Universal Product Code type.

Enum Value	Description
UPC-A	N/A
UPC-B	N/A
UPC-C	N/A
UPC-D	N/A
UPC-E	N/A
UPC-2	N/A
UPC-5	N/A
code
required
string [ 6 .. 17 ] characters
The UPC product code of the item.

Copy
{
"type": "UPC-A",
"code": "string"
}
url
Describes the URL.

string (url)
Describes the URL.

Copy
"http://example.com"
v3_vault_instruction_base
Base vaulting specification. The object can be extended for specific use cases within each payment_source that supports vaulting.

store_in_vault
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Defines how and when the payment source gets vaulted.

Value	Description
ON_SUCCESS	Defines that the payment_source will be vaulted only when at least one authorization or capture using that payment_source is successful.
Copy
{
"store_in_vault": "ON_SUCCESS"
}
vault_id
The PayPal-generated ID for the vaulted payment source. This ID should be stored on the merchant's server so the saved payment source can be used for future transactions.

string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated ID for the vaulted payment source. This ID should be stored on the merchant's server so the saved payment source can be used for future transactions.

Copy
"string"
vault_instruction_base
Basic vault instruction specification that can be extended by specific payment sources that supports vaulting.

store_in_vault	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Defines how and when the payment source gets vaulted.

Value	Description
ON_SUCCESS	Defines that the payment_source will be vaulted only when at least one authorization or capture using that payment_source is successful.
Copy
{
"store_in_vault": "ON_SUCCESS"
}
vault_paypal_wallet_base
Resource consolidating common request and response attributes for vaulting PayPal Wallet.

store_in_vault	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Defines how and when the payment source gets vaulted.

Value	Description
ON_SUCCESS	Defines that the payment_source will be vaulted only when at least one authorization or capture using that payment_source is successful.
description	
string [ 1 .. 128 ] characters
The description displayed to PayPal consumer on the approval flow for PayPal, as well as on the PayPal payment token management experience on PayPal.com.

usage_pattern	
string [ 1 .. 30 ] characters
Expected business/pricing model for the billing agreement.

Enum Value	Description
IMMEDIATE	On-demand instant payments – non-recurring, pre-paid, variable amount, variable frequency.
DEFERRED	Pay after use, non-recurring post-paid, variable amount, irregular frequency.
RECURRING_PREPAID	Pay upfront fixed or variable amount on a fixed date before the goods/service is delivered.
RECURRING_POSTPAID	Pay on a fixed date based on usage or consumption after the goods/service is delivered.
THRESHOLD_PREPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, before the goods/service is delivered.
THRESHOLD_POSTPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, after the goods/service is delivered.
SUBSCRIPTION_PREPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due before the good/service is delivered.
SUBSCRIPTION_POSTPAID	Subscription plan where the "amount due" and the "billing frequency" are fixed, and there is no defined duration with the payment due after the goods/services are delivered.
UNSCHEDULED_PREPAID	Unscheduled card on file plan where the merchant can bill buyer upfront based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
UNSCHEDULED_POSTPAID	Unscheduled card on file plan where the merchant can bill buyer based on an agreed logic, but "amount due" and "frequency" can vary. Inclusive of automatic reload plans.
INSTALLMENT_PREPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due before the good/service is delivered.
INSTALLMENT_POSTPAID	Merchant-managed installment plan when the "amount" to be paid and the "billing frequency" are fixed, but there is a defined number of payments with the payment due after the goods/services are delivered.
usage_type
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The usage type associated with the PayPal payment token.

Enum Value	Description
MERCHANT	The PayPal Payment Token will be used for future transaction directly with a merchant.
PLATFORM	The PayPal Payment Token will be used for future transaction on a platform. A platform is typically a marketplace or a channel that a payer can purchase goods and services from multiple merchants.
customer_type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "CONSUMER"
The customer type associated with the PayPal payment token. This is to indicate whether the customer acting on the merchant / platform is either a business or a consumer.

Enum Value	Description
CONSUMER	The customer vaulting the PayPal payment token is a consumer on the merchant / platform.
BUSINESS	The customer vaulting the PayPal payment token is a business on merchant / platform.
permit_multiple_payment_tokens	
boolean
Default: false
Create multiple payment tokens for the same payer, merchant/platform combination. Use this when the customer has not logged in at merchant/platform. The payment token thus generated, can then also be used to create the customer account at merchant/platform. Use this also when multiple payment tokens are required for the same payer, different customer at merchant/platform. This helps to identify customers distinctly even though they may share the same PayPal account. This only applies to PayPal payment source.

Copy
{
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
vault_response
The details about a saved payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The vault status.

Enum Value	Description
VAULTED	The payment source has been saved in your customer's vault. This vault status reflects /v3/vault status.
CREATED	DEPRECATED. The payment source has been saved in your customer's vault. This status applies to deprecated integration patterns and will not be returned for v3/vault integrations.
APPROVED	Customer has approved the action of saving the specified payment_source into their vault. Use v3/vault/payment-tokens with given setup_token to save the payment source in the vault
links	
Array of objects [ 1 .. 10 ] items
An array of request-related HATEOAS links.

CopyExpand allCollapse all
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
]
}
vault_response
The details about a saved payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The vault status.

Enum Value	Description
VAULTED	The payment source has been saved in your customer's vault. This vault status reflects /v3/vault status.
CREATED	DEPRECATED. The payment source has been saved in your customer's vault. This status applies to deprecated integration patterns and will not be returned for v3/vault integrations.
APPROVED	Customer has approved the action of saving the specified payment_source into their vault. Use v3/vault/payment-tokens with given setup_token to save the payment source in the vault
links	
Array of objects [ 1 .. 10 ] items
An array of request-related HATEOAS links.

CopyExpand allCollapse all
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
]
}
vault_response
The details about a saved payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The vault status.

Enum Value	Description
VAULTED	The payment source has been saved in your customer's vault. This vault status reflects /v3/vault status.
CREATED	DEPRECATED. The payment source has been saved in your customer's vault. This status applies to deprecated integration patterns and will not be returned for v3/vault integrations.
APPROVED	Customer has approved the action of saving the specified payment_source into their vault. Use v3/vault/payment-tokens with given setup_token to save the payment source in the vault
links	
Array of objects [ 1 .. 10 ] items
An array of request-related HATEOAS links.

customer	
object
This object represents a merchant’s customer, allowing them to store contact details, and track all payments associated with the same customer.

CopyExpand allCollapse all
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"name": {
"given_name": "string",
"surname": "string"
}
}
}
vault_response
The details about a saved payment source.

id	
string [ 1 .. 255 ] characters
The PayPal-generated ID for the saved payment source.

status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The vault status.

Enum Value	Description
VAULTED	The payment source has been saved in your customer's vault. This vault status reflects /v3/vault status.
CREATED	DEPRECATED. The payment source has been saved in your customer's vault. This status applies to deprecated integration patterns and will not be returned for v3/vault integrations.
APPROVED	Customer has approved the action of saving the specified payment_source into their vault. Use v3/vault/payment-tokens with given setup_token to save the payment source in the vault
links	
Array of objects [ 1 .. 10 ] items
An array of request-related HATEOAS links.

CopyExpand allCollapse all
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
]
}
vault_Venmo_wallet_base
Resource consolidating common request and response attirbutes for vaulting Venmo Wallet.

store_in_vault
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Defines how and when the payment source gets vaulted.

Value	Description
ON_SUCCESS	Defines that the payment_source will be vaulted only when at least one authorization or capture using that payment_source is successful.
description	
string [ 1 .. 128 ] characters
The description displayed to Venmo consumer on the approval flow for Venmo, as well as on the Venmo payment token management experience on Venmo.com.

usage_pattern	
string [ 1 .. 30 ] characters ^[0-9A-Z_]+$
Expected business/pricing model for the billing agreement.

Enum Value	Description
IMMEDIATE	On-demand instant payments – non-recurring, pre-paid, variable amount, variable frequency.
DEFERRED	Pay after use, non-recurring post-paid, variable amount, irregular frequency.
RECURRING_PREPAID	Pay upfront fixed or variable amount on a fixed date before the goods/service is delivered.
RECURRING_POSTPAID	Pay on a fixed date based on usage or consumption after the goods/service is delivered.
THRESHOLD_PREPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, before the goods/service is delivered.
THRESHOLD_POSTPAID	Charge payer when the set amount is reached or monthly billing cycle, whichever comes first, after the goods/service is delivered.
usage_type
required
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The usage type associated with the Venmo payment token.

Enum Value	Description
MERCHANT	The Venmo Payment Token will be used for future transaction directly with a merchant.
PLATFORM	The Venmo Payment Token will be used for future transaction on a platform. A platform is typically a marketplace or a channel that a payer can purchase goods and services from multiple merchants.
customer_type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "CONSUMER"
The customer type associated with the Venmo payment token. This is to indicate whether the customer acting on the merchant / platform is either a business or a consumer.

Enum Value	Description
CONSUMER	O cliente que armazena o token de pagamento do Venmo é um consumidor na plataforma/comerciante.
NEGÓCIOS	O cliente que armazena o token de pagamento Venmo é uma empresa na plataforma/comerciante.
permitir múltiplos tokens de pagamento
booleano
Padrão: 
falso
Crie vários tokens de pagamento para a mesma combinação de pagador, comerciante/plataforma. Use isso quando o cliente não tiver feito login no comerciante/plataforma. O token de pagamento gerado poderá ser usado para criar a conta do cliente no comerciante/plataforma. Use isso também quando forem necessários vários tokens de pagamento para o mesmo pagador, mas para clientes diferentes no comerciante/plataforma. Isso ajuda a identificar os clientes de forma distinta, mesmo que compartilhem a mesma conta Venmo.

Cópia
{
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
resposta_venmo_vault
Detalhes sobre uma forma de pagamento Venmo salva.

eu ia
string [ 1 .. 255 ] caracteres
O ID gerado pelo PayPal para a forma de pagamento salva.

status
string [ 1 .. 255 ] caracteres ^[0-9A-Z_]+$
O estado do cofre.

Valor de enumeração	Descrição
ABOBADADO	A forma de pagamento foi salva no cofre do seu cliente. O status deste cofre reflete /v3/vaulta situação atual.
CRIADO	OBSOLETO. A forma de pagamento foi salva no cofre do seu cliente. Este status se aplica a padrões de integração obsoletos e não será retornado para integrações v3/cofre.
APROVADO	O cliente aprovou a ação de salvar a fonte de pagamento especificada em seu cofre. Use v3/vault/payment-tokens com o setup_token fornecido para salvar a fonte de pagamento no cofre.

links
Matriz de objetos [ 1 .. 10 ] itens
Uma série de links HATEOAS relacionados a solicitações.


cliente
objeto
Este objeto representa o cliente de um comerciante, permitindo-lhe armazenar detalhes de contato e rastrear todos os pagamentos associados ao mesmo cliente.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
atributos_da_carteira_venmo
Atributos adicionais associados ao uso desta carteira Venmo.


cliente
objeto
Este objeto representa o cliente de um comerciante, permitindo-lhe armazenar detalhes de contato e rastrear todos os pagamentos associados ao mesmo cliente.


cofre
objeto
Atributos usados ​​para fornecer as instruções durante o processo de armazenamento seguro da Carteira Venmo.

CópiaExpandir tudoRecolher tudo
{
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
venmo_wallet_attributes_response
Atributos adicionais associados ao uso de uma carteira Venmo.


cofre
objeto
Detalhes sobre uma forma de pagamento Venmo salva.

CópiaExpandir tudoRecolher tudo
{
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
}
cliente_carteira_venmo
Este objeto representa o cliente de um comerciante, permitindo-lhe armazenar detalhes de contato e rastrear todos os pagamentos associados ao mesmo cliente.

eu ia
string [ 1 .. 22 ] caracteres ^[0-9a-zA-Z_-]+$
O ID exclusivo de um cliente gerado pelo PayPal.

endereço de email
string [ 3 .. 254 ] caracteres( e-mail) (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Mostrar padrão
Endereço de e-mail do cliente, conforme fornecido ao comerciante ou registrado em seus arquivos. O endereço de e-mail é obrigatório caso a transação seja processada por meio do Processamento de Convidados do PayPal, recurso oferecido a parceiros e comerciantes selecionados.


telefone
objeto
O número de telefone do cliente, conforme fornecido ao comerciante ou registrado em seus arquivos. phone.phone_numberApenas para suporte national_number.


nome
objeto
O nome completo do cliente, conforme fornecido ao comerciante ou registrado em arquivo pelo comerciante.

CópiaExpandir tudoRecolher tudo
{
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
venmo_wallet_experience_context
Personaliza a experiência do comprador durante o processo de aprovação de pagamento com o Venmo.

Observação: Parceiros e marketplaces podem configurar opções shipping_preferencedurante a configuração da conta de parceiro, o que substituirá os valores da solicitação.
nome_da_marca
string [ 1 .. 127 ] caracteres ^.*$
Nome comercial do comerciante. O padrão é definido por uma entidade externa e é compatível com Unicode.

preferência de envio
string [ 1 .. 24 ] caracteres ^[A-Z_]+$
Padrão: 
"OBTER_DO_ARQUIVO"
A localização a partir da qual o endereço de entrega é derivado.

Valor de enumeração	Descrição
OBTER_DO_ARQUIVO	Obtenha o endereço de entrega fornecido pelo cliente no site do PayPal.
SEM ENVIO	Oculta o endereço de entrega do site do PayPal. Recomendado para produtos digitais.
DEFINIR_ENDEREÇO_FORNECIDO	Obtenha o endereço fornecido pelo comerciante. O cliente não pode alterar esse endereço no site do PayPal. Se o comerciante não fornecer um endereço, o cliente poderá escolher o endereço nas páginas do PayPal.
ação_do_usuário
string [ 1 .. 8 ] caracteres ^[0-9A-Z_]+$
Padrão: 
"CONTINUAR"
Configura um fluxo de finalização de compra " Continuar " ou "Pagar agora" .

Valor de enumeração	Descrição
CONTINUAR	Após redirecionar o cliente para a página de pagamento do Venmo, um botão "Continuar" será exibido. Use essa opção quando o valor final não for conhecido no momento do início do processo de finalização da compra e você quiser redirecionar o cliente para a página do comerciante sem processar o pagamento.
PAGAR AGORA	Após redirecionar o cliente para a página de pagamento do Venmo, um botão "Pagar agora" será exibido. Use essa opção quando o valor final for conhecido no momento do início do processo de finalização da compra e você quiser processar o pagamento imediatamente assim que o cliente clicar em " Pagar agora" .

order_update_callback_config
objeto
O comerciante forneceu a configuração de retorno de chamada de atualização de pedido para a carteira Venmo. O Venmo retornará a ligação para o comerciante quando o evento especificado ocorrer. Recomendamos que os comerciantes passem os eventos de retorno de chamada `shipping_options` e `shipping_address`. Não é compatível quando shipping.type`shipping_address` é especificado ou quando `application_context.shipping_preference` está definido como `NO_SHIPPING` ou `SET_PROVIDED_ADDRESS`.

CópiaExpandir tudoRecolher tudo
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
}
solicitação_de_carteira_venmo
Informações necessárias para efetuar o pagamento via Venmo.


contexto_de_experiência
objeto
Personaliza a experiência do comprador durante o processo de aprovação de pagamento com o Venmo.

Observação: Parceiros e marketplaces podem configurar opções shipping_preferencedurante a configuração da conta de parceiro, o que substituirá os valores da solicitação.
id_do_cofre
string [ 1 .. 255 ] caracteres ^[0-9a-zA-Z_-]+$
O ID gerado pelo PayPal para a fonte de pagamento salva na carteira Venmo (payment_source). Esse ID deve ser armazenado no servidor do comerciante para que a fonte de pagamento salva possa ser usada em transações futuras.

endereço de email
string [ 3 .. 254 ] caracteres( e-mail) (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Mostrar padrão
O endereço de e-mail do pagador.


atributos
objeto
Atributos adicionais associados ao uso desta carteira.

CópiaExpandir tudoRecolher tudo
{
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"user_action": "CONTINUE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
},
"vault": {
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
}
}
resposta_da_carteira_venmo
Resposta da carteira Venmo.

nome de usuário
string [ 1 .. 50 ] caracteres ^[-a-zA-Z0-9_]*$
O nome de usuário do Venmo escolhido pelo usuário, também conhecido como identificador do Venmo.

fluxo de retorno
string [ 1 .. 6 ] caracteres ^[A-Z_]+$
Padrão: 
"AUTO"
Preferência do comerciante sobre como o comprador pode navegar de volta ao site do comerciante após aprovar a transação no aplicativo Venmo.

Valor de enumeração	Descrição
AUTO	Após a aprovação do pagamento no aplicativo PayPal, o comprador será redirecionado automaticamente para o site do comerciante.
MANUAL	Após a aprovação do pagamento no aplicativo PayPal, o comprador será solicitado a navegar manualmente de volta ao site do comerciante onde iniciou a transação. Uma mensagem como "Retornar ao comerciante" será exibida para que o comprador retorne à origem da transação.

atributos
objeto
Atributos adicionais associados ao uso de uma carteira Venmo.

endereço de email
string [ 3 .. 254 ] caracteres (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...( e-mail ) Mostrar padrão
O endereço de e-mail do pagador.

id_da_conta
string = 13 caracteres ^[2-9A-HJ-NP-Z]{13}$( id_da_conta))
Este é um ID imutável gerado pelo sistema para a conta Venmo de um usuário.


nome
objeto
O nome associado à conta Venmo. Suporta apenas as propriedades `name` given_namee surname`name`.


número_de_telefone
objeto
O número de telefone associado à conta Venmo, em seu formato canônico internacional de numeração E.164 . Suporta apenas a national_numberpropriedade.


endereço
objeto
Endereço do pagador. Suporta apenas as propriedades address_line_1`id` address_line_2, admin_area_1`name`, admin_area_2`id`, postal_code`name` e `name` country_code. Também conhecido como endereço de cobrança do cliente.

CópiaExpandir tudoRecolher tudo
{
"user_name": "string",
"return_flow": "AUTO",
"attributes": {
"vault": {
"id": "string",
"status": "VAULTED",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"customer": {
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
},
"name": {
"given_name": "string",
"surname": "string"
}
}
}
},
"email_address": "string",
"account_id": "stringstrings",
"name": {
"given_name": "string",
"surname": "string"
},
"phone_number": {
"national_number": "string"
},
"address": {
"address_line_1": "string",
"address_line_2": "string",
"admin_area_2": "string",
"admin_area_1": "string",
"postal_code": "string",
"country_code": "st"
}
}
Referência
PayPal.com
Privacidade
Apoiar
Jurídico
Contato
