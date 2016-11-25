> For readers at home: this chapter is covered in [Installing Python & Code Editor](https://www.youtube.com/watch?v=pVTaqzKZCdA) video.
> 
> This section is based on a tutorial by Geek Girls Carrots (https://github.com/ggcarrots/django-carrots)

Django, Python ile yazılmıştır. Django ile bir şey yapmak için Python diline ihtiyacımız var. Hadi Python kurmaya başlayalım! Biz Python 3.4 kurmak istiyoruz, eğer daha düşük bir sürüme sahipseniz, güncellemelisiniz.

### Windows

Windows için Python indirmek için resmi siteyi ziyaret edebilirsiniz: https://www.python.org/downloads/release/python-343/. ***.msi** dosyasını indirdikten sonra, dosyayı çalıştırın (çift-tık) ve yönergeleri izleyin. Python kurulumunu yaptığınız dizinin yolunu unutmamanız önemli. Daha sonra lazım olacak!

One thing to watch out for: on the second screen of the installation wizard, marked "Customize", make sure you scroll down to the "Add python.exe to the Path" option and select "Will be installed on local hard drive", as shown here:

![Python'u arama yoluna eklemeyi unutmayın](../python_installation/images/add_python_to_windows_path.png)

### GNU/Linux

Muhtemelen sisteminizde Python zaten yüklüdür. Yüklü olup olmadığını (ya da hangi versiyon olduğunu) kontrol etmek için komut satırını açın ve aşağıdaki komutları girin: 

    $ python3 --version
    Python 3.4.3
    

If you have a different 'micro version' of Python installed, e.g. 3.4.0, then you don't have to upgrade. Python yüklü değilse ya da farklı bir versiyon edinmek istiyorsanız aşağıdaki adımları takip edin:

#### Debian veya Ubuntu

Terminale bu komutu girin:

    $ sudo apt-get install python3.4
    

#### Fedora (21'e kadar)

Terminalde kullanmanız gereken komut:

    $ sudo yum install python3
    

#### Fedora (22+)

Terminalde kullanmanız gereken komut:

    $ sudo dnf install python3
    

### OS X

Python kurulum dosyasını indirmek için resmi siteye gitmelisiniz: https://www.python.org/downloads/release/python-342/:

* *Mac OS X 64-bit/32-bit yükleyici* dosyasını indirin,
* *python-3.4.3-macosx10.6.pkg* dosyasına çift tıklayarak yükleyiciyi çalıştırın.

Kurulumun başarılı olup olmadığını kontrol etmek için *Terminal* uygulamasını açın ve aşağıdaki `python3` komutunu çalıştırın:

    $ python3 --version
    Python 3.4.3
    

* * *

Herhangi bir şüpheniz varsa, kurulumda bir şeyler ters gittiyse ya da sonrasında ne yapacağınızı bilmiyorsanız eğitmene sorabilirsiniz! Bazen işler düzgün gitmiyor, bu durumda daha fazla deneyime sahip birinden yardım istemelisiniz.