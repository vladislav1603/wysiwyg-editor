<template>
    <div>
      <div id="toolbar">
        <button @click="undo"><img src="../assets/undo.png" alt="Undo" /></button>
        <button @click="redo"><img src="../assets/redo.png" alt="Redo" /></button>
        <button @click="setHeader"><img src="../assets/h1.png" alt="H1" /></button>
        <button @click="indent"><img src="../assets/h1.png" alt="H1" /><img src="../assets/h1.png" alt="H1" /></button>
        <button @click="addImage"><img src="../assets/image.png" alt="Add Image" /></button>
        <button @click="copyHTML">Copy HTML</button>
    </div>
    <div ref="editorContainer" id="editorContainer"></div>
  </div>
</template>

<script>
import Quill from 'quill';

export default {
  name: 'TextEditor',
  mounted() {
    this.quill = new Quill(this.$refs.editorContainer, {
      theme: 'snow',
      modules: {
        toolbar: false,
        history: {
          delay: 2000,
          maxStack: 500,
          userOnly: true
        }
      }
    });
  },
  methods: {
    undo() {
      this.quill.history.undo();
    },
    redo() {
      this.quill.history.redo();
    },
    toggleHeader() {
      const selection = this.quill.getSelection();
      if (selection) {
        const format = this.quill.getFormat(selection);
        if (format.header === 1) {
          this.quill.format('header', false);
        } else {
          this.quill.format('header', 1);
        }
      }
    },
    toggleIndent() {
      const selection = this.quill.getSelection();
      if (selection) {
        const format = this.quill.getFormat(selection);
        const currentIndent = format.indent || 0;
        if (currentIndent >= 3) {
          this.quill.format('indent', false);
        } else {
          this.quill.format('indent', currentIndent + 1);
        }
      }
    },
    insertImage() {
      const url = prompt('Введите URL изображения:');
      if (url) {
        const range = this.quill.getSelection();
        this.quill.insertEmbed(range.index, 'image', url);
      }
    },
    copyHtml() {
      const html = this.$refs.editorContainer.querySelector('.ql-editor').innerHTML;
      navigator.clipboard.writeText(html).then(() => {
        alert('HTML скопирован в буфер обмена');
      });
    }
  }
};
</script>

<style src="./TextEditor.css"></style>