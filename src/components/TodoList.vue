<template>
  <section>
    <transition-group name="list" tag="ul">
      <li v-for="(todoItem, index) in propsdata" :key="todoItem" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        {{todoItem}}
        <div class="actionBtn">
          <span class="updateBtn" type="button" @click="updateTodo(todoItem, index)">
            <i class="far fa-edit" aria-hidden="true"></i>
          </span>
          <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
            <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
        </div>
      </li>
    </transition-group>

    <modal v-if="showModal" @close="showModal = false">
      <h3 slot="header">수정하기</h3>
      <span slot="body">
        <input type="text" v-model="editTodoItem">
      </span>
      <span slot="footer" @click="showModal = false">
         닫기
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
    </span>
    </modal>
  </section>
</template>

<script>
    import Modal from "./common/Modal";

    export default {
        props: ['propsdata'],
        data() {
            return {
                newTodoItem: '',
                showModal: false,
                editTodoItem: '',
                index: '',
            }
        },
        methods: {
            removeTodo(todoItem, index) {
                this.$emit('removeTodo', todoItem, index);
            },
            updateTodo(todoItem, index) {
                this.editTodoItem = todoItem;
                this.index = index;
                this.showModal = !this.showModal;
            },
        },
        watch: {
            editTodoItem: function (data) {
                console.log(data);
                this.$emit('updateTodo', data, this.index);
            }
        },
        components: {
            Modal: Modal
        },
    }
</script>

<style scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0px;
    text-align: left;
  }

  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }

  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }

  .actionBtn {
    margin-left: auto;
  }

  .updateBtn {
    color: cornflowerblue;
  }

  .removeBtn {
    color: #de4343;
  }

  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }

  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
</style>