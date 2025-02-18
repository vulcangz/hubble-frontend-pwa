<template>
    <div class="search-context-wrapper">
        <div class="container expand-content">
            <div class="row overlay-header">
                <hbl-button class="button-icon" @click.native="hideOffcanvasAction">
                    <div class="hidden-link-name" v-text="'Close'" />
                    <svg-icon icon="x" />
                </hbl-button>
                <div class="overlay-headline" v-text="'Customer Account'" />
            </div>

            <div v-if="!isLoading" class="row">
                <template v-if="isLoggedIn && !isGuest && !isLoading">
                    <lazy-customer-navigation v-on:logout-success="isLoggedIn = false" />
                </template>

                <template v-if="isLoggedIn && isGuest && !isLoading">
                    <div class="link-wrp">
                        <button class="button-primary logout-button" @click.prevent="logOutGuest">
                            {{ 'Quit guest session' }}
                        </button>
                    </div>
                </template>

                <div v-if="!isLoggedIn && !isLoading" class="col-12">
                    <lazy-customer-login-form v-on:login-success="isLoggedIn = true" />

                    <div class="register-form-wrp">
                        <div class="register-form">
                            <div class="headline headline-3" v-text="'I am not having an account yet'" />
                            <div class="subline">{{ 'Simply create a customer account in a few simple steps.' }}</div>
                            <hbl-button class="button-primary" @click.native="goToRegister">
                                {{ 'Register' }}
                            </hbl-button>
                        </div>
                    </div>
                </div>
            </div>

            <loader v-else />
        </div>
    </div>
</template>

<script>
import { mapActions, mapMutations, mapState } from 'vuex';
import apiClient from '@/utils/api-client';

export default {
    name: 'TheCustomerContext',

    data() {
        return {
            isLoading: true,
            isLoggedIn: false,
            isGuest: false,
        };
    },

    computed: {
        ...mapState({
            contextToken: (state) => state.modSession.contextToken,
        }),
    },

    async mounted() {
        try {
            let response = await this.fetchContext();

            if (response.data.customer != null) {
                this.isLoggedIn = response.data.customer.active;
                this.isGuest = response.data.customer.guest;
            }

            this.isLoading = false;
        } catch (e) {
            this.isLoading = false;
            throw e;
        }
    },

    methods: {
        ...mapActions({
            hideOffcanvasAction: 'modNavigation/hideOffcanvasAction',
        }),
        ...mapMutations({
            resetContextToken: 'modSession/resetContextToken',
            resetCart: 'modCart/resetCart',
        }),
        fetchContext: async function () {
            return await new apiClient().apiCall({
                action: 'get',
                endpoint: 'store-api/context',
                contextToken: this.contextToken,
            });
        },
        goToRegister: function () {
            this.$router.push({
                name: 'customer-login',
                query: { tab: 1 },
            });
        },
        logOutGuest: function () {
            this.resetContextToken();
            this.resetCart();
            this.isLoggedIn = false;
            this.isGuest = false;
        },
    },
};
</script>

<style lang="scss" scoped>
@import '~assets/scss/hubble/variables';
@import '~assets/scss/hubble/typography';

.register-form-wrp {
    .register-form {
        max-width: $form-max-width;
        margin: 0 auto;
        padding: 20px 0;
    }

    .headline {
        margin-bottom: 15px;
    }

    .subline {
        margin-bottom: 15px;
    }

    button {
        width: 100%;
    }

    .form-row {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;

        &.zip-city {
            .hbl-input-group:nth-child(1) {
                width: 80px;
            }

            .hbl-input-group:nth-child(2) {
                width: calc(100% - 90px);
            }
        }
    }
}

.loader-wrp {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 70vh;
}

@media (min-width: 768px) {
    .register-form-wrp {
        padding: 0;
    }
}
</style>
