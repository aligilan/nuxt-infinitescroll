# nuxt-infinitescroll
An infinite scroll for nuxt applications  

# Install
```Bash
npm install @aligilan/infinitescroll --save
```

# Usage
```Bash
<template>
    <div>
        <ul>
            <li></li>
            <li></li>
            <li></li>
            <infinite-scroll :enough="enough" @load-more="getData()" />
        </ul>
    </div>
</template>
```
```Bash
<script>
    data(){
        return{
            dataArray: [],
            ...
            //=======infiniteScroll
            enough: false,
            page: 1,
            pageSize: 10,
            //=====================
            ...
        }
    },
    mounted(){
        this.getData();
    },
    methods:{
        ...
        //=======infiniteScroll
        async getData(){
            try{
                let result = await this.$axios.$get(`my-api-url?page=${this.page++}`);
                if(result.success == 'true'){
                    this.dataArray = this.this.dataArray.concat(result.data);
                    // Stop scroll-loader
                    result.data.length < this.pageSize && (this.enough = true);
                }
            }catch(error){
               console.log('error', error)
            }
        },
        //=====================
        ...
    }
</script>
```