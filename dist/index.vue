<style src="./style.css"></style>
<template>
    <ul>
        <li v-for="item in data">
            <div :class="{bold: isFolder(item),'tree__item--noChild':!isFolder(item)}">
                <span class="tree__opened--status" v-if="isFolder(item)" @click="toggle(item)">[{{item.opened ? '-' : '+'}}]</span>
                <label @dblclick="toggle(item)">
                    <input v-if="checkbox" class="tree__checkbox" type="checkbox" v-model="item[checkedField]" @change="checkdChange(item)" @click.stop @dblclick.stop>
                    {{item[textField]}}
                </label>
            </div>
            <ul v-if="isFolder(item)" v-show="item.opened">
                <tree class="item" :textField="textField" :childrenField="childrenField" :checkbox="checkbox"
                      :data="item[childrenField]"></tree>
            </ul>
        </li>
    </ul>
</template>

<script type="text/javascript">
    import Vue from 'vue'
    export default {
        name: 'tree',
        props: {
            data: {
                type: Array
            },
            textField: {
                type: String,
                default: 'text'
            },
            childrenField: {
                type: String,
                default: 'children'
            },
            checkbox: {
                type: Boolean,
                default: false
            },
            checkedField:{
                type: String,
                default: 'checked'
            },
            cascadeCheck: {
                type: Boolean,
                default: true
            },
        },
        methods: {
            isFolder: function (item) {
                return item[this.childrenField] && item[this.childrenField].length
            },
            toggle: function (item) {
                if (this.isFolder(item)) {
                    if (item.opened !== undefined) {
                        this.$set(item, "opened", !item.opened)
                    } else {
                        this.$set(item, "opened", true)
                    }
                }
            },
            checkdChange(item){
                if (item&&this.cascadeCheck) {
                    changeAllChild(item, item.checked, this.childrenField)
                }
                this.$nextTick(()=>{
                    if (this.$parent.$options._componentTag.toLowerCase() !== "tree") {
                        this.$emit("check", this.getCheckedNodes())
                    } else {
                        this.$parent.checkdChange()
                    }
                })

                function changeAllChild(data, checked, childrenField) {
                    if (data[childrenField] && data[childrenField].length) {
                        for (let child of data[childrenField]) {
                            Vue.set(child, 'checked', checked)
                            changeAllChild(child, checked, childrenField)
                        }
                    }
                }
            },
            getCheckedNodes(){
                return getChecked(this.data,this.childrenField)
            }
        }
    }
    function getChecked(data, childrenField) {
        var arr = [];
        find(data)
        return arr

        function find(data) {
            for (let item of data) {
                if (item.checked) {
                    arr.push(item)
                }
                if (item[childrenField] && item[childrenField].length) {
                    find(item[childrenField])
                }
            }
        }
    }


</script>
