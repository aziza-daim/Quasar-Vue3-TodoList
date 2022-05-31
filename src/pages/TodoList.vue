<template>
  <q-page class="bg-grey-3">
    <div class="row q-pa-sm bg-blue-grey-13">
      <q-input
        v-model="newTask"
        @keyup.enter="AddTask"
        filled
        square
        label="Add Task"
        class="col"
        bg-color="white"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="AddTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="index"
        :class="{ 'bg-primary done': task.done }"
        @click="task.done = !task.done"
        clickable
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            dense
            color="grey-8"
            icon="delete_outline"
            @click.stop="confirm = true"
          />
          <q-dialog
            v-model="confirm"
            persistent
            transition-show="flip-down"
            transition-hide="flip-up"
          >
            <q-card class="bg-primary text-white" style="width: 300px">
              <q-card-section>
                <div class="text-h6">Confirm</div>
              </q-card-section>

              <q-card-section class="q-pt-none">
                Really delete?
              </q-card-section>

              <q-card-actions align="right" class="bg-white text-primary">
                <q-btn
                  flat
                  label="Cancel"
                  color="blue-grey-13"
                  @click="confirm = false"
                />
                <q-btn
                  flat
                  label="Delete"
                  color="blue-grey-13"
                  @click.stop="deleteTask(index)"
                />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="no-tasks absolute-center" v-if="!tasks.length">
      <q-icon
        name="emoji_food_beverage"
        size="4rem"
        color="primary"
        class="q-ml-md"
      />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, reactive, ref } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  setup() {
    // data

    const $q = useQuasar();
    const confirm = ref(false);
    let newTask = ref("");

    let tasks = reactive([]);

    //methods

    const deleteTask = (index) => {
      tasks.splice(index, 1);
      confirm.value = false;
      showCustom();
    };

    const showCustom = () => {
      $q.notify({
        message: "Task deleted...",
        color: "blue-grey-13",
        icon: "tag_faces",
        position: "top-right",
        timeout: 2000,
      });
    };

    const AddTask = () => {
      if (newTask.value.length <= 0) {
        $q.notify({
          message: "Please write something...",
          color: "blue-grey-13",
          icon: "announcement",
          position: "center",
          timeout: 2000,
        });

        return;
      }
      tasks.push({
        title: newTask.value,
        done: false,
      });
      newTask.value = "";
    };

    // return
    return {
      //data
      tasks,
      confirm,
      newTask,
      //methods
      deleteTask,
      showCustom,
      AddTask,
    };
  },
});
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: white;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>

