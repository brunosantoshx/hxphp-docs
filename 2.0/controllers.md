---
currentMenu: controllers
---
## Controllers {#controllers}
Esta seção aborda a primeira das três camadas do padrão <b>MVC</b>.

----
### O que são Controllers? {#o-que-sao-controllers}

No padrão <b>MVC</b> existem basicamente três camadas: *Model*, *View* e *Controller*.

O *controller* é o intermediário entre as outras duas camadas, ou seja, ele controla (daí o nome) todo o fluxo de informação do site/sistema.

Em outras palavras, o *controller*, comumente chamado de <b>controlador</b>, tem como objetivo definir as propriedades da visualização (*view*), isto é, ele tem a função de condicionar, executar as regras de negócio(*models*), interpretar parâmetros, dentre muitas outras.

Resumindo, nesta camada é decidido o “se”, “o que”, “quando” e “onde” deve funcionar na nossa aplicação.

----
### Criando Controllers {#criando-controllers}

Agora que você já sabe o que é um *controller* e a sua importância para o funcionamento da aplicação, você poderá finalmente criar o seu primeiro *controller*. Para tal, siga estes passos:

+ Defina o link desejado (Ex: http://site.com.br/<code>produtos</code>/ ou http://site.com.br/<code>lista-de-produtos</code>/);
+ Crie um arquivo nomeado no padrão *CamelCase*; com o link desejado, sem espaços, hífens ou *underscores*; com o sufixo 'Controller', e; a extensão '.php'. Ou seja, para os exemplos acima, os controllers seriam: `ProdutosController.php` e `ListaDeProdutosController.php`;
+ Salve este arquivo na pasta: `app/controllers/`

Após salvar o arquivo, comece com o desenvolvimento de seu código e atente-se para o exemplo abaixo que mostra o código padrão para *controllers* utilizando os exemplos acima:


*app/controllers/ProdutosController.php*
```php
  class ProdutosController extends \HXPHP\System\Controller
  {

  }
```


Fique atento às seguintes características do código listado acima:

+ O nome da classe é igual ao nome do arquivo;
+ Cada *controller* é uma extensão da classe mestre <code><em>\HXPHP\System\Controller</em></code>, e;
+ A `indexAction()`, *action* padrão do controller, é executada automaticamente.

Mas, afinal de contas o que são *actions*?
*Actions* são os métodos públicos de um *controller*. Cada *action* é responsável por um link específico. Veremos isto com mais detalhes na próxima seção.

----
### Criando Actions {#criando-actions}

Após criar o *controller* é provável que seja necessário a criação de *actions* específicas e, para tal, siga estes passos:

+ Defina o link desejado, por exemplo: http://site.com.br/produtos/<code>listar</code>/ , e;
+ No *controller* desejado, crie um método `público` nomeado com o link desejado, sem espaços, hífens ou *underscores*; com o sufixo 'Action'.

O código resultante do exemplo acima seria:
```php
  public function listarAction()
  {
    ...
  }
```

Com isto você poderá concluir que a nomenclatura dos *controllers* e *actions* está diretamente ligada às URL's escolhidas. E isto é um fator muito importante para técnicas de otimização para motores de pesquisa (SEO), dentre outras vantagens.

----
### Controller NotFound {#controller-not-found}

O erro 404, comumente chamado de *Error Not Found*, é um dos erros mais conhecidos e portanto dispensa definições. No HXPHP esse erro é manipulado por um *controller* que é definido nas [configurações globais da aplicação](#bootstrapping). Por padrão, o valor desta configuração é: `Error404Controller`, ou seja, o arquivo *Error404Controller.php* é requisitado toda vez que ocorre o erro 404 durante a execução da aplicação.

----
### Utilizando parâmetros informados na URL {#utilizando-parametros}

Como já mencionado na seção de [funcionamento da URL](#funcionamento-da-url), os parâmetros que serão interpretados no controlador (*controller*) devem constar na URL, ou seja, é necessário que o *controller* e a *action* sejam definidos.

Com estas condições atendidas tem-se a seguinte estrutura:
```
  http://site.com.br/controller/action/parametro1/parametro2/parametro3/
```

Para resgatar estes valores, atente-se ao fato de que o parâmetro só pode ser interpretado pela *action* requisitada, ou seja, no exemplo: `http://site.com.br/produtos/listar/1/`, é definido que a *action* `listar` do *controller* `Produtos` irá receber como `argumentos` os `parâmetros` informados.

O código resultante do exemplo acima seria:
```php
  class ProdutosController extends \HXPHP\System\Controller
  {
    public function listarAction( $pagina = 1 )
    {
      echo $pagina;
    }
  }
```


Algumas observações importantes:

+ Os parâmetros devem ser declarados como argumentos do método (*action*);
+ Estes argumentos podem ser nomeados livremente, porém devem ter <b>obrigatoriamente um valor pré-definido</b>, por exemplo: `listarAction($categoria = '', $pagina = 1)`, e;
+ Os argumentos devem ser declarados na mesma ordem que os parâmetros.

Através deste mecanismo é possível ter <code><b>N</b> parâmetros</code> e nomeá-los de forma sugestiva, o que possibilitará uma melhor experiência com seus códigos.

----

### Utilizando o método construtor {#metodo-construtor}

O [método construtor](http://php.net/manual/pt_BR/language.oop5.decon.php#language.oop5.decon.constructor) de uma classe é um [método mágico](http://php.net/manual/pt_BR/language.oop5.magic.php) e basicamente tem como função ser o primeiro método de um objeto a ser executado, ou seja, é executado automaticamente quando uma instância é criada.

As vantagens são muitas, porém a principal é que o conteúdo do método construtor é dominante sobre todos os demais.

Imagine um processo de validação de autenticação de usuários. Este processo deve prevalecer em todas as *actions* de um *controller* restrito. Com o uso do método construtor o código não precisa ser repetido.

A única regra para o uso do método construtor nos *controllers* é que seja declarado o método construtor da classe mãe, pois, caso contrário, um irá sobrescrever o outro e isto acarretará na desconfiguração da aplicação.

Portanto, quando criar métodos construtores siga este padrão:
```php
  class ProdutosController extends \HXPHP\System\Controller
  {

    public function __construct()
    {
      parent::__construct();
      ...
    }

  }
```

Caso queira utilizar os valores das configurações e até mesmo recursos como o *Request*, que utiliza as configurações como dependência, é necessário resgatar as configurações como argumento no construtor.

```php
  class ProdutosController extends \HXPHP\System\Controller
  {

    public function __construct($configs)
    {
      parent::__construct($configs);
      echo $configs->baseURI;
      echo $configs->meumodulo->propriedade;
      ...
    }

  }
```

----
### Redirecionamento {#redirecionamento}

É muito comum que seja necessário o processo de redirecionamento nos *controllers*.

Imagine o seguinte processo:
<code>produtos/cadastrar/ <b>-></b> produtos/salvar/ <b>-></b> produtos/listar/</code>

Após o formulário ser processado e disparado para a *action* <code><b>salvar</b></code> será necessário um redirecionamento para a *action* <code><b>listar</b></code> e para fazermos isto utilizamos o seguinte código:


```php
  public function salvarAction()
  {
    if ($processo->status === true) {

      /**
       * @param string $url Especifique o link para qual a aplicação será redirecionada
       */
      $this->redirectTo(URL);

    }
  }
```
