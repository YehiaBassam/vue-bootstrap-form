<template>
    <b-container>
        <b-form @submit.prevent="onSubmit" @reset="onReset" v-if="show">
            <div v-for="(question, index) in questions" :key="question.id" class="mb-4 border p-5">
                <b-row>
                    <div class="col-md-6">
                        <b-form-group :id="`input-group-${index}`" label="Email address:" :label-for="`input-${index}`"
                            description="We'll never share your email with anyone else.">
                            <b-form-input :id="`input-${index}`" v-model="question.form.email" type="email"
                                placeholder="Enter email" required></b-form-input>
                        </b-form-group>
                    </div>
                    <b-col md="6">

                        <h1>
                            {{ index }}
                        </h1>

                        <b-form-group :id="`input-group-${index}`" label="Your Name:" :label-for="`input-${index}`">
                            <b-form-input :id="`input-${index}`" v-model="question.form.name" placeholder="Enter name"
                                required></b-form-input>
                        </b-form-group>
                    </b-col>

                    <b-form-group :id="`input-group-${index}`" label="Food:" :label-for="`input-${index}`">
                        <b-form-select :id="`input-${index}`" v-model="question.form.food" :options="foods"
                            required></b-form-select>
                    </b-form-group>

                    <b-form-group class="my-4" :id="`input-group-${index}`" v-slot="{ ariaDescribedby }">
                        <b-form-checkbox-group v-model="question.form.checked" id="checkboxes-4"
                            :aria-describedby="ariaDescribedby">
                            <b-form-checkbox value="me">Check me out</b-form-checkbox>
                            <b-form-checkbox value="that">Check that out</b-form-checkbox>
                        </b-form-checkbox-group>

                        <b-form-group v-if="question.form.checked.includes('me')" :id="`input-group-${index}`"
                            label="Your Name:" :label-for="`input-${index}`">
                            <b-form-input :id="`input-${index}`" v-model="question.form.newName" placeholder="Enter name"
                                required></b-form-input>
                        </b-form-group>
                    </b-form-group>

                    <b-card class="mt-3" header="Form Data Result">
                        <pre class="m-0">{{ question.form }}</pre>
                    </b-card>
                </b-row>

                <div class="text-end">
                    <b-button class="mt-4" variant="danger" @click="removeForm(index)">delete</b-button>
                </div>
            </div>
            <div class="mb-3">
                <b-button class="mt-4 me-5" variant="success" @click="addForm">Add +</b-button>
            </div>
            <div class="mb-3">
                <b-button type="submit" variant="primary" class="me-5">Submit</b-button>
                <b-button type="reset" variant="danger">Reset</b-button>
            </div>
        </b-form>

    </b-container>
</template>

<script>
export default {
    data() {
        return {
            questions: [
                {
                    form: {
                        email: '',
                        name: '',
                        newName: '',
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
            console.log(this.questions);
            // alert(JSON.stringify(this.form))
        },
        onReset(event) {
            event.preventDefault()

            for (const q of this.questions) {
                console.log(q)
            }
            // Reset our form values
            // this.form.email = ''
            // this.form.name = ''
            // this.form.food = null
            // this.form.checked = []
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        }
    }
}
</script>

