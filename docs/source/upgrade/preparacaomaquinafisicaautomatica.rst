Preparando Máquinas Físicas - Automatizado
================================================

Criar, configurar e provisionar o SO nas máquinas físicas de forma automatizada utilizando o **Script** que está no projeto **Image-builder**:
https://git.camara.gov.br/sevir/image-builder.git

**Image-Builder** é um projeto que tem as ferramentas para customizar uma **ISO** de instalação e usá-la para provisionar uma máquina física.

Pré requisitos
------------
    Ter o **Docker** instalado

    Pacotes a serem instalados:
        **sudo apt-get install -y p7zip-full mtools isolinux xorriso**


Máquina com as ferramentas necessárias para o provisionamento:
---------
Caso precisem esta maquina esta com todas as configuracoes.
 - hostname: vkvupses02
 - ip: 10.2.33.23

 **O acesso nesse servidor vkvupses02 será sempre utilizando o ponto:**
 **p_ponto@vkvupses02**

 Exemplo do preenchimento da image utilizada no **Provision.sh**
 ------------

 As imagens utilizadas ficam no diretorio **image-builder/images** com o nome-da-maquina.sh.

 As informacoes utilizadas sao passadas pela equipe **SEDAC** ou entao entrando na idrac pelo navegador.

 Segue exemplo:

     `!/bin/bash`

     `export CUSTOM_NET_IFACE=eno1`

     `export CUSTOM_NET_NAMESERVERS="10.1.3.6 10.1.3.25 10.1.3.49"`

     `export CUSTOM_NET_GATEWAY=10.2.32.1`

     `export CUSTOM_NET_IPADDRESS=10.2.32.232/21`

     `export CUSTOM_NET_HOSTNAME=fcnupkub10`

     `export CUSTOM_NET_DOMAIN=redecamara.camara.gov.br`

     `export CUSTOM_NET_DISABLE_DHCP=true`

     `export IMAGE_ID=H97D342`

     `export IDRAC_SVCTAG=${IMAGE_ID}`

     `export IDRAC_IP=10.2.1.129`

 **Os parâmetros utilizados são:**

     **CUSTOM_NET_NAMESERVERS**: Endereços IP dos sevidores DNS, separados por espaço, caso haja mais de um

     **CUSTOM_NET_GATEWAY**: Endereço IP do Default Gateway para a interface padrão

     **CUSTOM_NET_IPADDRESS**: Endereço IP CIDR para o host (xxx.xxx.xxx.xxx/xx)

     **CUSTOM_NET_HOSTNAME**: Nome do host

     **CUSTOM_NET_DOMAIN**: Domínio do host

     **CUSTOM_NET_DISABLE_DHCP**: true/false. Caso seja false, habilita a obtenção de IP via DHCP.

     **IMAGE_ID**: Identificação da imagem gerada. A extensão .iso será adicionada a este identificador para forma o nome do arquivo.

     **IDRAC_SVCTAG**: Service Tag da IDRAC do servidor que se deseja provisionar. Utilizado para verificar se o IP fornecido é o do servidor correto

     **IDRAC_IP**: Endereço IP da IDRAC que se deseja provisionar.

Provisionamento
--------

1. Baixar o projeto **image-builder** utilizando o git clone:

    `git clone https://git.camara.gov.br/sevir/image-builder.git`

2. Dentro do projeto adicionar as credenciais, para acesso as máquinas na IDRAC, no arquivo **racadm.conf**

    `vim image-builder/idrac/racadm.conf`

    **RACADM_USER=p_xxxxxx@redecamara.camara.gov.br**

    **RACADM_PASS=<senha>**

3. Neste projeto tem o script "**provision.sh**" que será utilizado para formatar e provisionar as máquinas. O uso desse **script** se dá com um parâmentro que será a imagem da máquina a ser formatada:

    **obs:**
    **Esse comando tem que ser rodado como root**

    **Ex:**
    `sudo ./provision.sh (imagem da máquina)`

    `sudo ./provision.sh fcsupkub07`

    As imagens para uso com esse script estão dentro do diretóio **"images"** neste mesmo projeto.

4. Após rodar o comando ele vai executar alguns procedimentos, tendo apenas que aguardar e acompanhar a instalação pelo virtual console da IDRAC, pra ver se a máquina está respondendo (reiniciando, instalando e etc) utilizando as informações das máquinas.

    .. image:: ../_static/IDRAC/evolução.jpg

5. Quando então aparecer a informação **RELEASED (liberada)** no terminal, pode pressionar **CTRL+C** e o processo será encerrado finalizando assim a formatação e provisionamento dessa máquina.

    .. image:: ../_static/IDRAC/finalizado.jpg



