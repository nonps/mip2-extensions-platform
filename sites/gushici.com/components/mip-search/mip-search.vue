<template>
  <div class="search-box">
    <form
      class="search-input"
      action="https://mip.gushici.com/so.html">
      <input
        id="history"
        :value="old"
        type="hidden">
      <input
        id="inp"
        type="text"
        name="value"
        placeholder="在古诗词网中搜索"
        autocomplete="off"
        @input="_input">
      <i/>
    </form>
    <div
      v-show="showResult"
      class="search-result">
      <div class="inner">
        <dl>
          <dt>诗文</dt>
          <dd>
            <a
              v-for="(item,i) in poetry"
              :key="i"
              :href="'https://mip.gushici.com'+item.url+'.html'"
              mip-link>{{ item.name }}-{{ item.auth }}</a>
          </dd>
        </dl>
        <dl>
          <dt>名句</dt>
          <dd>
            <a
              v-for="(item,i) in mingju"
              :key="i"
              :href="'https://mip.gushici.com'+item.url+'.html'"
              mip-link>{{ item.body }}</a>
          </dd>
        </dl>
        <dl>
          <dt>作者</dt>
          <dd>
            <a
              v-for="(item,i) in auth"
              :key="i"
              :href="'https://mip.gushici.com'+item.url+'.html'"
              mip-link>{{ item.name }}</a>
          </dd>
        </dl>
      </div>
      <p
        class="close"
        @click="_close">关闭搜索</p>
    </div>
  </div>
</template>

<script>
import './index.less'
import ajax from '../../common/ajax.js'
export default{
  props: {

  },
  data () {
    return {
      showResult: false,
      old: '',
      cur: '',
      poetry: [],
      mingju: [],
      auth: []
    }
  },
  created () {

  },
  mounted () {

  },
  methods: {
    _input (e) {
      this.cur = e.target.value
      this.old = e.target.value
      this._search()
    },
    _search () {
      let v = this.cur
      let t = this
      if (v !== '' && !/[a-z]/.test(v)) {
        let body = document.querySelector('body')
        let result = document.querySelector('.search-result')
        ajax.get('search_menu', {text: v}, function (res) {
          t.poetry = res.poetry
          t.mingju = res.mingju
          t.auth = res.auth
          t.showResult = true
          MIP.util.css(body, 'position', 'fixed')
          MIP.util.css(result, 'height', MIP.viewport.getHeight())
        })
      }
    },
    _close () {
      this.showResult = false
      document.querySelector('#inp').value = ''
      let body = document.querySelector('body')
      let result = document.querySelector('.search-result')
      MIP.util.css(body, 'position', 'static')
      MIP.util.css(result, 'height', 0)
    }
  }
}
</script>
