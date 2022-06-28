<template>
  <div>
    <v-dialog v-model="isOpenSet" width="1000" persistent>
      <v-card class="pa-10">
        <div align="center" class="text-h6">New Building</div>
        <v-col cols="12" class="px-0">
          <div>Label</div>
          <div>
            <v-text-field outlined v-model="labelName"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>No. of Floors</div>
          <div>
            <v-select
              outlined
              v-model="floors"
              :items="[1, 2, 3, 4, 5, 6]"
            ></v-select>
          </div>
        </v-col>
        <!-- <v-col cols="12" class="px-0">
          <div>Column</div>
          <div>
            <v-text-field outlined v-model="events.column"></v-text-field>
          </div>
        </v-col> -->
        <v-card-actions>
          <v-row align="center">
            <v-col align="end">
              <v-btn color="red" text @click="isOpenSet = false">
                Cancel
              </v-btn>
            </v-col>
            <v-col>
              <v-btn
                color="success"
                text
                @click="saveInput"
                :loading="buttonLoad"
              >
                Save
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="isOpen" width="1000" persistent>
      <v-card class="pa-10">
        <div align="center" class="text-h6">Settings</div>
        <v-col cols="12" class="px-0">
          <div>Row</div>
          <div>
            <v-text-field outlined v-model="events.row"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Column</div>
          <div>
            <v-text-field outlined v-model="events.column"></v-text-field>
          </div>
        </v-col>
        <v-card-actions>
          <v-row align="center">
            <v-col align="end">
              <v-btn color="red" text @click="isOpen = false"> Cancel </v-btn>
            </v-col>
            <v-col>
              <v-btn
                color="success"
                text
                @click="saveDesign"
                :loading="buttonLoad"
              >
                Save
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="isOpen" width="1000" persistent>
      <v-card class="pa-10">
        <div align="center" class="text-h6">Set up</div>
        <v-col cols="12" class="px-0">
          <div>Row</div>
          <div>
            <v-text-field outlined v-model="events.row"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Column</div>
          <div>
            <v-text-field outlined v-model="events.column"></v-text-field>
          </div>
        </v-col>
        <v-card-actions>
          <v-row align="center">
            <v-col align="end">
              <v-btn color="red" text @click="isOpen = false"> Cancel </v-btn>
            </v-col>
            <v-col>
              <v-btn
                color="success"
                text
                @click="saveDesign"
                :loading="buttonLoad"
              >
                Save
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-card class="pa-5 rounded-xl">
      <v-card elevation="5" class="pa-5 rounded-xl">
           <v-row class="py-10">
        <v-col>
          <div class="text-h5" align="start">
          <b>Building Management </b>
      </div>
      <div>
         <v-col align="end">
          <v-row>
            <v-col cols="auto">
              <div align="end">
                <v-btn
                  depressed
                  color="#404040"
                  dark
                  @click="addRow"
                  :loading="isLoaded"
                >
                 <v-icon>mdi-plus</v-icon> Row
                </v-btn>
              </div>
            </v-col>
            <v-col cols="auto">
              <div align="end">
                <v-btn
                  depressed
                  color="#404040"
                  dark
                  @click="addColumn"
                  :loading="isLoaded"
                >
                   <v-icon>mdi-plus</v-icon> Column
                </v-btn>
              </div>
            </v-col>
            <v-col cols="auto">
              <div align="end">
                <v-btn
                  depressed
                  color="#404040"
                  dark
                  @click="isOpen = true"
                  :loading="isLoaded"
                >
                  Build
                </v-btn>
              </div>
            </v-col>
            <v-col cols="auto">
              <div align="end">
                <v-btn
                  depressed
                  color="#404040"
                  dark
                  @click="saveForm"
                  :loading="isLoaded"
                >
                  Save
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-col>
      </div>
        </v-col>
       
      </v-row>
        <v-img src="/NEW_2.png" height="780" width="750">
          <div align="center">
          <v-row
            no-gutters
            v-for="(row, indexRow) in tiles"
            :key="indexRow"
            align="center"
          >
            <v-col
              cols="auto"
              align="start"
              v-for="(col, indexColumn) in row.column"
              :key="indexColumn"
              @click="editTile(indexColumn, indexRow)"
            >
              <v-card height="150" width="150" class="pa-5" color="transparent" elevation="1">
                <div align="end" v-if="col.label != ''">
                  <v-icon @click="viewFloor(col.label,col.floor)">mdi-stairs</v-icon>
                </div>
              <div class="pl-10">
                  <b style="font-size:10px">{{ col.label }}</b>
              </div>
                <!-- <div v-if="col.label != ''">
                  <v-img height="60" width="60" :src="`/${col.image}`"></v-img>
                </div> -->
              </v-card>
            </v-col>
          </v-row>
        </div>
        </v-img>
      </v-card>
    </v-card>
  </div>
</template>

<script>
export default {
  created() {
    this.eventsGetall();
  },
  data() {
    return {
      buttonLoad:false,
      isLoaded: false,
      buttonLoad: false,
      labelName: "",
      tiles: [],
      rowTile: 0,
      columnTile: 0,
      columns: 0,
      floors: 0,
      rows: 0,
      events: {
        row: 0,
        column: 0,
      },
      isOpen: false,
      isOpenSet: false,
    };
  },
  methods: {
    addRow(){
        this.tiles.push({ label: '', column: [] });
        for (let i = 0; i < parseInt(this.tiles[0]['column'].length); i++) {
          this.tiles[this.tiles.length-1]["column"].push({
            label: "",
            image: "",
            id: `${i} - ${1}`,
          });
        }
    },
    addColumn(){
       for(let x in this.tiles){
         this.tiles[x]['column'].push({
            label: "",
            image: "",
            id: `${''} - ''`,
          })
       }
    },
    viewFloor(label,floor){
      window.location.href=`/floor?building_name=${label}&floor=${floor}`
    },
    async eventsGetall() {
      const res = await this.$axios
        .get(`/building/`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem("token")}`,
          },
        })
        .then((res) => {
          for (let key in res.data) {
            this.tiles.push({ label: key, column: [] });
            for (let i in JSON.parse(res.data[key]["tiles"])) {
              this.tiles[key]["column"].push(
                JSON.parse(res.data[key]["tiles"])[i]
              );
            }
          }
          console.log(JSON.parse(res.data[0]["tiles"]));
        });
    },
     saveForm() {
            this.buttonLoad = true
      var characters = "abcdefghijklmnopqrstuvwxyz0123456789";
      var result = "";
      var chaactersLength = characters.length;

      for (var i = 0; i < 5; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * chaactersLength)
        );
      }

      for (let key in this.tiles) {``
        this.tiles[key].formatID = result;
        const res =  this.$axios
          .post(
            `/building/`,
            {
              tiles: JSON.stringify(this.tiles[key].column),
              formatID: result,
              row: key,
              floor: this.tiles[key].floor,
            },
            {
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            }
          )
          .then((res) => {
           

          });
      }
       this.buttonLoad =false
       alert('Successfully Saved.')
    },
    cancel() {},
    saveInput() {
      var image = "";
      this.tiles[this.rowTile]["column"][this.columnTile]["label"] =
        this.labelName;
      if (this.floors == 1) {
        image = "1F_Apilado_KB.png";
      } else if (this.floors == 2) {
        image = "2F_Old Laboratory HS_KB.png";
      } else if (this.floors == 3) {
        image = "3FLRS_Regala_KB.png";
      } else if (this.floors == 4) {
        image = "4FLRS_Nudas_KB.png";
      } else if (this.floors == 5) {
        image = "5F_CIT_KB.png";
      } else if (this.floors == 6) {
        image = "6F_Ceafa_KB.png";
      }
      this.tiles[this.rowTile]["column"][this.columnTile]["image"] = image;
      this.tiles[this.rowTile]["column"][this.columnTile]["floor"] = this.floors;
      this.isOpenSet = false;
    },
    editTile(column, row) {
      this.isOpenSet = true;
      this.columnTile = column;
      this.rowTile = row;
    },
    saveDesign() {
      if(parseInt(this.events.column>5)){
        alert('Limited of 5 Column')
        return
      }
      this.tiles = [];
      for (let x = 0; x < parseInt(this.events.row); x++) {
        this.tiles.push({ label: x, column: [] });
        for (let i = 0; i < parseInt(this.events.column); i++) {
          this.tiles[x]["column"].push({
            label: "",
            image: "",
            id: `${i} - ${x}`,
          });
        }
      }
      this.columns = parseInt(this.events.column);
      this.rows = parseInt(this.events.row);
      this.isOpen = false;
    },
  },
};
</script>

<style>
</style>