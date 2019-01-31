---
layout: post
title:  "Obtendo os telefones dos anunciantes da OLX com Python"
date:   2019-01-31 14:46:00 -0200
categories: back-end python web-scraping
---

## O problema

Obter os números de telefone dos anunciantes da OLX.

## A solução manual

![Solução manual](/assets/images/olx-phone-loader-solucao-manual.gif){:class="img-responsive"}

O processo consiste em:

1. obter a lista de anúncios;
2. clicar no anúncio;
3. clicar no link "ver número";
4. copiar o número. Obs: não tem como fazer Ctrl+C/Ctrl+V pois o número é uma imagem;
5. repetir para cada anúncio.

## A solução automatizada

![Solução automatizada](/assets/images/olx-phone-loader-solucao-automatizada.gif){:class="img-responsive"}

O processo consiste em:

1. obter a URL da lista de anúncios;
2. executar o programa;
3. ser FELIZ!

## Como isso funciona?

![Magic](/assets/images/magic.gif){:class="img-responsive"}

**1.** Obtém a lista de anúncios utilizando a biblioteca `urllib`.

{% highlight python %}
def get_site_html(url):
    try:
        html = urlopen(url)
        return html
    except HTTPError as e:
        print(e)
        return None
{% endhighlight %}

**2.** Lê o HTML utilizando a biblioteca `BeautifulSoup`:

{% highlight python %}
def get_bsobj_from(html):
    try:
        bs_obj = BeautifulSoup(html.read(), "html.parser")
        return bs_obj
    except AttributeError as e:
        print(e)
        return None
{% endhighlight %}

**3.** Faz uma nova requisição para cada anúncio.

**4.** Procura pelo telefone na resposta. Obs: Ele é um arquivo GIF. :(

{% highlight python %}
obj_anuncio = get_bsobj_from(html_anuncio)
if obj_anuncio is None:
    return None

span_visible_phone = obj_anuncio.find(id="visible_phone")
div_codigo_do_anuncio = obj_anuncio.find("div", {"class": "OLXad-id"})
codigo_do_anuncio = div_codigo_do_anuncio.p.strong.get_text()
anuncio["codigo"] = codigo_do_anuncio
phone = "Desconhecido"
if (span_visible_phone):
    imgurl = span_visible_phone.img['src']

    img = get_site_html(imgurl)
    if img is None:
        return None
{% endhighlight %}

**5.** Salva o GIF na pasta `images`.
{% highlight python %}
gif_name = "images/" + codigo_do_anuncio + '.gif'
localFile = open(gif_name, 'wb')
localFile.write(img.read())
localFile.close()
{% endhighlight %}

**6.** Converte o GIF para PNG e salva.

{% highlight python %}
def gif_to_png(gif):
    img = Image.open(gif)
    img.save(gif+".png",'png', optimize=True, quality=70)
{% endhighlight %}

**7.** Lê o texto da imagem utilizando a biblioteca `pytesseract`.

{% highlight python %}
def image_to_text(file_name):
    phrase = ocr.image_to_string(Image.open(file_name))
    return phrase
{% endhighlight %}

**8.** Finalmente, salva os dados em um arquivo **CSV** utilizando a biblioteca `csv` do Python.

{% highlight python %}
def dictionary_list_to_csv(dict_list):
    keys = dict_list[0].keys()
    csvfile = get_csv_file_name()
    print("Gravando o arquivo " + csvfile)
    with open(csvfile, "w") as output:
        writer = csv.DictWriter(output, fieldnames=keys, lineterminator='\n')
        writer.writeheader()
        writer.writerows(dict_list)
{% endhighlight %}

## Referências:

* [Projeto no GitHub](https://github.com/rodolfoghi/olx-phone-loader){:target="_blank"}
* [BeautifulSoup](https://www.crummy.com/software/BeautifulSoup/){:target="_blank"}
* [urllib — URL handling modules](https://docs.python.org/3/library/urllib.html){:target="_blank"}
* [Python-tesseract](https://pypi.org/project/pytesseract/){:target="_blank"}
* [Livro Web Scraping com Python](https://novatec.com.br/livros/web-scraping-com-python/){:target="_blank"}
* [Reconhecimento ótico de caracteres](https://pt.wikipedia.org/wiki/Reconhecimento_%C3%B3tico_de_caracteres){:target="_blank"}