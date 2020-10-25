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

初試 API[¶](#playing-with-the-api "永久連結至標題")
---------------------------------------------------

現在讓我們進入交互式 Python 命令欄，嘗試一下 Django 為你建立的各種
API。透過以下命令打開 Python 命令欄：


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

### 建立一個管理員帳號[¶](#creating-an-admin-user "永久連結至標題")

首先，我們得建立一個能登錄管理頁面的用戶。請執行下面的命令：


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


    $ python manage.py runserver

現在，打開瀏覽器，轉到你本地域名稱的 "/admin/" 目錄， -- 例如
"[http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)"
。你應該會看見管理員登錄界面：

***

由於
[翻譯功能](https://docs.djangoproject.com/zh-hans/3.0/topics/i18n/translation/)
預設為打開狀態，因此如果您設定了 [`LANGUAGE_CODE`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-LANGUAGE_CODE)，則登錄畫面將會以指定的語言顯示(如果
Django 有 適當的翻譯)。

### 進入管理網站頁面[¶](#enter-the-admin-site "永久連結至標題")

現在，試著使用你在上一步中建立的超級用戶來登錄。然後你將會看到 Django
管理頁面的索引頁：

***

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

***

點擊 "Questions" 。現在看到是問題 "Questions" 物件的欄表 "change list"
。這個界面會顯示所有資料庫裡的問題 Question
物件，你可以選擇一個來修改。這裡現在有我們在上一部分中建立的 “What's
up?” 問題。

***

點擊 “What's up?” 來編輯這個問題（Question）物件：

***

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

***

當你熟悉了資料庫 API 之後，你就可以開始閱讀 [教學第 3
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)
，下一部分我們將會學習如何為投票應用程式增加更多視圖。

[** 編寫你的第一個 Django 應用程式，第 1
部分](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial01/)

[編寫你的第一個 Django 應用程式，第 3 部分
**](https://docs.djangoproject.com/zh-hans/3.0/intro/tutorial03/)

[** Back to Top](#top)






