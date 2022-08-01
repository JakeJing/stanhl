# Stanhl — Stan Syntax Highlighting in knitr




## Requirements

- pygmentize

```bash
pip3 install Pygments
# pygmentize -V
```

- update xcode if you are using a mac

```bash
xcode-select --install
```


## Installation

- Install stanhl pkg

```R
library(devtools)
install_github('JakeJing/stanhl')
```

- download the [stanhl.tex](https://raw.githubusercontent.com/JakeJing/knitr-markdown-engines/master/templates/latextemplates/stanhl.tex) in your working directory and include it in the yaml header

```bash
wget https://raw.githubusercontent.com/JakeJing/knitr-markdown-engines/master/templates/latextemplates/stanhl.tex
```

```yaml
output:
   bookdown::pdf_document2:
     includes:
       in_header: latextemplates/stanhl.tex
```

## Usage

Pls see also the example scripts inside the **template** folder.

### 1. Hightlight stan script with compilation

- (1) using `stan_model`  and `stanhl` functions to compile and highlight stan code chunk

~~~R
```{r compile stan model, results='asis', echo=F}
# results='asis' is important, so that it can be preserved in the latex
md_binom <- stan_model("./stanscripts/stan_binom.stan")
stanhl(md_binom@model_code[1])
```
~~~

- (2) using stan chunk to compile and online R script to highlight

You first need to put this code chunk at the beginning of your R script with header setting as `results='asis', echo = F`.

~~~R
```{results='asis', echo = F}
library(devtools)
source_url("https://raw.githubusercontent.com/JakeJing/knitr-markdown-engines/master/templates/stan_highlight/stan_hl.R")
# You need to run it before the stan code chunk, it will create a highlight.tex at your current working directory.
stanhl_init()
```
~~~

With this, your `stan` code chunk will be automatically highlighted.

~~~R
```{stan, output.var="stan_binom", eval = T}
data{
int N; // number of observations
int H; // number of observed head
}
parameters{
real<lower=0, upper=1> p; // parameter for binomial distribution
}
model{
// you can also specify the prior here.
// If you leave it empty, it will use a flat or uniform prior
p ~ uniform(0, 1);
H ~ binomial(N, p); // likelihood func
}
```
~~~

### 2. Hightlight stan script without compilation

- (1) include a stan script

~~~R
```{r results='asis', echo = F}
stanhl_file("./stanscripts/stan_binom.stan")
```
~~~

- (2) include a stan script as string

~~~R
```{r results='asis', echo=F}
stan_binom <- "
data{
int N; // number of observations
int H; // number of observed head
}
parameters{
real<lower=0, upper=1> p; // parameter for binomial distribution
}
model{
// you can also specify the prior here.
// If you leave it empty, it will use a flat or uniform prior
p ~ uniform(0, 1);
H ~ binomial(N, p); // likelihood func
}
"
stanhl(stan_binom)
```
~~~

## Dependencies

- Rstudio 1.3
- R 4.1.2
- Pandoc 1.19

Note: you may get a different coloring if you compile the script on different versions of R studio.
