Class6
================

Sort weekdate order\_day &lt;- c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday") mutate(Weekday = weekdays(Date)) %&gt;% mutate(Weekday = factor(Weekday, order\_day))

``` r
weekdays(Sys.Date())
```

    ## [1] "Thursday"

SL SQL
======

Connect to the database, list the tables and figure out how they relate to eachother.

``` r
con <- dbConnect(SQLite(), "Class_files/sthlm_metro.sqlite")
tables <- dbListTables(con)
#lapply(tables, dbReadTable, conn = con)
```

Query for the LineName from table Line where LineNumber is 18.

``` r
dbGetQuery(con, "SELECT StationName, count(PlatformNumber) AS Number 
           FROM Platform GROUP BY StationName ORDER BY Number DESC")
```

    ##             StationName Number
    ## 1           T-Centralen      6
    ## 2               Slussen      5
    ## 3          Fridhemsplan      4
    ## 4            Gamla stan      4
    ## 5          Gullmarsplan      4
    ## 6              Högdalen      4
    ## 7           Liljeholmen      4
    ## 8          Skärmarbrink      4
    ## 9             Vällingby      4
    ## 10              Åkeshov      4
    ## 11                 Alby      3
    ## 12         Hallonbergen      3
    ## 13                Sätra      3
    ## 14        Västra skogen      3
    ## 15         Abrahamsberg      2
    ## 16               Akalla      2
    ## 17                Alvik      2
    ## 18             Aspudden      2
    ## 19            Axelsberg      2
    ## 20          Bagarmossen      2
    ## 21            Bandhagen      2
    ## 22           Bergshamra      2
    ## 23           Björkhagen      2
    ## 24           Blackeberg      2
    ## 25               Blåsut      2
    ## 26              Bredäng      2
    ## 27           Brommaplan      2
    ## 28    Danderyds sjukhus      2
    ## 29                Duvbo      2
    ## 30         Enskede gård      2
    ## 31               Farsta      2
    ## 32        Farsta strand      2
    ## 33               Fittja      2
    ## 34             Fruängen      2
    ## 35               Globen      2
    ## 36            Gubbängen      2
    ## 37               Gärdet      2
    ## 38             Hagsätra      2
    ## 39             Hallunda      2
    ## 40       Hammarbyhöjden      2
    ## 41              Hjulsta      2
    ## 42            Hornstull      2
    ## 43                Husby      2
    ## 44             Huvudsta      2
    ## 45       Hägerstensåsen      2
    ## 46        Hässelby gård      2
    ## 47      Hässelby strand      2
    ## 48           Hökarängen      2
    ## 49             Hötorget      2
    ## 50        Islandstorget      2
    ## 51          Johannelund      2
    ## 52            Karlaplan      2
    ## 53                Kista      2
    ## 54         Kristineberg      2
    ## 55      Kungsträdgården      2
    ## 56             Kärrtorp      2
    ## 57          Mariatorget      2
    ## 58                Masmo      2
    ## 59     Medborgarplatsen      2
    ## 60     Midsommarkransen      2
    ## 61          Mälarhöjden      2
    ## 62        Mörby centrum      2
    ## 63             Norsborg      2
    ## 64            Näckrosen      2
    ## 65             Odenplan      2
    ## 66              Rinkeby      2
    ## 67               Rissne      2
    ## 68              Ropsten      2
    ## 69              Råcksta      2
    ## 70             Rådhuset      2
    ## 71         Rådmansgatan      2
    ## 72              Rågsved      2
    ## 73        S:t Eriksplan      2
    ## 74            Sandsborg      2
    ## 75            Skanstull      2
    ## 76            Skarpnäck      2
    ## 77     Skogskyrkogården      2
    ## 78           Skärholmen      2
    ## 79           Sockenplan      2
    ## 80        Solna centrum      2
    ## 81         Solna strand      2
    ## 82              Stadion      2
    ## 83           Stadshagen      2
    ## 84         Stora mossen      2
    ## 85              Stureby      2
    ## 86  Sundbybergs centrum      2
    ## 87             Svedmyra      2
    ## 88           Tallkrogen      2
    ## 89   Tekniska högskolan      2
    ## 90          Telefonplan      2
    ## 91               Tensta      2
    ## 92         Thorildsplan      2
    ## 93        Universitetet      2
    ## 94           Västertorp      2
    ## 95              Vårberg      2
    ## 96           Vårby gård      2
    ## 97          Zinkensdamm      2
    ## 98            Ängbyplan      2
    ## 99             Örnsberg      2
    ## 100      Östermalmstorg      2
    ## 101     Kymlinge norrut      1
    ## 102    Kymlinge söderut      1

Query for the five most southern StationName, where position is measured as the average Latitude of its PlatformNumber.

``` r
dbGetQuery(con, "SELECT StationName, avg(Latitude) AS avg_Latitude FROM Platform 
           GROUP BY StationName ORDER BY avg_Latitude DESC LIMIT 5")
```

    ##     StationName avg_Latitude
    ## 1        Akalla     59.41381
    ## 2         Husby     59.40971
    ## 3         Kista     59.40295
    ## 4 Mörby centrum     59.39826
    ## 5       Hjulsta     59.39677

Query for the number of stations on LineNumber 18.

``` r
dbGetQuery(con, "SELECT LineNumber, count(DISTINCT StationName) AS Number
           FROM LinePlatform LEFT JOIN Platform 
           ON LinePlatform.PlatformNumber = Platform.PlatformNumber
           WHERE LineNumber = 18")
```

    ##   LineNumber Number
    ## 1         18     35

Query for all StationName on LineNumber 18 in alphabetical order.

``` r
dbGetQuery(con, "SELECT DISTINCT StationName FROM LinePlatform LEFT JOIN Platform 
           ON LinePlatform.PlatformNumber = Platform.PlatformNumber
           WHERE LineNumber = 18")
```

    ##         StationName
    ## 1           Slussen
    ## 2        Gamla stan
    ## 3          Hötorget
    ## 4      Rådmansgatan
    ## 5          Odenplan
    ## 6     S:t Eriksplan
    ## 7      Fridhemsplan
    ## 8      Thorildsplan
    ## 9      Kristineberg
    ## 10            Alvik
    ## 11     Stora mossen
    ## 12     Abrahamsberg
    ## 13       Brommaplan
    ## 14          Åkeshov
    ## 15        Ängbyplan
    ## 16    Islandstorget
    ## 17       Blackeberg
    ## 18          Råcksta
    ## 19        Vällingby
    ## 20      Johannelund
    ## 21    Hässelby gård
    ## 22  Hässelby strand
    ## 23 Medborgarplatsen
    ## 24        Skanstull
    ## 25     Gullmarsplan
    ## 26     Skärmarbrink
    ## 27           Blåsut
    ## 28        Sandsborg
    ## 29 Skogskyrkogården
    ## 30       Tallkrogen
    ## 31        Gubbängen
    ## 32       Hökarängen
    ## 33           Farsta
    ## 34    Farsta strand
    ## 35      T-Centralen

``` r
dbDisconnect(con)
```

Pokemon SQL
===========

``` r
con <- dbConnect(SQLite(), "Class_files/pokemon.sqlite")
tables <- dbListTables(con)
lapply(tables, dbReadTable, conn = con)
```

    ## [[1]]
    ##        id              identifier species_id height weight base_experience
    ## 1       1               bulbasaur          1      7     69              64
    ## 2       2                 ivysaur          2     10    130             142
    ## 3       3                venusaur          3     20   1000             236
    ## 4       4              charmander          4      6     85              62
    ## 5       5              charmeleon          5     11    190             142
    ## 6       6               charizard          6     17    905             240
    ## 7       7                squirtle          7      5     90              63
    ## 8       8               wartortle          8     10    225             142
    ## 9       9               blastoise          9     16    855             239
    ## 10     10                caterpie         10      3     29              39
    ## 11     11                 metapod         11      7     99              72
    ## 12     12              butterfree         12     11    320             178
    ## 13     13                  weedle         13      3     32              39
    ## 14     14                  kakuna         14      6    100              72
    ## 15     15                beedrill         15     10    295             178
    ## 16     16                  pidgey         16      3     18              50
    ## 17     17               pidgeotto         17     11    300             122
    ## 18     18                 pidgeot         18     15    395             216
    ## 19     19                 rattata         19      3     35              51
    ## 20     20                raticate         20      7    185             145
    ## 21     21                 spearow         21      3     20              52
    ## 22     22                  fearow         22     12    380             155
    ## 23     23                   ekans         23     20     69              58
    ## 24     24                   arbok         24     35    650             157
    ## 25     25                 pikachu         25      4     60             112
    ## 26     26                  raichu         26      8    300             218
    ## 27     27               sandshrew         27      6    120              60
    ## 28     28               sandslash         28     10    295             158
    ## 29     29               nidoran-f         29      4     70              55
    ## 30     30                nidorina         30      8    200             128
    ## 31     31               nidoqueen         31     13    600             227
    ## 32     32               nidoran-m         32      5     90              55
    ## 33     33                nidorino         33      9    195             128
    ## 34     34                nidoking         34     14    620             227
    ## 35     35                clefairy         35      6     75             113
    ## 36     36                clefable         36     13    400             217
    ## 37     37                  vulpix         37      6     99              60
    ## 38     38               ninetales         38     11    199             177
    ## 39     39              jigglypuff         39      5     55              95
    ## 40     40              wigglytuff         40     10    120             196
    ## 41     41                   zubat         41      8     75              49
    ## 42     42                  golbat         42     16    550             159
    ## 43     43                  oddish         43      5     54              64
    ## 44     44                   gloom         44      8     86             138
    ## 45     45               vileplume         45     12    186             221
    ## 46     46                   paras         46      3     54              57
    ## 47     47                parasect         47     10    295             142
    ## 48     48                 venonat         48     10    300              61
    ## 49     49                venomoth         49     15    125             158
    ## 50     50                 diglett         50      2      8              53
    ## 51     51                 dugtrio         51      7    333             149
    ## 52     52                  meowth         52      4     42              58
    ## 53     53                 persian         53     10    320             154
    ## 54     54                 psyduck         54      8    196              64
    ## 55     55                 golduck         55     17    766             175
    ## 56     56                  mankey         56      5    280              61
    ## 57     57                primeape         57     10    320             159
    ## 58     58               growlithe         58      7    190              70
    ## 59     59                arcanine         59     19   1550             194
    ## 60     60                 poliwag         60      6    124              60
    ## 61     61               poliwhirl         61     10    200             135
    ## 62     62               poliwrath         62     13    540             230
    ## 63     63                    abra         63      9    195              62
    ## 64     64                 kadabra         64     13    565             140
    ## 65     65                alakazam         65     15    480             225
    ## 66     66                  machop         66      8    195              61
    ## 67     67                 machoke         67     15    705             142
    ## 68     68                 machamp         68     16   1300             227
    ## 69     69              bellsprout         69      7     40              60
    ## 70     70              weepinbell         70     10     64             137
    ## 71     71              victreebel         71     17    155             221
    ## 72     72               tentacool         72      9    455              67
    ## 73     73              tentacruel         73     16    550             180
    ## 74     74                 geodude         74      4    200              60
    ## 75     75                graveler         75     10   1050             137
    ## 76     76                   golem         76     14   3000             223
    ## 77     77                  ponyta         77     10    300              82
    ## 78     78                rapidash         78     17    950             175
    ## 79     79                slowpoke         79     12    360              63
    ## 80     80                 slowbro         80     16    785             172
    ## 81     81               magnemite         81      3     60              65
    ## 82     82                magneton         82     10    600             163
    ## 83     83               farfetchd         83      8    150             132
    ## 84     84                   doduo         84     14    392              62
    ## 85     85                  dodrio         85     18    852             165
    ## 86     86                    seel         86     11    900              65
    ## 87     87                 dewgong         87     17   1200             166
    ## 88     88                  grimer         88      9    300              65
    ## 89     89                     muk         89     12    300             175
    ## 90     90                shellder         90      3     40              61
    ## 91     91                cloyster         91     15   1325             184
    ## 92     92                  gastly         92     13      1              62
    ## 93     93                 haunter         93     16      1             142
    ## 94     94                  gengar         94     15    405             225
    ## 95     95                    onix         95     88   2100              77
    ## 96     96                 drowzee         96     10    324              66
    ## 97     97                   hypno         97     16    756             169
    ## 98     98                  krabby         98      4     65              65
    ## 99     99                 kingler         99     13    600             166
    ## 100   100                 voltorb        100      5    104              66
    ## 101   101               electrode        101     12    666             172
    ## 102   102               exeggcute        102      4     25              65
    ## 103   103               exeggutor        103     20   1200             186
    ## 104   104                  cubone        104      4     65              64
    ## 105   105                 marowak        105     10    450             149
    ## 106   106               hitmonlee        106     15    498             159
    ## 107   107              hitmonchan        107     14    502             159
    ## 108   108               lickitung        108     12    655              77
    ## 109   109                 koffing        109      6     10              68
    ## 110   110                 weezing        110     12     95             172
    ## 111   111                 rhyhorn        111     10   1150              69
    ## 112   112                  rhydon        112     19   1200             170
    ## 113   113                 chansey        113     11    346             395
    ## 114   114                 tangela        114     10    350              87
    ## 115   115              kangaskhan        115     22    800             172
    ## 116   116                  horsea        116      4     80              59
    ## 117   117                  seadra        117     12    250             154
    ## 118   118                 goldeen        118      6    150              64
    ## 119   119                 seaking        119     13    390             158
    ## 120   120                  staryu        120      8    345              68
    ## 121   121                 starmie        121     11    800             182
    ## 122   122                 mr-mime        122     13    545             161
    ## 123   123                 scyther        123     15    560             100
    ## 124   124                    jynx        124     14    406             159
    ## 125   125              electabuzz        125     11    300             172
    ## 126   126                  magmar        126     13    445             173
    ## 127   127                  pinsir        127     15    550             175
    ## 128   128                  tauros        128     14    884             172
    ## 129   129                magikarp        129      9    100              40
    ## 130   130                gyarados        130     65   2350             189
    ## 131   131                  lapras        131     25   2200             187
    ## 132   132                   ditto        132      3     40             101
    ## 133   133                   eevee        133      3     65              65
    ## 134   134                vaporeon        134     10    290             184
    ## 135   135                 jolteon        135      8    245             184
    ## 136   136                 flareon        136      9    250             184
    ## 137   137                 porygon        137      8    365              79
    ## 138   138                 omanyte        138      4     75              71
    ## 139   139                 omastar        139     10    350             173
    ## 140   140                  kabuto        140      5    115              71
    ## 141   141                kabutops        141     13    405             173
    ## 142   142              aerodactyl        142     18    590             180
    ## 143   143                 snorlax        143     21   4600             189
    ## 144   144                articuno        144     17    554             261
    ## 145   145                  zapdos        145     16    526             261
    ## 146   146                 moltres        146     20    600             261
    ## 147   147                 dratini        147     18     33              60
    ## 148   148               dragonair        148     40    165             147
    ## 149   149               dragonite        149     22   2100             270
    ## 150   150                  mewtwo        150     20   1220             306
    ## 151   151                     mew        151      4     40             270
    ## 152   152               chikorita        152      9     64              64
    ## 153   153                 bayleef        153     12    158             142
    ## 154   154                meganium        154     18   1005             236
    ## 155   155               cyndaquil        155      5     79              62
    ## 156   156                 quilava        156      9    190             142
    ## 157   157              typhlosion        157     17    795             240
    ## 158   158                totodile        158      6     95              63
    ## 159   159                croconaw        159     11    250             142
    ## 160   160              feraligatr        160     23    888             239
    ## 161   161                 sentret        161      8     60              43
    ## 162   162                  furret        162     18    325             145
    ## 163   163                hoothoot        163      7    212              52
    ## 164   164                 noctowl        164     16    408             158
    ## 165   165                  ledyba        165     10    108              53
    ## 166   166                  ledian        166     14    356             137
    ## 167   167                spinarak        167      5     85              50
    ## 168   168                 ariados        168     11    335             140
    ## 169   169                  crobat        169     18    750             241
    ## 170   170                chinchou        170      5    120              66
    ## 171   171                 lanturn        171     12    225             161
    ## 172   172                   pichu        172      3     20              41
    ## 173   173                  cleffa        173      3     30              44
    ## 174   174               igglybuff        174      3     10              42
    ## 175   175                  togepi        175      3     15              49
    ## 176   176                 togetic        176      6     32             142
    ## 177   177                    natu        177      2     20              64
    ## 178   178                    xatu        178     15    150             165
    ## 179   179                  mareep        179      6     78              56
    ## 180   180                 flaaffy        180      8    133             128
    ## 181   181                ampharos        181     14    615             230
    ## 182   182               bellossom        182      4     58             221
    ## 183   183                  marill        183      4     85              88
    ## 184   184               azumarill        184      8    285             189
    ## 185   185               sudowoodo        185     12    380             144
    ## 186   186                politoed        186     11    339             225
    ## 187   187                  hoppip        187      4      5              50
    ## 188   188                skiploom        188      6     10             119
    ## 189   189                jumpluff        189      8     30             207
    ## 190   190                   aipom        190      8    115              72
    ## 191   191                 sunkern        191      3     18              36
    ## 192   192                sunflora        192      8     85             149
    ## 193   193                   yanma        193     12    380              78
    ## 194   194                  wooper        194      4     85              42
    ## 195   195                quagsire        195     14    750             151
    ## 196   196                  espeon        196      9    265             184
    ## 197   197                 umbreon        197     10    270             184
    ## 198   198                 murkrow        198      5     21              81
    ## 199   199                slowking        199     20    795             172
    ## 200   200              misdreavus        200      7     10              87
    ## 201   201                   unown        201      5     50             118
    ## 202   202               wobbuffet        202     13    285             142
    ## 203   203               girafarig        203     15    415             159
    ## 204   204                  pineco        204      6     72              58
    ## 205   205              forretress        205     12   1258             163
    ## 206   206               dunsparce        206     15    140             145
    ## 207   207                  gligar        207     11    648              86
    ## 208   208                 steelix        208     92   4000             179
    ## 209   209                snubbull        209      6     78              60
    ## 210   210                granbull        210     14    487             158
    ## 211   211                qwilfish        211      5     39              88
    ## 212   212                  scizor        212     18   1180             175
    ## 213   213                 shuckle        213      6    205             177
    ## 214   214               heracross        214     15    540             175
    ## 215   215                 sneasel        215      9    280              86
    ## 216   216               teddiursa        216      6     88              66
    ## 217   217                ursaring        217     18   1258             175
    ## 218   218                  slugma        218      7    350              50
    ## 219   219                magcargo        219      8    550             151
    ## 220   220                  swinub        220      4     65              50
    ## 221   221               piloswine        221     11    558             158
    ## 222   222                 corsola        222      6     50             144
    ## 223   223                remoraid        223      6    120              60
    ## 224   224               octillery        224      9    285             168
    ## 225   225                delibird        225      9    160             116
    ## 226   226                 mantine        226     21   2200             170
    ## 227   227                skarmory        227     17    505             163
    ## 228   228                houndour        228      6    108              66
    ## 229   229                houndoom        229     14    350             175
    ## 230   230                 kingdra        230     18   1520             243
    ## 231   231                  phanpy        231      5    335              66
    ## 232   232                 donphan        232     11   1200             175
    ## 233   233                porygon2        233      6    325             180
    ## 234   234                stantler        234     14    712             163
    ## 235   235                smeargle        235     12    580              88
    ## 236   236                 tyrogue        236      7    210              42
    ## 237   237               hitmontop        237     14    480             159
    ## 238   238                smoochum        238      4     60              61
    ## 239   239                  elekid        239      6    235              72
    ## 240   240                   magby        240      7    214              73
    ## 241   241                 miltank        241     12    755             172
    ## 242   242                 blissey        242     15    468             608
    ## 243   243                  raikou        243     19   1780             261
    ## 244   244                   entei        244     21   1980             261
    ## 245   245                 suicune        245     20   1870             261
    ## 246   246                larvitar        246      6    720              60
    ## 247   247                 pupitar        247     12   1520             144
    ## 248   248               tyranitar        248     20   2020             270
    ## 249   249                   lugia        249     52   2160             306
    ## 250   250                   ho-oh        250     38   1990             306
    ## 251   251                  celebi        251      6     50             270
    ## 252   252                 treecko        252      5     50              62
    ## 253   253                 grovyle        253      9    216             142
    ## 254   254                sceptile        254     17    522             239
    ## 255   255                 torchic        255      4     25              62
    ## 256   256               combusken        256      9    195             142
    ## 257   257                blaziken        257     19    520             239
    ## 258   258                  mudkip        258      4     76              62
    ## 259   259               marshtomp        259      7    280             142
    ## 260   260                swampert        260     15    819             241
    ## 261   261               poochyena        261      5    136              56
    ## 262   262               mightyena        262     10    370             147
    ## 263   263               zigzagoon        263      4    175              56
    ## 264   264                 linoone        264      5    325             147
    ## 265   265                 wurmple        265      3     36              56
    ## 266   266                 silcoon        266      6    100              72
    ## 267   267               beautifly        267     10    284             178
    ## 268   268                 cascoon        268      7    115              72
    ## 269   269                  dustox        269     12    316             173
    ## 270   270                   lotad        270      5     26              44
    ## 271   271                  lombre        271     12    325             119
    ## 272   272                ludicolo        272     15    550             216
    ## 273   273                  seedot        273      5     40              44
    ## 274   274                 nuzleaf        274     10    280             119
    ## 275   275                 shiftry        275     13    596             216
    ## 276   276                 taillow        276      3     23              54
    ## 277   277                 swellow        277      7    198             159
    ## 278   278                 wingull        278      6     95              54
    ## 279   279                pelipper        279     12    280             154
    ## 280   280                   ralts        280      4     66              40
    ## 281   281                  kirlia        281      8    202              97
    ## 282   282               gardevoir        282     16    484             233
    ## 283   283                 surskit        283      5     17              54
    ## 284   284              masquerain        284      8     36             159
    ## 285   285               shroomish        285      4     45              59
    ## 286   286                 breloom        286     12    392             161
    ## 287   287                 slakoth        287      8    240              56
    ## 288   288                vigoroth        288     14    465             154
    ## 289   289                 slaking        289     20   1305             252
    ## 290   290                 nincada        290      5     55              53
    ## 291   291                 ninjask        291      8    120             160
    ## 292   292                shedinja        292      8     12              83
    ## 293   293                 whismur        293      6    163              48
    ## 294   294                 loudred        294     10    405             126
    ## 295   295                 exploud        295     15    840             221
    ## 296   296                makuhita        296     10    864              47
    ## 297   297                hariyama        297     23   2538             166
    ## 298   298                 azurill        298      2     20              38
    ## 299   299                nosepass        299     10    970              75
    ## 300   300                  skitty        300      6    110              52
    ## 301   301                delcatty        301     11    326             140
    ## 302   302                 sableye        302      5    110             133
    ## 303   303                  mawile        303      6    115             133
    ## 304   304                    aron        304      4    600              66
    ## 305   305                  lairon        305      9   1200             151
    ## 306   306                  aggron        306     21   3600             239
    ## 307   307                meditite        307      6    112              56
    ## 308   308                medicham        308     13    315             144
    ## 309   309               electrike        309      6    152              59
    ## 310   310               manectric        310     15    402             166
    ## 311   311                  plusle        311      4     42             142
    ## 312   312                   minun        312      4     42             142
    ## 313   313                 volbeat        313      7    177             151
    ## 314   314                illumise        314      6    177             151
    ## 315   315                 roselia        315      3     20             140
    ## 316   316                  gulpin        316      4    103              60
    ## 317   317                  swalot        317     17    800             163
    ## 318   318                carvanha        318      8    208              61
    ## 319   319                sharpedo        319     18    888             161
    ## 320   320                 wailmer        320     20   1300              80
    ## 321   321                 wailord        321    145   3980             175
    ## 322   322                   numel        322      7    240              61
    ## 323   323                camerupt        323     19   2200             161
    ## 324   324                 torkoal        324      5    804             165
    ## 325   325                  spoink        325      7    306              66
    ## 326   326                 grumpig        326      9    715             165
    ## 327   327                  spinda        327     11     50             126
    ## 328   328                trapinch        328      7    150              58
    ## 329   329                 vibrava        329     11    153             119
    ## 330   330                  flygon        330     20    820             234
    ## 331   331                  cacnea        331      4    513              67
    ## 332   332                cacturne        332     13    774             166
    ## 333   333                  swablu        333      4     12              62
    ## 334   334                 altaria        334     11    206             172
    ## 335   335                zangoose        335     13    403             160
    ## 336   336                 seviper        336     27    525             160
    ## 337   337                lunatone        337     10   1680             161
    ## 338   338                 solrock        338     12   1540             161
    ## 339   339                barboach        339      4     19              58
    ## 340   340                whiscash        340      9    236             164
    ## 341   341                corphish        341      6    115              62
    ## 342   342               crawdaunt        342     11    328             164
    ## 343   343                  baltoy        343      5    215              60
    ## 344   344                 claydol        344     15   1080             175
    ## 345   345                  lileep        345     10    238              71
    ## 346   346                 cradily        346     15    604             173
    ## 347   347                 anorith        347      7    125              71
    ## 348   348                 armaldo        348     15    682             173
    ## 349   349                  feebas        349      6     74              40
    ## 350   350                 milotic        350     62   1620             189
    ## 351   351                castform        351      3      8             147
    ## 352   352                 kecleon        352     10    220             154
    ## 353   353                 shuppet        353      6     23              59
    ## 354   354                 banette        354     11    125             159
    ## 355   355                 duskull        355      8    150              59
    ## 356   356                dusclops        356     16    306             159
    ## 357   357                 tropius        357     20   1000             161
    ## 358   358                chimecho        358      6     10             159
    ## 359   359                   absol        359     12    470             163
    ## 360   360                  wynaut        360      6    140              52
    ## 361   361                 snorunt        361      7    168              60
    ## 362   362                  glalie        362     15   2565             168
    ## 363   363                  spheal        363      8    395              58
    ## 364   364                  sealeo        364     11    876             144
    ## 365   365                 walrein        365     14   1506             239
    ## 366   366                clamperl        366      4    525              69
    ## 367   367                 huntail        367     17    270             170
    ## 368   368                gorebyss        368     18    226             170
    ## 369   369               relicanth        369     10    234             170
    ## 370   370                 luvdisc        370      6     87             116
    ## 371   371                   bagon        371      6    421              60
    ## 372   372                 shelgon        372     11   1105             147
    ## 373   373               salamence        373     15   1026             270
    ## 374   374                  beldum        374      6    952              60
    ## 375   375                  metang        375     12   2025             147
    ## 376   376               metagross        376     16   5500             270
    ## 377   377                regirock        377     17   2300             261
    ## 378   378                  regice        378     18   1750             261
    ## 379   379               registeel        379     19   2050             261
    ## 380   380                  latias        380     14    400             270
    ## 381   381                  latios        381     20    600             270
    ## 382   382                  kyogre        382     45   3520             302
    ## 383   383                 groudon        383     35   9500             302
    ## 384   384                rayquaza        384     70   2065             306
    ## 385   385                 jirachi        385      3     11             270
    ## 386   386           deoxys-normal        386     17    608             270
    ## 387   387                 turtwig        387      4    102              64
    ## 388   388                  grotle        388     11    970             142
    ## 389   389                torterra        389     22   3100             236
    ## 390   390                chimchar        390      5     62              62
    ## 391   391                monferno        391      9    220             142
    ## 392   392               infernape        392     12    550             240
    ## 393   393                  piplup        393      4     52              63
    ## 394   394                prinplup        394      8    230             142
    ## 395   395                empoleon        395     17    845             239
    ## 396   396                  starly        396      3     20              49
    ## 397   397                staravia        397      6    155             119
    ## 398   398               staraptor        398     12    249             218
    ## 399   399                  bidoof        399      5    200              50
    ## 400   400                 bibarel        400     10    315             144
    ## 401   401               kricketot        401      3     22              39
    ## 402   402              kricketune        402     10    255             134
    ## 403   403                   shinx        403      5     95              53
    ## 404   404                   luxio        404      9    305             127
    ## 405   405                  luxray        405     14    420             235
    ## 406   406                   budew        406      2     12              56
    ## 407   407                roserade        407      9    145             232
    ## 408   408                cranidos        408      9    315              70
    ## 409   409               rampardos        409     16   1025             173
    ## 410   410                shieldon        410      5    570              70
    ## 411   411               bastiodon        411     13   1495             173
    ## 412   412                   burmy        412      2     34              45
    ## 413   413          wormadam-plant        413      5     65             148
    ## 414   414                  mothim        414      9    233             148
    ## 415   415                  combee        415      3     55              49
    ## 416   416               vespiquen        416     12    385             166
    ## 417   417               pachirisu        417      4     39             142
    ## 418   418                  buizel        418      7    295              66
    ## 419   419                floatzel        419     11    335             173
    ## 420   420                 cherubi        420      4     33              55
    ## 421   421                 cherrim        421      5     93             158
    ## 422   422                 shellos        422      3     63              65
    ## 423   423               gastrodon        423      9    299             166
    ## 424   424                 ambipom        424     12    203             169
    ## 425   425                drifloon        425      4     12              70
    ## 426   426                drifblim        426     12    150             174
    ## 427   427                 buneary        427      4     55              70
    ## 428   428                 lopunny        428     12    333             168
    ## 429   429               mismagius        429      9     44             173
    ## 430   430               honchkrow        430      9    273             177
    ## 431   431                 glameow        431      5     39              62
    ## 432   432                 purugly        432     10    438             158
    ## 433   433               chingling        433      2      6              57
    ## 434   434                  stunky        434      4    192              66
    ## 435   435                skuntank        435     10    380             168
    ## 436   436                 bronzor        436      5    605              60
    ## 437   437                bronzong        437     13   1870             175
    ## 438   438                  bonsly        438      5    150              58
    ## 439   439                 mime-jr        439      6    130              62
    ## 440   440                 happiny        440      6    244             110
    ## 441   441                  chatot        441      5     19             144
    ## 442   442               spiritomb        442     10   1080             170
    ## 443   443                   gible        443      7    205              60
    ## 444   444                  gabite        444     14    560             144
    ## 445   445                garchomp        445     19    950             270
    ## 446   446                munchlax        446      6   1050              78
    ## 447   447                   riolu        447      7    202              57
    ## 448   448                 lucario        448     12    540             184
    ## 449   449              hippopotas        449      8    495              66
    ## 450   450               hippowdon        450     20   3000             184
    ## 451   451                 skorupi        451      8    120              66
    ## 452   452                 drapion        452     13    615             175
    ## 453   453                croagunk        453      7    230              60
    ## 454   454               toxicroak        454     13    444             172
    ## 455   455               carnivine        455     14    270             159
    ## 456   456                 finneon        456      4     70              66
    ## 457   457                lumineon        457     12    240             161
    ## 458   458                 mantyke        458     10    650              69
    ## 459   459                  snover        459     10    505              67
    ## 460   460               abomasnow        460     22   1355             173
    ## 461   461                 weavile        461     11    340             179
    ## 462   462               magnezone        462     12   1800             241
    ## 463   463              lickilicky        463     17   1400             180
    ## 464   464               rhyperior        464     24   2828             241
    ## 465   465               tangrowth        465     20   1286             187
    ## 466   466              electivire        466     18   1386             243
    ## 467   467               magmortar        467     16    680             243
    ## 468   468                togekiss        468     15    380             245
    ## 469   469                 yanmega        469     19    515             180
    ## 470   470                 leafeon        470     10    255             184
    ## 471   471                 glaceon        471      8    259             184
    ## 472   472                 gliscor        472     20    425             179
    ## 473   473               mamoswine        473     25   2910             239
    ## 474   474               porygon-z        474      9    340             241
    ## 475   475                 gallade        475     16    520             233
    ## 476   476               probopass        476     14   3400             184
    ## 477   477                dusknoir        477     22   1066             236
    ## 478   478                froslass        478     13    266             168
    ## 479   479                   rotom        479      3      3             154
    ## 480   480                    uxie        480      3      3             261
    ## 481   481                 mesprit        481      3      3             261
    ## 482   482                   azelf        482      3      3             261
    ## 483   483                  dialga        483     54   6830             306
    ## 484   484                  palkia        484     42   3360             306
    ## 485   485                 heatran        485     17   4300             270
    ## 486   486               regigigas        486     37   4200             302
    ## 487   487        giratina-altered        487     45   7500             306
    ## 488   488               cresselia        488     15    856             270
    ## 489   489                  phione        489      4     31             216
    ## 490   490                 manaphy        490      3     14             270
    ## 491   491                 darkrai        491     15    505             270
    ## 492   492            shaymin-land        492      2     21             270
    ## 493   493                  arceus        493     32   3200             324
    ## 494   494                 victini        494      4     40             270
    ## 495   495                   snivy        495      6     81              62
    ## 496   496                 servine        496      8    160             145
    ## 497   497               serperior        497     33    630             238
    ## 498   498                   tepig        498      5     99              62
    ## 499   499                 pignite        499     10    555             146
    ## 500   500                  emboar        500     16   1500             238
    ## 501   501                oshawott        501      5     59              62
    ## 502   502                  dewott        502      8    245             145
    ## 503   503                samurott        503     15    946             238
    ## 504   504                  patrat        504      5    116              51
    ## 505   505                 watchog        505     11    270             147
    ## 506   506                lillipup        506      4     41              55
    ## 507   507                 herdier        507      9    147             130
    ## 508   508               stoutland        508     12    610             225
    ## 509   509                purrloin        509      4    101              56
    ## 510   510                 liepard        510     11    375             156
    ## 511   511                 pansage        511      6    105              63
    ## 512   512                simisage        512     11    305             174
    ## 513   513                 pansear        513      6    110              63
    ## 514   514                simisear        514     10    280             174
    ## 515   515                 panpour        515      6    135              63
    ## 516   516                simipour        516     10    290             174
    ## 517   517                   munna        517      6    233              58
    ## 518   518                musharna        518     11    605             170
    ## 519   519                  pidove        519      3     21              53
    ## 520   520               tranquill        520      6    150             125
    ## 521   521                unfezant        521     12    290             220
    ## 522   522                 blitzle        522      8    298              59
    ## 523   523               zebstrika        523     16    795             174
    ## 524   524              roggenrola        524      4    180              56
    ## 525   525                 boldore        525      9   1020             137
    ## 526   526                gigalith        526     17   2600             232
    ## 527   527                  woobat        527      4     21              65
    ## 528   528                 swoobat        528      9    105             149
    ## 529   529                 drilbur        529      3     85              66
    ## 530   530               excadrill        530      7    404             178
    ## 531   531                  audino        531     11    310             390
    ## 532   532                 timburr        532      6    125              61
    ## 533   533                 gurdurr        533     12    400             142
    ## 534   534              conkeldurr        534     14    870             227
    ## 535   535                 tympole        535      5     45              59
    ## 536   536               palpitoad        536      8    170             134
    ## 537   537              seismitoad        537     15    620             229
    ## 538   538                   throh        538     13    555             163
    ## 539   539                    sawk        539     14    510             163
    ## 540   540                sewaddle        540      3     25              62
    ## 541   541                swadloon        541      5     73             133
    ## 542   542                leavanny        542     12    205             225
    ## 543   543                venipede        543      4     53              52
    ## 544   544              whirlipede        544     12    585             126
    ## 545   545               scolipede        545     25   2005             218
    ## 546   546                cottonee        546      3      6              56
    ## 547   547              whimsicott        547      7     66             168
    ## 548   548                 petilil        548      5     66              56
    ## 549   549               lilligant        549     11    163             168
    ## 550   550    basculin-red-striped        550     10    180             161
    ## 551   551                 sandile        551      7    152              58
    ## 552   552                krokorok        552     10    334             123
    ## 553   553              krookodile        553     15    963             234
    ## 554   554                darumaka        554      6    375              63
    ## 555   555     darmanitan-standard        555     13    929             168
    ## 556   556                maractus        556     10    280             161
    ## 557   557                 dwebble        557      3    145              65
    ## 558   558                 crustle        558     14   2000             170
    ## 559   559                 scraggy        559      6    118              70
    ## 560   560                 scrafty        560     11    300             171
    ## 561   561                sigilyph        561     14    140             172
    ## 562   562                  yamask        562      5     15              61
    ## 563   563              cofagrigus        563     17    765             169
    ## 564   564                tirtouga        564      7    165              71
    ## 565   565              carracosta        565     12    810             173
    ## 566   566                  archen        566      5     95              71
    ## 567   567                archeops        567     14    320             177
    ## 568   568                trubbish        568      6    310              66
    ## 569   569                garbodor        569     19   1073             166
    ## 570   570                   zorua        570      7    125              66
    ## 571   571                 zoroark        571     16    811             179
    ## 572   572                minccino        572      4     58              60
    ## 573   573                cinccino        573      5     75             165
    ## 574   574                 gothita        574      4     58              58
    ## 575   575               gothorita        575      7    180             137
    ## 576   576              gothitelle        576     15    440             221
    ## 577   577                 solosis        577      3     10              58
    ## 578   578                 duosion        578      6     80             130
    ## 579   579               reuniclus        579     10    201             221
    ## 580   580                ducklett        580      5     55              61
    ## 581   581                  swanna        581     13    242             166
    ## 582   582               vanillite        582      4     57              61
    ## 583   583               vanillish        583     11    410             138
    ## 584   584               vanilluxe        584     13    575             241
    ## 585   585                deerling        585      6    195              67
    ## 586   586                sawsbuck        586     19    925             166
    ## 587   587                  emolga        587      4     50             150
    ## 588   588              karrablast        588      5     59              63
    ## 589   589              escavalier        589     10    330             173
    ## 590   590                 foongus        590      2     10              59
    ## 591   591               amoonguss        591      6    105             162
    ## 592   592                frillish        592     12    330              67
    ## 593   593               jellicent        593     22   1350             168
    ## 594   594               alomomola        594     12    316             165
    ## 595   595                  joltik        595      1      6              64
    ## 596   596              galvantula        596      8    143             165
    ## 597   597               ferroseed        597      6    188              61
    ## 598   598              ferrothorn        598     10   1100             171
    ## 599   599                   klink        599      3    210              60
    ## 600   600                   klang        600      6    510             154
    ## 601   601               klinklang        601      6    810             234
    ## 602   602                  tynamo        602      2      3              55
    ## 603   603               eelektrik        603     12    220             142
    ## 604   604              eelektross        604     21    805             232
    ## 605   605                  elgyem        605      5     90              67
    ## 606   606                beheeyem        606     10    345             170
    ## 607   607                 litwick        607      3     31              55
    ## 608   608                 lampent        608      6    130             130
    ## 609   609              chandelure        609     10    343             234
    ## 610   610                    axew        610      6    180              64
    ## 611   611                 fraxure        611     10    360             144
    ## 612   612                 haxorus        612     18   1055             243
    ## 613   613                 cubchoo        613      5     85              61
    ## 614   614                 beartic        614     26   2600             177
    ## 615   615               cryogonal        615     11   1480             180
    ## 616   616                 shelmet        616      4     77              61
    ## 617   617                accelgor        617      8    253             173
    ## 618   618                stunfisk        618      7    110             165
    ## 619   619                 mienfoo        619      9    200              70
    ## 620   620                mienshao        620     14    355             179
    ## 621   621               druddigon        621     16   1390             170
    ## 622   622                  golett        622     10    920              61
    ## 623   623                  golurk        623     28   3300             169
    ## 624   624                pawniard        624      5    102              68
    ## 625   625                 bisharp        625     16    700             172
    ## 626   626              bouffalant        626     16    946             172
    ## 627   627                 rufflet        627      5    105              70
    ## 628   628                braviary        628     15    410             179
    ## 629   629                 vullaby        629      5     90              74
    ## 630   630               mandibuzz        630     12    395             179
    ## 631   631                 heatmor        631     14    580             169
    ## 632   632                  durant        632      3    330             169
    ## 633   633                   deino        633      8    173              60
    ## 634   634                zweilous        634     14    500             147
    ## 635   635               hydreigon        635     18   1600             270
    ## 636   636                larvesta        636     11    288              72
    ## 637   637               volcarona        637     16    460             248
    ## 638   638                cobalion        638     21   2500             261
    ## 639   639               terrakion        639     19   2600             261
    ## 640   640                virizion        640     20   2000             261
    ## 641   641      tornadus-incarnate        641     15    630             261
    ## 642   642     thundurus-incarnate        642     15    610             261
    ## 643   643                reshiram        643     32   3300             306
    ## 644   644                  zekrom        644     29   3450             306
    ## 645   645      landorus-incarnate        645     15    680             270
    ## 646   646                  kyurem        646     30   3250             297
    ## 647   647         keldeo-ordinary        647     14    485             261
    ## 648   648           meloetta-aria        648      6     65             270
    ## 649   649                genesect        649     15    825             270
    ## 650   650                 chespin        650      4     90              63
    ## 651   651               quilladin        651      7    290             142
    ## 652   652              chesnaught        652     16    900             239
    ## 653   653                fennekin        653      4     94              61
    ## 654   654                 braixen        654     10    145             143
    ## 655   655                 delphox        655     15    390             240
    ## 656   656                 froakie        656      3     70              63
    ## 657   657               frogadier        657      6    109             142
    ## 658   658                greninja        658     15    400             239
    ## 659   659                bunnelby        659      4     50              47
    ## 660   660               diggersby        660     10    424             148
    ## 661   661              fletchling        661      3     17              56
    ## 662   662             fletchinder        662      7    160             134
    ## 663   663              talonflame        663     12    245             175
    ## 664   664              scatterbug        664      3     25              40
    ## 665   665                  spewpa        665      3     84              75
    ## 666   666                vivillon        666     12    170             185
    ## 667   667                  litleo        667      6    135              74
    ## 668   668                  pyroar        668     15    815             177
    ## 669   669                 flabebe        669      1      1              61
    ## 670   670                 floette        670      2      9             130
    ## 671   671                 florges        671     11    100             248
    ## 672   672                  skiddo        672      9    310              70
    ## 673   673                  gogoat        673     17    910             186
    ## 674   674                 pancham        674      6     80              70
    ## 675   675                 pangoro        675     21   1360             173
    ## 676   676                 furfrou        676     12    280             165
    ## 677   677                  espurr        677      3     35              71
    ## 678   678           meowstic-male        678      6     85             163
    ## 679   679                 honedge        679      8     20              65
    ## 680   680                doublade        680      8     45             157
    ## 681   681        aegislash-shield        681     17    530             234
    ## 682   682                spritzee        682      2      5              68
    ## 683   683              aromatisse        683      8    155             162
    ## 684   684                 swirlix        684      4     35              68
    ## 685   685                slurpuff        685      8     50             168
    ## 686   686                   inkay        686      4     35              58
    ## 687   687                 malamar        687     15    470             169
    ## 688   688                 binacle        688      5    310              61
    ## 689   689              barbaracle        689     13    960             175
    ## 690   690                  skrelp        690      5     73              64
    ## 691   691                dragalge        691     18    815             173
    ## 692   692               clauncher        692      5     83              66
    ## 693   693               clawitzer        693     13    353             100
    ## 694   694              helioptile        694      5     60              58
    ## 695   695               heliolisk        695     10    210             168
    ## 696   696                  tyrunt        696      8    260              72
    ## 697   697               tyrantrum        697     25   2700             182
    ## 698   698                  amaura        698     13    252              72
    ## 699   699                 aurorus        699     27   2250             104
    ## 700   700                 sylveon        700     10    235             184
    ## 701   701                hawlucha        701      8    215             175
    ## 702   702                 dedenne        702      2     22             151
    ## 703   703                 carbink        703      3     57             100
    ## 704   704                   goomy        704      3     28              60
    ## 705   705                 sliggoo        705      8    175             158
    ## 706   706                  goodra        706     20   1505             270
    ## 707   707                  klefki        707      2     30             165
    ## 708   708                phantump        708      4     70              62
    ## 709   709               trevenant        709     15    710             166
    ## 710   710       pumpkaboo-average        710      4     50              67
    ## 711   711       gourgeist-average        711      9    125             173
    ## 712   712                bergmite        712     10    995              61
    ## 713   713                 avalugg        713     20   5050             180
    ## 714   714                  noibat        714      5     80              49
    ## 715   715                 noivern        715     15    850             187
    ## 716   716                 xerneas        716     30   2150             306
    ## 717   717                 yveltal        717     58   2030             306
    ## 718   718                 zygarde        718     50   3050             270
    ## 719   719                 diancie        719      7     88             270
    ## 720   720                   hoopa        720      5     90             270
    ## 721   721               volcanion        721     17   1950             270
    ## 722   722                  rowlet        722      3     15              64
    ## 723   723                 dartrix        723      7    160             147
    ## 724   724               decidueye        724     16    366             239
    ## 725   725                  litten        725      4     43              64
    ## 726   726                torracat        726      7    250             147
    ## 727   727              incineroar        727     18    830             239
    ## 728   728                 popplio        728      4     75              64
    ## 729   729                 brionne        729      6    175             147
    ## 730   730               primarina        730     18    440             239
    ## 731   731                 pikipek        731      3     12              53
    ## 732   732                trumbeak        732      6    148             124
    ## 733   733               toucannon        733     11    260             218
    ## 734   734                 yungoos        734      4     60              51
    ## 735   735                gumshoos        735      7    142             146
    ## 736   736                 grubbin        736      4     44              60
    ## 737   737               charjabug        737      5    105             140
    ## 738   738                vikavolt        738     15    450             225
    ## 739   739              crabrawler        739      6     70              68
    ## 740   740            crabominable        740     17   1800             167
    ## 741   741          oricorio-baile        741      6     34             167
    ## 742   742                cutiefly        742      1      2              61
    ## 743   743                ribombee        743      2      5             162
    ## 744   744                rockruff        744      5     92              56
    ## 745   745         lycanroc-midday        745      8    250             170
    ## 746   746         wishiwashi-solo        746      2      3              61
    ## 747   747                mareanie        747      4     80              61
    ## 748   748                 toxapex        748      7    145             173
    ## 749   749                 mudbray        749     10   1100              77
    ## 750   750                mudsdale        750     25   9200             175
    ## 751   751                dewpider        751      3     40              54
    ## 752   752               araquanid        752     18    820             159
    ## 753   753                fomantis        753      3     15              50
    ## 754   754                lurantis        754      9    185             168
    ## 755   755                morelull        755      2     15              57
    ## 756   756               shiinotic        756     10    115             142
    ## 757   757                salandit        757      6     48              64
    ## 758   758                salazzle        758     12    222             168
    ## 759   759                 stufful        759      5     68              68
    ## 760   760                  bewear        760     21   1350             175
    ## 761   761               bounsweet        761      3     32              42
    ## 762   762                 steenee        762      7     82             102
    ## 763   763                tsareena        763     12    214             230
    ## 764   764                  comfey        764      1      3             170
    ## 765   765                oranguru        765     15    760             172
    ## 766   766               passimian        766     20    828             172
    ## 767   767                  wimpod        767      5    120              46
    ## 768   768               golisopod        768     20   1080             186
    ## 769   769               sandygast        769      5    700              64
    ## 770   770               palossand        770     13   2500             168
    ## 771   771               pyukumuku        771      3     12             144
    ## 772   772               type-null        772     19   1205             107
    ## 773   773                silvally        773     23   1005             257
    ## 774   774       minior-red-meteor        774      3    400             154
    ## 775   775                  komala        775      4    199             168
    ## 776   776              turtonator        776     20   2120             170
    ## 777   777              togedemaru        777      3     33             152
    ## 778   778       mimikyu-disguised        778      2      7             167
    ## 779   779                 bruxish        779      9    190             166
    ## 780   780                  drampa        780     30   1850             170
    ## 781   781                dhelmise        781     39   2100             181
    ## 782   782                jangmo-o        782      6    297              60
    ## 783   783                hakamo-o        783     12    470             147
    ## 784   784                 kommo-o        784     16    782             270
    ## 785   785               tapu-koko        785     18    205             257
    ## 786   786               tapu-lele        786     12    186             257
    ## 787   787               tapu-bulu        787     19    455             257
    ## 788   788               tapu-fini        788     13    212             257
    ## 789   789                  cosmog        789      2      1              40
    ## 790   790                 cosmoem        790      1   9999             140
    ## 791   791                solgaleo        791     34   2300             306
    ## 792   792                  lunala        792     40   1200             306
    ## 793   793                nihilego        793     12    555             257
    ## 794   794                buzzwole        794     24   3336             257
    ## 795   795               pheromosa        795     18    250             257
    ## 796   796               xurkitree        796     38   1000             257
    ## 797   797              celesteela        797     92   9999             257
    ## 798   798                 kartana        798      3      1             257
    ## 799   799                guzzlord        799     55   8880             257
    ## 800   800                necrozma        800     24   2300             270
    ## 801   801                magearna        801     10    805             270
    ## 802   802               marshadow        802      7    222             270
    ## 803   803                 poipole        803      6     18             189
    ## 804   804               naganadel        804     36   1500             243
    ## 805   805               stakataka        805     55   8200             257
    ## 806   806             blacephalon        806     18    130             257
    ## 807   807                 zeraora        807     15    445             270
    ## 808 10001           deoxys-attack        386     17    608             270
    ## 809 10002          deoxys-defense        386     17    608             270
    ## 810 10003            deoxys-speed        386     17    608             270
    ## 811 10004          wormadam-sandy        413      5     65             148
    ## 812 10005          wormadam-trash        413      5     65             148
    ## 813 10006             shaymin-sky        492      4     52             270
    ## 814 10007         giratina-origin        487     69   6500             306
    ## 815 10008              rotom-heat        479      3      3             182
    ## 816 10009              rotom-wash        479      3      3             182
    ## 817 10010             rotom-frost        479      3      3             182
    ## 818 10011               rotom-fan        479      3      3             182
    ## 819 10012               rotom-mow        479      3      3             182
    ## 820 10013          castform-sunny        351      3      8             147
    ## 821 10014          castform-rainy        351      3      8             147
    ## 822 10015          castform-snowy        351      3      8             147
    ## 823 10016   basculin-blue-striped        550     10    180             161
    ## 824 10017          darmanitan-zen        555     13    929             189
    ## 825 10018      meloetta-pirouette        648      6     65             270
    ## 826 10019        tornadus-therian        641     14    630             261
    ## 827 10020       thundurus-therian        642     30    610             261
    ## 828 10021        landorus-therian        645     13    680             270
    ## 829 10022            kyurem-black        646     33   3250             315
    ## 830 10023            kyurem-white        646     36   3250             315
    ## 831 10024         keldeo-resolute        647     14    485             261
    ## 832 10025         meowstic-female        678      6     85             163
    ## 833 10026         aegislash-blade        681     17    530             234
    ## 834 10027         pumpkaboo-small        710      3     35              67
    ## 835 10028         pumpkaboo-large        710      5     75              67
    ## 836 10029         pumpkaboo-super        710      8    150              67
    ## 837 10030         gourgeist-small        711      7     95             173
    ## 838 10031         gourgeist-large        711     11    140             173
    ## 839 10032         gourgeist-super        711     17    390             173
    ## 840 10033           venusaur-mega          3     24   1555             281
    ## 841 10034        charizard-mega-x          6     17   1105             285
    ## 842 10035        charizard-mega-y          6     17   1005             285
    ## 843 10036          blastoise-mega          9     16   1011             284
    ## 844 10037           alakazam-mega         65     12    480             270
    ## 845 10038             gengar-mega         94     14    405             270
    ## 846 10039         kangaskhan-mega        115     22   1000             207
    ## 847 10040             pinsir-mega        127     17    590             210
    ## 848 10041           gyarados-mega        130     65   3050             224
    ## 849 10042         aerodactyl-mega        142     21    790             215
    ## 850 10043           mewtwo-mega-x        150     23   1270             351
    ## 851 10044           mewtwo-mega-y        150     15    330             351
    ## 852 10045           ampharos-mega        181     14    615             275
    ## 853 10046             scizor-mega        212     20   1250             210
    ## 854 10047          heracross-mega        214     17    625             210
    ## 855 10048           houndoom-mega        229     19    495             210
    ## 856 10049          tyranitar-mega        248     25   2550             315
    ## 857 10050           blaziken-mega        257     19    520             284
    ## 858 10051          gardevoir-mega        282     16    484             278
    ## 859 10052             mawile-mega        303     10    235             168
    ## 860 10053             aggron-mega        306     22   3950             284
    ## 861 10054           medicham-mega        308     13    315             179
    ## 862 10055          manectric-mega        310     18    440             201
    ## 863 10056            banette-mega        354     12    130             194
    ## 864 10057              absol-mega        359     12    490             198
    ## 865 10058           garchomp-mega        445     19    950             315
    ## 866 10059            lucario-mega        448     13    575             219
    ## 867 10060          abomasnow-mega        460     27   1850             208
    ## 868 10061         floette-eternal        670      2      9             243
    ## 869 10062             latias-mega        380     18    520             315
    ## 870 10063             latios-mega        381     23    700             315
    ## 871 10064           swampert-mega        260     19   1020             286
    ## 872 10065           sceptile-mega        254     19    552             284
    ## 873 10066            sableye-mega        302      5   1610             168
    ## 874 10067            altaria-mega        334     15    206             207
    ## 875 10068            gallade-mega        475     16    564             278
    ## 876 10069             audino-mega        531     15    320             425
    ## 877 10070           sharpedo-mega        319     25   1303             196
    ## 878 10071            slowbro-mega         80     20   1200             207
    ## 879 10072            steelix-mega        208    105   7400             214
    ## 880 10073            pidgeot-mega         18     22    505             261
    ## 881 10074             glalie-mega        362     21   3502             203
    ## 882 10075            diancie-mega        719     11    278             315
    ## 883 10076          metagross-mega        376     25   9429             315
    ## 884 10077           kyogre-primal        382     98   4300             347
    ## 885 10078          groudon-primal        383     50   9997             347
    ## 886 10079           rayquaza-mega        384    108   3920             351
    ## 887 10080       pikachu-rock-star         25      4     60             112
    ## 888 10081           pikachu-belle         25      4     60             112
    ## 889 10082        pikachu-pop-star         25      4     60             112
    ## 890 10083             pikachu-phd         25      4     60             112
    ## 891 10084           pikachu-libre         25      4     60             112
    ## 892 10085         pikachu-cosplay         25      4     60             112
    ## 893 10086           hoopa-unbound        720     65   4900             306
    ## 894 10087           camerupt-mega        323     25   3205             196
    ## 895 10088            lopunny-mega        428     13    283             203
    ## 896 10089          salamence-mega        373     18   1126             315
    ## 897 10090           beedrill-mega         15     14    405             223
    ## 898 10091           rattata-alola         19      3     38              51
    ## 899 10092          raticate-alola         20      7    255             145
    ## 900 10093    raticate-totem-alola         20     14   1050             145
    ## 901 10094    pikachu-original-cap         25      4     60             112
    ## 902 10095       pikachu-hoenn-cap         25      4     60             112
    ## 903 10096      pikachu-sinnoh-cap         25      4     60             112
    ## 904 10097       pikachu-unova-cap         25      4     60             112
    ## 905 10098       pikachu-kalos-cap         25      4     60             112
    ## 906 10099       pikachu-alola-cap         25      4     60             112
    ## 907 10100            raichu-alola         26      7    210             218
    ## 908 10101         sandshrew-alola         27      7    400              60
    ## 909 10102         sandslash-alola         28     12    550             158
    ## 910 10103            vulpix-alola         37      6     99              60
    ## 911 10104         ninetales-alola         38     11    199             177
    ## 912 10105           diglett-alola         50      2     10              53
    ## 913 10106           dugtrio-alola         51      7    666             149
    ## 914 10107            meowth-alola         52      4     42              58
    ## 915 10108           persian-alola         53     11    330             154
    ## 916 10109           geodude-alola         74      4    203              60
    ## 917 10110          graveler-alola         75     10   1100             137
    ## 918 10111             golem-alola         76     17   3160             223
    ## 919 10112            grimer-alola         88      7    420              65
    ## 920 10113               muk-alola         89     10    520             175
    ## 921 10114         exeggutor-alola        103    109   4156             186
    ## 922 10115           marowak-alola        105     10    340             149
    ## 923 10116    greninja-battle-bond        658     15    400             239
    ## 924 10117            greninja-ash        658     15    400             288
    ## 925 10118              zygarde-10        718     12    335             219
    ## 926 10119              zygarde-50        718     50   3050             270
    ## 927 10120        zygarde-complete        718     45   6100             319
    ## 928 10121          gumshoos-totem        735     14    600             146
    ## 929 10122          vikavolt-totem        738     26   1475             225
    ## 930 10123        oricorio-pom-pom        741      6     34             167
    ## 931 10124            oricorio-pau        741      6     34             167
    ## 932 10125          oricorio-sensu        741      6     34             167
    ## 933 10126       lycanroc-midnight        745     11    250             170
    ## 934 10127       wishiwashi-school        746     82    786             217
    ## 935 10128          lurantis-totem        754     15    580             168
    ## 936 10129          salazzle-totem        758     21    810             168
    ## 937 10130    minior-orange-meteor        774      3    400             154
    ## 938 10131    minior-yellow-meteor        774      3    400             154
    ## 939 10132     minior-green-meteor        774      3    400             154
    ## 940 10133      minior-blue-meteor        774      3    400             154
    ## 941 10134    minior-indigo-meteor        774      3    400             154
    ## 942 10135    minior-violet-meteor        774      3    400             154
    ## 943 10136              minior-red        774      3      3             175
    ## 944 10137           minior-orange        774      3      3             175
    ## 945 10138           minior-yellow        774      3      3             175
    ## 946 10139            minior-green        774      3      3             175
    ## 947 10140             minior-blue        774      3      3             175
    ## 948 10141           minior-indigo        774      3      3             175
    ## 949 10142           minior-violet        774      3      3             175
    ## 950 10143          mimikyu-busted        778      2      7             167
    ## 951 10144 mimikyu-totem-disguised        778      4     28             167
    ## 952 10145    mimikyu-totem-busted        778      4     28             167
    ## 953 10146           kommo-o-totem        784     24   2075             270
    ## 954 10147       magearna-original        801     10    805             270
    ## 955 10148     pikachu-partner-cap         25      4     60             112
    ## 956 10149           marowak-totem        105     17    980             149
    ## 957 10150          ribombee-totem        743      4     20             162
    ## 958 10151      rockruff-own-tempo        744      5     92              56
    ## 959 10152           lycanroc-dusk        745      8    250             170
    ## 960 10153         araquanid-totem        752     31   2175             159
    ## 961 10154        togedemaru-totem        777      6    130             152
    ## 962 10155           necrozma-dusk        800     38   4600             306
    ## 963 10156           necrozma-dawn        800     42   3500             306
    ## 964 10157          necrozma-ultra        800     75   2300             339
    ##     order is_default
    ## 1       1          1
    ## 2       2          1
    ## 3       3          1
    ## 4       5          1
    ## 5       6          1
    ## 6       7          1
    ## 7      10          1
    ## 8      11          1
    ## 9      12          1
    ## 10     14          1
    ## 11     15          1
    ## 12     16          1
    ## 13     17          1
    ## 14     18          1
    ## 15     19          1
    ## 16     21          1
    ## 17     22          1
    ## 18     23          1
    ## 19     25          1
    ## 20     27          1
    ## 21     30          1
    ## 22     31          1
    ## 23     32          1
    ## 24     33          1
    ## 25     35          1
    ## 26     43          1
    ## 27     45          1
    ## 28     47          1
    ## 29     49          1
    ## 30     50          1
    ## 31     51          1
    ## 32     52          1
    ## 33     53          1
    ## 34     54          1
    ## 35     56          1
    ## 36     57          1
    ## 37     58          1
    ## 38     60          1
    ## 39     63          1
    ## 40     64          1
    ## 41     65          1
    ## 42     66          1
    ## 43     68          1
    ## 44     69          1
    ## 45     70          1
    ## 46     72          1
    ## 47     73          1
    ## 48     74          1
    ## 49     75          1
    ## 50     76          1
    ## 51     78          1
    ## 52     80          1
    ## 53     82          1
    ## 54     84          1
    ## 55     85          1
    ## 56     86          1
    ## 57     87          1
    ## 58     88          1
    ## 59     89          1
    ## 60     90          1
    ## 61     91          1
    ## 62     92          1
    ## 63     94          1
    ## 64     95          1
    ## 65     96          1
    ## 66     98          1
    ## 67     99          1
    ## 68    100          1
    ## 69    101          1
    ## 70    102          1
    ## 71    103          1
    ## 72    104          1
    ## 73    105          1
    ## 74    106          1
    ## 75    108          1
    ## 76    110          1
    ## 77    112          1
    ## 78    113          1
    ## 79    114          1
    ## 80    115          1
    ## 81    118          1
    ## 82    119          1
    ## 83    121          1
    ## 84    122          1
    ## 85    123          1
    ## 86    124          1
    ## 87    125          1
    ## 88    126          1
    ## 89    128          1
    ## 90    130          1
    ## 91    131          1
    ## 92    132          1
    ## 93    133          1
    ## 94    134          1
    ## 95    136          1
    ## 96    139          1
    ## 97    140          1
    ## 98    141          1
    ## 99    142          1
    ## 100   143          1
    ## 101   144          1
    ## 102   145          1
    ## 103   146          1
    ## 104   148          1
    ## 105   149          1
    ## 106   153          1
    ## 107   154          1
    ## 108   156          1
    ## 109   158          1
    ## 110   159          1
    ## 111   160          1
    ## 112   161          1
    ## 113   164          1
    ## 114   166          1
    ## 115   168          1
    ## 116   170          1
    ## 117   171          1
    ## 118   173          1
    ## 119   174          1
    ## 120   175          1
    ## 121   176          1
    ## 122   178          1
    ## 123   179          1
    ## 124   183          1
    ## 125   185          1
    ## 126   188          1
    ## 127   190          1
    ## 128   192          1
    ## 129   193          1
    ## 130   194          1
    ## 131   196          1
    ## 132   197          1
    ## 133   198          1
    ## 134   199          1
    ## 135   200          1
    ## 136   201          1
    ## 137   207          1
    ## 138   210          1
    ## 139   211          1
    ## 140   212          1
    ## 141   213          1
    ## 142   214          1
    ## 143   217          1
    ## 144   218          1
    ## 145   219          1
    ## 146   220          1
    ## 147   221          1
    ## 148   222          1
    ## 149   223          1
    ## 150   224          1
    ## 151   227          1
    ## 152   228          1
    ## 153   229          1
    ## 154   230          1
    ## 155   231          1
    ## 156   232          1
    ## 157   233          1
    ## 158   234          1
    ## 159   235          1
    ## 160   236          1
    ## 161   237          1
    ## 162   238          1
    ## 163   239          1
    ## 164   240          1
    ## 165   241          1
    ## 166   242          1
    ## 167   243          1
    ## 168   244          1
    ## 169    67          1
    ## 170   245          1
    ## 171   246          1
    ## 172    34          1
    ## 173    55          1
    ## 174    62          1
    ## 175   247          1
    ## 176   248          1
    ## 177   250          1
    ## 178   251          1
    ## 179   252          1
    ## 180   253          1
    ## 181   254          1
    ## 182    71          1
    ## 183   257          1
    ## 184   258          1
    ## 185   260          1
    ## 186    93          1
    ## 187   261          1
    ## 188   262          1
    ## 189   263          1
    ## 190   264          1
    ## 191   266          1
    ## 192   267          1
    ## 193   268          1
    ## 194   270          1
    ## 195   271          1
    ## 196   202          1
    ## 197   203          1
    ## 198   272          1
    ## 199   117          1
    ## 200   274          1
    ## 201   276          1
    ## 202   278          1
    ## 203   279          1
    ## 204   280          1
    ## 205   281          1
    ## 206   282          1
    ## 207   283          1
    ## 208   137          1
    ## 209   285          1
    ## 210   286          1
    ## 211   287          1
    ## 212   180          1
    ## 213   288          1
    ## 214   289          1
    ## 215   291          1
    ## 216   293          1
    ## 217   294          1
    ## 218   295          1
    ## 219   296          1
    ## 220   297          1
    ## 221   298          1
    ## 222   300          1
    ## 223   301          1
    ## 224   302          1
    ## 225   303          1
    ## 226   305          1
    ## 227   306          1
    ## 228   307          1
    ## 229   308          1
    ## 230   172          1
    ## 231   310          1
    ## 232   311          1
    ## 233   208          1
    ## 234   312          1
    ## 235   313          1
    ## 236   152          1
    ## 237   155          1
    ## 238   182          1
    ## 239   184          1
    ## 240   187          1
    ## 241   314          1
    ## 242   165          1
    ## 243   315          1
    ## 244   316          1
    ## 245   317          1
    ## 246   318          1
    ## 247   319          1
    ## 248   320          1
    ## 249   322          1
    ## 250   323          1
    ## 251   324          1
    ## 252   325          1
    ## 253   326          1
    ## 254   327          1
    ## 255   329          1
    ## 256   330          1
    ## 257   331          1
    ## 258   333          1
    ## 259   334          1
    ## 260   335          1
    ## 261   337          1
    ## 262   338          1
    ## 263   339          1
    ## 264   340          1
    ## 265   341          1
    ## 266   342          1
    ## 267   343          1
    ## 268   344          1
    ## 269   345          1
    ## 270   346          1
    ## 271   347          1
    ## 272   348          1
    ## 273   349          1
    ## 274   350          1
    ## 275   351          1
    ## 276   352          1
    ## 277   353          1
    ## 278   354          1
    ## 279   355          1
    ## 280   356          1
    ## 281   357          1
    ## 282   358          1
    ## 283   362          1
    ## 284   363          1
    ## 285   364          1
    ## 286   365          1
    ## 287   366          1
    ## 288   367          1
    ## 289   368          1
    ## 290   369          1
    ## 291   370          1
    ## 292   371          1
    ## 293   372          1
    ## 294   373          1
    ## 295   374          1
    ## 296   375          1
    ## 297   376          1
    ## 298   256          1
    ## 299   377          1
    ## 300   379          1
    ## 301   380          1
    ## 302   381          1
    ## 303   383          1
    ## 304   385          1
    ## 305   386          1
    ## 306   387          1
    ## 307   389          1
    ## 308   390          1
    ## 309   392          1
    ## 310   393          1
    ## 311   395          1
    ## 312   396          1
    ## 313   397          1
    ## 314   398          1
    ## 315   400          1
    ## 316   402          1
    ## 317   403          1
    ## 318   404          1
    ## 319   405          1
    ## 320   407          1
    ## 321   408          1
    ## 322   409          1
    ## 323   410          1
    ## 324   412          1
    ## 325   413          1
    ## 326   414          1
    ## 327   415          1
    ## 328   416          1
    ## 329   417          1
    ## 330   418          1
    ## 331   419          1
    ## 332   420          1
    ## 333   421          1
    ## 334   422          1
    ## 335   424          1
    ## 336   425          1
    ## 337   426          1
    ## 338   427          1
    ## 339   428          1
    ## 340   429          1
    ## 341   430          1
    ## 342   431          1
    ## 343   432          1
    ## 344   433          1
    ## 345   434          1
    ## 346   435          1
    ## 347   436          1
    ## 348   437          1
    ## 349   438          1
    ## 350   439          1
    ## 351   440          1
    ## 352   444          1
    ## 353   445          1
    ## 354   446          1
    ## 355   448          1
    ## 356   449          1
    ## 357   451          1
    ## 358   453          1
    ## 359   454          1
    ## 360   277          1
    ## 361   456          1
    ## 362   457          1
    ## 363   460          1
    ## 364   461          1
    ## 365   462          1
    ## 366   463          1
    ## 367   464          1
    ## 368   465          1
    ## 369   466          1
    ## 370   467          1
    ## 371   468          1
    ## 372   469          1
    ## 373   470          1
    ## 374   472          1
    ## 375   473          1
    ## 376   474          1
    ## 377   476          1
    ## 378   477          1
    ## 379   478          1
    ## 380   479          1
    ## 381   481          1
    ## 382   483          1
    ## 383   485          1
    ## 384   487          1
    ## 385   489          1
    ## 386   490          1
    ## 387   494          1
    ## 388   495          1
    ## 389   496          1
    ## 390   497          1
    ## 391   498          1
    ## 392   499          1
    ## 393   500          1
    ## 394   501          1
    ## 395   502          1
    ## 396   503          1
    ## 397   504          1
    ## 398   505          1
    ## 399   506          1
    ## 400   507          1
    ## 401   508          1
    ## 402   509          1
    ## 403   510          1
    ## 404   511          1
    ## 405   512          1
    ## 406   399          1
    ## 407   401          1
    ## 408   513          1
    ## 409   514          1
    ## 410   515          1
    ## 411   516          1
    ## 412   517          1
    ## 413   518          1
    ## 414   521          1
    ## 415   522          1
    ## 416   523          1
    ## 417   524          1
    ## 418   525          1
    ## 419   526          1
    ## 420   527          1
    ## 421   528          1
    ## 422   529          1
    ## 423   530          1
    ## 424   265          1
    ## 425   531          1
    ## 426   532          1
    ## 427   533          1
    ## 428   534          1
    ## 429   275          1
    ## 430   273          1
    ## 431   536          1
    ## 432   537          1
    ## 433   452          1
    ## 434   538          1
    ## 435   539          1
    ## 436   540          1
    ## 437   541          1
    ## 438   259          1
    ## 439   177          1
    ## 440   163          1
    ## 441   542          1
    ## 442   543          1
    ## 443   544          1
    ## 444   545          1
    ## 445   546          1
    ## 446   216          1
    ## 447   548          1
    ## 448   549          1
    ## 449   551          1
    ## 450   552          1
    ## 451   553          1
    ## 452   554          1
    ## 453   555          1
    ## 454   556          1
    ## 455   557          1
    ## 456   558          1
    ## 457   559          1
    ## 458   304          1
    ## 459   560          1
    ## 460   561          1
    ## 461   292          1
    ## 462   120          1
    ## 463   157          1
    ## 464   162          1
    ## 465   167          1
    ## 466   186          1
    ## 467   189          1
    ## 468   249          1
    ## 469   269          1
    ## 470   204          1
    ## 471   205          1
    ## 472   284          1
    ## 473   299          1
    ## 474   209          1
    ## 475   360          1
    ## 476   378          1
    ## 477   450          1
    ## 478   459          1
    ## 479   563          1
    ## 480   569          1
    ## 481   570          1
    ## 482   571          1
    ## 483   572          1
    ## 484   573          1
    ## 485   574          1
    ## 486   575          1
    ## 487   576          1
    ## 488   578          1
    ## 489   579          1
    ## 490   580          1
    ## 491   581          1
    ## 492   582          1
    ## 493   584          1
    ## 494   585          1
    ## 495   586          1
    ## 496   587          1
    ## 497   588          1
    ## 498   589          1
    ## 499   590          1
    ## 500   591          1
    ## 501   592          1
    ## 502   593          1
    ## 503   594          1
    ## 504   595          1
    ## 505   596          1
    ## 506   597          1
    ## 507   598          1
    ## 508   599          1
    ## 509   600          1
    ## 510   601          1
    ## 511   602          1
    ## 512   603          1
    ## 513   604          1
    ## 514   605          1
    ## 515   606          1
    ## 516   607          1
    ## 517   608          1
    ## 518   609          1
    ## 519   610          1
    ## 520   611          1
    ## 521   612          1
    ## 522   613          1
    ## 523   614          1
    ## 524   615          1
    ## 525   616          1
    ## 526   617          1
    ## 527   618          1
    ## 528   619          1
    ## 529   620          1
    ## 530   621          1
    ## 531   622          1
    ## 532   624          1
    ## 533   625          1
    ## 534   626          1
    ## 535   627          1
    ## 536   628          1
    ## 537   629          1
    ## 538   630          1
    ## 539   631          1
    ## 540   632          1
    ## 541   633          1
    ## 542   634          1
    ## 543   635          1
    ## 544   636          1
    ## 545   637          1
    ## 546   638          1
    ## 547   639          1
    ## 548   640          1
    ## 549   641          1
    ## 550   642          1
    ## 551   644          1
    ## 552   645          1
    ## 553   646          1
    ## 554   647          1
    ## 555   648          1
    ## 556   650          1
    ## 557   651          1
    ## 558   652          1
    ## 559   653          1
    ## 560   654          1
    ## 561   655          1
    ## 562   656          1
    ## 563   657          1
    ## 564   658          1
    ## 565   659          1
    ## 566   660          1
    ## 567   661          1
    ## 568   662          1
    ## 569   663          1
    ## 570   664          1
    ## 571   665          1
    ## 572   666          1
    ## 573   667          1
    ## 574   668          1
    ## 575   669          1
    ## 576   670          1
    ## 577   671          1
    ## 578   672          1
    ## 579   673          1
    ## 580   674          1
    ## 581   675          1
    ## 582   676          1
    ## 583   677          1
    ## 584   678          1
    ## 585   679          1
    ## 586   680          1
    ## 587   681          1
    ## 588   682          1
    ## 589   683          1
    ## 590   684          1
    ## 591   685          1
    ## 592   686          1
    ## 593   687          1
    ## 594   688          1
    ## 595   689          1
    ## 596   690          1
    ## 597   691          1
    ## 598   692          1
    ## 599   693          1
    ## 600   694          1
    ## 601   695          1
    ## 602   696          1
    ## 603   697          1
    ## 604   698          1
    ## 605   699          1
    ## 606   700          1
    ## 607   701          1
    ## 608   702          1
    ## 609   703          1
    ## 610   704          1
    ## 611   705          1
    ## 612   706          1
    ## 613   707          1
    ## 614   708          1
    ## 615   709          1
    ## 616   710          1
    ## 617   711          1
    ## 618   712          1
    ## 619   713          1
    ## 620   714          1
    ## 621   715          1
    ## 622   716          1
    ## 623   717          1
    ## 624   718          1
    ## 625   719          1
    ## 626   720          1
    ## 627   721          1
    ## 628   722          1
    ## 629   723          1
    ## 630   724          1
    ## 631   725          1
    ## 632   726          1
    ## 633   727          1
    ## 634   728          1
    ## 635   729          1
    ## 636   730          1
    ## 637   731          1
    ## 638   732          1
    ## 639   733          1
    ## 640   734          1
    ## 641   735          1
    ## 642   737          1
    ## 643   739          1
    ## 644   740          1
    ## 645   741          1
    ## 646   743          1
    ## 647   746          1
    ## 648   748          1
    ## 649   750          1
    ## 650   751          1
    ## 651   752          1
    ## 652   753          1
    ## 653   754          1
    ## 654   755          1
    ## 655   756          1
    ## 656   757          1
    ## 657   758          1
    ## 658   759          1
    ## 659   762          1
    ## 660   763          1
    ## 661   764          1
    ## 662   765          1
    ## 663   766          1
    ## 664   767          1
    ## 665   768          1
    ## 666   769          1
    ## 667   770          1
    ## 668   771          1
    ## 669   772          1
    ## 670   773          1
    ## 671   775          1
    ## 672   776          1
    ## 673   777          1
    ## 674   778          1
    ## 675   779          1
    ## 676   780          1
    ## 677   781          1
    ## 678   782          1
    ## 679   784          1
    ## 680   785          1
    ## 681   786          1
    ## 682   788          1
    ## 683   789          1
    ## 684   790          1
    ## 685   791          1
    ## 686   792          1
    ## 687   793          1
    ## 688   794          1
    ## 689   795          1
    ## 690   796          1
    ## 691   797          1
    ## 692   798          1
    ## 693   799          1
    ## 694   800          1
    ## 695   801          1
    ## 696   802          1
    ## 697   803          1
    ## 698   804          1
    ## 699   805          1
    ## 700   206          1
    ## 701   806          1
    ## 702   807          1
    ## 703   808          1
    ## 704   809          1
    ## 705   810          1
    ## 706   811          1
    ## 707   812          1
    ## 708   813          1
    ## 709   814          1
    ## 710   815          1
    ## 711   819          1
    ## 712   823          1
    ## 713   824          1
    ## 714   825          1
    ## 715   826          1
    ## 716   827          1
    ## 717   828          1
    ## 718   829          1
    ## 719   833          1
    ## 720   835          1
    ## 721   837          1
    ## 722   838          1
    ## 723   839          1
    ## 724   840          1
    ## 725   841          1
    ## 726   842          1
    ## 727   843          1
    ## 728   844          1
    ## 729   845          1
    ## 730   846          1
    ## 731   847          1
    ## 732   848          1
    ## 733   849          1
    ## 734   850          1
    ## 735   851          1
    ## 736   853          1
    ## 737   854          1
    ## 738   855          1
    ## 739   857          1
    ## 740   858          1
    ## 741   859          1
    ## 742   863          1
    ## 743   864          1
    ## 744   866          1
    ## 745   868          1
    ## 746   871          1
    ## 747   873          1
    ## 748   874          1
    ## 749   875          1
    ## 750   876          1
    ## 751   877          1
    ## 752   878          1
    ## 753   880          1
    ## 754   881          1
    ## 755   883          1
    ## 756   884          1
    ## 757   885          1
    ## 758   886          1
    ## 759   888          1
    ## 760   889          1
    ## 761   890          1
    ## 762   891          1
    ## 763   892          1
    ## 764   893          1
    ## 765   894          1
    ## 766   895          1
    ## 767   896          1
    ## 768   897          1
    ## 769   898          1
    ## 770   899          1
    ## 771   900          1
    ## 772   901          1
    ## 773   902          1
    ## 774   903          1
    ## 775   917          1
    ## 776   918          1
    ## 777   919          1
    ## 778   921          1
    ## 779   925          1
    ## 780   926          1
    ## 781   927          1
    ## 782   928          1
    ## 783   929          1
    ## 784   930          1
    ## 785   932          1
    ## 786   933          1
    ## 787   934          1
    ## 788   935          1
    ## 789   936          1
    ## 790   937          1
    ## 791   938          1
    ## 792   939          1
    ## 793   940          1
    ## 794   941          1
    ## 795   942          1
    ## 796   943          1
    ## 797   944          1
    ## 798   945          1
    ## 799   946          1
    ## 800   947          1
    ## 801   951          1
    ## 802   953          1
    ## 803   954          1
    ## 804   955          1
    ## 805   956          1
    ## 806   957          1
    ## 807   958          1
    ## 808   491          0
    ## 809   492          0
    ## 810   493          0
    ## 811   519          0
    ## 812   520          0
    ## 813   583          0
    ## 814   577          0
    ## 815   564          0
    ## 816   565          0
    ## 817   566          0
    ## 818   567          0
    ## 819   568          0
    ## 820   441          0
    ## 821   442          0
    ## 822   443          0
    ## 823   643          0
    ## 824   649          0
    ## 825   749          0
    ## 826   736          0
    ## 827   738          0
    ## 828   742          0
    ## 829   745          0
    ## 830   744          0
    ## 831   747          0
    ## 832   783          0
    ## 833   787          0
    ## 834   816          0
    ## 835   817          0
    ## 836   818          0
    ## 837   820          0
    ## 838   821          0
    ## 839   822          0
    ## 840     4          0
    ## 841     8          0
    ## 842     9          0
    ## 843    13          0
    ## 844    97          0
    ## 845   135          0
    ## 846   169          0
    ## 847   191          0
    ## 848   195          0
    ## 849   215          0
    ## 850   225          0
    ## 851   226          0
    ## 852   255          0
    ## 853   181          0
    ## 854   290          0
    ## 855   309          0
    ## 856   321          0
    ## 857   332          0
    ## 858   359          0
    ## 859   384          0
    ## 860   388          0
    ## 861   391          0
    ## 862   394          0
    ## 863   447          0
    ## 864   455          0
    ## 865   547          0
    ## 866   550          0
    ## 867   562          0
    ## 868   774          0
    ## 869   480          0
    ## 870   482          0
    ## 871   336          0
    ## 872   328          0
    ## 873   382          0
    ## 874   423          0
    ## 875   361          0
    ## 876   623          0
    ## 877   406          0
    ## 878   116          0
    ## 879   138          0
    ## 880    24          0
    ## 881   458          0
    ## 882   834          0
    ## 883   475          0
    ## 884   484          0
    ## 885   486          0
    ## 886   488          0
    ## 887    37          0
    ## 888    38          0
    ## 889    39          0
    ## 890    40          0
    ## 891    41          0
    ## 892    36          0
    ## 893   836          0
    ## 894   411          0
    ## 895   535          0
    ## 896   471          0
    ## 897    20          0
    ## 898    26          0
    ## 899    28          0
    ## 900    29          0
    ## 901    36          0
    ## 902    37          0
    ## 903    38          0
    ## 904    39          0
    ## 905    40          0
    ## 906    41          0
    ## 907    44          0
    ## 908    46          0
    ## 909    48          0
    ## 910    59          0
    ## 911    61          0
    ## 912    77          0
    ## 913    79          0
    ## 914    81          0
    ## 915    83          0
    ## 916   107          0
    ## 917   109          0
    ## 918   111          0
    ## 919   127          0
    ## 920   129          0
    ## 921   147          0
    ## 922   150          0
    ## 923   760          0
    ## 924   761          0
    ## 925   830          0
    ## 926   831          0
    ## 927   832          0
    ## 928   852          0
    ## 929   856          0
    ## 930   860          0
    ## 931   861          0
    ## 932   862          0
    ## 933   869          0
    ## 934   872          0
    ## 935   882          0
    ## 936   887          0
    ## 937   904          0
    ## 938   905          0
    ## 939   906          0
    ## 940   907          0
    ## 941   908          0
    ## 942   909          0
    ## 943   910          0
    ## 944   911          0
    ## 945   912          0
    ## 946   913          0
    ## 947   914          0
    ## 948   915          0
    ## 949   916          0
    ## 950   922          0
    ## 951   923          0
    ## 952   924          0
    ## 953   931          0
    ## 954   952          0
    ## 955    42          0
    ## 956   151          0
    ## 957   865          0
    ## 958   867          0
    ## 959   870          0
    ## 960   879          0
    ## 961   920          0
    ## 962   948          0
    ## 963   949          0
    ## 964   950          0
    ## 
    ## [[2]]
    ##      pokemon_id type_id slot
    ## 1             1      12    1
    ## 2             1       4    2
    ## 3             2      12    1
    ## 4             2       4    2
    ## 5             3      12    1
    ## 6             3       4    2
    ## 7             4      10    1
    ## 8             5      10    1
    ## 9             6      10    1
    ## 10            6       3    2
    ## 11            7      11    1
    ## 12            8      11    1
    ## 13            9      11    1
    ## 14           10       7    1
    ## 15           11       7    1
    ## 16           12       7    1
    ## 17           12       3    2
    ## 18           13       7    1
    ## 19           13       4    2
    ## 20           14       7    1
    ## 21           14       4    2
    ## 22           15       7    1
    ## 23           15       4    2
    ## 24           16       1    1
    ## 25           16       3    2
    ## 26           17       1    1
    ## 27           17       3    2
    ## 28           18       1    1
    ## 29           18       3    2
    ## 30           19       1    1
    ## 31           20       1    1
    ## 32           21       1    1
    ## 33           21       3    2
    ## 34           22       1    1
    ## 35           22       3    2
    ## 36           23       4    1
    ## 37           24       4    1
    ## 38           25      13    1
    ## 39           26      13    1
    ## 40           27       5    1
    ## 41           28       5    1
    ## 42           29       4    1
    ## 43           30       4    1
    ## 44           31       4    1
    ## 45           31       5    2
    ## 46           32       4    1
    ## 47           33       4    1
    ## 48           34       4    1
    ## 49           34       5    2
    ## 50           35      18    1
    ## 51           36      18    1
    ## 52           37      10    1
    ## 53           38      10    1
    ## 54           39       1    1
    ## 55           39      18    2
    ## 56           40       1    1
    ## 57           40      18    2
    ## 58           41       4    1
    ## 59           41       3    2
    ## 60           42       4    1
    ## 61           42       3    2
    ## 62           43      12    1
    ## 63           43       4    2
    ## 64           44      12    1
    ## 65           44       4    2
    ## 66           45      12    1
    ## 67           45       4    2
    ## 68           46       7    1
    ## 69           46      12    2
    ## 70           47       7    1
    ## 71           47      12    2
    ## 72           48       7    1
    ## 73           48       4    2
    ## 74           49       7    1
    ## 75           49       4    2
    ## 76           50       5    1
    ## 77           51       5    1
    ## 78           52       1    1
    ## 79           53       1    1
    ## 80           54      11    1
    ## 81           55      11    1
    ## 82           56       2    1
    ## 83           57       2    1
    ## 84           58      10    1
    ## 85           59      10    1
    ## 86           60      11    1
    ## 87           61      11    1
    ## 88           62      11    1
    ## 89           62       2    2
    ## 90           63      14    1
    ## 91           64      14    1
    ## 92           65      14    1
    ## 93           66       2    1
    ## 94           67       2    1
    ## 95           68       2    1
    ## 96           69      12    1
    ## 97           69       4    2
    ## 98           70      12    1
    ## 99           70       4    2
    ## 100          71      12    1
    ## 101          71       4    2
    ## 102          72      11    1
    ## 103          72       4    2
    ## 104          73      11    1
    ## 105          73       4    2
    ## 106          74       6    1
    ## 107          74       5    2
    ## 108          75       6    1
    ## 109          75       5    2
    ## 110          76       6    1
    ## 111          76       5    2
    ## 112          77      10    1
    ## 113          78      10    1
    ## 114          79      11    1
    ## 115          79      14    2
    ## 116          80      11    1
    ## 117          80      14    2
    ## 118          81      13    1
    ## 119          81       9    2
    ## 120          82      13    1
    ## 121          82       9    2
    ## 122          83       1    1
    ## 123          83       3    2
    ## 124          84       1    1
    ## 125          84       3    2
    ## 126          85       1    1
    ## 127          85       3    2
    ## 128          86      11    1
    ## 129          87      11    1
    ## 130          87      15    2
    ## 131          88       4    1
    ## 132          89       4    1
    ## 133          90      11    1
    ## 134          91      11    1
    ## 135          91      15    2
    ## 136          92       8    1
    ## 137          92       4    2
    ## 138          93       8    1
    ## 139          93       4    2
    ## 140          94       8    1
    ## 141          94       4    2
    ## 142          95       6    1
    ## 143          95       5    2
    ## 144          96      14    1
    ## 145          97      14    1
    ## 146          98      11    1
    ## 147          99      11    1
    ## 148         100      13    1
    ## 149         101      13    1
    ## 150         102      12    1
    ## 151         102      14    2
    ## 152         103      12    1
    ## 153         103      14    2
    ## 154         104       5    1
    ## 155         105       5    1
    ## 156         106       2    1
    ## 157         107       2    1
    ## 158         108       1    1
    ## 159         109       4    1
    ## 160         110       4    1
    ## 161         111       5    1
    ## 162         111       6    2
    ## 163         112       5    1
    ## 164         112       6    2
    ## 165         113       1    1
    ## 166         114      12    1
    ## 167         115       1    1
    ## 168         116      11    1
    ## 169         117      11    1
    ## 170         118      11    1
    ## 171         119      11    1
    ## 172         120      11    1
    ## 173         121      11    1
    ## 174         121      14    2
    ## 175         122      14    1
    ## 176         122      18    2
    ## 177         123       7    1
    ## 178         123       3    2
    ## 179         124      15    1
    ## 180         124      14    2
    ## 181         125      13    1
    ## 182         126      10    1
    ## 183         127       7    1
    ## 184         128       1    1
    ## 185         129      11    1
    ## 186         130      11    1
    ## 187         130       3    2
    ## 188         131      11    1
    ## 189         131      15    2
    ## 190         132       1    1
    ## 191         133       1    1
    ## 192         134      11    1
    ## 193         135      13    1
    ## 194         136      10    1
    ## 195         137       1    1
    ## 196         138       6    1
    ## 197         138      11    2
    ## 198         139       6    1
    ## 199         139      11    2
    ## 200         140       6    1
    ## 201         140      11    2
    ## 202         141       6    1
    ## 203         141      11    2
    ## 204         142       6    1
    ## 205         142       3    2
    ## 206         143       1    1
    ## 207         144      15    1
    ## 208         144       3    2
    ## 209         145      13    1
    ## 210         145       3    2
    ## 211         146      10    1
    ## 212         146       3    2
    ## 213         147      16    1
    ## 214         148      16    1
    ## 215         149      16    1
    ## 216         149       3    2
    ## 217         150      14    1
    ## 218         151      14    1
    ## 219         152      12    1
    ## 220         153      12    1
    ## 221         154      12    1
    ## 222         155      10    1
    ## 223         156      10    1
    ## 224         157      10    1
    ## 225         158      11    1
    ## 226         159      11    1
    ## 227         160      11    1
    ## 228         161       1    1
    ## 229         162       1    1
    ## 230         163       1    1
    ## 231         163       3    2
    ## 232         164       1    1
    ## 233         164       3    2
    ## 234         165       7    1
    ## 235         165       3    2
    ## 236         166       7    1
    ## 237         166       3    2
    ## 238         167       7    1
    ## 239         167       4    2
    ## 240         168       7    1
    ## 241         168       4    2
    ## 242         169       4    1
    ## 243         169       3    2
    ## 244         170      11    1
    ## 245         170      13    2
    ## 246         171      11    1
    ## 247         171      13    2
    ## 248         172      13    1
    ## 249         173      18    1
    ## 250         174       1    1
    ## 251         174      18    2
    ## 252         175      18    1
    ## 253         176      18    1
    ## 254         176       3    2
    ## 255         177      14    1
    ## 256         177       3    2
    ## 257         178      14    1
    ## 258         178       3    2
    ## 259         179      13    1
    ## 260         180      13    1
    ## 261         181      13    1
    ## 262         182      12    1
    ## 263         183      11    1
    ## 264         183      18    2
    ## 265         184      11    1
    ## 266         184      18    2
    ## 267         185       6    1
    ## 268         186      11    1
    ## 269         187      12    1
    ## 270         187       3    2
    ## 271         188      12    1
    ## 272         188       3    2
    ## 273         189      12    1
    ## 274         189       3    2
    ## 275         190       1    1
    ## 276         191      12    1
    ## 277         192      12    1
    ## 278         193       7    1
    ## 279         193       3    2
    ## 280         194      11    1
    ## 281         194       5    2
    ## 282         195      11    1
    ## 283         195       5    2
    ## 284         196      14    1
    ## 285         197      17    1
    ## 286         198      17    1
    ## 287         198       3    2
    ## 288         199      11    1
    ## 289         199      14    2
    ## 290         200       8    1
    ## 291         201      14    1
    ## 292         202      14    1
    ## 293         203       1    1
    ## 294         203      14    2
    ## 295         204       7    1
    ## 296         205       7    1
    ## 297         205       9    2
    ## 298         206       1    1
    ## 299         207       5    1
    ## 300         207       3    2
    ## 301         208       9    1
    ## 302         208       5    2
    ## 303         209      18    1
    ## 304         210      18    1
    ## 305         211      11    1
    ## 306         211       4    2
    ## 307         212       7    1
    ## 308         212       9    2
    ## 309         213       7    1
    ## 310         213       6    2
    ## 311         214       7    1
    ## 312         214       2    2
    ## 313         215      17    1
    ## 314         215      15    2
    ## 315         216       1    1
    ## 316         217       1    1
    ## 317         218      10    1
    ## 318         219      10    1
    ## 319         219       6    2
    ## 320         220      15    1
    ## 321         220       5    2
    ## 322         221      15    1
    ## 323         221       5    2
    ## 324         222      11    1
    ## 325         222       6    2
    ## 326         223      11    1
    ## 327         224      11    1
    ## 328         225      15    1
    ## 329         225       3    2
    ## 330         226      11    1
    ## 331         226       3    2
    ## 332         227       9    1
    ## 333         227       3    2
    ## 334         228      17    1
    ## 335         228      10    2
    ## 336         229      17    1
    ## 337         229      10    2
    ## 338         230      11    1
    ## 339         230      16    2
    ## 340         231       5    1
    ## 341         232       5    1
    ## 342         233       1    1
    ## 343         234       1    1
    ## 344         235       1    1
    ## 345         236       2    1
    ## 346         237       2    1
    ## 347         238      15    1
    ## 348         238      14    2
    ## 349         239      13    1
    ## 350         240      10    1
    ## 351         241       1    1
    ## 352         242       1    1
    ## 353         243      13    1
    ## 354         244      10    1
    ## 355         245      11    1
    ## 356         246       6    1
    ## 357         246       5    2
    ## 358         247       6    1
    ## 359         247       5    2
    ## 360         248       6    1
    ## 361         248      17    2
    ## 362         249      14    1
    ## 363         249       3    2
    ## 364         250      10    1
    ## 365         250       3    2
    ## 366         251      14    1
    ## 367         251      12    2
    ## 368         252      12    1
    ## 369         253      12    1
    ## 370         254      12    1
    ## 371         255      10    1
    ## 372         256      10    1
    ## 373         256       2    2
    ## 374         257      10    1
    ## 375         257       2    2
    ## 376         258      11    1
    ## 377         259      11    1
    ## 378         259       5    2
    ## 379         260      11    1
    ## 380         260       5    2
    ## 381         261      17    1
    ## 382         262      17    1
    ## 383         263       1    1
    ## 384         264       1    1
    ## 385         265       7    1
    ## 386         266       7    1
    ## 387         267       7    1
    ## 388         267       3    2
    ## 389         268       7    1
    ## 390         269       7    1
    ## 391         269       4    2
    ## 392         270      11    1
    ## 393         270      12    2
    ## 394         271      11    1
    ## 395         271      12    2
    ## 396         272      11    1
    ## 397         272      12    2
    ## 398         273      12    1
    ## 399         274      12    1
    ## 400         274      17    2
    ## 401         275      12    1
    ## 402         275      17    2
    ## 403         276       1    1
    ## 404         276       3    2
    ## 405         277       1    1
    ## 406         277       3    2
    ## 407         278      11    1
    ## 408         278       3    2
    ## 409         279      11    1
    ## 410         279       3    2
    ## 411         280      14    1
    ## 412         280      18    2
    ## 413         281      14    1
    ## 414         281      18    2
    ## 415         282      14    1
    ## 416         282      18    2
    ## 417         283       7    1
    ## 418         283      11    2
    ## 419         284       7    1
    ## 420         284       3    2
    ## 421         285      12    1
    ## 422         286      12    1
    ## 423         286       2    2
    ## 424         287       1    1
    ## 425         288       1    1
    ## 426         289       1    1
    ## 427         290       7    1
    ## 428         290       5    2
    ## 429         291       7    1
    ## 430         291       3    2
    ## 431         292       7    1
    ## 432         292       8    2
    ## 433         293       1    1
    ## 434         294       1    1
    ## 435         295       1    1
    ## 436         296       2    1
    ## 437         297       2    1
    ## 438         298       1    1
    ## 439         298      18    2
    ## 440         299       6    1
    ## 441         300       1    1
    ## 442         301       1    1
    ## 443         302      17    1
    ## 444         302       8    2
    ## 445         303       9    1
    ## 446         303      18    2
    ## 447         304       9    1
    ## 448         304       6    2
    ## 449         305       9    1
    ## 450         305       6    2
    ## 451         306       9    1
    ## 452         306       6    2
    ## 453         307       2    1
    ## 454         307      14    2
    ## 455         308       2    1
    ## 456         308      14    2
    ## 457         309      13    1
    ## 458         310      13    1
    ## 459         311      13    1
    ## 460         312      13    1
    ## 461         313       7    1
    ## 462         314       7    1
    ## 463         315      12    1
    ## 464         315       4    2
    ## 465         316       4    1
    ## 466         317       4    1
    ## 467         318      11    1
    ## 468         318      17    2
    ## 469         319      11    1
    ## 470         319      17    2
    ## 471         320      11    1
    ## 472         321      11    1
    ## 473         322      10    1
    ## 474         322       5    2
    ## 475         323      10    1
    ## 476         323       5    2
    ## 477         324      10    1
    ## 478         325      14    1
    ## 479         326      14    1
    ## 480         327       1    1
    ## 481         328       5    1
    ## 482         329       5    1
    ## 483         329      16    2
    ## 484         330       5    1
    ## 485         330      16    2
    ## 486         331      12    1
    ## 487         332      12    1
    ## 488         332      17    2
    ## 489         333       1    1
    ## 490         333       3    2
    ## 491         334      16    1
    ## 492         334       3    2
    ## 493         335       1    1
    ## 494         336       4    1
    ## 495         337       6    1
    ## 496         337      14    2
    ## 497         338       6    1
    ## 498         338      14    2
    ## 499         339      11    1
    ## 500         339       5    2
    ## 501         340      11    1
    ## 502         340       5    2
    ## 503         341      11    1
    ## 504         342      11    1
    ## 505         342      17    2
    ## 506         343       5    1
    ## 507         343      14    2
    ## 508         344       5    1
    ## 509         344      14    2
    ## 510         345       6    1
    ## 511         345      12    2
    ## 512         346       6    1
    ## 513         346      12    2
    ## 514         347       6    1
    ## 515         347       7    2
    ## 516         348       6    1
    ## 517         348       7    2
    ## 518         349      11    1
    ## 519         350      11    1
    ## 520         351       1    1
    ## 521         352       1    1
    ## 522         353       8    1
    ## 523         354       8    1
    ## 524         355       8    1
    ## 525         356       8    1
    ## 526         357      12    1
    ## 527         357       3    2
    ## 528         358      14    1
    ## 529         359      17    1
    ## 530         360      14    1
    ## 531         361      15    1
    ## 532         362      15    1
    ## 533         363      15    1
    ## 534         363      11    2
    ## 535         364      15    1
    ## 536         364      11    2
    ## 537         365      15    1
    ## 538         365      11    2
    ## 539         366      11    1
    ## 540         367      11    1
    ## 541         368      11    1
    ## 542         369      11    1
    ## 543         369       6    2
    ## 544         370      11    1
    ## 545         371      16    1
    ## 546         372      16    1
    ## 547         373      16    1
    ## 548         373       3    2
    ## 549         374       9    1
    ## 550         374      14    2
    ## 551         375       9    1
    ## 552         375      14    2
    ## 553         376       9    1
    ## 554         376      14    2
    ## 555         377       6    1
    ## 556         378      15    1
    ## 557         379       9    1
    ## 558         380      16    1
    ## 559         380      14    2
    ## 560         381      16    1
    ## 561         381      14    2
    ## 562         382      11    1
    ## 563         383       5    1
    ## 564         384      16    1
    ## 565         384       3    2
    ## 566         385       9    1
    ## 567         385      14    2
    ## 568         386      14    1
    ## 569         387      12    1
    ## 570         388      12    1
    ## 571         389      12    1
    ## 572         389       5    2
    ## 573         390      10    1
    ## 574         391      10    1
    ## 575         391       2    2
    ## 576         392      10    1
    ## 577         392       2    2
    ## 578         393      11    1
    ## 579         394      11    1
    ## 580         395      11    1
    ## 581         395       9    2
    ## 582         396       1    1
    ## 583         396       3    2
    ## 584         397       1    1
    ## 585         397       3    2
    ## 586         398       1    1
    ## 587         398       3    2
    ## 588         399       1    1
    ## 589         400       1    1
    ## 590         400      11    2
    ## 591         401       7    1
    ## 592         402       7    1
    ## 593         403      13    1
    ## 594         404      13    1
    ## 595         405      13    1
    ## 596         406      12    1
    ## 597         406       4    2
    ## 598         407      12    1
    ## 599         407       4    2
    ## 600         408       6    1
    ## 601         409       6    1
    ## 602         410       6    1
    ## 603         410       9    2
    ## 604         411       6    1
    ## 605         411       9    2
    ## 606         412       7    1
    ## 607         413       7    1
    ## 608         413      12    2
    ## 609         414       7    1
    ## 610         414       3    2
    ## 611         415       7    1
    ## 612         415       3    2
    ## 613         416       7    1
    ## 614         416       3    2
    ## 615         417      13    1
    ## 616         418      11    1
    ## 617         419      11    1
    ## 618         420      12    1
    ## 619         421      12    1
    ## 620         422      11    1
    ## 621         423      11    1
    ## 622         423       5    2
    ## 623         424       1    1
    ## 624         425       8    1
    ## 625         425       3    2
    ## 626         426       8    1
    ## 627         426       3    2
    ## 628         427       1    1
    ## 629         428       1    1
    ## 630         429       8    1
    ## 631         430      17    1
    ## 632         430       3    2
    ## 633         431       1    1
    ## 634         432       1    1
    ## 635         433      14    1
    ## 636         434       4    1
    ## 637         434      17    2
    ## 638         435       4    1
    ## 639         435      17    2
    ## 640         436       9    1
    ## 641         436      14    2
    ## 642         437       9    1
    ## 643         437      14    2
    ## 644         438       6    1
    ## 645         439      14    1
    ## 646         439      18    2
    ## 647         440       1    1
    ## 648         441       1    1
    ## 649         441       3    2
    ## 650         442       8    1
    ## 651         442      17    2
    ## 652         443      16    1
    ## 653         443       5    2
    ## 654         444      16    1
    ## 655         444       5    2
    ## 656         445      16    1
    ## 657         445       5    2
    ## 658         446       1    1
    ## 659         447       2    1
    ## 660         448       2    1
    ## 661         448       9    2
    ## 662         449       5    1
    ## 663         450       5    1
    ## 664         451       4    1
    ## 665         451       7    2
    ## 666         452       4    1
    ## 667         452      17    2
    ## 668         453       4    1
    ## 669         453       2    2
    ## 670         454       4    1
    ## 671         454       2    2
    ## 672         455      12    1
    ## 673         456      11    1
    ## 674         457      11    1
    ## 675         458      11    1
    ## 676         458       3    2
    ## 677         459      12    1
    ## 678         459      15    2
    ## 679         460      12    1
    ## 680         460      15    2
    ## 681         461      17    1
    ## 682         461      15    2
    ## 683         462      13    1
    ## 684         462       9    2
    ## 685         463       1    1
    ## 686         464       5    1
    ## 687         464       6    2
    ## 688         465      12    1
    ## 689         466      13    1
    ## 690         467      10    1
    ## 691         468      18    1
    ## 692         468       3    2
    ## 693         469       7    1
    ## 694         469       3    2
    ## 695         470      12    1
    ## 696         471      15    1
    ## 697         472       5    1
    ## 698         472       3    2
    ## 699         473      15    1
    ## 700         473       5    2
    ## 701         474       1    1
    ## 702         475      14    1
    ## 703         475       2    2
    ## 704         476       6    1
    ## 705         476       9    2
    ## 706         477       8    1
    ## 707         478      15    1
    ## 708         478       8    2
    ## 709         479      13    1
    ## 710         479       8    2
    ## 711         480      14    1
    ## 712         481      14    1
    ## 713         482      14    1
    ## 714         483       9    1
    ## 715         483      16    2
    ## 716         484      11    1
    ## 717         484      16    2
    ## 718         485      10    1
    ## 719         485       9    2
    ## 720         486       1    1
    ## 721         487       8    1
    ## 722         487      16    2
    ## 723         488      14    1
    ## 724         489      11    1
    ## 725         490      11    1
    ## 726         491      17    1
    ## 727         492      12    1
    ## 728         493       1    1
    ## 729         494      14    1
    ## 730         494      10    2
    ## 731         495      12    1
    ## 732         496      12    1
    ## 733         497      12    1
    ## 734         498      10    1
    ## 735         499      10    1
    ## 736         499       2    2
    ## 737         500      10    1
    ## 738         500       2    2
    ## 739         501      11    1
    ## 740         502      11    1
    ## 741         503      11    1
    ## 742         504       1    1
    ## 743         505       1    1
    ## 744         506       1    1
    ## 745         507       1    1
    ## 746         508       1    1
    ## 747         509      17    1
    ## 748         510      17    1
    ## 749         511      12    1
    ## 750         512      12    1
    ## 751         513      10    1
    ## 752         514      10    1
    ## 753         515      11    1
    ## 754         516      11    1
    ## 755         517      14    1
    ## 756         518      14    1
    ## 757         519       1    1
    ## 758         519       3    2
    ## 759         520       1    1
    ## 760         520       3    2
    ## 761         521       1    1
    ## 762         521       3    2
    ## 763         522      13    1
    ## 764         523      13    1
    ## 765         524       6    1
    ## 766         525       6    1
    ## 767         526       6    1
    ## 768         527      14    1
    ## 769         527       3    2
    ## 770         528      14    1
    ## 771         528       3    2
    ## 772         529       5    1
    ## 773         530       5    1
    ## 774         530       9    2
    ## 775         531       1    1
    ## 776         532       2    1
    ## 777         533       2    1
    ## 778         534       2    1
    ## 779         535      11    1
    ## 780         536      11    1
    ## 781         536       5    2
    ## 782         537      11    1
    ## 783         537       5    2
    ## 784         538       2    1
    ## 785         539       2    1
    ## 786         540       7    1
    ## 787         540      12    2
    ## 788         541       7    1
    ## 789         541      12    2
    ## 790         542       7    1
    ## 791         542      12    2
    ## 792         543       7    1
    ## 793         543       4    2
    ## 794         544       7    1
    ## 795         544       4    2
    ## 796         545       7    1
    ## 797         545       4    2
    ## 798         546      12    1
    ## 799         546      18    2
    ## 800         547      12    1
    ## 801         547      18    2
    ## 802         548      12    1
    ## 803         549      12    1
    ## 804         550      11    1
    ## 805         551       5    1
    ## 806         551      17    2
    ## 807         552       5    1
    ## 808         552      17    2
    ## 809         553       5    1
    ## 810         553      17    2
    ## 811         554      10    1
    ## 812         555      10    1
    ## 813         556      12    1
    ## 814         557       7    1
    ## 815         557       6    2
    ## 816         558       7    1
    ## 817         558       6    2
    ## 818         559      17    1
    ## 819         559       2    2
    ## 820         560      17    1
    ## 821         560       2    2
    ## 822         561      14    1
    ## 823         561       3    2
    ## 824         562       8    1
    ## 825         563       8    1
    ## 826         564      11    1
    ## 827         564       6    2
    ## 828         565      11    1
    ## 829         565       6    2
    ## 830         566       6    1
    ## 831         566       3    2
    ## 832         567       6    1
    ## 833         567       3    2
    ## 834         568       4    1
    ## 835         569       4    1
    ## 836         570      17    1
    ## 837         571      17    1
    ## 838         572       1    1
    ## 839         573       1    1
    ## 840         574      14    1
    ## 841         575      14    1
    ## 842         576      14    1
    ## 843         577      14    1
    ## 844         578      14    1
    ## 845         579      14    1
    ## 846         580      11    1
    ## 847         580       3    2
    ## 848         581      11    1
    ## 849         581       3    2
    ## 850         582      15    1
    ## 851         583      15    1
    ## 852         584      15    1
    ## 853         585       1    1
    ## 854         585      12    2
    ## 855         586       1    1
    ## 856         586      12    2
    ## 857         587      13    1
    ## 858         587       3    2
    ## 859         588       7    1
    ## 860         589       7    1
    ## 861         589       9    2
    ## 862         590      12    1
    ## 863         590       4    2
    ## 864         591      12    1
    ## 865         591       4    2
    ## 866         592      11    1
    ## 867         592       8    2
    ## 868         593      11    1
    ## 869         593       8    2
    ## 870         594      11    1
    ## 871         595       7    1
    ## 872         595      13    2
    ## 873         596       7    1
    ## 874         596      13    2
    ## 875         597      12    1
    ## 876         597       9    2
    ## 877         598      12    1
    ## 878         598       9    2
    ## 879         599       9    1
    ## 880         600       9    1
    ## 881         601       9    1
    ## 882         602      13    1
    ## 883         603      13    1
    ## 884         604      13    1
    ## 885         605      14    1
    ## 886         606      14    1
    ## 887         607       8    1
    ## 888         607      10    2
    ## 889         608       8    1
    ## 890         608      10    2
    ## 891         609       8    1
    ## 892         609      10    2
    ## 893         610      16    1
    ## 894         611      16    1
    ## 895         612      16    1
    ## 896         613      15    1
    ## 897         614      15    1
    ## 898         615      15    1
    ## 899         616       7    1
    ## 900         617       7    1
    ## 901         618       5    1
    ## 902         618      13    2
    ## 903         619       2    1
    ## 904         620       2    1
    ## 905         621      16    1
    ## 906         622       5    1
    ## 907         622       8    2
    ## 908         623       5    1
    ## 909         623       8    2
    ## 910         624      17    1
    ## 911         624       9    2
    ## 912         625      17    1
    ## 913         625       9    2
    ## 914         626       1    1
    ## 915         627       1    1
    ## 916         627       3    2
    ## 917         628       1    1
    ## 918         628       3    2
    ## 919         629      17    1
    ## 920         629       3    2
    ## 921         630      17    1
    ## 922         630       3    2
    ## 923         631      10    1
    ## 924         632       7    1
    ## 925         632       9    2
    ## 926         633      17    1
    ## 927         633      16    2
    ## 928         634      17    1
    ## 929         634      16    2
    ## 930         635      17    1
    ## 931         635      16    2
    ## 932         636       7    1
    ## 933         636      10    2
    ## 934         637       7    1
    ## 935         637      10    2
    ## 936         638       9    1
    ## 937         638       2    2
    ## 938         639       6    1
    ## 939         639       2    2
    ## 940         640      12    1
    ## 941         640       2    2
    ## 942         641       3    1
    ## 943         642      13    1
    ## 944         642       3    2
    ## 945         643      16    1
    ## 946         643      10    2
    ## 947         644      16    1
    ## 948         644      13    2
    ## 949         645       5    1
    ## 950         645       3    2
    ## 951         646      16    1
    ## 952         646      15    2
    ## 953         647      11    1
    ## 954         647       2    2
    ## 955         648       1    1
    ## 956         648      14    2
    ## 957         649       7    1
    ## 958         649       9    2
    ## 959         650      12    1
    ## 960         651      12    1
    ## 961         652      12    1
    ## 962         652       2    2
    ## 963         653      10    1
    ## 964         654      10    1
    ## 965         655      10    1
    ## 966         655      14    2
    ## 967         656      11    1
    ## 968         657      11    1
    ## 969         658      11    1
    ## 970         658      17    2
    ## 971         659       1    1
    ## 972         660       1    1
    ## 973         660       5    2
    ## 974         661       1    1
    ## 975         661       3    2
    ## 976         662      10    1
    ## 977         662       3    2
    ## 978         663      10    1
    ## 979         663       3    2
    ## 980         664       7    1
    ## 981         665       7    1
    ## 982         666       7    1
    ## 983         666       3    2
    ## 984         667      10    1
    ## 985         667       1    2
    ## 986         668      10    1
    ## 987         668       1    2
    ## 988         669      18    1
    ## 989         670      18    1
    ## 990         671      18    1
    ## 991         672      12    1
    ## 992         673      12    1
    ## 993         674       2    1
    ## 994         675       2    1
    ## 995         675      17    2
    ## 996         676       1    1
    ## 997         677      14    1
    ## 998         678      14    1
    ## 999         679       9    1
    ## 1000        679       8    2
    ## 1001        680       9    1
    ## 1002        680       8    2
    ## 1003        681       9    1
    ## 1004        681       8    2
    ## 1005        682      18    1
    ## 1006        683      18    1
    ## 1007        684      18    1
    ## 1008        685      18    1
    ## 1009        686      17    1
    ## 1010        686      14    2
    ## 1011        687      17    1
    ## 1012        687      14    2
    ## 1013        688       6    1
    ## 1014        688      11    2
    ## 1015        689       6    1
    ## 1016        689      11    2
    ## 1017        690       4    1
    ## 1018        690      11    2
    ## 1019        691       4    1
    ## 1020        691      16    2
    ## 1021        692      11    1
    ## 1022        693      11    1
    ## 1023        694      13    1
    ## 1024        694       1    2
    ## 1025        695      13    1
    ## 1026        695       1    2
    ## 1027        696       6    1
    ## 1028        696      16    2
    ## 1029        697       6    1
    ## 1030        697      16    2
    ## 1031        698       6    1
    ## 1032        698      15    2
    ## 1033        699       6    1
    ## 1034        699      15    2
    ## 1035        700      18    1
    ## 1036        701       2    1
    ## 1037        701       3    2
    ## 1038        702      13    1
    ## 1039        702      18    2
    ## 1040        703       6    1
    ## 1041        703      18    2
    ## 1042        704      16    1
    ## 1043        705      16    1
    ## 1044        706      16    1
    ## 1045        707       9    1
    ## 1046        707      18    2
    ## 1047        708       8    1
    ## 1048        708      12    2
    ## 1049        709       8    1
    ## 1050        709      12    2
    ## 1051        710       8    1
    ## 1052        710      12    2
    ## 1053        711       8    1
    ## 1054        711      12    2
    ## 1055        712      15    1
    ## 1056        713      15    1
    ## 1057        714       3    1
    ## 1058        714      16    2
    ## 1059        715       3    1
    ## 1060        715      16    2
    ## 1061        716      18    1
    ## 1062        717      17    1
    ## 1063        717       3    2
    ## 1064        718      16    1
    ## 1065        718       5    2
    ## 1066        719       6    1
    ## 1067        719      18    2
    ## 1068        720      14    1
    ## 1069        720       8    2
    ## 1070        721      10    1
    ## 1071        721      11    2
    ## 1072        722      12    1
    ## 1073        722       3    2
    ## 1074        723      12    1
    ## 1075        723       3    2
    ## 1076        724      12    1
    ## 1077        724       8    2
    ## 1078        725      10    1
    ## 1079        726      10    1
    ## 1080        727      10    1
    ## 1081        727      17    2
    ## 1082        728      11    1
    ## 1083        729      11    1
    ## 1084        730      11    1
    ## 1085        730      18    2
    ## 1086        731       1    1
    ## 1087        731       3    2
    ## 1088        732       1    1
    ## 1089        732       3    2
    ## 1090        733       1    1
    ## 1091        733       3    2
    ## 1092        734       1    1
    ## 1093        735       1    1
    ## 1094        736       7    1
    ## 1095        737       7    1
    ## 1096        737      13    2
    ## 1097        738       7    1
    ## 1098        738      13    2
    ## 1099        739       2    1
    ## 1100        740       2    1
    ## 1101        740      15    2
    ## 1102        741      10    1
    ## 1103        741       3    2
    ## 1104        742       7    1
    ## 1105        742      18    2
    ## 1106        743       7    1
    ## 1107        743      18    2
    ## 1108        744       6    1
    ## 1109        745       6    1
    ## 1110        746      11    1
    ## 1111        747       4    1
    ## 1112        747      11    2
    ## 1113        748       4    1
    ## 1114        748      11    2
    ## 1115        749       5    1
    ## 1116        750       5    1
    ## 1117        751      11    1
    ## 1118        751       7    2
    ## 1119        752      11    1
    ## 1120        752       7    2
    ## 1121        753      12    1
    ## 1122        754      12    1
    ## 1123        755      12    1
    ## 1124        755      18    2
    ## 1125        756      12    1
    ## 1126        756      18    2
    ## 1127        757       4    1
    ## 1128        757      10    2
    ## 1129        758       4    1
    ## 1130        758      10    2
    ## 1131        759       1    1
    ## 1132        759       2    2
    ## 1133        760       1    1
    ## 1134        760       2    2
    ## 1135        761      12    1
    ## 1136        762      12    1
    ## 1137        763      12    1
    ## 1138        764      18    1
    ## 1139        765       1    1
    ## 1140        765      14    2
    ## 1141        766       2    1
    ## 1142        767       7    1
    ## 1143        767      11    2
    ## 1144        768       7    1
    ## 1145        768      11    2
    ## 1146        769       8    1
    ## 1147        769       5    2
    ## 1148        770       8    1
    ## 1149        770       5    2
    ## 1150        771      11    1
    ## 1151        772       1    1
    ## 1152        773       1    1
    ## 1153        774       6    1
    ## 1154        774       3    2
    ## 1155        775       1    1
    ## 1156        776      10    1
    ## 1157        776      16    2
    ## 1158        777      13    1
    ## 1159        777       9    2
    ## 1160        778       8    1
    ## 1161        778      18    2
    ## 1162        779      11    1
    ## 1163        779      14    2
    ## 1164        780       1    1
    ## 1165        780      16    2
    ## 1166        781       8    1
    ## 1167        781      12    2
    ## 1168        782      16    1
    ## 1169        783      16    1
    ## 1170        783       2    2
    ## 1171        784      16    1
    ## 1172        784       2    2
    ## 1173        785      13    1
    ## 1174        785      18    2
    ## 1175        786      14    1
    ## 1176        786      18    2
    ## 1177        787      12    1
    ## 1178        787      18    2
    ## 1179        788      11    1
    ## 1180        788      18    2
    ## 1181        789      14    1
    ## 1182        790      14    1
    ## 1183        791      14    1
    ## 1184        791       9    2
    ## 1185        792      14    1
    ## 1186        792       8    2
    ## 1187        793       6    1
    ## 1188        793       4    2
    ## 1189        794       7    1
    ## 1190        794       2    2
    ## 1191        795       7    1
    ## 1192        795       2    2
    ## 1193        796      13    1
    ## 1194        797       9    1
    ## 1195        797       3    2
    ## 1196        798      12    1
    ## 1197        798       9    2
    ## 1198        799      17    1
    ## 1199        799      16    2
    ## 1200        800      14    1
    ## 1201        801       9    1
    ## 1202        801      18    2
    ## 1203        802       2    1
    ## 1204        802       8    2
    ## 1205        803       4    1
    ## 1206        804       4    1
    ## 1207        804      16    2
    ## 1208        805       6    1
    ## 1209        805       9    2
    ## 1210        806      10    1
    ## 1211        806       8    2
    ## 1212        807      13    1
    ## 1213      10001      14    1
    ## 1214      10002      14    1
    ## 1215      10003      14    1
    ## 1216      10004       7    1
    ## 1217      10004       5    2
    ## 1218      10005       7    1
    ## 1219      10005       9    2
    ## 1220      10006      12    1
    ## 1221      10006       3    2
    ## 1222      10007       8    1
    ## 1223      10007      16    2
    ## 1224      10008      13    1
    ## 1225      10008      10    2
    ## 1226      10009      13    1
    ## 1227      10009      11    2
    ## 1228      10010      13    1
    ## 1229      10010      15    2
    ## 1230      10011      13    1
    ## 1231      10011       3    2
    ## 1232      10012      13    1
    ## 1233      10012      12    2
    ## 1234      10013      10    1
    ## 1235      10014      11    1
    ## 1236      10015      15    1
    ## 1237      10016      11    1
    ## 1238      10017      10    1
    ## 1239      10017      14    2
    ## 1240      10018       1    1
    ## 1241      10018       2    2
    ## 1242      10019       3    1
    ## 1243      10020      13    1
    ## 1244      10020       3    2
    ## 1245      10021       5    1
    ## 1246      10021       3    2
    ## 1247      10022      16    1
    ## 1248      10022      15    2
    ## 1249      10023      16    1
    ## 1250      10023      15    2
    ## 1251      10024      11    1
    ## 1252      10024       2    2
    ## 1253      10025      14    1
    ## 1254      10026       9    1
    ## 1255      10026       8    2
    ## 1256      10027       8    1
    ## 1257      10027      12    2
    ## 1258      10028       8    1
    ## 1259      10028      12    2
    ## 1260      10029       8    1
    ## 1261      10029      12    2
    ## 1262      10030       8    1
    ## 1263      10030      12    2
    ## 1264      10031       8    1
    ## 1265      10031      12    2
    ## 1266      10032       8    1
    ## 1267      10032      12    2
    ## 1268      10033      12    1
    ## 1269      10033       4    2
    ## 1270      10034      10    1
    ## 1271      10034      16    2
    ## 1272      10035      10    1
    ## 1273      10035       3    2
    ## 1274      10036      11    1
    ## 1275      10037      14    1
    ## 1276      10038       8    1
    ## 1277      10038       4    2
    ## 1278      10039       1    1
    ## 1279      10040       7    1
    ## 1280      10040       3    2
    ## 1281      10041      11    1
    ## 1282      10041      17    2
    ## 1283      10042       6    1
    ## 1284      10042       3    2
    ## 1285      10043      14    1
    ## 1286      10043       2    2
    ## 1287      10044      14    1
    ## 1288      10045      13    1
    ## 1289      10045      16    2
    ## 1290      10046       7    1
    ## 1291      10046       9    2
    ## 1292      10047       7    1
    ## 1293      10047       2    2
    ## 1294      10048      17    1
    ## 1295      10048      10    2
    ## 1296      10049       6    1
    ## 1297      10049      17    2
    ## 1298      10050      10    1
    ## 1299      10050       2    2
    ## 1300      10051      14    1
    ## 1301      10051      18    2
    ## 1302      10052       9    1
    ## 1303      10052      18    2
    ## 1304      10053       9    1
    ## 1305      10054       2    1
    ## 1306      10054      14    2
    ## 1307      10055      13    1
    ## 1308      10056       8    1
    ## 1309      10057      17    1
    ## 1310      10058      16    1
    ## 1311      10058       5    2
    ## 1312      10059       2    1
    ## 1313      10059       9    2
    ## 1314      10060      12    1
    ## 1315      10060      15    2
    ## 1316      10061      18    1
    ## 1317      10062      16    1
    ## 1318      10062      14    2
    ## 1319      10063      16    1
    ## 1320      10063      14    2
    ## 1321      10064      11    1
    ## 1322      10064       5    2
    ## 1323      10065      12    1
    ## 1324      10065      16    2
    ## 1325      10066      17    1
    ## 1326      10066       8    2
    ## 1327      10067      16    1
    ## 1328      10067      18    2
    ## 1329      10068      14    1
    ## 1330      10068       2    2
    ## 1331      10069       1    1
    ## 1332      10069      18    2
    ## 1333      10070      11    1
    ## 1334      10070      17    2
    ## 1335      10071      11    1
    ## 1336      10071      14    2
    ## 1337      10072       9    1
    ## 1338      10072       5    2
    ## 1339      10073       1    1
    ## 1340      10073       3    2
    ## 1341      10074      15    1
    ## 1342      10075       6    1
    ## 1343      10075      18    2
    ## 1344      10076       9    1
    ## 1345      10076      14    2
    ## 1346      10077      11    1
    ## 1347      10078       5    1
    ## 1348      10078      10    2
    ## 1349      10079      16    1
    ## 1350      10079       3    2
    ## 1351      10080      13    1
    ## 1352      10081      13    1
    ## 1353      10082      13    1
    ## 1354      10083      13    1
    ## 1355      10084      13    1
    ## 1356      10085      13    1
    ## 1357      10086      14    1
    ## 1358      10086      17    2
    ## 1359      10087      10    1
    ## 1360      10087       5    2
    ## 1361      10088       1    1
    ## 1362      10088       2    2
    ## 1363      10089      16    1
    ## 1364      10089       3    2
    ## 1365      10090       7    1
    ## 1366      10090       4    2
    ## 1367      10091      17    1
    ## 1368      10091       1    2
    ## 1369      10092      17    1
    ## 1370      10092       1    2
    ## 1371      10093      17    1
    ## 1372      10093       1    2
    ## 1373      10094      13    1
    ## 1374      10095      13    1
    ## 1375      10096      13    1
    ## 1376      10097      13    1
    ## 1377      10098      13    1
    ## 1378      10099      13    1
    ## 1379      10100      13    1
    ## 1380      10100      14    2
    ## 1381      10101      15    1
    ## 1382      10101       9    2
    ## 1383      10102      15    1
    ## 1384      10102       9    2
    ## 1385      10103      15    1
    ## 1386      10104      15    1
    ## 1387      10104      18    2
    ## 1388      10105       5    1
    ## 1389      10105       9    2
    ## 1390      10106       5    1
    ## 1391      10106       9    2
    ## 1392      10107      17    1
    ## 1393      10108      17    1
    ## 1394      10109       6    1
    ## 1395      10109      13    2
    ## 1396      10110       6    1
    ## 1397      10110      13    2
    ## 1398      10111       6    1
    ## 1399      10111      13    2
    ## 1400      10112       4    1
    ## 1401      10112      17    2
    ## 1402      10113       4    1
    ## 1403      10113      17    2
    ## 1404      10114      12    1
    ## 1405      10114      16    2
    ## 1406      10115      10    1
    ## 1407      10115       8    2
    ## 1408      10116      11    1
    ## 1409      10116      17    2
    ## 1410      10117      11    1
    ## 1411      10117      17    2
    ## 1412      10118      16    1
    ## 1413      10118       5    2
    ## 1414      10119      16    1
    ## 1415      10119       5    2
    ## 1416      10120      16    1
    ## 1417      10120       5    2
    ## 1418      10121       1    1
    ## 1419      10122       7    1
    ## 1420      10122      13    2
    ## 1421      10123      13    1
    ## 1422      10123       3    2
    ## 1423      10124      14    1
    ## 1424      10124       3    2
    ## 1425      10125       8    1
    ## 1426      10125       3    2
    ## 1427      10126       6    1
    ## 1428      10127      11    1
    ## 1429      10128      12    1
    ## 1430      10129       4    1
    ## 1431      10129      10    2
    ## 1432      10130       6    1
    ## 1433      10130       3    2
    ## 1434      10131       6    1
    ## 1435      10131       3    2
    ## 1436      10132       6    1
    ## 1437      10132       3    2
    ## 1438      10133       6    1
    ## 1439      10133       3    2
    ## 1440      10134       6    1
    ## 1441      10134       3    2
    ## 1442      10135       6    1
    ## 1443      10135       3    2
    ## 1444      10136       6    1
    ## 1445      10136       3    2
    ## 1446      10137       6    1
    ## 1447      10137       3    2
    ## 1448      10138       6    1
    ## 1449      10138       3    2
    ## 1450      10139       6    1
    ## 1451      10139       3    2
    ## 1452      10140       6    1
    ## 1453      10140       3    2
    ## 1454      10141       6    1
    ## 1455      10141       3    2
    ## 1456      10142       6    1
    ## 1457      10142       3    2
    ## 1458      10143       8    1
    ## 1459      10143      18    2
    ## 1460      10144       8    1
    ## 1461      10144      18    2
    ## 1462      10145       8    1
    ## 1463      10145      18    2
    ## 1464      10146      16    1
    ## 1465      10146       2    2
    ## 1466      10147       9    1
    ## 1467      10147      18    2
    ## 1468      10148      13    1
    ## 1469      10149      10    1
    ## 1470      10149       8    2
    ## 1471      10150       7    1
    ## 1472      10150      18    2
    ## 1473      10151       6    1
    ## 1474      10152       6    1
    ## 1475      10153      11    1
    ## 1476      10153       7    2
    ## 1477      10154      13    1
    ## 1478      10154       9    2
    ## 1479      10155      14    1
    ## 1480      10155       9    2
    ## 1481      10156      14    1
    ## 1482      10156       8    2
    ## 1483      10157      14    1
    ## 1484      10157      16    2
    ## 
    ## [[3]]
    ##       id identifier generation_id damage_class_id
    ## 1      1     normal             1               2
    ## 2      2   fighting             1               2
    ## 3      3     flying             1               2
    ## 4      4     poison             1               2
    ## 5      5     ground             1               2
    ## 6      6       rock             1               2
    ## 7      7        bug             1               2
    ## 8      8      ghost             1               2
    ## 9      9      steel             2               2
    ## 10    10       fire             1               3
    ## 11    11      water             1               3
    ## 12    12      grass             1               3
    ## 13    13   electric             1               3
    ## 14    14    psychic             1               3
    ## 15    15        ice             1               3
    ## 16    16     dragon             1               3
    ## 17    17       dark             2               3
    ## 18    18      fairy             6              NA
    ## 19 10001    unknown             2              NA
    ## 20 10002     shadow             3              NA

Query for the average height and average weight of pokemon (pokemon table).

``` r
dbGetQuery(con, "SELECT avg(height) AS avg_height, avg(weight) AS avg_weight
           FROM pokemon")
```

    ##   avg_height avg_weight
    ## 1   12.46473     677.14

Query for the number of pokemon of less than average weight.

``` r
dbGetQuery(con, "SELECT COUNT(*) AS number
            FROM pokemon
            WHERE weight < (SELECT avg(weight) FROM pokemon)")
```

    ##   number
    ## 1    720

Query for the average weight of ghost-type pokemon.

``` r
dbGetQuery(con, "SELECT avg(pokemon.weight) AS avg_weight
           FROM pokemon 
           LEFT JOIN pokemon_types ON pokemon.id = pokemon_types.pokemon_id
           LEFT JOIN types ON pokemon_types.type_id = types.id
           WHERE types.identifier = 'ghost'")
```

    ##   avg_weight
    ## 1   693.2787

``` r
dbDisconnect(con)
```
