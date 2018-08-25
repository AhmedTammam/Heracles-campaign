<template>
<div class="col-12">
     <Message v-if="isSubmit" />

    <div v-else class="row justify-content-center">
        <form class="form"  @submit.prevent="onSubmit">
            <div class="row">
                <div class="col-lg-6 col-sm-12 l-form">
                    <div class="form-group">
                        <label for="firstName">Voornaam<span class="err" v-show="errors.has('firstName')">*</span></label>
                        <input class="form-control" id="firstName" name="firstName" type="text"
                            v-model="user.firstName"
                            v-validate="'required|alpha'"
                            :class="{'input': true, 'is-danger': errors.has('firstName') }">
                    </div>
                    <div class="form-group">
                        <label for="lastName">Achternaam<span class="err" v-show="errors.has('lastName')">*</span></label>
                        <input type="text" class="form-control" id="lastName" name="lastName"
                            v-model="user.lastName"
                            v-validate="'required|alpha'"
                            :class="{'input': true, 'is-danger': errors.has('lastName') }">
                    </div>
                    <div class="row">
                        <div class="col-md-7">
                            <label class="d-block">Geslacht<span class="err" v-show="errors.has('gender')">*</span></label>
                            <div class="custom-control custom-radio custom-control-inline">
                                <input type="radio" class="custom-control-input" id="defaultInline1"
                                name="gender" v-validate="'required|included:female,male'" value="male" @click="setGenderToMale">
                                <label class="custom-control-label" for="defaultInline1">MAN</label>
                            </div>
                            <div class="custom-control custom-radio custom-control-inline">
                                <input type="radio" class="custom-control-input" id="defaultInline2"
                                name="gender" value="female" @click="setGenderToFemale">
                                <label class="custom-control-label" for="defaultInline2">VROUW</label>
                            </div>
                        </div>
                        <div class="col-md-5 my-1">
                            <label class="d-block" for="date">Geboortedatum
                                <span class="err" v-show="errors.has('dateOfBirth')">*</span>
                            </label>
                            <masked-input
                                type="text"
                                id="date"
                                name="dateOfBirth"
                                class="form-control"
                                v-model="user.dateOfBirth"
                                :mask="[/\d/, /\d/,'/',/\d/,/\d/,'/',/\d/, /\d/, /\d/, /\d/]"
                                :class="{'input': true, 'is-danger': errors.has('dateOfBirth') }"
                                :guide="true"
                                placeholderChar="_"
                                placeholder="DD/MM/YYYY"
                                v-validate="'required'">
                            </masked-input>
                        </div>
                    </div>
                    <div class="form-group">
                        <label for="email">E-mailadres<span class="err" v-show="errors.has('email')">*</span></label>
                        <input id="email" name="email" v-model="user.email" type="email" class="form-control" v-validate="'required|email'" :class="{'input': true, 'is-danger': errors.has('email') }">
                    </div>
                </div>
                <div class="col-lg-6 col-md-12 email">
                    <h2 class="text-center">Met welke vrienden wil jij dezeervaring delen?</h2>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon1">1</span>
                        </div>
                        <input type="text" class="form-control" placeholder="E-mailadres vriend" aria-label="Username" aria-describedby="basic-addon1">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon1">2</span>
                        </div>
                        <input type="text" class="form-control" placeholder="E-mailadres vriend" aria-label="Username" aria-describedby="basic-addon1">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon1">3</span>
                        </div>
                        <input type="text" class="form-control" placeholder="E-mailadres vriend" aria-label="Username" aria-describedby="basic-addon1">
                    </div>
                    <div class="input-group mb-3">
                        <div class="input-group-prepend">
                            <span class="input-group-text" id="basic-addon1">4</span>
                        </div>
                        <input type="text" class="form-control" placeholder="E-mailadres vriend" aria-label="Username" aria-describedby="basic-addon1">
                    </div>
                </div>
            </div>
            <div class="row justify-content-center">
                <div class="col-12 ml-auto text-center">
                    <div class="custom-control custom-checkbox custom-control-inline">
                        <input type="checkbox" class="custom-control-input" id="agreed"
                        name="agreed" v-model="user.agreed" v-validate="'required'" :class="{'input': true, 'is-danger': errors.has('agreed') }" >
                        <label class="custom-control-label" for="agreed">Ja, ik accepteer de voorwaarden om mee te doen</label>
                        <span class="err" v-show="errors.has('agreed')">*</span>
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn btn-primary button" data-toggle="modal" data-target="#exampleModal">Ik wil winnen</button>
                    </div>
                </div>
            </div>
        </form>
    </div>
</div>
</template>

<script>
import Vue from "vue";
import VeeValidate from 'vee-validate'
import MaskedInput from 'vue-text-mask'
import Message from './Message.vue'

Vue.use(VeeValidate);

export default {
  name: 'FormComp',
  components: {
      MaskedInput,
      Message
    },
  data: () => ({
      user: {
        firstName: '',
        lastName: '',
        email: '',
        gender: '',
        dateOfBirth: '',
        agreed: false
      },
      isSubmit: false
  }),
  methods: {
    setGenderToMale() {
        this.gender = 'male'
    },
    setGenderToFemale() {
        this.gender = 'female'
    },
    onSubmit() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.$http.post('https://jsonplaceholder.typicode.com/posts', this.user )
            .then( res => {
                this.isSubmit = true
            }, err => {
                console.log(err);
            })
          
         return; 
        }

        alert('Please fill in all fields correctly and accept our terms and conditions.');
      });
    }
  }
}
</script>

<style>
    .err{
        margin-left: 10px;
        color:red;
        font-size: 18px;
        text-align: center;
    }
</style>
