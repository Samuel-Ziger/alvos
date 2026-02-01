Referência da API NVP e SOAP
API
Última atualização: 13 de maio, 14h37


Aviso de descontinuação : as APIs NVP/SOAP do PayPal são APIs legadas, mas o PayPal continua a oferecer suporte a elas. Para acessar os recursos e a segurança mais recentes:

Para novas integrações, comece com as ferramentas mais recentes .
Para integrações existentes, visite a central de atualizações .
Aprenda como começar a usar aplicativos e credenciais de pares nome-valor (NVP) e protocolo de comunicação padrão (SOAP) do PayPal:

Aplicativos
Credenciais
SDKs
Use nossos SDKs para simplificar sua integração.

Operações disponíveis
Veja quais operações NVP e SOAP estão disponíveis:

NVP
SABÃO
Produtos NVP/SOAP
Veja os tipos de pagamento do PayPal disponíveis que usam NVP/SOAP:

Contas Adaptáveis
Gerenciador de botões
Botões para fazer login com o PayPal
Pagamento expresso
Faturamento
Pagamentos em massa
Portal de fluxo de pagamento
Serviço de Permissões

-------------------------------------------------------------------------------------------------------
Saiba mais sobre aplicativos de API NVP/SOAP
API
LEGADO
Última atualização: 18 de agosto, 14h12

Importante: NVP/SOAP é um método de integração legado. Aceitamos novas integrações e damos suporte às integrações existentes, mas existem soluções mais recentes. Se você estiver iniciando uma integração, recomendamos nossas soluções mais recentes .

As APIs NVP/SOAP do PayPal fornecem um conjunto de interfaces de programação de aplicativos (APIs) que permitem adicionar funcionalidades de pagamento e transação do PayPal aos seus aplicativos comerciais. As APIs NVP/SOAP oferecem interfaces que abrangem todos os aspectos de transações comerciais complexas, incluindo serviços de faturamento, notificações, serviços de assinatura, pagamentos paralelos, serviços de permissões e funcionalidades para processar reembolsos.

O ciclo de vida de desenvolvimento do PayPal
O ciclo de vida de desenvolvimento de alto nível é o mesmo para todos os serviços do PayPal. Você incorpora a funcionalidade de transações do PayPal em aplicativos da seguinte forma:

Cadastre-se como desenvolvedor do PayPal.
Integre as funcionalidades do PayPal ao seu site e aplicativos móveis.
Teste suas rotinas de transação do PayPal.
Publique seu aplicativo.
Faça a manutenção e a atualização do seu aplicativo.
O processo de desenvolvedor do PayPal
Veja como programar, testar e implementar seus aplicativos do PayPal:

Utilize sua conta PayPal existente para acessar o site de desenvolvedores do PayPal ou crie uma nova conta clicando no botão "Cadastrar-se" neste site.
Familiarize-se com a forma de fazer chamadas à API do PayPal, conforme descrito no Guia de Introdução às APIs do PayPal .
Crie contas virtuais de teste no ambiente de sandbox, conforme descrito no Guia de Testes do Sandbox do PayPal .
Utilize os endpoints e as credenciais corretas para o ambiente e as operações em questão.
Consulte a documentação de integração para obter orientações. Você também pode utilizar o site de demonstração do Express Checkout .
Teste suas rotinas do PayPal usando o Sandbox .
Publique seu aplicativo, conforme descrito em Publicar seu aplicativo .
Documentos de processo e política de desenvolvimento
Os documentos a seguir abordam as diferentes fases de desenvolvimento e descrevem as políticas do PayPal para desenvolvedores:

Políticas e diretrizes
Políticas e diretrizes de inscrição do PayPal
Processo de desenvolvimento
Guia de Introdução às APIs do PayPal
Guia de Testes Sandbox do PayPal
Lance seu aplicativo ao vivo
Visite a página de suporte no site de desenvolvedores do PayPal para obter recursos de suporte e informações adicionais.
-----------------------------------------------------------------------------------------------------
Criar e gerenciar credenciais de API NVP/SOAP
API
Última atualização: 22 de maio, 10h54


Aviso de descontinuação : as APIs NVP/SOAP do PayPal são APIs legadas, mas o PayPal continua a oferecer suporte a elas. Para acessar os recursos e a segurança mais recentes:

Para novas integrações, comece com as ferramentas mais recentes .
Para integrações existentes, visite a central de atualizações .
Ao usar as APIs NVP/SOAP do PayPal, você precisa autenticar cada solicitação por meio de um conjunto de credenciais de API . O PayPal associa essas credenciais a uma conta do PayPal. Você pode gerar credenciais para qualquer conta PayPal Business ou Premier.

Tipos de credenciais
Todas as credenciais de certificado da API do PayPal são certificados SHA-256 de 2048 bits que expiram a cada três anos.

As APIs NVP/SOAP suportam:

Certificados de API
Contém o nome de usuário e a senha da API, além do certificado.

O PayPal recomenda que você use credenciais de certificado por motivos de segurança. Consulte Certificados de API .

Assinaturas de API
Contém o nome de usuário e a senha da API, além da assinatura. Consulte Assinaturas da API .

Observação: Todas as APIs da plataforma Adaptive exigem que você forneça uma appIDassinatura ou um certificado. As APIs da Adaptive incluem Adaptive Payments, Adaptive Accounts, Permissions Service e Invoicing Service.

Certificados de API
Aprenda como criar e gerenciar credenciais de API de certificado.

Criar certificados de API
Observação: Se o seu certificado de API estiver expirando, prossiga para Renovar um certificado de API .

Para obter credenciais válidas, faça login na sua conta comercial do PayPal em www.paypal.com .

Para obter credenciais de teste, faça login no ambiente de testes do PayPal em www.sandbox.paypal.com com uma conta comercial de teste .


Passe o cursor sobre seu nome e clique em Configurações da conta no menu suspenso.

Na página de acesso à conta , clique em Atualizar para o item de acesso à API .

Clique em Gerenciar credenciais de API na seção Integração de API NVP/SOAP (Clássica) .

Observação: Se você já gerou um certificado de API, clicar em Gerenciar credenciais de API exibirá as informações do certificado. Se precisar gerar um certificado de API, clique em Remover certificado para excluir o certificado existente.

Na página Solicitar certificado de API , selecione Solicitar certificado de API .

Em seguida, clique em Concordar e Enviar .

A página Gerenciar certificado de API é exibida.

Clique em Baixar Certificado .

O cert_key_pem.txtarquivo contém o certificado. Salve o arquivo em um local seguro.

O PayPal formata o arquivo de certificado da API no formato PEM. O arquivo contém tanto o seu certificado público quanto a chave privada associada . Embora o certificado PEM não seja legível por humanos, o arquivo não é criptografado. Para obter detalhes, consulte Criptografar seu certificado .

Renovar certificados de API
Para evitar interrupções nos serviços da API, você deve renovar e substituir seu certificado antes que ele expire.

Para obter credenciais válidas, faça login na sua conta comercial do PayPal em www.paypal.com .

Para obter credenciais de teste, faça login no ambiente de testes (sandbox) do PayPal em www.sandbox.paypal.com com uma conta comercial de teste .


Passe o cursor sobre seu nome e clique em Configurações da conta no menu suspenso.

Na página de acesso à conta , clique em Atualizar para o item de acesso à API .

Clique em Gerenciar credenciais de API na seção Integração de API NVP/SOAP (Clássica) .

Verifique o status do seu certificado de API para confirmar se ele está ativo ou se expirará em breve .

Se o status for "Expira em breve" , clique em "Renovar certificado" .

Essa ação gera um certificado adicional com o status Ativo . A página Gerenciar certificado de API exibe ambos os certificados.

No certificado marcado como Ativo , clique em Baixar Certificado e siga os passos para baixar o certificado .

Após importar o novo certificado da API, teste sua integração para garantir que ela funcione corretamente com o certificado. Distribua o certificado a todos os parceiros afetados. Após o certificado antigo expirar, clique em " Remover Certificado" para removê-lo.

Criptografar certificados de API
Os SDKs do PayPal para Java e ASP.NET exigem que você criptografe o certificado no formato PKCS12 antes de poder usá-lo com os SDKs.

Observação: O SDK do PayPal para PHP não requer criptografia SSL.

Dica: Se você usar criptografia, certifique-se de criptografar tanto o certificado da sua API de teste quanto o da API de produção.

Os passos para criptografar seu certificado exigem a ferramenta de criptografia OpenSSL. Embora usuários de UNIX provavelmente já tenham essa ferramenta instalada em seu sistema operacional, usuários do Windows precisam baixar o OpenSSL. Para instalar o OpenSSL, aceite as configurações padrão.

Em um prompt de comando, verifique se o bindiretório do OpenSSL está no seu PATH do sistema. Caso contrário, adicione-o ao seu PATH.

Mude o diretório para o local do certificado a ser criptografado ( cert_key_pem.txt) e execute:

openssl pkcs12 - export - in cert_key_pem . txt - inkey cert_key_pem . txt - out paypal_cert . p12
Observação: ao criptografar um certificado, você será solicitado a inserir uma senha para descriptografar o arquivo. No campo "Inserir senha de exportação" , digite uma senha. Armazene-a em um local seguro.

O paypal_cert.p12arquivo contém seu certificado de API criptografado.

Instalar certificados de API para ASP.NET
Se você estiver desenvolvendo com o SDK do PayPal para ASP.NET, o Windows exige que você:

Importe o certificado para o repositório de certificados do Windows.

Para obter mais informações, consulte o artigo de suporte da Microsoft sobre importação ou exportação de certificados e chaves privadas .

Conceda ao usuário que executa seu aplicativo web acesso à sua chave privada.
Para obter mais informações, consulte Conceder permissões de terceiros .

Para obter mais informações, consulte o artigo da base de conhecimento do PayPal " Como importar meu certificado para o armazenamento de chaves do Windows?" .

Assinaturas de API
Para criar uma assinatura de API:

Para obter credenciais válidas, faça login na sua conta comercial do PayPal em www.paypal.com .

Para obter credenciais de teste, faça login no ambiente de testes (sandbox) do PayPal em www.sandbox.paypal.com com uma conta comercial de teste .


Passe o cursor sobre seu nome e clique em Configurações da conta no menu suspenso.

Na página de acesso à conta , clique em Atualizar para o item de acesso à API .

Clique em Gerenciar credenciais de API na seção Integração de API NVP/SOAP (Clássica) .

Observação: Se você já gerou uma assinatura de API, clicar em Gerenciar credenciais de API exibirá as informações da assinatura. Se precisar gerar uma assinatura de API, clique em Remover para excluir a assinatura de API existente.

Selecione " Solicitar assinatura da API" . Em seguida, clique em "Concordar e enviar" .
