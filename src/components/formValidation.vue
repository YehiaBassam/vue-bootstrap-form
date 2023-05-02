<template>
    <b-container>
        <b-form @submit.prevent="onSubmit" @reset="onReset" v-if="show">
            <div v-for="(question, index) in questions" :key="question.id" class="mb-4 border p-5">
                <b-row>
                    <div class="col-md-6">
                        <b-form-group class="text-start" :id="`input-group-${index}`" label="Email address:"
                            :label-for="`input-${index}`" description="We'll never share your email with anyone else.">
                            <b-form-input :id="`input-${index}`" v-model="question.form.email" type="email"
                                placeholder="Enter email"></b-form-input>
                            <template v-if="$v.questions.$each[index].form.email.$error">
                                <div v-if="!$v.questions.$each[index].form.email.required" class="mt-2 text-danger">
                                    email is required *
                                </div>
                                <div v-else-if="!$v.questions.$each[index].form.email.email" class="mt-2 text-danger">
                                    email is invalid *
                                </div>
                            </template>
                        </b-form-group>
                    </div>
                    <b-col md="6">
                        <b-form-group class="text-start" :id="`input-group-${index}`" label="Your Name:"
                            :label-for="`input-${index}`">
                            <b-form-input :id="`input-${index}`" v-model="question.form.name" placeholder="Enter name" />
                            <span v-if="$v.questions.$each[index].form.name.$error &&
                                !$v.questions.$each[index].form.name.required
                                " class="mt-2 text-danger">
                                name is required *
                            </span>
                        </b-form-group>
                    </b-col>

                    <b-form-group class="text-start" :id="`input-group-${index}`" label="Your numbers:"
                        :label-for="`input-${index}`">
                        <b-form-input :id="`input-${index}`" v-model="question.form.numbers" placeholder="Enter numbers" />

                        <template v-if="$v.questions.$each[index].form.numbers.$error">
                            <div v-if="!$v.questions.$each[index].form.numbers.required" class="mt-2 text-danger">
                                numbers is required *
                            </div>
                            <div v-else-if="!$v.questions.$each[index].form.numbers.numbers" class="mt-2 text-danger">
                                must be numbers only *
                            </div>
                        </template>
                    </b-form-group>

                    <b-form-group class="my-4">
                        <b-form-textarea :id="`textarea-${index}`" v-model="question.form.text"
                            placeholder="Enter something..." rows="3" max-rows="6"></b-form-textarea>
                        <span v-if="$v.questions.$each[index].form.text.$error &&
                            !$v.questions.$each[index].form.text.required
                            " class="mt-2 text-danger">
                            text is required *
                        </span>
                    </b-form-group>

                    <b-form-group :id="`input-group-${index}`" label="Food:" :label-for="`input-${index}`">
                        <b-form-select :id="`input-${index}`" v-model="question.form.food" :options="foods"></b-form-select>

                        <span v-if="$v.questions.$each[index].form.food.$error &&
                            !$v.questions.$each[index].form.food.required
                            " class="mt-2 text-danger">
                            food is required *
                        </span>
                    </b-form-group>

                    <b-form-group class="my-4" :id="`input-group-${index}`" v-slot="{ ariaDescribedby }">
                        <b-form-checkbox-group v-model="question.form.checked" id="checkboxes-4"
                            :aria-describedby="ariaDescribedby">
                            <b-form-checkbox value="me">Check me out</b-form-checkbox>
                            <b-form-checkbox value="that">Check that out</b-form-checkbox>
                        </b-form-checkbox-group>

                        <b-form-group v-if="question.form.checked.includes('me')" :id="`input-group-${index}`"
                            label="Your New Name:" :label-for="`input-${index}`" class="mt-3">
                            <b-form-input :id="`input-${index}`" v-model="question.form.newName"
                                placeholder="Enter name"></b-form-input>
                            <span v-if="$v.questions.$each[index].form.newName.$error &&
                                !$v.questions.$each[index].form.newName.required
                                " class="mt-2 text-danger">
                                newName is required *
                            </span>
                        </b-form-group>
                    </b-form-group>
                </b-row>

                <div class="text-start">
                    <b-button class="mt-4" variant="danger" @click="removeForm(index)">delete</b-button>
                </div>
            </div>
            <div class="mb-3">
                <b-button class="my-4" variant="success" @click="addForm">Add +</b-button>
            </div>
            <div class="mb-3">
                <b-button type="submit" variant="primary" class="me-5">Submit</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </div>
        </b-form>

    </b-container>
</template>

<script>
// vuelidate on :
// text - textarea - select
// requiredIf - Custom Validation 
// Form Builder

import { required, minLength, email, requiredIf, helpers } from 'vuelidate/lib/validators';

const numbersOnly = helpers.regex(
    "numbersOnly",
    /^[0-9]*$/
);

export default {
    validations: {
        questions: {
            $each: {
                form: {
                    name: {
                        required,
                        minLength: minLength(1),
                        // between: between(2, 30),
                    },
                    email: {
                        required,
                        email
                    },
                    text: {
                        required,
                    },
                    food: {
                        required,
                    },
                    numbers: {
                        required,
                        numbersOnly
                    },
                    newName: {
                        required: requiredIf(function (data) { // data means all data in $each
                            return data.checked.includes('me');
                        })
                    }
                }
            }
        },
    },
    data() {
        return {
            questions: [
                {
                    form: {
                        email: '',
                        name: '',
                        newName: '',
                        text: '',
                        numbers: 0,
                        food: null,
                        checked: []
                    },
                },
            ],
            foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
            show: true
        }
    },
    methods: {
        addForm() {
            this.questions.push(
                {
                    form: {
                        email: '',
                        name: '',
                        text: '',
                        numbers: 0,
                        food: null,
                        checked: []
                    },
                }
            )
        },
        removeForm(index) {
            this.questions.splice(index, 1);
        },
        onSubmit() {
            this.$v.$touch();

            if (this.$v.$invalid) {
                console.log('error')
                return
            }
            console.log('success', this.questions);
        },
        onReset() {
            for (const q of this.questions) {
                q.form.email = "";
                q.form.name = "";
                q.form.text = "";
                q.form.food = null;
                q.form.numbers = 0;
                q.form.checked = [];
            }
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        }
    }
}
</script>

