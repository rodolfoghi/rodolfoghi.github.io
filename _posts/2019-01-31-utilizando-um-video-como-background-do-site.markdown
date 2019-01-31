---
layout: post
title:  "Utilizando um vídeo como background do site"
date:   2019-01-31 11:53:00 -0200
categories: front-end css tutorial
---

Você pode ver a versão final do site [clicando aqui](https://rodolfoghi.github.io/morning-coffee/){:target="_blank"}.

![Morning Coffee](/assets/images/good_morning_coffee.jpg){:class="img-responsive"}

Crie um arquivo chamado index.html com o seguinte conteúdo:

{% highlight html %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Morning Coffee</title>

    <link rel="stylesheet" href="style.css">
</head>
<body>

</body>
</html>
{% endhighlight %}

Acrescente o vídeo dentro da tag body:

{% highlight html %}
<body>
    <!-- O vídeo -->
    <video autoplay muted loop id="meuVideo">
        <source src="morning-coffee.mp4" type="video/mp4">
    </video>
</body>
{% endhighlight %}

Adicione o seguinte código CSS para o vídeo:

{% highlight css %}
#meuVideo {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
}
{% endhighlight %}

Adicione umadiv para o conteúdo do site:

{% highlight html %}
<div class="conteudo">
      <h1>Morning Coffee</h1>
      <p>
        O café é uma bebida produzida a partir dos grãos torrados do fruto do
        cafeeiro.
      </p>
      <p>
        É servido tradicionalmente quente, mas também pode ser consumido gelado.
        O café é um estimulante, por possuir cafeína — geralmente 80 a 140 mg
        para cada 207 ml dependendo do método de preparação.
      </p>
      <button id="meuBotao">Saiba mais</button>
</div>
{% endhighlight %}

Por fim, adicione o seguinte código CSS para estilizar o conteúdo:

{% highlight css %}
.conteudo {
  position: fixed;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  color: #f1f1f1;
  width: 100%;
  padding: 20px;
}

#meuBotao {
  width: 200px;
  font-size: 18px;
  padding: 10px;
  border: none;
  background: #000;
  color: #fff;
  cursor: pointer;
}

#meuBotao:hover {
  background: #ddd;
  color: black;
}
{% endhighlight %}

## Referências

1. [Projeto no GitHub](https://github.com/rodolfoghi/morning-coffee){:target="_blank"}
2. [W3schools](https://www.w3schools.com/howto/howto_css_fullscreen_video.asp){:target="_blank"}