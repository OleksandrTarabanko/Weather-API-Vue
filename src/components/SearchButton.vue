<script>
export default {
    props: {
        text: String,
        action: Function
    },
}
</script>

<template>
    <button @click="action">
        {{ text }}
    </button>
</template>