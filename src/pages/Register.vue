<template>
  <q-page class="window-height window-width row justify-center">
    <div
      v-if="isForm"
      class="column"
    >
      <div class="row">
        <q-card class="credentials bg-grey-2 shadow-1 q-mt-xl">
          <q-card-section>
            <div class="text-h5 text-center">{{ $t("createAccount") }}</div>
          </q-card-section>
          <q-card-section>
            <p
              v-if="errorMsg"
              class="text-red q-mt-md"
            >{{ errorMsg }}</p>
            <q-form class="q-gutter-md">
              <q-input
                clearable
                v-model="email"
                type="email"
                :label="$t('email')"
              />
              <q-input
                clearable
                v-model="password"
                type="password"
                :label="$t('password')"
              />
            </q-form>
          </q-card-section>
          <q-card-actions class="q-px-md q-pb-md">
            <q-btn
              unelevated
              no-caps
              color="primary"
              class="full-width"
              :label="$t('create')"
              @click="doRegister"
            />
          </q-card-actions>
        </q-card>
      </div>
    </div>
    <div
      v-else-if="isSuccess"
      class="column"
    >
      <div class="row">
        <h5 class="text-h5 q-my-md">
          {{ $t("confirmationMessage") }}
        </h5>
      </div>
    </div>
  </q-page>
</template>

<style lang="sass">
.q-card.credentials
  width: 300px
</style>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";

enum State {
  Form,
  Success
}

@Component
export default class PageLogin extends Vue {
  state = State.Form;
  errorMsg = "";

  email = "";
  password = "";

  get isForm() {
    return this.state === State.Form;
  }
  get isSuccess() {
    return this.state === State.Success;
  }

  doRegister() {
    this.$ccApi
      .registerUser(this.email, this.password)
      .then(() => (this.state = State.Success))
      .catch(err => {
        this.errorMsg = this.$t("errorMessage", {message: err.message}) as string;
      });
  }
}
</script>
