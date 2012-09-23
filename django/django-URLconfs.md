Overview
====================

去設計app的一個URLs，首先你必須寫一個叫做 **URLconf** 的python module，這個模組是純python寫的，用來配對URL patterns和callback function.

Django處理要求的機制
====================

當一個request近來的時候，系統會遵循演算法去決定哪段程式需要被執行：

1. Django使用setting裡面的ROOT_URLCONF值，來決定載入哪個URL路徑
2. Django載入python模組，尋找變數[urlpatterns](#)，而且必須要是python list
3. Django會執行所有URL pattern，但是會以第一個尋找到的結果為主
4. 當配對成功時，Django會引入並且呼叫一個python寫的函式的view，這個view會收到[HttpRequest]()當作第一個參數，並且將剩下抓取到的變數當作參數傳入。
5. 如果沒有吻合的Django會呼叫一個是當的處理錯誤的view。

Example
--------------------

    from django.conf.urls import patterns, url, include

    urlpatterns = patterns('',
        (r'^articles/2003/$', 'news.views.special_case_2003'),
        (r'^articles/(\d{4})/$', 'news.views.year_archive'),
        (r'^articles/(\d{4})/(\d{2})/$', 'news.views.month_archive'),
        (r'^articles/(\d{4})/(\d{2})/(\d+)/$', 'news.views.article_detail'),
    )

筆記：

+ 要捕捉URL的值，只要使用括弧包起來就可以了
+ 不需要在前面增加斜線，因為預設就已經有了，例如`^articles`不是`^/articles`
+ regexp前面的`r`不是必要的，但是建議

Names Groups
====================

上面的範例是使用 *無命名regexp群組* 來獲得URL中傳遞近來的參數，在更進階的使用中，使用 *命名的regexp群組* 來獲取URL是可能的，並且將其以關鍵字的方式當作參數傳入

python中，命名regexp群組的語法是`(?P<name>pattern)`，其中name是變數名稱，pattern是模板
