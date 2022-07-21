# sever_mongo
Neste repositório encontrase os desafios propostos pelo compasso.uol durante o estágio.

## Descrição do desafio 1
* criar uma aplicação com uma imagem mongo no docker
* Para subir um servidor Oracle Linux, sem interface gráfica.

1. Instalar 1 imagem Oracle Linux last stable version, para execução de atividades;
2. Ajustar a rede da máquina virtual para um cidr/24, sob o protocolo IPv4, utilizando um range classe A.
3. Deixar a rede virtual em modo NAT para acesso à internet;
4. Ajustar LVMs para as partições em separado: /home, /var, /tmp; (durante a instalação);
5. Configurar hostname;
6. Ajustar DNS com o nome labmongodbancodocker;
7. Configurar IP fixo;
8. Configurar SSH;
9. Bloquear o acesso SSH para root;
10. Criar um filesystem /var/lib/docker com 10GB em EXT4 para as imagens do docker;
11. Criar um projeto de versionamento;
12. Subir um Docker;
13. Instalar uma imagem da aplicação mongoDB via Docker;

## Features
###  Oracle VM VirtualBox
O VirtualBox é um programa de virtualização da Oracle que permite instalar e executar diferentes sistemas operacionais em um único computador sem complicações. Com ele, o usuário pode executar o Linux dentro do Windows 7, o Windows dentro do Mac, o Mac dentro do Windows e até mesmo todos os sistemas suportados dentro de um.
### Oracle Linux 
O Oracle Linux fornece uma alternativa compatível para o Red Hat Enterprise Linux e CentOS Linux e é suportado em ambientes híbridos e multi-nuvem. É um ambiente operacional seguro e com alto desempenho entrega ferramentas de virtualização, gestão, automatização e computação nativa na nuvem, junto do sistema operacional, com um suporte fácil de gerenciar.
### Docker
O Docker é uma plataforma open source que facilita a criação e administração de ambientes isolados. Ele possibilita o empacotamento de uma aplicação ou ambiente dentro de um container, se tornando portátil para qualquer outro host que contenha o Docker instalado. Então, é possível criar, implantar, copiar e migrar de um ambiente para outro com maior flexibilidade. A ideia do Docker é subir apenas uma máquina, ao invés de várias. E, nessa única máquina, você pode rodar várias aplicações sem que haja conflitos entre elas.
### MongoDB
É um banco de dados opensource, de alta performance e flexível, sendo considerado o principal banco de dados NoSQL. O MongoDB é orientado a documentos, ou seja, os dados são armazenados como documentos, ao contrário de bancos de dados de modelo relacional, onde trabalhamos com registros em linhas e colunas. Os documentos podem ser descritos como dados no formato de chave-valor, no caso, utilizando o formato JSON (JavaScript Object Notation).
### OpenSSH
OpenSSH é a principal ferramenta de conectividade para login remoto com o protocolo SSH. Ele criptografa todo o tráfego para eliminar espionagem, seqüestro de conexão e outros ataques. Além disso, o OpenSSH oferece um grande conjunto de recursos de encapsulamento seguro, vários métodos de autenticação e opções de configuração sofisticadas.
fonte: https://www.openssh.com/