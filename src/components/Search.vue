<script setup>
const props = defineProps({
    modelValue: String
});

const emit = defineEmits(['update:modelValue']);

const updateValue = (event) => {
    emit('update:modelValue', event.target.value);
}

const clearValue = () => {
    emit('update:modelValue', '');
}
</script>
<template>
    <label for="search" class="label-search search-icon">
        <input type="text" placeholder="Поиск" id="search" class="search-input" :value="modelValue" @input="updateValue">
        <i v-show="modelValue" class="cross-icon" @click="clearValue"></i>
    </label>
</template>
<style scoped>
.search-input {
    font-style: normal;
    font-weight: 400;
    font-size: 15px;
    line-height: 108%;
    color: #000;
    height: 30px;
    padding: 0 27px 12px;
    border: none;
    outline: none;
    border-bottom: 1px solid #bfc9e0;
    width: 100%;
    max-width: 564px;
    transition: 0.3s;
}

.search-input::placeholder {
    font-style: italic;
    font-weight: 400;
    font-size: 15px;
    line-height: 108%;
    color: #8e9cbb;
}

.search-input:focus {
    border-color: #0066ff;
}

.label-search {
    position: relative;
}

.search-icon::before {
    content: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTYiIGhlaWdodD0iMTciIHZpZXdCb3g9IjAgMCAxNiAxNyIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEwLjcwMzcgMTFMMTQuNjU2OSAxNS4yNDI2TTEyLjE4MTMgN0MxMi4xODEzIDEwLjMxMzcgOS42NzgyNiAxMyA2LjU5MDY0IDEzQzMuNTAzMDEgMTMgMSAxMC4zMTM3IDEgN0MxIDMuNjg2MjkgMy41MDMwMSAxIDYuNTkwNjQgMUM5LjY3ODI2IDEgMTIuMTgxMyAzLjY4NjI5IDEyLjE4MTMgN1oiIHN0cm9rZT0iIzhFOUNCQiIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiLz4KPC9zdmc+Cg==);
    position: absolute;
    top: 0;
    left: 0;
}

.cross-icon {
    position: absolute;
    top: 0;
    right: 0;
    cursor: pointer;
}

.cross-icon::before {
    content: url(data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTIiIGhlaWdodD0iMTIiIHZpZXdCb3g9IjAgMCAxMiAxMiIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTAuMzI1NjA4IDEwLjc0NkMtMC4wNjQ5MTYyIDEwLjM1NTUgLTAuMDY0OTE2MiA5LjcyMjMzIDAuMzI1NjA4IDkuMzMxODFMOS4zNjMxMiAwLjI5Mjg5M0M5Ljc1MzY0IC0wLjA5NzYzMTMgMTAuMzg2OCAtMC4wOTc2MzA5IDEwLjc3NzMgMC4yOTI4OTNDMTEuMTY3OSAwLjY4MzQxOCAxMS4xNjc5IDEuMzE2NTggMTAuNzc3MyAxLjcwNzExTDEuNzM5ODIgMTAuNzQ2QzEuMzQ5MyAxMS4xMzY1IDAuNzE2MTMyIDExLjEzNjUgMC4zMjU2MDggMTAuNzQ2WiIgZmlsbD0iI0ZGMjM4RCIvPgo8cGF0aCBkPSJNMTAuNjgzNiAxMC42ODM1QzExLjA3NDEgMTAuMjkzIDExLjA3NDEgOS42NTk4NiAxMC42ODM2IDkuMjY5MzNMNi4xOTUzMyA0Ljc4MTExTDEuNzA3MTEgMC4yOTI4OTNDMS4zMTY1OCAtMC4wOTc2MzEzIDAuNjgzNDE3IC0wLjA5NzYzMDkgMC4yOTI4OTMgMC4yOTI4OTNDLTAuMDk3NjMxMSAwLjY4MzQxOCAtMC4wOTc2MzExIDEuMzE2NTggMC4yOTI4OTMgMS43MDcxMUw5LjI2OTM1IDEwLjY4MzVDOS42NTk4NyAxMS4wNzQxIDEwLjI5MyAxMS4wNzQxIDEwLjY4MzYgMTAuNjgzNVoiIGZpbGw9IiNGRjIzOEQiLz4KPC9zdmc+Cg==);
}
</style>