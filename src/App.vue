<script>
import axios from 'axios'
import 'bootstrap/dist/css/bootstrap.css';
import 'bootstrap/dist/js/bootstrap.bundle';
import './assets/style/style.css'

export default {
  name: 'App',
  data() {
    return {
      loading : false ,
      langs: null,
      userLang: 'en',
      transLang: 'ar',
      targetText: '',
      translatedText : ''
    }
  },
  methods: {
    fetchLanguages() {
      return this.loading = true ,
      axios.get('https://text-translator2.p.rapidapi.com/getLanguages', {
        headers: {
          'X-RapidAPI-Key': 'bf736c96cemsh65cd5b8678a7252p1251e7jsn83c9ef881654',
          'X-RapidAPI-Host': 'text-translator2.p.rapidapi.com'
        }
      }).then((res) => {
        return this.loading=false , this.langs = res.data.data.languages 
      })
    },
    fetchTranslation() {
      const encodedParams = new URLSearchParams();
        encodedParams.set('source_language', this.userLang);
        encodedParams.set('target_language', this.transLang);
        encodedParams.set('text', this.targetText);

      axios.post('https://text-translator2.p.rapidapi.com/translate',encodedParams ,{
        headers: {
          'content-type': 'application/x-www-form-urlencoded',
          'X-RapidAPI-Key': 'bf736c96cemsh65cd5b8678a7252p1251e7jsn83c9ef881654',
          'X-RapidAPI-Host': 'text-translator2.p.rapidapi.com'
          }
        }).then((res) => {
        return this.translatedText = res.data.data.translatedText
      })
    },
    handleSubmit() {
      return this.translatedText= '' , this.fetchTranslation()
    }
  },
  created() {
    return this.fetchLanguages()
  }
}
</script>

<template>
  <div class="loading">
    
  </div>
  <div class="page">
    <div class="container">
      <div class="langs">
        <select v-model="userLang">
          <option v-for="lang in langs" :key="lang.code" :value="lang.code">{{ lang.name }}</option>
        </select>
        <span>=></span>
        <select v-model="transLang">
          <option v-for="lang in langs" :key="lang.code" :value="lang.code">{{ lang.name }}</option>
        </select>
      </div>
      <form action="" @submit.prevent="handleSubmit">
        <textarea  dir="auto" v-model="targetText" placeholder="Write here..." />
        <button @click="fetchTranslation">Translate</button>
      </form>
      <div class="translation" dir="auto" v-if="translatedText.length !== 0">
        {{translatedText}}
      </div>
      <!-- <input
        type="text"
        disabled="true"
        v-model="translatedText"
        v-if="translatedText.length !== 0"
        class="translation"
        dir="auto"
        > -->
      <div class="tabs" v-if="userLang === 'en'">
        <h3>Examples (أمثلة)</h3>
        <h3>Definiations (مصطلحات)</h3>
        <h3>Synonyms (مرادفات الكلمه)</h3>
        <h3>Antonyms (مضادات الكلمه)</h3>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.page{
  padding: 50px 0;
  .langs{
    display: flex;
    flex-direction: row;
    justify-content: center;
    width: 100%;
    align-items: center;
    margin-bottom: 30px;
    gap: 50px;
    select{
      padding: 5px 25px;
      outline: none;
    }
    span{
      font-size: 20px;
      font-weight: 800;
    }
  }
  form{
    display: flex;
    flex-direction: row;
    gap: 20px;
    height: fit-content;
    textarea{
      width: 100%;
      padding: 20px;
      border: none;
      background: #352F44;
      color: #fff;
      outline: none;
      border-radius: 5px;
      min-height: 70px;
      max-height: 250px;
      &::placeholder{
        color: #fff;
      }
    }
    button{
      padding: 5px 15px;
      border: none;
      background: #5C5470;
      border-radius: 5px;
      color: #fff;
      z-index: 3;
      position: relative;
      overflow: hidden;
      max-height: 60px;
      &::after{
        content: '';
        position: absolute;
        left: 0;
        top: 0;
        width: 0%;
        height: 100%;
        background:#B9B4C7;
        z-index: -1;
        transition: 0.3s;
      }
      &::before{
        content: '';
        position: absolute;
        right: 0;
        top: 0;
        width: 0%;
        height: 100%;
        background:#B9B4C7;
        z-index: -1;
        transition: 0.3s;
      }
      &:hover{
        &::after , &::before{
          width: 50%;
        }
      }
    }
  }
  .translation{
    width: 100%;
    background: #B9B4C7;
    color: #352F44;
    margin: 20px 0;
    border-radius: 5px;
    padding: 15px 20px;
    font-size: 20px;
    font-weight: 400;
    direction: auto;
  }
  .tabs{
    display: grid;
    grid-template-columns: repeat( 4 , 1fr);
    gap: 50px;
    align-items: center;
    margin-top: 30px;
    h3{
      font-size: 20px;
      font-weight: 400;
      // border: 2px solid #352F44;
      border-radius: 4px;
      text-align: center;
      padding: 15px;
      cursor: pointer;
      background: #FAF0E6;
      color: #352F44;
      z-index: 3;
      position: relative;
      overflow: hidden;
      &::after{
        content: '';
        position: absolute;
        left: 0;
        top: 0;
        width: 0%;
        height: 100%;
        background:#B9B4C7;
        z-index: -1;
        transition: 0.3s;
      }
      &::before{
        content: '';
        position: absolute;
        right: 0;
        top: 0;
        width: 0%;
        height: 100%;
        background:#B9B4C7;
        z-index: -1;
        transition: 0.3s;
      }
      &:hover{
        color: #fff;
        border: none;
        &::after , &::before{
          width: 50%;
        }
      }
    }
  }
}
</style>