<template>
  <div>
    <v-menu
      id="notification-indicator"
      transition="slide-y-transition"
      bottom
      offset-y
    >
      <template #activator="{ on }">
        <v-btn icon class="secondary--text text--lighten-2" v-on="on">
          <v-icon v-if="hasRunningTasks" class="top-loading-icon">
            fa fa-circle-o-notch fa-spin
          </v-icon>
          <v-icon v-else>
            fa fa-check-circle
          </v-icon>
        </v-btn>
      </template>
      <v-row class="progress-indicator__details">
        <v-col v-if="tasks.length > 0">
          <v-list two-line>
            <template v-for="task in tasks">
              <v-list-item :key="task.id" class="progress-indicator__task">
                <v-list-item-content>
                  <v-list-item-title
                    v-text="task.meta.description"
                  ></v-list-item-title>
                  <v-progress-linear
                    v-if="task.type !== 'process_trade_history'"
                    indeterminate
                    class="progress-indicator__task__progress"
                  ></v-progress-linear>
                  <v-progress-linear
                    v-else
                    :value="progress"
                    class="progress-indicator__task__progress"
                  ></v-progress-linear>
                </v-list-item-content>
              </v-list-item>
              <v-divider :key="`task-${task.id}`"></v-divider>
            </template>
          </v-list>
        </v-col>
        <v-col v-else class="progress-indicator__no-tasks align justify">
          <v-icon color="primary">fa fa-info-circle</v-icon>
          <p class="progress-indicator__no-tasks__label">No running tasks</p>
        </v-col>
      </v-row>
    </v-menu>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { createNamespacedHelpers } from 'vuex';
import { Task, TaskMeta } from '@/model/task';

const { mapGetters } = createNamespacedHelpers('tasks');
const { mapGetters: mapReportGetters } = createNamespacedHelpers('reports');

@Component({
  computed: {
    ...mapGetters(['hasRunningTasks', 'tasks']),
    ...mapReportGetters(['progress'])
  }
})
export default class ProgressIndicator extends Vue {
  hasRunningTasks!: boolean;
  tasks!: Task<TaskMeta>[];
  progress!: number;
}
</script>

<style scoped lang="scss">
.progress-indicator {
  &__details {
    width: 300px;
    height: 350px;
    background-color: white;
  }

  &__no-tasks {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;

    &__label {
      font-size: 22px;
      margin-top: 22px;
      font-weight: 300;
      color: rgb(0, 0, 0, 0.6);
    }
  }

  &__task {
    min-height: 35px;

    &__progress {
      margin-top: 8px;
    }
  }
}
</style>
