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

## 10. *Storage* {#storage}

### Sessões

Para trabalhar com sessões é necessário utilizar os recursos de *Storage*. Para tal, utiliza-se o objeto *Session* que contém os seguintes métodos:

```php
class ProdutosController
{
    public function listarAction()
    {
        $this->load('Storage\Session');

        $this->session->set('name', 'value'); //Cria uma sessão
        echo $this->session->get('name'); //Seleciona uma sessão
        $this->session->clear('name'); //Exclui uma sessão
        print_r($this->session->exists('name')); //Verifica a existência de uma sessão
    }
}
```

### Cookies

Para trabalhar com *cookies* utiliza-se o objeto *Cookie* que contém os seguintes métodos:

+ `set()`;
+ `exists()`;
+ `get()`, e;
+ `clear()`.

```php
class ProdutosController
{
    public function listarAction()
    {
        $this->load('Storage\Cookie');

        $this->cookie->set('name', 'value', '31556926'); //Cria um cookie ($name, $value, $time)
        echo $this->cookie->get('name'); //Seleciona um cookie
        $this->cookie->clear('name'); // Exclui um cookie
        print_r($this->cookie->exists('name')); //Verifica a existência do cookie
    }
}
```