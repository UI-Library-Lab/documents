# Component development Guildline

## Table of Content

## Overview
In this guideline we'll go through a process step by step to implement a component from scratch, from where we pick a task to finish it.

For this guideline we assume that we want to implement a Button component for our library.

Let's dive into it.

## Pick the task
To start implementation of a component we should go to projects board in the github organization and in the related board you can pick any unassigned task in `ready` column.

> You can check react project board in this [link](https://github.com/orgs/UI-Library-Lab/projects/1/views/1).

On clicking each task you can check all descriptions and acceptance criterias of task which is the all requirements for admission of this task.

After choosing one task you can assign it to yourself and move it to `In Progress` column.

## Making branch
In this step picking our task we need to make a branch based on `develop` branch.

You can go to `develop` branch and pull last changes of develop from origin.

Now we should make a new branch based on contribution rules.

> To find how to name a branch for contribution, please check our [CONTRIBUTING.md](./../../CONTRIBUTING.md) .

In this example as we want to implement a button and it's a feature we name our branch as `feature/button`.

## Check The design and requirements
In the task's descriptions you can find a link for related design for this component. on click of it, it leads you to the corresponding design.

Now we want to know how to check component properties to know about all requirements for our component.


## Implement the component
Finally we reached to the step, where we want to implement our component.


### Folder selection
First of we should find a proper place to start our implementation. Based on [full-documentation](./../full-documentation/full-documentation.md) file all of our components categorized based on their functionality which you can see in that documentation file.

This type of component has written in task description in the board. So we can go to `components` folder, then based on type of component in the task description we can choose its folder and if it not exists we can create a folder for that category and inside of it we can make our folder for our component. 

So based on this details our Button component location will be:

`src/components/inputs/button`

> Note: don't forget to follow the file and folder naming convention which is kebab-case.

### Required files
Now we're in component folder and we wanna make required files to implement our component.

Each component we can have one folder and four files:

1. story folder: in this folder we put our story template file
2. button.tsx: this is the file where our component implementation located.
3. style.ts: as its name says, it contains all styles related to this component.
4. props.ts: which is our interface for specifing the properties for our component.

### Props interface
The first step is to specify interface for our component to know which properties will come to our component. to know about these properties we need to look at the task descriptions and in the design properties you can see list of properties which this component needs.

## Styling

## Pull request
