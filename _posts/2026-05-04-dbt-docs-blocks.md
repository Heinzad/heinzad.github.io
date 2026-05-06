---
title: "dbt docs blocks"
author: "Adam Heinz"
date: 2026-05-04T12:00:00-00:00
categories:
  dbt
tags:
  - docs
---
How to use docs-blocks in dbt.  


# Overview

If a description is repeated in multiple places, it can be written once in a docs-block and read everywhere it it used. 
More than one docs-block can be stored in a markdown file, so each file can collate all related information on a single topic. 

## Getting started

1. Create a subdirectory named "docs" in your project tree.
2. To the resource paths listed in the `dbt_project.yml` file, add:  

*yaml:*   
```
docs-paths: ["docs"]
```

A typical directory structure may now be: 

```
project/
├── analysis/
├── docs/
├── macros/
├── models/
├── snapshots/
├── seeds/
└── dbt_project.yml
```

## docs blocks

A docs block is identified with its opening and closing declarations

*markdown jinja:*  

{{ "{{ "{% docs Lorem %}" }}  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.  
{{ "{{ "{% enddocs %}" }}  
  
{{ "{{ "{% docs Excepteur %}" }}  
Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.  
{{ "{{ "{% enddocs %}" }} 
 


## doc references 

The docs block may now be referenced in any schema file. 

The syntax depends on the usage for single or multiple-line descriptions. 

*yaml:*  
```
models:
  - name: eg
    description: This model is an example

    columns:
      - name: eg1
        description: '{{ doc("LoremIpsum") }}'
      - name: eg2
        description: Prefixed {{ doc("LoremIpsum") }}
      - name: eg3
        description: >
         MultiLine {{ doc("LoremIpsum") }}
         {{ doc("Excepteur") }}
```

The schema files now simply map to the docs blocks. 

## Outcome 

dbt will add the descriptions to the target object when built. 


## Further Reading 
 
- dbt (4 May 2026), [About doc function](https://docs.getdbt.com/reference/dbt-jinja-functions/doc?version=1.12), accessed May 2026. 
- dbt (4 May 2026), [About documentation](https://docs.getdbt.com/docs/build/documentation?version=1.12), accessed May 2026.
- dbt (4 May 2026), [docs-paths](https://docs.getdbt.com/reference/project-configs/docs-paths?version=1.12), accessed May 2026. 
- Mikael Thorup, 17 May 2023, [Accelerate your documentation workflow: Generate docs for whole folders at once](https://docs.getdbt.com/blog/generating-dynamic-docs-dbt?version=1.12), accessed May 2026.


QED 

© Adam Heinz 

6 May 2026
