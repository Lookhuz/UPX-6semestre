<template>
  <v-container id="contact-us">
    <v-row>
      <v-col cols="12">
        <h2>Contato</h2>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field label="Seu Nome" v-model="nome"></v-text-field>
      </v-col>
      <v-col cols="12" md="6">
        <v-text-field label="Título do Email" v-model="titulo"></v-text-field>
      </v-col>
      <v-col cols="12">
        <v-textarea
          label="Mensagem"
          v-model="mensagem"
          :counter="500"
          maxlength="500"
        ></v-textarea>
      </v-col>
      <v-col cols="12">
        <v-btn color="green darken-1" @click="enviarEmail">Enviar</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import emailjs from 'emailjs-com';

export default {
  name: 'ContactUs',
  data() {
    return {
      nome: '',
      titulo: '',
      mensagem: '',
    };
  },
  methods: {
    enviarEmail() {
      if (this.nome && this.titulo && this.mensagem) {
        const templateParams = {
          from_name: this.nome,
          subject: this.titulo,
          message_html: this.mensagem,
        };

        emailjs
          .send(
            'service_m7rhlbe', // Substitua pelo seu Service ID do EmailJS
            'SEU_TEMPLATE_ID', // Substitua pelo seu Template ID do EmailJS
            templateParams,
            'SEU_USER_ID' // Substitua pelo seu User ID do EmailJS
          )
          .then(
            (response) => {
              alert('Email enviado com sucesso!');
              this.nome = '';
              this.titulo = '';
              this.mensagem = '';
            },
            (error) => {
              alert('Ocorreu um erro ao enviar o email:', error);
            }
          );
      } else {
        alert('Por favor, preencha todos os campos.');
      }
    },
  },
};
</script>

<style scoped>
</style>
