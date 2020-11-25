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

**T2.6.1.2** Vá para Administração e clique em Detalhes de locação .

**T2.6.1.3** O OCID do tenant é mostrado em Informações de tenant . 

**T2.6.1.4** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.2 - OCI do Usuário**

**T2.6.2.1** Se você estiver conectado como o usuário:
**T2.6.2.1.1** Abra o menu Perfil (Menu Hambúrger) e clique em Configurações do usuário .

**T2.6.2.1.2** Se você for um administrador fazendo isso para outro usuário: Abra o menu de navegação . Em Governança e Administração , acesse Identidade e clique em Usuários . Selecione o usuário na lista.

**T2.6.2.1.3** O OCID do usuário é mostrado em Informações do usuário . 
**T2.6.2.1.4** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.3 - Como fazer upload da chave pública**

Você pode fazer upload da chave pública PEM no console , que pode ser acessado fazendo login aqui: https://cloud.oracle.com . Se você não tiver um login e senha para o console , entre em contato com um administrador.

Abra o Console , e login.
Visualize os detalhes do usuário que chamará a API com o par de chaves:
Se você estiver conectado como o usuário:
Abra o menu Perfil ( ) e clique em Configurações do usuário . Ícone do menu do usuário
Se você for um administrador fazendo isso para outro usuário: Abra o menu de navegação . Em Governança e Administração , acesse Identidade e clique em Usuários . Selecione o usuário na lista.
Clique em Adicionar chave pública .
Cole o conteúdo da chave pública PEM na caixa de diálogo e clique em Adicionar .
A impressão digital da chave é exibida (por exemplo, 12: 34: 56: 78: 90: ab: cd: ef: 12: 34: 56: 78: 90: ab: cd: ef).

**T2.6.4 Configurando OCI CLI**

Para que a CLI o guie pelo processo de configuração inicial, use o comando:

    oci setup config


**O comando solicita as informações necessárias para o arquivo de configuração e as chaves públicas / privadas da API.**

    A caixa de diálogo de configuração gera um par de chaves API e cria o arquivo de configuração.

Você vai precisar de algumas informações para preencher a configuração quando executar o comando **oci setup config**. Estas informações você anotou nos passos anteriores.

- OCID do Tenant
- OCID do seu Usuário na Cloud Oracle





----

# T4 - Tutorial 4 - Criando a Instância do OKE

**Este tutorial de 10 minutos mostra como:**

- Criar um novo cluster com configurações padrão e novos recursos de rede
- Criar um pool de nós
- Configurar o arquivo kubeconfig para o cluster
- Verificar se você pode acessar o cluster usando kubectl e o painel Kubernetes

O Oracle Cloud Infrastructure Container Engine para Kubernetes é um serviço totalmente gerenciado, escalonável e altamente disponível que você pode usar para implantar seus aplicativos em contêineres na nuvem. Use o Container Engine para Kubernetes quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de maneira confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o Container Engine for Kubernetes os provisiona no Oracle Cloud Infrastructure em uma locação OCI existente.

Neste tutorial, você usa as configurações padrão para definir um novo cluster. Quando você cria o novo cluster, novos recursos de rede para o cluster são criados automaticamente, junto com um pool de nós e três nós de trabalho privados. Em seguida, você configurará o arquivo de configuração do Kubernetes para o cluster (o arquivo 'kubeconfig' do cluster). O arquivo kubeconfig permite que você acesse o cluster usando kubectl e o painel Kubernetes.

**Para isto, você necessitará:**

- Uma máquina local (Windows, Linux ou Mac)
- Chaves de acesso (pública e privada)

- Um nome de usuário e senha do Oracle Cloud Infrastructure.
- Dentro da sua locação, já deve haver um compartimento para conter os recursos de rede necessários (VCN, sub-redes, gateway de internet, gateway de NAT, tabela de rotas, listas de segurança). Se tal compartimento ainda não existir, você terá que criá-lo antes de iniciar este tutorial.
- Pelo menos três instâncias de computação devem estar disponíveis na locação para concluir este tutorial conforme descrito. Observe que, se apenas uma instância de computação estiver disponível, é possível criar um cluster com um pool de nós que possui uma única sub-rede e um único nó no pool de nós. No entanto, esse cluster não estará altamente disponível.
- Para criar e / ou gerenciar clusters, você deve pertencer a um dos seguintes:
 - O grupo Administradores da locação.
 - Um grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões apropriadas sobre esses recursos. Para mais detalhes e exemplos, consulte o tópico Configuração de política para criação e implantação de cluster na documentação do Container Engine para Kubernetes.
- Antes de configurar o arquivo kubeconfig posteriormente no tutorial, você já deve ter feito o seguinte (se não tiver feito ou não tiver certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes):

****** REVISAR
 - gerou um par de chaves de assinatura de API
 - adicionou o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para o seu nome de usuário
 - instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior)
- Você deve ter instalado e configurado a ferramenta de linha de comando do Kubernetes kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .


### T4.A. Criando a Instância do OKE

**T4.A1.** Em um navegador, acesse o url que você recebeu para fazer login no Oracle Cloud Infrastructure.

Página de login
![Descrição da ilustração](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-login-page.png)

**T4.A2.** Especifique um username no qual você tenha as permissões apropriadas para criar clusters. Você herda essas permissões de uma das seguintes maneiras:
 - Por pertencer ao grupo Administradores da locação.
 - Por pertencer a outro grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões listadas em O que você precisa? seção.

**T4.A3.** Digite seu nome de usuário e senha.

---

### T4.B. Definir os detalhes do cluster

**T4.B1.** No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Clusters Kubernetes.

**T4.B2.** Escolha um compartimento no qual você tenha permissão para trabalhar e no qual deseja criar o novo cluster e os recursos de rede associados.

![Página de clusters](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-console-create-cluster.png)

**T4.B3.** Na página Clusters , clique em Criar Cluster .

**T4.B4.** Na caixa de diálogo Criar Cluster , clique em Criação Rápida e em Iniciar Fluxo de Trabalho.

![Caixa de diálogo Criar Solução de Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-solution-v2.png)

**T4.B5.** Na página Criar Cluster , altere o valor do marcador no campo Nome e digite Tutorial Cluster.

![Criação de Cluster - página Criar Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-complete-top-v2.png)

**T4.B6.** Clique em Avançar para revisar os detalhes inseridos para o novo cluster.
![Criação de cluster - página de revisão](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-review-top-v2.png)

**T4.B7.** Na página Revisar , clique em Criar Cluster para criar os novos recursos de rede e o novo cluster.
Você vê os diferentes recursos de rede sendo criados para você.

![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-top-v1.png)


**T4.B8.** Clique em Fechar para retornar ao console.
![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-bottom-v2.png)


**T4.B9.** O novo cluster é mostrado na página Detalhes do Cluster . Depois de criado, o novo cluster tem o status Ativo.
![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-active.png)


**T4.B10.** Em Recursos , selecione Pools de nós e clique no nome do pool de nós no cluster que você acabou de criar (pool1). Em Recursos , selecione Nós e role para baixo para ver os detalhes dos novos nós de trabalho (instâncias de computação) no pool de nós.

![Recursos](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-nodepool.png)

----

### T4.C. Configure o arquivo kubeconfig para o cluster

**T4.C1.** Confirme que você já fez o seguinte:
 - Gerou um par de chaves de assinatura de API.
 - Adicionado o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para seu nome de usuário.
 - Instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior).
**** REVISAR
Se você não fez uma ou mais das opções acima, ou não tem certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes.

**T4.C2.** Com a página Node Pools mostrando detalhes de pool1, clique em Tutorial Cluster no caminho de navegação. Clique em Access Cluster para exibir a caixa de diálogo Access Your Cluster e, em seguida, clique em Local Access .
Como acessar a caixa de diálogo do Kubeconfig
Descrição da ilustração

**T4.C3.** Em uma janela de terminal, crie um diretório para conter o arquivo kubeconfig, fornecendo ao diretório o nome e a localização padrão esperados $HOME/.kube. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 


       $ mkdir -p $ HOME / .kube

**T4.C4.** Execute o comando CLI do Oracle Cloud Infrastructure para configurar o arquivo kubeconfig e salve-o com o nome e localização padrão esperados $HOME/.kube/config. Esse nome e local garantem que o arquivo kubeconfig esteja acessível para kubectl e o painel do Kubernetes sempre que você executá-los em uma janela de terminal. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ):

    $ oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaae ... --file $ HOME / .kube / config --region us-phoenix-1 --token-version 2.0.0

onde ocid1.cluster.oc1.phx.aaaaaaaaae ... é o OCID do cluster atual. Por conveniência, o comando na caixa de diálogo Acessar seu cluster já inclui o OCID do cluster.

**T4.C5.** Configure o valor da variável de ambiente KUBECONFIG para o nome e localização do arquivo kubeconfig. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 

    $ export KUBECONFIG = $ HOME / .kube / config

**T4.C6.** Clique em Fechar para fechar a caixa de diálogo Acessar seu cluster .

----

### T4.D. Verifique o acesso do painel de kubectl e Kubernetes ao cluster

**T4.D1.** Confirme se você já instalou o kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .

**T4.D2.** Verifique se você pode usar kubectl para se conectar ao novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl get nodes

Você vê detalhes dos nós em execução no cluster. Por exemplo:

    NOME STATUS ROLES IDADE VERSÃO
    10.0.10.2 Nó pronto 1d v1.13.5
    10.0.11.2 Nó pronto 1d v1.13.5
    10.0.12.2 Nó pronto 1d v1.13.5

**T4.D3.** Implante o painel do Kubernetes no novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

**T4.D4.** Verifique se você pode usar o painel Kubernetes para se conectar ao cluster:

**T4.D4.1** Em um editor de texto, crie um arquivo chamado oke-admin-service-account.yaml com o seguinte conteúdo:

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

**T4.D4.2** Crie a conta de serviço e o clusterrolebinding no cluster inserindo:

    $ kubectl apply -f oke-admin-service-account.yaml

A saída do comando acima confirma a criação da conta de serviço e o clusterrolebinding:

    serviceaccount "oke-admin" created
    clusterrolebinding.rbac.authorization.k8s.io "oke-admin" created

Agora você pode usar a conta de serviço oke-admin para visualizar e controlar o cluster e se conectar ao painel do Kubernetes.

**T4.D4.3** Obtenha um token de autenticação para a conta de serviço oke-admin inserindo:

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

**T4.D4.4** Copie o valor do token:elemento da saída. Você usará esse token para se 
conectar ao painel.

**T4.D4.5** Em uma janela de terminal, digite o seguinte comando:

    $ kubectl proxy

**T4.D4.6** Abra uma nova janela do navegador e acesse 

    http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login
    
    
 para exibir o painel do Kubernetes.
 

![Página de login do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-sign-in.png)

**T4.D4.7** Selecione a opção Token e cole o valor do token:elemento que você copiou anteriormente no campo Token .

**T4.D4.8** Clique em Entrar .

**T4.D4.9** Clique em Visão geral para ver se o Kubernetes é o único serviço em execução no cluster.

![Página de visão geral do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-overview.png)

**T4.D4.10** Parabéns! Você criou com êxito um novo cluster e confirmou que o novo cluster está instalado e funcionando conforme o esperado.

----

### T4.E. Limpeza (opcional)

Depois de concluir o tutorial, se você deseja liberar recursos do Oracle Cloud Infrastructure, agora pode excluir o VCN e o cluster que criou. Por outro lado, é uma boa ideia mantê-los (especialmente o VCN) para seus próprios fins de teste. Se você pretende seguir o tutorial Extração de uma imagem do Oracle Cloud Infrastructure Registry ao implantar um aplicativo com carga balanceada em um cluster , definitivamente não exclua o VCN e o cluster, porque você os usará nesse tutorial.

**T4.E1.** (opcional) Volte para a janela do navegador com a página Detalhes do Cluster mostrando o Cluster do Tutorial, clique em Excluir Cluster e confirme se deseja excluir o Cluster do Tutorial criado.

![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-delete-cluster.png)
Descrição da ilustração

**T4.E2.** (opcional) Exclua o VCN criado durante o tutorial:

**T4.E2.1** No console, abra o menu de navegação. Em Infraestrutura central , vá para Rede e clique em Redes em nuvem virtual .

**T4.E2.2** Localize o VCN que você criou durante este tutorial.

**T4.E2.3** Clique no ícone Ações ao lado do VCN e clique em Terminar .

![Página VCN](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-vcn-terminate.png)

**T4.E2.4** Confirme que deseja encerrar o VCN.
