<script>
import vSelect from 'vue-multiselect';
import FilesInput from '../FilesInput';

export default {
    components: {
        'v-select': vSelect,
        'files-input': FilesInput,
    },

    props: {
        content: {
            type: Array,
            required: true
        }
    },

    methods: {
        handleInput(row, obj) {
            if (!obj.included && obj.new) {
                this.updateRow(row);
                this.addRow();
            }
        },

        updateRow(row) {
            this.content[row].included = true;
            this.content[row].new = false;
        },

        addRow() {
            this.content.push({
                included: false,
                key: null,
                value: null,
                description: null,
                new: true,
                type: 'text',
            });
        },

        removeRow(row) {
            this.content.splice(row, 1);
        },
    }
}
</script>

<template>
    <table class="w-full text-left table-collapse">
        <thead>
            <tr>
                <th class="p-4 border-gray-200 text-xs font-semibold text-gray-700 w-auto"></th>
                <th class="p-2 border-l border-gray-200 text-xs font-semibold text-gray-700 w-1/4">Key</th>
                <th class="p-2 border-l border-gray-200 text-xs font-semibold text-gray-700 w-1/4">Value</th>
                <th class="p-2 border-l border-gray-200 text-xs font-semibold text-gray-700 w-1/2">Description</th>
            </tr>
        </thead>
        <tbody class="align-baseline">
            <tr v-for="(reqBody, row) in content"
                :key="row"
                @mouseover="activateElmnt('body#' + row)"
                @mouseout="activateElmnt(null)">
                <td class="border-t border-gray-200 text-xs text-gray-800 text-center">
                    <input type="checkbox"
                        v-model="reqBody.included"
                        :class="reqBody.new ? 'hidden' : ''">
                </td>
                <td class="p-2 border-l border-t border-gray-200 text-xs text-gray-800 relative">
                    <input type="text"
                        class="mt-0 mb-0 appearance-none focus:outline-none w-full"
                        placeholder="Key"
                        v-model="reqBody.key"
                        @input="handleInput(row, reqBody)">

                    <div v-show="elementId==='body#' + row" class="w-1/4 absolute inset-y-0 right-0">
                        <v-select
                            class="min-vs capitalize text-gray-500 text-xs"
                            v-model="reqBody.type"
                            openDirection="bottom"
                            :allowEmpty="false"
                            :showLabels="false"
                            :searchable="false"
                            :options="['text','file']"></v-select>
                    </div>
                </td>
                <td class="p-2 border-l border-t border-gray-200 text-xs text-gray-800">
                    <input v-show="reqBody.type==='text'"
                        type="text"
                        class="mt-0 mb-0 appearance-none focus:outline-none w-full"
                        placeholder="Value"
                        v-model="reqBody.value">

                    <files-input v-show="reqBody.type==='file'" v-model="reqBody.value"></files-input>
                </td>
                <td class="p-2 border-l border-t border-gray-200 text-xs text-gray-800 relative">
                    <input type="text"
                        class="mt-0 mb-0 appearance-none focus:outline-none w-full"
                        placeholder="Description"
                        v-model="reqBody.description">

                    <a v-show="elementId==='body#' + row && !reqBody.new"
                        href="#"
                        class="font-bold absolute inset-y-0 right-0 flex items-center pr-3"
                        @click="removeRow(row)">

                        <svg class="h-3 w-3 fill-current text-gray-700 hover:text-gray-900" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M10 8.586L2.929 1.515 1.515 2.929 8.586 10l-7.071 7.071 1.414 1.414L10 11.414l7.071 7.071 1.414-1.414L11.414 10l7.071-7.071-1.414-1.414L10 8.586z"/></svg>
                    </a>
                </td>
            </tr>
        </tbody>
    </table>
</template>
