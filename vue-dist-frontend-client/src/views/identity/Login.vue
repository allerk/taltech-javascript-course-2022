<template>
    <h1>Login</h1>

    <hr />
    <div class="row">
        <div class="col-md-4">
            <div v-if="errorMsg != null" class="text-danger validation-summary-errors" data-valmsg-summary="true">
                <ul>
                    <li>{{ errorMsg }}</li>
                </ul>
            </div>


            <div>
                <div class="form-group">
                    <label class="control-label" for="Email">Email</label>
                    <input v-model="email" class="form-control" type="email" />
                </div>
                <div class="form-group">
                    <label class="control-label" for="Password">Password</label>
                    <input v-model="password" class="form-control" type="password" />
                </div>
                <div class="form-group">
                    <input @click="loginClicked()" type="submit" value="Sumbit" class="btn btn-primary" />
                </div>
            </div>
        </div>
    </div>

</template>

<script lang="ts">
import { RouterLink, RouterView } from 'vue-router';
import { Options, Vue } from "vue-class-component";
import { IdentityService } from '@/services/IdentityService';
import { useIdentityStore } from '@/stores/identity';

@Options({
    components: {

    },
    props: {},
    emits: {}
})
export default class Login extends Vue {
    identityStore = useIdentityStore();

    email: string = '';
    password: string = '';
    errorMsg: string | null = null;

    identityService = new IdentityService();
    async loginClicked(): Promise<void> {
        console.log(this.email);
        console.log(this.password)
        let res = await this.identityService.login(this.email, this.password);
        console.log(res);
        console.dir(res);

        this.identityStore.$state.jwt = res.data!;
        this.$router.push('/')
    }
}
</script>