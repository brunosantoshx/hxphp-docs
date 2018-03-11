---
currentMenu: load
---
## Método *load()* {#metodo-load}

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