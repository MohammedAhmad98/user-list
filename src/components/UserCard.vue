<template>
    <div class="user-card"  @click="showOnMap(user)">
        <h3>{{ user.name.title }}.{{ user.name.first }} {{ user.name.last }}</h3>
        <p>{{ user.email }}</p>
        <p>{{ user.gender }}</p>
        <p>{{ user.location.country }}</p>
        <p>{{ user.dob.date | formatDate }}</p>
      </div>
</template>


<script>
export default {
  name: "UserCard",
  props: {
    user: {
      type: Object,
      required: true
    }
  },

  methods: {
    getGoogleMapsLink(latitude, longitude) {
      return `https://www.google.com/maps/search/?api=1&query=${latitude},${longitude}`;
    },
    showOnMap(user) {
      const googleMapsLink = this.getGoogleMapsLink(user.location.coordinates.latitude, user.location.coordinates.longitude);
      window.open(googleMapsLink, '_blank');
    },
  },
  filters: {
    formatDate: function (dateString) {
      const date = new Date(dateString);
      return date.toLocaleDateString();
    },
  },
}

</script>

<style scoped>

.user-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 20px;
  cursor: pointer;
  transition: transform 0.2s;
}

.user-card:hover {
  transform: scale(1.05);
}

.user-card p {
  margin: 8px 0;
}

.user-card a {
  display: block;
  color: #007BFF;
  text-decoration: underline;
  cursor: pointer;
}


</style>