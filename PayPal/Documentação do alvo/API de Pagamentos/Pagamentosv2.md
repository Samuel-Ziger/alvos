

Versão da API v2
Use a API de Pagamentos para autorizar pagamentos, capturar pagamentos autorizados, reembolsar pagamentos já capturados e exibir informações de pagamento. Utilize a API de Pagamentos em conjunto com a API de Pedidos . Para mais informações, consulte a Visão geral do PayPal Checkout .

Exibir detalhes do pagamento capturado
pegar
/v2/pagamentos/capturas/{capture_id}
Experimente
Exibe detalhes de um pagamento registrado, por ID.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
id_de_captura
obrigatório
corda
O ID gerado pelo PayPal para o pagamento capturado, cujos detalhes devem ser exibidos.

Respostas
200Uma solicitação bem-sucedida retorna o 200 OKcódigo de status HTTP e um corpo de resposta JSON que mostra os detalhes do pagamento capturados.
401A autenticação falhou devido à ausência do cabeçalho de autorização ou a credenciais de autenticação inválidas.
403A solicitação falhou porque o solicitante não possui permissões suficientes.
404A solicitação falhou porque o recurso não existe.
500A solicitação falhou devido a um erro interno do servidor.
Solicitar amostras
cURL

Exemplo 1 - 200 - Exibir detalhes do pagamento capturado
Exemplo 1 - 200 - Exibir detalhes do pagamento capturado
Cópia
curl  -v  -X GET https://api-m.sandbox.paypal.com/v2/payments/captures/74L756601X447022Y \ 
-H  'Content-Type: application/json'  \ 
-H  'Authorization: Bearer A21AAFs9YK9gWL6Vl6AqeoPtm-nf6JmtPOwAc8kfzHVdeigPEhrOJLCvbeIt3fJ4NKvyZo_iWic7sC3RIQrVUdu7igagcuMVQ'  
Amostras de resposta
200
401
403
404
500
aplicativo/json

Exemplo 1 - 200 - Exibir detalhes do pagamento capturado
Exemplo 1 - 200 - Exibir detalhes do pagamento capturado
CópiaExpandir tudoRecolher tudo
{
"id": "74L756601X447022Y",
"amount": {
"currency_code": "USD",
"value": "100.00"
},
"final_capture": true,
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"ITEM_NOT_RECEIVED",
"UNAUTHORIZED_TRANSACTION"
]
},
"disbursement_mode": "INSTANT",
"seller_receivable_breakdown": {
"gross_amount": {
"currency_code": "USD",
"value": "100.00"
},
"paypal_fee": {
"currency_code": "USD",
"value": "3.98"
},
"net_amount": {
"currency_code": "USD",
"value": "96.02"
}
},
"invoice_id": "OrderInvoice-23_10_2024_12_27_32_pm",
"status": "COMPLETED",
"supplementary_data": {
"related_ids": {
"order_id": "25M43554V9523650M",
"authorization_id": "0T620041CK889853A"
}
},
"payee": {
"email_address": "merchant@example.com",
"merchant_id": "YXZY75W2GKDQE"
},
"create_time": "2024-10-23T20:55:19Z",
"update_time": "2024-10-23T20:55:19Z",
"links": [
{
"href": "https://api-m.sandbox.paypal.com/v2/payments/captures/74L756601X447022Y",
"rel": "self",
"method": "GET"
},
{
"href": "https://api-m.sandbox.paypal.com/v2/payments/captures/74L756601X447022Y/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api-m.sandbox.paypal.com/v2/payments/authorizations/0T620041CK889853A",
"rel": "up",
"method": "GET"
}
]
}
Reembolso de pagamento capturado
publicar
/v2/pagamentos/capturas/{capture_id}/reembolso
Experimente
Reembolsa um pagamento capturado, por ID. Para um reembolso total, inclua um payload vazio no corpo da requisição JSON. Para um reembolso parcial, inclua um amountobjeto no corpo da requisição JSON.

Segurança
OAuth2
Solicitar
Parâmetros do caminho
id_de_captura
obrigatório
corda
O ID gerado pelo PayPal para o pagamento capturado a ser reembolsado.

Parâmetros do cabeçalho
ID da solicitação do PayPal
corda
O servidor armazena as chaves por 45 dias.

Preferência
corda
Padrão: 
retorno=mínimo
Resposta preferencial do servidor após a conclusão bem-sucedida da solicitação. O valor é:

return=minimalO servidor retorna uma resposta mínima para otimizar a comunicação entre o chamador da API e o servidor. Uma resposta mínima inclui os links `<username> id`, ` status<username>` e `<username>`.
return=representationO servidor retorna uma representação completa do recurso, incluindo o estado atual do recurso.
Declaração de autenticação do PayPal
corda
Uma declaração JSON Web Token (JWT) fornecida pelo chamador da API que identifica o comerciante. Para obter detalhes, consulte PayPal-Auth-Assertion .

Observação: Para transações entre três partes em que um parceiro gerencia as chamadas da API em nome de um comerciante, o parceiro deve identificar o comerciante usando um cabeçalho PayPal-Auth-Assertion ou um token de acesso com target_subject.
Esquema do corpo da requisição:
aplicativo/json
aplicativo/json
id_personalizado
string [ 1 .. 127 ] caracteres ^.*$
O ID externo fornecido pelo chamador da API. Usado para conciliar transações iniciadas pelo chamador da API com transações do PayPal. Aparece em relatórios de transações e liquidações. O padrão é definido por uma entidade externa e é compatível com Unicode.

id_da_fatura
string [ 1 .. 127 ] caracteres ^.*$
O ID da fatura externa fornecido pelo solicitante da API para este pedido. O padrão é definido por uma entidade externa e é compatível com Unicode.

nota_ao_pagador
string [ 1 .. 255 ] caracteres ^.*$
O motivo do reembolso. Aparece tanto no histórico de transações do pagador quanto nos e-mails que ele recebe. O padrão é definido por uma entidade externa e é compatível com Unicode.


quantia
objeto
O valor a ser reembolsado. Para reembolsar uma parte do valor capturado, especifique um valor. Se o valor não for especificado, captured amount - previous refundsserá reembolsado um valor igual a [valor omitido]. O valor deve ser um número positivo e na mesma moeda em que o pagamento foi capturado.


instruções_de_pagamento
objeto
Any additional refund instructions to be set during refund payment processing. This object is only applicable to merchants that have been enabled for PayPal Commerce Platform for Marketplaces and Platforms capability. Please speak to your account manager if you want to use this capability.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows refund details.
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows refund details.
400The request failed because it is not well-formed or is syntactically incorrect or violates schema.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
409The request failed because a previous call for the given resource is in progress.
422The request failed because it either is semantically incorrect or failed business validation.
500The request failed because an internal server error occurred.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Refund Captured Payment with an empty request
Sample 1 - 201 - Refund Captured Payment with an empty request
Copy
{ }
Response samples
200
201
400
401
403

4 more
4 more
application/json

Sample 1 - 201 - Refund Captured Payment with an empty request
Sample 1 - 201 - Refund Captured Payment with an empty request
CopyExpand allCollapse all
{
"id": "58K15806CS993444T",
"amount": {
"currency_code": "USD",
"value": "89.00"
},
"seller_payable_breakdown": {
"gross_amount": {
"currency_code": "USD",
"value": "89.00"
},
"paypal_fee": {
"currency_code": "USD",
"value": "0.00"
},
"net_amount": {
"currency_code": "USD",
"value": "89.00"
},
"total_refunded_amount": {
"currency_code": "USD",
"value": "100.00"
}
},
"invoice_id": "OrderInvoice-10_10_2024_12_58_20_pm",
"status": "COMPLETED",
"create_time": "2024-10-14T15:03:29-07:00",
"update_time": "2024-10-14T15:03:29-07:00",
"links": [
{
"href": "https://api.msmaster.qa.paypal.com/v2/payments/refunds/58K15806CS993444T",
"rel": "self",
"method": "GET"
},
{
"href": "https://api.msmaster.qa.paypal.com/v2/payments/captures/7TK53561YB803214S",
"rel": "up",
"method": "GET"
}
]
}
Void authorized payment
post
/v2/payments/authorizations/{authorization_id}/void
Try it
Voids, or cancels, an authorized payment, by ID. You cannot void an authorized payment that has been fully captured.

Security
Oauth2
Request
path Parameters
authorization_id
required
string
The PayPal-generated ID for the authorized payment to void.

header Parameters
PayPal-Auth-Assertion	
string
An API-caller-provided JSON Web Token (JWT) assertion that identifies the merchant. For details, see PayPal-Auth-Assertion.

Note:For three party transactions in which a partner is managing the API calls on behalf of a merchant, the partner must identify the merchant using either a PayPal-Auth-Assertion header or an access token with target_subject.
PayPal-Request-Id	
string
The server stores keys for 45 days.

Prefer	
string
Default: return=minimal
The preferred server response upon successful completion of the request. Value is:

return=minimal. The server returns a minimal response to optimize communication between the API caller and the server. A minimal response includes the id, status and HATEOAS links.
return=representation. The server returns a complete resource representation, including the current state of the resource.
Request Body schema: 
application/json
application/json
any
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows authorization details. This response is returned when the Prefer header is set to return=representation.
204A successful request returns the HTTP 204 No Content status code with no JSON response body. This response is returned when the Prefer header is set to return=minimal.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
409The request failed because a previous call for the given resource is in progress.
422The request failed because it either is semantically incorrect or failed business validation.
500The request failed because an internal server error occurred.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 204 - Void Authorized Payment
Sample 1 - 204 - Void Authorized Payment
Copy
{ }
Response samples
200
204
401
403
404

3 more
3 more
application/json

Sample 1 - 204 - Void Authorized Payment
Sample 1 - 204 - Void Authorized Payment
Copy
{ }
Show details for authorized payment
get
/v2/payments/authorizations/{authorization_id}
Try it
Shows details for an authorized payment, by ID.

Security
Oauth2
Request
path Parameters
authorization_id
required
string
The ID of the authorized payment for which to show details.

header Parameters
PayPal-Auth-Assertion	
string
An API-caller-provided JSON Web Token (JWT) assertion that identifies the merchant. For details, see PayPal-Auth-Assertion.

Note:For three party transactions in which a partner is managing the API calls on behalf of a merchant, the partner must identify the merchant using either a PayPal-Auth-Assertion header or an access token with target_subject.
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows authorization details.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
500The request failed because an internal server error occurred.
Request samples
cURL

Sample 1 - 200 - Show Authorized Payment Details
Sample 1 - 200 - Show Authorized Payment Details
Copy
curl -v -X GET https://api-m.sandbox.paypal.com/v2/payments/authorizations/0VF52814937998046 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer A21AAFs9YK9gWL6Vl6AqeoPtm-nf6JmtPOwAc8kfzHVdeigPEhrOJLCvbeIt3fJ4NKvyZo_iWic7sC3RIQrVUdu7igagcuMVQ'  
Response samples
200
401
403
404
500
application/json

Sample 1 - 200 - Show Authorized Payment Details
Sample 1 - 200 - Show Authorized Payment Details
CopyExpand allCollapse all
{
"id": "0VF52814937998046",
"status": "CREATED",
"amount": {
"value": "10.99",
"currency_code": "USD"
},
"invoice_id": "INVOICE-123",
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"ITEM_NOT_RECEIVED",
"UNAUTHORIZED_TRANSACTION"
]
},
"payee": {
"email_address": "merchant@example.com",
"merchant_id": "7KNGBPH2U58GQ"
},
"expiration_time": "2017-10-10T23:23:45Z",
"create_time": "2017-09-11T23:23:45Z",
"update_time": "2017-09-11T23:23:45Z",
"links": [
{
"rel": "self",
"method": "GET",
"href": "https://api-m.paypal.com/v2/payments/authorizations/0VF52814937998046"
},
{
"rel": "capture",
"method": "POST",
"href": "https://api-m.paypal.com/v2/payments/authorizations/0VF52814937998046/capture"
},
{
"rel": "void",
"method": "POST",
"href": "https://api-m.paypal.com/v2/payments/authorizations/0VF52814937998046/void"
},
{
"rel": "reauthorize",
"method": "POST",
"href": "https://api-m.paypal.com/v2/payments/authorizations/0VF52814937998046/reauthorize"
}
]
}
Reauthorize authorized payment
post
/v2/payments/authorizations/{authorization_id}/reauthorize
Try it
Reauthorizes an authorized PayPal account payment, by ID. To ensure that funds are still available, reauthorize a payment after its initial three-day honor period expires. Within the 29-day authorization period, you can issue multiple re-authorizations after the honor period expires.

If 30 days have transpired since the date of the original authorization, you must create an authorized payment instead of reauthorizing the original authorized payment.

A reauthorized payment itself has a new honor period of three days.

You can reauthorize an authorized payment from 4 to 29 days after the 3-day honor period. The allowed amount depends on context and geography, for example in US it is up to 115% of the original authorized amount, not to exceed an increase of $75 USD.

Supports only the amount request parameter.

Security
Oauth2
Request
path Parameters
authorization_id
required
string
The PayPal-generated ID for the authorized payment to reauthorize.

header Parameters
PayPal-Request-Id	
string
The server stores keys for 45 days.

Prefer	
string
Default: return=minimal
The preferred server response upon successful completion of the request. Value is:

return=minimal. The server returns a minimal response to optimize communication between the API caller and the server. A minimal response includes the id, status and HATEOAS links.
return=representation. The server returns a complete resource representation, including the current state of the resource.
PayPal-Auth-Assertion	
string
An API-caller-provided JSON Web Token (JWT) assertion that identifies the merchant. For details, see PayPal-Auth-Assertion.

Note:For three party transactions in which a partner is managing the API calls on behalf of a merchant, the partner must identify the merchant using either a PayPal-Auth-Assertion header or an access token with target_subject.
Request Body schema: 
application/json
application/json
amount	
object
The amount to reauthorize for an authorized payment.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows the reauthorized payment details.
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows the reauthorized payment details.
400The request failed because it is not well-formed or is syntactically incorrect or violates schema.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
422The request failed because it either is semantically incorrect or failed business validation.
500The request failed because an internal server error occurred.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Reauthorize Authorized Payment with an empty request
Sample 1 - 201 - Reauthorize Authorized Payment with an empty request
Copy
{ }
Response samples
200
201
400
401
403

3 more
3 more
application/json

Sample 1 - 201 - Reauthorize Authorized Payment with an empty request
Sample 1 - 201 - Reauthorize Authorized Payment with an empty request
CopyExpand allCollapse all
{
"id": "8AA831015G517922L",
"status": "CREATED",
"links": [
{
"rel": "self",
"method": "GET",
"href": "https://api.paypal.com/v2/payments/authorizations/8AA831015G517922L"
},
{
"rel": "capture",
"method": "POST",
"href": "https://api.paypal.com/v2/payments/authorizations/8AA831015G517922L/capture"
},
{
"rel": "void",
"method": "POST",
"href": "https://api.paypal.com/v2/payments/authorizations/8AA831015G517922L/void"
},
{
"rel": "reauthorize",
"method": "POST",
"href": "https://api.paypal.com/v2/payments/authorizations/8AA831015G517922L/reauthorize"
}
]
}
Capture authorized payment
post
/v2/payments/authorizations/{authorization_id}/capture
Try it
Captures an authorized payment, by ID.

Security
Oauth2
Request
path Parameters
authorization_id
required
string
The PayPal-generated ID for the authorized payment to capture.

header Parameters
PayPal-Request-Id	
string
The server stores keys for 45 days.

Prefer	
string
Default: return=minimal
The preferred server response upon successful completion of the request. Value is:

return=minimal. The server returns a minimal response to optimize communication between the API caller and the server. A minimal response includes the id, status and HATEOAS links.
return=representation. The server returns a complete resource representation, including the current state of the resource.
PayPal-Auth-Assertion	
string
An API-caller-provided JSON Web Token (JWT) assertion that identifies the merchant. For details, see PayPal-Auth-Assertion.

Note:For three party transactions in which a partner is managing the API calls on behalf of a merchant, the partner must identify the merchant using either a PayPal-Auth-Assertion header or an access token with target_subject.
Request Body schema: 
application/json
application/json
invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

note_to_payer	
string [ 1 .. 255 ] characters
An informational note about this settlement. Appears in both the payer's transaction history and the emails that the payer receives.

final_capture	
boolean
Default: false
Indicates whether you can make additional captures against the authorized payment. Set to true if you do not intend to capture additional payments against the authorization. Set to false if you intend to capture additional payments against the authorization.

payment_instruction	
object
Any additional payment instructions to be consider during payment processing. This processing instruction is applicable for Capturing an order or Authorizing an Order.

soft_descriptor	
string <= 22 characters
The payment descriptor on the payer's account statement.

amount	
object
The amount to capture. To capture a portion of the full authorized amount, specify an amount. If amount is not specified, the full authorized amount is captured. The amount must be a positive number and in the same currency as the authorization against which the payment is being captured.

Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows captured payment details.
201A successful request returns the HTTP 201 Created status code and a JSON response body that shows captured payment details.
400The request failed because it is not well-formed or is syntactically incorrect or violates schema.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
409The server has detected a conflict while processing this request.
422The request failed because it is semantically incorrect or failed business validation.
500The request failed because an internal server error occurred.
Request samples
Payload
cURL

application/json
application/json

Sample 1 - 201 - Capture Authorized Payment with an empty request
Sample 1 - 201 - Capture Authorized Payment with an empty request
Copy
{ }
Response samples
200
201
400
401
403

4 more
4 more
application/json

Sample 1 - 201 - Capture Authorized Payment with an empty request
Sample 1 - 201 - Capture Authorized Payment with an empty request
CopyExpand allCollapse all
{
"id": "7TK53561YB803214S",
"amount": {
"currency_code": "USD",
"value": "100.00"
},
"final_capture": true,
"seller_protection": {
"status": "ELIGIBLE",
"dispute_categories": [
"ITEM_NOT_RECEIVED",
"UNAUTHORIZED_TRANSACTION"
]
},
"seller_receivable_breakdown": {
"gross_amount": {
"currency_code": "USD",
"value": "100.00"
},
"paypal_fee": {
"currency_code": "USD",
"value": "3.98"
},
"net_amount": {
"currency_code": "USD",
"value": "96.02"
},
"exchange_rate": { }
},
"invoice_id": "OrderInvoice-10_10_2024_12_58_20_pm",
"status": "PENDING",
"status_details": {
"reason": "OTHER"
},
"create_time": "2024-10-14T21:37:10Z",
"update_time": "2024-10-14T21:37:10Z",
"links": [
{
"href": "https://api.msmaster.qa.paypal.com/v2/payments/captures/7TK53561YB803214S",
"rel": "self",
"method": "GET"
},
{
"href": "https://api.msmaster.qa.paypal.com/v2/payments/captures/7TK53561YB803214S/refund",
"rel": "refund",
"method": "POST"
},
{
"href": "https://api.msmaster.qa.paypal.com/v2/payments/authorizations/6DR965477U7140544",
"rel": "up",
"method": "GET"
}
]
}
Show refund details
get
/v2/payments/refunds/{refund_id}
Try it
Shows details for a refund, by ID.

Security
Oauth2
Request
path Parameters
refund_id
required
string
The PayPal-generated ID for the refund for which to show details.

header Parameters
PayPal-Auth-Assertion	
string
An API-caller-provided JSON Web Token (JWT) assertion that identifies the merchant. For details, see PayPal-Auth-Assertion.

Note:For three party transactions in which a partner is managing the API calls on behalf of a merchant, the partner must identify the merchant using either a PayPal-Auth-Assertion header or an access token with target_subject.
Responses
200A successful request returns the HTTP 200 OK status code and a JSON response body that shows refund details.
401Authentication failed due to missing authorization header, or invalid authentication credentials.
403The request failed because the caller has insufficient permissions.
404The request failed because the resource does not exist.
500The request failed because an internal server error occurred.
Request samples
cURL

Sample 1 - 200 - Show Refund Details with Platform Fees
Sample 1 - 200 - Show Refund Details with Platform Fees
Copy
curl -v -X GET https://api-m.sandbox.paypal.com/v2/payments/refunds/1JU08902781691411 \
-H 'Content-Type: application/json' \
-H 'Authorization: Bearer A21AAFs9YK9gWL6Vl6AqeoPtm-nf6JmtPOwAc8kfzHVdeigPEhrOJLCvbeIt3fJ4NKvyZo_iWic7sC3RIQrVUdu7igagcuMVQ'  
Response samples
200
401
403
404
500
application/json

Sample 1 - 200 - Show Refund Details with Platform Fees
Sample 1 - 200 - Show Refund Details with Platform Fees
CopyExpand allCollapse all
{
"id": "1JU08902781691411",
"amount": {
"value": "10.99",
"currency_code": "USD"
},
"status": "COMPLETED",
"note": "Defective product",
"seller_payable_breakdown": {
"gross_amount": {
"value": "10.99",
"currency_code": "USD"
},
"paypal_fee": {
"value": "0.33",
"currency_code": "USD"
},
"platform_fees": [
{
"amount": {
"currency_code": "USD",
"value": "1.00"
},
"payee": {
"email_address": "fee@example.com"
}
}
],
"net_amount": {
"value": "9.66",
"currency_code": "USD"
},
"total_refunded_amount": {
"value": "10.99",
"currency_code": "USD"
}
},
"invoice_id": "INVOICE-123",
"create_time": "2018-09-11T23:24:19Z",
"update_time": "2018-09-11T23:24:19Z",
"links": [
{
"rel": "self",
"method": "GET",
"href": "https://api-m.paypal.com/v2/payments/refunds/1JU08902781691411"
},
{
"rel": "up",
"method": "GET",
"href": "https://api-m.paypal.com/v2/payments/captures/2GG279541U471931P"
}
]
}
Errors
AUTH_CAPTURE_CURRENCY_MISMATCH
Message:
Currency of capture must be the same as currency of authorization.

Description:
Verify the currency of the capture and try the request again.

AUTH_CURRENCY_MISMATCH
Message:
The currency specified during reauthorization should be the same as the currency specified in the original authorization. Please check the currency of the authorization for which you are trying to reauthorize and try again.

AUTHENTICATION_FAILURE
Message:
Authentication failed due to missing authorization header, or invalid authentication credentials.

Description:
Account validations failed for the user.

AUTHORIZATION_ALREADY_CAPTURED
Message:
Authorization has already been captured.

Description:
If final_capture is set to to true, additional captures are not possible against the authorization.

AUTHORIZATION_AMOUNT_EXCEEDED
Description: Authorization amount specified exceeded allowable limit. Specify a different amount and try the request again. Alternately, contact Customer Support to increase your limits. Local regulations (e.g. in PSD2 countries) prohibit overages above the amount authorized by the payer.

AUTHORIZATION_DENIED
Message:
A denied authorization cannot be captured.

Description:
You cannot capture a denied authorization.

AUTHORIZATION_EXPIRED
Message:
An expired authorization cannot be captured.

Description:
You cannot capture an expired authorization.

AUTHORIZATION_VOIDED
Message:
A voided authorization cannot be captured or reauthorized.

Description:
You cannot capture or reauthorize a voided authorization.

CANNOT_BE_NEGATIVE
Description: Must be greater than or equal to 0.

CANNOT_BE_VOIDED
Message:
A reauthorization cannot be voided. Please void the original parent authorization.

Description:
You cannot void a reauthorized payment. You must void the original parent authorized payment.

CANNOT_BE_ZERO_OR_NEGATIVE
Message:
Must be greater than zero. If the currency supports decimals, only two decimal place precision is supported.

Description:
Specify a different value and try the request again.

CANNOT_REFUND_SELF
Description: The payer and payee for the refund cannot be same.

CAPTURE_FULLY_REFUNDED
Message:
The capture has already been fully refunded.

Description:
You cannot capture additional refunds against this capture.

CAPTURED_AMOUNT_FULLY_REFUNDED
Description: The captured amount has already been fully refunded.

CARD_BILLING_ADDRESS_COUNTRY_NOT_SUPPORTED
Description: Specified country is not currently supported for payment processing.

CARD_BRAND_NOT_SUPPORTED
Description: Refund cannot be issued to this card. The card brand card_brand is not supported. Please try again with another card.

CARD_EXPIRED
Description: The card is expired.

CARD_ISSUER_COUNTRY_NOT_SUPPORTED
Description: Card is issued by a financial institution for a country (e.g. Cuba, Iran, North Korea, Syria) that is not current supported for payment processing.

CARD_TYPE_NOT_SUPPORTED
Description: Processing of this card type is not supported. Use another type of card.

COMPLIANCE_VIOLATION
Description: Transaction is declined due to compliance violation.

CURRENCY_MISMATCH
Message:
All amounts specified should be in the same currency. Please ensure that the currency for the 'amount' and that of 'platform_fees.amount' is the same.

CURRENCY_NOT_SUPPORTED_FOR_CARD_BRAND
Description: Currency not supported for card specified. Card is card_brand. Only currency_code is supported for this brand of card.

CURRENCY_NOT_SUPPORTED_FOR_CARD_TYPE
Description: Currency code not supported for direct card payments using this card type. See Currency codes for list of supported currency codes.

CURRENCY_NOT_SUPPORTED_FOR_COUNTRY
Description: Currency code not supported for card payments in your country.

DECIMAL_PRECISION
Message:
The value of the field should not be more than two decimal places.

Description:
If the currency supports decimals, only two decimal place precision is supported.

DECIMALS_NOT_SUPPORTED
Message:
Currency does not support decimals.

Description:
Currency does not support decimals. Please refer to https://developer.paypal.com/docs/api/reference/currency-codes/ for more information.

DELAYED_DISBURSEMENT_NOT_SUPPORTED
Message:
The API Caller is not enabled to process transactions by specifying disbursement mode as delayed.

Description:
Verify the API caller is enabled to process transaction with delayed disbursement.

DUPLICATE_INVOICE_ID
Message:
Requested invoice number has been previously captured. Possible duplicate transaction.

Description:
Payment for this invoice was already captured.

DUPLICATE_REFUND
Description: Requested invoice_id has been previously refunded. Possible duplicate transaction.

DUPLICATE_TRANSACTION
Description: Duplicate invoice Id detected.

FX_RATE_CHANGE_DUE_TO_MARKET_EVENT
Description: The FX rate associated with the specified FX rate ID has been changed due to market events. Please refer to the provided new_fx_id link to retrieve a new FX rate ID and try the request again.

INSTRUMENT_DECLINED
Description: The instrument presented was either declined by the processor or bank, or it can't be used for this payment.

INTERNAL_SERVER_ERROR
Message:
An internal server error has occurred.

Description:
Try your request again later.

INVALID_ACCOUNT_STATUS
Message:
Account validations failed for the user.

Description:
The user account could not be validated.

INVALID_ARRAY_LENGTH
Message:
Request is not well-formed, syntactically incorrect, or violates schema.

Description:
The number of items in an array parameter is too small or too large.

INVALID_CARD_NUMBER
Description: The card number is invalid.

INVALID_COUNTRY_CODE
Message:
The requested action could not be performed, semantically incorrect, or failed business validation.

Description:
Country code is invalid. Please refer to https://developer.paypal.com/docs/integration/direct/rest/country-codes/ for a list of supported country codes.

INVALID_CURRENCY_CODE
Message:
Currency code should be a three-character ISO-4217 currency code.

Description:
Currency code is invalid or is not currently supported. Please refer https://developer.paypal.com/docs/api/reference/currency-codes/ for list of supported currency codes.

INVALID_FX_RATE_ID
Description: The specified FX Rate ID is not valid.

INVALID_INVOICE_ID
Message:
Specified invoice_id does not exist.

Description:
Please check the invoice_id and try again.

INVALID_PARAMETER_SYNTAX
Message:
The value of the field does not conform to the expected format.

Description:
Verify the specification for supported pattern and try the request again.

INVALID_PARAMETER_VALUE
Message:
The value of a field is invalid.

Description:
Verify the specification for the allowed values and try the request again.

INVALID_PAYEE_ACCOUNT
Message:
Payee account is invalid.

Description:
Verify the payee account information and try the request again.

INVALID_PLATFORM_FEES_ACCOUNT
Message:
The specified platform_fees payee account is either invalid or account setup is incomplete. Please work with your PayPal Account Manager to enable this option for your account.

Description:
Verify the platform fee account set up is completed or correct account.

INVALID_PLATFORM_FEES_AMOUNT
Message:
The platform_fees amount cannot be greater than the capture amount.

Description:
Verify the platform_fees amount and try the request again.

INVALID_RESOURCE_ID
Message:
Specified resource ID does not exist. Please check the resource ID and try again.

Description:
Verify the resource ID and try the request again.

INVALID_SECURITY_CODE_LENGTH
Description: The security_code length is invalid for the specified card brand.

INVALID_STRING_LENGTH
Message:
The value of a field is either too short or too long.

Description:
Verify the specification for the supported min and max values and try the request again.

INVALID_STRING_MAX_LENGTH
Message:
The value of a field is too long.

Description:
The parameter string is too long.

KYC_HOLD
Message:
The merchant's KYC status is incomplete.

Description: The payment was pending due to the merchant's incomplete KYC status.

MALFORMED_REQUEST_JSON
Message:
Request is not well-formed, syntactically incorrect, or violates schema.

Description: The request JSON is not well formed.

MAX_CAPTURE_AMOUNT_EXCEEDED
Message:
Capture amount exceeds allowable limit. Please contact customer service or your account manager to request the change to your overage limit. The default overage limit is 115%, which allows the sum of all captures to be up to 115% of the order amount. Local regulations (e.g. in PSD2 countries) prohibit overages above the amount authorized by the payer.

Description:
Specify a different amount and try the request again. Alternately, contact Customer Support to increase your limits.

MAX_CAPTURE_COUNT_EXCEEDED
Message:
Maximum number of allowable captures has been reached. No additional captures are possible for this authorization. Please contact customer service or your account manager to change the number of captures that be made for a given authorization.

Description:
You cannot make additional captures.

MAX_NUMBER_OF_REFUNDS_EXCEEDED
Message:
You have exceeded the number of refunds that can be processed per capture.

Description:
Please contact customer support or your account manager to review the number of refunds that can be processed per capture.

MAX_PAYEE_AMOUNT_LIMIT_EXCEEDED
Description: Refund amount exceeds the allowed cumulative limit that the payee can receive.

MAX_PAYER_AMOUNT_LIMIT_EXCEEDED
Description: Refund amount exceeds the allowed cumulative limit that the payer can refund.

MAX_REFUND_LIMIT_EXCEEDED
Description: Refund amount exceeds allowable limit. Specify a different amount and try the request again. Alternately, contact Customer Support to increase your limits.

MISSING_REQUIRED_PARAMETER
Message:
A required field / parameter is missing.

Description:
Verify the specification for required fields and try the request again.

MULTI_CURRENCY_CODE
Message:
The requested action could not be performed, semantically incorrect, or failed business validation.

Description:
Multiple differing values of currency_code are not supported. Entire request must have the same currency_code.

MULTIPLE_AUTHORIZATIONS_FOUND
Message:
Cannot void multiple authorizations.

Description:
Specified invoice_id is associated with more than one authorization.

NOT_AUTHORIZED
Message:
You do not have permission to access or perform operations on this resource.

Description:
To make API calls on behalf of a merchant, ensure that you have sufficient permissions to proceed with this transaction.

PARTIAL_REFUND_NOT_ALLOWED
Message:
You cannot do a refund for an amount less than the original capture amount.

Description:
Specify an amount equal to the capture amount or omit the amount object from the request. Then, try the request again.

PAYEE_ACCOUNT_INVALID
Message:
The requested action could not be performed, semantically incorrect, or failed business validation.

Description:
Payee account specified is invalid. Please check the payee.merchant_id specified and try again.

PAYEE_ACCOUNT_LOCKED_OR_CLOSED
Message:
Transaction could not complete because payee account is locked or closed.

Description:
To get more information about the status of the account, call Customer Support.

PAYEE_ACCOUNT_RESTRICTED
Message:
Payee account is restricted.

Description:
To get more information about the status of the account, call Customer Support.

PAYEE_FX_RATE_ID_CURRENCY_MISMATCH
Description: The specified FX Rate ID is for a currency that does not match with the currency of this request.

PAYEE_FX_RATE_ID_EXPIRED
Description: The specified FX Rate ID has expired.

PAYEE_NOT_CONSENTED
Description: Payee does not have appropriate consent to allow the API caller to process this type of transaction on their behalf. Your current setup requires the 'payee' to provide a consent before this transaction can be processed successfully.

PAYEE_REFUND_AMOUNT_LIMIT_EXCEEDED
Description: Refund amount exceeds per transaction limit that the payee can receive.

PAYER_ACCOUNT_LOCKED_OR_CLOSED
Message:
The payer account cannot be used for this transaction.

Description:
Contact the payer for an alternate payment method.

PAYER_ACCOUNT_RESTRICTED
Message:
Payer account is restricted.

Description:
To get more information about the status of the account, call Customer Support.

PAYER_CANNOT_PAY
Message:
Payer cannot pay for this transaction.

Description:
Please contact the payer to find other ways to pay for this transaction.

PAYER_REFUND_AMOUNT_LIMIT_EXCEEDED
Description: Refund amount exceeds per transaction limit that the payer can refund.

PENDING_CAPTURE
Message:
Cannot initiate a refund as the capture is pending.

Description:
Capture is typically pending when the payer has funded the transaction by using an e-check or bank account.

PERMISSION_DENIED
Message:
You do not have permission to access or perform operations on this resource.

Description:
To make API calls on behalf of a merchant, ensure that you have sufficient permissions to proceed with this transaction.

PERMISSION_NOT_GRANTED
Message:
Payee of the authorization has not granted permission to perform capture on the authorization.

Description:
To make API calls on behalf of a merchant, ensure that you have sufficient permissions to capture the authorization.

PLATFORM_FEE_EXCEEDED
Message:
Platform fee amount specified exceeds the amount that is available for refund. You can only refund up to the available platform fee amount. This error is also returned when no platform_fee was specified or was zero when the payment was captured.

PLATFORM_FEE_NOT_ENABLED
Message:
The API Caller account is not setup to be able to process refunds with 'platform_fees'. Please contact your Account Manager. This feature is useful when you want to contribute a portion of the 'platform_fees' you had capture as part of the refund being processed.

PLATFORM_FEES_NOT_SUPPORTED
Message:
The API Caller is not enabled to process transactions by specifying 'platform_fees'. Please work with your PayPal Account Manager to enable this option for your account.

Description:
Verify the API caller is enabled to process the transaction with platform fees.

PREVIOUS_REQUEST_IN_PROGRESS
Message:
A previous request on this resource is currently in progress. Please wait for sometime and try again. It is best to space out the initial and the subsequent request(s) to avoid receiving this error.

Description:
This scenario only occurs when making multiple API requests on the same resource within a very short duration. To resolve this, API callers need to make subsequent requests with a delay.

PREVIOUSLY_CAPTURED
Message:
Authorization has been previously captured and hence cannot be voided.

Description:
This authorized payment was already captured. You cannot capture it again.

PREVIOUSLY_VOIDED
Message:
Authorization has been previously voided and hence cannot be voided again.

Description:
This authorized payment was already voided. You cannot void it again.

RATE_LIMIT_REACHED
Message:
Too many requests. Blocked due to rate limiting.

Description: The rate limit was reached.

REAUTHORIZATION_DECLINED_BY_PROCESSOR
Description: The reauthorization was declined by the processor.

REAUTHORIZATION_NOT_SUPPORTED
Message:
A reauthorize cannot be attempted on an authorization_id that is the result of a prior reauthorization or on an authorization made on an Order saved using the v2/orders/id/save API.

REAUTHORIZATION_NOT_SUPPORTED_FOR_PAYMENT_SOURCE
Description: Reauthorization is not supported for the payment source.

REAUTHORIZATION_TOO_SOON
Description: A reauthorization is only allowed once from Day 4 to Day 29 since the date of the original authorization.

REFUND_AMOUNT_EXCEEDED
Message:
The refund amount must be less than or equal to the capture amount that has not yet been refunded.

Description:
Verify the refund amount and try the request again.

REFUND_AMOUNT_TOO_LOW
Message:
The amount after applying currency conversion is zero and hence the capture cannot be refunded. The currency conversion is required because the currency of the capture is different than the currency in which the amount was settled into the payee account.

Description:
The amount after applying currency conversion is zero and hence the capture cannot be refunded. The currency conversion is required because the currency of the capture is different than the currency in which the amount was settled into the payee account.

REFUND_CAPTURE_CURRENCY_MISMATCH
Message:
Refund must be in the same currency as the capture.

Description:
Verify the currency of the refund and try the request again.

REFUND_CURRENCY_MISMATCH
Description: Refund must be in the same currency as the capture.

REFUND_FAILED_BY_PAYMENT_SOURCE
Message:
Refund was refused by the payment source.

Description:
We're unable to process refunds for the payer's selected payment source. Contact the payer directly to arrange a refund via alternative means.

REFUND_FAILED_INSUFFICIENT_FUNDS
Message:
Refund cannot be processed due to insufficient funds.

Description:
Capture could not be refunded due to insufficient funds. Please check to see if you have sufficient funds in your PayPal account or if the bank account linked to your PayPal account is verified and has sufficient funds.

REFUND_FUNDS_NOT_AVAILABLE
Description: There is no valid funding instrument linked to the account from which refund can be processed.

REFUND_IS_RESTRICTED
Message:
This refund can only be processed by the API caller that had 'captured' the transaction. If you facilitate your transactions via a platform/partner, please initiate a refund through them.

REFUND_NOT_ALLOWED
Message:
Capture cannot be refunded.

Description:
You cannot refund this capture.

REFUND_NOT_PERMITTED_DUE_TO_CHARGEBACK
Message:
The requested action could not be performed, semantically incorrect, or failed business validation.

Description:
Refunds not allowed on this capture due to a chargeback on the card or bank. Please contact the payee to resolve the chargeback.

REFUND_NOT_SUPPORTED_FOR_PAYMENT_SOURCE
Message:
Refund was refused by the payment source.

Description:
Refunds are not supported for the payer's selected payment source. Contact the payer directly to arrange a refund via alternative means.

REFUND_REFUSED_BY_PROCESSOR
Description: The funding instrument linked to the account has been declined by either the processor or PayPal internal system.

REFUND_TIME_EXCEEDED_FOR_PAYMENT_SOURCE
Message:
Refund was refused by the payment source.

Description:
The time limit set by the payer's selected payment source to refund this transaction has expired. Contact the payer directly to arrange a refund via alternative means.

REFUND_TIME_LIMIT_EXCEEDED
Message:
You are over the time limit to perform a refund on this capture.

Description:
The refund cannot be issued at this time.

REFUND_TRANSACTION_REFUSED
Description: PayPal's internal controls or user account settings prevent refund from being processed.

SHIPPING_ADDRESS_INVALID
Message:
Address field does not match the corresponding validation regex.

SHIPPING_ADDRESS_NOT_ALLOWED_BY_RESIDENCE_COUNTRY
Message:
The provided shipping address is not allowed by buyer's residency country.

TRANSACTION_DISPUTED
Message:
Partial refunds cannot be offered at this time because there is an open case on this transaction. Visit the PayPal Resolution Center to review this case.

Description:
Refund for an amount less than the remaining transaction amount cannot be processed at this time because of an open dispute on the capture. Please visit the PayPal Resolution Center to view the details.

TRANSACTION_REFUSED
Message:
PayPal's internal controls prevent authorization from being captured.

Description:
For more information about this transaction, contact customer support.

UPDATE_AUTHORIZATION_NOT_SUPPORTED
Message:
Update authorization is not allowed for this type of authorization.

Definitions
3ds_card_brand
Card brand that the transaction was processed for authentication.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Card brand that the transaction was processed for authentication.

Enum Value	Description
AMERICAN_EXPRESS	Card Brand Amex.
DISCOVER	Card Brand DISCOVER.
JCB	Card Brand JCB.
MAESTRO	Card Brand MAESTRO.
MASTERCARD	Card Brand MASTERCARD.
SOLO	Card Brand SOLO.
VISA	Card Brand VISA.
ELECTRON	Card Brand ELECTRON.
ELO	Card Brand ELO.
Copy
"AMERICAN_EXPRESS"
activity_timestamps
The date and time stamps that are common to authorized payment, captured payment, and refund transactions.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

Copy
{
"create_time": "string",
"update_time": "string"
}
altpay_customer
Alternative Payment Method (APM) customer information.

merchant_customer_id	
string [ 1 .. 64 ] characters
Merchants and partners may already have a data-store where their customer information is persisted. Use merchant_customer_id to get the ID of the merchant's customer.

Copy
{
"merchant_customer_id": "string"
}
amount_breakdown
The breakdown of the amount. Breakdown provides details such as total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.

item_total	
object
The subtotal for all items. Required if the request includes purchase_units[].items[].unit_amount. Must equal the sum of (items[].unit_amount * items[].quantity) for all items. item_total.value can not be a negative number.

shipping	
object
The shipping fee for all items within a given purchase_unit. shipping.value can not be a negative number.

handling	
object
The handling fee for all items within a given purchase_unit. handling.value can not be a negative number.

tax_total	
object
The total tax for all items. Required if the request includes purchase_units.items.tax. Must equal the sum of (items[].tax * items[].quantity) for all items. tax_total.value can not be a negative number.

insurance	
object
The insurance fee for all items within a given purchase_unit. insurance.value can not be a negative number.

shipping_discount	
object
The shipping discount for all items within a given purchase_unit. shipping_discount.value can not be a negative number.

discount	
object
The discount for all items within a given purchase_unit. discount.value can not be a negative number.

CopyExpand allCollapse all
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
amount_with_breakdown
The total order amount with an optional breakdown that provides details, such as the total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.
If you specify amount.breakdown, the amount equals item_total plus tax_total plus shipping plus handling plus insurance minus shipping_discount minus discount.
The amount must be a positive number. For listed of supported currencies and decimal precision, see the PayPal REST APIs Currency Codes.

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
breakdown	
object
The breakdown of the amount. Breakdown provides details such as total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.

CopyExpand allCollapse all
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
apple_pay_attributes
Additional attributes associated with apple pay.

customer	
object
This object represents a merchant’s customer, allowing them to store contact details, and track all payments associated with the same customer.

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
apple_pay_experience_context
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
assurance_details
Information about cardholder possession validation and cardholder identification and verifications (ID&V).

account_verified	
boolean
Default: false
If true, indicates that Cardholder possession validation has been performed on returned payment credential.

card_holder_authenticated	
boolean
Default: false
If true, indicates that identification and verifications (ID&V) was performed on the returned payment credential.If false, the same risk-based authentication can be performed as you would for card transactions. This risk-based authentication can include, but not limited to, step-up with 3D Secure protocol if applicable.

Copy
{
"account_verified": false,
"card_holder_authenticated": false
}
authentication_response
Results of Authentication such as 3D Secure.

liability_shift	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Liability shift indicator. The outcome of the issuer's authentication.

Enum Value	Description
NO	Liability is with the merchant.
POSSIBLE	Liability may shift to the card issuer.
UNKNOWN	The authentication system is not available.
three_d_secure	
object
Results of 3D Secure Authentication.

CopyExpand allCollapse all
{
"liability_shift": "NO",
"three_d_secure": {
"authentication_status": "Y",
"enrollment_status": "Y"
}
}
authentication_type
Indicates the type of authentication that was used to challenge the card holder.

string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Indicates the type of authentication that was used to challenge the card holder.

Enum Value	Description
STATIC	Indicates fixed password.
DYNAMIC	Indicates one-time password. Could be single-step or multi-step.
OUT_OF_BAND	Indicates biometric over the phone.
DECOUPLED	Indicates decoupled authentication.
Copy
"STATIC"
authorization
The authorized payment transaction.

status	
string
The status for the authorized payment.

Enum Value	Description
CREATED	The authorized payment is created. No captured payments have been made for this authorized payment.
CAPTURED	The authorized payment has one or more captures against it. The sum of these captured payments is greater than the amount of the original authorized payment.
DENIED	PayPal cannot authorize funds for this authorized payment.
PARTIALLY_CAPTURED	A captured payment was made for the authorized payment for an amount that is less than the amount of the original authorized payment.
VOIDED	The authorized payment was voided. No more captured payments can be made against this authorized payment.
PENDING	The created authorization is in pending state. For more information, see status.details.
status_details	
object
The details of the authorized order pending status.

id	
string
The PayPal-generated ID for the authorized payment.

invoice_id	
string
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

custom_id	
string <= 255 characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

links	
Array of objects
An array of related HATEOAS links.

amount	
object
The amount for this authorized payment.

network_transaction_reference	
object
Reference values used by the card network to identify a transaction.

seller_protection	
object
The level of protection offered as defined by PayPal Seller Protection for Merchants.

expiration_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the authorized payment expires, in Internet date and time format.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

CopyExpand allCollapse all
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
Authorization
The authorized payment transaction.

status	
string
The status for the authorized payment.

Enum Value	Description
CREATED	The authorized payment is created. No captured payments have been made for this authorized payment.
CAPTURED	The authorized payment has one or more captures against it. The sum of these captured payments is greater than the amount of the original authorized payment.
DENIED	PayPal cannot authorize funds for this authorized payment.
PARTIALLY_CAPTURED	A captured payment was made for the authorized payment for an amount that is less than the amount of the original authorized payment.
VOIDED	The authorized payment was voided. No more captured payments can be made against this authorized payment.
PENDING	The created authorization is in pending state. For more information, see status.details.
status_details	
object
The details of the authorized order pending status.

id	
string
The PayPal-generated ID for the authorized payment.

invoice_id	
string
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

custom_id	
string <= 255 characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

links	
Array of objects
An array of related HATEOAS links.

amount	
object
The amount for this authorized payment.

network_transaction_reference	
object
Reference values used by the card network to identify a transaction.

seller_protection	
object
The level of protection offered as defined by PayPal Seller Protection for Merchants.

expiration_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the authorized payment expires, in Internet date and time format.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

supplementary_data	
object
An object that provides supplementary/additional data related to a payment transaction.

payee	
object
The details associated with the merchant for this transaction.

CopyExpand allCollapse all
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
"supplementary_data": {
"related_ids": {
"order_id": "string",
"authorization_id": "string",
"capture_id": "string"
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
}
}
authorization_status
The status fields and status details for an authorized payment.

status	
string
The status for the authorized payment.

Enum Value	Description
CREATED	The authorized payment is created. No captured payments have been made for this authorized payment.
CAPTURED	The authorized payment has one or more captures against it. The sum of these captured payments is greater than the amount of the original authorized payment.
DENIED	PayPal cannot authorize funds for this authorized payment.
PARTIALLY_CAPTURED	A captured payment was made for the authorized payment for an amount that is less than the amount of the original authorized payment.
VOIDED	The authorized payment was voided. No more captured payments can be made against this authorized payment.
PENDING	The created authorization is in pending state. For more information, see status.details.
status_details	
object
The details of the authorized order pending status.

CopyExpand allCollapse all
{
"status": "CREATED",
"status_details": {
"reason": "PENDING_REVIEW"
}
}
authorization_status_details
The details of the authorized payment status.

reason	
string [ 1 .. 64 ] characters ^[A-Z_]+$
The reason why the authorized status is PENDING.

Enum Value	Description
PENDING_REVIEW	Authorization is pending manual review.
DECLINED_BY_RISK_FRAUD_FILTERS	Risk Filter set by the payee failed for the transaction.
Copy
{
"reason": "PENDING_REVIEW"
}
BIC
The business identification code (BIC). In payments systems, a BIC is used to identify a specific business, most commonly a bank.

string [ 8 .. 11 ] characters ^[A-Z-a-z0-9]{4}[A-Z-a-z]{2}[A-Z-a-z0-9]{2}([...Show pattern
The business identification code (BIC). In payments systems, a BIC is used to identify a specific business, most commonly a bank.

Copy
"stringst"
Billing Cycle
The billing cycle providing details of the billing frequency, amount, duration and if the billing cycle is a free, discounted or regular billing cycle.

id	
string [ 1 .. 128 ] characters Show pattern
The unique identifier for this billing cycle or recurring series.

current_period	
boolean
Indicates if this period, with its subscription terms, is currently active.

billing_interval_unit	
string [ 1 .. 24 ] characters
The billing interval unit for the billing cycle.

Enum Value	Description
YEAR	The billing interval for the billing cycle is a year.
MONTH	The billing interval for the billing cycle is a month.
SEMIMONTH	The billing interval for the billing cycle is half a month.
WEEK	The billing interval for the billing cycle is a week.
DAY	The billing interval for the billing cycle is a day.
billing_frequency	
integer [ 1 .. 365 ]
Default: 1
The frequency, i.e. the number of billing interval units, at which the user is charged. E.g. - if the billing_interval_unit is DAY, with a billing_frequency of 2, the user is billed once every two days.

total_billing_cycles	
integer [ 0 .. 999 ]
Default: 1
The number of billing cycles in the period. This is effectively the number of times the user will be charged in the period.

current_billing_cycle	
integer [ 0 .. 999 ]
Indicates the index of the current billing cycle for the period. E.g. - a value of 3 would mean that its the third billing cycle for the period.

regular_period	
boolean
Indicates if the period is a regular period. A regular period would have standard billing cycle terms, as opposed to some special terms that might be applicable for a trial or promotional period.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The create date and time for this billing cycle or recurring series.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The update date and time for this billing cycle or recurring series.

billing_amount	
object
The amount that the user will be charged in each billing cycle.

shipping_amount	
object
The shipping charges applicable for each cycle.

tax_amount	
object
The tax amount applicable for each cycle.

CopyExpand allCollapse all
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
billing_agreement_id
The PayPal billing agreement ID. References an approved recurring payment for goods or services.

string [ 2 .. 128 ] characters ^[a-zA-Z0-9-]+$
The PayPal billing agreement ID. References an approved recurring payment for goods or services.

Copy
"string"
billing_cycle
The billing cycle providing details of the billing frequency, amount, duration and if the billing cycle is a free, discounted or regular billing cycle. The sequence of the billing cycle will be in the following order - free trial billing cycle(s), discounted trial billing cycle(s), regular billing cycle(s).

tenure_type
required
string [ 1 .. 24 ] characters
The tenure type of the billing cycle identifies if the billing cycle is a trial(free or discounted) or regular billing cycle.

Enum Value	Description
REGULAR	A regular billing cycle to identify recurring charges for the billing agreement.
TRIAL	A trial billing cycle to identify free or discounted charge for the billing agreement. Free trails will not have a price object in pricing scheme where as a discounted trial would have a discounted price compared to regular billing cycle.
total_cycles	
integer [ 0 .. 999 ]
Default: 1
The number of times this billing cycle gets executed. Trial billing cycles can only be executed a finite number of times (value between 1 and 999 for total_cycles). Regular billing cycles can be executed infinite times (value of 0 for total_cycles) or a finite number of times (value between 1 and 999 for total_cycles).

sequence	
integer [ 1 .. 3 ]
Default: 1
The order in which this cycle is to run among other billing cycles. For example, a trial billing cycle has a sequence of 1 while a regular billing cycle has a sequence of 2, so that trial cycle runs before the regular cycle.

pricing_scheme	
object
The active pricing scheme for this billing cycle. A free trial billing cycle does not require a pricing scheme.

start_date	
string = 10 characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The start date for the billing cycle, in YYYY-MM-DD. This field should be not be provided if the billing cycle starts at the time of checkout. When this field is not provided, the billing cycle amount will be included in any data validations confirming that the total provided by the merchant match the sum of individual items due at the time of checkout. Only one billing cycle (with sequence equal to 1) can have a no start date.

CopyExpand allCollapse all
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
bin_details
Bank Identification Number (BIN) details used to fund a payment.

bin	
string [ 1 .. 25 ] characters
The Bank Identification Number (BIN) signifies the number that is being used to identify the granular level details (except the PII information) of the card.

issuing_bank	
string [ 1 .. 64 ] characters
The issuer of the card instrument.

products	
Array of strings [ 1 .. 256 ] items
The type of card product assigned to the BIN by the issuer. These values are defined by the issuer and may change over time. Some examples include: PREPAID_GIFT, CONSUMER, CORPORATE.

bin_country_code	
string = 2 characters ^([A-Z]{2}|C2)$
The two-character ISO-3166-1 country code of the bank.

CopyExpand allCollapse all
{
"bin": "string",
"issuing_bank": "string",
"products": [
"string"
],
"bin_country_code": "string"
}
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
callback_configuration
CallBack Configuration that the merchant can provide to PayPal/Venmo.

callback_events
required
Array of strings [ 1 .. 5 ] items unique
An array of callback events merchant can subscribe to for the corresponding callback url.

callback_url
required
string [ 10 .. 2040 ] characters ^.*$
Merchant provided CallBack url.PayPal/Venmo will use this url to call the merchant back when the events occur .PayPal/Venmo expects a secured url usually in the https format.merchant can append the cart id or other params part of the url as query or path params.

CopyExpand allCollapse all
{
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
capture
A captured payment.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

status	
string
The status of the captured payment.

Enum Value	Description
COMPLETED	The funds for this captured payment were credited to the payee's PayPal account.
DECLINED	The funds could not be captured.
PARTIALLY_REFUNDED	An amount less than this captured payment's amount was partially refunded to the payer.
PENDING	The funds for this captured payment was not yet credited to the payee's PayPal account. For more information, see status.details.
REFUNDED	An amount greater than or equal to this captured payment's amount was refunded to the payer.
FAILED	There was an error while capturing payment.
status_details	
object
The details of the captured payment status.

id	
string
The PayPal-generated ID for the captured payment.

invoice_id	
string
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

custom_id	
string <= 255 characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

final_capture	
boolean
Default: false
Indicates whether you can make additional captures against the authorized payment. Set to true if you do not intend to capture additional payments against the authorization. Set to false if you intend to capture additional payments against the authorization.

links	
Array of objects
An array of related HATEOAS links.

amount	
object
The amount for this captured payment.

network_transaction_reference	
object
Reference values used by the card network to identify a transaction.

seller_protection	
object
The level of protection offered as defined by PayPal Seller Protection for Merchants.

seller_receivable_breakdown	
object
The detailed breakdown of the capture activity. This is not available for transactions that are in pending state.

disbursement_mode	
string [ 1 .. 16 ] characters ^[A-Z_]+$
Default: "INSTANT"
The funds that are held on behalf of the merchant.

Enum Value	Description
INSTANT	The funds are released to the merchant immediately.
DELAYED	The funds are held for a finite number of days. The actual duration depends on the region and type of integration. You can release the funds through a referenced payout. Otherwise, the funds disbursed automatically after the specified duration.
processor_response	
object
An object that provides additional processor information for a direct credit card transaction.

CopyExpand allCollapse all
{
"create_time": "string",
"update_time": "string",
"status": "COMPLETED",
"status_details": {
"reason": "BUYER_COMPLAINT"
},
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
Capture Identifier
The capture identification-related fields. Includes the invoice ID, custom ID, note to payer, and soft descriptor.

invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

note_to_payer	
string [ 1 .. 255 ] characters
An informational note about this settlement. Appears in both the payer's transaction history and the emails that the payer receives.

Copy
{
"invoice_id": "string",
"note_to_payer": "string"
}
Capture Request
Captures either a portion or the full authorized amount of an authorized payment.

invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

note_to_payer	
string [ 1 .. 255 ] characters
An informational note about this settlement. Appears in both the payer's transaction history and the emails that the payer receives.

final_capture	
boolean
Default: false
Indicates whether you can make additional captures against the authorized payment. Set to true if you do not intend to capture additional payments against the authorization. Set to false if you intend to capture additional payments against the authorization.

payment_instruction	
object
Any additional payment instructions to be consider during payment processing. This processing instruction is applicable for Capturing an order or Authorizing an Order.

soft_descriptor	
string <= 22 characters
The payment descriptor on the payer's account statement.

amount	
object
The amount to capture. To capture a portion of the full authorized amount, specify an amount. If amount is not specified, the full authorized amount is captured. The amount must be a positive number and in the same currency as the authorization against which the payment is being captured.

CopyExpand allCollapse all
{
"invoice_id": "string",
"note_to_payer": "string",
"final_capture": false,
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
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
},
"soft_descriptor": "string",
"amount": {
"currency_code": "str",
"value": "string"
}
}
capture_status
The status and status details of a captured payment.

status	
string
The status of the captured payment.

Enum Value	Description
COMPLETED	The funds for this captured payment were credited to the payee's PayPal account.
DECLINED	The funds could not be captured.
PARTIALLY_REFUNDED	An amount less than this captured payment's amount was partially refunded to the payer.
PENDING	The funds for this captured payment was not yet credited to the payee's PayPal account. For more information, see status.details.
REFUNDED	An amount greater than or equal to this captured payment's amount was refunded to the payer.
FAILED	There was an error while capturing payment.
status_details	
object
The details of the captured payment status.

CopyExpand allCollapse all
{
"status": "COMPLETED",
"status_details": {
"reason": "BUYER_COMPLAINT"
}
}
capture_status_details
The details of the captured payment status.

reason	
string [ 1 .. 64 ] characters ^[A-Z_]+$
The reason why the captured payment status is PENDING or DENIED.

Enum Value	Description
BUYER_COMPLAINT	The payer initiated a dispute for this captured payment with PayPal.
CHARGEBACK	The captured funds were reversed in response to the payer disputing this captured payment with the issuer of the financial instrument used to pay for this captured payment.
ECHECK	The payer paid by an eCheck that has not yet cleared.
INTERNATIONAL_WITHDRAWAL	Visit your online account. In your Account Overview, accept and deny this payment.
OTHER	No additional specific reason can be provided. For more information about this captured payment, visit your account online or contact PayPal.
PENDING_REVIEW	The captured payment is pending manual review.
RECEIVING_PREFERENCE_MANDATES_MANUAL_ACTION	The payee has not yet set up appropriate receiving preferences for their account. For more information about how to accept or deny this payment, visit your account online. This reason is typically offered in scenarios such as when the currency of the captured payment is different from the primary holding currency of the payee.
REFUNDED	The captured funds were refunded.
TRANSACTION_APPROVED_AWAITING_FUNDING	The payer must send the funds for this captured payment. This code generally appears for manual EFTs.
UNILATERAL	The payee does not have a PayPal account.
VERIFICATION_REQUIRED	The payee's PayPal account is not verified.
DECLINED_BY_RISK_FRAUD_FILTERS	Risk Filter set by the payee failed for the transaction.
Copy
{
"reason": "BUYER_COMPLAINT"
}
Captured Payment
A captured payment.

create_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction occurred, in Internet date and time format.

update_time	
string [ 20 .. 64 ] characters ^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[1-2][0-9]|...Show pattern
The date and time when the transaction was last updated, in Internet date and time format.

status	
string
The status of the captured payment.

Enum Value	Description
COMPLETED	The funds for this captured payment were credited to the payee's PayPal account.
DECLINED	The funds could not be captured.
PARTIALLY_REFUNDED	An amount less than this captured payment's amount was partially refunded to the payer.
PENDING	The funds for this captured payment was not yet credited to the payee's PayPal account. For more information, see status.details.
REFUNDED	An amount greater than or equal to this captured payment's amount was refunded to the payer.
FAILED	There was an error while capturing payment.
status_details	
object
The details of the captured payment status.

id	
string
The PayPal-generated ID for the captured payment.

invoice_id	
string
The API caller-provided external invoice number for this order. Appears in both the payer's transaction history and the emails that the payer receives.

custom_id	
string <= 255 characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports.

final_capture	
boolean
Default: false
Indicates whether you can make additional captures against the authorized payment. Set to true if you do not intend to capture additional payments against the authorization. Set to false if you intend to capture additional payments against the authorization.

links	
Array of objects
An array of related HATEOAS links.

amount	
object
The amount for this captured payment.

network_transaction_reference	
object
Reference values used by the card network to identify a transaction.

seller_protection	
object
The level of protection offered as defined by PayPal Seller Protection for Merchants.

seller_receivable_breakdown	
object
The detailed breakdown of the capture activity. This is not available for transactions that are in pending state.

disbursement_mode	
string [ 1 .. 16 ] characters ^[A-Z_]+$
Default: "INSTANT"
The funds that are held on behalf of the merchant.

Enum Value	Description
INSTANT	The funds are released to the merchant immediately.
DELAYED	The funds are held for a finite number of days. The actual duration depends on the region and type of integration. You can release the funds through a referenced payout. Otherwise, the funds disbursed automatically after the specified duration.
processor_response	
object
An object that provides additional processor information for a direct credit card transaction.

supplementary_data	
object
An object that provides supplementary/additional data related to a payment transaction.

payee	
object
The details associated with the merchant for this transaction.

CopyExpand allCollapse all
{
"create_time": "string",
"update_time": "string",
"status": "COMPLETED",
"status_details": {
"reason": "BUYER_COMPLAINT"
},
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
},
"supplementary_data": {
"related_ids": {
"order_id": "string",
"authorization_id": "string",
"capture_id": "string"
}
},
"payee": {
"email_address": "string",
"merchant_id": "string"
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
The card expiration year and month, in Internet date format For example: 2028-04

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
The card expiration year and month, in Internet date format For example: 2028-04

card_type	
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
card_attributes_response
Additional attributes associated with the use of this card.

vault	
object
The details about a saved Card payment source.

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
}
}
card_brand
The card network or brand. Applies to credit, debit, gift, and payment cards.

string [ 1 .. 255 ] characters ^[A-Z_]+$
The card network or brand. Applies to credit, debit, gift, and payment cards.

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
"VISA"
card_customer
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
card_from_request
Representation of card details as received in the request.

last_digits	
string [ 2 .. 4 ] characters
The last digits of the payment card.

expiry	
string = 7 characters ^[0-9]{4}-(0[1-9]|1[0-2])$
The card expiration year and month, in Internet date format.

Copy
{
"last_digits": "stri",
"expiry": "string"
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
Common response fields
Common response fields for all payment methods.

can_be_vaulted	
boolean
Default: "false"
Indicates if the payment method can be vaulted or not. A true value indicates the payment method can be vaulted using our vaults product. If false, vaulting is not currently supported for this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
Returns a country_code which is derived from the buyer's country.

product_code	
string [ 1 .. 64 ] characters ^[A-Z0-9_]+$
Returns a product code.

Enum Value	Description
CREDIT	Open ended credit products.
PAYLATER	Pay Later suite of products.
PAY_IN_3	Pay In 3 suite of products.
PAY_IN_4	Pay In 4 suite of products.
Copy
{
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT"
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
Credit Button Eligibility Button Code
The button code corresponding to a particular product or set of products. The values followed are defined by the SDK team.

string [ 1 .. 64 ] characters ^[A-Z0-9_]+$
The button code corresponding to a particular product or set of products. The values followed are defined by the SDK team.

Enum Value	Description
CREDIT	Open ended credit products.
PAYLATER	Pay Later suite of products.
PAY_IN_3	Pay In 3 suite of products.
PAY_IN_4	Pay In 4 suite of products.
Copy
"CREDIT"
cryptocurrency_symbol
The cryptocurrency symbol or code ticker options. Assigned by liquidity providers and exchanges.

string [ 1 .. 10 ] characters ^[0-9A-Za-z]{1,10}$
The cryptocurrency symbol or code ticker options. Assigned by liquidity providers and exchanges.

Enum Value	Description
BTC	The ticker symbol for Bitcoin. https://en.wikipedia.org/wiki/Bitcoin.
ETH	The ticker symbol for Ethereum. https://en.wikipedia.org/wiki/Ethereum.
BCH	The ticker symbol for Bitcoin Cash. https://en.wikipedia.org/wiki/Bitcoin_Cash.
LTC	The ticker symbol for Litecoin. https://en.wikipedia.org/wiki/Litecoin.
Copy
"BTC"
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
Customer
Customer who is making a purchase from the merchant/partner.

id	
string [ 1 .. 36 ] characters
The unique ID for a customer in merchant's or partner's system of records.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
Country from which the customer is purchasing products/services from the merchant/partner. Result will factor in local payment methods that are available in the country mentioned. Value should be in the 2-character ISO 3166-1 code format.

channel	
object
Channel through which the request is being posted. This object intends to collect information such as browser, operating system, device of the buyer. If both channel and User-Agent header are passed, data passed in the channel object takes precedence and User-Agent header will be used for information not provided in channel object.

email	
string [ 3 .. 254 ] characters ^(?:[A-Za-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[A-Za...Show pattern
The email address of the customer.

phone	
object
The phone number of the customer.

CopyExpand allCollapse all
{
"id": "string",
"country_code": "string",
"channel": {
"browser_type": "string",
"client_os": "string",
"device_type": "string"
},
"email": "string",
"phone": {
"country_code": "str",
"national_number": "string"
}
}
Customer Channel
Channel through which the request is being posted.

browser_type	
string [ 1 .. 30 ] characters
The browser used by the customer. Example: Safari, Chrome, etc.

client_os	
string [ 1 .. 30 ] characters
The operating system on the device used by the customer. Example: iOS 16.5, Android 30, etc.

device_type	
string [ 1 .. 30 ] characters
The type of device used by the customer. Example: Mobile, Desktop, Tablet, etc.

Copy
{
"browser_type": "string",
"client_os": "string",
"device_type": "string"
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
eligibility_purchase_unit_request
Purchase unit for payment eligibility.

amount	
object
The total order amount with an optional breakdown that provides details, such as the total item amount, total tax amount, shipping, handling, insurance, and discounts, if any.
If you specify amount.breakdown, the amount equals item_total plus tax_total plus shipping plus handling plus insurance minus shipping_discount minus discount.
The amount must be a positive number. For listed of supported currencies and decimal precision, see the PayPal REST APIs Currency Codes.

payee	
object
The merchant who receives payment for this transaction.

CopyExpand allCollapse all
{
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
}
}
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
item
The details for the items to be purchased.

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
}
}
language
The language tag for the language in which to localize the error-related strings, such as messages, issues, and suggested actions. The tag is made up of the ISO 639-2 language code, the optional ISO-15924 script tag, and the ISO-3166 alpha-2 country code or M49 region code.

string (language) [ 2 .. 10 ] characters ^[a-z]{2}(?:-[A-Z][a-z]{3})?(?:-(?:[A-Z]{2}|[...Show pattern
The language tag for the language in which to localize the error-related strings, such as messages, issues, and suggested actions. The tag is made up of the ISO 639-2 language code, the optional ISO-15924 script tag, and the ISO-3166 alpha-2 country code or M49 region code.

Copy
"string"
level_2
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
merchant_partner_customer_id
The unique ID for a customer generated by PayPal.

string [ 1 .. 22 ] characters ^[0-9a-zA-Z_-]+$
The unique ID for a customer generated by PayPal.

Copy
"string"
MerchantCommonComponentsSpecification-v1-schema-common_components-v4-schema-json-openapi-2.0-currency_code.json
The 3-character ISO-4217 currency code that identifies the currency.

string (MerchantCommonComponentsSpecification-v1-schema-common_components-v4-schema-json-openapi-2.0-currency_code.json) = 3 characters
The 3-character ISO-4217 currency code that identifies the currency.

Copy
"str"
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
The date that the transaction was authorized by the scheme. This field may not be returned for all networks. MasterCard refers to this field as "BankNet reference date.

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
The date that the transaction was authorized by the scheme. This field may not be returned for all networks. MasterCard refers to this field as "BankNet reference date.

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
payer_base
The customer who approves and pays for the order. The customer is also known as the payer.

email_address	
string [ 3 .. 254 ] characters (?:[a-zA-Z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-zA-...Show pattern
The email address of the payer.

Copy
{
"email_address": "string"
}
Payment Methods
List of payment methods.

paypal	
object
PayPal payment method.

venmo	
object
Venmo payment method.

paypal_credit	
object
PayPal Credit payment method.

paypal_pay_later	
object
PayPal Pay Later payment method.

CopyExpand allCollapse all
{
"paypal": {
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT",
"eligible_in_paypal_network": true,
"recommended": "false",
"recommended_priority": 1
},
"venmo": {
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT",
"eligible_in_paypal_network": true,
"recommended": "false",
"recommended_priority": 1
},
"paypal_credit": {
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT"
},
"paypal_pay_later": {
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT"
}
}
Payment Supplementary Data
The supplementary data.

related_ids	
object
Identifiers related to a specific resource.

CopyExpand allCollapse all
{
"related_ids": {
"order_id": "string",
"authorization_id": "string",
"capture_id": "string"
}
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
"payee_receivable_fx_rate_id": "string",
"disbursement_mode": "INSTANT"
}
payment_instruction
Any additional payments instructions during refund payment processing. This object is only applicable to merchants that have been enabled for PayPal Commerce Platform for Marketplaces and Platforms capability. Please speak to your account manager if you want to use this capability.

platform_fees	
Array of objects [ 0 .. 1 ] items
Specifies the amount that the API caller will contribute to the refund being processed. The amount needs to be lower than platform_fees amount originally captured or the amount that is remaining if multiple refunds have been processed. This field is only applicable to merchants that have been enabled for PayPal Commerce Platform for Marketplaces and Platforms capability. Please speak to your account manager if you want to use this capability.

CopyExpand allCollapse all
{
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
}
payment_method
Set of unique payment methods.

string [ 1 .. 30 ] characters ^[A-Z0-9_]+$
Set of unique payment methods.

Enum Value	Description
PAYPAL	PAYPAL
VENMO	VENMO
PAYPAL_CREDIT	PAYPAL_CREDIT
PAYPAL_PAY_LATER	PAYPAL_PAY_LATER
Copy
"PAYPAL"
payment_token
Vaulted instrument for a payment-method.

id	
string [ 7 .. 36 ] characters
The PayPal-generated ID for the vault token.

payment_source	
object
The vaulted payment method details.

links	
Array of objects [ 1 .. 32 ] items
An array of related HATEOAS links.

CopyExpand allCollapse all
{
"id": "strings",
"payment_source": {
"venmo": {
"user_name": "string"
},
"paypal": {
"email_address": "string"
}
},
"links": [
{
"href": "string",
"rel": "string",
"method": "GET"
}
]
}
PayPal Account Identifier
The account identifier for a PayPal account.

string (PayPal Account Identifier) = 13 characters ^[2-9A-HJ-NP-Z]{13}$
The account identifier for a PayPal account.

Copy
"stringstrings"
PayPal services member indicator
Flag that indicates if the customer is in the PayPal network. This value will be included in the response if the include_account_details flag is set to "true" in the API request.

boolean
Flag that indicates if the customer is in the PayPal network. This value will be included in the response if the include_account_details flag is set to "true" in the API request.

Copy
true
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
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
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

country_code
required
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
Phone
The phone number in its canonical international E.164 numbering plan format.

country_code
required
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
Phone
The phone number in its canonical international E.164 numbering plan format.

country_code
required
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
Phone
The phone number, in its canonical international E.164 numbering plan format.

country_code
required
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

Enum Value	Description
FAX	Fax number.
HOME	Home phone number.
MOBILE	Mobile phone number.
OTHER	Other phone number.
PAGER	Pager number.
Copy
"FAX"
phone_with_type
The phone information.

phone_type	
string
The phone type.

Enum Value	Description
FAX	Fax number.
HOME	Home phone number.
MOBILE	Mobile phone number.
OTHER	Other phone number.
PAGER	Pager number.
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
platform_fee
The platform or partner fee, commission, or brokerage fee that is associated with the transaction. Not a separate or isolated transaction leg from the external perspective. The platform fee is limited in scope and is always associated with the original payment for the purchase unit.

amount
required
object
The fee for this transaction.

payee	
object
The recipient of the fee for this transaction. If you omit this value, the default is the API caller.

CopyExpand allCollapse all
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
platform_fee
The platform or partner fee, commission, or brokerage fee that is associated with the transaction. Not a separate or isolated transaction leg from the external perspective. The platform fee is limited in scope and is always associated with the original payment for the purchase unit.

amount
required
object
The fee for this transaction.

CopyExpand allCollapse all
{
"amount": {
"currency_code": "str",
"value": "string"
}
}
Point of sale
The API caller-provided information about the store.

store_id	
string [ 1 .. 50 ] characters
The API caller-provided external store identification number.

terminal_id	
string [ 1 .. 50 ] characters
The API caller-provided external terminal identification number.

Copy
{
"store_id": "string",
"terminal_id": "string"
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
Preferences
Preferences of merchant/partner consuming the API.

payment_flow	
string [ 1 .. 64 ] characters ^[A-Z0-9_]+$
This field specifies the payment flow, expected to provide a hint about which payment action the customer is intending to perform.

Enum Value	Description
ONE_TIME_PAYMENT	Indicates that a customer is attempting to acquire a product within the context of a purchase. Most often, this will be the standard PayPal Checkout experience.
RECURRING_PAYMENT	Indicates that the customer is in the recurring payment experience.
VAULT_WITH_PAYMENT	Indicates that the customer is entering through checkout and continue to the vaulting experience.
VAULT_WITHOUT_PAYMENT	Indicates that the customer is in the vaulting experience without any purchase.
include_account_details	
boolean
Default: "false"
If this value is set to true, response will include confirmation if the customer has PayPal and/or Venmo accounts if they are eligible payment methods. Value defaults to false.

include_vault_tokens	
boolean
Default: "false"
If this value is set to true, response will include vaulted token information if the eligible funding source has any instrument vaulted for the customer. Value defaults to false.

payment_source_constraint	
object
Payment source constraint defines the payment methods that needs to be included/excluded for eligibility assessment. If not passed, all payment methods will be assessed for eligibility.

CopyExpand allCollapse all
{
"payment_flow": "ONE_TIME_PAYMENT",
"include_account_details": "false",
"include_vault_tokens": "false",
"payment_source_constraint": {
"constraint_type": "INCLUDE",
"payment_sources": [
"PAYPAL"
]
}
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
qr_details
QR details received from processor.

qr_image	
string [ 1 .. 20000 ] characters
QR image received from APM processor, The pattern is not provided because the value is defined by an external party.

qr_payload	
string [ 1 .. 520 ] characters
QR payload received from APM processor, The pattern is not provided because the value is defined by an external party.

Copy
{
"qr_image": "string",
"qr_payload": "string"
}
Reauthorize Request
Reauthorizes an authorized PayPal account payment, by ID. To ensure that funds are still available, reauthorize a payment after its initial three-day honor period expires. You can reauthorize a payment only once from days four to 29.

If 30 days have transpired since the date of the original authorization, you must create an authorized payment instead of reauthorizing the original authorized payment.

A reauthorized payment itself has a new honor period of three days.

You can reauthorize an authorized payment once. The allowed amount depends on context and geography, for example in US it is up to 115% of the original authorized amount, not to exceed an increase of $75 USD.

Supports only the amount request parameter.

amount	
object
The amount to reauthorize for an authorized payment.

CopyExpand allCollapse all
{
"amount": {
"currency_code": "str",
"value": "string"
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
Refund Request
Refunds a captured payment, by ID. For a full refund, include an empty request body. For a partial refund, include an amount object in the request body.

custom_id	
string [ 1 .. 127 ] characters
The API caller-provided external ID. Used to reconcile API caller-initiated transactions with PayPal transactions. Appears in transaction and settlement reports. The pattern is defined by an external party and supports Unicode.

invoice_id	
string [ 1 .. 127 ] characters
The API caller-provided external invoice ID for this order. The pattern is defined by an external party and supports Unicode.

note_to_payer	
string [ 1 .. 255 ] characters
The reason for the refund. Appears in both the payer's transaction history and the emails that the payer receives. The pattern is defined by an external party and supports Unicode.

amount	
object
The amount to refund. To refund a portion of the captured amount, specify an amount. If amount is not specified, an amount equal to captured amount - previous refunds is refunded. The amount must be a positive number and in the same currency as the one in which the payment was captured.

payment_instruction	
object
Any additional refund instructions to be set during refund payment processing. This object is only applicable to merchants that have been enabled for PayPal Commerce Platform for Marketplaces and Platforms capability. Please speak to your account manager if you want to use this capability.

CopyExpand allCollapse all
{
"custom_id": "string",
"invoice_id": "string",
"note_to_payer": "string",
"amount": {
"currency_code": "str",
"value": "string"
},
"payment_instruction": {
"platform_fees": [
{
"amount": {
"currency_code": "str",
"value": "string"
}
}
]
}
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
Related Identifiers
Identifiers related to a specific resource.

order_id	
string [ 1 .. 20 ] characters
Order ID related to the resource.

authorization_id	
string [ 1 .. 20 ] characters
Authorization ID related to the resource.

capture_id	
string [ 1 .. 20 ] characters
Capture ID related to the resource.

Copy
{
"order_id": "string",
"authorization_id": "string",
"capture_id": "string"
}
Response for PayPal
Response for PayPal.

can_be_vaulted	
boolean
Default: "false"
Indicates if the payment method can be vaulted or not. A true value indicates the payment method can be vaulted using our vaults product. If false, vaulting is not currently supported for this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
Returns a country_code which is derived from the buyer's country.

product_code	
string [ 1 .. 64 ] characters ^[A-Z0-9_]+$
Returns a product code.

Enum Value	Description
CREDIT	Open ended credit products.
PAYLATER	Pay Later suite of products.
PAY_IN_3	Pay In 3 suite of products.
PAY_IN_4	Pay In 4 suite of products.
eligible_in_paypal_network	
boolean
Flag that indicates if the customer is in the PayPal network. This value will be included in the response if the include_account_details flag is set to "true" in the API request.

recommended	
boolean
Default: "false"
Indicates if the payment method is recommended or not. A true value indicates the customer is payment ready and this payment method may be presented upfront.

recommended_priority	
integer [ 1 .. 3 ]
This value is included in the response when recommended is true for a payment method. It indicates the priority of recommendation for payment readiness of eligible payment methods with lowest number taking the highest precedence.

Copy
{
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT",
"eligible_in_paypal_network": true,
"recommended": "false",
"recommended_priority": 1
}
Response for Venmo
Response for Venmo.

can_be_vaulted	
boolean
Default: "false"
Indicates if the payment method can be vaulted or not. A true value indicates the payment method can be vaulted using our vaults product. If false, vaulting is not currently supported for this payment method.

country_code	
string = 2 characters ^([A-Z]{2}|C2)$
Returns a country_code which is derived from the buyer's country.

product_code	
string [ 1 .. 64 ] characters ^[A-Z0-9_]+$
Returns a product code.

Enum Value	Description
CREDIT	Open ended credit products.
PAYLATER	Pay Later suite of products.
PAY_IN_3	Pay In 3 suite of products.
PAY_IN_4	Pay In 4 suite of products.
eligible_in_paypal_network	
boolean
Flag that indicates if the customer is in the PayPal network. This value will be included in the response if the include_account_details flag is set to "true" in the API request.

recommended	
boolean
Default: "false"
Indicates if the payment method is recommended or not. A true value indicates the customer is payment ready and this payment method may be presented upfront.

recommended_priority	
integer [ 1 .. 3 ]
This value is included in the response when recommended is true for a payment method. It indicates the priority of recommendation for payment readiness of eligible payment methods with lowest number taking the highest precedence.

Copy
{
"can_be_vaulted": "false",
"country_code": "st",
"product_code": "CREDIT",
"eligible_in_paypal_network": true,
"recommended": "false",
"recommended_priority": 1
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
signature_verification_status
Transaction signature status identifier.

string [ 1 .. 50 ] characters ^[A-Z]+$
Transaction signature status identifier.

Enum Value	Description
YES	Indicates that the signature of the PARes has been validated successfully and the message contents can be trusted.
NO	Indicates that the PARes could not be validated.
Copy
"YES"
single_use_token
The PayPal-generated, short-lived, one-time-use token, used to communicate payment information to PayPal for transaction processing.

string [ 1 .. 255 ] characters ^[0-9a-zA-Z_-]+$
The PayPal-generated, short-lived, one-time-use token, used to communicate payment information to PayPal for transaction processing.

Copy
"string"
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
Subscription period
Describes a period in a subscription. A period is a chunk of time for which a set of subscription terms are applicable.

current_period	
boolean
Indicates if this period, with its subscription terms, is currently active.

billing_interval_unit	
string [ 1 .. 24 ] characters
The billing interval unit for the subscription period.

Enum Value	Description
YEAR	The billing interval for the subscription period is a year.
MONTH	The billing interval for the subscription period is a month.
SEMIMONTH	The billing interval for the subscription period is half a month.
WEEK	The billing interval for the subscription period is a week.
DAY	The billing interval for the subscription period is a day.
billing_frequency	
integer [ 1 .. 365 ]
Default: 1
The frequency, i.e. the number of billing interval units, at which the subscriber is charged. E.g. - if the billing_interval_unit is DAY, with a billing_frequency of 2, the subscriber is billed once every two days.

total_billing_cycles	
integer [ 0 .. 999 ]
Default: 1
The number of billing cycles in the period. This is effectively the number of times the subscriber will be charged in the period.

current_billing_cycle	
integer [ 0 .. 999 ]
Indicates the index of the current billing cycle for the period. E.g. - a value of 3 would mean that its the third billing cycle for the period.

regular_period	
boolean
Indicates if the period is a regular period. A regular period would have standard subscription terms, as opposed to some special terms that might be applicable for a trial or promotional period.

billing_amount	
object
The amount that the subscriber will be charged in each billing cycle.

shipping_amount	
object
The shipping charges applicable for each cycle.

tax_amount	
object
The tax amount applicable for each cycle.

CopyExpand allCollapse all
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
three_d_secure_authentication_response
Results of 3D Secure Authentication.

authentication_status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
The outcome of the issuer's authentication.

Enum Value	Description
Y	Successful authentication.
N	Failed authentication / account not verified / transaction denied.
U	Unable to complete authentication.
A	Successful attempts transaction.
C	Challenge required for authentication.
R	Authentication rejected (merchant must not submit for authorization).
D	Challenge required; decoupled authentication confirmed.
I	Informational only; 3DS requestor challenge preference acknowledged.
enrollment_status	
string [ 1 .. 255 ] characters ^[0-9A-Z_]+$
Status of authentication eligibility.

Enum Value	Description
Y	Yes. The bank is participating in 3-D Secure protocol and will return the ACSUrl.
N	No. The bank is not participating in 3-D Secure protocol.
U	Unavailable. The DS or ACS is not available for authentication at the time of the request.
B	Bypass. The merchant authentication rule is triggered to bypass authentication.
Copy
{
"authentication_status": "Y",
"enrollment_status": "Y"
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
Create multiple payment tokens for the same payer, merchant/platform combination. Use this when the customer has not logged in at merchant/platform. The payment token thus generated, can then also be used to create the customer account at merchant/platform. Use this also when multiple payment tokens are required for the same payer, different customer at merchant/platform. This helps to identify customers distinctly even though they may share the same Venmo account.

Copy
{
"store_in_vault": "ON_SUCCESS",
"description": "string",
"usage_pattern": "IMMEDIATE",
"usage_type": "MERCHANT",
"customer_type": "CONSUMER",
"permit_multiple_payment_tokens": false
}
venmo_payment_token
Payment Token info for Venmo payment source.

user_name	
string [ 1 .. 50 ] characters
The Venmo username, as chosen by the user.

Copy
{
"user_name": "string"
}
venmo_wallet_attributes
Additional attributes associated with the use of this Venmo Wallet.

customer	
object
This object represents a merchant’s customer, allowing them to store contact details, and track all payments associated with the same customer.

vault	
object
Attributes used to provide the instructions during vaulting of the Venmo Wallet.

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
venmo_wallet_experience_context
Customizes the buyer experience during the approval process for payment with Venmo.

Note: Partners and Marketplaces might configure shipping_preference during partner account setup, which overrides the request values.
brand_name	
string [ 1 .. 127 ] characters
The business name of the merchant. The pattern is defined by an external party and supports Unicode.

shipping_preference	
string [ 1 .. 24 ] characters
Default: "GET_FROM_FILE"
The location from which the shipping address is derived.

Enum Value	Description
GET_FROM_FILE	Get the customer-provided shipping address on the PayPal site.
NO_SHIPPING	Redacts the shipping address from the PayPal site. Recommended for digital goods.
SET_PROVIDED_ADDRESS	Obtenha o endereço fornecido pelo comerciante. O cliente não pode alterar esse endereço no site do PayPal. Se o comerciante não fornecer um endereço, o cliente poderá escolher o endereço nas páginas do PayPal.

order_update_callback_config
objeto
O comerciante forneceu a configuração de retorno de chamada de atualização de pedido para a carteira Venmo. O Venmo retornará a ligação para o comerciante quando o evento especificado ocorrer. Recomendamos que os comerciantes passem os eventos de retorno de chamada `shipping_options` e `shipping_address`. Não é compatível quando shipping.type`shipping_address` é especificado ou quando `application_context.shipping_preference` está definido como `NO_SHIPPING` ou `SET_PROVIDED_ADDRESS`.

CópiaExpandir tudoRecolher tudo
{
"brand_name": "string",
"shipping_preference": "GET_FROM_FILE",
"order_update_callback_config": {
"callback_events": [
"string"
],
"callback_url": "http://example.com"
}
}
Referência
PayPal.com
Privacidade
Apoiar
Jurídico
Contato
