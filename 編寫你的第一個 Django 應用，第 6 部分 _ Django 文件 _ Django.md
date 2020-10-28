編寫你的第一個 Django 應用，第 6 部分 | Django 文件 | Django

-   [Getting Help](https://docs.djangoproject.com/zh-hans/3.0/faq/help/)

-   -   -   -   -   -   -   -   -   -   

-   -   -   -   -   

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

[** Back to Top](#top)

附加資訊 {.visuallyhidden printedit-style="0⁋" style="display:none!important"}
========

內容 {printedit-style="0⁋" style="display:none!important"}
----

瀏覽 {printedit-style="0⁋" style="display:none!important"}
----

當前位置 {printedit-style="0⁋" style="display:none!important"}
--------

取得協助 {#getting-help-sidebar printedit-style="0⁋" style="display:none!important"}
--------

下載： {printedit-style="0⁋" style="display:none!important"}
------


