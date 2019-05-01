<template>
    <v-app id="inspire" dark>
        <v-container fluid grid-list-lg text-xs-center>
            <v-layout row wrap justify-center>
                <v-flex xs12 pa-4 ma-4>
                    <span class="display-4 font-weight-bold red--text">Fur Elise</span>
                </v-flex>
            </v-layout>
            <v-flex xs12 style="text-align:center">
                <v-btn round large class="general-btn"
                       color="success"
                       @click="driver.play()"
                       :disabled="!ready">
                    <v-icon>music_note</v-icon>
                    Play!
                    <v-icon>music_note</v-icon>
                </v-btn>
            </v-flex>
            <v-layout row align-center>
                <v-flex xs12 pa-4>
                    <v-slider name="bpm"
                              label="BPM"
                              :min="20"
                              :max="500"
                              step="5"
                              v-model="driver.bpm"
                              @change="updateBPM"
                              thumb-label="always"
                              class="slider"
                              height="30"
                              color="red"
                    ></v-slider>
                </v-flex>
            </v-layout>
            <v-layout column wrap align-center>
                <v-flex xs12>
                    <v-switch dark color="green"
                              @change="driver.toggleMetronome()"
                              label="Metronome"
                    ></v-switch>
                </v-flex>
                <v-flex xs12>
                    <span class="red--text">{{time}}</span>
                </v-flex>
            </v-layout>
            <v-layout row wrap align-center justify-center>
                <v-flex xs2>
                    <v-text-field v-model="trans" name="Transpose" label="Interval"
                                  id="id"></v-text-field>
                </v-flex>
                <v-flex xs1>
                    <v-btn color="red lighten-1" @click="transpose">Transpose</v-btn>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
    import {Piano, Driver, Piece, app, Guitar} from 'note-art'

    app.set('path', () => {
        return './audio/'
    })

    //Create a new piece
    const piece = new Piece({timeSignature: [3, 8], bpm: 120})

    //Add another voice to the piece
    piece.addVoice()

    //Setup the number of measures we want
    for (let i = 0; i < 8; ++i) {
        piece.addMeasure(i, 0)
        piece.addMeasure(i, 1)
    }

    //Create the first voice

    //Measure 1
    ['e1', 'e2', 'e3', 'e4', 'e5', 'd#5'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 0)
    });

    //Measure 2
    ['e5', 'd#5', 'e5', 'b4', 'd5', 'c5'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 1)
    })

    //Measure 3
    piece.addNote({note: 'a4', duration: '8n'}, 0, 2);
    ['r', 'c4', 'e4', 'a4'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 2)
    })

    //Measure 4
    piece.addNote({note: 'b4', duration: '8n'}, 0, 3);
    ['r', 'e4', 'g#4', 'b4'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 3)
    })

    //Measure 5
    piece.addNote({note: 'c5', duration: '8n'}, 0, 4);
    ['r', 'e4', 'e5', 'd#5'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 4)
    });

    //Measure 6
    ['e5', 'd#5', 'e5', 'b4', 'd5', 'c5'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 5)
    })

    //Measure 7
    piece.addNote({note: 'a4', duration: '8n'}, 0, 6);
    ['r', 'c4', 'e4', 'a4'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 6)
    })

    //Measure 8
    piece.addNote({note: 'b4', duration: '8n'}, 0, 7);
    ['r', 'e4', 'c5', 'b4'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 7)
    })

    //Measure 9
    piece.addNote({note: 'a4', duration: '4n'}, 0, 8);
    ['e5', 'd#5'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index + 1, 8)
    })

    //Voice 2
    const notes1 = ['a2', 'e3', 'a3']
    const notes2 = ['e2', 'e3', 'g#3']

    //Measure 3
    notes1.forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 2, 1)
    })

    //Measure 4
    notes2.forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 3, 1)
    })

    //Measure 5
    notes1.forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 4, 1)
    })

    //Measure 7
    notes1.forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 6, 1)
    })

    //Measure 8
    notes2.forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 7, 1)
    });

    //Measure 9
    ['a2', 'e3', 'a3'].forEach((note, index) => {
        piece.addNote({note, duration: '16n'}, index, 8, 1)
    })

    const piano  = new Piano()
    const driver = new Driver(piece, [piano, piano]).init()
    driver.scheduleVoices()
    driver.loopStart = '1m'

    export default {
        name:     'play',
        data:     () => {
            return {
                bpm:    120,
                vol:    1,
                driver,
                piece,
                trans:  1,
                ready:  false,
                piano,
                guitar: new Guitar(),
            }
        },
        created() {
            setTimeout(() => {
                this.ready = true
            }, 100)
        },
        destroyed() {
            // if (this.driver.state === 'playing') {
            //     this.driver.play()
            // }
        },
        computed: {
            time: function () {
                const time = this.driver.position.split(':')
                return `Measure: ${parseInt(time[0]) + 1} Beat: ${time[1]}`
            },

            seconds: function () {
                return this.driver.seconds
            },
        },
        methods:  {
            g() {

            },
            updateBPM() {

            },
            transpose() {
                const interval = parseInt(this.trans)
                if (interval) {
                    if (this.driver.state === 'started') {
                        setTimeout(() => {
                            this.driver.play()
                        }, 1000)
                    }
                    this.driver.transport.stop()
                    this.driver.transport.cancel()
                    this.piece.transpose(interval, 0)
                    this.piece.transpose(interval, 1)
                    this.driver.scheduleVoices()
                    if (this.driver.metronome) {
                        this.driver.startMetronome()
                    }
                }
            },
            isReady() {

            },
        },
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    #app {
        text-align: center;
    }

    h1,
    h2 {
        font-weight: normal;
        font-size:   40pt;
        /* background-color: aqua */
    }

    p,
    span {
        font-size: 20pt;
        display:   inline;
    }

    .general-btn {
        width:  100pt;
        height: 60pt;
    }
</style>
