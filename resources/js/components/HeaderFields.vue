<script>
import vSelect from 'vue-multiselect';
import HeaderFields from './../data/HeaderFields.json';

export default {
    components: {
        'v-select': vSelect,
    },

    props: ['value', 'list'],

    data() {
        return {
            fields: [],
            selectedField: this.value,
        }
    },

    computed: {
        placeholder() {
            return 'Select ' + this.list.slice(0, -1) + ' options';
        }
    },

    mounted() {
        this.list === 'keys'
            ? this.fields = HeaderFields.keys
            : this.fields = HeaderFields.values;
    },

    methods: {
        handleHeader() {
            this.$emit('input', this.selectedField);
        },

        addTag(newTag) {
            this.fields.push(newTag);
            this.selectedField = newTag;
            this.handleHeader();
        }
    }
}
</script>

<template>
    <div>
        <v-select
            class="min-vs select-hide text-xs"
            v-model="selectedField"
            openDirection="bottom"
            tag-placeholder="Add this as new header"
            :placeholder="placeholder"
            :showLabels="false"
            :show-no-results="false"
            :taggable="true"
            :options="fields"
            @input="handleHeader"
            @tag="addTag"></v-select>
    </div>
</template>
