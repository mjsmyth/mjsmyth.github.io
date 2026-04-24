# Confluence release management

This post describes a release process for Confluence documentation using Python scripts.

## Situation

The Abiquo cloud management platform was very large with over 75 different entities and 750 operations. And the main documentation site had over 1500 pages, including 750 API reference pages. A new software release could involve over 50 new or changed pages. For each major version, we created a new copy of the site. Copying changes to 50 pages of the new version by hand could take over two days.

## Obstacles

To automatically manage documentation versions and releases, you can use a tool such as Scroll Versions. At the time of defining this release process, for every version, Scroll versions created a copy of every page in the site. At times Abiquo released two major versions in a year, so the number of pages in the documentation site would have soon increased out of control.

## Actions

I wrote a group of Python scripts to manage a basic release process with the following steps:

1. Create a copy of the pages to change and add a suffix of `vNNN` to the draft page name. The number `NNN` represented the new version number, for example, `My_Page_v600` would be for version 6.0.0.
2. Add new pages, also adding the version suffix to the page name.
3. Near the release date, run a script to create an "original" page for any new pages.
4. To perform the release, run a script that copied the draft pages over the original pages.
5. Run a cleanup script to move the draft pages to a staging area under a single main page, to delete them.

## Results

The scripts reduced the time to release a new version of the documentation from around 2 days to around 2 hours. The table of changes created by the scripts could be adapted to create documentation release notes.
