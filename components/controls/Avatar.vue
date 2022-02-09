<template>
    <div class="avatar">
        <div class="image" :style="{ backgroundImage: `url(${require('~/assets/images/avatar.jpeg')})`}" />
        <div class="data">
            <b>{{ name }}</b>
            <div class="role">{{ role }}</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Avatar',
    props: ['name', 'role']
}
</script>
