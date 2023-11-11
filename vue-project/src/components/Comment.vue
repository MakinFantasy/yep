<template>
  <li class="item">
    <div class="item__comment" :class="colorReaction">
      <p class="item__author">
        {{ comment.author }}
      </p>
      <p class="item__text">
        {{ comment.text }}
      </p>
      <div class="item__bottom">
        <my-button class="item__button" @click="showForm"> Answer</my-button>
        <span class="item__replies" v-if="childs">
          {{ childs.length }}
        </span>
        <time class="item__date">
          {{ convertDate }}
        </time>
      </div>
    </div>

    <comments-form v-show="formVisible" @send="sendComment" :parentId="comment.id" class="item__form" />

    <ul v-if="childs" class="item__childs">
      <Comment @showDialog="showChildDialog" @send="sendComment" v-for="child in childs" :comment="child"
        :childs="child.childs" :key="child.id" :style="{
          'margin-left': 5 + 'px',
        }" />
    </ul>

  </li>
</template>

<script>
import CommentsForm from '@/components/CommentsForm.vue';
export default {
  components: { CommentsForm },
  props: {
    comment: {
      type: Object,
      required: true,
    },
    childs: {
      type: Array,
    },
  },

  data() {
    return {
      date: this.comment.createdAt,
      formVisible: false,
    };
  },

  computed: {
    colorReaction() {
      if (!this.childs) return;
      let sum = 0;
      this.childs.forEach((comment) => {
        sum += comment.reaction;
      });
      if (sum > 0) {
        return 'item__positive';
      } else if (sum < 0) {
        return 'item__negative';
      }
      return;
    },

    convertDate() {
      return new Intl.DateTimeFormat('ru', {
        hour: 'numeric',
        minute: 'numeric',
        day: 'numeric',
        month: 'numeric',
        year: 'numeric',
      }).format(Date.parse(this.date));
    },
  },

  methods: {
    showChildDialog(id) {
      this.$emit('showDialog', id);
    },
    showForm() {
      this.formVisible = !this.formVisible;
    },
    sendComment(comment) {
      this.$emit('send', comment);
    },
  },
};
</script>

<style scoped lang="scss">
@use '@/assets/scss/mixin' as *;
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;

.item__childs {
  border-left: #e4e4e4 0.1rem solid;
}

.item__comment {
  color: rgb(245, 245, 220);
  padding: 1rem;
  margin-top: 1.5rem;
  list-style: none;
  border-radius: 7px;
  background-color: rgb(0, 0, 0);
  transition: box-shadow 0.8s linear;
  border: 1px solid rgb(245, 245, 220);

  @include media(min, md) {
    padding: 15px;
  }

  &:hover {
    box-shadow: 0px 0px 3px 0px #bebbbb;
  }
}

.item__positive {
  background-color: rgba(0, 188, 94, 0.17);
}

.item__negative {
  background-color: rgba(246, 62, 62, 0.17);
}

.item__author {
  font-size: 2.5rem;
  font-weight: 600;
  word-break: break-word;
}

.item__text {
  margin-top: 1rem;
  font-size: 1.6em;
  word-break: break-word;
}

.item__svg {
  height: 2rem;
}

.item__bottom {
  margin-top: 15px;
  display: flex;
  align-items: center;
}

.item__button {
  font-size: 1.2rem;
  font-weight: 600;

  @include media(min, md) {
    font-size: 1.2rem;
  }
}

.item__replies {
  margin-left: auto;
  font-size: 1.5rem;
  font-weight: 600;
  line-height: 1;
  display: flex;
  align-items: center;
  gap: 5px;
}

.item__replies+.item__date {
  @include media(min, xs) {
    margin-left: 15px;
  }
}

.item__date {
  margin-left: auto;
  font-size: 1.5rem;
  line-height: 1;
}
</style>
