﻿<template>

        <el-popover placement="bottom"
                    width="160"
                    v-model="visible"
                    trigger="manual">
            <p>{{message}}</p>
            <div style="text-align: right; margin: 0">
                <el-button size="small" type="text" @click="cancel()">cancel</el-button>
                <el-button size="small" type="text" v-if="alwaysIsAvailable()" @click="confirm(true)">always</el-button>
                <el-button type="primary" size="small" @click="confirm(false)">confirm</el-button>
            </div>
            <template #reference>
                <slot @click="getConfirmation"></slot>
            </template>
        </el-popover>
 


</template>

<script lang="ts">
    import { Component, Vue, Prop, toNative, Emit } from 'vue-facing-decorator';
        

    @Component
    class ConfirmationDialog extends Vue {
 
        @Prop({ default: "" })
        message!: string;

        @Prop({ default: undefined })
        alwaysKey: string | undefined;

        visible: boolean = false;


        alwaysIsAvailable() {
            return this.alwaysKey && window.localStorage;
        }

        cancel() {
            this.visible = false;
        }

        @Emit("confirm")
        confirm(always: boolean) {

            if (always) {
                window.localStorage.setItem("always-" + this.alwaysKey, "true");
            }

            this.visible = false;
        }

        private checkReferenceVisible() {
            if (this.visible) {

                if (this.$slots.default && this.$slots.default().length > 0) {
                    if ((!this.$slots.default()![0]!.el as any).offsetParent) {
                        this.visible = false;
                    }
                }

                setTimeout(this.checkReferenceVisible, 100);
            }
        }

        getConfirmation() {


            if (this.alwaysIsAvailable() && window.localStorage.getItem("always-" + this.alwaysKey) === "true") {
                this.confirm(false);
            } else {
                this.visible = true;
                setTimeout(this.checkReferenceVisible, 100);
            }
        }



    }

    export default toNative(ConfirmationDialog);
</script>