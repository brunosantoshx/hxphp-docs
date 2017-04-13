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

## 9. *HTTP* {#http}

Nesta seção você irá conhecer o processo de obtenção de dados via requisições HTTP.

----

### HTTP Request {#request}

Para a obtenção de dados de requisições como <b>POST</b> e <b>GET</b> é necessário utilizar o objeto *Request*.

As vantagens são muitas e dentre as principais destaca-se a maior segurança, visto que os dados são tratados nativamente.

O objeto *Request* contém os seguintes métodos:

<b>Customização</b>
+ `setCustomFilters()`;

<b>Obtenção de dados</b>
+ `cookie()`;
+ `get()`;
+ `post()`;
+ `server()`;
+ `getMethod()`;

<b>Validação</b>
+ `isValid()`;
+ `isPost()`;
+ `isGet()`;
+ `isPut()`, e;
+ `isHead()`.

O método `setCustomFilters()` tem o objetivo de customizar os filtros de tratamento aplicados aos dados das requisições. Para tal, é necessário informar um <b>*array* associativo</b> com o índice igual ao nome do campo e o valor com a constante de filtro. Leia: [http://php.net/manual/en/filter.filters.sanitize.php](http://php.net/manual/en/filter.filters.sanitize.php). Por padrão, o filtro aplicado em todos os dados é o `FILTER_SANITIZE_STRING` que evita a injeção de códigos HTML.


O código resultante seria:
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function salvarAction()
    {
        $this->request->setCustomFilters([
            'namedoinput' => FILTER_SANITIZE_NUMBER_FLOAT
        ]);
    }
}
```

### Trabalhando com checkboxes, multiple selects e semelhantes

Como o filtro padrão trata os dados para `STRING`, isto afeta a obtenção de dados de campos que enviam múltiplas informações. A solução é bem simples:

```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function salvarAction()
    {
        $this->request->setCustomFilters([
            'id' => [
                'filter' => FILTER_SANITIZE_NUMBER_INT,
                'flags' => FILTER_FORCE_ARRAY
            ]
        ]);
    }
}
```

No exemplo acima, utilizamos a *flag* `FILTER_FORCE_ARRAY` que irá sempre retornar um array no campo definido e em conjunto com um filtro para tratar os valores para números inteiros.

Já os métodos `get()` e `post()` retornam os dados filtrados, sendo que é possível retornar todo o conteúdo ou apenas um dado específico.

O código resultante seria:
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function salvarAction()
    {
        $this->request->setCustomFilters([
            'nomedocampo' => FILTER_SANITIZE_NUMBER_FLOAT
        ]);

        print_r($this->request->post()); //Array com todos os dados enviados via POST
        echo $this->request->post('nomedocampo'); //Apenas o conteúdo do campo valor

        echo $this->request->server('SERVER_NAME');
    }
}
```

E, por fim, têm-se os métodos: `getMethod()`, `isPost()`, `isGet()`, `isPut()` e `isHead()` que tem como função determinar qual é o método de requisição.