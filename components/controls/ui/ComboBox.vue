<template>
    <div id="combo" class="combo-box" @click="showMenu = !showMenu">
        <div class="name">{{ model.name }}<img width="14" :src="`${require('~/assets/images/down.png')}`" /></div>
        <div id="items" class="items" v-if="showMenu">
            <div class="item" v-for="(item, index) in items"
            @click="setItem(item)">{{ item.name }} <label v-if="addedItems && addedItems.some(a => a.id == item.id) && index != 0">Добавлено</label></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ComboBox',
    props: ['model', 'name', 'items', 'addedItems'],
    data (){
        return {
            showMenu: false,
            currentItem: null
        }
    },
    watch: {
        'showMenu'(to, from){
            if (to == true){
                setTimeout(
                    function() {
                        for (var i = 0; i < document.getElementsByClassName("items").length; i++)
                            document.getElementsByClassName('items')[i].style = "width: " + document.getElementById('combo').clientWidth + 'px'
                    }
                , 1)
            }
        }
    },
    methods: {
        setItem(item){
            if (this.addedItems) {
                if (!this.addedItems.some(a => a.id == item.id)){

            this.currentItem = item;
            
            this.model.id = item.id
            this.model.name = item.name

            //For conditions
            this.model.conditionId = item.id
            }
            }
            else {
                this.currentItem = item;
                
                this.model.id = item.id
                this.model.name = item.name
            }
        }
    }
}
</script>
