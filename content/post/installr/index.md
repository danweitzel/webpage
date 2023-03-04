--- 


title: "How to install R on Ubuntu" 

subtitle: "" 

summary: "" 

authors: [Daniel Weitzel] 

tags: [] 

categories: [software] 

date: 2023-03-01T09:44:57+01:00 

lastmod: 2023-03-01T09:45:00+01:00 

featured: true 

draft: false 

  

  

  

# Featured image 

# To use, add an image named `featured.jpg/png` to your page's folder. 

# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight. 

image: 

  caption: "" 

  focal_point: "" 

  preview_only: false 

  

# Projects (optional). 

#   Associate this post with one or more of your projects. 

#   Simply enter your project's folder or file name without extension. 

#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`. 

#   Otherwise, set `projects = []`. 

projects: [] 

--- 

  
In this post I'll explain how to quickly install R on Ubuntu-based operating systems and make sure that R packages are installed with all necessary dependencies. There are two ways to do this. A fast way and the right way. The fast way is using Ubuntu's repositories to install R. However, the R version in the repositories might not be the most up to date one. With a couple extra steps, we can make sure that we have the most recent version of R available. 

## The fast way 

If you do not care about the most recent version and just want a fast and easy way to install R you can do so in two steps. First, we update the indices (It is recommended that you do this before any installation). Second, we install R. Just like this: 

``` 
sudo apt update
sudo apt install r-base r-base-dev 
``` 

Congratulations, you are done! 

I added `r-base-dev` so you can compile the source code of packages you want to install. 

## The better way 

If you care about having the most recent version of R follow these steps. 

As before, we first want to update our indices: 

``` 
sudo apt update 
``` 

Afterwards we install `software-properties-common` and `dirmngr` to assist with the installation: 

``` 
sudo apt install --no-install-recommends software-properties-common dirmngr 
``` 

Next up, we add the signing key for the repositories: 

``` 
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
``` 

and finally, we add the R 4.0 repository.  

``` 
sudo add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" 
``` 

Now you can finally install R: 

``` 
sudo apt install r-base r-base-dev 
``` 

## Installing R packages

You can install R packages from within R using the `install.packages()` command. In this case I recommend that you use the Posit/R Studio public package manager available [here](https://packagemanager.rstudio.com/client/#/). You can alternatively use your terminal to do that (that's the recommended way!). Just a heads up, the approach described here is only working for LTS releases.

Just add the current CRAN repository: 

``` 
sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+ 
``` 

You'll now be able to install almost all packages listed in the CRAN Task Views listed [here](https://cran.r-project.org/web/views/). As an example, you can install packages like the `tidyverse` like this:

``` 
sudo apt install r-cran-tidyverse 
``` 

If you just ran this code without reading ahead, you might have run into an error. This is because some packages (like the `tidyverse`) require non-R packages. While R is great at resolving dependencies on other R packages it cannot help you with dependencies on non-R packages. To get the tidyverse up and going you need to install `libcurl4-openssl-dev`,  `libssl-dev`, and `libxml2-dev`. 

``` 
sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev 
``` 

Additional packages that might be useful for you are `r-cran-dblyr` (a backend for databases) `r-cran-tidymodels` (tidy ML), `r-cran-naniar` and `r-cran-mice` (for missing data), `r-cran-quanteda`, `r-cran-tesseract`, `r-cran-tidytext` (for NLP. text analysis), `r-cran-modelsummary` and `r-cran-sjplot` (for model stats/visualizations), `r-cran-data.table` (as a tidyverse alternative). 

## What packages are available?

I mentioned earlier that almost all packages in the CRAN Task Views should be available in the Ubuntu repository. That is not 100% true. The overlap between available R packages on CRAN and the packages in the Ubuntu Repository is large but not perfect. For example, I was not able to install h2o through apt. 

All available packages in the Ubuntu repository are listed [here](https://launchpad.net/~marutter/+archive/ubuntu/c2d4u).

Most of the packages of the [CRAN Task Views](https://cran.r-project.org/web/views/) packages are in fact available. If you want to check those out you can do so on the website or using the `ctv` package. The data there is grouped into topics. Most social scientists are probably interested in the following topics: Bayesian, CausalInference, Databases, Econometrics, ExperimentalDesign, MachineLearning, MissingData, MixedModels, ModelDeployment, NaturalLanguageProcessing, and ReproducibleResearch.

You can use the `ctv` package with the following code within R:

``` 
install.packages("ctv")

# Listing all topics:
ctv::available.views()

# Listing all packages in a topic
ctv::ctv("MachineLearning")
``` 

Easiest way to find out if a given package is available? Just run `r-cran-packagename` and see. 

## Installing R Studio

While we can install R Studio with the terminal, I'd have to regularly update the blog post to add the correct link. To make my life easier I recommend you just head to the Posit website and download [R Studio](https://posit.co/download/rstudio-desktop/). 

If you do not know what version, you need you can run the following code in your terminal:

``` 
lsb_release -a 
``` 

