<template>
    <MyTooltip
        v-model:active="showSelect"
        :triggers="['click']"
        :always_active="false"
    >
        <template #label>
            <div class="selectValue">{{ label }}</div>
        </template>
        <template #content>
            <div
                v-for="[key, value] in list"
                :key="key"
                @click=";(selected = key) | (label = value)"
            >
                <slot :value1="value"></slot>
            </div>
        </template>
    </MyTooltip>
    <button @click="$emit('reload')">обновить</button>
</template>

<script setup>
const props = defineProps(['list', 'modelValue', 'classList'])
const emit = defineEmits(['update:modelValue', 'reload'])
const selected = ref(props.modelValue)
const label = ref('')
const showSelect = ref(false)
watch(selected, (selected) => {
    emit('update:modelValue', selected)
})
onMounted(() => updateList(props.list, selected.value))

watch(
    () => props.list,
    (list) => updateList(list, selected.value)
)

watch(
    () => props.modelValue,
    (newValue) => updateList(props.list, newValue)
)

const updateList = (list = new Map(), selected = undefined) => {
    label.value = list.get(selected) ?? 'test'
}
</script>
<style>
.slectList {
    display: block;
    position: absolute;
    border: 1px solid black;
    border-radius: 5px;
    padding: 5px;
    background: inherit;
}

.mySelect {
    border: 1px solid black;
    border-radius: 3px;
    cursor: pointer;
    -moz-user-select: none;
    -khtml-user-select: none;
    user-select: none;
    background: #ffffff;
}

.selectValue {
    padding: 3px;
}
</style>
