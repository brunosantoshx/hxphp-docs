---
currentMenu: modulos
---
## *Módulos* {#modulos}

Nesta seção você irá conhecer os módulos.
----
### O que são Módulos? {#o-que-sao-modulos}

Módulos são basicamente plugins, ou seja, são bibliotecas de códigos que tem por objetivo adicionar recursos para os componentes do framework.

----
### *Message Module* {#message-module}

O único módulo nativo do framework é o <b>Message Module</b>. Ele é responsável por gerenciar todas as mensagens de erro, aviso, sucesso e etc.

Para utilizar este módulo é necessário:

+ Criar um template de mensagens no formato JSON, e;
+ Salvar na pasta `templates/Modules/Messages/`.

Veja o exemplo do template `auth.json`:
```json
{
    "description" : "Template de mensagens para o serviço de autenticação",
    "messages" : {
        "usuario-bloqueado" : {
            "subject" : "Acesso bloqueado por excesso de tentativas",
            "message" : "Olá %s, <br /> Seu acesso foi bloqueado pelo excesso de tentativas, contate o administrador para liberação. Lembre-se de que esse recurso é para sua segurança, caso necessário solicite um novo acesso para evitar transtornos futuros."
        }
    },
    "alerts" : {
        "conta-em-uso" : {
            "style"   : "danger",
            "title"   : "Ops! Conta em uso!",
            "message" : "Esta conta já está registrada e sendo utilizada no momento!"
        },
        "tentativas-esgotando" : {
            "style"   : "warning",
            "title"   : "Ops! Verifique com cuidado os dados!",
            "message" : "Suas tentativas estão acabando, você só tem %s tentativa(s)! Após exceder esse número seu acesso só será liberado através da aprovação do administrador!"
        },
        "dados-incorretos" : {
            "style"   : "info",
            "title"   : "Ops! Dados incorretos!",
            "message" : "Não foi possível realizar a autenticação, confira seus dados!"
        },
        "usuario-bloqueado" : {
            "style"   : "danger",
            "title"   : "Ops! Usuário bloqueado!",
            "message" : "Não foi possível realizar a autenticação, pois este usuário encontra-se bloqueado no sistema, contate o administrador para liberação!"
        },
        "usuario-inexistente" : {
            "style"   : "danger",
            "title"   : "Ops! Dados incorretos!",
            "message" : "Não foi possível realizar a autenticação, confira seus dados!"
        }
    }
}
```

Perceba que existem três estruturas principais:

+ Description;
+ Messages, e;
+ Alerts.

A estrutura <b>description</b> deve conter a descrição do template.

Já a estrutura <b>messages</b> deve acomodar mensagens que serão enviadas por e-mail.

E, por fim, a estrutura <b>alerts</b> que deve acomodar as mensagens que serão renderizadas pelo [Alert Helper](#alert-helper).

As estruturas <b>messages</b> e <b>alerts</b> contém seções pré-definidas:

+ <b>subject</b> e <b>message</b> para a a estrutura <b>messages</b>, e;
+ <b>style</b>, <b>title</b> e <b>message</b> para a estrutura <b>alerts</b>.

Estas seções devem ser acomodadas dentro de uma estrura intermediária que é nomeada com um código referente ao seu conteúdo.

Após criar e preencher os templates com seus respectivos conteúdos é possível resgatar estes valores de forma prática através do <b>Messages Module</b>.

O primeiro passo é declarar para o método construtor do módulo qual é o nome do template que será utilizado.

Obs: <b>Lembre-se de não informar a extensão .json</b>.

O código resultante seria:
```php
$messages = new Messages('auth');
//ou
$this->load('Modules\Messages', 'auth');
```

Para retornar o conteúdo de uma mensagem|alerta é necessário utilizar o seguinte caminho:

```php
$this->messages->alerts->getByCode('conta-em-uso');
```

Executando o código acima tem-se como retorno um *array* no formato necessário para ser interpretado pelo [Alert Helper](#alert-helper).

Perceba que `{alerts}` é uma das opções disponíveis, isto é, trata-se da estrutura que você deseja utilizar. No template anteriormente apresentado, pode-se observar duas estruturas: *messages* e *alerts*.

```php
$this->messages->messages->getByCode('usuario-bloqueado');
```

Caso queira definir uma estrutura padrão e utilizar diretamente o método `getByCode($codigo)` configure o método `setBlock($estrutura);`.

```php
$this->messages->setBlock('messages');
$this->messages->getByCode('usuario-bloqueado');
```

Também é possível preencher valores coringas presentes na mensagem do template (%s), para tal, é necessário informar estes parâmetros em um *array* seguindo o padrão abaixo:

Template para exemplo:
```json
{
    "description" : "Template de mensagens para exemplo",
    "alerts" : {
        "codigo" : {
            "style"   : "info",
            "title"   : "%s",
            "message" : "Olá %s %s, como vai?"
        }
    }
}
```
O código resultante seria:
```php
$this->load('Modules\Messages', 'meutemplate');
$this->messages->setBlock('alerts');

$substituir_coringas = [
    'title' => 'Hello World',
    'message' => [
      'Bruno',
      'Santos'
    ]
];

$this->messages->getByCode('codigo', $substituir_coringas);
```