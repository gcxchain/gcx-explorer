<template>
    <div id="app">
        <v-header></v-header>
        <div class="main-container">
            <div class="text-center" v-if="keywords">
                <div id="seg" class="btn-group btn-group-lg" role="group">
                    <router-link :to="{path:'/block/'+keywords}" class="btn btn-default">{{$t('header.block')}}
                    </router-link>
                    <router-link :to="{path:'/transaction/'+keywords}" class="btn btn-default">
                        {{$t('header.transaction')}}
                    </router-link>
                    <router-link :to="{path:'/account/'+keywords}" class="btn btn-default">{{$t('header.account')}}
                    </router-link>
                    <router-link :to="{path:'/asset/'+keywords}" class="btn btn-default">{{$t('header.asset')}}
                    </router-link>
                </div>
            </div>
            <router-view></router-view>
        </div>
        <modal-api></modal-api>
        <modal-about></modal-about>
        <v-footer></v-footer>
    </div>
</template>

<script>
    import 'bootstrap';
    import 'bootstrap/dist/css/bootstrap.css';
    import modalApi from './components/modals/modal-api.vue';
    import modalAbout from './components/modals/modal-about.vue';
    import header from './components/partial/Header.vue';
    import footer from './components/partial/Footer.vue';
    import { mapGetters } from 'vuex';

    export default {
        name: 'app',
        computed: {
            ...mapGetters({
                keywords: 'keywords'
            })
        },
        watch: {
            keywords () {
                this.keywordsChanged();
            }
        },
        methods: {
            keywordsChanged () {
                if (!this.keywords) {
                    if (this.$route.name !== 'Holdrank') {
                        this.$router.push('/');
                    }
                } else if (/^\d+$/.test(this.keywords)) { // block
                    this.$router.push(`/block/${this.keywords}`);
                } else if (this.keywords.length === 40) { // transaction
                    this.$router.push(`/transaction/${this.keywords}`);
                } else if (/^1.3.\d+$/.test(this.keywords) || (this.keywords.charCodeAt(0) <= 'Z'.charCodeAt(0) && this.keywords.charCodeAt(0) >= 'A'.charCodeAt(0))) { // account
                    this.$router.push(`/asset/${this.keywords}`);
                } else { // account
                    this.$router.push(`/account/${this.keywords}`);
                }
            }
        },
        components: {
            vHeader: header,
            vFooter: footer,
            modalApi,
            modalAbout
        }
    };
</script>

<style scoped>
    #seg {
        margin: 20px 10px;
    }

    .router-link-active {
        border-color: #008fcd !important;
        background: #008fcd !important;;
        color: #fff !important;;
    }
</style>

<style>
    .no-padding {
        padding: 0 !important;
    }

    .no-margin {
        margin: 0 !important;
    }
    .pace-done #app .main-container .container .panel-default > .panel-heading{
        color: #495060;
        background: #f8f8f9;
        border-bottom:none;
    }
    .pace-done #app .main-container .container .panel-default td,.pace-done #app .main-container .container .panel-default th{
        padding: 14px 8px;
    }
    .panel > .table:last-child > tbody:last-child > tr:last-child, .panel > .table-responsive:last-child > .table:last-child > tbody:last-child > tr:last-child,
     .panel > .table:last-child > tfoot:last-child > tr:last-child, .panel > .table-responsive:last-child > .table:last-child > tfoot:last-child > tr:last-child{
         border-radius:0;
     }
     #seg .router-link-active{
            border-color: #201b37 !important;
            background: #201b37 !important;
            color: #fff !important;
     }
</style>
