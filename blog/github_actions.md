---
layout: page
title: github actions
---

##### intro

github actions provide an ci/cd platform to automate builds, tests and deployments.
this post will include the core concepts of github actions and examples how to use it for machine learning
or software projects.

##### terminology

we will give a quick introduction to the main terminology of github actions. for detailed and extensive explanations,
we refer to [github's documentation](https://docs.github.com/en/actions).

in github actions, one can configure automated processes to build, test and deploy code. these processes are called <i>workflows</i> which
contain one or more <i>jobs</i>. A single job is a set of <i>actions</i> where a single action is defined to perform complex
but frequently repeated tasks. <i>events</i> are activities such as pull requests that trigger <i> workflows </i>. finally,
<i> runners </i> are the servers that run your <i> workflows </i>.

there are many options to customize your workflows such as actions developed by the community, using variables, execute custom
scripts and process using various expressions and literals.

