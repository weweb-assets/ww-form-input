<template>
    <input
        v-if="content.globalSettings && content.globalSettings.type !== 'textarea'"
        class="ww-form-input"
        :class="{ editing: isEditing }"
        :type="content.globalSettings.type"
        :name="isEditing ? `${content.globalSettings.name}-editing` : content.globalSettings.name"
        :required="content.globalSettings.required"
        :placeholder="wwLang.getText(content.globalSettings.placeholder)"
        :style="style"
        :min="content.globalSettings.min"
        :max="content.globalSettings.max"
    />
    <textarea
        v-else-if="content.globalSettings"
        class="ww-form-input"
        :class="{ editing: isEditing }"
        :type="content.globalSettings.type"
        :name="content.globalSettings.name"
        :required="content.globalSettings.required"
        :placeholder="wwLang.getText(content.globalSettings.placeholder)"
        :style="[style, { resize: content.globalSettings.resize ? '' : 'none' }]"
        :rows="content.globalSettings.rows"
    />
</template>

<script>
/* wwEditor:start */
import { getSettingsConfigurations } from './configurations';
/* wwEditor:end */

export default {
    name: '__COMPONENT_NAME__',
    props: {
        content: Object,
        /* wwEditor:start */
        wwEditorState: Object,
        /* wwEditor:end */
    },
    wwDefaultContent: {
        globalSettings: {
            type: 'text',
            name: '',
            required: true,
            placeholder: {},
            min: 0,
            max: 10000,
            rows: 4,
            cols: 10,
            resize: false,
        },
        globalStyle: {
            fontSize: '15px',
            color: 'black',
        },
    },
    /* wwEditor:start */
    wwEditorConfiguration({ content }) {
        return {
            settingsOptions: {
                name: {
                    path: 'globalSettings.name',
                    label: { en: 'Name', fr: 'fr' },
                    type: 'Text',
                    options: {
                        placeholder: 'Name',
                    },
                },
                required: {
                    path: 'globalSettings.required',
                    label: { en: 'Required', fr: 'Requis' },
                    type: 'OnOff',
                },
                type: {
                    path: 'globalSettings.type',
                    label: { en: 'Input type', fr: 'fr' },
                    type: 'TextSelect',
                    options: {
                        options: [
                            { value: 'text', label: { en: 'Short answer', fr: 'Texte' } },
                            { value: 'textarea', label: { en: 'Long answer', fr: 'Texte' } },
                            { value: 'email', label: { en: 'Email', fr: 'Email' } },
                            { value: 'password', label: { en: 'Password', fr: 'Mot de passe' } },
                            { value: 'number', label: { en: 'Number', fr: 'Nombre' } },
                            { value: 'date', label: { en: 'Date', fr: 'Date' } },
                            { value: 'time', label: { en: 'Time', fr: 'Heure' } },
                            { value: 'tel', label: { en: 'Phone', fr: 'Téléphone' } },
                        ],
                    },
                },
                ...getSettingsConfigurations(content.globalSettings.type),
            },
        };
    },
    /* wwEditor:end */
    data() {
        return {
            wwLang: wwLib.wwLang,
        };
    },
    computed: {
        isEditing() {
            /* wwEditor:start */
            return this.wwEditorState.editMode === wwLib.wwSectionHelper.EDIT_MODES.CONTENT;
            /* wwEditor:end */
            // eslint-disable-next-line no-unreachable
            return false;
        },
        style() {
            if (!this.content || !this.content.globalStyle) return {};
            return {
                color: this.content.globalStyle.color || 'black',
                fontSize: `${this.content.globalStyle.fontSize || '15px'}`,
            };
        },
    },
};
</script>

<style lang="scss" scoped>
.ww-form-input {
    width: 100%;
    outline: none;
    font-family: inherit;
    border: inherit;
    &::placeholder {
        color: inherit;
        opacity: 0.7;
    }
    /* wwEditor:start */
    &.editing {
        pointer-events: none;
    }
    /* wwEditor:end */
}
</style>
