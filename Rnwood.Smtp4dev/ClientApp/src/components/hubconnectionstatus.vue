﻿<template>
    <el-button v-show="!connection || !connection.connected" size="small" :type="buttonType()" :icon="buttonIcon()" @click="buttonClick" :title="buttonTitle()">{{buttonText()}}</el-button>
</template>

<script lang="ts">
    
    import { Component, Vue, Prop, toNative } from "vue-facing-decorator";
    import HubConnectionManager from "../HubConnectionManager";

    @Component
    class HubConnectionStatus extends Vue {

        @Prop({ default: null })
        connection: HubConnectionManager | null = null;

        buttonType(): string {
            return this.connection && this.connection.connected ? "success" : this.connection && this.connection.started ? "warning" : "danger";
        }

        buttonText(): string {
var errorText = this.connection && this.connection.error ? "\nError: " + this.connection.error.message : "";

            return (this.connection && this.connection.connected ? "Auto refresh enabled" : this.connection && this.connection.started ? "Connecting..." : "Auto refresh disabled") + errorText;
        }

        buttonTitle(): string {
            var errorText = this.connection && this.connection.error ? "\nError: " + this.connection.error.message : "";

            return (this.connection && this.connection.connected ? "Auto refresh enabled - Click to disconnect" : this.connection && this.connection.started ? "Auto refresh re-connecting..." : "Auto refresh disabled - click to enable") + errorText;
        }

        buttonIcon(): string {
            return this.connection && this.connection.connected ? "success" : this.connection && this.connection.started ? "warning" : "warning";
        }


        buttonClick = () => {
            if (this.connection) {

                if (!this.connection.started) {
                    this.connection.start();
                } else {
                    this.connection.stop();
                }
            }
        }

        async created() {

        }

        async destroyed() {

        }

    }

    export default toNative(HubConnectionStatus)
</script>