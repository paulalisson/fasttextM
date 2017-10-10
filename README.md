## fasttextM: Fast Multilingual Word Embeddings

**Authors:** Taylor B. Arnold, Nicolas Bailler, Paula Liss√≥n<br/>
**License:** [LGPL-2](https://opensource.org/licenses/LGPL-2.1)

 [![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/statsmaths/fasttextM?branch=master&svg=true)](https://ci.appveyor.com/project/statsmaths/fasttextM) [![Travis-CI Build Status](https://travis-ci.org/statsmaths/fasttextM.svg?branch=master)](https://travis-ci.org/statsmaths/fasttextM) [![Coverage Status](https://img.shields.io/codecov/c/github/statsmaths/fasttextM/master.svg)](https://codecov.io/github/statsmaths/fasttextM?branch=master)

## Overview

The **fasttextM** R package is designed to make it easy to
apply multilingual word embeddings to a dataset.

To install, grab the development version using devtools:
```{r}
devtools::install_github("statsmaths/fasttextM")
```

The basic installation of the package contains a very small set
of embeddings in English and French for testing purposes. To do
any real work, we need to install the full versions of these.
Here we use only the top 500MB's of the file; the full file is
6GB but more frequent words are contained at the top and we find
that the first 500-1000MB's are all we ever need in practice. Feel
free to reduce the number depending on your needs, internet speed,
and disk space.

```{r}
library(fasttextM)
ft_download_model("en", mb = 500)
ft_download_model("fr", mb = 500)
```

**Note that these only need to be downloaded once. They are then
saved locally on your machine.**

Next, we load these two models into memory:

```{r}
ft_load_model("en")
ft_load_model("fr")
```

We can now compute the embeddings of words in either language. Each
of these embeddings is a length 300 vector:

```{r}
en_embed <- ft_embed(words = c("hello", "fish", "cheese"),
                     lang = "en")
en_embed[,1:20]
```
```
          [,1]     [,2]      [,3]     [,4]      [,5]     [,6]     [,7]     [,8]
[1,] -0.159450 -0.18259  0.033443  0.18813 -0.067903 -0.13663 -0.25559  0.11000
[2,]  0.010938  0.32371 -0.169970  0.42405 -0.447940  0.15972  0.31668 -0.15638
[3,]  0.207420  0.04882  0.078373 -0.24411 -0.247880  0.35715  0.12923 -0.38060
         [,9]     [,10]     [,11]    [,12]     [,13]    [,14]    [,15]
[1,]  0.17275 0.0519710 -0.023302 0.038866 -0.245150 -0.21588 0.359250
[2,] -0.18606 0.0088676  0.167340 0.212200 -0.048738 -0.11182 0.098233
[3,]  0.40952 0.3056300 -0.209410 0.174500  0.070295 -0.39164 0.300000
         [,16]     [,17]    [,18]   [,19]    [,20]
[1,] -0.082526  0.121760 -0.26775 0.10072 -0.13639
[2,] -0.151830  0.043405 -0.22468 0.19034 -0.30115
[3,] -0.454120 -0.141620 -0.17220 0.24395 -0.18230
```

More interestingly, we can see the words that are close to these words
in the French embedding:

```{r}
en_embed <- ft_nn(words = c("jump", "fish", "cheese", "city", "swim"),
                  lang = "en", lang_out = "fr", n = 10)
en_embed
```
```
     [,1]       [,2]       [,3]        [,4]        [,5]        [,6]
[1,] "saut"     "sauts"    "sautant"   "Èlancer"   "sauter"    "saute"
[2,] "poissons" "poisson"  "anguilles" "crevettes" "anguille"  "salmonidÈs"
[3,] "fromage"  "fromages" "confiture" "beurre"    "saucisson" "confitures"
[4,] "ville"    "villes"   "capitale"  "faubourgs" "mÈgapole"  "quartier"
[5,] "nager"    "nage"     "nageurs"   "nageant"   "natation"  "nagent"
     [,7]        [,8]         [,9]          [,10]
[1,] "sauteurs"  "sauteur"    "tamgho"      "grimper"
[2,] "pÍchÈes"   "Ècrevisses" "crevette"    "pÍchÈs"
[3,] "pommes"    "babeurre"   "charcuterie" "saucissons"
[4,] "municipal" "banlieue"   "citÈ"        "quartiers"
[5,] "natatoire" "nagÈ"       "nageur"      "plongeon"
```

It is also possible, and often interesting, to use the nearest neighbours
function to find similar words in the same language.

To see a list of all available language for download, run `ft_languages()`.
It also indicates which models are downloaded and which have been loaded
into memory:

```{r}
ft_languages()[20:30,]
```
```
   iso_language_name iso_code installed loaded
20           Persian       fa
21           Finnish       fi
22            French       fr         *      *
23   Western Frisian       fy
24          Galician       gl
25          Gujarati       gu
26   Hebrew (modern)       he
27             Hindi       hi
28          Croatian       hr
29         Hungarian       hu
30          Armenian       hy
```

The package is a work in progress. If you need some functionality not
supported yet, please open a Issue and we will attempt to get it working
for the next release.

## Note

Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md).
By participating in this project you agree to abide by its terms.


