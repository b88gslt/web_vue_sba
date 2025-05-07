<template>
  <div id="app">
    <div class="main-container" :style="mainBackgroundStyle">
      <header class="page-header">
        <div class="logo">
          <img :src="getImage('molot2.png')" alt="Логотип SEA Law Office" />
          <h1>SBA Law Office<span class="underline"></span></h1>
        </div>
        <div class="navigation">
          <ul>
            <li>
              <a href="#services">
                <img :src="getImage('teleph.png')" alt="Услуги" class="nav-icon" />
              </a>
            </li>
            <li>
              <a href="#about">
                <img :src="getImage('teleg.png')" alt="О нас" class="nav-icon" />
              </a>
            </li>
            <li @click="toggleMenu" class="nav-icon-btn">
              <img :src="getImage('tg22.png')" alt="Меню" class="nav-icon" />
            </li>
          </ul>
        </div>
      </header>
      <!-- Выплывающее меню -->
      <div class="side-menu" :class="{ open: isMenuOpen }">
        <div class="menu-header">
          <span class="close-btn" @click="toggleMenu">&times;</span>
          <h3>SBA LAW OFFICE</h3>
          <p>LEGAL TEAM</p>
        </div>
        <ul class="menu-links">
          <li>
            <button class="nav-icon-btn" @click="switchView('FormComponent')">Главная</button>
          </li>
          <li>
            <button class="nav-icon-btn" @click="switchView('TestComponent')">Тест</button>
          </li>
          <li>
            <button class="nav-icon-btn" @click="switchView('TableComponent')">Таблица</button>
          </li>
        </ul>
      </div>
      <!-- Вывод текущего компонента -->
      <component :is="currentView" />
    </div>
  </div>
</template>

<script>
import FormComponent from './components/FormComponent.vue'
import TableComponent from './components/TableComponent.vue'
import TestComponent from './components/TestComponent.vue'

export default {
  name: 'App',
  components: {
    FormComponent,
    TableComponent,
    TestComponent
  },
  data() {
    return {
      isMenuOpen: false,
      currentView: 'FormComponent',
      currentImageIndex: 0,
      backgroundImages: [
        new URL('./assets/images/main1.jpg', import.meta.url).href,
        new URL('./assets/images/main2.jpg', import.meta.url).href,
        new URL('./assets/images/main3.jpg', import.meta.url).href
      ]
    }
  },
  computed: {
    mainBackgroundStyle() {
      return {
        backgroundImage: `url(${this.backgroundImages[this.currentImageIndex]})`,
        backgroundSize: 'cover',
        backgroundRepeat: 'no-repeat',
        backgroundPosition: 'center',
        minHeight: '100vh',
        width: '100%'
      }
    }
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen
    },
    switchView(view) {
      this.currentView = view
      this.isMenuOpen = false
    },
    getImage(name) {
      return new URL(`./assets/images/${name}`, import.meta.url).href
    }
  },
  mounted() {
    setInterval(() => {
      this.currentImageIndex = (this.currentImageIndex + 1) % this.backgroundImages.length
    }, 5000)
  }
}
</script>

<style>
@import './assets/styles.css';
</style>