# Django views - yaratma zamanı geldi!

Evvelki bölümde yaptığımız hatayı yok edelim :)

*view*, "uygulama mantığının" ifade edildiği yerdir. Daha önce oluşturulan `model` den bilgi alıp `template`'e iletir. Gelecek bölümde bir template oluşturacağız. Views are just Python functions that are a little bit more complicated than the ones we wrote in the **Introduction to Python** chapter.

View'ler `views.py` doyasına yazılır. Şimdi, `blog/views.py` dosyasına *view* ekleyelim.

## blog/views.py

Dosyayı açıp inceleyelim:

```python
from django.shortcuts import render

# Create your views here.
```

Not too much stuff here yet.

`#` ile başlayan satırlar yorum satırlarıdır - bu satırlar Python tarafından çalıştırılmayacak manasına gelir. Çok pratik, değil mi?

The simplest *view* can look like this.

```python
def post_list(request):
    return render(request, 'blog/post_list.html', {})
```

As you can see, we created a function (`def`) called `post_list` that takes `request` and `return` a function `render` that will render (put together) our template `blog/post_list.html`.

Dosyamızı kaydedelim ve http://127.0.0.1:8000/ e gidip bakalım.

Yine hata! Okuyup anlamaya çalışalım:

![Hata](images/error.png)

This shows that the server is running again, at least, but it still doesn't look right, does it? Don't worry, it's just an error page, nothing to be scared of! Just like the error messages in the console, these are actually pretty useful. You can read that the *TemplateDoesNotExist*. Let's fix this bug and create a template in the next chapter!

> Learn more about Django views by reading the official documentation: https://docs.djangoproject.com/en/1.9/topics/http/views/