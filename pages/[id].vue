<template>
  <div class="container my-5">
    <div class="row">
      <div class="col-md-5">
        <img :src="'https://zbtfeoishdvbsciusmsn.supabase.co/storage/v1/object/public/images/' + books[0].Pilt"
             class="img-fluid" alt="Book cover">
      </div>
      <div class="col-md-7 d-flex flex-column">
        <h1 class="mb-4">{{ books[0].Pealkiri }}</h1>
        <h4 class="mb-3">{{ books[0].Autor }}</h4>
        <p class="mb-4">{{ books[0].Kirjeldus }}</p>
        <div class="text-center mt-auto">
          <div class="d-inline-block">
            <button v-if="userIn" class="button" @click="borrow">Laenuta</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const client = useSupabaseClient()
const userIn = useSupabaseUser()
const {id} = useRoute().params
const {auth} = useSupabaseAuthClient()
const {data: {user}} = await client.auth.getUser()

let {data: books, error} = await client
    .from('RAAMATUD')
    .select('*')
    .eq('Raamatu_ID', id)

async function borrow() {
  const {data, error} = await client
      .from('LAENUTUSED')
      .insert({kasutaja_id: user.id, Raamatu_ID: id});
}
</script>

<style scoped>
.img-fluid {
  min-width: 500px;
  max-width: 500px;
  min-height: 500px;
  max-height: 700px;
  object-fit: cover;
}

.button {
  padding: 20px 40px;
  font-weight: bold;
  border-radius: 50px;
  border: 0;
  background-color: #FF9B42;
  box-shadow: rgb(0 0 0 / 5%) 0 0 8px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  font-size: 15px;
  transition: all .5s ease;
}

.button:hover {
  letter-spacing: 3px;
  background-color: rgb(252, 133, 28);
  color: hsl(0, 0%, 100%);
}
</style>