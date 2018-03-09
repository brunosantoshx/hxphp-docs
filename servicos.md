---
currentMenu: servicos
---
## Serviços {#servicos}

Nesta seção você irá conhecer os serviços nativos do HXPHP Framework.

----
### O que são Serviços? {#o-que-sao-servicos}

Serviços são objetos que executam uma ação específica, ou seja, o serviço de autenticação tem como objetivo autenticar o usuário e assim por diante. Os serviços do HXPHP Framework são armazenados na pasta `src/HXPHP/System/Services/`.

----
### Serviço de Autenticação {#servicos-lista}

Este serviço contém os seguintes métodos:

+ `login($user_id, $username, $user_role)` - Autentica o usuário;
+ `logout()` - Exclui sessões de autenticação;
+ `redirectCheck($public_page)` - Redireciona o usuário mediante condições;
+ `roleCheck(array $roles)` - Verifica o nível de acesso e redireciona caso seja necessário;
+ `login_check()` - Verifica se o usuário está autenticado;
+ `getUserRole()` - Retorna o nível de acesso do usuário autenticado, e;
+ `getUserId()` - Retorna o Id do usuário autenticado.

Este serviço suporta 4 parâmetros de configuração no método construtor:

+ URL de redirecionamento após o login bem sucedido;
+ URL de redirecionamento após o logout;
+ *(Opcional)* Booleano que determina se o redirecionamento será automático no método `login(...);` e `logout()`, e;
+ *(Opcional)* Subpasta que permite diferenciar autenticações para múltiplas aplicações.

Para facilitar a configuração deste serviço existe um módulo de configuração. Portanto, ao invés de setar os valores repetidamente em todos os carregamentos do serviço, pode-se centralizar no arquivo de configuração `app/config.php`.

Configuração na prática:
```php
$configs->env->development->auth->setURLs(
    '/hxphp/home/',
    '/hxphp/login/'
);
$configs->env->development->auth->setURLs(
    '/hxphp/admin/home/',
    '/hxphp/admin/login/',
    'admin'
);
```

O módulo de configuração suporta 3 parâmetros:

+ URL after login;
+ URL after logout, e;
+ *(Opcional)* Subpasta.

Após definir todas as configurações, carregue o serviço no controller:
```php
class LoginController extends \HXPHP\System\Controller
{
    public function __construct($configs)
    {
        parent::__construct($configs);

        $this->load('Services\Auth',
            $this->configs->auth->after_login,
            $this->configs->auth->after_logout,
            true,
            $this->request->subfolder
        );

        // Páginas públicas
        $this->auth->redirectCheck(true);

        // Páginas privadas
        //$this->auth->redirectCheck();
    }

    public function logarAction()
    {
        $this->auth->login(1, 'brunosantos', 'user');
    }

    public function sairAction()
    {
        $this->auth->logout();
    }
}
```

Este serviço geralmente trabalha em conjunto com o *model* de usuários e autentica somente após todas as validações obterem sucesso.

----
### Serviço de E-mail {#servico-de-e-mail}

O serviço de e-mail também conta um módulo de configuração. Este módulo contém dois métodos:

+ `setFrom(array $from)`, e;
+ `getFrom()`.

O primeiro tem a função de definir o nome e e-mail do remetente e o segundo de obter estes valores.

Exemplo de configuração:
```php
$configs->env->development->mail->setFrom([
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
]);
```

O serviço contém dois métodos:

+ `setFrom(array $from)`, e;
+ `send(...)`.

O método `setFrom()` do <b>módulo de configuração</b> tem os objetivos de definir as credenciais globais e tornar estes valores disponíveis no controller. Já o método do serviço é que definirá realmente qual será a credencial utilizada, isto é, tanto é possível utilizar as credenciais do módulo com o método `getFrom()` como também definir um valor diferente.

Exemplo de uso:
```php
$this->load('Services\Email');

$this->email->setFrom([
    'from' => 'Remetente',
    'from_mail' => 'email@remetente.com.br'
]);

// ou

$this->email->setFrom($this->configs->mail->getFrom());
```


Já o método `send()`, responsável pelo envio, suporta os seguintes parâmetros:

+ E-mail para qual será enviada a mensagem;
+ Assunto da mensagem;
+ Mensagem;
+ *(Opcional)* Credenciais, e;
+ Tipo do e-mail (Padrão: `true`; Caso seja setado como `false` o e-mail será enviado sem tags HTML).

Após executar o método `send()` ele retornará um booleano com o status do processo.


Serviço na prática:
```php
class ProdutosController extends \HXPHP\System\Controller
{
    public function comprarAction()
    {
        $this->load('Services\Email');
        $this->email->setFrom($this->configs->mail->getFrom());

        $compraComSucesso = $this->email->send(
            'fulano@email.com.br',
            'Compra realizada com sucesso!',
            'Mensagem',
            [],
            false
        );

        $outroEmail = $this->email->send(
            'fulano@email.com.br',
            'Outro e-mail',
            'Mensagem'
        );
    }
}
```

----
### Serviço de Redefinição de Senhas {#servico-de-redefinicao-de-senhas}

Este serviço também é extremamente simples e requer um único parâmetro no método construtor que é o link para o controller e action responsáveis pelo processo de redefinir a senha.

Este link deve ser absoluto e ter **obrigatoriamente** uma `/` no final, pois o serviço concatena este valor com o token gerado.

O token é uma propriedade pública e deve ser utilizado para validar a autenticidade da redefinição durante todo o processo.

Serviço na prática:
```php
class EsqueciASenhaController extends \HXPHP\System\Controller
{
    public function enviarAction()
    {
        $this->load(
            'Services\PasswordRecovery',
            $this->configs->site->url .
            $this->configs->baseURI .
            'recuperar/redefinir/'
        );

        echo $this->passwordrecovery->link;
        echo $this->passwordrecovery->token;
    }
}
```


O token gerado deverá ser armazenado no banco de dados para validação futura e conclusão do processo.


Obtenção do TOKEN:
```php
$token = $this->passwordrecovery->token;
```

----
### Serviço de Sessão {#servico-de-sessao}

O serviço de sessão tem a única finalidade de iniciar a sessão do PHP de forma personalizada, para tal, utiliza-se o método estático `sec_session_start()`.


Serviço na prática:
```php
\HXPHP\System\Services\StartSession\StartSession::sec_session_start();
```

----
### Serviço Simple cURL {#servico-simple-curl}

É muito comum a necessidade de obter e enviar dados para URLs externas através de código. Existem diferentes formas de realizar esta tarefa e uma delas é com o uso da biblioteca cURL.

O framework contém um serviço que executa uma das aplicações mais comuns desta biblioteca.

O serviço *Simple cURL* contém um único método estático (`connect()`) que suporta 3 parâmetros:

+ URL alvo;
+ *(Opcional)* $post;
+ *(Opcional)* $get;

O funcionamento consiste em acessar a URL alvo e enviar os parâmetros via POST ou GET mediante o preenchimento dos respectivos parâmetros.

Algumas observações:

+ O serviço conta com a configuração `CURLOPT_FOLLOWLOCATION` definida como `true`, isto é, a cURL está configurada para seguir qualquer redirecionamento, e;
+ A configuração `CURLOPT_RETURNTRANSFER` também está definida como `true`, isto é, o valor é retornado ao invés de impresso.


Serviço na prática:
```php
$retorno = \HXPHP\System\Services\SimplecURL\SimplecURL::connect(
    'http://www.hxphp.com.br/produtos/novo',
    [
        'name' => 'Curso Dominando o HXPHP Framework',
        'description' => 'Lorem Ipsum'
    ],
    [
        'token' => '1RSG6K2MV0PS2LVM01PD'
    ]
);
```