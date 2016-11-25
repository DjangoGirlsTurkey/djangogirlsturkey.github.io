> Part of this section is based on tutorials by Geek Girls Carrots (https://github.com/ggcarrots/django-carrots).
> 
> Bu bölümün bir parçası Creative Commons Attribution-ShareAlike 4.0 International License ile lisanslı [django-marcador tutorial](http://django-marcador.keimlink.de/)'a dayanılarak hazırlanmıştır. Django-marcador tutorial'ının hakları Markus Zapke-Gründemann'e aittir.

## Virtual environment (Sanal ortam)

Django'yu yüklemeden önce kod ortamınızı düzenli tutmak için son derece yararlı bir araç yükleyeceğiz. Bu adımı atlayabilirsiniz, fakat atlamamanızı tavsiye ederiz. En iyi olası kurulum ile başlamanız sizi gelecekteki bir sürü sorundan koruyacaktır!

Öyleyse bir **virtual environment**(diğer adıyla *virtualenv*) kuralım. Virtualenv Python/Django kurulumunuzu her proje için ayrı tutup izole eder. Bu, bir websitesine yapacağınız değişikliklerin diğer geliştirdiklerinize yansımayacağı anlamına gelir. Muazzam, değil mi?

Yapmanız gereken tek şey `virtualenv` oluşturmak için bir dizin bulmak; örneğin giriş dizininiz. Windows'da bu `C:\Users\isim` olabilir (`isim` kısmı kullanıcı adınız olacak şekilde).

Bu eğitim için giriş dizininizde yeni açtığımız `djangogirls` adlı bir klasör kullanacağız:

    $ mkdir djangogirls
    $ cd djangogirls
    

`myvenv` adında bir virtualenv oluşturacağız. Genel komut aşağıdaki şekilde olacak:

    $ python3 -m venv myvenv
    

### Windows

Yeni bir `virtualenv` oluşturmak için konsolu açıp (nasıl yapıldığını birkaç adım önce anlatmıştık - hatırlıyorsunuz değil mi?) `C:\Python34\python -m venv myvenv` komutunu çalıştırın. Şöyle görünmeli:

    C:\Users\İsim\djangogirls> C:\Python34\python -m venv myvenv
    

`C:\Python34\python` dizini önceden Python'u kurduğunuz dizin ve `myenv` ise `virtualenv`'inizin ismi olacaktır. İstediğiniz herhangi bir ismi kullanabilirsiniz, ama küçük harfle yazılmasına ve boşluk, aksan karakterleri (örn: å) ve özel karakterleri kullanmamaya dikkat edin. Ayrıca ismi kısa tutmak iyi bir fikir olabilir, zira bu ismi çok kullanıyor olacaksınız!

### GNU/Linux ve OS X

Linux ve OS X işletim sistemlerinde `virtualenv` oluşturmak için `python3 -m venv myvenv` komutunu çalıştırmak yeter. Komut şu şekilde görünecektir:

    $ python3 -m venv myvenv
    

Burada `myenv` sizin `virtualenv`'inizin ismi. Dilerseniz istediğiniz herhangi bir isim kullanabilirsiniz, ama büyük harf ve boşluk kullanmamaya dikkat edin. İsmi çok fazla kullanacağınız için kısa tutmak da işinize yarayacaktır.

> **NOTE:** On some versions of Debian/Ubuntu you may receive the following error
> 
>     The virtual environment was not created successfully because ensurepip is not available.  On Debian/Ubuntu systems, you need to install the python3-venv package using the following command.
>       apt-get install python3-venv
>     You may need to use sudo with that command.  After installing the python3-venv package, recreate your virtual environment.
>     
> 
> In this case, follow the instructions above and install the `python3-venv` package:
> 
>     $ sudo apt-get install python3-venv
>     
> 
> **NOT:** Ubuntu 14.04 işletim sisteminde sanal ortam yaratmaya çalışırken şu hatayla karşılaşabilirsiniz:
> 
>     Error: Command '['/home/zeynep/Slask/tmp/venv/bin/python3', '-Im', 'ensurepip', '--upgrade', '--default-pip']' returned non-zero exit status 1
>     
> 
> Bu sorunu çözmek için `virtualenv` komutunu kullanabilirsiniz.
> 
>     $ sudo apt-get install python-virtualenv
>     $ virtualenv --python=python3.4 myvenv
>     

## Virtualenv ile çalışmak

Yukarıdaki komutlar `myvenv` (veya seçtiğiniz isimde) bir klasör oluşturacaktır. Bu klasörde birçok dosya ve klasör bulunur.

#### Windows

Şu komutu çalıştırarak virtualenv'i başlatın:

    C:\Users\Name\djangogirls> myvenv\Scripts\activate
    

#### GNU/Linux ve OS X

Şu komutu çalıştırarak virtualenv'i başlatın:

    $ source myvenv/bin/activate
    

`myvenv` yerine kendi seçtiğiniz `virtualenv` ismini koymayı unutmayın!

> **NOT:** bazen `source` komutu kullanılamıyor durumda olabilir. Böyle durumlarda onun yerine aşağıdaki komutu da kullanabilirsiniz:
> 
>     $ . myvenv/bin/activate
>     

You will know that you have `virtualenv` started when you see that the prompt in your console is prefixed with `(myvenv)`.

Virtualenv ile çalışırken `python` otomatik olarak doğru sürümü çalıştıracaktır. Yani `python3` yerine `python` yazabilirsiniz.

Artık bütün gerekli uygulamaları bir araya getirdiğimize göre sonunda Django'yu yükleyebiliriz!

## Django'yu yüklemek

Now that you have your `virtualenv` started, you can install Django using `pip`. In the console, run `pip install django~=1.9.0` (note that we use a tilde followed by an equal sign: `~=`).

    (myvenv) ~$ pip install django~=1.9.0
    Downloading/unpacking django==1.9
    Installing collected packages: django
    Successfully installed django
    Cleaning up...
    

Windows'ta

> If you get an error when calling pip on Windows platform please check if your project pathname contains spaces, accents or special characters (for example, `C:\Users\User Name\djangogirls`). If it does please consider using another place without spaces, accents or special characters (suggestion is: `C:\djangogirls`). Create a new virtualenv in the new directory, then delete the old one and try the above command again. (Moving the virtualenv directory won't work since virtualenv uses absolute paths.)

on Windows 8 and Windows 10

> Your command line might freeze after when you try to install Django. If this happens, instead of the above command use:
> 
>     C:\Users\Name\djangogirls> python -m pip install django~=1.9.0
>     

Linux'ta

> Eğer Ubuntu 12.04 işletim sisteminde pip komutunu çağırırken bir hata iletisiyle karşılaştıysanız `python -m pip install -U --force-reinstall pip` komutunu çalıştırarak pip kurulumunu onarmayı deneyin.

İşte bu kadar! Sonunda Django uygulamanızı oluşturmaya hazırsınız!