---
title: Meia hora aprendendo Rust - Parte 1
author: Rodolfo Ghiggi
date: 2020-08-09 00:21:00 -0300
categories: [back-end, rust]
tags: [rust]
---

![Logotipo da linguagem Rust]({{ "/assets/img/rust-logo.png" | relative_url }})

Esta é uma tradução livre do post escrito por [Amos](https://fasterthanli.me/about/) que você pode ler [aqui](https://fasterthanli.me/blog/2020/a-half-hour-to-learn-rust/). Como o artigo é bem extenso vou quebrar ele em pelo menos duas partes, esta é a parte 1.

A tradução foi autorizada por Amos através de mensagem direta no Twitter.

Para adquirir fluência em uma linguagem de programação é preciso ler muito. Mas como ler muito se ainda não sabemos o que aquele linguagem significa?

Neste artigo, em vez de focar em um ou dois conceitos, tentarei analisar o maior número possível de trechos de Rust e explicar o que significam as palavras-chave e os símbolos que eles contêm.

Pronto? Vamos lá!

## Palavra chave let

`let` é utilizado pra criar uma variável:

```rust
let x; // declara "x"
x = 42; // define o valor 42 para "x"
```

Isso também pode ser escrito em apenas uma linha:

```rust
let x = 42;
```

Você pode definir explicitamente o tipo da variável utilizando : , que é uma anotação de tipo:

```rust
let x: i32; // `i32` é um valor inteiro de 32 bits com sinal
x = 42;


// também há i8, i16, i32, i64, i128
//    além disso há u8, u16, u32, u64, u128 para valores sem sinal
```

Isso também pode ser escrito em apenas uma linha:

```rust
let x: i32 = 42;
```

O compilador de Rust impede o uso de variáveis não inicializadas.

```rust
let x;
foobar(x); // error: borrow of possibly-uninitialized variable: `x`
x = 42;
```

E o fato de ele fazer isso é muito bom:

```rust
let x;
x = 42;
foobar(x); // a variável `x` pode ser utilizada aqui
```

O sublinhado(underline ou underscore) _ é um nome especial, ou melhor uma falta de nome, basicamente significa jogar algo fora:

```rust
// isso não faz nada pois 42 é uma constante
let _ = 42;

// isso chama a função `get_thing` mas joga o seu resultado fora
let _ = get_thing();
```

Nomes que começam com sublinhado são nomes válidos, mas o compilador não alerta sobre a não utilização dessas variáveis:

```rust
// nós podemos usar `_x` eventualmente, quando queremos nos 
// livrar do aviso do compilador sobre variável não utilizada.
let _x = 42;
```

É possível inicializar variáveis separadamente utilizado o mesmo nome — você pode sobrescrever uma variável:

```rust
let x = 13;
let x = x + 3;
// usando `x` depois desta linha, refere-se apenas ao segundo `x`,
// o primeiro `x` não existe mais.
```

## Tuplas

O Rust possui tuplas, que você pode considerar como “coleções de valores fixos de tipos diferentes”.

```rust
let pair = ('a', 17);
pair.0; // isso é'a'
pair.1; // isso é 17
```

Se você realmente precisar, pode definir os tipos dos valores da tupla:

```rust
let pair: (char, i32) = ('a', 17);
```

Tuplas podem ser desconstruídas, ou seja, os seus valores podem ser atribuídos a variáveis individuais:

```rust
let (some_char, some_int) = ('a', 17);
// agora, `some_char` é 'a', e `some_int` é17
```

Isso é bem útil quando uma função retorna uma tupla:

```rust
let (left, right) = slice.split_at(middle);
```

Obviamente, ao desconstuir uma tulpa pode-se descartar parte dela utilizando _ :

```rust
let (_, right) = slice.split_at(middle);
```

## Ponto e vírgula

O ponto e vírgula ; define o fim da instrução:

```rust
let x = 3;
let y = 5;
let z = y + x;
```

Isso significa que uma instrução pode conter várias linhas:

```rust
let x = vec![1, 2, 3, 4, 5, 6, 7, 8]
    .iter()
    .map(|x| x + 3)
    .fold(0, |x, y| x + y);
```

Omitir o ponto e vírgula no final da função é o mesmo que retornar:

```rust
fn fair_dice_roll() -> i32 {
    return 4;
}

fn fair_dice_roll() -> i32 {
    4
}
```

## Funções

`fn` declara uma função:

```rust
fn greet() {
    println!("Olá!");
}
```

E esta função retorna um inteiro com sinal, a seta -> indica o tipo de retorno:

```rust
fn fair_dice_roll() -> i32 {
    4
}
```

## Blocos {}

O par de chaves é utilizado para declarar um bloco que possui seu próprio escopo:

```rust
// Isso imprime "in", e depois "out"
fn main() {
    let x = "out";
    {
        // este é outro `x`
        let x = "in";
        println!(x);
    }
    println!(x);
}
```

Blocos também são expressões, o que significa que eles avaliam um valor:

```rust
// isso:
let x = 42;

// é equivalente a:
let x = { 42 };
```

Dentro de um bloco, podemos ter várias instruções:

```rust
let x = {
    let y = 1; // primeira instrução
    let z = 2; // segunda instrução
    y + z // isso é o que o bloco avaliará e colocará o resultado em `x`
};
```

## Condicionais

Condicionais `if` também são expressões:

```rust
fn fair_dice_roll() -> i32 {
    if feeling_lucky {
        6
    } else {
        4
    }
}
```

Um `match` também é uma expressão:

```rust
fn fair_dice_roll() -> i32 {
    match feeling_lucky {
        true => 6,
        false => 4,
    }
}
```

O `match` é exaustivo: pelo menos um dos cados precisa bater:

```rust
fn print_number(n: Number) {
    match n {
        Number { value: 1, .. } => println!("One"),
        Number { value: 2, .. } => println!("Two"),
        Number { value, .. } => println!("{}", value),
        // Se este último caso não existir você receberá um erro do compilador
    }
}
```

Se isso for ruim para você, pode-se usar o `_` para “pegar todos” os padrões:

```rust
fn print_number(n: Number) {
    match n.value {
        1 => println!("One"),
        2 => println!("Two"),
        _ => println!("{}", n.value),
    }
}
```

## Acessando valores

Ponto `.` normalmente é utilizado para acessar o valor de um campo:

```rust
let a = (10, 20);
a.0; // isso é 10

let amos = get_some_struct();
amos.nickname; // isso é "fasterthanlime"
```

Ou para chamar uma função:

```rust
let nick = "fasterthanlime";
nick.len(); // isso é 14
```

Os dois pontos duplos `::` , são parecidos, mas eles acessam os namespaces (não sabia exatamente como traduzir isso, então deixei assim).

Neste exemplo, std é uma biblioteca, cmp é um módulo (~ um arquivo de código), e min é uma função:

```rust
let least = std::cmp::min(3, 8); // isso é 3 
```

A diretiva use pode ser utilizada para “trazer para o escopo” nomes de outros namespaces:

```rust
use std::cmp::min;

let least = min(7, 1); // isso é 1
```

## Diretiva `use`

Na diretiva `use` as chaves {} possuem outro significado: elas são “ globs”. Se quisermos importar as funções min e max , podemos fazer assim:

```rust
// isso funciona:
use std::cmp::min;
use std::cmp::max;

// isso também:
use std::cmp::{min, max};

// e isso também!
use std::{cmp::min, cmp::max}
```

O curinga (*) permite importar tudo de um namespace:

```rust
// isso importa `min` e `max` e muitas outras coisas
use std::cmp::*;
```

## Tipos também são namespaces

Tipos também são namespaces, e os seus métodos podem ser chamados normalmente:

```rust
let x = "amos".len(); // isso é 4
let x = str::len("amos"); // isso também é 4
```

`str` é um tipo primitivo, mas muitos tipos não primitivos, também estão no escopo por padrão:

```rust
// `Vec` é uma estrutura regular, não um tipo primitivo
let v = Vec::new();

// este é exatamente o mesmo código, mas com o caminho completo para `Vec`
let v = std::vec::Vec::new();
```

Isso funciona porque o Rust insere automaticamente no início de cada módulo:

```rust
use std::prelude::v1::*;
```

(Que por sua vez, reexporta muitos símbolos, como Vec, String, Option e Result).

## Estruturas

Estruturas são declaradas através da palavra chave `struct`:

```rust
struct Vec2 {
    x: f64, // 64 bits com ponto flutuante, também conhecido como "double precision"
    y: f64,
}
```

Elas podem ser inicializadas através de estruturas literais:

```rust
let v1 = Vec2 { x: 1.0, y: 3.0 };
let v2 = Vec2 { y: 2.0, x: 4.0 };
// a ordem não importa, apenas os nomes
```

Existe um atalho para inicializar o resto dos campos a partir de outra estrutura:

```rust
let v3 = Vec2 {
    x: 14.0,
    ..v2
};
```

Isso é chamado de “sintaxe de atualização de estrutura”, só pode acontecer na última posição e não pode ser seguido por uma vírgula.

Observe que podemos inicializar todos os campos:

```rust
let v4 = Vec2 { ..v3 };
```

Estruturas, assim como as tuplas, também podem ser desconstruídas:

Assim como isto é um padrão válido:

```rust
let (left, right) = slice.split_at(middle);
```

Isso também:

```rust
let v = Vec2 { x: 3.0, y: 6.0 };
let Vec2 { x, y } = v;
// `x` agora é 3.0, `y` é `6.0`
```

E isso também:

```rust
let Vec2 { x, .. } = v;
// isso descarta o valor de `v.y`
```

Você pode declarar funções nas suas estrutuas:

```rust
struct Number {
    odd: bool,
    value: i32,
}

impl Number {
    fn is_strictly_positive(self) -> bool {
        self.value > 0
    }
}
```

E utilizá-las como de costume:

```rust
fn main() {
    let minus_two = Number {
        odd: false,
        value: -2,
    };
    println!("positive? {}", minus_two.is_strictly_positive());
    // isso imprime "positive? false"
}
```

## `let` em condicionais

O `let` também pode ser usado em condicionais `if`:

```rust
struct Number {
    odd: bool,
    value: i32,
}

fn main() {
    let one = Number { odd: true, value: 1 };
    let two = Number { odd: false, value: 2 };
    print_number(one);
    print_number(two);
}

fn print_number(n: Number) {
    if let Number { odd: true, value } = n {
        println!("Odd number: {}", value);
    } else if let Number { odd: false, value } = n {
        println!("Even number: {}", value);
    }
}

// isso imprime:
// Odd number: 1
// Even number: 2
```

Os casos do `match` também são padrões como `if let`:

```rust
fn print_number(n: Number) {
    match n {
        Number { odd: true, value } => println!("Odd number: {}", value),
        Number { odd: false, value } => println!("Even number: {}", value),
    }
}
// isso imprime o mesmo de antes
```

## Variáveis são imutáveis por padrão

Variáveis são imutáveis por padrão, então você não pode alterá-las:

```rust
fn main() {
    let n = Number {
        odd: true,
        value: 17,
    };
    n.odd = false; // error: cannot assign to `n.odd`,
                   // as `n` is not declared to be mutable
}
```

E elas também não podem receber valores assim:

```rust
fn main() {
    let n = Number {
        odd: true,
        value: 17,
    };
    n = Number {
        odd: false,
        value: 22,
    }; // error: cannot assign twice to immutable variable `n`
}
```

A palavra chave mut declara a variável como mútavel:

```rust
fn main() {
    let mut n = Number {
        odd: true,
        value: 17,
    }
    n.value = 19; // tudo certo
}
```

## Referências

 - [Post original (em inglês)](https://fasterthanli.me/blog/2020/a-half-hour-to-learn-rust/)
 - [Twitter do Amos](https://twitter.com/fasterthanlime)