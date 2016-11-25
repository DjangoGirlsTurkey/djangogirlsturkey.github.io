# Template içerisinde dinamik veri

Birkaç parçayı yerine oturttuk: `Post` (gönderi) modelini `models.py`'de tanımladık, `views.py`'de `post_list` (gönderi listesi) var ve template ekledik. Ama gönderilerimizi HTML'de görünür kıldık mı? Because that is what we want to do. Take some content (models saved in the database) and display it nicely in our template, right?

Bu tam olarak *view*'lerin yapmasını beklediğimiz şey: modelleri ve template'leri bağlamak. `post_list` *view*'de göstermek istediğimiz modelleri alıp template'e iletmemiz gerekecek. In a *view* we decide what (model) will be displayed in a template.

Peki, bunu nasıl yapacağız?

`blog/views.py`'ı açacağız. Şu anda `post_list` *view*ü şöyle:

```python
from django.shortcuts import render

def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

Kodları farklı dosyalara eklemekten bahsettiğimizi hatırlıyor musunuz? Şimdi `models.py`'de yazdığımız modeli ekleme zamanı. `from .models import Post` satırını şu şekilde ekleyeceğiz:

```python
from django.shortcuts import render
from .models import Post
```

The dot before `models` means *current directory* or *current application*. Both `views.py` and `models.py` are in the same directory. This means we can use `.` and the name of the file (without `.py`). Arkasından modelin adını (`Post`)'u dahil ediyoruz).

Sırada ne var? `Post` modelinden gönderileri almamız için `QuerySet` dediğimiz bir şeye ihtiyacımız var.

## QuerySet (Sorgu Seti)

QuerySet'in nasıl çalıştığı konusunda bir fikriniz oluşmuştur. [Django ORM (QuerySets) bölümü](../django_orm/README.md)nde konuşmuştuk.

So now we want published blog posts sorted by `published_date`, right? We already did that in QuerySets chapter!

    Post.objects.filter(yayinlama_tarihi__lte=timezone.now()).order_by('yayinlama_tarihi')
    

Şimdi bu kodu `blog/views.py` dosyasında `def post_list(request)` fonksiyonuna ekleyelim:

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {})
```

QuerySet'imiz için bir *değişken* yarattığımıza dikkat edin: `posts`. Bu QuerySet'in ismi. Bundan sonra ondan ismi ile bahsedebiliriz.

Bu kod `timezone.now()` fonksiyonunu kullanıyor, dolayısıyla `timezone` için bir 'import' eklememiz gerekiyor.

The last missing part is passing the `posts` QuerySet to the template. Don't worry we will cover how to display it in a next chapter.

`render` fonksiyonunda halihazırda `request` diye bir parametremiz var (dolayısıyla Internet üzerinden kullanıcı ile ilgili aldığımız her şey) ve bir de template dosyamız `'blog/post_list.html'` var. `{}` şeklindeki son parametremiz, template içinde kullanılmak üzere bir şeyler ekleyebileceğimiz bir değişken. Bunlara isimler vermemiz gerekiyor (`'posts'` ismini kullanmaya devam edeceğiz şimdilik :)). Şöyle olması lazım: `{'posts': posts}`. `:`'dan önceki kısmın bir string olduğuna dikkat edin; etrafına tek tırnak koymanız gerekiyor `''`.

Nihayetinde `blog/views.py` şu şekle gelmiş olmalı:

```python
from django.shortcuts import render
from django.utils import timezone
from .models import Post

def post_list(request):
    posts = Post.objects.filter(published_date__lte=timezone.now()).order_by('published_date')
    return render(request, 'blog/post_list.html', {'posts': posts})
```

İşte bu kadar! Template'e geri gidip QuerySet'leri görünür hale getirme zamanı!

Want to read a little bit more about QuerySets in Django? You should look here: https://docs.djangoproject.com/en/1.9/ref/models/querysets/