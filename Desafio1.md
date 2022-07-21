## Instalação
Caso o Sistema Operacional da máquina seja Oracle Linux, não será necessário baixar a Oracle VM VirtualBox para completar estes passos. Caso contrário, é importante baixar a Oracle VM VirtualBox.

## Oracle VM VirtualBox
1. Na ferramenta de buscas, pesquisar o site oficial da Oracle VM VirtualBox: https://www.virtualbox.org/
2. Entrar em downloads e baixar o instalador de acordo com o sistema operacional da máquina.
3. Executar o instalador para iniciar a instalação da VM. 
> 4. HD de 60gb
> 5. RAM 2gb
6. Depois de criada, já está pronta para ser utilizada. 
## Oracle Linux
1. Na ferramenta de buscas, pesquisar o site oficial de download do Oracle Linux: https://yum.oracle.com/oracle-linux-isos.html
2. Baixar a última versão Full ISO.
3. Na VirtualBox, criar uma nova máquina. Geralmente, as opções recomendadas pelo sistema da VM são as melhores para o momento de instalação.
4. É possível fazer melhorias customizadas nas configurações da máquina antes mesmo da instalação.
5. Em configurações, deve-se entrar em Armazenamento e selecionar a imagem ISO (ex: OracleLinux-R8-U6-x86_64-dvd.iso) com a qual essa específica máquina virtual deverá interagir.
6. Iniciar a máquina.
7. Selecionar Install Oracle Linux.
8. Na parte de escolher o idioma, é recomendado escolher o inglês padrão.
9. Alterar o __Keyboard Layout__, se necessário (ex: mudar o padrão do teclado para portugues Brasil).
10. Em __Installation Destination__, selecione o disco.
a. Em __Storage Configuration__, selecione __Costum__, depois confirme em __Done__ (Isso permitirá que façamos repartições personalizadas no nosso hd automaticamente antes da instalação).
b. Selecione __Click here to create them automatically__ (selecionando essa opção ele nos apresentará as partições que serão criadas e nos permitirá criar e redimensionar a quantidade de memória em cada uma das repartições ).
c. redimensione a pasta __/__ e __/home__ e adicione as pasta __/tmp, /var__ e __/var/lib/docker__ (!a pasta __/var/lib/docker__ tem que ter __10 GIB__ e o __File System__ tem que estar selecionado em __ext4__ | a pasta __/__ tem que ter no mínimo __10 GIB__ pra rodar o sistema | e as pasta __/var, /tmp, /home__ e __/__ tem que estar com o __File System__ padrão que é o __xfs__ !).
d. Selecione __Done__ e depois __Accept Changes__.
11. Vá em __time & date__  para mudar o padrão de hora e data para o de sua região.
12. Vá em __Software Selection__ e mude para o pacote de instalação __Minimal Install__.
13. Vá em __Root Password__ e defina uma senha para o usuário Root.
14. Vá em __User Creation__ e defina um usuário privilegiado para utilizar.
15. Vá em __Network & Host..__ e ative a internet.
16. Agora só instalar no __Begin Installation__.

## Configurando o acesso ssh
1. Execute o comando `sudo vi /etc/ssh/sshd_config` e aperte __:__ depois digite `set number` isso fará com que apareçam números nas linhas, agora altere a seguintes linhas respectivamente:
```
 43 PermitRootLogin no
 48 PubkeyAuthentication yes
 84 #GSSAPIAuthentication yes
 85 #GSSAPICleanupCredentials no
 101 UsePAM yes
 ```
2. Depois salve apertando __esc__ seguido de __:wq__ para salvar e sair.
3. Execute o comando `systemctl restart sshd` depois `systemctl inicia sshd` seguido de `systemctl habilita sshd` e pronto.

## Configurando a internet
1. Execute o comando `cd /etc/sysconfig/network-scripts/`  seguido de `sudo vi ifcfg-enp0s3` ou `sudo vi {unico arquivo do diretório}` e mude a configuração `BOOTPROTO=dhcp` para  ``static`  e adicione as seguintes linhas:
```
IPADDR=10.0.2.15
NETMASK=255.255.255.0
GATEWAY=10.0.2.2
```
2. Depois salve apertando __esc__ seguido de `:wq` para salvar e sair.
3. Execute o comando `systemctl restart network` caso não funcione dê um reboot. 
4. Confira através do `ping google.com` ou do `yum install mlocate` se a internet está funcionando.

## Configurando o DNS
1. Execute o seguinte comando `sudo vi /etc/hosts` e adicione a seguinte linha: 
```
127.0.0.1   labmongodbancodocker 
```
depois  salve __esc__ aperte `:wq` para salvar e sair.
2. Agora no Windows vá no arquivo `C:\Windows\System32\drivers\etc\hosts` do Windows e adicione a seguinte linha:
```
127.0.0.1   labmongodbancodocker 
```
depois salve e saia.

## Configurando o redirecionamento de portas na VM
1. Vá nas configurações da máquina linux.
2. Vá na opção __rede__ e aperte em __avançado (D)__ e selecione __Redirecionamento de Portas__.
3. Utilizando o botão verde à esquerda adicione dois registros com os seguintes parâmetros.
* porta hendereço ip do ospodeiro: 127.0.0.1, porta hospedeiro: 2222 e porta convidado: 22
* porta hendereço ip do ospodeiro: 127.0.0.1, porta hospedeiro: 8081 e porta convidado: 8081
Por fim salve tudo e saia.

## Docker
1. Instale o docker https://docs.docker.com/engine/install/centos/
2. Crie um grupo docker `sudo groupadd dock` depois adicione seu usuário ao grupo `sudo usermod -aG docker $USER` e por fim execute `sudo systemctl enable docker.service` e `sudo systemctl enable containerd.service` para o docker iniciar junto da vm. 
3. Agora só rodar o docker compose com `docker compose up -d` (obs o comando deve ser rodado na mesma pasta que se encontra o arquivo bancomongo.yml).
4. a plicação mongoexpres extara rodando na porta 8081 do pc acesse a vm usando o comando `ssh user@dns -p2222` no prompt de comando ou colocando no navegador __dns:8081__.