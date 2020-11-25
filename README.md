Tutorial 1 - Criando uma conta na OCI



Tutorial 2 - Criando a Instância do OKE

Este tutorial de 10 minutos mostra como:

- Criar um novo cluster com configurações padrão e novos recursos de rede
- Criar um pool de nós
- Configurar o arquivo kubeconfig para o cluster
- Verificar se você pode acessar o cluster usando kubectl e o painel Kubernetes

O Oracle Cloud Infrastructure Container Engine para Kubernetes é um serviço totalmente gerenciado, escalonável e altamente disponível que você pode usar para implantar seus aplicativos em contêineres na nuvem. Use o Container Engine para Kubernetes quando sua equipe de desenvolvimento quiser criar, implantar e gerenciar de maneira confiável aplicativos nativos da nuvem. Você especifica os recursos de computação que seus aplicativos exigem, e o Container Engine for Kubernetes os provisiona no Oracle Cloud Infrastructure em uma locação OCI existente.

Neste tutorial, você usa as configurações padrão para definir um novo cluster. Quando você cria o novo cluster, novos recursos de rede para o cluster são criados automaticamente, junto com um pool de nós e três nós de trabalho privados. Em seguida, você configurará o arquivo de configuração do Kubernetes para o cluster (o arquivo 'kubeconfig' do cluster). O arquivo kubeconfig permite que você acesse o cluster usando kubectl e o painel Kubernetes.

Para isto, você necessitará:

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


A. Criando a Instância do OKE

A1. Em um navegador, acesse o url que você recebeu para fazer login no Oracle Cloud Infrastructure.

Página de login
![Descrição da ilustração](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-login-page.png)

A2. Especifique um username no qual você tenha as permissões apropriadas para criar clusters. Você herda essas permissões de uma das seguintes maneiras:
 - Por pertencer ao grupo Administradores da locação.
 - Por pertencer a outro grupo ao qual uma política concede as permissões apropriadas do Container Engine para Kubernetes. Como você criará e configurará um cluster e recursos de rede associados durante o tutorial, as políticas também devem conceder ao grupo as permissões listadas em O que você precisa? seção.

A3. Digite seu nome de usuário e senha.

---

B. Definir os detalhes do cluster

B1. No console, abra o menu de navegação. Em Soluções e plataforma , acesse Serviços para desenvolvedores e clique em Clusters Kubernetes.

B2. Escolha um compartimento no qual você tenha permissão para trabalhar e no qual deseja criar o novo cluster e os recursos de rede associados.

![Página de clusters](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-console-create-cluster.png)

B3. Na página Clusters , clique em Criar Cluster .

B4. Na caixa de diálogo Criar Cluster , clique em Criação Rápida e em Iniciar Fluxo de Trabalho.

![Caixa de diálogo Criar Solução de Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-solution-v2.png)

B5. Na página Criar Cluster , altere o valor do marcador no campo Nome e digite Tutorial Cluster.

![Criação de Cluster - página Criar Cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-complete-top-v2.png)

B6. Clique em Avançar para revisar os detalhes inseridos para o novo cluster.
![Criação de cluster - página de revisão](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-review-top-v2.png)

B7. Na página Revisar , clique em Criar Cluster para criar os novos recursos de rede e o novo cluster.
Você vê os diferentes recursos de rede sendo criados para você.

![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-top-v1.png)


B8. Clique em Fechar para retornar ao console.
![Diálogo de status de criação de cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-create-cluster-creation-status-bottom-v2.png)


B9. O novo cluster é mostrado na página Detalhes do Cluster . Depois de criado, o novo cluster tem o status Ativo.
![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-active.png)


B10. Em Recursos , selecione Pools de nós e clique no nome do pool de nós no cluster que você acabou de criar (pool1). Em Recursos , selecione Nós e role para baixo para ver os detalhes dos novos nós de trabalho (instâncias de computação) no pool de nós.

![Recursos](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-clusters-page-nodepool.png)

----

C. Configure o arquivo kubeconfig para o cluster

C1. Confirme que você já fez o seguinte:
 - Gerou um par de chaves de assinatura de API.
 - Adicionado o valor da chave pública do par de chaves de assinatura da API às Configurações do usuário para seu nome de usuário.
 - Instalado e configurado o Oracle Cloud Infrastructure CLI (versão 2.6.4 ou posterior).
**** REVISAR
Se você não fez uma ou mais das opções acima, ou não tem certeza, consulte o tópico Configurando o acesso ao cluster na documentação do Container Engine para Kubernetes.

C2. Com a página Node Pools mostrando detalhes de pool1, clique em Tutorial Cluster no caminho de navegação. Clique em Access Cluster para exibir a caixa de diálogo Access Your Cluster e, em seguida, clique em Local Access .
Como acessar a caixa de diálogo do Kubeconfig
Descrição da ilustração

C3. Em uma janela de terminal, crie um diretório para conter o arquivo kubeconfig, fornecendo ao diretório o nome e a localização padrão esperados $HOME/.kube. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 


       $ mkdir -p $ HOME / .kube

C4. Execute o comando CLI do Oracle Cloud Infrastructure para configurar o arquivo kubeconfig e salve-o com o nome e localização padrão esperados $HOME/.kube/config. Esse nome e local garantem que o arquivo kubeconfig esteja acessível para kubectl e o painel do Kubernetes sempre que você executá-los em uma janela de terminal. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ):

    $ oci ce cluster create-kubeconfig --cluster-id ocid1.cluster.oc1.phx.aaaaaaaaae ... --file $ HOME / .kube / config --region us-phoenix-1 --token-version 2.0.0

onde ocid1.cluster.oc1.phx.aaaaaaaaae ... é o OCID do cluster atual. Por conveniência, o comando na caixa de diálogo Acessar seu cluster já inclui o OCID do cluster.

C5. Configure o valor da variável de ambiente KUBECONFIG para o nome e localização do arquivo kubeconfig. Por exemplo, no Linux, digite o seguinte comando (ou copie e cole da caixa de diálogo Acessar seu cluster ): 

    $ export KUBECONFIG = $ HOME / .kube / config

C6. Clique em Fechar para fechar a caixa de diálogo Acessar seu cluster .

----

D. Verifique o acesso do painel de kubectl e Kubernetes ao cluster

D1. Confirme se você já instalou o kubectl. Se você ainda não fez isso, consulte a documentação do kubectl .

D2. Verifique se você pode usar kubectl para se conectar ao novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl get nodes

Você vê detalhes dos nós em execução no cluster. Por exemplo:

    NOME STATUS ROLES IDADE VERSÃO
    10.0.10.2 Nó pronto 1d v1.13.5
    10.0.11.2 Nó pronto 1d v1.13.5
    10.0.12.2 Nó pronto 1d v1.13.5

D3. Implante o painel do Kubernetes no novo cluster que você criou. Em uma janela de terminal, digite o seguinte comando:

    $ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

D4. Verifique se você pode usar o painel Kubernetes para se conectar ao cluster:

D4.1 Em um editor de texto, crie um arquivo chamado oke-admin-service-account.yaml com o seguinte conteúdo:

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

D4.2 Crie a conta de serviço e o clusterrolebinding no cluster inserindo:

    $ kubectl apply -f oke-admin-service-account.yaml

A saída do comando acima confirma a criação da conta de serviço e o clusterrolebinding:

    serviceaccount "oke-admin" created
    clusterrolebinding.rbac.authorization.k8s.io "oke-admin" created

Agora você pode usar a conta de serviço oke-admin para visualizar e controlar o cluster e se conectar ao painel do Kubernetes.

D4.3 Obtenha um token de autenticação para a conta de serviço oke-admin inserindo:

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

D4.4 Copie o valor do token:elemento da saída. Você usará esse token para se 
conectar ao painel.

D4.5 Em uma janela de terminal, digite o seguinte comando:

    $ kubectl proxy

D4.6 Abra uma nova janela do navegador e acesse 

    http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/login
    
    
 para exibir o painel do Kubernetes.
 

![Página de login do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-sign-in.png)

D4.7 Selecione a opção Token e cole o valor do token:elemento que você copiou anteriormente no campo Token .

D4.8 Clique em Entrar .

D4.9 Clique em Visão geral para ver se o Kubernetes é o único serviço em execução no cluster.

![Página de visão geral do painel do Kubernetes](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-k8s-dashboard-overview.png)

D4.10 Parabéns! Você criou com êxito um novo cluster e confirmou que o novo cluster está instalado e funcionando conforme o esperado.

----

E. Limpeza (opcional)

Depois de concluir o tutorial, se você deseja liberar recursos do Oracle Cloud Infrastructure, agora pode excluir o VCN e o cluster que criou. Por outro lado, é uma boa ideia mantê-los (especialmente o VCN) para seus próprios fins de teste. Se você pretende seguir o tutorial Extração de uma imagem do Oracle Cloud Infrastructure Registry ao implantar um aplicativo com carga balanceada em um cluster , definitivamente não exclua o VCN e o cluster, porque você os usará nesse tutorial.

E1. (opcional) Volte para a janela do navegador com a página Detalhes do Cluster mostrando o Cluster do Tutorial, clique em Excluir Cluster e confirme se deseja excluir o Cluster do Tutorial criado.

![Página de detalhes do cluster](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-delete-cluster.png)
Descrição da ilustração

E2. (opcional) Exclua o VCN criado durante o tutorial:

E2.1 No console, abra o menu de navegação. Em Infraestrutura central , vá para Rede e clique em Redes em nuvem virtual .

E2.2 Localize o VCN que você criou durante este tutorial.

E2.3 Clique no ícone Ações ao lado do VCN e clique em Terminar .

![Página VCN](https://www.oracle.com/webfolder/technetwork/tutorials/obe/oci/oke-full/img/oci-vcn-terminate.png)

E2.3 Confirme que deseja encerrar o VCN.
