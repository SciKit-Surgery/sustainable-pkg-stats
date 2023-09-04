sustainable-pkg-stats
=====================

.. image:: https://img.shields.io/twitter/url?style=social&url=http%3A%2F%2Fscikit-surgery.org
   :target: https://twitter.com/intent/tweet?screen_name=scikit_surgery&ref_src=twsrc%5Etfw
   :alt: Get in touch via twitter

.. image:: https://img.shields.io/twitter/follow/scikit_surgery?style=social
   :target: https://twitter.com/scikit_surgery?ref_src=twsrc%5Etfw
   :alt: Follow scikit_surgery on twitter

This is a set of scripts to get statistics on the scikit-surgery library
and turn them into a nice webpage

.. image:: https://github.com/scikit-surgery/sustainable-pkg-stats/raw/master/assets/screenshot.png
    :width: 400px
    :target: http://scikit-surgery.github.io/sustainable-pkg-stats/
    :alt: Link to the dashboard



Sustainability dashboard template for community
================================================

We provide a repository template for creating a sustainability dashboard for your own library/ecosystem of interest.
It automatically creates the scripts and files needed to run the analysis needed for deployment of dashboard
showing metrics of libraries existing within a given base Python package/ecosystem.
It includes the Github Action that deploys the produced html files to the `gh-pages` branch of a target repository,
which triggers a deployment every 12 hours, using the `cron` scheduler.

Using the template in local machine
===================================

1. Create conda or mamba environment with `cookieninja <https://libraries.io/pypi/cookieninja>`__ which is part of requirements.
.. code-block::
    conda create -n susdbVE pip -c conda-forge
    conda activate susdbVE
    pip install -r requirements.txt

2. Run cookieninja in the desired location
.. code-block::
    cookieninja gh:scikit-surgery/sustainable-pkg-stats

If you have this repo locally (this may be the case if you are developing, or you cloned this repository before), you can alternatively run the following:
.. code-block::
    cookieninja /path/to/your/checkout/of/python-template

3. A series of questions will pop up to configure the project.
Type the answer or hit return to use the default option (shown in square brackets).
Note that these project variables are defined in the `cookiecutter.json` file.

..  tip::
    It is crucial you enter a value for `base_library_name` as the dashboard analysis scripts will be configured for this base package. There is a
    script the cookieninja runs placed under `hooks/pre_gen_project.py` that checks if the name given returns package entries in pypi search.

.. code-block::
    author_name [John Smith]:
    author_email [temp@gmail.com]:
    project_name [Community Dashboard]:
    project_slug [dashboard_for_scikit-surgery]:
    base_library_name [scikit-surgery]:
    project_short_description [A dashboard template from scikit-surgery]:
    funder [JBFC: The Joe Bloggs Funding Council]:
    Select licence:
    1 - MIT
    2 - BSD-3
    3 - GPL-3.0
    Choose from 1, 2, 3 [1]:



4. This will create a directory with the following configuration:
For example, for a project with the following variables:
.. code-block::
    project_name : Community Dashboard
    base_library_name : scikit-surgery

We will get a project folder named after `dashboard_for_scikit-surgery`, structured like this:
.. code-block::
    ├── assets
    │   └── logo-dashboard.svg
    ├── _config.yml
    ├── get_badges.py
    ├── get_github_repos.py
    ├── get_loc.py
    ├── get_pypi_repos.py
    ├── html
    │   ├── dashboard.html
    │   ├── dashboard.html.in.head
    │   ├── dashboard.html.in.tail
    │   ├── excluded.html.in.head
    │   ├── excluded.html.in.tail
    │   └── exclusions.html
    ├── index.html
    ├── libraries
    │   ├── exclusions
    │   └── lines_of_code
    ├── LICENSE
    ├── loc
    │   ├── CMakeCatchTemplate.html
    │   └── PythonTemplate.html
    ├── pypi-simple-search
    ├── README.md
    ├── requirements.txt
    ├── sksurgerystats
    │   ├── common.py
    │   ├── from_github.py
    │   ├── from_pypi.py
    │   ├── html.py
    │   ├── __init__.py
    │   ├── __pycache__
    │   │   ├── common.cpython-310.pyc
    │   │   ├── html.cpython-310.pyc
    │   │   └── __init__.cpython-310.pyc
    │   └── pypi_downloads.py
    ├── static
    │   └── loc_plot.js
    ├── templates
    │   ├── dashboard.css
    │   └── loc_plot.html
    ├── tests
    │   ├── conftest.py
    │   └── test_template_workflow.py
    ├── update_dashboard.py
    ├── update_github_stats.py
    └── update_pypi_stats.py



Important configurations to note:

   1.  `get_github_repos.py` and `get_pypi_repos.py` will take `base_library_name` as the base name to search packages in `https://pypi.org/search/` and github

   2.   `project_name` will appear in the README.md as the human-readable name of the project.

   3.   `html/dashboard.html` will take `project_name` as the main title, Community Dashboard, and also use `project_slug` for a description below the logo, as shown below:

.. image:: assets/header_cookieninja_template.png
   :width: 400
   :alt: Dashboard header for the given example

5. To run the pipeline, you first need to install the dependencies using the `requirements.txt` file installed via step 3.
.. code-block::
    pip install -r requirements.txt

6. To run the analysis scripts, test locally, you need a personal access token for Github API generated from `here <https://github.com/settings/personal-access-tokens/new>`__

+ Save it in the base directory under a text file named `github.token`

7. Few [optional] things to set before you can run the pipeline!

    a. You can specify a list for the libraries you want to exclude from your dashboard deployment, under `libraries/exclusions`

        Similar to `libraries` folder, this (as shown below) has a dict entry for each package, such as in this example from `scikit-surgery`:
            | libraries/exclusions
            | ├── scikit-surgeryoverlay
            | ├── scikit-surgerytorsosimulator
            | └── scikit-surgeryvideoutils

        Each file entry (ex. scikit-surgeryoverlay) is a `.json` file that has :
        an `obsolete` key and a value that is a sentence describing why they are obsolete, such as:
        ```{"obsolete" : "Became <a href='https://github.com/UCL/scikit-surgeryvtk'>sikit-surgeryvtk.</a>"}```

    b. You can save the logo of your base package (a .svg file) under `assets/logo-dashboard.svg` for it to show up in your deployment header

8. ESSENTIAL: Github Configurations
    a. You need to initialise github pages in your repository and set the deployment source from branch `gh-pages` :
        Github Action will automatically initialise this branch and deploy from
        here. You can find the instructions
        `here <https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site>`__

        You might need admin rights from your organisation to use your organisation's base name. You can also use your username as the domain.

        Your configuration will need to look like this (In the example below, our domain name is the `scikit-surgery` organisation):

.. image:: assets/github_pages_configuration.png
   :width: 500
   :alt: Configuration

b. You need a secret personal token to use the github API in the Github Action workflow, saved as `secrets.ADMIN_TOKEN`. For this you
will need admin rights in your organisation and repository. You can read more on secret Github tokens
`here <https://docs.github.com/en/actions/security-guides/encrypted-secrets`__

    1. Go to the Settings
    2. Go to Security -> Actions -> Repository secrets
    3. Add a key named `ADMIN_TOKEN` and the token you created at step 6.

    This is the same type of token you saved locally in Step 6. Yo should never
    version control/track this token in your remote repository,  so here we are creating
    a field for it which Github Action can reference in deployment.

9. Running the pipeline

The Github Actions workflow will run this pipeline, so you do not need to do anything. But locally, you can check if the pipeline works correctly,
by running the python scripts ordered and referenced in the `Makefile` file of this repository.

Note for checking if things work properly:
- while running `get_badges.py` you should notice that under `libraries` folder, there are .json files of dictionary entries for each package


Instructions for developers
===========================

Clone repository
----------------
(Optional) Generate your SSH keys as suggested
`here <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>`_
(Optional) GitHub CLI as suggested
`here <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account?tool=cli>`_
Clone the repository by typing (or copying) the following line in a terminal at your selected path in your machine:
.. code-block::
    git clone git@github.com:SciKit-Surgery/sustainable-pkg-stats.git

Creating and activating the environment either with venv or conda
-----------------------------------------------------------------
Using conda
.. code-block::
    conda create -n susdbVE pip -c conda-forge
    conda activate susdbVE
    pip install -r requirements.txt

Using venv
.. code-block::
    mkdir env
    python -m venv env/
    source env/bin/activate
    pip install -r requirements

Token for Github API
--------------------
Make sure you have a personal access token for Github API generated from `here <https://github.com/settings/personal-access-tokens/new>`_
    and is saved in the base directory under a file named `github.token`

Running the pipeline
--------------------
Running the pipeline that generates dashboard.html and associated files needed by Github Pages
.. code-block::
    bash Makefile

You can also run the individual python scripts to check outputs:

Search for relevant packages on pypi and githib
.. code-block::
    python get_pypi_repos.py
    python get_github_repos.py

update stats
.. code-block::
    python update_pypi_stats.py
    python update_github_stats.py

get coverage/docs/etc badges
.. code-block::
    python get_badges.py

update html files
.. code-block::
    python update_dashboard.py

Inspect libraries with pypi
.. code-block::
    ./pypi-simple-search scikit-surgery > scikit-surgery-onpypi.txt
    python get_github_repos.py > scikit-surgery-ongithub.txt

We can use pypinfo to get data for things on pypi
.. code-block::
    pypinfo --auth snappy-downloads-3d3fb7e245fd.json
    pypinfo scikit-surgeryvtk country

