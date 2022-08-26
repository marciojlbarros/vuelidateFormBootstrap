<script>
import useVuelidate from '@vuelidate/core'
import { required, email, minLength } from "@vuelidate/validators"

export default {
    name: 'FormVuelidate',
    setup(){
        return { v$: useVuelidate() }
    },
    data(){
        return {
            user: {
                firstName: "",
                lastName: "",
                email: "",
                password: ""
            },
            submitted: false
        }
    },
    validations: {
        user: {
            firstName: { required },
            lastName: { required },
            email: { required, email },
            password: { required, minLength: minLength(6) }
        }
    },
    methods: {
        handleSubmit() {
        this.submitted = true

        this.v$.$touch();
        if(this.v$.$invalid){
            return;
        }
         alert("SUCCESS!! :-)\n\n" + JSON.stringify(this.user));
        }
    }
}
</script>
<template>
    <div class="jumbotron">
        <div class="container my-5">
            <div class="row">
                <div class="col-sm-8 offset-sm-2">
                    <h3 class="mb-3">Vue.Js + Vuelidate - Form Validation</h3>
                    <form @submit.prevent="handleSubmit" class="row g-3">
                        <div class="form-group">
                            <label for="firstName">Primeiro Nome</label>
                            <input
                            type="text"
                            v-model="user.firstName"
                            id="firstName"
                            name="firstName"
                            class="form-control"
                            :class="{
                                'is-invalid': submitted && v$.user.firstName.$error,
                                'is-valid': submitted && !v$.user.firstName.$invalid
                                }"
                            >
                            <span v-if="submitted && v$.user.firstName.required"
                            class="invalid-feedback">Campo obrigatório!</span>
                        </div>
                        <div class="form-group">
                            <label for="lastName">Segundo Nome</label>
                            <input
                            type="text"
                            v-model="user.lastName"
                            id="lastName"
                            name="lastName"
                            class="form-control"
                            :class="{
                                'is-invalid': submitted && v$.user.lastName.$error,
                                'is-valid': submitted && !v$.user.lastName.$invalid
                                }"
                            >
                            <span v-if="submitted && v$.user.lastName.required"
                            class="invalid-feedback">Campo obrigatório!</span>
                        </div>
                        <div class="form-group">
                            <label for="email">Email</label>
                            <input
                            type="email"
                            v-model="user.email"
                            id="email"
                            name="email"
                            class="form-control"
                            :class="{
                                'is-invalid': submitted && v$.user.email.$error,
                                'is-valid': submitted && !v$.user.email.$invalid
                                }"
                            >
                            <span v-if="submitted && v$.user.email.$error"
                            class="invalid-feedback">Campo obrigatório!</span>
                            <span v-if="!v$.user.email.email"
                            class="invalid-feedback">Email inválido!</span>
                        </div>
                        <div class="form-group">
                            <label for="password">Digite sua senha</label>
                            <input
                            type="password"
                            v-model="user.password"
                            id="password"
                            name="password"
                            class="form-control"
                            :class="{
                                'is-invalid': submitted && v$.user.password.$error,
                                'is-valid': submitted && !v$.user.password.$invalid
                                }"
                            >
                                <div v-if="submitted && v$.user.password.$error" class="invalid-feedback">
                                    <span v-if="!v$.user.password.required">Password is required</span>
                                    <span v-else-if="!v$.user.password.minLength">Password must be at least 6 characters</span>
                                </div>
                        </div>

                        <div class="form-group">
                            <button class="btn btn-primary">Cadastre-se</button>
                        </div>
                        <pre>
                            {{ user }}
                        </pre>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>