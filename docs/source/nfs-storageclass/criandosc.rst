Gerenciando Storage Classes
============================

Padrão de nomenclatura
::::::::::::::::::::::

A ideia é que para cada projeto no  `Rancher <https://rancher-kube.camara.gov.br/>`_ exista um storage-class associado,
com o nome nfs-nomedoprojetonorancher. Portanto, se o nome do projeto no rancher for **sesap**, o nome do storage-class
será **nfs-sesap**

Pré-requisito
:::::::::::::

Necessário instalar o chart do nfs-client-provisioner

.. code-block:: text

  helm repo add stable https://charts.helm.sh/stable
  helm repo update


Criando o storage-class
:::::::::::::::::::::::

Para criar um novo storage-class, verifique se o servidor NFS não está sendo utilizado por outro storage-class e se
o servidor NFS (pegar da lista de servidores disponível na documentação) pertence ao mesmo ambiente do cluster (teste ou produção).
Para a criação do storage-class, você precisará de acesso ao cluster e do utilitário Helm disponível.

Pegando como exemplo o projeto **sesap** no Rancher e que você queira criar um novo storage-class utilizando o servidor
NFS **srvnfs** no share **/mnt/scsesap** deste mesmo servidor, o comando para instalação ficaria:

.. code-block:: text

  helm -n nfs install nfs-sesap stable/nfs-client-provisioner --set nfs.server=srvnfs \
  --set nfs.path=/mnt/scsesap --set storageClass.name=nfs-sesap \
  --set storageClass.accessModes=ReadWriteMany --set nfs.mountOptions={nfsvers=4}

**Observação: o storage-class será instalado no cluster que estiver com o contexto ativado**

Verificação
:::::::::::

Para verificar se a criação do storage-class ocorreu com sucesso, na Interface do Rancher, após escoher 
o cluster verifique no Menu *Storage -> Storage Classes* se o storage-class que você acabou de criar está listado
e no estado **Active**