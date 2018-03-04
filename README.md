# COPS - Collaborative Open Playbook Standard

This repository contains schema definitions for a DFIR (Digital Forensics Incident Response) Playbook.
The scheme is based on YAML (http://yaml.org/), and describes an incident response runbook (aka. playbook, “use case”) that is a written guidance for identifying, containing, eradicating and recovering from cyber security incidents.

Yaml was chosen since it's both human readable and can describe complex nested data structure, we use YAML version 1.2.

## What is the promise of the DFIR Playbook Spec?
* **Open** - open sourced, so analysts can create, share and contribute incident response process in same language.
* **Semi/Fully automated** - since playbook in the incident response world often must have some manual steps, but sometimes can be fully automated.
* **Visibility** - give the organization members (SOC team, management) an overview on the incident response process.

## Version
This is version 0.1 of the spec

## Playbook Hierarchy structure:
1. Playbook - the high level process.
2. Task - this is a single step in the process, can represent a script execution or manual step.

## Playbook fields

* **id**: a unique id of the playbook, usually UUID
* **name**: playbook name
* **description**: the purpose of the playbook
* **tasks**: an (orderd) list of playbook tasks

## Task fields
* **id**: this is the id of the task inside the playbook, it must by unique in playbook level only
* **taskid**: this is the global task id, should be unique globally (usually UUID), needed in order to share task between playbooks
* **type**: one of the three title (represent a new playbook section/header), regular (script or manual task) or condition (to decide what is the next branch/step)
* **name**: name of the task
* **description**: the purpose of the task
* **script**: if this is an automated task, the script to execute for this task
* **tags**: general tags to add to task
* **condition**: if this task is condition type, this fields will hold a nested map of string keys that map to branch's (list of tasks), so this task can continue to correct branch according to result of script
* **scriptarguments**: these are the task inputs, should be a map of string (input name) to array (input value or identifier)
* **results**: script results that we can propagate to the rest of the playbook execution

### Example playbook Yaml:

``` yaml
id: 40202fbb-9ed4-4b8f-86e1-68722d808e3d
version: 3
name: Hello-world-COPS
starttaskid: "0"
tasks:
  "0":
    id: "0"
    taskid: c44160b9-16d8-4a1e-8765-1c034006a183
    type: start
    task:
      id: c44160b9-16d8-4a1e-8765-1c034006a183
      version: -1
      name: ""
      iscommand: false
      brand: ""
    nexttasks:
      '#none#':
      - "1"
    separatecontext: false
  "1":
    id: "1"
    taskid: 015bd0d8-5d01-4c2d-8d38-fed3e5c77938
    type: regular
    task:
      id: 015bd0d8-5d01-4c2d-8d38-fed3e5c77938
      version: -1
      name: Hello world COPS
      scriptName: Print
      type: regular
      iscommand: false
      brand: ""
    nexttasks:
      '#none#':
      - "2"
    scriptarguments:
      value:
        simple: Hello DFIR community, this is COPS!
    separatecontext: false
  "2":
    id: "2"
    taskid: b8193b45-293e-4035-858a-36d84050395a
    type: condition
    task:
      id: b8193b45-293e-4035-858a-36d84050395a
      version: -1
      name: Is this incident high severity
      type: condition
      iscommand: false
      brand: ""
    nexttasks:
      '#default#':
      - "4"
      "yes":
      - "3"
    separatecontext: false
    conditions:
    - label: "yes"
      condition:
      - - operator: string.isEqual
          left:
            value:
              simple: incident.severity
            iscontext: true
          right:
            value:
              simple: "3"
  "3":
    id: "3"
    taskid: 11e43d6c-a9bb-4fea-8641-6f256a5d11f7
    type: regular
    task:
      id: 11e43d6c-a9bb-4fea-8641-6f256a5d11f7
      version: -1
      name: Investigate it!
      type: regular
      iscommand: false
      brand: ""
    separatecontext: false
    sla:
      hours: 0
      days: 0
      weeks: 1
  "4":
    id: "4"
    taskid: eb04267c-b749-40f7-888b-b00c720112ea
    type: regular
    task:
      id: eb04267c-b749-40f7-888b-b00c720112ea
      version: -1
      name: Go Sleep
      type: regular
      iscommand: false
      brand: ""
    separatecontext: false
inputs: []
outputs: []
```

This is of course a sample (and simple example) just to show an overview of the scheme.
For real DFIR playbooks look at the [Demisto content repo](https://github.com/demisto/content).

Feel free to contribute by providing feedback, creating new DFIR playbooks, or using the spec in your security product, contact using issues of this GitHub repo.
