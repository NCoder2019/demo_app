<template>
<div>
    <div class="header">Добавить опрос</div>
    <div v-for="(condition, index) in conditions" :class="['questions-generator',
    { 'light': condition.conditionId == 1 || condition.conditionId == null },
    { 'rose': condition.conditionId == 2 },
    { 'orange': condition.conditionId == 3 } ] ">
        <div class="row">
            <label>Условие {{ index + 1 }}</label>
        <ComboBox
            :items="items"
            :addedItems="conditions"
            :model="condition"
        ></ComboBox>
        </div>
        
        <div>
            <!--Range-->
        <div class="row" v-if="condition.conditionId == 1">
            <label>Диапазон</label>
            <div>
            <div class="row range" v-for="(range, index) in ranges" >
                <div class="box" v-if="ranges.length != 1 && index != 0">или</div>
                <Range 
                    :name="'Диапазон ' + index"
                    v-on:from-input="range.from = $event;"
                    v-on:to-input="range.to = $event"
                    :from="range.from"
                    :to="range.to">
                </Range>
            </div>
            <div>
                <button @click="ranges.push({ id: ranges.length + 1, from: 0, to: 0 })"><img width="16" src="~/assets/images/plus.png" /> Добавить диапазон</button>
            </div>
            </div>
        </div>
        <!---->

        <!--Card type-->
        <div class="row" v-if="condition.conditionId == 2">
            <label>Тип</label>
            <div>
            <div>
            <ComboBox v-for="item in addedTypes"
            :items="types"
            :addedItems="addedTypes"
            :model="item"

            style="width: 30rem; margin-top: 15px;"
        ></ComboBox>
        </div>
        <div class="row">
        <button v-if="addedTypes.length != types.length - 1" @click="addType"><img width="16" src="~/assets/images/plus.png" /> Добавить тип</button>
        </div>
            </div>
        </div>
        <!---->

        <!--Card status-->
        <div class="row" v-if="condition.conditionId == 3">
            <label>Статус</label>
            <div>
            <div>
            <ComboBox v-for="item in addedStatuses"
            :items="statuses"
            :addedItems="addedStatuses"
            :model="item"

            style="width: 30rem; margin-top: 15px;"
        ></ComboBox>
            </div>
            <div class="row">
        <button v-if="addedStatuses.length != statuses.length - 1" @click="addStatus"><img width="16" src="~/assets/images/plus.png" /> Добавить статус</button>
            </div>
            </div>
        </div>
        <!---->
        
        </div>
        <div><br /></div>
        <div class="row flex-end">
                <button class="remove-button" @click="conditions.splice(index, 1)">
                    <img width="18px" :src="`${require('~/assets/images/remove.png')}`" />
                    Удалить условие</button>
            </div>
            <div class="box" v-if="conditions.length != 0 && index != conditions.length - 1">И</div>
    </div>

    <div class="add-condition-button" @click="addCondition" v-if="conditions.length != items.length - 1">
        <div>
        <center><img width="28" :src="`${require('~/assets/images/plus.png')}`" /></center>
        <div>
            <p>Нажмите, чтобы добавить новое условие выборки.</p>
            <p>Все условия связываются между собой логическим "И"</p>
        </div>
        </div>
    </div>

    <center>
    <div class="data" v-if="showData">
        <b>Вы выбрали респодентов:</b>
        <p/>
        <b>С диапазонами</b>
        <div>
            <div v-for="range in ranges">{{ `${range.from} - ${range.to}` }}</div>
        </div>
        <b>С типами</b>
        <div>
            <div v-for="type in addedTypes" v-if="type.id">{{ type.name }}</div>
        </div>
        <b>Со статусами</b>
        <div>
            <div v-for="status in addedStatuses" v-if="status.id">{{ status.name }}</div>
        </div>
    </div>
    </center>

    <div class="footer">
        <button class="test-button" @click="showData = true">Протестировать опрос</button>
        <button class="submit-button" @click="sendRequest" v-if="!sending">Далее</button>
        <button class="submit-button" v-else>Отправка...</button>
    </div>
</div>
</template>

<script>
import ComboBox from './ComboBox.vue'
import Range from './Range.vue'

import axios from 'axios'

export default {
    components: { ComboBox, Range },
    name: 'QuestionGenerator',
    data (){
        return {
            conditions: [],
            items: [
                { name: 'Выберите категорию' },
                { id: 1, name: 'Возраст респондента' },
                { id: 2, name: 'Тип карты лояльности' },
                { id: 3, name: 'Статус карты лояльности' }
            ],

            types: [
                { name: 'Выберите тип' },
                { id: 1, name: 'Gold' },
                { id: 2, name: 'Metal' },
                { id: 3, name: 'Wood' }
            ],

            statuses: [
                { name: 'Выберите статус' },
                { id: 1, name: 'Активный' },
                { id: 2, name: 'Неактивный' }
            ],

            //For request
            ranges: [],
            addedTypes: [],
            addedStatuses: [],

            //Booleans
            sending: false,
            showData: false
        }
    },
    methods: {
        addCondition(){
            this.conditions.push({ id: null, name: this.items[0].name, conditionId: null })
        },
        addType(){
            this.addedTypes.push({ id: null, name: this.types[0].name })
        },
        addStatus(){
            this.addedStatuses.push({ id: null, name: this.statuses[0].name })
        },
        sendRequest(){
            this.sending = true
            this.showData = true

            //Ranges (Диапазоны возрастов)
            var ranges = this.ranges
            
            //Types (Создание массива ID'ов типов)
            var types = []
            for (var i = 0; i < this.types.length; i++)
                types.push(this.types[i].id)

            //Types (Создание массива ID'ов статусов)
            var statuses = []
            for (var i = 0; i < this.statuses.length; i++)
                statuses.push(this.statuses[i].id)

            //Создание общего объекта данных
            var request = {
                ranges, types, statuses
            }

            //Sending request (Отправка запроса с JSON data)
            axios.post(`/api/method`, request)
            .then(e => this.sending = false)
            .catch(e => this.sending = false)
        }
    }
}
</script>
