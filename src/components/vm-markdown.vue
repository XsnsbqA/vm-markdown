<template>
    <div class="vm-markdown" :style="{width: width, height:height}">
        <VmMarkdownMenu
                v-show="toolbar && editable"
                :bgMenu="themeValue.bgMenu"
                :menuBorder="themeValue.menuBorder"
                :menuColor="themeValue.menuColor"
                :hoverColor="themeValue.hoverColor"
                :id="id"
                @textChange="updateHtmlString"
        >
        </VmMarkdownMenu>
        <div class="content" >
            <div class="vm-markdown-edit" :style="{backgroundColor: themeValue.bgRight}">
            <!--<div class="vm-markdown-edit" :style="{backgroundColor: themeValue.bgRight}">-->
                <textarea v-if="height!='0'" class="vm-markdown-content" v-model="markdString" :placeholder="placeholder"></textarea>
            </div>
            <div class="vm-markdown-html" v-html="htmlString" :style="{backgroundColor: themeValue.bgLeft}">
            </div>
        </div>

    </div>
</template>
<style lang="scss" rel="stylesheet/scss">
    @import url('../assets/iconfont/iconfont.css');
    // @import url('//at.alicdn.com/t/font_395110_atvjh67aqsuzyqfr.css');
    .vm-markdown {
        background-color: white;
        border-radius: 4px;
        min-width: 600px;
        min-height: 100px;
        overflow: hidden;
        .content {
            display: flex;
            position: relative;
            box-sizing: border-box;
            overflow: hidden;
            height: calc(100% - 40px);
            width: 100%;
            text-align: left;
            font-size: 16px;
            border: 1px solid #eeeff1;
            border-top: none;
            .vm-markdown-edit, .render {
                height: 100%;
            }
            .vm-markdown-edit {
                width: 50%;
                box-sizing: border-box;
                outline: none;
                border-right: 1px solid #eeeff1;
                flex-shrink: 0;
                .vm-markdown-content {
                    width: 100%;
                    height: 100%;
                    padding: 15px;
                    overflow: auto;
                    box-sizing: border-box;
                    resize: none;
                    outline: none;
                    border: none;
                    background-color: transparent;
                    font-size: 14px;
                    color: #232323;
                    line-height: 24px;
                }
            }
            .vm-markdown-html {
                padding: 15px;
                overflow: auto;
                flex-grow: 1;
                font-size: 13px;
                line-height: 28px;
                word-wrap: break-word;
                h1{
                    font-size: 16px;
                }
                ul {
                    margin: 10px 20px;
                    list-style-type: square;
                    padding: 0;
                }
                ol {
                    margin: 10px 20px;
                    list-style-type: decimal;
                    padding: 0;
                }
                li {
                    display: list-item;
                    padding: 0;
                }
                hr {
                    margin: 10px 0;
                    border-top: 1px solid #eeeff1;
                }
                pre {
                    display: block;
                    margin: 10px 0;
                    padding: 5px;
                    border-radius: 4px;
                    background-color: #f2f2f2;
                    color: #656565;
                    font-size: 14px;
                }
                blockquote {
                    display: block;
                    border-left: 4px solid #ddd;
                    margin: 15px 0;
                    padding: 0 15px;
                }
                img {
                    margin: 20px 0;
                }
                a {
                    color: #41b883;
                }
                table {
                    border: 1px solid #eee;
                    border-collapse: collapse;
                }
                tr {
                    border: 1px solid #eee;
                }
                th {
                    padding: 5px 10px;
                    border-right: 1px solid #eee;
                    background-color: #f2f2f2;
                }
                td {
                    padding: 5px 10px;
                    border-right: 1px solid #eee;
                }
            }
        }
    }
</style>
<script type="text/ecmascript-6">
  import VmMarkdownMenu from './vm-markdown-menu.vue'
  import marked from 'marked'
  import theme from '../theme/theme.js'
  export default {
    name: 'VmMarkdown',
    components: {
      VmMarkdownMenu,
    },
    props: {
      theme: {
        type: String,
        default: 'default'
      },
      width: {
        type: String,
        default: '500px'
      },
      height: {
        type: String,
        default: '50px'
      },
      defaultText: {
        type: String,
        default: ''
      },
      toolbar: {
        type: Boolean,
        default: true
      },//是否展示toolbar
      editable: {
        type: Boolean,
        default: true
      },
      placeholder:{
        type:String,
        default:'新人首次沟通需填写8周计划, 以后每次沟通的时候做check, 8周计划可以在后续每周修改.转正谈话和季度谈话填写改进计划'
      },
      layout: {
        type: String,
        default: ''
      },
      id:{
        type:String,
        required:true
      }
    },
    data: function () {
      return {
        markdString: '',
        htmlString: ''
      }
    },
    computed: {
      themeValue: function () {
        if (theme.hasOwnProperty(this.theme)) {
          return theme[this.theme]
        } else {
          return theme.dark
        }
      }
    },
    methods: {
      updateHtmlString (data) {
        this.markdString = data
      },
      layoutControl () {
        let VmMarkdownLayout = document.querySelectorAll('.vm-markdown-layout')[this.id]
        let VmMarkdown = document.querySelectorAll('.vm-markdown')[this.id]
        let VmMarkdownEdit = document.querySelectorAll('.vm-markdown-edit')[this.id]
        let VmMarkdownArea = document.querySelectorAll('.vm-markdown-content')[this.id]
        let is = VmMarkdownLayout.querySelectorAll('i')
        if(this.layout){
          switch (this.layout) {
            case 'default' :
              VmMarkdownEdit.style.width = '50%'
              break;
            case 'right' :
              VmMarkdownEdit.style.width = '100%'
              break;
            case 'left' :
              VmMarkdownEdit.style.width = '0'
              break;
            case 'zoom' :
              if (VmMarkdown.style.position === 'fixed') {
                VmMarkdown.style.cssText = 'width:' + this.width + ';' +
                  'height:' + this.height + ';'
                // VmMarkdown.style.position = ''
                // VmMarkdown.style.left = ''
                // VmMarkdown.style.top = ''
                // VmMarkdown.style.margin = ''
                // VmMarkdown.style.width = this.width
                // VmMarkdown.style.height = this.height
              } else {
                VmMarkdown.style.position = 'fixed'
                VmMarkdown.style.left = '0'
                VmMarkdown.style.top = '0'
                VmMarkdown.style.margin = '0'
                VmMarkdown.style.width = '100%'
                VmMarkdown.style.height = '100%'
              }
              break
          }
        }
        if(this.editable){
          for (let i = 0; i < is.length; i++) {
            is[i].addEventListener('click', evt => {
              switch (is[i].dataset.layout) {
                case 'default' :
                  VmMarkdownArea.style.width = '100%'
                  VmMarkdownEdit.style.width = '50%'
                  break;
                case 'right' :
                  VmMarkdownArea.style.width = '100%'
                  VmMarkdownEdit.style.width = '100%'
                  break;
                case 'left' :
                  VmMarkdownArea.style.width = 'auto'
                  VmMarkdownEdit.style.width = '0'
                  break;
                case 'zoom' :
                  if (VmMarkdown.style.position === 'fixed') {
                    VmMarkdown.style.cssText = 'width:' + this.width + ';' +
                      'height:' + this.height + ';' + 'z-index:'+ 14+';'
                    // VmMarkdown.style.position = ''
                    // VmMarkdown.style.left = ''
                    // VmMarkdown.style.top = ''
                    // VmMarkdown.style.margin = ''
                    // VmMarkdown.style.width = this.width
                    // VmMarkdown.style.height = this.height
                  } else {
                    VmMarkdown.style.position = 'fixed'
                    VmMarkdown.style.left = '0'
                    VmMarkdown.style.top = '0'
                    VmMarkdown.style.margin = '0'
                    VmMarkdown.style.width = '100%'
                    VmMarkdown.style.height = '100%'
                    VmMarkdown.style.zIndex = 10001
                  }
                  break
              }
            })
          }
        }else{
          VmMarkdownEdit.style.width = '0'
        }
      },
      parseHtml () {
        let style = {
          ul: `
              margin: 1px 2px;
              list-style-type: square;
              padding: 0;
            `,
          ol: `
              margin: 10px 20px;
              list-style-type: decimal;
              padding: 0;
            `,
          li: `
              display: list-item;
              padding: 0;
            `,
          hr: `
              margin: 15px 0;
              border-top: 1px solid #eeeff1;
            `,
          pre: `
              display: block;
              margin: 10px 0;
              padding: 8px;
              border-radius: 4px;
              background-color: #f2f2f2;
              color: #656565;
              font-size: 14px;
             `,
          blockquote: `
                      display: block;
                      border-left: 4px solid #ddd;
                      margin: 15px 0;
                      padding: 0 15px;
                    `,
          img: `
               margin: 20px 0;
             `,
          a: `
            color: #41b883;
           `,
          table: `
                 border: 1px solid #eee;
                 border-collapse: collapse;
               `,
          tr: `
              border: 1px solid #eee;
            `,
          th: `
              padding: 8px 30px;
              border-right: 1px solid #eee;
              background-color: #f2f2f2;
            `,
          td: `
              padding: 8px 30px;
              border-right: 1px solid #eee;
            `
        }
        let html = document.getElementsByClassName('vm-markdown-html')[0]
        let tagNames = Object.keys(style)
        for (let i = 0; i < tagNames.length; i++) {
          let _tagNames = html.getElementsByTagName(tagNames[i])
          if (_tagNames.length > 0) {
            for (let j = 0; j < _tagNames.length; j++) {
              _tagNames[j].style.cssText = style[tagNames[i]]
            }
          }
        }
      },
      getHtml () {
//        let html = document.querySelectorAll('.vm-markdown-html')[this.id]
        this.$emit('gethtml', this.markdString)
      }
    },
    watch: {
      defaultText:function(value){
        this.markdString = value
      },
      markdString(value){
        marked.setOptions({
          renderer: new marked.Renderer(),
          gfm: true,
          tables: true,
          breaks: true,
          pedantic: false,
          sanitize: false,
          smartLists: true,
          smartypants: false
        })
        this.htmlString = marked(value)

        setTimeout(() => {
//          this.parseHtml()
          this.getHtml()
        }, 0)
      }
    },
    mounted () {
      this.markdString = this.defaultText
      this.layoutControl()
    }
  }
</script>
