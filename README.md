
# ICUBE M2 Code Checker

This is the standard code checker for Magento 2 in ICUBE.



## Installation

Clone this project

```bash
  git clone https://github.com/icubeus/m2-codechecker.git
```

Copy these folders and files to your project folder.
```bash
  .github/
  eslint/
  package.json
```    

Modify .github/workflows/main.yml according to your project.
1. Line 27 (composer_name)
```bash
  composer_name: nameof/repository
``` 
example
```bash
  composer_name: icube-mage/magento2-focusnusantara
``` 
2. Line 44 (destination folder to scan for jslint)
```bash
  run: npm run eslint -- foldername
``` 
example
```bash
  run: npm run eslint -- src
``` 
You can comment out the whole js code checking job if there's no js code in your repository, because the job will mark as failed if there's no js code in the scanned folders.

3. Line 4-5 (branches). You can specify which action will be the trigger for the jobs to be run.
```bash
  pull_request:
    branches: [ master, staging, develop ]
``` 
This means that every time pull request is created on branch master/staging/develop, the workflow will run.
If there is a new commit added to the branch that's being pull requested (not merged yet), it will retrigger the workflow to run.
You can also trigger the workflow on every push on a branch. Example:
```bash
  pull_request:
    branches: [ master, staging, develop ]
  push:
    branches: [ develop ]
``` 

## For Developers: How To Test & Fix In Local
(run these commands from the magento root folder)

PHP Unit Testing
```bash
  vendor/bin/phpunit path/to/folder
``` 

Magento Coding Standard Scan
```bash
  vendor/bin/phpcs --standard=vendor/magento/magento-coding-standard/Magento2 path/to/folder
  vendor/bin/phpcs --standard=vendor/magento/magento-coding-standard/Magento2Framework path/to/folder
``` 

Magento Coding Standard General Fixing
```bash
  vendor/bin/phpcbf --standard=Magento2 path/to/folder
``` 
If you get this error,
```bash
  ERROR: the "Magento2" coding standard is not installed. The installed coding standards are PSR12, Zend, PSR1, PSR2, PEAR, MySource and Squiz
```
copy the `Magento2` folder to your project root folder and try to rerun the command. If you then receive this error,
```bash
  ERROR: Referenced sniff "PHPCompatibility.FunctionUse.RemovedFunctions" does not exist
```
run this command
```bash
  vendor/bin/phpcs --config-set installed_paths ../../magento/magento-coding-standard/,../../phpcompatibility/php-compatibility
```
then rerun the cbf command.

JS Eslint Checker
```bash
  eslint -- app/code/Icube
``` 

Additional: Code Sniffer
```bash
  vendor/bin/rector process path/to/folder --dry-run --autoload-file vendor/squizlabs/php_codesniffer/autoload.php --autoload-file vendor/phpcompatibility/php-compatibility/PHPCSAliases.php
``` 