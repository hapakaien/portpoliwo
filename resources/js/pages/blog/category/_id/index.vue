<template>
  <div class="box">
    <ValidationObserver ref="form" v-slot="{ passes }">
      <form @submit.prevent="passes(onSubmit)">
        <div class="columns is-multiline">
          <div class="column is-half-tablet">
            <FormInput v-model="category.title" label="Title" name="title" />
          </div>
          <div class="column is-half-tablet">
            <FormInput
              v-model="category.slug"
              label="Slug (Optional)"
              name="slug"
            />
          </div>
        </div>
        <hr />
        <SaveButton />
      </form>
    </ValidationObserver>
  </div>
</template>

<script>
import { ValidationObserver } from 'vee-validate'
import { api } from '@/js/api'
import pick from 'lodash/pick'

export default {
  name: 'BlogCategoryDetail',
  metaInfo: {
    title: 'Blog: Category Detail',
  },
  components: {
    ValidationObserver,
    FormInput: () => import('@/js/components/form/Input'),
    SaveButton: () => import('@/js/components/SaveButton'),
  },
  data() {
    return {
      category: {
        title: '',
        slug: null,
      },
    }
  },
  mounted() {
    this.fetchData()
  },
  methods: {
    async fetchData() {
      await api
        .get(`/blog/category/${this.$route.params.id}`)
        .then(({ data: { data } }) => {
          this.category = pick(data, ['title', 'slug'])
        })
        .catch(() => {
          this.category = {
            title: '',
            slug: null,
          }
        })
    },
    async onSubmit() {
      await api
        .put(`/blog/category/${this.$route.params.id}`, this.category)
        .then(({ data }) => {
          if (data.success) {
            this.$buefy.toast.open({
              message: data.message,
              type: 'is-success',
            })
          }
          this.fetchData()
        })
        .catch(({ response: { data } }) => {
          if (data.errors) {
            this.$refs.form.setErrors(data.errors)
          }
          this.$buefy.toast.open({
            message: data.message,
            type: 'is-danger',
          })
        })
    },
  },
}
</script>
