<script setup>

const props = defineProps({
    id: {
        type: String,
        required: true,
    },
    label: {
        type: String,
        required: false,
    },
    placeholder: {
        type: String,
        required: false,
        default: 'Select an option',
    },
    items: {
        type: Array,
        required: true,
    },
    valueKey: {
        type: String,
        required: true,
    },
    labelKey: {
        type: String,
        required: true,
    },
    modelValue: {
        type: String,
        default: '',
    },
});

const getLabel = (item) => {
    if (typeof item === 'string') {
        return item;
    }

    return item[props.labelKey];
};

const emit = defineEmits(['updateValue']);

const handleChange = (event) => {
    emit('updateValue', event.target.value);
};
</script>

<template>
    <div class="select-container py-3">
        <label v-if="label" :for="id" class="block mb-3 pb-2 text-sm font-medium text-gray-700">{{ label }}</label>
        <div class="flex">
            <select :id="id" :value="modelValue"
                class="block w-full px-3 py-2 text-base border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-primary-500 focus:border-primary-500"
                @change="handleChange">
                <option value="" disabled>{{ placeholder }}</option>
                <option v-for="item in items" :key="item[valueKey]" :value="getLabel(item)">
                    {{ getLabel(item) }}
                </option>
            </select>
        </div>
    </div>
</template>