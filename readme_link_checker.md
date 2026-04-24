# ReadMe link checker

When I first started  working with the [ReadMe](http://readme.io) documentation platform, it was difficult to find out if all the links in the page text and at the bottom of the page were valid. The links refer to page slugs, but these can change. [ReadMe](http://Readme.io) will maintain a valid link for a couple of page slug changes, but after that, the link can break. I also like to make sure that the page slugs match the titles of the pages, wherever possible, so this can lead to additional changes.

Checking the links manually took some time, so I wrote a Python script to automate this process and save time.

## Initial version

The first Python script written in early 2024 used the ReadMe API to pull the information about the page slugs. I downloaded the Markdown files for all the pages containing the links to check.

## New version for the GitHub integration

When ReadMe released the GitHub integration, I updated the Python script to work with just the documentation source files. This meant adapting the script because every top level page source in a folder was called `index.md`, and there was a new section of front matter in each file. After these structural changes, the script could also check links in the reference section within the API docs or integration docs too.

## Script versions

This script is in a GitHub repository at [https://github.com/mjsmyth/readme\_link\_checker](https://github.com/mjsmyth/readme_link_checker). Currently, the main branch version of the script prints output to the terminal, but there is another branch with a new version that creates a CSV output  file.

## Obsolescence

Fortunately, this script is no longer needed because ReadMe now has link checking as part of the AI linter.

