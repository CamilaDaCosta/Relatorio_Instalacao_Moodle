Título: Relatório de Instalação do Wordpress
Autor: Camila da costa
Disciplina: Insfraestrutura para Sistemas Web

## Instalação do Moodle no Ubuntu Server 18.04

#### Introdução:
Moodle é uma plataforma online e gratuita de aprendizado a distância, também conhecido por ser um Ambiente Virtual de Aprendizagem (AVA). É um sistema que oferece a possibilidade de disponibilizar cursos e treinamentos de forma online. O nome Moodle é uma sigla em inglês para Modular Object-Oriented Dynamic Learning Environment. Podemos traduzir como “Ambiente de aprendizado modular orientado ao objeto”.

Para realizar o processo de instalação exposto neste relatório é necessário possuir a pilha LAMP com um servidor Web como Linux, Apache, MySQL, e PHP já previamente instalados na máquina.

#### Passo 1 - Baixe e instale a aplicação Moodle:
Crie uma nova pasta chamada downloads, onde será armazenado os arquivos para instalação:

    mkdir /downloads
    cd /downloads
O comando a seguir irá realizar o download da aplicação:

    wget https://download.moodle.org/stable38/moodle-latest-38.tgz
Em seguida, descompacte o arquivo baixado:

    tar -zxvf moodle-latest-38.tgz
Copie o arquivo descompactado para a pasta padrão do servidor web:

    cp /downloads/moodle /var/www/html/ -R
Para finalizar atribua as permissões de acesso à pasta, com:

    chown www-data.www-data /var/www/html/moodle -R
    chmod 0755 /var/www/html/moodle -R
#### Passo 2 - Configure a pasta moodledata:
A pasta moodledata é onde será armazenados os dados da aplicação, ela precisa de permissoes especificas para que a instalação tenha sucesso, execute os seguintes comandos:

    mkdir /var/www/moodledata
    chown www-data /var/www/moodledata -R
    chmod 0770 /var/www/moodledata -R
#### Passo 3 - Configurando o banco de dados mysql:
Acesso o mysql como usuário root:
    
    mysql -u root -p
Crie o banco de dados "moodle":

    CREATE DATABASE moodle DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
Crie o usuário "moodle" e atribua uma senha no campo "password":

    CREATE USER 'moodle'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
Dê ao usuário moodle as permissões de acesso ao bando criado, com:

    GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,CREATE TEMPORARY TABLES,DROP,INDEX,ALTER ON moodle.* TO moodle@localhost;
    
#### Passo 4 - Configurando arquivo config.php
Como usuário root, entre em:

    #cd /var/www/html/moodle
Copie o arquivo padrão de configuração para o config.php que utilizaremos e dê as permissões de acesso:

    cp config-dist.php config.php
    chmod 777 config.php
Edite o arquivo config.php com as seguintes linhas:

    $CFG->dbtype = 'mysqli'; // mysqli ou psql
    $CFG->dbhost = '192.168.0.1'; // o endereço do servidor
    $CFG->dbname = 'moodle'; // nome do banco criado no passo 3
    $CFG->dbuser = 'moodle'; // nome do usuário criado no passo 3
    $CFG->dbpass = 'moodle'; // senha do usuário criado no passo 3
    $CFG->prefix = 'mdl_'; // Prefixo para usar todos os nomes de tabelas
Verifique também as seguintes linhas no arquivo, elas devem estar com os caminhos corretos para os diretórios:
    
    $CFG->wwwroot = 'http://192.168.0.1/moodle';
    $CFG->dirroot = '/var/www/moodle';
    $CFG->dataroot = '/var/moodledata';

Salve e feche o arquivo config.php.

#### Passo 5 - Concluindo a instalação utilizando o Moodle Web Installer:
Primeiramente verifique o endereço IP de seu servidor, com:
    
    ifconfig
Depois abra o navegador web de sua máquina e o digite na barra de naveação, como no exemplo:
    
    http://192.168.0.1/html/moodle
Nesse endereço a interface de instalação web Moodle será apresentada, selecione o idioma e avançe:

![imagem seleção idioma](https://d3pjq9s091b915.cloudfront.net/wp-content/uploads/2020/02/moodle-install-language-800x61.jpg)

Certifique-se que todos os requisitos do PHP foram atendidos como na imagem, e avançe:
![imagem requisitos](https://d3pjq9s091b915.cloudfront.net/wp-content/uploads/2020/02/moodle-php-requirements-800x572.jpg)

Na próxima tela, execute as seguintes configurações com os seus dados pessoais, como na imagem de exemplo:

![imagem configuração](https://d3pjq9s091b915.cloudfront.net/wp-content/uploads/2020/02/moodle-administrator-account.jpg)

Conclua as configurações e já poderá utilizar os recursos do Moodle.




