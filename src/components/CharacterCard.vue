<script>
export default {
  props: {
    name: String,
    status: String,
    species: String,
    location: String,
    epizode: String,
    image: String
  }
}
</script>

<template>
  <div class="item">
    <div class="item__picture">
      <img :src="image" alt="Character image" />
    </div>
    <div class="item__info">
    <h1>
      {{ name }}
    </h1>
    <div>
      {{ status }} - {{ species }}
    </div>
    <div class="details">
      <div class="details__item">
        <p>Last known location:</p>
        {{ location }}
      </div>
      <div class="details__item">
        <p>First seen in:</p>
        {{ epizode }}
      </div>
    </div>
  </div>
  </div>
</template>

<style scoped>
.item {
  display: flex;
  flex-direction: row; 
  background-color: #333;
  color: #fff;
  width: 600px; 
  height: 220px;
  border-radius: 10px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.item__picture img {
  width: 220px;
  height: 220px; 
  height: 100%;
  object-fit: cover;
  border-radius: 10px 0 0 10px; 
  margin-right: 1rem; 
  overflow: hidden;
}

.item__info {
  flex: 1; 
  display: flex;
  flex-direction: column; 
  justify-content: center; 
}

h1 {
  font-size: 1.5rem;
  font-weight: bold;
  margin-bottom: 0;
}

p {
  font-size: 1rem;
  color: #666;
  margin-bottom: 0; 
}

.details {
  display: flex;
  flex-direction: column; 
}

.details__item {
  padding: 0.5rem 0;
  text-align: left;
  border-bottom: 1px solid #555; 
}

.details__item:last-child {
  border-bottom: none;
}
</style>