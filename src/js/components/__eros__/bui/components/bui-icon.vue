<template>
    <text @click="_click($event)" class="iconfont" :style="{color: color, fontSize: size, 'font-family': 'bui-iconfont' }">{{getFontName}}</text>
</template>

<script>
    //引入he模块，使用它解决weex-template-compiler在编译阶段进行decode
    var he = require("../utils/he"),
        domModule = weex.requireModule("dom");

    module.exports = {
        beforeCreate() {
            domModule.addRule('fontFace',{
                'fontFamily': 'bui-iconfont',
                'src': "url('bmlocal://iconfont/icon.ttf')"
            });
        },
        data: function () {
            return {
                //weex字体图标目前只支持unicode格式
                iconItems:{
                    'left':'&#xe6e4;',
                    'right':'&#xe6b4;',
                    'item':'&#xe606;',
                    'setting':'&#xe7da;'
                }
            }
        },
        props: {
            name: {
                type: String
            },
            color: {
                type: String,
                default: '#9ea7b4'
            },
            size: {
                type: String,
                default: '40px'
            }
        },
        computed:{
            //匹配对应的字体图标的unicode
            getFontName: function() {
                return he.decode(this.iconItems[this.name]);
            }
        },
        methods: {
            "_click": function ($event) {
                this.$emit("click", $event);
            }
        }
    }
</script>
