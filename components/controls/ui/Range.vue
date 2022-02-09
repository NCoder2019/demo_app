<template>
    <div>
        <div class="row">
            <b>{{ name }}</b>
            <span>От</span>
            <div>
                <input type="number" v-bind:value="from" @input="$emit('from-input', $event.target.value)" />
            </div>
            <span>До</span>
            <div>
                <input type="number" v-bind:value="to" @input="$emit('to-input', $event.target.value)" />
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Range',
    props: ['name', 'from', 'to']
}
</script>
