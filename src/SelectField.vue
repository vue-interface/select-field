<script lang="ts">
import { ActivityIndicator } from '@vue-interface/activity-indicator';
import { FormControl } from '@vue-interface/form-control';
import { defineComponent } from 'vue';

export default defineComponent({

    name: 'SelectField',

    components: {
        ActivityIndicator,
    },

    extends: FormControl,

    props: {
        /**
         * The default class name assigned to the control element
         *
         * @param {String}
         * @default 'form-select'
         */
        formControlClass: {
            type: String,
            default: 'form-select'
        }   
    },

    computed: {
        controlClass() {
            return this.plaintext ?
                `${this.formControlClass}-plaintext`
                : this.formControlClass;
        }
    }

});
</script>

<template>
    <div :class="formGroupClasses">
        <slot name="label">
            <label
                v-if="label"
                ref="label"
                :for="id"
                :class="labelClass">
                {{ label }}
            </label>
        </slot>

        <div class="form-group-inner">
            <slot
                name="control"
                v-bind="{ bindEvents, controlAttributes }">
                <div
                    v-if="$slots.icon"
                    class="form-group-inner-icon"
                    @click="focus">
                    <slot name="icon" />
                </div>
                <select
                    ref="field"
                    v-model="model"
                    v-bind-events
                    v-bind="Object.assign({
                        ['disabled']: $attrs.readonly
                    }, controlAttributes)">
                    <slot />
                </select>
            </slot>

            <slot name="activity">
                <Transition name="select-field-fade">
                    <ActivityIndicator
                        v-if="activity"
                        key="activity"
                        ref="activity"
                        :type="indicator"
                        :size="indicatorSize || size" />
                </Transition>
            </slot>
        </div>

        <slot
            name="errors"
            v-bind="{ error, errors, id: $attrs.id, name: $attrs.name }">        
            <FormControlErrors
                v-if="!!(error || errors)"
                :id="$attrs.id"
                v-slot="{ error }"
                :name="$attrs.name"
                :error="error"
                :errors="errors">
                <div
                    invalid
                    class="invalid-feedback">
                    {{ error }}<br>
                </div>
            </FormControlErrors>
        </slot>
        
        <slot
            name="feedback"
            v-bind="{ feedback }">
            <FormControlFeedback
                v-slot="{ feedback }"
                :feedback="feedback">
                <div
                    valid
                    class="valid-feedback">
                    {{ feedback }}
                </div>
            </FormControlFeedback>
        </slot>

        <slot name="help">
            <small
                v-if="helpText"
                ref="help">
                {{ helpText }}
            </small>
        </slot>
    </div>
</template>

<style>
.select-field,
.select-field .form-group-inner {
    position: relative;
    transition: all .25s ease-in-out;
}

.select-field .activity-indicator {
    position: absolute;
    right: 1.25em;
    top: 50%;
    transform: translate(-1rem, -50%);
    transition: all .15s ease-in;
}

.select-field .activity-indicator-xs {
    font-size: .5em;
    right: 2em;
}

.select-field.has-activity .form-control-xs {
    padding-right: 3.75em;
}

.select-field .activity-indicator-sm {
    font-size: .5em;
    right: 2em;
}

.select-field.has-activity .form-control-sm {
    padding-right: 3em;
}

.select-field .activity-indicator-md {
    font-size: .666em;
}

.select-field.has-activity .form-control-md {
    padding-right: 3em;
}

.select-field .activity-indicator-lg {
    font-size: .75em;
}

.select-field.has-activity .form-control-lg {
    padding-right: 3em;
}

.select-field .activity-indicator-xl {
    font-size: 1em;
}

.select-field.has-activity .activity-indicator-xl ~ .form-control-xl {
    padding-right: 3.75em;
}

.select-field .activity-indicator {
    opacity: 1;
}

.select-field .select-field-fade-enter,
.select-field .select-field-fade-leave-to {
    opacity: 0;
}

.select-field.is-valid .valid-feedback,
.select-field.is-invalid .invalid-feedback {
    display: flex;
}

.select-field .form-control-icon {
    padding-left: 2em;
}

.select-field .form-group-inner-icon {
    position: absolute;
    top: 50%;
    left: .666em;
    width: 1rem;
    font-size: 1em;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    justify-content: center;
}

.select-field-sm .form-group-inner-icon {
    position: absolute;
    top: 50%;
    left: .666rem;
    width: .75em;
    font-size: 1.5em;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    justify-content: center;
}

.select-field-lg .form-group-inner-icon {
    position: absolute;
    top: 50%;
    left: .666rem;
    width: 1.25em;
    font-size: 1em;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    justify-content: center;
}

.select-field .activity-indicator {
    opacity: 1;
}

.select-field .select-field-fade-enter,
.select-field .select-field-fade-leave-to {
    opacity: 0;
}
</style>

