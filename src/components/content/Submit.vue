<template>
    <div class="row justify-content-center">
        <div class="col-12 ml-auto text-center">
            <div class="custom-control custom-checkbox custom-control-inline">
                <input type="checkbox" class="custom-control-input" id="agreed"
                name="agreed" >
                <label class="custom-control-label" for="agreed">Ja, ik accepteer de voorwaarden om mee te doen</label>
                <span class="err" v-show="errors.has('agreed')">*</span>
            </div>
            <div class="text-center">
                <button type="submit" class="btn btn-primary button" data-toggle="modal" data-target="#exampleModal">Ik wil winnen</button>
            </div>
        </div>
    </div>
</template>
<script>
    import Vue from "vue";
    import VeeValidate from 'vee-validate'
    
    export default {
        data: () => ({
            user: {
                agreed:false
            }
        }),
    }
</script>