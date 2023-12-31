# OML Meta-model of Electronic Data Sheet

## Authors
Bc. [Michael Kozel](https://www.linkedin.com/in/michael-kozel/)\
Bc. [Patrik Mokriš](https://www.linkedin.com/in/patrik-mokri%C5%A1/)

Software Engineering students from Czech Technical University in Prague

## Introduction
This project was done as a team assignment for course LOG8505E at Polytechnique Montréal 2023.
The focus of the course was to introduce students to Model Driven Software Engineering and
let them practice and test obtained knowledge about DSMLs (Domain Specific Modeling Language)
on real-world projects. The selected domain of the project was Electronic Data Sheets (EDS) in the space industry.
Throughout this course, we had an opportunity to cooperate and get feedback from OpenCaesar and NASA
engineers. This post is the final output from our assignment. For all output of our work check [Github repository](https://github.com/mokripat/LOG8505EDeliverable) of this project.

## Problem description

Electronic Data Sheets (EDS) are comprehensive documents that contain detailed information about various
electronic components used in space missions, satellites, spacecraft, and other aerospace applications. These sheets
serve as a standardized reference for engineers, designers, and technicians involved in the development, maintenance,
and repair of electronic systems used in space exploration. These sheets play a vital role in the design, testing, and operation of electronic systems critical for space exploration endeavors.

The high-level objective of this project was to explore, analyze, and data-mine this standardized domain. By creating the OML model
we would face Model-Driven Engineering challenges, dive into the complexity and standardization of EDS, and come up
with feedback or potential improvements of the EDS regarding data interoperability in the Space Industry.

We were given the task of creating a meta-model of EDS using OML. It had four stages OML vocabulary, OML description,
writing SPARQL queries, and some form of transformation. Those four stages correspond to traditional steps of MDD
using DSML.

Our team's main goal was to create an OML meta-model based on already existing XSD specifications and data-mine information
from it using SPARQL. With this approach, we could test and verify this specification as opposed to the official documentation
of EDS. Throughout this process, we would collect knowledge and feedback to provide.

## Methodology

The methodology included a comprehensive examination and study of the SEDS domain. For creating the meta-model itself,
the main source was an XSD specification, obtained from https://sanaregistry.org/r/sois/. With a PDF description of SEDS,
it served as our source of truth. To create the meta-model, our approach was to utilize a series of transformations facilitated
by various tools. Initially, we employed the tool available [here](https://www.davidpace.de/generating-emf-models-from-xml-schema-definitions-xsds/)
for the first transformation, converting the XSD specification into an Ecore model.
Subsequently, the Ecore model underwent a second transformation utilizing the tool
available at OpenCaesar [Github](https://github.com/opencaesar/ecore-adapter), transitioning it into an OML vocabulary.
This was one of the recommended methodologies. For working with the OML, we were advised to use a tool called [Rosetta](https://github.com/opencaesar/oml-rosetta)
which is built on Eclipse IDE. Afterward, from the abstract OML vocabulary, we instantiated a
concrete OML description. Having the description, it is possible to create SPARQL queries to data-mine information
from the model. During the course, we had two meetings to discuss and obtain feedback for our meta-models.

## Progress background

During the course of our project involving Ecore to the OML Adapter, we encountered a bug that led
to a NullPointerException within the Adapter. We tried to fix this issue independently, we undertook
debugging efforts which resulted in commenting out a line of code responsible for triggering this failure.
This resulted in obtaining an output from the adapter which we considered as a base of our OML dictionary. We referred to
it as a "proto" meta-model. However, as we discovered throughout time, it was far from a complete and usable meta-model. To put it simply, it generated
all elements from the specification but not their relationships or generalizations.

<figure style="text-align: center;">
    <img src="resources/img.png"
         alt="Proto meta-model visualization">
    <figcaption>Proto meta-model visualization</figcaption>
</figure>

Over a span of four weeks, our team diligently worked on tuning the meta-model manually. By analyzing the XSD specification
we created a few Python scripts that would parse the relationships, generalization, and documentation from it to
enhance the proto meta-model. Despite our efforts, the complexity of the SEDS domain was too high to spot and finish
all constraints, restrictions, and details of the specification. Instantiating the meta-model was impossible. At least,
it helped us greatly in understanding the domain.

<figure style="text-align: center;">
    <img src="resources/img_1.png"
         alt="Proto meta-model improvement">
    <figcaption>Proto meta-model improvement, the complexity does not allow a view of the whole meta-model even on the smallest zoom ratio (5%)</figcaption>
</figure>

After the second round of feedback, we reported the exact bug to Elaasar Maged, which by cooperation was fixed and we obtained
the correct and fully detailed OML vocabulary. However, we had only a week left to complete our assignment.

## Metamodel (Vocabulary)

In this section, we would like to present and explain the final OML vocabulary. As could be seen in the previous
section, the meta-model is quite complex. With documenting comments, it has 3658 lines.

Firstly, the list of used OML keywords to define elements of the meta-model is as follows:
- Aspect - used for abstract types
- Concept - used for defining specific types (classes)
- Relation - used for modeling relation between classes
- Scalar - used to define sets of values that can be assigned
- Scalar property - used to specify types of properties (class variables)

Secondly, the list of used OML keywords to define the constraints of the meta-model is as follows:
- functional - can have a maximum of one value per instance (constraint for relations)
- domain - constraint for the type that scalar property belongs to
- range - constraint for a range of values that scalar property can have
- restricts (to exactly, to min) - constraint how many instances of scalar property can type have

In the following image, the high-level schema of the metamodel corresponds to the official specification.
It describes all the main components of the EDS. The root component Datasheet consists of two parts:
- Device - wraps information and meta-data of the concrete device
- Package - wraps three major parts defining the functionality, interface, and used data types of the device

<figure style="text-align: center;">
    <img src="resources/img_2.png"
         alt="Datasheets schema">
    <figcaption>High-level schema of the metamodel</figcaption>
</figure>

### Vocabulary examples

In this section, it is shown how different OML keywords in the vocabulary were used. Selected
examples showcase abstract syntax which will be used in the example in the next section.

#### Example 1

<figure style="text-align: center;">
    <img src="resources/img_6.png"
         alt="Example 1" style="max-height: 250px;">
    <figcaption>Example #1 - IntegerDataType</figcaption>
</figure>

To define integers, IntegerDataType was defined using the **concept** keyword. It is a specialization of the NumericDataType.

It possesses a single class variable named integerDataEncoding, defined as a **relation**. This means that it expects
an instance as a value and creates a connection between two instances when assigned.
The integerDataEncoding relation is guarded by the following constraints:

- **from** - specifies that this relation originates from instances of IntegerDataType.
- **to** - indicates that the relation points to instances of IntegerDataEncodingType.
- **functional** - implies that each instance of IntegerDataType can be associated with at most one instance of IntegerDataEncodingType through this relation.

The IntegerDataType concept is further restricted by specifying exactly one value for the IntegerDataType_range.

#### Example 2

<figure style="text-align: center;">
    <img src="resources/img_5.png"
         alt="Example 2" style="max-height: 250px;">
    <figcaption>Example #2 - DataEncodingType and ByteOrderType</figcaption>
</figure>

To define the abstract type DataEncodingType, the **aspect** keyword was used. It contains a single class variable named byteOrder,
established using the **scalar property** construct. The byteOrder class variable is defined with the following constraints:

- **domain** - specifies that this property is applicable to instances of the DataEncodingType.
- **range** - indicates the possible values which are enumerated under the ByteOrderType.
- **functional** - implies that each instance of DataEncodingType can possess only one value for byteOrder.

ByteOrderType is a **scalar** type that defines the enumeration of two specific values - "bigEndian" and "littleEndian" - representing the two possible endianness formats.

#### Example 3

<figure style="text-align: center;">
    <img src="resources/img_3.png"
         alt="Example 3" style="max-height: 250px;">
    <figcaption>Example #3 - IntegerDataEncodingType</figcaption>
</figure>

IntegerDataEncodingType uses the **concept** keyword to define the encoding type for integers which derives from DataEncodingType.
It contains two class variables encoding and sizeInBits. These class variables are defined using **scalar property**.
With the usage of **restricts** we specify that every instance of this element must have values of these variables
assigned and exactly one time.

Both class variables have specified:
- **domain** to express being assigned to IntegerDataEncodingType
- **range** to express allowed values (list of values or range)
- **functional** trait expressing that instance of the class variable can belong only to one instance of the IntegerDataEncodingType


## Instance Model - Calculator

To define a OML description we chose to instantiate DataSheet specification for [Calculator](https://gitrepos.estec.esa.int/taste/spacecreator/-/blob/master/tests/integrationtests/applications/sedsconverter/sedstosdl/resources/test_calculator.xml)
as one of the given Space Creator examples, we obtained during the feedback meeting. The description should resemble an instance
of a simple calculator with the ability to sum two integers from range <-1000000,1000000> and notify a user about the result.

The Calculator specification required these components:
- StateMachine with 2 states - Init and Idle
- 2 Commands - Init and Add
- 2 Operations with two Arguments - Add and Swap Operations, X1 and X2 arguments
- 1 activity - AddAndNotifyActivity to notify a user about the result

### Description syntax example
In this section, we will show a few examples covering important elements of the Calculator we defined.

<figure style="text-align: center;">
    <img src="resources/img_4.png"
         alt="Example 4" style="max-height: 250px;">
    <figcaption>Example #4 - CalculatorDataSheet description</figcaption>
</figure>

Firstly we created the root component - CalculatorDataSheet. It has 2 main components:

- Calculator: DeviceType - Wraps all information and meta-data of that device.
- Package: PackageType - Wraps functionality, interface, and used data types of the device.

In our instance, we focused on the Package element, to instantiate the functionality described in the
Calculator specification. The main elements of the Package are as follows:

- DataTypeSet - contains a set of used data types - In our case Integers (more in next example).
- CalculatorInterfaceSet - contains a set of interfaces - We needed only one, defining commands as user input for the Calculator.
- ComponentSet - contains a set of components providing the functionality of the Calculator.

<figure style="text-align: center;">
    <img src="resources/img_7.png"
         alt="Example 5" style="max-height: 300px;">
    <figcaption>Example #5 - Integer data type description</figcaption>
</figure>

In this example, it can be seen the definition of Integer data type and its related elements.
To be fully described, it needs a range and encoding which abstract syntax could be seen in the previous section.

<figure style="text-align: center;">
    <img src="resources/img_8.png"
         alt="Example 6" style="max-height: 230px;">
    <figcaption>Example #6 - Implementation of Calculator component description</figcaption>
</figure>

The implementation of the Calculator component consists of three components:
- VariableSet - Set of variables, for our calculation purposes, we created five of them.
- CalculatorActivitySet - Sets of activities, which can be interpreted as an output of the calculator, we needed only one.
- CalculatorStateMachineSet - Set of state machines that the device needs, we needed only one.

<figure style="text-align: center;">
    <img src="resources/img_9.png"
         alt="Example 7" style="max-height: 200px;">
    <figcaption>Example #7 - Activity</figcaption>
</figure>

To create an activity, we instantiated the ActivityType to the AddAndNotifyActivity, also this activity
has two arguments, that notify the user when two numbers are added.

<figure style="text-align: center;">
    <img src="resources/img_10.png"
         alt="Example 8" style="max-height: 200px;">
    <figcaption>Example #8 - StateMachine</figcaption>
</figure>

Then we also created a state machine that had two states -- Entry and Idle states. The Entry set is before the calculator
is initialized. The idle state represents when the calculator is ready to take input. The state machines also have assigned two
transitions.

<figure style="text-align: center;">
    <img src="resources/img_11.png"
         alt="Example 9" style="max-height: 200px;">
    <figcaption>Example #9 - Transition</figcaption>
</figure>

The Example #9 is the instance of the Add transition. This transition changes state from the IdleState to the IdleState.
This transition is evoked when the calculator is commanded to add two integers. With the evocation, the do will be executed.

<figure style="text-align: center;">
    <img src="resources/img_12.png"
         alt="Example 10" style="max-height: 200px;">
    <figcaption>Example #10 - Activity instance</figcaption>
</figure>

Lastly, the example of the invocation instance can be seen. When invoked, it calls the
AddAndNotify activity and the user will get notified about the result. All these shown elements resemble
the core of the Calculator device.

## Instance model
In these images, we can see how the vocabulary corresponds to the metamodel.
It is hard to display the whole model, so for better understandability, we provide
a simplified model that contains only important parts.

In Example #11 and Example #12 we can see a higher-level view of the system as a whole and
the relations between all elements of the description of the electronic datasheet.

<figure style="text-align: center;">
    <img src="resources/img_13.png"
         alt="Example 11" style="max-height: 300px;">
    <figcaption>Example #11 - Instance model - first part</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="resources/img_14.png"
         alt="Example 12" style="max-height: 300px;">
    <figcaption>Example #12 - Instance model - second part</figcaption>
</figure>


## Reflection

Reflecting on our project journey, it's evident that we struggled with the proto meta-model that hindered our progress significantly.
The time invested in completing the prototype output through the adapter ended up being a loss, costing us not finishing on time.
We realized, later than anticipated, that Maged could have helped us much earlier, highlighting a need
for more proactive communication and inquiry. Our oversight resulted in missed opportunities to delve deeper
into transformation processes and accomplish our goal.

However, despite the challenges,
this experience was not without earning valuable knowledge and contributions.
We sharpened our proficiency in Python, deeply learned about SEDS concepts, and delved into the fundamentals of the OML
language within the Eclipse environment. Constructing abstract syntax and integrating it with concrete syntax
in a complex domain taught us perseverance in the face of complexity. Lastly, the discovery and fix of the Ecore
to OML adapter bug helped the tool to be more robust for future uses.

Undoubtedly, this journey underscored the inherent difficulties, reminding us
of the resilience needed when navigating complicated tasks. Looking ahead, these insights will profoundly influence
our future approaches, underscoring the significance of proactive collaboration and comprehensive exploration
to streamline our path forward.

## Conclusion
In this project, we created a complete and precise meta-model of Spacecraft onboard interface services
electronic data sheets.
We analyzed the domain of the Electronic Data Sheets and created an OML meta-model using the provided tools, namely
- Eclipse for converting an XSD model to the Ecore model
- Ecore to OML adapter for creating OML meta-model from Ecore meta-model.

During the transformations of the metamodel, we ran into several problems with the provided ECORE to OML, which we first tried to repair ourselves
but without success. Then after contacting Maged Elaasar - the author of the Ecore to OML adapter, it was able to create the metamodel in OML.
However due to these problems after repairing the tool we had about two days to create some reasonable output.
We ended up creating a description of the calculator instance, but due to Gradle :owlReason task errors we have not been able to
create SPARQL queries. Because of this we were very close but ultimately failed to reach our main goal.
Our output can be seen in a [GitHub repository](https://github.com/mokripat/LOG8505E_Deliverable).
