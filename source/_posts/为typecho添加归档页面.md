---
title: 为typecho添加归档页面
date: 2015-10-23 03:15:00
categories: code
tags: [typecho,归档,php]
urlname: add-archive-pages-for-typecho
---
![chino][1]
[演示地址][2] 

食用方法：在当前主题根目录下自定一个模板，复制php部分到自定的模板中，在主题的header.php中引入css和js，然后新建页面，启用该模板即可，如果产生错误，请尝试关闭该页面评论开关。
*由于本博客右键菜单的js与归档页面的js冲突，我没有引入此js，引入后会产生一个目录*
php部分：
```php
<?php
/**
 * archive
 *
 * @package custom
 */
$this->need('header.php'); ?>
<div id="archives">         
    <div id="archives-content">   
        <?php  
            $this->widget('Widget_Contents_Post_Recent', 'pageSize=10000')->to($archives);   
            $year=0; $mon=0; $i=0; $j=0;   
            $all = array();   
            $output = '';   
            while($archives->next()):   
                $year_tmp = date('Y',$archives->created);   
                $mon_tmp = date('n',$archives->created);   
  
                $y=$year; $m=$mon;   
                if ($mon != $mon_tmp && $mon > 0) $output .= '</div></div>';   
                if ($year != $year_tmp) {   
                    $year = $year_tmp;   
                    $all[$year] = array();   
                }   
  
                if ($mon != $mon_tmp) {   
                    $mon = $mon_tmp;   
                    array_push($all[$year], $mon);   
                    $output .= "<div class='archive-title' id='arti-$year-$mon'><h3>$year-$mon</h3><div class='archives archives-$mon' data-date='$year-$mon'>";   
                }   
                $output .= '<div class="brick"><a href="'.$archives->permalink .'"><span class="time">'.date('m-d',$archives->created).'</span>'.$archives->title .'<em>('. $archives->commentsNum.')</em></a></div>';   
            endwhile;   
            $output .= '</div></div>';   
            echo $output;   
  
            $html = "";   
            $year_now = date("Y");   
            foreach($all as $key => $value){   
                $html .= "<li class='year' id='year-$key'><a href='#' class='year-toogle' id='yeto-$key'>$key</a><ul class='monthall'>";   
                for($i=12; $i>0; $i--){   
                    if($key == $year_now && $i > $value[0]) continue;   
                    $html .= in_array($i, $value) ? ("<li class='month monthed' id='mont-$key-$i'>$i</li>") : ("<li class='month'>$i</li>");   
                }   
                $html .= "</ul></li>";   
            }   
        ?>  
    </div>         
    <div id="archive-nav">         
        <ul class="archive-nav"><?php echo $html;?></ul>         
    </div>         
</div><!-- #archives -->  
<?php $this->need('footer.php'); ?>
```

css部分（实际使用中请根据自身情况进行调整）：
```css
#archives:after {      
    clear: both;      
    display: block;      
    visibility: hidden;      
    height: 0!important;      
    content: " ";      
    font-size: 0!important;      
    line-height: 0!important     
}   
   
#archives {      
    zoom: 1     
}   
   
#archives-content {      
padding: 10px 30px 10px 60px;
}   
   
#archive-nav {      
    float: left;      
    width: 50px     
}   
   
.archive-nav {      
    display: block;      
    position: fixed;      
    background: #f9f9f9;      
    width: 40px;      
    padding: 5px;      
    border: 1px solid #eee;      
    text-align: center     
}   
   
.year {      
    border-top: 1px solid #ddd     
}   
   
.month {      
    color: #ccc;      
    padding: 5px;      
    cursor: pointer;      
    background: #f9f9f9     
}   
   
.month.monthed {      
    color: #777     
}   
   
.month.selected,.month:hover {      
    background: #f2f2f2     
}   
   
.monthall {      
    display: none     
}   
   
.year.selected .monthall {      
    display: block     
}   
   
.year-toogle {      
    display: block;      
    padding: 5px;      
    text-decoration: none;      
    background: #eee;      
    color: #333;      
    font-weight: bold     
}   
   
.archive-title {      
    padding-bottom: 40px     
}   
   
.brick {      
    margin-bottom: 10px     
}   
   
.archives a {      
    position: relative;      
    display: block;      
    padding: 10px;      
    background-color: #f9f9f9;      
    color: #333;      
    font-style: normal;      
    line-height: 18px     
}   
   
.time {      
    color: #888;      
    padding-right: 10px     
}   
   
.archives a:hover {      
    background: #eee     
}   
   
#archives h3 {      
    padding-bottom: 10px     
}   
   
.brick em {      
    color: #aaa;      
    padding-left: 10px     
}   
```

JS部分（需要jQuery）：
```javascript
jQuery(document).ready(function($) {   
    var old_top = $("#archives").offset().top,   
        _year = parseInt($(".year:first").attr("id").replace("year-", ""));   
    $(".year:first, .year .month:first").addClass("selected");   
    $(".month.monthed").click(function() {   
        var _this = $(this),   
            _id = "#" + _this.attr("id").replace("mont", "arti");   
        if (!_this.hasClass("selected")) {   
            var _stop = $(_id).offset().top - 10,   
                _selected = $(".month.monthed.selected");   
            _selected.removeClass("selected");   
            _this.addClass("selected");   
            $("body, html").scrollTop(_stop)   
        }   
    });   
    $(".year-toogle").click(function(e) {   
        e.preventDefault();   
        var _this = $(this),   
            _thisp = _this.parent();   
        if (!_thisp.hasClass("selected")) {   
            var _selected = $(".year.selected"),   
                _month = _thisp.children("ul").children("li").eq(0);   
            _selected.removeClass("selected");   
            _thisp.addClass("selected");   
            _month.click()   
        }   
    });   
    $(window).scroll(function() {   
        var _this = $(this),   
            _top = _this.scrollTop();   
        if (_top >= old_top + 10) {   
            $(".archive-nav").css({   
                top: 10   
            })   
        } else {   
            $(".archive-nav").css({   
                top: old_top + 10 - _top   
            })   
        }   
        $(".archive-title").each(function() {   
            var _this = $(this),   
                _ooid = _this.attr("id"),   
                _newyear = parseInt(_ooid.replace(/arti-(\d*)-\d*/, "$1")),   
                _offtop = _this.offset().top - 40,   
                _oph = _offtop + _this.height();   
            if (_top >= _offtop && _top < _oph) {   
                if (_newyear != _year) {   
                    $("#year-" + _year).removeClass("selected");   
                    $("#year-" + _newyear).addClass("selected");   
                    _year = _newyear   
                }   
                var _id = _id = "#" + _ooid.replace("arti", "mont"),   
                    _selected = $(".month.monthed.selected");   
                _selected.removeClass("selected");   
                $(_id).addClass("selected")   
            }   
        })   
    })   
});  
```
修改自[@牧风][3]


  [1]: http://7xoffh.com1.z0.glb.clouddn.com/2015/10/2157273208.jpg
  [2]: http://lovead.ml/archives.html
  [3]: https://mufeng.me/typecho-archive.html