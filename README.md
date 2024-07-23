#Prolific Annotation Task Setup

## Overview

This branch documents the progress of setting up annotation tasks using a server and integrating it with Prolific. The goal is to create a reproducible process that can be customized for future projects.

## Objectives

1. **Setup Prolific Annotation Tasks**: Document the step-by-step process of setting up annotation tasks on a server and integrating them with Prolific.
2. **Customize Data Sampling**: Learn and implement methods to customize the data sampling process for participants.
3. **Remote Machine Execution**: Ensure that the entire setup can run efficiently on a remote machine.
4. **Integration with Prolific**: Seamlessly integrate the annotation server with Prolific for smooth task management and participant interaction.
5. **Reproducible Process**: Develop a custom, reproducible process that can be used for future projects.

### Upcoming Tasks

1. **Customize Data Sampling**: 
   - Learn how to customize data sampling for participants.
   - Implement the customized data sampling process.

2. **Remote Machine Execution**: 
   - Ensure that the entire setup can run on a remote machine.
   - Test the setup on different remote environments.

3. **Integration with Prolific**:
   - Finalize the integration of the annotation server with Prolific.
   - Test the integration with real tasks and participants.

4. **Reproducible Process**:
   - Document the entire setup and integration process.
   - Create templates and scripts to make the process reproducible for future projects.




# Potato Annotation Setup Guide

This guide provides step-by-step instructions for setting up Potato for annotations via Prolific.

## 1. Install Potato

To install Potato, use the following pip command:

```bash
pip install potato-annotation
```

## 2. Choose Project Schema or Examples

You can select a project schema or modify an example to fit your needs:

- [Example Projects](https://potato-annotation.readthedocs.io/en/latest/example-projects/)
- [Schemas and Templates](https://potato-annotation.readthedocs.io/en/latest/schemas_and_templates/)

## 3. Create Custom Project Folder

To create a custom project folder, execute the following commands:

```bash
mkdir project_name
cd project_name
mkdir data annotation_output configs surveyflow
```

## 4. Create a Basic Config File

Create a `config.yaml` file in the `configs` directory with the following content:

```yaml
# Name of the annotation task
annotation_task_name: "Example Name"

# Directory where Potato will write the annotation files
output_annotation_dir: "annotation_output/simple-multirate/"

# Format of the output annotation files. Allowed formats are:
# * jsonl
# * json (same output as jsonl)
# * csv
# * tsv
output_annotation_format: "csv"

# URL of the annotation codebook (if any)
annotation_codebook_url: ""

# List of data files to be used for the annotation task
data_files:
  - "data/toy-example.json"  # Input data, see allowed types here: https://potato-annotation.readthedocs.io/en/latest/data_format/

# Properties of the items in the data files
item_properties:
  id_key: "id"
  text_key: "text"

# User configuration, see here: https://potato-annotation.readthedocs.io/en/latest/user_and_collaboration/
user_config:
  allow_all_users: True
  users: []

# Alert time configuration for each instance
alert_time_each_instance: 10000000

# Choose scheme from here: https://potato-annotation.readthedocs.io/en/latest/schemas_and_templates/ e.g., "multirate"
# Here you will define the Survey layout
annotation_schemes:
  - annotation_type: "multirate"
    name: "awesomeness"  # This name gets used in reporting the annotation results
    description: "Rate this Speech"  # This text is shown to the user and can be a longer statement
    display_config:
      num_columns: 1
    arrangement: "vertical"
    options: ["Evidence Based", "Intuition based"]
    labels: ["Strongly disagree", "Disagree", "Neutral", "Agree", "Strongly Agree"]
    label_requirement:
      required: True  # Adding requirements for labels, when "required" is True, the annotators will be asked to finish the current instance to proceed
    sequential_key_binding: True  # If true, keys [1-size] will be bound to scale responses. Likert scales larger than 10 are not supported with this simple keybinding and will need to use the full item specification to bind all scale points to keys.
    option_randomization: True  # Whether randomizing the order of the options

# HTML layout configuration
html_layout: "default"

# Base HTML template configuration
base_html_template: "default"
header_file: "default"

# Directory where the actual HTML files will be generated
site_dir: "default"
```

## 5. Add Survey Flow

Add the following survey flow configuration to the `config.yaml` file:

```yaml
surveyflow:
  on: true
  order:
    - "pre_annotation"
    - "post_annotation"
  pre_annotation:
    - "surveyflow/consent.jsonl"
  post_annotation: []
  testing: []
```

## 6. Add Task Assignment

Add the following task assignment configuration to the `config.yaml` file:

```yaml
automatic_assignment:
  on: True
  output_filename: 'task_assignment.json'
  sampling_strategy: 'random'
  labels_per_instance: 3
  instance_per_annotator: 5
  test_question_per_annotator: 0
```

## 7. Integrate Potato with Prolific

Add the following Prolific integration to the `config.yaml` file:

```yaml
prolific:
  config_file_path: 'configs/prolific_config.yaml'

# Login configuration
login:
  type: 'prolific'

# UI configuration
jumping_to_id_disabled: False
hide_navbar: True
```

## 8. Create Prolific Config File

Create a `prolific_config.yaml` file in the `configs` directory with the following content:

```yaml
# Prolific API token
token: 'your prolific token'
# Prolific study ID
study_id: 'your study id'
# Maximum number of concurrent sessions
max_concurrent_sessions: 3
# Workload checker period in seconds
workload_checker_period: 60
```

## 9. Create Server Directory and Open Port

Set up the server directory and open the required port.

## 10. Run Project

Run the project with the following command (select the main config file when asked):

```bash
potato run project_name -p8000
```

## 11. Update Server Address on Prolific

Update the server address on Prolific as described in the [Potato Crowdsourcing Guide](https://potato-annotation.readthedocs.io/en/latest/crowdsourcing/).

---

This completes the setup guide for using Potato for annotations via Prolific. If you have any issues, refer to the [Potato documentation](https://potato-annotation.readthedocs.io/) for further assistance.



