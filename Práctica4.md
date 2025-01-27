Práctica 4 - Excel y más gráficos
================
AE
24/7/2018

Importando más bases de datos
-----------------------------

Ya importamos datos desde el formato "dbf". Muchas veces construimos nuestra bases de datos desde excel. Vamos a trabajar con una base por Estados, con indicadores que dan cuenta de la competitividad estatal. Vamos instalar la librerìa "readxl"

``` r
install.packages("readxl", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## 
    ##   There is a binary version available but the source version is
    ##   later:
    ##        binary source needs_compilation
    ## readxl  1.0.0  1.1.0              TRUE

    ## installing the source package 'readxl'

    ## Warning in install.packages("readxl", repos = "http://cran.us.r-
    ## project.org", : installation of package 'readxl' had non-zero exit status

Llamamos la librería

``` r
library(readxl)
```

¡Recuerda poner el directorio!

``` r
setwd("/Users/anaescoto/Dropbox/DGAPA/2018/RMD")
```

Luego aplicamos el comando. Es de la siguiente forma. Nota que en el el paréntesis va el directorio donde tenemos nuestro archivo. (La base se puede descargar desde "<https://www.dropbox.com/s/1e6hevsv17bozzg/Excel_2014_IMCO.xlsx?dl=0>"

``` r
Excel_2014_IMCO <- read_excel("Excel_2014_IMCO.xlsx")
```

    ## Warning in strptime(x, format, tz = tz): unknown timezone 'zone/tz/2018c.
    ## 1.0/zoneinfo/America/Mexico_City'

``` r
#View(Excel_2014_IMCO)
```

¿Cómo qué tipo de objeto nos importa este comando?

``` r
class(Excel_2014_IMCO)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

Ya revisamos análisis descriptivo para algunas variables nominales u ordinales. Vamos a hacer algunos gráficos para variables cuantitativas. \#Histograma básico

``` r
hist(Excel_2014_IMCO$Homicidios)
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-6-1.png)

Podemos modificarlo de a poco a poco. Modificar el título

``` r
hist(Excel_2014_IMCO$Homicidios, main="Histograma de Homicidios") #Histograma de homicidios en los estados con el título “Histograma de homicidos”
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-7-1.png) Le podemos modificar el título del eje de las x y de las y

``` r
hist(Excel_2014_IMCO$Homicidios, main="Histograma de Homicidios", xlab="Homicidios", ylab="Frecuencia") 
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-8-1.png)

¡A ponerle colorcitos! Recuerda aquí hay una lista <http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf>

``` r
hist(Excel_2014_IMCO$Homicidios, main="Histograma de Homicidios", xlab="Homicidios", ylab="Frecuencia", col="deeppink1") 
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-9-1.png)

Notamos que hay una agrupación de estados con menos de 40 homicidios por cada 100,000 habitantes.

Hagamos histogramas de otras variables

``` r
hist(Excel_2014_IMCO$`Esperanza de vida`, main="Esperanza de Vida", xlab="Años", ylab="Frecuencia", col="deepskyblue", border ="deepskyblue4" ) 
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-10-1.png)

¿Algo diferente? Cuando hay espacios en el nombre de nuestras variables en un dataframe, tenemos que ponerlo dentro de estos símbolos \`\`

ggplot &lt;3
============

Hoy vamos a presentar a un gran paquete ¡Es de los famosos! Y tiene más de diez años. <https://qz.com/1007328/all-hail-ggplot2-the-code-powering-all-those-excellent-charts-is-10-years-old/>

Hay dos funciones en paquete ggplot2

qplot() – para "quick plots", gráficos rápidos. Nos vamos a concentrar en este curso en estos.
ggplot() – Para controlar de manera "granular" todo lo de nuestros gráficos.

Más info de qplot <http://ggplot2.org/book/qplot.pdf>

Vamos a empezar, como siempre, instalando el comando

``` r
install.packages("ggplot2", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## also installing the dependency 'vdiffr'

    ## 
    ##   There are binary versions available but the source versions are
    ##   later:
    ##         binary source needs_compilation
    ## vdiffr   0.2.1  0.2.3              TRUE
    ## ggplot2  2.2.1  3.0.0             FALSE

    ## installing the source packages 'vdiffr', 'ggplot2'

    ## Warning in install.packages("ggplot2", repos = "http://cran.us.r-
    ## project.org", : installation of package 'vdiffr' had non-zero exit status

    ## Warning in install.packages("ggplot2", repos = "http://cran.us.r-
    ## project.org", : installation of package 'ggplot2' had non-zero exit status

``` r
library(ggplot2)
```

Univariado
==========

Podemos hacer histogramas y gráficos de densidad

El comando tiene la siguiente form

qplot (VAR, dara = NombreDataframe, geom="histogram") qplot (VAR, dara = NombreDataframe, geom="density")

``` r
qplot(Homicidios, data = Excel_2014_IMCO, geom = "histogram")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](Práctica4_files/figure-markdown_github/unnamed-chunk-12-1.png)

``` r
qplot(Homicidios, data = Excel_2014_IMCO, geom = "density")
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-12-2.png)

Bivariado
=========

Vamos a hacer un par de gráficos de dos variables. Vamos a graficar los datos de los secuestros y pobreza. Este gráfico se conoce como un <b>scatterplot</b>.

Checa: en este comando, como usamos la opción de "data", no tenemos que poner el nombre del data frame antes de la variable.

El formato es el siguiente:

qplot(VariableX, VariableY, data = NombreDataframe)

En nuestro ejemplo queda así:

``` r
qplot(Pobreza, Homicidios, data = Excel_2014_IMCO)
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-13-1.png)

Una transformación importante en los análisis cuantitativos es la transformación logarítmica

``` r
qplot(log(Pobreza), log(Homicidios), data = Excel_2014_IMCO)
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-14-1.png) \# Guardando nuestro gráfico como un objeto

``` r
grafico<-qplot(log(Pobreza), log(Homicidios), data = Excel_2014_IMCO)
```

Theme Assist Addins
===================

Existe un add-in - es decir un complemento - para que podamos modificar más fácilmente los gr´áficos que hacemos con ggplot. Lo tenemos que instalar. Así como hemos instalado nuestros otras paqueter´ías.

``` r
install.packages("ggThemeAssist", repos = "http://cran.us.r-project.org", dependencies = TRUE)
```

    ## Installing package into '/Users/anaescoto/Library/R/3.3/library'
    ## (as 'lib' is unspecified)

    ## 
    ## The downloaded binary packages are in
    ##  /var/folders/fr/mw1x21js54367mjdhqsjfwqm0000gn/T//Rtmp71GT2G/downloaded_packages

``` r
library(ggThemeAssist)
```

Para utilizarlo, tenemos que nombrar el objeto que queremos editar (por eso aprendimos a guardarlo en un objeto anteriormente). Lo escribimos. Lo seleccionamos y buscamos el complemento en la pestaña "Addins"

![image](/Users/anaescoto/Dropbox/DGAPA/2018/RMD/addin.png)

Complicando las cosas (nomás tantito)
=====================================

Vamos hoy a establecer las regiones para evidenciar subgrupos en nuestros estados.

1.  Noroeste: Baja California, Baja California Sur Chihuahua, Durango, Sinaloa y Sonora;
2.  Noreste: Coahuila, Nuevo León, Tamaulipas;
3.  Occidente: Colima, Jalisco, Michoacán y Nayarit;
4.  Oriente: Hidalgo, Puebla; Tlaxcala y Veracruz;
5.  Centro norte: Aguascalientes, Guanajuato. Querétaro, San Luis Potosí y Zacatecas;
6.  Centro sur: Ciudad de México, México y Morelos;
7.  Suroeste Campeche, Quintana Roo, Tabasco y Yucatán Roo; y
8.  Suroeste: Chiapas, Guerrero y Oaxaca

``` r
Excel_2014_IMCO$region<-NA
```

Vamos a hacer vectores de cada región

``` r
noroeste<-c("Baja California","Baja California Sur", "Chihuahua", "Durango","Sinaloa", "Sonora")
noreste<- c("Coahuila", "Nuevo León", "Tamaulipas")
occidente <-c("Colima", "Jalisco", "Michoacán", "Nayarit") 
oriente <- c("Hidalgo", "Puebla","Tlaxcala", "Veracruz")
centronorte<-c("Aguascalientes", "Guanajuato", "Querétaro", "San Luis Potosí", "Zacatecas")
centrosur<- c("Ciudad de México", "México",  "Morelos")
suroeste <-c("Campeche", "Quintana Roo", "Tabasco","Yucatán")
sureste <- c("Chiapas", "Guerrero", "Oaxaca")
```

<b>Mi primer loop </b>

R puede hacer loop con el comando for Nos permite repetir una operación de acuerdo a los valores de una secuencia o de un vector. Aquí lo hacemos para el vector noroeste

``` r
for(i in noroeste) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Noroeste"
}
```

Lo hacemos para el resto de regiones

``` r
for(i in noreste) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Noreste"
}

for(i in occidente) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Occidente"
}

for(i in oriente) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Oriente"
}
for(i in centrosur) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Centro Sur"
}

for(i in centronorte) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Centro Norte"
}

for(i in suroeste) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Sur oeste"
}

for(i in sureste) {
  reg<-i
  Excel_2014_IMCO$region[Excel_2014_IMCO$edo==reg] <- "Sur Este"
}
```

Con subgrupos podemos agregar otra variable en nuestro gráficos

``` r
q1<-qplot(log(Pobreza), log(Homicidios), data = Excel_2014_IMCO, shape = region)
q1
```

    ## Warning: The shape palette can deal with a maximum of 6 discrete values
    ## because more than 6 becomes difficult to discriminate; you have 8.
    ## Consider specifying shapes manually if you must have them.

    ## Warning: Removed 7 rows containing missing values (geom_point).

![](Práctica4_files/figure-markdown_github/unnamed-chunk-21-1.png)

``` r
q2<-qplot(log(Pobreza), log(Homicidios), data = Excel_2014_IMCO, colour = region)
q2
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-22-1.png)

Con una regresión lineal

``` r
q3<-qplot(Pobreza, Homicidios, data = Excel_2014_IMCO, geom = c("point", "smooth"), method = "lm")
```

    ## Warning: Ignoring unknown parameters: method

``` r
q3
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-23-1.png)

Para tener un subgráfico por región

``` r
q4<-qplot(Pobreza, Homicidios, data = Excel_2014_IMCO) + facet_grid(. ~ region)
q4
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-24-1.png)

Etiquetas
=========

``` r
q5<-qplot(Pobreza, Homicidios, data = Excel_2014_IMCO, label=Excel_2014_IMCO$edo, geom="text")
q5
```

![](Práctica4_files/figure-markdown_github/unnamed-chunk-25-1.png)
