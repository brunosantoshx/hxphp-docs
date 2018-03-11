---
currentMenu: helpers
---
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

----
### Table Helper {#table-helper}

O Table Helper é responsável por criar, montar e exibir tabelas utilizando apenas o PHP.

Vamos às demonstrações de uso!

Para carregar nosso Helper, basta chamá-lo no Controller:

```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function indexAction()
    {
        $this->load('Helpers\Table');
```

#### Criando linhas para a tabela

Há basicamente **4** funções que nos permitem criar registros na tabela.
Vamos por partes.

Para criarmos um cabeçalho para a tabela, usamos a seguinte função:

```php
$this->table->addHeader(['ID', 'Nome', 'Idade']);
```
Resultado:
```html
<thead>
    <th>ID</th><th>Nome</th><th>Idade</th>
</thead>
```
Basicamente passamos um _array_ como parâmetro da nossa função, onde cada índice do _array_ representa um dos títulos do cabeçalho.

Seguindo o mesmo padrão podemos utilizar as funções abaixo para criação de outros membros da tabela:

```php
//Adicionar uma linha (dentro do tbody) à nossa tabela. 
//Repare que cada índice do array passado, representa uma célula da linha.
$this->table->addRow(['1', 'Caio', '21']);
```

```php
//É possível adicionar diversas linhas ao mesmo tempo.
$linhas = array(
    ['1', 'Caio', '21'],
    ['2', 'Bruno', '22']
);

//Desta vez utilizaremos a função no plural
$this->table->addRows($linhas);
```
Resultado:
```html
<tr>
    <td>1</td><td>Caio</td><td>21</td>
</tr>
<tr>
    <td>2</td><td>Bruno</td><td>22</td>
</tr>
```
Repare que criamos uma "_matriz_", onde cada linha dessa _matriz_ é um _array_ contendo as células de sua própria linha na tabela.

Podemos, da mesma forma, adicionar um rodapé à nossa tabela:

```php
$this->table->addFooter(['34', 'João', '52']);
```

Neste caso, as linhas ficarão dentro da tag `<tfoot>` na tabela.

E por fim, podemos adicionar um _Caption_ à nossa tabela.
```php
$this->table->addCaption('Título da Tabela');
```

#### Atributos e Customização

Muitas vezes é necessário adicionar atributos às diversas _tags_ da tabela.

Por exemplo, você pode querer dizer que alguma das linhas da tabela precisa ter alguma _class_ ou um _id_ específico.
É muito simples resolver este problema.

Todas as funções citadas anteriormente aceitam um segundo parâmetro, que serão os nossos **atributos**.
Passaremos este parâmetro em forma de _array_.

```php
$this->table->addRow(['Celula1', 'Celula2'], ['class' => 'primeira-linha']);
//<tr class="primeira-linha"><td>Celula1</td><td>Celula2</td></tr>

$this->table->addHeader(['Titulo1', 'Titulo2'],['id' => 'cabecalho']);

$this->table->addFooter(['Rodape1', 'Rodape2'], array('class' => 'tabela-rodape'));

$this->table->addCaption('Titulo da Tabela', ['style'=>'color: red;']);
```

Simples não?

Ainda assim, muitas das vezes é necessário que os atributos estejam em tags mais específicas ainda, como `<td>` ou `<th>.`

Para adicionar atributos à células específicas, basta transformar a célula em um array, e adicionar os atributos.
Exemplo:

```php
$celulas = array('01', 'Caio', '21 anos', ['programador', 'class'=>'profissao']);
$this->table->addRow($celulas);
```
Resultado:
```html
<tr>
    <td>01</td><td>Caio</td><td>anos</td><td class="profissao">programador</td>
</tr>
```
Observe que, o primeiro elemento do array da célula, é o conteúdo da própria célula (programador), e os demais elementos, são os atributos, que devem obedecer à seguinte forma:
```php
'NomeDoAtributo'=>'Valor';
```

Este recurso funciona tanto para as linhas do corpo, quanto para as linhas do Header e do Footer.
Deste modo, qualquer célula ou linha da tabela, pode possuir atributos específicos.
Exemplo:
```php
$this->table->addHeader(['ID', 'Nome', 'Idade', array('Profissao', 'colspan'=>'2')]);
```


Contudo, há ainda outras tags que podem ser customizadas. São elas:
```html
<table>, <thead>, <tbody>, <tfooter>
```

Para adicionar atributos à estas _tags_, podemos usar a seguinte função:
```php
$this->table->addTagAttr('table', ['class'=>'table table-bordered']);
```
Onde o primeiro parâmetro é nome de uma dessas 4 tags (table, thead, tbody ou tfoot), e o segundo parâmetro são os atributos, utilizando o mesmo padrão apresentado anteriormente.

#### Criando templates para a tabela

O template de tabela trata-se de um arquivo JSON. É basicamente um arquivo de configuração das _tags_ da tabela.
Os templates devem ser colocados na pasta `templates/Helpers/Table`.

Segue abaixo todas as configurações permitidas:
```json
{
    "tag_table": "<table %s>%s</table>",
    "tag_caption": "<caption %s>%s</caption>",
    "tag_thead": "<thead %s>%s</thead>",
    "tag_thead_row": "<tr %s>%s</tr>",
    "tag_thead_cell": "<th %s>%s</th>",
    "tag_tbody": "<tbody %s>%s</tbody>",
    "tag_tbody_row": "<tr %s>%s</tr>",
    "tag_tbody_cell": "<td %s>%s</td>",
    "tag_tfoot": "<tfoot %s>%s</tfoot>",
    "tag_tfoot_row": "<tr %s>%s</tr>",
    "tag_tfoot_cell": "<td %s>%s</td>"
}
```

Não é necessário inserir todas as _tags_. **Insira apenas as necessárias!**
Exemplo:
```json
{
    "tag_table": "<table %s class='tabela'>%s</table>",
    "tag_tbody_cell": "<td %s>Valor: %s</td>"
}
```
**Observação:** É obrigatório sempre colocar os dois coringas (_%s_), um para os atributos da tag, e outro para o conteúdo em si.

#### Definindo template

Para escolher um template de tabela, basta passá-lo como segundo argumento da função **load**.
Não é necessário passar o caminho e a extensão do arquivo. Apenas o nome.
Exemplo:
```php
$this->load('Helpers\Table', 'template-da-tabela');
```

#### Exemplo de template

Há diversas utilidades de se usar um template customizado.
Vamos supor que você necessite que todas as linhas da tabela comece com um _checkbox_.
Para resolver esta situação, criamos um template com o nome '**tabela-com-inputs**' da seguinte maneira:
```json
{
    "tag_tbody_row": "<tr %s> <td><input type='checkbox'></td> %s</tr>"
}
```
Chamando no Controller:
```php
$this->load('Helpers\Table', 'tabela-com-inputs');
```

**Observação:** Não delete o template default.json.

#### Exibindo a Tabela

Há duas maneira de renderizar a tabela:

No controller:
```php
$this->table->getTable();
```

Ou através da View:
```php
echo $this->table;
```

#### Exemplo Geral

Vamos criar uma tabela utilizando o Table Helper.

```php
$this->load('Helpers\Table');

$this->table->addCaption('Funcionários');
$this->table->addHeader(['ID', 'Nome', 'Profissao'], ['class'=>'cabecalho']);
$this->table->addRows(array(
    ['1', 'Caio', 'Programador'],
    ['2', 'Bruno', 'Programador'],
    ['3', 'João', ['Diretor', 'class'=>'success']]
));
//Integração com o Bootstrap
$this->table->addTagAttr('table', ['class'=>'table table-bordered']);

echo $this->table->getTable();

```

#### Table Helper e o PHP ActiveRecord

É muito fácil utilizar o Table Helper para criar tabelas através das consultas do PHP ActiveRecord. 
Exemplo:

```php
$rows = Book::all();

foreach ($rows as $book) {
    $book = $book->to_array();
    $this->table->addRow($book);
}
```

#### Observações
Para fins de _debug_ ou maior liberdade na criação de tabelas é possível "capturar" partes da tabela, sem ter que renderizá-la por completo.
Podemos fazer uso das seguintes funções:

```php
//Retorna o array somente com as linhas da tabela renderizadas
$this->table->getRows();

//Retorna o array somente com as linhas do cabeçalho renderizadas
$this->table->getHeader();

//Retorna o array somente com as linhas do rodapé renderizadas
$this->table->getFooter();

//Retorna um array com o conteúdo do título e seus atributos renderizados
$this->table->getCaption();
```
