# Yayına alın!

> **Not** Bir sonraki bölüm ara ara zor gelebilir. Dayanın ve bölümü bitirin; yayına alma, website geliştirme sürecinin önemli bir parçasıdır. Biraz daha uğraşmalı olan websitesini canlıya alma işine eğitmeninizin yardım edebilmesi için bu bölümü tutorial'ın ortasına yerleştirdik. Böylece eğer zaman yetmezse tutorial'ı kendi başınıza bitirebilirsiniz.

Until now, your website was only available on your computer. Now you will learn how to deploy it! Yayına alma uygulamanızı internette yayınlama sürecidir, böylelikle insanlar sonunda gidip uygulamanızı görebilirler :).

Öğrendiğimiz üzere, bir websitesi bir sunucunun üstünde olmalıdır. Internette birçok sunucu sağlayıcı bulunuyor. Görece kolay bir yayına alma süreci olanlardan birini kullanacağız: [PythonAnywhere](http://pythonanywhere.com/). PythonAnymwhere çok fazla ziyaretçisi olmayan ufak uygulamalar için ücretsiz yani sizin için kesinlikle yeterli olacaktır.

Dışarıdan kullanacağımız diğer servis bir kod barındırma hizmeti olan [Github](http://www.github.com). Başkaları da var, ama nerdeyse her programcının bir Github hesabı var, sizin de olacak!

These three places will be important to you. Your local computer will be the place where you do development and testing. When you're happy with the changes, you will place a copy of your program on GitHub. Your website will be on PythonAnywhere and you will update it by getting a new copy of your code from GitHub.

# Git

Git, birçok programcı tarafından kullanılan bir "sürüm kontrol sistemi"dir. Bu yazılım dosyaların zaman içindeki değişimlerini izler, böylelikle sonradan eski sürümlere ulaşabilirsiniz. Biraz Microsoft Word'deki "değişiklikleri izle" özelliği gibi, ama çok daha güçlü.

## Git'i kurmak

> **Not** Kurulum adımlarını halihazırda yaptıysanız, bunu tekrar yapmanıza gerek yok - bir sonraki alt bölüme geçip Git reponuzu oluşturabilirsiniz.

{% include "/deploy/install_git.md" %}

## Git repomuzu oluşturmak

Git, kod reposu (veya "repository") denen belli dosyaların değişikliklerini izler. Projemiz için bir tane oluşturalım. Konsolunuzu açın ve `djangogirls` klasöründe aşağıdaki komutları çalıştırın:

> **Not** Reponuzu başlatmadan önce, `pwd` (OS/Linux) veya `cd` (Windows) komutu ile bulunduğunuz dizini kontrol edin. `djangogirls` dizininde olmanız gerekiyor.

    Hatırlatma: Kullanıcı adı seçerken özel Türkçe karakter kullanmayın.
    $ git init 
    Initialized empty Git repository in ~/djangogirls/.git/ 
    $ git config --global user.name "Adınız" 
    $ git config --global user.email you@example.com
    

Git reposunu başlatma işi, proje başına bir kere yapmamız gereken birşey (ayrıca kullanıcı adı ve eposta adresini tekrar girmenize gerek olmayacak).

Git bu dizindeki tüm dizin ve dosyalardaki değişiklikleri kaydedecek, ama takip etmemesini istediğimiz bazı dosyalar var. Bunu dizinin dibinde `.gitignore` adında bir dosya oluşturarak yapıyoruz. Editörünüzü açın ve aşağıdaki içeriklerle yeni bir dosya yaratın:

    *.pyc
    __pycache__
    myvenv
    db.sqlite3
    /static
    .DS_Store
    

And save it as `.gitignore` in the "djangogirls" folder.

> **Not** Dosya adının başındaki nokta önemli! Eğer dosyayı oluştururken zorlanırsanız (örneğin Mac'ler Finder ile nokta ile başlayan dosya yaratmanızdan hoşlanmıyor), editörünüzdeki "Farklı Kaydet" özelliğini kullanın, kesin çalışır.
> 
> **Note** One of the files you specified in your`.gitignore` file is `db.sqlite3`. That file is your local database, where all or your posts are stored. We don't want to add this to your repository, because your website on PythonAnywhere is going to be using a different database. That database could be SQLite, like your development machine, but usually, you will use one called MySQL which can deal with a lot more site visitors than SQLite. Either way, by ignoring your SQLite database for the GitHub copy, it means that all of the posts you created so far are going to stay and only be available locally, but you're gonna have to add them again on production. You should think of your local database as a good playground where you can test different things and not be afraid that you're going to delete your real posts from your blog.

`git add` kullanmadan önce veya nelerin değiştiğinden emin değilseniz, `git status` komutunu kullanmakta yarar var. Bu, yanlış dosyaların eklenmesi ve gönderilmesi gibi istenmeyen sürprizlerin engelenmesine yardımcı olacak. `git status` komutu, takip edilmeyen/değişen/gönderilecek dosyalar (staged), dal durumu (branch status) gibi bilgiler verir. Çıktının aşağıdaki gibi olması gerekiyor:

    $ git status 
    On branch master 
    
    Initial commit
    
    Untracked files:
      (use "git add <dosya>..." to include in what will be committed)
    
             .gitignore
             blog/
             manage.py
             mysite/ 
    
    nothing added to commit but untracked files present (use "git add" to track)
    

Ve son olarak değişikliklerimizi kaydediyoruz. Komut satırına gidin ve aşağıdaki komutları çalıştırın:

    $ git add --all .
    $ git commit -m "My Django Girls app, first commit"
     [...]
     13 files changed, 200 insertions(+)
     create mode 100644 .gitignore
     [...]
     create mode 100644 mysite/wsgi.py
    

## Kodunuzu Github'a gönderme

[GitHub.com>](http://www.github.com) adresine gidin ve kendinize yeni bedava bir kullanıcı hesabı açın. (Bunu atölye hazırlıklarında zaten yaptıysanız, harika!)

Arkasından "ilk-blogum" (veya "my-first-blog") isminde yeni bir repo oluşturun. "Initialize with a README" kutucuğunu işaretlemeden bırakın, .gitignore opsiyonunu boş bırakın (onu elle yaptık) ve 'License'ı 'None' olarak bırakın.

![](images/new_github_repo.png)

> **Not** `ilk-blogum` ismi önemli -- başka birşey de seçebilirsiniz, ama aşağıdaki yönergelerde çok geçiyor, her seferinde değiştirmeniz gerekir. En kolayı `ilk-blogum` ismi ile devam etmek.

Bir sonraki ekranda, repo'yu klonlamak için gereken URL'yi göreceksiniz. "HTTPS"li versiyonunu seçin, kopyalayın. Birazdan onu komut penceresine yapıştıracağız:

![](images/github_get_repo_url_screenshot.png)

Şimdi bilgisayarınızdaki Git reposunu Github'daki repo ile ilişkilendirmemiz gerekiyor.

Aşağıdakini komut satırına yazın (`<github-kullanıcı-adınız>` kısmını Github hesabını yarattığınız sırada kullandığınız kullanıcı adı ile değiştirin, büyüktür küçüktür işaretlini eklemeyin):

    $ git remote add origin https://github.com/<github-kullanıcı-adınız>/ilk-blogum.git 
    $ git push -u origin master
    

Github kullanıcı adı ve şifrenizi girin, arkasından aşağıdakine benzer bir şey görmeniz gerekiyor:

    Username for 'https://github.com': zeynep 
    Password for 'https://zeynep@github.com': 
    Counting objects: 6, done.
    Writing objects: 100% (6/6), 200 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0) 
    To https://github.com/zeynep/my-first-blog.git 
     * [new branch] master -> master 
    Branch master set up to track remote branch master from origin.
    

<!--TODO: maybe do ssh keys installs in install party, and point ppl who dont have it to an extention -->

Kodunuz artık Github'da. Siteye girin ve kontrol edin! İyi bir çevrede olduğunu göreceksiniz - [Django](https://github.com/django/django), the [Django Girls Tutorial](https://github.com/DjangoGirls/tutorial), ve daha birçok harika açık kaynak yazılım projesi de kodlarını Github'da tutuyor :)

# Blogumuzun PythonAnywhere üzerinde kurulumu

> **Not** En baştaki kurulum adımlarında PythonAnywhere hesabını açmış olabilirsiniz - öyleyse bu kısmı tekrar yapmanıza gerek yok.

{% include "/deploy/signup_pythonanywhere.md" %}

## Kodunuzu PythonAnywhere üzerine çekmek

PythonAnywhere'de hesap açtığınızda, 'dashboard' (gösterge paneli) sayfanıza veya "Consoles" sayfasına yönlendirileceksiniz. "Bash" konsolu başlatma seçeneğini seçin -- bu bilgisayarınızdaki konsolun PythonAnywhere versiyonu.

> **Not** PythonAnywhere Linux tabanlı, o yüzden kendi makinanız Windows ise konsol biraz farklı gözükecektir.

Reponuzun bir klonunu yaratarak kodumuzu Github'dan PythonAnywhere üzerine çekelim. Aşağıdakileri PythonAnywhere konsoluna yazın (`<github-kullanıcı-adınız>` yerine kendi Github kullanıcı adınızı yazmayı unutmayın):

    $ git clone https://github.com/<github-kullanıcı-adınız>/ilk-blogum.git
    

Bu kodunuzun bir kopyasını PythonAnywhere üzerine indirecektir. `tree ilk-blogum` yazarak kontrol edin:

    $ tree ilk-blogum
    ilk-blogum/
    ├── blog
    │   ├── __init__.py
    │   ├── admin.py
    │   ├── migrations
    │   │   ├── 0001_initial.py
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   └── views.py
    ├── manage.py
    └── mysite
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
    

### PythonAnywhere üzerine bir virtualenv (sanal ortam) oluşturmak

Bilgisayarınızda nasıl bir virtualenv (sanal ortam) oluşturduysanız, aynı şekilde PythonAnywhere üzerinde de oluşturabilirsiniz. Bash konsoluna, aşağıdakileri yazın:

    $ cd ilk-blogum
    
    $ virtualenv --python=python3.4 myvenv
    Running virtualenv with interpreter /usr/bin/python3.4
    [...]
    Installing setuptools, pip...done.
    
    $ source myvenv/bin/activate
    
    (myvenv) $  pip install django~=1.9.0
    Collecting django
    [...]
    Successfully installed django-1.9
    

> **Not** `pip install` birkaç dakika sürebilir. Sabır, sabır! Ama 5 dakikadan uzun sürüyorsa, birşeyler yanlış olmuştur. Eğitmeninize sorun.

<!--TODO: think about using requirements.txt instead of pip install.-->

### PythonAnywhere üzerinde veritabanının oluşturulması

Bilgisayarınız ve sunucu arasında farklı olan bir başka şey daha: farklı bir veritabanı kullanıyor. Dolayısıyla bilgisayarınızdaki ve sunucudaki kullanıcı hesapları ve blog yazıları farklı olabilir.

Sunucudaki veritabanına aynen bilgisayardaki gibi `migrate` (taşımak) ve `createsuperuser` (yetkili bir kullanıcı oluşturmak) komutlarıyla oluşturup ilk örnek verilerle ile doldurabiliriz:

    (myvenv) $ python manage.py migrate
    Operations to perform:
    [...]
      Applying sessions.0001_initial... OK
    
    
    (myvenv) $ python manage.py createsuperuser
    

## Blog'umuzu web uygulaması olarak yayınlama

Now our code is on PythonAnywhere, our virtualenv is ready, and the database is initialised. We're ready to publish it as a web app!

PythonAnywhere logosuna tıklayarak 'dashboard'a geri gidin, burda **Web** sekmesine tıklayın. En son **Add a new web app** (yeni bir web uygulaması yaratın) linkine tıklayın.

Açılan pencerede alan adınızı kontrol edin, **manual configuration** (elle konfigürasyon)'ı seçin ("Django" opsiyonunu *değil*). Arkasından **Python 3.4**'ü seçin ve işlemi bitirmek için 'Next'e basın.

> **Not** "Manual configuration" seçeneğini seçtiğinizden emin olun, "Django" seçeneğini değil. Hazır PythonAnywhere Django kurulumunu seçmek için fazla havalıyız ;-)

### virtualenv'in (sanal ortamın) ayarlanması

Son bahsettiğimiz adım sizi web uygulamanızın PythonAnywhere ayar ekranına getirecek. Sunucudaki uygulamanızda değişiklik yapmak istediğinizde bu ekranı kullanmanız gerekiyor.

![](images/pythonanywhere_web_tab_virtualenv.png)

"Virtualenv" bölümünde "Enter the path to a virtualenv" (virtualenv için dizin yolu girin) linkini tıklayın ve şunu yazın: `/home/<kullanıcı-adınız>/ilk-blogum/myvenv`. Devam etmeden önce, dizin yolunu kaydetmek için tik işareti olan mavi kutuyu tıklayın.

> **Not** İlgili yeri kendi kullanıcı adınızı yazın. Eğer hata yaparsanız, PythonAnywhere size bir uyarı gösterecektir.

### WSGI dosyasının ayarlanması

Django, "WSGI protokolü"nü kullanarak çalışır. WSGI, PythonAnywhere'in de desteklediği Python kullanan websitelerinin servis edilmesi için kullanılan bir standart. PythonAnywhere'in Django blogumuzu anlaması için WSGI ayar dosyasını düzenleyiyoruz.

"WSGI Configuration file" (WSGI ayar dosyası) linkine tıklayın ("Code" denen kısımda, sayfanın üst tarafında -- adı `/var/www/<kullanıcı-adınız>_pythonanywhere_com_wsgi.py`'a benzer birşey olacak). Burdan bir edöitöre yönlendirileceksiniz.

Tüm içeriği silin ve onların yerine aşağıdakileri yazın:

```python
import os
import sys

path = '/home/<your-username>/my-first-blog'  # use your own username here
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'

from django.core.wsgi import get_wsgi_application
from django.contrib.staticfiles.handlers import StaticFilesHandler
application = StaticFilesHandler(get_wsgi_application())
```

> **Note** Don't forget to substitute in your own username where it says `<your-username>` **Note** In line three, we make sure Python anywhere knows how to find our application. It is very important that this path name is correct, and especially that there are no extra spaces here. Otherwise you will see an "ImportError" in the error log.

This file's job is to tell PythonAnywhere where our web app lives and what the Django settings file's name is.

The `StaticFilesHandler` is for dealing with our CSS. This is taken care of automatically for you during local development by the `runserver` command. Tutorial'ın ilerleyen kısımlarında sitemizin CSS'ini düzenlerken statik dosyalar konusuna biraz daha fazla gireceğiz.

**Save** (kaydet)'e basın. Arkasından **Web** sekmesine geri gidin.

Hazırız! Yeşil ve büyük **Reload** (Yeniden yükle) butonuna tıklayın. Uygulamanıza girip görebileceksiniz. Sayfanın tepesinde uygulamaya giden linki bulabilirsiniz.

## Hata ayıklama önerileri

Eğer sitenize girdiğinizde bir hata görürseniz, hata ayıklama bilgileri için ilk bakacağınız yer, **error log**'unuz (hata kayıtlarınız). Buraya olan linki PythonAnywhere'deki [Web sekme](https://www.pythonanywhere.com/web_app_setup/)sinde bulabilirsiniz. Burda hata mesajları olup olmadığına bakın; en yeni mesajlar en altta. Sık karşılaşılan problemler şunlar:

- Forgetting one of the steps we did in the console: creating the virtualenv, activating it, installing Django into it, migrating the database.

- Web sekmesinde virtualenv dizin yolunda bir hata yapılması -- eğer problem varsa, ilgili yerde küçük kırmızı bir hata mesajı olur.

- WSGI ayar dosyasına bir hata yapmak -- ilk-blogum dizinine olan yolu doğru yazdığınızdan emin misiniz?

- Virtualenv'ınız için seçtiğiniz Python versiyonu web uygulamanız için seçtiğiniz Python versiyonu ile aynı mı? İkisinin de 3.4 olması gerekiyor.

- [Python Anywhere vikisinde (bilgi sayfalarında) genel hata ayıklama önerileri](https://www.pythonanywhere.com/wiki/DebuggingImportError) bulunuyor.

Ve eğitmeniniz size yardıma hazır, unutmayın!

# Siteniz canlıda!

Sitenizin varsayılan sayfası "Welcome to Django" diyor olmalı, aynen bilgisayarınızda olduğu gibi. URL'nin sonuna `/admin/` yazın, 'giriş' tuşuna bastığınızda admin sitesi açılacak. Kullanıcı adı ve şifrenizle giriş yapın, sunucuda yeni blog yazıları girebildiğinizi göreceksiniz.

Once you have a few posts created, you can go back to your local setup (not PythonAnywhere). From here you should work on your local setup to make changes. This is a common workflow in Web development (make changes locally, push those changes to GitHub, pull your changes down to your live Web server). This allows you to work and experiment without breaking your live Web site. Pretty cool, huh?

Kendinize *KOCAMAN* bir aferin diyin! Yayına alma web geliştirme işinin en uğraştırmalı kısımlarından biridir ve genelde çalışana kadar insanların birkaç gününü alır. Ama işte siteniz canlıda, gerçek internette!