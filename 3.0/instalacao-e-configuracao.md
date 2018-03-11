---
currentMenu: instalacao-e-configuracao
---
## Instalação e Configuração {#instalacao-e-configuracao}

Esta seção contempla os procedimentos de instalação e configuração do framework.

----
### Bootstrapping {#bootstrapping}

Após vistar todos os itens da lista de requisitos, prossiga com a instalação, que tanto em rede local como remota, consiste em:

+ Enviar os arquivos para a pasta desejada;
+ Editar o arquivo `app/config.php`, e;
+ Editar o arquivo `.htaccess`.

----

#### Configurando o framework

Após enviar os arquivos, configure o framework. Todas as configurações devem ser definidas no arquivo:

`/app/config.php`

As configurações são divididas em dois grupos:

+ Configurações globais, e;
+ Configurações de ambiente.

As `globais` tem como objetivo definir parâmetros que, em suma, não necessitam de alteração, pois, consistem em intervenções no funcionamento padrão do framework. Exemplos: Diretório dos controllers, diretório das views e etc.

Já as `configurações de ambiente` são parâmetros que variam de acordo com o `ambiente atual`. O ambiente além de conter a sua configuração padrão também pode conter módulos.

Para iniciar a configuração é necessário instanciar o objeto `$configs = new HXPHP\System\Configs\Config`. E, após isto, é possível:

+ Alterar as configurações globais, e/ou;
+ Adicionar e configurar ambientes.

Para adicionar um ambiente: `$configs->env->add('ambiente');`. 
Os ambientes padrão são: `development` e `production`.

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

----
#### Configurando o .htaccess

O arquivo `.htaccess` é responsável pelo processo de reescrita das URLs, isto é, trata-se de um componente fundamental para a aplicação.

Portanto, edite o arquivo `.htaccess`, localizado na pasta raiz do framework, para configurar a BASE. O bloco que deve ser configurado é listado abaixo:


```php
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /hxphp/
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /hxphp/index.php [L]
  </IfModule>
```

Atente-se para os comandos: `RewriteBase` e `RewriteRule`. Veja que ambos contém a menção do valor da `baseURI`, portanto, caso você tenha definido um valor customizado, altere-o. Adotando nosso exemplo anterior, tendo a `baseURI` igual a *administrativo*, tem-se o seguinte código:


```php
  <IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteBase /administrativo/
    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /administrativo/index.php [L]
  </IfModule>
```


Para testar o sucesso da configuração acesse: 
`http://localhost/{baseURI}/`

----

### Instalando as dependências do framework {#instalando-as-dependencias}

O HXPHP Framework utiliza o [Composer](https://getcomposer.org/download) para gerenciar as dependências, portanto, antes de prosseguir, certifique-se que o mesmo está instalado em sua máquina. Para isso, abra o terminal (CMD no Windows) e digite o seguinte comando: `composer --version`.

Após constatar que o [Composer](https://getcomposer.org/download) está em pleno funcionamento, navegue até a pasta do framework e execute o comando: `composer install`.

----

<b>Para lhe auxiliar neste processo, confira essa série completa de videoaulas gratuitas sobre instalação e uso do Composer: [Série completa do canal HXTUTORS](https://goo.gl/9oQNr5)</b>
