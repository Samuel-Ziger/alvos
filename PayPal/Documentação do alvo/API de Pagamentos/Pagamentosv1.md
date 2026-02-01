
Pagamentos
Versão da API v1
Aviso de descontinuação: O /v1/paymentsendpoint está obsoleto. Use o /v2/paymentsendpoint em vez disso. Para obter detalhes, consulte Integração básica do PayPal Checkout .
Use a API REST de Pagamentos para aceitar pagamentos online e via dispositivos móveis de forma fácil e segura. O namespace de pagamentos contém coleções de recursos para pagamentos, vendas, reembolsos, autorizações, capturas e pedidos.
Importante: O uso das /paymentsAPIs REST do PayPal para aceitar pagamentos com cartão de crédito é restrito. Em vez disso, você pode aceitar pagamentos com cartão de crédito usando o Braintree Direct .
Dependendo do país, você pode permitir que seus clientes façam pagamentos via PayPal e cartão de crédito com apenas alguns cliques. Você pode aceitar um pagamento imediatamente ou autorizar um pagamento e capturá-lo posteriormente. Você pode exibir detalhes de pagamentos concluídos, reembolsos e autorizações. Você pode fazer reembolsos totais ou parciais. Você também pode cancelar ou reautorizar autorizações. Para obter mais informações, consulte a Visão geral de Pagamentos .
Criar pagamento
publicar
/v1/pagamentos/pagamento
Experimente
Aviso de descontinuação: O /v1/paymentsendpoint está obsoleto. Use o /v2/paymentsendpoint em vez disso. Para obter detalhes, consulte Integração básica do PayPal Checkout .
Cria uma venda, um pagamento autorizado para ser capturado posteriormente ou um pedido. Para criar uma venda, autorização ou pedido, inclua os detalhes do pagamento no corpo da solicitação JSON. Defina o valor intentpara sale, authorize, ou order.
Nota: Os clientes TPP (provedores de serviços terceirizados no contexto da regulamentação PSD2) estão proibidos de usar authorizee de orderimplementar tais funcionalidades.
Inclua o pagador, os detalhes da transação e, somente para pagamentos via PayPal, os URLs de redirecionamento. A combinação desses payment_methodparâmetros funding_instrumentdetermina o tipo de pagamento criado. Para mais informações, consulte a API REST de Pagamentos .
Segurança
OAuth2
Solicitar
Parâmetros do cabeçalho
ID de atribuição de parceiro do PayPal
string com menos de 32 caracteres
Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
intenção
obrigatório
corda
A intenção de pagamento. O valor é:

saleEfetua um pagamento imediato.
authorize. Autoriza um pagamento para captura posterior .
orderCria um pedido .
Enum : 
"oferta"
 
"autorizar"
 
"ordem"

transações
Conjunto de objetos
Um conjunto de transações relacionadas a pagamentos. Uma transação define o motivo do pagamento e quem o efetua. Para chamadas de atualização e execução de pagamento, o transactionsobjeto aceita amountapenas o objeto.

id_do_perfil_de_experiência
corda
Obsoleto. O ID gerado pelo PayPal para o perfil de experiência de pagamento do comerciante. Para obter informações, consulte Criar perfil de experiência da Web . Use application_context em vez disso.

nota_ao_pagador
string <= 165 caracteres
Um campo de texto livre que os clientes podem usar para enviar uma mensagem ao pagador.


redirecionar_urls
objeto
Um conjunto de URLs de redirecionamento que você fornece para pagamentos via PayPal.


pagador
obrigatório
objeto
A origem dos fundos para este pagamento. O método de pagamento é Carteira PayPal ou débito direto em conta bancária.


contexto_de_aplicação
objeto
Utilize o recurso de contexto do aplicativo para personalizar a experiência do fluxo de pagamento para seus compradores.

Respostas
201Uma solicitação bem-sucedida retorna o 201 Createdcódigo de status HTTP e um corpo de resposta JSON que exibe os detalhes do pagamento.
Solicitar amostras
Carga útil
cURL

aplicativo/json
aplicativo/json

Exemplo 1 - 201 - Criar pagamento PayPal
Exemplo 1 - 201 - Criar pagamento PayPal
CópiaExpandir tudoRecolher tudo
{
"intent": "sale",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "30.11",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "0.07",
"shipping": "0.03",
"handling_fee": "1.00",
"shipping_discount": "-1.00",
"insurance": "0.01"
}
},
"description": "The payment transaction description.",
"custom": "EBAY_EMS_90048630024435",
"invoice_number": "48787589673",
"payment_options": {
"allowed_payment_method": "INSTANT_FUNDING_SOURCE"
},
"soft_descriptor": "ECHI5786786",
"item_list": {
"items": [
{
"name": "hat",
"description": "Brown hat.",
"quantity": "5",
"price": "3",
"tax": "0.01",
"sku": "1",
"currency": "USD"
},
{
"name": "handbag",
"description": "Black handbag.",
"quantity": "1",
"price": "15",
"tax": "0.02",
"sku": "product34",
"currency": "USD"
}
],
"shipping_address": {
"recipient_name": "Brian Robinson",
"line1": "4th Floor",
"line2": "Unit #34",
"city": "San Jose",
"country_code": "US",
"postal_code": "95131",
"phone": "011862212345678",
"state": "CA"
}
}
}
],
"note_to_payer": "Contact us for any questions on your order.",
"redirect_urls": {
"return_url": "https://example.com/return",
"cancel_url": "https://example.com/cancel"
}
}
Amostras de resposta
201
aplicativo/json

Exemplo 1 - 201 - Criar pagamento PayPal
Exemplo 1 - 201 - Criar pagamento PayPal
CópiaExpandir tudoRecolher tudo
{
"id": "PAY-1B56960729604235TKQQIYVY",
"create_time": "2017-09-22T20:53:43Z",
"update_time": "2017-09-22T20:53:44Z",
"state": "CREATED",
"intent": "sale",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "30.11",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "0.07",
"shipping": "0.03",
"handling_fee": "1.00",
"insurance": "0.01",
"shipping_discount": "-1.00"
}
},
"description": "The payment transaction description.",
"custom": "EBAY_EMS_90048630024435",
"invoice_number": "48787589673",
"item_list": {
"items": [
{
"name": "hat",
"sku": "1",
"price": "3.00",
"currency": "USD",
"quantity": "5",
"description": "Brown hat.",
"tax": "0.01"
},
{
"name": "handbag",
"sku": "product34",
"price": "15.00",
"currency": "USD",
"quantity": "1",
"description": "Black handbag.",
"tax": "0.02"
}
],
"shipping_address": {
"recipient_name": "Brian Robinson",
"line1": "4th Floor",
"line2": "Unit #34",
"city": "San Jose",
"state": "CA",
"phone": "011862212345678",
"postal_code": "95131",
"country_code": "US"
}
}
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-1B56960729604235TKQQIYVY",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.sandbox.paypal.com/cgi-bin/webscr?cmd=_express-checkout&token=EC-60385559L1062554J",
"rel": "approval_url",
"method": "REDIRECT"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-1B56960729604235TKQQIYVY/execute",
"rel": "execute",
"method": "POST"
}
]
}
Lista de pagamentos
pegar
/v1/pagamentos/pagamento
Experimente
Aviso de descontinuação: O /v1/paymentsendpoint está obsoleto. Use o /v2/paymentsendpoint em vez disso. Para obter detalhes, consulte Integração básica do PayPal Checkout .
Lista os pagamentos concluídos. Os pagamentos que você acabou de criar com a chamada `create payment` não aparecem na lista.

A lista mostra os pagamentos feitos ao comerciante que fez a chamada. Para filtrar os pagamentos que aparecem na resposta, você pode especificar um ou mais parâmetros opcionais de consulta e paginação. Consulte Filtragem e paginação .
Segurança
OAuth2
Solicitar
Parâmetros de consulta
contar
inteiro <= 20
Padrão: 
10
O número de itens a serem listados na resposta.

id_inicial
corda
O ID do recurso inicial na resposta. Quando os resultados são paginados, você pode usar esse next_idvalor para start_idcontinuar com o próximo conjunto de resultados.

índice_inicial
inteiro
The start index of the payments to list. Typically, you use the start_index to jump to a specific position in the resource history based on its cart. For example, to start at the second item in a list of results, specify ?start_index=2.

start_time	
string
The start date and time for the range to show in the response, in Internet date and time format. For example, start_time=2016-03-06T11:00:00Z.

end_time	
string
The end date and time for the range to show in the response, in Internet date and time format. For example, end_time=2016-03-06T11:00:00Z.

payee_id	
string
Filters the payments in the response by a PayPal-assigned merchant ID that identifies the payee.

sort_by	
string
Sorts the payments in the response by a create time.

Value: "create_time"
sort_order	
string
Sorts the payments in the response in descending order.

Value: "desc"
Request Body schema: 
application/json
application/json
any
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that lists payments with payment details.
Request samples
Payload
cURL

application/json
application/json
Copy
{ }
Response samples
200
application/json

Sample 1 - 200 - List Payments
Sample 1 - 200 - List Payments
CopyExpand allCollapse all
{
"payments": [
{
"id": "PAY-0US81985GW1191216KOY7OXA",
"create_time": "2017-06-30T23:48:44Z",
"update_time": "2017-06-30T23:49:27Z",
"state": "APPROVED",
"intent": "order",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "41.15",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "1.15",
"shipping": "10.00"
}
},
"description": "The payment transaction description.",
"item_list": {
"items": [
{
"name": "hat",
"sku": "1",
"price": "3.00",
"currency": "USD",
"quantity": "5"
},
{
"name": "handbag",
"sku": "product34",
"price": "15.00",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "John Doe",
"line1": "4th Floor, One Lagoon Drive",
"line2": "Unit #34",
"city": "Redwood City",
"state": "CA",
"phone": "4084217591",
"postal_code": "94065",
"country_code": "US"
}
},
"related_resources": [
{
"authorization": {
"id": "53P09338XY5426455",
"create_time": "2017-06-30T23:50:01Z",
"update_time": "2017-06-30T23:50:01Z",
"amount": {
"total": "41.15",
"currency": "USD"
},
"parent_payment": "PAY-0US81985GW1191216KOY7OXA",
"valid_until": "2017-07-29T23:49:52Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0US81985GW1191216KOY7OXA",
"rel": "parent_payment",
"method": "GET"
}
]
}
}
]
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0US81985GW1191216KOY7OXA",
"rel": "self",
"method": "GET"
}
]
},
{
"id": "PAY-53485002LD6169910KOZQ25I",
"create_time": "2017-07-01T19:35:17Z",
"update_time": "2017-07-01T19:36:05Z",
"state": "APPROVED",
"intent": "order",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "33.00",
"currency": "USD",
"details": {
"subtotal": "21.00",
"tax": "2.00",
"shipping": "10.00"
}
},
"description": "The payment transaction description.",
"item_list": {
"items": [
{
"name": "hat",
"sku": "1",
"price": "3.00",
"currency": "USD",
"quantity": "2"
},
{
"name": "handbag",
"sku": "product34",
"price": "15.00",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "Hannah Lu",
"line1": "1602 Crane ct",
"line2": "",
"city": "San Jose",
"state": "CA",
"phone": "4084217591",
"postal_code": "95052",
"country_code": "US"
}
},
"related_resources": [
{
"authorization": {
"id": "91527087GH224122L",
"create_time": "2017-07-01T19:36:22Z",
"update_time": "2017-07-01T19:36:22Z",
"amount": {
"total": "33.00",
"currency": "USD"
},
"parent_payment": "PAY-53485002LD6169910KOZQ25I",
"valid_until": "2017-07-30T19:36:22Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-53485002LD6169910KOZQ25I",
"rel": "parent_payment",
"method": "GET"
}
]
}
}
]
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-53485002LD6169910KOZQ25I",
"rel": "self",
"method": "GET"
}
]
},
{
"id": "PAY-7F5790198P134484LKOZSG7Q",
"create_time": "2017-07-01T21:09:18Z",
"update_time": "2017-07-01T22:31:56Z",
"state": "APPROVED",
"intent": "order",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "42.00",
"currency": "USD",
"details": {
"subtotal": "36.00",
"tax": "1.00",
"shipping": "5.00"
}
},
"description": "The payment transaction description.",
"item_list": {
"items": [
{
"name": "handbag",
"sku": "product34",
"price": "36.00",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "Anna Joseph",
"line1": "2525 North 1st street",
"line2": "unit 4",
"city": "San Jose",
"state": "CA",
"phone": "011862212345678",
"postal_code": "95031",
"country_code": "US"
}
},
"related_resources": [
{
"capture": {
"id": "26062838D7499294V",
"create_time": "2017-07-01T21:16:22Z",
"update_time": "2017-07-01T21:16:24Z",
"amount": {
"total": "7.00",
"currency": "USD"
},
"state": "COMPLETED",
"parent_payment": "PAY-7F5790198P134484LKOZSG7Q",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/26062838D7499294V",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/26062838D7499294V/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-7F5790198P134484LKOZSG7Q",
"rel": "parent_payment",
"method": "GET"
}
]
}
},
{
"capture": {
"id": "0YU20012P1477553M",
"create_time": "2017-07-01T22:31:54Z",
"update_time": "2017-07-01T22:31:56Z",
"amount": {
"total": "35.00",
"currency": "USD"
},
"state": "COMPLETED",
"parent_payment": "PAY-7F5790198P134484LKOZSG7Q",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/0YU20012P1477553M",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/0YU20012P1477553M/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-7F5790198P134484LKOZSG7Q",
"rel": "parent_payment",
"method": "GET"
}
]
}
}
]
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-7F5790198P134484LKOZSG7Q",
"rel": "self",
"method": "GET"
}
]
}
],
"count": 3,
"next_id": "PAY-9X4935091L753623RKOZTRHI"
}
Show payment details
get
/v1/payments/payment/{payment_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for a payment, by ID, that has yet to complete. For example, shows details for a payment that was created, approved, or failed.
Security
Oauth2
Request
path Parameters
payment_id
required
string
The ID of the payment for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows payment details.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0US81985GW1191216KOY7OXA \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Response samples
200
application/json

Sample 1 - 200 - Show Payment Details for BOPIS use case
Sample 1 - 200 - Show Payment Details for BOPIS use case
CopyExpand allCollapse all
{
"id": "PAY-0US81985GW1191216KOY7OXA",
"create_time": "2017-06-30T23:48:44Z",
"update_time": "2017-06-30T23:49:27Z",
"state": "APPROVED",
"intent": "order",
"payer": {
"payment_method": "paypal"
},
"transactions": [
{
"amount": {
"total": "41.15",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "1.15",
"shipping": "10.00"
}
},
"description": "The payment transaction description.",
"item_list": {
"items": [
{
"name": "hat",
"sku": "1",
"price": "3.00",
"currency": "USD",
"quantity": "5"
},
{
"name": "handbag",
"sku": "product34",
"price": "15.00",
"currency": "USD",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "John Doe",
"line1": "4th Floor, One Lagoon Drive",
"line2": "Unit #34",
"city": "Redwood City",
"state": "CA",
"phone": "4084217591",
"postal_code": "94065",
"country_code": "US"
},
"shipping_options": [
{
"id": "PICKUP0000001",
"label": "Free Shipping",
"type": "PICKUP",
"amount": {
"currency_code": "USD",
"value": "5.00"
},
"selected": true
}
]
},
"related_resources": [
{
"authorization": {
"id": "53P09338XY5426455",
"create_time": "2017-06-30T23:50:01Z",
"update_time": "2017-06-30T23:50:01Z",
"amount": {
"total": "41.15",
"currency": "USD"
},
"parent_payment": "PAY-0US81985GW1191216KOY7OXA",
"valid_until": "2017-07-29T23:49:52Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0US81985GW1191216KOY7OXA",
"rel": "parent_payment",
"method": "GET"
}
]
}
}
]
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0US81985GW1191216KOY7OXA",
"rel": "self",
"method": "GET"
}
]
}
Partially update payment
patch
/v1/payments/payment/{payment_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Partially updates a payment, by ID. You can update the amount, shipping address, invoice ID, and custom data. You cannot update a payment after the payment executes.
Note: TPP Clients (Third Party Providers in the context of PSD2 regulation) are restricted from patching amount once authorized.
Security
Oauth2
Request
path Parameters
payment_id
required
string
The ID of the payment to update.

Request Body schema: application/json
Array 
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
object
The value to apply. The remove operation does not require a value.

from	
string
The JSON Pointer to the target document location from which to move the value. Required for the move operation.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows payment details.
Request samples
Payload
cURL
application/json

Sample 1 - 200 - Update Payment
Sample 1 - 200 - Update Payment
CopyExpand allCollapse all
[
{
"op": "replace",
"path": "/transactions/0/item_list/shipping_options",
"value": [
{
"id": "PICKUP0000001",
"type": "PICKUP",
"label": "Pickup Info",
"amount": {
"currency": "USD",
"value": "10.00"
},
"selected": true
}
]
}
]
Response samples
200
application/json

Sample 1 - 200 - Update Payment
Sample 1 - 200 - Update Payment
CopyExpand allCollapse all
{
"id": "PAY-5YK922393D847794YKER7MUI",
"intent": "authorize",
"create_time": "2017-04-24T14:26:59.059Z",
"payer": {
"payer_info": {
"country_code": "DE",
"email": "payer@example.com",
"first_name": "Gruneberg",
"last_name": "Anna",
"payer_id": "8J4VWY56VUXQ6",
"phone": "605-521-1234"
},
"payment_method": "paypal",
"status": "VERIFIED"
},
"state": "APPROVED",
"transactions": [
{
"amount": {
"total": "18.37",
"currency": "EUR",
"details": {
"subtotal": "13.37",
"shipping": "5.00"
}
},
"description": "Uber",
"item_list": {
"items": [
{
"currency": "EUR",
"name": "iPad",
"price": "13.37",
"quantity": "1"
}
],
"shipping_address": {
"recipient_name": "Anna Gruneberg",
"line1": "Kathwarinenhof 1",
"city": "Flensburg",
"postal_code": "24939",
"country_code": "DE"
},
"shipping_options": [
{
"id": "PICKUP0000001",
"label": "Pickup Info",
"type": "PICKUP",
"amount": {
"value": "10.00",
"currency": "USD"
},
"selected": true
}
]
},
"payee": {
"email": "payee@example.com"
}
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-5YK922393D847794YKER7MUI",
"method": "GET",
"rel": "self"
}
]
}
Execute approved PayPal payment
post
/v1/payments/payment/{payment_id}/execute
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Executes a PayPal payment that the customer has approved. You can optionally update one or more transactions when you execute the payment.
Important: This call works only after a customer has approved the payment. For more information, learn about PayPal payments.
Security
Oauth2
Request
path Parameters
payment_id
required
string
The ID of the payment to execute.

header Parameters
PayPal-Request-Id	
string <= 78 characters
The server stores keys for 30 days.

PayPal-Partner-Attribution-Id	
string <= 32 characters
Request Body schema: 
application/json
application/json
payer_id	
string
The ID of the payer that PayPal passes in the return_url.

transactions	
Array of objects
An array of transaction details. Includes the amount and item details. For update and execute payment calls, the transactions object accepts only the amount object.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows details for the executed payment.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 200 - Execute Order
Sample 1 - 200 - Execute Order
Copy
{
"payer_id": "CR87QHB7JTRSC"
}
Response samples
200
application/json

Sample 1 - 200 - Execute Order
Sample 1 - 200 - Execute Order
CopyExpand allCollapse all
{
"id": "PAY-9N9834337A9191208KOZOQWI",
"create_time": "2017-07-01T16:56:57Z",
"update_time": "2017-07-01T17:05:41Z",
"state": "APPROVED",
"intent": "order",
"payer": {
"payment_method": "paypal",
"payer_info": {
"email": "qa152-biz@example.com",
"first_name": "Thomas",
"last_name": "Miller",
"payer_id": "PUP87RBJV8HPU",
"shipping_address": {
"line1": "4th Floor, One Lagoon Drive",
"line2": "Unit #34",
"city": "Redwood City",
"state": "CA",
"postal_code": "94065",
"country_code": "US",
"phone": "011862212345678",
"recipient_name": "Thomas Miller"
}
}
},
"transactions": [
{
"amount": {
"total": "41.15",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "0.15",
"shipping": "11.00"
}
},
"description": "The payment transaction description.",
"item_list": {
"items": [
{
"name": "hat",
"sku": "1",
"price": "3.00",
"currency": "USD",
"quantity": "5"
},
{
"name": "handbag",
"sku": "product34",
"price": "15.00",
"currency": "USD",
"quantity": "1"
}
],
"shipping_options": [
{
"id": "PICKUP0000001",
"label": "Free Shipping",
"type": "PICKUP",
"amount": {
"currency_code": "USD",
"value": "5.00"
},
"selected": true
}
],
"shipping_address": {
"recipient_name": "Thomas Miller",
"line1": "4th Floor, One Lagoon Drive",
"line2": "Unit #34",
"city": "Redwood City",
"state": "CA",
"phone": "011862212345678",
"postal_code": "94065",
"country_code": "US"
}
},
"related_resources": [
{
"order": {
"id": "O-3SP845109F051535C",
"create_time": "2017-07-01T16:56:58Z",
"update_time": "2017-07-01T17:05:41Z",
"state": "PENDING",
"amount": {
"total": "41.15",
"currency": "USD"
},
"parent_payment": "PAY-9N9834337A9191208KOZOQWI",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-3SP845109F051535C",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-9N9834337A9191208KOZOQWI",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-3SP845109F051535C/void",
"rel": "void",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-3SP845109F051535C/authorize",
"rel": "authorization",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-3SP845109F051535C/capture",
"rel": "capture",
"method": "POST"
}
]
}
}
]
}
],
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-9N9834337A9191208KOZOQWI",
"rel": "self",
"method": "GET"
}
]
}
Show sale details
get
/v1/payments/sale/{sale_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for a sale, by ID. Returns only sales that were created through the REST API.
Security
Oauth2
Request
path Parameters
sale_id
required
string
The ID of the sale for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows sale details.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/sale/34P65696MB8050805 \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Response samples
200
application/json

Sample 1 - 200 - Show Sale Details
Sample 1 - 200 - Show Sale Details
CopyExpand allCollapse all
{
"id": "PAYID-L22F5TI6U989123YN3449833",
"intent": "sale",
"state": "approved",
"cart": "7FB02792N0577162W",
"payer": {
"payment_method": "paypal",
"status": "VERIFIED",
"payer_info": {
"email": "testaccountlta@p.com(opens in new tab)",
"first_name": "Edward",
"last_name": "Daphne",
"payer_id": "EY5JYNPZYU43U",
"shipping_address": {
"recipient_name": "Hello World",
"line1": "4th Floor,One Lagoon Drive",
"line2": "unit #34",
"city": "Redwood City",
"state": "CA",
"postal_code": "94065",
"country_code": "US"
},
"country_code": "US"
}
},
"transactions": [
{
"amount": {
"total": "10.00",
"currency": "USD",
"details": {
"subtotal": "0.00",
"tax": "0.00",
"shipping": "0.00",
"insurance": "0.00",
"handling_fee": "0.00",
"shipping_discount": "0.00"
}
},
"payee": {
"merchant_id": "D87XQWW2N6V8W",
"email": "_sys_aquarium-2171730659297732@paypal.com(opens in new tab)"
},
"description": "This is the payment transaction description.",
"custom": "Nouphal Custom",
"item_list": {
"shipping_address": {
"recipient_name": "Hello World",
"line1": "4th Floor,One Lagoon Drive",
"line2": "unit #34",
"city": "Redwood City",
"state": "CA",
"postal_code": "94065",
"country_code": "US"
},
"shipping_phone_number": "4084217591"
},
"related_resources": [
{
"sale": {
"id": "0GP63733UF4238932",
"state": "completed",
"amount": {
"total": "10.00",
"currency": "USD",
"details": {
"subtotal": "0.00",
"tax": "0.00",
"shipping": "0.00",
"insurance": "0.00",
"handling_fee": "0.00",
"shipping_discount": "0.00"
}
},
"payment_mode": "INSTANT_TRANSFER",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE,UNAUTHORIZED_PAYMENT_ELIGIBLE",
"transaction_fee": {
"value": "0.22",
"currency": "USD"
},
"transaction_fee_in_receivable_currency": {
"value": "1",
"currency": "CNY"
},
"receivable_amount": {
"value": "59.26",
"currency": "CNY"
},
"exchange_rate": "5.9483297432325",
"parent_payment": "PAYID-L22F5TI6U989123YN3449833",
"create_time": "2020-05-07T19:18:28Z",
"update_time": "2020-05-07T19:18:28Z",
"links": [
{
"href": "https://www.msmaster.qa.paypal.com:12326/v1/payments/sale/0GP63733UF4238932",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.msmaster.qa.paypal.com:12326/v1/payments/sale/0GP63733UF4238932/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://www.msmaster.qa.paypal.com:12326/v1/payments/payment/PAYID-L22F5TI6U989123YN3449833",
"rel": "parent_payment",
"method": "GET"
}
]
}
}
]
}
],
"failed_transactions": [ ],
"create_time": "2020-05-07T19:17:31Z",
"update_time": "2020-05-07T19:18:28Z",
"links": [
{
"href": "https://www.msmaster.qa.paypal.com:12326/v1/payments/payment/PAYID-L22F5TI6U989123YN3449833",
"rel": "self",
"method": "GET"
}
]
}
Refund sale
post
/v1/payments/sale/{sale_id}/refund
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Refunds a sale, by ID. For a full refund, do not include the amount object in the JSON request body. For a partial refund, include an amount object in the JSON request body.
Security
Oauth2
Request
path Parameters
sale_id
required
string
The ID of the sale transaction to refund.

header Parameters
PayPal-Request-Id	
string <= 78 characters
The server stores keys for 30 days.

Request Body schema: 
application/json
application/json
description	
string <= 255 characters
The refund description. Value is a string of single-byte alphanumeric characters.

reason	
string <= 30 characters
The refund reason description.

invoice_number	
string <= 127 characters
The invoice number that tracks this payment. Value is a string of single-byte alphanumeric characters.

amount	
object
The refund amount. Includes both the amount to refund to the payer and the fee amount to refund to the payee.

Responses
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows details for the refunded sale.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Refund Sale by Invoice ID
Sample 1 - 201 - Refund Sale by Invoice ID
CopyExpand allCollapse all
{
"amount": {
"total": "2.34",
"currency": "USD"
},
"using": "INVOICE_ID",
"payer_info": {
"email": "payer@example.com"
}
}
Response samples
201
application/json

Sample 1 - 201 - Refund Sale by Invoice ID
Sample 1 - 201 - Refund Sale by Invoice ID
CopyExpand allCollapse all
{
"id": "4CF18861HF410323U",
"create_time": "2017-01-31T04:13:34Z",
"update_time": "2017-01-31T04:13:36Z",
"state": "COMPLETED",
"amount": {
"total": "2.34",
"currency": "USD"
},
"sale_id": "2MU78835H4515710F",
"parent_payment": "PAY-46E69296BH2194803KEE662Y",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/refund/4CF18861HF410323U",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-46E69296BH2194803KEE662Y",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/sale/2MU78835H4515710F",
"rel": "sale",
"method": "GET"
}
]
}
Show authorization details
get
/v1/payments/authorization/{authorization_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for an authorization, by ID.
Security
Oauth2
Request
path Parameters
authorization_id
required
string
The ID of the authorization for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows authorization details.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/authorization/2DC87612EK520411B \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Response samples
200
application/json

Sample 1 - 200 - Shows Authorization Details
Sample 1 - 200 - Shows Authorization Details
CopyExpand allCollapse all
{
"id": "2DC87612EK520411B",
"create_time": "2017-06-25T21:39:15Z",
"update_time": "2017-06-25T21:39:17Z",
"state": "AUTHORIZED",
"amount": {
"total": "7.47",
"currency": "USD",
"details": {
"subtotal": "7.47"
}
},
"parent_payment": "PAY-36246664YD343335CKHFA4AY",
"valid_until": "2017-07-24T21:39:15Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/2DC87612EK520411B",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/2DC87612EK520411B/capture",
"rel": "capture",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/2DC87612EK520411B/void",
"rel": "void",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-36246664YD343335CKHFA4AY",
"rel": "parent_payment",
"method": "GET"
}
]
}
Capture authorization
post
/v1/payments/authorization/{authorization_id}/capture
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Captures and processes an authorization, by ID. The original payment call must specify an intent of authorize.
Security
Oauth2
Request
path Parameters
authorization_id
required
string
The ID of the authorization to capture.

Request Body schema: 
application/json
application/json
is_final_capture	
boolean
Default: false
Indicates whether to release all remaining held funds.

invoice_number	
string <= 127 characters
The invoice number to track this payment.

note_to_payer	
string <= 255 characters
A free-form field that clients can use to send a note to the payer.

amount	
object
The amount to capture. If the amount matches the originally authorized amount, the state of the authorization changes to captured. Otherwise, the state changes to partially_captured.

Responses
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows details for the captured authorization.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Capture Funds for Authorized Payment
Sample 1 - 201 - Capture Funds for Authorized Payment
CopyExpand allCollapse all
{
"amount": {
"currency": "USD",
"total": "4.54"
},
"is_final_capture": true
}
Response samples
201
application/json

Sample 1 - 201 - Capture Funds for Authorized Payment
Sample 1 - 201 - Capture Funds for Authorized Payment
CopyExpand allCollapse all
{
"id": "05L60402VH257294E",
"amount": {
"total": "1.10",
"currency": "USD"
},
"state": "completed",
"reason_code": "NONE",
"custom": "Nouphal Custom",
"transaction_fee": {
"value": "0.35",
"currency": "USD"
},
"transaction_fee_in_receivable_currency": {
"value": "0.35",
"currency": "CNY"
},
"receivable_amount": {
"value": "0.07",
"currency": "CNY"
},
"exchange_rate": "0.009370279361376",
"is_final_capture": false,
"parent_payment": "PAYID-L2ZOT5Y1K876380CL583530L",
"invoice_number": "",
"create_time": "2020-05-06T16:49:32Z",
"update_time": "2020-05-06T16:49:32Z",
"links": [
{
"href": "https://www.${stage_domain}:12326/v1/payments/capture/05L60402VH257294E",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.${stage_domain}:12326/v1/payments/capture/05L60402VH257294E/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://www.${stage_domain}:12326/v1/payments/authorization/6JD14952FS4103539",
"rel": "authorization",
"method": "GET"
},
{
"href": "https://www.${stage_domain}:12326/v1/payments/payment/PAYID-L2ZOT5Y1K876380CL583530L",
"rel": "parent_payment",
"method": "GET"
}
]
}
Void authorization
post
/v1/payments/authorization/{authorization_id}/void
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Voids, or cancels, an authorization, by ID. You cannot void a fully captured authorization.
Security
Oauth2
Request
path Parameters
authorization_id
required
string
The ID of the authorization to void.

header Parameters
PayPal-Request-Id	
string <= 78 characters
The server stores keys for 30 days.

Request Body schema: 
application/json
application/json
any
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows details for the voided authorization.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 200 - Void Authorization
Sample 1 - 200 - Void Authorization
Copy
{ }
Response samples
200
application/json

Sample 1 - 200 - Void Authorization
Sample 1 - 200 - Void Authorization
CopyExpand allCollapse all
{
"id": "6CR34526N64144512",
"create_time": "2017-05-06T21:56:50Z",
"update_time": "2017-05-06T21:57:51Z",
"state": "VOIDED",
"amount": {
"total": "110.54",
"currency": "USD",
"details": {
"subtotal": "110.54"
}
},
"parent_payment": "PAY-0PL82432AD7432233KGECOIQ",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/6CR34526N64144512",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0PL82432AD7432233KGECOIQ",
"rel": "parent_payment",
"method": "GET"
}
]
}
Re-authorize payment
post
/v1/payments/authorization/{authorization_id}/reauthorize
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Re-authorizes a PayPal account payment, by authorization ID. To ensure that funds are still available, re-authorize a payment after the initial three-day honor period. Supports only the amount request parameter. You can re-authorize a payment only once from four to 29 days after three-day honor period for the original authorization expires. If 30 days have passed from the original authorization, you must create a new authorization instead. A re-authorized payment itself has a new three-day honor period. You can re-authorize a transaction once for up to 115% of the originally authorized amount, not to exceed an increase of $75 USD.
Security
Oauth2
Request
path Parameters
authorization_id
required
string
The ID of the authorization to re-authorize.

Request Body schema: 
application/json
application/json
fmf_details	
object
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

processor_response	
object
The processor-provided response codes that describe the submitted payment. Supported only when the payment_method is credit_card.

amount
required
object
The payment amount, with details.

Responses
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows details for the re-authorized authorization.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Re-authorize Authorization
Sample 1 - 201 - Re-authorize Authorization
CopyExpand allCollapse all
{
"amount": {
"total": "7.00",
"currency": "USD"
}
}
Response samples
201
application/json

Sample 1 - 201 - Re-authorize Authorization
Sample 1 - 201 - Re-authorize Authorization
CopyExpand allCollapse all
{
"id": "8AA831015G517922L",
"create_time": "2017-06-25T21:39:15Z",
"update_time": "2017-06-25T21:39:17Z",
"state": "AUTHORIZED",
"amount": {
"total": "7.00",
"currency": "USD"
},
"parent_payment": "PAY-7LD317540C810384EKHFAGYA",
"valid_until": "2017-07-24T21:39:15Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/8AA831015G517922L",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-7LD317540C810384EKHFAGYA",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/8AA831015G517922L/capture",
"rel": "capture",
"method": "POST"
}
]
}
Show order details
get
/v1/payments/orders/{order_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for an order, by ID.
Security
Oauth2
Request
path Parameters
order_id
required
string
The ID of the order for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows details for the voided authorization.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/orders/O-0PW72302W3743444R \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Response samples
200
application/json

Sample 1 - 200 - Show Order Details
Sample 1 - 200 - Show Order Details
CopyExpand allCollapse all
{
"id": "O-0PW72302W3743444R",
"create_time": "2017-06-19T22:05:06Z",
"update_time": "2017-06-19T22:08:36Z",
"state": "PENDING",
"amount": {
"total": "41.15",
"currency": "USD"
},
"pending_reason": "order",
"parent_payment": "PAY-4D805864V5423372TKOQLRUQ",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-0PW72302W3743444R",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-4D805864V5423372TKOQLRUQ",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-0PW72302W3743444R/authorization",
"rel": "authorize",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-0PW72302W3743444R/capture",
"rel": "capture",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-0PW72302W3743444R/void",
"rel": "void",
"method": "POST"
}
]
}
Capture order
post
/v1/payments/orders/{order_id}/capture
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Captures a payment for an order, by ID. To use this call, the original payment call must specify an order intent. In the JSON request body, include the payment amount and indicate whether this capture is the final capture for the authorization.
Security
Oauth2
Request
path Parameters
order_id
required
string
The ID of the order to capture.

Request Body schema: 
application/json
application/json
is_final_capture	
boolean
Default: false
Indicates whether to release all remaining held funds.

invoice_number	
string <= 127 characters
The invoice number to track this payment.

note_to_payer	
string <= 255 characters
A free-form field that clients can use to send a note to the payer.

amount	
object
The amount to capture. If the amount matches the originally authorized amount, the state of the authorization changes to captured. Otherwise, the state changes to partially_captured.

Responses
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows details for the captured order.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Capture Funds for Order
Sample 1 - 201 - Capture Funds for Order
CopyExpand allCollapse all
{
"amount": {
"currency": "USD",
"total": "4.54"
},
"is_final_capture": true
}
Response samples
201
application/json

Sample 1 - 201 - Capture Funds for Order
Sample 1 - 201 - Capture Funds for Order
CopyExpand allCollapse all
{
"id": "51366113MA710110S",
"create_time": "2017-07-01T17:13:45Z",
"update_time": "2017-07-01T17:13:47Z",
"amount": {
"total": "7.00",
"currency": "USD"
},
"is_final_capture": false,
"state": "COMPLETED",
"parent_payment": "PAY-9N9834337A9191208KOZOQWI",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/51366113MA710110S",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/51366113MA710110S/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-3SP845109F051535C",
"rel": "order",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-9N9834337A9191208KOZOQWI",
"rel": "parent_payment",
"method": "GET"
}
]
}
Void order
post
/v1/payments/orders/{order_id}/do-void
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Voids, or cancels, an order, by ID. You can only void orders that are either in the PENDING or AUTHORIZED states or those in the CAPTURED state that are not fully captured.
Security
Oauth2
Request
path Parameters
order_id
required
string
The ID of the order to void.

header Parameters
PayPal-Request-Id	
string <= 78 characters
The server stores keys for 30 days.

Request Body schema: 
application/json
application/json
any
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows details for the voided order.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 200 - Void Order
Sample 1 - 200 - Void Order
Copy
{ }
Response samples
200
application/json

Sample 1 - 200 - Void Order
Sample 1 - 200 - Void Order
CopyExpand allCollapse all
{
"id": "O-0NR488530V5211123",
"create_time": "2017-06-28T07:35:08Z",
"update_time": "2017-06-28T07:36:25Z",
"state": "VOIDED",
"amount": {
"total": "41.15",
"currency": "USD",
"details": {
"subtotal": "30.00",
"tax": "0.15",
"shipping": "11.00"
}
},
"parent_payment": "PAY-0AY778532K612520BKOXHAKY",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/O-0NR488530V5211123",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0AY778532K612520BKOXHAKY",
"rel": "parent_payment",
"method": "GET"
}
]
}
Authorize order
post
/v1/payments/orders/{order_id}/authorize
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Authorizes an order, by ID. In the JSON request body, include an amount object.
Security
Oauth2
Request
path Parameters
order_id
required
string
The ID of the order to authorize.

Request Body schema: 
application/json
application/json
fmf_details	
object
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

amount
required
object
The amount to collect.

Note: For an order authorization, you cannot include amount details.
Responses
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows details for the authorized order.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Authorizes Order
Sample 1 - 201 - Authorizes Order
CopyExpand allCollapse all
{
"amount": {
"currency": "USD",
"total": "4.54"
}
}
Response samples
201
application/json

Sample 1 - 201 - Authorizes Order
Sample 1 - 201 - Authorizes Order
CopyExpand allCollapse all
{
"id": "0PG032325D352531H",
"create_time": "2017-06-28T07:38:10Z",
"update_time": "2017-06-28T07:38:12Z",
"state": "PENDING",
"amount": {
"total": "41.15",
"currency": "USD"
},
"parent_payment": "O-0NR488530V5211123",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/orders/O-0NR488530V5211123",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/0PG032325D352531H",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/authorization/0PG032325D352531H/capture",
"rel": "capture",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-0AY778532K612520BKOXHAKY",
"rel": "parent_payment",
"method": "GET"
}
]
}
Show captured payment details
get
/v1/payments/capture/{capture_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for a captured payment, by ID.
Security
Oauth2
Request
path Parameters
capture_id
required
string
The ID of the captured payment for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows details for the captured payment.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/capture/26186325LB254292P \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  \ 
-d  '{}' 
Response samples
200
application/json

Sample 1 - 200 - Shows denied Capture Details
Sample 1 - 200 - Shows denied Capture Details
CopyExpand allCollapse all
{
"id": "26186325LB254292P",
"amount": {
"total": "188.00",
"currency": "USD"
},
"state": "DENIED",
"custom": "8a26ba02-8b50-4c49-ab53-921e97d2f1a0",
"parent_payment": "PAYID-LIL265Q8YW05382SU050021N",
"invoice_number": "1058763:6563c66a-2254-404e-88c4-333b43a9cbcd",
"create_time": "2017-11-24T05:35:05Z",
"update_time": "2017-11-27T05:38:06Z",
"links": [
{
"href": "https://api-m.paypal.com/v1/payments/capture/26186325LB254292P",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.paypal.com/v1/payments/capture/26186325LB254292P/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api-m.paypal.com/v1/payments/payment/PAYID-LIL265Q8YW05382SU050021N",
"rel": "parent_payment",
"method": "GET"
}
]
}
Refund captured payment
post
/v1/payments/capture/{capture_id}/refund
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Refunds a captured payment, by ID. In the JSON request body, include an amount object.
Security
Oauth2
Request
path Parameters
capture_id
required
string
The ID of the captured payment to refund.

header Parameters
PayPal-Request-Id	
string <= 78 characters
The server stores keys for 30 days.

Request Body schema: 
application/json
application/json
description	
string <= 255 characters
The refund description. Value is a string of single-byte alphanumeric characters.

reason	
string <= 30 characters
The refund reason description.

invoice_number	
string <= 127 characters
The invoice number that tracks this payment. Value is a string of single-byte alphanumeric characters.

amount	
object
The refund amount. Includes both the amount to refund to the payer and the fee amount to refund to the payee.

Responses
201A successful request returns the HTTP 201 OK status code and a JSON response body that shows details for the captured payment.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Refund Capture by Invoice ID
Sample 1 - 201 - Refund Capture by Invoice ID
CopyExpand allCollapse all
{
"amount": {
"currency": "USD",
"total": "110.54"
},
"using": "INVOICE_ID",
"payer_info": {
"email": "payer@example.com"
}
}
Response samples
201
application/json

Sample 1 - 201 - Refund Capture by Invoice ID
Sample 1 - 201 - Refund Capture by Invoice ID
CopyExpand allCollapse all
{
"id": "0P209507D6694645N",
"create_time": "2017-05-06T22:11:51Z",
"update_time": "2017-05-06T22:11:51Z",
"state": "COMPLETED",
"amount": {
"total": "110.54",
"currency": "USD"
},
"capture_id": "8F148933LY9388354",
"parent_payment": "PAY-8PT597110X687430LKGECATA",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/refund/0P209507D6694645N",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/payment/PAY-8PT597110X687430LKGECATA",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v1/payments/capture/8F148933LY9388354",
"rel": "capture",
"method": "GET"
}
]
}
Show refund details
get
/v1/payments/refund/{refund_id}
Try it
Deprecation notice: The /v1/payments endpoint is deprecated. Use the /v2/payments endpoint instead. For details, see PayPal Checkout Basic Integration.
Shows details for a refund, by ID.
Security
Oauth2
Request
path Parameters
refund_id
required
string
The ID of the refund for which to show details.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows refund details.
Request samples
cURL
Copy
curl  -v  -X GET https://api-m.sandbox.paypal.com/v1/payments/refund/6G729074TC0089708 \ 
-H  'Authorization: Bearer access_token6V7rbVwmlM1gFZKW_8QtzWXqpcwQ6T5vhEGYNJDAAdn3paCgRpdeMdVYmWzgbKSsECednupJ3Zx5Xd-g'  
Response samples
200
application/json

Sample 1 - 200 - Show Refund Details
Sample 1 - 200 - Show Refund Details
CopyExpand allCollapse all
{
"id": "6G729074TC0089708",
"state": "completed",
"amount": {
"total": "2.10",
"currency": "USD"
},
"refund_from_received_amount": {
"value": "2.03",
"currency": "USD"
},
"refund_from_transaction_fee": {
"value": "0.07",
"currency": "USD"
},
"total_refunded_amount": {
"value": "2.10",
"currency": "USD"
},
"refund_to_payer": {
"value": "2.10",
"currency": "USD"
},
"refund_from_partner_fee": {
"value": "1.00",
"currency": "USD"
},
"invoice_number": "",
"custom": "Custom Order 001",
"parent_payment": "PAYID-LV4N6XY29519853018496600",
"sale_id": "49001682WB0416317",
"create_time": "2019-09-11T12:12:51Z",
"update_time": "2019-09-11T12:12:51Z",
"links": [
{
"href": "https://www.${stage_domain}:11888/v1/payments/refund/6G729074TC0089708",
"rel": "self",
"method": "GET"
},
{
"href": "https://www.${stage_domain}:11888/v1/payments/payment/PAYID-LV4N6XY29519853018496600",
"rel": "parent_payment",
"method": "GET"
},
{
"href": "https://www.${stage_domain}:11888/v1/payments/sale/49001682WB0416317",
"rel": "sale",
"method": "GET"
}
],
"refund_reason_code": "REFUND"
}
Errors
AGREEMENT_ALREADY_CANCELLED
Message:
The requested agreement is already canceled.

Description: The agreement that was used to make the payment is already canceled.

AMOUNT_MISMATCH
Message:
The totals of the cart item amounts do not match sale amounts.

Description: The amount total does not match the item amount totals.

AUTH_SETTLE_NOT_ALLOWED
Message:
Authorization and Capture feature is not enabled for the merchant.

Description: Make sure that the recipient of the funds is a verified business account.

AUTHORIZATION_ALREADY_COMPLETED
Message:
Capture refused - this authorization has already been completed.

Description: The capture on the authorization failed because it was already completed by one or more previous captures on this authorization.

AUTHORIZATION_AMOUNT_LIMIT_EXCEEDED
Message:
Authorization amount exceeds allowed order limit.

Description: The authorization amount exceeds the allowed order limit.

AUTHORIZATION_CANNOT_BE_VOIDED
Message:
Authorization is in [x] state and hence cannot be voided.

Description: You cannot void this authorization because it is in a state, such as captured or expired, that cannot be voided.

AUTHORIZATION_EXPIRED
Message:
Authorization has expired.

Description: The authorization related to this request has expired. You must reauthorize the transaction.

AUTHORIZATION_ID_DOES_NOT_EXIST
Message:
The requested authorization ID does not exist.

Description: The authorization ID in the request does not exist in the PayPal system.

AUTHORIZATION_VOIDED
Message:
Authorization has been voided.

Description: This authorization was already voided. You can show authorization details for a voided authorization.

BA_TOKEN_CREATION_FAILED
Message:
Billing agreement token creation failed.

Description: The billing agreement token creation failed.

BANK_ACCOUNT_VALIDATION_FAILED
Message:
Bank account validation failed.

Description: The validation of the bank account failed.

BILLING_AGREEMENT_CREATION_FAILED
Message:
Billing agreement creation failed.

Description: The billing agreement creation failed.

BILLING_AGREEMENT_ID_MISMATCH
Message:
Billing Agreement ID must match the one that was provided during payment creation.

Description: Billing Agreement ID must match the one that was provided during payment creation.

BUYER_NOT_SET
Message:
Buyer is not yet set for this purchase.

Description: The customer cannot make this purchase.

CANNOT_PAY_SELF
Message:
Payer cannot pay him- or herself.

Description: The customer cannot pay him- or herself.

CANNOT_REAUTH_CHILD_AUTHORIZATION
Message:
Can only reauthorize the original authorization, not a reauthorization.

Description: Reauthorization only works on an original authorization ID.

CANNOT_REAUTH_INSIDE_HONOR_PERIOD
Message:
Reauthorization is not allowed within the honor period.

Description: You can only reauthorize a payment after the three-day honor period concludes.

CAPTURE_AMOUNT_LIMIT_EXCEEDED
Message:
Capture amount specified exceeded allowable limit.

Description: You can only capture up to the original authorization amount.

CARD_TOKEN_PAYER_MISMATCH
Message:
payer_id does not match ID for this token.

Description: The payer_id must match the one that was provided when the credit card was stored in the vault.

COMPLIANCE_VIOLATION
Message:
Transaction is declined due to compliance violation.

Description: The transaction is declined due to a compliance violation.

CREDIT_CARD_CVV_CHECK_FAILED
Message:
The credit card CVV check failed.

Description: The CVV provided for the credit card was not valid. Resend the payment with a valid CVV for the credit card.

CREDIT_CARD_REFUSED
Message:
Credit card was refused.

Description: The credit card used for the payment was refused. Resend the request with another credit card.

CREDIT_PAYMENT_NOT_ALLOWED
Message:
Buyer cannot use credit to complete the payment.

Description: You cannot use credit to complete this payment. Ask the customer to retry the transaction with alternate funding instrument.

CURRENCY_MISMATCH
Message:
Currency provided in the request must match the currency of the parent order or authorization.

Description: The currency that was used to capture an authorization must match the original currency of the authorization.

CURRENCY_NOT_ALLOWED
Message:
Currency is not supported.

Description: The currency is not currently supported.

DOMESTIC_TRANSACTION_REQUIRED
Message:
This transaction requires the payee and payer to be resident in the same country, a domestic transaction is required to create this payment.

Description: The payer and payee do not reside in the same country.

DUPLICATE_REQUEST_ID
Message:
The value of PayPal-Request-Id header has already been used.

Description: Resend the request with a unique PayPal-Request-Id header value.

DUPLICATE_TRANSACTION
Message:
Duplicate invoice Id detected.

Description: Resend the request with a unique invoice_number value.

ERROR_AUTH
Message:
Unauthorized payment.

Description: You do not have the proper permissions to complete this request.

EXPIRED_CREDIT_CARD_TOKEN
Message:
Credit card token is expired.

Description: Use the Vault API to store the credit card again.

FEATURE_UNSUPPORTED_FOR_PAYEE
Message:
This feature is unsupported.

Description: This feature is not supported for the payee.

FULL_REFUND_NOT_ALLOWED_AFTER_PARTIAL_REFUND
Message:
Full refund refused - partial refund has already been done on this payment.

Description: You cannot refund the full payment amount after you have partially refunded a payment.

IMMEDIATE_PAY_NOT_SUPPORTED
Message:
Immediate pay is not supported for the specified payment intent.

Description: For this payment intent, immediate payment is not supported.

INSTRUMENT_DECLINED
Message:
The instrument presented was either declined by the processor or bank, or it can't be used for this payment.

Description: If the customer's funding source has insufficient funds, restart the payment and prompt the customer to choose another payment method that is available on your site.

INSUFFICIENT_FUNDS
Message:
Buyer cannot pay - insufficient funds.

Description: The customer must add a valid funding instrument, such as a credit card or bank account, to their PayPal account.

INTERNAL_SERVICE_ERROR
Message:
An internal service error has occurred.

Description: Resend the request at another time. If this error persists, contact PayPal Merchant Technical Support.

INVALID_ACCOUNT_NUMBER
Message:
Account number does not exist.

Description: Provide a valid account number and resend the request.

INVALID_CITY_STATE_ZIP
Message:
The combination of city, state, and zip in the address is invalid.

Description: The address contains an invalid combination of a city, state, and zip code.

INVALID_EXPERIENCE_PROFILE_ID
Message:
The requested experience profile ID was not found.

Description: To get the profile ID for a merchant, list web experience profiles.

INVALID_FACILITATOR_CONFIGURATION
Message:
This transaction cannot be processed due to an invalid facilitator configuration.

Description: To process this transaction type, you must have the right account configuration.

INVALID_PAYER_ID
Message:
Payer ID is invalid.

Description: The specified payer_id is not valid. Resend the request with a valid payer_id.

INVALID_PAYPAL_PAYMENT_TOKEN
Message:
Payment token is invalid. A payment token is typically valid for 3 hours since the time it was issued. Please check the token and try again.

Description: Will be returned only for Google Pay Integration for the Create payment call.

INVALID_PICKUP_ADDRESS
Message:
Invalid shipping address.

Description: If the 'shipping_option.type' is set as 'PICKUP' then the 'shipping_address.recipient_name' should start with 'S2S' meaning Ship To Store. Example: 'S2S My Store'.

INVALID_RESOURCE_ID
Message:
The requested resource ID was not found.

Description: Provide a valid resource ID and resend the request.

MALFORMED_REQUEST
Message:
JSON request is malformed.

Description: Review the JSON request.

MAX_NUMBER_OF_PAYMENT_ATTEMPTS_EXCEEDED
Message:
You have exceeded the maximum number of payment attempts.

Description: The maximum number of payment attempts was reached.

MAXIMUM_ALLOWED_AUTHORIZATION_REACHED_FOR_ORDER
Message:
You have reached the configured maximum number of authorizations allowed for an order.

Description: You cannot create any more authorizations for this order.

MERCHANT_NOT_ENABLED_FOR_CHANNEL_INITIATED_BILLING
Message:
Merchant is not enabled for channel-initiated billing.

Description: The merchant cannot use channel-initiated billing.

MERCHANT_NOT_ENABLED_FOR_REFERENCE_TRANSACTION
Message:
Merchant is not enabled for reference transaction.

Description: The merchant cannot make reference transactions.

NEED_CREDIT_CARD
Message:
Need credit card to complete the payment.

Description: To complete this payment, a credit card is required.

NEED_CREDIT_CARD_OR_BANK_ACCOUNT
Message:
Need bank or credit card to complete the payment.

Description: To complete this payment, a bank card or credit card is required.

NOT_IMPLEMENTED
Message:
Not implemented.

Description: This API capability is not implemented.

ORDER_ALREADY_COMPLETED
Message:
Order has already been voided, expired, or completed.

Description: This order was already voided, completed, or expired.

ORDER_CANNOT_BE_VOIDED
Message:
You can only void orders that are either in the PENDING or AUTHORIZED state, or those in the CAPTURED state that are not fully captured.

Description: Due to the state of the order, you cannot void it.

ORDER_IS_EXPIRED
Message:
Order has expired.

Description: The order has expired.

ORDER_VOIDED
Message:
Order has been voided.

Description: The order was already voided. For more information, you can show order details.

PAYEE_ACCOUNT_INVALID
Message:
Payee account is invalid.

Description: The payee account is not valid.

PAYEE_ACCOUNT_LOCKED_OR_CLOSED
Message:
Payee account is locked or closed.

Description: The recipient account is locked or closed and cannot receive payments.

PAYEE_ACCOUNT_NO_CONFIRMED_EMAIL
Message:
Refused - payee account does not have a confirmed email.

Description: For this transaction to proceed, the payment recipient must have a confirmed email.

PAYEE_ACCOUNT_RESTRICTED
Message:
Refused - payee account is restricted.

Description: The account receiving this payment is restricted and cannot receive payments at this time.

PAYEE_BLOCKED_TRANSACTION
Message:
The Fraud settings for this seller are such that this payment cannot be executed.

Description: The fraud filter configuration for the seller blocks this payment.

PAYEE_COUNTRY_NOT_ENABLED
Message:
Payee country is not enabled for this feature.

Description: The payee country is not enabled for billing product.

PAYER_ACCOUNT_LOCKED_OR_CLOSED
Message:
The payer account cannot be used for this transaction.

Description: The payer account is locked or closed.

PAYER_ACCOUNT_RESTRICTED
Message:
The payer account is restricted.

Description: The payer account cannot be used for this transaction.

PAYER_ACTION_REQUIRED
Message:
Transaction cannot complete successfully, instruct the buyer to return to PP.

Description: The customer must return to PayPal to before the transaction can complete.

PAYER_CANNOT_PAY
Message:
Payer cannot pay for this transaction.

Description: Payer cannot pay for this transaction. Please contact the payer to find other ways to pay for this transaction.

PAYER_COUNTRY_NOT_ENABLED
Message:
Payer country is not enabled for this feature.

Description: The payer country is not enabled for billing product.

PAYER_EMPTY_BILLING_ADDRESS
Message:
Billing address is empty.

Description: The billing address is required for credit card transactions without a PayPal account.

PAYER_ID_MISSING_FOR_CARD_TOKEN
Message:
payer_id is required for payments made with this token.

Description: A payer_id is required when one was used to store the credit card in the vault.

PAYMENT_ALREADY_DONE
Message:
Payment has been done already for this cart.

Description: You have completed the payment for this request already. Look up the transaction to get the details.

PAYMENT_APPROVAL_EXPIRED
Message:
Payment approval has expired.

Description: Inform customers that the transaction has expired and that they must restart the transaction. Offer customers a link to restart the payment flow from payment creation and redirect the customer to PayPal.

PAYMENT_CANNOT_BE_INITIATED
Message:
Payment cannot be processed.

Description: PayPal cannot start processing the payment due to an issue with the specified information.

PAYMENT_DENIED
Message:
PayPal has declined to process this transaction.

Description: PayPal declined the payment due to one or more customer issues.

PAYMENT_EXPIRED
Message:
The payment is expired.

Description: The payment expired because too much time has passed between payment creation or approval and execution of that payment. Restart the payment request starting from payment creation.

PAYMENT_METHOD_UNUSABLE
Message:
Payer cannot pay with this payment method.

Description: The specified payment method is not usable. For example, PayPal business rules do not allow the payment method or the payment method information was incorrect in the request.

PAYMENT_NOT_APPROVED_FOR_EXECUTION
Message:
Payer has not approved payment.

Description: The customer must approve all payments that use the PayPal payment method.

PAYMENT_REQUEST_ID_INVALID
Message:
PayPal request ID is invalid. Please try a different one.

Description: Try a different PayPal request ID.

PAYMENT_STATE_INVALID
Message:
This request is invalid due to the current state of the payment.

Description: The payment state does not allow this kind of request.

PERMISSION_DENIED
Message:
No permission for the requested operation.

Description: You do not have the proper permissions to complete this request.

PHONE_NUMBER_REQUIRED
Message:
This transaction requires the payer to provide a valid phone number.

Description: The customer must provide a phone number to PayPal to proceed with the payment.

PLATFORM_FEE_PAYEE_CANNOT_BE_SAME_AS_PAYER
Message:
The payer cannot pay themselves. The recipient account of the platform fees must be different from the payer account.

Description: The customer cannot pay platform fee for themselves.

PREVIOUS_REQUEST_IN_PROGRESS
Message:
A previous request on this resource is currently in progress. Please wait for some time and try again. It is best to space out the initial and the subsequent request(s) to avoid receiving this error.

Description: This scenario only occurs when making multiple API requests on the same resource within a very short duration. To resolve this, API callers need to make subsequent requests with a delay.

REAUTH_NOT_SUPPORTED_FOR_ORDER
Message:
Re-authorization is not allowed for this type of authorization.

Description: You cannot re-authorize an order.

REDIRECT_PAYER_FOR_ALTERNATE_FUNDING
Message:
Transaction failed. Redirect the payer to select another funding source.

Description: The transaction failed so try another funding instrument.

REFUND_EXCEEDED_TRANSACTION_AMOUNT
Message:
Refund refused - the requested refund amount would exceed the amount of transaction being refunded.

Description: The requested refund must be less than or equal to the original transaction amount. To see the original transaction amount, show the refund details.

REFUND_FAILED_INSUFFICIENT_FUNDS
Message:
Refund failed due to insufficient funds in your PayPal account.

Description: You do not have sufficient funds in your PayPal account to process this refund.

REFUND_TIME_LIMIT_EXCEEDED
Message:
This transaction is too old to refund.

Description: For information about refund time limits, see PayPal Customer Support. After the time limit expires, you must send a payment instead of issuing a refund.

REQUESTED_OPERATION_REFUSED_DUE_TO_SELLER_OPT_OUT
Message:
You cannot perform this operation as payee has opted out on PayPal.com.

Description: The third-party-initiated operations are blocked because the payee has opted out on paypal.com.

REQUIRED_SCOPE_MISSING
Message:
Access token does not have required scope.

Description: You must get user consent with the correct scope for this type of request.

SENDING_LIMIT_EXCEEDED
Message:
The transaction exceeds the buyer's sending limit.

Description: The customer cannot make any more transactions.

SHIPPING_ADDRESS_INVALID
Message:
Provided shipping address is invalid.

Description: The specified shipping address is not valid.

TOO_MANY_REAUTHORIZATIONS
Message:
Maximum number of reauthorizations for this authorization has been reached.

Description: You can only reauthorize a payment once.

TOO_MANY_SETTLEMENTS
Message:
Maximum number of allowable settlements has been reached. No more settlement for the authorization.

Description: The maximum number of settlements was reached.

TRANSACTION_ALREADY_REFUNDED
Message:
Refund transaction refused - this transaction has already been refunded.

Description: You can only refund a transaction up to the original amount. While you can do multiple partial refunds up to the original amount, you can only do a full refund once.

TRANSACTION_LIMIT_EXCEEDED
Message:
Total payment amount exceeded transaction limit.

Description: The transaction limit was exceeded. For information about the PayPal transaction limits, see PayPal Customer Support.

TRANSACTION_RECEIVING_LIMIT_EXCEEDED
Message:
The transaction exceeds the receiver’s receiving limit.

Description: The amount exceeds the maximum amount for a single transaction.

TRANSACTION_REFUSED
Message:
This request was refused.

Description: The possible reasons for this failure are:

The partial refund amount must be less than or equal to the original transaction amount.
The partial refund amount must be less than or equal to the remaining amount.
The partial refund amount is not valid.
The partial refund must be the same currency as the original transaction.
Because a complaint case exists on this transaction, only a refund of the full or full remaining amount of the transaction can be messaged.
You are over the time limit to complete a refund on this transaction.
Cannot do a full refund after a partial refund.
Transaction was fully refunded.
You cannot refund this type of transaction.
You cannot do a partial refund on this transaction.
The merchant account has limitations or restrictions.
Refunds are not supported for the payer's selected payment method. Contact the payer directly to arrange a refund via alternative means.
The time limit set by the payer's selected payment method to refund this transaction has expired. Contact the payer directly to arrange a refund via alternative means.
We're unable to process refunds for the payer's selected payment method. Contact the payer directly to arrange a refund via alternative means.
Please find alternate means to refund the payment to the buyer.
Please check the amount and the currency of the transaction and try again. Alternately, please find alternate means to refund the payment to the buyer.
Please find alternate means to refund the payment to the buyer as this type of transaction cannot be refunded.
Please find alternate means to refund the payment to the buyer or try again after some time.
The card account is closed or never existed. Please find alternate means to refund the payment to the buyer.
The transaction cannot be refunded. Please find alternate means to refund the payment to the buyer.
The transaction was refused because of suspected fraud. Please find alternate means to refund the payment to the buyer or try again after some time.
The transaction cannot be refunded due to regulatory restrictions that are permanent or temporary. Please find alternate means to refund the payment to the buyer or try again after some time.
TRANSACTION_REFUSED_BY_PAYPAL_RISK
Message:
PayPal risk refused this transaction.

Description: PayPal risk assessment refused this transaction.

TRANSACTION_REFUSED_PAYEE_PREFERENCE
Message:
Merchant profile preference is set to automatically deny certain transactions.

Description: The merchant account preferences are set to deny this kind of transaction.

UNAUTHORIZED_PAYMENT
Message:
Unauthorized payment.

Description: This payment was not authorized.

UNSUPPORTED_PAYEE_COUNTRY
Message:
Payee country is not supported for this transaction.

Description: The merchant does not accept payments from this country.

UNSUPPORTED_PAYEE_CURRENCY
Message:
The currency is not accepted by payee.

Description: The merchant does not accept payments in this currency.

VALIDATION_ERROR
Message:
Invalid request - see details.

Description: A validation error occurred with your request.

Definitions
Address
The billing address or shipping address for a payment.

line1
required
string <= 300 characters
The first line of the address. For example, number, street, and so on.

line2	
string <= 300 characters
The second line of the address. For example, suite or apartment number.

city	
string <= 64 characters
The city name.

postal_code	
string
The postal code, which is the zip code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

state	
string <= 300 characters
The code for a US state or the equivalent for other countries. Required for transactions if the address is in one of these countries: Argentina, Brazil, Canada, China, India, Italy, Japan, Mexico, Thailand, or United States.

phone	
string
The phone number, in E.123 format. Maximum length is 50 characters.

normalization_status	
string
The address normalization status. Returned only for payers from Brazil.

Enum Value	Description
UNKNOWN	Unknown.
UNNORMALIZED_USER_PREFERRED	Unnormalized user preferred.
NORMALIZED	Normalized.
UNNORMALIZED	Unnormalized.
type	
string
The type of address. For example, HOME_OR_WORK, GIFT, and so on.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The address country code.

Copy
{
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st"
}
Amount
The payment amount, with details.

currency
required
string
The three-character ISO-4217 currency code. PayPal does not support all currencies.

total
required
string
The total amount charged to the payee by the payer. For refunds, represents the amount that the payee refunds to the original payer. Maximum length is 10 characters, which includes:

Seven digits before the decimal point.
The decimal point.
Two digits after the decimal point.
For currencies like JPY do not support decimals.
details	
object
The additional details about the payment amount.

Note: For an order authorization or capture, you cannot include the amount details object.
CopyExpand allCollapse all
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
apple_pay_attributes
Additional attributes associated with apple pay.

customer	
object
The details about a customer in PayPal's system of record.

vault	
object
Base vaulting specification. The object can be extended for specific use cases within each payment_source that supports vaulting.

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
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
}
}
apple_pay_decrypted_token_data
Information about the Payment data obtained by decrypting Apple Pay token.

device_manufacturer_id	
string [ 1 .. 2000 ] characters
Apple Pay Hex-encoded device manufacturer identifier. The pattern is defined by an external party and supports Unicode.

payment_data_type	
string [ 1 .. 16 ] characters
Indicates the type of payment data passed, in case of Non China the payment data is 3DSECURE and for China it is EMV.

Enum Value	Description
3DSECURE	The card was authenticated using 3D Secure (3DS) authentication scheme. While using this value make sure to populate cryptogram and eci_indicator as part of payment data..
EMV	The card was authenticated using EMV method, which is applicable for China. While using this value make sure to pass emv_data and pin as part of payment data.
transaction_amount	
object
The transaction amount for the payment that the payer has approved on apple platform.

tokenized_card
required
object
Apple Pay tokenized credit card used to pay.

payment_data	
object
Apple Pay payment data object which contains the cryptogram, eci_indicator and other data.

CopyExpand allCollapse all
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
apple_pay_payment_data
Information about the decrypted apple pay payment data for the token like cryptogram, eci indicator.

cryptogram	
string [ 1 .. 2000 ] characters
Online payment cryptogram, as defined by 3D Secure. The pattern is defined by an external party and supports Unicode.

eci_indicator	
string [ 1 .. 256 ] characters
ECI indicator, as defined by 3- Secure. The pattern is defined by an external party and supports Unicode.

emv_data	
string [ 1 .. 2000 ] characters
Encoded Apple Pay EMV Payment Structure used for payments in China. The pattern is defined by an external party and supports Unicode.

pin	
string [ 1 .. 2000 ] characters
Bank Key encrypted Apple Pay PIN. The pattern is defined by an external party and supports Unicode.

Copy
{
"cryptogram": "string",
"eci_indicator": "string",
"emv_data": "string",
"pin": "string"
}
apple_pay_request
Information needed to pay using ApplePay.

id	
string [ 1 .. 250 ] characters
ApplePay transaction identifier, this will be the unique identifier for this transaction provided by Apple. The pattern is defined by an external party and supports Unicode.

stored_credential	
object
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
attributes	
object
Additional attributes associated with apple pay.

name	
string [ 3 .. 300 ] characters
Name on the account holder associated with apple pay.

email_address	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with apple pay.

phone_number	
object
The phone number, in its canonical international E.164 numbering plan format. Supports only the national_number property.

decrypted_token	
object
The decrypted payload details for the apple pay token.

vault_id	
string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated ID for the saved apple pay payment_source. This ID should be stored on the merchant's server so the saved payment source can be used for future transactions.

CopyExpand allCollapse all
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
"vault_id": "string"
}
Application Context
The application context. Set these properties to customize the payment flow experience for your customers.

brand_name	
string <= 127 characters
A label that overrides the business name in the merchant's PayPal account on the PayPal checkout pages.

locale	
string
The locale of pages that the PayPal payment experience displays. Please refer here for list of supported local codes. Defaulted to en_US if not provided or invalid.

landing_page	
string
The type of landing page to show on the PayPal site for customer checkout. To use the non-PayPal account landing page, set to Billing. To use the PayPal account log in landing page, set to Login.

shipping_preference	
string
Default: "GET_FROM_FILE"
The shipping preference.

Enum Value	Description
NO_SHIPPING	Redacts the shipping address from the PayPal pages. Recommended for digital goods.
GET_FROM_FILE	Uses the customer-selected shipping address on PayPal pages.
SET_PROVIDED_ADDRESS	If available, uses the merchant-provided shipping address, which the customer cannot change on the PayPal pages. If the merchant does not provide an address, the customer can enter the address on PayPal pages.
user_action	
string
The user action. Presents the customer with either the Continue or Pay Now checkout flow:

Flow	Action	Description
Pay Now	user_action=commit	After the customer is redirected to the PayPal payment page, shows the Pay Now button.

Use this option when you know the final amount when checkout is initiated and you want to process the payment immediately when the customer clicks Pay Now.
Continue	user_action=continue	After the customer is redirected to the PayPal payment page, shows the Continue button.

Use this option when you do not know the final amount when you initiate the checkout flow and you want to redirect the customer to the merchant page without processing the payment.
payment_pattern	
string [ 3 .. 255 ] characters ^[A-Z_]+$
Provides context (e.g. frequency of payment (Single, Recurring) along with whether (Customer is Present, Not Present) for the payment being processed. For Card and PayPal Vaulted/Billing Agreement transactions, this helps specify the appropriate indicators to the networks (e.g. Mastercard, Visa) which ensures compliance as well as ensure a better auth-rate. For bank processing, indicates to clearing house whether the transaction is recurring or not depending on the option chosen.

Enum Value	Description
CUSTOMER_PRESENT_ONETIME_PURCHASE	If the payments is an e-commerce payment initiated by the customer. Customer typically enters payment information (e.g. card number, approves payment within the PayPal Checkout flow) and such information that has not been previously stored on file.
CUSTOMER_NOT_PRESENT_RECURRING	Subsequent recurring payments (e.g. subscriptions with a fixed amount on a predefined schedule when customer is not present.
CUSTOMER_PRESENT_RECURRING_FIRST	The first payment initiated by the customer which is expected to be followed by a series of subsequent recurring payments transactions (e.g. subscriptions with a fixed amount on a predefined schedule when customer is not present. This is typically used for scenarios where customer stores credentials and makes a purchase on a given date and also set’s up a subscription.
CUSTOMER_PRESENT_ONETIME_PURCHASE_VAULTED	Also known as (card-on-file) transactions. Payment details are stored to enable checkout with one-click, or simply to streamline the checkout process. For card transaction customers typically do not enter a CVC number as part of the Checkout process.
CUSTOMER_NOT_PRESENT_ONETIME_PURCHASE_VAULTED	Unscheduled payments that are not recurring on a predefined schedule (e.g. balance top-up).
MAIL_ORDER_TELEPHONE_ORDER	Payments that are initiated by the customer via the merchant by mail or telephone.
preferred_payment_source	
object
The preferred payment source for the payer. Currently supported only for PayPal Billing Agreements. If provided, checkout experience will have this payment source pre-selected for the payer.

CopyExpand allCollapse all
{
"brand_name": "string",
"locale": "string",
"landing_page": "string",
"shipping_preference": "NO_SHIPPING",
"user_action": "string",
"payment_pattern": "CUSTOMER_PRESENT_ONETIME_PURCHASE",
"preferred_payment_source": {
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
},
"billing_agreement_id": "string",
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
"vault_id": "string"
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": null,
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
},
"attributes": {
"verification": {
"method": "SCA_ALWAYS"
}
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string"
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
Authorization
The authorization details.

id	
string
The ID of the authorization.

payment_mode	
string
The payment mode of the authorization.

Value	Description
INSTANT_TRANSFER	Instant transfer.
state	
string
The authorized payment state.

Enum Value	Description
authorized	The authorized payment is authorized.
partially_captured	The authorized payment is partially captured.
captured	The authorized payment is captured.
expired	The authorized payment is expired.
denied	PayPal can no longer authorize funds against this authorization.
voided	The authorized payment is voided.
reason_code	
string
The reason code for the pending transaction state.

Value	Description
AUTHORIZATION	Authorization.
pending_reason	
string
Deprecated. The reason code for the pending transaction state. Obsolete. Use reason_code instead.

Value	Description
AUTHORIZATION	Authorization.
protection_eligibility	
string
The level of seller protection present for the transaction. Supported for the PayPal payment method only.

Enum Value	Description
ELIGIBLE	Merchant is protected by PayPal's Seller Protection Policy for Unauthorized Payments and Item Not Received.
PARTIALLY_ELIGIBLE	Merchant is protected by PayPal's Seller Protection Policy for Item Not Received or Unauthorized Payments. For details, see protection_eligibility_type.
INELIGIBLE	Merchant is not protected under the Seller Protection Policy.
protection_eligibility_type	
string
The type of seller protection for the transaction. Returned only when the protection_eligibility property is ELIGIBLE or PARTIALLY_ELIGIBLE. Supported for the PayPal payment method only.

Enum Value	Description
ITEM_NOT_RECEIVED_ELIGIBLE	Sellers are protected against claims for items not received.
UNAUTHORIZED_PAYMENT_ELIGIBLE	Sellers are protected against claims for unauthorized payments.
ITEM_NOT_RECEIVED_ELIGIBLE,UNAUTHORIZED_PAYMENT_ELIGIBLE	Sellers are protected against claims for items not received and sellers are protected against claims for unauthorized payments.
fmf_details	
object
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

parent_payment	
string
The ID of the payment on which this transaction is based.

processor_response	
object
The processor-provided response codes that describe the submitted payment. Supported only when the payment_method is credit_card.

valid_until	
string
The date and time when the authorization expires, in Internet date and time format.

create_time	
string
The date and time when the authorization was created, in Internet date and time format.

update_time	
string
The date and time when the authorization was last updated, in Internet date and time format.

receipt_id	
string
The receipt ID, which identifies the payment. Value is 16-digit numeric payment ID number that is returned for guest users.

links	
Array of objects
An array of request-related HATEOAS links.

amount
required
object
The payment amount, with details.

CopyExpand allCollapse all
{
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "authorized",
"reason_code": "AUTHORIZATION",
"pending_reason": "AUTHORIZATION",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"valid_until": "2019-08-24T14:15:22Z",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"receipt_id": "string",
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
bancontact_request
Information needed to pay using Bancontact.

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
BIC
The business identification code (BIC). In payments systems, a BIC is used to identify a specific business, most commonly a bank.

string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The business identification code (BIC). In payments systems, a BIC is used to identify a specific business, most commonly a bank.

Copy
"stringst"
billing_agreement_id
The PayPal billing agreement ID. References an approved recurring payment for goods or services.

string [ 2 .. 128 ] characters ^[a-zA-Z0-9-]+$
The PayPal billing agreement ID. References an approved recurring payment for goods or services.

Copy
"string"
blik_experience_context
Customizes the payer experience during the approval process for the BLIK payment.

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
SET_PROVIDED_ADDRESS	Get the merchant-provided address. The customer cannot change this address on the PayPal site. If merchant does not pass an address, customer can choose the address on PayPal pages.
locale	
string [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The BCP 47-formatted locale of pages that the PayPal payment experience shows. PayPal supports a five-character code. For example, da-DK, he-IL, id-ID, ja-JP, no-NO, pt-BR, ru-RU, sv-SE, th-TH, zh-CN, zh-HK, or zh-TW.

return_url	
string
The URL where the customer is redirected after the customer approves the payment.

cancel_url	
string
The URL where the customer is redirected after the customer cancels the payment.

consumer_user_agent	
string [ 1 .. 256 ] characters
The payer's User Agent. For example, Mozilla/5.0 (Macintosh; Intel Mac OS X x.y; rv:42.0).

consumer_ip	
string [ 7 .. 39 ] characters ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[...Show pattern
The IP address of the consumer. It could be either IPv4 or IPv6.

Copy
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
Information used to pay using BLIK level_0 flow.

auth_code
required
string = 6 characters
The 6-digit code used to authenticate a consumer within BLIK.

Copy
{
"auth_code": "string"
}
blik_one_click
Information used to pay using BLIK one-click flow.

auth_code	
string = 6 characters
The 6-digit code used to authenticate a consumer within BLIK.

consumer_reference
required
string [ 3 .. 64 ] characters
The merchant generated, unique reference serving as a primary identifier for accounts connected between Blik and a merchant.

alias_label	
string [ 8 .. 35 ] characters
A bank defined identifier used as a display name to allow the payer to differentiate between multiple registered bank accounts.

alias_key	
string [ 1 .. 19 ] characters
A Blik-defined identifier for a specific Blik-enabled bank account that is associated with a given merchant. Used only in conjunction with a Consumer Reference.

Copy
{
"auth_code": "string",
"consumer_reference": "string",
"alias_label": "stringst",
"alias_key": "string"
}
blik_request
Information needed to pay using BLIK.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

email	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the account holder associated with this payment method.

experience_context	
object
Customizes the payer experience during the approval process for the payment.

level_0	
object
The level_0 integration flow object.

one_click	
object
The one-click integration flow object.

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
Capture
The capture transaction details.

id	
string
The ID of the capture transaction.

is_final_capture	
boolean
Default: false
Indicates whether to release all remaining held funds.

state	
string
The state of the capture.

Enum Value	Description
pending	The capture is pending.
completed	The capture has successfully completed.
refunded	The capture was fully refunded.
partially_refunded	The capture was partially refunded.
denied	PayPal has declined to process this transaction.
reason_code	
string
The reason code that describes why the transaction state is pending or reversed.

Enum Value	Description
CHARGEBACK	The transaction state is pending or reversed due to a chargeback.
GUARANTEE	The transaction state is pending or reversed due to a guarantee.
BUYER_COMPLAINT	The transaction state is pending or reversed due to a customer complaint.
REFUND	The transaction state is pending or reversed due to a REFUND.
UNCONFIRMED_SHIPPING_ADDRESS	The transaction state is pending or reversed due to an unconfirmed shipping address.
ECHECK	The transaction state is pending or reversed due to an e-check.
INTERNATIONAL_WITHDRAWAL	The transaction state is pending or reversed due to an international withdrawal.
RECEIVING_PREFERENCE_MANDATES_MANUAL_ACTION	The transaction state is pending or reversed due to a receiving preference that mandates manual action.
PAYMENT_REVIEW	The transaction state is pending or reversed due to a payment review.
REGULATORY_REVIEW	The transaction state is pending or reversed due to a regulatory review.
UNILATERAL	The transaction state is pending or reversed due to a unilateral action.
VERIFICATION_REQUIRED	The transaction state is pending or reversed because verification is required.
TRANSACTION_APPROVED_AWAITING_FUNDING	The transaction state is pending or reversed because an approved transaction is awaiting funding.
NONE	Returned only when the state is 'completed'. Returned only as part of the 'POST' response.
parent_payment	
string
The ID of the payment on which this transaction is based.

invoice_number	
string <= 127 characters
The invoice number to track this payment.

exchange_rate	
string
The exchange rate applied for this transaction. Returned when there is a currency conversion from the transaction currency to the receivable currency.

note_to_payer	
string <= 255 characters
A free-form field that clients can use to send a note to the payer.

create_time	
string
The date and time of the capture, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount	
object
The amount to capture. If the amount matches the originally authorized amount, the state of the authorization changes to captured. Otherwise, the state changes to partially_captured.

transaction_fee	
object
The currency and amount of the transaction fee for this payment. Would not be returned for pending transactions where the funds have not been realized in the payee account.

transaction_fee_in_receivable_currency	
object
The currency and amount of the PayPal fee for this payment in the receivable currency. Returned only in cases the fee is charged in the receivable currency. Example CNY. Would not be returned for pending transactions where the funds have not been realized in the payee account.

receivable_amount	
object
The net amount and currency that the merchant receives for this transaction in the receivable currency.

CopyExpand allCollapse all
{
"id": "string",
"is_final_capture": false,
"state": "pending",
"reason_code": "CHARGEBACK",
"parent_payment": "string",
"invoice_number": "string",
"exchange_rate": "string",
"note_to_payer": "string",
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
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
},
"receivable_amount": {
"currency": "string",
"value": "string"
}
}
card
The payment card to use to fund a payment. Can be a credit or debit card.

name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

number	
string [ 13 .. 19 ] characters
The primary account number (PAN) for the payment card.

security_code	
string [ 3 .. 4 ] characters
The three- or four-digit security code of the card. Also known as the CVV, CVC, CVN, CVE, or CID. This parameter cannot be present in the request when payment_initiator=MERCHANT.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

attributes	
object
Additional attributes associated with the use of this card.

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
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
}
}
card
The payment card to use to fund a payment. Can be a credit or debit card.

name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

number	
string [ 13 .. 19 ] characters
The primary account number (PAN) for the payment card.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

card_type	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastecard card.
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
brand	
string [ 1 .. 255 ] characters ^[A-Z_]+$
The card brand or network. Typically used in the response.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastecard card.
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
UNKNOWN	UNKNOWN payment network.
billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

CopyExpand allCollapse all
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
Card Verification
The API caller can opt in to verify the card through PayPal offered verification services (e.g. Smart Dollar Auth, 3DS).

method	
string [ 1 .. 255 ] characters
Default: "SCA_WHEN_REQUIRED"
The method used for card verification.

Enum Value	Description
SCA_ALWAYS	Selecting this option will attempt to force a strong customer authentication for the authorization/transaction. In countries where SCA has been defined and implemented it will result in a contingency and HATEOAS link being returned. The API caller should redirect the payer to that link so that they can authenticate themselves against their issuing bank or other entity. As noted, the HATEOAS link is only available in all regions where strong authentication is supported, (e.g. in European countries where 3DS is live). Merchants can use this setting as an additional layer of security if they choose to. In all cases, when an authorization is requested the AVS/CVV results will be returned in the response.
SCA_WHEN_REQUIRED	This is the default. When an authorization or transaction is attempted this option will return a contingency and HATEOAS link only when local regulations require strong customer authentication, (e.g. 3DS in countries and use cases where it is mandated). The API caller should redirect the payer to the link so that they can authenticate themselves. In all cases, when an authorization is requested the AVS/CVV results will be returned in the response.
3D_SECURE	The contingency surfaced as an additional security layer that helps prevent unauthorized card-not-present transactions and protects the merchant from exposure to fraud.
AVS_CVV	Places a temporary hold on the card to ensure its validity. This process protects the merchant from exposure to fraud. This verification method will confirm that the address information or CVV included matches what the issuing bank has on file for the associated card, ensuring that only authorized card users are able to make purchases from you.
Copy
{
"method": "SCA_ALWAYS"
}
card_attributes
Additional attributes associated with the use of this card.

customer	
object
The details about a customer in PayPal's system of record.

vault	
object
Instruction to vault the card based on the specified strategy.

verification	
object
Instruction to optionally verify the card based on the specified strategy.

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
}
},
"vault": {
"store_in_vault": "ON_SUCCESS"
},
"verification": {
"method": "SCA_ALWAYS"
}
}
card_attributes
Additional attributes associated with the use of this card.

verification	
object
Instruction to optionally verify the card based on the specified strategy.

CopyExpand allCollapse all
{
"verification": {
"method": "SCA_ALWAYS"
}
}
card_brand
The card network or brand. Applies to credit, debit, gift, and payment cards.

string [ 1 .. 255 ] characters ^[A-Z_]+$
The card network or brand. Applies to credit, debit, gift, and payment cards.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastecard card.
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
UNKNOWN	UNKNOWN payment network.
Copy
"VISA"
card_experience_context
Customizes the payer experience during the 3DS Approval for payment.

return_url	
string [ 10 .. 4000 ] characters
The URL where the customer will be redirected upon successfully completing the 3DS challenge.

cancel_url	
string [ 10 .. 4000 ] characters
The URL where the customer will be redirected upon cancelling the 3DS challenge.

Copy
{
"return_url": "string",
"cancel_url": "string"
}
card_request
The payment card to use to fund a payment. Can be a credit or debit card.

Note: Passing card number, cvv and expiry directly via the API requires PCI SAQ D compliance.
PayPal offers a mechanism by which you do not have to take on the PCI SAQ D burden by using hosted fields - refer to this Integration Guide.
name	
string [ 1 .. 300 ] characters
The card holder's name as it appears on the card.

number	
string [ 13 .. 19 ] characters
The primary account number (PAN) for the payment card.

security_code	
string [ 3 .. 4 ] characters
The three- or four-digit security code of the card. Also known as the CVV, CVC, CVN, CVE, or CID. This parameter cannot be present in the request when payment_initiator=MERCHANT.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

billing_address	
object
The billing address for this card. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

attributes	
object
Additional attributes associated with the use of this card.

stored_credential	
object
Provides additional details to process a payment using a card that has been stored or is intended to be stored (also referred to as stored_credential or card-on-file).
Parameter compatibility:

payment_type=ONE_TIME is compatible only with payment_initiator=CUSTOMER.
usage=FIRST is compatible only with payment_initiator=CUSTOMER.
previous_transaction_reference or previous_network_transaction_reference is compatible only with payment_initiator=MERCHANT.
Only one of the parameters - previous_transaction_reference and previous_network_transaction_reference - can be present in the request.
vault_id	
string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated ID for the saved card payment source. Typically stored on the merchant's server.

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
}
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
Cart Base
The cart.

reference_id	
string <= 256 characters
The merchant-provided ID for the purchase unit.

payee	
object
The payee who receives the funds and fulfills the order.

description	
string <= 127 characters
The purchase description.

note_to_payee	
string <= 255 characters
The note to the recipient of the funds in this transaction.

custom	
string <= 127 characters
The free-form field for the client's use.

invoice_number	
string <= 127 characters
The invoice number to track this payment.

soft_descriptor	
string <= 22 characters
The payment descriptor on account transactions on the customer's credit card statement, that PayPal sends to processors. The maximum length of the soft descriptor information that you can pass in the API field is 22 characters, in the following format:

22 - len(PAYPAL * (8)) - len(Descriptor in Payment Receiving Preferences of Merchant account + 1)
The PAYPAL prefix uses 8 characters.

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
payment_options	
object
The payment options for this transaction.

item_list	
object
An array of items that are being purchased.

notify_url	
string <= 2048 characters
The send payment notifications URL.

order_url	
string <= 2048 characters
The payment-related URL on the merchant site.

amount
required
object
The payment amount, with details.

CopyExpand allCollapse all
{
"reference_id": "string",
"payee": {
"email": "user@example.com",
"merchant_id": "string"
},
"description": "string",
"note_to_payee": "string",
"custom": "string",
"invoice_number": "string",
"soft_descriptor": "string",
"payment_options": {
"allowed_payment_method": "UNRESTRICTED"
},
"item_list": {
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "str",
"tax": "string"
}
],
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
},
"shipping_phone_number": "string"
},
"notify_url": "http://example.com",
"order_url": "http://example.com",
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
Credit Card Token
The tokenized credit card details. You can use this instrument to fund a payment.

credit_card_id
required
string [ 1 .. 256 ] characters
The ID of credit card that is stored in the PayPal vault.

payer_id	
string [ 256 .. 1 ] characters
Deprecated. A unique ID that you can assign and track when you store a credit card in the vault or use a vaulted credit card. This ID can help to avoid unintentional use or misuse of credit cards and can be any value, such as a UUID, user name, or email address. Required when you use a vaulted credit card and if a payer_id was originally provided when you vaulted the credit card. Use external_customer_id instead.

external_customer_id	
string [ 1 .. 256 ] characters
The externally-provided ID of the customer.

last4	
string = 4 characters
The last four digits of the stored credit card number.

type	
string [ 1 .. 256 ] characters
The credit card type. Value is visa, mastercard, discover, or amex. Do not use these lowercase values for display.

expire_month	
integer [ 1 .. 12 ]
The expiration month with no leading zero. Value is from 1 to 12.

expire_year	
string = 4 characters
The four-digit expiration year.

Copy
{
"credit_card_id": "string",
"payer_id": "s",
"external_customer_id": "string",
"last4": "stri",
"type": "string",
"expire_month": 1,
"expire_year": "stri"
}
Currency
The currency and amount for a transaction.

currency
required
string
The three-character ISO-4217 currency code. PayPal does not support all currencies.

value
required
string
The amount. Includes the specified number of digits after decimal separator for the ISO-4217 currency code.

Copy
{
"currency": "string",
"value": "string"
}
currency_code
The three-character ISO-4217 currency code that identifies the currency.

string (currency_code) = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

Copy
"str"
currency_code
The three-character ISO-4217 currency code that identifies the currency.

string (currency_code) = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

Copy
"str"
customer
The details about a customer in PayPal's system of record.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
Email address of the buyer as provided to the merchant or on file with the merchant. Email Address is required if you are processing the transaction using PayPal Guest Processing which is offered to select partners and merchants. For all other use cases we do not expect partners/merchant to send email_address of their customer.

phone	
object
The phone number of the buyer as provided to the merchant or on file with the merchant. The phone.phone_number supports only national_number.

CopyExpand allCollapse all
{
"id": "string",
"email_address": "string",
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
}
}
customer
The details about a customer in PayPal's system of record.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
Email address of the buyer as provided to the merchant or on file with the merchant. Email Address is required if you are processing the transaction using PayPal Guest Processing which is offered to select partners and merchants. For all other use cases we do not expect partners/merchant to send email_address of their customer.

Copy
{
"id": "string",
"email_address": "string"
}
date_no_time
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

string (date_no_time) = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The stand-alone date, in Internet date and time format. To represent special legal values, such as a date of birth, you should use dates with no associated time or time-zone data. Whenever possible, use the standard date_time type. This regular expression does not validate all dates. For example, February 31 is valid and nothing is known about leap years.

Copy
"stringstri"
date_year_month
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The year and month, in ISO-8601 YYYY-MM date format. See Internet date and time format.

Copy
"strings"
Detailed Refund
The refund transaction details.

id	
string
The ID of the refund transaction. Maximum length is 17 characters.

state	
string
The state of the refund.

Enum Value	Description
pending	The refund state is pending.
completed	The refund state is completed.
cancelled	The refund state is cancelled.
failed	The refund state is failed.
reason	
string
The reason that the transaction is being refunded.

invoice_number	
string <= 127 characters
The invoice or tracking ID number.

sale_id	
string
The ID of the sale transaction being refunded.

capture_id	
string
The ID of the sale transaction being refunded.

parent_payment	
string
The ID of the payment on which this transaction is based.

description	
string
The refund description. Value must be single-byte alphanumeric characters.

create_time	
string
The date and time when the refund was created, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount	
object
The payment amount, with details.

custom	
string <= 127 characters
The note to the payer in this transaction.

refund_from_transaction_fee	
object
The currency and amount of the transaction fee that is refunded to original the recipient of payment.

refund_from_received_amount	
object
The currency and amount of the refund that is subtracted from the original payment recipient's PayPal balance.

total_refunded_amount	
object
The currency and amount of the total refund from the original purchase. For example, if a payer makes $100 purchase and the payer was refunded $20 a week ago and $30 in this transaction, the gross refund amount is $30 for this transaction and the total refunded amount is $50.

CopyExpand allCollapse all
{
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
"custom": "string",
"refund_from_transaction_fee": {
"currency": "string",
"value": "string"
},
"refund_from_received_amount": {
"currency": "string",
"value": "string"
},
"total_refunded_amount": {
"currency": "string",
"value": "string"
}
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

information_link	
string
The information link, or URI, that shows detailed information about this error for the developer.

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

description	
string
The human-readable description for an issue. The description can change over the lifetime of an API, so clients must not depend on this value.

Copy
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"description": "string"
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

description	
string
The human-readable description for an issue. The description can change over the lifetime of an API, so clients must not depend on this value.

Copy
{
"field": "string",
"value": "string",
"location": "body",
"issue": "string",
"description": "string"
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
SET_PROVIDED_ADDRESS	Get the merchant-provided address. The customer cannot change this address on the PayPal site. If merchant does not pass an address, customer can choose the address on PayPal pages.
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
FMF Details
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

filter_type
required
string
The filter type.

Enum Value	Description
ACCEPT	The accept filter type.
PENDING	The pending filter type.
DENY	The deny filter type.
REPORT	The report filter type.
filter_id
required
string
The filter ID.

Enum Value	Description
AVS_NO_MATCH	AVS no match.
AVS_PARTIAL_MATCH	AVS partial match.
AVS_UNAVAILABLE_OR_UNSUPPORTED	AVS unavailable or unsupported.
CARD_SECURITY_CODE_MISMATCH	Card security code mismatch.
MAXIMUM_TRANSACTION_AMOUNT	The maximum transaction amount.
UNCONFIRMED_ADDRESS	Unconfirmed address.
COUNTRY_MONITOR	Country monitor.
LARGE_ORDER_NUMBER	Large order number.
BILLING_OR_SHIPPING_ADDRESS_MISMATCH	Billing or shipping address mismatch.
RISKY_ZIP_CODE	Risky zip code.
SUSPECTED_FREIGHT_FORWARDER_CHECK	Suspected freight forwarder check.
TOTAL_PURCHASE_PRICE_MINIMUM	Total purchase price minimum.
IP_ADDRESS_VELOCITY	IP address velocity.
RISKY_EMAIL_ADDRESS_DOMAIN_CHECK	Risky email address domain check.
RISKY_BANK_IDENTIFICATION_NUMBER_CHECK	Risky bank identification number check.
RISKY_IP_ADDRESS_RANGE	Risky IP address range.
PAYPAL_FRAUD_MODEL	PayPal fraud model.
name	
string
The filter name.

description	
string
The filter description.

Copy
{
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
}
Funding Instrument
The funding instrument details. An instance of this schema is valid if and only if it validates against exactly one of these supported properties.

credit_card_token	
object
The tokenized credit card details. You can use this instrument to fund a payment.

CopyExpand allCollapse all
{
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "s",
"external_customer_id": "string",
"last4": "stri",
"type": "string",
"expire_month": 1,
"expire_year": "stri"
}
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
google_pay_card
any (google_pay_card)
Copy
null
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

Copy
{
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
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
any
The payment card information.

decrypted_token	
object
The decrypted payload details for the Google Pay token.

attributes	
object
Additional attributes associated with the use of this card.

CopyExpand allCollapse all
{
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": null,
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
},
"attributes": {
"verification": {
"method": "SCA_ALWAYS"
}
}
}
ideal_request
Information needed to pay using iDEAL.

name
required
string [ 3 .. 300 ] characters
The name of the account holder associated with this payment method.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO 3166-1 country code.

bic	
string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The bank identification code (BIC).

experience_context	
object
Customizes the payer experience during the approval process for the payment.

CopyExpand allCollapse all
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
Installment Option
The installment option details.

term
required
integer
The number of installments.

monthly_payment
required
object
The currency and amount for a transaction.

discount_amount	
object
The currency and amount for a transaction.

discount_percentage	
string^((-?[0-9]+)|(-?([0-9]+)?[.][0-9]+))$
The percentage as a fixed-point, signed decimal value. Use for all interest rates. For example, specify an interest rate of 19.99% as 19.99. The allowed number formats are plain decimal numbers and whole numbers.

CopyExpand allCollapse all
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
Instrument Authorization Context
Instrument related authorization context.

predicted_approval_score	
string [ 1 .. 255 ] characters
This enum represents category of predicted approval score.

Enum Value	Description
LOW	Predicted approval score is low.
MEDIUM	Predicted approval score is medium.
HIGH	Predicted approval score is high.
Copy
{
"predicted_approval_score": "LOW"
}
Instrument context
Provides context about the funding instrument used.

authorization_context	
object
Instrument related authorization context.

CopyExpand allCollapse all
{
"authorization_context": {
"predicted_approval_score": "LOW"
}
}
instrument_id
The identifier of the instrument.

string [ 1 .. 256 ] characters ^[A-Za-z0-9-_.+=]+$
The identifier of the instrument.

Copy
"string"
IP Address
An Internet Protocol address (IP address). This address assigns a numerical label to each device that is connected to a computer network through the Internet Protocol. Supports IPv4 and IPv6 addresses.

string (IP Address) [ 7 .. 39 ] characters ^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[...Show pattern
An Internet Protocol address (IP address). This address assigns a numerical label to each device that is connected to a computer network through the Internet Protocol. Supports IPv4 and IPv6 addresses.

Copy
"strings"
Item
The item details.

sku	
string <= 127 characters
The stock keeping unit (SKU) for the item.

name	
string <= 127 characters
The item name. If this value is greater than the maximum allowed length, the API truncates the string.

description	
string <= 127 characters
The item description. Supported for only the PayPal payment method.

quantity
required
string <= 10 characters
The item quantity. Must be a whole number.

price
required
string <= 10 characters
The item cost. Supports two decimal places.

currency
required
string = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

tax	
string
The item tax. Supported only for the PayPal payment method.

Copy
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "str",
"tax": "string"
}
language
The language tag for the language in which to localize the error-related strings, such as messages, issues, and suggested actions. The tag is made up of the ISO 639-2 language code, the optional ISO-15924 script tag, and the ISO-3166 alpha-2 country code or M49 region code.

string (language) [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The language tag for the language in which to localize the error-related strings, such as messages, issues, and suggested actions. The tag is made up of the ISO 639-2 language code, the optional ISO-15924 script tag, and the ISO-3166 alpha-2 country code or M49 region code.

Copy
"string"
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

Enum: "GET" "POST" "PUT" "DELETE" "HEAD" "CONNECT" "OPTIONS" "PATCH"
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

Enum: "GET" "POST" "PUT" "DELETE" "HEAD" "CONNECT" "OPTIONS" "PATCH"
Copy
{
"href": "string",
"rel": "string",
"method": "GET"
}
merchant_partner_customer_id
The unique ID for a customer generated by PayPal.

string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

Copy
"string"
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

full_name	
string <= 300 characters
When the party is a person, the party's full name.

Copy
{
"full_name": "string"
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
network_transaction_reference
Reference values used by the card network to identify a transaction.

id
required
string [ 9 .. 36 ] characters
Transaction reference id returned by the scheme. For Visa and Amex, this is the "Tran id" field in response. For MasterCard, this is the "BankNet reference id" field in response. For Discover, this is the "NRID" field in response. The pattern we expect for this field from Visa/Amex/CB/Discover is numeric, Mastercard/BNPP is alphanumeric and Paysecure is alphanumeric with special character -.

date	
string = 4 characters
The date that the transaction was authorized by the scheme. This field may not be returned for all networks. MasterCard refers to this field as "BankNet reference date.

acquirer_reference_number	
string [ 1 .. 36 ] characters
Reference ID issued for the card transaction. This ID can be used to track the transaction across processors, card brands and issuing banks.

network	
string [ 1 .. 255 ] characters ^[A-Z_]+$
Name of the card network through which the transaction was routed.

Enum Value	Description
VISA	Visa card.
MASTERCARD	Mastecard card.
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
UNKNOWN	UNKNOWN payment network.
Copy
{
"id": "stringstr",
"date": "stri",
"acquirer_reference_number": "string",
"network": "VISA"
}
Order
The order transaction details.

id	
string
The ID of the order transaction.

payment_mode	
string
The transaction payment mode.

Enum Value	Description
INSTANT_TRANSFER	The payment mode is instant transfer.
MANUAL_BANK_TRANSFER	The payment mode is manual bank transfer.
DELAYED_TRANSFER	The payment mode is delayed transfer.
ECHECK	The payment mode is eCheck.
state	
string
The state of the order transaction.

Enum Value	Description
PENDING	The order was created but no authorizations or captures were made against the order.
AUTHORIZED	The order has only been authorized. No capture was made against the order.
CAPTURED	The order has at least one capture initiated.
COMPLETED	The order is complete. A capture was made against the order with is_final_capture set to TRUE. No more authorizations or captures can be made against this order.
VOIDED	The order was voided. No more authorizations or captures can be made against this order.
reason_code	
string
The reason code that describes why the transaction state is pending or reversed. Supported only for PayPal payments.

Enum Value	Description
PAYER_SHIPPING_UNCONFIRMED	The shipping address of the payer is not confirmed.
MULTI_CURRENCY	The transaction is in multiple currencies.
RISK_REVIEW	The transaction is under risk review.
REGULATORY_REVIEW	The transaction is under regulatory review.
VERIFICATION_REQUIRED	Verification is required.
ORDER	The transaction is an order.
OTHER	Other.
pending_reason	
string
Deprecated. The reason code for the pending transaction state. Obsolete. Use reason_code instead.

Enum: "payer_shipping_unconfirmed" "multi_currency" "risk_review" "regulatory_review" "verification_required" "order" "other"
protection_eligibility	
string
The level of seller protection in effect for the transaction.

Enum Value	Description
ELIGIBLE	The transaction is eligible for seller protection.
PARTIALLY_ELIGIBLE	The transaction is partially eligible for seller protection.
INELIGIBLE	The transaction is ineligible for seller protection.
protection_eligibility_type	
string
The kind of seller protection in effect for the transaction. Returned only when the protection_eligibility property is ELIGIBLE or PARTIALLY_ELIGIBLE. Supported only for PayPal payments.

Enum Value	Description
ITEM_NOT_RECEIVED_ELIGIBLE	Sellers are protected against claims for items not received.
UNAUTHORIZED_PAYMENT_ELIGIBLE	Sellers are protected against claims for unauthorized payments.
ITEM_NOT_RECEIVED_ELIGIBLE,UNAUTHORIZED_PAYMENT_ELIGIBLE	Sellers are protected against claims for items not received and claims for unauthorized payments.
parent_payment	
string
The ID of the payment on which this transaction is based.

fmf_details	
object
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

create_time	
string
The date and time when the resource was created, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount
required
object
The amount to collect.

Note: For an order authorization, you cannot include amount details.
CopyExpand allCollapse all
{
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "PENDING",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"pending_reason": "payer_shipping_unconfirmed",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"parent_payment": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
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
object
The value to apply. The remove operation does not require a value.

from	
string
The JSON Pointer to the target document location from which to move the value. Required for the move operation.

CopyExpand allCollapse all
{
"op": "add",
"path": "string",
"value": { },
"from": "string"
}
Patch Request
An array of JSON patch objects to apply partial updates to resources.

Array 
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
object
The value to apply. The remove operation does not require a value.

from	
string
The JSON Pointer to the target document location from which to move the value. Required for the move operation.

CopyExpand allCollapse all
[
{
"op": "add",
"path": "string",
"value": { },
"from": "string"
}
]
Payee
The payee who receives the funds and fulfills the order.

email	
string
The email address associated with the payee's PayPal account. For an intent of authorize or order, the email address must be associated with a confirmed PayPal business account. For an intent of sale, the email can either:

Be associated with a confirmed PayPal personal or business account.
Not be associated with a PayPal account.
merchant_id	
string
The PayPal account ID for the payee.

Copy
{
"email": "user@example.com",
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
"merchant_id": "stringstrings"
}
Payer
The payer. The payer funds the payment.

payment_method	
string [ 4 .. 17 ] characters
The payment method.

Value	Description
paypal	A PayPal Wallet payment.
status	
string [ 8 .. 10 ] characters
The status of payer's PayPal account.

Enum Value	Description
VERIFIED	
UNVERIFIED	
funding_instruments	
Array of objects = 1 items
An array of a single funding instrument for the current payment. Valid only and required for the credit card payment method. The array must include either a credit_card or credit_card_token object. If the array contains more than one instrument, the payment is declined.

payer_info	
object
The payer information.

CopyExpand allCollapse all
{
"payment_method": "paypal",
"status": "VERIFIED",
"funding_instruments": [
{
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "s",
"external_customer_id": "string",
"last4": "stri",
"type": "string",
"expire_month": 1,
"expire_year": "stri"
}
}
],
"payer_info": {
"email": "user@example.com",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
}
}
}
Payer Information
The payer information.

email	
string
The payer's email address. Maximum length is 127 characters.

salutation	
string
The payer's salutation.

first_name	
string
The payer's first name.

middle_name	
string
The payer's middle name.

last_name	
string
The payer's last name.

suffix	
string
The payer's suffix.

payer_id	
string
The PayPal-assigned encrypted payer ID.

birth_date	
string
The birth date of the payer, in Internet date format. For example, 1990-04-12.

tax_id	
string <= 14 characters
The payer's tax ID. Supported for the PayPal payment method only.

tax_id_type	
string
The payer's tax ID type. Supported for the PayPal payment method only.

Enum Value	Description
BR_CPF	BR CPF tax ID type.
BR_CNPJ	BR CNPJ tax ID type.
billing_address	
object
The billing address or shipping address for a payment.

shipping_address	
object
The shipping address details.

CopyExpand allCollapse all
{
"email": "user@example.com",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
}
}
Payment
The payment details.

id	
string
The ID of the payment.

intent
required
string
The payment intent. Value is:

sale. Makes an immediate payment.
authorize. Authorizes a payment for capture later.
order. Creates an order.
Enum: "sale" "authorize" "order"
transactions	
Array of objects
An array of payment-related transactions. A transaction defines what the payment is for and who fulfills the payment. For update and execute payment calls, the transactions object accepts the amount object only.

state	
string
The state of the payment, authorization, or order transaction. Value is:

created. The transaction was successfully created.
approved. The customer approved the transaction. The state changes from created to approved on generation of the sale_id for sale transactions, authorization_id for authorization transactions, or order_id for order transactions.
failed. The transaction request failed.
Enum: "created" "approved" "failed"
experience_profile_id	
string
Deprecated. The PayPal-generated ID for the merchant's payment experience profile. For information, see create web experience profile. Use application_context instead.

note_to_payer	
string <= 165 characters
A free-form field that clients can use to send a note to the payer.

redirect_urls	
object
A set of redirect URLs that you provide for PayPal-based payments.

failure_reason	
string
The reason code for a payment failure.

Enum: "UNABLE_TO_COMPLETE_TRANSACTION" "INVALID_PAYMENT_METHOD" "PAYER_CANNOT_PAY" "CANNOT_PAY_THIS_PAYEE" "REDIRECT_REQUIRED" "PAYEE_FILTER_RESTRICTIONS"
create_time	
string
The date and time when the payment was created, in Internet date and time format.

update_time	
string
The date and time when the payment was updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

payer
required
object
The source of the funds for this payment. Payment method is PayPal Wallet payment or bank direct debit.

application_context	
object
Use the application context resource to customize payment flow experience for your buyers.

CopyExpand allCollapse all
{
"id": "string",
"intent": "sale",
"transactions": [
{
"payee": {
"email": "user@example.com",
"merchant_id": "string"
},
"description": "string",
"note_to_payee": "string",
"custom": "string",
"invoice_number": "string",
"soft_descriptor": "string",
"payment_options": {
"allowed_payment_method": "UNRESTRICTED"
},
"item_list": {
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "str",
"tax": "string"
}
],
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
},
"shipping_method": "string",
"shipping_phone_number": "string"
},
"notify_url": "http://example.com",
"related_resources": [
{
"sale": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "completed",
"reason_code": "CHARGEBACK",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"clearing_time": "2019-08-24T14:15:22Z",
"payment_hold_status": "HELD",
"payment_hold_reasons": [
{
"payment_hold_reason": null
}
],
"exchange_rate": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"receipt_id": "string",
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"billing_agreement_id": "string",
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
},
"receivable_amount": {
"currency": "string",
"value": "string"
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
}
},
"authorization": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "authorized",
"reason_code": "AUTHORIZATION",
"pending_reason": "AUTHORIZATION",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"valid_until": "2019-08-24T14:15:22Z",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"receipt_id": "string",
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
},
"order": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "PENDING",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"pending_reason": "payer_shipping_unconfirmed",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"parent_payment": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
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
}
},
"capture": {
"id": "string",
"is_final_capture": false,
"state": "pending",
"reason_code": "CHARGEBACK",
"parent_payment": "string",
"invoice_number": "string",
"exchange_rate": "string",
"note_to_payer": "string",
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
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
},
"receivable_amount": {
"currency": "string",
"value": "string"
}
},
"refund": {
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
}
}
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
"state": "created",
"experience_profile_id": "string",
"note_to_payer": "string",
"redirect_urls": {
"return_url": "http://example.com",
"cancel_url": "http://example.com"
},
"failure_reason": "UNABLE_TO_COMPLETE_TRANSACTION",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"payer": {
"payment_method": "paypal",
"status": "VERIFIED",
"funding_instruments": [
{
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "s",
"external_customer_id": "string",
"last4": "stri",
"type": "string",
"expire_month": 1,
"expire_year": "stri"
}
}
],
"payer_info": {
"email": "user@example.com",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
}
}
},
"application_context": {
"brand_name": "string",
"locale": "string",
"landing_page": "string",
"shipping_preference": "NO_SHIPPING",
"user_action": "string",
"payment_pattern": "CUSTOMER_PRESENT_ONETIME_PURCHASE",
"preferred_payment_source": {
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
},
"billing_agreement_id": "string",
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
"national_number": null
}
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
"vault_id": "string"
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": null,
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
},
"attributes": {
"verification": {
"method": "SCA_ALWAYS"
}
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string"
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
}
Payment Execution
Executes a PayPal account-based payment with the payer_id obtained from the web approval URL.

payer_id	
string
The ID of the payer that PayPal passes in the return_url.

transactions	
Array of objects
An array of transaction details. Includes the amount and item details. For update and execute payment calls, the transactions object accepts only the amount object.

CopyExpand allCollapse all
{
"payer_id": "string",
"transactions": [
{
"reference_id": "string",
"payee": {
"email": "user@example.com",
"merchant_id": "string"
},
"description": "string",
"note_to_payee": "string",
"custom": "string",
"invoice_number": "string",
"soft_descriptor": "string",
"payment_options": {
"allowed_payment_method": "UNRESTRICTED"
},
"item_list": {
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "str",
"tax": "string"
}
],
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
},
"shipping_phone_number": "string"
},
"notify_url": "http://example.com",
"order_url": "http://example.com",
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
]
}
Payment History
An array of merchant payments that are complete. Payments that you just created with the create payment call do not appear in the list.

payments	
Array of objects
An array of payments that are complete. Payments that you just created with the create payment call do not appear in the list.

count	
integer <= 20
The number of items returned in each range of results. Note that the last results range might have fewer items than the requested number of items.

next_id	
string
The ID of the element to use to get the next range of results.

CopyExpand allCollapse all
{
"payments": [
{
"id": "string",
"intent": "sale",
"transactions": [
{
"payee": {
"email": "user@example.com",
"merchant_id": "string"
},
"description": "string",
"note_to_payee": "string",
"custom": "string",
"invoice_number": "string",
"soft_descriptor": "string",
"payment_options": {
"allowed_payment_method": "UNRESTRICTED"
},
"item_list": {
"items": [
{
"sku": null,
"name": null,
"description": null,
"quantity": null,
"price": null,
"currency": null,
"tax": null
}
],
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
},
"shipping_method": "string",
"shipping_phone_number": "string"
},
"notify_url": "http://example.com",
"related_resources": [
{
"sale": {
"id": null,
"payment_mode": null,
"state": null,
"reason_code": null,
"protection_eligibility": null,
"protection_eligibility_type": null,
"clearing_time": null,
"payment_hold_status": null,
"payment_hold_reasons": [ ],
"exchange_rate": null,
"fmf_details": null,
"receipt_id": null,
"parent_payment": null,
"processor_response": null,
"billing_agreement_id": null,
"create_time": null,
"update_time": null,
"links": [ ],
"amount": null,
"transaction_fee": null,
"receivable_amount": null,
"transaction_fee_in_receivable_currency": null
},
"authorization": {
"id": null,
"payment_mode": null,
"state": null,
"reason_code": null,
"pending_reason": null,
"protection_eligibility": null,
"protection_eligibility_type": null,
"fmf_details": null,
"parent_payment": null,
"processor_response": null,
"valid_until": null,
"create_time": null,
"update_time": null,
"receipt_id": null,
"links": [ ],
"amount": null
},
"order": {
"id": null,
"payment_mode": null,
"state": null,
"reason_code": null,
"pending_reason": null,
"protection_eligibility": null,
"protection_eligibility_type": null,
"parent_payment": null,
"fmf_details": null,
"create_time": null,
"update_time": null,
"links": [ ],
"amount": null
},
"capture": {
"id": null,
"is_final_capture": null,
"state": null,
"reason_code": null,
"parent_payment": null,
"invoice_number": null,
"exchange_rate": null,
"note_to_payer": null,
"create_time": null,
"update_time": null,
"links": [ ],
"amount": null,
"transaction_fee": null,
"transaction_fee_in_receivable_currency": null,
"receivable_amount": null
},
"refund": {
"id": null,
"state": null,
"reason": null,
"invoice_number": null,
"sale_id": null,
"capture_id": null,
"parent_payment": null,
"description": null,
"create_time": null,
"update_time": null,
"links": [ ],
"amount": null
}
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
"state": "created",
"experience_profile_id": "string",
"note_to_payer": "string",
"redirect_urls": {
"return_url": "http://example.com",
"cancel_url": "http://example.com"
},
"failure_reason": "UNABLE_TO_COMPLETE_TRANSACTION",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
],
"payer": {
"payment_method": "paypal",
"status": "VERIFIED",
"funding_instruments": [
{
"credit_card_token": {
"credit_card_id": "string",
"payer_id": "s",
"external_customer_id": "string",
"last4": "stri",
"type": "string",
"expire_month": 1,
"expire_year": "stri"
}
}
],
"payer_info": {
"email": "user@example.com",
"salutation": "string",
"first_name": "string",
"middle_name": "string",
"last_name": "string",
"suffix": "string",
"payer_id": "string",
"birth_date": "2019-08-24T14:15:22Z",
"tax_id": "string",
"tax_id_type": "BR_CPF",
"billing_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st"
},
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
}
}
},
"application_context": {
"brand_name": "string",
"locale": "string",
"landing_page": "string",
"shipping_preference": "NO_SHIPPING",
"user_action": "string",
"payment_pattern": "CUSTOMER_PRESENT_ONETIME_PURCHASE",
"preferred_payment_source": {
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
},
"billing_agreement_id": "string",
"vault_id": "string",
"email_address": "string",
"name": {
"given_name": "string",
"surname": "string"
},
"phone": {
"phone_type": "FAX",
"phone_number": {
"national_number": null
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
"id": null,
"email_address": null,
"merchant_customer_id": null
},
"vault": {
"store_in_vault": null,
"description": null,
"usage_pattern": null,
"usage_type": null,
"customer_type": null,
"permit_multiple_payment_tokens": null
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
"id": null,
"date": null,
"acquirer_reference_number": null,
"network": null
}
},
"attributes": {
"customer": {
"id": null,
"email_address": null,
"phone": null
},
"vault": {
"store_in_vault": null
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
"currency_code": null,
"value": null
},
"tokenized_card": {
"name": null,
"number": null,
"expiry": null,
"card_type": null,
"type": null,
"brand": null,
"billing_address": null
},
"payment_data": {
"cryptogram": null,
"eci_indicator": null,
"emv_data": null,
"pin": null
}
},
"vault_id": "string"
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": null,
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
},
"attributes": {
"verification": {
"method": null
}
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": null,
"email_address": null
},
"vault": {
"store_in_vault": null,
"description": null,
"usage_pattern": null,
"usage_type": null,
"customer_type": null,
"permit_multiple_payment_tokens": null
}
}
}
}
}
}
],
"count": 20,
"next_id": "string"
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
payment_pattern
Provides context (e.g. frequency of payment (Single, Recurring) along with whether (Customer is Present, Not Present) for the payment being processed. For Card and PayPal Vaulted/Billing Agreement transactions, this helps specify the appropriate indicators to the networks (e.g. Mastercard, Visa) which ensures compliance as well as ensure a better auth-rate. For bank processing, indicates to clearing house whether the transaction is recurring or not depending on the option chosen.

string [ 3 .. 255 ] characters ^[A-Z_]+$
Provides context (e.g. frequency of payment (Single, Recurring) along with whether (Customer is Present, Not Present) for the payment being processed. For Card and PayPal Vaulted/Billing Agreement transactions, this helps specify the appropriate indicators to the networks (e.g. Mastercard, Visa) which ensures compliance as well as ensure a better auth-rate. For bank processing, indicates to clearing house whether the transaction is recurring or not depending on the option chosen.

Enum Value	Description
CUSTOMER_PRESENT_ONETIME_PURCHASE	If the payments is an e-commerce payment initiated by the customer. Customer typically enters payment information (e.g. card number, approves payment within the PayPal Checkout flow) and such information that has not been previously stored on file.
CUSTOMER_NOT_PRESENT_RECURRING	Subsequent recurring payments (e.g. subscriptions with a fixed amount on a predefined schedule when customer is not present.
CUSTOMER_PRESENT_RECURRING_FIRST	The first payment initiated by the customer which is expected to be followed by a series of subsequent recurring payments transactions (e.g. subscriptions with a fixed amount on a predefined schedule when customer is not present. This is typically used for scenarios where customer stores credentials and makes a purchase on a given date and also set’s up a subscription.
CUSTOMER_PRESENT_ONETIME_PURCHASE_VAULTED	Also known as (card-on-file) transactions. Payment details are stored to enable checkout with one-click, or simply to streamline the checkout process. For card transaction customers typically do not enter a CVC number as part of the Checkout process.
CUSTOMER_NOT_PRESENT_ONETIME_PURCHASE_VAULTED	Unscheduled payments that are not recurring on a predefined schedule (e.g. balance top-up).
MAIL_ORDER_TELEPHONE_ORDER	Payments that are initiated by the customer via the merchant by mail or telephone.
Copy
"CUSTOMER_PRESENT_ONETIME_PURCHASE"
payment_source
The payment source definition.

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
"token": {
"id": "string",
"type": "BILLING_AGREEMENT"
},
"paypal": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
},
"billing_agreement_id": "string",
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
"vault_id": "string"
},
"google_pay": {
"name": "string",
"email_address": "string",
"phone_number": {
"country_code": "str",
"national_number": "string"
},
"card": null,
"decrypted_token": {
"message_id": "string",
"message_expiration": "stringstrings",
"payment_method": "CARD",
"authentication_method": "PAN_ONLY",
"cryptogram": "string",
"eci_indicator": "string"
},
"attributes": {
"verification": {
"method": "SCA_ALWAYS"
}
}
},
"venmo": {
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string"
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
PaymentsCommonComponentsSpecification-v1-schema-common_components-v3-schema-json-openapi-2.0-currency_code.json
The three-character ISO-4217 currency code that identifies the currency.

string (PaymentsCommonComponentsSpecification-v1-schema-common_components-v3-schema-json-openapi-2.0-currency_code.json) = 3 characters
The three-character ISO-4217 currency code that identifies the currency.

Copy
"str"
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
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
},
"billing_agreement_id": "string",
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
paypal_wallet_customer
The details about a customer in PayPal's system of record.

id	
string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
Email address of the buyer as provided to the merchant or on file with the merchant. Email Address is required if you are processing the transaction using PayPal Guest Processing which is offered to select partners and merchants. For all other use cases we do not expect partners/merchant to send email_address of their customer.

merchant_customer_id	
string [ 1 .. 64 ] characters
Merchants and partners may already have a data-store where their customer information is persisted. Use merchant_customer_id to associate the PayPal-generated customer.id to your representation of a customer.

Copy
{
"id": "string",
"email_address": "string",
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
landing_page	
string [ 1 .. 13 ] characters
Default: "NO_PREFERENCE"
The type of landing page to show on the PayPal site for customer checkout.

Enum Value	Description
LOGIN	When the customer clicks PayPal Checkout, the customer is redirected to a page to log in to PayPal and approve the payment.
GUEST_CHECKOUT	When the customer clicks PayPal Checkout, the customer is redirected to a page to enter credit or debit card and other relevant billing information required to complete the purchase. This option has previously been also called as 'BILLING'
NO_PREFERENCE	When the customer clicks PayPal Checkout, the customer is redirected to either a page to log in to PayPal and approve the payment or to a page to enter credit or debit card and other relevant billing information required to complete the purchase, depending on their previous interaction with PayPal.
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

Copy
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"landing_page": "LOGIN",
"user_action": "CONTINUE",
"payment_method_preference": "UNRESTRICTED",
"locale": "string",
"return_url": "string",
"cancel_url": "string"
}
Percentage
The percentage as a fixed-point, signed decimal value. Use for all interest rates. For example, specify an interest rate of 19.99% as 19.99. The allowed number formats are plain decimal numbers and whole numbers.

string (Percentage) ^((-?[0-9]+)|(-?([0-9]+)?[.][0-9]+))$
The percentage as a fixed-point, signed decimal value. Use for all interest rates. For example, specify an interest rate of 19.99% as 19.99. The allowed number formats are plain decimal numbers and whole numbers.

Copy
"string"
Phone
The phone number, in its canonical international E.164 numbering plan format.

national_number
required
string [ 1 .. 14 ] characters
The national number, in its canonical international E.164 numbering plan format. The combined length of the country calling code (CC) and the national number must not be greater than 15 digits. The national number consists of a national destination code (NDC) and subscriber number (SN).

Copy
{
"national_number": "string"
}
Phone
The phone number, in its canonical international E.164 numbering plan format.

national_number
required
string [ 1 .. 14 ] characters
The national number, in its canonical international E.164 numbering plan format. The combined length of the country calling code (CC) and the national number must not be greater than 15 digits. The national number consists of a national destination code (NDC) and subscriber number (SN).

Copy
{
"national_number": "string"
}
Phone
The phone number in its canonical international E.164 numbering plan format.

country_code	
string [ 1 .. 3 ] characters
The country calling code (CC), in its canonical international E.164 numbering plan format. The combined length of the CC and the national number must not be greater than 15 digits. The national number consists of a national destination code (NDC) and subscriber number (SN).

national_number
required
string [ 1 .. 14 ] characters
The national number, in its canonical international E.164 numbering plan format. The combined length of the country calling code (CC) and the national number must not be greater than 15 digits. The national number consists of a national destination code (NDC) and subscriber number (SN).

Copy
{
"country_code": "str",
"national_number": "string"
}
Phone Type
The phone type.

string
The phone type.

Enum: "FAX" "HOME" "MOBILE" "OTHER" "PAGER"
Copy
"FAX"
phone_with_type
The phone information.

phone_type	
string
The phone type.

Enum: "FAX" "HOME" "MOBILE" "OTHER" "PAGER"
phone_number
required
object
The phone number, in its canonical international E.164 numbering plan format. Supports only the national_number property.

CopyExpand allCollapse all
{
"phone_type": "FAX",
"phone_number": {
"national_number": "string"
}
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
Processor Response
The processor-provided response codes that describe the submitted payment. Supported only when the payment_method is credit_card.

response_code
required
string <= 4 characters
The PayPal normalized response code, which is generated from the processor's specific response code.

avs_code	
string <= 1 characters
The Address Verification System (AVS) response code.

cvv_code	
string <= 1 characters
The CVV system response code.

advice_code	
string
The merchant advice on how to handle declines for recurring payments.

Enum Value	Description
01_NEW_ACCOUNT_INFORMATION	01 New account information.
02_TRY_AGAIN_LATER	02 Try again later.
02_STOP_SPECIFIC_PAYMENT	02 Stop specific payment.
03_DO_NOT_TRY_AGAIN	03 Do not try again.
03_REVOKE_AUTHORIZATION_FOR_FUTURE_PAYMENT	03 Revoke authorization for future payment.
21_DO_NOT_TRY_AGAIN_CARD_HOLDER_CANCELLED_RECURRRING_CHARGE	21 Do not try again. Card holder cancelled recurring charge.
21_CANCEL_ALL_RECURRING_PAYMENTS	21 Cancel all recurring payments.
eci_submitted	
string
The processor-provided authorization response.

vpas	
string
The processor-provided Visa Payer Authentication Service (VPAS) status.

Copy
{
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
}
Refund
The refund details.

id	
string
The ID of the refund transaction. Maximum length is 17 characters.

state	
string
The state of the refund.

Enum Value	Description
pending	The refund state is pending.
completed	The refund state is completed.
cancelled	The refund state is cancelled.
failed	The refund state is failed.
reason	
string
The reason that the transaction is being refunded.

invoice_number	
string <= 127 characters
The invoice or tracking ID number.

sale_id	
string
The ID of the sale transaction being refunded.

capture_id	
string
The ID of the sale transaction being refunded.

parent_payment	
string
The ID of the payment on which this transaction is based.

description	
string
The refund description. Value must be single-byte alphanumeric characters.

create_time	
string
The date and time when the refund was created, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount	
object
The payment amount, with details.

CopyExpand allCollapse all
{
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
}
}
Refund Request
The refund request.

description	
string <= 255 characters
The refund description. Value is a string of single-byte alphanumeric characters.

reason	
string <= 30 characters
The refund reason description.

invoice_number	
string <= 127 characters
The invoice number that tracks this payment. Value is a string of single-byte alphanumeric characters.

amount	
object
The refund amount. Includes both the amount to refund to the payer and the fee amount to refund to the payee.

CopyExpand allCollapse all
{
"description": "string",
"reason": "string",
"invoice_number": "string",
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
refund_transaction_details
The refund transaction details.

id	
string <= 17 characters
The ID of the refund transaction.

state	
string
The state of the refund.

Enum: "pending" "completed" "cancelled" "failed"
reason	
string
The reason that the transaction is being refunded.

invoice_number	
string <= 127 characters
Your own invoice or tracking ID number. Value is a string of single-byte alphanumeric characters.

sale_id	
string
The ID of the sale transaction being refunded.

capture_id	
string
The ID of the sale transaction being refunded.

parent_payment	
string
The ID of the payment on which this transaction is based.

description	
string
The refund description.

create_time	
string
The date and time when the refund was created, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount	
object
The refund amount. Includes both the amount refunded to the payer and the fee refunded to the payee.

CopyExpand allCollapse all
{
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
}
}
Related Resources
The payment-related financial transactions, which include sales, authorizations, captures, and refunds. To show resource details, use the resource ID. For example, to show details for a related authorization, use the ID returned in the authorization object. You can also use the HATEOAS links for a resource to complete operations for that resource. For example, a sale object provides a refund link that enables you to refund the sale.

sale	
object
The sale transaction details.

authorization	
object
The authorization details.

order	
object
The order transaction details.

capture	
object
The capture transaction details.

refund	
object
The refund details.

CopyExpand allCollapse all
{
"sale": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "completed",
"reason_code": "CHARGEBACK",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"clearing_time": "2019-08-24T14:15:22Z",
"payment_hold_status": "HELD",
"payment_hold_reasons": [
{
"payment_hold_reason": "PAYMENT_HOLD"
}
],
"exchange_rate": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"receipt_id": "string",
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"billing_agreement_id": "string",
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
},
"receivable_amount": {
"currency": "string",
"value": "string"
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
}
},
"authorization": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "authorized",
"reason_code": "AUTHORIZATION",
"pending_reason": "AUTHORIZATION",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"valid_until": "2019-08-24T14:15:22Z",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"receipt_id": "string",
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
},
"order": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "PENDING",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"pending_reason": "payer_shipping_unconfirmed",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"parent_payment": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
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
},
"capture": {
"id": "string",
"is_final_capture": false,
"state": "pending",
"reason_code": "CHARGEBACK",
"parent_payment": "string",
"invoice_number": "string",
"exchange_rate": "string",
"note_to_payer": "string",
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
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
},
"receivable_amount": {
"currency": "string",
"value": "string"
}
},
"refund": {
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
}
}
}
Related Resources
The payment-related financial transactions, which include sales, authorizations, captures, and refunds. To show resource details, use the resource ID. For example, to show details for a related authorization, use the ID returned in the authorization object. You can also use the HATEOAS links for a resource to complete operations for that resource. For example, a sale object provides a refund link that enables you to refund the sale.

payment_hold_reason	
string
The reason that PayPal holds the recipient fund. Set only if the payment hold status is HELD.

Enum: "PAYMENT_HOLD" "SHIPPING_RISK_HOLD"
Copy
{
"payment_hold_reason": "PAYMENT_HOLD"
}
Rewards Decimal Precision
The decimal precision to use to compute rewards.

length	
string
The number of allowed decimal places after the decimal point for rewards conversions.

conversion_type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The list of conversion types.

Enum Value	Description
AMOUNT_TO_REWARDS	An amount-to-rewards conversion.
REWARDS_TO_AMOUNT	A rewards-to-amount conversion.
Copy
{
"length": "string",
"conversion_type": "AMOUNT_TO_REWARDS"
}
Rewards Rounding Mode
The rounding mode to use to compute rewards.

value	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The rewards amount rounding mode.

Enum Value	Description
UP	Round away from zero.
DOWN	Round toward zero.
HALF_UP	Round toward nearest neighbor unless both neighbors are equidistant. If equidistant, round up.
HALF_DOWN	Round toward nearest neighbor unless both neighbors are equidistant. If equidistant, round down.
HALF_EVEN	Round toward the nearest neighbor unless both neighbors are equidistant. If equidistant, round toward the even neighbor.
UNNECESSARY	Because the requested operation has an exact result, no rounding is necessary.
conversion_type	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The list of conversion types.

Enum Value	Description
AMOUNT_TO_REWARDS	An amount-to-rewards conversion.
REWARDS_TO_AMOUNT	A rewards-to-amount conversion.
Copy
{
"value": "UP",
"conversion_type": "AMOUNT_TO_REWARDS"
}
rewards_conversion_type
The list of conversion types.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The list of conversion types.

Enum Value	Description
AMOUNT_TO_REWARDS	An amount-to-rewards conversion.
REWARDS_TO_AMOUNT	A rewards-to-amount conversion.
Copy
"AMOUNT_TO_REWARDS"
rounding_mode_type
The rewards amount rounding mode.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The rewards amount rounding mode.

Enum Value	Description
UP	Round away from zero.
DOWN	Round toward zero.
HALF_UP	Round toward nearest neighbor unless both neighbors are equidistant. If equidistant, round up.
HALF_DOWN	Round toward nearest neighbor unless both neighbors are equidistant. If equidistant, round down.
HALF_EVEN	Round toward the nearest neighbor unless both neighbors are equidistant. If equidistant, round toward the even neighbor.
UNNECESSARY	Because the requested operation has an exact result, no rounding is necessary.
Copy
"UP"
Sale
The sale transaction details.

id
required
string
The ID of the sale transaction.

payment_mode	
string
The transaction payment mode. Supported only for PayPal payments.

Enum: "INSTANT_TRANSFER" "MANUAL_BANK_TRANSFER" "DELAYED_TRANSFER" "ECHECK"
state
required
string
The state of the sale transaction.

Enum: "completed" "partially_refunded" "pending" "refunded" "denied"
reason_code	
string
A reason code that describes why the transaction state is pending or reversed. Supported only for PayPal payments.

Enum: "CHARGEBACK" "GUARANTEE" "BUYER_COMPLAINT" "REFUND" "UNCONFIRMED_SHIPPING_ADDRESS" "ECHECK" "INTERNATIONAL_WITHDRAWAL" "RECEIVING_PREFERENCE_MANDATES_MANUAL_ACTION" "PAYMENT_REVIEW" "REGULATORY_REVIEW" "UNILATERAL" "VERIFICATION_REQUIRED" "TRANSACTION_APPROVED_AWAITING_FUNDING"
protection_eligibility	
string
The merchant protection level in effect for the transaction. Supported only for PayPal payments.

Enum: "ELIGIBLE" "PARTIALLY_ELIGIBLE" "INELIGIBLE"
protection_eligibility_type	
string
The merchant protection type in effect for the transaction. Returned only when protection_eligibility is ELIGIBLE or PARTIALLY_ELIGIBLE. Supported only for PayPal payments.

Enum: "ITEM_NOT_RECEIVED_ELIGIBLE" "UNAUTHORIZED_PAYMENT_ELIGIBLE" "ITEM_NOT_RECEIVED_ELIGIBLE,UNAUTHORIZED_PAYMENT_ELIGIBLE"
clearing_time	
string
The date and time when the PayPal eCheck transaction is expected to clear, in Internet date and time format.

payment_hold_status	
string
The recipient fund status. Returned only when the fund status is held.

Value: "HELD"
payment_hold_reasons	
Array of objects
An array of reasons that PayPal holds the recipient fund. Set only if the payment hold status is HELD.

exchange_rate	
string
The exchange rate for this transaction. Returned only in cross-currency use cases where a merchant bills a buyer in a non-primary currency for that buyer.

fmf_details	
object
The Fraud Management Filter (FMF) details that are applied to the payment that result in an accept, deny, or pending action. Returned in a payment response only if the merchant has enabled FMF in the profile settings and one of the fraud filters was triggered based on those settings. For more information, see Fraud Management Filters Summary.

receipt_id	
string
The receipt ID, which is a payment ID number that is returned for guest users to identify the payment.

parent_payment
required
string
The ID of the payment on which this transaction is based.

processor_response	
object
The processor-provided response codes that describe the submitted payment. Supported only when the payment_method is credit_card.

billing_agreement_id	
string
The ID of the billing agreement. Used as reference to execute this transaction.

create_time
required
string
The date and time of the sale, in Internet date and time format.

update_time	
string
The date and time when the resource was last updated, in Internet date and time format.

links	
Array of objects
An array of request-related HATEOAS links.

amount
required
object
The amount to collect.

transaction_fee	
object
The currency and amount of the transaction fee for this payment. Would not be returned for pending transactions where the funds have not been realized in the payee account.

receivable_amount	
object
The currency and amount of the net that the merchant receives for this transaction in their receivable currency. Returned only in cross-currency use cases where a merchant bills a buyer in a non-primary currency for that buyer.

transaction_fee_in_receivable_currency	
object
The currency and amount of the PayPal fee for this payment in the receivable currency. Returned only in cases the fee is charged in the receivable currency. Example CNY. Would not be returned for pending transactions where the funds have not been realized in the payee account.

CopyExpand allCollapse all
{
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "completed",
"reason_code": "CHARGEBACK",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"clearing_time": "2019-08-24T14:15:22Z",
"payment_hold_status": "HELD",
"payment_hold_reasons": [
{
"payment_hold_reason": "PAYMENT_HOLD"
}
],
"exchange_rate": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"receipt_id": "string",
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"billing_agreement_id": "string",
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
},
"receivable_amount": {
"currency": "string",
"value": "string"
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
}
}
Shipping Address
The shipping address details.

line1
required
string <= 300 characters
The first line of the address. For example, number, street, and so on.

line2	
string <= 300 characters
The second line of the address. For example, suite or apartment number.

city	
string <= 64 characters
The city name.

postal_code	
string
The postal code, which is the zip code or equivalent. Typically required for countries with a postal code or an equivalent. See postal code.

state	
string <= 300 characters
The code for a US state or the equivalent for other countries. Required for transactions if the address is in one of these countries: Argentina, Brazil, Canada, China, India, Italy, Japan, Mexico, Thailand, or United States.

phone	
string
The phone number, in E.123 format. Maximum length is 50 characters.

normalization_status	
string
The address normalization status. Returned only for payers from Brazil.

Enum Value	Description
UNKNOWN	Unknown.
UNNORMALIZED_USER_PREFERRED	Unnormalized user preferred.
NORMALIZED	Normalized.
UNNORMALIZED	Unnormalized.
type	
string
The type of address. For example, HOME_OR_WORK, GIFT, and so on.

country_code
required
string = 2 characters ^([A-Z]{2}|C2)$
The address country code.

recipient_name	
string <= 127 characters
The name of the recipient at this address.

Copy
{
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
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

address	
object
The address of the person to whom to ship the items. Supports only the address_line_1, address_line_2, admin_area_1, admin_area_2, postal_code, and country_code properties.

CopyExpand allCollapse all
{
"type": "SHIPPING",
"name": {
"full_name": "string"
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
sofort_request
Information needed to pay using Sofort.

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
store_in_vault_instruction
Defines how and when the payment source gets vaulted.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Defines how and when the payment source gets vaulted.

Value	Description
ON_SUCCESS	Defines that the payment_source will be vaulted only when at least one authorization or capture using that payment_source is successful.
Copy
"ON_SUCCESS"
stored_payment_source_payment_type
Indicates the type of the stored payment_source payment.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Indicates the type of the stored payment_source payment.

Enum Value	Description
ONE_TIME	One Time payment such as online purchase or donation. (e.g. Checkout with one-click).
RECURRING	Payment which is part of a series of payments with fixed or variable amounts, following a fixed time interval. (e.g. Subscription payments).
UNSCHEDULED	Payment which is part of a series of payments that occur on a non-fixed schedule and/or have variable amounts. (e.g. Account Topup payments).
Copy
"ONE_TIME"
stored_payment_source_usage_type
Indicates if this is a first or subsequent payment using a stored payment source (also referred to as stored credential or card on file).

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Default: "DERIVED"
Indicates if this is a first or subsequent payment using a stored payment source (also referred to as stored credential or card on file).

Enum Value	Description
FIRST	Indicates the Initial/First payment with a payment_source that is intended to be stored upon successful processing of the payment.
SUBSEQUENT	Indicates a payment using a stored payment_source which has been successfully used previously for a payment.
DERIVED	Indicates that PayPal will derive the value of FIRST or SUBSEQUENT based on data available to PayPal.
Copy
"FIRST"
tax_info
The tax ID of the customer. The customer is also known as the payer. Both tax_id and tax_id_type are required.

tax_id
required
string [ 1 .. 14 ] characters
The customer's tax ID value.

tax_id_type
required
string [ 1 .. 14 ] characters
The customer's tax ID type.

Enum Value	Description
BR_CPF	The individual tax ID type, typically is 11 characters long.
BR_CNPJ	The business tax ID type, typically is 14 characters long.
Copy
{
"tax_id": "string",
"tax_id_type": "BR_CPF"
}
token
The tokenized payment source to fund a payment.

id
required
string [ 1 .. 255 ] characters
The PayPal-generated ID for the token.

type
required
string [ 1 .. 255 ] characters
The tokenization method that generated the ID.

Value	Description
BILLING_AGREEMENT	The PayPal billing agreement ID. References an approved recurring payment for goods or services.
Copy
{
"id": "string",
"type": "BILLING_AGREEMENT"
}
Transaction
An array of payment-related transactions. A transaction defines what the payment is for and who fulfills the payment.

payee	
object
The payee who receives the funds and fulfills the order.

description	
string <= 127 characters
The purchase description.

note_to_payee	
string <= 255 characters
The note to the recipient of the funds in this transaction.

custom	
string <= 255 characters
The free-form field for the client's use.

invoice_number	
string <= 127 characters
The invoice number to track this payment.

soft_descriptor	
string <= 22 characters
The soft descriptor to use to charge this funding source. If greater than the maximum allowed length, the API truncates the string.

payment_options	
object
The payment options for this transaction.

item_list	
object
An array of items that are being purchased.

notify_url	
string <= 2048 characters
The URL to send payment notifications.

related_resources	
Array of objects
An array of payment-related transactions. A transaction defines what the payment is for and who fulfills the payment.

amount	
object
The payment amount, with details.

CopyExpand allCollapse all
{
"payee": {
"email": "user@example.com",
"merchant_id": "string"
},
"description": "string",
"note_to_payee": "string",
"custom": "string",
"invoice_number": "string",
"soft_descriptor": "string",
"payment_options": {
"allowed_payment_method": "UNRESTRICTED"
},
"item_list": {
"items": [
{
"sku": "string",
"name": "string",
"description": "string",
"quantity": "string",
"price": "string",
"currency": "str",
"tax": "string"
}
],
"shipping_address": {
"line1": "string",
"line2": "string",
"city": "string",
"postal_code": "string",
"state": "string",
"phone": "string",
"normalization_status": "UNKNOWN",
"type": "string",
"country_code": "st",
"recipient_name": "string"
},
"shipping_method": "string",
"shipping_phone_number": "string"
},
"notify_url": "http://example.com",
"related_resources": [
{
"sale": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "completed",
"reason_code": "CHARGEBACK",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"clearing_time": "2019-08-24T14:15:22Z",
"payment_hold_status": "HELD",
"payment_hold_reasons": [
{
"payment_hold_reason": "PAYMENT_HOLD"
}
],
"exchange_rate": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"receipt_id": "string",
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"billing_agreement_id": "string",
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
},
"receivable_amount": {
"currency": "string",
"value": "string"
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
}
},
"authorization": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "authorized",
"reason_code": "AUTHORIZATION",
"pending_reason": "AUTHORIZATION",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
},
"parent_payment": "string",
"processor_response": {
"response_code": "stri",
"avs_code": "s",
"cvv_code": "s",
"advice_code": "01_NEW_ACCOUNT_INFORMATION",
"eci_submitted": "string",
"vpas": "string"
},
"valid_until": "2019-08-24T14:15:22Z",
"create_time": "2019-08-24T14:15:22Z",
"update_time": "2019-08-24T14:15:22Z",
"receipt_id": "string",
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
},
"order": {
"id": "string",
"payment_mode": "INSTANT_TRANSFER",
"state": "PENDING",
"reason_code": "PAYER_SHIPPING_UNCONFIRMED",
"pending_reason": "payer_shipping_unconfirmed",
"protection_eligibility": "ELIGIBLE",
"protection_eligibility_type": "ITEM_NOT_RECEIVED_ELIGIBLE",
"parent_payment": "string",
"fmf_details": {
"filter_type": "ACCEPT",
"filter_id": "AVS_NO_MATCH",
"name": "string",
"description": "string"
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
},
"capture": {
"id": "string",
"is_final_capture": false,
"state": "pending",
"reason_code": "CHARGEBACK",
"parent_payment": "string",
"invoice_number": "string",
"exchange_rate": "string",
"note_to_payer": "string",
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
},
"transaction_fee_in_receivable_currency": {
"currency": "string",
"value": "string"
},
"receivable_amount": {
"currency": "string",
"value": "string"
}
},
"refund": {
"id": "string",
"state": "pending",
"reason": "string",
"invoice_number": "string",
"sale_id": "string",
"capture_id": "string",
"parent_payment": "string",
"description": "string",
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
}
}
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
CONSUMER	The customer vaulting the Venmo payment token is a consumer on the merchant / platform.
BUSINESS	The customer vaulting the Venmo payment token is a business on merchant / platform.
permit_multiple_payment_tokens	
boolean
Default: false
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
atributos_da_carteira_venmo
Atributos adicionais associados ao uso desta carteira Venmo.


cliente
objeto
Os dados pessoais de um cliente registrados no sistema do PayPal.


cofre
objeto
Atributos usados ​​para fornecer as instruções durante o processo de armazenamento seguro da Carteira Venmo.

CópiaExpandir tudoRecolher tudo
{
"customer": {
"id": "string",
"email_address": "string"
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
Cópia
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
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
string [ 3 .. 254 ] caracteres (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-... (email) Mostrar padrão
O endereço de e-mail do pagador.


atributos
objeto
Atributos adicionais associados ao uso desta carteira.

CópiaExpandir tudoRecolher tudo
{
"experience_context": {
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE"
},
"vault_id": "string",
"email_address": "string",
"attributes": {
"customer": {
"id": "string",
"email_address": "string"
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
Referência
PayPal.com
Privacidade
Apoiar
Jurídico
Contato
