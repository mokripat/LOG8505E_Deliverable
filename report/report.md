# OML Meta-model of Electronic Data Sheet

## Authors
Bc. [Michael Kozel](https://www.linkedin.com/in/michael-kozel/)\
Bc. [Patrik Mokriš](https://www.linkedin.com/in/patrik-mokri%C5%A1/)

Software Engineering students from Czech Technical University in Prague

## Introduction
This project was done as team assignment for course LOG8505E at Polytechnique Montréal 2023.
The focus of the course was to introduce students to Model Driven Software Engineering and 
let them practice and test obtained knowledge about DSMLs (Domain Specific Modeling Language)
on real-world project. The selected domain of the project were Electronic Data Sheets (EDS) in the space industry.
Throughout this course we had an opportunity to cooperate and get feedback from OpenCaesar and NASA
engineers. This post is the final output from our assignment.

## Problem description

## Methodology

The methodology included comprehensive examination and study of the SEDS domain. For creating of the meta-model itself,
the main source was a XSD specification, obtained from https://sanaregistry.org/r/sois/. With PDF description of SEDS,
it served as our source of truth. To create the meta-model, our approach was to utilize a series of transformations facilitated 
by various tools. Initially, we employed the tool available [here](https://www.davidpace.de/generating-emf-models-from-xml-schema-definitions-xsds/) 
for the first transformation, converting the XSD specification into an Ecore model. 
Subsequently, the Ecore model underwent a second transformation utilizing the tool 
available at OpenCaesar [Github](https://github.com/opencaesar/ecore-adapter), transitioning it into an OML vocabulary. 
This was one of the recommended methodologies. Afterward, from the abstract OML vocabulary we instantiated a 
concrete OML description. Having the description, it is possible to create a SPARQL queries to data-mine information
from the model.

## Progress background

During the course of our project involving Ecore to the OML Adapter, we encountered a bug that led
to a _NullPointerException_ within the Adapter. We tried to fix this issue independently, we undertook 
debugging efforts which resulted in commenting out a line of code responsible for triggering this failure. 
This resulted in obtaining an output from the adapter which we considered as a base of our OML dictionary. We referred to
it as a "proto" meta-model. However, as we discovered throughout time, it was far from a complete and usable meta-model. To put it simply, it generated
all elements from the specification but not their relationships or generalization.

<figure>
    <img src="img.png"
         alt="Proto meta-model visualization">
    <figcaption>Proto meta-model visualization</figcaption>
</figure>

Over a span of four weeks, our team diligently worked on tuning the meta-model manually. By analysing the XSD specification
we created few python scripts that would parse the relationships, generalization and documentation from it to
enhance the proto meta-model. Despite our efforts, the complexity of SEDS domain was too high to spot and finishing
all constraints, restrictions and details of the specification. Instantiating the meta-model was impossible. At least,
it helped us greatly in understanding the domain.

<figure>
    <img src="img_1.png"
         alt="Proto meta-model improvement">
    <figcaption>Proto meta-model improvement, the complexity does not allow view of the whole meta-model even on the smallest zoom ration (5%)</figcaption>
</figure>

After second round of feedback, we reported the exact bug to Elaasar Maged, which by cooperation was fixed and we obtained
the correct and fully-detailed OML vocabulary. However, we had only a week left to complete our assignment.

## Metamodel (Vocabulary)



## Instance Model - Calculator

## Reflection

## Conclusion