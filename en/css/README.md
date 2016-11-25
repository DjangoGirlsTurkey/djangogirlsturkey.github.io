# CSS - güzelleştir!

Blogumuz hala epey çirkin gözüküyor, değil mi? Güzelleştirme zamanı! Bunun için CSS kullanacağız.

## CSS Nedir?

Basamaklı Stil Sayfaları (Cascading Style Sheets - CSS) bir websayfasının görünüm ve biçimlendirmelerini tanımlamak için kullanılan bir işaretleme (markup) dilidir (HTML gibi). CSS'i websayfanızın makyajı olarak görebilirsiniz ;).

But we don't want to start from scratch again, right? Once more, we'll use something that programmers released on the Internet for free. You know, reinventing the wheel is no fun.

## Hadi Bootstrap kullanalım!

Bootstrap güzel websayfaları geliştirmek için kullanılan en popüler HTML ve CSS çerçevesidir (framework): http://getbootstrap.com/

It was written by programmers who worked for Twitter. Now it's developed by volunteers from all over the world!

## Bootstrap kurulumu

Bootstrap kurmak için `.html` dosyanızda `<head>` kısmına şunları eklemeniz gerekli (`blog/templates/blog/post_list.html`):

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
```

Bu, projemize hiçbir yeni dosya eklemez. Yalnızca internet üzerinde var olan dosyalara işaret eder. Şimdi websitenizi açın ve sayfayı yenileyin. İşte oldu!

![Şekil 14.1](images/bootstrap1.png)

Şimdiden daha güzel gözüküyor!

## Django'da statik dosyalar

Son olarak **statik dosyalar** diye bahsettiğimiz şeylere daha yakından bakalım. Static files are all your CSS and images. Their content doesn't depend on the request context and will be the same for every user.

### Django'da statik dosyaları nereye koymalı

Django already knows where to find the static files for the built-in "admin" app. Now we just need to add some static files for our own app, `blog`.

Bunu blog uygulamamızın içerisinde `static` isimli bir klasör oluşturarak yapacağız:

    djangogirls
    ├── blog
    │   ├── migrations
    │   └── static
    └── mysite
    

Django will automatically find any folders called "static" inside any of your apps' folders. Then, it will be able to use their contents as static files.

## İlk CSS dosyanız!

Şimdi web sayfamıza kendi stilimizi eklemek için bir CSS dosyası oluşturalım. `static` klasörü içinde `css` adlı yeni bir klasör oluşturalım. Şimdi de `css` klasörü içinde `blog.css` adlı yeni bir dosya oluşturalım. Hazır mısınız?

    djangogirls
    └─── blog
         └─── static
              └─── css
                   └─── blog.css
    

Şimdi CSS yazma zamanı! `blog/static/css/blog.css` dosyasını kod editöründe açın.

We won't be going too deep into customizing and learning about CSS here. It's pretty easy and you can learn it on your own after this workshop. There is a recommendation for a free course to learn more at the end of this page.

Ancak az da olsa yapalım. Acaba başlığımızın rengini mi değiştirsek? Bilgisayarlar renkleri anlamak için özel kodlar kullanır. These codes start with `#` followed by 6 letters (A-F) and numbers (0-9). For example, the code for blue is `#0000FF`. You can find the color codes for many colors here: http://www.colorpicker.com/. Ayrıca [önceden tanımlanmış renkler](http://www.w3schools.com/cssref/css_colornames.asp)i de kullanabilirsin, `red` (kırmızı) ve `green` (yeşil) gibi.

`blog/static/css/blog.css` dosyanıza şu kodu eklemelisiniz:

```css
h1 a {
    color: #FCA205;
}
```

`h1 a` bir CSS Seçicisidir (Selector). This means we're applying our styles to any `a` element inside of an `h1` element. So when we have something like: `<h1><a href="">link</a></h1>` the `h1 a` style will apply. Bu durumda, rengi `#FCA205` yani turuncu yapmasını söylüyoruz. Elbette, buraya kendi arzu ettiğin rengi koyabilirsin!

Bir CSS dosyasında, HTML dosyasındaki öğeler için stil belirleriz. The first way we identify elements is with the element name. You might remember these as tags from the HTML section. Things like `a`, `h1`, and `body` are all examples of element names. We also identify elements by the attribute `class` or the attribute `id`. Sınıf ve id (kimlik), bir elemente senin tarafından verilen isimlerdir. Sınıflar bir öğe grubunu tanımlar, id'ler ise belirli bir öğeye işaret ederler. For example, you could identify the following tag by using the tag name `a`, the class `external_link`, or the id `link_to_wiki_page`:

```html
<a href="http://en.wikipedia.org/wiki/Django" class="external_link" id="link_to_wiki_page">
```

Daha fazla bilgi için [w3schools'da CSS seçicileri](http://www.w3schools.com/cssref/css_selectors.asp)ni okuyabilirsin.

Sonrasında, ayrıca HTML şablonumuza (template) bir takım CSS eklemeleri yaptığımızı bildirmemiz gerekiyor. `blog/templates/blog/post_list.html` dosyasını açın ve en başına şu satırı ekleyin:

```html
{% load staticfiles %}
```

We're just loading static files here :). Between the `<head>` and `</head>`, after the links to the Bootstrap CSS files add this line:

```html
<link rel="stylesheet" href="{% static 'css/blog.css' %}">
```

The browser reads the files in the order they're given, so we need to make sure this is in the right place. Otherwise the code in our file may override code in Bootstrap files. Az evvel şablonumuza (template) CSS dosyamızın nerede olduğunu söylemiş olduk.

Dosyanız şu şekilde gözüküyor olmalı:

```html
{% load staticfiles %}
<html>
    <head>
        <title>Django Girls blog</title>
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
        <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css">
        <link rel="stylesheet" href="{% static 'css/blog.css' %}">
    </head>
    <body>
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
    </body>
</html>
```

Tamamdır, dosyayı kaydedip sayfayı yenileyebilirsiniz.

![Şekil 14.2](images/color2.png)

Güzel! Şimdi de sitemizi biraz rahatlatıp sol kenar boşluğunu (margin'i) arttırsak mı? Hadi deneyelim!

```css
body {
    padding-left: 15px;
}
```

Bunu CSS dosyanıza ekleyin, dosyayı kaydedin ve nasıl çalıştığını görelim!

![Şekil 14.3](images/margin2.png)

Belki de başlığımızın yazı tipini özelleştirebiliriz? Aşağıdaki satırı `blog/templates/blog/post_list.html` dosyasının içinde `<head>` bölümüne yapıştırın:

```html
<link href="http://fonts.googleapis.com/css?family=Lobster&subset=latin,latin-ext" rel="stylesheet" type="text/css">
```

Bu satır *Lobster* adlı bir fontu Google Fonts (https://www.google.com/fonts) sitesinden sayfamıza aktarır.

Find the `h1 a` declaration block (the code between braces `{` and `}`) in the CSS file ``blog/static/css/blog.css`.  Now add the line`font-family: 'Lobster';` between the braces, and refresh the page:

```css
h1 a {
    color: #FCA205;
    font-family: 'Lobster';
}
```

![Şekil 14.3](images/font.png)

Harika!

As mentioned above, CSS has a concept of classes. These allow you to name a part of the HTML code and apply styles only to this part, without affecting other parts. This can be super helpful! Maybe you have two divs that are doing something different (like your header and your post). A class can help you make them look different.

Devam edelim ve HTML kodumuzun bir kısmına isim verelim. Başlığı içeren `div`'e `page-header` isimli bir class ekleyelim:

```html
<div class="page-header">
    <h1><a href="/">Django Girls Blog</a></h1>
</div>
```

Şimdi de gönderi metnini içeren `div`'e `post` isimli bir sınıf ekleyelim.

```html
<div class="post">
    <p>published: {{ post.published_date }}</p>
    <h1><a href="">{{ post.title }}</a></h1>
    <p>{{ post.text|linebreaks }}</p>
</div>
```

Şimdi farklı seçicilere (selectors) bildirim (deklarasyon) blokları ekleyeceğiz. `.` ile başlayan seçiciler sınıflara işaret eder. Web'de, aşağıdaki kodu anlamanıza yardımcı olacak pek çok güzel CSS öğreticisi ve açıklama mevcut. Şimdilik sadece bu kodu kopyalayıp `blog/static/css/blog.css` dosyamıza yapıştıralım:

```css
.page-header {
    background-color: #ff9400;
    margin-top: 0;
    padding: 20px 20px 20px 40px;
}

.page-header h1, .page-header h1 a, .page-header h1 a:visited, .page-header h1 a:active {
    color: #ffffff;
    font-size: 36pt;
    text-decoration: none;
}

.content {
    margin-left: 40px;
}

h1, h2, h3, h4 {
    font-family: 'Lobster', cursive;
}

.date {
    color: #828282;
}

.save {
    float: right;
}

.post-form textarea, .post-form input {
    width: 100%;
}

.top-menu, .top-menu:hover, .top-menu:visited {
    color: #ffffff;
    float: right;
    font-size: 26pt;
    margin-right: 20px;
}

.post {
    margin-bottom: 70px;
}

.post h1 a, .post h1 a:visited {
    color: #000000;
}
```

Sonra da blog gönderilerimizi gösteren HTML kodunu sınıf bildirimleriyle saralım. <0>blog/templates/blog/post_list. html</0> dosyasındaki şu kodu atıp:

```html
{% for post in posts %}
    <div class="post">
        <p>published: {{ post.published_date }}</p>
        <h1><a href="">{{ post.title }}</a></h1>
        <p>{{ post.text|linebreaks }}</p>
    </div>
{% endfor %}
```

bununla değiştirelim:

```html
<div class="content container">
    <div class="row">
        <div class="col-md-8">
            {% for post in posts %}
                <div class="post">
                    <div class="date">
                        <p>published: {{ post.published_date }}</p>
                    </div>
                    <h1><a href="">{{ post.title }}</a></h1>
                    <p>{{ post.text|linebreaks }}</p>
                </div>
            {% endfor %}
        </div>
    </div>
</div>
```

Bu dosyaları kaydedin ve web sayfanızı yenileyin.

![Şekil 14.4](images/final.png)

Woohoo! Looks awesome, right? Look at the code we just pasted to find the places where we added classes in the HTML and used them in the CSS. Where would you make the change if you wanted the date to be turquoise?

Don't be afraid to tinker with this CSS a little bit and try to change some things. Playing with the CSS can help you understand what the different things are doing. If you break something, don't worry, you can always undo it!

We really recommend taking this free online [Codeacademy HTML & CSS course](http://www.codecademy.com/tracks/web). It can help you learn all about making your websites prettier with CSS.

Sonraki bölüm için hazır mısınız? :)