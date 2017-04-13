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
## 13. *Tools* {#tools}

Nesta seção você irá conhecer o objeto auxiliar *Tools*.
----
### Métodos auxiliares {#metodos-auxiliares}

O objeto *Tools* é uma ferramenta de apoio geral, ou seja, trata-se de um objeto que contém métodos comuns com funções de filtro, tratamento, criptografia e afins.

Por padrão, este objeto contém os seguintes métodos <b>estáticos</b>:

+ `dd($data, $dump = false)` - Exibe os dados;
+ `hashHX($password, $salt = null)` - Criptografa a senha do usuário no padrão HXPHP;
+ `filteredName($input)` - Aplica o *camelize*;
+ `filteredFileName($input)` - Trata o nome de arquivos, e;
+ `decamelize($cameled, [$sep])` - Reverte o *camelize*;


#### Criptografia para senhas

Um dos recursos do HXPHP mais utilizados para sistemas de cadastro e login é a criptografia de senhas.

+ Gerando o `hash` e o `salt`:
```php
\HXPHP\System\Tools::hashHX('senhabruta');

/**
    * Retorno:
    * [
    *   'salt' => '...',
    *   'password' => '...'
    * ];
*/
```

O `salt` é um valor randômico que é concatenado com a senha bruta informada para gerar o `hash`.

----

+ Gerando o `hash` para validação com a senha bruta e um `salt` já definido:
```php
$senhaCriptografada = \HXPHP\System\Tools::hashHX('senhabruta', $user->salt);

if ($user->password === $senhaCriptografada['password'])
    return 'Usuário autenticado';
```

Como o `salt` é um valor aleatório, se o mesmo não for informado não será possível obter um `hash` idêntico para comparação mesmo que a senha informada pelo usuário esteja correta. Portanto, é necessário informar o `salt` que foi gerado para o `hash` armazenado no banco de dados, isto é, ambos os valores devem ser armazenados utilizados conforme o exemplo acima.