<template>
  <div class="box">
    <AddButton :to="{ name: 'blog-tag-create' }" />
    <b-button
      v-if="checkedRows.length"
      type="is-danger"
      size="is-small"
      icon-left="delete"
      rounded
      @click="deleteData"
    >
      <span>Delete</span>
    </b-button>
    <hr />
    <b-table
      :data="data"
      :loading="loading"
      :checked-rows.sync="checkedRows"
      checkable
      :checkbox-position="checkboxPosition"
      paginated
      hoverable
      scrollable
      backend-pagination
      :total="total"
      :per-page="perPage"
      aria-next-label="Next page"
      aria-previous-label="Previous page"
      aria-page-label="Page"
      aria-current-label="Current page"
      backend-sorting
      :default-sort-direction="defaultSortOrder"
      :default-sort="[sortField, sortOrder]"
      @page-change="onPageChange"
      @sort="onSort"
    >
      <template slot="empty">
        <div class="has-text-centered">Empty</div>
      </template>
      <b-table-column
        v-slot="props"
        field="created_at"
        label="Created At"
        sortable
        centered
      >
        {{
          props.row.created_at
            ? new Date(props.row.created_at).toLocaleString()
            : 'unknown'
        }}
      </b-table-column>

      <b-table-column v-slot="props" field="title" label="Title" sortable>
        {{ props.row.title }}
      </b-table-column>

      <b-table-column v-slot="props" field="slug" label="Slug" sortable>
        {{ props.row.slug }}
      </b-table-column>

      <b-table-column v-slot="props" field="posts_count" label="Posts" sortable>
        {{ props.row.posts_count }}
      </b-table-column>

      <b-table-column v-slot="props" field="action" label="Action">
        <ActionButton
          :edit="false"
          :detail-to="{
            name: 'blog-tag-id',
            params: { id: props.row.id },
          }"
        />
      </b-table-column>
      <template slot="bottom-left">
        <b>Total checked</b>: {{ checkedRows.length }}
      </template>
    </b-table>
  </div>
</template>

<script>
import { datatableMixin } from '@/js/mixins/datatable'

export default {
  name: 'BlogTagIndex',
  metaInfo: {
    title: 'Blog: Tag',
  },
  mixins: [datatableMixin],
  data() {
    return {
      url: 'blog/tag',
    }
  },
}
</script>
