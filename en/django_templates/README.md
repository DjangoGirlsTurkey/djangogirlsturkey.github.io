# Django template

Bazı verileri gösterme zamanı! Django bunun için bize faydalı bazı yerleşik **template etiketleri** sunuyor.

## Template etiketleri nedir?

Görüyoruz ki aslında, HTML'de Python kodu yazamayız, çünkü tarayıcılar bunu anlamaz. Tarayıcılar yalnızca HTML'den anlar. Biliyoruz ki Python daha dinamik bir dil iken, HTML oldukça statiktir.

**Django template etiketleri** Python benzeri yapıların HTML'ye aktarılmasını sağlar, böylece dinamik web sitelerini daha kolay ve hızlı oluşturabiliriz!

## Gönderi listesi template'ini göster

Bir önceki bölümde, template'e `posts` değişkeni içinde gönderiler listesi verdik. Şimdi, bunu HTML'de göstereceğiz.

Django şablonunda bir değişken yazdırmak için, değişken adını çift kıvrımlı parantez içinde şu şekilde kullanırız:

```html
{{ posts }}
```

Bunu `blog/templates/blog/post_list.html` şablonunda deneyelim. İkinci `<div>`'den üçüncü `</div>`'e kadar olan her şeyi `{{ posts }}` ile değiştirelim. Ne olduğunu görmek için dosyayı kaydedip sayfayı yenileyelim:

![Şekil 13.1](images/step1.png)

Gördüğümüz sadece bu:

    [<Post: Gönderi 2>, <Post: Gönderi 1>]
    

Yani Django bunu bir nesneler listesi olarak algılıyor. **Python'a giriş**'ten listelerin nasıl gösterildiğini hatırlıyor musun? Evet, döngülerle! Bir Django template ile bunu şöyle yaparsın:

```html
{% for post in posts %}
    {{ post }}
{% endfor %}
```

Bunu kendi template'imizle deneyelim.

![Şekil 13.2](images/step2.png)

İşe yarıyor! Fakat bunların daha önce **HTML'ye giriş** bölümünde oluşturduğumuz statik gönderiler gibi görünmesini istiyoruz. HTML ve template etiketlerini karıştırabiliriz. `body` şöyle görünecektir:

```html
<div>
    <h1><a href="/">Django Girls Blog</a></h1>
</div>

{% for post in posts %}
    <div>
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaks }}</p>
    </div>
{% endfor %}
```

{% raw %}`{% for %}` ve `{% endfor %}` arasına koyduğunuz her şey listedeki her nesne için tekrarlanır. Sayfanı yenile:{% endraw %}

![Şekil 13.3](images/step3.png)

`{{ post.baslik }}` ya da `{{ post.yazi }}` için biraz farklı bir yazım kullandığımızı farkettin mi? Böylece `Post` modelinde tanımlanan alanlardaki verilere ulaşıyoruz. Ayrıca `|linebreaks` (satırsonu), gönderilerin metnini, satır sonlarını paragraflara çeviren bir filtreden geçiriyor.

## Bir şey daha

Web sitemizin İnternet'te hâlâ çalıştığını görmek iyi olacak, değil mi? PythonAnywhere yükleyelim yine. Adımları hatırlayalım...

* İlk önce kodumuzu Github'a push komutu ile yükleyelim

    $ git status
    [...]
    $ git add --all .
    $ git status
    [...]
    $ git commit -m "Modified templates to display posts from database."
    [...]
    $ git push
    

* [PythonAnywhere](https://www.pythonanywhere.com/consoles/)'e bağlanalım ve **Bash konsolu**'na gidelim (veya yeni bir konsol açalım) ve şunu çalıştıralım:

    $ cd ilk-blogum
    $ git pull
    [...]
    

* Ve son olarak da [Web tab](https://www.pythonanywhere.com/web_app_setup/) sekmesine gidip web uygulamamızdaki **Reload**'a basalım. Şimdi güncellememiz yayında olmalı! If the blog posts on your PythonAnywhere site don't match the posts appearing on the blog hosted on your local server - that's OK. The databases on your local computer and Python Anywhere don't sync with the rest of your files.

Congrats! Now go ahead and try adding a new post in your Django admin (remember to add published_date!) Make sure you are in the Django admin for your pythonanywhere site, https://www.yourname.pythonanywhere.com/admin. Then refresh your page to see if the post appears there.

Şiir gibi çalışıyor, değil mi? Gurur duyabiliriz! Şimdi bilgisayar başından bir süre kalkalım, çünkü bir molayı hak ettik. :)

![Şekil 13.4](images/donut.png)