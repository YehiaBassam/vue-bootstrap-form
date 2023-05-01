<template>
    <b-container>
        <b-form @submit="onSubmit" @reset="onReset" v-if="show">
            <b-row v-for="question in questions" :key="question.id">
                <div class="col-md-6">
                    <b-form-group id="input-group-1" label="Email address:" label-for="input-1"
                        description="We'll never share your email with anyone else.">
                        <b-form-input id="input-1" v-model="question.form.email" type="email" placeholder="Enter email"
                            required></b-form-input>
                    </b-form-group>
                </div>
                <b-col md="6">

                    <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                        <b-form-input id="input-2" v-model="question.form.name" placeholder="Enter name" required></b-form-input>
                    </b-form-group>
                </b-col>

                <b-form-group id="input-group-3" label="Food:" label-for="input-3">
                    <b-form-select id="input-3" v-model="question.form.food" :options="question.foods" required></b-form-select>
                </b-form-group>

                <b-form-group class="my-4" id="input-group-4" v-slot="{ ariaDescribedby }">
                    <b-form-checkbox-group v-model="question.form.checked" id="checkboxes-4" :aria-describedby="ariaDescribedby">
                        <b-form-checkbox value="me">Check me out</b-form-checkbox>
                        <b-form-checkbox value="that">Check that out</b-form-checkbox>
                    </b-form-checkbox-group>
                </b-form-group>

                <div class="my-3">
                    <b-button type="submit" variant="primary" class="me-5">Submit</b-button>
                    <b-button type="reset" variant="danger">Reset</b-button>
                </div>
                <b-card class="mt-3" header="Form Data Result">
                    <pre class="m-0">{{ question.form }}</pre>
                </b-card>
            </b-row>
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
                        food: null,
                        checked: []
                    },
                    foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
                },
            ],
            show: true
        }
    },
    methods: {
        onSubmit(event) {
            event.preventDefault()
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

