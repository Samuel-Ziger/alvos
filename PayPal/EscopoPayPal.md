Escopo válido — SOMENTE DOMÍNIOS
Domínios Críticos (Elegíveis)

Esses são os principais alvos de alto valor para bug bounty:

py.pl

paypal.me

*.paypal.com

*.paypalcorp.com

*.xoom.com

*.venmo.com

*.paylution.com

*.paydiant.com

*.hyperwallet.com

*.braintreepayments.com
⚠️ Preferir testes em: *.sand.braintreepayments.com

*.braintreegateway.com

*.braintree-api.com
⚠️ Preferir testes em: *.sandbox.braintree-api.com

*.braintree.tools
ℹ️ Ambiente de desenvolvimento → impacto e recompensa geralmente menores

👉 Opinião direta: *.paypal.com e *.braintree são onde está o ouro*, especialmente em APIs e fluxos de pagamento.

Domínios de Gravidade Média (Elegíveis)

sandbox.braintreegateway.com

paypalobjects.com

Domínios de Baixa Gravidade (Elegíveis)

Normalmente ligados a serviços financeiros auxiliares / APIs específicas:

www.swiftfinancial.com

swiftfinancial.com

www.swiftcapital.com

swiftcapital.com

www.loanbuilder.com

loanbuilder.com

my.swiftfinancial.com

my.loanbuilder.com

api.swiftfinancial.com

api.loanbuilder.com

scrutiny.swiftfinancial.com

prequal.swiftfinancial.com

pigeon.swiftfinancial.com

decision.swiftfinancial.com

parceiro.swiftfinancial.com

ℹ️ Observação importante: vários desses domínios retornam erro na URL raiz, mas as APIs funcionam normalmente (isso não é bug por si só).

Domínios Elegíveis com Restrições

www.paypal-*.com

Sites parceiros / marketing
⚠️ Muitos estão em processo de desativação
⚠️ Bugs em sites prestes a sair do ar não são pagos

Domínios FORA do escopo (Inelegíveis)

Esses não devem ser testados para bug bounty PayPal:

www.gopay.com

→ Relatórios devem ser enviados para Paypal.vulbox.com

braintree.com
❌ Não pertence ao PayPal

*.paypal.cn
→ Programa separado (Paypal.vulbox.com)

*.atlassian.net