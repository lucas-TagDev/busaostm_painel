<template>
  <b-row>
    <b-col cols="12">
      <h2>
        Edit Rute
      </h2>
      <b-jumbotron>
        <b-form @submit="onSubmit">
          <b-form-group id="fieldsetHorizontal"
                    horizontal
                    :label-cols="4"
                    breakpoint="md"
                    label="Enter Name">
            <b-form-input id="name" v-model.trim="rute_bus.rute_name"></b-form-input>
          </b-form-group>
          <b-form-group id="fieldsetHorizontal"
                    horizontal
                    :label-cols="4"
                    breakpoint="md"
                    label="Enter Type">
            <b-form-input id="type" v-model.trim="rute_bus.rute_type"></b-form-input>
          </b-form-group>
          <b-button class="mx-2" variant="danger" @click.stop="$router.go(-1)">Cancel</b-button>
          <b-button class="mx-2" type="submit" variant="primary">Update</b-button>
        </b-form>
      </b-jumbotron>
    </b-col>
  </b-row>
</template>

<script>

import firebase from '@/Firebase'
import router from '@/router/index.js'

export default {
  name: 'EditHalte',
  data () {
    return {
      key: this.$route.params.id,
      rute_bus: {}
    }
  },
  created () {
    const ref = firebase.firestore().collection('rute_bus').doc(this.$route.params.id);
    ref.get().then((doc) => {
      if (doc.exists) {
        this.rute_bus = doc.data();
      } else {
        alert("No such document!");
      }
    });
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      const updateRef = firebase.firestore().collection('rute_bus').doc(this.$route.params.id);
      updateRef.set(this.rute_bus).then(() => {
        this.key = ''
        this.rute_bus.rute_name = ''
        this.rute_bus.rute_type = ''
        router.go(-1)
      })
      .catch((error) => {
        alert("Error adding document: ", error);
      });
    }
  }
}
</script>

<style>
h2{
    text-align: Center;
  }
  .jumbotron {
    padding: 2rem;
  }
</style>