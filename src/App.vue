<template>
  <div id="app">



    <VmMarkdown :theme="theme"
                v-on:gethtml="showHtml"
                class="markdown"
                id="0"
                height="500px"
                defaultText="1">
    </VmMarkdown>

    <div class="author">
      <a href="https://github.com/luosijie/vm-markdown">
        <i class="iconfont icon-github"></i>
      </a>
      <span>Designed by <a href="https://github.com/luosijie">JesseLuo</a></span>
    </div>
  </div>
</template>

<script>
import VmMarkdown from './components/vm-markdown.vue'
// import VmMarkdown from '../dist/vm_markdown.min.js'
import intro from './text/intro.js'
export default {
  name: 'app',
  components: {
    VmMarkdown
  },
  data: function () {
    return {
      theme: 'gray',
      intro: intro
    }
  },
  methods: {
    showHtml (html) {
      // get html string here
    },
    selectTheme (evt) {
      if(evt.target.tagName.toLowerCase() === 'span'){
        let theme = evt.target.dataset.theme
        let themeType = document.querySelector('.theme-type')
        let spans = evt.target.parentNode.querySelectorAll('span')
        for(let i; i < spans.length; i++){
          spans[i].style = ''
        }
        this.theme = theme
        themeType.innerText = theme
        evt.target.style.width = '28px'
        evt.target.style.height = '28px'
      } 
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss">
// @import url('./assets/iconfont/iconfont.css');
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.markdown{
  margin-top: 16px;
}
.author{
  margin: 30px 0 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  i{
    color: #232323;
    font-size: 28px;
  }
}
.logo{
  svg{
    transform: scale(0.7);
  }
}
h2.logo-name{
  color: #35495e;
}
.themes{
  display: flex;
  align-items: center;
  span{
    display: inline-block;
    width: 24px;
    height: 24px;
    background-color: red;
    border-radius: 50%;
    margin: 0 6px;
    border: 2px solid #e0e0e0;
    cursor: pointer;
    color: white;
    &:nth-child(1){
      background-color: #35495e;
    }
    &:nth-child(2){
      background-color: #232323;
    }
    &:nth-child(3){
      background-color: #41b883;
    }
    &:nth-child(4){
      background-color: #fafbfc;
    }
    &:nth-child(5){
      background-color: #27292c;
    }
    &:nth-child(5){
      background-color: #f05959;
    }
  }
}
h2 {
  font-weight: bold;
  margin: 0px 0;
}
p.desc{
  color: #858585;
  margin: 20px 0 30px 0;
}
a {
  color: #42b983;
  text-decoration: none;
}
</style>
