---
title: Entity classes and relationships for recipes
author: Dave Dubin
date: February, 2019
references:
- id: winston1987
  type: article-journal
  author:
  - family: Winston
    given: Morton E.
  - family: Chaffin
    given: Roger
  - family: Herrmann
    given: Douglas
  issued:
  - year: '1987'
  title: A taxonomy of part-whole relations
  container-title: 'Cognitive science'
  publisher: Elsevier
  volume: 11
  issue: 4
  page: '417-444'
...
---

# Overview

This is a first draft of entity classes and relationships in the
domain of recipes.  I've based my choice of the twenty most important
terms on a use case of matching published recipes with tools in the
user's kitchen and ingredients in the user's pantry. An information
system based on these modeling choices would need to answer questions
about what meal could be prepared with ingredients at hand, or what
would need to be purchased in advance of preparing a particular
recipe. Most people don't currently have the contents of their kitchen
cabinets listed in a database, but advances in smart appliances and
smart shopping carts make prospects for such information systems
plausible for the near future.

Examples of how the use cases informed the model are the "equal or
greater amount" and "equal or greater magnitude" relationships that
don't appear directly in published recipes or cookbooks, but which
would be instrumental in determining whether, for example, a bag of
flour would contain enough for three sheets of chocolate
cookies. Other examples are the "uses tool" and "realizes property"
relationships that can help identify tools necessary for the recipe
whether explicity listed (the former) or not (the latter).

In any information system all entities would have titles and and
descriptions expressed in text. Since that is understood (and since
this is a domain model, not a data model), standard metadata fields
like title and description are not listed among the attributes.


# Entity Classes

### Food Substance 
A type of food or edible material that forms an ingredient or
completed recipe. Examples include "potato," "salt," "cornstarch," and
"chicken broth." Instances of this class are themselves classes.

### Ingredient
A class of physical objects, each a like quantity of the same food
substance. Examples include "one bay leaf," "two teaspoons of
cornstarch," and ""three grams of salt." Instances of the class
"Ingredient" are themselves classes, since "one bay lead" is not a
particular bay leaf, but whatever bay leaf is used during the
execution of a recipe.

#### Ingredient attributes
* Optional (truth value). This property obtains if the ingredient is
  optional.

### Qualifying property 
A property required for a quantity of a food substance in order to
qualify for an ingredient Examples include "chopped," "sifted fine,"
and "sautéd to clear."  Since any ingredient may have more than one of
these, this can't be a simple attribute of an ingredient.

#### Attributes of a qualifying property
* Prerequisite (truth value). A prerequisite property is expected to
  obtain prior to the first step of the recipe through action by the
  cook (chopping, defrosting, etc.)
* Requirement (truth value). A requirement property distinguishes
  suitable from unsuitable food substances (fresh lemon juice as
  compared to bottled, kosher salt as compared to ordinary).


### Quantity
A number with an optional unit of measurement, for example "2" or "2
teaspoons." Quantities are descriptions of magnitudes.  Units of
measurement are essential to the identity of what we'll call a
quantity, so that "1 teaspoon" is a different quantity than "4.92892
cc" even though they are the same magnitude.

#### Quantity attributes
* Type: discrete vs. continuous

### Recipe
A step-wise procedure for producing a quantity of food from a group of
ingredients.

#### Recipe attributes
* Yield (Quantity).
* Number of servings (number)
* Serving size (quantity)
* Total time (temporal extent)
* Language

### Source
* A bibliographic description of an information resource.

#### Attributes of a source
* Content (text). The bibliographic description expressed as text.
* Notation (text). The notation in which the description is expressed.

### Step

A discrete ordered part of a repeatable process.  An example of a step
would be, "place chicken and next 11 ingredients in a 6-qt. slow
cooker. Cover and cook on LOW 6 hours or until chicken and vegetables
are tender and chicken separates from bone."

#### Attributes of a step

* First in sequence (truth value). This property obtains if the step
  is the first in a sequence of steps.
* Optional (truth value). This property obtains if the step is
  optional.


### Tool
A physical object that participates in a step or causes a qualifying
property to obtain. Examples include "saucepan," "paring knife,"
"grater."

# Relationships

### Directly follows
* **Scope note:** a temporal relationship between a step and the step
    that immediately follows it
* **Domain:** Step
* **Codomain:** Step
* **Arity:** 2
* **Cardinality:** 1-to-1

### Equal magnitude
* **Scope note:** equivalence relationship obtaining between
    quantities of equal magnitude
* **Domain:** Quantity
* **Codomain:** Quantity
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** For example, 1 US teaspoon equals 4.92892 ml.

### Equal or greater amount
* **Scope note:** an asymmetric relationship between ingredients
    constituted of the same substance, without the requirement for
    commensurable quantities or magnitudes.
* **Domain:** Ingredient
* **Codomain:** Ingredient
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** Domain and codomain are ingredient, not quantity. For
    example, "2 pounds of salt is an equal or greater amount than 2
    teaspoons of salt," but not "Five cups is an equal or greater
    amount than 2 teaspoons."

### Equal or greater magnitude
* **Scope note:** relationship obtaining between commensurable
    quantities where one has a greater or equal magnitude.
* **Domain:** Quantity
* **Codomain:** Quantity
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** Domain and codomain are quantity, not amounts of a
    particular substance, so participants must be commensurable even
    if the measurement units are different. For example, "five cups is
    greater than or equal to one teaspoon." But not "2 pounds of salt
    is greater than or equal to 2 teaspoons of salt."

### Has constituent
* **Scope note:** the food substance constituting the ingredient
* **Domain:** Ingredient
* **Codomain:** Food substance
* **Arity:** 2
* **Cardinality:** many-to-1

### Has quantity
* **Scope note:** The amount of a substance constituting an
    ingredient.
* **Domain:** Ingredient
* **Codomain:** Quantity
* **Arity:** 2
* **Cardinality:** many-to-1

### Has source
* **Scope note:** An attribution of the source of some recipe or other
    information.
* **Domain:** Recipe
* **Codomain:** Source
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** More properly, the domain should be a superclass like
    "information resource," but we're not including upper level
    classes in this list.

### Has substep
* **Scope note:** An ordered, functional feature of an activity. A
    piece of a larger step.
* **Domain:** Step
* **Codomain:** Step
* **Arity:** 2
* **Cardinality:** 1-to-many
* **Remarks:** Some recipe steps are themselves sequences of smaller
    steps. In the meronymic classification of Winston, Chaffin, and
    Herrmann -@winston1987 this is an "activity/feature" relationship.

### Includes
* **Scope note:** participation of a step in a recipe.
* **Domain:** Recipe
* **Codomain:** Step
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** Some steps will recur in many recipes.

### Realizes property
* **Scope note:** An instrumental relationship obtaining between a
    tool and a qualifying property realized through the use of the
    tool.
* **Domain:** Tool
* **Codomain:** Qualifying property
* **Arity:** 2
* **Cardinality:** many-to-many
* **Remarks:** For example, a grater is used for grating, and can
    cause something to be grated.

### Requires
* **Scope note:** participation of an ingredient in a recipe.
* **Domain:** Recipe
* **Codomain:** Ingredient
* **Arity:** 2 (binary relationship)
* **Cardinality:** many-to-many
* **Remarks:** Consider recording the nutritional content of an
    ingredient; it will be easier to keep consistent if each
    "ingredient" is characterized once. Assuming the nutritional
    details are consistent across recipes, many-to-many is the more
    appropriate cardinality.

### Uses tool
* **Scope note:** The relationship obtaining between a step in a
    recipe and a tool employed during that step.
* **Domain:** Step
* **Codomain:** Tool
* **Arity:** 2
* **Cardinality:** many-to-many

# References

