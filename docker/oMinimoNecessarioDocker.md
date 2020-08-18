# Docker: o mínimo que você precisa saber

**TODO**

## 1 Executar containers 

Um dos pilares do sucesso do Docker é sua simplicidade. Você precisa de *pouco* para começar a ser produtivo e sair do zero.

## 1.1 Primeiros passos

Você "roda" containers usando o comando *docker run*.

* **Execução de um container Nginx**:

```
# # Transcrção semi integral da saída! Não precisa ler a totalidade deste quadro ou dos demais!

# docker run nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bf5952930446: Pull complete 
cb9a6de05e5a: Pull complete 
9513ea0afb93: Pull complete 
b49ea07d2e93: Pull complete 
a5e4a503d449: Pull complete 
Digest: sha256:b0ad43f7ee5edbc0effbc14645ae7055e21bc1973aee5150745632a24a752661
Status: Downloaded newer image for nginx:latest
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Configuration complete; ready for start up

```

Pronto! Vocẽ tem um servidor Nginx em execução na sua máquina.

Duvida? É só conferir:

```
# pgrep -a nginx 
31831 nginx: master process nginx -g daemon off;
31901 nginx: worker process
```
---

Docker é **tão eficiente** em ser simples que é bastante provável que você chute o nome de qualquer software famoso e consiga executá-lo sem problemas. De onde eles vêm? Para onde vão? Ah, quem se importa, apenas execute e siga em frente!

* **execução de um container Tomcat**:

```
# docker run tomcat
Unable to find image 'tomcat:latest' locally
latest: Pulling from library/tomcat
d6ff36c9ec48: Pull complete 
...
Digest: sha256:9de2415ccf10fe8e5906e4b72eda21649a7a1d0b88e9111f8409062599f3728e
Status: Downloaded newer image for tomcat:latest
...
18-Aug-2020 03:05:07.211 INFO [main] org.apache.catalina.core.StandardEngine.startInternal Starting Servlet engine: [Apache Tomcat/9.0.37]
18-Aug-2020 03:05:07.231 INFO [main] org.apache.coyote.AbstractProtocol.start Starting ProtocolHandler ["http-nio-8080"]
18-Aug-2020 03:05:07.253 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [150] milliseconds

```
---
Fantástico!

* **Execução de um container Wordpress**

```
# docker run wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
bf5952930446: Already exists 
a409b57eb464: Pull complete 
...
Digest: sha256:6da8f886b20632dd05eeb22462f850a38e30600cedd894d2c6b1eb1a58e9763c
Status: Downloaded newer image for wordpress:latest
WordPress not found in /var/www/html - copying now...
Complete! WordPress has been successfully copied to /var/www/html
...
[Tue Aug 18 03:05:14.382338 2020] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
```

* **Execução de um container Elasticsearch**

Ouvi falar  desse tal de Elasticsearch. Vamos testá-lo:

```
# docker run elasticsearch 
Unable to find image 'elasticsearch:latest' locally
docker: Error response from daemon: manifest for elasticsearch:latest not found: manifest unknown: manifest unknown.
See 'docker run --help'.
```

Falhou. Será que não existe?

---

## Repositórios de imagens Docker

O Docker faz um excelente trabalho em simplificar as coisas para o usuário. Ele abstrai o mundo de você até certo ponto. Eventualmente, você precisa saber o que está fazendo.

Containers Docker são criados a partir de **imagens Docker**.  Alguém precisa criar - e disponibilizar - conteúdo como uma imagem especificamente para esta finalidade. 

Ao executar '*docker run*', o comando verifica se existe uma imagem com o nome especificado localmente. Em **todas** as execuções anteriores, ele exibiu a mensagem indicando isso:

```
# docker run nginx
Unable to find image 'nginx:latest' locally
...
# docker run tomcat
Unable to find image 'tomcat:latest' locally
latest: Pulling from library/tomcat
...

# docker run wordpress
Unable to find image 'wordpress:latest' locally
latest: Pulling from library/wordpress
...

# docker run elasticsearch 
Unable to find image 'elasticsearch:latest' locally
...
```
O [Docker Hub](https://hub.docker.com/) é um dos principais repositórios de imagens Docker da Internet. O [Red Hat Quay](https://quay.io/) é outro. 

Em todos os exemplos acima, não foi indicado **de onde baixar** as imagens Docker. Desta forma, assume-se que a imagem especificada obviamente está no **Docker Hub**.

Os links abaixo apontam para a "página" do repositório oficial de cada imagem.

* https://hub.docker.com/_/nginx
* https://hub.docker.com/_/tomcat
* https://hub.docker.com/_/wordpress
* https://hub.docker.com/_/elasticsearch

Seguindo o link acima, pode-se observar que existe, sim, um repositório para Elasticsearch.

Por que a tentativa de execução não funcionou?

## Nome e Tag de imagens

A razão pela qual a execução da imagem Elasticsearch falhou foi pela inexistência da '*tag*' especificada. 

Por incrível que pareça, a mensagem, embora relativamente críptica, explica isso:

```
...
docker: Error response from daemon: manifest for elasticsearch:latest not found: manifest unknown: manifest unknown.
...
```

Remetendo à simplicidade Docker de ser, da mesma forma que a ausência de nome de um servidor leva o Docker a buscar a imagem no Docker Hub, a ausẽncia de uma '*Tag*' faz com que o Docker busque automaticamente a Tag '*latest*', como indicado na mensagem (note o '*elasticsearch:latest*' acima).

Como *Latest* remete imeditamente a versão, fica claro que este é o propósito da *Tag*: diferenciar entre várias versões de um mesmo software.

A seção '*Description*' do Elasticsearch confirma essa declaração:

```
Supported tags and respective Dockerfile links
* 7.8.1
* 6.8.11
```

Por que não usar '*Version*' em vez de '*Tag*'? Bem, vocẽ pode usar para especificar outras diferenças de uma mesma versão, como, por exemplo, nas imagens do [HAProxy](https://hub.docker.com/_/haproxy)

```
Supported tags and respective Dockerfile links

* 2.3-dev3, 2.3-dev
* 2.3-dev3-alpine, 2.3-dev-alpine
* 2.2.2, 2.2, latest, lts
* 2.2.2-alpine, 2.2-alpine, alpine, lts-alpine
```

Observe que o HAProxy conta com uma '*Tag*' *Latest*, além de duas imagens versão 2.2.2: uma "simples" e a outra "alpine" (que, ironicamente, é ainda mais simples!)

**FAQ**: Por que o Elasticsearch não tem uma imagem com a *Tag* *Latest*? 

Além de não ser obrigatório, usar a imagem *Latest* para alguma coisa que não seja um simples teste aleatório da tecnologia é uma práticamente **amplamente não recomendada**. Não é possível saber, de antemão, que versão será executada, e na esmagadora maioria dos softwares modernos, uma diferença de '*major version* (como no caso do Elastic, 7.8.1 para 6.8.11) significa quebra de retrocompatibilidade. 

Mas a resposta definitiva mesmo é: porque eles não querem, e deixam bem claro isso no próprio Docker Hub:
```
How to use this image
Note: Pulling an images requires using a specific version number tag. The latest tag is not supported.
```

Portanto, não conte com versões '*Latest*' de containers, e fora de laboratórios de curiosidade, não as use ainda que existam.

---

Voltando ao Elasticsearch, basta especificar uma das versões válidas de '*tag*' para executá-lo como os demais:

```
# docker run elasticsearch:7.8.1
Unable to find image 'elasticsearch:7.8.1' locally
7.8.1: Pulling from library/elasticsearch
...
Status: Downloaded newer image for elasticsearch:7.8.1
{"type": "server", "timestamp": "2020-08-18T04:06:12,571Z", "level": "INFO", "component": "o.e.n.Node", "cluster.name": "docker-cluster", "node.name": "24b678d385af", "message": "version[7.8.1], pid[11], build[default/docker/b5ca9c58fb664ca8bf9e4057fc229b3396bf3a89/2020-07-21T16:40:44.668009Z], OS[Linux/5.3.0-62-generic/amd64], JVM[AdoptOpenJDK/OpenJDK 64-Bit Server VM/14.0.1/14.0.1+7]" }
...
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /usr/share/elasticsearch/logs/docker-cluster.log
```

O container Elasticsearch executou! Tudo bem que ele foi temperamental e deu um erro, mas vamos deixá-lo para lá. Talvez em um "Elasticsearch: o mínimo necessário" podemos voltar para ele!

## Passagem de variáveis de ambiente para o container

Vamos tentar executar um **Mysql**:

```
# docker run mysql
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
bf5952930446: Already exists 
8254623a9871: Pull complete 
...
2020-08-18 02:27:05+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-08-18 02:27:06+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2020-08-18 02:27:06+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
2020-08-18 02:27:06+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
```

A imagem foi encontrada e baixada do Docker Hub. Mas a execução falhou com um erro!

Quer dizer que ainda não sabemos o mínimo necessário para subir containers Docker... Sigamos em frente.

O container [Mysql](#executar-um-mysql) do exemplo anterior falhou. A mensagem de log explica que você precisa fazer algo:

```
2020-08-18 02:27:06+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
```

Isto é, definir uma das três variáveis de ambiente da mensagem.

Então, o objetivo é passar uma variável de ambiente com a senha 'root' do Mysql para o container. 

Como "passar" variáveis de ambiente para o container a ser executado? 

```
# # Várias mensagens foram suprimidas, sem prejuizo do objetivo final.

# docker run  -e MYSQL_ROOT_PASSWORD=mpnsc@96 mysql
Unable to find image 'mysql:latest' locally
latest: Pulling from library/mysql
...
Status: Downloaded newer image for mysql:latest
...
2020-08-18 02:41:59+00:00 [Note] [Entrypoint]: MySQL init process done. Ready for start up.

2020-08-18T02:42:00.251841Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.21) starting as process 1
...
[MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.21'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
```

