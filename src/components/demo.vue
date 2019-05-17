<template>
    <v-app id="inspire" dark>
        <v-container fluid grid-list-lg text-xs-center>
            <v-layout row wrap justify-center align-center>
                <v-flex xs12 py-3>
                    <span class="display-2 blue--text">
                        Chords & Scales
                    </span>
                    <v-flex xs12 mt-3>
                        <span class="blue--text body-1">
                        Pick any pitch class:
                    </span>
                    </v-flex>
                </v-flex>
                <v-flex xs12>
                    <span class="caption blue-grey--text">
                        Pitch Classes - {{pitchClasses.toString().replace(/,/g, ', ')}}
                    </span>
                </v-flex>
                <v-flex xs5>
                    <v-text-field box v-model="note"
                                  label="Pitch Class"
                                  color="blue"
                                  prepend-icon="music_note">
                    </v-text-field>
                </v-flex>
                <v-flex xs12>
                    <span class="blue--text">
                        Major Chord: {{chord}}
                    </span>
                </v-flex>
                <v-flex xs12>
                    <span class="blue--text">
                        Major Scale: {{scale}}
                    </span>
                </v-flex>
                <v-flex xs12>
                    <v-divider></v-divider>
                </v-flex>
                <v-flex xs12 pa-1 ma-2>
                    <span class="display-2 font-weight-bold red--text">Fur Elise</span>
                </v-flex>
                <span v-if="!app.get('ready')" class="title red--text">
                Buffers are loading...
            </span>
                <v-flex xs12 pt-4>
                    <v-btn fab class=""
                           :color="driver.state==='stopped' ? 'success' : 'error'"
                           @click="play"
                           :disabled="!app.get('ready')">
                        <v-icon v-if="driver.state === 'stopped'">play_arrow</v-icon>
                        <v-icon v-else>stop</v-icon>
                    </v-btn>
                </v-flex>
                <v-flex xs12 py-3>
                    <span class="red--text title">{{time}}</span>
                </v-flex>
                <v-flex xs12 px-5 py-2>
                    <v-slider name="bpm"
                              label="BPM"
                              :min="40"
                              :max="300"
                              step="5"
                              v-model="driver.bpm"
                              thumb-label="always"
                              class="slider"
                              height="30"
                              max-width="50"
                              color="red"
                    ></v-slider>
                </v-flex>
            </v-layout>
            <v-layout column wrap align-center py-1>
                <v-flex xs12 pa-2>
                    <v-switch dark color="green"
                              @change="driver.toggleMetronome()"
                              label="Metronome"
                    ></v-switch>
                </v-flex>
                <v-flex xs12>
                    <span class="title red--text">
                        Transpose the piece to a different key:
                    </span>
                </v-flex>
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
    import {Piano, Driver, Score, app, Chord, Scale, Note, MusicTheoryStructures as mts} from 'note-art'

    //Create a new piece
    const piece = new Score({timeSignature: [3, 8], bpm: 120})

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
        name: 'play',
        data: () => {
            return {
                app,
                bpm:          120,
                vol:          1,
                driver,
                piece,
                trans:        1,
                piano,
                note:         'c',
                pitchClasses: mts.pitchClasses,
            }
        },

        computed: {
            time: function () {
                const time = this.driver.position.split(':')
                return `Measure: ${parseInt(time[0]) + 1} Beat: ${time[1]}`
            },

            seconds: function () {
                return this.driver.seconds
            },

            chord: function () {
                return new Chord({
                    root:    Note.builder(`${this.note}3`),
                    pattern: [4, 7],
                }).pitchClassesString
            },

            scale: function () {
                return new Scale({
                    tonic: Note.builder(`${this.note}3`),
                    name:  'Major',
                }).pitchClassesString
            },
        },

        methods: {
            play() {
                app.get('audio-manager').resumeContext()
                this.driver.toggle()
            },
            transpose() {
                const interval = parseInt(this.trans)
                if (interval) {
                    if (this.driver.state === 'started') {
                        setTimeout(() => {
                            this.driver.toggle()
                        }, 1000)
                    }
                    this.driver.transport.stop()
                    this.driver.transport.cancel()
                    this.piece.transpose(interval, 0)
                    this.piece.transpose(interval, 1)
                    this.driver.scheduleVoices()
                    // if (this.driver.metronome) {
                    //     this.driver.startMetronome()
                    // }
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
