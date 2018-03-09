---
currentMenu: storage
---
## *Storage* {#storage}

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