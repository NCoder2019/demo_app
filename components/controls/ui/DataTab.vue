<template>
    <div class="data-tab">
        <div class="tabs">
            <div :class="['tab', { 'active': $route.query.tab == index + 1 }]"
            @click="$router.replace({ name: $route.name, query: { tab: index + 1 } })"
            v-for="(item, index) in tabs">
                <div>{{ item.name }}</div>
            </div>
        </div>
        <div class="body">
            <div class="body">
                <question-generator v-if="$route.query.tab == 5"></question-generator>
            </div>
        </div>
    </div>
</template>

<script>
import QuestionGenerator from './QuestionGenerator.vue'
export default {
    name: 'DataTab',
    components: { QuestionGenerator },
    props: ['tabs']
}
</script>
