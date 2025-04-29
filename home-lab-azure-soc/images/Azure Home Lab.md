Notas do processo de criação do meu primeiro Home lab feito no AZURE

primeiro eu criei a conta no azure no link: **

[https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account)

**

na aba de portal da azure o inicio/dashboard inicial eu busquei por Resource groups e criei o meu primeiro com o nome de EW-SOC-Lab

após realizar a criação do Resource Group eu criei uma Virtual network que seria como uma rede de cabos dentro do meu lab as configs da VN foram nome Vnet-soc-lab na região US east US

em security deixei sem marcar nada, o ip da VM deixei padrão no range 10.0.0.0 - 10.0.0.255

deixei sem tags

### Basics

Subscription

Azure subscription 1

Resource Group

EW-SOC-Lab

Name

Vnet-soc-lab

Region

East US

### Security

Azure Bastion

Disabled

Azure Firewall

Disabled

Azure DDoS Network Protection

Disabled

### IP addresses

Address space

10.0.0.0/16 (65 536 addresses)

Subnet

default (10.0.0.0/24) (256 addresses)


depois disso eu decidi criar um 'Honey pop' para atrair atacantes de proposito, pois vou usar isso no monitoramento, a ideia é ser vuneravel de proposito ou seja um VM

criando essa VM dei o nome de CORP-NET-EAST-1 pra atrair atacantes, deixei dentro do grupo feito antes o EW-SOC-Lab, deixei a zona em AUTO e coloquei a IMagen do W10 pro mesmo

ele apenas funciona na zona 2 acabei de colocar na zona 2 então, criei no tamanho basico B1 para não ter custos, já que estou usando conta gratuita 

dei o nome de labuser

e a senha criei como @labuser@365


as configs do network deixei 

## Network interface

When creating a virtual machine, a network interface will be created for you.

Virtual network

Vnet-soc-lab



Subnet

default (10.0.0.0/24)



Public IP

(new) CORP-NET-EAST-1-ip

segui em fui finalizando o processo agora dentro do meu EW-SOC-lab

está assim
![[Resource Group PRINT.png]]

afora eu configurar o nsg a parte de sec da minha maquina que está na cloud para permitir qualquer pessoa acessar as coisas, em um cenario real e normal isso seria terrivel, mas aqui é para estudo então ta valendo

então no inblound sec rules, eu removi o RDP para configurar que qualquer coisa possa acessar a maquina não apensas RDP

depois em configurações eu fui em inbliound sec rules e coliquei em ADD para criar uma 

e configuro com tudo em any os que não possuem opção de any eu coloco uma * 

![[inbound sec rules PRINT.png]]

agora vou abrir a maquina e desligar o firewall dela, fui em vms abri a CORP-NET-EAST-1 que criei o ip publico dela é 52.146.21.69 conectei e

![[LoginW10 PRINT.png]]

depois disso dentro da VM busquei wf.msc para configurar o firewall como eu tinha falado mais cedo, clicando em windows defender firwall properties

![[Firewall off PRINT.png]]

testei se deu certo fazendo um ping da minha maquina mesmo, segue o print 

![[ping Public IP PRINT.png]]

depois do ping confirmando que pessoas podem foder com esse IP publico pq desloiguei o firewall eu fechei a maquina, abri de novo e tentei logar com um usuario falso e errei a senha 4 vezes de proprosito, ai loguei com o usuario certo o labuser e abri event viewer

para analisar logs, fui em windows log, security e busquei por 4625 e achei o erros de login

![[logs login PRINT.png]]
![[log details PRINT.png]]

até agora nada foi realmente feito, apena configurei a maquina virtual e abri ela pra que possam tentar logar nela 

agora vou creiar um repositorio de LOG

dentro do azure fui em log analytics woekspace e criei um log desse chamado LAW-soc-lab-0001

depois vou criar meu SIEM 

em Microsoft Sentinal 
e adicionar o siem no log que criei o law soc lab 0001

agora eu consegui acessar o log do nosso log repositorio no SIEM

dentro do Microsoft sentinel eu baixei em content hub 

windows security event e comecei a configurar em manage

ativei o Windows Security Events via AMA e criei um data collection rule 

e chamei de WSE-windows

fui no meu log analytcs workpace fui em logs coloquei por KQL mode e filtrei com SecuritySEvent

e segue os logs ![[logs do LAW-soc.png]]


esse evento de segurança são TABLES 

e tem como sempre filtar nesse KQL um exemplo foi que filtrei pra ver o TimeGenerated, Account, Computer, EventID, Activity, IpAddress

![[logs filtrados KQL mode.png]]


depois eu vou criar um localizador que vai mostrar de onde vem o ip de forma grafic, baixei um arquivo chamado geoip.csv e no azure fui no microsoft setinel configuration watchlisyt

criei um chamado geoip e criei um

adicionei o geoip então isso basicamente criou outra table dentro do loga analisys

então eu filtrei no law-soc

com _GetWatchlist("geoip")

e exibiu o country, cidade, latitude e afins 

![[LOGS with geoip.png]]

depois disso agora para ter de uma forma visual melhor de monitoramente eu baixei um arquivo da hnnet chamado map.json para eu criar um workbook que fica em Microsoft Sentinel 

copiei o codigo e coloquei no workbook 

![[windows vm attack map.png]]