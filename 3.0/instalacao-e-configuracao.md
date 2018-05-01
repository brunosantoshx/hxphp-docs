---
currentMenu: instalacao-e-configuracao
---

## Instalação {#instalacao}

Você precisa do [Composer](https://getcomposer.org/download) para instalar o HXPHP, portanto, antes de prosseguir, certifique-se que o mesmo está instalado em sua máquina. Para isso, abra o terminal (CMD no Windows) e digite o seguinte comando: 

> `composer --version`

Após constatar que o [Composer](https://getcomposer.org/download) está em pleno funcionamento, navegue até o diretório escolhido para instalação e execute o comando: 


> `composer create-project --prefer-dist hxphp/hxphp hxphp`


*Este comando cria o projeto e instala todas as dependências do framework.*

----

## Usando o Docker

Você precisa tanto do [Docker](https://www.docker.com/community-edition) como do [Docker Compose](https://docs.docker.com/compose/install/) instalados em sua máquina. 

Você pode conferir se ambos estão instalados, executando os seguintes comandos no terminal **(PowerShell, se usar Windows)**:

> `docker -v`

> `docker-compose -v`

----

### Credenciais

As credenciais de conexão com o MySQL estão no arquivo `.env` no diretório em que o HXPHP foi instalado. 

----

### Levantando os containers

Após isto, para usar o Docker siga os seguintes passos:

+ Abra o terminal **(PowerShell, se usar Windows)**
+ Navegue até o diretório do HXPHP. Ex: `cd ~/Desktop/hxphp`
+ Delete a pasta `data` *(se existir)*
+ Execute o seguinte comando:

> `docker-compose up -d`

----

### Serviços

Após a conclusão do processo, você poderá utilizar os softwares listados abaixo nos respectivos endereços:

+ **HXPHP**: [http://localhost:8000](http://localhost:8000)
+ **phpMyAdmin**: [http://localhost:8080](http://localhost:8080)

Os dados de acesso ao **phpMyAdmin** estão no arquivo `.env`. O servidor/host é `mysql`, por padrão.

----

### Observações

1. Este processo pode demorar alguns minutos na primeira execução, pois, o Docker irá baixar as imagens necessárias
2. Para parar os containers use:

> `docker-compose down -v`

3. Um diretório *(volume)* **data** será criado na raiz do projeto e será responsável por armazenar os dados do MySQL *(container mysql)*. Certifique-se que esta pasta não existe antes de rodar o `docker-compose up -d`.

---- 

## Configurando o framework {#configuracao}

Após enviar os arquivos, configure o framework. Todas as configurações devem ser definidas no arquivo:

`/app/config.php`

As configurações são divididas em dois grupos:

+ Configurações globais, e;
+ Configurações de ambiente.

----

### Configurações globais

As `globais` tem como objetivo definir parâmetros que, em suma, não necessitam de alteração, pois, consistem em intervenções no funcionamento padrão do framework. Exemplos: Diretório dos controllers, diretório das views e etc.

----

### Configurações de Ambiente

Já as `configurações de ambiente` são parâmetros que variam de acordo com o `ambiente atual`. O ambiente além de conter a sua configuração padrão também pode conter módulos.

----

#### Adicionando um ambiente

Para adicionar um ambiente: `$configs->env->add('ambiente');`. 
Os ambientes padrão são: `development` e `production`.

----

#### Criando um ambiente

Para criar um ambiente: 
+ Acesse a pasta `HXPHP\System\Configs\Environments`, e;
+ Crie um arquivo no padrão `EnvironmentNomeDoAmbiente`.

Conteúdo padrão de um ambiente:

```php
  namespace HXPHP\System\Configs\Environments;

  use HXPHP\System\Configs as Configs;

  class EnvironmentNomeDoAmbiente extends Configs\AbstractEnvironment
  {

  }
```


Por padrão, a única configuração padrão de ambiente é a `baseURI` que define a [BASE DA URL](#funcionamento-da-url).
Para alterá-la: `$configs->env->nomedoambiente->baseURI = '/hxphp/';`.

----

#### Módulos

Os módulos padrão de ambiente são: `Database` e `Mail`. Os módulos não são padronizados.

Para criar um módulo é necessário salvá-lo na pasta: `/src/HXPHP/System/Configs/Modules/` e <b>adicioná-lo na lista de módulos</b> do arquivo `/src/HXPHP/System/Configs/RegisterModules.php`.


Exemplo demonstrando o registro de um módulo qualquer chamado Youtube (RegisterModules.php):

```php
  namespace HXPHP\System\Configs;

  class RegisterModules
  {
    public $modules = [];

    public function __construct()
    {
      //Informe os nomes dos módulos em lowercase
      $this->modules = array(
        'database',
        'mail',
        'youtube'
      );

      return $this;
    }
  }
```

----

##### Exemplo de configuração:

```php

  //Constantes
  $configs = new HXPHP\System\Configs\Config;

  //Globais
  $configs->global->models->directory = APP_PATH . 'models' . DS;

  $configs->global->views->directory = APP_PATH . 'views' . DS;
  $configs->global->views->extension = '.phtml';

  $configs->global->controllers->directory = APP_PATH . 'controllers' . DS;
  $configs->global->controllers->notFound = 'Error404Controller';

  $configs->title = 'Titulo customizado';

  //Configurações de Ambiente - Desenvolvimento
  $configs->env->add('development');

  $configs->env->development->baseURI = '/hxphp/';

  $configs->env->development->database->setConnectionData(array(
    'driver' => 'mysql',
    'host' => 'localhost',
    'user' => 'root',
    'password' => '',
    'dbname' => 'hxphp',
    'charset' => 'utf8'
  ));

  $configs->env->development->mail->setFrom(array(
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
  ));

  //Configurações de Ambiente - Produção
  $configs->env->add('production');

  $configs->env->production->baseURI = '/';

  $configs->env->production->database->setConnectionData(array(
    'driver' => 'mysql',
    'host' => 'localhost',
    'user' => 'usuariodobanco',
    'password' => 'senhadobanco',
    'dbname' => 'hxphp',
    'charset' => 'utf8'
  ));

  $configs->env->production->mail->setFrom(array(
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
  ));


  return $configs;
```
