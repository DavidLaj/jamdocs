<template>
  <Layout :sidebar="false">
    <div class="content">
      <h1>{{ $static.metadata.siteName }} - {{ this.description }}</h1>
      <nav>
        <!-- To use other icons here, you need to import them in the Shortcut component -->
        <Shortcut link="/btc-overview" text="Bitcoin Functioning" icon="link-icon"/>
        <Shortcut link="/eth-overview" text="Bitcoin Security" icon="sliders-icon" />
        <Shortcut link="/theme-configuration#changing-colors" text="A Cooperative Mining Model for Bitcoin (Research Paper)" icon="eye-icon" />
    </div>
  </Layout>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
import fetch from 'node-fetch'
import {get, upperCase} from 'lodash'

const API_ENDPOINT = "https://localbitcoins.com/bitcoinaverage/ticker-all-currencies/"

const getCurrencyData = (data, path) => get(data, path, 0.0)

const getRelation =(base, divider) => parseFloat(base) / parseFloat(divider)

exports.handler = async (event, context) => {
  const {base, divider} = event.queryStringParameters
  return fetch(API_ENDPOINT)
    .then(response => response.json())
    .then(data => {
      const baseData =  getCurrencyData(get(data,upperCase(base), 'USD'), 'rates.last')
      const dividerData = getCurrencyData(get(data, upperCase(divider), 'USD'),'rates.last')
      try {
        return {
          statusCode: 200,
          body: JSON.stringify({currencies: {
              base: baseData,
              divider: dividerData,
              relation: getRelation(baseData, dividerData)
            }
          }, null, 2)
        }
      } catch (error) {
        return {
          statusCode: 400,
          body: error
        }
      }
    })
    .catch(error => ({ statusCode: 422, body: String(error) }));
};
</script>

<script>
import Shortcut from '~/components/Shortcut.vue'

export default {
  components: {
    Shortcut
  },
  data() {
    return {
      description: 'Summaries of my crypto learnings. Hope this can be useful for someone out there...'
    }
  },
  metaInfo() {
    return {
      title: this.description,
      meta: [
        { key: 'description', name: 'description', content: 'A theme for static site documentation based on Gridsome, ready to deploy to Netlify in one click.' }
      ]
    }
  }
}
</script>

<style lang="scss" scoped>
.content {
  display: flex;
  flex-direction: column;
}

h1 {
  text-align: center;
  max-width: 600px;
  margin: 1.5em auto 1.5em;

  @include respond-above(md) {
    max-width: 1000px;
  }
}

h2 {
  font-size: 1.4rem;
  margin: 0;
}

nav {
  display: flex;
  justify-content: space-between;
  flex-direction: column;

  @include respond-above(sm) {
    flex-direction: row;
  }
}

.git {
  margin: 3em 0 0;
  align-self: center;

  @include respond-above(md) {
    margin: 5em 0 0;
  }
}
</style>
