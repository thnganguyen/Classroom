Class1
================

``` r
Sortiment_hela <- read.csv("Class_files/systembolaget2018-10-08.csv")
head(Sortiment_hela)
```

    ##        nr Artikelid Varnummer                 Namn
    ## 1     101         1         1                Renat
    ## 2 7548901   1000008     75489 Valtellina Superiore
    ## 3 7774701   1000080     77747              Canella
    ## 4 7563901   1000083     75639  Vi\303\261a Soledad
    ## 5 7521801   1000131     75218              Purcari
    ## 6 8936603   1000155     89366 Midas Golden Pilsner
    ##                                        Namn2 Prisinklmoms Volymiml
    ## 1                                                   204.0      700
    ## 2                           Sassella Riserva        339.0      750
    ## 3 Valdobbiadene Prosecco Superiore Extra Dry        147.0      750
    ## 4        T\303\252te de Cuv\303\251e Reserva        159.0      750
    ## 5                              Freedom Blend        181.0      750
    ## 6                                                    26.7      330
    ##   PrisPerLiter  Saljstart Utg..tt                 Varugrupp
    ## 1       291.43 1993-10-01       0 Vodka och Br\303\244nnvin
    ## 2       452.00 2015-09-01       0           R\303\266tt vin
    ## 3       196.00 2015-09-01       0           Mousserande vin
    ## 4       212.00 2015-09-01       0                  Vitt vin
    ## 5       241.33 2015-09-01       0           R\303\266tt vin
    ## 6        80.91 2015-09-01       0                 \303\226l
    ##                  Typ        Stil Forpackning Forslutning   Ursprung
    ## 1              Vodka                  Flaska                       
    ## 2                                     Flaska             Lombardiet
    ## 3         Vitt Torrt                  Flaska               Venetien
    ## 4 Fylligt & Smakrikt                  Flaska       Natur      Rioja
    ## 5                                     Flaska                       
    ## 6         Ljus lager Modern stil      Flaska                       
    ##   Ursprunglandnamn                       Producent              Leverantor
    ## 1          Sverige                   Pernod Ricard Pernod Ricard Sweden AB
    ## 2          Italien                          Arpepe       Vinoliv Import AB
    ## 3          Italien                     Canella SpA   Fine Brands Sweden AB
    ## 4          Spanien Bodegas Franco-Espa\303\261olas       Terrific Wines AB
    ## 5        Moldavien                         Purcari      High Coast Wine AB
    ## 6          Sverige            Imperiebryggeriet AB   Imperiebryggeriet  AB
    ##   Argang Provadargang Alkoholhalt Sortiment           SortimentText
    ## 1     NA           NA      37.50%        FS     Ordinarie sortiment
    ## 2   2011           NA      13.50%        BS \303\226vrigt sortiment
    ## 3   2014           NA      11.00%        BS \303\226vrigt sortiment
    ## 4   2006           NA      12.00%        BS \303\226vrigt sortiment
    ## 5   2015           NA      13.50%        BS \303\226vrigt sortiment
    ## 6     NA           NA       4.90%        BS \303\226vrigt sortiment
    ##   Ekologisk Etiskt Koscher RavarorBeskrivning Pant EtisktEtikett
    ## 1         0      0       0        S\303\244d.   NA          <NA>
    ## 2         0      0       0               <NA>   NA          <NA>
    ## 3         0      0       0               <NA>   NA          <NA>
    ## 4         0      0       0             Viura.   NA          <NA>
    ## 5         0      0       0               <NA>   NA          <NA>
    ## 6         0      0       0               <NA>   NA          <NA>

``` r
MySortiment <- Sortiment_hela %>%
  mutate(Alkoholhalt = as.numeric(gsub("%","",Alkoholhalt))/100)

MySortiment <- Sortiment_hela %>%
  mutate(Varugrupp = ifelse(Varugrupp=="Röda", "Rött vin", ifelse(Varugrupp=="Vita", "Vitt vin", Varugrupp)))

PrisMax <- Sortiment_hela %>%
  filter(PrisPerLiter == max(PrisPerLiter)) %>%
  select(Namn)
PrisMax
```

    ##            Namn
    ## 1 Highland Park
