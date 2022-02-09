<template>
    <div>
        <DataTab
            :tabs="tabs"
        ></DataTab>
    </div>
</template>

<script>
import DataTab from './../../components/controls/ui/DataTab.vue'

export default {
    name: 'Questions',
    components: { DataTab },
    data(){
        return {
            tabs: [
                { name: 'Параметры' },
                { name: 'Вопросы' },
                { name: 'Логика' },
                { name: 'Условия' },
                { name: 'Респонденты' },
            ]
        }
    }
}
</script>
