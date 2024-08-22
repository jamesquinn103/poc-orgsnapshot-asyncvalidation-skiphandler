# Copy of Dreamhouse Sample App - Used to Test Org Snapshot, Async Validation, & Install Skip Handlers

[Link to actively maintained Dreamhouse repo](https://github.com/trailheadapps/dreamhouse-lwc)

This repo was created to test out, document, and benchmark the following Summer '24 new features designed to speed up the package dev lifecycle.

-   [Scratch Org Snapshot Release Note](https://help.salesforce.com/s/articleView?id=release-notes.rn_dev_environments_snapshots_ga.htm&release=250&type=5)
-   [Async Validation Release Note](#https://help.salesforce.com/s/articleView?id=release-notes.rn_packaging_async_validation.htm&release=250&type=5)
-   [Scratch Org Install Skip Handler Release Note](https://help.salesforce.com/s/articleView?id=release-notes.rn_packaging_skip_handler.htm&release=250&type=5)

> **Note**
> This repo uses non-namespaced unlocked packages. If you wish to use managed 2GP packages instead, you would need to add a namespace, remove any metadata types that do not support managed 2GP, and do a bit of troubleshooting when creating a package version

---
# How to Use this Repo to Test these Three Features

## Prerequisites

-   Set up your environment. Follow the steps here: [Get Started with Scratch Org Snapshots](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_snapshots_get_started.htm) 

-   Clone this repository to create a local copy on your machine:

    ```
    git clone <<REPO_URL_GOES_HERE>>
    cd dreamhouse-lwc
    ```

## Scratch Org Snapshot

Scratch Org Snapshot is a huge timesaver when working with **packages that depend on other packages**, sometimes called "extension" packages.

This repo provides two directories and packages:

-   DreamhouseBaseUnlockedPkg, a "base" package, made of the contents of the "force-app" folder
-   DreamhouseExtensionUnlockedPkg, an "extension" package, made of the contents of the "dreamhouse-extension" folder

There are two main ways to work with these directories and packages:

1.   Reference the existing package version IDs directly

     - These can be found in the sfdx-project.json file. They begin with '04t'
     - Use this option to create new, separate packages that have dependencies on these existing package versions, install these existing package versions, create and use snapshots that have them pre-installed, etc.

2.   Package the contents of these directories yourself

     - Ignore the existing packages, package version IDs, etc.
     - In your local copy of this repo, use the below CLI commands to create your own packages
     - Use this option if you want to create new package versions from these directories, promote these package versions, upgrade package version installations, create these package versions using snapshots, try this with 2GP managed packages, etc.


Steps to create your own packages using this repo's two directories:

-   Create the "base" package
    ```
sf package create --name "DreamhouseBaseUnlockedPkg" --path force-app --package-type Unlocked
    ```

-   Create a package version for the "base" package
    ```
sf package version create --package DreamhouseBaseUnlockedPkg --installation-key-bypass --code-coverage 
--definition-file config/project-scratch-def.json --wait 10
    ```

-   Create the "extension" package
    ```
sf package create --name "DreamhouseExtensionUnlockedPkg" --path dreamhouse-extension --package-type Unlocked
    ```

-   Create a package version for the "extension" package
    ```
sf package version create --package DreamhouseExtensionUnlockedPkg --installation-key-bypass --code-coverage --definition-file config/project-scratch-def.json --wait 10
    ```

### Creating and Using Snapshots

-   Create a non-namespaced scratch org - see [Create Scratch Orgs](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_create.htm)
-   Optionally, install one or more packages in this scratch org - see [Install Packages with the CLI](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_unlocked_pkg_install_pkg_cli.htm)
-   Create a snapshot - see [Create a Scratch Org Snapshot](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_snapshots_create.htm)
-   Create a scratch org using the snapshot - see [Create a Scratch Org Based on a Snapshot](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_snapshots_create_scratch_org.htm)

The scratch org created from the snapshot will carry over the installed packages, features, limits, licenses, metadata, and data from the scratch org that was used to create the snapshot. In addition, it [can be namespaced](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_snapshots_namespace_limitations.htm).

TODO: run experiment to compare time taken to create a scratch org from a snapshot that comes with the dreamhouse base package version pre-installed vs. time taken to create a scratch org normally, and then install the dreamhouse base package version, to arrive at the same end state. This is admittedly a naive experiment that misses much of the value prop of snapshots (unpackaged metadata config - avoiding scripting this, if the metadata type even supports MDAPI, and scripting data loads and waiting for them to run).

## Async Validation

TODO: run experiment to compare creating the dreamhouse base package version using full validation vs. async validation vs. skip validation. Async validation is most useful for creating package versions that have a lot of Apex tests that take a long time to run. It benefits both base and extension packages.

## Scratch Org Install Skip Handler

TODO: run experiment to compare installing the dreamhouse base package version with and without the skip handler. 

[![CI Workflow](https://github.com/trailheadapps/dreamhouse-lwc/workflows/CI/badge.svg)](https://github.com/trailheadapps/dreamhouse-lwc/actions?query=workflow%3ACI) [![Packaging Workflow](https://github.com/trailheadapps/dreamhouse-lwc/workflows/Packaging/badge.svg)](https://github.com/trailheadapps/dreamhouse-lwc/actions?query=workflow%3APackaging) [![codecov](https://codecov.io/gh/trailheadapps/dreamhouse-lwc/branch/main/graph/badge.svg)](https://codecov.io/gh/trailheadapps/dreamhouse-lwc)

> [!IMPORTANT]
> This is the modern Lightning Web Components version of the Dreamhouse sample application. If you are looking for the legacy Aura version, click [here](https://github.com/dreamhouseapp/dreamhouse-sfdx).

![dreamhouse-logo](dreamhouse-logo.png)

Dreamhouse is a sample application that demonstrates the unique value proposition of the Salesforce platform for building Employee Productivity and Customer Engagement apps.

<div>
    <img src="https://res.cloudinary.com/hy4kyit2a/f_auto,fl_lossy,q_70,w_50/learn/projects/quick-start-dreamhouse-sample-app/17d9a9454cb84973b3adfe25e9f12b01_badge.png" align="left" alt="Trailhead Badge"/>
    Learn more about this app by completing the <a href="https://trailhead.salesforce.com/en/content/learn/projects/quick-start-dreamhouse-sample-app">Quick Start: Explore the Dreamhouse Sample App</a> Trailhead project or by watching this <a href="https://www.youtube.com/watch?v=UvUDi8acq2w&list=PLgIMQe2PKPSJcuCwM61dEc4jFG_jHqV2t&index=4">short presentation video</a>.
    <br/>
    <br/>
    <br/>
</div>

> This sample application is designed to run on Salesforce Platform. If you want to experience Lightning Web Components on any platform, please visit https://lwc.dev, and try out our Lightning Web Components sample application [LWC Recipes OSS](https://github.com/trailheadapps/lwc-recipes-oss).

## Table of contents

-   [Installing Dreamhouse Using a Scratch Org](#installing-dreamhouse-using-a-scratch-org): This is the recommended installation option. Use this option if you are a developer who wants to experience the app and the code.

-   [Installing Dreamhouse Using an Unlocked Package](#installing-dreamhouse-using-an-unlocked-package): This option allows anybody to experience the sample app without installing a local development environment.

-   [Installing Dreamhouse using a Developer Edition Org or a Trailhead Playground](#installing-dreamhouse-using-a-developer-edition-org-or-a-trailhead-playground): Useful when tackling Trailhead Badges or if you want the app deployed to a more permanent environment than a Scratch org.

-   [Note on Sample Data Import](#note-on-sample-data-import)

-   [Optional installation instructions](#optional-installation-instructions)

-   [Code tours](#code-tours)

## Installing Dreamhouse using a Scratch Org

1. Set up your environment. Follow the steps in the [Quick Start: Lightning Web Components](https://trailhead.salesforce.com/content/learn/projects/quick-start-lightning-web-components/) Trailhead project. The steps include:

    - Enable Dev Hub in your Trailhead Playground
    - Install Salesforce CLI
    - Install Visual Studio Code
    - Install the Visual Studio Code Salesforce extensions, including the Lightning Web Components extension

1. If you haven't already done so, authorize your hub org and provide it with an alias (**myhuborg** in the command below):

    ```
    sf org login web -d -a myhuborg
    ```

1. Clone this repository:

    ```
    git clone https://github.com/dreamhouseapp/dreamhouse-lwc
    cd dreamhouse-lwc
    ```

1. Create a scratch org and provide it with an alias (**dreamhouse** in the command below):

    ```
    sf org create scratch -d -f config/project-scratch-def.json -a dreamhouse
    ```

1. Push the app to your scratch org:

    ```
    sf project deploy start
    ```

1. Assign the **dreamhouse** permission set to the default user:

    ```
    sf org assign permset -n dreamhouse
    ```

1. (Optional) Assign the `Walkthroughs` permission set to the default user.

    > Note: this will enable your user to use In-App Guidance Walkthroughs, allowing you to be taken through a guided tour of the sample app. The Walkthroughs permission set gets auto-created with In-App guidance activation.

    ```
    sf org assign permset -n Walkthroughs
    ```

1. Import sample data:

    ```
    sf data tree import -p data/sample-data-plan.json
    ```

1. Open the scratch org:

    ```
    sf org open
    ```

1. In **Setup**, under **Themes and Branding**, activate the **Lightning Lite** theme.

1. In App Launcher, select the **Dreamhouse** app.

## Installing Dreamhouse using an Unlocked Package

Follow this set of instructions if you want to deploy the app to a more permanent environment than a Scratch org or if you don't want to install the local developement tools. You can use a non source-tracked orgs such as a free [Developer Edition Org](https://developer.salesforce.com/signup) or a [Trailhead Playground](https://trailhead.salesforce.com/).

Make sure to start from a brand-new environment to avoid conflicts with previous work you may have done.

1. Log in to your org

1. Click [this link](https://login.salesforce.com/packaging/installPackage.apexp?p0=04tKY000000LOv6YAG) to install the Dreamhouse unlocked package in your org.

1. Select **Install for All Users**

1. In App Launcher, click **View all**, select the Dreamhouse app.

1. Click the **Settings** tab and click the **Import Data** button in the **Sample Data Import** component.

1. If you're attempting the [Quick Start](https://trailhead.salesforce.com/en/content/learn/projects/quick-start-dreamhouse-sample-app) on Trailhead, this step is required, but otherwise, skip:

    - Go to **Setup > Users > Permission Sets**.
    - Click **dreamhouse**.
    - Click **Manage Assignments**.
    - Check your user and click **Add Assignments**.

1. In **Setup**, under **Themes and Branding**, activate the **Lightning Lite** theme.

1. In App Launcher, select the **Dreamhouse** app.

## Installing Dreamhouse using a Developer Edition Org or a Trailhead Playground

Follow this set of instructions if you want to deploy the app to a more permanent environment than a Scratch org.
This includes non source-tracked orgs such as a [free Developer Edition Org](https://developer.salesforce.com/signup) or a [Trailhead Playground](https://trailhead.salesforce.com/).

Make sure to start from a brand-new environment to avoid conflicts with previous work you may have done.

1. Clone this repository:

    ```
    git clone https://github.com/dreamhouseapp/dreamhouse-lwc
    cd dreamhouse-lwc
    ```

1. Authorize your Trailhead Playground or Developer org and provide it with an alias (**mydevorg** in the command below):

    ```
    sf org login web -s -a mydevorg
    ```

1. Run this command in a terminal to deploy the app.

    ```
    sf project deploy start -d force-app
    ```

1. Assign the `dreamhouse` permission set to the default user.

    ```
    sf org assign permset -n dreamhouse
    ```

1. Import some sample data.

    ```
    sf data tree import -p ./data/sample-data-plan.json
    ```

1. If your org isn't already open, open it now:

    ```
    sf org open -o mydevorg
    ```

1. In **Setup**, under **Themes and Branding**, activate the **Lightning Lite** theme.

1. In App Launcher, select the **Dreamhouse** app.

## Note on Sample Data Import

Properties inserted using the Salesforce CLI will appear as listed on TODAY() - 10 days. If you want to have this value randomized, perform the data import from the app Settings tab instead.

## Optional Installation Instructions

This repository contains several files that are relevant if you want to integrate modern web development tooling to your Salesforce development processes, or to your continuous integration/continuous deployment processes.

### Code formatting

[Prettier](https://prettier.io 'https://prettier.io/') is a code formatter used to ensure consistent formatting across your code base. To use Prettier with Visual Studio Code, install [this extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) from the Visual Studio Code Marketplace. The [.prettierignore](/.prettierignore) and [.prettierrc](/.prettierrc) files are provided as part of this repository to control the behavior of the Prettier formatter.

> **Warning**
> The current Apex Prettier plugin version requires that you install Java 11 or above.

### Code linting

[ESLint](https://eslint.org/) is a popular JavaScript linting tool used to identify stylistic errors and erroneous constructs. To use ESLint with Visual Studio Code, install [this extension](https://marketplace.visualstudio.com/items?itemName=salesforce.salesforcedx-vscode-lwc) from the Visual Studio Code Marketplace. The [.eslintignore](/.eslintignore) file is provided as part of this repository to exclude specific files from the linting process in the context of Lightning Web Components development.

### Pre-commit hook

This repository also comes with a [package.json](./package.json) file that makes it easy to set up a pre-commit hook that enforces code formatting and linting by running Prettier and ESLint every time you `git commit` changes.

To set up the formatting and linting pre-commit hook:

1. Install [Node.js](https://nodejs.org) if you haven't already done so
1. Run `npm install` in your project's root folder to install the ESLint and Prettier modules (Note: Mac users should verify that Xcode command line tools are installed before running this command.)

Prettier and ESLint will now run automatically every time you commit changes. The commit will fail if linting errors are detected. You can also run the formatting and linting from the command line using the following commands (check out [package.json](./package.json) for the full list):

```
npm run lint
npm run prettier
```

## Code Tours

Code Tours are guided walkthroughs that will help you understand the app code better. To be able to run them, install the [CodeTour VSCode extension](https://marketplace.visualstudio.com/items?itemName=vsls-contrib.codetour).

## Credits

The app GeocodingService uses OpenStreetMap API to geocode property addresses. OpenStreetMap® is open data, licensed under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/) by the [OpenStreetMap Foundation (OSMF)](https://wiki.osmfoundation.org/wiki/Main_Page). You are free to copy, distribute, transmit and adapt OpenStreetMap data, as long as you credit OpenStreetMap and its contributors. If you alter or build upon our data, you may distribute the result only under the same licence. The [full legal code](https://opendatacommons.org/licenses/odbl/1-0/) explains your rights and responsibilities in regard to the service.
