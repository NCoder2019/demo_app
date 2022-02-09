<template>
    <!--Main Tab Component-->
    <div class="tab-control">
        <div class="tab-items">
            <div class="tab-header">
            <center>
            <div class="logo"></div>
            <div><b>{{ header ? header.toUpperCase() : 'Demo' }}</b></div>
            </center>
            </div>
            <NuxtLink :to="item.link"
            @click="activeItem = index"
            :class="['tab-item', { 'active': $route.name == 'index-' + item.link }]"
            v-for="(item, index) in tabItems">
                <center>
                    <img v-if="item.image" class="image" :src="require(`~/assets/images/${item.image}`)"/>
                    <div class="name">{{ item.name }}</div>
                </center>
            </NuxtLink>
        </div>

        <div class="tab-body">
            <div class="tab-body-header">
                <div class="search-box">
                    <input type="searchbox" placeholder="Поиск по системе" />
                    <button><img width="28" src="~/assets/images/search.png" /></button>
                </div>
                <Avatar
                    name="Islam"
                    role="Администратор"
                ></Avatar>
            </div>
            <div class="route">
                <img width="21" src="~/assets/images/home.png" /> <span>/</span> <span>Опросы</span> <span>/</span> <span>Добавить опрос</span>
            </div>

            <div class="tab-view">
                <nuxt-child></nuxt-child>
            </div>
        </div>
    </div>
    <!---->
</template>

<script>
import Avatar from './Avatar.vue'
export default {
    name: 'TabControl',
    props: ['header', 'tabItems'],
    components: { Avatar },
    data (){
        return {
            activeItem: 0
        }
    }
}
</script>
