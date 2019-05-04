<template>
  <section class="container">
    <div v-for="item in items" :key="item.sys.id" class="entry">
      <h2>{{item.fields.title}}</h2>
      <img :src="item.fields.image[0].fields.file.url"/>
      <div v-html="item.fields.body"></div>
    </div>
  </section>
</template>

<script>
import { documentToHtmlString } from '@contentful/rich-text-html-renderer'
const contentful = require('contentful')

const client = contentful.createClient({
  space: process.env.CTF_SPACE_ID,
  accessToken: process.env.CTF_CDA_ACCESS_TOKEN
})

export default {
  async asyncData() {
    return await client.getEntries()
      .then(entries => {
        entries.items.forEach(item => {
          item.fields.body = documentToHtmlString(item.fields.body)
        })

        return {
          items: entries.items
        }
      })
  },
}
</script>

<style>
.container {
  min-height: 100vh;
  text-align: center;
}

.entry {
  width: 600px;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid gray;
}

p {
  text-align: left;
}

img {
  width: 100%
}
</style>

