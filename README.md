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

- include a stan script with compilation

~~~R
```{r compile stan model, results='asis', echo=F}
# results='asis' is important, so that it can be kept in the latex
md_binom <- stan_model("./stanscripts/stan_binom.stan")
stanhl(md_binom@model_code[1])
```
~~~

- include a stan script without compilation

~~~R
```{r results='asis', echo = F}
stanhl_file("./stanscripts/stan_binom.stan")
```
~~~

- include a stan script as string without compilation

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

- If you want to highlight the stan code chunk directly, pls check this [example script](https://github.com/JakeJing/knitr-markdown-engines/blob/master/templates/stan_highlight/stan_highlight.Rmd).

  ~~~R
  ```{stan, output.var="binom_md", echo = F}
  ```
  ~~~

  
