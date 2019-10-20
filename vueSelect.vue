<template>
    <div class="v-select">
        <div class="v-selected" @click="showList($event)">{{label ? display[label] : display}}</div>
        <span class="v-select-icon" :class="{'rotate' :rotate}">
            <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 60 60"  xml:space="preserve">
            <g id="Layer_1">
                <path fill="#58595B" d="M31.601,39.612c-0.916,0.824-2.399,0.824-3.314,0l-27.6-24.9c-0.916-0.825-0.916-2.165,0-2.99l0,0
                    c0.916-0.826,2.399-0.826,3.314,0l27.599,24.9C32.517,37.447,32.517,38.786,31.601,39.612L31.601,39.612z"/>
                <path fill="#58595B" d="M28.469,39.612c-0.915-0.826-0.915-2.165,0-2.99l27.598-24.901c0.915-0.826,2.401-0.826,3.315,0l0,0
                    c0.916,0.826,0.916,2.165,0,2.991l-27.599,24.9C30.868,40.437,29.384,40.437,28.469,39.612L28.469,39.612z"/>
            </g>
            <g id="Layer_2" display="none">
                <path display="inline" fill="#231F20" d="M31.694,53.612c-0.916,0.824-2.399,0.824-3.314,0l-27.6-24.9
                    c-0.916-0.825-0.916-2.165,0-2.99l0,0c0.916-0.826,2.399-0.826,3.314,0l27.599,24.9C32.61,51.447,32.61,52.786,31.694,53.612
                    L31.694,53.612z"/>
                <path display="inline" fill="#231F20" d="M28.652,53.59c-0.824-0.915-0.824-2.398,0-3.313l24.9-27.6
                    c0.825-0.916,2.165-0.916,2.99,0l0,0c0.826,0.916,0.826,2.399,0,3.314l-24.9,27.599C30.817,54.506,29.479,54.506,28.652,53.59
                    L28.652,53.59z"/>
            </g>
            </svg>
        </span>
        <div class="v-dropdown">
            <div v-for="(option, index) in options" class="v-dropdown__item" :key="index" :class="[option.class ? option.class : '',{'v-dropdown-hide': option.hide ? true : false}]" @click="selectOption(option)">
                {{label ? option[label] : option}}
            </div>
            <div v-if="options.length == 0" class="v-dropdown__item">
                <slot name="noOptions">
                    No options
                </slot>
            </div>
        </div>
    </div>
</template>
<script>

export default {

    name: 'vueselect',
    props:{

        value:{
            default:'',
        },

        options:{
            type: Array,
            default: () => [],
        },

        label:{
            type: String,
            default: '',
        },

        reduce:{
            type: String,
            default: '',
        }

    },
    data(){

      return{

        select: this.value,
        display: '',
        rotate: false,

      }

    },
    watch:{

      value(){

        this.findModel();

      },

      options(){

        this.findModel();

      },

    },
    methods:{

        findModel(){

            if(this.reduce){

                let checkDisplay = this.options.find(o => o[this.reduce] == this.value);

                if(!checkDisplay){

                    this.display = '';

                }else{

                    this.display = checkDisplay;

                }

                return;

            }

            this.display = this.value;

        },

        selectOption(option){

            this.display = option

            if(this.reduce){

                this.$emit('input', option[this.reduce])
                return;

            }
            
            this.$emit('input', option)

        },

        showList(e){

            let lists = document.querySelectorAll('.v-dropdown');

                if(lists){
                    lists.forEach(element => {
                        element.classList.remove('active')
                        let icon = element.parentElement.querySelector('.v-select-icon')
                        icon.classList.remove('rotate');
                    });
                }

           let list = e.target.parentElement.querySelector('.v-dropdown');
           list.classList.add('active');

           let icon = e.target.parentElement.querySelector('.v-select-icon');
           icon.classList.add('rotate');

        },

        close(e){

            if(!e.target.classList.contains('v-selected') && !e.target.classList.contains('v-dropdown')){

                let lists = document.querySelectorAll('.v-dropdown');

                if(lists){
                    lists.forEach(element => {
                        element.classList.remove('active')
                        let icon = element.parentElement.querySelector('.v-select-icon')
                        icon.classList.remove('rotate');
                    });
                }

            }

        },

    },
    mounted(){

        this.findModel();
        document.addEventListener('click', this.close)

    }

}

</script>
<style scoped>

.v-select{
    position:relative;
}

.v-select-icon{
    position: absolute;
    right:-5px;
    bottom:2px; 
}

.v-select-icon svg{
    width:8px;
    height:8px; 
}

.v-select-icon.rotate{
    transform: rotate(180deg);
}

.v-selected{
    position:relative;
    width:100%;
    border-bottom:1px rgb(192, 192, 192) solid;
    padding: 5px;
    cursor: default;
    text-align: left;
    min-height: 10px;
}

.v-dropdown{
    position:absolute;
    width:auto;
    min-width: 100%;
    max-height: 300px;
    overflow: hidden;
    overflow-y: auto;
    word-break: break-word;
    left: 0px;
    z-index: 100;
    border:1px rgb(192, 192, 192) solid;
    background:#fff;
    display:none;
    box-shadow: 0px 0px 8px 0px #d1d1d1;
    cursor: default;
    text-align: left;
}

.v-dropdown.active{
    display:block;
}

.v-dropdown__item{
    padding:10px 5px;
}

.v-dropdown__item:hover{
    background:rgb(233, 233, 233);
}

.v-dropdown-hide{
    display:none;
}

</style>