<template>
  <v-container id="main-project">
    <v-row>
      <v-col cols="12">
        <h2>Planeje Sua Plantação</h2>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          :items="cidades"
          label="Selecione a cidade"
          v-model="cidadeSelecionada"
        ></v-select>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          :items="plantas"
          label="Selecione a planta"
          v-model="plantaSelecionada"
        ></v-select>
      </v-col>
      <v-col cols="12" md="4">
        <v-combobox
          :items="quantidades"
          label="Quantidade de mudas"
          v-model="quantidadeSelecionada"
        ></v-combobox>
      </v-col>
      <v-col cols="12">
        <v-btn color="green darken-1" @click="mostrarCalendario">Mostrar Calendário</v-btn>
      </v-col>
      <v-col cols="12" v-if="calendarioVisivel">
        <v-card>
          <v-card-title>
            Calendário de Plantio para {{ plantaSelecionada }} em {{ cidadeSelecionada }}
          </v-card-title>
          <v-card-text>
            <v-calendar
              type="month"
              :events="eventosCalendario"
              :weekdays="[0, 1, 2, 3, 4, 5, 6]"
              color="green lighten-4"
              event-color="indigo"
            ></v-calendar>
            <p>
              <strong>Dicas de Cultivo:</strong> {{
                dadosPlantio[cidadeSelecionada][plantaSelecionada].dicas
              }}
            </p>
            <p>
              <strong>Quantidade de Mudas:</strong> {{ quantidadeSelecionada }}
            </p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'MainProject',
  data() {
    return {
      cidades: ['Sorocaba', 'Votorantim', 'Itu', 'Salto de Pirapora'],
      plantas: ['Alface', 'Couve', 'Rúcula', 'Salsa'],
      quantidades: ['10', '20', '30', '40', '50'],
      cidadeSelecionada: null,
      plantaSelecionada: null,
      quantidadeSelecionada: null,
      calendarioVisivel: false,
      eventosCalendario: [], // Lista para armazenar eventos de plantio e colheita
      dadosPlantio: {
        Sorocaba: {
          Alface: {
            epocaPlantio: '2024-03-01', // Data simulada de início de plantio
            epocaColheita: '2024-06-01', // Data simulada de colheita
            dicas: 'Prefere clima ameno e solo bem drenado.',
          },
          Couve: {
            epocaPlantio: '2024-02-01',
            epocaColheita: '2024-05-01',
            dicas: 'Necessita de solo fértil e boa irrigação.',
          },
        },
        // ... outras cidades e plantas
      },
    };
  },
  methods: {
    mostrarCalendario() {
      if (
        this.cidadeSelecionada &&
        this.plantaSelecionada &&
        this.quantidadeSelecionada
      ) {
        const dados = this.dadosPlantio[this.cidadeSelecionada][this.plantaSelecionada];
        this.eventosCalendario = [
          {
            name: 'Início do Plantio',
            start: dados.epocaPlantio,
            color: 'green',
          },
          {
            name: 'Início da Colheita',
            start: dados.epocaColheita,
            color: 'orange',
          },
        ];
        this.calendarioVisivel = true;
      } else {
        alert('Por favor, preencha todos os campos.');
      }
    },
  },
};
</script>

<style scoped>
</style>
