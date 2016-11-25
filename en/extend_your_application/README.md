# Uygulamanı genişlet

Websitemizi oluşturmak için gerekli adımların hepsini tamamladık: bir modelin, url'nin, view'ün ve template'in nasıl yazılacağını biliyoruz. Websitemizi nasıl güzelleştirebiliriz onu da biliyoruz.

Pratik zamanı!

Blog'umuzda lazım olan ilk şey bir gönderiyi göstermek için bir sayfa, değil mi?

Halihazırda bir `Post` modelimiz var, dolayısıyla `models.py` dosyasına bir şey eklememize gerek yok.

## Bir gönderinin detayı için bir şablon linki oluşturun

`blog/templates/blog/post_list.html` dosyasına bir link (bağlantı) ekleyerek başlayacağız. Şu ana kadar yaptıklarımızın şöyle gözüküyor olması lazım:

```html
{% extends 'blog/base.html' %}

{% block content %}
    {% for post in posts %}
        <div class="post">
            <div class="date">
                {{ post.published_date }}
            </div>
            <h1><a href="">{{ post.title }}</a></h1>
            <p>{{ post.text|linebreaks }}</p>
        </div>
    {% endfor %}
{% endblock content %}

```

{% raw %}Gönderi listesindeki bir gönderinin başlığından bir gönderinin detay sayfasına bir link (bağlantı) olsun istiyoruz. `<h1><a href="">{{ post.baslik }}</a></h1>`'i gönderinin detay sayfasına link verecek şekilde değiştirelim:{% endraw %}

```html
<h1><a href="{% url 'post_detail' pk=post.pk %}">{{ post.title }}</a></h1>
```

{% raw %}Gizemli `{% url 'post_detail' pk=post.pk %}` satırını anlatma zamanı. Şüphelendiğiniz üzere, `{% %}` notasyonu, Django template tags (şablon etiketleri) kullandığımız manasına geliyor. Bu sefer bizim için URL oluşturacak bir template etiketi kullanacağız!{% endraw %}

`blog.views.post_detail`, oluşturmak istediğimiz `post_detail` *view*'una bir yol. Lütfen dikkat: `blog` uygulamamızın adı (`blog` dizini); `views`, `views.py` dosyasının adından geliyor ve son kısım - `post_detail` - *view*'ün adından geliyor.

Şimdi http://127.0.0.1:8000/'ye gittiğimizde bir hata alacağız (beklediğimiz bir şey, çünkü URL'miz veya `post_detail` için bir *view*'ümüz yok). Şöyle görünecektir:

![NoReverseMatch error (Tersi yok hatası)](images/no_reverse_match2.png)

## Bir gönderinin detayı için URL oluşturun

`post_detail` *view*'ümüz için `urls.py`'un içinde bir URL oluşturalım!

İlk gönderimizin detayının şu **URL**'de gösterilmesini istiyoruz: http://127.0.0.1:8000/post/1/

`blog/urls.py` dosyasında `post_detail` adında bir Django *view*'una işaret eden bir URL yapalım. Bu <1>view</1> bir gönderinin tümünü gösterecek. Add the line `url(r'^post/(?P<pk>\d+)/$', views.post_detail, name='post_detail'),` to the `blog/urls.py` file. Dosyanın şu hale gelmiş olması gerekiyor:

```python
from django.conf.urls import include, url
from . import views

urlpatterns = [
    url(r'^$', views.post_list, name='post_list'),
    url(r'^post/(?P<pk>\d+)/$', views.post_detail, name='post_detail'),
]
```

This part `^post/(?P<pk>\d+)/$` looks scary, but no worries - we will explain it for you: - it starts with `^` again -- "the beginning" - `post/` only means that after the beginning, the URL should contain the word **post** and **/**. Şimdilik iyi gidiyor. - `(?P<pk>\d+)` - this part is trickier. Buranın anlamı şu: Django bu alana yerleştirdiğimiz her şeyi alacak ve onu `pk` adında bir değişken olarak view'e aktaracak. `\d` also tells us that it can only be a digit, not a letter (so everything between 0 and 9). `+` en az bir veya daha fazla rakam olması gerektiğini ifade ediyor. Yani `http://127.0.0.1:8000/post//` eşleşmez ama `http://127.0.0.1:8000/post/1234567890/` eşleşir! - `/` - gene **/** - `$` - "son"!

Bu şu demek, eğer tarayıcınıza `http://127.0.0.1:8000/post/5/` yazarsanız, Django `post_detail` adında bir *view* aradığınızı anlar ve `pk` eşittir `5` bilgisini *view*'e aktarır.

`pk`, `primary key`'in (tekil anahtarın) kısaltılmış hali. Bu isim sıklıkla Django projelerinde kullanılır. Değişkeninize istediğiniz ismi verebilirsiniz (hatırlayın: küçük harfler ve boşluk yerine `_`!). For example instead of `(?P<pk>\d+)` we could have variable `post_id`, so this bit would look like: `(?P<post_id>\d+)`.

Ok, we've added a new URL pattern to `blog/urls.py`! Let's refresh the page: http://127.0.0.1:8000/ Boom! The server has stopped running again. Have a look at the console - as expected, there's yet another error!

![AttributeError (Özellik hatası)](images/attribute_error2.png)

Bir sonraki adımın ne olduğunu hatırlıyor musunuz? Tabi ki: view'ü eklemek!

## Gönderi detayı için bir view ekleyin

Bu sefer *view*'ümüze `pk` adında bir parametre ekleyeceğiz. *view*'ümüzün onu yakalaması gerekiyor, değil mi? Fonksiyonumuzu `def post_detail(request, pk):` olarak tanımlayacağız. Dikkat edin, url'lerde kullandığımız ismin birebir aynısını kullanmamız gerekiyor (`pk`). Bu değişkeni kullanmamak yanlıştır ve hataya sebep olacaktır!

Şimdi sadece ve sadece bir blog gönderisini almak istiyoruz. Bunu yapmak için querysets'i şu şekilde kullanabiliriz:

    Post.objects.get(pk=pk)
    

Ama bu kodun bir problemi var. Eğer gelen `primary key` (`pk` - tekil anahtar) ile bir `Post` (gönderi) yoksa, çok çirkin bir hatamız olacak!

![DoesNotExist error (Yok hatası)](images/does_not_exist2.png)

Bunu istemiyoruz! Ama tabi Django'da bunu ele alan bir şey var: `get_object_or404`. Eğer verilen `pk` ile bir `Post` bulunamazsa, çok daha güzel bir sayfa gösterilecek (`Sayfa bulunamadı 404` sayfası).

![Page not found (Sayfa bulunamadı)](images/404_2.png)

İyi haber şu, kendi `Sayfa bulunamadı` sayfasını yapabilir ve istediğiniz kadar güzelleştirebilirsiniz. Ama şu anda çok önemli değil, o yüzden bu kısmı atlayacağız.

Evet, `views.py` dosyamıza bir *view* ekleme zamanı!

`blog/views.py` dosyasını açıp aşağıdaki kodu ekleyelim:

```python
from django.shortcuts import render, get_object_or_404
```

Bunu diğer `from` satırlarının yakınına eklememiz gerekiyor. Dosyanın sonuna *view*'ümüzü ekleyeceğiz:

```python
def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})
```

Evet. http://127.0.0.1:8000/ sayfasını tazeleme zamanı

![Post list view (Gönderi listesi görünümü)](images/post_list2.png)

Çalıştı! Fakat blog gönderisi başlığındaki bir bağlantıya tıkladığınızda ne oluyor?

![TemplateDoesNotExist error (Template yok hatası)](images/template_does_not_exist2.png)

Of hayır! Başka bir hata! Ama onu nasıl halledeceğimizi biliyoruz, di mi? Bir template eklememiz gerekiyor!

## Gönderi detayı için bir template oluşturun

`blog/templates/blog` dizininde `post_detail.html` adında bir dosya oluşturacağız.

Şöyle görünmeli:

```html
{% extends 'blog/base.html' %}

{% block content %}
    <div class="post">
        {% if post.published_date %}
            <div class="date">
                {{ post.published_date }}
            </div>
        {% endif %}
        <h1>{{ post.title }}</h1>
        <p>{{ post.text|linebreaks }}</p>
    </div>
{% endblock %}
```

Bir kere daha `base.html` dosyasını genişleteceğiz. `content` bloğunda bir gönderinin varsa yayınlama tarihini , başlığını ve metnini göstermek istiyoruz. Ama daha önemli şeyleri konuşmalıyız, değil mi?

{% raw %}`{% if ... %} ... {% endif %}` bir şeyi kontrol etmek istediğimizde kullanabileceğimiz bir template etiketidir ( **Python'a giriş** bölümünden <1>if ... else ..</code> 'i hatırladınız mı?). Bu senaryoda gönderinin `yayinlama_tarihi`'nin boş olup olmadığına bakmak istiyoruz.{% endraw %}

Ok, we can refresh our page and see if `TemplateDoesNotExist` is gone now.

![Post detail page (Gönderi detay sayfası)](images/post_detail2.png)

Heyo! Çalışıyor!

## Bir şey daha: deployment (yayına alma) zamanı!

Sitenizin hala PythonAnywhere'de çalışıp çalışmadığına bakmakta fayda var, değil mi? Yeniden taşımayı deneyelim.

    $ git status
    $ git add --all .
    $ git status
    $ git commit -m "Added view and template for detailed blog post as well as CSS for the site."
    $ git push
    

* Sonra bir [PythonAnywhere Bash konsol](https://www.pythonanywhere.com/consoles/) una gidip:

    $ cd ilk-blogum
    $ git pull
    [...]
    

* Nihayet, [Web tab](https://www.pythonanywhere.com/web_app_setup/) ına gidip **Reload** edelim.

O kadar! Tebrikler :)