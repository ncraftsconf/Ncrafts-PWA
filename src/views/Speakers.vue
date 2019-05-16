<template>
    <div>
        <Menu />
    
        <div class="container">
            <div class="progress" v-show="loading">
                <div class="indeterminate"></div>
            </div>

            <div class="row">
                <Speaker v-for="(speaker, index) in speakers" v-bind:key="index" :speaker="speaker" />
            </div>
        </div>
    </div>
</template>

<script>
    import Menu from '../components/Menu.vue'
    import SpeakersService from '../services/SpeakersService'
    import Speaker from '../components/Speaker.vue'
    
    export default {
        data()
        {
            return {
                speakersService: new SpeakersService(),
                speakers: [],
                loading: true
            }
        },
        methods: {
            fetchDatas: function ()
            {
                let speakers = this.speakersService.get();
                this.speakers = speakers;
                this.loading = false;

                if(navigator.onLine) {
                    this.speakersService.fetch()
                            .then(speakers => this.speakers = speakers);
                }
            }
        },
        components: {
            Menu,
            Speaker
        },
        mounted() {
            this.fetchDatas();    
            $('.tabs').tabs();
            $(".button-collapse").sideNav();
        }
    }
</script>