<template>
  <div id="App">
    <div id="headingDiv">
      <button class="heading" @click="handleHeading(1)">H</button>
      <button class="addIn" @click="handleAddIn('**bold**')"><b>B</b></button>
      <button class="addIn" @click="handleAddIn('*italic*')"><i>i</i></button>
      <button class="addIn" @click="handleAddIn('~~strikethrough~~')"><del>s</del></button>
      <button class="addIn" @click="handleAddIn('- ')">⚈</button>
      <button class="addIn" @click="handleAddIn('1. ')">📝</button>
      <button class="addIn" @click="handleAddIn('- [x] ')">✅</button>
      <button class="addIn" @click="handleAddIn('> ')">💬</button>
      <button class="addIn" @click="handleAddIn('```')">👨‍💻</button>
      <button class="addIn" @click="handleAddIn('![alt text](url)')">🖼️</button>
      <button class="addIn" @click="handleAddIn('[alt text](url)')">🔗</button>
      <button class="addIn" @click="handleAddIn('---')">―</button>
    </div>

    <div id="editorDiv">
      <div id="editorWindow">
        <textarea 
          id="editor" 
          v-model="inputText" 
          :placeholder="inputPlaceholder">
        </textarea>
      </div>
      <div id="displayWindow">
        <div id="display" v-html="stylizedMarkdown"></div>
      </div>
    </div>
  </div>
</template>

<script>
import marked from "marked";
import DOMPurify from "dompurify";

export default {
  name: "App",
  data: () => ({
    inputText: "", // Start empty so placeholder appears
    inputPlaceholder: "# Welcome To Markdown Editor", // Placeholder text
  }),
  computed: {
    stylizedMarkdown() {
    return DOMPurify.sanitize(marked(this.inputText, { breaks: true }));
    },
  },
  methods: {
    handleHeading(size) {
      const textarea = document.getElementById("editor");
      const start = textarea.selectionStart;
      const end = textarea.selectionEnd;
      const selectedText = this.inputText.substring(start, end);
      const formattedText = `#${"#".repeat(size - 1)} ${selectedText || "Heading"}`;

      this.inputText =
        this.inputText.substring(0, start) +
        formattedText +
        this.inputText.substring(end);
    },
    handleAddIn(wrapper) {
      const textarea = document.getElementById("editor");
      const start = textarea.selectionStart;
      const end = textarea.selectionEnd;
      const selectedText = this.inputText.substring(start, end);
      let formattedText;

      if (wrapper === "**bold**") {
        formattedText = `**${selectedText || "bold"}**`;
      } else if (wrapper === "*italic*") {
        formattedText = `*${selectedText || "italic"}*`;
      } else if (wrapper === "~~strikethrough~~") {
        formattedText = `~~${selectedText || "strikethrough"}~~`;
      } else if (wrapper === "- ") {
        formattedText = `- ${selectedText || "list item"}`;
      } else if (wrapper === "1. ") {
        formattedText = `1. ${selectedText || "numbered item"}`;
      } else if (wrapper === "- [x] ") {
        formattedText = `- [x] ${selectedText || "completed task"}`;
      } else if (wrapper === "> ") {
        formattedText = `> ${selectedText || "quote"}`;
      } else if (wrapper === "```") {
        formattedText = `\n\`\`\`\n${selectedText || "code"}\n\`\`\`\n`;
      } else if (wrapper === "![alt text](url)") {
        formattedText = `![${selectedText || "alt text"}](url)`;
      } else if (wrapper === "[alt text](url)") {
        formattedText = `[${selectedText || "alt text"}](url)`;
      } else if (wrapper === "---") {
        formattedText = `\n---\n`;
      }

      this.inputText =
        this.inputText.substring(0, start) +
        formattedText +
        this.inputText.substring(end);
    },
  },
};
</script>

<style lang="scss">
body, html {
  height: 100%;
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100%;
  display: flex;
  flex-direction: column;
}

#editorDiv {
  display: flex;
  height: 100%; /* Make editor container take full height */
  flex-direction: row; /* Ensure side-by-side layout by default */
}

#editorWindow,
#displayWindow {
  padding-top: 20px;
  width: 40%;
  height: 100%; /* Make both columns take full height */
}

#displayWindow {
  padding-left: 1rem;
}

#editorWindow {
  margin-right: 1rem;

  #editor {
    background-color: #f5f5f5;
    border: none;
    height: 100%;
    width: 99%;
    resize: none;
    font-size: 1.25rem;
    border-radius: 10px;
    color: #2c3e50;
    padding: 10px;
    margin: 10px;
  }

  #editor::placeholder {
    color: #9c9c9c;
  }
}

#headingDiv {
  button {
    background-color: #f5f5f5;
    border: 1px solid;
    margin: 5px;
    border-radius: 20px;
  }
}

/* Media Query for Mobile Devices */
@media (max-width: 768px) {
  #editorDiv {
    flex-direction: column; /* Stack the columns on mobile */
  }

  #editorWindow,
  #displayWindow {
    width: 100%; /* Make each section take full width on mobile */
  }

  #displayWindow {
    padding-left: 1rem;
    padding-top: 2rem; /* Optional: add padding to the top of the display section */
  }
}
</style>
