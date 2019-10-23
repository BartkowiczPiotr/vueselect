<template>
    <div class="v-select">
        <div class="v-select-container">
            <div class="v-select-container__inner">
                <div v-if="!showSearch" class="v-selected" @click="showList($event)" :title="label ? display[label] : display">{{label ? display[label] : display}}</div>
                <input v-if="showSearch" class="v-selected v-select-search" type="text" v-model="search" />
            </div>
        </div>
        <span class="v-select-icon" :class="{'rotate' :rotate}">
            <slot name="icon">
                <svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 60 60"  xml:space="preserve">
                <g>
                    <path fill="#58595B" d="M31.601,39.612c-0.916,0.824-2.399,0.824-3.314,0l-27.6-24.9c-0.916-0.825-0.916-2.165,0-2.99l0,0
                        c0.916-0.826,2.399-0.826,3.314,0l27.599,24.9C32.517,37.447,32.517,38.786,31.601,39.612L31.601,39.612z"/>
                    <path fill="#58595B" d="M28.469,39.612c-0.915-0.826-0.915-2.165,0-2.99l27.598-24.901c0.915-0.826,2.401-0.826,3.315,0l0,0
                        c0.916,0.826,0.916,2.165,0,2.991l-27.599,24.9C30.868,40.437,29.384,40.437,28.469,39.612L28.469,39.612z"/>
                </g>
                </svg>
            </slot>
        </span>
        <div class="v-dropdown">
            <div v-for="(option, index) in filteredOptions" class="v-dropdown__item" :key="index" :class="[option.class ? option.class : '',{'v-dropdown-hide': option.hide ? true : false}]" @click="selectOption(option)">
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
        },

        searchable:{
            type: Boolean,
            default: false,
        }

    },
    data(){

      return{

        select: this.value,
        display: '',
        rotate: false,

        showSearch: false,
        search: '',

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

            const select = e.target.parentElement.parentElement.parentElement;

            let lists = document.querySelectorAll('.v-dropdown');

            if(lists){
                lists.forEach(element => {
                    element.classList.remove('active')
                    let icon = select.querySelector('.v-select-icon')
                    icon.classList.remove('rotate');
                });
            }

           const list = select.querySelector('.v-dropdown');
           list.classList.add('active');

           let icon = select.querySelector('.v-select-icon');
           icon.classList.add('rotate');

           this.setListPossition(select, list);
           this.enableSearch(select);

        },

        close(e){

            if(!e.target.classList.contains('v-selected') && !e.target.classList.contains('v-dropdown') && !e.target.classList.contains('v-select-search')){

                let lists = document.querySelectorAll('.v-dropdown');
                this.showSearch = false;
                this.search = '';

                if(lists){
                    lists.forEach(element => {
                        element.classList.remove('active')
                        let icon = element.parentElement.querySelector('.v-select-icon')
                        icon.classList.remove('rotate');
                    });
                }

            }

        },

        setListPossition(select, list){

           const space = window.innerHeight - select.getBoundingClientRect().top + select.offsetHeight;
           const dropdown = list.offsetHeight;

           list.style.bottom = '';
           list.style.top = select.offsetHeight + 'px';
           
           if(dropdown > space){

               list.style.top = '';
               list.style.bottom = select.offsetHeight + 'px';

           }

        },

        enableSearch(select){

            if(this.searchable){ 

                this.showSearch = true;

                setTimeout(() => {
                    select.querySelector('.v-select-search').focus();
                }, 100);
                

            }

        },

    },
    mounted(){

        this.findModel();
        document.addEventListener('click', this.close)

    },
    computed:{

        filteredOptions() {

            if(!this.label){
            
                if(typeof this.options[0] === 'object'){

                    return this.options;

                }
                
                return this.options.filter(option => {
                    return option.toLowerCase().includes(this.search.toLowerCase());
                })

            }else{

                return this.options.filter(option => {
                    return option[this.label].toLowerCase().includes(this.search.toLowerCase());
                })

            }

        }

    }

}

</script>
<style scoped>

.v-select{
    display:flex;
    position:relative;
    border-bottom:1px rgb(192, 192, 192) solid;
}

.v-select-container{
    overflow: hidden;
    width: 100%;
}

.v-select-container__inner{
    display: flex;
    align-items: center;
}

.v-select-icon{
    position: relative;
    padding: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
    right:0px;
    bottom:0px; 
}

.v-select-icon svg{
    width:8px;
    height:8px; 
}

.v-select-icon.rotate{
    transform: rotate(180deg);
}

.v-selected{
    display:block;
    position:relative;
    width:100%;
    height:32px;
    border-radius: 0;
    border:none;
    padding: 5px;
    cursor: default;
    text-align: left;
    overflow: hidden;
    outline: none;
    box-sizing: border-box;
    white-space: nowrap;
    font-size: inherit;
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