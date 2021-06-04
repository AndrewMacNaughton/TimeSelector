# Timeselector
A simple Vue component that allows you to select a time.
It is built as a renderless component, with 1 view included for demonstration purposes.


The view and an associated control can be found at `/docs/src/.vuepress/components`

A demo can be found by running `npm run docs:dev`, and one will be hosted online very shortly.



## Installation
With npm or yarn - Currently not published.
```
yarn add timeselector
npm install timeselector
```

## Typical Usage
As this is a renderless component, there is complete flexibility to build your own view and controls.
Or feel free to use the provided one.

Creating your own view
You will need to access all of the exposed slots from the Component. You can see all the slots in the documentation here.
From there you can do whatever you like. Please take a look at the TimeSelector.vue for a thorough example

```
<template>
    <time-selector-component :selectedTime="{hours:'09',minutes:':00'}">
         <div class="start-time" slot-scope="{ <!-- Unpack the slot scopes-->
            times,
            items,
            visibility,
        }"
    >
            <div v-for="(item, index) in times" :key="index">{{item.value}}</div>
        </div>
    </time-selector-component>
</template>
```

 Using the provided default view
```
<template>
    <start-time :selectedTime="{hours:'09',minutes:':00'}">
        <template v-slot:noTimeIcon>
            <span class="material-icons smaller-icon">schedule</span>
        </template>
        09:00
    </start-time>
</template>
```

## Props
---

Name | Type | Required |  Default | Description
---| --- | --- | ---| ---|
hours | Array | false | Array ranging from 0-23.(['01,'02'...,'22','23']) | This is an array of strings displayed in the hours column. You can customize it by passing only the hours you want to see, eg ['09','10','11','12']
minutes | Array | False | Array ranging from 0-55 in intervals of 5. | This is an array of strings displayed in the minutes column. You can customize it by passing only the minutes you want to see, eg ['00','15','30','45']
seconds|Array |False |Array ranging from 0-55 in intervals of 5.| This is an array of strings displayed in the seconds column. You can customize it by passing only the seconds you want to see, eg ['00','15','30','45']
seperator|String | False |':'| The value to use for separating times. #CHECK TO SEE IF THIS IS STILL IN USE
selectedTime |Object | False |{hours:'00',minutes:'00'}.| The value that is selected when the control intially opens.

\
&nbsp;  
 
## Scoped Slots
Name | Type | Description
---  | --- | ---
times|Object |An object containing the control definition. Format {value: String, include: Boolean, key: String}
visibility|Object |An object that holds the state of the visible components. Format `{controls: Boolean, times:{ hours: Boolean, minutes: Boolean, seconds: Boolean }}`
showChild|Function |A function that shows the control based on the key passed in.Usage: `showChild('hours').`
updated |Function | A function that fires when a value is selected. This updates the selcted value and changes the visibility of the control to false. Usage `updated('hours', $event)`
clickAwayParent|Function | A function that fires when the control is visible but the user clicks outside of it. This allows the control to be hidden Usage `clickAwayParent()`
clickAwayControls |Function |A function that fires when an individual control is visible but the user clicks outside of it. This allows that control only to be hidden and still keep the rest of the control open Usage `clickAwayControls()`
displayTime |String, null| This is returns null when everything is set to '00'. or will return hours|seperator|minutes|seperator|seconds depending on what is configured.
showControls |Function | Used to show the control. Often will be used in conjuction with the displayTime variable to display the time and show the controls on click.Usage:`<div @click='showControls>   {{displayTime}}</div>`
close|Function |Emits the saved event and hides all the controls that are visible. Normally used in conjunction with a save button
