<template>
    <default-field :field="field" :errors="errors">
        <template slot="field">
            <currency-input
                v-bind="options"
                :id="field.name"
                type="text"
                class="w-full form-control form-input form-input-bordered"
                :class="errorClasses"
                :placeholder="field.name"
                v-model="value"
                v-if="hasLoaded"
            />
        </template>
    </default-field>
</template>

<script>
import { FormField, HandlesValidationErrors } from 'laravel-nova'
import { CurrencyInput } from 'vue-currency-input'

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    components: { CurrencyInput },

    data: () => ({
        options: {
            currency: {
                suffix: '%',
            },
            locale: 'en',
            valueAsInteger: true,
            allowNegative: false,
            valueRange: { min: 0, max: 100 },
        },
        hasLoaded: false,
    }),

    mounted() {
        // It was acting weird, so has to wait until mounted to show the field
        this.hasLoaded = true
    },

    methods: {
        /*
         * Set the initial, internal value for the field.
         */
        setInitialValue() {
            if (!this.field.value) {
                this.value = null
            }

            this.value = this.field.value
        },

        /**
         * Fill the given FormData object with the field's internal value.
         */
        fill(formData) {
            if (!this.value) {
                this.value = null
            }

            formData.append(this.field.attribute, this.value)
        },

        /**
         * Update the field's internal value.
         */
        handleChange(value) {
            this.value = value
        },
    },
}
</script>
