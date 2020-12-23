Provisionamento de VMs
-----------------------

1) Executa ssh para checar todas as VM



2) Colocar a chave nas novas VMs, foi utilizado o usuario sesap



   - ssh-keygen -t rsa

   - ssh-copy-id sesap@vvmupkum04

            

 3) Fazer o usuario utilizado (sesap)  ser "sudo sem senha" em cada uma das maquinas



   - echo "sesap ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/sesap

 
 4) Configurar as VMs para receber o cluster


    - Instalar para o seu usuario o modulo ntp do ansible

         "ansible-galaxy install geerlingguy.ntp"


    - Alterar o arquivo https://git.camara.gov.br/sevir/rancher-operator/src/master/codigo/ansible/hosts.yml
      ATENÇÃO: O exemplo a seguir inclui as máquinas físicas como WORKERS (worknodes) e deve ser executado após a preparação delas, os procedimentos de formatação e instalação do SO destas estão no artigo "Preparando Máquinas Físicas", os dados das mesmas estão no topo deste artigo.

        .. code-block :: text 

                master:
                  hosts:
                    10.2.32.237:
                    10.2.32.238:
                    10.2.32.239:
                etcd:
                  hosts:
                    10.2.32.240:
                    10.2.32.241:
                    10.2.32.242:
                worknodes:
                  hosts:
                    10.2.32.227:
                    10.2.32.232:
                    10.2.32.233:
                    10.2.32.234:
                    10.2.32.235:
                    10.2.32.236:




    - Executa o comando "ansible-playbook -u sesap -i hosts.yml site.yml"

5)Adicionar o novo cluster no rancher-kube (https://rancher-kube.camara.gov.br/)

    - Global>Add Cluster>From Existing Nodes(Custom) 
        preencher Cluster Name: cluster2

    - Selecionar o novo cluster (cluster2)
        Selecionar Edit Cluster

        No final da pagina em Customize Node Run Command selecionar o tipo de node (control plane, etcd e worker),
        
        Copiar o comando gerado
       
        Realizar ssh para maquina a ser inserida no cluster

        Executar comando e aguardar na interface do rancher.

        ATENÇÃO: Não esquecer de rodar o comando nas máquinas físicas, listadas no começo deste arquivo.


    -
