###############################################################################
                            PYTHON TRAINING COURSE
###############################################################################

This is the training course to master the Python programming language. This
includes the very basics programming topics like data types, variables and
object-oriented programming and advanced topics like multithreading and
web-frameworks for Python.

Getting started
===============

Getting the sources
-------------------

Current repository is dependent on some other repos (is **multi-repo**).
You may clone it, but this would not download the source of other repos.
To get the full content, do:

.. code-block:: shell

    git clone <this_repo_url> [local_repository]
    # cd into cloned repo and switch to devel branch
    git switch devel
    # clone submodules content
    git submodule update --init --recursive

Installing dependencies
-----------------------

This project comes with dependencies listed in formats suitable for `pip`_
and `poetry`_ package managers. It's recommended to use virtual environment
while working with this project.

To install dependencies via pip do:

.. code-block::

    pip install -r requirements.txt

To do the same using poetry:

.. code-block::

    poetry install

.. _pip: https://pip.pypa.io
.. _poetry: https://python-poetry.org

Building documentation
======================

This repository comes with *Makefile*. For now this is the easiest way to do
anything within this source code. There are several targets defined:

-   ``clean`` will clean up the documentation builds
-   ``html`` will create HTML pages
-   ``locales`` will create/update translations

Just do:

.. code-block:: shell

    make <target>  # e.g. make html

By default all builds are done for english locale.
Define ``LANGUAGE`` environment variable to build documents for other locales.

.. code-block:: shell

    LANGUAGE=uk make html

Using ``make`` without arguments (targets) will clean up documentation and
build HTML pages for *en* and *uk* locales.

.. code-block:: shell

    make

This command is equal to:

.. code-block:: shell

    make clean
    LANGUAGE=en make html
    LANGUAGE=uk make html

Building without make
---------------------

``make`` just automates the build process, however in case you don't have it
installed, you can run builds manually:

.. code-block:: shell

    sphinx-build -b html -D language=en src _build/html/en
    sphinx-build -b html -D language=uk src _build/html/uk

Building with MkDocs (New)
===========================

The project is being migrated to MkDocs with Material theme. MkDocs provides
a modern documentation experience with Markdown authoring.

Prerequisites
-------------

- Python 3.9 or higher (MkDocs setup is regularly tested on Python 3.11+)
- `uv`_ (Python package manager)

.. _uv: https://docs.astral.sh/uv/

Local Development
-----------------

.. code-block:: shell

    # Clone repository
    git clone https://github.com/OpenRoost/pymastery-vp.git
    cd pymastery-vp

    # Install dependencies
    uv sync

    # Serve documentation locally (with live reload)
    uv run mkdocs serve

    # Open http://127.0.0.1:8000 in your browser

Building for Production
------------------------

.. code-block:: shell

    # Build static site
    uv run mkdocs build

    # Output will be in ./site/ directory

Project Structure
-----------------

::

    content/
      en/           # English course content (Markdown)
      uk/           # Ukrainian translations (in progress)
      ru/           # Legacy Russian content (historical)
    mkdocs.yml      # MkDocs configuration
    pyproject.toml  # Project dependencies
    src/            # Legacy Sphinx source (being phased out)

Contributing to the project
===========================

This is an open-source project, so anyone is welcome to contribute to it.
Please see `contributing guide <./.github/CONTRIBUTING.md>`_ for more details.
