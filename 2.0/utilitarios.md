---
currentMenu: utilitarios
---
## Tools {#tools}

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

----

#### Hash

Um dos recursos do HXPHP mais utilizados para sistemas de cadastro e login é a geração de hash.

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

----

O `salt` é um valor randômico que é concatenado com a senha bruta informada para gerar o `hash`.

----

+ Gerando o `hash` para validação com a senha bruta e um `salt` já definido:

```php
$senhaCriptografada = \HXPHP\System\Tools::hashHX('senhabruta', $user->salt);

if ($user->password === $senhaCriptografada['password'])
    return 'Usuário autenticado';
```

----

Como o `salt` é um valor aleatório, se o mesmo não for informado não será possível obter um `hash` idêntico para comparação mesmo que a senha informada pelo usuário esteja correta. Portanto, é necessário informar o `salt` que foi gerado para o `hash` armazenado no banco de dados, isto é, ambos os valores devem ser armazenados utilizados conforme o exemplo acima.