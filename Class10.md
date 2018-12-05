Class10
================

Bokus top sellers
=================

``` r
html_book <- read_html("https://www.bokus.com/topplistor/pockettoppen")
html_book %>%
  html_nodes(".pricing__price, .ProductList__authors a, .Item__title--large a") %>%
  html_text() %>%
  str_remove_all("[ ]\n[ ]+Pocket\n[ ]+")
```

    ##  [1] "Silvervägen"                                                     
    ##  [2] "Stina Jackson"                                                   
    ##  [3] "59"                                                              
    ##  [4] "Jag kommer hem till jul"                                         
    ##  [5] "Joanna Bolouri"                                                  
    ##  [6] "55"                                                              
    ##  [7] "1793"                                                            
    ##  [8] "Niklas Natt Och Dag"                                             
    ##  [9] "55"                                                              
    ## [10] "Och sen var hon borta"                                           
    ## [11] "Lisa Jewell"                                                     
    ## [12] "55"                                                              
    ## [13] "Döden är alltid sann"                                            
    ## [14] "Anna Jansson"                                                    
    ## [15] "59"                                                              
    ## [16] "Bortom varje rimligt tvivel"                                     
    ## [17] "Malin Persson Giolito"                                           
    ## [18] "59"                                                              
    ## [19] "Hjärnstark : hur motion och träning stärker din hjärna"          
    ## [20] "Anders Hansen"                                                   
    ## [21] "59"                                                              
    ## [22] "Utan att släppa taget"                                           
    ## [23] "Denise Rudberg"                                                  
    ## [24] "Hugo Rehnberg"                                                   
    ## [25] "55"                                                              
    ## [26] "Omgiven av idioter : hur man förstår dem som inte går att förstå"
    ## [27] "Thomas Erikson"                                                  
    ## [28] "59"                                                              
    ## [29] "Mannen som sökte sin skugga"                                     
    ## [30] "David Lagercrantz"                                               
    ## [31] "59"                                                              
    ## [32] "De sju som såg"                                                  
    ## [33] "Denise Rudberg"                                                  
    ## [34] "59"                                                              
    ## [35] "Kvinnan i fönstret"                                              
    ## [36] "A J Finn"                                                        
    ## [37] "59"                                                              
    ## [38] "Min mammas hemlighet"                                            
    ## [39] "Nikola Scott"                                                    
    ## [40] "59"                                                              
    ## [41] "Koka björn"                                                      
    ## [42] "Mikael Niemi"                                                    
    ## [43] "55"                                                              
    ## [44] "En annan Alice"                                                  
    ## [45] "Liane Moriarty"                                                  
    ## [46] "59"                                                              
    ## [47] "Coffin Road"                                                     
    ## [48] "Peter May"                                                       
    ## [49] "55"                                                              
    ## [50] "Ormen i Essex"                                                   
    ## [51] "Sarah Perry"                                                     
    ## [52] "59"                                                              
    ## [53] "Höstdåd"                                                         
    ## [54] "Anders De La Motte"                                              
    ## [55] "59"                                                              
    ## [56] "Lust: Håll glöden levande i långa relationer"                    
    ## [57] "Esther Perel"                                                    
    ## [58] "55"                                                              
    ## [59] "Sanningen"                                                       
    ## [60] "Harlan Coben"                                                    
    ## [61] "55"                                                              
    ## [62] "Systrarna"                                                       
    ## [63] "Claire Douglas"                                                  
    ## [64] "55"                                                              
    ## [65] "Flätan"                                                          
    ## [66] "Laetitia Colombani"                                              
    ## [67] "55"                                                              
    ## [68] "Hustrun"                                                         
    ## [69] "Meg Wolitzer"                                                    
    ## [70] "59"                                                              
    ## [71] "Den stora utställningen"                                         
    ## [72] "Marie Hermanson"                                                 
    ## [73] "59"                                                              
    ## [74] "Bödelskyssen"                                                    
    ## [75] "Mons Kallentoft"                                                 
    ## [76] "59"                                                              
    ## [77] "I år blir det nog bättre"                                        
    ## [78] "Maeve Binchy"                                                    
    ## [79] "55"                                                              
    ## [80] "Se till mig som liten är"                                        
    ## [81] "Dag Öhrlund"                                                     
    ## [82] "55"                                                              
    ## [83] "Vågornas viskningar"                                             
    ## [84] "Tamara McKinley"                                                 
    ## [85] "59"                                                              
    ## [86] "Det tyska huset"                                                 
    ## [87] "Arnaldur Indridason"                                             
    ## [88] "59"                                                              
    ## [89] "Skuggan av tvivel"                                               
    ## [90] "Peter Robinson"                                                  
    ## [91] "59"

``` r
html_book %>%
  html_nodes(".u-vertical--top") %>%
  html_attr("data-rating")
```

    ##  [1] "4" "4" "3" "3" "4" "4" "4" "4" "4" "3" "4" "4" "4" "4" "5" "4" "5"
    ## [18] "5" "4" "4" "4"

SHL players
===========

``` r
html_rl <- read_html("https://www.shl.se/lag/087a-087aTQv9u__frolunda-hc/qQ9-a5b4QRqdS__ryan-lasch")

info <- html_text(html_nodes(html_rl, ".rmss_c-squad__player-header-info-items-item-value"))
head <- html_text(html_nodes(html_rl, ".rmss_c-squad__player-header-info-items-item-name"))
rbind(head, info)
```

    ##      [,1]         [,2]    [,3]           [,4]     [,5]    [,6]     
    ## head "Född"       "Ålder" "Nationalitet" "Längd"  "Vikt"  "Skjuter"
    ## info "1987-01-22" "31"    "USA"          "170 cm" "71 kg" "Höger"  
    ##      [,7]         [,8]    [,9]           [,10]    [,11]   [,12]    
    ## head "Född"       "Ålder" "Nationalitet" "Längd"  "Vikt"  "Skjuter"
    ## info "1987-01-22" "31"    "USA"          "170 cm" "71 kg" "Höger"

``` r
html_statistic <- read_html("https://www.shl.se/lag/087a-087aTQv9u__frolunda-hc/qQ9-a5b4QRqdS__ryan-lasch/statistics")
html_statistic %>%
  html_table() %>% .[[1]]
```

    ##       Säsong      Serie Lag GP  G  A TP +/- PIM SOG TOI/GP
    ## 1  2010/2011 Elitserien SSK 55 12 18 30 -16  40  76  15:02
    ## 2  2012/2013 Elitserien VLH 10  0  5  5   4   4  16  16:22
    ## 3  2013/2014        SHL VLH 54 20 16 36  13  16  96  16:07
    ## 4  2013/2014   Slutspel VLH 12  1  5  6   4   0  15  14:15
    ## 5  2014/2015        SHL FHC 12  6  8 14   5   2  24  17:26
    ## 6  2014/2015   Slutspel FHC  9  2  1  3   1   4  16  17:21
    ## 7  2015/2016        SHL FHC 51 15 36 51  10  20  82  17:46
    ## 8  2015/2016   Slutspel FHC 16  8 11 19  11   2  41  18:12
    ## 9  2017/2018        SHL FHC 49 15 40 55   4  18 108  18:33
    ## 10 2017/2018   Slutspel FHC  5  0  3  3  -2   2  13  20:00
    ## 11 2018/2019        SHL FHC 23  7 15 22  -3  14  33  19:20

``` r
html_team <- read_html("https://www.shl.se/lag/2459-2459QTs1f__djurgarden-hockey/roster")
firstName <- html_team %>% 
  html_nodes(".rmss_c-squad__team-cont-roster-group-item-info") %>% 
  html_text() %>%
  str_extract_all("[A-ZÄÅÖ][a-zäåö]+") %>%
  unlist()
lastName <- html_team %>% 
  html_nodes(".rmss_c-squad__team-cont-roster-group-item-info") %>% 
  html_text() %>%
  str_remove("[A-ZÄÅÖ][a-zäåö]+") %>%
  str_extract_all("\\D+") %>%
  unlist()
Number <- html_team %>% 
  html_nodes(".rmss_c-squad__team-cont-roster-group-item-info") %>% 
  html_text() %>%
  str_extract_all("\\d+") %>%
  unlist()
team <- data.frame(First_Name = firstName[1:28], Last_Name = lastName[1:28], Number = Number)
team
```

    ##    First_Name        Last_Name Number
    ## 1       Robin           Jensen     35
    ## 2      Jensen        Reideborn     39
    ## 3        Adam        Johansson     18
    ## 4   Reideborn        Hultström     33
    ## 5       Simon       Pettersson     37
    ## 6   Johansson           Alsing     38
    ## 7       Linus          Nilsson     43
    ## 8   Hultström        Bernhardt     49
    ## 9      Jesper        Petersson     53
    ## 10 Pettersson            Urbom     55
    ## 11       Olle           Norell     56
    ## 12     Alsing         Bergfors     10
    ## 13        Tom         Wirtanen     13
    ## 14    Nilsson            Lilja     15
    ## 15      David         Eriksson     17
    ## 16  Bernhardt         Engqvist     21
    ## 17      Bobbo Walli-Walterholm     24
    ## 18  Petersson              Wiå     28
    ## 19  Alexander       Strandberg     29
    ## 20      Urbom         Axelsson     31
    ## 21      Robin           Brodin     34
    ## 22     Norell         Josefson     40
    ## 23     Niclas            Ågren     62
    ## 24   Bergfors        Davidsson     70
    ## 25    Petteri        Davidsson     71
    ## 26   Wirtanen         Bemström     72
    ## 27      Jakob          Possler     80
    ## 28      Lilja  Jonsson-Fjällby     91

TV listings
===========

``` r
html_tv <- read_html("https://www.tv.nu/kanal/svt1/2018-12-05")
channel_id <- html_tv %>%
  html_nodes(".box--emphasize") %>%
  html_attr("data-channel-id")
title <- html_tv %>%
  html_nodes(".box--emphasize") %>%
  html_attr("data-title")
start_time <- html_tv %>%
  html_nodes(".box--emphasize") %>%
  html_attr("data-start-time")
```

SVT news
========

``` r
html_svt <- read_html("https://www.svt.se/")
html_svt %>%
  html_nodes(".nyh_teaser__heading-title") %>%
  html_text()
```

    ##  [1] "Nobelpristagaren dömer ut klimatmålet ”Är inte möjligt”"                     
    ##  [2] "”En obegriplig utmaning, alla måste säga ja”"                                
    ##  [3] "Världens koldioxidutsläpp väntas slå rekord 2018"                            
    ##  [4] "Ett litet men mycket viktigt steg för Jemen – efter fyra år av strider"      
    ##  [5] "Jemens delegation i Sverige – samtal inleds på torsdag"                      
    ##  [6] "Fredsaktivisten: ”Egentligen ett krig mellan Saudiarabien och Iran”"         
    ##  [7] "”En feministisk utrikespolitik viktigare nu än någonsin”"                    
    ##  [8] "Huthi-rebellerna på plats för fredssamtal – har landat i Sverige"            
    ##  [9] "Minidokumentär: Det här handlar konflikten i Jemen om"                       
    ## [10] "”En feministisk utrikespolitik viktigare nu än någonsin”"                    
    ## [11] "Nytt rekord för Kallur – två år efter karriärens slut"                       
    ## [12] "Det här är ”gula västarna” – som skapat kaos i Paris"                        
    ## [13] "Frankrike backar om höjda bensinpriserna"                                    
    ## [14] "Se de dramatiska bilderna från protesterna i Frankrike"                      
    ## [15] "Trump: Glad att Macron insett Parisavtalets fatala brister"                  
    ## [16] "Svenska barnfamiljen mitt i kaoset i Paris: ”Som en krigszon”"               
    ## [17] "Brand i ladugård på Gotland – kor befaras innebrända"                        
    ## [18] "Så blev rådgivarens Twitter-slarv en anti-Trump-kampanj"                     
    ## [19] "Årets spel stäms för stöld av dans"                                          
    ## [20] "Kim Andersson klev av skadad i förlustmatchen"                               
    ## [21] "Likdelar hittade i skogsområde i Ludvika"                                    
    ## [22] "Så ska 5G göra vardagen mer uppkopplad"                                      
    ## [23] "”Europa har en utmaning att hänga med”"                                      
    ## [24] "Från fyra till fem – detta är 5G"                                            
    ## [25] "Putin: Skaffar USA förbjudna missiler gör vi det också"                      
    ## [26] "”Vår högsta önskan är att vi ska kunna återvända hem”"                       
    ## [27] "Fredspriset till Nadia Murad väcker hopp"                                    
    ## [28] "Togs till fånga av terroristerna – nu får hon Nobels fredspris"              
    ## [29] "Skidgymnasieelever: Svårt att sluta med de farliga vallorna "                
    ## [30] "Kemikalieinspektionen: Undvik fluorvallor"                                   
    ## [31] "Proffset tipsar hur du vallar säkert"                                        
    ## [32] "”Många har trott att de varit ensamma”"                                      
    ## [33] "”Många har fått både kroppsliga och mentala ärr”"                            
    ## [34] "Så blev ett mandarinnät en medicinteknisk produkt"                           
    ## [35] "Iren skadades i lins-operationen – vägrar skriva på företagets tystnadsavtal"
    ## [36] "Läkare har tvingats operera ut 1 300 undermåliga hjärtprodukter"             
    ## [37] "Långt mellan S och C i klimatfrågor – kan bli budgethinder"                  
    ## [38] "Krav på tuffare miljöåtgärder – men ingen vill beskatta köttet"              
    ## [39] "”Lööf och Sjöstedt verkar strunta helt i klimatet”"                          
    ## [40] "Över 80 gripna i razzia mot italiensk maffia i flera länder "                
    ## [41] "Här rivs maffians lyxvilla"                                                  
    ## [42] "Klimatet – vad kan vi göra?"                                                 
    ## [43] "Stämmer det? Hjälp oss granska makthavare"                                   
    ## [44] "Nörda ner dig – så röstade svenskarna "                                      
    ## [45] "Expertens 3 tips: Bästa julfilmerna att streama"                             
    ## [46] "Så lurar blufföretag dig att köpa bantningspiller på Facebook"               
    ## [47] "”Det enda som är svårt är att jag ska lämna mina barn”"                      
    ## [48] "Här slår den 100 meter höga sandväggen till mot staden i Kina"               
    ## [49] "Därför är lågutbildade män oftare barnlösa"                                  
    ## [50] "Gymping för käken – så gör du"                                               
    ## [51] "Värmeljusen som ger billigast ljusmys för pengarna"                          
    ## [52] "Här tillverkas SVT:s stora julkalender"                                      
    ## [53] "Avskaffandet av sommartid dröjer"                                            
    ## [54] "Varningen: Julbelysning kan innehålla farliga ämnen"
