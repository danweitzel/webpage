--- 

# Documentation: https://sourcethemes.com/academic/docs/managing-content/ 

  

title: "A Guide to Linux for Social Scientists" 

subtitle: "" 

summary: "" 

authors: [Daniel Weitzel] 

tags: [] 

categories: [software] 

date: 2023-02-20T09:44:57+01:00 

lastmod: 2023-02-20T09:45:00+01:00 

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

  

  

When I was a graduate student, I immensely benefitted from a friend's guide to Linux for social scientists. Unfortunately, that post is not available anymore. This is an attempt at recreating the guide in the hope that it helps other social scientists who want to try out Linux-based operating systems.  

  

In this guide I will first talk a bit about early configurations of your Pop!_OS system and then dive into the social science config. We will install R, R Studio, Python, LaTeX, Pandoc, Git, and Zotero.  

  

I also added a section at the end in which I explain how to install frequently used software for collaborations (Libre Office, Dropbox, Skype, Zoom, Signal, and Slack).  

  

  

## Table of contents 

  

### The OS 

   1. [Terminal](#terminal) 
   2. [Early configs](#early) 
   3. [Snap and Flatpak](#sf) 

### Social Science Software
   1. [Emacs](#emacs) 
   2. [R](#r) 
   3. [Python](#python) 
   4. [LaTeX](#latex) 
   5. [Pandoc](#pandoc) 
   6. [Git](#git) 
   7. [Zotero](#zotero) 

### Other software 
   1. [Libre Office](#libre) 
   2. [Firefox](#firefox) 
   3. [Dropbox](#dropbox) 
   4. [Skype](#skype) 
   5. [Zoom](#zoom) 
   6. [Slack](#slack) 
   7. [Signal](#signal) 



# The operating system 

This guide is focused on Ubuntu, or Ubuntu-based operating systems. You can check out Ubuntu [here](https://ubuntu.com/download/desktop). Personally, I am currently running the Ubuntu-based [Pop!_OS](https://pop.system76.com/). The instructions in the blog post apply to both operating systems (you should not run into problems with any other Debian-based system either). 


<a name="terminal"></a> 
## Using the terminal

We will use the terminal a lot in this guide. You can open it with `Super + t` on Pop!_OS and `Alt+Shift+t` on Ubuntu. If you have never used the terminal before I recommend you check out this guide by Canonical [here](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview). 


<a name="early"></a> 
## Early configs 

First things first: update and upgrade your system and the recovery partition! 

``` 
sudo apt update && sudo apt upgrade 

sudo pop-upgrade recovery upgrade from-release 
``` 

If you are using Pop!_OS on a laptop with a GPU, you might want to enable [hybrid graphics](https://support.system76.com/articles/graphics-switch-pop/) to save some battery: 

``` 
sudo system76-power graphics hybrid 
``` 

If you are using a laptop, you might also want to check out `caffeine` to prevent your laptop from hibernating: `sudo apt install caffeine`. 

It is annoying but you will likely need proprietary media codecs and fonts that do not ship by default. `ubuntu-restricted-extras` to the rescue: 

``` 
sudo apt install ubuntu-restricted-extras 
``` 

Pop!_OS uses Gnome, so we want to install Gnome Tweaks. It allows us to customize our desktop rather easily. You can read more about it [here](https://itsfoss.com/gnome-tweak-tool/) and install it this way: 

``` 
sudo apt install gnome-tweaks 
``` 

In order to be able to deal with multiple audio sources, like headphones and speakers, I recommend PulseAudio Volume Control.

``` 
sudo apt install pavucontrol 
``` 

For some installations you might need curl (certainly for one of the installations in this guide): 

``` 
sudo apt install curl 
``` 

<a name="sf"></a> 
## Snap and Flatpak

I use apt a lot but sometimes software is only available in Snap or Flatpak. So, we need to install those as well. 

``` 
sudo apt install snapd 

sudo apt install flatpak gnome-software-plugin-flatpak gnome-software 
``` 

Using Snap and Flatpak can be more convenient sometimes. I am not going to dive into the pros and cons of snap, flatpak, and apt in this post. If you want to read more, you can do so [here](https://www.fosslinux.com/42410/snap-vs-flatpak-vs-appimage-know-the-differences-which-is-better.htm). 

# Social Science Software 

This guide assumes that you conduct quantitative social science research in R or Python and write documents using markdown or LaTeX. 

<a name="emacs"></a> 
## Emacs 

I use emacs for my writing, code, version control, calendar, todo, emails, and even as window manager. It is an extremely capable editor but certainly has a steep learning curve. I'll write more about how to customize it for social science use later. At the moment, I can highly recommend the System Crafters guides on [YouTube](https://www.youtube.com/@SystemCrafters/videos). 

If you want to give emacs a try you can install it using apt: 

``` 
sudo apt install emacs 
``` 

This usually is an older version of emacs. You can also install emacs via Snap.

``` 
sudo snap install emacs --classic 
```

Snap has the option of installing the most recent version of emacs. 

``` 
sudo snap install emacs --edge --classic 
``` 

<a name="r"></a> 
## R 

Next up is installing R! There are two ways to do this. A fast way and the right way. The fast way is using Ubuntu's repositories to install R. However, the R version in the repositories might not be the most up to date one. With a couple extra steps, we can make sure that we have the most recent version of R available. 

### The fast way 

If you do not care about the most recent version and just want a fast and easy way to install R you can do so in two steps. First, we update the indices (It is recommended that you do this before any installation). Second, we install R. Just like this: 

``` 
sudo apt update
sudo apt install r-base r-base-dev 
``` 

Congratulations, you are done! 

I added `r-base-dev` so you can compile the source code of packages you want to install. 

### The better way 

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

### Installing R packages

You can install R packages from within R using the `install.packages()` command or you can use your terminal to do that (that's the recommended way!). 

Just add the current CRAN repository: 

``` 
sudo add-apt-repository ppa:c2d4u.team/c2d4u4.0+ 
``` 

and install packages like the `tidyverse` like this:

``` 
apt install r-cran-tidyverse 
``` 

If you just ran this code without reading ahead, you might have run into an error. This is because some packages (like the `tidyverse`) require non-R packages. While R is great at resolving dependencies on other R packages it cannot help you with dependencies on non-R packages. To get the tidyverse up and going you need to install `libcurl4-openssl-dev`,  `libssl-dev`, and `libxml2-dev`. 

``` 
sudo apt install libcurl4-openssl-dev libssl-dev libxml2-dev 
``` 

### Installing R Studio

While we can install R Studio with the terminal, I'd have to regularly update the blog post to add the correct link. To make my life easier I recommend you just head to the Posit website and download [R Studio](https://posit.co/download/rstudio-desktop/). 

If you do not know what version, you need you can run the following code in your terminal:

``` 
lsb_release -a 
``` 

<a name="python"></a> 
## Python 

If you are working in `Python` and want to set it up on your Linux machine an installation is straightforward. Debian-based Linux versions ship with `Python3` by default. The version in the repositories is most likely not the most recent one though. If you are okay with that you can just add pip, some developer tools, and a virtual environment manager to your installation: 

``` 
sudo apt install -y python3-pip 

sudo apt install -y build-essential libssl-dev libffi-dev python3-dev 

sudo apt install -y python3-venv 
``` 

More recent versions of `Python` can be installed via the deadsnakes repository. If you jumped over the `R` installation, earlier make sure to install `software-properties-common` before you go ahead. 

Adding the deadsnakes repository and a specific `Python` version. Below we are installing `Python3.11`: 

``` 
sudo add-apt-repository ppa:deadsnakes/ppa 

sudo apt install python3.11 
``` 

If you want to use Anaconda the installation guide is on their website. You can find it [here](https://docs.anaconda.com/anaconda/install/linux/). 

<a name="latex"></a> 
## LaTeX 

We obviously want to write beautiful documents in LaTeX. To be able to do that we need to install the program first. Here things get a bit tricky. We want to install TeXLive but there are different options that vary in the packages included and accordingly also in their size. `texlive-base` is a mere 160MB, while `texlive-full` is almost 6GB. If you have plenty of hard drive space, I recommend you install `texlive-full`. Installing the full version minimizes the risk of ever running into a problem. If space is limited, I recommend you go for `texlive-latex-extra` with 464MB. 

Below is the code to install either of these options: 

``` 
sudo apt install texlive-latex-extra 

# or 

sudo apt install texlive-full  
``` 

Once you have installed LaTeX you also need an editor to write your documents in. Personally, I use emacs for this purpose. While emacs is great and has enormous customizability it also comes with somewhat of a learning curve. A quick intro to emacs and LaTeX is available [here](https://kevinlanguasco.wordpress.com/2019/04/29/latex-editing-with-emacs-on-manjaro/). Be aware, the author of the post uses Arch and the installation guide for emacs will not work if you have a Debian-based OS installed. If you just want a LaTeX editor to be able to start writing, I recommend [TeXstudio](https://www.texstudio.org/).  

```
sudo add-apt-repository ppa:sunderme/texstudio 

sudo apt update 

sudo apt install texstud 
``` 

If you want to use Sublime Text you can find a good guide [here](https://tspeckhofer.github.io/2021/07/17/latex-and-sublime-text.html). 

<a name="pandoc"></a> 
## Pandoc 

Pandoc is an amazing piece of software that will allow us to convert our plain text markdown or LaTeX files into various different file formats. Even Word documents! 

There are again two ways to install this software. We can use the version in the Ubuntu repositories or a more recent version from the developer. Everything I use `pandoc` for works with the older version in the Ubuntu repositories. Hence, I just go with that. However, if you want to have the newest version check out the installation instructions [here](https://github.com/jgm/pandoc/blob/main/INSTALL.md). 

If you are okay with an older version, go ahead and install it with the following command. 

``` 
sudo apt-get install pandoc 
``` 

If you want to (let’s be honest: must) convert a LaTeX document to a Word document (`.docx`) you can do it in one easy step. 

If you do not have any citations in your document, you can just convert the `.tex` file to a `.docx` file: 

``` 
pandoc manuscript.tex -o manuscript.docx 
``` 

However, if you have citations in your document, you must add the library file for `pandoc` and `pandoc-citeproc`. 

``` 
pandoc manuscript.tex --bibliography=library.bib -o manuscript.docx 
``` 

<a name="git"></a> 
## Git 

Git is software used for version control. It does an amazing job tracking changes in plain text files. You can find a great intro to git on [opensource.com](https://opensource.com/article/18/1/step-step-guide-git). You can use it locally on your computer or in connection with a cloud service. [Gitlab](https://about.gitlab.com/) and [Github](https://github.com/) are very popular options with free private repositories.  

We can install git using the terminal: 

``` 
sudo apt install git 
``` 

We can let git know what our user name and email are. This information allows others to see who wrote a commit and how to contact that person. You can set both globally with the following code (obviously using your own name and email: 


``` 
git config --global user.name "Firstname Lastname" 

git config --global user.email first.last@email.com 
``` 

<a name="zotero"></a> 
## Zotero 

If you do not want to learn how to use emacs and use packages like org-ref and helm-bibtex to organize your bibliography I recommend that you check out [Zotero](https://www.zotero.org/). Everyone should have a literature and reference management system. It makes your life as a researcher so much easier - all your PDFs, annotations, and citations live in one place and integrating citations into LaTeX or markdown documents is easy and straightforward. 

You can either download the Zotero installer from their website or use the following code to set it up:

``` 
curl -sL https://raw.githubusercontent.com/retorquere/zotero-deb/master/install.sh | sudo bash 

sudo apt update 

sudo apt install zotero 
``` 

Now Zotero should be installed, and you can go ahead and add your PDFs to it.  

If you want to install Zotero via Snap, just run `sudo snap install zotero-snap`. 

<a name="other_software"></a> 
# Other Software 

There is other software that I frequently (have to) use in order to collaborate with other people. Below I explain how you can install those quickly. 

<a name="libre"></a> 
## Libre Office 

Even though we can write in LaTeX and markdown and convert documents using pandoc there is probably no way around a WYSIWYG editor. A lot of people work with Word or Excel, and we need to be able to open these files. I prefer to use Libre Office for this, and it is available in the Ubuntu repositories: 

``` 
sudo apt install libreoffice-gnome libreoffice 
``` 

<a name="firefox"></a>
## Firefox 

Install Firefox with `sudo apt install firefox`. 

Note: if you frequently use Google Meets you might want to consider installing Google Chrome or Chromium. The full set of features of Google Meet is not available in other browsers. 

Chromium is installed with this command `sudo apt install chromium-browser` and Chrome with this one `sudo apt install google-chrome-stable`. 

<a name="dropbox"></a> 
## Dropbox 

I remember that installing Dropbox was a pain some time ago. Not anymore! They have straightforward instructions on their website. You can find them [here](https://www.dropbox.com/install-linux). 

<a name="skype"></a> 
## Skype 

Skype is not as popular as it once was, but some people still use it. You can install Skype with Flatpak or Snap. If you want to use apt just download it from the Skype website and install it: 

``` 
wget https://go.skype.com/skypeforlinux-64.deb 

sudo apt install ./skypeforlinux-64.deb 
``` 

If you want to install Skype via Snap, just run `sudo snap install skype`. 

<a name="zoom"></a> 
## Zoom 

Installing Zoom should also not be hard. You need to get the most recent deb installer file from the [Zoom website](https://support.zoom.us/hc/en-us/articles/204206269-Installing-or-updating-Zoom-on-Linux) and then install it: 

``` 
sudo apt install ./zoom_amd64.deb 
``` 

Zoom lists all dependencies that are required on the webpage linked above. If you run into any problems, make sure that these are installed.  

If you want to install Zoom via Snap, just run `sudo snap install zoom-client`. 

<a name="slack"></a> 
## Slack 

Slack is available in Flatpak and Snap but AFAIK not in apt. If you want to install it via apt head over to the Slack website and download the most recent deb file [here](https://slack.com/intl/en-in/downloads/linux). They are pushing the .rpm file, you have to browse down for the .deb file. Below I added a `#` as a placeholder for the version number you'll download. Change that before you run the commands: 

``` 
sudo chmod +x slack-desktop-#.##.#-amd64.deb 

sudo apt install ./slack-desktop-#.##.#-amd64.deb 

``` 

If you want to install Slack via Snap, just run `sudo snap install slack --classic`. 

<a name="signal"></a> 
## Signal 

I also use Signal a lot for quick communication about projects. Installing it via Snap is once more a one liner: `sudo snap install signal-desktop`. 


## MS Teams 

Lol, no. 

 
