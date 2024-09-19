<template>
  <div class="about">
    <h1>This is a database page</h1>
    <table v-if="entries.length" class="data-table">
      <thead>
        <tr>
          <th>ID</th>
          <th>Item ID</th>
          <th>Seller ID</th>
          <th>Price</th>
          <th>User ID</th>
          <th>Expired Date</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(entry, index) in entries" :key="index">
          <td>{{ entry.id }}</td>
          <td>{{ entry.item_id }}</td>
          <td>{{ entry.seller_id }}</td>
          <td>{{ entry.price.toLocaleString() }} VND</td>
          <td>{{ entry.user_id }}</td>
          <td>{{ new Date(entry.expired_date).toLocaleDateString() }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>No data found</p>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { createClient } from '@supabase/supabase-js'

// Create a single Supabase client for interacting with your database
const supabase = createClient('https://pskpouhbuvzeqdhjtfdu.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InBza3BvdWhidXZ6ZXFkaGp0ZmR1Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3MDkzMDU2NzEsImV4cCI6MjAyNDg4MTY3MX0.Dayhg5OljEZ0mX4mDG8YkWvdmi3wwBystnRr4i0PnqE')

// Reactive state to store the fetched data
const entries = ref([])

onMounted(async () => {
  const { data, error } = await supabase.from('entry').select()
  
  if (error) {
    console.error(error)
  } else {
    entries.value = data
  }
})
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }
}

.data-table {
  width: 100%;
  border-collapse: collapse;
}

.data-table th,
.data-table td {
  border: 1px solid #525252;
  padding: 8px;
  text-align: left;
}

.data-table th {
  background-color: #41B883;
  font-weight: bold;
  color:#000000;
}
</style>
