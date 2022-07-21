# desafio 2 
## Status das vms
Nesse desafio estarei usando a vm do primeiro desafio como sendo o "servidor remoto" e criarei uma segunda vm seguindo as mesmas configurações da primeira vm essa sera minha "maquina local" com a diferença que nessa não instalarei o docker.
> * primeira vm: labmongodbancodocker (vm remota)
> * segunda vm: vmlocal (criatividade ao maximo)
## Configurando a internet
Esses passos devem ser feitos em anbas tanto na labmongodbancodocker quanto na vmlocal para que possao funcionar adequadamente.
### Configurando a internet na VM VirtualBox

1. Va nas configuraçoes da maquina linux labmongodbancodocker pela VirtualBox, va em __rede__ e mude a opção __conectado a: NAT__ para __placa em modo bridge__
2. Faça o mesmo passos na vmlocal

### Configurando a internet no Oracle linux

1. Na labmongodbancodocker execute o comando `cd /etc/sysconfig/network-scripts/` seguido de `sudo vi ifcfg-enp0s3` ou `sudo vi {unico arquivo do diretório}` e mude a configuração `BOOTPROTO=static` para `dhcp` e comente as seguintes linhas:
```
#IPADDR=10.0.2.15
#NETMASK=255.255.255.0
#GATEWAY=10.0.2.2
```
2. Apos comentar as linhas acima apertando esc seguido de :wq para salvar e sair.
3. Execute o comando `systemctl restart network` caso não funcione dê um reboot.
4. agora execute o comando `ip addr` e confira o ip fornecido pelo servidor dns(roteador).
ex. 
```
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:77:ac:f5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.100.106/24 brd 192.168.100.255 scope global dynamic noprefixroute enp0s3
       valid_lft 86241sec preferred_lft 86241sec
    inet6 fe80::a00:27ff:fe77:acf5/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```
5. em seguida abra novamente o arquivo ifcfg-enp0s3 e mude a configuração `BOOTPROTO=dhcp` para `static` e modifique e descomente as segintes linhas:
```
IPADDR=192.168.100.106 (ip dado pelo roteador)
NETMASK=255.255.255.0 (mesma mascara de rede)
GATEWAY=192.168.100.1 (ip do roteador)
```
6. Apos modificar as linhas acima apertando __esc__ seguido de `:wq` para salvar e sair.
7. Execute o comando `systemctl restart network` caso não funcione dê um reboot.
8. Confira através do `ping google.com` ou do `yum install mlocate` se a internet está funcionando.
9. refaça o mesmo processo na vm local.
10. Tente pingar uma maquina na outra atraves dos comando `ping labmongodbancodocker` na vmlocal e `ping vmlocal` na labmongodbancodocker

## Configurando o DNS
1. na vmlocal va no arquivo hosts pelo comando `sudo vi /etc/hosts` e adicione as seguintes linhas.
```
#ip da maquina remota   nome
192.168.100.107         labmongodbancodocker
#ip loopback            nome
127.0.0.1               vmlocal 
``` 
2. na labmongodbancodocker(vm remota) va no mesmo arquivo hosts pelo comando `sudo vi /etc/hosts` e adicione as seguintes linhas.
```
#ip da maquina vmlocal   nome
192.168.100.110         vmlocal
#ip loopback            nome
127.0.0.1               labmongodbancodocker 
```

## Configurando a relação de confiança entre as duas VMs

1. na vmlocal crie uma chave ssh con seguinte comando `ssh-keygen -t ed25519 -f ~/.ssh/lan` nesse processo ele pede uma palavra chave caso queira pode colocar mas não é necessario(OBS caso coloque a palavra chave ela será pedida quando realizar a conexão via chave).
```
$ ssh-keygen -t ed25519 -f ~/.ssh/lan

Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/kauavinicios/.ssh/lan.
Your public key has been saved in /home/kauavinicios/.ssh/lan.pub.
The key fingerprint is:
SHA256:F8HeDJUnDadbYNvK6Ulp/0srFRXhqwKTXlFH2U750ZM kauavinicios@localhost.localdomain
The key's randomart image is:
+--[ED25519 256]--+
|         ...==o=O|
|          oo+*=E=|
|         ..=ooo+=|
|          oo+* .+|
|        S+..O  ..|
|        ..++ o.. |
|         . .o.o. |
|            .....|
|              .oo|
+----[SHA256]-----+
```
2. depois de criada a chave copie para para labmongodbancodocker(vm remota) com o comando `ssh-copy-id -i ~/.ssh/lan.pub usuario@labmongodbancodocker`.
```
$ ssh-copy-id -i ~/.ssh/lan.pub grupo2@labmongodbancodocker
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/kauavinicios/.ssh/lan.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
grupo2@labmongodbancodocker's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'grupo2@labmongodbancodocker'"
and check to make sure that only the key(s) you wanted were added.
```
3. Depois de copiar a chave basta acessar a vm com o comando `ssh -i ~/.ssh/lan usuario@labmongodbancodocker`.
