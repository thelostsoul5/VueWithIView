<!--
v-on="$listeners" 继承所有事件
-->
<template>
    <div id="tableWithPage">
        <div id="table">
        <Table v-bind="$attrs" v-bind:data="datas" v-bind:columns="columns"></Table>
        </div>
        <div style="margin: 10px;overflow: hidden">
            <div id="page" style="float: right;">
                <Page v-bind="$attrs" v-bind:total="total" v-bind:current="current" v-on="$listeners"></Page>
            </div>
        </div>
    </div>
</template>
<script>
export default {
  name: "TableWithPage",
  inheritAttrs: false,
  props: ["datas", "columns", "total", "current"]
};
</script>