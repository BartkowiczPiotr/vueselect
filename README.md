## Table of contents
* [General](#general)
* [Setup](#setup)
* [Usage](#usage)
* [Object options](#object-options)
* [Props](#props)
* [Slots](#slots)
* [To do](#to-do)

# General
Simple select vue component, that allows you to control selected options from array

component is created with:
* Vue 2.6.10

<img src="vueselect.png"/>

# Setup
install module locally with npm or yarn

`npm i @pbartkowicz/vueselect`

or

`yarn add @pbartkowicz/vueselect`

# Usage
Import

```
import vueselect from '@pbartkowicz/vueselect'
```

Register in components section

```
components:{
    vueselect,
},
```

Define array with select options

```
data(){

    return{

        yourArrayWitOptions:[
            'option1',
            'option2',
            'option3'
        ],

        output: '',

    }

}
```
Use component in your code

```
<vueselect :options="yourArrayWitOptions" v-model="output"/>
```

You allways can define array with objects

```
data(){

    return{

        yourArrayWitOptions:[
            {id: 1, name: 'option1'},
            {id: 2, name: 'option2'},
            {id: 3, name: 'option3'}
        ],

        output: '',

    }

}
```

And then define <b>label</b> prop for displayed value in select, and <b>reduce</b> prop for key in object for select output

```
<vueselect :options="yourArrayWitOptions" label="name" reduce="id" v-model="output"/>
```

For option named 'option2' output will be '2'

# Object options
When you will define array with objects, you can pass in to objects two options: 

| Option      | Type   | Default | Required | Description                                                                                 |
| ----------- | ------ | ------- | -------- | ------------------------------------------------------------------------------------------- |
| class       | String |         | No       | Name of class for option.                                                                   |
| hide        | Boolean|         | No       | Option to hide item.                                                                        |

```
data(){

    return{

        yourArrayWitOptions:[
            {id: 1, name: 'option1', class: 'your-class-for-option'},
            {id: 2, name: 'option2', hide: true},
            {id: 3, name: 'option3'}
        ],

        output: '',

    }

}
```

In this case 'option1' will have class "your-class-for-option" and 'option2' will not be displayed

# Props

| Prop        | Type   | Default | Required | Description                                                                                 |
| ----------- | ------ | ------- | -------- | ------------------------------------------------------------------------------------------- |
| options     | Array  |         | No       | Array with select options.                                                                  |
| label       | String |         | No       | Displayed value in select for array with objects.                                           |
| reduce      | String |         | No       | Select key in object for vueselect component output.                                        |

# Slots
There is avaible slot in case of no avaible options, default: "No options".

```
<vueselect :options="yourArrayWitOptions" label="name" reduce="id" v-model="output">
    <template v-slot:noOptions>
        No avaible options
    </template>
<vueselect/>
```

# To do
In next update I plan to add possibility to search in options