# OCI DEV

O objetivo deste workshop é permitir ao participante que deseja conhecer um pouco do mundo de containers docker, kubernetes e cloud.

Em nosso dia-a-dia, ouvimos muito sobre estes termos mas muitas vezes, entender cada conceito se torna algo bastante complexo. Certo?

O workshop irá compreender os seguintes conceitos:

- Cloud
- Containers Docker
- Kubernetes
- Repositório de Imagens Docker
- Deployments
- Load-Balancer

Esperamos que, ao final deste workshop, o participante possa entender esses conceitos e praticar cada vez mais neste mundo de aplicações distribuídas.


Este material foi baseado em materiais já existentes **Oracle** (vide seção **Referências** ao final) e foi organizado especialmente para tornar a experiência prática deste workshop mais agradável. O material original se encontra em inglês.

Observação: Este material faz referência às imagens ilustrativas originais, portanto, se a imagem não aparecer neste material, provavelmente é porque o material original foi reformulado por algum motivo. Pode ser que este motivo seja uma mudança de versão do tutorial ou mesmo do recurso cloud (OKE, OCIR, OCI CLI, etc). Agradeço imensamente se puder avisar, pois poderemos atualizar este material com mais velocidade.

-----
# T1 - Tutorial 1 - Pré Check-in


### T1.1 - Criando uma conta na OCI

Abra a sua conta trial na Oracle Cloud Infrastructure.

Siga este link:

    https://videohub.oracle.com/media/1_m4904ocr

### T1.2 - Instalando o kubectl 

Siga este link:

    https://kubernetes.io/docs/tasks/tools/install-kubectl/
    
    Obs: O kubectl é a linha de comandos para se trabalhar com kubernetes. 
    Neste tutorial da página kubernetes.io, apenas certifique-se de que o comando 
    seja instalado e que você consiga acessá-lo com
    
    kubectl version
    
    Mais adiante, integraremos o kubectl à Oracle Cloud Infrastructure (OCI).
    

### T1.3 - Assine sua conta no github

Se você ainda não possui uma conta no github.com, por favor, siga estes passos:


**T1.3.1** Siga este link:

    https://github.com
    
**T1.3.2** Preencha seus dados e click em **Signup for GitHub**

![figura](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/github.png?raw=true)


### T1.4 Instalando o Docker

O docker é uma alternativa à virtualização, onde o maior diferencial é o compartilhamento do Kernel da máquina hospedeira (Linux) com os containers que fazem o papel do ambiente virtualizado.
As aplicações assim podem se beneficiar da implantação nestes containers e contar com flexibilidade e portabilidade independente do ambiente em que sejam implatados (nuvem pública, nuvem privada, on-premisses, etc).
A instalação do docker é necessária para este workshop para execução de comandos de manipulação do repositório de imagens.

A instalação do docker segue o site oficial Docker, conforme abaixo:

    https://docs.docker.com/engine/install/centos/
    https://docs.docker.com/engine/install/debian/
    https://docs.docker.com/engine/install/ubuntu/
    https://docs.docker.com/docker-for-mac/install/
    https://docs.docker.com/docker-for-windows/install/
    

Se você pretende instalar o docker com WSL no Windows:

    WSL deve funcionar bem se você instalar o docker com WSL2 e Ubuntu
    Até o fechamento deste material, o WSL1 e Debian não funcionaram corretamente
    
    
**T1.4.1 Linux**

Para instalar o docker no Oracle Linux, CentOS, Redhat, primeiramente faça o setup do repositório:

    $ sudo yum install -y yum-utils

    $ sudo yum-config-manager \ 
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

Em seguida, instale a última versão do Docker:

    $ sudo yum install docker-ce docker-ce-cli containerd.io

Será apresentado o fingerprint conforme abaixo. Assim que for apresentado, clique em aceitar.

    060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35
    
Inicie o docker com:

    $ sudo systemctl start docker
    
Teste para saber se o docker está funcionando:

    $ sudo docker run hello-world
    

**T1.4.2 Ubuntu**

Para instalar o docker no Ubuntu, primeiramente faça o setup do repositório:

    $ sudo apt-get update

    $ sudo apt-get install \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg-agent \
      software-properties-common
    
Adicione o GPG key oficial do docker:

    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    $ sudo apt-key fingerprint 0EBFCD88

Atualize o repositório:

    $ sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \      
      $(lsb_release -cs) \
      stable"
    $ sudo apt-get update
          
Em seguida, instale a última versão do Docker:

    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
    $ sudo /etc/init.d/docker start
       
Teste para saber se o docker está funcionando:

    $ sudo docker run hello-world
    
**T1.4.3 Mac OS X**

Faça o download na página do docker para Mac OS X:

    https://hub.docker.com/editions/community/docker-ce-desktop-mac/
    
Dê duplo-clique com o mouse no arquivo Docker.dmg e siga os passos.

Na janela do terminal, teste o docker com o comando:

    $ sudo docker run hello-world

**T1.4.4 Windows**

Faça o download na página do docker para Mac OS X:

    https://hub.docker.com/editions/community/docker-ce-desktop-windows/
    
Dê duplo-clique com o mouse no arquivo "Docker for Windows Installer" e siga os passos.

Na janela do Powershell, teste o docker com o comando:

    $ docker run hello-world
    

### T1.5 Como gerar uma chave de assinatura de API

    Obs: A etapa T1.5 pode ser feita automaticamente na instalação do OCI CLI. Apenas para conhecimento, entenda que o processo manual de gerar as chaves pública e privada é feita da forma descrita aqui em T1.5. Pule para a etapa T2 para ganhar tempo e gere as chaves automaticamente.


**T1.5.1 - Gerando uma chave de assinatura de API (Linux e Mac OS X)**

Use os seguintes comandos OpenSSL para gerar o par de chaves no formato PEM necessário.

**T1.5.1.1** Crie um .ocidiretório para armazenar as credenciais, caso ainda não o tenha feito :


    mkdir ~/.oci                

**T1.5.1.2** Gere a chave privada com um dos seguintes comandos.

Para gerar a chave, criptografada com uma senha longa que você fornece quando solicitado:
 
    Observação

    Recomendamos que você use uma senha longa para sua chave.
    
    openssl genrsa -out ~/.oci/oci_api_key.pem -aes128 2048       
                 

Para gerar a chave sem senha:

    openssl genrsa -out ~/.oci/oci_api_key.pem 2048                        
    
**T1.5.1.3** Altere a permissão do arquivo para garantir que apenas você possa ler o arquivo da chave privada:

    chmod go-rwx ~/.oci/oci_api_key.pem               
    
**T1.5.1.4** Gere a chave pública a partir de sua nova chave privada:

    openssl rsa -pubout -in ~/.oci/oci_api_key.pem -out ~/.oci/oci_api_key_public.pem             
    
**T1.5.1.5** Copie o conteúdo da chave pública para a área de transferência usando pbcopy, xclip ou uma ferramenta semelhante (você precisará colar o valor no console posteriormente). Por exemplo:

    cat ~/.oci/oci_api_key_public.pem | pbcopy           
    
Suas solicitações de API serão assinadas com sua chave privada e a Oracle usará a chave pública para verificar a autenticidade da solicitação. Você deve carregar a chave pública para o IAM (instruções abaixo).

**T1.5.2 - Gerando uma chave de assinatura de API (Windows)**

Se estiver usando o Windows, você precisará instalar o Git Bash para Windows antes de executar os comandos a seguir.

    Observação

    Certifique-se de incluir o openssl binário no caminho do Windows. Na instalação
    padrão, o arquivo openssl.exe pode ser encontrado em 
    C:\Program Files\Git\mingw64\bin.
    
Use os seguintes comandos OpenSSL para gerar o par de chaves no formato PEM necessário.

**T1.5.2.1** Crie um .ocidiretório para armazenar as credenciais, caso ainda não o tenha feito . Por exemplo:

    mkdir %HOMEDRIVE%%HOMEPATH%\.oci                
    
**T1.5.2.2** Gere a chave privada com um dos seguintes comandos:

Para gerar a chave que é criptografada com uma senha longa que você fornece quando solicitado:
 
    Observação
    Recomendamos que você use uma senha longa para sua chave.

    openssl genrsa -out %HOMEDRIVE%%HOMEPATH%\.oci\oci_api_key.pem -aes128 -passout stdin 2048
                     
Para gerar a chave sem senha:

    openssl genrsa -out %HOMEDRIVE%%HOMEPATH%\.oci\oci_api_key.pem 2048                        

**T1.5.2.3** Gere a chave pública a partir de sua nova chave privada:

    openssl rsa -pubout -in %HOMEDRIVE%%HOMEPATH%\.oci\oci_api_key.pem -out %HOMEDRIVE%%HOMEPATH%\.oci\oci_api_key_public.pem             

**T1.5.2.4** Copie o conteúdo da chave pública para a área de transferência (você precisará colar o valor no console posteriormente). Por exemplo:

    type \.oci\oci_api_key_public.pem     
    
Suas solicitações de API serão assinadas com sua chave privada e a Oracle usará a chave pública para verificar a autenticidade da solicitação. Você deve carregar a chave pública para o IAM (instruções abaixo).

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

### T2.1 - Linux e Unix (incluindo Oracle Linux 8, Ubuntu)

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

**T2.4.1** Abra o console do PowerShell usando a opção Executar **como administrador** .

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

        Windows e Linux: Você é solicitado a fornecer um local para instalar os binários
        e executáveis. O script instalará o Python para você.

        MacOS: você é notificado de que sua versão do Python é incompatível. Você deve
        atualizar antes de prosseguir com a instalação. O script não instalará o 
        Python para você.

    Quando solicitado a atualizar o CLI para a versão mais recente, responda 
    com Y para sobrescrever uma instalação existente.
    Quando solicitado a atualizar seu PATH, responda com Y para poder invocar a 
    CLI sem fornecer o caminho completo para o executável. Isso adicionará oci.exe 
    ao seu PATH.

### T2.6 - Configurando o arquivo config

Antes de usar a CLI, você deve criar um arquivo de configuração que contém as credenciais necessárias para trabalhar com o **Oracle Cloud Infrastructure** .

Para isto, vamos seguir o roteiro:
- Buscar as informações de Tenant e Usuário (OCID)
- Subir a chave pública para o seu usuário
- Executar o comando para configurar o OCI CLI

Vamos lá

 
**T2.6.1 - OCI do Tenant**

**T2.6.1.1** Clique em Configurações do usuário conforme a imagem e em seguida clique no Tenant:

![Configuração do Tenant](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/Acessando%20Tenant%201.png?raw=true)

**T2.6.1.2** O OCID do tenant é mostrado em Informações de tenant . 

![OCID do Tenant](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/Acessando%20Tenant%202.png?raw=true)


**T2.6.1.3** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.2 - OCI do Usuário**

**T2.6.2.1** Se você estiver conectado como o usuário:

**T2.6.2.1.1** Clique em Configurações do usuário conforme a imagem e em seguida clique no seu usuário:

![Configuração do Usuário](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/Acessando%20Usuario%201.png?raw=true)

**T2.6.2.1.2** Se você for um administrador fazendo isso para outro usuário: Abra o menu de navegação . Em Governança e Administração , acesse Identidade e clique em Usuários . Selecione o usuário na lista.

**T2.6.2.1.3** O OCID do usuário é mostrado em Informações do usuário . 

![OCID do Usuário](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/Acessando%20Usuario%202.png?raw=true)

**T2.6.2.1.4** Clique em Copiar para copiá-lo para a área de transferência e guarde em suas anotações para uso posterior


**T2.6.3 Configurando OCI CLI**

Para que a CLI o guie pelo processo de configuração inicial, use o comando:

    oci setup config


**O comando solicita as informações necessárias para o arquivo de configuração e as chaves públicas / privadas da API.**

    Obs: Na etapa T1.4, mostramos como gerar as chaves pública e privada. Se você 
    optou por gerá-las automaticamente, é aqui que faremos isso. Ao executar o comando
    oci setup config, você será questionado se deseja carregar suas chaves previamente
    geradas ou se deseja gerar as chaves.
    Lembre que esta etapa pede para você determinar o diretório em que as chaves estão
    ou que você deseja ter como padrão. Normalmente .oci

Você vai precisar de algumas informações para preencher a configuração quando executar o comando **oci setup config**. Estas informações você anotou nos passos anteriores.

- OCID do Tenant
- OCID do seu Usuário na Cloud Oracle


**T2.6.4 - Como fazer upload da chave pública**

Você pode fazer upload da chave pública PEM no console , que pode ser acessado fazendo login aqui: https://cloud.oracle.com

**T2.6.4.1** Clique em Configurações do usuário conforme a imagem e em seguida clique no seu usuário:

![Configuração do Usuário](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/Acessando%20Usuario%201.png?raw=true)

**T2.6.4.2** Visualize os detalhes do usuário que chamará a API com o par de chaves:

**T2.6.4.3** Se você for um administrador fazendo isso para outro usuário: 
- Abra o menu de navegação . 
- Em Governança e Administração , acesse Identidade e clique em Usuários . 
- Selecione o usuário na lista.

**T2.6.4.4** Clique em Adicionar chave pública .

![Adicionando chave pública](https://github.com/hoshikawa2/OCI-DEV/blob/main/images/API%20Key.png?raw=true)

**T2.6.4.5** Cole o conteúdo da chave pública PEM na caixa de diálogo e clique em Adicionar .

    A impressão digital da chave é exibida (por exemplo, 12: 34: 56: 78: 90: ab: cd: ef: 12: 34: 56: 78: 90: ab: cd: ef).


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


- Um nome de usuário e senha do Oracle Cloud Infrastructure.
- Dentro da sua tenant, já deve haver um compartimento para conter os recursos de rede necessários (VCN, sub-redes, gateway de internet, gateway de NAT, tabela de rotas, listas de segurança). Se tal compartimento ainda não existir, você terá que criá-lo antes de iniciar este tutorial.
- Pelo menos três instâncias de computação devem estar disponíveis na tenant para concluir este tutorial conforme descrito. Observe que, se apenas uma instância de computação estiver disponível, é possível criar um cluster com um pool de nós que possui uma única sub-rede e um único nó no pool de nós. No entanto, esse cluster não estará altamente disponível.
- Para criar e / ou gerenciar clusters, você deve pertencer a um dos seguintes:
 - O grupo Administradores da tenant.
 - Um grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões apropriadas sobre esses recursos. Para mais detalhes e exemplos, consulte o tópico Configuração de política para criação e implantação de cluster na documentação do Container Engine para Kubernetes.
- Antes de configurar o arquivo kubeconfig posteriormente no tutorial, você já deve ter feito o seguinte (se não tiver feito ou não tiver certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes):

 - gerou um par de chaves de assinatura de API
 - adicionou o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para o seu nome de usuário
 - instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior)
- Você deve ter instalado e configurado a ferramenta de linha de comando do Kubernetes kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .


### T3.1. Criando a Instância do OKE

**T3.1.1** Em um navegador, acesse o url que você recebeu para fazer login no Oracle Cloud Infrastructure.

Página de login
![Descrição da ilustração](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-login-page.png)

**T3.1.2** Especifique um username no qual você tenha as permissões apropriadas para criar clusters. Você herda essas permissões de uma das seguintes maneiras:
 - Por pertencer ao grupo Administradores da tenant.
 - Por pertencer a outro grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões listadas em O que você precisa? seção.

**T3.1.3** Digite seu nome de usuário e senha.

---

### T3.2 - Definir os detalhes do cluster

**T3.2.1** No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Clusters Kubernetes.

**T3.2.2** Escolha um compartimento no qual você tenha permissão para trabalhar e no qual deseja criar o novo cluster e os recursos de rede associados.

![Página de clusters](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-console-create-cluster.png)

**T3.2.3** Na página Clusters , clique em Criar Cluster .

**T3.2.4** Na caixa de diálogo Criar Cluster , clique em Criação Rápida e em Iniciar Fluxo de Trabalho.

![Caixa de diálogo Criar Solução de Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-solution-v2.png)

**T3.2.5** Na página Criar Cluster , altere o valor do marcador no campo Nome e digite Tutorial Cluster.

![Criação de Cluster - página Criar Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-complete-top-v2.png)

**T3.2.6** Clique em Avançar para revisar os detalhes inseridos para o novo cluster.
![Criação de cluster - página de revisão](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-review-top-v2.png)

**T3.2.7** Na página Revisar , clique em Criar Cluster para criar os novos recursos de rede e o novo cluster.
Você vê os diferentes recursos de rede sendo criados para você.

![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-top-v1.png)


**T3.2.8** Clique em Fechar para retornar ao console.
![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-bottom-v2.png)


**T3.2.9** O novo cluster é mostrado na página Detalhes do Cluster . Depois de criado, o novo cluster tem o status Ativo.
![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-active.png)


**T3.2.10** Em Recursos , selecione Pools de nós e clique no nome do pool de nós no cluster que você acabou de criar (pool1). Em Recursos , selecione Nós e role para baixo para ver os detalhes dos novos nós de trabalho (instâncias de computação) no pool de nós.

![Recursos](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-nodepool.png)

----

### T3.3 - Configure o arquivo kubeconfig para o cluster

**T3.3.1** Confirme que você já fez o seguinte:
 - Gerou um par de chaves de assinatura de API.
 - Adicionado o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para seu nome de usuário.
 - Instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior).


**T3.3.2** Com a página Node Pools mostrando detalhes de pool1, clique em Tutorial Cluster no caminho de navegação. Clique em Access Cluster para exibir a caixa de diálogo Access Your Cluster e, em seguida, clique em Local Access.

![Como acessar a caixa de diálogo do Kubeconfig](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-how-to-access-kubeconfig.png)


**T3.3.3** Em uma janela de terminal, crie um diretório para conter o arquivo kubeconfig, fornecendo ao diretório o nome e a localização padrão esperados $HOME/.kube. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 


       $ mkdir -p $HOME/.kube

**T3.3.4** Execute o comando CLI do Oracle Cloud Infrastructure para configurar o arquivo kubeconfig e salve-o com o nome e localização padrão esperados $HOME/.kube/config. Esse nome e local garantem que o arquivo kubeconfig esteja acessível para kubectl e o painel do Kubernetes sempre que você executá-los em uma janela de terminal. Por exemplo, no Linux, digite o seguinte comando ( **ou copie e cole da caixa de diálogo Acessar seu cluster** ):

    $ oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaae ... --file $HOME/.kube/config --region us-phoenix-1 --token-version 2.0.0

onde ocid1.cluster.oc1.phx.aaaaaaaaae ... é o OCID do cluster atual. Por conveniência, o comando na caixa de diálogo Acessar seu cluster já inclui o OCID do cluster.

**T3.3.5** Configure o valor da variável de ambiente KUBECONFIG para o nome e localização do arquivo kubeconfig. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 

    $ export KUBECONFIG=$HOME/.kube/config

**T3.3.6** Clique em Fechar para fechar a caixa de diálogo Acessar seu cluster .

----

### T3.4 - Verifique o acesso do painel de kubectl e Kubernetes ao cluster

**T3.4.1** Confirme se você já instalou o kubectl. Se você ainda não fez isso, consulte a documentação do kubectl ( **Passo T1.2** ).

**T3.4.2** Verifique se você pode usar kubectl para se conectar ao novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl get nodes

Você vê detalhes dos nós em execução no cluster. Por exemplo:

    NOME STATUS ROLES IDADE VERSÃO
    10.0.10.2 Nó pronto 1d v1.13.5
    10.0.11.2 Nó pronto 1d v1.13.5
    10.0.12.2 Nó pronto 1d v1.13.5

**T3.4.3** Implante o painel do Kubernetes no novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

**T3.4.4** Verifique se você pode usar o painel Kubernetes para se conectar ao cluster:

**T3.4.4.1** Em um editor de texto, crie um arquivo chamado oke-admin-service-account.yaml com o seguinte conteúdo:

    apiVersion: v1
    kind: ServiceAccount
    metadata:
      name: oke-admin
      namespace: kube-system
    ---
    apiVersion: rbac.authorization.k8s.io/v1
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

Obtenha um token de autenticação para a conta de serviço oke-admin da seguinte forma
Em um editor de texto, crie um arquivo chamado oke-admin-sa-token.yaml com o seguinte conteúdo:
	
    apiVersion: v1
    kind: Secret
    metadata:
      name: oke-admin-sa-token
      namespace: kube-system
      annotations:
       kubernetes.io/service-account.name: oke-admin
    type: kubernetes.io/service-account-token


	
**T3.4.4.2** Crie a conta de serviço, o clusterrolebinding no cluster e obtenha o token executando os seguintes comandos:

    kubectl apply -f oke-admin-service-account.yaml
    kubectl apply -f oke-admin-sa-token.yaml

A saída do comando acima confirma a criação da conta de serviço e o clusterrolebinding:

    serviceaccount "oke-admin" created
    clusterrolebinding.rbac.authorization.k8s.io "oke-admin" created

Agora você pode usar a conta de serviço oke-admin para visualizar e controlar o cluster e se conectar ao painel do Kubernetes.

**T3.4.4.3** Obtenha um token de autenticação para a conta de serviço oke-admin inserindo:

    kubectl describe secrets oke-admin-sa-token -n kube-system

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

    http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
    
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

**T4.5.2** Quando solicitado, digite seu nome de usuário no formato < tenancy-namespace >/< username >. 

Por exemplo ansh81vru1zp/jdoe@acme.com. Se a sua tenant for federada com o Oracle Identity Cloud Service, use o formato < tenancy-namespace >/oracleidentitycloudservice/< username >.

**T4.5.3** Quando solicitado, digite o token de autenticação que você copiou anteriormente como a senha.

![Janela do terminal](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/img/oci-docker-login.png)

### T4.6 - Extraia a imagem hello-world do DockerHub

Em uma janela de terminal na máquina cliente que executa o Docker, digite docker pull karthequian/helloworld:latest para recuperar a versão mais recente da imagem hello-world do DockerHub.

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

# T5 - Implante uma imagem do OCIR para o cluster de OKE 

Este tutorial de 15 minutos mostra como:

- criar um segredo nomeado contendo credenciais do Oracle Cloud Infrastructure
- adicione o segredo nomeado a um arquivo .yml de manifesto, junto com o nome e a localização de uma imagem para extrair do Oracle Cloud Infrastructure Registry
- use o arquivo manifest .yml para implantar o aplicativo helloworld em um cluster Kubernetes e criar um balanceador de carga do Oracle Cloud Infrastructure
- verifique se o aplicativo helloworld está funcionando conforme o esperado e se o balanceador de carga está distribuindo solicitações entre os nós em um cluster
fundo

### T5.1 - Introdução


O Oracle Cloud Infrastructure Container Engine para Kubernetes é um serviço totalmente gerenciado, escalonável e altamente disponível que você pode usar para implantar seus aplicativos em contêineres na nuvem. Use o Container Engine para Kubernetes quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de maneira confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o Container Engine for Kubernetes os provisiona no Oracle Cloud Infrastructure em uma locação OCI existente.

Este tutorial pressupõe que você já tenha concluído:

- o tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry
- o tutorial Como criar um cluster com o Oracle Cloud Infrastructure Container Engine para Kubernetes

### T5.2 - O que você precisa?

- Você deve ter atendido os pré-requisitos para o tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry e concluído com êxito o tutorial. Refaça o tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry se você não o fez:
 - Crie um token de autenticação. Você precisará do valor desse token de autenticação para concluir este tutorial. Se você não tiver o valor do token de autenticação, volte para o tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry e siga as instruções para criar um novo token de autenticação.
 - Envie a imagem helloworld para um repositório privado no Oracle Cloud Infrastructure Registry. Por padrão, a imagem que você enviou deve ter sido enviada para um repositório privado, que só pode ser acessado fornecendo um token de autenticação.
- Você deve ter atendido aos pré-requisitos para o tutorial Criação de um cluster com a infraestrutura do Oracle Cloud Container Engine para Kubernetes e concluído com êxito o tutorial. Refaça o tutorial Criação de um cluster com a infraestrutura do Oracle Cloud Container Engine para Kubernetes se você não o fez:
 - Crie um VCN configurado adequadamente e recursos relacionados (se ainda não existissem).
 - Crie um novo cluster do Kubernetes e adicione um pool de nós ao novo cluster.
 - Configure o arquivo de configuração do Kubernetes para o cluster (o arquivo 'kubeconfig' do cluster) como um arquivo nomeado configlocalizado no  $HOME/.kube/configdiretório. O arquivo kubeconfig permite que você acesse o cluster usando kubectl e o painel Kubernetes. Observe que, se você não armazenou o arquivo kubeconfig como $HOME/.kube/config, terá que definir explicitamente a variável de ambiente KUBECONFIG para apontar para o arquivo kubeconfig sempre que usar kubectl em uma nova janela de terminal.
- Você deve ter cota de serviço de balanceamento de carga de infraestrutura em nuvem da Oracle suficiente disponível em sua região para criar um balanceador de carga de 100 Mbps.

### T5.3 - Preparando-se para o tutorial

**T5.3.1** Confirme se você tem o valor para o Tutorial **auth token** que você criou no tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry . 
Se você não tiver o valor do **token** de autenticação, volte para esse tutorial e siga as instruções para criar um novo token de autenticação.

**T5.3.2** Verifique se você pode usar **kubectl** para se conectar ao cluster criado no tutorial Criando um cluster com o Oracle Cloud Infrastructure Container Engine para Kubernetes inserindo o seguinte comando em uma janela de terminal:

    $ kubectl get nodes
    
Você vê detalhes dos nós em execução no cluster. Por exemplo:

    NAME               STATUS   ROLES   AGE   VERSION
    10.0.10.2          Ready    node    1d    v1.13.5
    10.0.11.2          Ready    node    1d    v1.13.5
    10.0.12.2          Ready    node    1d    v1.13.5

Você confirmou que o cluster está instalado e funcionando conforme o esperado. Agora você pode implantar um aplicativo no cluster.

### T5.4 - Crie um secret para o tutorial


Para permitir que o Kubernetes extraia uma imagem do Oracle Cloud Infrastructure Registry ao implantar um aplicativo, você precisa criar um segredo do Kubernetes. O segredo inclui todos os detalhes de login que você forneceria se estivesse efetuando login manualmente no Oracle Cloud Infrastructure Registry usando o docker logincomando, incluindo seu token de autenticação.

**T5.4.1** Em uma janela de terminal, digite o seguinte comando:


    $ kubectl create secret docker-registry ocirsecret --docker-server=<region-key>.ocir.io --docker-username='<tenancy-namespace>/<oci-username>' --docker-password='<oci-auth-token>' --docker-email='<email-address>'

Onde:

    ocirsecret é o nome do segredo que você está criando e que usará no arquivo de manifesto para se referir ao segredo. Para os fins deste tutorial, você deve nomear o segredo  ocirsecret. Quando tiver concluído o tutorial e estiver criando seus próprios segredos para seu próprio uso, você pode escolher como chamar seus segredos. 

    <region-key>é a chave para a região do Oracle Cloud Infrastructure Registry que você está usando. Por exemplo phx,. Consulte o tópico Disponibilidade por região na documentação do Oracle Cloud Infrastructure Registry.
    
    ocir.io é o nome do Oracle Cloud Infrastructure Registry.

    <tenancy-namespace>é a string de namespace de armazenamento de objeto gerado automaticamente da locação (conforme mostrado na página Informações de locação ) contendo o repositório do qual o aplicativo deve obter a imagem. Por exemplo, o namespace da acme-devlocação pode ser ansh81vru1zp.

    <oci-username>é o nome de usuário a ser usado ao puxar a imagem. O nome de usuário deve ter acesso à locação especificada por tenancy-namespace. Por exemplo jdoe@acme.com,. Se a sua locação for federada com o Oracle Identity Cloud Service, use o formato . /oracleidentitycloudservice/<oci-username>

    <oci-auth-token>é o token de autenticação do usuário especificado por oci-username. Por exemplo, k]j64r{1sJSSF-;)K8
    
    <email-address>é um endereço de e-mail. É necessário um endereço de e-mail, mas não importa o que você especificar. Por exemplo,jdoe@acme.com

    Observe o uso de aspas simples em torno de strings contendo caracteres especiais.


Por exemplo, combinando os exemplos anteriores, você pode inserir:

    $ kubectl create secret docker-registry ocirsecret --docker-server=phx.ocir.io --docker-username='ansh81vru1zp/jdoe@acme.com' --docker-password='​​k]j64r{1sJSSF-;)K8' --docker-email='jdoe@acme.com'

**T5.4.2** Verifique se o segredo foi criado inserindo:

    $ kubectl get secrets
    
Detalhes sobre o segredo ocirsecret que você acabou de criar são exibidos.

Depois de criar o segredo, agora você pode consultá-lo no arquivo de manifesto do aplicativo.

### T5.5 - Adicione o segredo e o caminho da imagem ao arquivo de manifesto

Depois de criar o segredo, agora você inclui o nome do segredo no arquivo de manifesto que o Kubernetes usa ao implantar o aplicativo helloworld em um cluster. Você também inclui no arquivo de manifesto o caminho para a imagem helloworld no Oracle Cloud Infrastructure Registry.

**T5.5.1** Crie um novo arquivo de texto com o nome helloworld-lb.yml em um diretório local acessível para kubectl.

**T5.5.2** Abra o novo arquivo helloworld-lb.yml em um editor de texto.

**T5.5.3** Copie e cole o seguinte texto no arquivo helloworld-lb.yml:

	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: helloworld-deployment
	spec:
	  selector:
	    matchLabels:
	      app: helloworld
	  replicas: 1
	  template:
	    metadata:
	      labels:
	        app: helloworld
	    spec:
	      containers:
	      - name: helloworld
	    # enter the path to your image, be sure to include the correct region prefix    
	        image: <region-key>.ocir.io/<tenancy-namespace>/<repo-name>/<image-name>:<tag>
	        ports:
	        - containerPort: 80
	      imagePullSecrets:
	    # enter the name of the secret you created  
	      - name: <secret-name>
	---
	apiVersion: v1
	kind: Service
	metadata:
	  name: helloworld-service
	spec:
	  type: LoadBalancer
	  ports:
	  - port: 80
	    protocol: TCP
	    targetPort: 80
	  selector:
	    app: helloworld

**T5.5.4** Altere a seguinte linha no arquivo helloworld-lb.yml para incluir o caminho que você especificou ao enviar a imagem helloworld para o Oracle Cloud Infrastructure Registry no tutorial Enviar uma imagem para o Oracle Cloud Infrastructure Registry :

    image: <region-key>.ocir.io/<tenancy-namespace>/<repo-name>/<image-name>:<tag>
    
Por exemplo, se você deu a tag à imagem phx.ocir.io/ansh81vru1zp/helloworld:latest, altere a linha para ler:

    image: phx.ocir.io/ansh81vru1zp/helloworld:latest

**T5.5.5** Altere a seguinte linha no arquivo helloworld-lb.yml para incluir o nome do segredo que você criou anteriormente:

    name: <secret-name>

**T5.5.6** Como você deu ao segredo o nome ocirsecret, altere a linha para ler:

    name: ocirsecret

**T5.5.7** Salve o arquivo helloworld-lb.yml em um diretório local acessível para kubectl e feche o arquivo.

### T5.6 - Implante o aplicativo helloworld

Tendo atualizado o arquivo de manifesto do aplicativo helloworld, agora você pode implantar o aplicativo.

**T5.6.1** Em uma janela de terminal, implante o aplicativo helloworld de amostra no cluster inserindo:

    $ kubectl create -f <local-path>/helloworld-lb.yml
    
onde 


    <local-path> é a localização do arquivo helloworld-lb.yml.

As mensagens confirmam que a implantação helloworld-deployment e o balanceador de carga de serviço helloworld-service foram criados.

O balanceador de carga helloworld-service é implementado como um balanceador de carga Oracle Cloud Infrastructure com um back-end configurado para rotear o tráfego de entrada para nós no cluster. Você pode ver o novo balanceador de carga na página Balanceadores de carga no Oracle Cloud Infrastructure Console.

### T5.6 - Verifique se o aplicativo helloworld com carga balanceada está funcionando corretamente

**T5.6.1** Em uma janela de terminal, digite o seguinte comando:

    $ kubectl get services
    
Você vê detalhes dos serviços em execução nos nós do cluster. Para o balanceador de carga helloworld-service que você acabou de implantar, você verá:

- o endereço IP externo do balanceador de carga (por exemplo, 129.146.147.91)
- o número da porta

**T5.6.2** Abra uma nova janela do navegador e digite o url para acessar o aplicativo helloworld no campo URL do navegador . Por exemplo, http://129.146.147.91

Quando o balanceador de carga recebe a solicitação para acessar o aplicativo helloworld, o balanceador de carga encaminha a solicitação para um dos nós disponíveis no cluster. Os resultados da solicitação são retornados ao navegador, que exibe uma página com uma mensagem como:

    Hello

    Is it me you're looking for?

![Janela do navegador](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-and-registry/img/oci-hello-world-visit1.png)

Na parte inferior da página, um contador de visualizações de página mostra o número de vezes que a página foi visitada e inicialmente exibe '1'.

**T5.6.3** Recarregue a página na janela do navegador (por exemplo, clicando em Atualizar ou Recarregar ).

![Janela do navegador](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-and-registry/img/oci-hello-world-visit2.png)

O contador na parte inferior da página agora exibe '2'.

**T5.6.4** Parabéns! Você implantou com sucesso o aplicativo helloworld. O Kubernetes usou o segredo que você criou para extrair a imagem helloworld do Oracle Cloud Infrastructure Registry. Em seguida, ele implantou a imagem e criou um balanceador de carga do Oracle Cloud Infrastructure para distribuir solicitações entre os nós no cluster. Por fim, você verificou se o aplicativo está funcionando conforme o esperado.


### T5.7 - Serviço de limpeza

Tendo concluído o tutorial, agora você pode excluir o aplicativo que implementou no cluster. Se quiser liberar recursos do Oracle Cloud Infrastructure, você também pode excluir o VCN e o cluster que criou no tutorial Criando um cluster com o Oracle Cloud Infrastructure Container Engine para Kubernetes . Por outro lado, como demorou um pouco para configurar o VCN e o cluster, é uma boa ideia mantê-los (especialmente o VCN) para seus próprios fins de teste.

**T5.7.1** Em uma janela de terminal, digite o seguinte comando para excluir o aplicativo helloworld:

    $ kubectl delete deployment helloworld-deployment
    
Quando você exclui a implantação, todos os pods em execução também são excluídos automaticamente.

**T5.7.2** Digite o seguinte comando para excluir o serviço do balanceador de carga:

    $ kubectl delete service helloworld-service
    
Quando você exclui o serviço de balanceador de carga, o balanceador de carga do Oracle Cloud Infrastructure também é excluído automaticamente.

Opcionalmente, você pode liberar recursos do Oracle Cloud Infrastructure excluindo o cluster e o VCN que você criou no tutorial Criando um Cluster com o Oracle Cloud Infrastructure Container Engine para Kubernetes .

**T5.7.3** (opcional) Para excluir o cluster que você criou no tutorial Criar um cluster com a infraestrutura do Oracle Cloud Container Engine para Kubernetes (denominado Tutorial Cluster nesse tutorial):

**T5.7.3.1** No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Clusters Kubernetes .

**T5.7.3.2** Clique no nome do cluster que você criou para o tutorial.

**T5.7.3.3** Clique em Excluir cluster .

**T5.7.3.4** Página de detalhes do cluster

![Descrição da ilustração](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-and-registry/img/oci-delete-cluster.png)

**T5.7.3.5** Confirme que você deseja excluir o cluster.

**T5.7.4** (opcional) Para excluir o VCN que você criou no tutorial Criando um Cluster com o Oracle Cloud Infrastructure Container Engine para Kubernetes (denominado oke-vcn-quick-Tutorial Cluster- <creation_date> naqueletutorial):

**T5.7.4.1** No console, abra o menu de navegação. Em Infraestrutura central , vá para Rede e clique em Redes em nuvem virtual .

**T5.7.4.2** Localize o VCN que você criou para o tutorial.

**T5.7.4.3** Clique no ícone Actions ao lado do VCN que você criou e clique em Terminate .

![Página VCN](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-and-registry/img/oci-vcn-terminate.png)

**T5.7.4.4** Confirme que deseja encerrar o VCN.

----


### T6 - Limpeza do cluster OKE e VCN (opcional)

Depois de concluir o tutorial, se você deseja liberar recursos do Oracle Cloud Infrastructure, agora pode excluir o VCN e o cluster que criou. Por outro lado, é uma boa ideia mantê-los (especialmente o VCN) para seus próprios fins de teste. Se você pretende seguir o tutorial Extração de uma imagem do Oracle Cloud Infrastructure Registry ao implantar um aplicativo com carga balanceada em um cluster , definitivamente não exclua o VCN e o cluster, porque você os usará nesse tutorial.

**T6.1** (opcional) Volte para a janela do navegador com a página Detalhes do Cluster mostrando o Cluster do Tutorial, clique em Excluir Cluster e confirme se deseja excluir o Cluster do Tutorial criado.

![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-delete-cluster.png)
Descrição da ilustração

**T6.2** (opcional) Exclua o VCN criado durante o tutorial:

**T6.2.1** No console, abra o menu de navegação. Em Infraestrutura central , vá para Rede e clique em Redes em nuvem virtual .

**T6.2.2** Localize o VCN que você criou durante este tutorial.

**T6.2.3** Clique no ícone Ações ao lado do VCN e clique em Terminar .

![Página VCN](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-vcn-terminate.png)

**T6.2.4** Confirme que deseja encerrar o VCN.

----

Referências:

- https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/index.html
- https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/registry/index.html
- https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-and-registry/index.html
- https://docs.cloud.oracle.com/en-us/iaas/Content/API/Concepts/apisigningkey.htm#two

Projeto como Exemplo:
- Java: https://github.com/hoshikawa2/oraclejavasoapkubernetes
- .Net: https://github.com/hoshikawa2/callPDFReportJenkins.git
- Python: https://github.com/hoshikawa2/findCustomerPython.git
- NodeJS: https://github.com/hoshikawa2/consultaCEP.git
