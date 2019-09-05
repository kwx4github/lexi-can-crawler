# lexi-can-crawler

Crawler for Cantonese pronunciation data on Research Institute for the Humanities, Faculty of Arts, The Chinese University of Hong Kong

## Result

See [releases page](https://github.com/sgalal/lexi_can_crawler/releases).

## Design

Get a list of all Chinese characters from the classified character table.

Query for each characters by Scrapy.

## Tips

### Decoding Problem

Decode as `big5hkscs` instead of `big5`.

### Extract Data

#### Get data rows in a single page

`form > table:first-child > tr:not(:first-child)`

#### Get fields in a single data row

##### Basic

Initial: `td:nth-child(1) > font[color="red"]::text`

Rhyme: `td:nth-child(1) > font[color="green"]::text`

Tone: `td:nth-child(1) > font[color="blue"]::text`

##### Patterns (incomplete)

Explanation: `td:nth-child(6)`

(0)

![](patterns/00.png)

```html
<td>
    <div nowrap="">枝指</div>
</td>
```

(1)

![](patterns/01.png)

```html
<td>
    <div nowrap="">
        枝葉, 荔枝, 花枝招展
        <a href="#1" onclick="xid_down('zi1_detial')">
            <font size="-1">[11..]</font>
        </a>
    </div>
    <div id="zi1_detial" style="display: block;">
        枝幹, 枝椏, 樹枝, 折枝, 接枝, 比翼連枝, 同氣連枝, 金枝玉葉, 節外生枝, 細枝末節, 枝葉扶疏
    </div>
</td>
```

(2)

![](patterns/02.png)

```html
<td>
    <div nowrap="">
        蟬聯
        <font size="-1" color="maroon">(連任)</font>
        , 蟬蛻, 寒蟬
        <a href="#1" onclick="xid_down('sim4_detial')">
            <font size="-1">[1..]</font>
        </a>
    </div>
    <div id="sim4_detial" style="display: none">
        噤若寒蟬
    </div>
</td>
```

(3)

![](patterns/03.png)

```html
<td>
    <div nowrap=""></div>
    <font size="-1" color="forestgreen">
        「蟬
        <font size="+1" color="red">s</font>
        <font size="+1" color="green">im</font>
        <font size="+1" color="blue">4</font>
        」的異讀字
    </font>
</td>
```

(4)

![](patterns/04.png)

```html
<td>
    <div nowrap=""></div>
    <font size="-1" color="forestgreen">
        通「
        <a href="search.php?q=%D6r">琀</a>
        」字
    </font>
</td>
```

(5)

![](patterns/05.png)

```html
<td>
    <div nowrap=""></div>
    <font size="-1" color="forestgreen">
        同「
        <a href="search.php?q=%A7t">含</a>
        」字
    </font>
</td>
```

(6)

![](patterns/06.png)

```html
<td>
    <div nowrap="">
        唅蟬
        <font size="-1" color="maroon">(殉葬時置死者口中的蟬形玉石)</font>
        , 
        <br>
        唅以槁骨, 羹藜唅糗
    </div>
    <font size="-1" color="forestgreen">嘴裡銜著食物</font>
</td>
```

## Run

```sh
$ wget -P preprocessing/classified.php.txt http://humanum.arts.cuhk.edu.hk/Lexis/lexi-can/classified.php?st=0
$ python3 preprocessing/extract.py
$ scrapy crawl lexi -o result.json
```
