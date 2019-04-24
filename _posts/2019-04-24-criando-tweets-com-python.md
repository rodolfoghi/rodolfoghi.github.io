---
layout: post
title:  "Criando Tweets com Python"
date:   2019-04-24 17:49:00 -0200
categories: back-end python
---

# Criando Tweets com Python

## TL;DR

* Está com preguiça de ler? O código pronto [está aqui](https://github.com/rodolfoghi/getting-started-with-twitter-api).
* Quer ler um tutorial mais completo em inglês? Vá para a sessão de referências no final deste post.

## O que iremos precisar

* Python 3 que pode ser baixado aqui.
* Pacote Twython, instruções de como instalar aqui.
* Uma conta no Twitter.

## Criando uma aplicação no Twitter

Após ter criado sua conta no Twitter, é necessário registrar uma aplicação para que nosso programa Python tenha acesso.

1. Acesse apps.twitter.com e clique no botão Create New App.
2. Preencha os campos obrigatórios e clique no botão Create your Twitter application.
3. Na aba ‘Permissions’ verifique se o acesso está Read and Write, se não estiver, modifique e salve.
4. Clique na aba ‘Keys and Access Tokens’ e no final da página clique no botão ‘Create my access token’.

Isso é tudo que precisa ser feito, mais adiante neste post iremos utilizar os valores de Consumer key, Consumer secret, Access token, e Access token secret.

## Let’s Code

Utilize um terminal de linha de comando de sua preferência, enquanto escrevo este post estou utilizando o [Git Bash](https://gitforwindows.org/) no Windows 10.

### Verificando a versão do Python

{% highlight bash %}
$ python -V
Python 3.6.4
{% endhighlight %}

### Criando o arquivo de autenticação

Lembra dos dados da aplicação Twitter que criamos anteriormente? Eles serão utilizados aqui.

Crie uma pasta para o seu projeto e dentro dela um arquivo chamado auth.py
{% highlight bash %}

mkdir tweets-com-python
cd tweets-com-python/
touch auth.py

{% endhighlight %}

Edite o arquivo criado com o seu editor de textos favorito. Estou utilizando o [Visual Studio Code](https://code.visualstudio.com/).

{% highlight python %}

consumer_key = "YOUR_DATA_HERE"
consumer_secret = "YOUR_DATA_HERE"
access_token = "YOUR_DATA_HERE"
access_token_secret = "YOUR_DATA_HERE"

{% endhighlight %}

## Criando o arquivo de execução

Crie um arquivo chamado main.py, com o seguinte conteúdo.

{% highlight python %}
# Importação do arquivo com os dados de autenticação na aplicação Twitter
from auth import (
 consumer_key,
 consumer_secret,
 access_token,
 access_token_secret
)

# Importação do pacote e criação do objeto Twython que fará a mágica acontecer
from twython import Twython

twitter = Twython(
 consumer_key,
 consumer_secret,
 access_token,
 access_token_secret
)

# Variável com o texto da mensagem do Tweet
message = "Olá Twython!!"

# Aqui a mágica acontece :)
twitter.update_status(status=message)

{% endhighlight %}

Para executar o programa, basta digitar o seguinte comando em seu terminal. Ei, não esqueça de salvar o arquivo antes de executar.

{% highlight python %}

python main.py

{% endhighlight %}

Se tudo ocorrer conforme o planejado, você verá um Tweet em sua conta.


## Referências:
* [Getting started with the Twitter API](https://projects.raspberrypi.org/en/projects/getting-started-with-the-twitter-api)
* [Twython 3.6.0 documentation](https://twython.readthedocs.io/en/latest/)