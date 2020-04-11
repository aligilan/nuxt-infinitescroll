# nuxt-infinitescroll
An infinite scroll for nuxt applications  

# Requirements
bootstrap-vue: ^2.9.0

# Install
```Bash
npm install @aligilan/infinitescroll --save
```

# Usage
```Bash
...
modules:[
    '@aligilan/infinitescroll'
],
...
```

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
                    this.dataArray = this.dataArray.concat(result.data);
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

# Options
| Option | Description |
| ----- | ----- |
| :id | set an id for loader element. |
| :enough | flag to visible loader or hide. |
| :offset | add offset to current page scroll position, before reach loader position. |

# Event
@load-more: When page current scroll position reached loader position, will emit this event.
