<template>
    <div class="row win">
        <div class="col-12">
            <div class="row justify-content-center">
                <FormComp/>
                <Email/>
            </div>
            <Submit/>
        </div>
    </div>
</template>
<script>
    import FormComp from './content/FormComp';
    import Email from './content/Email';
    import Submit from './content/Submit';

    export default {
        components: {
            FormComp,
            Email,
            Submit
        }
    }
</script>