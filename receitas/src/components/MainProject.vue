<template>
  <v-container id="main-project">
    <v-row>
      <v-divider horizontal class="py-2"></v-divider>
      <v-col cols="12">
        <div class="d-flex justify-center text-h4 font-weight-bold">
          <span>Planeje Sua Plantação</span>
        </div>
      </v-col>

      <!-- Selecione a cidade e a quantidade de mudas -->
      <v-col cols="12" md="6">
        <div class="d-flex justify-center mb-4">
          <h4>Diga a cidade que você deseja fazer sua plantação</h4>
        </div>
        <v-select
          :items="cidades"
          label="Selecione a cidade"
          v-model="cidadeSelecionada"
        ></v-select>
      </v-col>
      <v-col cols="12" md="6">
        <div class="d-flex justify-center mb-4">
          <h4>Diga quantas mudas você irá plantar</h4>
        </div>
        <v-combobox
          :items="quantidades"
          label="Quantidade de mudas"
          v-model="quantidadeSelecionada"
        ></v-combobox>
      </v-col>

      <v-col cols="12">
        <div class="d-flex justify-center">
          <h4>Selecione a Planta que você deseja fazer sua plantação</h4>
        </div>
        <v-row class="justify-space-around mt-3">
          <v-col
            v-for="(planta, index) in plantas"
            :key="index"
            cols="auto"
            @click="selecionarPlanta(planta.nome)"
          >
            <v-card
              :elevation="plantaSelecionada === planta.nome ? 8 : 2"
              :color="plantaSelecionada === planta.nome ? 'rgb(15, 46, 15)' : 'white'"
              class="d-flex flex-column align-center pa-3"
            >
              <v-icon size="40">{{ planta.icone }}</v-icon>
              <p class="text-center">{{ planta.nome }}</p>
            </v-card>
          </v-col>
        </v-row>
      </v-col>

      <v-col cols="12">
        <div class="d-flex justify-center mt-5">
          <v-btn color="rgb(15, 46, 15)" @click="toggleCalendario">
            {{ calendarioVisivel ? 'Ocultar Calendário' : 'Mostrar Calendário' }}
          </v-btn>
        </div>
      </v-col>

      <v-col cols="12" v-if="calendarioVisivel">
        <v-card>
          <v-card-title>
            Calendário de Plantio para {{ plantaSelecionada }} em {{ cidadeSelecionada }}
          </v-card-title>
          <v-card-text>
            <v-calendar
              type="month"
              :start="startDate"
              :end="endDate"
              :events="eventosCalendario"
              :weekdays="[0, 1, 2, 3, 4, 5, 6]"
              color="green lighten-4"
              :event-color="getEventColor"
            ></v-calendar>
            <div class="text-subtitle-1 mt-5">
              <span>
                <strong>Dicas de Cultivo:</strong> {{
                  dadosPlantio[cidadeSelecionada][plantaSelecionada].dicas
                }}
              </span>
            </div>
            <div class="text-subtitle-1">
              <span>
                <strong>Quantidade de Mudas:</strong> {{ quantidadeSelecionada }}
              </span>
            </div>

            <!-- Novo: Selecionar data de plantio -->
            <div class="text-h6 mt-4">
              <span>
                <strong>Selecione a data que você plantou ou deseja plantar:</strong>
              </span>
            </div>
            <v-menu
              ref="menu"
              v-model="menu"
              :close-on-content-click="false"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ props }">
                <v-text-field
                  v-model="dataPlantioUsuario"
                  label="Selecione a data de plantio"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="props"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="dataPlantioUsuario"
                :min="today"
                @click:date="menu = false"
              ></v-date-picker>
            </v-menu>
            <div class="text-subtitle-1">
              <span v-if="dataColheita">
                <strong>Você poderá colher entre:</strong> {{ dataColheita }}
              </span>
            </div>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { useDate } from 'vuetify';

export default {
  name: 'MainProject',
  data() {
    return {
      cidades: ['Sorocaba', 'Votorantim', 'Ibiúna', 'Itu', 'Salto de Pirapora'],
      quantidades: ['10', '20', '30', '40', '50'],
      cidadeSelecionada: null,
      plantaSelecionada: null,
      quantidadeSelecionada: null,
      calendarioVisivel: false,
      eventosCalendario: [],
      menu: false,
      dataPlantioUsuario: null,
      dataColheita: '',
      startDate: null,
      endDate: null,
      today: '', // Nova propriedade para a data atual
      // Lista de plantas com ícones
      plantas: [
        { nome: 'Alface', icone: 'mdi-leaf' },
        { nome: 'Rúcula', icone: 'mdi-seed' },
        { nome: 'Couve', icone: 'mdi-sprout' },
        { nome: 'Milho', icone: 'mdi-corn' },
        { nome: 'Melancia', icone: 'mdi-fruit-watermelon' },
        { nome: 'Cenoura', icone: 'mdi-carrot' },
      ],
      dadosPlantio: {
        Sorocaba: {
          Alface: {
            epocaPlantio: { mesInicio: 3, mesFim: 9 }, // Março a Setembro
            tempoColheita: { minDias: 35, maxDias: 70 },
            dicas:
              'O alface prefere solo fértil e bem drenado, com pH entre 6 e 7. É recomendável preparar o solo com matéria orgânica e irrigar regularmente, mas sem encharcar. Plantio ideal em locais com sol direto pela manhã e sombra parcial à tarde, especialmente no verão para evitar queimaduras nas folhas.',
          },
          Couve: {
            epocaPlantio: { mesInicio: 3, mesFim: 8 }, // Março a Agosto
            tempoColheita: { minDias: 60, maxDias: 90 },
            dicas:
              'A Cenoura se desenvolve bem em solo fértil, com pH entre 6 e 7,5. Prefere locais com sol pleno e solo úmido, mas bem drenado. É importante realizar a adubação com matéria orgânica e manter o solo sempre levemente úmido. Recomenda-se o plantio em canteiros elevados para evitar acúmulo de água.',
          },
        },
        Ibiúna: {
          Alface: {
            epocaPlantio: { mesInicio: 2, mesFim: 9 }, // Fevereiro a Setembro
            tempoColheita: { minDias: 30, maxDias: 65 },
            dicas:
              'Em Ibiúna, que tem clima mais ameno, o alface pode ser plantado ao longo de boa parte do ano. Prefere solo fértil e arenoso, com adição de matéria orgânica. Regas devem ser frequentes, especialmente durante períodos de seca, mas sem encharcar. Evite plantar durante os períodos mais frios do ano para não comprometer o crescimento.',
          },
          Couve: {
            epocaPlantio: { mesInicio: 3, mesFim: 8 }, // Março a Agosto
            tempoColheita: { minDias: 60, maxDias: 100 },
            dicas:
              'A Cenoura se adapta bem ao clima ameno de Ibiúna. É importante preparar o solo com composto orgânico e mantê-lo bem drenado. O cultivo em pleno sol é ideal, mas também pode ser feito em meia-sombra. Recomenda-se rega constante, sem deixar o solo encharcado. A Cenoura é uma planta que demanda nutrientes, portanto, adubação orgânica é recomendada ao longo do cultivo.',
          },
        },
      },
    };
  },
  mounted() {
    const adapter = useDate();
    this.adapter = adapter;
    // Definir a data atual no formato 'YYYY-MM-DD'
    const today = new Date();
    this.today = this.formatDateISO(today);
  },
  watch: {
    dataPlantioUsuario(newVal) {
      if (newVal && this.cidadeSelecionada && this.plantaSelecionada) {
        const dados = this.dadosPlantio[this.cidadeSelecionada][this.plantaSelecionada];
        const tempoColheita = dados.tempoColheita;
        const dataPlantio = new Date(newVal);

        const dataColheitaMin = new Date(dataPlantio);
        dataColheitaMin.setDate(dataColheitaMin.getDate() + tempoColheita.minDias);

        const dataColheitaMax = new Date(dataPlantio);
        dataColheitaMax.setDate(dataColheitaMax.getDate() + tempoColheita.maxDias);

        this.dataColheita = `entre ${this.formatDate(dataColheitaMin)} e ${this.formatDate(
          dataColheitaMax
        )}`;
      }
    },
  },
  methods: {
    selecionarPlanta(nome) {
      this.plantaSelecionada = nome;
    },
    toggleCalendario() {
      this.calendarioVisivel = !this.calendarioVisivel;
      if (this.calendarioVisivel) {
        this.mostrarCalendario();
      }
    },
    mostrarCalendario() {
      if (
        this.cidadeSelecionada &&
        this.plantaSelecionada &&
        this.quantidadeSelecionada
      ) {
        const dados = this.dadosPlantio[this.cidadeSelecionada][this.plantaSelecionada];

        // Obter a data atual e o ano atual
        const today = new Date();
        let currentYear = today.getFullYear();
        const mesInicio = dados.epocaPlantio.mesInicio - 1; // Meses em JavaScript começam em 0
        const mesFim = dados.epocaPlantio.mesFim - 1;

        let dataInicioPlantio = new Date(currentYear, mesInicio, 1);
        let dataFimPlantio = new Date(currentYear, mesFim + 1, 0); // Último dia do mês fim

        // Se a data atual for após o fim da época de plantio, ajustar para o próximo ano
        if (today > dataFimPlantio) {
          currentYear += 1;
          dataInicioPlantio = new Date(currentYear, mesInicio, 1);
          dataFimPlantio = new Date(currentYear, mesFim + 1, 0);
        }

        // Se a data atual estiver dentro da época de plantio, mas após o início, ajustar dataInicioPlantio para hoje
        if (today > dataInicioPlantio && today < dataFimPlantio) {
          dataInicioPlantio = new Date(today);
        }

        // Gerar eventos para cada dia no período de plantio
        let eventosPlantio = [];
        let date = new Date(dataInicioPlantio);
        while (date <= dataFimPlantio) {
          eventosPlantio.push({
            title: 'Bom para plantar',
            start: new Date(date),
            end: new Date(date),
            color: 'green',
            allDay: true,
          });
          date.setDate(date.getDate() + 1);
        }

        this.eventosCalendario = eventosPlantio;
        this.startDate = dataInicioPlantio;
        this.endDate = dataFimPlantio;
        this.calendarioVisivel = true;
      } else {
        alert('Por favor, preencha todos os campos.');
      }
    },
    getEventColor(event) {
      return event.color;
    },
    formatDate(date) {
      return date.toLocaleDateString('pt-BR');
    },
    formatDateISO(date) {
      return date.toISOString().substr(0, 10);
    },
  },
};
</script>

<style scoped>
.v-card {
  cursor: pointer;
}
.v-card:hover {
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
}
</style>
