# T1 - Tutorial 1 - Criando uma conta na OCI

Abra a sua conta trial na Oracle Cloud Infrastructure.

Siga este link:

    https://videohub.oracle.com/media/1_m4904ocr


----
# T2 - Tutorial 2 - Instalando o CLI

O OCI CLI (Command Line Interface) permite trabalhar com os recursos da Cloud Oracle através de linhas de comando.
Além disto, para trabalhar com a linha de comandos para Kubernetes (kubectl), é necessário integrar o kubectl com o OCI CLI.
Neste tutorial iremos instalar o CLI como prévia para que você possa então, nos próximos passos, configurar seu kubectl para integrar-se à OCI.
Vamos lá.

    O script do instalador instala automaticamente o CLI e suas dependências, Python e virtualenv. 

**Antes de executar o instalador, certifique-se de atender aos requisitos:**

Versões e sistemas operacionais compatíveis com Python

- A CLI é compatível com Python versões 2.7+ e 3.5 a 3.8 em execução em MacOS, Windows ou uma distribuição Linux compatível:

- Oracle Linux 6.10, Oracle Linux 7.7 e 7.8 e Oracle Linux 8.0
- Oracle Autonomous Linux 7.8
- CentOS 6.9, CentOS 6.10 e CentOS 7.0
- Ubuntu 16.04, Ubuntu 18.04 e Ubuntu 20.04

### T2.1 - Linux e Unix (incluindo Oracle Linux 8)

**T2.1.1** Abra um terminal.

**T2.1.2** Para executar o script do instalador, execute o seguinte comando.

    bash -c "$(curl -L https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.sh)"

**Nota**

    Para executar uma instalação 'silenciosa' que aceita todos os valores padrão sem prompts, use o parâmetro --accept-all-defaults

**T2.1.3** Responda às solicitações do script de instalação .

### T2.2 - Oracle Linux 7

Se você estiver usando o Oracle Linux 7, você pode usar o yum para instalar o CLI.

**T2.2.1** Para usar o yum para instalar a CLI:

    sudo yum install python36-oci-cli

A CLI será instalada nos pacotes do site Python:

    /usr/lib/python3.6/site-packages/oci_cli
    /usr/lib/python3.6/site-packages/services

A documentação e os exemplos serão instalados no /usr/share/doc/python36-oci-cli-<version>/diretório.

**T2.2.2** Para desinstalar a CLI:

    sudo yum remove python36-oci-cli

### T2.3 - Mac OS X

Você pode usar o Homebrew para instalar, atualizar e desinstalar o CLI no Mac OS.

**T2.3.1** Para instalar a CLI no Mac OS X com Homebrew:

    brew update && brew install oci-cli

**T2.3.2** Para atualizar sua CLI, instale no Mac OS X usando o Homebrew:

    brew update && brew upgrade oci-cli

**T2.3.3** Para desinstalar a CLI no Mac OS X usando o Homebrew:

    brew uninstall oci-cli

### T2.4 - Windows

**T2.4.1** Abra o console do PowerShell usando a opção Executar como administrador .

O instalador permite o preenchimento automático instalando e executando um script. Para permitir que este script seja executado, você deve habilitar a política de execução **RemoteSigned**.

Para configurar a política de execução remota para PowerShell, execute o seguinte comando.

    Set-ExecutionPolicy RemoteSigned

**T2.4.2** Baixe o script do instalador:

    Invoke-WebRequest https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1 -OutFile install.ps1

**T2.4.3** Execute o script do instalador com ou sem prompts:

**T2.4.3.1** Para executar o script do instalador com prompts, execute o seguinte comando:

    iex ((New-ObjectSystem.Net.WebClient) .DownloadString ('https://raw.githubusercontent.com/oracle/oci-cli/master/scripts/install/install.ps1'))

... e responda às solicitações do script de instalação .


**T2.4.4** Para executar o script do instalador sem avisar o usuário, aceitando as configurações padrão, execute o seguinte comando:


       install.ps1 -AcceptAllDefaults 


### T2.5 - Instruções do script de instalação

O script de instalação solicita as seguintes informações.

    Se você não tiver uma versão compatível do Python instalada:

        Windows e Linux: Você é solicitado a fornecer um local para instalar os binários e executáveis. O script instalará o Python para você.

        MacOS: você é notificado de que sua versão do Python é incompatível. Você deve atualizar antes de prosseguir com a instalação. O script não instalará o Python para você.

    Quando solicitado a atualizar o CLI para a versão mais recente, responda com Y para sobrescrever uma instalação existente.
    Quando solicitado a atualizar seu PATH, responda com Y para poder invocar a CLI sem fornecer o caminho completo para o executável. Isso adicionará oci.exe ao seu PATH.

### T2.6 - Configurando o arquivo config

Antes de usar a CLI, você deve criar um arquivo de configuração que contém as credenciais necessárias para trabalhar com o **Oracle Cloud Infrastructure** .

Para isto, vamos seguir o roteiro:
- Buscar as informações de Tenant e Usuário (OCID)
- Subir a chave pública para o seu usuário
- Executar o comando para configurar o OCI CLI

Vamos lá

 
**T2.6.1 - OCI do Tenant**

**T2.6.1.1** Abra o menu de navegação , em Governança e administração

**T2.6.1.2** Vá para Administração e clique em Detalhes de tenant .

**T2.6.1.3** O OCID do tenant é mostrado em Informações de tenant . 

**T2.6.1.4** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.2 - OCI do Usuário**

**T2.6.2.1** Se você estiver conectado como o usuário:
**T2.6.2.1.1** Abra o menu Perfil (Menu Hambúrger) e clique em Configurações do usuário .

**T2.6.2.1.2** Se você for um administrador fazendo isso para outro usuário: Abra o menu de navegação . Em Governança e Administração , acesse Identidade e clique em Usuários . Selecione o usuário na lista.

**T2.6.2.1.3** O OCID do usuário é mostrado em Informações do usuário . 

**T2.6.2.1.4** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.3 - Como fazer upload da chave pública**

Você pode fazer upload da chave pública PEM no console , que pode ser acessado fazendo login aqui: https://cloud.oracle.com

**T2.6.3.1** Abra o Console , e login.

**T2.6.3.2** Visualize os detalhes do usuário que chamará a API com o par de chaves:

**T2.6.3.2.1** Se você estiver conectado como o usuário:

- Abra o menu Perfil (Menú Hambúrger) e clique em Configurações do usuário .

**T2.6.3.2.2** Se você for um administrador fazendo isso para outro usuário: 
- Abra o menu de navegação . 
- Em Governança e Administração , acesse Identidade e clique em Usuários . 
- Selecione o usuário na lista.

**T2.6.3.2.3** Clique em Adicionar chave pública .

**T2.6.3.2.4** Cole o conteúdo da chave pública PEM na caixa de diálogo e clique em Adicionar .

    A impressão digital da chave é exibida (por exemplo, 12: 34: 56: 78: 90: ab: cd: ef: 12: 34: 56: 78: 90: ab: cd: ef).

**T2.6.4 Configurando OCI CLI**

Para que a CLI o guie pelo processo de configuração inicial, use o comando:

    oci setup config


**O comando solicita as informações necessárias para o arquivo de configuração e as chaves públicas / privadas da API.**

    A caixa de diálogo de configuração gera um par de chaves API e cria o arquivo de configuração.

Você vai precisar de algumas informações para preencher a configuração quando executar o comando **oci setup config**. Estas informações você anotou nos passos anteriores.

- OCID do Tenant
- OCID do seu Usuário na Cloud Oracle


**T2.6.5 - Testando o OCI CLI**

Para testar suas configurações, execute o seguinte comando:

    $ oci iam user list

Você deve obter algo como:

	{
	  "data": [
	    {
	      "capabilities": {
	        "can-use-api-keys": true,
	        "can-use-auth-tokens": true,
	        "can-use-console-password": true,
	        "can-use-customer-secret-keys": true,
	        "can-use-o-auth2-client-credentials": true,
	        "can-use-smtp-credentials": true
	      },
	      "compartment-id": "ocid1.tenancy.oc1..aaaaaaaarmfxxxxxxxxxxxxyazqycjapqsyaxxxxxxxxxxxxxxbyb6j6q",
	      "defined-tags": {},
	      "description": "Cristiano Hoshikawa",
	      "email": "teste@teste.com",
	      "email-verified": true,
	      "external-identifier": null,
	      "freeform-tags": {},
	      "id": "ocid1.user.oc1..aaaaaaxccccccccccccdpmntisfgxeizqwwwwwewwweeeesol4f3keeeeeexmlcmphda",
	      "identity-provider-id": null,
	      "inactive-status": null,
	      "is-mfa-activated": false,
	      "lifecycle-state": "ACTIVE",
	      "name": "teste@teste.com",
	      "time-created": "2020-04-17T10:57:09.254000+00:00"
	    }
	}


----

# T3 - Tutorial 3 - Criando a Instância do OKE

**Este tutorial de 10 minutos mostra como:**

- Criar um novo cluster com configurações padrão e novos recursos de rede
- Criar um pool de nós
- Configurar o arquivo kubeconfig para o cluster
- Verificar se você pode acessar o cluster usando kubectl e o painel Kubernetes

O Oracle Cloud Infrastructure Container Engine para Kubernetes é um serviço totalmente gerenciado, escalonável e altamente disponível que você pode usar para implantar seus aplicativos em contêineres na nuvem. Use o Container Engine para Kubernetes quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de maneira confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o Container Engine for Kubernetes os provisiona no Oracle Cloud Infrastructure em uma tenant OCI existente.

Neste tutorial, você usa as configurações padrão para definir um novo cluster. Quando você cria o novo cluster, novos recursos de rede para o cluster são criados automaticamente, junto com um pool de nós e três nós de trabalho privados. Em seguida, você configurará o arquivo de configuração do Kubernetes para o cluster (o arquivo 'kubeconfig' do cluster). O arquivo kubeconfig permite que você acesse o cluster usando kubectl e o painel Kubernetes.

**Para isto, você necessitará:**

- Uma máquina local (Windows, Linux ou Mac)
- Chaves de acesso (pública e privada)

- Um nome de usuário e senha do Oracle Cloud Infrastructure.
- Dentro da sua tenant, já deve haver um compartimento para conter os recursos de rede necessários (VCN, sub-redes, gateway de internet, gateway de NAT, tabela de rotas, listas de segurança). Se tal compartimento ainda não existir, você terá que criá-lo antes de iniciar este tutorial.
- Pelo menos três instâncias de computação devem estar disponíveis na tenant para concluir este tutorial conforme descrito. Observe que, se apenas uma instância de computação estiver disponível, é possível criar um cluster com um pool de nós que possui uma única sub-rede e um único nó no pool de nós. No entanto, esse cluster não estará altamente disponível.
- Para criar e / ou gerenciar clusters, você deve pertencer a um dos seguintes:
 - O grupo Administradores da tenant.
 - Um grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões apropriadas sobre esses recursos. Para mais detalhes e exemplos, consulte o tópico Configuração de política para criação e implantação de cluster na documentação do Container Engine para Kubernetes.
- Antes de configurar o arquivo kubeconfig posteriormente no tutorial, você já deve ter feito o seguinte (se não tiver feito ou não tiver certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes):

****** REVISAR
 - gerou um par de chaves de assinatura de API
 - adicionou o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para o seu nome de usuário
 - instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior)
- Você deve ter instalado e configurado a ferramenta de linha de comando do Kubernetes kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .


### T3.1. Criando a Instância do OKE

**T3.1.1.** Em um navegador, acesse o url que você recebeu para fazer login no Oracle Cloud Infrastructure.

Página de login
![Descrição da ilustração](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-login-page.png)

**T3.1.2.** Especifique um username no qual você tenha as permissões apropriadas para criar clusters. Você herda essas permissões de uma das seguintes maneiras:
 - Por pertencer ao grupo Administradores da tenant.
 - Por pertencer a outro grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões listadas em O que você precisa? seção.

**T3.1.3.** Digite seu nome de usuário e senha.

---

### T3.2. Definir os detalhes do cluster

**T3.2.1.** No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Clusters Kubernetes.

**T3.2.2.** Escolha um compartimento no qual você tenha permissão para trabalhar e no qual deseja criar o novo cluster e os recursos de rede associados.

![Página de clusters](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-console-create-cluster.png)

**T3.2.3.** Na página Clusters , clique em Criar Cluster .

**T3.2.4.** Na caixa de diálogo Criar Cluster , clique em Criação Rápida e em Iniciar Fluxo de Trabalho.

![Caixa de diálogo Criar Solução de Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-solution-v2.png)

**T3.2.5.** Na página Criar Cluster , altere o valor do marcador no campo Nome e digite Tutorial Cluster.

![Criação de Cluster - página Criar Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-complete-top-v2.png)

**T3.2.6.** Clique em Avançar para revisar os detalhes inseridos para o novo cluster.
![Criação de cluster - página de revisão](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-review-top-v2.png)

**T3.2.7.** Na página Revisar , clique em Criar Cluster para criar os novos recursos de rede e o novo cluster.
Você vê os diferentes recursos de rede sendo criados para você.

![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-top-v1.png)


**T3.2.8.** Clique em Fechar para retornar ao console.
![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-bottom-v2.png)


**T3.2.9.** O novo cluster é mostrado na página Detalhes do Cluster . Depois de criado, o novo cluster tem o status Ativo.
![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-active.png)


**T3.2.10.** Em Recursos , selecione Pools de nós e clique no nome do pool de nós no cluster que você acabou de criar (pool1). Em Recursos , selecione Nós e role para baixo para ver os detalhes dos novos nós de trabalho (instâncias de computação) no pool de nós.

![Recursos](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-nodepool.png)

----

### T3.3. Configure o arquivo kubeconfig para o cluster

**T3.3.1.** Confirme que você já fez o seguinte:
 - Gerou um par de chaves de assinatura de API.
 - Adicionado o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para seu nome de usuário.
 - Instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior).
**** REVISAR
Se você não fez uma ou mais das opções acima, ou não tem certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes.

**T3.3.2.** Com a página Node Pools mostrando detalhes de pool1, clique em Tutorial Cluster no caminho de navegação. Clique em Access Cluster para exibir a caixa de diálogo Access Your Cluster e, em seguida, clique em Local Access .
Como acessar a caixa de diálogo do Kubeconfig
Descrição da ilustração

**T3.3.3.** Em uma janela de terminal, crie um diretório para conter o arquivo kubeconfig, fornecendo ao diretório o nome e a localização padrão esperados $HOME/.kube. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 


       $ mkdir -p $ HOME / .kube

**T3.3.4.** Execute o comando CLI do Oracle Cloud Infrastructure para configurar o arquivo kubeconfig e salve-o com o nome e localização padrão esperados $HOME/.kube/config. Esse nome e local garantem que o arquivo kubeconfig esteja acessível para kubectl e o painel do Kubernetes sempre que você executá-los em uma janela de terminal. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ):

    $ oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaae ... --file $ HOME / .kube / config --region us-phoenix-1 --token-version 2.0.0

onde ocid1.cluster.oc1.phx.aaaaaaaaae ... é o OCID do cluster atual. Por conveniência, o comando na caixa de diálogo Acessar seu cluster já inclui o OCID do cluster.

**T3.3.5.** Configure o valor da variável de ambiente KUBECONFIG para o nome e localização do arquivo kubeconfig. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 

    $ export KUBECONFIG = $ HOME / .kube / config

**T3.3.6.** Clique em Fechar para fechar a caixa de diálogo Acessar seu cluster .

----

### T3.4. Verifique o acesso do painel de kubectl e Kubernetes ao cluster

**T3.4.1.** Confirme se você já instalou o kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .

**T3.4.2.** Verifique se você pode usar kubectl para se conectar ao novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl get nodes

Você vê detalhes dos nós em execução no cluster. Por exemplo:

    NOME STATUS ROLES IDADE VERSÃO
    10.0.10.2 Nó pronto 1d v1.13.5
    10.0.11.2 Nó pronto 1d v1.13.5
    10.0.12.2 Nó pronto 1d v1.13.5

**T3.4.3.** Implante o painel do Kubernetes no novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

**T3.4.4.** Verifique se você pode usar o painel Kubernetes para se conectar ao cluster:

**T3.4.4.1** Em um editor de texto, crie um arquivo chamado oke-admin-service-account.yaml com o seguinte conteúdo:

    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: oke-admin
      namespace: kube-system
    ---
    apiVersion: rbac.authorization.k8s.io/v1beta1
    kind: ClusterRoleBinding
    metadata:
      name: oke-admin
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: cluster-admin
    subjects:
      - kind: ServiceAccount
        name: oke-admin
        namespace: kube-system 


  	    
O arquivo define uma conta de serviço de administrador e um clusterrolebinding, ambos chamados oke-admin.

**T3.4.4.2** Crie a conta de serviço e o clusterrolebinding no cluster inserindo:

    $ kubectl apply -f oke-admin-service-account.yaml

A saída do comando acima confirma a criação da conta de serviço e o clusterrolebinding:

    serviceaccount "oke-admin" created
    clusterrolebinding.rbac.authorization.k8s.io "oke-admin" created

Agora você pode usar a conta de serviço oke-admin para visualizar e controlar o cluster e se conectar ao painel do Kubernetes.

**T3.4.4.3** Obtenha um token de autenticação para a conta de serviço oke-admin inserindo:

    $ kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep oke-admin | awk '{print $1}')

A saída do comando acima inclui um token de autenticação (uma longa string alfanumérica) como o valor do token:elemento, conforme mostrado abaixo:

    Name: oke-admin-token-gwbp2
    Namespace: kube-system
    Labels: <none>
    Annotations: kubernetes.io/service-account.name: oke-admin
    kubernetes.io/service-account.uid: 3a7fcd8e-e123-11e9-81ca-0a580aed8570
    Type: kubernetes.io/service-account-token
    Data
    ====
    ca.crt: 1289 bytes
    namespace: 11 bytes
    token: eyJh______px1QQ

No exemplo acima, eyJh______px1Q(abreviado para legibilidade) é o token de autenticação.

**T3.4.4.4** Copie o valor do token:elemento da saída. Você usará esse token para se 
conectar ao painel.

**T3.4.4.5** Em uma janela de terminal, digite o seguinte comando:

    $ kubectl proxy

**T3.4.4.6** Abra uma nova janela do navegador e acesse 

    http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login
    
    
 para exibir o painel do Kubernetes.
 

![Página de login do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-sign-in.png)

**T3.4.4.7** Selecione a opção Token e cole o valor do token:elemento que você copiou anteriormente no campo Token .

**T3.4.4.8** Clique em Entrar .

**T3.4.4.9** Clique em Visão geral para ver se o Kubernetes é o único serviço em execução no cluster.

![Página de visão geral do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-overview.png)

**T3.4.4.10** Parabéns! Você criou com êxito um novo cluster e confirmou que o novo cluster está instalado e funcionando conforme o esperado.

----

# T4 - Tutorial 4 - Usando o OCIR (Oracle Cloud Infrastructure Registry)


Este tutorial de 10 minutos mostra como:

- criar um token de autenticação para usar com o Oracle Cloud Infrastructure Registry
- faça login no Oracle Cloud Infrastructure Registry a partir do Docker CLI
- extrair uma imagem de teste do DockerHub
- colocando uma tag em uma imagem
- envie a imagem para o Oracle Cloud Infrastructure Registry usando Docker CLI
- verificar se a imagem foi enviada para o Oracle Cloud Infrastructure Registry usando o console
fundo

### T4.1 - Introdução

O Oracle Cloud Infrastructure Registry é um registro gerenciado pela Oracle que permite simplificar o fluxo de trabalho de desenvolvimento para produção. O Oracle Cloud Infrastructure Registry torna mais fácil para você, como desenvolvedor, armazenar, compartilhar e gerenciar artefatos de desenvolvimento como imagens Docker. E a arquitetura altamente disponível e escalonável do Oracle Cloud Infrastructure garante que você possa implantar seus aplicativos de maneira confiável. Assim, você não precisa se preocupar com problemas operacionais ou com o dimensionamento da infraestrutura subjacente.

Neste tutorial, você primeiro criará um token de autenticação para acessar o Oracle Cloud Infrastructure Registry. Em seguida, você obterá uma imagem de teste do DockerHub e dará a ela uma nova tag. A nova tag identifica a região, tenant e repositório do Oracle Cloud Infrastructure Registry para o qual você deseja enviar a imagem.

Depois de fornecer a tag à imagem, você a envia por push para o Oracle Cloud Infrastructure Registry usando o Docker CLI. Por fim, você verificará se a imagem foi enviada com sucesso, visualizando o repositório que foi criado.

### T4.2 - O que você precisa?

Um nome de usuário e senha do Oracle Cloud Infrastructure.
Para enviar imagens para o Oracle Cloud Infrastructure Registry, você deve pertencer a um dos seguintes:
- O grupo Administradores da tenant.
- Um grupo ao qual uma política concede as permissões apropriadas do Oracle Cloud Infrastructure Registry, incluindo a permissão REPOSITORY_CREATE (consulte o tópico Políticas para controlar o acesso ao repositório na documentação do Oracle Cloud Infrastructure Registry).
- Acesso ao Docker CLI. Por exemplo, para enviar e receber imagens em uma máquina cliente local, conforme descrito neste tutorial, você precisará ter instalado o Docker na máquina local. (Como alternativa, você pode usar o Docker no ambiente do Cloud Shell.)

### T4.3 - Iniciar Oracle Cloud Infrastructure

**T4.3.1** Em um navegador, acesse o url que você recebeu para fazer login no Oracle Cloud Infrastructure.

![Página de login](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-login-page.png)

**T4.3.2** Especifique o seu tenant na qual você tenha as permissões apropriadas para criar repositórios no Oracle Cloud Infrastructure Registry. Você herda essas permissões de uma das seguintes maneiras:

- por pertencer ao grupo Administradores do locatário
- por pertencer a outro grupo ao qual uma política concede as permissões apropriadas do Oracle Cloud Infrastructure Registry, incluindo a permissão REPOSITORY_CREATE

Este tutorial assume que a tenant é acme-dev.

**T4.3.3** Digite seu nome de usuário e senha e clique em Entrar . 

Este tutorial assume que o nome de usuário é jdoe@acme.com.

### T4.4 - Obtenha um token de autenticação

**T4.4.1** No canto superior direito do console, abra o menu Usuário ( Menu do usuário) e clique em Configurações do usuário .

![Tela do console](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-console-settings.png)

**T4.4.2** Na página Tokens de autenticação , clique em Gerar token .

![Caixa de diálogo de geração de token](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-auth-tokens.png)

**T4.4.3** Insira Tutorial auth tokenuma descrição amigável para o token de autenticação e clique em Gerar token . O novo token de autenticação é exibido.

**T4.4.4** Copie o token de autenticação imediatamente para um local seguro de onde você possa recuperá-lo mais tarde, porque você não verá o token de autenticação novamente no console.

**T4.4.5** Feche a caixa de diálogo Gerar token.

Confirme se você pode acessar o Oracle Cloud Infrastructure Registry:
- No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Container Registry .
- Escolha a região em que você estará trabalhando (por exemplo, us-phoenix-1). 
- Revise os repositórios que já existem. Este tutorial assume que nenhum repositório foi criado ainda.

![Página de registro](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-registry-no-images.png)

### T4.5 - Faça login no Oracle Cloud Infrastructure Registry a partir do Docker CLI

**T4.5.1** Em uma janela de terminal na máquina cliente que executa o Docker, faça login no Oracle Cloud Infrastructure Registry digitando:

    docker login <region-key>.ocir.io
    
onde **<region-key>** está a chave para a região do Oracle Cloud Infrastructure Registry que você está usando. 

    Por exemplo phx,. Consulte o tópico Disponibilidade por região na documentação do Oracle Cloud Infrastructure Registry.
    DICA: As regiões são as siglas dos aeroportos. Por exemplo: GRU - Guarulhos-SP, IAD - Ashburn, etc

**T4.5.2** Quando solicitado, digite seu nome de usuário no formato <tenancy-namespace>/<username>. 

Por exemplo ansh81vru1zp/jdoe@acme.com,. Se a sua tenant for federada com o Oracle Identity Cloud Service, use o formato <tenancy-namespace>/oracleidentitycloudservice/<username>.

**T4.5.3** Quando solicitado, digite o token de autenticação que você copiou anteriormente como a senha.

![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-login.png)

### T4.6 - Extraia a imagem hello-world do DockerHub

Em uma janela de terminal na máquina cliente que executa o Docker, digite docker pull karthequian/helloworld:latestpara recuperar a versão mais recente da imagem hello-world do DockerHub.

![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-pull.png)

As diferentes camadas da imagem do helloworld são puxadas separadamente.

### T4.7 - Marque a imagem para push

Em uma janela de terminal na máquina cliente que executa o Docker, dê uma tag à imagem que você vai enviar para o Oracle Cloud Infrastructure Registry inserindo:

    docker tag karthequian/helloworld:latest
    <region-key>.ocir.io/<tenancy-namespace>/<repo-name>/<image-name>:<tag>
    
Onde:

    <region-key> é a chave para a região do Oracle Cloud Infrastructure Registry que você está usando. Por exemplo phx,. Consulte o tópico Disponibilidade por região na documentação do Oracle Cloud Infrastructure Registry.
        
    ocir.io é o nome do Oracle Cloud Infrastructure Registry.

    <tenancy-namespace>é a string de namespace de armazenamento de objeto gerada automaticamente da tenant (conforme mostrado na página Informações de tenant ) para a qual você deseja enviar a imagem. Por exemplo, o namespace da acme-devtenant pode ser ansh81vru1zp. Observe que seu usuário deve ter acesso ao arrendamento.

    <repo-name>(se especificado) é o nome de um repositório para o qual você deseja enviar a imagem (por exemplo, project01). Observe que especificar um repositório é opcional. Se você não especificar um nome de repositório, o nome da imagem será usado como o nome do repositório no Oracle Cloud Infrastructure Registry.

    <image-name>é o nome que você deseja dar à imagem no Oracle Cloud Infrastructure Registry (por exemplo, helloworld).

    <tag>é uma marca de imagem que você deseja fornecer à imagem no Oracle Cloud Infrastructure Registry (por exemplo, latest).
    
Por exemplo:

    docker tag karthequian/helloworld:latest phx.ocir.io/ansh81vru1zp/helloworld:latest
    
![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-tag.png)

**T4.7.1** Revise a lista de imagens disponíveis digitando:

    docker images
    
![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-images.png)

Observe que, embora duas imagens marcadas sejam mostradas, ambas são baseadas na mesma imagem (com o mesmo id de imagem).

### T4.8 - Envie a imagem hello-world para o Oracle Cloud Infrastructure Registry

Em uma janela de terminal na máquina cliente que executa o Docker, envie a imagem Docker da máquina cliente para o Oracle Cloud Infrastructure Registry inserindo:

    docker push <region-key>.ocir.io/<tenancy-namespace>/<repo-name>/<image-name>:<tag>
    
Onde:

    <region-key>é a chave para a região do Oracle Cloud Infrastructure Registry que você está usando. Por exemplo phx,. Consulte o tópico Disponibilidade por região na documentação do Oracle Cloud Infrastructure Registry.
ocir.io é o nome do Oracle Cloud Infrastructure Registry.

    <tenancy-namespace>é a string de namespace de armazenamento de objeto gerada automaticamente da tenant (conforme mostrado na página Informações de tenant ) que possui o repositório para o qual você deseja enviar a imagem. Por exemplo, o namespace da acme-devtenant pode ser ansh81vru1zp. Observe que seu usuário deve ter acesso ao arrendamento.

    <repo-name>(se especificado) é o nome de um repositório para o qual você deseja enviar a imagem (por exemplo, project01). Observe que especificar um repositório é opcional. Se você não especificar um nome de repositório, o nome da imagem será usado como o nome do repositório no Oracle Cloud Infrastructure Registry.

    <image-name>é o nome que você deseja dar à imagem no Oracle Cloud Infrastructure Registry (por exemplo, helloworld).

    <tag>é uma marca de imagem que você deseja fornecer à imagem no Oracle Cloud Infrastructure Registry (por exemplo, latest).

Por exemplo:

    docker push phx.ocir.io/ansh81vru1zp/helloworld:latest

![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-push.png)

As diferentes camadas da imagem do helloworld são empurradas uma por uma.

### T4.9 - Verifique se a imagem foi enviada ao Oracle Cloud Infrastructure Registry

**T4.9.1** Na janela do navegador que mostra o Console com a página Registro exibida, clique em Recarregar . Você vê todos os repositórios no registro aos quais tem acesso, incluindo o repositório privado helloworld que foi criado quando você enviou a imagem helloworld.

![Página de registro](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-registry-repositories.png)

**T4.9.2** Clique no nome do repositório helloworld que contém a imagem que você acabou de enviar. 

Veja que:

- As diferentes imagens no repositório. Nesse caso, há apenas uma imagem, com a tag mais recente.
- Detalhes sobre o repositório, incluindo quem o criou e quando, seu tamanho e se é um repositório público ou privado
- O Readme associado ao repositório. Nesse caso, ainda não há Readme.

![Página de registro](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-repository-images.png)

**T4.9.3** Forneça um leia-me para o repositório helloworld da seguinte maneira:

**T4.9.3.1** Clique no botão Editar na seção Readme .

**T4.9.3.2** Em Edit Readme, selecione a opção Markdown e copie e cole a seguinte descrição da imagem helloworld no conteúdo de campo:

	## Exemplo Hello World
	por Karthequian [retirado do Dockerhub] (https://hub.docker.com/r/karthequian/helloworld/)
	! [Helloworld por Karthquian] (https://raw.githubusercontent.com/oracle/cloud-native-devops-workshop/master/containers/docker001/images/004-hello-world.png)

![Janela Editar Leiame, guia Editar](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-image-readme-complete.png)

**T4.9.3.3** Clique na guia Visualizar para ver como o leiame aparecerá.

![Janela Editar Leiame, guia Visualizar](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-image-readme-preview.png)

**T4.9.3.4** Clique em Salvar para fechar a caixa de diálogo Editar Leiame.

**T4.9.4** Clique na última marca de imagem. A seção Detalhes mostra o tamanho da imagem, quando ela foi empurrada e por qual usuário, e o número de vezes que a imagem foi puxada.

![Página de registro](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-image-summary.png)

**(Opcional)** Mais tarde, se você deseja puxar a imagem, clique no botão Ações ao lado do nome da imagem e selecione Copiar comando de puxar . Por exemplo, o comando pode ser:
**docker pull phx.ocir.io/ansh81vru1zp/helloworld:latest**

Parabéns! Você puxou com sucesso a imagem helloworld do DockerHub, marcou-a e enviou-a para o Oracle Cloud Infrastructure Registry usando o Docker CLI. Você verificou que a imagem foi enviada com sucesso e adicionou uma descrição no readme.


----


### T3.E. Limpeza (opcional)

Depois de concluir o tutorial, se você deseja liberar recursos do Oracle Cloud Infrastructure, agora pode excluir o VCN e o cluster que criou. Por outro lado, é uma boa ideia mantê-los (especialmente o VCN) para seus próprios fins de teste. Se você pretende seguir o tutorial Extração de uma imagem do Oracle Cloud Infrastructure Registry ao implantar um aplicativo com carga balanceada em um cluster , definitivamente não exclua o VCN e o cluster, porque você os usará nesse tutorial.

**T3.E1.** (opcional) Volte para a janela do navegador com a página Detalhes do Cluster mostrando o Cluster do Tutorial, clique em Excluir Cluster e confirme se deseja excluir o Cluster do Tutorial criado.

![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-delete-cluster.png)
Descrição da ilustração

**T3.E2.** (opcional) Exclua o VCN criado durante o tutorial:

**T3.E2.1** No console, abra o menu de navegação. Em Infraestrutura central , vá para Rede e clique em Redes em nuvem virtual .

**T3.E2.2** Localize o VCN que você criou durante este tutorial.

**T3.E2.3** Clique no ícone Ações ao lado do VCN e clique em Terminar .

![Página VCN](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-vcn-terminate.png)

**T3.E2.4** Confirme que deseja encerrar o VCN.

----

Referências:

- https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/index.html
- https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/index.html

