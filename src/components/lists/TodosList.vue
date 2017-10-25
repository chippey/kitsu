<template>
<div class="data-list">
  <div style="overflow: hidden">
  <table class="table table-header" ref="headerWrapper">
    <thead>
      <tr>
        <th class="production">
          {{ $t("tasks.fields.production") }}
        </th>
        <th class="name">
          {{ $t("tasks.fields.entity") }}
        </th>
        <th class="type">
          {{ $t("tasks.fields.task_type") }}
        </th>
        <th class="status">
          {{ $t("tasks.fields.task_status") }}
        </th>
        <th class="actions">
        </th>
      </tr>
    </thead>
  </table>
  </div>

  <div class="table-body" v-scroll="onBodyScroll">
    <table class="table">
      <tbody>
        <tr v-for="entry in entries">
          <production-name-cell
            class="production"
            :entry="{
              name: entry.project_name,
              id: entry.production_id
            }"
            :only-avatar="true"
          >
          </production-name-cell>
          <td class="name">{{ entityName(entry) }}</td>
          <task-type-name
            class="type"
            :entry="{
              name: entry.task_type_name,
              color: entry.task_type_color
            }"
          >
          </task-type-name>
          <td class="status">
            <validation-tag
              :task="entry"
            >
            </validation-tag>
          </td>
          <td class="actions"></td>
       </tr>
      </tbody>
    </table>
  </div>
  <spinner v-if="isLoading">
  </spinner>
  <div class="has-text-centered" v-if="isError">
    <span class="tag is-danger">
      An error occured while loading data
    </span>
  </div>
  <p class="has-text-centered footer-info" v-if="!isLoading">
    {{ entries.length }} {{ $tc('tasks.tasks', entries.length) }}
  </p>
</div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

import ProductionNameCell from '../cells/ProductionNameCell'
import TaskTypeName from '../cells/TaskTypeName'
import Spinner from '../widgets/Spinner'
import ValidationTag from '../widgets/ValidationTag'

export default {
  name: 'todos-list',
  components: {
    ProductionNameCell,
    Spinner,
    TaskTypeName,
    ValidationTag
  },
  props: [
    'entries',
    'isLoading',
    'isError'
  ],
  computed: {
    ...mapGetters([
    ])
  },
  methods: {
    ...mapActions([
    ]),
    entityName (entry) {
      if (entry.entity_type_name === 'Shot') {
        return `${entry.sequence_name} / ${entry.entity_name}`
      } else {
        return `${entry.entity_type_name} / ${entry.entity_name}`
      }
    },
    onBodyScroll (event, position) {
      console.log(this.$refs.headerWrapper.style.left)
      console.log(`${position.scrollLeft}px`)
      this.$refs.headerWrapper.style.left = `-${position.scrollLeft}px`
    }
  }
}
</script>

<style scoped>
.name {
  width: 230px;
  min-width: 230px;
}

.production {
  width: 70px;
  min-width: 70px;
}

.type {
  width: 100px;
  min-width: 100px;
}

.status {
  width: 100px;
  min-width: 100px;
}

.table-header {
  display: block;
  width: 100%;
  margin-bottom: 0;
  flex-wrap: wrap;
  position: relative;
}

.table-header th.actions {
  width: 100%;
}

.table-body {
  flex: 1;
  overflow: auto;
  min-height: 1px;
}

.table {
  margin-bottom: 0;
}
</style>