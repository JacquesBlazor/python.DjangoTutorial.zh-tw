開始[¶](#getting-started "永久連結至標題")
==========================================

初識 Django？或為了 Web 開發？好，那你來對地方了：看看這些資料快速上手。

-   [初識
    Django](https://docs.djangoproject.com/zh-hans/3.0/intro/overview/)
-   [快速安裝指南](https://docs.djangoproject.com/zh-hans/3.0/intro/install/)
-   [編寫你的第一個 Django 應用，第 1
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
-   [編寫你的第一個 Django 應用，第 2
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
-   [編寫你的第一個 Django 應用，第 3
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
-   [編寫你的第一個 Django 應用，第 4
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)
-   [編寫你的第一個 Django 應用，第 5
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial05/)
-   [編寫你的第一個 Django 應用，第 6
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial06/)
-   [編寫你的第一個 Django 應用，第 7
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial07/)
-   [進階指南：如何編寫可重用程式](https://docs.djangoproject.com/zh-hans/3.0/intro/reusable-apps/)
-   [下一步看什麼](https://docs.djangoproject.com/zh-hans/3.0/intro/whatsnext/)
-   [編寫你的第一個 Django
    修補程式](https://docs.djangoproject.com/zh-hans/3.0/intro/contributing/)

參見

如果你初次了解
[Python](https://python.org/)，可能想知道它是怎樣一門語言。Django 是
100% 由 Python 編寫的，所以熟悉 Python 可以加深對 Django 的理解。

如果你毫無編程經驗，可能需要從 [適合零編程經驗者的 Python
學習資源](https://wiki.python.org/moin/BeginnersGuide/NonProgrammers)
起步

如果你已經了解過一門其他的編程語言，並且向快速上手Python，我們向你推薦
[Dive Into
Python](https://diveinto.org/python3/table-of-contents.html)。如果不太適合你的話，還有其它很多
[books about Python](https://wiki.python.org/moin/PythonBooks)。

[** Django 文件](https://docs.djangoproject.com/zh-hans/3.0/)

[初識 Django
**](https://docs.djangoproject.com/zh-hans/3.0/intro/overview/)

初識 Django[¶](#django-at-a-glance "永久連結至標題")
====================================================

Django
最初被設計用於具有快速開發需求的新聞類網站，目的是要實現簡單快捷的網站開發。以下內容簡要介紹了如何使用
Django 實現一個資料庫驅動的 Web 應用。

為了讓您充分理解 Django
的工作原理，這份文件為您詳細描述了相關的技術細節，不過這並不是一份入門教學或者是參考文件（我們當然也為您準備了這些）。如果您想要馬上開始一個專案，可以從
[實例教學](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
開始入手，或者直接開始閱讀詳細的
[參考文件](https://docs.djangoproject.com/zh-hans/3.0/topics/) 。

設計模型[¶](#design-your-model "永久連結至標題")
------------------------------------------------

Django 無需資料庫就可以使用，它提供了
[物件關聯映射器](https://en.wikipedia.org/wiki/Object-relational_mapping)
透過此技術，你可以使用 Python 程式來描述資料庫結構。

你可以使用強大的
[資料-模型語句](https://docs.djangoproject.com/zh-hans/3.0/topics/db/models/)
來描述你的資料模型，這解決了數年以來在資料庫模式中的難題。以下是一個簡明的例子：

mysite/news/models.py[¶](#id1 "永久連結至程式")**

    from django.db import models

    class Reporter(models.Model):
        full_name = models.CharField(max_length=70)

        def __str__(self):
            return self.full_name

    class Article(models.Model):
        pub_date = models.DateField()
        headline = models.CharField(max_length=200)
        content = models.TextField()
        reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

        def __str__(self):
            return self.headline

應用資料模型[¶](#install-it "永久連結至標題")
---------------------------------------------

接下來，執行Django命令欄實用程式以自動建立資料庫表：

/ 

    $ python manage.py makemigrations
    $ python manage.py migrate

該 [`makemigrations`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-makemigrations)
命令查找所有可用的models，為任意一個在資料庫中不存在對應資料表的model建立
migrations 腳本文件。[`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令則執行這些 migrations 自動建立資料庫表。還提供可選的
[更豐富的控制模式](https://docs.djangoproject.com/zh-hans/3.0/topics/migrations/)。

享用便捷的 API[¶](#enjoy-the-free-api "永久連結至標題")
-------------------------------------------------------

接下來，你就可以使用一套便捷而豐富的 [Python
API](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/)
開啟你的資料。API是動態建立的，不需要程式產生：

    # Import the models we created from our "news" app
    >>> from news.models import Article, Reporter

    # No reporters are in the system yet.
    >>> Reporter.objects.all()
    <QuerySet []>

    # Create a new Reporter.
    >>> r = Reporter(full_name='John Smith')

    # Save the object into the database. You have to call save() explicitly.
    >>> r.save()

    # Now it has an ID.
    >>> r.id
    1

    # Now the new reporter is in the database.
    >>> Reporter.objects.all()
    <QuerySet [<Reporter: John Smith>]>

    # Fields are represented as attributes on the Python object.
    >>> r.full_name
    'John Smith'

    # Django provides a rich database lookup API.
    >>> Reporter.objects.get(id=1)
    <Reporter: John Smith>
    >>> Reporter.objects.get(full_name__startswith='John')
    <Reporter: John Smith>
    >>> Reporter.objects.get(full_name__contains='mith')
    <Reporter: John Smith>
    >>> Reporter.objects.get(id=2)
    Traceback (most recent call last):
        ...
    DoesNotExist: Reporter matching query does not exist.

    # Create an article.
    >>> from datetime import date
    >>> a = Article(pub_date=date.today(), headline='Django is cool',
    ...     content='Yeah.', reporter=r)
    >>> a.save()

    # Now the article is in the database.
    >>> Article.objects.all()
    <QuerySet [<Article: Django is cool>]>

    # Article objects get API access to related Reporter objects.
    >>> r = a.reporter
    >>> r.full_name
    'John Smith'

    # And vice versa: Reporter objects get API access to Article objects.
    >>> r.article_set.all()
    <QuerySet [<Article: Django is cool>]>

    # The API follows relationships as far as you need, performing efficient
    # JOINs for you behind the scenes.
    # This finds all articles by a reporter whose name starts with "John".
    >>> Article.objects.filter(reporter__full_name__startswith='John')
    <QuerySet [<Article: Django is cool>]>

    # Change an object by altering its attributes and calling save().
    >>> r.full_name = 'Billy Goat'
    >>> r.save()

    # Delete an object with delete().
    >>> r.delete()

一個動態管理接口：並非徒有其表[¶](#a-dynamic-admin-interface-it-s-not-just-scaffolding-it-s-the-whole-house "永久連結至標題")
-----------------------------------------------------------------------------------------------------------------------------

當你的模型完成定義，Django 就會自動產生一個專業的生產級
[管理接口](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/)
——一個允許認證用戶增加、更改和刪除物件的 Web 網站。你只需在 admin
網站上注冊你的模型即可：

mysite/news/models.py[¶](#id2 "永久連結至程式")**

    from django.db import models

    class Article(models.Model):
        pub_date = models.DateField()
        headline = models.CharField(max_length=200)
        content = models.TextField()
        reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

mysite/news/admin.py[¶](#id3 "永久連結至程式")**

    from django.contrib import admin

    from . import models

    admin.site.register(models.Article)

這樣設計所遵循的理念是，網站編輯人員可以是你的員工、你的客戶、或者就是你自己——而你大概不會樂意去廢半天勁建立一個只有內容管理功能的管理管理界面。

建立 Django
應用的典型流程是，先建立資料模型，然後搭建管理網站，之後你的員工（或者客戶）就可以向網站裡填充資料了。後面我們會談到如何展示這些資料。

規劃 URLs[¶](#design-your-urls "永久連結至標題")
------------------------------------------------

簡潔優雅的 URL 規劃對於一個高質量 Web 應用來說至關重要。Django
推崇優美的 URL 設計，所以不要把諸如 `.php`
之類的冗餘的後綴放到 URL 裡。

為了設計你自己的
[URLconf](https://docs.djangoproject.com/zh-hans/3.0/topics/http/urls/)
，你需要建立一個叫做 URLconf 的 Python
模組。這是網站的目錄，它包含了一張 URL 和 Python
回調函數之間的映射表。URLconf 也有利於將 Python 程式與 URL
進行解耦（譯注：使各個模組分離，獨立）。

下面這個 URLconf 適用於前面 `Reporter` 的例子：

mysite/news/urls.py[¶](#id4 "永久連結至程式")**

    from django.urls import path

    from . import views

    urlpatterns = [
        path('articles/<int:year>/', views.year_archive),
        path('articles/<int:year>/<int:month>/', views.month_archive),
        path('articles/<int:year>/<int:month>/<int:pk>/', views.article_detail),
    ]

上述程式將 URL 路徑映射到了 Python
回調函數（“視圖”）。路徑字串使用參數標籤從URL中“捕獲”相應值。當用戶請求頁面時，Django
依次遍歷路徑，直至初次比對到了請求的 URL。(如果無比對項，Django 會調用
404 視圖。) 這個過程非常快，因為路徑在載入時就編譯成了正規表達式。

一旦有 URL 路徑比對成功，Django
會調用相應的視圖函數。每個視圖函數會接受一個請求物件——包含請求元資訊——以及在比對式中獲取的參數值。

例如，當用戶請求了這樣的 URL "/articles/2005/05/39323/"，Django 會調用
`news.views.article_detail(request, year=2005, month=5, pk=39323)`。

編寫視圖[¶](#write-your-views "永久連結至標題")
-----------------------------------------------

視圖函數的執行結果只可能有兩種：回傳一個包含請求頁面元素的
[`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
物件，或者是拋出 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
這類異常。至於執行過程中的其它的動作則由你決定。

通常來說，一個視圖的工作就是：從參數獲取資料，裝載一個範本，然後將根據獲取的資料對範本進行渲染。下面是一個
`year_archive` 的視圖樣例：

mysite/news/views.py[¶](#id5 "永久連結至程式")**

    from django.shortcuts import render

    from .models import Article

    def year_archive(request, year):
        a_list = Article.objects.filter(pub_date__year=year)
        context = {'year': year, 'article_list': a_list}
        return render(request, 'news/year_archive.html', context)

這個例子使用了 Django
[範本系統](https://docs.djangoproject.com/zh-hans/3.0/topics/templates/)
，它有著很多強大的功能，而且使用起來足夠簡單，即使不是程式員也可輕松使用。

設計範本[¶](#design-your-templates "永久連結至標題")
----------------------------------------------------

上面的程式載入了 `news/year_archive.html` 範本。

Django 允許設置搜索範本路徑，這樣可以最小化範本之間的冗餘。在 Django
設置中，你可以透過 [`DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-DIRS)
參數指定一個路徑欄表用於檢索範本。如果第一個路徑中不包含任何範本，就繼續檢查第二個，以此類推。

讓我們假設 `news/year_archive.html`
範本已經找到。它看起來可能是下面這個樣子：

mysite/news/templates/news/year\_archive.html[¶](#id6 "永久連結至程式")**

    {% extends "base.html" %}

    {% block title %}Articles for {{ year }}{% endblock %}

    {% block content %}
    <h1>Articles for {{ year }}</h1>

    {% for article in article_list %}
        <p>{{ article.headline }}</p>
        <p>By {{ article.reporter.full_name }}</p>
        <p>Published {{ article.pub_date|date:"F j, Y" }}</p>
    {% endfor %}
    {% endblock %}

我們看到變數都被雙大括號括起來了。 `{{ article.headline }}` 的意思是“輸出 article 的 headline
屬性值”。這個“點”還有更多的用途，例如查找字典鍵值、查找索引和函數調用。

我們注意到 `{{ article.pub_date|date:"F j, Y" }}` 使用了 Unix
風格的“管道符”（“|”字串）。這是一個範本過濾器，用於過濾變數值。在這裡過濾器將一個
Python datetime 物件轉化為指定的格式（就像 PHP 中的日期函數那樣）。

你可以將多個過濾器連在一起使用。你還可以使用你
[自定義的範本過濾器](https://docs.djangoproject.com/zh-hans/3.0/howto/custom-template-tags/#howto-writing-custom-template-filters)
。你甚至可以自己編寫
[自定義的範本標籤](https://docs.djangoproject.com/zh-hans/3.0/howto/custom-template-tags/)
，相關的 Python 程式會在使用標籤時在管理執行。

Django 使用了 ''範本繼承'' 的概念。這就是
`{% extends "base.html" %}`
的作用。它的含義是''先載入名為 base
的範本，並且用下面的標記塊對範本中定義的標記塊進行填充''。簡而言之，範本繼承可以使範本間的冗餘內容最小化：每個範本只需包含與其它文件有區別的內容。

下面是 base.html 可能的樣子，它使用了
[靜態文件](https://docs.djangoproject.com/zh-hans/3.0/howto/static-files/)
：

mysite/templates/base.html[¶](#id7 "永久連結至程式")**

    {% load static %}
    <html>
    <head>
        <title>{% block title %}{% endblock %}</title>
    </head>
    <body>
        <img src="{% static "images/sitelogo.png" %}" alt="Logo">
        {% block content %}{% endblock %}
    </body>
    </html>

簡而言之，它定義了這個網站的外觀（利用網站的
logo），並且給子範本們挖好了可以填的”坑“。這就意味著你可以透過修改基礎範本以達到重新設計網頁的目的。

它還可以讓你利用不同的基礎範本並重用子範本建立一個網站的多個版本。透過建立不同的基礎範本，Django
的建立者已經利用這一技術來創造了明顯不同的手機版本的網頁。

注意，你並不是非得使用 Django
的範本系統，你可以使用其它你喜歡的範本系統。盡管 Django
的範本系統與其模型層能夠集成得很好，但這不意味著你必須使用它。同樣，你可以不使用
Django 的資料庫 API。你可以用其他的資料庫抽象層，像是直接讀取 XML
文件，亦或直接讀取磁碟文件，你可以使用任何方式。Django
的任何組成——模型、視圖和範本——都是獨立的。

這僅是基本入門知識[¶](#this-is-just-the-surface "永久連結至標題")
-----------------------------------------------------------------

以上只是 Django 的功能性概述。Django 還有更多實用的特性：

-   [緩存框架](https://docs.djangoproject.com/zh-hans/3.0/topics/cache/)
    可以與 memcached 或其它後端集成。
-   [聚合器框架](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/syndication/)
    可以透過編寫一個小型 Python 類來建立 RRS 和 Atom摘要。
-   功能豐富的自動產生的管理——這份概要只是簡單介紹了下。

接下來您可以 [下載 Django](https://www.djangoproject.com/community/)
，閱讀
[實例教學](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
，然後加入我們的 [社區](https://www.djangoproject.com/download/)
！感謝您的說明！

[** 開始](https://docs.djangoproject.com/zh-hans/3.0/intro/)

[快速安裝指南
**](https://docs.djangoproject.com/zh-hans/3.0/intro/install/)

快速安裝指南[¶](#quick-install-guide "永久連結至標題")
======================================================

開始用 Django 前，需要先進行安裝。我們寫了 :doc:\`
完整安裝指南\</topics/install\>\`
羅欄了各種安裝方法和情況；它會指導你完成一個簡易安裝，只要你按照指示操作，就可以執行得起來。

安裝 Python[¶](#install-python "永久連結至標題")
------------------------------------------------

作為一個 Python Web 框架，Django 需要 Python。更多細節請參見
[我應該使用哪個版本的 Python 來配合
Django?](https://docs.djangoproject.com/zh-hans/3.0/faq/install/#faq-python-version-support)。Python
包含了一個名為 [SQLite](https://sqlite.org/)
的輕量級資料庫，所以你暫時不必自行設置一個資料庫。

最新版本的 Python 可以透過開啟
[https://www.python.org/downloads/](https://www.python.org/downloads/)
或者操作系統的套件管理工具獲取。

你可以在你的 shell 中輸入 `python`{.docutils .literal .notranslate}
來確定你是否安裝過 Python；你看到的可能是像這樣子的:

    Python 3.x.y
    [GCC 4.x] on linux
    Type "help", "copyright", "credits" or "license" for more information.
    >>>

設置資料庫[¶](#set-up-a-database "永久連結至標題")
--------------------------------------------------

此步驟僅在你打算使用諸如 PostgreSQL, MariaDB, MySQL, 或者 Oracle
這些大型資料庫引擎時需要。要安裝這種資料庫, 請參考 [database
installation
information](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#database-installation)。

安裝 Django[¶](#install-django "永久連結至標題")
------------------------------------------------

安裝 Django有以下三種方式：

-   [Install an official
    release](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#installing-official-release)
    適合大部分用戶。
-   安裝 Django [provided by your operating system
    distribution](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#installing-distribution-package)。
-   [Install the latest development
    version](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#installing-development-version)
    這個選擇是針對那些想要體驗最新和最好的特性的愛好者們，並不怕執行全新程式。你在開發版中可能會遇到新的
    bug，可以報告給社區團隊協助 Django
    開發。此外，第三方發行的軟件套件也可能不與開發版進行相容。

請始終參考與你所使用的版本對應的 Django 文件！

如果採用了前兩種方式進行安裝，你需要注意在文件中標明
**在開發版中新增**。這個標記表示這個特性僅適用開發版
Django，並且他們可能不會在官方發布的穩定版中出現。

驗證[¶](#verifying "永久連結至標題")
------------------------------------

若要驗證 Django 是否能被 Python 識別，可以在 shell 中輸入
`python`{.docutils .literal .notranslate}。 然後在 Python
提示符下，嘗試導入 Django：

``` {.literal-block}
>>> import django
>>> print(django.get_version())
3.0
```

當然了，你也可能安裝的是其它版本的 Django。

搞定！[¶](#that-s-it "永久連結至標題")
--------------------------------------

搞定，現在你可以 [move onto the
tutorial](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)。

[** 初識
Django](https://docs.djangoproject.com/zh-hans/3.0/intro/overview/)

[編寫你的第一個 Django 應用，第 1 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)

編寫你的第一個 Django 應用，第 1 部分[¶](#writing-your-first-django-app-part-1 "永久連結至標題")
================================================================================================

讓我們透過範例來學習 Django。透過這個教學，我們將帶著你建立一個基本的投票應用程式。它將由兩部分組成：

-   一個讓人們查看和投票的公開網站。
-   一個讓你能增加、修改和刪除投票的管理網站。

我們假定你已經閱讀了 [安裝
Django](https://docs.djangoproject.com/zh-hans/3.0/intro/install/)。你能知道
Django 已被安裝，且安裝的是哪個版本，透過在命令提示列輸入命令（由 \$ 前置符號）。

    $ python -m django --version

如果這行命令輸出了一個版本號碼，證明你已經安裝了此版本的
Django；如果你得到的是一個“No module named
django”的錯誤提示，則表示你尚未安裝。

這個教學是為了 Django 3.0 寫的，它支援 Python 3.6 和後續版本。如果
Django
的版本不符合，你可以透過頁面右下角的版本切換器切換到對應你版本的教學，或更新至最新版本。如果你正在使用一個較舊版本的
Python，在 [我應該使用哪個版本的 Python 來配合
Django?](https://docs.djangoproject.com/zh-hans/3.0/faq/install/#faq-python-version-support)
查找一個合適的 Django 版本。

你可以查看文件 [如何安裝
Django](https://docs.djangoproject.com/zh-hans/3.0/topics/install/)
來取得關於移除舊版本，安裝新版本的流程和建議。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的版塊。

建立專案[¶](#creating-a-project "永久連結至標題")
-------------------------------------------------

如果這是你第一次使用 Django
的話，你需要一些初始化設置。也就是說，你需要用一些自動產生的程式設定一個
Django
[專案](https://docs.djangoproject.com/zh-hans/3.0/glossary/#term-project)
—— 即一個 Django 專案實例需要的設定項集合，套件括資料庫設定、Django
設定和應用程式設定。

打開命令列，`cd`
到一個你想放置你程式的目錄，然後執行以下命令：



    $ django-admin startproject mysite

這行程式將會在當前目錄下建立一個 `mysite` 目錄。如果命令失敗了，參考 [執行 django-admin
時遇到的問題](https://docs.djangoproject.com/zh-hans/3.0/faq/troubleshooting/#troubleshooting-django-admin)，也許可以提供協助。

注解

你得避免使用 Python 或 Django
的內部保留字來命名你的專案。具體地說，你得避免使用像 `django` (會和 Django 自己產生衝突)或 `test` (會和 Python 的內置組件產生衝突)這樣的名字。

我的程式該放在哪？

如果你曾經是原生 PHP
程式設計師（沒有使用過現代框架），你可能會習慣於把程式放在 Web
伺服器的文件根目錄(諸如 `/var/www`)。當使用 Django 時不需要這樣做。把所有 Python 程式放在 Web
伺服器的根目錄不是個好主意，因為這樣會有風險。比如會提高人們在網站上看到你的程式的可能性。這不利於網站的安全。

把你的程式放在文件根目錄 **以外** 的某些地方吧，比如 /home/mycode。

讓我們看看 [`startproject`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-startproject)
建立了些什麼:

    mysite/
        manage.py
        mysite/
            __init__.py
            settings.py
            urls.py
            asgi.py
            wsgi.py

這些目錄和文件的用處是：

-   最外層的 `mysite/`
    根目錄只是你專案的容器，
    根目錄名稱對Django沒有影響，你可以將它重命名為任何你喜歡的名稱。
-   `manage.py`:
    一個讓你用各種方式管理 Django 專案的命令列工具。你可以閱讀
    [django-admin and
    manage.py](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/)
    獲取所有 `manage.py` 的細節。
-   裡面一層的 `mysite/`
    目錄套件含你的專案，它是一個純 Python
    套件。它的名字就是當你引用它內部任何東西時需要用到的 Python 套件名。
    (比如 `mysite.urls`).
-   `mysite/__init__.py`{.file .docutils .literal
    .notranslate}：一個空文件，告訴 Python 這個目錄應該被認為是一個
    Python 套件。如果你是 Python 初學者，閱讀官方文件中的
    [更多關於套件的知識](https://docs.python.org/3/tutorial/modules.html#tut-packages "(在 Python v3.8)")。
-   `mysite/settings.py`：Django
    專案的設定文件。如果你想知道這個文件是如何運作的，請查看 [Django
    設定](https://docs.djangoproject.com/zh-hans/3.0/topics/settings/)
    了解細節。
-   `mysite/urls.py`：Django
    專案的 URL 宣告，就像你網站的“目錄”。閱讀
    [URL調度器](https://docs.djangoproject.com/zh-hans/3.0/topics/http/urls/)
    文件來獲取更多關於 URL 的內容。
-   `mysite/asgi.py`{.file .docutils .literal
    .notranslate}：作為你的專案的執行在 ASGI
    相容的Web伺服器上的入口。閱讀 [如何使用 WSGI
    進行部署](https://docs.djangoproject.com/zh-hans/3.0/howto/deployment/wsgi/)
    了解更多細節。
-   `mysite/wsgi.py`{.file .docutils .literal
    .notranslate}：作為你的專案的執行在 WSGI
    相容的Web伺服器上的入口。閱讀 [如何使用 WSGI
    進行部署](https://docs.djangoproject.com/zh-hans/3.0/howto/deployment/wsgi/)
    了解更多細節。

用於開發的簡易伺服器[¶](#the-development-server "永久連結至標題")
-----------------------------------------------------------------

讓我們來確認一下你的 Django
專案是否真的建立成功了。如果你的當前目錄不是外層的 `mysite`{.file
.docutils .literal .notranslate}
目錄的話，請切換到此目錄，然後執行下面的命令：



    $ python manage.py runserver

你應該會看到如下輸出：

``` 
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
October 25, 2020 - 12:15:41
Django version 3.1.2, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.

```

注解

忽略有關未套用最新資料庫遷移的警告(You have 18 unapplied migration(s). Your project may not work properly ... to apply them.)，稍後我們會處理資料庫。

你剛剛啟動的是 Django 自帶的用於開發的簡易伺服器，它是一個用純 Python
寫的輕量級的 Web 伺服器。我們將這個伺服器內置在 Django
中是為了讓你能快速的開發出想要的東西，因為你不需要進行設定生產級別的伺服器（比如
Apache）方面的工作，除非你已經準備好投入生產環境了。

現在是個提醒你的好時機：**千萬不要**
將這個伺服器用於和生產環境相關的任何地方。這個伺服器只是為了開發而設計的。(我們在
Web 框架方面是專家，在 Web 伺服器方面並不是。)

現在，伺服器正在執行，瀏覽器開啟
[https://127.0.0.1:8000/](https://127.0.0.1:8000/)。你將會看到一個“恭喜”頁面，隨著一只火箭發射，伺服器已經執行了。

更換連接埠

預設情況下，[`runserver`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-runserver)
命令會將伺服器設置為監聽本機內部 IP 的 8000 連接埠。

如果你想更換伺服器的監聽連接埠，請使用命令列參數。舉個例子，下面的命令會使伺服器監聽
8080 連接埠：



    $ python manage.py runserver 8080

如果你想要修改伺服器監聽的IP，在連接埠之前輸入新的。比如，為了監聽所有伺服器的公開IP（在你執行
Vagrant 或想要向網絡上的其它電腦展示你的成果時很有用），使用：



    $ python manage.py runserver 0:8000

**0** 是 **0.0.0.0** 的簡寫。完整的關於開發伺服器的文件可以在
[:djamdin:\`runserver\`](#id1) 參考文件中找到。

會自動重新載入的伺服器 [`runserver`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-runserver)

用於開發的伺服器在需要的情況下會對每一次的開啟請求重新載入一遍 Python
程式。所以你不需要為了讓修改的程式生效而頻繁的重新啟動伺服器。然而，一些動作，比如增加新文件，將不會觸發自動重新載入，這時你得自己手動重啟伺服器。

建立投票應用程式[¶](#creating-the-polls-app "永久連結至標題")
-------------------------------------------------------------

現在你的開發環境——這個“專案” ——已經設定好了，你可以開始作業了。

在 Django 中，每一個應用都是一個 Python
套件，並且遵循著相同的約定。Django
自帶一個工具，可以幫你產生應用程式的基礎目錄結構，這樣你就能專心寫程式，而不是建立目錄了。

專案 VS 應用程式

專案和應用程式有什麼區別？應用程式是一個專門做某件事的網絡應用程式——比如部落格系統，或者公享記錄的資料庫，或者小型的投票程式。專案則是一個網站使用的設定和應用程式的集合。專案可以套件含很多個應用程式。應用程式可以被很多個專案使用。

你的應用程式可以存放在任何 [Python
path](https://docs.python.org/3/tutorial/modules.html#tut-searchpath "(在 Python v3.8)")
中定義的路徑。在這個教學中，我們將在你的 `manage.py`{.file .docutils
.literal .notranslate}
同階層目錄下建立投票應用程式。這樣它就可以作為頂層模組匯入，而不是
`mysite` 的子模組。

請確定你現在處於 `manage.py`
所在的目錄下，然後執行這行命令來建立一個應用程式：



    $ python manage.py startapp polls

這將會建立一個 `polls`
目錄，它的目錄結構大致如下：

    polls/
        __init__.py
        admin.py
        apps.py
        migrations/
            __init__.py
        models.py
        tests.py
        views.py

這個目錄結構套件括了投票應用程式的全部內容。

編寫第一個視圖[¶](#write-your-first-view "永久連結至標題")
----------------------------------------------------------

讓我們開始編寫第一個視圖吧。打開 `polls/views.py`，把下面這些 Python 程式輸入進去：

polls/views.py[¶](#id1 "永久連結至程式")**

    from django.http import HttpResponse


    def index(request):
        return HttpResponse("Hello, world. You're at the polls index.")

這是 Django 中最簡單的視圖。如果想看見效果，我們需要將一個 URL
對映到它——這就是我們需要 URLconf 的原因了。

為了建立 URLconf，請在 polls 目錄裡新建一個 `urls.py` 文件。你的應用程式目錄現在看起來應該是這樣：

    polls/
        __init__.py
        admin.py
        apps.py
        migrations/
            __init__.py
        models.py
        tests.py
        urls.py
        views.py

在 `polls/urls.py` 中，輸入如下程式碼：

polls/urls.py[¶](#id2 "永久連結至程式")**

    from django.urls import path

    from . import views

    urlpatterns = [
        path('', views.index, name='index'),
    ]

下一步是要在根 URLconf 文件中指定我們建立的 `polls.urls` 模組。在 `mysite/urls.py` 文件的 `urlpatterns`
欄表裡插入一個 `include()`， 如下：

mysite/urls.py[¶](#id3 "永久連結至程式")**

    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path('polls/', include('polls.urls')),
        path('admin/', admin.site.urls),
    ]

函數 [`include()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.include "django.urls.include")
允許引用其它 URLconfs。每當 Django 遇到 [`include()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.include "django.urls.include")
時，它會將 URL
與函式參數指定字串比對符合的部分截斷，並將剩餘的字串傳送到 URLconf
以供進一步處理。

我們設計 [`include()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.include "django.urls.include")
的理念是使其可以即插即用。因為投票應用程式有它自己的 URLconf(
`polls/urls.py` )，他們能夠被放在
"/polls/" ， "/fun\_polls/"
，"/content/polls/"，或者其他任何路徑下，這個應用程式都能夠正常工作。

何時使用 [`include()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.include "django.urls.include")

當套件括其它 URL 模式時你應該總是使用 `include()` ， `admin.site.urls`
是唯一例外。

你現在把 `index` 視圖增加進了
URLconf。透過以下命令驗證是否正常工作：



    $ python manage.py runserver

用你的瀏覽器開啟 <http://localhost:8000/polls/>，你應該能夠看見 "*Hello,
world. You're at the polls index.*" ，這是你在 `index` 視圖中定義的。

無法找到頁面 Page Not Found?

如果你在這裡得到了一個錯誤頁面，檢查一下你是不是開啟
http://localhost:8000/polls/ 頁面而不是 http://localhost:8000/。

函數 [`path()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.path "django.urls.path")
具有四個參數，兩個必要參數：`route` 和
`view`，兩個可選參數：`kwargs`
和 `name`。現在，是時候來研究這些參數的含義了。

### [`path()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.path "django.urls.path") 參數： `route`[¶](#path-argument-route "永久連結至標題")

`route` 是一個包含 URL
模式的字串（類似正規表達式）。當 Django 回應一個請求時，它會從
`urlpatterns`
清單的第一項開始，依序比對清單中的每一個項目，直到找到比對符合的項目為止。

這些模式不會比對 GET 和 POST 參數或網域名稱。例如，URLconf 在處理請求
`https://www.example.com/myapp/`時，它會嘗試比對 `myapp/` 。在處理
`https://www.example.com/myapp/?page=3`請求時，也只會嘗試比對 `myapp/`。

### [`path()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.path "django.urls.path") 參數： `view`[¶](#path-argument-view "永久連結至標題")

當 Django
找到了一個比對符合的模式，就會呼叫這個特定的視圖函數，並傳入一個
[`HttpRequest`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest")
物件作為第一個參數，被 “捕獲”
的參數以關鍵字參數的形式傳入。稍後，我們會提供一個範例。

### [`path()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.path "django.urls.path") 參數： `kwargs`[¶](#path-argument-kwargs "永久連結至標題")

任意個關鍵字參數可以作為一個字典傳遞給目標視圖函數。本教學中不會使用這一特性。

### [`path()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.urls.path "django.urls.path") 參數： `name`[¶](#path-argument-name "永久連結至標題")

為你的 URL 取名能使你在 Django
的任意地方唯一地引用它，尤其是在範本中。這個有用的特性允許你只改一個文件就能全域地修改某個
URL 模式。

當你了解了基本的請求和回應流程後，請閱讀 [教學的第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
開始使用資料庫.

[**
快速安裝指南](https://docs.djangoproject.com/zh-hans/3.0/intro/install/)

編寫你的第一個 Django 應用，第 2 部分[¶](#writing-your-first-django-app-part-2 "永久連結至標題")
================================================================================================

這部分教學從 [教學第 1
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
結尾的地方繼續講起。我們將建立資料庫，建立您的第一個模型，並主要說明
Django 提供的自動產生的管理頁面。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

資料庫設定[¶](#database-setup "永久連結至標題")
-----------------------------------------------

現在，打開 `mysite/settings.py`
。這是個包含了 Django 專案設定的 Python 模組。

通常，這個設定文件使用 SQLite
作為預設資料庫。如果你不熟悉資料庫，或者只是想嘗試一下
Django，這是最簡單的選擇。Python 內置
SQLite，所以你無需安裝額外東西來使用它。當你開始一個真正的專案時，你可能更傾向使用一個更具擴展性的資料庫，例如
PostgreSQL，避免中途切換資料庫這個令人頭疼的問題。

如果你想使用其他資料庫，你需要安裝合適的 [database
bindings](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#database-installation)
，然後改變設定文件中 [`DATABASES`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-DATABASES)
`'default'` 專案中的一些鍵值：

-   [`ENGINE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-DATABASE-ENGINE)
    -- 可選值有 `'django.db.backends.sqlite3'`，`'django.db.backends.postgresql'`，`'django.db.backends.mysql'`，或 `'django.db.backends.oracle'`。其它
    [可使用的後端資料庫](https://docs.djangoproject.com/zh-hans/3.0/ref/databases/#third-party-notes)。
-   [`NAME`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-NAME)
    - 資料庫的名稱。如果使用的是
    SQLite，資料庫將是你電腦上的一個文件，在這種情況下， [`NAME`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-NAME)
    應該是此文件的絕對路徑，包括文件名稱。預設值
    `BASE_DIR / 'db.sqlite3'` 將會把資料庫文件儲存在專案的根目錄。

如果你不使用 SQLite，則必須增加一些額外設定，例如 [`USER`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-USER)
、 [`PASSWORD`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-PASSWORD)
、 [`HOST`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-HOST)
等等。想了解更多資料庫設定方面的內容，請參考文件：[`DATABASES`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-DATABASES)
。

SQLite 以外的其它資料庫

如果你使用了 SQLite
以外的資料庫，請確認在使用前已經建立了資料庫。你可以透過在你的資料庫交互式命令欄中使用
"`CREATE DATABASE database_name;`"
命令來完成這件事。

另外，還要確保該資料庫用戶中提供 `mysite/settings.py` 具有 "create database" 權限。這使得自動建立的
[test
database](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/overview/#the-test-database)
能被以後的教學使用。

如果你使用
SQLite，那麼你不需要在使用前做任何事——資料庫會在需要的時候自動建立。

編輯 `mysite/settings.py`
文件前，先設定 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TIME_ZONE)
為你自己時區。在台灣台北時區，請設定 `TIME_ZONE = 'Asia/Taipei'` 以及 `LANGUAGE_CODE = 'zh-Hant'`。

此外，說明一下文件頂部的 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
設定項。這裡包括了會在你專案中啟用的所有 Django
應用程式。應用程式能在多個專案中使用，你也可以包裝並且發佈應用程式，讓別人使用它們。

通常， [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
預設包括了以下 Django 的內建應用：

-   [`django.contrib.admin`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#module-django.contrib.admin "django.contrib.admin: Django's admin site.")
    -- 管理員網站， 你很快就會使用它。
-   [`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/3.0/topics/auth/#module-django.contrib.auth "django.contrib.auth: Django's authentication framework.")
    -- 認證授權框架。
-   [`django.contrib.contenttypes`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/contenttypes/#module-django.contrib.contenttypes "django.contrib.contenttypes: Provides generic interface to installed models.")
    -- 內容類型框架。
-   [`django.contrib.sessions`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/sessions/#module-django.contrib.sessions "django.contrib.sessions: Provides session management for Django projects.")
    -- 會議框架。
-   [`django.contrib.messages`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/messages/#module-django.contrib.messages "django.contrib.messages: Provides cookie- and session-based temporary message storage.")
    -- 訊息框架。
-   [`django.contrib.staticfiles`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles "django.contrib.staticfiles: An app for handling static files.")
    -- 管理靜態文件的框架。

為了給常見的專案提供方便因此這些應用程式預設是啟用的。

預設開啟的某些應用程式需要至少一個資料表，所以，在使用他們之前需要在資料庫中建立一些表。請執行以下命令：

    $ python manage.py migrate

這個 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令檢查 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
設定，為其中的每個應用程式建立需要的資料表，至於具體會建立什麼，這取決於你的
`mysite/settings.py`
設定文件和每個應用程式的資料庫遷移文件（我們稍後會介紹這個）。這個命令所執行的每個遷移操作都會在終端中顯示出來。如果你感興趣的話，執行你資料庫的命令列工具，並輸入
`\dt` (PostgreSQL)，
`SHOW TABLES;` (MariaDB,MySQL)，
`.schema` (SQLite)或者
`SELECT TABLE_NAME FROM USER_TABLES;`
(Oracle) 來看看 Django 到底建立了哪些表。以下是我個人的例子：

    $ python manage.py migrate

    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, sessions
    Running migrations:
      Applying contenttypes.0001_initial... OK
      Applying auth.0001_initial... OK
      Applying admin.0001_initial... OK
      Applying admin.0002_logentry_remove_auto_add... OK
      Applying admin.0003_logentry_add_action_flag_choices... OK
      Applying contenttypes.0002_remove_content_type_name... OK
      Applying auth.0002_alter_permission_name_max_length... OK
      Applying auth.0003_alter_user_email_max_length... OK
      Applying auth.0004_alter_user_username_opts... OK
      Applying auth.0005_alter_user_last_login_null... OK
      Applying auth.0006_require_contenttypes_0002... OK
      Applying auth.0007_alter_validators_add_error_messages... OK
      Applying auth.0008_alter_user_username_max_length... OK
      Applying auth.0009_alter_user_last_name_max_length... OK
      Applying auth.0010_alter_group_name_max_length... OK
      Applying auth.0011_update_proxy_permissions... OK
      Applying auth.0012_alter_user_first_name_max_length... OK
      Applying sessions.0001_initial... OK
      
寫給極簡主義者

就像之前說的，為了方便大多數專案，我們預設啟用了一些應用程式，但並不是每個人都需要它們。如果你不需要某個或某些應用程式，你可以在執行
[`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
前毫無顧慮地從 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
裡注釋或者刪除掉它們。 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令只會為在 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
裡有宣告的應用程式進行資料庫遷移。

建立模型[¶](#creating-models "永久連結至標題")
----------------------------------------------

在 Django 裡寫一個資料庫驅動的 Web 應用程式的第一步是定義模型 -
也就是資料庫結構設計和附加的其它資料摘要。

設計哲學

模型是真實資料的簡單明確的描述。它包含了儲存的資料所必要的欄位和行為。Django
遵循 [DRY
Principle](https://docs.djangoproject.com/zh-hans/3.0/misc/design-philosophies/#dry)
。它的目標是在一個地方定義資料模型，然後其相關的程式由模型自動產生。

來介紹一下遷移 - 舉個例子，不像 Ruby On Rails，Django
的遷移程式是由你的模型文件自動產生的，它本質上是個歷史記錄，Django
可以用它來進行資料庫的滾動更新，透過這種方式使其能夠和當前的模型相符合。

在這個投票應用程式中，需要建立兩個模型：問題 `Question` 和選項 `Choice`。問題 `Question`
模型包括問題描述 (question_text) 和發佈時間 (pub_date)。選項 `Choice`
模型有兩個欄位，選項描述 (choice_text) 和當前得票數 (votes)。每個選項 `Choice` 屬於一個問題 `Question`。

這些概念可以透過一個 Python 類別來描述。按照下面的例子來編輯
`polls/models.py` 文件：

polls/models.py[¶](#id2 "永久連結至程式")**

    from django.db import models


    class Question(models.Model):  # 問題模型
        question_text = models.CharField(max_length=200)  # 問題描述
        pub_date = models.DateTimeField('date published')  # 發佈時間


    class Choice(models.Model):  # 選項模型
        question = models.ForeignKey(Question, on_delete=models.CASCADE)
        choice_text = models.CharField(max_length=200)  # 選項描述
        votes = models.IntegerField(default=0)  # 當前得票數

每個模型被表示為 [`django.db.models.Model`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/instances/#django.db.models.Model "django.db.models.Model")
類別的子類別。每個模型有許多類別變數，它們都表示模型裡的一個資料庫欄位。

每個欄位都是 [`Field`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field "django.db.models.Field")
類別的實例 - 例如，字串欄位被表示為 [`CharField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField")
，日期時間欄位被表示為 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")
。這將告訴 Django 每個欄位要處理的資料類型。

每個 [`Field`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field "django.db.models.Field")
類別實例變數的名字（例如 `question_text` 或 `pub_date`
）也是欄位名稱，所以最好使用對機器友好的格式。你將會在 Python
程式裡使用它們，而資料庫會將它們作為欄名稱。

你可以使用可選的選項來為 [`Field`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field "django.db.models.Field")
定義一個人類可讀的名字。這個功能在很多 Django
內部組成部分中都被使用了，而且作為文件的一部分。如果某個欄位沒有提供此名稱，Django
將會使用對機器友好的名稱，也就是變數名稱。在上面的例子中，我們只為
`Question.pub_date`
定義了對人類友好的名字。對於模型內的其它欄位，它們的機器友好名稱也會被作為人類友好名稱使用。

定義某些 [`Field`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field "django.db.models.Field")
類別實例需要參數。例如 [`CharField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField")
需要一個 [`max_length`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.CharField.max_length "django.db.models.CharField.max_length")
參數。這個參數的用途不僅止於用來定義資料庫結構，也用於驗證資料，我們稍後將會看到這方面的內容。

[`Field`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field "django.db.models.Field")
也能夠接收多個可選參數；在上面的例子中：我們將 `votes` 的 [`default`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.Field.default "django.db.models.Field.default")
也就是預設值，設為0。

注意在最後，我們使用 [`ForeignKey`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.ForeignKey "django.db.models.ForeignKey")
定義了一個關聯。這將告訴 Django，每個 `Choice` 物件都關聯到一個 `Question` 物件。Django
支援所有常用的資料庫關聯：多對一、多對多和一對一。

啟用模型[¶](#activating-models "永久連結至標題")
------------------------------------------------

上面的一小段用於建立模型的程式給了 Django 很多資訊，透過這些資訊，Django
可以：

-   為這個應用程式建立資料庫 schema（產生 `CREATE TABLE` 語句）。
-   建立可以與 `Question` 和
    `Choice` 物件進行交互的 Python
    資料庫 API。

但是首先得把 `polls`
應用程式安裝到我們的專案裡。

設計哲學

Django
應用程式是“可插拔”的。你可以在多個專案中使用同一個應用程式。除此之外，你還可以發佈自己的應用程式，因為它們並不會被綁定到當前的
Django 安裝上。

為了在我們的建置中包含這個應用程式，我們需要在設定類別
[`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
中增加設定。因為 `PollsConfig`
類別寫在文件 `polls/apps.py`
中，所以它的點式路徑是 `'polls.apps.PollsConfig'`。在文件 `mysite/settings.py` 中 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
子項增加點式路徑後，它看起來像這樣：

mysite/settings.py[¶](#id3 "永久連結至程式")**

    INSTALLED_APPS = [
        'polls.apps.PollsConfig',
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
    ]

現在你的 Django 專案會包含 `polls`
應用程式。接著執行下面的命令：


    $ python manage.py makemigrations polls

你將會看到類似於下面這樣的輸出：

    Migrations for 'polls':
      polls/migrations/0001_initial.py
        - Create model Question
        - Create model Choice

透過執行 `makemigrations` 命令，Django
會檢測你對模型文件的修改（在目前這個情況下，你已經剛建立了一個新的模型），並且把修改的部分儲存為一次
*遷移*。

遷移是 Django 對於模型定義（也就是你的資料庫結構）的變化的儲存形式 -
它們其實也只是一些你磁碟上的文件。如果你想的話，你可以閱讀一下你模型的遷移資料，它被儲存在
`polls/migrations/0001_initial.py`
裡。別擔心，你不需要每次都閱讀遷移文件，但是它們被設計成人類可讀的形式，這是為了便於你手動調整
Django 的修改方式。

Django 有一個自動執行資料庫遷移並同步管理你的資料庫結構的命令 -
這個命令是 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)，我們馬上就會接觸它
- 但是首先，讓我們看看遷移命令會執行哪些 SQL 語句。[`sqlmigrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-sqlmigrate)
命令取得遷移的名稱，然後回傳對應的 SQL 語句：


    $ python manage.py sqlmigrate polls 0001

你將會看到類似下面這樣的輸出（我把輸出重組成了人類可讀的格式）：

    BEGIN;
    --
    -- Create model Question
    --
    CREATE TABLE "polls_question" (
        "id" serial NOT NULL PRIMARY KEY,
        "question_text" varchar(200) NOT NULL,
        "pub_date" timestamp with time zone NOT NULL
    );
    --
    -- Create model Choice
    --
    CREATE TABLE "polls_choice" (
        "id" serial NOT NULL PRIMARY KEY,
        "choice_text" varchar(200) NOT NULL,
        "votes" integer NOT NULL,
        "question_id" integer NOT NULL
    );
    ALTER TABLE "polls_choice"
      ADD CONSTRAINT "polls_choice_question_id_c5b4b260_fk_polls_question_id"
        FOREIGN KEY ("question_id")
        REFERENCES "polls_question" ("id")
        DEFERRABLE INITIALLY DEFERRED;
    CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");

    COMMIT;

請注意以下幾點：

-   輸出的內容和你使用的資料庫有關，上面的輸出範例使用的是 PostgreSQL。
-   資料庫的表名稱是結合了應用程式名稱 (`polls`) 和資料庫模型的英文小寫名稱 – `question` 和 `choice`
    組合而成的。（如果需要，你可以覆寫這個定義。）
-   主鍵 (IDs) 會被自動建立。(你也可以覆寫這個定義。)
-   依照慣例，Django 會在外鍵欄位名稱後追加字串 `"_id"` 。（是，這個定義也可以覆寫。）
-   外鍵關聯由 `FOREIGN KEY`
    產生。你不用關心 `DEFERRABLE`
    部分，它只是告訴
    PostgreSQL，請在整個交易過程全部執行完之後再建立外鍵關聯。
-   產生的 SQL
    語句是使用你選用的資料庫客製化成的，所以那些和資料庫有關的欄位類型，例如
    `auto_increment` (MySQL)、
    `serial` (PostgreSQL)和
    `integer primary key autoincrement`
    (SQLite)，Django 會幫你自動處理。那些和引號相關的事情 –
    例如，是使用單引號還是雙引號 – 也一樣會被自動處理。
-   這個 [`sqlmigrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-sqlmigrate)
    命令並沒有真正在你的資料庫中的執行遷移 -
    相反，它只是把命令輸出到螢幕上，讓你看看 Django 認為需要執行哪些 SQL
    語句。這在你想看看 Django
    到底準備做什麼，或者當你是資料庫管理員，需要寫腳本來批量處理資料庫時會很有用。

如果你有興趣，你也可以試試看執行 [`python manage.py check`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-check)；這個命令協助你檢查專案中的問題，並且在檢查過程中不會對資料庫進行任何操作。

現在，再次執行 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令，在資料庫裡建立新定義的模型的資料表：


    $ python manage.py migrate
    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, polls, sessions
    Running migrations:
      Rendering model states... DONE
      Applying polls.0001_initial... OK

這個 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令取得所有還沒有執行過的遷移（Django 透過在資料庫中建立一個特殊的表
`django_migrations`
來追蹤執行過哪些遷移）並套用在資料庫上 -
也就是將你對模型的更改同步到資料庫結構上。

遷移是非常強大的功能，它能讓你在開發過程中持續的改變資料庫結構而不需要重新刪除和建立表
-
它專注於使資料庫即時更新而不會遺失資料。我們會在後面的教學中更加深入的學習這部分內容，現在，你只需要記住，改變模型需要這三個步驟：

-   編輯 `models.py` 文件，改變模型。
-   執行 [`python manage.py makemigrations`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-makemigrations)
    為模型的改變產生遷移文件。
-   執行 [`python manage.py migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
    來套用資料庫遷移。

資料庫遷移被分解成產生和套用兩個命令是因為你將可以在版本控制系統上與你的應用程式一起進行遷移確認；這不僅僅會讓開發更加簡單，也給別的開發者和生產環境中的使用帶來方便。

透過閱讀文件 [Django
管理文件](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/)
，你可以取得關於 `manage.py`
工具的更多資訊。

初次嘗試 API[¶](#playing-with-the-api "永久連結至標題")
---------------------------------------------------

現在讓我們進入交互式 Python 命令欄，嘗試一下 Django 為你建立的各種的 API。
請透過以下命令開啟 Python 命令列模式 (shell)：

    $ python manage.py shell

我們不是簡單的輸入 "Python" 而是改用上面這樣的指令是因為 `manage.py` 會設定 `DJANGO_SETTINGS_MODULE`
的環境變數，這個變數會讓 Django 依據 `mysite/settings.py` 文件來設定 Python 套件的匯入路徑。

當你成功進入命令列後，來試試 [database API](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/) 吧:

    >>> from polls.models import Choice, Question  # 從 polls.models 匯我我們剛寫好的兩個資料模型類別 (model classes)

    # 呼叫 objects.all() 來列出目前的問題 (Question) 清單。但目前系統中還沒有任何已建立的問題 (Question)。
    >>> Question.objects.all()
    <QuerySet []>

    # 現在我們來建立一個新的問題 (Question)。在預設的設定文件中啟用了對時區的支援，
    # 因此 Django 預期你使用的 pub_date 變數中有包含 tzinfo 的日期時間。所以我們將
    # 使用 timezone.now() 而不是 datetime.datetime.now() 這樣才能建立正確的結果。
    >>> from django.utils import timezone
    >>> q = Question(question_text="有什麼新鮮事?", pub_date=timezone.now())

    # 要能剛建立的將物件儲存到資料庫中，必須明確地呼叫 save() 函式。
    >>> q.save()

    # 現在這個物件有一個 ID 屬性.
    >>> q.id
    1

    # 透過 Python 來存取模型的屬性字串值。
    >>> q.question_text
    "有什麼新鮮事?"
    
    >>> q.pub_date
    datetime.datetime(2020, 12, 27, 13, 0, 0, 775217, tzinfo=<UTC>)

    # 你可以直接透過更改屬性來變更其值，然後再一次呼叫 save() 函式來儲存到資料庫中。
    >>> q.question_text = "目前什麼情況?"
    >>> q.save()

    # objects.all() 顯示資料庫中的所有的問題。
    >>> Question.objects.all()
    <QuerySet [<Question: Question object (1)>]>

到這裡我們看到輸出的結束似乎可以更好。因為`<Question: Question object (1)>`對於我們瞭解這個物件的細節並沒有什麼幫助。
我們透過編輯 `Question` 模型的程式（位於`polls/models.py`中）來修正這個問題。為 `Question` 和 `Choice` 增加
[`__str__()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/instances/#django.db.models.Model.__str__ "django.db.models.Model.__str__")
方法。

polls/models.py[¶](#id4 "永久連結至程式")**

    from django.db import models

    class Question(models.Model):
        # ...
        def __str__(self):
            return self.question_text

    class Choice(models.Model):
        # ...
        def __str__(self):
            return self.choice_text

為模型增加 [`__str__()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/instances/#django.db.models.Model.__str__ "django.db.models.Model.__str__")
方法是很重要的，這不僅只為你在命令列裡的使用帶來方便，同時 Django 所自動產生的管理頁面中也將會使用這個方法函式來表示物件。

現在我們再為此模型增加一個自定義方法：

polls/models.py[¶](#id5 "永久連結至程式")**

    import datetime

    from django.db import models
    from django.utils import timezone

    class Question(models.Model):
        # ...
        def was_published_recently(self):
            return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

新加入的 `import datetime` 和 `from django.utils import timezone` 分別匯入了 Python 的
標準 [`datetime`](https://docs.python.org/3/library/datetime.html#module-datetime "(在 Python v3.8)")
模組和 Django 中和時區相關的 [`django.utils.timezone`](https://docs.djangoproject.com/zh-hans/3.0/ref/utils/#module-django.utils.timezone "django.utils.timezone: Timezone support.") 工具模組。如果你不太熟悉 Python 中的時區處理方式，請參考
[時區支援文件](https://docs.djangoproject.com/zh-hans/3.0/topics/i18n/timezones/) 給予的協助。

請儲存剛才編輯的程式然後重新透過 `python manage.py shell` 命令再次開啟 Python 交互式命令列 (shell)：

    >>> from polls.models import Choice, Question

    # 以下指令可以確認我們新增的 __str__() 是否有發揮功能。
    >>> Question.objects.all()
    <QuerySet [<Question: 目前什麼情況?>]>

    # 在 Django 中提供了一個豐富的資料庫搜尋的 API，該 API 完全由
    # 關鍵字參數來操作你的搜尋。
    >>> Question.objects.filter(id=1)
    <QuerySet [<Question: 目前什麼情況?>]>
    >>> Question.objects.filter(question_text__startswith='What')
    <QuerySet [<Question: 目前什麼情況?>]>

    # 如果要取得今年發佈的問題 (Question) 清單，可以使用下列方式。
    >>> from django.utils import timezone
    >>> current_year = timezone.now().year
    >>> Question.objects.get(pub_date__year=current_year)
    <Question: 目前什麼情況?>

    # 如果你指定了一個不存在的 ID 而對 API 提出請求，這樣將會引起程式的異常錯誤發生。
    >>> Question.objects.get(id=2)
    Traceback (most recent call last):
        ...
    DoesNotExist: Question matching query does not exist.

    # 由於使用主鍵 (Primary Key) 搜尋資料庫是最常見的使用情境，
    # 因此 Django 提供了一個用主鍵能夠精確搜尋到你要的資料的捷徑。
    
    # 以下與 Question.objects.get(id=1) 是相同的。
    >>> Question.objects.get(pk=1)
    <Question: 目前什麼情況?>

    # 然後呼叫我們剛寫的函式方法來確認我們的自定方法有正常運作。
    >>> q = Question.objects.get(pk=1)
    >>> q.was_published_recently()
    True

    # 接下來我們會為問題 (Question) 建立幾個不同的選擇 (Choices)。 
    >>> q = Question.objects.get(pk=1)

    # 顯示相關物件集合的所有 choices -- 到目前為止還沒有。
    >>> q.choice_set.all()
    <QuerySet []>

    # 然後接著將建立三個選項 (Choices)。我們會呼叫 create 函式方法來
    # 構造一個新的 Choice 物件。它會執行 INSERT 語句，把選項 (choice)   
    # 新增到一個可在問題 (Question) 中選用的選項 (Choice)，並回傳
    # 新的 Choice 物件集合 (set)。Django 會建立用於儲存 ForeignKey 
    # 關係的 "相對面" 的集合(例如問題的選擇)，所以可以透過 API 進行存取。
    >>> q.choice_set.create(choice_text='Not much', votes=0)
    <Choice: Not much>
    >>> q.choice_set.create(choice_text='The sky', votes=0)
    <Choice: The sky>
    >>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

    # 這個 Choice 物件可以透過 API 存取其相關的 Question 物件。
    >>> c.question
    <Question: What's up?>

    # 反之亦然； Question 物件可以存取 Choice 物件。
    >>> q.choice_set.all()
    <QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
    >>> q.choice_set.count()
    3

    # API 會根據您的需要自動遵循關係。
    # 使用雙底線分隔關係。
    # 這可以根據需要進行任意深度的工作；沒有限制。
    # 查詢今年 pub_date 為任何 Question 的所有 Choice
    # (重複使用我們上面建立的 'current_year' 變數).
    >>> Choice.objects.filter(question__pub_date__year=current_year)
    <QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

    # 讓我們刪除其中一個選項。 為此我們使用 delete() 函式。
    >>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
    >>> c.delete()

閱讀
[開啟關聯物件](https://docs.djangoproject.com/zh-hans/3.0/ref/models/relations/)
文件可以獲取關於資料庫關聯的更多內容。想知道關於雙底線的更多用法，參見
[查找欄位](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/#field-lookups-intro)
文件。資料庫 API 的所有細節可以在 [資料庫 API
參考](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/)
文件中找到。

介紹 Django 管理頁面[¶](#introducing-the-django-admin "永久連結至標題")
-----------------------------------------------------------------------

設計哲學

為你的員工或客戶建立一個用戶增加、修改和刪除內容的管理頁面是一項缺乏創造性和乏味的工作。因此， Django 將
全自動地依據資料庫模型來建立管理界面。

Django 是誔生於一個使用者公開網頁和後台內容發佈者管理頁面完全分離的新聞類網站的開發過程中。網站的管理人員
使用管理系統來增加新聞、事件和體育時訊內容等，這些增加的內容被顯示在對使用者的公開網頁上。而 Django 透過
為網站管理人員建立統一的內容編輯界面來解決上述的問題。

管理界面是為網站管理者所提供的，而不是為了網站的一般公開瀏覽的使用者。

### 建立一個管理員帳號[¶](#creating-an-admin-user "永久連結至標題")

首先，我們得建立一個能登入管理者的用戶頁面。請執行下面的命令：

    $ python manage.py createsuperuser

輸入你想要使用的網站管理者名稱 (例如 admin)，然後按下 Enter 鍵：

    Username: admin

然後系統會提示你輸入網站管理者所使用的郵件地址：

    Email address: admin@example.com

最後一步是輸入管理者密碼。它會要求你輸入兩次密碼，第二次的目的是為了確認與第一次輸入密碼符合以確認這是你想要設定的密碼。

    Password: **********
    Password (again): *********
    Superuser created successfully.

### 啟動開發伺服器[¶](#start-the-development-server "永久連結至標題")

Django 的管理界面預設就是啟用的。讓我們啟動開發伺服器，看看它到底是什麼樣子。

如果開發伺服器未啟動，用以下命令啟動它：

    $ python manage.py runserver

現在，打開瀏覽器，指向到你本地網域名稱的 "/admin/" 目錄， -- 例如
"[http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)"
。你應該會看見管理員登入界面：

***

由於
[翻譯功能](https://docs.djangoproject.com/zh-hans/3.0/topics/i18n/translation/)
預設為打開狀態，因此如果您設定了 [`LANGUAGE_CODE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-LANGUAGE_CODE)，則登入畫面將會以指定的語言顯示(如果
Django 有 適當的翻譯)。我們在此章節最前面已提及過，在台灣台北時區，請設定 `TIME_ZONE = 'Asia/Taipei'` 以及 `LANGUAGE_CODE = 'zh-Hant'`。

### 進入管理網站頁面[¶](#enter-the-admin-site "永久連結至標題")

現在，試著使用你在上一步中建立的管理者用戶來登入。然後你將會看到 Django 管理頁面的索引頁：

***

你將會看到幾種可編輯的內容：使用者和群組。它們是由
[`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/3.0/topics/auth/#module-django.contrib.auth "django.contrib.auth: Django's authentication framework.")
提供的，這是 Django 開發的認證授權框架。

### 向管理頁面中加入投票應用程式[¶](#make-the-poll-app-modifiable-in-the-admin "永久連結至標題")

但是我們的投票應用程式在哪呢？它沒有顯示在索引頁面裡。

只需要再做一件事：我們得告訴管理員，問題 `Question` 物件需要一個管理介面。打開 `polls/admin.py` 文件，輸入下列的程式碼，將它編輯成下面這樣：

polls/admin.py[¶](#id6 "永久連結至程式")**

    from django.contrib import admin

    from .models import Question

    admin.site.register(Question)

### 體驗便捷的管理功能[¶](#explore-the-free-admin-functionality "永久連結至標題")

現在我們對管理頁面注冊了問題 `Question` 類別。Django 知道它應該被顯示在索引頁裡：

***

點選 "Questions" 。現在看到是問題 "Questions" 物件的列表 "change list"
。這個界面會顯示所有資料庫裡的問題 Question
物件，你可以選擇一個來修改。現在這裡有我們在前面建立的 “現在什麼情況?” 這個問題。

***

點選 “What's up?” 來編輯這個問題（Question）物件：

***

注意事項：

-   這個表單是從問題 `Question` 模型中自動產生的
-   不同的欄位類型（日期時間欄位 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")
    、字串欄位 [`CharField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField")）會產生對應的
    HTML 輸入控件。每個類型的欄位都知道它們該如何在管理頁面裡顯示自己。
-   每個日期時間欄位 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")
    都有 JavaScript
    寫的快捷按鈕。日期有顯示成今天（Today）的快捷按鈕和一個彈出式日曆界面。時間有設為現在（Now）的快捷按鈕和一個方便的列出常用時間的彈出式列表。

頁面的底部提供了幾個選項：

-   儲存（Save）- 儲存改變，然後回傳物件欄表。
-   儲存並繼續編輯（Save and continue editing） -
    儲存改變，然後重新載入當前物件的修改界面。
-   儲存並新增（Save and add another） -
    儲存改變，然後增加一個新的空物件並載入修改界面。
-   刪除（Delete）- 顯示一個確認刪除頁面。

如果顯示的 “發佈日期(Date Published)” 和你在 [教學
1](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
裡建立它們的時間不一致，這意味著你可能沒有正確的設定 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TIME_ZONE)
。改變設定，然後重新載入頁面看看是否顯示了正確的值。

透過點選 “今天(Today)” 和 “現在(Now)” 按鈕改變 “發佈日期 (Date Published)”。然後點選 “儲存並繼續編輯 (Save and add another)” 按鈕。然後點選右上角的
“歷史(History)”按鈕。你會看到一個列出了所有透過 Django 管理頁面對當前物件進行的改變的頁面，其中列出了時間戳記和進行修改操作的使用者：

***

當你熟悉了資料庫 API 之後，你就可以開始閱讀 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
，下一部分我們將會學習如何為投票應用程式增加更多視圖。

[** 編寫你的第一個 Django 應用程式，第 1
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)

[編寫你的第一個 Django 應用程式，第 3 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)


編寫你的第一個 Django 應用，第 3 部分[¶](#writing-your-first-django-app-part-3 "永久連結至標題")
================================================================================================

這一篇從 [教學第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
結尾的地方繼續講起。我們將繼續編寫投票應用，並且專注於如何建立公有界面 —
也被稱為“視圖”。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

概況[¶](#overview "永久連結至標題")
-----------------------------------

Django 中的「視圖」是 Django
應用程式中網頁的「樣式(type)」，通常具有特定功能並具有特定範本。例如，在一個部落格應用中，你可能會建立如下幾個視圖：

-   部落格首頁 — 顯示最近的幾項內容。
-   項目 “詳情” 頁 — 單個項目的永久連結頁面。
-   以年為單位的歸檔頁 — 顯示取得的年份裡各個月份建立的內容。
-   以月為單位的歸檔頁 — 顯示取得的月份裡各天建立的內容。
-   以天為單位的歸檔頁 — 顯示取得天裡建立的所有內容。
-   評論行為 — 用於對回應特定項目發布評論的處理。

而在我們的投票應用中，我們需要下欄幾個視圖：

-   問題 “索引” 頁 — 顯示最近的幾個投票問題。
-   問題 “詳情” 頁 — 顯示一個問題本文，不帶結果，但有一個投票表單。
-   問題 “結果” 頁 — 顯示某個投票的結果。
-   投票行為 — 用於對特定問題的特定選擇的投票處理。

在 Django 中，網頁和其他內容都是經由視圖表達的。每一個視圖均由一個
Python
函數（或者說方法，如果是在基於類別的視圖裡的話）來陳述內容。Django
將會根據使用者請求的 URL 來選擇使用哪個視圖（更準確的說，是根據 URL
中域名之後的部分）。

現在，在您上網的時候，您可能會遇到像是
`ME2/Sites/dirmod.htm?sid=&type=gen&mod=Core+Pages&gid=A6CD4967199A42D9B65B1B`之類優美語法。 您會很高興知道 Django
允許我們提供比這更優雅的 *URL 模式 (URL patterns)*。

URL模式是URL的一般形式 - 例如: `/newsarchive/<year>/<month>/`.

為了將 URL 和視圖關聯起來，Django 使用了 'URLconfs' 來設定。URLconf 將
URL 模式映射到視圖。

本教學只會介紹 URLconf 的基礎內容，你可以看看
[URL調度器](https://docs.djangoproject.com/zh-hans/3.0/topics/http/urls/)
以取得更多內容。

編寫更多視圖[¶](#writing-more-views "永久連結至標題")
-----------------------------------------------------

現在讓我們向 `polls/views.py`
裡增加更多視圖。這些視圖有一些不同，因為他們接收參數：

polls/views.py[¶](#id2 "永久連結至程式")**

    def detail(request, question_id):
        return HttpResponse("You're looking at question %s." % question_id)

    def results(request, question_id):
        response = "You're looking at the results of question %s."
        return HttpResponse(response % question_id)

    def vote(request, question_id):
        return HttpResponse("You're voting on question %s." % question_id)

把這些新視圖增加進 `polls.urls`
模組裡，只要增加幾個 [`url()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.conf.urls.url "django.conf.urls.url")
函數呼叫就行：

polls/urls.py[¶](#id3 "永久連結至程式")**

    from django.urls import path

    from . import views

    urlpatterns = [
        # ex: /polls/
        path('', views.index, name='index'),
        # ex: /polls/5/
        path('<int:question_id>/', views.detail, name='detail'),
        # ex: /polls/5/results/
        path('<int:question_id>/results/', views.results, name='results'),
        # ex: /polls/5/vote/
        path('<int:question_id>/vote/', views.vote, name='vote'),
    ]

然後看看你的瀏覽器，如果你轉到 "/polls/34/" ，Django 將會執行
`detail()` 方法並且顯示你在 URL
裡提供的問題 ID。再試試 "/polls/34/vote/" 和 "/polls/34/vote/" —
你將會看到暫時用於佔位的結果和投票頁。

當某人請求你網站的某一頁面時 — 例如像是， "/polls/34/" ，Django 將會載入
`mysite.urls` 模組，因為這在設定項
[`ROOT_URLCONF`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-ROOT_URLCONF)
中設定了。然後 Django 尋找名為 `urlpatterns` 變數並且按序比對正規表達式。在找到比對項
`'polls/'`，它切掉了比對的文字（`"polls/"`），將剩餘文字 — `"34/"`，發送至 'polls.urls' URLconf
做進一步處理。在這裡剩餘文字比對了 `'<int:question_id>/'`，使得我們 Django 以如下形式呼叫
`detail()`:

    detail(request=<HttpRequest object>, question_id=34)

`question_id=34` 由
`<int:question_id>`
比對產生。使用尖括號“擷取”這部分
URL，且以關鍵字參數的形式發送給視圖函數。上述字串的
`:question_id>`
部分定義了將被用於區分比對模式的變數名，而 `int:` 則是一個轉換器決定了應該以什麼變數類型比對這部分的 URL
路徑。

為每個 URL 加上不必要的東西，例如 `.html` ，是沒有必要的。不過如果你非要加的話，也是可以的:

    path('polls/latest.html', views.index),

但是，別這樣做，這太傻了。

寫一個真正有用的視圖[¶](#write-views-that-actually-do-something "永久連結至標題")
---------------------------------------------------------------------------------

每個視圖必須要做的只有兩件事中的其中一個：回傳一個包含被請求頁面內容的
[`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
物件，或者拋出一個類似於 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
的異常狀況，而剩下的就取決於你。

你的視圖可以從資料庫裡讀取記錄，可以使用一個範本引擎（例如 Django
內建的，或者其他第三方的），可以產生一個 PDF 文件，可以輸出一個
XML，建立一個 ZIP 文件，你可以做任何你想做的事，使用任何你想用的 Python
庫。

Django 只要求回傳的是一個 [`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
，或者拋出一個異常。

因為 Django 內建的資料庫 API 很方便，我們曾在 [教學第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
中學過，所以我們試試在視圖裡使用它。我們在 `index()`
函數裡插入了一些新內容，讓它能顯示資料庫裡以發布日期排序的最近 5
個投票問題，以空格分割：

polls/views.py[¶](#id4 "永久連結至程式")**

    from django.http import HttpResponse

    from .models import Question


    def index(request):
        latest_question_list = Question.objects.order_by('-pub_date')[:5]
        output = ', '.join([q.question_text for q in latest_question_list])
        return HttpResponse(output)

    # Leave the rest of the views (detail, results, vote) unchanged

這裡有個問題：頁面的設計直接以程式碼寫在視圖函數的程式裡的。如果你想改變頁面的樣子，你需要編輯
Python 程式。所以讓我們使用 Django
的範本系統，只要建立一個視圖，就可以將頁面的設計從程式中分離出來。

首先，在你的 `polls` 目錄裡建立一個
`templates` 目錄。Django
將會在這個目錄裡尋找範本文件。

你專案的 [`TEMPLATES`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES)
設定項描述了 Django 如何載入和實現範本。預設的設定文件設定了
`DjangoTemplates` 後端，並將
[`APP_DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-APP_DIRS)
設定成了 True。這一選項將會讓 `DjangoTemplates` 在每個 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
文件夾中尋找 "templates"
子目錄。這就是為什麼儘管我們沒有像在第二部分中那樣修改 DIRS 設定，Django
也能正確找到 polls 的範本位置的原因。

在你剛剛建立的 `templates`
目錄裡，再建立一個目錄 `polls`，然後在其中新建一個文件 `index.html` 。換句話說，你的範本文件的路徑應該是
`polls/templates/polls/index.html`
。因為 `app_directories`
範本載入器是透過上述描述的方法執行的，所以 Django 可以引用得到
`polls/index.html` 這一範本了。

範本命名空間

雖然我們現在可以將範本文件直接放在 `polls/templates` 文件夾中（而不是再建立一個 `polls` 子文件夾），但是這樣做不太好。Django
將會選擇第一個比對的範本文件，如果你有一個範本文件正好和另一個應用中的某個範本文件重名，Django
沒有辦法 *區分* 它們。我們需要協助 Django
選擇正確的範本，最好的方法就是把他們放入各自的 *命名空間*
中，也就是把這些範本放入一個和 *自身* 應用重名的子文件夾裡。

將下面的程式輸入到剛剛建立的範本文件中：

polls/templates/polls/index.html[¶](#id5 "永久連結至程式")**

    {% if latest_question_list %}
        <ul>
        {% for question in latest_question_list %}
            <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
        {% endfor %}
        </ul>
    {% else %}
        <p>No polls are available.</p>
    {% endif %}

注解

為了讓教學看起來不那麼長，所有的範本文件都只寫出了核心程式。在你自己建立的專案中，你應該使用
[完整的 HTML
文件](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Getting_started#Anatomy_of_an_HTML_document).

然後，讓我們更新一下 `polls/views.py`
裡的 `index` 視圖來使用範本：

polls/views.py[¶](#id6 "永久連結至程式")**

    from django.http import HttpResponse
    from django.template import loader

    from .models import Question


    def index(request):
        latest_question_list = Question.objects.order_by('-pub_date')[:5]
        template = loader.get_template('polls/index.html')
        context = {
            'latest_question_list': latest_question_list,
        }
        return HttpResponse(template.render(context, request))

上述程式的作用是，載入 `polls/index.html`
範本文件，並且向它傳遞一個上下文(context)。這個上下文是一個字典，它將範本內的變數映射為
Python 物件。

用你的瀏覽器開啟 "/polls/" ，你將會看見一個無序清單，列出了我們在
[教學第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
中增加的 “What's up” 投票問題，連結指向這個投票的詳情頁。

### 一個快捷函數： [`render()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.render "django.shortcuts.render")[¶](#a-shortcut-render "永久連結至標題")

載入範本、填入上下文再回傳由它產生的 [`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
物件是一個非常常用的操作流程。於是 Django
提供了一個快捷函數，我們用它來重寫 `index()` 視圖：

polls/views.py[¶](#id7 "永久連結至程式")**

    from django.shortcuts import render

    from .models import Question


    def index(request):
        latest_question_list = Question.objects.order_by('-pub_date')[:5]
        context = {'latest_question_list': latest_question_list}
        return render(request, 'polls/index.html', context)

注意到，我們不再需要匯入 [`loader`](https://docs.djangoproject.com/zh-hans/3.0/topics/templates/#module-django.template.loader "django.template.loader")
和 [`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
（不過如果你還有其他函數例如像是 `detail`, `results`, 和
`vote` 需要用到它的話，就需要保留匯入
`HttpResponse` ）。

此 [`render()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.render "django.shortcuts.render")
函數將對網頁的請求物件做為其第一個參數、一個範本名稱做為第二個參數，以及一個字典做為選擇性的第三個參數。它會將你所傳遞的上下文實現在範本後回傳一個
[`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")
物件。

拋出 404 錯誤[¶](#raising-a-404-error "永久連結至標題")
-------------------------------------------------------

現在，我們來處理投票詳情視圖 —
它會顯示指定投票的問題本文。下面是這個視圖的程式：

polls/views.py[¶](#id8 "永久連結至程式")**

    from django.http import Http404
    from django.shortcuts import render

    from .models import Question
    # ...
    def detail(request, question_id):
        try:
            question = Question.objects.get(pk=question_id)
        except Question.DoesNotExist:
            raise Http404("Question does not exist")
        return render(request, 'polls/detail.html', {'question': question})

這裡的新觀念是：如果指定問題 ID 所對應的問題不存在，這個視圖就會拋出一個
[`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
異常。

我們稍後再討論你需要在 `polls/detail.html`
裡輸入什麼，但是如果你想試試上面這段程式是否正常運作的話，你可以暫時把下面這段寫進去：

polls/templates/polls/detail.html[¶](#id9 "永久連結至程式")**

    {{ question }}

這樣你就能測試了。

### 一個快捷函數： [`get_object_or_404()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.get_object_or_404 "django.shortcuts.get_object_or_404")[¶](#a-shortcut-get-object-or-404 "永久連結至標題")

嘗試用 [`get()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/querysets/#django.db.models.query.QuerySet.get "django.db.models.query.QuerySet.get")
函數取得一個物件，如果不存在就拋出 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
錯誤也是一個普遍的流程。Django 也提供了一個快捷函數，下面是修改後的詳情
`detail()` 視圖程式：

polls/views.py[¶](#id10 "永久連結至程式")**

    from django.shortcuts import get_object_or_404, render

    from .models import Question
    # ...
    def detail(request, question_id):
        question = get_object_or_404(Question, pk=question_id)
        return render(request, 'polls/detail.html', {'question': question})

此 [`get_object_or_404()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.get_object_or_404 "django.shortcuts.get_object_or_404")
函數將 Django
模型作為第一個參數，並將任意數量的關鍵字參數傳遞給模型管理器的
[`get()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/querysets/#django.db.models.query.QuerySet.get "django.db.models.query.QuerySet.get")
函數。 如果物件不存在，則會引發 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
異常狀況。

設計哲學

為什麼我們使用輔助函數 [`get_object_or_404()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.get_object_or_404 "django.shortcuts.get_object_or_404")
而不是自己擷取 [`ObjectDoesNotExist`](https://docs.djangoproject.com/zh-hans/3.0/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist "django.core.exceptions.ObjectDoesNotExist")
異常呢？還有，為什麼模型 API 不直接拋出 [`ObjectDoesNotExist`](https://docs.djangoproject.com/zh-hans/3.0/ref/exceptions/#django.core.exceptions.ObjectDoesNotExist "django.core.exceptions.ObjectDoesNotExist")
而是拋出 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
呢？

因為那樣會將模型層耦合到視圖層。在 Django
的首要設計目標之一就是保持鬆散耦合。有些受控管的耦合則於
[`django.shortcuts`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#module-django.shortcuts "django.shortcuts: Convenience shortcuts that span multiple levels of Django's MVC stack.")
模組中採用。

另外也有 [`get_list_or_404()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.get_list_or_404 "django.shortcuts.get_list_or_404")
函數，除了 [`get()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/querysets/#django.db.models.query.QuerySet.get "django.db.models.query.QuerySet.get")
函數被換成了 [`filter()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/querysets/#django.db.models.query.QuerySet.filter "django.db.models.query.QuerySet.filter")
函數，工作原理和 [`get_object_or_404()`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/shortcuts/#django.shortcuts.get_object_or_404 "django.shortcuts.get_object_or_404")
一樣。如果清單為空的話會拋出 [`Http404`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/views/#django.http.Http404 "django.http.Http404")
異常。

使用範本系統[¶](#use-the-template-system "永久連結至標題")
----------------------------------------------------------

回過頭去看看我們的 `detail()`
視圖。它向範本傳遞了上下文變數 `question` 。下面是 `polls/detail.html` 範本裡正式的程式：

polls/templates/polls/detail.html[¶](#id11 "永久連結至程式")**

    <h1>{{ question.question_text }}</h1>
    <ul>
    {% for choice in question.choice_set.all %}
        <li>{{ choice.choice_text }}</li>
    {% endfor %}
    </ul>

在範本系統中使用點符號來存取變數的屬性。在範例
`{{ question.question_text }}` 中，首先
Django 嘗試對 `question`
物件使用字典尋找（也就是使用 obj.get(str)
作業），如果失敗了就嘗試屬性尋找（也就是 obj.str
作業），結果是成功了。如果這一作業也失敗的話，將會嘗試清單尋找（也就是
obj[int] 操作）。

方法（函數）呼叫發生在 [`{% for %}`](https://docs.djangoproject.com/zh-hans/3.0/ref/templates/builtins/#std:templatetag-for)
循環中：`question.choice_set.all`
被解釋為 Python 程式 `question.choice_set.all()`，它會回傳一個可迭代的 `Choice`](https://docs.djangoproject.com/zh-hans/3.0/ref/templates/builtins/#std:templatetag-for)
標籤內部使用。

查看
[範本指南](https://docs.djangoproject.com/zh-hans/3.0/topics/templates/)
可以了解關於範本的更多資訊。

去除範本中用程式直接編寫的 URL[¶](#removing-hardcoded-urls-in-templates "永久連結至標題")
-----------------------------------------------------------------------------------------

還記得嗎，我們在 `polls/index.html`
裡編寫投票連結時，連結是用程式直接編寫的：

    <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>

問題在於，這樣用程式直接編寫成緊密耦合的超連結，對於一個包含很多應用的專案來說，修改這樣的
URLs 是十分困難的。不過，由於你在 `polls.urls` 的 [`url()`](https://docs.djangoproject.com/zh-hans/3.0/ref/urls/#django.conf.urls.url "django.conf.urls.url")
函數中透過 name 參數為 URL 定義了名字，你可以使用 `{% url %}` 標籤替代它：

    <li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>

它的工作方式是透過查詢 `polls.urls`
模組中指定的 URL 定義來完成的。你可以在下面看到具有 'detail' 名稱的 URL
的明確定義：

    ...
    # 由 {% url %} 範本標籤呼叫使用的 'name' 值
    path('<int:question_id>/', views.detail, name='detail'),
    ...

如果你想改變投票詳情視圖的 URL，例如想改成
`polls/specifics/12/`
，你不用在範本裡修改任何東西（包括其它範本），只要在
`polls/urls.py` 裡稍微修改一下就行：

    ...
    # 新增 'specifics' 這個字
    path('specifics/<int:question_id>/', views.detail, name='detail'),
    ...

為 URL 名稱增加命名空間[¶](#namespacing-url-names "永久連結至標題")
-------------------------------------------------------------------

教學專案只有一個應用，`polls`
。在一個真實的 Django
專案中，可能會有五個，十個，二十個，甚至更多應用。Django 如何分辨重名的
URL 呢？舉個例子，`polls` 應用有
`detail`
視圖，可能另一個部落格應用也有同名的視圖。Django 如何知道
`{% url %}` 標籤到底對應哪一個應用的
URL 呢？

答案是：在根 URLconf 中增加命名空間。在 `polls/urls.py` 文件中稍作修改，加上 `app_name` 設定命名空間：

polls/urls.py[¶](#id12 "永久連結至程式")**

    from django.urls import path

    from . import views

    app_name = 'polls'
    urlpatterns = [
        path('', views.index, name='index'),
        path('<int:question_id>/', views.detail, name='detail'),
        path('<int:question_id>/results/', views.results, name='results'),
        path('<int:question_id>/vote/', views.vote, name='vote'),
    ]

現在，編輯 `polls/index.html`
文件，從：

polls/templates/polls/index.html[¶](#id13 "永久連結至程式")**

    <li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>

修改為指向具有命名空間的詳細視圖：

polls/templates/polls/index.html[¶](#id14 "永久連結至程式")**

    <li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>

當你對你寫的視圖感到滿意後，請閱讀 [教學的第 4
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)
了解基礎的表單處理和通用視圖。

[** 編寫你的第一個 Django 應用，第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)

[編寫你的第一個 Django 應用，第 4 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)


編寫你的第一個 Django 應用，第 4 部分[¶](#writing-your-first-django-app-part-4 "永久連結至標題")
================================================================================================

這一篇從 [教學第 3 部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
結尾的地方繼續講起。我們將繼續編寫投票應用，專注於表單處理並且精簡我們的程式。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

編寫一個簡單的表單[¶](#write-a-minimal-form "永久連結至標題")
-------------------------------------------------------------

讓我們更新一下在上一個教學中編寫的投票詳細頁面的範本
("polls/detail.html") ，讓它包含一個 HTML `<form>` 元素：

polls/templates/polls/detail.html[¶](#id1 "永久連結至程式")**

    <h1>{{ question.question_text }}</h1>

    {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}

    <form action="{% url 'polls:vote' question.id %}" method="post">
    {% csrf_token %}
    {% for choice in question.choice_set.all %}
        <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
        <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
    {% endfor %}
    <input type="submit" value="Vote">
    </form>

很簡單地概要說明：

-   上面的範本在 Question 的每個 Choice 前增加一個單選按鈕。
    每個單選按鈕的 `value`是`"choice"`，其中 \#
    為選擇的 Choice 的 ID。這是 HTML 表單的基本概念。
-   我們設定表單的 `         action     `，並設定
    `         method="post"     `
    ）是非常重要的，因為這個提交表單的行為會改變伺服器端的資料。無論何時，當你需要建立一個改變伺服器端資料的表單時，使用
    `method="post"` 指示 [`for`](https://docs.djangoproject.com/zh-hans/3.0/ref/templates/builtins/#std:templatetag-for)
    標籤已經循環多少次。
-   由於我們建立一個 POST
    表單（它具有修改資料的作用），所以我們需要小心跨網站請求偽造。這就要感謝由於
    Django
    內建了一個非常有用的防禦系統，因此你並不需要太過擔心。簡而言之，所有針對內部
    URL 的 POST 表單都應該使用 [`{% csrf_token %}`](https://docs.djangoproject.com/zh-hans/3.0/ref/templates/builtins/#std:templatetag-csrf_token)
    範本標籤。

現在，讓我們來建立一個 Django 視圖來處理提交的資料。記住，在 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
中，我們為投票應用建立了一個 URLconf ，包含這一行：

polls/urls.py[¶](#id2 "永久連結至程式")**

    path('<int:question_id>/vote/', views.vote, name='vote'),

我們還建立了一個 `vote()` ：

polls/views.py[¶](#id3 "永久連結至程式")**

    from django.http import HttpResponse, HttpResponseRedirect
    from django.shortcuts import get_object_or_404, render
    from django.urls import reverse

    from .models import Choice, Question
    # ...
    def vote(request, question_id):
        question = get_object_or_404(Question, pk=question_id)
        try:
            selected_choice = question.choice_set.get(pk=request.POST['choice'])
        except (KeyError, Choice.DoesNotExist):
            # 重新顯示問題投票表單。
            return render(request, 'polls/detail.html', {
                'question': question,
                'error_message': "You didn't select a choice.",
            })
        else:
            selected_choice.votes += 1
            selected_choice.save()
            # 在成功處理 POST 資料後，緊跟著就回應 HttpResponseRedirect。
            # 這樣可以防止如果使用者按下 "上一頁" 按鈕時，
            # 資料會傳送兩次的情況。
            return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))

以上程式中有些內容還未在本教學中提到過：

-   [`request.POST`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest.POST "django.http.HttpRequest.POST")
    是一個類字典物件，讓你可以透過關鍵字的名字取得提交的資料。
    這個例子中， `request.POST['choice']` 以字串形式回傳選擇的 Choice 的 ID。
    [`request.POST`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest.POST "django.http.HttpRequest.POST")
    的值永遠是字串。

    注意，Django 還以同樣的方式提供 [`request.GET`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest.GET "django.http.HttpRequest.GET")
    用於開啟 GET 資料 — 但我們在程式中明確地使用 [`request.POST`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest.POST "django.http.HttpRequest.POST")
    ，以保證資料只能透過 POST 呼叫改動。

-   如果在 `request.POST['choice']` ， POST
    將引發一個 [`KeyError`](https://docs.python.org/3/library/exceptions.html#KeyError "(在 Python v3.8)")
    。上面的程式檢查 [`KeyError` 將重新顯示
    Question 表單和一個錯誤資訊。

-   在增加 Choice 的得票數之後，程式回傳一個
    [`HttpResponseRedirect`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponseRedirect "django.http.HttpResponseRedirect")
    而不是常用的 [`HttpResponse`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponse "django.http.HttpResponse")。[`HttpResponseRedirect`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponseRedirect "django.http.HttpResponseRedirect")
    只接收一個參數：用戶將要被重定向的
    URL（請繼續看下去，我們將會解釋如何構造這個例子中的 URL）。

    正如上面的 Python 註釋所指出的那樣，在成功處理 POST
    資料後，你應該總是返回一個 [`HttpResponseRedirect`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponseRedirect "django.http.HttpResponseRedirect")。這個技巧並非專門針對
    Django；總體而言，這是一項很好的 Web 開發實踐。

-   在這個例子中，我們在 [`HttpResponseRedirect`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpResponseRedirect "django.http.HttpResponseRedirect")
    的構造函數中使用 `reverse()` 函數。這個函數避免了我們在視圖函數中用程式直接編寫
    URL。它需要我們給出我們想要跳轉的視圖的名字和該視圖所對應的 URL
    模式中需要給該視圖提供的參數。 在本例中，使用在 [教學第 3
    部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
    中設定的 URLconf，`reverse()` 呼叫將回傳一個這樣的字串：

        '/polls/3/results/'

    其中 `3` 視圖來顯示最終的頁面。

正如在 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
中提到的，[`HttpRequest`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest")
是一個 [`HttpRequest`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest")
物件。更多關於 [`HttpRequest`](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/#django.http.HttpRequest "django.http.HttpRequest")
物件的內容，請參見
[請求和回應的文件](https://docs.djangoproject.com/zh-hans/3.0/ref/request-response/)
。

當有人對 Question 進行投票後， `vote()`
視圖將請求重定向到 Question 的結果界面。讓我們來編寫這個視圖：

polls/views.py[¶](#id4 "永久連結至程式")**

    from django.shortcuts import get_object_or_404, render


    def results(request, question_id):
        question = get_object_or_404(Question, pk=question_id)
        return render(request, 'polls/results.html', {'question': question})

這和 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/) 中的
`detail()`
視圖幾乎一模一樣。唯一的不同是範本的名字。
我們將在稍後解決這個重複項目。

現在，建立一個 `polls/results.html`
範本：

polls/templates/polls/results.html[¶](#id5 "永久連結至程式")**

    <h1>{{ question.question_text }}</h1>

    <ul>
    {% for choice in question.choice_set.all %}
        <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
    {% endfor %}
    </ul>

    <a href="{% url 'polls:detail' question.id %}">Vote again?</a>

現在，在你的瀏覽器中開啟 `/polls/1/`
然後為 Question
投票。你應該看到一個投票結果頁面，並且在你每次投票之後都會更新。
如果你提交時沒有選擇任何 Choice，你應該看到錯誤資訊。

注解

我們的 `vote()` 物件，接著計算
`vote`
回傳。然後，對於兩個使用者，新值43計算完畢，並被儲存，但是期望值是44。

這個問題被稱為 *競爭條件* 。如果你對此有興趣，你可以閱讀 [Avoiding race
conditions using
F()](https://docs.djangoproject.com/zh-hans/3.0/ref/models/expressions/#avoiding-race-conditions-using-f)
來學習如何解決這個問題。

使用通用視圖：程式還是少點好[¶](#use-generic-views-less-code-is-better "永久連結至標題")
----------------------------------------------------------------------------------------

`detail()` 視圖都很精簡 —
並且，像上面提到的那樣，存在重複項目。用來顯示一個投票欄表的
`index()` 視圖（也在 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
中）和它們類似。

這些視圖反映基本的 Web 開發中的一個常見情況：根據 URL
中的參數從資料庫中取得資料、載入範本文件然後回傳實現後的範本。
由於這種情況特別常見，Django 提供一種快捷方式，叫做 “通用視圖” 系統。

通用視圖將常見的模式抽象化，可以使你在編寫應用時甚至不需要編寫 Python
程式。

讓我們將我們的投票應用轉換成使用通用視圖系統，這樣我們可以刪除許多我們的程式。我們僅僅需要做以下幾步來完成轉換，我們將：

1.  轉換 URLconf。
2.  刪除一些舊的、不再需要的視圖。
3.  基於 Django 的通用視圖引入新的視圖。

請繼續閱讀來了解詳細資訊。

為什麼要重構程式？

一般來說，當編寫一個 Django
應用時，你應該先評估一下通用視圖是否可以解決你的問題，你應該在一開始使用它，而不是進行到一半時重構程式。本教學目前為止是有意將重點放在以
“艱難的方式” 編寫視圖，這是為了將重點放在核心概念上。

就像在使用計算機之前你需要掌握基礎數學一樣。

### 改良 URLconf[¶](#amend-urlconf "永久連結至標題")

首先，打開 `polls/urls.py` 這個 URLconf
並將它修改成：

polls/urls.py[¶](#id6 "永久連結至程式")**

    from django.urls import path

    from . import views

    app_name = 'polls'
    urlpatterns = [
        path('', views.IndexView.as_view(), name='index'),
        path('<int:pk>/', views.DetailView.as_view(), name='detail'),
        path('<int:pk>/results/', views.ResultsView.as_view(), name='results'),
        path('<int:question_id>/vote/', views.vote, name='vote'),
    ]

注意，第二個和第三個比對準則中，路徑字串中比對模式的名稱已經由
`<question_id>`。

### 改良視圖[¶](#amend-views "永久連結至標題")

下一步，我們將刪除舊的 `index` 視圖，並用 Django 的通用視圖代替。打開
`polls/views.py` 文件，並將它修改成：

polls/views.py[¶](#id7 "永久連結至程式")**

    from django.http import HttpResponseRedirect
    from django.shortcuts import get_object_or_404, render
    from django.urls import reverse
    from django.views import generic

    from .models import Choice, Question


    class IndexView(generic.ListView):
        template_name = 'polls/index.html'
        context_object_name = 'latest_question_list'

        def get_queryset(self):
            """傳回最後五個發佈的問題。"""
            return Question.objects.order_by('-pub_date')[:5]


    class DetailView(generic.DetailView):
        model = Question
        template_name = 'polls/detail.html'


    class ResultsView(generic.DetailView):
        model = Question
        template_name = 'polls/results.html'


    def vote(request, question_id):
        ... # 沒有需要變更，和上面的內容相同。

我們在這裡使用兩個通用視圖： [`ListView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.list.ListView "django.views.generic.list.ListView")
和 [`DetailView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView "django.views.generic.detail.DetailView")
。這兩個視圖分別抽象 “顯示一個物件欄表” 和
“顯示一個特定類型物件的詳細資訊頁面” 這兩種概念。

-   每個通用視圖需要知道它將作用於哪個模型。 這由 `model` 屬性提供。
-   [`DetailView`
    的主鍵值，所以我們為通用視圖把 `question_id` 。

預設情況下，通用視圖 [`DetailView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView "django.views.generic.detail.DetailView")
使用一個叫做 `<app name>/<model name>_detail.html`
範本。`template_name` 欄表視圖指定了
`template_name` — 這確保 results 視圖和
detail 視圖在實現時具有不同的外觀，即使它們在底層都是使用同一個
[`DetailView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.detail.DetailView "django.views.generic.detail.DetailView")
。

類似地，[`ListView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.list.ListView "django.views.generic.list.ListView")
使用一個叫做 `<app name>/<model name>_list.html` 來告訴 [`ListView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.list.ListView "django.views.generic.list.ListView")
使用我們建立的已經存在的 `"polls/index.html"` 範本。

在之前的教學中，提供範本文件時都帶有一個包含 `question` 變數的 context。對於 `DetailView`
變數會自動提供 — 因為我們使用 Django 的模型 (Question)，Django 能夠為
context 變數決定一個合適的名字。然而對於 ListView，自動產生的 context
變數是 `question_list`
屬性，表示我們想使用 `latest_question_list`。作為一種替換方案，你可以改變你的範本來比對新的 context
變數 — 這是一種更便捷的方法，告訴 Django 使用你想使用的變數名。

啟動伺服器，使用一下基於通用視圖的新投票應用。

更多關於通用視圖的詳細資訊，請查看
[通用視圖的文件](https://docs.djangoproject.com/zh-hans/3.0/topics/class-based-views/)

當你對你所寫的表單和通用視圖感到滿意後，請閱讀 [教學的第 5
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial05/)
來了解如何測試我們的投票應用。

[** 編寫你的第一個 Django 應用，第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)

[編寫你的第一個 Django 應用，第 5 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial05/)

編寫你的第一個 Django 應用，第 5 部分[¶](#writing-your-first-django-app-part-5 "永久連結至標題")
================================================================================================

這一篇從 [教學第 4
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)
結尾的地方繼續講起。我們在前幾章成功的構建了一個在線投票應用，在這一部分裡我們將為它建立一些自動化測試。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

自動化測試簡介[¶](#introducing-automated-testing "永久連結至標題")
------------------------------------------------------------------

### 自動化測試是什麼？[¶](#what-are-automated-tests "永久連結至標題")

測試程式，是用來檢查你的程式能否正常執行的程式。

測試在不同的層次中都存在。有些測試只說明某個很小的細節（某個模型的某個方法的回傳值是否滿足預期？），而另一些測試可能檢查對某個軟體的一系列操作（*某一用戶輸入序列是否造成了預期的結果？*）。其實這和我們在
[教學第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)，裡做的並沒有什麼不同，我們使用
[`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)
來測試某一方法的功能，或者執行某個應用並輸入資料來檢查它的行為。

真正不同的地方在於，*自動化*
測試是由某個系統幫你自動完成的。當你建立好了一系列測試，每次修改應用程式後，就可以自動檢查出修改後的程式是否還像你曾經預期的那樣正常工作。你不需要花費大量時間來進行手動測試。

### 為什麼你需要寫測試[¶](#why-you-need-to-create-tests "永久連結至標題")

但是，為什麼需要測試呢？又為什麼是現在呢？

你可能覺得學 Python/Django
對你來說已經很滿足了，再學一些新東西的話看起來有點負擔過重並且沒什麼必要。畢竟，我們的投票應用看起來已經完美工作了。寫一些自動測試並不能讓它工作的更好。如果寫一個投票應用是你想用
Django
完成的唯一工作，那你確實沒必要學寫測試。但是如果你還想寫更複雜的專案，現在就是學習測試寫法的最好時機了。

#### 測試將節約你的時間[¶](#tests-will-save-you-time "永久連結至標題")

在某種程度上，能夠「判斷出程式是否正常工作」的測試，就稱得上是個令人滿意的了。在更複雜的應用程式中，組件之間可能會有數十個複雜的交互。

對其中某一組件的改變，也有可能會造成意想不到的結果。判斷「程式是否正常工作」意味著你需要用大量的資料來完整的測試全部程式的功能，以確保你的小修改沒有對應用整體造成破壞
— 這太費時間了。

尤其是當你發現自動化測試能在幾秒鍾之內幫你完成這件事時，就更會覺得手動測試實在是太浪費時間了。當某人寫出錯誤的程式時，自動化測試還能協助你定位錯誤程式的位置。

有時候你會覺得，和富有創造性和生產力的業務程式比起來，編寫枯燥的測試程式實在是太無聊了，特別是當你知道你的程式完全沒有問題的時候。

然而，編寫測試還是要比花費幾個小時手動測試你的應用，或者為了找到某個小錯誤而胡亂翻看程式要有意義的多。

#### 測試不僅能發現錯誤，而且能預防錯誤[¶](#tests-don-t-just-identify-problems-they-prevent-them "永久連結至標題")

「測試是開發的對立面」，這種思想是不對的。

如果沒有測試，整個應用的行為意圖會變得更加的不清晰。甚至當你在看自己寫的程式時也是這樣，有時候你需要仔細研讀一段程式才能搞清楚它有什麼用。

而測試的出現改變了這種情況。測試就好像是從內部仔細檢查你的程式，當有些地方出錯時，這些地方將會變得很顯眼
— *就算你自己沒有意識到那裡寫錯了*。

#### 測試使你的程式更有吸引力[¶](#tests-make-your-code-more-attractive "永久連結至標題")

你也許遇到過這種情況：你編寫了一個絕贊的軟體，但是其他開發者看都不看它一眼，因為它缺少測試。沒有測試的程式不值得信任。
Django 最初開發者之一的 Jacob Kaplan-Moss
說過：“專案規劃時沒有包含測試是不科學的。”

其他的開發者希望在正式使用你的程式前先看你的程式的測試，這是你需要寫測試的另一個重要原因。

#### 測試有利於團隊協作[¶](#tests-help-teams-work-together "永久連結至標題")

前面的幾點都是從單人開發的角度來說的。複雜的應用可能由團隊維護。測試的存在保證了同事不會不小心破壞了了你的程式（也保證你不會不小心弄壞他們的）。如果你想作為一個
Django 程式員謀生的話，你必須擅長編寫測試！

基礎測試策略[¶](#basic-testing-strategies "永久連結至標題")
-----------------------------------------------------------

有好幾種不同的方法可以寫測試。

一些開發者遵循
"[測試驅動](https://en.wikipedia.org/wiki/Test-driven_development)"
的開發原則，他們在寫程式之前先寫測試。這種方法看起來有點反直覺，但事實上，這和大多數人日常的做法是相吻合的。我們會先描述一個問題，然後寫程式來解決它。「測試驅動」的開發方法只是將問題的描述抽象為了
Python 的測試樣例。

更普遍的情況是，一個剛接觸自動化測試的新手更傾向於先寫程式，然後再寫測試。雖然提前寫測試可能更好，但是晚點寫並不算太遲。

有時候很難決定從哪裡開始下手寫測試。如果你才寫了幾千行 Python
程式，選擇從哪裡開始寫測試確實不怎麼簡單。如果是這種情況，那麼在你下次修改程式（例如加新功能，或者修正
Bug）之前寫個測試是比較合理且有效的。

所以，我們現在就開始寫吧。

開始寫我們的第一個測試[¶](#writing-our-first-test "永久連結至標題")
-------------------------------------------------------------------

### 首先得有個 Bug[¶](#we-identify-a-bug "永久連結至標題")

幸運的是，我們的 `polls`
應用現在就有一個小 bug 需要被修正：我們的要求是如果 Question
是在一天之內發佈的， `Question.was_published_recently()` ，然而現在這個方法在 `Question`
欄位比當前時間還晚時也會回傳 True（這是個 Bug）。

用[`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)``命令確認一下這個方法的日期的 bug：

/ 

    $ python manage.py shell

    >>> import datetime
    >>> from django.utils import timezone
    >>> from polls.models import Question
    >>> # 建立一個　pub_date 在未來 30 天的　Question　實例
    >>> future_question = Question(pub_date=timezone.now() + datetime.timedelta(days=30))
    >>> # 是最近發佈的嗎？?
    >>> future_question.was_published_recently()
    True

因為將來發生的是肯定不是最近發生的，所以程式明顯是錯誤的。

### 建立一個測試來暴露這個 bug[¶](#create-a-test-to-expose-the-bug "永久連結至標題")

我們剛剛在 [`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)
裡做的測試也就是自動化測試應該做的工作。所以我們來把它改寫成自動化的吧。

按照慣例，Django 應用的測試應該寫在應用的 `tests.py` 開頭的文件裡尋找並執行測試程式。

將下面的程式寫入 `polls` 文件內：

polls/tests.py[¶](#id1 "永久連結至程式")**

    import datetime

    from django.test import TestCase
    from django.utils import timezone

    from .models import Question


    class QuestionModelTests(TestCase):

        def test_was_published_recently_with_future_question(self):
            """
            was_published_recently() 對於 questions 的 pub_date　是在未來
            會回傳 False。 
            """
            time = timezone.now() + datetime.timedelta(days=30)
            future_question = Question(pub_date=time)
            self.assertIs(future_question.was_published_recently(), False)

我們建立了一個 [`django.test.TestCase`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.TestCase "django.test.TestCase")
的子類別，並增加了一個方法，此方法建立一個 `pub_date`
實例。然後檢查它的 `was_published_recently()` 方法的回傳值 — 它 *應該* 是 False。

### 執行測試[¶](#running-tests "永久連結至標題")

在終端中，我們透過輸入以下程式執行測試:

/ 

    $ python manage.py test polls

你將會看到執行結果:

    Creating test database for alias 'default'...
    System check identified no issues (0 silenced).
    F
    ======================================================================
    FAIL: test_was_published_recently_with_future_question (polls.tests.QuestionModelTests)
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "/path/to/mysite/polls/tests.py", line 16, in test_was_published_recently_with_future_question
        self.assertIs(future_question.was_published_recently(), False)
    AssertionError: True is not False

    ----------------------------------------------------------------------
    Ran 1 test in 0.001s

    FAILED (failures=1)
    Destroying test database for alias 'default'...

不一樣的錯誤？

若在此處你得到了一個 `NameError`
錯誤，你可能漏了
[第二步](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/#tutorial02-import-timezone)
中將 `datetime` 的步驟。復制這些語句，然後試著重新執行測試。

發生了什麼呢？以下是自動化測試的執行過程：

-   `python manage.py test polls` 應用裡的測試程式
-   它找到了 [`django.test.TestCase`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.TestCase "django.test.TestCase")
    的一個子類別
-   它建立一個特殊的資料庫供測試使用
-   它在類別中尋找測試方法 — 以 `test`
    開頭的方法。
-   在 `test_was_published_recently_with_future_question` 值為 30 天後的 `Question` 方法，發現
    `was_published_recently()`。

測試系統通知我們哪些測試樣例失敗了，和造成測試失敗的程式所在的行號。

### 修正這個 bug[¶](#fixing-the-bug "永久連結至標題")

我們早已知道，當 `pub_date` 應該回傳 `False`
裡的方法，讓它只在日期是過去式的時候才回傳 `True`：

polls/models.py[¶](#id2 "永久連結至程式")**

    def was_published_recently(self):
        now = timezone.now()
        return now - datetime.timedelta(days=1) <= self.pub_date <= now

然後重新執行測試:

    Creating test database for alias 'default'...
    System check identified no issues (0 silenced).
    .
    ----------------------------------------------------------------------
    Ran 1 test in 0.001s

    OK
    Destroying test database for alias 'default'...

發現 bug 後，我們編寫了能夠暴露這個 bug 的自動化測試。在修正 bug
之後，我們的程式順利的透過了測試。

將來，我們的應用可能會出現其他的問題，但是我們可以肯定的是，一定不會再次出現這個
bug，因為只要執行一遍測試，就會立刻收到警告。我們可以認為應用的這一小部分程式永遠是安全的。

### 更全面的測試[¶](#more-comprehensive-tests "永久連結至標題")

我們已經搞定一小部分了，現在可以考慮全面的測試
`was_published_recently()`
這個方法以確定它的安全性，然後就可以把這個方法穩定下來了。事實上，在修正一個
bug 時不小心引入另一個 bug 會是非常令人尷尬的。

我們在上次寫的類別裡再增加兩個測試，來更全面的測試這個方法：

polls/tests.py[¶](#id3 "永久連結至程式")**

    def test_was_published_recently_with_old_question(self):
        """
        was_published_recently() 對於 questions 的 pub_date 是比 1 天還早(older)的
        會回傳 False。
        """
        time = timezone.now() - datetime.timedelta(days=1, seconds=1)
        old_question = Question(pub_date=time)
        self.assertIs(old_question.was_published_recently(), False)

    def test_was_published_recently_with_recent_question(self):
        """
        was_published_recently() 對於 questions 的 pub_date 是在 1 天之內的
        會回傳 True。
        """
        time = timezone.now() - datetime.timedelta(hours=23, minutes=59, seconds=59)
        recent_question = Question(pub_date=time)
        self.assertIs(recent_question.was_published_recently(), True)

現在，我們有三個測試來確保 `Question.was_published_recently()`
方法對於過去，最近，和將來的三種情況都回傳正確的值。

再次申明，儘管 `polls`
現在是個小型的應用，但是無論它以後變得到多麼複雜，無論他和其他程式如何交互，我們可以在一定程度上保證我們為之編寫測試的方法將按照預期的方式執行。

測試視圖[¶](#test-a-view "永久連結至標題")
------------------------------------------

我們的投票應用是相當地沒有差別待遇的：它將會發佈任何的問題，包括那些
`pub_date` 為未來的某一天時，這應該被解釋為這個問題 Question
要等到所指定的時間點才發佈，而在此之前在視圖上是看不到的。

### 針對視圖的測試[¶](#a-test-for-a-view "永久連結至標題")

修復上述 bug
錯誤後，我們首先撰寫了測試，然後再編寫了程式進行修復。事實上，這是一個「測試驅動」開發模式的實例，但我們按什麼順序進行工作並不重要。

在我們的第一個測試中，我們密切關注程式的內部行為。對於此次測試，我們希望檢查其行為，就像用戶使用瀏覽器開啟我們的應用所經歷的那樣。

在嘗試修復任何問題之前，讓我們看一下可供使用的工具。

### Django 測試工具之 Client[¶](#the-django-test-client "永久連結至標題")

Django 提供了一個 [`Client`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.Client "django.test.Client")
測試能模擬用戶和程式在視圖層的互動。我們可以在 `tests.py` 甚至是 [`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)
中使用它。

我們將再次從 [`shell`
中不需要做的事情。第一步是在 [`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)
中設定測試環境:

/ 

    $ python manage.py shell

    >>> from django.test.utils import setup_test_environment
    >>> setup_test_environment()

[`setup_test_environment()`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/advanced/#django.test.utils.setup_test_environment "django.test.utils.setup_test_environment")
安裝了一個範本實現器，使我們可以檢查 response 的一些額外的屬性，例如
`response.context`，否則這些屬性將無法被使用。注意，這個方法並 *不會*
設定測試資料庫，所以接下來的程式將會在當前存在的資料庫上執行，輸出的內容可能由於資料庫內容的不同而不同。如果你的
`settings.py`
的設定不對，你可能無法取得到期望的結果。如果你之前忘了設定，在繼續之前檢查一下。

接下來，我們需要匯入測試 client 類別（稍後在 `tests.py` 中我們將會使用 [`django.test.TestCase`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.TestCase "django.test.TestCase")
類別，該類別裡包含了自己的 client 實例，因此不需要這一步驟）:

    >>> from django.test import Client
    >>> # 建立用戶端實例以供我們使用
    >>> client = Client()

準備好之後，我們可以請 client 為我們做一些工作:

    >>> # 從 '/' 取得一個回應
    >>> response = client.get('/')
    Not Found: /
    >>> # 我們應該從該位址獲得一個 404；如果您看到一個
    >>> # "Invalid HTTP_HOST header" 錯誤和一個 400 的回應，您可能
    >>> # 忽略了前面描述的 setup_test_environment() 呼叫。
    >>> response.status_code
    404
    >>> # 另一方面，我們預期應該在 '/polls/' 找到些什麼
    >>> # 我們將會使用 'reverse()' 而不是一個直接以程式碼撰寫的 URL
    >>> from django.urls import reverse
    >>> response = client.get(reverse('polls:index'))
    >>> response.status_code
    200
    >>> response.content
    b'\n    <ul>\n    \n        <li><a href="/polls/1/">What&#x27;s up?</a></li>\n    \n    </ul>\n\n'
    >>> response.context['latest_question_list']
    <QuerySet [<Question: What's up?>]>

### 改善視圖程式[¶](#improving-our-view "永久連結至標題")

現在的投票清單表會顯示尚未發佈的投票（即`pub_date` 的值是未來的某天)。我們來解決這個問題。

在 [教學的第 4
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)
裡，我們介紹了基於 [`ListView`](https://docs.djangoproject.com/zh-hans/3.0/ref/class-based-views/generic-display/#django.views.generic.list.ListView "django.views.generic.list.ListView")
的視圖類別：

polls/views.py[¶](#id4 "永久連結至程式")**

    class IndexView(generic.ListView):
        template_name = 'polls/index.html'
        context_object_name = 'latest_question_list'

        def get_queryset(self):
            """回傳最近發佈的五個問題。"""
            return Question.objects.order_by('-pub_date')[:5]

我們需要修改 `get_queryset()` 來檢查日期。首先我們需要新增一行 import 語句：

polls/views.py[¶](#id5 "永久連結至程式")**

    from django.utils import timezone

然後我們把 `get_queryset`
方法改寫成下面這樣：

polls/views.py[¶](#id6 "永久連結至程式")**

    def get_queryset(self):
        """
        回傳最後五個已發佈的問題（不包括計劃在
        未來發佈的問題）。
        """
        return Question.objects.filter(
            pub_date__lte=timezone.now()
        ).order_by('-pub_date')[:5]

`Question.objects.filter(pub_date__lte=timezone.now())`
的值比'現在時間'還小於或等於的一個查詢集(queryset) — 意即早於或等於
`timezone.now`。

### 測試新視圖[¶](#testing-our-new-view "永久連結至標題")

啟動伺服器、在瀏覽器中載入網站、建立一些發佈時間在過去和將來的
`Questions`
會顯示出來，現在你可以對自己感到滿意了。*你不想每次修改可能與這相關的程式時都重復這樣做*
— 所以讓我們基於以上 [`shell`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-shell)
會話中的內容，再編寫一個測試。

將下面的程式增加到 `polls/tests.py` ：

polls/tests.py[¶](#id7 "永久連結至程式")**

    from django.urls import reverse

然後我們寫一個公用的快捷函數用於建立投票問題，再為視圖建立一個測試類別：

polls/tests.py[¶](#id8 "永久連結至程式")**

    def create_question(question_text, days):
        """
        使用指定的 `question_text` 建立一個問題，並發佈
        從現在起所指定偏移的 `天數`  (對於過去發佈的問題為負數
        ，對於尚未發佈的問題為正數）。
        """
        time = timezone.now() + datetime.timedelta(days=days)
        return Question.objects.create(question_text=question_text, pub_date=time)


    class QuestionIndexViewTests(TestCase):
        def test_no_questions(self):
            """
            如果沒有問題，則會顯示相關的訊息。
            """
            response = self.client.get(reverse('polls:index'))
            self.assertEqual(response.status_code, 200)
            self.assertContains(response, "No polls are available.")
            self.assertQuerysetEqual(response.context['latest_question_list'], [])

        def test_past_question(self):
            """
            過去帶有 pub_date 的問題將顯示在索引
            的頁面上。
            """
            create_question(question_text="Past question.", days=-30)
            response = self.client.get(reverse('polls:index'))
            self.assertQuerysetEqual(
                response.context['latest_question_list'],
                ['<Question: Past question.>']
            )

        def test_future_question(self):
            """
            帶有在未來 pub_date 的問題沒有顯示在
            索引的頁面上。
            """
            create_question(question_text="Future question.", days=30)
            response = self.client.get(reverse('polls:index'))
            self.assertContains(response, "No polls are available.")
            self.assertQuerysetEqual(response.context['latest_question_list'], [])

        def test_future_question_and_past_question(self):
            """
            即使存在過去和將來的問題，也只有過去的問題
            會顯示在頁面上。
            """
            create_question(question_text="Past question.", days=-30)
            create_question(question_text="Future question.", days=30)
            response = self.client.get(reverse('polls:index'))
            self.assertQuerysetEqual(
                response.context['latest_question_list'],
                ['<Question: Past question.>']
            )

        def test_two_past_questions(self):
            """
            問題索引頁面可能會顯示多個問題。
            """
            create_question(question_text="Past question 1.", days=-30)
            create_question(question_text="Past question 2.", days=-5)
            response = self.client.get(reverse('polls:index'))
            self.assertQuerysetEqual(
                response.context['latest_question_list'],
                ['<Question: Past question 2.>', '<Question: Past question 1.>']
            )

讓我們更詳細地看下以上這些內容。

首先是一個快捷函數 `create_question`，它封裝了建立投票的流程，減少了重復程式。

`test_no_questions`
方法裡沒有建立任何投票，它檢查回傳的網頁上有沒有 "No polls are
available." 這段消息和 `latest_question_list` 是否為空。注意到 [`django.test.TestCase`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.TestCase "django.test.TestCase")
類別提供了一些額外的 assertion 方法，在這個例子中，我們使用了
[`assertContains()`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.SimpleTestCase.assertContains "django.test.SimpleTestCase.assertContains")
和 [`assertQuerysetEqual()`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.TransactionTestCase.assertQuerysetEqual "django.test.TransactionTestCase.assertQuerysetEqual")
。

在 `test_past_question`
方法中，我們建立了一個投票並檢查它是否出現在欄表中。

在 `test_future_question`
在未來某天的投票。資料庫會在每次呼叫測試方法前被重置，所以第一個投票已經沒了，所以主頁中應該沒有任何投票。

剩下的那些也都差不多。實際上，測試就是假裝一些管理員的輸入，然後透過用戶端的表現是否符合預期來判斷新加入的改變是否破壞了原有的系統狀態。

### 測試 `DetailView`[¶](#testing-the-detailview "永久連結至標題")

我們的工作似乎已經很完美了？不，還有一個問題：就算在發佈日期時未來的那些投票不會在目錄頁
*index* 裡出現，但是如果用戶知道或者猜到正確的 URL
，還是可以開啟到它們。所以我們得在 `DetailView` 裡增加一些約束：

polls/views.py[¶](#id9 "永久連結至程式")**

    class DetailView(generic.DetailView):
        ...
        def get_queryset(self):
            """
            排除所有尚未發佈的問題。
            """
            return Question.objects.filter(pub_date__lte=timezone.now())

當然，我們將增加一些測試來檢驗 `pub_date`
在未來的不可以：

polls/tests.py[¶](#id10 "永久連結至程式")**

    class QuestionDetailViewTests(TestCase):
        def test_future_question(self):
            """
            帶有未來 pub_date 的問題的詳細視圖
            會回傳一個 404 not found。
            """
            future_question = create_question(question_text='Future question.', days=5)
            url = reverse('polls:detail', args=(future_question.id,))
            response = self.client.get(url)
            self.assertEqual(response.status_code, 404)

        def test_past_question(self):
            """
            帶有過去 pub_date 的問題的詳細視圖
            顯示該問題的本文。
            """
            past_question = create_question(question_text='Past Question.', days=-5)
            url = reverse('polls:detail', args=(past_question.id,))
            response = self.client.get(url)
            self.assertContains(response, past_question.question_text)

### 更多的測試思路[¶](#ideas-for-more-tests "永久連結至標題")

我們應該給 `ResultsView`
方法，並且為它建立測試。這和我們之前幹的差不多，事實上，基本就是重復一遍。

我們還可以從各個方面改進投票應用，但是測試會一直伴隨我們。比方說，在目錄頁上顯示一個沒有選項
`Choices`
的投票問題就沒什麼意義。我們可以檢查並排除這樣的投票題。測試可以建立一個沒有選項的投票，然後檢查它是否被顯示在目錄上。當然也要建立一個有選項的投票，然後確認它確實被顯示了。

恩，也許你想讓管理員能在目錄上看見未被發佈的那些投票，但是普通用戶看不到。不管怎麼說，如果你想要增加一個新功能，那麼同時一定要為它編寫測試。不過你是先寫程式還是先寫測試那就隨你了。

在未來的某個時刻，你一定會去查看測試程式，然後開始懷疑：「這麼多的測試不會使程式越來越複雜嗎？」。別著急，我們馬上就會談到這一點。

當需要測試的時候，測試用例越多越好[¶](#when-testing-more-is-better "永久連結至標題")
------------------------------------------------------------------------------------

貌似我們的測試多的快要失去控制了。按照這樣發展下去，測試程式就要變得比應用的實際程式還要多了。而且測試程式大多都是重復且不優雅的，特別是在和業務程式比起來的時候，這種感覺更加明顯。

**但是這沒關聯！**
就讓測試程式繼續肆意增長吧。大部分情況下，你寫完一個測試之後就可以忘掉它了。在你繼續開發的過程中，它會一直默默無聞地為你做貢獻的。

但有時測試也需要更新。想象一下如果我們修改了視圖，只顯示有選項的那些投票，那麼只前寫的很多測試就都會失敗。*但這也明確地告訴了我們哪些測試需要被更新*，所以測試也會測試自己。

最壞的情況是，當你繼續開發的時候，發現之前的一些測試現在看來是多餘的。但是這也不是什麼問題，多做些測試也
*不錯*。

如果你對測試有個整體規劃，那麼它們就幾乎不會變得混亂。下面有幾條好的建議：

-   對於每個模型和視圖都建立單獨的 `TestClass`
-   每個測試方法只測試一個功能
-   給每個測試方法起個能描述其功能的名字

深入程式測試[¶](#further-testing "永久連結至標題")
--------------------------------------------------

在本教學中，我們僅僅是了解了測試的基礎知識。你能做的還有很多，而且世界上有很多有用的工具來幫你完成這些有意義的事。

舉個例子，在上述的測試中，我們已經從程式邏輯和視圖回應的角度檢查了應用的輸出，現在你可以從一個更加
"in-browser" 的角度來檢查最終實現出的 HTML 是否符合預期，使用 Selenium
可以很輕松的完成這件事。這個工具不僅可以測試 Django
框架裡的程式，還可以檢查其他部分，例如說你的
JavaScript。它假裝成是一個正在和你網站進行交互的瀏覽器，就好像有個真人在開啟網站一樣！Django
它提供了 [`LiveServerTestCase`](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/tools/#django.test.LiveServerTestCase "django.test.LiveServerTestCase")
來和 Selenium 這樣的工具進行交互。

如果你在開發一個很複雜的應用的話，你也許想在每次提交程式時自動執行測試，也就是我們所說的持續集成
[continuous
integration](https://en.wikipedia.org/wiki/Continuous_integration)
，這樣就能實現質量控制的自動化，起碼是部分自動化。

一個找出程式中未被測試部分的方法是檢查程式覆蓋率。它有助於找出程式中的薄弱部分和無用部分。如果你無法測試一段程式，通常說明這段程式需要被重構或者刪除。想知道程式覆蓋率和無用程式的詳細資訊，查看文件
[Integration with
coverage.py](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/advanced/#topics-testing-code-coverage)
取得詳細資訊。

文件 [Django
中的測試](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/)
裡有關於測試的更多資訊。

接下來要做什麼？[¶](#what-s-next "永久連結至標題")
--------------------------------------------------

如果你想深入了解測試，就去看 [Django
中的測試](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/) 。

當你已經比較熟悉測試 Django 視圖的方法後，就可以繼續閱讀 [教學第 6
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial06/)
，學習靜態文件管理的相關知識。

[** 編寫你的第一個 Django 應用，第 4
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial04/)

[編寫你的第一個 Django 應用，第 6 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial06/)

編寫你的第一個 Django 應用，第 6 部分[¶](#writing-your-first-django-app-part-6 "永久連結至標題")
================================================================================================

這一篇從 [教學第 5
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial05/)
結尾的地方繼續講起。在上一節中我們為網絡投票應用編寫了測試，而現在我們要為它加上樣式和圖片。

除了服務端產生的 HTML
以外，網絡應用通常需要一些額外的文件—例如圖片，腳本和樣式表—來協助實現網絡頁面。在
Django 中，我們把這些文件統稱為“靜態文件”。

對於小專案來說，這個問題沒什麼大不了的，因為你可以把這些靜態文件隨便放在哪，只要服務程式能夠找到它們就行。然而在大專案—特別是由好幾個應用組成的大專案—中，處理不同應用所需要的靜態文件的工作就顯得有點麻煩了。

這就是 `django.contrib.staticfiles`{.docutils .literal .notranslate}
存在的意義：它將各個應用的靜態文件（和一些你指明的目錄裡的文件）統一收集起來，這樣一來，在生產環境中，這些文件就會集中在一個便於分發的地方。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

自定義 *應用* 的界面和風格[¶](#customize-your-app-s-look-and-feel "永久連結至標題")
-----------------------------------------------------------------------------------

首先，在你的 `polls`{.docutils .literal .notranslate} 目錄下建立一個名為
`static`{.docutils .literal .notranslate} 的目錄。Django
將在該目錄下尋找靜態文件，這種方式和 Diango 在
`polls/templates/`{.docutils .literal .notranslate} 目錄下尋找 template
的方式類似。

Django 的 [`STATICFILES_FINDERS`{.xref .std .std-setting .docutils
.literal
.notranslate}](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-STATICFILES_FINDERS)
設定包含了一系列的尋找器，它們知道去哪裡找到 static
文件。`AppDirectoriesFinder`{.docutils .literal .notranslate}
是預設尋找器中的一個，它會在每個 [`INSTALLED_APPS`{.xref .std
.std-setting .docutils .literal
.notranslate}](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
中指定的應用的子文件中尋找名稱為 `static`{.docutils .literal
.notranslate} 的特定文件夾，就像我們在 `polls`{.docutils .literal
.notranslate}
中剛建立的那個一樣。管理管理採用相同的目錄結構管理它的靜態文件。

在你剛建立的 `static`{.docutils .literal .notranslate}
文件夾中建立一個名為 `polls`{.docutils .literal .notranslate}
的文件夾，再在 `polls`{.docutils .literal .notranslate}
文件夾中建立一個名為 `style.css`{.docutils .literal .notranslate}
的文件。換句話說，你的樣式表路徑應是
`polls/static/polls/style.css`{.docutils .literal .notranslate}。因為
`AppDirectoriesFinder`{.docutils .literal .notranslate} 的存在，你可以在
Django 中以 `polls/style.css`{.docutils .literal .notranslate}
的形式引用此文件，類似你引用範本路徑的方式。

靜態文件命名空間

雖然我們 *可以* 像管理範本文件一樣，把 static 文件直接放入
`polls/static`{.docutils .literal .notranslate} （而不是建立另一個名為
`polls`{.docutils .literal .notranslate}
的子文件夾），不過這實際上是一個很蠢的做法。Django
只會使用第一個找到的靜態文件。如果你在 *其它*
應用中有一個相同名字的靜態文件，Django 將無法區分它們。我們需要指引
Django 選擇正確的靜態文件，而最好的方式就是把它們放入各自的 *命名空間*
。也就是把這些靜態文件放入 *另一個* 與應用名相同的目錄中。

將以下程式放入樣式表(`polls/static/polls/style.css`{.docutils .literal
.notranslate})：

polls/static/polls/style.css[¶](#id1 "永久連結至程式")**

    li a {
        color: green;
    }

下一步，在 `polls/templates/polls/index.html`{.docutils .literal
.notranslate} 的文件頭增加以下內容：

polls/templates/polls/index.html[¶](#id2 "永久連結至程式")**

    {% load static %}

    <link rel="stylesheet" type="text/css" href="{% static 'polls/style.css' %}">

`{% static %}`{.docutils .literal .notranslate}
範本標籤會產生靜態文件的絕對路徑。

這就是你開發所需要做的所有事情了。

啟動伺服器(如果它正在執行中，重新啟動一次):

/ 

    $ python manage.py runserver

重新載入\`\`http://localhost:8000/polls/\`\`
，你會發現有問題的連結是綠色的 (這是Django自己的問題標注方式)
，意思是你追加的樣式表起作用了。

增加一個背景圖[¶](#adding-a-background-image "永久連結至標題")
--------------------------------------------------------------

接著，我們會建立一個用於存在圖像的目錄。在
`polls/static/polls`{.docutils .literal .notranslate} 目錄下建立一個名為
`images`{.docutils .literal .notranslate}
的子目錄。在這個目錄中，放一張名為 `background.gif`{.docutils .literal
.notranslate} 的圖片。換言之，在目錄
`polls/static/polls/images/background.gif`{.docutils .literal
.notranslate} 中放一張圖片。

隨後，在你的樣式表（`polls/static/polls/style.css`{.docutils .literal
.notranslate}）中增加：

polls/static/polls/style.css[¶](#id3 "永久連結至程式")**

    body {
        background: white url("images/background.gif") no-repeat;
    }

瀏覽器重載 `http://localhost:8000/polls/`{.docutils .literal
.notranslate}，你將在螢幕的左上角見到這張背景圖。

警告

當然，`{% static %}`{.docutils .literal .notranslate}
範本標籤在靜態文件（例如樣式表）中是不可用的，因為它們不是由 Django
產生的。你仍需要使用 *相對路徑*
的方式在你的靜態文件之間互相引用。這樣之後，你就可以任意改變
`` STATIC_URL`（由 :ttag:`static ``{.xref .std .std-setting .docutils
.literal .notranslate} 範本標籤用於產生
URL），而無需修改大量的靜態文件。

這些只是 **基礎** 。更多關於設定和框架的資料，參考
[靜態文件解惑](https://docs.djangoproject.com/zh-hans/3.0/howto/static-files/)
和
[靜態文件指南](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/staticfiles/)。[部署靜態文件](https://docs.djangoproject.com/zh-hans/3.0/howto/static-files/deployment/)
介紹了如何在真實伺服器上使用靜態文件。

當你熟悉靜態文件後，閱讀 [此教學的第 7
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial07/)
來學習如何自定義 Django 自動產生管理網頁的過程。

[** 編寫你的第一個 Django 應用，第 5
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial05/)

[編寫你的第一個 Django 應用，第 7 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial07/)

編寫你的第一個 Django 應用，第 7 部分[¶](#writing-your-first-django-app-part-7 "永久連結至標題")
================================================================================================

這篇教學承接 [教學第 6
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial06/)
結束的地方。我們繼續修改在線投票應用，這次我們專注於自定義我們在 [教學第
2 部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/)
初見過的 Django 自動產生管理的過程。

從哪裡取得協助：

如果你在閱讀本教學的過程中有任何疑問，可以前往FAQ的:doc:Getting
Help\</faq/help\> 的小節。

自定義管理表單[¶](#customize-the-admin-form "永久連結至標題")
-------------------------------------------------------------

透過 `admin.site.register(Question)` 模型，Django
能夠構建一個預設的表單用於顯示。通常來說，你期望能自定義表單的外觀和工作方式。你可以在注冊模型時將這些設定告訴
Django。

讓我們透過重排欄表單上的欄位來看看它是怎麼工作的。用以下內容替換
`admin.site.register(Question)`：

polls/admin.py[¶](#id1 "永久連結至程式")**

    from django.contrib import admin

    from .models import Question


    class QuestionAdmin(admin.ModelAdmin):
        fields = ['pub_date', 'question_text']

    admin.site.register(Question, QuestionAdmin)

你需要遵循以下流程—建立一個模型管理類，接著將其作為第二個參數傳給
`admin.site.register()`
—在你需要修改模型的管理管理選項時這麼做。

以上修改使得 "Publication date" 欄位顯示在 "Question" 欄位之前：

![Fields have been
reordered

這在只有兩個欄位時並不特別引人注意，但對於擁有數十個欄位的表單來說，為表單選擇一個直觀的排序方法就是個很有用的細節。

說到擁有數十個欄位的表單，你可能更期望將表單分為幾個欄位集：

polls/admin.py[¶](#id2 "永久連結至程式")**

    from django.contrib import admin

    from .models import Question


    class QuestionAdmin(admin.ModelAdmin):
        fieldsets = [
            (None,               {'fields': ['question_text']}),
            ('Date information', {'fields': ['pub_date']}),
        ]

    admin.site.register(Question, QuestionAdmin)

[`fieldsets`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets "django.contrib.admin.ModelAdmin.fieldsets")
元組中的第一個元素是欄位集的標題。以下是我們的表單現在的樣子：

***

增加關聯的物件[¶](#adding-related-objects "永久連結至標題")
-----------------------------------------------------------

好了，現在我們有了投票的管理頁。不過，一個 `Question`，但管理頁卻沒有顯示多個選項。

好了。

有兩個方法可以解決這個問題。第一個就是仿照我們向管理注冊
`Question` ：

polls/admin.py[¶](#id3 "永久連結至程式")**

    from django.contrib import admin

    from .models import Choice, Question
    # ...
    admin.site.register(Choice)

現在 "Choices" 在 Django
管理頁中是一個可用的選項了。“增加選項”的表單看起來像這樣：

***

在這個表單中，"Question" 欄位是一個包含資料庫中所有投票的選擇框。Django
知道要將 [`ForeignKey`
的形式顯示。此時，我們只有一個投票。

同時也注意下 "Question" 旁邊的“增加”按鈕。每個使用
`ForeignKey`
關聯到另一個物件的物件會自動取得這個功能。當你點擊“增加”按鈕時，你會見到一個包含“增加投票”的表單。如果你在這個對話框中增加了一個投票，並點擊了“儲存”，Django
會將其儲存至資料庫，並動態地在你正在查看的“增加選項”表單中取得它。

不過，這是一種很低效地增加“選項”的方法。更好的辦法是在你建立“投票”物件時直接增加好幾個選項。讓我們實現它。

移除呼叫 `register()` 的注冊程式：

polls/admin.py[¶](#id4 "永久連結至程式")**

    from django.contrib import admin

    from .models import Choice, Question


    class ChoiceInline(admin.StackedInline):
        model = Choice
        extra = 3


    class QuestionAdmin(admin.ModelAdmin):
        fieldsets = [
            (None,               {'fields': ['question_text']}),
            ('Date information', {'fields': ['pub_date'], 'classes': ['collapse']}),
        ]
        inlines = [ChoiceInline]

    admin.site.register(Question, QuestionAdmin)

這會告訴 Django：“`Choice` 管理頁面編輯。預設提供 3
個足夠的選項欄位。”

載入“增加投票”頁面來看看它看起來的樣子：

***

它看起來像這樣：有三個關聯的選項位置—由 `extra`
定義，且每次你回傳任意已建立的物件的“修改”頁面時，你會見到三個新的位置。

在三個位置的末端，你會看到一個“增加新選項”的按鈕。如果你單擊它，一個新的位置會被增加。如果你想移除已有的位置，可以點擊位置右上角的X。注意，你不能移除原始的
3 個位置。以下圖片顯示了一個已增加的位置：

***

不過，仍然有點小問題。它佔據了大量的螢幕區域來顯示所有關聯的
`Choice`
物件的欄位。對於這個問題，Django
提供了一種表格式的單行顯示關聯物件的方法。要使用它，只需按如下形式修改
`ChoiceInline` 申明：

polls/admin.py[¶](#id5 "永久連結至程式")**

    class ChoiceInline(admin.TabularInline):
        #...

透過 ``` TabularInline``（取代 ``StackedInline ``` ），關聯物件以一種表格式的方式顯示，顯得更加緊湊：

***

注意這裡有一個額外的“刪除？”欄，這允許移除透過“增加新選項”按鈕增加的，或是已被儲存的行。

自定義管理更改欄表[¶](#customize-the-admin-change-list "永久連結至標題")
------------------------------------------------------------------------

現在投票的管理頁看起來很不錯，讓我們對“更改欄表”頁面進行一些調整—改成一個能顯示系統中所有投票的頁面。

以下是它此時的外觀：

***

預設情況下，Django 顯示每個物件的 `str()`
回傳的值。但有時如果我們能夠顯示單個欄位，它會更有協助。為此，使用
[`list_display`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display "django.contrib.admin.ModelAdmin.list_display")
管理選項，它是一個包含要顯示的欄位名的元組，在更改欄表頁中以欄的形式顯示這個物件：

polls/admin.py[¶](#id6 "永久連結至程式")**

    class QuestionAdmin(admin.ModelAdmin):
        # ...
        list_display = ('question_text', 'pub_date')

另外，讓我們把 [教學第 2
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial02/) 中的
`was_published_recently()` 方法也加上：

polls/admin.py[¶](#id7 "永久連結至程式")**

    class QuestionAdmin(admin.ModelAdmin):
        # ...
        list_display = ('question_text', 'pub_date', 'was_published_recently')

現在修改投票的欄表頁看起來像這樣：

***

你可以點擊欄標題來對這些行進行排序—除了
`was_published_recently`
這個欄，因為沒有實現排序方法。順便看下這個欄的標題
`was_published_recently`，預設就是方法名（用空格替換下劃線），該欄的每行都以字串形式顯示出處。

你可以透過給這個方法（在 `polls/models.py` 中）一些屬性來達到優化的目的，像這樣：

polls/models.py[¶](#id8 "永久連結至程式")**

    class Question(models.Model):
        # ...
        def was_published_recently(self):
            now = timezone.now()
            return now - datetime.timedelta(days=1) <= self.pub_date <= now
        was_published_recently.admin_order_field = 'pub_date'
        was_published_recently.boolean = True
        was_published_recently.short_description = 'Published recently?'

更多關於這些方法屬性的資訊，參見 [`list_display`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display "django.contrib.admin.ModelAdmin.list_display")。

再次編輯文件 `polls/admin.py`
變更頁：過濾器，使用 [`list_filter`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter "django.contrib.admin.ModelAdmin.list_filter")。將以下程式增加至
`QuestionAdmin`：

    list_filter = ['pub_date']

這樣做增加了一個“過濾器”側邊欄，允許人們以 `pub_date` 欄位來過濾欄表：

***

顯示的過濾器類型取決你你要過濾的欄位的類型。因為 `pub_date` 是類 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")，Django
知道要提供哪個過濾器：“任意時間”，“今天”，“過去7天”，“這個月”和“今年”。

這已經弄的很好了。讓我們再擴充些功能:

    search_fields = ['question_text']

在欄表的頂部增加一個搜索框。當輸入待搜項時，Django 將搜索
`question_text`
來查詢資料，將待搜索的欄位數限制為一個不會出問題大小，會便於資料庫進行查詢操作。

現在是給你的修改欄表頁增加分頁功能的好時機。預設每頁顯示 100
項。[`變更頁分頁`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_per_page "django.contrib.admin.ModelAdmin.list_per_page"),
[`搜索框`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.search_fields "django.contrib.admin.ModelAdmin.search_fields"),
[`過濾器`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_filter "django.contrib.admin.ModelAdmin.list_filter"),
[`日期層次結構`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.date_hierarchy "django.contrib.admin.ModelAdmin.date_hierarchy"),
和 [`欄標題排序`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display "django.contrib.admin.ModelAdmin.list_display")
均以你期望的方式合作執行。

自定義管理界面和風格[¶](#customize-the-admin-look-and-feel "永久連結至標題")
----------------------------------------------------------------------------

在每個管理頁頂部顯示“Django 管理員”顯得很滑稽。這只是一串佔位文字。

不過，你可以透過 Django 的範本系統來修改。Django
的管理由自己驅動，且它的交互接口採用 Django 自己的範本系統。

### 自定義你的 *工程的* 範本[¶](#customizing-your-project-s-templates "永久連結至標題")

在你的工程目錄（指包含 `manage.py` 的目錄。範本可放在你系統中任何 Django
能找到的位置。（誰啟動了 Django，Django
就以他的用戶身份執行。）不過，把你的範本放在工程內會帶來很大便利，推薦你這樣做。

打開你的設定文件（`mysite/settings.py`，牢記），在 [`TEMPLATES`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES)
設定中增加 [`DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-DIRS)
選項：

mysite/settings.py[¶](#id9 "永久連結至程式")**

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]

[`DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-DIRS)
是一個包含多個系統目錄的文件欄表，用於在載入 Django
範本時使用，是一個待搜索路徑。

組織範本

就像靜態文件一樣，我們 *可以*
把所有的範本文件放在一個大範本目錄內，這樣它也能工作的很好。但是，屬於特定應用的範本文件最好放在應用所屬的範本目錄（例如
`polls/templates`）。我們會在
[建立可復用的應用教學](https://docs.djangoproject.com/zh-hans/3.0/intro/reusable-apps/)
中討論 *為什麼* 我們要這樣做。

現在，在 `templates` 的目錄，隨後，將存放 Django
預設範本的目錄（`django/contrib/admin/templates` 復制到這個目錄內。

Django 的源文件在哪裡？

如果你不知道 Django 源碼在你系統的哪個位置，執行以下命令：

/ 

    $ python -c "import django; print(django.__path__)"

接著，用你網頁網站的名字編輯替換文件內的 [\`\`](#id1){{
site\_header|default:\_('Django administration')
}}\`\`（包含大括號）。完成後，你應該看到如下程式：

    {% block branding %}
    <h1 id="site-name"><a href="{% url 'admin:index' %}">Polls Administration</a></h1>
    {% endblock %}

我們會用這個方法來教你復寫範本。在一個實際工程中，你可能更期望使用
[`django.contrib.admin.AdminSite.site_header`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#django.contrib.admin.AdminSite.site_header "django.contrib.admin.AdminSite.site_header")
來進行簡單的定制。

這個範本文件包含很多類似 `{% block branding %}` 和 `{{`
時，這個範本語言會被求值，產生最終的網頁，就像我們在 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
所學的一樣。

注意，所有的 Django 預設管理範本均可被復寫。若要復寫範本，像你修改
`base_site.html`
一樣修改其它文件—先將其從預設目錄中拷貝到你的自定義目錄，再做修改。

### 自定義你 *應用的* 範本[¶](#customizing-your-application-s-templates "永久連結至標題")

機智的同學可能會問： [`DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-DIRS)
預設是空的，Django 是怎麼找到預設的管理範本的？因為 [`APP_DIRS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TEMPLATES-APP_DIRS)
被置為 `True` 子目錄（不要忘了 `django.contrib.admin` 也是一個應用）。

我們的投票應用不是非常複雜，所以無需自定義管理範本。不過，如果它變的更加複雜，需要修改
Django 的標準管理範本功能時，修改 *應用* 的範本會比 *工程*
的更加明智。這樣，在其它工程包含這個投票應用時，可以確保它總是能找到需要的自定義範本文件。

更多關於 Django 如何尋找範本的文件，參見
[載入範本文件](https://docs.djangoproject.com/zh-hans/3.0/topics/templates/#template-loading)。

自定義管理主頁[¶](#customize-the-admin-index-page "永久連結至標題")
-------------------------------------------------------------------

在類似的說明中，你可能想要自定義 Django 管理索引頁的外觀。

預設情況下，它顯示了所有設定在 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
中，已透過管理應用注冊，按拼音排序的應用。你可能想對這個頁面的布局做重大的修改。畢竟，索引頁是管理的重要頁面，它應該便於使用。

需要自定義的範本是 `admin/index.html`
那樣修改此文件—從預設目錄中拷貝此文件至自定義範本目錄）。打開此文件，你將看到它使用了一個叫做
`app_list`
的範本變數。這個變數包含了每個安裝的 Django
應用。你可以用任何你期望的直接以程式編寫連結（連結至特定物件的管理頁）取代使用這個變數。

接下來要做什麼？[¶](#what-s-next "永久連結至標題")
--------------------------------------------------

初學者教學到這就結束了。隨後，你可能想閱讀
[下一步看什麼](https://docs.djangoproject.com/zh-hans/3.0/intro/whatsnext/)，看看下一步能做什麼。

如果你很熟悉 Python
包裝，且對學習如何把投票應用改成“可復用應用”感興趣，查看
[進階教學：如何建立可復用應用](https://docs.djangoproject.com/zh-hans/3.0/intro/reusable-apps/)。

[** 編寫你的第一個 Django 應用，第 6
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial06/)

[進階指南：如何編寫可重用程式
**](https://docs.djangoproject.com/zh-hans/3.0/intro/reusable-apps/)

進階指南：如何編寫可重用程式[¶](#advanced-tutorial-how-to-write-reusable-apps "永久連結至標題")
===============================================================================================

這篇進階指南從 [Tutorial
7](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial07/)
結尾的地方繼續講起。我們將會把我們的 Web-poll 放進一個獨立的 Python
套件中，以便你在新的專案中重用它或將它與他人分享。

如果你尚未完成教學
1-7，我們推薦你先瀏覽一遍教學，這樣你的樣例工程會和下面的一致。

可重用性很重要[¶](#reusability-matters "永久連結至標題")
--------------------------------------------------------

設計，構建，測試以及維護一個 web 應用要做很多的工作。很多 Python 以及
Django
專案都有一些常見問題。如果我們能儲存並利用這些重復的工作豈不是更好？

可重用性是 Python 的根本。[The Python Package Index
(PyPI)](https://pypi.python.org/pypi) 有許大量的套件，都可被用在你自己的
Python 專案中。同樣可以在 [Django Packages](https://djangopackages.org/)
中查找已發布的可重用應用，也可將其引入到你的專案中。Django 本身也是一個
Python 套件，也就是說你可以將已有的 Python 套件或 Django
應用並入你的專案。你只需要編寫屬於你的那部分即可。

假設你現在建立了一個新的專案，並且需要一個類似我們之前做的投票應用。你該如何復用這個應用呢？慶幸的是，其實你已經知道了一些。在
[教學
1](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)，我們使用過
`include`{.docutils .literal .notranslate} 從專案級別的 URLconf 分割出
polls。在本教學中，我們將進一步使這個應用易用於新的專案中，並發布給其他人安裝使用。

套件？應用？

一個
[package](https://docs.python.org/3/glossary.html#term-package "(在 Python v3.8)")
提供了一組關聯的 Python
程式的簡單復用方式。一個套件（“模組”）包含了一個或多個 Python 程式文件。

一個套件透過 `import foo.bar`{.docutils .literal .notranslate} 或
`from foo import bar`{.docutils .literal .notranslate}
的形式導入。一個目錄（例如 `polls`{.docutils .literal
.notranslate}）要成為一個套件，它必須包含一個特定的文件
`__init__.py`{.docutils .literal .notranslate}，即便這個文件是空的。

Django *應用* 僅僅是專用於 Django 專案的 Python 套件。應用會按照 Django
規則，建立好 `models`{.docutils .literal .notranslate},
`tests`{.docutils .literal .notranslate}, `urls`{.docutils .literal
.notranslate}, 以及 `views`{.docutils .literal .notranslate} 等子模組。

稍後，我們將解釋術語 *包裝* ——為了方便其它人安裝 Python
套件的處理流程。我知道，這可能會使你感到一點點迷惑。

你的專案和可復用應用[¶](#your-project-and-your-reusable-app "永久連結至標題")
-----------------------------------------------------------------------------

透過前面的教學，我們的工程應該看起來像這樣:

    mysite/
        manage.py
        mysite/
            __init__.py
            settings.py
            urls.py
            asgi.py
            wsgi.py
        polls/
            __init__.py
            admin.py
            apps.py
            migrations/
                __init__.py
                0001_initial.py
            models.py
            static/
                polls/
                    images/
                        background.gif
                    style.css
            templates/
                polls/
                    detail.html
                    index.html
                    results.html
            tests.py
            urls.py
            views.py
        templates/
            admin/
                base_site.html

1

目錄 `polls`{.docutils .literal .notranslate} 現在可以被拷貝至一個新的
Django
工程，且立刻被復用。不過現在還不是發布它的時候。為了這樣做，我們需要包裝這個應用，便於其他人安裝它。

安裝必須環境[¶](#installing-some-prerequisites "永久連結至標題")
----------------------------------------------------------------

目前，包裝 Python
程式需要工具，有許多工具可以完成此項工作。在此教學中，我們將使用
[setuptools](https://pypi.org/project/setuptools/)
來包裝我們的程式。這是推薦的包裝工具（與 `發布`{.docutils .literal
.notranslate} 分支合並）。我們仍舊使用
[pip](https://pypi.org/project/pip/)
來安裝和卸載這個工具。現在，你需要安裝這兩個套件。如果你需要協助，你可以參考
[如何透過 pip 安裝
Django](https://docs.djangoproject.com/zh-hans/3.0/topics/install/#installing-official-release)，你可以透過相同的方式安裝
`setuptools`{.docutils .literal .notranslate}。

包裝你的應用[¶](#packaging-your-app "永久連結至標題")
-----------------------------------------------------

Python 的 *包裝*
將以一種特殊的格式組織你的應用，意在方便安裝和使用這個應用。Django
本身就被包裝成類似的形式。對於一個小應用，例如 polls，這不會太難。

1.  首先，在你的 Django 專案目錄外建立一個名為 `django-polls`{.docutils
    .literal .notranslate} 的文件夾，用於盛放 `polls`{.docutils .literal
    .notranslate}。

    為你的應用選擇一個名字

    當為你的套件選一個名字時，避免使用像 PyPI
    這樣已存在的套件名，否則會導致衝突。當你建立你的發布套件時，可以在模組名前增加
    `django-`{.docutils .literal .notranslate}
    前綴，這是一個很常用也很有用的避免套件名衝突的方法。同時也有助於他人在尋找
    Django 應用時確認你的 app 是 Django 獨有的。

    應用標籤（指用點分隔的套件名的最後一部分）在 [`INSTALLED_APPS`{.xref
    .std .std-setting .docutils .literal
    .notranslate}](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
    中 *必須* 是獨一無二的。避免使用任何與 Django [contrib
    packages](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/)
    文件中相同的標籤名，例如 `auth`{.docutils .literal
    .notranslate}，`admin`{.docutils .literal
    .notranslate}，`messages`{.docutils .literal .notranslate}。

2.  將 `polls`{.docutils .literal .notranslate} 目錄移入
    `django-polls`{.docutils .literal .notranslate} 目錄。

3.  建立一個名為 `django-polls/README.rst`{.docutils .literal
    .notranslate} 的文件，包含以下內容：

    django-polls/README.rst[¶](#id1 "永久連結至程式")**

        =====
        Polls
        =====

        Polls is a Django app to conduct Web-based polls. For each question,
        visitors can choose between a fixed number of answers.

        Detailed documentation is in the "docs" directory.

        Quick start
        -----------

        1. Add "polls" to your INSTALLED_APPS setting like this::

            INSTALLED_APPS = [
                ...
                'polls',
            ]

        2. Include the polls URLconf in your project urls.py like this::

            path('polls/', include('polls.urls')),

        3. Run ``python manage.py migrate`` to create the polls models.

        4. Start the development server and visit http://127.0.0.1:8000/admin/
           to create a poll (you'll need the Admin app enabled).

        5. Visit http://127.0.0.1:8000/polls/ to participate in the poll.

4.  建立一個 `django-polls/LICENSE`{.docutils .literal .notranslate}
    文件。選擇一個非本教學使用的授權協議，但是要足以說明發布程式沒有授權證書是
    *不可能的* 。Django 和很多相容 Django 的應用是以 BSD
    授權協議發布的；不過，你可以自己選擇一個授權協議。只要確定你選擇的協議能夠限制未來會使用你的程式的人。

5.  下一步我們將建立 ``` setup.cfg``和``setup.py ```{.docutils .literal
    .notranslate}
    文件用於說明如何構建和安裝應用的細節。關於此文件的完整介紹超出了此教學的範圍，但是
    [setuptools docs](https://setuptools.readthedocs.io/en/latest/)
    有詳細的介紹。建立文件 `django-polls/setup.py`{.docutils .literal
    .notranslate} 包含以下內容：

    django-polls/setup.cfg[¶](#id2 "永久連結至程式")**

        [metadata]
        name = django-polls
        version = 0.1
        description = A Django app to conduct Web-based polls.
        long_description = file: README.rst
        url = https://www.example.com/
        author = Your Name
        author_email = yourname@example.com
        license = BSD-3-Clause  # Example license
        classifiers =
            Environment :: Web Environment
            Framework :: Django
            Framework :: Django :: X.Y  # Replace "X.Y" as appropriate
            Intended Audience :: Developers
            License :: OSI Approved :: BSD License
            Operating System :: OS Independent
            Programming Language :: Python
            Programming Language :: Python :: 3
            Programming Language :: Python :: 3 :: Only
            Programming Language :: Python :: 3.6
            Programming Language :: Python :: 3.7
            Programming Language :: Python :: 3.8
            Topic :: Internet :: WWW/HTTP
            Topic :: Internet :: WWW/HTTP :: Dynamic Content

        [options]
        include_package_data = true
        packages = find:

    django-polls/setup.py[¶](#id3 "永久連結至程式")**

        from setuptools import setup

        setup()

6.  預設套件中只包含 Python
    模組和套件。為了包含額外文件，我們需要建立一個名為
    `MANIFEST.in`{.docutils .literal .notranslate} 的文件。上一步中關於
    setuptools
    的文件詳細介紹了這個文件。為了包含範本、`README.rst`{.docutils
    .literal .notranslate} 和我們的 `LICENSE`{.docutils .literal
    .notranslate} 文件，建立文件 `django-polls/MANIFEST.in`{.docutils
    .literal .notranslate} 包含以下內容：

    django-polls/MANIFEST.in[¶](#id4 "永久連結至程式")**

        include LICENSE
        include README.rst
        recursive-include polls/static *
        recursive-include polls/templates *

7.  在應用中包含詳細文件是可選的，但我們推薦你這樣做。建立一個空目錄
    `django-polls/docs`{.docutils .literal .notranslate}
    用於未來編寫文件。額外增加一行至
    `django-polls/MANIFEST.in`{.docutils .literal .notranslate}

        recursive-include docs *

    注意，現在 `docs`{.docutils .literal .notranslate}
    目錄不會被加入你的應用套件，除非你往這個目錄加幾個文件。許多 Django
    應用也提供他們的在線文件透過類似
    [readthedocs.org](https://readthedocs.org/) 這樣的網站。

8.  試著構建你自己的應用套件透過 `ptyhon setup.py sdist`{.docutils
    .literal .notranslate} （在
    ``` django-polls``目錄內）。這將建立一個名為 ``dist ```{.docutils
    .literal .notranslate} 的目錄並構建你自己的應用套件，
    `django-polls-0.1.tar.gz`{.docutils .literal .notranslate}。

更多關於包裝的資訊，見 Python 的
[關於包裝和發布專案的教學](https://packaging.python.org/tutorials/packaging-projects/)。

使用你自己的套件名[¶](#using-your-own-package "永久連結至標題")
---------------------------------------------------------------

由於我們把 `polls`{.docutils .literal .notranslate}
目錄移出了專案，所以它無法工作了。我們現在要透過安裝我們的新
`django-polls`{.docutils .literal .notranslate} 應用來修正這個問題。

作為用戶庫安裝

以下步驟將 `django-polls`{.docutils .literal .notranslate}
以用戶庫的形式安裝。與安裝整個系統的軟件套件相比，用戶安裝具有許多優點，例如可在沒有管理員開啟權的系統上使用，以及防止應用套件影響系統服務和其他用戶。

Note that per-user installations can still affect the behavior of system
tools that run as that user, so using a virtual environment is a more
robust solution (see below).

1.  為了安裝這個套件，使用 pip (你早已 [安裝
    pip](#installing-reusable-apps-prerequisites), 對嗎？):

        python -m pip install --user django-polls/dist/django-polls-0.1.tar.gz

2.  幸運的話，你的 Django 專案應該再一次正確執行。啟動伺服器確認這一點。

3.  透過 pip 卸載套件:

        python -m pip uninstall django-polls

發布你的應用[¶](#publishing-your-app "永久連結至標題")
------------------------------------------------------

現在，你已經對 `django-polls`{.docutils .literal .notranslate}
完成了包裝和測試，準備好向世界分享它！如果這不是一個例子應用，你現在就可以這樣做。

-   透過郵件將你的套件發送給朋友。
-   將這個套件上傳至你的網站。
-   將你的套件發布至公共倉庫，例如 [the Python Package Index
    (PyPI)](https://pypi.python.org/pypi)。
    [packaging.python.org](https://packaging.python.org/) 有一個不錯的
    [教學](https://packaging.python.org/tutorials/packaging-projects/#uploading-the-distribution-archives)
    說明如何發布至公共倉庫。

Installing Python packages with a virtual environment[¶](#installing-python-packages-with-a-virtual-environment "永久連結至標題")
---------------------------------------------------------------------------------------------------------------------------------

早些時候，我們以用戶庫的形式安裝了投票應用。這樣做有一些缺點。

-   修改用戶庫會影響你系統上的其他 Python 軟件。
-   你將不能執行此套件的多個版本（或者其它用有相同套件名的套件）。

Typically, these situations only arise once you're maintaining several
Django projects. When they do, the best solution is to use
[venv](https://docs.python.org/3/tutorial/venv.html "(在 Python v3.8)").
This tool allows you to maintain multiple isolated Python environments,
each with its own copy of the libraries and package namespace.

[** 編寫你的第一個 Django 應用，第 7
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial07/)

[下一步看什麼
**](https://docs.djangoproject.com/zh-hans/3.0/intro/whatsnext/)


下一步看什麼[¶](#what-to-read-next "永久連結至標題")
====================================================

如果你已經讀完了
[介紹文件](https://docs.djangoproject.com/zh-hans/3.0/intro/)，且對繼續使用
Django
感興趣。不過，你讀的是整體文件的精簡版（實際上，如果你逐字閱讀了此文件，你已經閱讀了整體文件的
5%）。

那麼下一步做什麼？

不錯，我們已經透過邊學邊做成為了 Django
的死忠粉了。此時，你應該已經知道如何開始你自己的工程，且會到處搜索其它文件了。想知道更多技巧的話，請回到文件頁。

為了使 Django
的文件達到更易使用，更清晰，更加全面的目的，我們付出了巨大的努力。本文件的剩餘部分將更詳細地介紹此文件的工作方式，以便您可以充分利用它。

（是的，這就是傳說中的說明文件的說明文件。請放心，我們不打算寫一篇關於如何閱讀此文件的文件。）

查找文件[¶](#finding-documentation "永久連結至標題")
----------------------------------------------------

Django有 *許多* 的文件——差不多有 450000
字（英文單詞），所以查找你需要的文件可能需要點技巧。對此，有幾個不錯的去處，[搜索頁面](https://docs.djangoproject.com/zh-hans/3.0/search/)
和 [索引](https://docs.djangoproject.com/zh-hans/3.0/genindex/)。

或者你可以只是四處看看！

文件是如何組成[¶](#how-the-documentation-is-organized "永久連結至標題")
-----------------------------------------------------------------------

Django的主要文件以“塊”的形式劃分，用於滿足不同的需求：

-   The [介紹文件](https://docs.djangoproject.com/zh-hans/3.0/intro/)
    是專為 Django 初學者或 Web
    初學者設計的。它不會做任何的深入，只是以高度概括的方式介紹了如果以
    Django “風格”開發應用的方式。

-   另一方面，The
    [主題指南](https://docs.djangoproject.com/zh-hans/3.0/topics/)
    則深入介紹 Django 的各個部分。那裡有更多完整的關於 Django的
    [範本系統](https://docs.djangoproject.com/zh-hans/3.0/topics/db/),
    [範本引擎](https://docs.djangoproject.com/zh-hans/3.0/topics/templates/),
    [表單框架](https://docs.djangoproject.com/zh-hans/3.0/topics/forms/)
    和其它東西的資訊。

    這可能是你會花費你大部分時間的地方。如果你詳細閱讀了這些指南，你就能了解幾乎所有關於
    Django 的知識。

-   Web
    開發通常是廣而不深的——問題通常跨越多個領域。我們已經寫了一系欄的文件
    [how-to 指引](https://docs.djangoproject.com/zh-hans/3.0/howto/)
    用於回答常見的“為什麼我會……？”系欄問題。在這裡，你可以找到關於 [透過
    Django 產生
    PDF](https://docs.djangoproject.com/zh-hans/3.0/howto/outputting-pdf/)，[定義自定義範本標籤](https://docs.djangoproject.com/zh-hans/3.0/howto/custom-template-tags/)
    的文件，當然，還有其它的文件。

    關於常見問題的回答可參見
    [FAQ](https://docs.djangoproject.com/zh-hans/3.0/faq/)。

-   這個引導和怎麼做的文件不會覆蓋 Django
    中每個可用的類，函數和方法——在你想學的時候，你會發現實在是太多了。作為取代，每個類，函數，方法和模組的細節在
    [參考](https://docs.djangoproject.com/zh-hans/3.0/ref/)
    中介紹。那是你未來查找某個函數的細節或其它你需要的東西的地方。

-   如果你對部署一個公用的工程感興趣，我們有介紹各種部署設置的文件
    [幾個指引](https://docs.djangoproject.com/zh-hans/3.0/howto/deployment/)
    和介紹你幾個你需要了解的東西的文件
    [部署清單](https://docs.djangoproject.com/zh-hans/3.0/howto/deployment/checklist/)。

-   最後，這裡有一些與大部分開發者無關的“專業”文件。包含
    [發布說明](https://docs.djangoproject.com/zh-hans/3.0/releases/) 和
    [內部文件](https://docs.djangoproject.com/zh-hans/3.0/internals/)
    ，用於向那些想向 Django 提交程式的專業人士。還有一份文件
    [不適合放到其它地方的一點內容](https://docs.djangoproject.com/zh-hans/3.0/misc/)
    。

這個文件是如何更新的[¶](#how-documentation-is-updated "永久連結至標題")
-----------------------------------------------------------------------

就像 Django
的源碼每天都被更新和提升一樣，我們的文件也會持續優化。我們因為以下幾個原因優化文件：

-   更正內容，如更正語法/拼寫錯誤。
-   向已有的需要被擴展的某個章節增加介紹資訊或例子。
-   記錄尚未記錄的 Django 功能。
    （這些功能的欄表正在縮小，但仍然存在。）
-   在新特性被增加時，或 Django 的 API 或行為有變化時，會增加新文件。

Django
的文件以和它的程式一樣，以程式版本管理系統方式進行管理。它被儲存在 git
倉庫的 [docs](https://github.com/django/django/blob/master/docs)
目錄內。每份在線文件都是倉庫內的一份獨立文本文件。

從哪裡獲取這個[¶](#where-to-get-it "永久連結至標題")
----------------------------------------------------

你可以以好幾種形式閱讀 Django 的文件。他們按照優先順序排欄：

### 在網絡上[¶](#on-the-web "永久連結至標題")

Django 最新的在線版文件位於
[https://docs.djangoproject.com/en/dev/](https://docs.djangoproject.com/en/dev/)。這些網頁由
Django 的源碼控制系統中的純文本文件自動產生。這意味著它們展示了 Django
“最新最好”
的修改——它們包含最新的更正和補充，並討論了最新的Django功能，這些功能只可供Django開發版的用戶使用。（參見以下關於“不同版本之間的差異”的介紹。）

為提高文件質量，你可以選擇在 [工單系統](https://code.djangoproject.com/)
中提交變更，修正以及建議，為此我們將十分欣喜。 Django
的開發者們會積極的監控工單系統，並使用你的反饋為大家改善文件。

值得一提的是，工單(ticket)應該明確地關聯到文件，而不是詢問籠統的技術支援問題。
如果你需要針對你的 Django 設定尋求協助，嘗試聯系
[django-users](https://docs.djangoproject.com/zh-hans/3.0/internals/mailing-lists/#django-users-mailing-list)
郵件組 或者 [\#django IRC channel](irc://irc.freenode.net/django) 。

### 純文本形式[¶](#in-plain-text "永久連結至標題")

離線閱讀，或僅僅是為了方便，你可以閱讀 Djano 文件的純文本形式。

如果你正在使用的是 Django
的某個正式發布版，注意有一個程式壓縮套件，包含了 `docs/`{.docutils
.literal .notranslate} 目錄，內含這個版本的完整文件。

如果你正在使用開發版的 Django （又名“trunk”），注意目錄
`docs/`{.docutils .literal .notranslate} 包含所有的文件。你可以透過 git
獲取最新的修改。

一種沒啥技術含量的利用純文本文件的方式是使用 Unix 的 `grep`{.docutils
.literal .notranslate}
工具在文件中全域中搜索一個短語。舉個例子，接下來會向你展示 Django
文件中所有提到這個特定短語 "max\_length" 的地方：

/ 

    $ grep -r max_length /path/to/django/docs/

### 以本地網頁形式閱讀[¶](#as-html-locally "永久連結至標題")

經過幾步操作，你可以取得一份網頁文件的拷貝：

-   Django 文件使用了一個叫做 [Sphinx](https://www.sphinx-doc.org/)
    的系統將純文本轉換為網頁。你可以透過 Sphinx 的官方網站或
    `pip`{.docutils .literal .notranslate} 來下載和安裝它：

    / 

        $ python -m pip install Sphinx

-   接著，使用其中的 `Makefile`{.docutils .literal .notranslate}
    工具將文件轉換為網頁：

        $ cd path/to/django/docs
        $ make html

    你需要為此安裝 [GNU Make](https://www.gnu.org/software/make/) 工具。

    如果你是 Windows 系統，你應該使用其中的批處理文件：

        cd path\to\django\docs
        make.bat html

-   這個 HTML 文件將會被放置在 `docs/_build/html`{.docutils .literal
    .notranslate}。

版本之間的差異[¶](#differences-between-versions "永久連結至標題")
-----------------------------------------------------------------

The text documentation in the master branch of the Git repository
contains the "latest and greatest" changes and additions. These changes
include documentation of new features targeted for Django's next
[feature
release](https://docs.djangoproject.com/zh-hans/3.0/internals/release-process/#term-feature-release).
For that reason, it's worth pointing out our policy to highlight recent
changes and additions to Django.

我們遵循以下原則：

-   [https://docs.djangoproject.com/en/dev/](https://docs.djangoproject.com/en/dev/)
    上的開發文件來自主分支。
    這些文件對應於官方最新發布的特性，並且包含所有自當時起，我們在框架中增加或修改的功能。
-   當我們為 Django 的開發版本增加功能時，我們會在相同的 Git commit
    交易中更新文件。
-   為區分文件中修改或新增的內容，我們使用短語 : "New in Django
    Development version" 來表示其屬於還未發布的開發版，使用 "New in
    version X.Y" 來表示其屬於已經發布的某個版本。
-   文件的修正和提升可能只會提交到最新的一個發布版本，這取決於提交者，然而，一旦一個
    Django 的版本處於
    [不在維護欄表](https://docs.djangoproject.com/zh-hans/3.0/internals/release-process/#supported-versions-policy)
    中，這個版本對應的文件將不再更新。
-   The [main documentation Web
    page](https://docs.djangoproject.com/en/dev/) includes links to
    documentation for previous versions. Be sure you are using the
    version of the docs corresponding to the version of Django you are
    using!

[**
進階指南：如何編寫可重用程式](https://docs.djangoproject.com/zh-hans/3.0/intro/reusable-apps/)

[編寫你的第一個 Django 修補程式
**](https://docs.djangoproject.com/zh-hans/3.0/intro/contributing/)

編寫你的第一個 Django 修補程式[¶](#writing-your-first-patch-for-django "永久連結至標題")
========================================================================================

介紹[¶](#introduction "永久連結至標題")
---------------------------------------

想為 Django 社區做一點貢獻？也許是你發現了一個想修正的
bug，或者想增加一個新的功能。

回報 Django
這件事本身就是使你的顧慮得到解決的最好方式。一開始這可能會使你怯步，但這是一條有文件、工具和社區支援的成功之路。整個過程中我們會一步一步為你解說，所以你可以透過例子學習。

### 這個教學適合誰？[¶](#who-s-this-tutorial-for "永久連結至標題")

參見

如果你正在尋找一個關於如何提交修補程式的說明文件，請查看 [Submitting
patches](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/submitting-patches/)。

使用教學前，我們希望你至少對於 Django 的執行方式有一定的認識。
這意味著你可以很容易地通讀 [編寫第一個 Django
應用](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)。
除此之外，你應該對於 Python 有很好的理解。 如果不太熟悉
Python，我們為您推薦 [\`Dive Into Python\`\_\_](#id11)
對於初學Python的程式員來說這是一本很棒（而且免費）的在線電子書。

那些不熟悉版本控制系統及缺陷追蹤的朋友可以查看這個教學，這個連結包含了足夠的資訊。如果你打算定期地為
Django 做貢獻，你可能期望閱讀更多關於這些不同工具的資料。

當然對於此教學中的大部分內容，Django 會盡可能做出解釋以協助廣大的讀者。

從哪裡取得協助：

如果你在使用本教學時遇到困難, 你可以發資訊給
[django-developers](https://docs.djangoproject.com/zh-hans/3.0/internals/mailing-lists/#django-developers-mailing-list)
中的人或登陸 [\`\#django-dev on irc.freenode.net\`\_\_](#id11) 向其他
Django 使用者尋求協助。

### 這個指南涵蓋哪些內容？[¶](#what-does-this-tutorial-cover "永久連結至標題")

我們將指導你貢獻你的第一個 Django
修補程式，在本教學完畢時，你將對相關工具及流程有一個基本的認識。特別的，我們將覆蓋以下內容：

-   安裝 Git。
-   下載一份Django開發版的拷貝。
-   執行 Django 的測試套件。
-   為你的修補程式寫一個測試。
-   為你的修補程式編寫程式。
-   測試你的修補程式。
-   提交一個 pull request（PR）。
-   在哪裡查找更多的資訊。

一旦你完成了這份教學，你可以瀏覽 [Django
貢獻文件](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/)
的剩餘部分。它包含了大量資訊。任何想成為 Django
的正式貢獻者的人都必須閱讀它。如果你有問題，它也許會給你答案。

必須Python 3！

目前的 Django 版本不再支援 Python 2.7。你可以在 [Python
下載頁](https://www.python.org/downloads/)
或透過操作系統的套件管理器下載 Python 3。

對於 Windows 用戶

See
[安裝Python](https://docs.djangoproject.com/zh-hans/3.0/howto/windows/#install-python-windows)
on Windows docs for additional guidance.

程式規範[¶](#code-of-conduct "永久連結至標題")
----------------------------------------------

作為一個貢獻者, 你可以協助我們保持 Django
的社區開放性和套件容性。請仔細閱讀並遵守我們的
[行為守則](https://www.djangoproject.com/conduct/)。

安裝Git[¶](#installing-git "永久連結至標題")
--------------------------------------------

在本教學中，你需要安裝好 Git，用 Git 下載 Django
的最新開發版本並且為你的修改產生修補程式文件。

要檢查你是否已經安裝 Git，命令欄輸入 `git`{.docutils .literal
.notranslate}。如果提示這個命令無法找到，你必須下載並安裝它，參考
[\`Git's download page\`\_\_](#id11) 。

如果你還不熟悉 Git, 你可以在命令欄下輸入 `git help`{.docutils .literal
.notranslate} 了解更多關於 Git 命令的使用方法 (確保已安裝)

取得一個 Django 開發版本的副本[¶](#getting-a-copy-of-django-s-development-version "永久連結至標題")
---------------------------------------------------------------------------------------------------

為 Django 做貢獻的第一步就是獲取源程式副本。首先， fork Github 上的
Django 專案
\<https://github.com/django/django/fork\>。接下來，在命令欄中，使用
`cd`{.docutils .literal .notranslate} 命令切換至某個你想存放 Django
源碼的目錄。

使用下面的命令來下載 Django 的源碼庫：

/ 

    $ git clone https://github.com/YourGitHubName/django.git

低速寬帶連接？

你可以在用命令 `git clone`{.docutils .literal .notranslate}
下載倉庫的時候加上參數 `--depth 1`{.docutils .literal .notranslate}
來跳過 Django 的提交歷史，這大約能把下載大小從250MB減少到70MB

你現在已經將Django拷貝到本地，可以像安裝其他軟件套件一樣使用
[\`\`](#id1)pip\`\`進行安裝。 最便捷的方式是透過 *virtual environment*，
這是 Python
的一個內置特性，它可以讓你在一個目錄中保持獨立的軟件套件環境而不影響其他的專案。

將你的虛擬環境都放在一個位置是明智的做法，例如將它們放置在你主目錄下的
`.virtualenvs/`{.docutils .literal .notranslate} 中。

透過執行以下命令建立一個虛擬環境：

/ 

    $ python3 -m venv ~/.virtualenvs/djangodev

該路徑就是儲存這個新的虛擬執行環境的地方。

設置虛擬環境的最後一步是啟用它；

    $ source ~/.virtualenvs/djangodev/bin/activate

如果 `source`{.docutils .literal .notranslate} 命令不可用，你可以試試：

    $ . ~/.virtualenvs/djangodev/bin/activate

You have to activate the virtual environment whenever you open a new
terminal window.

對於 Windows 用戶

在Windows下採用如下命令進行啟用虛擬環境：

    ...\> %HOMEPATH%\.virtualenvs\djangodev\Scripts\activate.bat

當前啟用的虛擬環境的名稱會被展示在命令欄，這可以讓你搞清楚你正在使用哪一個虛擬環境。你透過
`pip`{.docutils .literal .notranslate}
安裝的任何軟件套件如果在安裝時顯示了該名稱，則都會被安裝到該虛擬環境中，而且這些軟件套件不會影響到其他虛擬環境，也不會與其他系統級的軟件套件發生衝突。

下一步安裝之前克隆的 Django 副本：

/ 

    $ python -m pip install -e /path/to/your/local/clone/django/

在可編輯的模式下，安裝的 Django
版本就是你本地副本的版本。你將立刻見到任何你對它的修改，這對你編寫第一個修補程式很有協助。

### 使用 Django 本地副本建立專案[¶](#creating-projects-with-a-local-copy-of-django "永久連結至標題")

這對你測試本地Django專案發生了那些變化很有協助。首先，你需要建立一個新的虛擬環境，
[在可編輯模式下安裝之前克隆的Django本地副本](#intro-contributing-install-local-copy),
接著在你本地Django副本之外建立一個新的Django專案。
在你的新專案中，一旦你改動任何文件，你都會立刻看到相關資訊，這對你編寫第一個修補程式是很有協助的。

首先執行 Django 的測試套件[¶](#running-django-s-test-suite-for-the-first-time "永久連結至標題")
-----------------------------------------------------------------------------------------------

當你貢獻程式給 Django 的時候，你修改的程式千萬不要給其它部分引入新的
bug。 有個辦法可以在你更改程式之後檢查 Django 是否能正常工作，就是執行
Django
的測試套件。如果所有的測試用例都透過，你就有理由相信你的改動完全沒有破壞
Django。如果你從來沒有執行過 Django
的測試套件，那麼比較好的做法是事先執行一遍，熟悉下正常情況下應該輸出什麼結果。

執行測試套件之前，先 `cd`{.docutils .literal .notranslate} 進入 Django
的 `test/`{.docutils .literal .notranslate} 目錄，安裝其依賴，執行：

/ 

    $ python -m pip install -r requirements/py3.txt

如果安裝過程中發生了錯誤，可能是你的系統缺少一個或多個 Python
依賴套件。請參考安裝失敗的套件的文件或者在網上搜索提示的錯誤資訊。

現在你可以執行測試套件。如果你用的是 GNU/Linux， macOS 或者其它類 Unix
系統，執行：

/ 

    $ ./runtests.py

Now sit back and relax. Django's entire test suite has thousands of
tests, and it takes at least a few minutes to run, depending on the
speed of your computer.

當Django的測試套件被執行時，您將看到一個代表測試執行狀態的字串流。
其中字串 `E`{.docutils .literal .notranslate} 表示測試中出現異常，
`F`{.docutils .literal .notranslate}
表示測試中的一個斷言失敗，這兩種情況都被認為測試結果失敗。而
`x`{.docutils .literal .notranslate} 和 `s`{.docutils .literal
.notranslate} 分別表示與期望結果不同和跳過測試，逗點則表示測試被透過了。

缺失外部依賴庫通常會導致測試被跳過；查看 [Running all the
tests](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-dependencies)
獲取依賴庫欄表，如果你修改了測試程式，請同時安裝相關依賴庫（本教學無需額外依賴庫）。某些測試使用了特定的資料庫後端，如果當前測試設置並未使用此資料庫後端，那麼這些相關的測試也會被跳過。SQLite
是預設的資料庫後端。如果想使用其他後端進行測試，查看 [Using another
settings
module](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/unit-tests/#running-unit-tests-settings)。

程式測試集當測試執行完畢後，得到反饋資訊顯示測試已透過，或者測試失敗。
因為還沒有對 Django 的程式做任何修改, 所有的測試集 **應該** 測試透過.
如果測試失敗或出現錯誤，回頭確認以上執行操作是否正確. 查看 [Running the
unit
tests](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/unit-tests/#running-unit-tests)
獲取更多資訊。

注意最新版本 Django
分支不總是穩定的。當在分支上開發時，你可以查看程式持續集成構建頁面的資訊
來判斷測試錯誤只在你指定的電腦上發生，還是官方版本中也存在該錯誤。如果點擊某個構建資訊，可以透過
"Configuration Matrix" 查看錯誤發生時 Python 以及後端資料庫的資訊。

注解

在本教學以及處理工單所用分支中，測試使用資料庫 SQLite
即可，然而在某些情況下需要（有時需要） ，參考 :ref:[\`](#id1)run the
tests using a different database [\`](#id3)。

嘗試搞定一項新功能[¶](#working-on-a-feature "永久連結至標題")
-------------------------------------------------------------

這次教學中，我們將學習去完成一個名叫 "fake ticket"
的例子。下面是關於這個例子的大體構想:

Ticket \#99999 -- 允許發表祝辭

Djando中需要聲明一個函數django.shortcuts.make\_toast()，它的回傳值為'toast'。

我們現在來實現這個新功能並且做一下相關的測試。

為你的修補程式建立一個分支[¶](#creating-a-branch-for-your-patch "永久連結至標題")
---------------------------------------------------------------------------------

在做出任何修改之前，為你的工單建立一個分支：

/ 

    $ git checkout -b ticket_99999

你可以選擇任意你想要的分支名，ticket\_99999只是一個例子。你在該分支上做出的所有更改，將只會針對該分支即ticket\_99999產生影響，而不會影響到我們早先克隆的原始程式。

為你的工單寫一些測試用例[¶](#writing-some-tests-for-your-ticket "永久連結至標題")
---------------------------------------------------------------------------------

大多數情況下，Django 的修補程式必需包含測試。Bug
修正修補程式的測試是一個回歸測試，確保該 Bug 不會再次在 Django
中出現。該測試應該在 Bug 存在時測試失敗，在 Bug
已經修正後透過測試。新功能修補程式的測試必須驗證新功能是否正常執行。新功能的測試將在功能正常時透過測試，功能未執行時測試失敗。

最好的方式是在修改程式之前寫測試單元程式。這種開發風格叫做
[\`test-driven development\`\_\_](#id11)
被應用在專案開發和單一修補程式開發過程中。單元測試編寫完畢後，執行單元測試，此時測試失敗（因為目前還沒有修正
bug
或增加新功能），如果測試成功透過，你需要重新修改單元測試保證測試失敗。因為單元測試並不能阻止
bug 發生。

現在看我們的操作範例。

### 為工單 \#99999 寫測試[¶](#writing-a-test-for-ticket-99999 "永久連結至標題")

為了解決這次的工單問題，我們將在最上層的 django 模組中增加一個函數
`make_toast()`{.docutils .literal
.notranslate}。首先我們來寫一個測試用例，用於測試該函數，並且驗證一下它的輸出項是否正確。

前往Django的 `tests/shortcuts/`{.docutils .literal .notranslate}
文件夾，建立一個名為 `test_make_toast.py`{.docutils .literal
.notranslate} 的新文件。增加如下程式:

    from django.shortcuts import make_toast
    from django.test import SimpleTestCase


    class MakeToastTests(SimpleTestCase):
        def test_make_toast(self):
            self.assertEqual(make_toast(), 'toast')

上述測試是用來檢測 `make_toast()`{.docutils .literal .notranslate}
函數是否會回傳 [\`\`](#id1)'toast'\` [\`](#id3)的。

但這種測試看起來有點困難……

如果你之前從未處理過測試，那他們看起來會有點難以編寫。幸運的是，測試是一個計算機編程中
*非常* 大的一個主題，所以這裡有大量的相關資料：

-   瀏覽
    [編寫並執行測試](https://docs.djangoproject.com/zh-hans/3.0/topics/testing/overview/)
    大致看一下如何在 Django 中編寫測試。
-   深入理解 Python（一本針對 Python
    初學者的免費在線書籍），包含了不錯的 [\`introduction to Unit
    Testing\`\_\_](#id11)。
-   讀到這裡，你還想深入了解的話，可以查看 Python [`unittest`{.xref .py
    .py-mod .docutils .literal
    .notranslate}](https://docs.python.org/3/library/unittest.html#module-unittest "(在 Python v3.8)")。

### 執行你的新測試[¶](#running-your-new-test "永久連結至標題")

由於我們還沒有對 `django.shortcuts`{.docutils .literal .notranslate}
進行任何修改，這次測試將會失敗。現在讓我們來執行一下
`shortcuts`{.docutils .literal .notranslate}
目錄中的所有測試，以便確定它們真的都會產生失敗的結果。使用
`cd`{.docutils .literal .notranslate} 命令進入Django的
`tests/`{.docutils .literal .notranslate} 文件夾，然後執行:

/ 

    $ ./runtests.py shortcuts

如果測試被正確執行，一個該測試方法所對應的錯誤將會呈現給你:

    ImportError: cannot import name 'make_toast' from 'django.shortcuts'

如果所有測試都執行過了，那麼你要確保在準確的文件夾和文件名中增加了上述新測試。

為你的工單編寫程式[¶](#writing-the-code-for-your-ticket "永久連結至標題")
-------------------------------------------------------------------------

接下來我們將增加 `make_toast()`{.docutils .literal .notranslate} 函數

打開 `django/`{.docutils .literal .notranslate} 文件夾中的
`shortcuts.py`{.docutils .literal .notranslate} 文件，在文件末尾追加:

    def make_toast():
        return 'toast'

現在我們需要保證之前所寫的測試會正常透過，以便我們判斷所追加的程式是否正確。再來一次，現跳轉到Django的
`tests/`{.docutils .literal .notranslate} 目錄，然後執行:

/ 

    $ ./runtests.py shortcuts

所有專案都需要正常透過測試。如果沒有，檢查一下你的函數是否書寫正確，並且是否寫在了正確的文件中。

第二次執行 Django 測試套件[¶](#running-django-s-test-suite-for-the-second-time "永久連結至標題")
------------------------------------------------------------------------------------------------

如果已經確認修補程式以及測試結果都正常，就執行 Django
的測試套件，驗證你的修改是否導致 Django 的其它部分引入了新的 bug。
雖然測試用例協助識別容易被人忽略的錯誤，但測試透過並不能保證完全沒有 bug
存在。

執行 Django 完整的測試用例，`cd`{.docutils .literal .notranslate} 進入
Django下的 `tests/`{.docutils .literal .notranslate} 目錄並執行：

/ 

    $ ./runtests.py

書寫文件[¶](#writing-documentation "永久連結至標題")
----------------------------------------------------

這是一項新功能，所以應該為它建立一個說明，打開文件
`docs/topics/http/shortcuts.txt`{.docutils .literal .notranslate}
，然後在文件末尾追加下記內容:

    ``make_toast()``
    ================

    .. versionadded:: 2.2

    Returns ``'toast'``.

由於這一新功能將在即將到來的版本中被加入，所以下個版本的發布說明裡也加入了相關內容。打開
`docs/releases/2.2.txt`{.docutils .literal .notranslate}
文件，即發布說明的最新版本文件，在小標題"Minor
Features"下面增加一個說明:

    :mod:`django.shortcuts`
    ~~~~~~~~~~~~~~~~~~~~~~~

    * The new :func:`django.shortcuts.make_toast` function returns ``'toast'``.

更多關於編寫文件和 `versionadded`{.docutils .literal .notranslate}
的解釋和資訊，請參考
[編寫文件](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-documentation/)。這個頁面還介紹了怎麼在本地重新產生一份文件，方便你在本地預覽文件。

預覽你的修改[¶](#previewing-your-changes "永久連結至標題")
----------------------------------------------------------

現在是時候完成我們這個分支的所有變更，準備將它們提交了，執行:

/ 

    $ git add --all

然後將你當前版本(包含有你修改的內容)的拷貝和你最初在教學中取出的版本對比:

/ 

    $ git diff --cached

使用方向鍵上下移動

    diff --git a/django/shortcuts.py b/django/shortcuts.py
    index 7ab1df0e9d..8dde9e28d9 100644
    --- a/django/shortcuts.py
    +++ b/django/shortcuts.py
    @@ -156,3 +156,7 @@ def resolve_url(to, *args, **kwargs):

         # Finally, fall back and assume it's a URL
         return to
    +
    +
    +def make_toast():
    +    return 'toast'
    diff --git a/docs/releases/2.2.txt b/docs/releases/2.2.txt
    index 7d85d30c4a..81518187b3 100644
    --- a/docs/releases/2.2.txt
    +++ b/docs/releases/2.2.txt
    @@ -40,6 +40,11 @@ database constraints. Constraints are added to models using the
     Minor features
     --------------

    +:mod:`django.shortcuts`
    +~~~~~~~~~~~~~~~~~~~~~~~
    +
    +* The new :func:`django.shortcuts.make_toast` function returns ``'toast'``.
    +
     :mod:`django.contrib.admin`
     ~~~~~~~~~~~~~~~~~~~~~~~~~~~

    diff --git a/docs/topics/http/shortcuts.txt b/docs/topics/http/shortcuts.txt
    index 7b3a3a2c00..711bf6bb6d 100644
    --- a/docs/topics/http/shortcuts.txt
    +++ b/docs/topics/http/shortcuts.txt
    @@ -271,3 +271,12 @@ This example is equivalent to::
             my_objects = list(MyModel.objects.filter(published=True))
             if not my_objects:
                 raise Http404("No MyModel matches the given query.")
    +
    +``make_toast()``
    +================
    +
    +.. function:: make_toast()
    +
    +.. versionadded:: 2.2
    +
    +Returns ``'toast'``.
    diff --git a/tests/shortcuts/test_make_toast.py b/tests/shortcuts/test_make_toast.py
    new file mode 100644
    index 0000000000..6f4c627b6e
    --- /dev/null
    +++ b/tests/shortcuts/test_make_toast.py
    @@ -0,0 +1,7 @@
    +from django.shortcuts import make_toast
    +from django.test import SimpleTestCase
    +
    +
    +class MakeToastTests(SimpleTestCase):
    +    def test_make_toast(self):
    +        self.assertEqual(make_toast(), 'toast')

當你檢查完修補程式後，敲擊 `q`{.docutils .literal .notranslate}
鍵回傳到命令欄。如果修補程式內容看起來沒問題，可以提交這些修改了。

提交修補程式中的修改[¶](#committing-the-changes-in-the-patch "永久連結至標題")
------------------------------------------------------------------------------

為了提交這些修改：

/ 

    $ git commit

這會打開文本編輯器以便輸入提交資訊。參考 [commit message
guidelines](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/committing-code/#committing-guidelines)
輸入類似這樣的資訊：

    Fixed #99999 -- Added a shortcut function to make toast.

推送這次提交並產生一個 pull 請求[¶](#pushing-the-commit-and-making-a-pull-request "永久連結至標題")
---------------------------------------------------------------------------------------------------

在提交這次的修改之後，將其發送到你在GitHub上的分支(如果你使用的名稱不是"ticket\_99999"，用你自己的分支的名稱取代它
):

/ 

    $ git push origin ticket_99999

你可以開啟 [Django GitHub page](https://github.com/django/django/)
建立一個 pull 請求。 你會在“你最近推送的分支”下看到你的分支。 單擊旁邊的
"Compare & pull request"。

本教學中請不要這麼做。不過，在接下來顯示修補程式預覽的頁面，你可以單擊
"Create pull request"。

下一步[¶](#next-steps "永久連結至標題")
---------------------------------------

恭喜，你已經學會了如何為 Django 建立 pull
request！如需獲知更多高級技巧，參考 [Working with Git and
GitHub](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/working-with-git/)。

現在你可以活用這些技能協助改善 Django 的程式庫。

### 針對新貢獻者的更多注意事項[¶](#more-information-for-new-contributors "永久連結至標題")

在你開始為 Django 編寫修補程式時，這裡有些資訊，你應該看一看：

-   確保你閱讀了 Django 的參考文件
    [建立工單和提交修補程式](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/submitting-patches/)。它涵蓋了Trac
    規則，如何建立自己的工單，修補程式期望的程式風格和其他一些重要資訊。
-   初次提交修補程式應額外閱讀
    [首次貢獻者文件](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/new-contributors/)。這裡有很多對新手貢獻者的建議。
-   接下來，如果你渴望更多關於為 Django 做貢獻的資訊，可以閱讀餘下的文件
    [為 Django
    文件上作出貢獻](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/)。它包含了大量的有用資訊，這裡可以解決你可能遇到的所有問題。

### 尋找你的第一個真正意義上的工單[¶](#finding-your-first-real-ticket "永久連結至標題")

一旦你看過了之前那些資訊，你便已經具備了走出困境，編寫修正自己找到的工單的修補程式的能力。對於那些有著“容易取得”標準的工單要尤其注意。這些工單實際上常常很簡單而且對於第一次撰寫修補程式的人很有協助。一旦你熟悉了給
Django 寫修補程式，你就可以進一步為更難且更復雜的工單寫修補程式。

如果你只是想要簡單的了解（沒人會因此責備你！），那麼你可以試著看看
[\`easy tickets that need patches\`\_\_](#id11) 和 [\`easy tickets that
have patches which need
improvement\`\_\_](#id11)。如果你比較擅長寫測試，那麼你也可以看看這個
[\`easy tickets that need tests\`\_\_](#id11)。一定要記得遵循在 Django
的文件聲明標籤和遞交修補程式中提到的關於聲明標籤的指導規則
[聲明標籤和提交修補程式](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/writing-code/submitting-patches/).

### 建立完 pull request，下一步做什麼呢？[¶](#what-s-next-after-creating-a-pull-request "永久連結至標題")

工單有了修補程式後，需要他人來復審。提交 pull
請求後，為工單打上如“有修補程式”，“無需測試”之類的標籤，如此他人便可查找到該工單以便復審。從頭開始編寫修補程式固然是貢獻的一種方式，但復審已有修補程式同樣能協助
Django。 查看 [Triaging
tickets](https://docs.djangoproject.com/zh-hans/3.0/internals/contributing/triaging-tickets/)
了解更多。

[**
下一步看什麼](https://docs.djangoproject.com/zh-hans/3.0/intro/whatsnext/)

[使用 Django **](https://docs.djangoproject.com/zh-hans/3.0/topics/)




