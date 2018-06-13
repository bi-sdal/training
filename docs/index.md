--- 
title: "Training Manual for SDAL"
#author: "Daniel Chen"
#date: "2018-06-13"
site: bookdown::bookdown_site
output:
  bookdown::gitbook:
    lib_dir: "book_assets"
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
github-repo: bi-sdal/training
description: "This is a training manual for The Social and Decision Analytics Laboratory." 
---

# Welcome {-}

The purpose of this website is to consolodate all of the 'basic' trainings and knowledge in the lab to be used as a reference and trainining handbook used
in our summer [Data Science for the Public Good Program](https://www.bi.vt.edu/sdal/projects/data-science-for-the-public-good-program).
<!-- \@ref(data-science-for-the-public-good-program) -->

## The Social and Decision Analytics Laboratory {-}

The Social and Decision Analytics Laboratory brings together statisticians and social and behavioral scientists to embrace today’s data revolution, developing evidence-based research and quantitative methods to inform policy decision-making.

https://www.bi.vt.edu/sdal/about

### Overview: Our Mission & Methods {-}

As a leading laboratory in the Biocomplexity Institute of Virginia Tech, the Social and Decision Analytics Laboratory is modeling the social condition of metropolitan areas, integrating novel sources of data to examine health and wellness, and uncovering key factors that drive industrial innovation.

Social and decision analytics have the power to fundamentally change our understanding of the world around us. Our research informs effective policy-making under uncertainty, combining expertise in statistics and the social sciences to transform all data into actionable knowledge.

Leveraging analytics to solve real-world problems requires a broad range of expertise. The SDAL team includes thought leaders in a wide variety of fields: statistics, social, behavioral and economic sciences, political science, psychology, simulation, computer science, data analytics, information technology policy, and infrastructural resilience.

SDAL is driven to make data analytics an accessible and effective part of the policy-making process. SDAL’s newest partnerships and recent discoveries are regularly profiled on our news page, while public lectures are available in our presentations archive. Our location at the Virginia Tech Research Center in Arlington, Virginia allows us to partner with local, state, and federal governments, non-profit organizations, and industry throughout the D.C. metro area and beyond.

### Innovation: Our Research Focus Areas {-}

Every global challenge we face today is rooted in information. Advances in big data analysis have the potential to revolutionize the ways we address long-standing social problems such as unequal access to services and affordable housing.

To address these and other pressing issues, the Social and Decision Analytics Laboratory is pioneering three major domains of research.

The Science of “All" Data aims to integrate disparate sources of data—surveys and experiments;  local, state, and federal administrative government records; social media; mobile technology, and geographic information—to deliver a comprehensive understanding of social problems. We are experts in drawing actionable information out of messy data sets of all sizes.

Information Diffusion Analytics charts how knowledge and social attitudes spread through information networks using a combination of network science, cognitive modeling, crowd-sourcing, and machine learning. Our research predicts how information reaches people and affects their outlook.

Community Learning Data-Driven Discovery (CLD3) allows organizations to identify individually maintained data resources that serve their mutual needs. Information that would have been siloed within a single department is “liberated,” freed for use throughout the community. Our research focuses on how to discovery, liberate, and repurpose these data flows to achieve data-informed policy development.

Our unique CLD3 approach is supported by expertise in five sub-domains of analytics research: 

- Metropolitan Analytics applies data on infrastructure, environment, and people to understand how urban areas can support the well-being of their expanding populations. Our research guides the growth of resilient cities.
- Education and Labor Force Analytics seeks to provide a clearer picture of the factors that help students and employees thrive by understanding learning processes, social interactions, and learning environments. Our work examines the impact of education on labor force outcomes.
- Health and Well Being Analytics leverages public and private sector medical data to help communities deliver healthcare services where they are most needed. SDAL studies help healthcare resources do more good.
- Emergency  Management Analytics creates deep characterizations of situational awareness at all levels through accessing and integrating comprehensive information of conditions that increase risks and stress to the emergency responders and the citizens they serve. Our research guides the development of equitable policies and practices to improve the safety and security of our communities.
- Industrial Innovation Analytics utilizes data from partners in the private sector to identify factors that reduce costs and improve efficiency. We enable the private sector to move beyond business analytics to develop information integration technologies.

### Collaboration: Our Research Partnerships {-}

Since its founding in 2013, SDAL has developed its world-class statistical and data science capabilities in support of the Biocomplexity Institute’s overarching mission to predict, explain, and visualize the behavior of massively interacting systems.

Backed by the Biocomplexity Institute’s diverse research programs and unique, high-performance computing infrastructure, SDAL has established long-term partnerships with a variety of organizations including Arlington County, Procter & Gamble, the Robert Wood Johnson Foundation, the United States Census Bureau, and the Department of Housing and Urban Development.

The Social and Decision Analytics Laboratory welcomes new collaboration opportunities. To inquire, please contact our director, Sallie Keller.


## Data Science for the Public Good Program {-}


<img src="https://www.bi.vt.edu/sdal/content/generic/data-science-for-public-good-student-photo-overview.jpg" width="100%" />

The Data Science for the Public Good program (DSPG) engages young scholars in finding solutions to some of the most pressing social issues of our time. DSPG fellows conduct research at the intersection of statistics, computation, and the social sciences to determine how information generated within every community can be leveraged to improve quality of life.


### Problem {-}

Our communities are engaged in a tremendous struggle to manage conflicting forces that threaten their ability to evolve and thrive. Government officials and policymakers must continuously develop their capacity to provide health, safety, security, employment, and leisure to their constituents. Sustaining this level of growth is becoming increasingly difficult in a climate of diminishing resources, mounting inequality, rapid technological change, and expanding global networks. 

<img src="https://www.bi.vt.edu/sdal/content/generic/data-science-for-public-good-problem-policy-planning.jpg?v=1465936766" width="100%" />

Data-driven research provides a rich, mutually rewarding opportunity to leverage community knowledge and public information resources to affect positive social change. The Data Science for the Public Good program was established to connect aspiring data science scholars to communities which can benefit from their expertise.

Students selected through a competitive application process are engaged in a series of hands-on learning experiences while policymakers and government leaders receive data analysis support to inform difficult decisions related to healthcare, education, and social justice.  

The story of each community—its problems, needs, and aspirations—is contained within its data. The DSPG program's overarching objective is to equip new generations of scientists with the skills they need to bring this story to light for leaders in local government. 


### Methods {-}

The Data Science for the Public Good program teaches student fellows how to sift through vast amounts of information related to public safety, employment, and the provision of services to discover how communities can become more efficient and sustainable. Through the lenses of statistics, social science, and data science research, DSPG students will learn to integrate all available data resources in order to:

- Identify pressing issues through direct engagement with government and community leaders.
- Develop mechanisms to assist decision-makers in framing their large-scale policy questions and identifying data sources which can be leveraged to address these issues at the local, state, and federal levels.
- Create a two-way data pipeline to give local leaders a direct link to cutting-edge scientific analyses and researchers easier access to local, state, and federal data flows.

DSPG fellows are also given opportunities to diversify their expertise and form lasting professional connections by taking part in the Social and Decision Analytics Laboratory's data-driven research projects. Our research teams are:

- Horizontally integrated, combining disciplines including statistics, data science, and the social and behavioral sciences to address complex issues.
- Vertically integrated, allowing students to collaborate with project stakeholders at all levels including undergraduates, graduates, postdoctoral associates, research faculty, and local, state, and federal agency leadership.

These unique fellowship experiences are made possible through the support of several research organizations dedicated to serving the public good: Virginia Tech’s Global Forum for Urban and Regional Resilience (GFURR); American Statistical Association’s NSF Research Experience for Undergraduates (REU); and sponsored research. 

### Impact {-}

<img src="https://www.bi.vt.edu/sdal/content/generic/data-science-for-public-good-impact-location.jpg?v=1465936895" width="100%" />

As metropolitan areas assume greater responsibility for driving social transformation and economic prosperity, the demand for publicly engaged and highly trained data scientists will continue to grow.

For students eager to participate in this emerging field, the Data Science for Public Good program promises to deliver a unique set of resources and opportunities.

World-Class Training: DSPG fellows participate in professional training and development workshops, poster presentations, and technical report and publication writing. Formal sessions on data science practices introduce students to scientific and statistical computing tools including R, Python, and GIS. Workshops also include accessing and using local, state and federal data resources such as Census products and open data portals. 

Professional Support: Each DSPG fellow is assigned a post-doctoral and senior researcher mentor. These mentors guide students' research and facilitate discussions about future career opportunities, with a focus on public service. DSPG fellows also participate in strategic planning activities related to their projects including sponsor meetings, presentations, and other events.

National Networks: Leveraging our laboratory's location in the National Capital Region, excursions are organized to events at AAAS, national academies, congressional hearings, and a variety of Washington metropolitan science and technology activities. Fellows also attend the SDAL seminar series with national and international speakers from government, academia, and industry.

To learn more about how our DSPG program is making a difference in the lives of students, read this profile on our inaugural cohort of summer research fellows. 

### Program Sponsors: Who We Work With {-}

Every DSPG research project begins with a collaboration. While our sponsor organizations face unique challenges, they share a common commitment: leveraging data science to better serve the public good.

<div class="figure">
<img src="https://www.bi.vt.edu/sdal/content/generic_page/dspg-sponsor-logos.jpg" alt="Nonexclusive list of sponsors" width="100%" />
<p class="caption">(\#fig:unnamed-chunk-4)Nonexclusive list of sponsors</p>
</div>

### Previous Projects {-}

Data Science for the Public Good Research Presentations
Research from the Data Science for the Public Good (DSPG) program addresses fundamental problems faced by governmental agencies tasked with serving the public good, institutions ranging from the Department of Defense to emergency response services in our local county. The poster presentations below represent the range of research we've performed on behalf of each of our sponsors.

You can find a list of previous presentations here:

[https://www.bi.vt.edu/sdal/careers/call-for-students/dspg-research-presentations](https://www.bi.vt.edu/sdal/careers/call-for-students/dspg-research-presentations)

## Lesson Materials {-}

The main source for the training materials come from Software-Carpentry [@swc], specifically the Bash, Git [@swc-git], and SQL lessons.

More references about Software-Carpentry and the challenges in scientific computing can be found here:
[@software-carpentry]
[@wilson-scientists-really-use-computers]
[@hannay-scientific-software-survey]
[@wilson-learn-history]
[@wilson-software-carpentry]
[@wilson-bottleneck]

### Software-Carpenty {-}

### Data-Carpentry {-}

### The Carpentries {-}

### DataCamp {-}

### R for Data Science {-}

By: Garrett Grolemund and Hadley Wickham

http://r4ds.had.co.nz/

<div class="figure" style="text-align: center">
<img src="http://r4ds.had.co.nz/cover.png" alt="R for Data Science Cover"  />
<p class="caption">(\#fig:unnamed-chunk-5)R for Data Science Cover</p>
</div>
