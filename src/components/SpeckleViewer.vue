<template>
  <div class="viewer-container fill-height d-flex">
    <div class="top-right pa-3 d-flex">
      <v-btn small color="primary" elevation="2" href="https://speckle.systems">
        <v-img
            alt="Speckle Logo"
            contain
            :src="require(`@/assets/img.png`)"
            transition="scale-transition"
            width="20"
            height="20"
        />
      </v-btn>
      <v-spacer></v-spacer>
      <v-btn outlined small :href="url">View in Server</v-btn>
    </div>
    <div id="renderer">
    </div>
  </div>
</template>

<script>
import {Viewer} from "@speckle/viewer"


export default {
  name: "SpeckleViewer",
  async mounted() {
    await this.fetchLatestCommit().then(() => {
      this.url = `${process.env.VUE_APP_SERVER_URL}/streams/${this.$route.params.id}/objects/${this.latestObject}`
      this.viewer = new Viewer({container: document.getElementById('renderer')})
      this.viewer.onWindowResize()
      this.viewer.loadObject(this.url, this.token)
    }).catch(err => {
      this.$router.replace({ path: "/error", query: {
          message: err
        } })
    })
  },
  data() {
    return {
      viewer: null,
      latestObject: null,
      url: null
    }
  },
  computed: {
    token() {
      return this.$route.query.token || process.env.VUE_APP_SERVER_TOKEN
    }
  },
  methods: {
    fetchLatestCommit() {
      if (this.token)
        return fetch(
            `${process.env.VUE_APP_SERVER_URL}/graphql`,
            {
              method: 'POST',
              headers: {
                'Authorization': 'Bearer ' + this.token,
                'Content-Type': 'application/json'
              },
              body: JSON.stringify({
                query: `query {
                  stream(id: "${this.$route.params.id}"){
                    branch(name: "${this.$route.params?.branch || 'main'}") {
                      commits(limit: 1){
                        items{
                          referencedObject
                        }
                      }
                    }
                  }
                }`
              })
            })
            .then(res => res.json())
            .then(json => {
              if(json.errors) throw json.errors
              this.latestObject = json.data.stream?.branch.commits.items[0].referencedObject
            }).catch(errors => {
              return Promise.reject(errors[0].message)
            })
      else
        return Promise.reject("You are not logged in (token does not exist)")
    }
  }
}
</script>

<style scoped>
#renderer {
  width: 100% !important;
  height: 100% !important;
}
.top-right {
  position: fixed;
  right: 0;
  width: 100% !important;

}

.viewer-container {

}
</style>