---
currentMenu: models
---
## *Models* {#models}
Nesta seção você irá conhecer mais uma das três camadas do padrão <b>MVC</b>.

----

### O que são *Models*? {#o-que-sao-models}
Os *models*, comumente chamados de <b>modelos</b>, são responsáveis por todos os processos relacionados à dados. Ou seja, o processo de leitura, escrita e validação de dados é realizado nesta camada. Estas rotinas são chamadas de <b>regras de negócio</b>.

O modelo acessa qualquer tipo de informação, isto é, desde um banco de dados até um arquivo *XML*.
As informações obtidas no modelo são repassadas para o controlador que, mediante a necessidade, repassa para a visualização. Em outro caminho existe o envio de informações da visualização para o controlador; que, por sua vez, envia para o modelo, e; este consuma o processo realizando as devidas alterações nos dados.

----

### Criando *Models* {#criando-models}
Agora que você já sabe o que é um *model*, deve se atentar para os detalhes na criação do mesmo. Quando um *model* é criado com o objetivo de manipular uma tabela do banco de dados é mais do que recomendado que este seja desenvolvido nos padrões do *ORM* vigente do framework (vide mais detalhes abaixo).

Para aproveitar dos benefícios do *ORM* é necessário que o *model* herde as configurações dele e, para tal, é necessário que o seu *model* seja uma extensão da classe mestre `\HXPHP\System\Model`.

O código base de um *model* é:
```php
class NomeDoModel extends \HXPHP\System\Model
{
        ...
}
```

As convenções da nomenclatura dos *models* são ditas pelo *ORM* vigente, portanto estas serão abordadas em uma seção específica.

----

### *ORM* PHP ActiveRecord {#orm-php-activerecord}

*ORM*, da sigla *Object-relational mapping*, tem a seguinte definição de acordo com a [Wikipedia](http://pt.wikipedia.org/wiki/Mapeamento_objeto-relacional):
> "Mapeamento objeto-relacional (ou ORM, do inglês: Object-relational mapping) é uma técnica de desenvolvimento utilizada para reduzir a impedância da programação orientada aos objetos utilizando bancos de dados relacionais. As tabelas do banco de dados são representadas através de classes e os registros de cada tabela são representados como instâncias das classes correspondentes.

> Com esta técnica, o programador não precisa se preocupar com os comandos em linguagem SQL; ele irá usar uma interface de programação simples que faz todo o trabalho de persistência.

> Não é necessária uma correspondência direta entre as tabelas de dados e as classes do programa. A relação entre as tabelas onde originam os dados e o objecto que os disponibiliza é configurada pelo programador, isolando o código do programa das alterações à organização dos dados nas tabelas do banco de dados."

Em outras palavras, com o uso de um *ORM* torna-se pouco provável o uso de comandos SQL nos códigos.

A lógica é a seguinte: para cada tabela tem-se um *model*; sendo que cada *model* é um objeto, e; o *model* é uma extensão da classe mestre do *ORM*.

O *ORM* nativo do HXPHP Framework é o [PHP ActiveRecord](http://phpactiverecord.org/), que é uma biblioteca *open source* desenvolvida nos padrões [ActiveRecord](http://en.wikipedia.org/wiki/Active_record_pattern). Trata-se de uma ferramenta de simples instalação e com uma sintaxe fácil, ou seja, além de ser fácil a sua implementação, o mesmo ainda conta com um nível mais do que considerável de legibilidade até mesmo para quem nunca ouviu falar deste *ORM*, pois, somente pelos métodos, nomeados de forma sugestiva, já é possível compreender o funcionamento.
De toda forma, é recomendado que você leia a documentação oficial para compreender todos os recursos e possibilidades.

#### Convenções
A questão fundamental no uso deste *ORM* está nas convenções de nomenclatura das tabelas do banco de dados e dos *models*. Confira a lista das 5 principais:

+ Todas as tabelas devem ser nomeadas em INGLÊS;
+ O nome do model é o <b>singular</b> do nome da tabela;
+ O nome do model deverá ser escrito no padrão CamelCase;
+ A chave primária deve ser 'id' e NÃO 'ID' ou 'Id', e;
+ {Tabela} login_attempts => {Model} LoginAttempt / lost_passwords => LostPassword.

----

### *Model* na prática {#model-na-pratica}

```php
class Product extends \HXPHP\System\Model
{
    static function exists($product_code)
    {
        $product = self::find_by_code($product_code);

        return !is_null($product) ? true : false;
    }
}
```
