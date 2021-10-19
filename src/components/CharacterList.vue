<template>
    <div>{{favShown}}</div>
</template>
<script lang="ts">
import { defineComponent, inject } from 'vue'

export default defineComponent({
    inject: ['favShown'],
    setup() {
        const favShown = inject("favShown")
        console.log(favShown)
        return {}
    },
})
</script>
