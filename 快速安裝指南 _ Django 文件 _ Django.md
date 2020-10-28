快速安裝指南 | Django 文件 | Django

-   [Getting Help](https://docs.djangoproject.com/zh-hans/3.0/faq/help/)

-   -   -   -   -   -   -   -   -   -

-   -   -   -   -

快速安裝指南[¶](#quick-install-guide "永久連結至標題")
======================================================

開始用 Django 前，需要先進行安裝。我們寫了 :doc:`完整安裝指南\</topics/install\>\`
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

[** Back to Top](#top)

附加資訊 {.visuallyhidden printedit-style="0⁋" style="display:none!important"}
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


