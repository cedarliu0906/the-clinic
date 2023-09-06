# Repo information

This repository contains notes and tooling around implementing coding standards on repositories as part of the data science clinic and related projects. This repository also includes a few scripts which analyze repositories and code using automation.

Information on the specific tests and automation can be found in the scripts directory README file.

There are currently three README files in the `docs` directory, all of 

# Coding standards for UChicago DSI

Motivation:
---
Much of the code that is produced here at the DSI is 80% complete and not in a state where it can be easily turned over. The purpose of this document is to provide a set of best practices _in checklist form_ so that we can quickly do code reviews and provide expectations on them.

We should always keep the following in mind: Analysis is _useless_ without a good repo.

Principles:
---
1. Automation
    -- All tasks should be automated
1. Reproducability
    -- All results should be reproducible
1. Documentation
    -- All tasks and results should be clearly documented
1. Data chain of ownership
    -- Data should have an obvious chain from source to result

Requirements
---
### Structure
1. Directory structure and naming should be obvious and easy to understand.
1. File names and directories should be useful:
    * There should be no ``v2`` or dates or people's name in filenames.
    * Spaces, punctuation marks, and parenthesis should not be in any file or directory names.
1. `.gitignore` should be used to avoid committing data and intermediate data files which are not appropriate for the repo.
    * There should be no `DS_Store` files or `.ipynb_checkpoints` directories.
    * You should start with the default python `.gitignore` from GitHub.
    * Make sure that there are not unnecessary files in the repo. If they are generated by the code, put them in `.gitignore`.
1. Secrets and API Keys should not be in the repository.
1. All file paths should be relative.
1. Bash scripts:
    * Should be set as executable (`chmod +x`)
    * Should end with `.sh`
    * Should begin with ```#!/bin/bash```
    * Should also have ```set -e```

### Code Quality
1. Function names should be descriptive.
1. No commented-out code
1. Code should be organized so that function definition is separate from execution.
1. Code should never silently break (such as using try/except without raising an error.)
1. All code should pass Pyflakes.
1. The code formatter `bloack` should be used for readability.

### Notebooks
1. Notebooks should _not_ contain function definitions.
1. Notebooks should have less than 10 cells and all cells should be 15 lines of code or less.
1. Notebooks should have documentation (preferably markdown) which describes the purpose of them.
1. There should be no `! pip install XXX` in any notebooks. All environment requirements should be handled using a `requirements.txt` file.
1. Documentation should include (at a _minimum_):
    * Doc strings on all functions
    * README files in directories specifying the contents.
    * README file in the root directory describing the purpose of the code, where to look for things, and how to run the code. If there are other locations for information regarding this project, links should be provided.
    * README file should describe your development process (e.g., how you did branches)

### Dependency Management
1. The following python libraries are banned unless given explicit permission:
    * `subprocess` or `subprocess` like library
1. All non-standard python libraries need to be justified:
    * If asked why you used library X, there needs to be a good answer.

### Git
1. Working branches need to be up to date with _main_ upon completion of task/code review and should not stray behind _main_ for more than a day.

### Docker
1. Repos should contain a Dockerfile:
    * Clear Instructions on how to run the code (via docker) in the main README.md file.
    * All code in the repo should be executable via docker.
    * The Dockerfile should use a `requirements.txt` to manage modules and should have versions on all modules.
    * There should be _no_ conda / pyenv etc.

### DSI Cluster
1. Include a conda recipe to manage the environment.


Suggested best practices:
---
1. Github
    * Branches should be merged into main and then deleted
1. Linting
    * All code should pass basic linting
1. Github actions
    * For linting and testing

FAQ
---

#### How will testing of the above be done?

On a code review day, the repo will be cloned, the dockerfile built, and the notebooks run. The notebooks will be inspected for the practices above. While the main branch will be the focus, each branch will be looked at to determine how far astray of the main it is.

#### How do I handle output images or tables?

Use an `\output` directory to put in images and other results.

#### If I can't put functions in notebooks, where should they go?

Functions should be put in a `utils` directory and loaded via import.

#### How should I document notebooks?

Notebooks, just like any other piece of code need to be well documented and readable. Some questions that we ask when evaluating:
* Does the notebook begin with a title, byline, date, and summary/description? 
* Is its content logically organized into sections with headers? 
* Does it walk the viewer through what the code is doing and why using both Markdown and comments, and in clear language?  
* Is cell output formatted for easier viewing (e.g., to avoid scrolling)? Are there any cells that were obviously used for testing/scratchwork and have not yet been removed? Are numbers rounded for display purposes?
* Are Python module imports located together near the top of the notebook, following PEP 8, rather than scattered throughout many cells? 
    * Are all cells 15 lines of code or less? 
    * Do notebooks have less than 10 cells?
    * Are `pyflakes` and `black` being run on the code for standardization.

#### What about Docker README.md information?

This example is a good starting point. Replace `project-name` with your project name.

```plaintext
This repository contains a basic dockerfile that will run a jupyter notebook instance. To build the docker image, please type in:

docker build . -t [project-name]

Note that the image name in the above command is drw

To run the image type in the following:

docker run -p 8888:8888 -v ${PWD}:/tmp [project-name]

as you can see we are running the [project-name] image.
```
#### What is a good folder structure?

For the simplest projects something like the below should work:
```
.
├── README.md
├── .gitignore
├── Dockerfile
├── notebooks/
│   ├── README.md
│   └── notebook Files
├── utils/
│   ├── README.md
│   ├── __init__.py
│   └── python utility files
├── data/
│   ├── README.md (or SOURCES.md)
│   └── Data files
├── output/
│   ├── README.md
│   └── output images, tables, etc.
└── documentation/
    └── README.md
```

#### What if I'm doing work outside of traditional python code?

If you are doing work outside of python it still requires documentation. There should be zero work that isn't shareable.
