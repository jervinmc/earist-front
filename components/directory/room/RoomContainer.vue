<template>
  <div>
      
       <v-dialog v-model="isOpenRoom" width="1000" persistent>
      <v-card class="pa-10">
        <div align="center" class="text-h6">Description</div>
        <div>
            {{descriptionItem}}
        </div>
        <v-card-actions>
          <v-row align="center">
            <v-col align="end">
              <v-btn color="red" text @click="isOpenRoom = false">
                Back
              </v-btn>
            </v-col>
          </v-row>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="isOpenSet" width="1000" persistent>
      <v-card class="pa-10">
        
        <v-col cols="12" class="px-0">
          <div>Label</div>
          <div>
            <v-text-field outlined v-model="labelName"></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Type</div>
          <div>
            <v-select
              outlined
              v-model="type"
              :items="['Entrance', 'Exit', 'Unit', 'Rest Room', 'Classroom']"
            ></v-select>
          </div>
        </v-col>
        <v-col cols="12" class="px-0">
          <div>Description</div>
          <div>
            <v-textarea
              outlined
              v-model="description"
            ></v-textarea>
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
      <v-row class="py-10">
        <v-col align="end">
          <v-row>
              <v-col align="start">
                  <div align="start" class="text-h6"><b>Room Management</b></div>
              </v-col>
            <v-col>
              <div align="end">
                <v-btn
                  depressed
                  color="#f0932b"
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
                  color="#f0932b"
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
      </v-row>
      <v-card elevation="5" class="pa-5 rounded-xl">
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
            >
              <v-card height="150" width="150" class="pa-5">
                <div align="end">
                    <v-row>
                        <v-spacer></v-spacer>
                        <v-col>
                              <v-icon @click="viewRoom(col)" v-if="col.label != ''">mdi-eye</v-icon>
                        </v-col>
                        <v-col>
                              <v-icon size="15" @click="editTile(indexColumn, indexRow)">mdi-pencil</v-icon>
                        </v-col>
                    </v-row>
                </div>
                <b>{{ col.label }}</b>
                <div v-if="col.label != ''">
                  <v-img height="60" width="60" :src="`${col.image}`"></v-img>
                </div>
              </v-card>
            </v-col>
          </v-row>
        </div>
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
        isOpenRoom:false,
        description:'',
      isLoaded: false,
      buttonLoad: false,
      labelName: "",
      tiles: [],
      rowTile: 0,
      columnTile: 0,
      columns: 0,
      floors: 0,
      type:'',
      rows: 0,
      descriptionItem:'',
      events: {
        row: 0,
        column: 0,
      },
      isOpen: false,
      isOpenSet: false,
    };
  },
  methods: {
      viewRoom(item){
          this.descriptionItem = item.description
          this.isOpenRoom = true
          
      },
    viewFloor(label,floor){
      window.location.href=`/floor?building_name=${label}&floor=${floor}`
    },
    async eventsGetall() {
      const res = await this.$axios
        .get(`/room-building?building_name=${this.$route.query.building_name}&floor=${this.$route.query.floor}`, {
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
    async saveForm() {
      var characters = "abcdefghijklmnopqrstuvwxyz0123456789";
      var result = "";
      var chaactersLength = characters.length;

      for (var i = 0; i < 5; i++) {
        result += characters.charAt(
          Math.floor(Math.random() * chaactersLength)
        );
      }
      for (let key in this.tiles) {
        this.tiles[key].formatID = result;
        const res = await this.$axios
          .post(
            `/room/`,
            {
              tiles: JSON.stringify(this.tiles[key].column),
              formatID: result,
              floor: this.$route.query.floor,
              building_name: this.$route.query.building_name,
            },
            {
              headers: {
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            }
          )
          .then((res) => {});
      }
    },
    cancel() {},
    saveInput() {
      var image = "";
      this.tiles[this.rowTile]["column"][this.columnTile]["label"] =
        this.labelName;
      if (this.type == 'Entrance') {
        image = "1F_Apilado.png";
      } else if (this.type == 'Exit') {
        image = "Fire Exit_3.png";
      } else if (this.type == 'Unit') {
        image = "3F_Regala.png";
      } else if (this.type == 'Rest Room') {
        image = "4F_Nudas.png";
      } else if (this.type == 'Classroom') {
        image = "Classroom_1.png";
      } 
      this.tiles[this.rowTile]["column"][this.columnTile]["image"] = image;
      this.tiles[this.rowTile]["column"][this.columnTile]["floor"] = this.floors;
      this.tiles[this.rowTile]["column"][this.columnTile]["description"] = this.description;
      this.isOpenSet = false;
    },
    editTile(column, row) {
      this.isOpenSet = true;
      this.columnTile = column;
      this.rowTile = row;
    },
    saveDesign() {
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