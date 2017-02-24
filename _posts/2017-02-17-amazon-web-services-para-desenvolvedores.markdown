---
title: "Amazon AWS para desenvolvedores!"
layout: post
date: 2017-02-17 19:50
image: /assets/images/aws.png
headerImage: true
star: true
category: aws
author: michel
description: Trabalhando na Amazon AWS
---

## Objetivo

Mostrar como um desenvolvedor/programador pode trabalhar na Amazon AWS escalando
sua aplicação de forma fácil.
Será mostrado passo a passo do ínicio ao fim.

#### Índice
- [Introdução](#introduo)
- [VPC](#vpc)
- [Subnets](#subnets)
- [Internet Gateway](#internet-gateway)
- [Route Tables](#route-tables)
- [EC2](#ec2)
- [Configurando nossa aplicação](#configurando-nossa-aplicao)
- [Amazon AMI](#amazon-ami)
- [Load Balancing](#load-balancing)

## Introdução

Ao entra pela 1° vez no painel da AWS geralmente tomamos um susto
devido a quantidade enorme de produtos e serviços oferecidos pela Amazon.

<figcaption class="caption">Serviços AWS 20/02/2017</figcaption>
<img class="bigger-image" src="{{ site.url }}/assets/images/services_aws.png" alt="Serviços AWS">

Isso porque a Amazon é uma plataforma completa para se trabalhar com aplicações
na nuvem, ela cobre quase 100% da área. Eu não sou um especialista no assunto
mais acredito que é uma das mais completas se não for a mais completa do mercado
hoje.

E, exatamente por ela ser tão grande que pode acabar sendo tão dificil trabalhar
nela, é muito fácil se perder ou configurar um serviço de forma errada.
Nesse artigo vamos ver desde o começo ao fim
de toda configuração necessária para deixar sua aplicação totalmente escalavél
na AWS.

## VPC

VPC(Virtual Private Cloud) como nossa aplicação está hospedada da nuvem geralmente
não temos acesso de forma transparente acesso a rede de nossos servidores Cloud.
Isso porque a empresa de hospedagem faz isso automaticamente. Mais fazer Isso
automaticamente não é legal na minha opinião. Eu quero ter o controle da rede
interna de meus servidores, quero dizer qual máquina vai ter acesso a internet e
qual não vai ter por exemplo. É exatamente ai que a VPC entra na AWS ela permite
que você tenha controle da rede interna de sua aplicação.

Para criar uma VPC vamos até os serviços da AWS e clicamos na opção VPC como
mostrado nas imagens a seguir:

<img class="image" src="{{ site.url }}/assets/images/services_aws_clique.png" alt="Services AWS">

<img class="image" src="{{ site.url }}/assets/images/vpc_aws.png" alt="VPC AWS">

A seguir não tem segredo, clique em <span class="evidence">Your VPCs</span> no
menu lateral da esquerda e depois no botão
<span class="evidence">Create VPC</span>

<img class="image" src="{{ site.url }}/assets/images/vpc_create_aws.png" alt="Create VPC AWS">

<div class="side-by-side">
    <div class="toleft">
        <img class="image" src="{{ site.url }}/assets/images/create_vpn_aws.png" alt="Create VPC AWS">
    </div>

    <div class="toright">
      * <strong>Name tag:</strong> Nome da VPC <br>
      * <strong>IPv4 CIDR block:</strong> Faixa de IP para trabalhar com a VPC
    </div>
</div>

Feito isso você já criou sua VPC

<img class="bigger-image" src="{{ site.url }}/assets/images/vpc.png" alt="VPC AWS">

Vamos agora setar algumas configurações importantes em nossa VPC.

1° Clique em Actions -> Edit DNS Hostnames -> Escolha a opção "Yes" para
a VPC possa localizar maquinas por nomes.

2° Clique em Actions -> Edit DNS Resolution -> Escolha a opção "Yes" para
a VPC resolver DNS automaticamente.

<img class="image" src="{{ site.url }}/assets/images/action_vpc_aws.png" alt="VPC AWS">
<img class="image" src="{{ site.url }}/assets/images/hostnames_vpc.png" alt="VPC AWS">
<img class="image" src="{{ site.url }}/assets/images/resolution_vpc.png" alt="VPC AWS">

Pronto! Com isso termina a configuração da VPC.

## Subnets

Na rede que acabei de criar "EconomiaMaxima" vou criar 4 Subnets para trabalhar
dentro dela.

A 1° e 3° Subnet vai operar na primeira zona de disponibilidade da AWS e a
2° e 4° irá trabalhar na segunda zona de disponibilidade.

Porque?

Caso 1° zona de disponibilidade fique inacessível por qualquer que seja o motivo
a segunda zona irá garantir que minha aplicação não fiquei fora do ar.

Também vamos criar uma subnet com acesso a internet e outra não, ou seja, na 1°
zona de disponibilidade vamos ter 2 Subnets:
Uma com acesso a internet e outra sem acesso a internet.

A mesma coisa na 2° zona de disponibilidade.

Para criar subnet:

1° - Vá para "Subnets" no menu esquerdo da lateral.
<img class="image" src="{{ site.url }}/assets/images/subnets_aws.png" alt="Subnets AWS">

2̣° - Clique em "Create Subnet"
<img class="image" src="{{ site.url }}/assets/images/create_subnet_aws.png" alt="Subnets AWS">

3° - Crie às Subnets como na imagem a seguir
<img class="image" src="{{ site.url }}/assets/images/create_dados_subnet_aws.png" alt="Subnets AWS">

Veja, na imagem está sendo criada a 1° subnet. Também deve ser criada as outras 3.

4° - Selecione a Subnet "Public 2a" -> Clique no botão "Subnet Actions" ->
Escolha a opção "Modify auto-assign IP settings" -> Habilite o "Auto-assign IPs"
<img class="image" src="{{ site.url }}/assets/images/subnet_modify_auto_assign_IP_aws.png" alt="Subnets AWS">

5° - Realize o mesmo procedimento do passo anterior na Subnet "Public 2b"

No final a tela de Subnets ficará assim:
<img class="bigger-image" src="{{ site.url }}/assets/images/subnets_criadas_aws.png" alt="Subnets AWS">

Perceba que cada subnet tem uma faixa de IP diferente e todas estão associadas a
VPC "EconomiaMaxima".

## Internet Gateway

Em Internet Gateway será setado qual subnet tem acesso a internet e qual não tem.

O procedimento é padrão não tem erro.

1° - Vá para "Internet Gateway".
<img class="image" src="{{ site.url }}/assets/images/internet_gataway_aws.png" alt="Internet Gateway">

2° - Clique em "Create Internet Gateway".
<img class="image" src="{{ site.url }}/assets/images/create_internet_gataway_aws.png" alt="Internet Gateway">

3° - Informe os dados.
<img class="image" src="{{ site.url }}/assets/images/create_dados_internet_gataway_aws.png" alt="Internet Gateway">

4° - Clique em Attach VPC
<img class="image" src="{{ site.url }}/assets/images/attach_internetgataway_aws.png" alt="Internet Gateway">

5° - Ataxada a VPC ao Internet Gateway que acabou de criar
<img class="image" src="{{ site.url }}/assets/images/attach_dados_internetgataway_aws.png" alt="Internet Gateway">

Pronto! Terminou Internet Gateway.

## Route Tables

Uma vez que o Internet Gateway já está configurado é hora de configurar o Route
Tables.

1° - Vá para "Route Tables".
<img class="image" src="{{ site.url }}/assets/images/route_table_aws.png" alt="Route Tables">

2° - Você vai ver que já existe um "Route table" criado por padrão -> Edite o
nome dele para "RT-Private"
<img class="image" src="{{ site.url }}/assets/images/default_route_table_aws.png" alt="Route Tables">
<img class="image" src="{{ site.url }}/assets/images/default_route_table_aws.png" alt="Route Tables">

3° - Clique em "Create Route Table"
<img class="image" src="{{ site.url }}/assets/images/create_route_table_aws.png" alt="Route Tables">

4° - Crie um Route Table com o nome "RT-Public"
<img class="image" src="{{ site.url }}/assets/images/create_dados_route_table_aws.png" alt="Route Tables">

Após esses procedimentos a grid de Route Table estará assim:
<img class="bigger-image" src="{{ site.url }}/assets/images/dados_route_table_aws.png" alt="Route Tables">

Agora que criamos esses 2 Route Tables é hora de dizer qual Route tem acesso a
Internet utilizando o Gateway criado no passo anterior.

1° - Com a "RT-Private" selecionada clique em "Subnet Associations"
<img class="image" src="{{ site.url }}/assets/images/subnets_associations_aws.png" alt="Route Tables">

2°  - Clique no botão "Edit" -> Selecione apenas as Subnets privadas -> Clique
em no botão Save
<img class="image" src="{{ site.url }}/assets/images/private_subnets_routes_tables.png" alt="Route Tables">

3° - Realize o mesmo procedimento do passo anterior na "RT-Public" mais agora
claro selecionando as Subnets publicas.
<img class="image" src="{{ site.url }}/assets/images/public_subnets_routes_tables.png" alt="Route Tables">

4° - Na "RT-Public" clique na aba "Routes"
<img class="image" src="{{ site.url }}/assets/images/route_routes_tables.png" alt="Route Tables">

5° - Clique no botão "Edit" -> Adicione os dados igual a imagem seguinte
<img class="image" src="{{ site.url }}/assets/images/dados_route_routes_tables.png" alt="Route Tables">

O que foi feito aqui é basicamente o seguinte: Todos os IP na minha Route Table
"RP-Public" que tem as Subnets publicas ataxadas a ela tem seus IPS liberados
associadas ao Internet Gateway configurado no passo anterior, ou seja, liberamos
acesso a internet na rede publica da aplicação.

## EC2

Agora que roda a rede da aplicação está configurada é hora de trabalhar com o
serviço EC2.
O EC2 é o serviço da amazon que vamos criar instancias de servidores Cloud para
hospedar a aplicação.

1° - Vá até o serviço EC2
<img class="bigger-image" src="{{ site.url }}/assets/images/ec2.png" alt="EC2">

2° - Clique no botão "Launch Instance"
<img class="image" src="{{ site.url }}/assets/images/create_instance_ec2.png" alt="EC2">

3° - Escolha qual será o sistema operacional do seu Cloud clicando no
botão "Select"
<img class="image" src="{{ site.url }}/assets/images/servidores_ec2.png" alt="EC2">

Nesse guia eu escolhi a imagem padrão da Amazon "amazon linux ami"

4° - Escolha qual será a configuração ou potencial do servidor -> Clique no
botão "Next: Configure Instance Details"
<img class="image" src="{{ site.url }}/assets/images/potencia_servidor_aws.png" alt="EC2">

Mais uma vez para o guia eu escolhi a opção t2.micro, a versão default mesmo.

5° - Em detalhes da instancia é importa prestar atenção nos campos: Network e
Subnet -> Após preencher os campos clique no botão "Next: Add Storage"
<img class="image" src="{{ site.url }}/assets/images/detalhes_instancia_aws.png" alt="EC2">

Em "Network" escolha a VPC que criamos, no campo "Subnet" selecione a
"Public 2a"

6° - Em Storage no campo "Size" configure o tamanho que quiser no HD do seu
Cloud no campo "Volume Type" você pode escolher se vai querer um HD SSD ou
Magnetico por exemplo -> Após suas configurações clique no botão "Review and Launch"
<img class="image" src="{{ site.url }}/assets/images/storage_ec2_aws.png" alt="EC2">

Para o guia eu deixei o HD com 20GB e do tipo SSH


7° - Na tela de revisão da sua instancia vá até "Security Groups" -> Clique em
"Edit security groups" -> Configure um nome para o grupo de segurança da sua
instancia -> Clique em Add Rule -> No campo "Type" escolha a opção "HTTP" ->
Clique no botão "Review and Launch"
<img class="image" src="{{ site.url }}/assets/images/review_ec2_aws.png" alt="EC2">
<img class="image" src="{{ site.url }}/assets/images/grupo_securanca_ec2_aws.png" alt="EC2">
<img class="image" src="{{ site.url }}/assets/images/novo_grupo_securanca_ec2_aws.png" alt="EC2">

8° - Clique no botão "Launch"
<img class="image" src="{{ site.url }}/assets/images/launch_eview_ec2_aws.png" alt="EC2">

9° - Será solicitado que você crie uma chave para acesso SSH do servidor
<img class="image" src="{{ site.url }}/assets/images/chave_ec2_aws.png" alt="EC2">

Importante que você não perca essa chave nunca. Após baixar a chave lembre-se de
dar a permissão 400 a ela. Caso trabalhe com Linux ou Mac use o seguinte comando:
{% highlight html %}
chmod 400 EconomiaMaxima.pem
{% endhighlight %}

Pronto! Sua instancia EC2 já foi criada.

## Configurando nossa aplicação

Uma vez que a instancia da EC2 foi criada é hora de configurar nossa aplicação
para rodar.

Agora é a hora de instalar o apache, python, php ou qualquer outro programa que você
tenha necessidade para rodar sua aplicação.

1° - Temos que acessar a instancia e pra isso precisamos do IP da instancia para
pegar o IP vá até instancias -> Clique sobre a instancia -> Na aba "Description"
tem um campo "IPv4 Public IP" nele contém o ip para acessar sua instancia.
<img class="image" src="{{ site.url }}/assets/images/ip_instancia_ec2_aws.png" alt="EC2">

2° - Com o IP em mãos acesse sua instancia e configure ela da forma como quiser.
Eu fiz a instalação dos seguintes programas:
{% highlight html %}
ssh -i EconomiaMaxima.pem ec2-user@54.203.128.77
sudo su
yum install php56
chkconfig httpd on
service httpd start
{% endhighlight %}
vi /var/www/html/index.php
{% highlight html %}
<?php
echo "Hello world";
{% endhighlight %}
vi /var/www/html/phpinfo.php
{% highlight html %}
<?php
phpinfo();
{% endhighlight %}

Para o guia apenas ter o servidor web rodando e uma linguagem de programação já
está ótimo.

No final ao acessar o IP da instancia vocÊ terá a seguinte página:

<img class="image" src="{{ site.url }}/assets/images/hora_ec2_aws.png" alt="EC2">

O phpinfo que criamos:

<img class="image" src="{{ site.url }}/assets/images/ec2_phpinfo_aws.png" alt="EC2">

## Amazon AMI

Uma vez que temos nossa instancia configurada e rodando é hora de trabalhar com o
Amazon AMI com esse serviço vamos criar ISO da instancia atual.

E porque isso? Quando chegar o momento de trabalhar com o Load Balance e o Auto
Scaling vamos apontar nossa ISO criada nesse momento para que ela serva de base
para as instantcias criadas automaticamente.

1° - Na grid de instancias selecione a isntancia que acabamos de criar ->
Clique em "Actions" -> Image -> Create Image
<img class="image" src="{{ site.url }}/assets/images/ami_create_image_aws.png" alt="AMI">

2° - Informe os dados da sua imagem
<img class="image" src="{{ site.url }}/assets/images/dados_ami_create_aws.png" alt="AMI">

3° - Vá para o menu lateral da esquerda IMAGES -> AMIs -> Vocẽ verá que temos
uma imagem nova, nesse caso ainda sendo processada.
<img class="image" src="{{ site.url }}/assets/images/ami_aws.png" alt="AMI">

Com o passo da AMI já termiado vamos para o penultimo passo do guia "Load Balancing"

## Load Balancing

Load Balancing é o serviço que fica responsável por receber a requisição do cliente
e dizer para que maquina essa requicição vai apontar.

1° - Aqule velho passo vá para o menu Load Balancing e clique no botão
"Create Load Balancing"
<img class="image" src="{{ site.url }}/assets/images/lb_aws.png" alt="LB">

2° - No passo seguinte você terá que dar um nome para o
<img class="image" src="{{ site.url }}/assets/images/name_create_lb_aws.png" alt="LB">
