長庚大學 大數據分析方法 作業六
================

作業說明 （繳交時請直接刪除這個章節）
-------------------------------------

作業目的：期末專題暖身

依下列指示，完成期末分析專題作業規劃：

-   訂出分析問題，並將R Markdown的一級標題(第一行的title:)中的"長庚大學 大數據分析方法 作業六"取代為期末專題的分析問題，並在分析議題背景前加上組員姓名 (`10pt`)
-   分析議題背景 (`10pt`) 與動機 (`10pt`)
-   資料說明 (`10pt`) 與 載入 (`10pt`)
-   資料處理與清洗 (`10pt`) 說明 (`10pt`)
-   對資料們進行探索式資料分析，圖文並茂佳!(`20pt`)
-   期末專題分析規劃與假設 (`10pt`)

分析議題背景
------------

自古以來，景氣好與壞與國家治安有極大的關係，甚至得以毀掉一個國家。例如明朝末年的大旱災導致農民無法耕種維生，加上官吏的貪腐，斂取民間的財富，導致農民揭竿而起成了流寇，大肆搶奪、劫掠，使官兵疲於奔命，最終攻陷北京，滅亡大明。因此從古今的歷史，犯罪率的關係早已在深植大眾心理。經濟景況不好，社會的治安就會惡化，這是大眾心中的鐵律。然，在這個講究數據與科學的時代，這個鐵律是不是早已生了鏽？

分析動機
--------

饑寒真的起盜心嗎？

使用資料
--------

內政部警政署受(處)理刑事案件破獲件數： 民國89年至105年各縣市刑事案件的破獲案件數。 由於資料數量龐大，先以台北市做分析。

主計處平均每戶(人)可支配所得--按區域別分： 民國69年至104年各地區每戶(人)的可支配所得。

載入使用資料們

``` r
library(readr)
```

    ## Warning: package 'readr' was built under R version 3.2.5

``` r
avg_money_house_66_99 <- read_csv("~/RCourse/RLectures/big_crime/data/平均每戶69-99可支配所得.csv")
```

    ## Warning: Missing column names filled in: 'X26' [26]

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   X26 = col_character()
    ## )

    ## See spec(...) for full column specifications.

``` r
avg_money_person_66_99 <- read_csv("~/RCourse/RLectures/big_crime/data/平均每人69-99可支配所得.csv")
```

    ## Warning: Missing column names filled in: 'X26' [26]

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   X26 = col_character()
    ## )
    ## See spec(...) for full column specifications.

``` r
avg_money_house_100_104 <- read_csv("~/RCourse/RLectures/big_crime/data/平均每戶100-104可支配所得.csv")
```

    ## Warning: Missing column names filled in: 'X23' [23]

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   X23 = col_character()
    ## )
    ## See spec(...) for full column specifications.

``` r
avg_money_person_100_104 <- read_csv("~/RCourse/RLectures/big_crime/data/平均每人100-104可支配所得.csv")
```

    ## Warning: Missing column names filled in: 'X23' [23]

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   X23 = col_character()
    ## )
    ## See spec(...) for full column specifications.

``` r
crime_0_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-0.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   一般及機車竊盜 = col_number(),
    ##   汽車竊盜 = col_number(),
    ##   贓物 = col_integer(),
    ##   賭博 = col_number(),
    ##   重傷害 = col_integer(),
    ##   一般傷害 = col_number(),
    ##   詐欺背信 = col_number(),
    ##   妨害自由 = col_number(),
    ##   故意殺人 = col_integer()
    ## )

``` r
crime_1_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-1.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   過失致死 = col_integer(),
    ##   駕駛過失 = col_number(),
    ##   妨害家庭及婚姻 = col_integer(),
    ##   妨害風化 = col_number(),
    ##   強制性交 = col_integer(),
    ##   共同強制性交 = col_integer(),
    ##   對幼性交 = col_integer(),
    ##   性交猥褻 = col_integer(),
    ##   重大恐嚇取財 = col_integer()
    ## )

``` r
crime_2_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-2.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   一般恐嚇取財 = col_integer(),
    ##   擄人勒贖 = col_integer(),
    ##   侵占 = col_number(),
    ##   偽造文書印文 = col_integer(),
    ##   第一級毒品 = col_number(),
    ##   第二級毒品 = col_number(),
    ##   第三級毒品 = col_integer(),
    ##   第四級毒品 = col_integer(),
    ##   其他 = col_integer()
    ## )

``` r
crime_3_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-3.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   毀棄損壞 = col_integer(),
    ##   妨害公務 = col_integer(),
    ##   妨害電腦使用 = col_integer(),
    ##   強盜 = col_integer(),
    ##   搶奪 = col_integer(),
    ##   內亂 = col_integer(),
    ##   重利 = col_integer(),
    ##   竊佔 = col_integer(),
    ##   偽造有價證券 = col_integer()
    ## )

``` r
crime_4_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-4.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   妨害秩序 = col_integer(),
    ##   違反藥事法 = col_integer(),
    ##   違反國家總動員法 = col_integer(),
    ##   違反森林法 = col_integer(),
    ##   違反著作權法 = col_integer(),
    ##   違反專利法 = col_integer(),
    ##   違反商標法 = col_integer(),
    ##   公共危險 = col_number(),
    ##   侵害墳墓屍體 = col_integer()
    ## )

``` r
crime_5_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-5.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   妨害名譽 = col_number(),
    ##   違反就業服務法 = col_integer(),
    ##   違反選罷法 = col_integer(),
    ##   妨害秘密 = col_integer(),
    ##   遺棄 = col_integer(),
    ##   違反貪污治罪條例 = col_integer(),
    ##   瀆職 = col_integer(),
    ##   懲治走私條例 = col_integer(),
    ##   妨害兵役 = col_integer()
    ## )

``` r
crime_6_taipei <-read_delim("~/RCourse/RLectures/big_crime/data/臺北市-6.tsv", "\t", escape_double = FALSE, trim_ws = TRUE)
```

    ## Parsed with column specification:
    ## cols(
    ##   時間 = col_character(),
    ##   偽造貨幣 = col_integer(),
    ##   偽造度量衡 = col_integer(),
    ##   偽證 = col_integer(),
    ##   誣告 = col_integer(),
    ##   湮滅證據 = col_integer(),
    ##   藏匿頂替 = col_integer(),
    ##   脫逃 = col_integer(),
    ##   違反槍砲彈藥刀械管制條例 = col_integer(),
    ##   其他 = col_number()
    ## )

資料處理與清洗
--------------

1.  寫python爬蟲爬下資料
2.  從犯罪資料中取出以年份做為統計而非月份的資料
3.  將台北市的每戶(人)平均所得資料從資料表取出
4.  

處理資料

``` r
crime_0_taipei <- crime_0_taipei[seq(from=1, to=221, by=13),]
crime_1_taipei <- crime_1_taipei[seq(from=1, to=221, by=13),]
crime_2_taipei <- crime_2_taipei[seq(from=1, to=221, by=13),]
crime_3_taipei <- crime_3_taipei[seq(from=1, to=221, by=13),]
crime_4_taipei <- crime_4_taipei[seq(from=1, to=221, by=13),]
crime_5_taipei <- crime_5_taipei[seq(from=1, to=221, by=13),]
crime_6_taipei <- crime_6_taipei[seq(from=1, to=221, by=13),]
crime_taipei   <- merge(crime_0_taipei, crime_1_taipei, by='時間')
crime_taipei   <- merge(crime_taipei, crime_2_taipei, by='時間')
crime_taipei   <- merge(crime_taipei, crime_3_taipei, by='時間')
crime_taipei   <- merge(crime_taipei, crime_4_taipei, by='時間')
crime_taipei   <- merge(crime_taipei, crime_5_taipei, by='時間')
crime_taipei   <- merge(crime_taipei, crime_6_taipei, by='時間')
crime_taipei$時間<- as.numeric(crime_taipei$時間)
```

    ## Warning: NAs introduced by coercion

``` r
knitr::kable(crime_taipei)
```

| 時間 | 一般及機車竊盜 | 汽車竊盜 | 贓物 | 賭博 | 重傷害 | 一般傷害 | 詐欺背信 | 妨害自由 | 故意殺人 | 過失致死 | 駕駛過失 | 妨害家庭及婚姻 | 妨害風化 | 強制性交 | 共同強制性交 | 對幼性交 | 性交猥褻 | 重大恐嚇取財 | 一般恐嚇取財 | 擄人勒贖 | 侵占 | 偽造文書印文 | 第一級毒品 | 第二級毒品 | 第三級毒品 | 第四級毒品 | 其他.x | 毀棄損壞 | 妨害公務 | 妨害電腦使用 | 強盜 | 搶奪 | 內亂 | 重利 | 竊佔 | 偽造有價證券 | 妨害秩序 | 違反藥事法 | 違反國家總動員法 | 違反森林法 | 違反著作權法 | 違反專利法 | 違反商標法 | 公共危險 | 侵害墳墓屍體 | 妨害名譽 | 違反就業服務法 | 違反選罷法 | 妨害秘密 | 遺棄 | 違反貪污治罪條例 | 瀆職 | 懲治走私條例 | 妨害兵役 | 偽造貨幣 | 偽造度量衡 | 偽證 | 誣告 | 湮滅證據 | 藏匿頂替 | 脫逃 | 違反槍砲彈藥刀械管制條例 | 其他.y |
|:----:|:--------------:|:--------:|:----:|:----:|:------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|:--------------:|:--------:|:--------:|:------------:|:--------:|:--------:|:------------:|:------------:|:--------:|:----:|:------------:|:----------:|:----------:|:----------:|:----------:|:------:|:--------:|:--------:|:------------:|:----:|:----:|:----:|:----:|:----:|:------------:|:--------:|:----------:|:----------------:|:----------:|:------------:|:----------:|:----------:|:--------:|:------------:|:--------:|:--------------:|:----------:|:--------:|:----:|:----------------:|:----:|:------------:|:--------:|:--------:|:----------:|:----:|:----:|:--------:|:--------:|:----:|:------------------------:|:------:|
|  NA  |      7288      |    271   |  123 |  333 |    6   |   2309   |   3436   |   1009   |    43    |    36    |   2383   |       122      |    457   |    233   |      12      |    29    |    94    |       0      |      318     |     1    | 1276 |      734     |     832    |    2973    |     223    |      2     |    3   |    648   |    152   |      122     |  48  |  71  |   0  |  203 |  131 |      23      |     6    |     69     |         0        |      0     |      278     |      0     |     80     |   4973   |       0      |   1008   |        1       |      6     |    82    |  24  |         1        |   5  |       1      |     0    |     7    |      0     |   8  |  112 |     0    |     2    |   1  |            90            |   583  |
|  NA  |      7458      |    288   |  148 |  645 |    5   |   2142   |   3042   |   1044   |    53    |    42    |   2292   |       125      |    490   |    212   |      12      |    19    |    116   |       0      |      109     |     2    | 1112 |      671     |     799    |    3263    |     229    |      0     |    8   |    549   |    194   |      108     |  52  |  68  |   0  |  297 |  105 |      21      |    13    |     54     |         0        |      0     |      260     |      0     |     131    |   4815   |       1      |   1002   |        6       |      6     |    96    |  28  |         2        |   8  |       0      |     0    |     3    |      1     |   5  |  112 |     3    |     2    |   0  |            118           |   603  |
|  NA  |      5973      |    147   |  143 |  764 |    2   |   2358   |   3053   |   1176   |    65    |    29    |   2562   |       129      |    431   |    155   |      13      |    20    |    132   |       0      |      170     |     2    | 1149 |      732     |     734    |    3664    |     309    |      2     |    2   |    600   |    181   |      102     |  52  |  38  |   0  |  431 |  132 |      20      |     9    |     59     |         0        |      0     |      332     |      0     |     125    |   6615   |       2      |   1192   |        3       |      0     |    80    |  40  |         1        |  10  |       0      |     1    |     4    |      0     |   8  |  151 |     4    |     2    |   0  |            102           |   707  |
|  NA  |      5360      |    212   |  129 | 1075 |    3   |   2242   |   3489   |   1187   |    59    |    32    |   2648   |       157      |    269   |    185   |       2      |    34    |    136   |       0      |      121     |     0    | 1170 |      804     |     674    |    3369    |     312    |      1     |    0   |    600   |    226   |      139     |  45  |  47  |   0  |  379 |  131 |      23      |    59    |     68     |         0        |      1     |      303     |      0     |     99     |   8010   |       5      |   1209   |        4       |     59     |    111   |  40  |         0        |   6  |       0      |     2    |     2    |      0     |   8  |  106 |     2    |     5    |   0  |            129           |   703  |
|  NA  |      5439      |    206   |  131 | 1456 |    3   |   2141   |   3954   |   1241   |    67    |    24    |   2619   |       145      |    433   |    127   |       2      |    11    |    213   |       0      |      238     |     2    | 1202 |      820     |     998    |    4572    |     349    |      0     |    4   |    601   |    239   |      181     |  32  |  34  |   0  |  540 |  107 |      20      |    45    |     143    |         0        |      0     |      560     |      0     |     203    |   8478   |       1      |   1157   |        2       |     16     |    138   |  32  |         1        |   5  |       0      |     7    |     2    |      0     |  11  |  119 |     3    |     6    |   1  |            177           |   799  |
|  NA  |      5080      |    106   |  47  | 1402 |    3   |   2395   |   4771   |   1339   |    66    |    14    |   2816   |       165      |    317   |    77    |       6      |     7    |    239   |       0      |      219     |     1    | 1365 |      876     |     933    |    4592    |     257    |      1     |    5   |    680   |    255   |      176     |  45  |  24  |   0  |  368 |  118 |      23      |    32    |     83     |         0        |      0     |      827     |      0     |     304    |   8412   |       0      |   1433   |        6       |     13     |    141   |  29  |         3        |   5  |       0      |    10    |    11    |      0     |   7  |  114 |     2    |     0    |   1  |            161           |   797  |
|  NA  |      19933     |   2500   |  243 |  371 |    5   |   1311   |    925   |    179   |    60    |     7    |    711   |       57       |    522   |    151   |       7      |    17    |    41    |       7      |      289     |     7    |  442 |      581     |     955    |    4086    |      3     |      0     |   52   |    130   |    33    |       0      |  225 |  164 |   0  |  408 |  76  |      30      |     5    |     11     |         0        |      1     |      779     |     31     |     283    |   3984   |       0      |    28    |       787      |      0     |     2    |   4  |         0        |   1  |       3      |     0    |    40    |      0     |   0  |  289 |     0    |     3    |   0  |            123           |   677  |
|  NA  |      17842     |   2199   |  216 |  237 |    8   |   1410   |   1156   |    177   |    46    |    16    |    669   |       46       |    643   |    139   |       8      |    27    |    53    |       2      |      308     |    10    |  523 |      503     |    1036    |    2416    |     12     |      0     |   370  |    146   |    77    |       0      |  194 |  265 |   0  |  158 |  48  |      12      |     4    |      8     |         0        |      0     |      826     |     18     |     288    |   6728   |       0      |    47    |       450      |      1     |     4    |   2  |         0        |   2  |       3      |     0    |    108   |      0     |   0  |  231 |     1    |     5    |   1  |            119           |   957  |
|  NA  |      18597     |   2460   |  237 |  224 |   12   |   1393   |   1500   |    197   |    86    |    11    |    693   |       27       |    601   |    126   |       7      |    36    |    48    |       1      |      263     |     8    |  481 |      430     |     971    |    1578    |     28     |      0     |   808  |    148   |    52    |       0      |  202 |  325 |   0  |  161 |  69  |      11      |     2    |      2     |         0        |      2     |      555     |     21     |     403    |   6923   |       0      |    55    |       59       |      7     |     9    |   7  |         1        |   1  |       0      |     0    |    59    |      0     |   0  |  174 |     0    |     6    |   1  |            134           |  1228  |
|  NA  |      18437     |   2475   |  159 |  203 |    9   |   1294   |   1640   |    209   |    76    |    29    |    743   |       56       |    757   |    142   |       9      |    38    |    60    |       3      |      457     |    13    |  547 |      536     |    1020    |    1296    |     18     |      0     |   546  |    147   |    58    |       0      |  176 |  302 |   0  |  30  |  78  |      10      |     4    |      3     |         0        |      2     |      296     |      3     |     336    |   5325   |       0      |    103   |        9       |      3     |    12    |  15  |         1        |   1  |       0      |     3    |    67    |      0     |   1  |  202 |     0    |     4    |   2  |            187           |   888  |
|  NA  |      20195     |   2563   |  119 |  120 |   11   |   1045   |   2264   |    173   |    56    |    25    |   1018   |       58       |    815   |    140   |       7      |    19    |    64    |       5      |      377     |    15    |  457 |      520     |    1468    |    1538    |     27     |      0     |   537  |    178   |    55    |       0      |  174 |  318 |   0  |  72  |  65  |      15      |     8    |     11     |         0        |      0     |      311     |      0     |     207    |   6248   |       0      |    146   |       20       |      2     |    10    |  18  |         0        |   1  |       0      |    35    |    51    |      0     |   1  |  180 |     0    |     2    |   2  |            319           |  1650  |
|  NA  |      20995     |   2463   |  96  |  158 |    3   |   1146   |   3472   |    280   |    65    |    32    |   1180   |       69       |    615   |    178   |       5      |    21    |    99    |       4      |      328     |    12    |  454 |      522     |    2320    |    2554    |     22     |      1     |   80   |    198   |    54    |      39      |  177 |  154 |   0  |  59  |  99  |      12      |     4    |      5     |         0        |      0     |      362     |      0     |     165    |   6689   |       0      |    168   |        2       |      0     |    13    |  20  |         0        |   3  |       0      |    25    |    31    |      0     |   2  |  136 |     0    |     5    |   0  |            360           |  1765  |
|  NA  |      18069     |   1637   |  176 |  252 |    3   |   1424   |   3930   |    367   |    70    |    27    |   1376   |       75       |    602   |    180   |      12      |    14    |    64    |       3      |      329     |     7    |  576 |      561     |    2539    |    2155    |     77     |      0     |   10   |    250   |    98    |      81      |  143 |  203 |   0  |  137 |  97  |      18      |     2    |      7     |         0        |      1     |      343     |      0     |     169    |   8253   |       0      |    296   |        7       |      7     |    32    |  17  |         4        |   3  |       1      |    30    |    32    |      0     |   1  |  170 |     0    |     4    |   0  |            321           |  1817  |
|  NA  |      14071     |   1348   |  166 |  257 |    5   |   1601   |   5175   |    479   |    56    |    22    |   1493   |       93       |    574   |    187   |      16      |    26    |    82    |       1      |      535     |     8    |  806 |      729     |    2559    |    3016    |     84     |      1     |    0   |    323   |    80    |      134     |  106 |  162 |   0  |  127 |  98  |      18      |     3    |      9     |         0        |      0     |      375     |      0     |     192    |   6535   |       0      |    461   |        3       |     11     |    31    |  31  |         0        |   2  |       3      |    34    |    19    |      0     |   7  |  158 |     3    |     0    |   1  |            213           |  1783  |
|  NA  |      12925     |   1157   |  203 |  246 |    5   |   1700   |   6351   |    601   |    57    |    11    |   1636   |       88       |   1288   |    214   |      12      |    23    |    58    |       1      |      707     |     8    | 1022 |      784     |    2231    |    2926    |     118    |      1     |    1   |    363   |    96    |      129     |  104 |  157 |   0  |  208 |  104 |      23      |     4    |     12     |         0        |      0     |      226     |      0     |     102    |   5145   |       1      |    528   |        0       |      9     |    47    |  38  |         2        |   7  |       0      |    52    |     8    |      0     |   1  |  124 |     1    |     4    |   1  |            188           |   976  |
|  NA  |      9997      |    902   |  143 |  299 |    3   |   1975   |   6482   |    759   |    68    |    23    |   1874   |       76       |    866   |    203   |      16      |    20    |    75    |       3      |      509     |     3    | 1194 |      916     |    1611    |    2818    |     133    |      1     |    0   |    417   |    97    |      165     |  140 |  105 |   0  |  127 |  88  |      26      |     6    |     12     |         0        |      1     |      338     |      0     |     110    |   5241   |       0      |    755   |        3       |      0     |    53    |  45  |         0        |   5  |       0      |    37    |    10    |      1     |   8  |  153 |     2    |     3    |   2  |            158           |   633  |
|  NA  |      8323      |    510   |  151 |  320 |    3   |   2119   |   4575   |    924   |    56    |    21    |   2259   |       127      |    459   |    223   |      12      |    19    |    86    |       1      |      317     |     1    | 1281 |      885     |    1099    |    3202    |     153    |      2     |    0   |    531   |    121   |      182     |  68  |  99  |   0  |  276 |  112 |      13      |    18    |     41     |         0        |      0     |      243     |      0     |     118    |   4847   |       0      |    967   |        3       |     35     |    60    |  32  |         1        |   5  |       0      |     9    |     8    |      0     |  13  |  160 |     1    |     2    |   0  |            96            |   469  |

``` r
money_avg_taipei_person <- rbind(avg_money_person_66_99[c('年別', '臺北市')], avg_money_person_100_104[c('年別', '臺北市')])
money_avg_taipei_house  <- rbind(avg_money_house_66_99[c('年別', '臺北市')],  avg_money_house_100_104[c('年別', '臺北市')])

knitr::kable(money_avg_taipei_house)
```

| 年別 |  臺北市 |
|:----:|:-------:|
|  69  |  284267 |
|  70  |  334435 |
|  71  |  362846 |
|  72  |  393517 |
|  73  |  405939 |
|  74  |  422786 |
|  75  |  433950 |
|  76  |  470692 |
|  77  |  534189 |
|  78  |  583086 |
|  79  |  657829 |
|  80  |  764274 |
|  81  |  834113 |
|  82  |  955357 |
|  83  | 1052440 |
|  84  | 1112806 |
|  85  | 1137761 |
|  86  | 1191250 |
|  87  | 1196141 |
|  88  | 1208578 |
|  89  | 1237777 |
|  90  | 1217932 |
|  91  | 1232387 |
|  92  | 1232396 |
|  93  | 1225096 |
|  94  | 1236014 |
|  95  | 1262406 |
|  96  | 1287803 |
|  97  | 1271060 |
|  98  | 1246310 |
|  99  | 1298640 |
|  100 | 1251519 |
|  101 | 1278278 |
|  102 | 1279195 |
|  103 | 1292604 |
|  104 | 1314031 |

``` r
knitr::kable(money_avg_taipei_person)
```

| 年別 |   臺北市  |
|:----:|:---------:|
|  69  |  62613.88 |
|  70  |  76181.09 |
|  71  |  83221.56 |
|  72  |  89639.41 |
|  73  |  92049.66 |
|  74  |  97867.13 |
|  75  | 101390.19 |
|  76  | 109718.41 |
|  77  | 132225.00 |
|  78  | 143617.24 |
|  79  | 166961.68 |
|  80  | 191068.50 |
|  81  | 211703.81 |
|  82  | 240039.45 |
|  83  | 265098.24 |
|  84  | 298339.41 |
|  85  | 300200.79 |
|  86  | 314313.98 |
|  87  | 331341.00 |
|  88  | 336651.25 |
|  89  | 338190.44 |
|  90  | 339256.82 |
|  91  | 357213.62 |
|  92  | 365696.14 |
|  93  | 380464.60 |
|  94  | 392385.40 |
|  95  | 377965.87 |
|  96  | 389064.35 |
|  97  | 386340.43 |
|  98  | 387052.80 |
|  99  | 402056.00 |
|  100 | 381561.00 |
|  101 | 398217.00 |
|  102 | 408688.00 |
|  103 | 412973.00 |
|  104 | 426633.00 |

探索式資料分析
--------------

圖文並茂圖文並茂

``` r
#這是R Code Chunk
```

期末專題分析規劃
----------------

期末專題要做XXOOO交叉分析
