<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-card>
          <v-card-title>
            Datos
            <v-spacer></v-spacer>
          </v-card-title>
          <v-data-table :headers="headers" :items="Records" :items-per-page="5" :footer-props="{
            showFirstLastPage: true,
            firstIcon: 'mdi-arrow-collapse-left',
            lastIcon: 'mdi-arrow-collapse-right',
            prevIcon: 'mdi-minus',
            nextIcon: 'mdi-plus'
          }">
            <template v-slot:item="props">
              <tr>
                <td>{{ props.item.ProdServCode }}</td>
                <td>{{ props.item.Description }}</td>
                <td>
                  <v-btn-toggle borderless>
                    <v-btn @click="getSecondData(props.item)">
                      <v-icon>mdi-open-in-new</v-icon>
                    </v-btn>
                  </v-btn-toggle>
                </td>
              </tr>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
      <v-dialog v-model="dialog" width="800">
        <v-card>
          <v-card-title class="text-h5 grey lighten-2">
            Resultados de la solicitud
          </v-card-title>
          <v-card-text>
            <v-text-field label="Code" v-model="responseData.Code" disabled></v-text-field>
            <v-text-field label="CurrentPage" v-model="responseData.CurrentPage" disabled></v-text-field>
            <v-text-field label="Message" v-model="responseData.Message" disabled></v-text-field>
            <v-text-field label="Description" v-model="responseData.Description" disabled></v-text-field>
            <v-text-field label="IdSatCode" v-model="responseData.IdSatCode" disabled></v-text-field>
            <v-text-field label="IncludeIepsTransferred" v-model="responseData.IncludeIepsTransferred"
              disabled></v-text-field>
            <v-text-field label="IncludeIvaTransferred" v-model="responseData.IncludeIvaTransferred"
              disabled></v-text-field>
            <v-text-field label="ProdServCode" v-model="responseData.ProdServCode" disabled></v-text-field>

          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">
              Cerrar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      headers: [
        {
          text: 'ProdServCode',
          align: 'center',
          sortable: false,
          value: 'ProdServCode',
        },
        {
          text: 'Description',
          align: 'center',
          sortable: false,
          value: 'Description'
        },
        {
          text: 'Ver Mas',
          align: 'center',
          sortable: false,
          value: ''
        },
      ],
      Records: [],
      responseData: {
        Code: "",
        CurrentPage: "",
        Message: "",
        Description: "",
        IdSatCode: "",
        IncludeIvaTransferred: "",
        IncludeIepsTransferred: "",
        ProdServCode: "",
      },

      dialog: false
    }
  },
  created() {
    //Carga la funcion
    this.getData();
  },
  methods: {
    // Obtiene la data que se muestra al cargar la pagina 
    async getData() {
      const urlGet = "https://services.schoolmanager.education/v1/sat/satproductserv/5251/10"
      try {
        const response = await fetch(urlGet);
        if (response.ok) {
          const data = await response.json();
          this.Records = data.Records;
        } else {
          console.error('Error al obtener datos:', response.statusText);
        }
      } catch (error) {
        console.error('Error al obtener datos:', error);
      }
    },
    // Obtiene la data al hacer click consumientdo la otra API
    async getSecondData(item) {
      this.dialog = true;
      // Estructura necesaria para obtener el GET haciendo POST
      const requestData = {
        "IdSatCode": item.IdSatCode,
        "ProdServCode": item.ProdServCode,
        "Description": item.Description,
        "IncludeIvaTransferred": item.IncludeIvaTransferred,
        "IncludeIepsTransferred": item.IncludeIepsTransferred,
        "Pagination": {
          "CurrentPage": "1",
          "PerPager": "1"
        }
      };
      const urlPost = "https://services.schoolmanager.education/v1/sat/satproductserv/find";

      try {
        const response = await fetch(urlPost, {
          method: "POST",
          body: JSON.stringify(requestData),
          headers: {
            "Content-Type": "application/json"
          }
        });
        if (response.ok) {
          const responseData = await response.json();
          //console.log(responseData)
          //elementos fuera 
          this.responseData.Code = responseData.Code;
          this.responseData.CurrentPage = responseData.CurrentPage;
          this.responseData.Message = responseData.Message;
          // Accede a IdSatCode dentro de Records y lo asígna
          if (responseData.Records && responseData.Records.length > 0) {
            const record = responseData.Records[0];
            // Elementos internos

            this.responseData.Description = record.Description;
            this.responseData.IdSatCode = record.IdSatCode;
            this.responseData.IncludeIvaTransferred = record.IncludeIvaTransferred;
            this.responseData.IncludeIepsTransferred = record.IncludeIepsTransferred;
            this.responseData.ProdServCode = record.ProdServCode;
          } else {
            // Si no hay datos en Records, puedes mostrar un mensaje de error
            console.error("No se encontraron datos en Records");
          }


        } else {
          console.error('Error al realizar la solicitud:', response.statusText);
        }
      } catch (error) {
        console.error("Ha ocurrido un error: " + error);
      }
    }
  }
}
</script>
