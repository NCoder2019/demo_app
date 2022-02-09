<!-- Please remove this file from your project -->
<template>
  <div class="bg-gray-50">
    <TabControl
      header="Tab App Demo"
      :tabItems="tabItems">
    </TabControl>
  </div>
</template>

<script>
import TabControl from './controls/TabControl.vue'
export default {
  components: {
    TabControl
  },
  data (){
    return {
      tabItems: [
                { name: 'Опросы', image: 'clipboard.png', link: 'questions' },
                { name: 'Пользователи', image: 'group.png', link: 'users' },
                { name: 'Черные списки', image: 'blacklist.png', link: 'blacklist' },
                { name: 'Колл-центр', image: 'phone-call.png', link: 'calls' }
            ]
    }
  },
  name: 'Layout'
}
</script>
