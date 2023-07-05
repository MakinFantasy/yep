<template>
  <main>
    <section class="comments">
      <div class="comments__wrapper">
        <h1 class="comments__title">Лабораторная работа №12</h1>
        <div class="comments__switch">
          <span class="comments__text">Отображение комментариев в реальном времени</span>
          <my-checkbox
            class="comments__input"
            :checked="switchChecked"
            @check="toggleStreamComment"
          />
        </div>

        <my-button
          @click="showForm"
          class="comments__button"
          v-show="!formVisible"
          >Оставить комментарий</my-button
        >

        <comments-form
          v-show="formVisible"
          @send="postComment"
          :parentId="null"
          class="comment__form"
        />

        <h2 class="comments__subtitle">
          Список всех комментариев ({{ commentsLength }})
        </h2>

        <comments
          :comments="comments"
          class="comments__section"
          @reply="showReplyDialog"
          @send="postComment"
        />

      </div>
    </section>
  </main>
</template>

<script>

import Comments from '@/components/Comments.vue';
import CommentsForm from '@/components/CommentsForm.vue';
import axios from 'axios'; 

export default {
  components: { Comments, CommentsForm },
  data() {
    return {
      comments: [],
      formVisible: false,
      evtSource: null,
      switchChecked: true,
      formSending: false,
    };
  },

  methods: {
    showReplyDialog(parentCommentId) {
      this.parentCommentId = parentCommentId;
      this.formVisible = true;
    },

    showForm() {
      this.formVisible = !this.formVisible;
    },

    addComment(comment) {
      this.comments.push(JSON.parse(comment.data));
    },

    toggleStreamComment() {
      this.switchChecked = !this.switchChecked;
    },

    async fetchComments() {
      try {
        const response = await axios.get('http://194.67.93.117:80/comments');
        this.comments.push(...response.data.reverse());
      } catch (error) {
        console.log('Ошибка при получении комментариев');
        console.log(error);
      }
    },

    async postComment(comment) {
      try {
        this.formSending = true;
        const url = 'http://194.67.93.117:80/comments';
        const commentBody = {
          author: comment.author,
          text: comment.text,
          reaction: comment.reaction,
          parentId: comment.parentId,
        };

        const response = await axios.post(url, commentBody, {
          headers: {
            'Content-Type': 'application/json',
            Username: 'dmitriytheprogrammer',
          },
        });

        console.log(response.data);

      } catch (error) {

        console.log(error);
      }
      this.formSending = false;
    },

    serverSentEvent() {
      if (!this.switchChecked) {
        this.closeConnection();
      } else {
        this.openConnection();
      }
    },

    openConnection() {
      this.evtSource = new EventSource(
        'http://194.67.93.117:80/comments/stream'
      );
      this.evtSource.onmessage = this.addComment;
    },

    closeConnection() {
      if (this.evtSource) {
        this.evtSource.close();
        this.evtSource = null;
      }
    },
  },

  computed: {
    commentsLength() {
      return this.comments.length;
    },
    appSending(){
      return this.formSending
    }
  },

  watch: {
    switchChecked: 'serverSentEvent',
  },
  beforeMount() {
    this.fetchComments();
  },
  mounted() {
    this.openConnection();
  },

};

</script>
<style lang="scss">
@use '@/assets/scss/mixin' as *;
@use '@/assets/scss/function' as *;
@use '@/assets/scss/variables' as *;

html,
body,
body div,
h1,
h2,
h3,
h4,
h5,
h6,
p,
img,
ul,
li,
fieldset,
form,
label,
article,
aside,
figure,
footer,
header,
menu,
nav,
section,
time,
summary {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font-weight: normal;
  vertical-align: baseline;
  background: transparent;
  list-style: none;
}

article,
aside,
figure,
footer,
header,
nav,
section,
details,
summary {
  display: block;
}

html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}
img,
object,
embed {
  max-width: 100%;
}

ul {
  list-style: none;
}

blockquote,
q {
  quotes: none;
}

blockquote:before,
blockquote:after,
q:before,
q:after {
  content: '';
  content: none;
}

a {
  margin: 0;
  padding: 0;
  font-size: 100%;
  vertical-align: baseline;
  background: transparent;
}

del {
  text-decoration: line-through;
}

abbr[title],
dfn[title] {
  border-bottom: 1px dotted #000;
  cursor: help;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}
th {
  font-weight: bold;
  vertical-align: bottom;
}
td {
  font-weight: normal;
  vertical-align: top;
}

hr {
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid #ccc;
  margin: 1em 0;
  padding: 0;
}

input,
select {
  vertical-align: middle;
}

pre {
  white-space: pre; /* CSS2 */
  white-space: pre-wrap; /* CSS 2.1 */
  white-space: pre-line; /* CSS 3 (and 2.1 as well, actually) */
  word-wrap: break-word; /* IE */
}

input[type='radio'] {
  vertical-align: text-bottom;
}
input[type='checkbox'] {
  vertical-align: bottom;
}
.ie7 input[type='checkbox'] {
  vertical-align: baseline;
}
.ie6 input {
  vertical-align: text-bottom;
}

select,
input,
textarea {
  font: 99% sans-serif;
}

table {
  font-size: inherit;
  font: 100%;
}

small {
  font-size: 85%;
}

strong {
  font-weight: bold;
}

td,
td img {
  vertical-align: top;
}
sub,
sup {
  font-size: 75%;
  line-height: 0;
  position: relative;
}
sup {
  top: -0.5em;
}
sub {
  bottom: -0.25em;
}

pre,
code,
kbd,
samp {
  font-family: monospace, sans-serif;
}

.clickable,
label,
input[type='button'],
input[type='submit'],
input[type='file'],
button {
  cursor: pointer;
}

button,
input,
select,
textarea {
  margin: 0;
}

button,
input[type='button'] {
  width: auto;
  overflow: visible;
}

.ie7 img {
  -ms-interpolation-mode: bicubic;
}
.clearfix:before,
.clearfix:after {
  content: '\0020';
  display: block;
  height: 0;
  overflow: hidden;
}
.clearfix:after {
  clear: both;
}
.clearfix {
  zoom: 1;
}
html {
  font-size: 62.5%;
}
body {
  font-family: 'Open Sans';
  font-style: normal;
  font-weight: 400;
  background-color: color(background-color);
}
@font-face {
  font-family: 'Open Sans';
  src: url('@/assets/fonts/OpenSans-Bold.eot');
  src: url('@/assets/fonts/OpenSans-Bold.eot?#iefix')
      format('embedded-opentype'),
    url('@/assets/fonts/OpenSans-Bold.woff2') format('woff2'),
    url('@/assets/fonts/OpenSans-Bold.woff') format('woff'),
    url('@/assets/fonts/OpenSans-Bold.ttf') format('truetype'),
    url('@/assets/fonts/OpenSans-Bold.svg#OpenSans-Bold') format('svg');
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Open Sans';
  src: url('@/assets/fonts/OpenSans-SemiBold.eot');
  src: url('@/assets/fonts/OpenSans-SemiBold.eot?#iefix')
      format('embedded-opentype'),
    url('@/assets/fonts/OpenSans-SemiBold.woff2') format('woff2'),
    url('@/assets/fonts/OpenSans-SemiBold.woff') format('woff'),
    url('@/assets/fonts/OpenSans-SemiBold.ttf') format('truetype'),
    url('@/assets/fonts/OpenSans-SemiBold.svg#OpenSans-SemiBold') format('svg');
  font-weight: 600;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Open Sans';
  src: url('@/assets/fonts/OpenSans-Regular.eot');
  src: url('@/assets/fonts/OpenSans-Regular.eot?#iefix')
      format('embedded-opentype'),
    url('@/assets/fonts/OpenSans-Regular.woff2') format('woff2'),
    url('@/assets/fonts/OpenSans-Regular.woff') format('woff'),
    url('@/assets/fonts/OpenSans-Regular.ttf') format('truetype'),
    url('@/assets/fonts/OpenSans-Regular.svg#OpenSans-Regular') format('svg');
  font-weight: 400;
  font-style: normal;
  font-display: swap;
}





.comments {
  padding-bottom: 20px;
}
.comments__wrapper {
  @include wrapper(block);
}
.comments__title {
  @include title();
  margin-top: 40px;
}
.comments__switch {
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
}
.comments__text {
  @include subTitle();
}
.comments__subtitle {
  @include subTitle();
  margin-top: 15px;
}
.comments__button {
  margin-top: 15px;
  @include secondTitle();
}
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  border: 0;
  padding: 0;
  white-space: nowrap;
  clip-path: inset(100%);
  clip: rect(0 0 0 0);
  overflow: hidden;
}




</style>
