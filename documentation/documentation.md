# DHIS2 Shared Component Platform (SCP) Documentation
##### Documentation for DHIS2 Shared Component platform website, SCP command line interface and SCP Whitelist repository

## 1. About DHIS2 Shared Component Platform (DHIS2 SCP)

DHIS2 Shared Component platform is a platform for sharing and reuse of software components created by HISP community. The components hosted on this platform are React or Angular UI components and can be used as building blocks for a web application. The project has been conducted within the DHIS2 Design Lab in collaboration with members of the DHIS2 Core team and individuals from HISP groups. The project consisted of exploring solutions to make app development less resource-intensive by managing resources (components) made in the different HISP groups, thereby making them available for others to use.

__Why do we need reusable components?__
* More effective development of Front end web application
* Consistency in web applications
* Smaller code base to maintain

__What characterizes reusable components?__
* Easy plug-able components
* Can be published directly to [NPM](https://www.npmjs.com)
* Extendable template
* Web-browser independent

Some of the examples of reusable component libraries: [Material UI](https://material-ui.com), [Dhis2/ui](https://github.com/dhis2/ui), [Semantic UI](https://www.npmjs.com/package/semantic-ui).


 DHIS2 Shared Component Platform consists of three distinct modules:

* [SCP website](https://dhis2designlab.github.io/scp-website/) 
* [SCP command line interface](https://github.com/dhis2designlab/scp-cli)
* [SCP whitelist repository](https://github.com/dhis2designlab/scp-whitelist)

Use the [SCP website](https://dhis2designlab.github.io/scp-website/) to discover components within NPM packages.
Run the [SCP command line interface](https://github.com/dhis2designlab/scp-cli) from the terminal to check if your package is set up properly, so that your components become discoverable on the [SCP website](https://dhis2designlab.github.io/scp-website/).
Apply for the package quality assurance in [the SCP whitelist repository](https://github.com/dhis2designlab/scp-whitelist) and let your package be assessed by the team of maintainers.

### Three different roles
We refer to component owners, component consumers, and maintainers as three separate roles who interact with the component platform in different ways.
* Component owners: creates components and publishes them to the platform.
* Component consumers: uses components the component owners have published to the platform.
* Maintainers: maintains the whitelist of verified components.

Keep in mind that none of these are mutually exclusive, as one person very well could have all of the roles.

## 1.2 Getting started

If you want to publish your package with components for further reuse, see section __2. Publishing your package__.
If you want to verify your package locally, see section __3. Verifying your package locally__.
If you want to apply for package verification, see section __4. Applying for package verification__.
If you are here to learn how to find components, see section __5. Finding reusable components__.
If you are part of the maintainer team and want to learn about the verification workflow, see section __6. Maintaining package verification workflow__. 

## 2. Publishing your package 

We assume you already have a package with reusable components that you want to publish on NPM and make available on the [SCP website](https://dhis2designlab.github.io/scp-website/). 
However if you do not know how to create and publish such a package, you can read more about it [here](https://dev.to/ramonak/how-to-publish-a-custom-react-component-to-npm-using-create-react-library-4bhi).

To make your package searchable on the [SCP website](https://dhis2designlab.github.io/scp-website/), two
conditions must be met:

* Your `package.json` file must include `dhis2-component-search` keyword. (see section __3.1.1 Keyword__)
* Your `package.json` file must include `dhis2ComponentSearch` property with correct values and structure (see section __3.1.3 The `dhis2ComponentSearch` property__).

However, there are more conditions that must be met if you plan to submit your package for verification and pre-verify it locally. If this is the case, read the section __3. Verifying your package locally__ and __4. Applying for package verification__.

When you have finished working on your `package.json` structure, you can proceed to publishing your package on NPM. 

## 3. Verifying your package locally

[SCP command line interface](https://github.com/dhis2designlab/scp-cli) helps you to create NPM packages with React components that can be used to build DHIS2 apps or other components.

You can install the SCP CLI with the following command:

```bash
npm install -g https://github.com/haheskja/scp-cli#master
```

This package provides a command line interface `dhis2-scp-cli` with various commands.

* `dhis2-scp-cli verify`: This command will check the quality of your NPM package.

The command will do the following:

* Verify that the package's `package.json` has the `dhis2-component-search` keyword (see section 3.1.1).
* Verify that the package's `package.json` has the `dhis2ComponentSearch` property with correct values and structure (see section 3.1.3).
* Run `npm audit` check and output the result. (See [npm audit](https://docs.npmjs.com/cli/v6/commands/npm-audit))
* Run `eslint` and output the result. (See [ESLint](https://www.npmjs.com/package/eslint))

This command should be run inside the directory of your npm package.

To see additional options for this command run `dhis2-scp-cli verify --help`. For verbose output run with `dhis2-scp-cli verify -vvv`.

The SCP CLI also provides a command for verifying pull requests: `dhis2-scp-cli pr-verify`.

This command can be used by component owners to check their pull requests, for example:

```bash
dhis2-scp-cli pr-verify --pull-request-url "https://api.github.com/repos/dhis2designlab/scp-whitelist/pulls/14"
```

It can also be used by component owners to check a specific package identifier and version without there being any pull request, for example:

```bash
dhis2-scp-cli pr-verify --fake-package-data "@dhis2/ui-core,5.7.3"
```


## 3.1 Verification prerequisites

### 3.1.1 Keyword

Your `package.json` file must include `dhis2-component-search` keyword as follows:

```json
{
  "keywords": [
      "dhis2-component-search"
  ]
}
```

### 3.1.2 Repository

Currently we only support packages hosted on Github.
Your `package.json` file must include `repository` property, that includes key/value pairs for repository type and url. 
Inside your `package.json`it would look like this:

```json
{
   "repository": {
        "type" : "git",
        "url" : "https://github.com/npm/cli.git"
    }
}
```
The URL should be a publicly available and you must specify the repository type and url. Note that you cannot use shortcut syntax, e.g. `"repository": "github:user/repo"`, and you must use `https://`.


### 3.1.3 The `dhis2ComponentSearch` property

The `dhis2ComponentSearch` property of a `package.json` file includes the information about the package that is relevant to SCP and its search functionality.

The `dhis2ComponentSearch` property must include key/value pairs for framework (using `language` key, we currently support `react` and `angular`), and components. The `component` property, in turn, takes an array of component objects. Each component object must include following information defined as key/value pairs:

__Required__:
* The `name` property contains the name of the exported component
* The `export` property contains the identifier of the exported component. The expectation is that if the package is loaded as a [commonJS module]((https://en.wikipedia.org/wiki/CommonJS)), that the component exist as a property with this identifier on the [commonJS module]((https://en.wikipedia.org/wiki/CommonJS)).
* The `description` property contains the description of the exported component

__Optional__:

* The `dhis2Version` optional property contains the DHIS2 versions supported by the exported component, the versions must be specified as an array of strings.

Inside your `package.json`, the `dhis2ComponentSearch` property may look something like this:

```json
{
   "dhis2ComponentSearch": {
    "language": "react",
    "components": [
      {
        "name": "Organizational Unit Tree",
        "export": "OrgUnitTree",
        "description": "A simple OrgUnit Tree",
        "dhis2Version": [
          "32.0.0",
          "32.1.0",
          "33.0.0"
        ]
      }
    ]
   }
}
```

### 3.1.4 Components as commonJS or ES modules

Our verification process requires your components to be distributed as [commonJS modules](https://en.wikipedia.org/wiki/CommonJS) or [ES modules](https://en.wikipedia.org/wiki/ECMAScript). The command `npm install` should result in a valid module.
One good way to achieve this is with the help of [@dhis2/cli-app-scripts](https://platform.dhis2.nu/#/), as it bundles `commonjs` and `es` module formats. A simple [boilerplate](https://github.com/haheskja/scp-react-boilerplate) can help you get started. Another good way is to build the library with [create-react-library](https://www.npmjs.com/package/create-react-library).

### 3.1.5 NPM and Github

Your package must be published on [NPM](https://www.npmjs.com) and have a public [Github](https://www.github.com) repository. Since we verify a specific version of the package, a git tag with this version should be added. When tagging releases in a version control system, the tag for a version must be `vX.Y.Z` e.g. `v1.0.2`.


## 4. Applying for package verification 

[DHIS2 SCP whitelist repository](https://github.com/dhis2designlab/scp-whitelist) contains a list of verified NPM packages with reusable components that comply with SCP. Pull requests to this repository will be validated with a GitHub actions workflow.

 If you want your package verified and added to the list of verified packages, you need to submit your package for verification. For that you would need to modify [`list.csv`](https://github.com/dhis2designlab/scp-whitelist/blob/main/list.csv) file by adding a new line containing your NPM package `identifier`and its `version` separated by a comma, e.g. `lodash,4.17.14`. Since you do not have a write access to the repository, a change in this file will write it to a new branch in your fork, so you can make a pull request. Fill in a title and description and create your pull request, which in turn, will trigger the verification workflow on your package.

The verification workflow:

* Checks for verification prerequisites defined in section __3.1 Verification prerequisites__
* Lints the code with [ESLint](https://www.npmjs.com/package/eslint)
* Runs [npm audit](https://docs.npmjs.com/cli/v6/commands/npm-audit)

The output of the verification workflow serves as a basis for acceptance of the pull request, but the final decision for acceptance is up to the maintaners.

## 5. Finding reusable components

[SCP website](https://dhis2designlab.github.io/scp-website/) provides necessary functionality for finding reusable components registered with SCP - both verified and unverified. It fetches and lists all the exported components within all packages published on [NPMjs](https://www.npmjs.com) that contain the `dhis2-component-search` keyword in `package.json` file and `dhis2ComponentSearch` property with correct values and structure.

The website provides the following filters to narrow search result:
* Framework
  * All: Shows all React and Angular components
  * React: Shows only React components
  * Angular: Shows only Angular components

* DHIS2 Version : allows filtering on supported DHIS2 version
* Show only verified components: shows the components within the packages that have their latest version verified.

Each component is represented by a component card that contains the following information:
* Component name
* Component description
* Component export
* Package identifier
* Package keywords
* Information on latest published version
* Link to the package on NPM
* Latest unverified package version
* Latest verified package version

![GitHub Logo](https://i.imgur.com/d8qM2K2.jpg)


## 6. Maintaining [the SCP whitelist](https://github.com/dhis2designlab/scp-whitelist)

[DHIS2 SCP whitelist repository](https://github.com/dhis2designlab/scp-whitelist) contains a list of verified NPM packages. Pull requests to this repository will be validated with a GitHub actions workflow.

If you are part of the [The SCP whitelist](https://github.com/dhis2designlab/scp-whitelist) maintaining team, you need to make sure you have the right access rights to be able to manage pull-requests. 

A user submits his package for verification by modifying [`list.csv`](https://github.com/dhis2designlab/scp-whitelist/blob/main/list.csv) file, adding a new line containing his NPM package `identifier`and its `version` separated by a comma, e.g. `lodash,4.17.14`.It will result in a pull-request that will trigger the automated verification workflow on the added package. 

The verification workflow:

* Checks for verification prerequisites defined in section __3.1 Verification prerequisites__
* Lints the code with [ESLint](https://www.npmjs.com/package/eslint) and outputs the result
* Runs [npm audit](https://docs.npmjs.com/cli/v6/commands/npm-audit) and outputs the result

The output of the verification workflow serves as a basis for acceptance of the pull request, but the final decision for acceptance is up to the maintaners. 



