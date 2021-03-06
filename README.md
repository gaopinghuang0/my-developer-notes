# my-developer-notes
Record what I know and what I don't know yet.  This repo includes the books and blog posts that I've read since 2014.

## Validate relative links in Markdown
Since this repo has many markdown files, it is difficult to keep relative links consistent after renaming or reorganizing another file. So remember to run the following command to validate the links whenever a file is renamed or moved.
```sh
# Install globally for once
$ npm install --global remark-cli remark-validate-links
# Only output warnings and errors.
# Do not miss the dot `.` at the end, which means the current directory.
$ remark -u validate-links -q .
```
For more details, check [the homepage of the plugin.](https://github.com/remarkjs/remark-validate-links)

## Add Table Of Contents
Since I am using VScode and have installed plugin: "Markdown All in One", it is super simple. Just (`Control/⌘`+`Shift`+`P`) and select the "Select Markdown: Create Table of Contents" option.

## References
* Inspired by [Web developer roadmap - 2020](https://github.com/kamranahmedse/developer-roadmap)
* Inspired by [Don’t be a Junior Developer: The Roadmap - June 2018](https://zerotomastery.io/blog/dont-be-a-junior-developer-the-roadmap/?utm_source=medium&utm_medium=dont-be-junior-the-roadmap)