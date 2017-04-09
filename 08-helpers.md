----
## *Helpers* {#helpers}

Esta seção aborda os *Helpers* do HXPHP.

----

### O que são *Helpers*? {#o-que-sao-helpers}

Os *Helpers* são componentes auxiliares. Em suma, são objetos que desempenham funções nas `views`.

----

### Alert Helper {#alert-helper}

O <b>Alert Helper</b> é responsável pela exibição de mensagens de aviso, erro, sucesso e afins.

Para utilizar este *helper* é necessário passar um *array* como parâmetro no construtor e este deve conter a seguinte estrutura:

```php
[
    'danger', // Classes Bootstrap disponíveis: danger, warning, info, success
    'Título da mensagem',
    'Conteúdo da mensagem' // Pode ser um array com várias mensagens
];
```

O código resultante seria:
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function indexAction()
    {

        $alerta = [
          'success',
          'Uhuul! Produto cadastrado com sucesso!',
          'O produto já está disponível para seus clientes.'
        ];

        $this->load('Helpers\Alert', $alerta);

        var_dump($this->alert->getAlert()); // ou
        echo $this->alert;
    }
}
```

O método `getAlert()`, responsável pela renderização do alerta, é inserido na *view* e geralmente é encontrado em cabeçalhos para evitar a repetição de códigos.

É válido ressaltar que o alerta renderizado é gerado a partir de um template localizado na pasta: `templates/Helpers/Alert/`.

----
### Menu Helper {#menu-helper}

O <b>Menu Helper</b> tem a função de renderizar um menu customizado mediante o nível de acesso do usuário.

Diferentemente do *Alert Helper*, que trabalha com templates, este conta com um módulo específico de configuração. Portanto, antes de utilizá-lo, é necessário verificar se o módulo está registrado.

#### Recursos do módulo de configuração

O módulo contém dois métodos:
+ `setMenus()`: Que é responsável por definir a estrutura do menu e, opcionalmente, o nível de acesso;
+ `setConfigs()`: Que é responsável por customizar o menu que será renderizado.

##### Definindo a estrutura dos menus

Exemplo de configuração para definir dois menus (o primeiro para usuários com nível de acesso igual a <b>administrator</b> e o segundo que é neutro):

```php
    $configs->env->development->menu->setMenus([
        'Home/home' => '%siteURL%',
        'Projetos/briefcase' => '%baseURI%/projetos/listar/',
        'Clientes/users' => [
          'Listar todos/users' => '%baseURI%',
          'Tipos de clientes/users' => '%baseURI%/clientes/tipos'
        ],
        'Usuários/users' => '%baseURI%/usuarios/listar/'
    ], 'administrator');


    $configs->env->development->menu->setMenus([
        'Home/home' => '%siteURL%/home',
        'Projetos/briefcase' => '%baseURI%/projetos/listar/'
    ]);
```

Observações:
+ O método suporta 2 argumentos: *(array)* `$menus` e *(string)* `$access_level`;
+ O segundo argumento é opcional, isto é, também é possível definir um menu sem um nível de acesso para páginas públicas e afins;
+ O *array* com os menus é padronizado da seguinte forma:
```php
[
    'Menu Sem Dropdown/codigo-icone-font-awesome-sem-prefixo' => 'http://www.link-absoluto.com',
    'Usuários/users' => '%baseURI%/link-relativo',
    'Menu Com Dropdown/home' => [
      'Submenu/briefcase' => '%baseURI%/link-relativo',
      'Submenu #2/users' => '%baseURI%/link-relativo'
    ]
]
```
+ É possível utilizar os coringas `%baseURI%` e `%siteURL%` que são substituídos pelo valor da configuração `baseURI` e pelo endereço do site, respectivamente;
+ A ferramenta [Font Awesome](http://fontawesome.io/) foi utilizada nos ícones que acompanham os títulos nos menus e submenus, e;
+ O prefixo `fa` utilizado para a exibição dos ícones é inserido nativamente.

##### Customizando o menu

Exemplo com todas as possíveis configurações do menu:
```php
$configs->env->development->menu->setConfigs([
    'container' => false, // Tag container (opcional). Ex: nav
    'container_id' => '',
    'container_class' => '',
    'menu_id' => 'menu',
    'menu_class' => 'menu',
    'menu_item_class' => 'menu-item',
    'menu_item_active_class' => 'active',
    'menu_item_dropdown_class' => 'dropdown',
    'link_before' => '<span>', // Conteúdo anterior ao título
    'link_after' => '</span>', // Conteúdo posterior ao título
    'link_class' => 'menu-link',
    'link_active_class' => 'menu-active-link',
    'link_dropdown_class' => 'dropdown-toggle',
    'link_dropdown_attrs' => [
      'data-toggle' => 'dropdown' // Data Atributos para ativação do dropdown
   ,
    'dropdown_class' => 'dropdown-menu',
    'dropdown_item_class' => 'dropdown-item',
    'dropdown_item_active_class' => 'active'
]);
```

O exemplo acima está com todos os valores padrão, portanto, caso necessite customizar o seu menu, insira <b>apenas as configurações que deseja alterar</b> o valor padrão.

#### Carregando o Menu Helper no Controller

Este *helper* requer duas dependências no construtor: `Request` e `Configs`. E se o menu for específico para um nível de acesso também é necessário informar o respectivo valor.


O código resultante seria:
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function indexAction()
    {
        $this->load(
            'Helpers\Menu',
            $this->request,
            $this->configs,
            $this->auth->getUserRole() //access_level
        );
    }
}
```

#### Renderizando o menu

Após configurar e carregar o *helper* na *action* desejada, será possível renderizar o menu na *view*. Para tal, utilize o método `getMenu()`.

Exemplos:
```php
echo $this->menu->getMenu();
```