---
description: Configure Python and test the Ingestion Framework
---

# Ingestion Framework

## Prerequisites

The Ingestion Framework is a Python module that wraps the OpenMetadata API and builds workflows and utilities on top of it. Therefore, you need to make sure that you have the complete OpenMetadata stack running: MySQL + ElasticSearch + OpenMetadata Server.

To do so, you can either build and run the [OpenMetadata Server](openmetadata-server.md) locally as well, or use the `metadata` CLI to spin up the [Docker containers](../../../overview/run-openmetadata/).

## Python Setup

We recommend using `pyenv` to properly install and manage different Python versions in your system. Note that OpenMetadata requires Python version +3.8. This [doc](https://python-docs.readthedocs.io/en/latest/dev/virtualenvs.html) might be helpful to set up the environment virtualization.

### Generated Sources

The backbone of OpenMetadata is the series of JSON schemas defining the Entities and their properties.

All different parts of the code rely on those definitions. The first step to start developing new connectors is to properly set up your local environment to interact with the Entities.

In the Ingestion Framework, this process is handled with `datamodel-code-generator`, which is able to read JSON schemas and automatically prepare `pydantic` models representing the input definitions. Please, make sure to run `make install_dev generate` from the project root to fill the `ingestion/src/metadata/generated` directory with the required models.

Once you have generated the sources, you should be able to run the tests and the `metadata` CLI. You can test your setup by running `make coverage` and see if you get any errors.

### Quality tools

When working on the Ingestion Framework, you might want to take into consideration the following style-check tooling:

* [pylint](https://www.pylint.org) is a Static Code Analysis tool to catch errors, align coding standards and help us follow conventions and apply improvements.
* [black](https://black.readthedocs.io/en/stable/) can be used to both autoformat the code and validate that the codebase is compliant.
* [isort](https://pycqa.github.io/isort/) helps us not lose time trying to find the proper combination of importing from `stdlib`, requirements, project files…

The main goal is to ensure standardized formatting throughout the codebase.

When developing, you can run these tools with `make` recipes: `make lint`, `make black` and `make isort`. Note that we are excluding the generated sources from the JSON Schema standards.

If you want to take this one step further and make sure that you are not committing any malformed changes, you can use [pre-commit hooks](https://pre-commit.com). This is a powerful tool that allows us to run specific validations at commit time. If those validations fail, the commit won't proceed. The interesting point is that the tools are going to fix your code for you, so you can freely try to commit again!

You can install our hooks via `make precommit_install`.

#### Tooling Status

We are currently using:

* `pylint` & `black` in the CI validations, so make sure to review your PRs for any warnings you generated.
* `black` & `isort` in the pre-commit hooks.

