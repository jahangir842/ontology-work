# Build a small HR ontology yourself in Protégé

This is a learning exercise. You will do the editing in Protégé and save your own `.owl` file. Use fictional employees and departments; do not enter real personnel data.

## 1. Decide what the ontology must answer

An ontology is a description of concepts and their relationships. Before opening the editor, write down a few questions you want it to answer. These are often called **competency questions**.

For this first version, use:

1. Which people are employees?
2. Which employees are managers?
3. Which department does an employee work in?
4. To whom does an employee report?

These questions set the scope. Payroll, attendance, leave approval, and recruitment can wait until this small model works. If you later add one of those topics, first write a question it should answer.

## 2. Choose a starting point

**Recommended path: inspect an example, then build your own HR ontology.** Protégé's [getting-started guide](https://protegeproject.github.io/protege/getting-started/) shows how to open its Pizza ontology with **File → Open from URL**. Spend a few minutes finding its class hierarchy, properties, and individuals. You do not need to understand the whole example. Then use **File → New** for the HR project.

You may instead open a ready-made ontology if your goal is only to learn the Protégé interface. For an HR learning project, building this small model yourself makes the thought process visible. Reuse an existing HR ontology later if it answers your questions and you understand its definitions and reuse terms.

## 3. Plan the model on paper

Use three kinds of building blocks:

| Kind | Meaning | First HR terms |
| --- | --- | --- |
| Class | A type of thing | `Employee`, `Manager`, `Department` |
| Object property | A relationship between two things | `worksIn`, `reportsTo` |
| Data property | A literal value such as text | `employeeId`, `fullName` |

Draw this small class hierarchy:

```text
owl:Thing
├── Employee
│   └── Manager
└── Department
```

`Manager` is a subclass of `Employee` because every manager in this model is an employee. `Department` is separate: it is an organizational unit, not a person. A named person such as `Ayesha` is an **individual**, not a subclass. A job title may eventually need its own class, but only add it when a question requires it.

Before creating each term, ask: “Is this a type, a relationship, a value, or a particular example?” That question prevents the most common modeling mix-ups.

## 4. Create the ontology in Protégé Desktop

These instructions follow the [Protégé Desktop documentation](https://protegeproject.github.io/protege/). Menu placement can vary slightly by version.

1. Open Protégé and choose **File → New**.
2. Set the ontology IRI to `https://example.org/hr-ontology` or another identifier you control. The IRI identifies the ontology; this exercise does not require a website at that address.
3. Choose **File → Save As** and save the file as `hr-ontology.owl` in this repository. You are creating this file yourself as you follow the guide.

An `.owl` file is your ontology. Protégé is the editor used to create and inspect it.

## 5. Add the classes

1. Open the **Entities** or **Classes** tab and look for the **Class hierarchy**.
2. In the **Asserted** hierarchy, select `owl:Thing` and add subclasses `Employee` and `Department`.
3. Select `Employee` and add subclass `Manager`.
4. Confirm that your hierarchy matches the diagram in section 3.

Use the add-subclass button or right-click a class and select **Add subclass**. The [class hierarchy guide](https://protegeproject.github.io/protege/views/class-hierarchy/) shows these controls.

**Think:** Would you put `HRDepartment` under `Department` as a subclass? For this exercise, no: it is one particular department, so it will be an individual. A class such as `SalesDepartment` would make sense only if you need a reusable type of department.

## 6. Add relationships and values

In the **Object properties** tab, add:

| Property | Domain | Range | Meaning |
| --- | --- | --- | --- |
| `worksIn` | `Employee` | `Department` | An employee works in a department |
| `reportsTo` | `Employee` | `Manager` | An employee reports to a manager |

In the **Data properties** tab, add:

| Property | Domain | Range | Example value |
| --- | --- | --- | --- |
| `employeeId` | `Employee` | `xsd:string` | `E001` |
| `fullName` | `Employee` | `xsd:string` | `Ayesha Khan` |

Select a property and use its description panel to add the domain and range. [Object property](https://protegeproject.github.io/protege/views/object-property-description/) and [data property](https://protegeproject.github.io/protege/views/data-property-hierarchy/) documentation can help you find the views.

**Think:** Domain and range are logical statements, not form-field restrictions. If you assert `Bilal reportsTo Ayesha`, the `reportsTo` range lets a reasoner infer that `Ayesha` is a `Manager`. Add a range only when that inference is always true in your intended HR system. For example, change the range to `Employee` if workers can report to someone who is not a manager in your model.

## 7. Add a tiny fictional example

Create these **individuals** in the **Individuals** tab:

| Individual | Type | Data values |
| --- | --- | --- |
| `Ayesha` | `Manager` | `employeeId` = `E001`; `fullName` = `Ayesha Khan` |
| `Bilal` | `Employee` | `employeeId` = `E002`; `fullName` = `Bilal Ali` |
| `HRDepartment` | `Department` | none needed |

Add these object property assertions:

```text
Ayesha worksIn HRDepartment
Bilal worksIn HRDepartment
Bilal reportsTo Ayesha
```

To create an individual, select its class and use **Add Individual**. Then select the individual to add its property values. See Protégé's [individuals guide](https://protegeproject.github.io/protege/views/instances/).

**Think:** `Bilal` is a person in your example data. `Employee` is the reusable type. If you remove Bilal later, the concept of employee remains.

## 8. Run the reasoner and review your answers

1. Choose **Reasoner → HermiT**, then **Reasoner → Start Reasoner** (or press `Ctrl+R`).
2. Check for any inconsistency reported by Protégé.
3. Check that `Ayesha` counts as an `Employee` because `Manager` is a subclass of `Employee`.
4. In the individuals and property views, check that the facts you entered answer the four questions in section 1.
5. Save the ontology and close and reopen it once to confirm the file can be loaded.

The [getting-started guide](https://protegeproject.github.io/protege/getting-started/) explains HermiT and how to inspect inferred information. If an inferred result surprises you, inspect the subclass, domain, and range statements that produced it.

## 9. What counts as finished?

Your first version is finished when you can explain the meaning of each class and property, show the three fictional individuals in Protégé, answer the four questions, run the reasoner without inconsistency, and reopen `hr-ontology.owl` successfully.

Write a short note for yourself after the exercise:

```text
My ontology answers: ...
I made Manager a subclass of Employee because: ...
I chose the reportsTo range because: ...
One thing I would add next is: ...
```

## Where Protégé fits, and alternatives

Protégé Desktop is useful here because its visual class and property editors let you focus on meaning while its reasoner checks logical consequences. [WebProtégé](https://protege.stanford.edu/software/) offers browser-based collaborative OWL editing. [VocBench](https://vocbench.uniroma2.it/) is another collaborative web editor for ontologies and vocabularies. [TopBraid EDG](https://www.topquadrant.com/doc/8.5/user_guide/guidance_specific_to_asset_collection_type/working_with_ontologies/ontology_editor_panels.html) supports ontology editing as part of a wider data governance platform.

An ontology reasoner checks logical consistency and derives facts. It does not replace HR application validation, such as requiring an employee ID on a form. Keep this first ontology about the meaning of HR concepts and relationships.
