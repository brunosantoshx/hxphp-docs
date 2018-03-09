## Sumário

1. [Informações essenciais](https://github.com/brunosantoshx/hxphp-docs/blob/master/01-essential-information.md)
2. [Instalação e configuração](https://github.com/brunosantoshx/hxphp-docs/blob/master/02-installation-and-configuration.md)
3. [Pontapé inicial](https://github.com/brunosantoshx/hxphp-docs/blob/master/03-kickoff.md)
4. [Controllers](https://github.com/brunosantoshx/hxphp-docs/blob/master/04-controllers.md)
5. [Models](https://github.com/brunosantoshx/hxphp-docs/blob/master/05-models.md)
6. [Views](https://github.com/brunosantoshx/hxphp-docs/blob/master/06-views.md)
7. [Método Load](https://github.com/brunosantoshx/hxphp-docs/blob/master/07-load.md)
8. [Helpers](https://github.com/brunosantoshx/hxphp-docs/blob/master/08-helpers.md)
9. [HTTP](https://github.com/brunosantoshx/hxphp-docs/blob/master/09-http.md)
10. [Storage](https://github.com/brunosantoshx/hxphp-docs/blob/master/10-storage.md)
11. [Módulos](https://github.com/brunosantoshx/hxphp-docs/blob/master/11-modules.md)
12. [Serviços](https://github.com/brunosantoshx/hxphp-docs/blob/master/12-services.md)
13. [Utilitários](https://github.com/brunosantoshx/hxphp-docs/blob/master/13-tools.md)

----
## 7. Método *load()* {#metodo-load}

Esta seção aborda o carregamento de *Helpers*, Serviços, Módulos e outros componentes nos *Controllers*.

----

Para utilizar os *helpers* e demais componentes do framework, tanto na view como no controller, é necessário "injetar" o objeto no controller através do método `load()`. Este método contém apenas um argumento obrigatório que é a <b>localização do objeto</b>. É necessário informar a *namespace* correspondente e o nome do objeto. A *namespace*, em suma, é o nome da pasta *(considerando a pasta System como raiz)* em que o objeto se encontra.


Os demais argumentos informados no método `load('Helpers\NomeDoHelper', $param1, $param2, $paramN)` serão utilizados como argumentos no método construtor do objeto que será carregado.


*O código resultante seria:*
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function indexAction()
    {
        $this->load('Helpers\NomeDoHelper', 'param1', 'param2');
        $this->load('Modules\NomeDoModulo', 'param1', 'param2');
        $this->load('Storage\Session');
        $this->load('Services\Email');

        $this->nomedohelper->metodo();
        echo $this->nomedomodulo->propriedade;
        ...
    }
}
```

Após injetar o objeto, no controller, cria-se um atributo com o seu nome no padrão *lowercase*.