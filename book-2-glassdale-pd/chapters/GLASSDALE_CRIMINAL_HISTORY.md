# Who Likely Done It?

## Introduction

> A few days have passed since you got your initial list of criminals showing up and Maggie seemed very pleased with that start. You have been reviewing the case file and taking some notes, but this morning, Maggie wants to talk about how to move forward.
>
> "You've been doing this a while, and you know as well as I do that people who commit crimes are, more often than not, the ones who commit it again in the future." Maggie flips through some pages in a file she has sitting on her desk, and then looks at you.
>
> "Take Gerald McReiny here," and she turns the file around so you can see him. "I nabbed him for theft back is 2001. He did two years in the state pen, and then 8 months after that, we had a rash of stolen property in a section of town that wasn't far from where he was staying."
>
> Maggie leans back and says, "It took us a whole week to review all of the files for past offenders to filter it down to a list of likely suspects." She raises her eyebrows at you, and pauses.
>
> "Ah, gotcha. If I could provide a way to look at only those criminals who committed a crime in the past, we could cut that work down to just a couple minutes instead of a week," you say happily.
>
> "Exactly!" Maggie exclaims.
>
> "See if you can get that working and then show me what you've got when it's done".
>
> You head on back to your desk to get to work.

## Goal

In this chapter, you are going to be implementing a `<select>` element so that you have a drop-down element on the screen that lists all of the crimes that have been committed by criminals in the past. Once the user selects a crime, then the only criminals that will be rendered are the ones that have committed that crime.

![filter criminals by crime animation](./images/filter-criminals-by-crime.gif)

The Glassdale PD API has a resource that gives you a collection of all possible crimes that have been committed.

http://criminals.glassdale.us/crimes

You need to create two, new components in your application

1. The **`ConvictionProvider`** component will `fetch` those crimes an export a `useConvictions()` method for other components to import.
1. The **`ConvictionSelect`** component, which will invoke `useConvictions()` and then iterate that collection to fill out the dropdown in the browser.

## Using the map() Array Method

Before you get started on your code, here is a review of how to use the `.map()` array method to iterate a collection, and convert every item in that collection into something else.

Assume that you have a list of people's full names in an array.

```js
const names = ['Caitlin Stein', 'Ryan Tanay', 'Leah Hoefling', 'Emily Lemmon', 'Bryan Nilson', 'Jenna Solis', 'Meg Ducharme', 'Madi Peper', 'Kristen Norris']
```

That's the raw data that should _**always remain unchanged**_. However, you want to display only a list of last names in a dropdown list in your user interface, you would need to build a new array containing those values. You can use `.map()` for that.

```js
const lastNames = names.map(name => name.split(" ")[1])

console.log(lastNames)
// ['Stein', 'Tanay', 'Hoefling', 'Lemmon', 'Nilson', 'Solis', 'Ducharme', 'Peper', 'Norris']
```

Now imagine you want to fill a dropdown box in your UI with last names. First, you would get a reference to an existing element, like a `section` or a `div`, and update its `.innerHTML` property like in the example below.

```js
const nameListContainer = document.querySelector(".names")

nameListContainer.innerHTML = `
    <select>
        ${
            names.map(name =>
                `<option>${name.split(" ")[1]}</option>`
            )
        }
    </select>
`
```

What's confusing, and also powerful, about the aboce code snippet is that you have string template **inside** another string template. That's the true power of the interpolation capability of `${}`. You can put any JavaScript expression inside those curly braces. The only limitation is that it is a single expression. You can't put multiple lines of code inside them. 😢

## Conviction Data Provider

The **`ConvictionProvider`** component will follow the same structure as your **`CriminalProvider`** component.

```js
let convictions = []

export const useConvictions = () => convictions

export const getConvictions = () => {
    // Load database state into application state with a fetch().
    // Make sure the last `then()` sets the local `convictions` variable
    // to what is in the response from the API.
}
```

## Conviction Select Component

This component will render a `<select>` element with child `<option>` elements for each crime committed in the API.

#### Starter Code

> **`glassdale/scripts/convictions/ConvictionSelect.js`**

```js
/*
 *   ConvictionSelect component that renders a select HTML element
 *   which lists all convictions in the Glassdale PD API
 */
import { useConvictions } from "./ConvictionProvider.js"

// Get a reference to the DOM element where the <select> will be rendered
const contentTarget = document.querySelector(".filters__crime")

// Component function that renders the HTML representation
const ConvictionSelect = () => {
    // Get all con
    const convictions = useConvictions()

    const render = convictionsCollection => {
        contentTarget.innerHTML = `
            <select class="dropdown" id="crimeSelect">
                <option value="0">Please select a crime...</option>

                Use interpolation here to invoke the map() method on
                the convictionsCollection

            </select>
        `
    }

    render(convictions)
}

export default ConvictionSelect
```
