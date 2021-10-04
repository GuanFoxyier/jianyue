> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.npmjs.com](https://www.npmjs.com/package/typescript-ast-explorer)

> Command line tool to explore TypeScript projects and SourceFile abstract syntax tree interactively

typescript-ast-explorer

![](https://static.npmjs.com/255a118f56f5346b97e56325a1217a16.svg)


---------------------------------------------------------------------------------------------

0.0.5 • Public • Published 2 years ago

*   [Readme](https://www.npmjs.com/package/typescript-ast-explorer?activeTab=readme)
*   [Explore BETA](https://www.npmjs.com/package/typescript-ast-explorer?activeTab=explore)
*   [8 Dependencies](https://www.npmjs.com/package/typescript-ast-explorer?activeTab=dependencies)
*   [0 Dependents](https://www.npmjs.com/package/typescript-ast-explorer?activeTab=dependents)
*   [4 Versions](https://www.npmjs.com/package/typescript-ast-explorer?activeTab=versions)

[](#contents)Contents
---------------------

*   [Summary](#summary)
*   [Install](#install)
*   [Usage](#usage)
*   [Options](#options)
*   [TODO](#todo)

[](#summary)Summary
-------------------

*   Demo [screen casts](https://cancerberosgx.github.io/demos/typescript-ast-explorer/index.html)
*   Explore a local TypeScript project with an Command Line interactive tool.
*   Navigate through the AST nodes and source code at same time
*   See it file structure and the AST nodes inside each file.
*   JavaScript / TypeScript API for GUI component to select files/folders/nodes interactively (based on blessed/accursed/ts-morph)

[](#install)Install
-------------------

```
npm install -g typescript-ast-explorer


```

[](#usage)Usage
---------------

```
cd my/typescript/project
typescript-ast-explorer


```

[](#options)Options
-------------------

No options - WIP - it's mostly an interactive tool

[](#todo)TODO
-------------

*   --tsConfigPath - to load a ts project other then current folder's
*   use accursed and remove a lots of files.
*   API to reuse as AST node selector - project file / folder selector
*   query elements across the project using CSS-like language (tsquery)
*   filter nodes by kind or name or query
*   apply refactors interactively
*   tree expand all - collapse all
*   move the tree to its own file
*   stateful modal, selections, expansions, etc
*   show errors except in modals
*   navigate with arrows 2-d instead of tab only (1-d)
*   a general option/menu to hide boxes - or perhaps a halo on them to collapse ?
*   confirmation before exit
*   move blessed reusable utilities to their own package

*   in file view - remove details parent and leave the children only.
*   should we add the code view in the file view?
*   currently, because of custom .d.ts, the project needs to declare the types in its own ts.config.json
*   file view: expand first folder automatically.
*   when switching from files view to code view it should open in last viewed node and vice versa - auto-expanding the tree

### Install

`npm i typescript-ast-explorer`

### DownloadsWeekly Downloads

### Collaborators

*   [![](https://www.npmjs.com/npm-avatar/eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJhdmF0YXJVUkwiOiJodHRwczovL3MuZ3JhdmF0YXIuY29tL2F2YXRhci81YmYyZDJlNjkyYTgxNDM0MGUyYjEwYTlmNjE1MDdmMj9zaXplPTEwMCZkZWZhdWx0PXJldHJvIn0.lnov-eL4tYro-rsXmIQ8-BYbhR8QEMwh0_5NkBi7wpo)](https://www.npmjs.com/~cancerberosgx)