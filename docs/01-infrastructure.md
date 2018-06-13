# Infrastructure

We have 2 servers in the lab, `lightfoot` and `snowmane` (named after horses from Lord of the Rings).
The main "production" server is `lightfoot` where all of our data and compute resources exist.

## Components

We use [Docker](https://www.docker.com/) in our lab.
This allows us to install various components without affecting the underlying server.
A brief history of how we settled on docker and why is in this blog post on [From VMs to LXC Containers to Docker Containers](https://chendaniely.github.io/sdal/2017/07/07/vm_lxc_docker/).

Figure \@ref(fig:docker-lightfoot) depicts what conatiners we have on `lightfoot`.
You can think of each 'container' as an 'application' just like one you are running on your laptop.
But the behvaior of each 'container' is more like a separate server you connect to.

<div class="figure" style="text-align: center">
<img src="./figs/sdal_docker_lightfoot.png" alt="The Docker infrastructure used in SDAL. The containers on the top row are the parts of the system lab members will be connecting to and working on." width="100%" />
<p class="caption">(\#fig:docker-lightfoot)The Docker infrastructure used in SDAL. The containers on the top row are the parts of the system lab members will be connecting to and working on.</p>
</div>

The primary conatiners you will be using are the RStudio, Adminer, and Django/Wagtail containers.
They all exist on `lightfoot` and can all be reached in a browser with [`https://analytics.bi.vt.edu`](https://analytics.bi.vt.edu) and an appropriate suffix (e.g., [`/rstudio`](https://analytics.bi.vt.edu/rstudio), [`/db`](https://analytics.bi.vt.edu/db)).

## Accessing Servers

Your main point of contact will be using RStudio on `lightfoot`.
There's a few things that can be setup so you don't have to type your password all the time.
This involves creating "SSH keys".
Aside from creating keys, below is a set of links you'll probably be using all the time:

- Rstudio
    - Your own container: https://analytics.bi.vt.edu/YOUR_PID/rstudio
        - It's suggested you use the container assigned to yourself, since your work and crashed code is isolated from everyone else
    - Generic RStudio container: https://analytics.bi.vt.edu/rstudio
        - There is a generic container that can be used as well,
        it's availiable to you, but if you have an individual conatiner,
        it's better to use that one instead

## Project Template

Why project templates: https://chendaniely.github.io/sdal/2017/05/30/project_templates/

Other resources:

- https://github.com/ropensci/rrrpkg
- https://github.com/benmarwick/rrtools
- A template for research projects structured as R packages: https://github.com/Pakillo/template

```
project/            # the project/code repository
|
|- data/            # raw and primary data, are not changed once created
|  |
|  +- project_data/ # subfolder that links to an encrypted data storage container
|  |  |
|  |  |- original/  # raw data, will not be altered
|  |  |- working/   # intermediate datasets from src code
|  |  +- final/     # datasets used in analysis
|  |
|  +- more_data/    # some projects will need multiple links
|
|- src/             # any programmatic code
|  |- analysis1/    # user1 assigned to the project
|  +- analysis2/    # user2 assigned to the project
|
|- R/               # functions
|
|- tests/           # unit tests
|
|- output           # all output and results from workflows and analyses
|  |- figures/      # graphs, likely designated for manuscript figures
|  |- pictures/     # diagrams, images, and other non-graph graphics
|  +- analysis/     # generated reports for (e.g. rmarkdown output)
|
|- README.md        # the top level description of content
|
|- .gitignore       # git ignore file
|- project.Rproj    # RStudio project
|
|- DESCRIPTION      # Description file to repo into R package, if applicable
+- Makefile         # Makefile, if applicable
```

Notes:

- you can find our project template `.gitignore` file here: https://github.com/bi-sdal/project_template/blob/master/gitignore

## Getting a project repository

All of our code exist on a GitLab server (https://devlab.vbi.vt.edu)

### Using Terminal

Find the link to the repository

![](./figs/get_repo/010-gitlab.png)<!-- -->

![](./figs/get_repo/020-get_repo_url.png)<!-- -->

You can then `git clone` the project into a directory (e.g., `~/git/`)

### Using RStudio

![](./figs/get_repo/010-gitlab.png)<!-- -->

![](./figs/get_repo/020-get_repo_url.png)<!-- -->

![](./figs/get_repo/030-new_project.png)<!-- -->

![](./figs/get_repo/040-version_control.png)<!-- -->

![](./figs/get_repo/050-git.png)<!-- -->

![](./figs/get_repo/060-paste_screen.png)<!-- -->

![](./figs/get_repo/070-check_project.png)<!-- -->
