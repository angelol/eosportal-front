<template>
    <div class="container">
        <ChainNavigation />
        <div class="row">
            <router-view></router-view>
        </div>
    </div>
</template>

<script lang="ts">
    import { Component, Vue } from "vue-property-decorator";
    import { mapState, mapActions, mapMutations } from "vuex";

    import ChainNavigation from '../components/ChainNavigation';
    import {getChainProducers, getChainState} from "@/utils/eos.util";

    @Component({
        components: {
            ChainNavigation
        },
        props: {},
        computed: mapState(['chainId', 'producers']),
        methods: {
            ...mapActions(["setChain", "setNetwork", "setProducers", "setChainData", "logout"])
        }
    })

    export default class Chain extends Vue {
        producers: Array<any>;
        chainId: number;
        setChain: (chainId: number) => void;
        setNetwork: (networkString:string | null) => void;
        setProducers: (producers:any[]) => void;
        setChainData: (chainData:any) => void;
        logout:() => void;

        created() {
            this.setChain(this.$route.params.chainId);
            this.initialize();
        }

        destroyed(){
            this.setNetwork(null);
            this.setChainData(null);
            this.setProducers([]);
            this.logout();
        }

        async initialize(){
            //TODO: Get chain data from id, bind to state, use api url to build network for Scatter

            this.setNetwork('http://193.93.219.219:8888/');
            await this.setChainData(await getChainState());
            await this.setProducers(await getChainProducers());
        }
    }
</script>

<style lang="scss">

</style>