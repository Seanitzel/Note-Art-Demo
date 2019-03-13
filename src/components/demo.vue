<template>
    <v-app id="inspire" dark>
        <v-container fluid grid-list-lg>
            <v-layout row wrap justify-center>
                <v-flex xs12>
                    <h1 class="st" style="text-align:center; color: black">Fur Elise</h1>
                </v-flex>
            </v-layout>
            <v-flex xs12 style="text-align:center" my-4>
                <v-btn color="black" @click="isReady">Make sure all sounds are loaded</v-btn>
            </v-flex>
            <v-flex xs12 style="text-align:center">
                <v-btn
                        class="general-btn"
                        color="success"
                        round
                        large
                        @click="g"
                        :disabled="!ready"
                >
                    <v-icon>music_note</v-icon>
                    Play!
                    <v-icon>music_note</v-icon>
                </v-btn>
            </v-flex>
            <br>
            <br>
            <v-layout row align-center>
                <v-flex xs12>
                    <v-slider
                            name="bpm"
                            label="BPM"
                            :min="40"
                            :max="300"
                            step="5"
                            v-model="bpm"
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
                    <v-btn
                            class="general-btn"
                            color=""
                            fab
                            @click="piece.isMetronome  = !piece.isMetronome"
                    >
                        Metronome
                    </v-btn>
                </v-flex>
                <v-flex xs12>
                    <span v-if="true">Current Beat:{{piece.getBeat()}}</span>
                </v-flex>
            </v-layout>
            <v-layout row wrap align-center justify-center>
                <v-flex xs1>
                    <v-text-field v-model="trans" name="Transpose" label="Interval" id="id"></v-text-field>
                </v-flex>
                <v-flex xs1>
                    <v-btn color="secondery" @click="transpose">Transpose</v-btn>
                </v-flex>
            </v-layout>
        </v-container>
    </v-app>
</template>

<script>
    import {Measure, Piece, Piano, PlayableNote, AudioManager} from 'note-art'

    export default {
        name:    'play',
        data:    () => {
            const piano     = new Piano()
            const fur_elise = []
            for (let i = 0; i < 7; i++) fur_elise.push(new Measure())
            const m_notes = [
                [
                    [piano.note('e52n'), piano.note('e42n')],
                    [piano.note('e58n')],
                    [piano.note('d#58n')],
                ],
                [
                    [piano.note('e58n')],
                    [piano.note('d#58n')],
                    [piano.note('e58n')],
                    [piano.note('b48n')],
                    [piano.note('d58n')],
                    [piano.note('c58n')],
                ],
                [
                    [piano.note('a44n'), piano.note('a28n')],
                    [piano.note('e38n')],
                    [piano.note('a38n')],
                    [piano.note('c48n')],
                    [piano.note('e48n')],
                    [piano.note('a48n')],
                ],
                [
                    [piano.note('b44n'), piano.note('e28n')],
                    [piano.note('e38n')],
                    [piano.note('g#38n')],
                    [piano.note('e48n')],
                    [piano.note('g#48n')],
                    [piano.note('b48n')],
                ],
                [
                    [piano.note('c54n'), piano.note('a28n')],
                    [piano.note('e38n')],
                    [piano.note('a38n')],
                    [piano.note('e48n')],
                    [piano.note('e58n')],
                    [piano.note('d#58n')],
                ],
                [
                    [piano.note('b44n'), piano.note('e28n')],
                    [piano.note('e38n')],
                    [piano.note('g#38n')],
                    [piano.note('e48n')],
                    [piano.note('c58n')],
                    [piano.note('b48n')],
                ],
                [
                    [piano.note('a44n'), piano.note('a24n')],
                    [piano.note('e38n')],
                    [piano.note('a38n')],
                    [piano.note('e58n')],
                    [piano.note('d#58n')],
                ],
            ]

            let bpm = 120
            for (let i = 0; i < 7; i++) {
                fur_elise[i].pushSets(m_notes[i])
            }
            const piece = new Piece(120, [3, 4])
            piece.pushMeasures([
                fur_elise[0], fur_elise[1],
                fur_elise[2],
                fur_elise[3],
                fur_elise[4],
                fur_elise[1],
                fur_elise[2],
                fur_elise[5],
                fur_elise[6],
            ])
            return {
                bpm,
                vol:   1,
                piece,
                trans: 1,
                ready: false,
            }
        },
        destroyed() {
            if (this.piece.state == 'playing') {
                this.piece.play()
            }
        },
        methods: {
            g() {
                this.piece.transport.loopStart = '1m'
                this.piece.transport.loopEnd   = '8m'
                this.piece.play()
            },
            updateBPM() {
                this.piece.bpm = this.bpm
            },
            transpose() {
                if (parseInt(this.trans)) {

                    this.piece.transpose(this.trans)
                    this.piece.play()
                    setTimeout(() => this.piece.play(), 500)
                }
            },
            isReady() {
                if (PlayableNote.audioManager.players.loaded) {
                    AudioManager.resumeContext()
                    this.ready = true
                } else {
                    alert('Not all sounds have loaded yet!')
                }
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
