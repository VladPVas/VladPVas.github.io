<template>
<div class="chat-widget">
  <div class="messages" id="scroll">
    <div v-for="message in messages" :key="message.id" :class="message.from">
      {{ message.text }}
    </div>
    <div class="suggestions">
      <button @click="sendSuggested('Установить будильник')">Установить будильник</button>
      <button @click="sendSuggested('Заказать пиццу')">Заказать пиццу</button>
      <button @click="sendSuggested('Вывести погоду')">Вывести погоду</button>
    </div>
  </div>
  <div class="input">
    <input type="text" v-model="newMessage" @keyup.enter="sendMessage" />
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      messages: [],
      newMessage: ""
    };
  },
  methods: {
    sendMessage() {
      if (this.newMessage) {
        // Добавляем новое сообщение с пользовательским вводом
        this.messages.push({
          id: Date.now(),
          text: this.newMessage,
          from: "user" // Пользовательское сообщение
        });

        // Генерируем ответ от бота, основанный на пользовательском вводе
        let botResponse = "";

        if (this.newMessage === "Заказать пиццу") {
          botResponse = "Хорошо, я закажу пиццу. Что еще могу сделать?";
        } else if (this.newMessage === "Установить будильник") {
          botResponse = "Я установлю будильник для вас. Есть еще что-то, что я могу сделать?";
        } else if (this.newMessage === "Вывести погоду") {
          botResponse = "Окей, вывожу погоду. Что-нибудь еще интересует?";
        } else {
          botResponse = "Извините, я не могу понять ваш запрос. Попробуйте еще раз.";
        }

        // Добавляем ответ бота в виджет чата
        this.messages.push({
          id: Date.now() + 1,
          text: botResponse,
          from: "bot" // Ответ бота
        });

        // Очищаем поле ввода сообщения
        this.newMessage = "";
        this.scrollToBottom()
      }
    },
    
    sendSuggested(suggestion) {
      this.newMessage = suggestion
      this.sendMessage()
    },

    scrollToBottom() {
      const scroll = document.getElementById("scroll")
      scroll.scrollTop = scroll.scrollHeight
    }
  }
};
</script>

<style scoped>


.chat-widget {
  display: flex;
  flex-direction: column;
  width: 400px;
  height: 500px;
  border-radius: 10px;
  border: solid 2px black;
  background-color: darkgray;
  padding: 10px;
}

.messages {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex-grow: 1;
  overflow-y: auto;
}

.messages>div {
  margin: 10px;
  padding: 10px;
  border-radius: 5px;
  max-width: 50%;
  min-width: 20%;
  width: fit-content;
  max-height: fit-content;
  text-align: start;
  overflow-wrap: initial;
  word-break: break-word;
  position: relative;
}

.user {
  align-self: flex-end;
  background-color: teal;
}

.bot {
  background-color: #f0f0f0;
}

.suggestions {
  display: flex;
  flex-direction: column;
  align-self: stretch;
  justify-content: space-between;
}

.suggestions button {
  padding: 5px;
  border-radius: 5px;
  width: fit-content;
  max-height: fit-content;
  text-align: start;
  overflow-wrap: initial;
  word-break: break-word;
  position: relative;
}

.input {
  display: flex;
  align-items: center;
  padding: 10px;
}

.input input {
  flex-grow: 1;
  padding: 5px;
  border-radius: 10px;
  border: solid 2px black;
  text-align: left;
  overflow-wrap: break-word;
}
</style>
