# desafio 2 
## Configurando a internet na VM VirtualBox

1. Va nas configuraçoes da maquina linux pela vm, va em rede e mude a opção conectado a: NAT para placa em modo bridge

## Configurando a internet

1. Execute o comando cd /etc/sysconfig/network-scripts/  seguido de sudo vi ifcfg-enp0s3 ou sudo vi {unico arquivo do diretório} e mude a configuração BOOTPROTO=static para dhcp e comente as seguintes linhas:
* #IPADDR=10.0.2.15
* #NETMASK=255.255.255.0
* #GATEWAY=10.0.2.2
2. Apos comentar as linhas acima apertando esc seguido de :wq para salvar e sair.
3. Execute o comando systemctl restart network caso não funcione dê um reboot.
4. agora execute o comando ip addr e confira o ip fornecido pelo servidor dns(roteador).
ex. 
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:77:ac:f5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.106/24 brd 192.168.100.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86241sec preferred_lft 86241sec
    inet6 fe80::a00:27ff:fe77:acf5/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
5. em seguida abra novamente o arquivo ifcfg-enp0s3 e mude a configuração BOOTPROTO=dhcp para static e modifique e descomente as segintes linhas:
* IPADDR=192.168.100.106 (ip dado pelo roteador)
* NETMASK=255.255.255.0 (mesma mascara de rede)
* GATEWAY=192.168.100.1 (ip do roteador)
