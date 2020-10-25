編寫你的第一個 Django 應用，第 1 部分[¶](#writing-your-first-django-app-part-1 "永久連結至標題")
================================================================================================

讓我們透過範例來學習。

透過這個教學，我們將帶著你建立一個基本的投票應用程式。

它將由兩部分組成：

-   一個讓人們查看和投票的公開網站。
-   一個讓你能增加、修改和刪除投票的管理網站。

我們假定你已經閱讀了 [安裝
Django](https://docs.djangoproject.com/zh-hans/3.0/intro/install/)。你能知道
Django 已被安裝，且安裝的是哪個版本，透過在命令提示欄輸入命令（由 \$
前置符號）。



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

這行程式將會在當前目錄下建立一個 `mysite` 目錄。如果命令失敗了，查看 [執行 django-admin
時遇到的問題](https://docs.djangoproject.com/zh-hans/3.0/faq/troubleshooting/#troubleshooting-django-admin)，可能能給你提供協助。

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
    專案的 URL 聲明，就像你網站的“目錄”。閱讀
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

編寫你的第一個 Django 應用，第 2 部分 | Django 文件 | Django

-   [Getting Help](https://docs.djangoproject.com/zh-hans/3.0/faq/help/)

-   -   -   -   -   -   -   -   -   -   

-   -   -   -   -   

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
    `os.path.join(BASE_DIR, 'db.sqlite3')` 將會把資料庫文件儲存在專案的根目錄。

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
為你自己時區。

此外，說明一下文件頂部的 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
設定項。這裡包括了會在你專案中啟用的所有 Django
應用程式。應用程式能在多個專案中使用，你也可以包裝並且發佈應用程式，讓別人使用它們。

通常， [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
預設包括了以下 Django 的內建應用：

-   [`django.contrib.admin`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#module-django.contrib.admin "django.contrib.admin: Django's admin site.")
    -- 管理員網站， 你很快就會使用它。
-   [`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/3.0/topics/auth/#module-django.contrib.auth "django.contrib.auth: Django's authentication framework.")
    -- 認證授權系統。
-   [`django.contrib.contenttypes`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/contenttypes/#module-django.contrib.contenttypes "django.contrib.contenttypes: Provides generic interface to installed models.")
    -- 內容類型框架。
-   [`django.contrib.sessions`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/sessions/#module-django.contrib.sessions "django.contrib.sessions: Provides session management for Django projects.")
    -- 會議框架。
-   [`django.contrib.messages`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/messages/#module-django.contrib.messages "django.contrib.messages: Provides cookie- and session-based temporary message storage.")
    -- 訊息框架。
-   [`django.contrib.staticfiles`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles "django.contrib.staticfiles: An app for handling static files.")
    -- 管理靜態文件的框架。

這些應用程式被預設啟用是為了給常見的專案提供方便。

預設開啟的某些應用程式需要至少一個資料表，所以，在使用他們之前需要在資料庫中建立一些表。請執行以下命令：

/ 

    $ python manage.py migrate

這個 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令檢查 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
設定，為其中的每個應用程式建立需要的資料表，至於具體會建立什麼，這取決於你的
`mysite/settings.py`
設定文件和每個應用程式的資料庫遷移文件（我們稍後會介紹這個）。這個命令所執行的每個遷移操作都會在終端中顯示出來。如果你感興趣的話，執行你資料庫的命令欄工具，並輸入
`\dt` (PostgreSQL)，
`SHOW TABLES;` (MariaDB,MySQL)，
`.schema` (SQLite)或者
`SELECT TABLE_NAME FROM USER_TABLES;`
(Oracle) 來看看 Django 到底建立了哪些表。

寫給極簡主義者

就像之前說的，為了方便大多數專案，我們預設啟用了一些應用程式，但並不是每個人都需要它們。如果你不需要某個或某些應用程式，你可以在執行
[`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
前毫無顧慮地從 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
裡注釋或者刪除掉它們。 [`migrate`](https://docs.djangoproject.com/zh-hans/3.0/ref/django-admin/#django-admin-migrate)
命令只會為在 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS)
裡聲明了的應用程式進行資料庫遷移。

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
可以用它來進行資料庫的滾動更新，透過這種方式使其能夠和當前的模型比對。

在這個投票應用程式中，需要建立兩個模型：問題 `Question` 和選項 `Choice`。`Question`
模型包括問題描述和發佈時間。`Choice`
模型有兩個欄位，選項描述和當前得票數。每個選項屬於一個問題。

這些概念可以透過一個 Python 類別來描述。按照下面的例子來編輯
`polls/models.py` 文件：

polls/models.py[¶](#id2 "永久連結至程式")**

    from django.db import models


    class Question(models.Model):
        question_text = models.CharField(max_length=200)
        pub_date = models.DateTimeField('date published')


    class Choice(models.Model):
        question = models.ForeignKey(Question, on_delete=models.CASCADE)
        choice_text = models.CharField(max_length=200)
        votes = models.IntegerField(default=0)

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

/ 

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

/ 

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

/ 

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

初試 API[¶](#playing-with-the-api "永久連結至標題")
---------------------------------------------------

現在讓我們進入交互式 Python 命令欄，嘗試一下 Django 為你建立的各種
API。透過以下命令打開 Python 命令欄：

/ 

    $ python manage.py shell

我們改用這個命令而不是簡單的輸入 "Python" 是因為 `manage.py` 會設定
`DJANGO_SETTINGS_MODULE`
環境變數，這個變數會讓 Django 根據 `mysite/settings.py` 文件來設定 Python 套件的導入路徑。

當你成功進入命令欄後，來試試 [database
API](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/) 吧:

    >>> from polls.models import Choice, Question  # Import the model classes we just wrote.

    # 系統中還沒有問題。
    >>> Question.objects.all()
    <QuerySet []>

    # 建立一個新的問題。
    # 在預設的設定文件中啟用了對時區的支援，因此
    # Django 預期 pub_date 與 tzinfo 的日期時間。使用 timezone.now()
    # 而不是 datetime.datetime.now() 然後它就會它會做出正確的事情。
    >>> from django.utils import timezone
    >>> q = Question(question_text="What's new?", pub_date=timezone.now())

    # 將物件儲存到資料庫中。 您必須明確地呼叫 save() 函式。
    >>> q.save()

    # 現在它有一個 ID.
    >>> q.id
    1

    # 透過 Python 屬性存取模型字串值。
    >>> q.question_text
    "What's new?"
    >>> q.pub_date
    datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

    # 透過更改屬性來更改值，然後呼叫 save()函式。
    >>> q.question_text = "What's up?"
    >>> q.save()

    # objects.all() 顯示資料庫中的所有的問題。
    >>> Question.objects.all()
    <QuerySet [<Question: Question object (1)>]>

等等。`<Question: Question object (1)>`
對於我們了解這個物件的細節沒什麼協助。讓我們透過編輯
`Question` 模型的程式（位於
`polls/models.py`
中）來修正這個問題。給 `Question` 和
`Choice` 增加 [`__str__()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/instances/#django.db.models.Model.__str__ "django.db.models.Model.__str__")
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

給模型增加 [`__str__()`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/instances/#django.db.models.Model.__str__ "django.db.models.Model.__str__")
方法是很重要的，這不僅僅能給你在命令欄裡使用帶來方便，Django 自動產生的
admin 裡也使用這個方法來表示物件。

讓我們再為此模型增加一個自定義方法：

polls/models.py[¶](#id5 "永久連結至程式")**

    import datetime

    from django.db import models
    from django.utils import timezone


    class Question(models.Model):
        # ...
        def was_published_recently(self):
            return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

新加入的 `import datetime` 和
`from django.utils import timezone`
分別導入了 Python 的標準 [`datetime`](https://docs.python.org/3/library/datetime.html#module-datetime "(在 Python v3.8)")
模組和 Django 中和時區相關的 [`django.utils.timezone`](https://docs.djangoproject.com/zh-hans/3.0/ref/utils/#module-django.utils.timezone "django.utils.timezone: Timezone support.")
工具模組。如果你不太熟悉 Python 中的時區處理，看看
[時區支援文件](https://docs.djangoproject.com/zh-hans/3.0/topics/i18n/timezones/)
吧。

儲存文件然後透過 `python manage.py shell` 命令再次打開 Python 交互式命令欄：

    >>> from polls.models import Choice, Question

    # 確認我們新增的 __str__() 有發揮功效。
    >>> Question.objects.all()
    <QuerySet [<Question: What's up?>]>

    # Django 提供了一個豐富的資料庫查找 API，該 API 完全由
    # 關鍵字參數來驅動。
    >>> Question.objects.filter(id=1)
    <QuerySet [<Question: What's up?>]>
    >>> Question.objects.filter(question_text__startswith='What')
    <QuerySet [<Question: What's up?>]>

    # 取得今年發佈的 question。
    >>> from django.utils import timezone
    >>> current_year = timezone.now().year
    >>> Question.objects.get(pub_date__year=current_year)
    <Question: What's up?>

    # 要求一個不存在的 ID，這將引起異常狀況。
    >>> Question.objects.get(id=2)
    Traceback (most recent call last):
        ...
    DoesNotExist: Question matching query does not exist.

    # 透過主鍵搜尋是最常見的情況，因此 Django 提供了一個
    # 主鍵精確搜尋的捷徑。
    # 以下與 Question.objects.get(id=1) 是相同的。
    >>> Question.objects.get(pk=1)
    <Question: What's up?>

    # 確認我們的自定方法有運作。
    >>> q = Question.objects.get(pk=1)
    >>> q.was_published_recently()
    True

    # 給 Question 幾個 Choices。 這個 create 呼叫會構造一個新的
    # Choice 物件，執行 INSERT 語句，將 choice 新增到一個
    # 可用選項並回傳新的 Choice 物件集合。 Django 建立
    # 用於儲存 ForeignKey 關係的 "另一面" 的集合
    # (例如問題的選擇)，因而可以透過 API 進行存取。
    >>> q = Question.objects.get(pk=1)

    # 顯示相關物件集合的所有 choices -- 到目前為止還沒有。
    >>> q.choice_set.all()
    <QuerySet []>

    # 建立三個 choices。
    >>> q.choice_set.create(choice_text='Not much', votes=0)
    <Choice: Not much>
    >>> q.choice_set.create(choice_text='The sky', votes=0)
    <Choice: The sky>
    >>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

    # Choice 物件可以透過 API 存取其相關的 Question 物件。
    >>> c.question
    <Question: What's up?>

    # 反之亦然：Question 物件可以存取 Choice 物件。
    >>> q.choice_set.all()
    <QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
    >>> q.choice_set.count()
    3

    # API 會根據您的需要自動遵循關係。
    # 使用雙下劃線分隔關係。
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
文件可以獲取關於資料庫關聯的更多內容。想知道關於雙下劃線的更多用法，參見
[查找欄位](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/#field-lookups-intro)
文件。資料庫 API 的所有細節可以在 [資料庫 API
參考](https://docs.djangoproject.com/zh-hans/3.0/topics/db/queries/)
文件中找到。

介紹 Django 管理頁面[¶](#introducing-the-django-admin "永久連結至標題")
-----------------------------------------------------------------------

設計哲學

為你的員工或客戶產生一個用戶增加，修改和刪除內容的管理是一項缺乏創造性和乏味的工作。因此，Django
全自動地根據模型建立管理界面。

Django
產生於一個公眾頁面和內容發佈者頁面完全分離的新聞類網站的開發過程中。網站管理人員使用管理系統來增加新聞、事件和體育時訊等，這些增加的內容被顯示在公眾頁面上。Django
透過為網站管理人員建立統一的內容編輯界面解決了這個問題。

管理界面不是為了網站的開啟者，而是為管理者準備的。

### 建立一個管理員賬號[¶](#creating-an-admin-user "永久連結至標題")

首先，我們得建立一個能登錄管理頁面的用戶。請執行下面的命令：

/ 

    $ python manage.py createsuperuser

鍵入你想要使用的使用者，然後按下 Enter 鍵：

    Username: admin

然後提示你輸入想要使用的郵件地址：

    Email address: admin@example.com

最後一步是輸入密碼。你會被要求輸入兩次密碼，第二次的目的是為了確認第一次輸入的確實是你想要的密碼。

    Password: **********
    Password (again): *********
    Superuser created successfully.

### 啟動開發伺服器[¶](#start-the-development-server "永久連結至標題")

Django
的管理界面預設就是啟用的。讓我們啟動開發伺服器，看看它到底是什麼樣子。

如果開發伺服器未啟動，用以下命令啟動它：

/ 

    $ python manage.py runserver

現在，打開瀏覽器，轉到你本地域名稱的 "/admin/" 目錄， -- 例如
"[http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)"
。你應該會看見管理員登錄界面：

![Django admin login
screen](data:image/png;base64,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)

由於
[翻譯功能](https://docs.djangoproject.com/zh-hans/3.0/topics/i18n/translation/)
預設為打開狀態，因此如果您設定了 [`LANGUAGE_CODE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-LANGUAGE_CODE)，則登錄畫面將會以指定的語言顯示(如果
Django 有 適當的翻譯)。

### 進入管理網站頁面[¶](#enter-the-admin-site "永久連結至標題")

現在，試著使用你在上一步中建立的超級用戶來登錄。然後你將會看到 Django
管理頁面的索引頁：

![Django admin index
page](data:image/png;base64,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)

你將會看到幾種可編輯的內容：群組和使用者。它們是由
[`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/3.0/topics/auth/#module-django.contrib.auth "django.contrib.auth: Django's authentication framework.")
提供的，這是 Django 開發的認證框架。

### 向管理頁面中加入投票應用程式[¶](#make-the-poll-app-modifiable-in-the-admin "永久連結至標題")

但是我們的投票應用程式在哪呢？它沒在索引頁面裡顯示。

只需要再做一件事：我們得告訴管理員，問題 `Question` 物件需要一個管理接口。打開 `polls/admin.py` 文件，把它編輯成下面這樣：

polls/admin.py[¶](#id6 "永久連結至程式")**

    from django.contrib import admin

    from .models import Question

    admin.site.register(Question)

### 體驗便捷的管理功能[¶](#explore-the-free-admin-functionality "永久連結至標題")

現在我們向管理頁面注冊了問題 `Question`
類別。Django 知道它應該被顯示在索引頁裡：

![Django admin index page, now with polls
displayed](data:image/png;base64,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)

點擊 "Questions" 。現在看到是問題 "Questions" 物件的欄表 "change list"
。這個界面會顯示所有資料庫裡的問題 Question
物件，你可以選擇一個來修改。這裡現在有我們在上一部分中建立的 “What's
up?” 問題。

![Polls change list
page](data:image/png;base64,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)

點擊 “What's up?” 來編輯這個問題（Question）物件：

![Editing form for question
object](data:image/png;base64,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)

注意事項：

-   這個表單是從問題 `Question`
    模型中自動產生的
-   不同的欄位類型（日期時間欄位 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")
    、字串欄位 [`CharField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField")）會產生對應的
    HTML 輸入控件。每個類型的欄位都知道它們該如何在管理頁面裡顯示自己。
-   每個日期時間欄位 [`DateTimeField`](https://docs.djangoproject.com/zh-hans/3.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField")
    都有 JavaScript
    寫的快捷按鈕。日期有轉到今天（Today）的快捷按鈕和一個彈出式日曆界面。時間有設為現在（Now）的快捷按鈕和一個欄出常用時間的方便的彈出式欄表。

頁面的底部提供了幾個選項：

-   儲存（Save） - 儲存改變，然後回傳物件欄表。
-   儲存並繼續編輯（Save and continue editing） -
    儲存改變，然後重新載入當前物件的修改界面。
-   儲存並新增（Save and add another） -
    儲存改變，然後增加一個新的空物件並載入修改界面。
-   刪除（Delete） - 顯示一個確認刪除頁面。

如果顯示的 “發佈日期(Date Published)” 和你在 [教學
1](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)
裡建立它們的時間不一致，這意味著你可能沒有正確的設定 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-TIME_ZONE)
。改變設定，然後重新載入頁面看看是否顯示了正確的值。

透過點擊 “今天(Today)” 和 “現在(Now)” 按鈕改變 “發佈日期(Date
Published)”。然後點擊 “儲存並繼續編輯(Save and add
another)”按鈕。然後點擊右上角的
“歷史(History)”按鈕。你會看到一個列出了所有透過 Django
管理頁面對當前物件進行的改變的頁面，其中列出了時間戳和進行修改操作的使用者：

![History page for question
object](data:image/png;base64,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)

當你熟悉了資料庫 API 之後，你就可以開始閱讀 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
，下一部分我們將會學習如何為投票應用程式增加更多視圖。

[** 編寫你的第一個 Django 應用程式，第 1
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)

[編寫你的第一個 Django 應用程式，第 3 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)

[** Back to Top](#top)

附加資訊 
========

內容 {printedit-style="0⁋" style="display:none!important"}
----

瀏覽 {printedit-style="0⁋" style="display:none!important"}
----

當前位置 {printedit-style="0⁋" style="display:none!important"}
--------

獲取協助 {#getting-help-sidebar printedit-style="0⁋" style="display:none!important"}
--------

下載： {printedit-style="0⁋" style="display:none!important"}
------




