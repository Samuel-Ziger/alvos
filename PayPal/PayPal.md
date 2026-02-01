Resumo — Campanha Bug Bounty PayPal (Checkout)

A campanha de recompensas por bugs do PayPal, hospedada na HackerOne, é focada exclusivamente nos fluxos de finalização de compra (checkout) e integrações de pagamento. O objetivo central é identificar vulnerabilidades técnicas, falhas lógicas e vetores de fraude que possam comprometer transações, usuários, comerciantes e a confiança na plataforma.

Visão geral

Campanha ativa, com término em 27 dias

Alta eficiência operacional

+90% de taxa de resposta

Primeira resposta em média: 6 horas

Pagamento médio da recompensa: ~1 mês e 3 semanas

Resolução média: ~3 meses e 2 semanas

Ativo elegível principal: SDK do PayPal

Gravidades Alta e Crítica recebem 1,5x o valor padrão da recompensa

Minha opinião: é uma das campanhas mais maduras do mercado — métricas boas, escopo claro e foco real em impacto, não só CVSS.

O que a campanha busca

Além de vulnerabilidades clássicas, o PayPal valoriza muito falhas de lógica e cenários de abuso, especialmente:

Pagamentos não autorizados

Manipulação de valores, moedas ou transações

Bypass de autenticação, autorização ou sessões

Fraudes escaláveis e falhas de design

Exploração de fluxos de checkout e pedidos

👉 Aqui está o diferencial: fraude e abuso contam tanto quanto bugs técnicos.

Sistemas e APIs no escopo

Inclui todos os fluxos de checkout PayPal, com destaque para:

APIs Orders (v1/v2)

APIs Payments (v1/v2)

SDK JavaScript do PayPal

WPS (Website Payments Standard)

NVP / SOAP

NCPS (No Code Payment Solution)

Pesquisadores podem integrar essas soluções em aplicações próprias de teste, permitindo testes ponta a ponta.

Recompensas

Valor mínimo: US$ 50

Valor máximo: US$ 30.000

Faixas principais:

Crítico: US$ 20.000 – 30.000

Alto: US$ 10.000 – 20.000

Médio: US$ 1.000 – 10.000

Baixo: US$ 50 – 1.000

Valores médios pagos recentemente:

Baixo: ~US$ 700

Médio: ~US$ 2.700

Alto: ~US$ 9.170

Crítico: ~US$ 11.280

💡 O pagamento não depende só do CVSS, mas do impacto real demonstrado (financeiro, reputacional, operacional e legal).

Vulnerabilidades aceitas (exemplos)

IDOR, auth bypass, session flaws

XSS, CSRF sensível, SQL/XML Injection

RCE (com regras extremamente rígidas)

Exposição de dados sensíveis

Falhas graves de configuração

Vulnerabilidades em IA/ML do PayPal

Fora do escopo (resumo)

Engenharia social

Resultados de scanners automáticos

Open Redirect sem impacto real

Problemas puramente teóricos

DoS em produção

DDoS (proibido)

Vulnerabilidades apenas em sandbox (exceto Braintree)

Bibliotecas de terceiros sem impacto comprovado

Regras críticas

Nada de testes disruptivos em produção

DoS somente em sandbox

Webshells são explicitamente proibidos

Não modificar arquivos nem acessar dados além do necessário

Confidencialidade total: não divulgar nada publicamente

Relatórios devem ser claros, técnicos e bem documentados

Marcas cobertas

Inclui:

PayPal

Venmo

Xoom

Braintree

Hyperwallet

Swift Financial

Exclui:

Zettle

Paidy

Honey

Tradera

outras marcas não listadas