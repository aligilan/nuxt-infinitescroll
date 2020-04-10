<template>
    <div :id="id">
        <b-overlay :show="!enough" rounded="lg">
            <template v-slot:overlay>
                <div class="d-flex align-items-center">
                    <b-spinner small type="grow" variant="primary"></b-spinner>
                    <b-spinner type="grow" variant="primary"></b-spinner>
                    <b-spinner small type="grow" variant="primary"></b-spinner>
                    <!-- We add an SR only text for screen readers -->
                    <span class="sr-only">لطفا صبر کنید...</span>
                </div>
            </template>
        </b-overlay>
    </div>
</template>

<script>
    export default {
        name: "InfiniteScroll",
        props:{
            id: {
                default: 'load-more'
            },
            offset:{
                default: 0,
            },
            enough:{
                default: false
            }
        },
        data(){
            return {
            }
        },
        methods:{
            handleScroll(){
                if (this.enough) return ;

                //==============with jquery
                // let element_toTop = $('#load-more').offset().top;
                // let current_distance = $('html,body').scrollTop() + window.innerHeight;
                //=========================

                let element_toTop = this.getElementOffset().top;
                let current_distance = document.documentElement.scrollTop + window.innerHeight;
                current_distance+this.offset > element_toTop && (this.$emit('load-more'));
            },
            getElementOffset() {
                let top = 0;
                let left = 0;
                let element = document.getElementById(this.id);

                // Loop through the DOM tree
                // and add it's parent's offset to get page offset
                do {
                    top += element.offsetTop || 0;
                    left += element.offsetLeft || 0;
                    element = element.offsetParent;
                } while (element);

                return {
                    top,
                    left,
                };
            },
        },
        beforeMount() {
            window.addEventListener('scroll', this.handleScroll);
        },
        beforeDestroy() {
            window.removeEventListener('scroll', this.handleScroll);
        }
    }
</script>

<style scoped>

</style>