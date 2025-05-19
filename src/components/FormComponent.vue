<template>
  <main class="hero">
    <div class="header-text">
      <h2>LEGAL SOLUTIONS BY <br> SBA LAW OFFICE</h2>
      <p>CONTACT US FOR LEGAL ADVICE</p>
    </div>
    <form class="contact-form" @submit.prevent="handleForm">
      <div>
        <input type="text" v-model="form.name" @blur="validateName" placeholder="Name">
        <div class="error-message" :class="{ show: errors.name }">{{ errors.name }}</div>
      </div>
      <div>
        <input type="tel" v-model="form.phone" @blur="validatePhone" placeholder="Phone">
        <div class="error-message" :class="{ show: errors.phone }">{{ errors.phone }}</div>
      </div>
      <div>
        <input type="email" v-model="form.email" @blur="validateEmail" placeholder="Email address">
        <div class="error-message" :class="{ show: errors.email }">{{ errors.email }}</div>
      </div>
      <button type="submit">CONTACT US</button>
    </form>
    <div v-if="showSuccessMessage" class="success-message">
      <p>Спасибо за заявку! Мы свяжемся с вами.</p>
      <button @click="showSuccessMessage = false">Закрыть</button>
    </div>
  </main>
</template>

<script>
export default {
  name: 'FormComponent',
  data() {
    return {
      form: { name: '', phone: '', email: '' },
      errors: { name: '', phone: '', email: '' },
      showSuccessMessage: false
    }
  },
  methods: {
    validateName() {
      const re = /^[A-Za-z]+$/
      this.errors.name = !this.form.name
        ? 'Name is required'
        : !re.test(this.form.name)
        ? 'Use only English letters'
        : ''
    },
    validatePhone() {
      const re = /^\d{11}$/
      this.errors.phone = !this.form.phone
        ? 'Phone is required'
        : !re.test(this.form.phone)
        ? 'Phone must be exactly 11 digits'
        : ''
    },
    validateEmail() {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      this.errors.email = !this.form.email
        ? 'Email is required'
        : !re.test(this.form.email)
        ? 'Invalid email format'
        : ''
    },
    handleForm() {
      this.validateName()
      this.validatePhone()
      this.validateEmail()
      if (!this.errors.name && !this.errors.phone && !this.errors.email) {
        this.showSuccessMessage = true
        this.form = { name: '', phone: '', email: '' }
      }
    }
  }
}
</script>
