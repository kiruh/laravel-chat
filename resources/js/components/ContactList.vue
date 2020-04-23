<template>
  <div class="contact-list">
    <ul>
      <li v-for="contact in contacts" :key="contact.id">
        <div class="avatar">
          <img src="https://via.placeholder.com/150" :alt="contact.name" />
        </div>
        <div class="contact">
          <p class="name">{{ contact.name }}</p>
          <p class="email">{{ contact.email }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    contacts: {
      type: Array,
      default: []
    }
  }
};
</script>
