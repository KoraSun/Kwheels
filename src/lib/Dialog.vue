<template>
   <template v-if="visible">
       <Teleport to="body">
            <div class="k-dialog-overlay" @click="OnClickOverlay"></div>
                <div class="k-dialog-wrapper">
                    <div class="k-dialog">
                        <header v-if="TitleVisible">
                            <slot name='title' />
                            <span @click="close" class="k-dialog-close"></span> 
                        </header>
                        <main>
                            <slot />
                            <slot name='content'/>
                        </main>
                        <footer>
                            <Button level='main' @click="ok">确定</Button>
                            <Button v-if="CancelVisible" @click="cancel">取消</Button>
                        </footer>
                    </div>
                </div>
       </Teleport>  
    </template>
</template>

<script lang = "ts">
import Button from './Button.vue'
export default{
    props:{
        visible:{
            type:Boolean,
            default:false
        },
        TitleVisible:{
            type:Boolean,
            default:true
        },
        closeOnClickOverlay:{
            type:Boolean,
            default:true
        },
        ok:{
            type:Function
        },
        cancel:{
            type:Function
        },
        CancelVisible:{
            type:Boolean,
            default:true
        }
    },
    components:{Button},
    setup(props,context){
        const close=()=>{
            context.emit('update:visible',false)
        }
        const OnClickOverlay=()=>{
            if(props.closeOnClickOverlay){
                close()
            }
        }  
        const cancel=()=>{
            props.cancel&&props.cancel()
            close()
        }
        const ok=()=>{
            if(props.ok&&props.ok()!== false){
            close()
            }
        }
        return {close,OnClickOverlay,cancel,ok}
    }
}

</script>

<style lang="scss" >
$radius:4px;
$border-color:#d9d9d9;

.k-dialog{
    background:white;
    border-radius: $radius;
    box-shadow: 0 0 3px fade_out(black,0.5);
    min-width: 15em;
    max-width: 90%;
    &-overlay{
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: fade_out(black,0.5);
        z-index: 10;
    }
    &-wrapper{
        position: fixed;
        left: 50%;
        top: 50%;
        transform: translate(-50%,-50%);
        z-index: 11;
    }
    >header{
        padding: 12px 16px;
        border-bottom:1px solid $border-color;
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 20px;
    }
    >main{
        padding:12px 16px;
    }

    >footer{
        border-top: 1px solid $border-color;
        padding: 12px 16px ;
        text-align: right;
    }

    &-close{
        position: absolute;
        display: inline-block;
        width: 16px;
        height: 16px;
        right:10px;
        top: 10px;
        cursor:pointer;
        &::before,
        &::after{
        content: "";
        position: absolute;
        height: 1px;
        background: black;
        width: 100%;
        top: 50%;
        left: 50%;
        }

        &::before{
        transform: translate(-50%,-50%) rotate(-45deg);
        }

        &::after{
        transform: translate(-50%,-50%) rotate(45deg);
        }
        @media (min-width:500px) {
            &hover {
                background: #f57272;
                &::before,
                &::after{
                    background-color: #fff;
                }
            }  
        }
    }
}


</style>