# WORDPRESS SITE:

Este repositório contem o código docker compose para montar a estrutura de containers para hospedar um site wordpess.

Um site wordpress para poder rodar necessita de um banco de dados Mysql na versão 5.7.

Na configuração do nosso compose, nós vamos criar dois serviços(services):

```docker
services:
  db_wordpress:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: wordpressadmin
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:
    depends_on:
      - db_wordpress
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db_wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
 ``` 

 O primeiro serviço que iremos criar é o serviço do db_wordpress que vai criar um container mysql necessário para armazenar as informações do site que será construído a partir do wordpress.

 Para este serviço vamos usar a imagem oficial do mysql na versão 5.7.

 Um volume chamado **db_data** será criado para armazenar os dados do banco criado para o wordpress.

 A imagem oficial do Mysql determina que algumas variáveis de ambiente devem ser informadas. No nosso arquivo compose. Estas são especificadas no item environmemt do serviço **db_wordpress**.São elas:
 
 **MYSQL_ROOT_PASSWORD** 

 **MYSQL_DATABASE**

 **MYSQL_USER**

 **MYSQL_PASSWORD**

 O segundo serviço que iremos criar é o próprio wordpress. Uma recomendação da comunidade do Wordpress é sempre usar a imagem latest devido a um contínuo processo de averigação de ameaças de segurança.

 No código do nosso compose, nós informamos que o nosso serviço depende do servico do banco de dados Mysql **db_wordpress** .

 No nosso serviço wordpress usaremos a imagem latest do wordpress.

 Um volume chamado **wordpress_data** será criado para mapear os nossos arquivos do site wordpress para dentro do diretório **var/www/html** no container que será criado.

 Para que a nosso site baseado no wordpress possa ser acessado vamos mapear a porta 8000 para a porta 80 do nosso container wordpress.

 A imagem do wordpress também necessita que algumas variáveis de ambiente sejam especifadas no item environment do serviço **wordpress**.

 **WORDPRESS_DB_HOST** Neste item devemos informar o nome do nosso serviço do banco **db_wordpress** .
 
 **WORDPRESS_DB_USER**
 
 **WORDPRESS_DB_PASSWORD**
 
 **WORDPRESS_DB_NAME** 

 No nosso compose, temos que informar os volumes que usaremos nos nossos serviços no item **volumes** .

 ```docker
 volumes:
  db_data: {}
  wordpress_data: {}
 ```
 Para executar o nosso arquivo compose, nós usaremos o seguinte comando:

 ```docker
 docker-compose up -d
 ```
 