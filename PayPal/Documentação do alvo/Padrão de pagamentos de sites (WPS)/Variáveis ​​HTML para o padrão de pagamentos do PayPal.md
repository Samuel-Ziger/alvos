Variáveis ​​HTML para o padrão de pagamentos do PayPal
OBSOLETO
Última atualização: 20 de janeiro, 14h12

Este produto está obsoleto e não deve ser usado para novas integrações. Use Links e Botões de Pagamento em vez disso. Para obter informações sobre a migração, visite a Central de Atualizações .

Os botões de pagamento padrão do PayPal Payments e o comando de upload do carrinho são compatíveis com as seguintes variáveis ​​HTML.

Nota: Para variáveis ​​obsoletas, consulte variáveis ​​obsoletas .

Variáveis ​​técnicas
As variáveis ​​técnicas de HTML controlam como o PayPal responde tecnicamente quando as pessoas clicam nos botões de pagamento do PayPal ou quando carrinhos de compras ou provedores de soluções de terceiros iniciam o processamento de pagamentos com o comando "Carregar Carrinho". Elas também controlam como seus botões interagem com recursos especiais do PayPal.

Nota: Utilize a IMGtag HTML para exibir o botão na sua página web.

Nota: Use o HTML

Os países dos EUA, Reino Unido, Canadá e Austrália podem usar o PayPal Subscriptions para pagamentos recorrentes.
Nos EUA e no Reino Unido, é possível usar o PayPal Credit. Entre em contato com o PayPal para obter informações sobre como ativar o PayPal Credit.
< img src = " https://www.paypalobjects.com/webstatic/en_US/i/btn/png/btn_buynow_107x26.png " alt = " Comprar agora " />   
Valores válidos para a variável cmd
cmdvalor	Descrição
_xclick	O botão em que a pessoa clicou era um botão "Comprar agora".
_cart	
Para compras no carrinho de compras. As seguintes variáveis ​​especificam o tipo de botão do carrinho de compras em que a pessoa clicou:

addBotões "Adicionar ao carrinho" para o carrinho de compras do PayPal.
displayBotões "Ver Carrinho" para o Carrinho de Compras do PayPal
uploadO comando Cart Upload para carrinhos de terceiros.
_xclick-subscriptions	O botão em que a pessoa clicou era um botão de inscrição.
_xclick-auto-billing	O botão em que a pessoa clicou era um botão de Cobrança Automática.
_donations	O botão em que a pessoa clicou era um botão de Doação.
_s-xclick	O botão clicado pela pessoa estava protegido contra adulteração por meio de criptografia, ou estava salvo na conta PayPal do comerciante. O PayPal determina qual tipo de botão foi clicado decodificando o código criptografado ou consultando o botão salvo na conta do comerciante.
Valores válidos para a variável cmd
valor	Descrição
_xclickOpcional	O URL para o qual o PayPal envia informações sobre o pagamento, na forma de mensagens de Notificação Instantânea de Pagamento. Comprimento: 255 caracteres
_cartExceção de requisito
Necessário para botões que foram salvos em contas do PayPal.

Caso contrário, não é permitido.

Para compras no carrinho de compras. As seguintes variáveis ​​especificam o tipo de botão do carrinho de compras em que a pessoa clicou:

Observação: Uma conta PayPal de um comerciante pode ter no máximo 1.000 botões de pagamento salvos.
_xclick-subscriptionsOpcional	
Um identificador da fonte que gerou o código do botão clicado pelo comprador, também conhecido como notação de compilação . Especifique um valor usando o seguinte formato:

Empresa_Serviço_Produto_País
Substitua <Service>por um valor apropriado:

BuyNow
Importante: Este BuyNowrecurso está obsoleto. Para novas integrações, consulte Personalizar o botão de finalização de compra do PayPal .
AddToCart
Donate
Subscribe
AutomaticBilling
ShoppingCart
Substitua " always" <Product>por WPS"always" nos botões de pagamento do PayPal Payments Standard e no comando "PayPal Payments Standard Cart Upload".

Substitua <Country>por um código de país de duas letras apropriado, conforme definido pela norma ISO 3166-1 .

Por exemplo, um botão "Comprar agora" em seu site, que você mesmo programou, pode ter a seguinte linha de código:

bn="DesignerFotos_BuyNow_WPS_US"
Observação: o código HTML do botão que você cria no site do PayPal inclui bnvariáveis ​​com valores válidos gerados pelo PayPal.
Variáveis ​​de itens individuais
Essas variáveis ​​de item individuais especificam informações sobre um produto ou serviço para os botões "Comprar agora" e "Adicionar ao carrinho", ou especificam informações sobre uma contribuição para os botões "Doar".

Nome	Descrição
amountConsulte a descrição para obter detalhes sobre os requisitos.	
O preço ou valor do produto, serviço ou contribuição, excluindo frete, manuseio ou impostos. Se você omitir essa variável dos botões "Comprar agora" ou "Doar", os compradores inserirão o valor desejado no momento do pagamento.

Necessário para os botões "Adicionar ao carrinho"
Opcional para os botões "Comprar agora" e "Doar"
Não é compatível com botões de inscrição.

discount_amount Opcional	
O valor do desconto associado a um item deve ser inferior ao preço de venda do item. Se você especificar um valor fixo discount_amounte discount_amount2ele não for definido, esse valor será aplicado independentemente da quantidade de itens comprados.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

discount_amount2Opcional	
O valor do desconto associado a cada unidade adicional do item deve ser igual ou inferior ao preço de venda do item. Para discount_amount2que o desconto entre em vigor, você também deve especificar um discount_amountvalor que seja maior ou igual a 0.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

discount_rateOpcional	
Taxa de desconto, em percentagem, associada a um produto.

Defina um valor menor que 100. Se você não definir discount_rate2, o valor em discount_ratese aplica apenas ao primeiro item, independentemente da quantidade de itens comprados.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

discount_rate2Opcional	
Taxa de desconto, em percentagem, associada a cada unidade adicional do item.

Deve ser igual ou menor que 100. Para discount_rate2que tenha efeito, você também deve especificar um discount_rateque seja maior ou igual a 0.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

discount_numOpcional	
Número de unidades adicionais do item às quais o desconto se aplica.

Aplicável quando você especifica discount_amount2ou discount_rate2. Especifica um limite máximo para o número de itens com desconto.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

item_nameConsulte a descrição para obter detalhes sobre os requisitos.	
Descrição do item. Se você omitir esta variável, os compradores inserirão seus próprios nomes durante a finalização da compra.

Opcional para os botões Comprar agora, Doar, Assinar, Cobrança automática e Adicionar ao carrinho.
Comprimento do caractere: 127
item_numberConsulte a descrição para obter detalhes sobre os requisitos.	Variável de passagem para você rastrear o produto ou serviço adquirido ou a contribuição feita. O valor que você especificar será retornado a você após a conclusão do pagamento. Necessário se você quiser que o PayPal rastreie o estoque ou o lucro e prejuízo do item vendido pelo botão. Comprimento do campo: 127 caracteres
quantityOpcional	
Número de itens. Se as taxas de envio baseadas no perfil forem configuradas com base na quantidade, a soma dos quantityvalores será usada para calcular as taxas de envio do pagamento. O PayPal adiciona um número sequencial para identificar exclusivamente o item no Carrinho de Compras do PayPal. Por exemplo, quantity1, quantity2, e assim por diante.

Observação: O valor de quantitydeve ser um número inteiro positivo. Números nulos, zero ou negativos não são permitidos.
shippingOpcional	
O custo do frete deste item. Se você especificar um valor fixo shippinge shipping2ele não estiver definido, esse valor será cobrado independentemente da quantidade de itens comprados.

Essa shippingvariável é válida apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

Por padrão, se as taxas de envio baseadas no perfil estiverem configuradas, os compradores serão cobrados de acordo com os métodos de envio escolhidos.

shipping2Opcional	
O custo de envio de cada unidade adicional deste item. Se você omitir essa variável e as taxas de envio baseadas no perfil forem configuradas, os compradores serão cobrados de acordo com os métodos de envio escolhidos.

Essa shippingvariável é válida apenas para os botões "Comprar agora" e "Adicionar ao carrinho".

taxOpcional	Variável de imposto baseada em transação. Defina esta variável com um valor fixo de imposto a ser aplicado ao pagamento, independentemente da localização do comprador. Este valor substitui quaisquer configurações de imposto definidas no seu perfil de conta. Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho". Por padrão, as configurações de imposto do perfil, se houver, são aplicadas.
tax_rateOpcional	
Variável de sobreposição de impostos baseada em transações. Defina esta variável para uma porcentagem que se aplica ao valor amountmultiplicado pela quantidade selecionada durante a finalização da compra. Este valor substitui quaisquer configurações de impostos definidas no seu perfil de conta. Um valor válido é de [valor 0.001mínimo 100] a [valor máximo]. Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho". Por padrão, as configurações de impostos do perfil, se houver, são aplicadas.

Comprimento do caractere: 6
undefined_quantityOpcional	
Permite que os compradores especifiquem a quantidade. Por exemplo, 1.

Opcional para botões "Comprar agora".
Não pode ser usado com outros botões.
Comprimento do caractere: 1
weightOpcional	
Peso dos itens. Se as taxas de envio baseadas em perfil forem configuradas com base no peso, a soma dos weightvalores será usada para calcular as taxas de envio para o pagamento.

Um valor válido é um número decimal com dois dígitos significativos à direita da vírgula decimal.

weight_unitOpcional	
A unidade de medida, se weightespecificada. O valor válido é lbsou kgs.

O padrão é lbs.

on0Opcional	
Primeiro campo de opção: nome e rótulo. A os0variável contém o valor correspondente a este campo de opção. Por exemplo, se on0for size, os0poderia ser large.

Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.
Comprimento do caractere: 64
on1Opcional	
Segundo campo de opção: nome e rótulo. A os1variável contém o valor correspondente a este campo de opção. Por exemplo, se on1for , colorentão os1poderia ser blue.

Você pode especificar um máximo de 7 nomes de campos de opção (6 com botões de inscrição) incrementando o índice do nome da opção ( on0 até on6).

Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.
Comprimento do caractere: 64
os0Opcional	
Seleção da opção pelo comprador para o primeiro campo de opção on0. Se o campo de opção for um menu suspenso ou um conjunto de botões de opção, cada valor válido terá no máximo 64 caracteres. Se os compradores inserirem esse valor em um campo de texto, o valor máximo será de 200 caracteres.

Observação: o campo de opção on0também deve ser definido. Por exemplo, poderia ser size.
Para opções com preço definido, inclua o preço e o símbolo da moeda no texto da seleção da opção:

< option value = " small " > pequeno - $10,00 </option> 
Adicione uma variável correspondente option_select0para option_amount0cada opção com preço. As opções com preço são suportadas apenas para os botões "Comprar agora" e "Adicionar ao carrinho". Apenas uma opção do menu suspenso pode ter opções com preço.

Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.

Comprimento do caractere: 64 ou 200, veja acima.
os1Opcional	
Seleção da opção pelo comprador para o segundo campo de opção, on1. Se o campo de opção for um menu suspenso ou um conjunto de botões de opção, cada valor válido terá no máximo 64 caracteres. Se os compradores inserirem esse valor em um campo de texto, o valor máximo será de 200 caracteres.

Você pode especificar um máximo de sete opções de seleção (seis com botões de inscrição) incrementando o índice de seleção de opções ( os0 até os6). Você pode implementar até cinco opções de seleção como menus suspensos e até duas opções de seleção como caixas de texto.

Nota: Um campo de opção correspondente ( on0 até on6) deve ser definido.
Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.

Comprimento do caractere: 64 ou 200, veja acima.
option_indexConsulte a descrição para obter os requisitos.	
O número cardinal do campo de opções, on0de 0 a 1 on9, que possui opções de produto com preços diferentes para cada opção. Inclua `option_index` se o campo de opções com preços não for nulo on0.

Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.
O padrão é 0.

option_select0Opcional	
Para opções com preço, o valor da primeira opção selecionada no on0menu suspenso. Os valores devem ser exatamente iguais:

< option value = " " > pequeno - $10,00 </option> ... < input type = " hidden " name = " option_select0 " value = " " >    
Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.
Comprimento do caractere: 64
option_amount0Opcional	
Para opções com preço, o valor que você deseja cobrar pela primeira opção selecionada no on0menu suspenso. Use apenas valores numéricos; a moeda é obtida da currency_codevariável. Por exemplo:

< option value = " small " > pequeno - $... < input type="hidden " name="option_amount0" </option> value="10.00"> 
Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.

Comprimento do caractere: 64
option_select1Opcional	
Para opções com preço, o valor da segunda opção selecionada no on0menu suspenso. Por exemplo:

... < option value = " " > pequeno - $10,00 </option> ... < input type = " hidden " name = " option_select " value = " " >    
Você pode especificar um máximo de 10 seleções de opções incrementando o índice de seleção de opções ( option_select0até option_select9).

Observação: Você também deve definir uma os0seleção de opção correspondente.
Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.
Comprimento do caractere: 64
option_amount1Opcional	
Para opções com preço, o valor que você deseja cobrar pela segunda opção selecionada no on0menu suspenso. Por exemplo:

... < option value = " small " > médio - $ </option> ... < input type = " hidden " name = " option_amount1 " value = " " >    
Você pode especificar um máximo de 10 valores de opção incrementando o índice de valor da opção ( option_amount0até option_amount9).

Observação: Você também deve definir uma os0seleção de opção correspondente.
Opcional para os botões Comprar agora, Adicionar ao carrinho, Assinar e Cobrança automática.
Não é compatível com botões de doação.

Comprimento do caractere: 64
Variáveis ​​da transação de pagamento
As variáveis ​​de transação de pagamento fornecem informações sobre pagamentos completos, independentemente dos itens individuais envolvidos. Você pode usar essas variáveis ​​com os botões "Adicionar ao carrinho" e o comando "Enviar para o carrinho".

Nome	Descrição
address_overrideOpcional	
1O endereço especificado com as variáveis ​​de preenchimento automático substitui o endereço armazenado do membro do PayPal. Os compradores veem os endereços que você informa, mas não podem editá-los. O PayPal não exibe endereços inválidos ou omitidos.

Para obter mais informações, consulte as variáveis ​​da página de finalização de compra do PayPal com preenchimento automático .

Comprimento do caractere: 1
currency_codeOpcional	
A moeda do pagamento. O padrão é USD.

Para valores válidos, consulte Moedas aceitas pelo PayPal .

Comprimento do caractere: 3
customOpcional	
Variável de passagem para seus próprios fins de rastreamento, que os compradores não veem.

Por padrão, nenhuma variável é retornada para você.

Comprimento do caractere: 256
handlingOpcional	
Custos de manuseio. Esta variável não está relacionada à quantidade. O mesmo custo de manuseio se aplica, independentemente do número de itens no pedido.

Por padrão, não estão incluídas taxas de manuseio.

invoiceOpcional	
Variável de passagem que você pode usar para identificar o número da sua fatura para esta compra.

Por padrão, nenhuma variável é retornada para você.

Comprimento do caractere: 127
tax_cartOpcional	Imposto sobre o valor total do carrinho, que substitui o valor de qualquer item individual.tax_x
weight_cartOpcional	
Se as taxas de envio baseadas no perfil forem configuradas com base no peso, o PayPal usará esse valor para calcular as despesas de envio do pagamento. Esse valor substitui os weightvalores dos itens individuais.

Um valor válido é um número decimal com dois dígitos significativos à direita da vírgula decimal.

weight_unitOpcional	
A unidade de medida, se weight_cartespecificada. O valor válido é lbsou kgs.

O padrão é lbs.

Variáveis ​​do carrinho de compras
Utilize essas variáveis ​​de carrinho de compras com botões "Adicionar ao carrinho", bem como com carrinhos de compras de terceiros ou carrinhos personalizados que iniciam o processamento de pagamento com o comando "Carregar carrinho".

Observação: O PayPal recomenda não enviar mais de 25 itens em um único botão ou comando de upload do carrinho.

Nome	Descrição
addConsulte a descrição para obter os requisitos.	
Adicione um item ao carrinho de compras do PayPal.

Defina essa variável da seguinte forma:

adicionar="1"
Alternativamente, utilize a display="1"variável, que exibe o conteúdo do carrinho de compras do PayPal para o comprador.

Se você especificar ambos , ` adda` e `b` display, display`c` terá precedência.

Comprimento do caractere: 1
amount_x Obrigatório	
O valor associado ao item x. Para passar um valor total para todo o carrinho, use amount_1.

Aplica-se somente ao comando de upload do carrinho.

businessObrigatório	Seu ID do PayPal ou um endereço de e-mail associado à sua conta do PayPal. Os endereços de e-mail devem ser confirmados.
discount_amount_cartOpcional	
Um único valor de desconto aplicado a todo o carrinho.

Deve ser menor que o preço de venda de todos os itens combinados no carrinho. Essa variável substitui quaisquer valores de itens individuais, se houver. discount_amount_x

Aplica-se somente ao comando de upload do carrinho.

Nota: Não aplicável se forem utilizados valores de impostos individuais . tax_x
discount_amount_xOpcional	
O valor do desconto associado ao item x.

O valor deve ser inferior ao preço de venda do item correspondente. Esse valor é adicionado a quaisquer outros descontos aplicados a itens no carrinho.

Aplica-se somente ao comando de upload do carrinho.

discount_rate_cartOpcional	
Taxa de desconto única, em percentagem, a ser aplicada a todo o carrinho de compras.

Definido para um valor inferior a 100. A variável substitui quaisquer valores de itens individuais, se presentes. discount_rate_x

Aplica-se somente ao comando de upload do carrinho.

Nota: Não aplicável se forem utilizados valores de impostos individuais . tax_x
discount_rate_xOpcional	
A taxa de desconto associada ao item x.

Definido para um valor inferior a 100. A variável leva em consideração todas as quantidades do item x.

Aplica-se somente ao comando de upload do carrinho.

displayConsulte a descrição para obter os requisitos.	
Exibir o conteúdo do carrinho de compras do PayPal para o comprador. Esta variável deve ser definida da seguinte forma:

exibir="1"
A alternativa é a add="1"variável, que adiciona um item ao carrinho de compras do PayPal.

Se ambos ` adda` e ` displayb` forem especificados, display`a` terá precedência.

Comprimento do caractere: 1
handling_cartOpcional	Uma única taxa de manuseio é cobrada para todo o carrinho. Se handling_cartusada em vários botões "Adicionar ao carrinho", o handling_cartvalor do primeiro item será utilizado.
item_name_xObrigatório	
O nome associado ao item x. Para passar um nome agregado para todo o carrinho, use item_name_1.

Aplica-se somente ao comando de upload do carrinho.

paymentactionOpcional	
Indica se o pagamento é uma venda final ou uma autorização para uma venda final, a ser registrada posteriormente.

O valor válido é sale, authorization, ou order.

O valor padrão é sale. Para bloquear o valor autorizado na conta PayPal, defina este valor como authorization. Para autorizar o pagamento sem bloquear o valor na conta PayPal, defina este valor como order.

Importante: Se você definir paymentactioncomo order, use a API de Autorização e Captura para autorizar e capturar pagamentos. Os Serviços para Comerciantes no site do PayPal permitem capturar pagamentos apenas para autorizações, mas não para pedidos.
quantity_xOpcional	A quantidade associada ao item xcarregado do carrinho de compras. Para passar a quantidade total do carrinho, use quantity_1.
shopping_urlOpcional	
O endereço URL da página no site do comerciante para a qual os compradores são direcionados quando clicam no botão "Continuar comprando" na página do carrinho de compras do PayPal.

Para obter mais informações, consulte Continuar comprando na página da loja .

uploadConsulte a descrição para obter os requisitos.	
Faça o upload do conteúdo de um carrinho de compras de terceiros ou de um carrinho de compras personalizado.

Defina esta variável da seguinte forma:

upload="1"
Alternativamente, utilize a add="1"variável e as display="1"variáveis ​​com o Carrinho de Compras do PayPal.

Comprimento do caractere: 1
Variáveis ​​de pagamento recorrente
As variáveis ​​de pagamentos recorrentes definem os termos para diferentes planos de pagamento automático do PayPal. Os botões de pagamento recorrente são:

Inscreva-se
Cobrança automática
Variáveis ​​de inscrição
Nome	Descrição
businessObrigatório	Seu ID do PayPal ou um endereço de e-mail associado à sua conta do PayPal. Os endereços de e-mail devem ser confirmados.
item_nameOpcional	Descrição do item à venda. Se você estiver coletando pagamentos agregados, o valor pode ser um resumo de todos os itens comprados, um número de rastreamento ou um termo genérico como "item" subscription. Se você omitir essa variável, os compradores verão um campo no qual poderão inserir o nome do item. Comprimento do campo: 127 caracteres
currency_codeOpcional	
A moeda de referência para os preços dos períodos de teste e da assinatura. O padrão é USD.

Para valores válidos, consulte Moedas aceitas pelo PayPal .

Comprimento do caractere: 3
a1Opcional	Preço referente ao período de teste de 1 dia. Para um período de teste gratuito, especifique 0.
p1Consulte a descrição para obter os requisitos.	Duração do período de teste: 1. Obrigatório se você especificar a1. Especifique um valor inteiro dentro do intervalo válido para as unidades de duração que você especificar com t1. Comprimento do caractere: 2
t1Obrigatório se você especificara1	
Período de teste com duração de 1 unidade.

O valor válido é:

D. Dias. O período válido p1é 1de 90.
W. Semanas. O período válido p1é 1de 52.
MMeses. O período válido p1é 1de 24.
YAnos. O período válido p1é 1de 5.
Comprimento do caractere: 1
a2Opcional	Preço referente ao período de teste 2. Só pode ser especificado se você também especificar a1.
p2Consulte a descrição para obter os requisitos.	Duração do período de teste: 2. Obrigatório se você especificar a2. Especifique um valor inteiro dentro do intervalo válido para as unidades de duração que você especificar com t2. Comprimento do caractere: 2
t2Consulte a descrição para obter os requisitos.	
Período de teste com duração de 2 unidades.

O valor válido é:

D. Dias. O período válido p2é 1de 90.
W. Semanas. O período válido p2é 1de 52.
MMeses. O período válido p2é 1de 24.
YAnos. O período válido p2é 1de 5.
Comprimento do caractere: 1
a3Obrigatório	Preço da assinatura normal.
p3Obrigatório	Duração da assinatura. Especifique um valor inteiro no intervalo válido para as unidades de duração especificadas t3. Comprimento do caractere: 2
t3Obrigatório	
Unidades de assinatura regulares com duração definida.

O valor válido é:

D. Dias. O período válido p3é 1de 90.
W. Semanas. O período válido p3é 1de 52.
MMeses. O período válido p3é 1de 24.
YAnos. O período válido p3é 1de 5.
Comprimento do caractere: 1
srcOpcional	
Pagamentos recorrentes. Os pagamentos de assinatura são recorrentes, a menos que os assinantes cancelem suas assinaturas antes do final do ciclo de faturamento atual ou que você limite o número de recorrências de pagamento com o valor especificado para srt.

O valor válido é:

0. Os pagamentos da assinatura não são recorrentes.
1. Os pagamentos da assinatura são recorrentes.
O valor padrão é 0.

Comprimento do caractere: 1
srtOpcional	Frequência de recorrência. Número de vezes que os pagamentos da assinatura se repetem. Especifique um número inteiro com valor mínimo de 2 e máximo de 52. Válido somente se você especificar src="1". Comprimento do caractere: 1
sraOpcional	
Tentativa de cobrança em caso de falha. Se um pagamento recorrente falhar, o PayPal tentará cobrar o pagamento mais duas vezes antes de cancelar a assinatura.

O valor válido é:

0. Não tente novamente pagamentos recorrentes que falharam.
1. Tente novamente os pagamentos recorrentes que falharam antes de cancelar.
O padrão é 1.

Para obter mais informações, consulte Tentando novamente pagamentos recorrentes com falha usando botões de assinatura .

Comprimento do caractere: 1
no_noteObrigatório	
Não solicite aos compradores que incluam uma nota com seus pagamentos. O valor válido é proveniente dos botões de inscrição.

1. Oculte a caixa de texto e o prompt.
Para botões de inscrição, defina sempre no_notecomo 1.

Comprimento do caractere: 1
customOpcional	Campo definido pelo usuário que o PayPal processa e retorna para você no e-mail de notificação de pagamento do comerciante. Os assinantes não veem este campo. Comprimento do campo: 255 caracteres
invoiceOpcional	Campo definido pelo usuário que deve ser único para cada assinatura. O número da fatura é exibido aos assinantes juntamente com os demais detalhes de seus pagamentos. Comprimento do campo: 127 caracteres
Variáveis ​​de inscrição
Nome	Descrição
businessObrigatório	
Especifique se deseja permitir que os compradores insiram os limites máximos de faturamento em uma caixa de texto ou escolham em uma lista de limites máximos de faturamento que você especificar.

O valor válido é:

max_limit_ownSeu botão exibe uma caixa de texto para que os compradores insiram seus próprios valores máximos acima de um limite mínimo de faturamento que você define com a min_amountvariável.
max_limit_definedSeu botão exibe um menu suspenso com opções de produtos e preços para que os compradores possam escolher seus limites máximos de faturamento.
item_nameOpcional	O limite mínimo de faturamento mensal, se houver. Válido somente se subscription= max_limit_own.
Variáveis ​​da página de finalização de compra do PayPal
Essas variáveis ​​controlam a aparência e o funcionamento das páginas de finalização de compra do PayPal.

Nome	Descrição
image_urlOpcional	
O URL da imagem de 150x50 pixels exibida como seu logotipo no canto superior esquerdo das páginas de finalização de compra do PayPal.

Por padrão, é o nome da sua empresa, caso você tenha uma conta PayPal Business, ou seu endereço de e-mail, caso você tenha uma conta PayPal Premier ou Pessoal.

Comprimento do caractere: 1.024
lcOpcional	
A localização da página de login ou cadastro no momento da finalização da compra. O PayPal oferece páginas de finalização de compra localizadas para alguns países e idiomas.

Para obter mais informações sobre códigos de localidade e uma lista de localidades compatíveis, consulte a página de referência de códigos de localidade do PayPal .

Comprimento do caractere: 5
no_shippingOpcional	
Não solicite o endereço de entrega aos compradores.

O valor válido é:

0Solicita um endereço, mas não o exige.
1Não solicite um endereço.
2Solicitar um endereço e exigir que ele seja fornecido.
O padrão é 0.

Comprimento do caractere: 1
returnOpcional	
O URL para o qual o PayPal redireciona o navegador dos compradores após a conclusão do pagamento. Por exemplo, especifique um URL em seu site que exiba uma página de agradecimento pelo pagamento.

Por padrão, o PayPal redireciona o navegador para uma página da web do PayPal.

Comprimento do caractere: 1.024
rmOpcional	
Método de retorno. FORM METHODUtilizado para enviar dados para a URL especificada pela returnvariável.

O valor válido é:

0Todos os pagamentos do carrinho de compras utilizam esse GETmétodo.
1O navegador do comprador é redirecionado para a URL de retorno usando esse GETmétodo, mas nenhuma variável de pagamento é incluída.
2O navegador do comprador é redirecionado para o URL de retorno usando o POSTmétodo, e todas as variáveis ​​de pagamento são incluídas.
O padrão é 0.

Nota: A rmvariável só entra em vigor se returnestiver definida e a Transferência de Dados de Pagamento estiver desativada.
Comprimento do caractere: 1
cancel_returnOpcional	
Um URL para o qual o PayPal redireciona os navegadores dos compradores caso eles cancelem a finalização da compra antes de concluir o pagamento. Por exemplo, especifique um URL em seu site que exiba a página de Pagamento Cancelado.

Por padrão, o PayPal redireciona o navegador para uma página da web do PayPal.

Comprimento do caractere: 1.024
Preenchimento automático de variáveis ​​na página de finalização de compra do PayPal
As variáveis ​​HTML para preenchimento automático das páginas de finalização de compra do PayPal permitem que você especifique informações sobre os compradores. O PayPal recomenda que você inclua variáveis ​​de preenchimento automático em todos os seus botões de pagamento para garantir um tratamento consistente dos endereços na experiência de finalização de compra dos seus compradores. Para determinar como a experiência de finalização de compra varia caso você não utilize variáveis ​​de preenchimento automático, consulte Tratamento de Endereços (Somente para Comerciantes dos EUA) .

Observação: ao inserir address_override=1variáveis ​​de impostos ou frete, o PayPal exibe os valores no widget de pagamento. Além disso, o PayPal oculta o widget de cálculo, independentemente de você ter configurado as taxas de frete e impostos no seu perfil da conta.

Nome	Descrição
address1Opcional	Rua (1 de 2 campos) Comprimento do caractere: 100
address2Opcional	Rua (2 de 2 campos) Comprimento do caractere: 100
cityOpcional	Comprimento do caractere da cidade : 40
countryOpcional	
Define o país de envio e de faturamento.

Para valores válidos, consulte Códigos de país e região .

Comprimento do caractere: 2
emailOpcional	Endereço de e-mail ( Comprimento de caracteres: 127)
first_nameOpcional	Nome ( Comprimento de caracteres: 32)
last_nameOpcional	Sobrenome ( Comprimento de caracteres: 32)
lcOpcional	
Define o idioma apenas para a página de informações de faturamento/login. O padrão é US.

Para valores válidos, consulte Países e regiões suportados pelo PayPal .

Comprimento do caractere: 2
charsetOpcional	
Define o conjunto de caracteres e a codificação de caracteres para a página de informações de faturamento/login no site do PayPal. Além disso, essa variável define os mesmos valores para as informações que você envia ao PayPal no código HTML do seu botão. O valor padrão é baseado nas configurações de codificação de idioma do seu perfil de conta.

Para valores válidos, consulte Definindo o conjunto de caracteres — charset .

night_phone_aOpcional	O código de área para números de telefone dos EUA ou o código do país para números de telefone fora dos EUA. O PayPal preenche automaticamente o número de telefone residencial do comprador. Veja acima.
night_phone_bOpcional	O prefixo de três dígitos para números de telefone dos EUA ou o número de telefone completo para números de telefone fora dos EUA, excluindo o código do país. O PayPal preenche automaticamente o número de telefone residencial do comprador. Veja acima.
night_phone_cOpcional	Número de telefone de quatro dígitos para números de telefone dos EUA. O PayPal preenche automaticamente o número de telefone residencial do comprador. Veja acima.
stateOpcional	
Estado dos EUA.

Para valores válidos, consulte os códigos de estado e província do PayPal .

Comprimento do caractere: 2
zipOpcional	Código postal. Comprimento do caractere: 32
Variáveis ​​da API de atualização instantânea
Configure o comando de upload do carrinho para o retorno de chamada da API de Atualização Instantânea. Estabeleça seu próprio servidor de retorno de chamada de Atualização Instantânea antes de usar essas variáveis. As variáveis ​​HTML para a API de Atualização Instantânea configuram o comando de upload do carrinho para o retorno de chamada da API de Atualização Instantânea. Estabeleça seu próprio servidor de retorno de chamada de Atualização Instantânea antes de usar essas variáveis.

Configure um pagamento para a API de Atualização Instantânea.
Algumas variáveis ​​de Atualização Instantânea configuram o Envio do Carrinho para usar seu servidor de retorno de chamada. Inclua as seguintes variáveis ​​obrigatórias no comando de Envio do Carrinho para que o PayPal envie solicitações de Atualização Instantânea para seu servidor de retorno de chamada. Inclua as seguintes variáveis ​​opcionais quando apropriado.

Nome	Descrição
callback_urlObrigatório	URL do seu servidor de retorno de chamada de atualização instantânea. Comprimento do texto: 1024
callback_timeoutObrigatório	
O tempo limite em segundos para respostas de retorno de chamada do seu servidor de retorno de chamada de Atualização Instantânea. Após exceder o tempo limite, o PayPal usa os valores de fallback na página Revisar seu pagamento para impostos, frete e seguro.

O valor válido é de 1até 6. O PayPal recomenda o valor 3.

Importante: Utilize valores diferentes 3apenas quando instruído a fazê-lo pelo seu representante do PayPal.
Comprimento do caractere: 1
callback_versionObrigatório	A versão da API Instant Update que seu servidor de retorno de chamada utiliza.
fallback_tax_amountOpcional	Valor do imposto a ser usado como alternativa, caso a resposta do callback expire.
fallback_shipping_option_name_xObrigatório	
Nome e rótulo da opção de envio xa ser usada como alternativa, caso a resposta da chamada expire. Por exemplo, "Expresso 2 dias". Você pode incluir no máximo 10 opções de envio como alternativas. Substitua xpor números ordinais, começando com 0.

Inclua uma instância desta variável, com seu índice ( x) definido como 0. Se você incluir apenas uma instância, inclua fallback_shipping_option_is_default_xo parâmetro com seu índice, x, definido como 0e seu valor definido como 1.

Um valor válido para xé de 0a 9.

Comprimento do caractere: 50
fallback_shipping_option_amount_xObrigatório	
Valor do frete para opção xa ser usada como alternativa, caso a resposta expire.

O valor válido para xé de 0a 9.

fallback_shipping_option_is_default_xObrigatório	
Indica que a opção de envio xé a padrão e deve ser selecionada no menu suspenso como alternativa, caso a resposta expire.

Apenas uma opção de envio pode ser definida como padrão para os compradores. Verifique se você definiu apenas uma instância de ` fallback_shipping_option_is_default_xto` 1.

O valor válido é:

1A opção de envio xé a opção de envio padrão.
0A opção de envio xnão é a opção de envio padrão.
O valor válido para xé de 0a 9.

Comprimento do caractere: 1
fallback_insurance_option_offeredOpcional	
Indica que o seguro é oferecido e que os consumidores podem optar por contratá-lo durante a finalização da compra. O PayPal ignora essa variável se você a omitir fallback_insurance_amountou se o seu valor for menor ou igual a 0.

O valor válido é:

1O seguro é oferecido e os consumidores podem optar por contratá-lo durante o processo de finalização da compra.
0O seguro é aplicado juntamente com a transação, se aplicável. O consumidor NÃO terá a opção de remover o seguro do valor da transação.
Comprimento do caractere: 1
fallback_insurance_amountOpcional	Valor do seguro a ser usado como alternativa, caso a resposta da chamada expire. Inclua a fallback_insurance_option_offeredvariável HTML se especificar um valor de seguro. O valor do seguro alternativo se aplica a todas as opções de envio que você especificar.
Variáveis ​​de atualização instantânea para dimensões de itens individuais
Às vezes, as taxas de envio são calculadas com base nas dimensões dos itens individuais no carrinho de compras. Inclua as seguintes variáveis ​​dimensionais opcionais abaixo no comando de upload do carrinho para fornecer as informações ao seu servidor de retorno de chamada.

Variáveis ​​HTML para atualização instantânea do pagamento de frete com base nas dimensões.
Nome	Descrição
height_xOpcional	
Altura do item xno carrinho de compras.

Um valor válido é um número inteiro positivo.

height_unitOpcional	
Unidade de medida para os valores especificados por valores. the height_x

Um valor válido é qualquer valor que você escolher. O PayPal passa o valor para o seu servidor de retorno de chamada nas solicitações de retorno de chamada.

width_xOpcional	
Largura do item xno carrinho de compras.

Um valor válido é um número inteiro positivo.

width_unitOpcional	
Unidade de medida para o valor especificado por . width_x

Um valor válido é qualquer valor que você escolher. O PayPal passa o valor para o seu servidor de retorno de chamada nas solicitações de retorno de chamada.

length_xOpcional	
Comprimento do item xno carrinho de compras.

O valor válido é um número inteiro positivo.

length_unit Opcional	
Unidade de medida para o valor especificado por . length_x

Um valor válido é qualquer valor que você escolher. O PayPal envia esse valor para o seu servidor de retorno de chamada nas solicitações de retorno de chamada.

Variáveis ​​obsoletas
As seguintes variáveis ​​foram descontinuadas. Variáveis ​​descontinuadas são ignoradas ao serem enviadas para o PayPal.

Variáveis ​​HTML do botão PayPal obsoletas
Nome	Descrição	Data de descontinuação
cbtOpcional	
Define o texto do botão "Retornar ao Comerciante" na página de conclusão do pagamento do PayPal. Para contas comerciais, o botão de retorno exibe o nome da sua empresa no lugar de "<nome da empresa>" Merchant, por padrão. Por padrão, o texto é exibido Return to donations coordinator para botões de doação.

Nota: A cbtvariável só tem efeito se returnestiver definida.
Comprimento do caractere: 60	Dezembro de 2016
cnOpcional	Relacionado à no_notevariável. Quando no_note`is` é `true` 0, uma caixa de texto é exibida na finalização da compra para os compradores que desejam incluir instruções especiais. Use o cncampo para inserir uma nota ou rótulo personalizado para esta caixa de texto, como, por exemplo, `information` Enter any special instructions:. Comprimento do campo: 40 caracteres	Setembro de 2016
cpp_cart_border_colorOpcional	
O código hexadecimal HTML da sua cor principal de identificação. O PayPal combina sua cor com branco em um preenchimento gradiente que contorna a área de revisão do carrinho na interface de finalização de compra do PayPal.

O valor válido consiste em 6 caracteres hexadecimais de um byte que representam um código hexadecimal HTML para uma cor.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".
Não é compatível com os botões "Inscrever-se" ou "Doar".

Comprimento do caractere: 6	Setembro de 2016
cpp_header_imageOpcional	
A imagem no canto superior esquerdo da página de finalização da compra. O tamanho máximo da imagem é de 750 pixels de largura por 90 pixels de altura. O PayPal recomenda que você forneça uma imagem armazenada apenas em um servidor seguro (https).

Para obter mais informações, consulte "Personalizar páginas de finalização de compra do PayPal" no Guia de Configuração e Administração do Comerciante.

Obsoleto para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".

Comprimento do caractere: Sem limite	Setembro de 2016
cpp_headerback_colorOpcional	
A cor de fundo do cabeçalho da página de finalização da compra.

O valor válido é um código de cor hexadecimal HTML de seis caracteres, em ASCII, que não diferencia maiúsculas de minúsculas.

Obsoleto para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".

Comprimento do caractere: 6	Setembro de 2016
cpp_headerborder_colorOpcional	
A cor da borda ao redor do cabeçalho da página de finalização da compra. A borda tem um perímetro de 2 pixels ao redor do cabeçalho, que possui um tamanho máximo de 750 pixels de largura por 90 pixels de altura.

O valor válido é um código de cor hexadecimal HTML de seis caracteres, em ASCII, que não diferencia maiúsculas de minúsculas.

Obsoleto para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".

Comprimento do caractere: 6	Setembro de 2016
cpp_logo_imageOpcional	
Um URL para a imagem do seu logotipo. Use um formato gráfico válido, como .gif.jpg, .jpg.png ou .png.png. Limite a imagem a 190 pixels de largura por 60 pixels de altura. O PayPal corta imagens maiores que isso. O PayPal coloca a imagem do seu logotipo na parte superior da área de revisão do carrinho.

Observação: o PayPal recomenda que você armazene a imagem em um servidor seguro (https). Caso contrário, os navegadores exibirão uma mensagem informando que as páginas de finalização da compra contêm itens não seguros.
O valor válido é de 127 caracteres alfanuméricos de um único byte.

Válido apenas para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".
Não é compatível com os botões "Inscrever-se" ou "Doar".

Comprimento do caractere: 127	Setembro de 2016
cpp_payflow_colorOpcional	
A cor de fundo da página de finalização da compra, abaixo do cabeçalho. O valor válido é um código de cor hexadecimal HTML de seis caracteres, em ASCII, que não diferencia maiúsculas de minúsculas.

Observação: Cores de fundo que conflitem com as mensagens de erro do PayPal não são permitidas; nesses casos, a cor padrão é branca.
Obsoleto para os botões "Comprar agora" e "Adicionar ao carrinho" e para o comando "Carregar carrinho".

Comprimento do caractere: 6	Setembro de 2016
max_textOpcional	Uma descrição do plano de cobrança automática. A página do botão "Criar um pagamento com PayPal" usa o mesmo valor que você insere no campo "Descrição". Seu botão envia a descrição para o PayPal para complementar o nome do item nos avisos de autorização e nos detalhes da transação. Se você escrever o código HTML do seu botão manualmente, o valor max_texte o texto acima do botão podem ser diferentes.	Setembro de 2016
modifyOpcional	
Comportamento de modificação.

O valor válido é:

0Padrão. Permite que os assinantes se inscrevam apenas para novas assinaturas.
1Permite que os assinantes se inscrevam em novas assinaturas e modifiquem suas assinaturas atuais.
2Permite que os assinantes modifiquem apenas suas assinaturas atuais.
Comportamento de descontinuação : Os modifyvalores `true` 1e 2`false` estão obsoletos. Se o seu cliente clicar em um botão de modificar assinatura existente que usa um valor modifyde `true` 1ou `false` 2, o PayPal retornará a seguinte mensagem: `NotificationException: Not ... Things don't appear to be working at the moment. Please try again latermodify=0

Comprimento do caractere: 1	15 de março de 2019 para modify=1emodify=2
no_noteOpcional	
Não solicite aos compradores que incluam uma nota junto com seus pagamentos.

O valor válido é:

0. Forneça uma caixa de texto e um campo para inserir a anotação.
1. Oculte a caixa de texto e o prompt.
O valor padrão é 0. Para adicionar um rótulo de texto à caixa de texto, como por exemplo, Enter any special instructions:, utilize a cnvariável.

Comprimento do caractere: 1	Setembro de 2016
page_styleOpcional	
Estilo de página de pagamento personalizado para páginas de finalização de compra.

O valor válido é:

paypalUse o estilo de página do PayPal.
primaryUse o estilo de página que você definiu como principal no seu perfil de conta.
page_style_nameUse o estilo de página de pagamento personalizado do seu perfil de conta que tenha o nome especificado.
O padrão é primaryse você adicionou um estilo de página de pagamento personalizado ao seu perfil de conta. Caso contrário, o padrão é paypal.

Comprimento do caractere: 30	Setembro de 2016
_oe-gift-certificateOpcional	
O botão em que a pessoa clicou era um botão "Comprar Vale-Presente".

1º de fevereiro de 2017
usr_manageOpcional	
Configure para 1que o PayPal gere nomes de usuário e senhas iniciais para os assinantes.

Para obter mais informações, consulte Gerar nomes de usuário e senhas de assinantes .

Comprimento do caractere: 1