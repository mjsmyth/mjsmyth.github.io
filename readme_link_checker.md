# Readme link checker

When I first started  working with the [ReadMe](http://readme.io) documentation platform, I found it difficult to know if all the links in the page text and at the bottom of the page were valid. The links refer to page slugs, but these can change. [ReadMe](http://Readme.io) will maintain a valid link for a couple of page slug changes, but after that, the link will break. I also like to make sure that the page slugs match the titles of the pages, wherever possible.

It took me some time to check links manually, so I wrote a Python script to automate this process and save time.

## Initial version

The first Python script I wrote In  early 2024 used the ReadMe API to pull the information about the page slugs. And I downloaded the Markdown files for all the pages containing the links to check.

## New version for the GitHub integration

Then when the GitHub integration was released, I updated the Python script to work with just the documentation source files. I had to adapt the script because every top level source folder was called `index.md`, and also because there was a new section of front matter in each file. After these structural changes, the script was able to check links in the reference section within the API docs or integration docs too.

## Script versions

This script is in a GitHub repository at [https://github.com/mjsmyth/readme\_link\_checker](https://github.com/mjsmyth/readme_link_checker). Currently, the main branch version of the script just prints output to the terminal, and there is a branch with a new script that creates a CSV output  file.

## Obsolescence

In any case, ReadMe now includes link checking as part of the AI linter feature.

