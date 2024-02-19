# az-204-preparatorio

*Aplicativo do Azure*

- Capacidade de dimensionamento horizontal e vertical automatico
- Integração e implantação continua (Github, Bitbucket, Azure Devops)
- Slots de implantação separado do de produção.


*Serviço de Aplicativo no Linux*

- Suporte para diversas liguagens de programação
- Execução de conteiners personalizados do Linux (Aplicativos Web para Conteiners)
- Hospedagem de aplicativos nativamente no Linux com pilhas compativeis

*Slots de implantaçãO*

São aplicativos dinamicos que possuem seus proprios nomes de host.
Podem determinar os ambientes e camadas da aplicação

*Plano de serviço de aplicativo*

Planos de serviço, é o que define uma serie de configurações para hospedagem do aplicativo.
Os planos de serviço podem estar vinculados a um ou mais serviços de aplicativo, e os mesmos compartilharão seus recursos.

- Sistema operacional (Windows e Linux)
- Região (Oeste dos EUA, Leste dos EUA, etc.)
- Número de instâncias de VM
- Tamanho de instâncias de máquina virtual (pequeno, médio, grande)
- Tipo de preço (Gratuito, Compartilhado, Básico, Standard, Premium, PremiumV2, PremiumV3, Isolado, IsoladoV2)

*Categorias de planos de serviço*
- Computação compartilhada: grátis e compartilhada : camadas alocam cotas de CPU a cada aplicativo que é executado nos recursos compartilhados
- Computação dedicada:  as camadas Básica, Standard, Premium, PremiumV2 e PremiumV3 executam os aplicativos nas VMs dedicadas do Azure e somente os aplicativos no mesmo plano do serviço de aplicativo compartilham os mesmos recursos de computação.
- Isolado: as camadas Isolada e IsoladaV2 executam VMs do Azure em Redes Virtuais do Azure dedicadas. Ele fornece isolamento de rede na parte superior do isolamento de computação para seus aplicativos.


*Funcionamento do dimensionamento do aplicativo*
- Planos de serviço Gratuito e compartilhado não possuem opção de dimensionamento e a computação é compartilhada entre outros clientes
- Caso diversos aplicativos estejam vinculados ao mesmo plano, todos eles serão dimensionados simultaneamente, pois compartilham sua computação
- Caso possua diversos slots de implantação, todos os slots serão dimensionados

*Autenticação Interna*
A autenticação promove logon da aplicação de forma simplificada sem criação de codigos internos.
É realizado por meio do Azure Functions e possui integração com Entra Id, Twitter, Google e Facebook
Criado diretamente na plataforma

Funcionamento:
Bascamente isola a aplicação como uma camada a mais necessitando de autorização paa qualquer requisição HTTP.
Feito isso, ao autenticar com o ponto de extremidade do provedor, é armazenado o token na aplicação.
pode ser realizado injeção de informações de identidade nos cabeçalhos da requisição.

*Fluxo de autenticação*
- Sem SDK: utilizado geralmente para aplicativos de navegador, na qual apresentam a pagina do provedor para o usuario, direcionando o fluxo diretamente para o servidor
- Com SDK: Utilizado geralemtne para apliativos sem navegador, onde o processo de captação dos dados é feito internamente e manualmente, e após é redirecionado ao provedor.

*Registro em log e rastreamento*	
Ao ver um erro de autenticação não esperado, é possível localizar com facilidade todos os detalhes examinando os logs do aplicativo existente.

*Token Store*
O Serviço de Aplicativo fornece um armazenamento de token integrado, que é um repositório de tokens associados aos usuários dos aplicativos Web, APIs ou aplicativos móveis nativos

*Recursos de rede Serviço e aplicativo*
- Serviço publico multilocatario: hospeda planos do Serviço de Aplicativo nas SKUs Gratuitas, Compartilhadas, Básicas, Standard, Premium, PremiumV2 e PremiumV3
- Serviço ASE (Ambiente do Serviço de Aplicativo) : de locatário único, que hospeda planos do Serviço do Aplicativo de SKU Isolado diretamente na rede virtual do Azure.


*Recursos de rede do Serviço de Aplicativo multilocatário*

Funções que tratam solicitações HTTP e HTTPS -> FrontEnd
Funções que hospedam a carga de trabalho -> Trabalhos

Como há muitos clientes diferentes na mesma unidade de escala do Serviço de Aplicativo, não é possível conectar a rede do Serviço de Aplicativo diretamente à sua rede.

*Definir as configurações gerais*

- Configuração de pilha -> linguagem para executar a aplicação, incluindo versões e SDK
- Configurações de plataforma
	- Número de bits -> 32 bits ou 64 bits.
	- Protocolo WebSocket 
	- Always On -> Mantenha o aplicativo carregado mesmo quando não existe trafego. Por padrão é desativado.
	- Versão do pipeline gerenciado -> modo de pipeline do IIS
	- HTTP versão -> Suporte para ativação do HTTP/2
	- Afinidade de ARR ->  em uma implantação de várias instâncias, verifique se o cliente é roteado para a mesma instância durante a vida útil da sessão
- Deupuração -> habilita a depuração remota para aplicativos ASP.NET, ASP.NET Core ou Node.js
- Certificados de cliente de entrada ->  exigem certificados de cliente na autenticação mútua.

*Configurar mapeamentos de caminho"
Na seção Configuração > Mapeamentos de caminho é possivel configurar o mapeamento de aplicativo virtual e diretorios. As opções de mapeamento é baseada no SO

*Configurar mapeamento de aplicativos Windows*

Os mapeamentos do manipulador permite a adição de processadores de script personalizados para manipular solicitações para extensões de arquivo especificadas.
Cada aplicativo tem o caminho raiz padrão (/) mapeado para D:\home\site\wwwroot, em que seu código é implantado por padrão. Se a raiz do aplicativo estiver em uma pasta diferente ou se o repositório tiver mais de um aplicativo,
 você poderá editar ou adicionar diretórios e aplicativos virtuais.

*Aplicativos Linux e em contêineres*

configure seu armazenamento personalizado da seguinte maneira:

    Nome: o nome de exibição.
    Opções de configuração: básica ou avançada.
    Contas de armazenamento: a conta de armazenamento com o contêiner desejado.
    Tipo de armazenamento: Blobs do Azure ou Arquivos do Azure. Os aplicativos de contêiner do Windows são compatíveis somente com os Arquivos do Azure.
    Contêiner de armazenamento: para configuração básica, o contêiner desejado.
    Nome do compartilhamento: para configuração avançada, o nome do compartilhamento de arquivos.
    Chave de acesso: para configuração avançada, a chave de acesso.
    Caminho de montagem: o caminho absoluto em seu contêiner para montar o armazenamento personalizado.

*Habilitar registro em log diagnostico*

Consulte os modelos em: https://learn.microsoft.com/pt-br/training/modules/configure-web-app-settings/5-enable-diagnostic-logging

*Habilitar o log de aplicativo (Windows)*

- Habilite os logs dos aplicativos em: Logs do Serviço de Aplicativo.
- Selecione ativado para o registro em log do aplicativo (Filesystem) ou log de aplicativo (BLOB) ou ambos.

A opção Filesystem é para fins de depuração temporária e fica desativada em 12 horas. A opção blob é para o log de longo prazo e precisa de um contêiner
 de armazenamento de blobs no qual os logs serão gravados.

*Configurar certificados de segurança*

Os certificados carregados em um aplicativo é armazenado em uma unidade, na qual e vinculada em um grupo de recursos, tornando-se acessivel para outros aplicativos 

Confira os tipos de certificados disponiveis para implantação: https://learn.microsoft.com/pt-br/training/modules/configure-web-app-settings/6-configure-security-certificates

*Importando um certificado privado*

Se você quiser usar um certificado privado no Serviço de Aplicativo, seu certificado deverá atender aos seguintes requisitos:
    Exportado como um arquivo PFX protegido por senha, criptografado usando DES triplo.
    Conter chave privada com pelo menos 2.048 bits de extensão
    Conter todos os certificados intermediários na cadeia de certificados

Para proteger um domínio personalizado em uma associação TLS, o certificado tem outros requisitos :
    Contém um Uso estendido de chave para autenticação do servidor (OID = 1.3.6.1.5.5.7.3.1)
    Assinado por uma autoridade de certificado confiável


*Criar um certificado gratuito*

O Certificado gerenciado gratuito do Serviço de Aplicativo é uma solução imediata para proteção de seu nome DNS personalizado no Serviço de Aplicativo.
É um certificado do servidor TLS/SSL totalmente gerenciado pelo Serviço de Aplicativo e renovado contínua e automaticamente em incrementos de seis meses,
45 dias antes da expiração.


