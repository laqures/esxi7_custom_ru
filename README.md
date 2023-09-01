# Материалы:
ESXi6.5, драйвер на RAID контроллер Adaptec 6405e и сервер Supermicro X10SLM-F.

# Обзор 
RAID контроллер Adaptec 6405e не поддерживается не одной из версий ESXI, необходимо создать свой ISO образ (аналогично можете использовать свои драйвера).

# Метод
В среде Windows 10 с помощью ESXi-Customizer-PS будет встроен драйвер "vmware-esxi-drivers-scsi-aacraid-600.6.2.1.52040.-1.0.6.2494585.x86_64.vib" с официального хранилища supermicro.

# Установка зависимостей

Устанавливаем Python 3.7.1 с официального сайта.

Заходим в папку с питоном через PowerShell (у меня был такой путь):

```
cd C:\Users\User\AppData\Local\Programs\Python\Python37\Scripts
```
Обновляем pip и устанавливаем зависимости:

```
python -m pip install --upgrade pip
.\pip3.7 install six, lxml, psutil, pyopenssl
```

# Установка VMware PowerCLI

Запуск Powershel ISE от имени адмиистратора и прописываем в скрипт:
1. Добавляем репозиторий в доверенные;
2. Устанавливаем VMware PowerCLI;
3. Игнорирование сертификатов (чтобы не было ошибок).

```
Set-PSRepository -Name PSGallery -InstallationPolicy Trusted
Install-Module VMware.PowerCLI
Set-PowerCLIConfiguration -InvalidCertificateAction Ignore 
```

# Скачать ESXi Customizer (V2.9)

Откройте следующий URL-адрес https://github.com/VFrontDe/ESXi-Customizer-PS.

Загрузите следующий файл https://raw.githubusercontent.com/VFrontDe/ESXi-Customizer-PS/master/ESXi-Customizer-PS.ps1 .

По состоянию на 01 сентября 2023 г. это была версия 2.9.

Файл A: ESXi-Customizer-PS.ps1.

# Создайте каталог C:\esxi7-custom\pkg для хранения файлов.

```
PS C:\Users\User> mkdir C:\esxi7-custom\pkg
```

# Далее есть два способа: получая бандл или оффлайн образ:
# 1 способ создания собственного ISO образа получая бандл.

Создание своего образа в формате .zip:
```
PS C:esxi7-custom\pkg> .\ESXi-Customizer-PS.ps1 -v65 -ozip -pkgDir .\ -NSC
```
Пакуем в ISO:
```
PS C:esxi7-custom\pkg> .\ESXi-Customizer-PS.ps1 -izip .\ESXi-6.5.0-20221004001-standard-customized.zip -NSC
```
# 2 способ создания собственного ISO образа имея оффлайн образ.

Объединение .zip оффлайн образа и драйвера
```
PS C:esxi7-custom\pkg> .\ESXi-Customizer-PS.ps1 -izip .\ESXi670-201912001.zip -pkgDir .\ -outDir C:\esxi7-custom\pkg
```

# Дополнительно:
Во время возни с ESXI (исправление ошибок, ковыряние биоса и т.д.) нашёлся третий способ - через графическую утилиту...
Качаем https://www.v-front.de/p/esxi-customizer.html и делаем исходя из статьи https://clck.ru/35XRzy (сслыка с якорем была бы длинной)
