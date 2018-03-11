---
currentMenu: pontape-inicial
---
## Pontapé inicial {#pontape-inicial}
Esta seção aborda o funcionamento do framework e suas características.

----
### Estrutura do framework {#estrutura-do-framework}
O HXPHP tem a seguinte estrutura de pastas:

```php
  /hxphp/
      app/
        controllers/
        models/
        views/
      public/
        css/
        img/
      src/
        HXPHP/
          System/
            Configs/
              Environments/
              Modules/
            Helpers/
            Http/
            Modules/
            Services/
            Storage/
      templates/
        Helpers
          Alert
        Modules
          Messages
```

A relação das funções de cada pasta pode ser conferida na lista abaixo:
  
+ A pasta `app` é o local dedicado para o desenvolvimento da sua aplicação;
+ A pasta `public` é o local apropriado para armazenar folhas de estilos, scripts e afins;
+ A pasta `src/HXPHP/System/` além de conter os arquivos de controle do framework, também acomoda os Helpers, Módulos e Serviços, e;
+ A pasta `templates` é o local padrão para armazenar os templates utilizados pelos componentes do framework.
  
----
### Funcionamento da URL {#funcionamento-da-url}

O HXPHP é baseado no padrão de arquitetura de software [MVC](http://pt.wikipedia.org/wiki/MVC) e, por isso, toda a aplicação é controlada pela URL.

Para que você compreenda completamente o funcionamento, tomemos como base dois exemplos:
  
+ a) HXPHP rodando na raiz, e;
+ b) HXPHP rodando em uma subpasta.

Sendo que os endereços de ambos serão, respectivamente:

+ a) `http://site.com.br/`
+ b) `http://localhost/hxphp/`
  
Agora com esses exemplos definidos é importante que você lembre-se da `baseURI`, vista no começo desta documentação. O valor desta configuração de ambiente para os dois exemplos seria, respectivamente:

+ a) `/`
+ b) `/hxphp/`
  
Mas afinal de contas para que serve essa configuração? Pois bem, ela é a fronteira entre o `hostname` e a `requisição`. Ou seja, o que vier depois dela são os parâmetros controladores de nossa aplicação.

Na prática, imagine os seguintes links requisitados por um usuário qualquer em ambos os exemplos:
  
+ <a>http://site.com.br<code>/</code><b>projetos/listar/1</b></a>
+ <a>http://localhost<code>/hxphp/</code><b>projetos/listar/1</b></a>
  
Com as situações ilustradas acima é possível compreender claramente o processo de separação, visto que, em ambas as situações a requisição foi: `projetos/listar/1`.

Mas, isso não é tudo! Essa requisição é subdividida em:
  
+ `controller` -> <b>projetos</b>
+ `action` -> <b>listar</b>
+ `parâmetro(s)` -> <b>1</b>

#### Subpastas

Também é possível organizar os *controllers* em subpastas. Esta funcionalidade permite uma fácil integração entre back e front-end, por exemplo. Confira o exemplo:

+ <a>http://localhost<code>/hxphp/</code><b>admin/projetos/listar/1</b></a>
+ <a>http://localhost<code>/hxphp/</code><b>/projetos/listar/1</b></a>

Essa requisição é subdividida em:

```
  http://localhost/hxphp/admin/projetos/novo/
```
  
+ `subpasta` -> <b>admin</b>
+ `controller` -> <b>projetos</b>
+ `action` -> <b>novo</b>
+ `parâmetro(s)` -> null

```
  http://localhost/hxphp/projetos/listar/1
```
  
+ `subpasta` -> null
+ `controller` -> <b>projetos</b>
+ `action` -> <b>listar</b>
+ `parâmetro(s)` -> <b>1</b>


<b>Para utilizar esta funcionalidade é necessária a existência da respectiva subpasta no diretório dos controllers. Se a subpasta não existir, o valor é automaticamente interpretado como Controller.</b>

Veja abaixo a estrutura necessária para reproduzir o exemplo acima:

```
  app/
    controllers/
      admin/
        ProjetosController.php
      ProjetosController.php
    views/
      admin/
        projetos/
          novo.phtml
      projetos/
        listar.phtml
```

<b>Obs: A configuração de subpasta também se aplica às views!</b>

O *Controller* e a *Action* tem posição fixa, mas os parâmetros são infinitos, ou seja, desde que você especifique os dois primeiros valores da requisição, poderá passar quantos parâmetros desejar!

Exemplo:
`http://site.com.br/controller/action/parametro1/parametro2/parametro3/parametroN`

É válido ressaltar que existe um *Controller* e uma *Action* padrão (IndexController e indexAction, respectivamente), que entra em funcionamento quando a requisição não é específica, por exemplo:

+ http://localhost/hxphp - `Nenhum controller solicitado`, e;
+ http://site.com.br/login/ - `Nenhuma action solicitada`.
    
  
