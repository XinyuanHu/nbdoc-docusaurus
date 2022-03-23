# Create Testable, Reproduceable Docs and Blogs With Notebooks

> Never copy and paste code into documentation again!

👉 [See a live example](https://outerbounds.github.io/nbdoc-docusaurus/docs/nb) of a post made with notebooks 🌐 🚀

<a id="markdown-background" name="background"></a>

## Background

This repo provides an example of how to create blog posts and/or documentation with [Docusaurus](https://docusaurus.io/) using Jupyter Notebooks.  When you create documentation with notebooks:

- Your **documentation is fully testable** with unit tests
- **No more copy / pasting code** into your blog posts and docs
- **The output of code is always up to date**, since it is generated by actually running  code, rather than copying and pasting it from somewhere else.
- You can **author technical posts and docs in a WYSIWYG manner**, which allows you to iterate faster.
- You can still write markdown posts if you want to.


<!-- TOC -->

- [Background](#background)
- [Getting Started](#getting-started)
    - [1. Fork this repo](#1-fork-this-repo)
    - [2. Edit `settings.ini`](#2-edit-settingsini)
    - [3. Install Dependencies](#3-install-dependencies)
- [Using Notebooks](#using-notebooks)
    - [Notebook Development Setup](#notebook-development-setup)
    - [Tutorial](#tutorial)
        - [Running Tests & Updating Notebooks](#running-tests--updating-notebooks)
        - [Skipping tests in cells](#skipping-tests-in-cells)
        - [Update all notebooks](#update-all-notebooks)
        - [Ignoring Skip Flags](#ignoring-skip-flags)
    - [Hotkeys For Jupyter](#hotkeys-for-jupyter)
- [Using Markdown](#using-markdown)
- [Running the documentation locally](#running-the-documentation-locally)
- [Automatic publishing](#automatic-publishing)
- [About](#about)

<!-- /TOC -->


<a id="markdown-getting-started" name="getting-started"></a>

## Getting Started

This project assumes some familiarity with static site generators.  It is also very helpful to gain some familiarity with [Docusaurus](https://docusaurus.io/) and it's general features.

<a id="markdown-1-fork-this-repo" name="1-fork-this-repo"></a>

### 1. Fork this repo

To create your own blog or docs site, you can start by forking this repo, or creating a new repo by using this one as a [repository template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository).

This repo has both blog and docs, but you might want to enable [blog only mode](https://docusaurus.io/docs/blog#blog-only-mode) or [docs only mode](https://docusaurus.io/docs/docs-introduction#docs-only-mode)


<a id="markdown-2-edit-settingsini" name="2-edit-settingsini"></a>

### 2. Edit `settings.ini`

After that, you should change the follwing lines in the [settings.ini](settings.ini) file.  The keys are described below:

```diff
[DEFAULT]
nbs_path = .
recursive = True
tst_flags = notest
-user = outerbounds
+user = <your github username>
-doc_host = https://outerbounds.github.io
+doc_host = https://<your github user name>.github.io or your custom domain
-doc_baseurl = /nbdoc-docusaurus
+doc_baseurl = /<name of your repo> or path
-module_baseurls = metaflow=https://github.com/Netflix/metaflow/tree/master/
+module_baseurls = <your python module>=https://github.com/a/path/tree/master/
```

The definintions of these fields are as follows:

- **`nbs_path`:** the top most in your directory that contains notebooks to be converted to markdown files.  The default value for this `.`, which just means the root of the repo.
- **`recursive`:** set to `True` if you want to recurisvely find all notebooks under `nbs_path`, and `False` otherwise.  Defaults to `True`
- **`tst_flags`:** special cell comment that will allow yout skip cell execution and test.  For example, adding the comment `#notest` in a code cell would result skip cell execution for that cell.  You can add mulitple values in this field seperated by a pipe `|`
- **`doc_host`:** this is the domain where your docs are served. If your docs are served on GitHub Pages, this is typically `your-username.github.io`
- **`doc_baseurl`:** the path on your domain that has the docs.  This is usually the root `/` so doesn't normally need to be changed.
- **`module_baseurl`:** This is optional for a situations where you want to document python apis.  This allows documentation to include links to source code.

<a id="markdown-3-install-dependencies" name="3-install-dependencies"></a>

### 3. Install Dependencies

1. Before you get started, you need to make sure you have [node and npm installed](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). 

2. Next, create an isolated python environment using your favorite tool such as `conda`, `pipenv`, `poetry` etc.  Then, from the root of this repo run this command in the terminal:

    ```sh
    make install
    ```

Note that this references `requirements.txt`, which you will have to update appropriately anytime you include addtional python dependencies in your documentation.


<a id="markdown-using-notebooks" name="using-notebooks"></a>

## Using Notebooks



<a id="markdown-notebook-development-setup" name="notebook-development-setup"></a>

### Notebook Development Setup

1. You need to open 3 different terminal windows (I recommend using split panes), and run the following commands in three seperate windows:

    _Note: we tried to use docker-compose but had trouble getting some things to run on Apple Silicon, so this will have to do for now._

    Start the docs server:
    
    ```sh
    make docs
    ```

    Watch for changes to notebooks:
    
    ```sh
    make watch
    ```

    Start Jupyter Lab:
    
    ```sh
    make nb
    ```

2. Open a browser window for the [authoring guide in the docs](http://localhost:3000/docs/nb).  You may have to hard-refresh the first time you make a change, but hot-reloading generally works.


<a id="markdown-tutorial" name="tutorial"></a>

### Tutorial

To go through the tutorial, open the [rendered authoring guide](http://localhost:3000/docs/nb) in one window, and the notebook [docs/nb.ipynb](docs/nb.ipynb) in another window.  Study the notebook cells and the corresponding rendered page carefully.  You will see many options for:

- How to deal with scripts vs interactive code cells
- How to run bash commands / scripts
- How to setup front matter for page configuration
- Hiding/Showing cell inputs, outputs or both
- Selectively displaying Metaflow logs
- How to setup tests
- How to Author API docs

After you complete the tutorial, you should run through this list and make sure you know how to do all of the above tasks.

<a id="markdown-running-tests--updating-notebooks" name="running-tests--updating-notebooks"></a>

#### Running Tests & Updating Notebooks

> To test the notebooks, run `make test` from the root of this repo.  This will execute all notebooks in parallel and report an error if there are any errors found.

<a id="markdown-skipping-tests-in-cells" name="skipping-tests-in-cells"></a>

#### Skipping tests in cells

> If you want to skip certain cells from running in tests because they take a really long time, you can place the comment `#notest` at the top of the cell.  You can add additional flags by adding pipe-delimited values into the `tst_flags` field of [settings.ini](settings.ini).


<a id="markdown-update-all-notebooks" name="update-all-notebooks"></a>

#### Update all notebooks

> You can refresh all notebooks in place with `make update`.  This runs all notebooks programatically, skipping over cells with `#notest` (or other `tst_flags` flags you set) and **saves the result to the same filename.**  This will also re-generate the markdown files from the updated notebooks.

> The reason for providing `make update` seperately from `make test` is so you can have the option of first testing your notebooks without overwriting them.

<a id="markdown-ignoring-skip-flags" name="ignoring-skip-flags"></a>

#### Ignoring Skip Flags

> We have previously discussed that you can skip tests or notebook refreshes in specific cells with the flag `#notest` (or other flags you set in [settings.ini](settings.ini)).  You can force these cells to be run or refreshed with the `--flags` argument.  For example, let's say we want to run tests or refresh notebooks and do not want to ignore cells with the `#notest` flag:

- For testing: `nboc_test --flags #notest`
- For refreshing notebooks: `nbdoc_update --flags #notest`


<a id="markdown-hotkeys-for-jupyter" name="hotkeys-for-jupyter"></a>

### Hotkeys For Jupyter

> People complain about "state" in Jupyter.  This can be easily avoided by frequently restarting the kernel and running all cells from the top.  Thankfully, you can set a hotkey that allows you to do this effortlessly.  In Jupyter Lab, go to `Settings` then `Advanced Settings Editor`.  Copy and paste the below json into the `User Prefences` pane.  If you already have user-defined shortcuts, modify this appropriately.

```
{
"shortcuts": [
    {
        "command": "notebook:restart-run-all",
        "keys": [
            "Ctrl R",
            "R"
        ],
        "selector": "body",
    },
    {
        "command": "notebook:restart-and-run-to-selected",
        "keys": [
            "Ctrl R",
            "S"
        ],
        "selector": "body",
    },
...
}
```

<a id="markdown-using-markdown" name="using-markdown"></a>

## Using Markdown

> You do not have to use notebooks to update documentation.  You can use markdown as you would normally do with Docusaurus.  This is a good option when there isn't much code in the document you are trying to write.


<a id="markdown-running-the-documentation-locally" name="running-the-documentation-locally"></a>

## Running the documentation locally

* Clone this repo
* `cd nbdoc-docusarus`
* `make install`
* `make docs`

> Any saved changes that you make to the `.md` files in the `docs` or `blogs` directory will automatically be reflected at [the local preview page](http://localhost:3000/).


<a id="markdown-automatic-publishing" name="automatic-publishing"></a>

## Automatic publishing

> Any pushes to the `master` branch will automatically publish to [the live documentation pages](https://outerbounds.github.io/docs/) . The publishing uses GitHub Actions and Github pages. You can see the progress of the publish action by going to the `Actions` tab of this repository.


<a id="markdown-about" name="about"></a>

## About

We use [nbdoc](https://github.com/outerbounds/nbdoc) as a notebook converter and testing utility, as well as [docusaurus](https://github.com/facebook/docusaurus) for the static site generator.
