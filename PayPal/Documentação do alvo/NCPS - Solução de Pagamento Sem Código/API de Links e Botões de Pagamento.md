Crie e gerencie recursos de pagamento personalizáveis ​​usando a API de Links e Botões de Pagamento. Um link de pagamento é um URL compartilhável que leva os clientes a uma página de pagamento hospedada pelo PayPal, onde eles podem concluir compras com segurança. Cada link de pagamento é exclusivo para um produto, mas pode ser compartilhado e reutilizado várias vezes. Os clientes que abrem um link de pagamento visualizam uma página de finalização de compra hospedada pelo PayPal, exibindo detalhes do produto, como nome, preço, descrição, variantes, taxas de envio e impostos.
A API de Links e Botões de Pagamento ajuda você a:
Aceite pagamentos através de links hospedados pelo PayPal.
Ofereça diversos métodos de pagamento, incluindo PayPal, Venmo, Pagar Depois, Apple Pay e os principais cartões de crédito e débito.
Veja as transações acontecerem em tempo real. O PayPal envia notificações por e-mail tanto para compradores quanto para vendedores.
Proteja suas vendas online elegíveis com a Proteção ao Vendedor do PayPal .
Segue abaixo uma lista de endpoints.
Caso de uso	Ponto final
Crie links e botões de pagamento.	POST /v1/checkout/payment-resources
Lista de links de pagamento	GET /v1/checkout/payment-resources
Obtenha os detalhes do link de pagamento	GET /v1/checkout/payment-resources/{id}
Atualizar link de pagamento	PUT /v1/checkout/payment-resources/{id}
Excluir link de pagamento	DELETE /v1/checkout/payment-resources/{id}
​
Elegibilidade
Esta API está disponível em todos os países onde o PayPal opera.
Consulte a seção Países e Moedas para obter uma lista das moedas aceitas.
​
Pré-requisitos
Crie uma conta comercial no PayPal .
Use o Painel de Desenvolvedores do PayPal para obter seu ID e segredo do cliente e, em seguida, integre seu backend usando o OAuth 2.0. Consulte Introdução às APIs REST do PayPal para obter mais informações.
Acesse a seção Aplicativos e Credenciais no Painel do Desenvolvedor do PayPal para garantir que a opção Links e Botões de Pagamento esteja marcada.
​
Como funciona
A API de Links e Botões de Pagamento ajuda os comerciantes a criar e gerenciar experiências de finalização de compra hospedadas pelo PayPal.
Obtenha um token de acesso OAuth usando as credenciais da sua conta PayPal Business.
Utilize a API para criar links de pagamento que definam itens, preços e URLs de retorno.
Compartilhe os links gerados diretamente ou incorpore-os em seu site. A API não oferece suporte a trechos de código de botão no momento, mas você pode criar seu próprio botão incorporável usando o link.
Os compradores selecionam o link para concluir o pagamento com segurança através da página de finalização de compra hospedada pelo PayPal. O pagamento é concluído em tempo real.
Tanto os vendedores quanto os compradores recebem uma notificação por e-mail para cada transação. Os vendedores podem visualizar todas as transações em sua conta PayPal Business. Os clientes do PayPal podem visualizar os detalhes da transação em suas contas PayPal.
Você pode recuperar, atualizar ou excluir links de pagamento existentes conforme necessário.







Query

Query

Start

Get Access Token
POST v1/oauth2/token

Create Resource
POST v1/checkout/payment-resources

Payment Resource
Status: ACTIVE

Update Resource
PUT v1/checkout/payment-resources/id

Delete Resource
DELETE v1/checkout/payment-resources/id

Resource Removed

List Resources
GET v1/checkout/payment-resources

Get Details
GET v1/checkout/payment-resources/id

​
Pontos finais
A tabela a seguir lista os endpoints disponíveis para gerenciar links de pagamento.
Caso de uso	Ponto final	Solicitar	Resposta
Criar links de pagamento	POST /v1/checkout/payment-resources	Inclua detalhes do produto, como nome, preço, descrição, variantes, impostos, informações de envio e URL de retorno.	Retorna 201 Createdo ID do link de pagamento, o URL do link compartilhável, o status e o registro de data e hora de criação.
Lista de links de pagamento	GET /v1/checkout/payment-resources	Nenhuma carga útil necessária. Use parâmetros de consulta opcionais, como page_sizepara resultados por página e page_tokenpara paginação.	Retorna 200 OKuma lista com links de pagamento e metadados de paginação.
Obtenha os detalhes do link de pagamento	GET /v1/checkout/payment-resources/{id}	Utilize o ID do link de pagamento no caminho da URL para obter os detalhes.	Devoluções 200 OKcom detalhes completos para o link de pagamento especificado.
Atualizar link de pagamento	PUT /v1/checkout/payment-resources/{id}	Utilize o ID do link de pagamento no caminho da URL. Inclua as informações atualizadas do produto no corpo da requisição.	Devoluções 200 OKcom os detalhes do link de pagamento atualizados.
Excluir link de pagamento	DELETE /v1/checkout/payment-resources/{id}	Utilize o ID do link de pagamento no caminho da URL para excluí-lo permanentemente.	Retorna 204 No Contentpara confirmar a exclusão bem-sucedida.
​
Campos do produto
Os links de pagamento utilizam informações e preços dos produtos para mostrar aos compradores o que sua empresa vende. Para criar um link de pagamento, crie um produto com as seguintes informações.
Campo	Descrição
name(Obrigatório)	Nome do produto ou serviço que está sendo vendido.
product_id	Identificador único do item, útil para rastreamento do comerciante e gerenciamento de estoque.
description	Texto explicativo sobre o produto ou serviço para facilitar a compreensão do cliente.
unit_amount(Obrigatório)	Quantidade fixa definida para o item.
taxes	Valor do imposto aplicado ao produto ou serviço.
shipping	Custo de envio aplicado ao produto ou serviço.
collect_shipping_address	Opção para exigir que os clientes forneçam um endereço de entrega no momento da finalização da compra.
customer_notes	Campo de entrada de texto com rótulo personalizável que captura informações do cliente no momento da finalização da compra, como instruções especiais e preferências de entrega. Este campo pode ser configurado como opcional ou obrigatório.
variants	Opções de produto como tamanho, cor ou material, com possíveis diferenças de preço. Suporta até 5 variantes, cada uma com até 10 opções. A primeira variante é classificada como principal e pode ter preços atribuídos a cada opção.
adjustable_quantity	O número máximo de unidades que podem ser adquiridas.
return_url	Redireciona os clientes para um URL especificado pelo comerciante após a conclusão do pagamento.
​
Casos de uso
Veja como criar links de pagamento que atendam a cenários e requisitos comuns.
​
POSTSolicitações de exemplo
A seguir, apresentamos diferentes exemplos de casos de uso da POSTAPI que demonstram como criar links de pagamento para vários cenários.
​
Crie um link de pagamento com um URL de retorno.
Crie um link de pagamento para um mouse sem fio com preço de US$ 39,99. Após a conclusão do pagamento, os clientes são redirecionados para o URL de retorno especificado, como por exemplo https://merchant.example.com/thank-you: .
curl -v -k -X POST 'https://api.sandbox.paypal.com/v1/checkout/payment-resources' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -d '{
    "type": "BUY_NOW",
    "integration_mode": "LINK",
    "reusable": "MULTIPLE",
    "return_url": "https://merchant.example.com/thank-you",
    "line_items": [
        {
            "name": "Wireless Mouse",
            "unit_amount": {
                "currency_code": "USD",
                "value": "39.99"
            }
        }
    ]
}'

Veja todas as 19 linhas
​
Criar um link de pagamento com variantes
Crie um link de pagamento para uma camiseta com preço de US$ 20,00. A Colordimensão principal está marcada como "primária" e inclui duas opções: Whitee Black. A Sizedimensão secundária inclui as opções 11oze 15oz. O preço base de US$ 20,00 se aplica a todas as combinações de variantes.
curl -v -k -X POST 'https://api.sandbox.paypal.com/v1/checkout/payment-resources' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -d '{
    "type": "BUY_NOW",
    "integration_mode": "LINK",
    "reusable": "MULTIPLE",
    "line_items": [
        {
            "name": "T-Shirt",
            "unit_amount": {
                "currency_code": "USD",
                "value": "20.00"
            },
            "variants": {
                "dimensions": [
                    {
                        "name": "Color",
                        "primary": true,
                        "options": [
                            {
                                "label": "White"
                            },
                            {
                                "label": "Black"
                            }
                        ]
                    },
                    {
                        "name": "Size",
                        "primary": false,
                        "options": [
                            {
                                "label": "11oz"
                            },
                            {
                                "label": "15oz"
                            }
                        ]
                    }
                ]
            }
        }
    ]
}'

Veja todas as 46 linhas
​
Crie um link de pagamento com preços por variante.
Crie um link de pagamento para uma camiseta com preços específicos para cada variante. A Colordimensão principal é marcada como primária e inclui duas opções: Whiteuma com preço de US$ 19,00 e outra Blackcom preço de US$ 20,00. A Sizedimensão secundária inclui as opções de cor 11oze 15oztamanho, sem diferença de preço. Nenhum valor base unit_amounté exibido no payload, pois cada variante de cor tem seu próprio preço definido.
curl -v -k -X POST 'https://api.sandbox.paypal.com/v1/checkout/payment-resources' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -d '{
    "integration_mode": "LINK",
    "type": "BUY_NOW",
    "reusable": "MULTIPLE",
    "line_items": [
        {
            "name": "T-Shirt",
            "variants": {
                "dimensions": [
                    {
                        "name": "Color",
                        "primary": true,
                        "options": [
                            {
                                "label": "White",
                                "unit_amount": {
                                    "currency_code": "USD",
                                    "value": "19.00"
                                }
                            },
                            {
                                "label": "Black",
                                "unit_amount": {
                                    "currency_code": "USD",
                                    "value": "20.00"
                                }
                            }
                        ]
                    },
                    {
                        "name": "Size",
                        "primary": false,
                        "options": [
                            {
                                "label": "11oz"
                            },
                            {
                                "label": "15oz"
                            }
                        ]
                    }
                ]
            }
        }
    ]
}'

Veja todas as 50 linhas
​
Crie um link de pagamento com impostos e frete inclusos.
Crie um link de pagamento para uma caneca de café no valor de US$ 12,00 com os impostos e o frete calculados. Os taxesparâmetros ` shippingtax` e `shipping` especificam type: PREFERENCEas value: PROFILEconfigurações de impostos e frete predefinidas pelo lojista em sua conta PayPal. Para que esse recurso funcione, o lojista precisa configurar essas opções de impostos e frete em sua conta PayPal.
curl -v -k -X POST 'https://api.sandbox.paypal.com/v1/checkout/payment-resources' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -d '{
    "integration_mode": "LINK",
    "type": "BUY_NOW",
    "reusable": "MULTIPLE",
    "return_url": "https://merchant.example.com/thank-you",
    "line_items": [
        {
            "name": "Coffee Mug",
            "unit_amount": {
                "currency_code": "USD",
                "value": "12.00"
            },
            "taxes": [
                {
                    "name": "Sales Tax",
                    "type": "PREFERENCE",
                    "value": "PROFILE"
                }
            ],
            "shipping": [
                {
                    "type": "PREFERENCE",
                    "value": "PROFILE"
                }
            ]
        }
    ]
}'

Veja todas as 32 linhas
​
Criar links e botões de pagamento ( POST)
Crie um URL de pagamento "Comprar agora" que direcione diretamente para uma experiência de finalização de compra hospedada pelo PayPal. Use este endpoint para gerar URLs de pagamento compartilháveis ​​ou diversos fluxos de compra.
Ponto final:POST /v1/checkout/payment-resources
​
Solicitar
A seguinte solicitação de exemplo inclui estas configurações principais:
Cria um link de pagamento reutilizável para fones de ouvido sem fio premium com cancelamento de ruído.
Inclui preços específicos para cada variante nas quatro cores principais.
Inclui três variantes onde a dimensão da cor é marcada como primária e inclui quatro opções. As outras duas variantes são Warranty, que possui três opções, e Package Type, que possui duas opções. Essas variantes não afetam o preço.
Acrescenta um imposto de 8,25%, aplica uma taxa de envio fixa e exige que os clientes forneçam um endereço de entrega.
Oferece aos clientes a opção de adicionar uma mensagem de presente no momento da finalização da compra.
Define uma quantidade máxima de pedido de 10 unidades.
Redireciona os clientes para um URL de retorno após a transação.
{
  "integration_mode": "LINK",
  "type": "BUY_NOW",
  "reusable": "MULTIPLE",
  "return_url": "https://example-merchant.com/payment/return-success",
  "line_items": [
    {
      "name": "Premium Wireless Headphones with Noise Cancellation",
      "product_id": "PROD-WH-NC-2024-12345",
      "description": "Experience crystal-clear sound with our premium wireless headphones featuring active noise cancellation, 30-hour battery life",
      "taxes": [
        {
          "name": "State Sales Tax",
          "type": "PERCENTAGE",
          "value": "8.25"
        }
      ],
      "shipping": [
        {
          "type": "FLAT",
          "value": "12.50"
        }
      ],
      "collect_shipping_address": true,
      "customer_notes": [
        {
          "label": "Gift Message (Optional)",
          "required": false
        }
      ],
      "variants": {
        "dimensions": [
          {
            "name": "Color",
            "primary": true,
            "options": [
              {
                "label": "Midnight Black",
                "unit_amount": {
                  "currency_code": "USD",
                  "value": "149.99"
                }
              },
              {
                "label": "Silver Gray",
                "unit_amount": {
                  "currency_code": "USD",
                  "value": "149.99"
                }
              },
              {
                "label": "Rose Gold",
                "unit_amount": {
                  "currency_code": "USD",
                  "value": "169.99"
                }
              },
              {
                "label": "Navy Blue",
                "unit_amount": {
                  "currency_code": "USD",
                  "value": "159.99"
                }
              }
            ]
          },
          {
            "name": "Warranty",
            "primary": false,
            "options": [
              { "label": "Standard 1-Year" },
              { "label": "Extended 2-Year" },
              { "label": "Premium 3-Year" }
            ]
          },
          {
            "name": "Package Type",
            "primary": false,
            "options": [
              { "label": "Standard Box" },
              { "label": "Gift Wrapped" }
            ]
          }
        ]
      },
      "adjustable_quantity": {
        "maximum": 10
      }
    }
  ]
}

Veja todas as 91 linhas
​
Resposta
A resposta a seguir contém um link para um recurso de pagamento com preço fixo, com detalhes e variantes do produto. A API retorna o código de status HTTP 201 Createde o tipo de conteúdo application/json.
{
  "id": "PLB-8H2K9J3N5P7Q",
  "integration_mode": "LINK",
  "type": "BUY_NOW",
  "reusable": "MULTIPLE",
  "return_url": "https://example.com/return",
  "line_items": [
    {
      "name": "Premium Wireless Headphones",
      "product_id": "HEADPHONE-001",
      "description": "High-quality noise-canceling wireless headphones",
      "unit_amount": { "currency_code": "USD", "value": "199.99" },
      "taxes": [
        { "name": "Sales Tax", "amount": { "currency_code": "USD", "value": "16.00" } }
      ],
      "shipping": [
        { "name": "Standard Shipping", "amount": { "currency_code": "USD", "value": "5.99" } }
      ],
      "collect_shipping_address": true,
      "customer_notes": [{ "label": "Gift message", "required": false }],
      "variants": {
        "options": [
          { "name": "Color", "values": ["Black", "Silver", "White"] }
        ]
      },
      "adjustable_quantity": { "enabled": true, "min_quantity": 1, "max_quantity": 10 }
    }
  ],
  "status": "ACTIVE",
  "create_time": "2025-10-10T12:30:45Z",
  "payment_link": "https://www.paypal.com/paymentpage/PLB-8H2K9J3N5P7Q"
}

Veja todas as 32 linhas
​
Lista de links de pagamento ( GET)
Recupere uma lista paginada de todos os links de pagamento criados pelo comerciante. Este endpoint suporta filtragem e paginação por meio de parâmetros de consulta, incluindo filtragem por status do link e tags.
Ponto final:GET /v1/checkout/payment-resources
​
Solicitar
O seguinte exemplo de solicitação inclui estes detalhes principais:
Envia uma GETsolicitação para listar os links de pagamento que você criou.
Utiliza o page_sizeparâmetro para limitar os resultados. Por exemplo, definindo-o como page_size=2retornando dois links de pagamento por página.
Inclui um token OAuth 2.0 Bearer para autenticação.
Cabeçalhos definidos como Aceitar: application/jsone Content-Type: application/json.
Utiliza um page_tokenparâmetro da resposta para recuperar páginas adicionais.
curl -v -k -X GET 'https://api.sandbox.paypal.com/v1/checkout/payment-resources' \
  -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json'
​
Resposta
A seguinte resposta de exemplo inclui estes detalhes importantes:
Retorna uma matriz de recursos com dois links de pagamento.
O primeiro link de pagamento é para um produto Wireless Mouse, com preço de US$ 29,99. Este link está ativo, configurado para [inserir valor aqui] BUY_NOWe pode ser reutilizado.
O segundo link de pagamento é para um teclado sem fio, com preço de US$ 99,99. Ele tem o mesmo status, tipo e configurações de reutilização que o primeiro link.
Cada recurso inclui um payment_linkURL compartilhável, metadados como ID, nome do produto, preço, status, tipo e data e hora de criação, além de uma matriz de links com ações HATEOAS para recuperar, atualizar, editar, excluir ou acessar o link de pagamento.
A resposta também inclui uma matriz de links de nível superior para navegação, com um link "self" que faz referência à solicitação atual e um link "next" para page_tokenrecuperar resultados adicionais.
{
  "resources": [
    {
      "id": "PLB-X7MNK9P2QR8T",
      "integration_mode": "LINK",
      "create_time": "2025-11-29T13:13:25.832592Z",
      "status": "ACTIVE",
      "payment_link": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
      "type": "BUY_NOW",
      "reusable": "MULTIPLE",
      "line_items": [
        {
          "name": "Wireless Mouse",
          "unit_amount": {
            "currency_code": "USD",
            "value": "29.99"
          }
        }
      ],
      "links": [
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
          "rel": "self",
          "method": "GET"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
          "rel": "replace",
          "method": "PUT"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
          "rel": "edit",
          "method": "PATCH"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
          "rel": "delete",
          "method": "DELETE"
        },
        {
          "href": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
          "rel": "payment_link",
          "method": "GET"
        }
      ]
    },
    {
      "id": "PLB-P2QR8TX7MNK9",
      "integration_mode": "LINK",
      "create_time": "2025-11-29T13:13:25.832592Z",
      "status": "ACTIVE",
      "payment_link": "https://www.paypal.com/ncp/payment/PLB-P2QR8TX7MNK9",
      "type": "BUY_NOW",
      "reusable": "MULTIPLE",
      "line_items": [
        {
          "name": "Wireless Keyboard",
          "unit_amount": {
            "currency_code": "USD",
            "value": "99.99"
          }
        }
      ],
      "links": [
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-P2QR8TX7MNK9",
          "rel": "self",
          "method": "GET"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-P2QR8TX7MNK9",
          "rel": "replace",
          "method": "PUT"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-P2QR8TX7MNK9",
          "rel": "edit",
          "method": "PATCH"
        },
        {
          "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-P2QR8TX7MNK9",
          "rel": "delete",
          "method": "DELETE"
        },
        {
          "href": "https://www.paypal.com/ncp/payment/PLB-P2QR8TX7MNK9",
          "rel": "payment_link",
          "method": "GET"
        }
      ]
    }
  ],
  "links": [
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources?page_size=2",
      "rel": "self"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources?page_token=eyJleGNsdXNpdmVfc3RhcnRfa2V5Ijp7ImlkIjoiUExCLVAyUVI4VFg3TU5LOSJ9LCJwYWdlX3NpemUiOjJ9",
      "rel": "next"
    }
  ]
}

Veja todas as 104 linhas
​
Obtenha os detalhes do link de pagamento ( GET)
Obtenha os detalhes de um link de pagamento específico pelo seu ID exclusivo. Este endpoint retorna todos os metadados disponíveis, incluindo o status do pagamento, links, tipo reutilizável e detalhes dos itens da linha para o link de pagamento solicitado.
Ponto final:GET /v1/checkout/payment-resources/{id}
​
Solicitar
O seguinte exemplo de solicitação inclui estes detalhes principais:
Inclui os detalhes completos de um link de pagamento específico identificado pelo ID do recurso PLB-X7MNK9P2QR8Tpor meio de uma GETsolicitação.
Utiliza um token OAuth 2.0 Bearer no cabeçalho de Autorização para autenticar a solicitação.
Define os Acceptcabeçalhos Content-Typepara application/jsonformatação adequada.
curl -v -k -X GET 'https://api.sandbox.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json'
​
Resposta
A resposta a seguir contém os detalhes completos de um link de pagamento com preço fixo. A API retorna o código de status 200 OKe o tipo de conteúdo application/json.
A seguinte resposta de exemplo inclui estes detalhes importantes:
Retorna os detalhes completos do link de pagamento PLB-X7MNK9P2QR8T.
O link de pagamento está ativo, configurado BUY_NOWpara uso múltiplo e definido como válido.
Mostra um produto chamado Mouse Sem Fio, com preço de US$ 29,99.
O link foi criado no 2025-11-29T13:13:25.832592Zmodo de integração LINK.
Após o pagamento, os clientes são redirecionados para https://merchant.example.com/thank-youo endereço especificado por `return_url`.
Inclui um link de pagamento compartilhável em https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T.
Fornece uma matriz de links com ações HATEOAS: recuperar os detalhes do link ( self), atualizar o link ( replace), excluir o link ( delete) e acessar a página de pagamento voltada para o cliente ( payment_link).
{
  "id": "PLB-X7MNK9P2QR8T",
  "integration_mode": "LINK",
  "create_time": "2025-11-29T13:13:25.832592Z",
  "status": "ACTIVE",
  "payment_link": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
  "type": "BUY_NOW",
  "reusable": "MULTIPLE",
  "return_url": "https://merchant.example.com/thank-you",
  "line_items": [
    {
      "name": "Wireless Mouse",
      "unit_amount": {
        "currency_code": "USD",
        "value": "29.99"
      }
    }
  ],
  "links": [
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "self",
      "method": "GET"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "replace",
      "method": "PUT"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "delete",
      "method": "DELETE"
    },
    {
      "href": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
      "rel": "payment_link",
      "method": "GET"
    }
  ]
}

Veja todas as 41 linhas
​
Atualizar link de pagamento ( PUT)
Atualize um link de pagamento específico pelo seu identificador único. Este endpoint permite substituir os detalhes do produto e do checkout de um link de pagamento com preço fixo por novos dados de configuração.
Ponto final:PUT /v1/checkout/payment-resources/{id}
​
Solicitar
O seguinte exemplo de solicitação inclui estes detalhes principais:
Envia uma PUTsolicitação para atualizar o link de pagamento com o ID PLB-X7MNK9P2QR8T.
Utiliza um token OAuth 2.0 Bearer no cabeçalho de Autorização para autenticação.
Inclui os Acceptcabeçalhos Content-Typedefinidos como application/json.
Envia a configuração de link atualizada no corpo da solicitação, substituindo completamente as informações de produto existentes.
Mantém o mesmo ID e URL do link de pagamento ao aplicar as novas configurações.
Observação: Atualmente, a API usa apenas PUTchamadas em vez de PATCHchamadas.
curl -v -k -X PUT 'https://api.sandbox.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json' \
 -d '{
    "type": "BUY_NOW",
    "integration_mode": "LINK",
    "reusable": "MULTIPLE",
    "return_url": "https://merchant.example.com/thank-you",
    "line_items": [
        {
            "name": "Wireless Mouse",
            "unit_amount": {
                "currency_code": "USD",
                "value": "29.99"
            }
        }
    ]
}'
​
Resposta
A resposta a seguir contém os detalhes de confirmação de um link de pagamento atualizado.
A API retorna o status HTTP 204 No Contentou, em alguns casos, um corpo de resposta de confirmação com o tipo de conteúdo application/json.
A seguinte resposta de exemplo inclui estes detalhes importantes:
Confirma que o link de pagamento atualizado PLB-X7MNK9P2QR8Tfoi aplicado com sucesso.
Retorna o link de pagamento com ACTIVEo status, mantendo o mesmo ID e URL do link de pagamento.
Preserva o horário de criação original.
Dependendo da implementação, pode incluir um corpo de resposta com detalhes de configuração atualizados.
{
  "id": "PLB-X7MNK9P2QR8T",
  "integration_mode": "LINK",
  "create_time": "2025-11-29T13:13:25.832592Z",
  "status": "ACTIVE",
  "payment_link": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
  "type": "BUY_NOW",
  "reusable": "MULTIPLE",
  "return_url": "https://merchant.example.com/thank-you",
  "line_items": [
    {
      "name": "Wireless Mouse",
      "unit_amount": {
        "currency_code": "USD",
        "value": "29.99"
      }
    }
  ],
  "links": [
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "self",
      "method": "GET"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "replace",
      "method": "PUT"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "delete",
      "method": "DELETE"
    },
    {
      "href": "https://api.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T",
      "rel": "edit",
      "method": "PATCH"
    },
    {
      "href": "https://www.paypal.com/ncp/payment/PLB-X7MNK9P2QR8T",
      "rel": "payment_link",
      "method": "GET"
    }
  ]
}

Veja todas as 46 linhas
​
Excluir link de pagamento ( DELETE)
Exclui um link de pagamento específico pelo seu identificador único. Este endpoint remove permanentemente um link ou botão de pagamento associado ao ID especificado.
Ponto final:DELETE /v1/checkout/payment-resources/{id}
​
Solicitar
O seguinte exemplo de solicitação inclui estes detalhes principais:
Envia uma DELETEsolicitação para remover permanentemente o link de pagamento com o ID PLB-X7MNK9P2QR8T.
Utiliza um token OAuth 2.0 Bearer no cabeçalho de Autorização para autenticação.
Inclui os cabeçalhos Accept e Content-Type definidos como application/json.
curl -v -k -X DELETE 'https://api.sandbox.paypal.com/v1/checkout/payment-resources/PLB-X7MNK9P2QR8T' \
 -H 'Authorization: Bearer A21_A.AAfdJ1oilr2YCRMAosyU48X03YgHCyOPkH82XYdvJBQvRLnuha9ut2lYvN7QgDab-iTVDMDwR3OL-S1t4_GCrh62XdW18A' \
 -H 'Accept: application/json' \
 -H 'Content-Type: application/json'
​
Resposta
A API retorna o status HTTP 204 No Contentcom o tipo de conteúdo application/json, indicando que o link de pagamento foi excluído. Nenhum corpo de resposta é retornado.
​
Tratamento de erros
Código HTTP	Nome do erro	Mensagem de erro	Causa comum
400	INVALID_REQUEST	A solicitação não está bem formada, está sintaticamente incorreta ou viola o esquema.	Faltam campos obrigatórios de recursos de pagamento, como nome do item, valor e URL de retorno, no corpo da requisição.
403	NOT_AUTHORIZED	A autorização falhou devido a permissões insuficientes.	O token de acesso expirou ou não possui os escopos OAuth corretos, e a conta do comerciante não tem permissão para botões de pagamento. Verifique a seção Aplicativos e Credenciais para garantir que a opção Links e Botões de Pagamento esteja marcada.
404	RESOURCE_NOT_FOUND	O recurso especificado não existe.	O ID do recurso de pagamento está incorreto, expirou ou foi excluído. Você pode ter tentado buscar, atualizar ou excluir um link ou botão que não existe mais.
422	UNPROCESSABLE_ENTITY	A ação solicitada não pôde ser executada, é semanticamente incorreta ou falhou na validação de negócios.	Valor de pagamento inválido, moeda não suportada ou violação das regras de negócio, como um link de pagamento duplicado.
500	INTERNAL_SERVER_ERROR	Ocorreu um erro interno no servidor.	Interrupção temporária no processamento do link ou botão de pagamento. Tente novamente ou entre em contato com o suporte se o problema persistir.
Para obter mais informações, consulte a Visão geral dos erros comuns .
​
Teste
Crie uma conta sandbox através do Portal do Desenvolvedor do PayPal.
Utilize seu ambiente sandbox para todo o desenvolvimento e teste.
Todos os endpoints são duplicados para os api-m.sandbox.paypal.comambientes sandbox ( ) e live​.
Teste seu link de pagamento ativo fazendo uma compra. Em seguida, verifique a transação na seção Atividade da sua conta PayPal. Lembre-se de reembolsar a transação de teste posteriormente.