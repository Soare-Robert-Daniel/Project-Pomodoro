<template>
    <div>

        <b-jumbotron bg-variant="dark" text-variant="white" border-variant="dark">
            <template v-slot:header>{{periods[currentPeriod].type}}
                <b-badge variant="light">{{time}}</b-badge>
            </template>
            <hr class="my-4">
            <div class="options">
                <h3>Options</h3>
                <b-form-checkbox
                        id="checkbox-1"
                        v-model="autoStart"
                        name="checkbox-1"
                        value=true
                        unchecked-value=false
                >
                    Auto Start
                </b-form-checkbox>

            </div>
        </b-jumbotron>

        <div v-if="isDone">
            <b-button v-if="isNotStarted" v-on:click="start" pill variant="primary">
                <b-icon-stopwatch font-scale="5" v-b-tooltip.hover title="Begin"></b-icon-stopwatch>
            </b-button>
            <b-button v-else v-on:click="next" pill variant="primary" v-b-tooltip.hover title="Start the next cycle">
                <h1> Next </h1></b-button>
        </div>
        <div v-else>
            <b-button-group>
                <b-button v-on:click="startTimer" v-if="isPaused" variant="success">
                    <b-icon-play-fill font-scale="5"></b-icon-play-fill>
                </b-button>
                <b-button v-on:click="stopTimer" v-else variant="info">
                    <b-icon-pause-fill font-scale="5"></b-icon-pause-fill>
                </b-button>
                <b-button v-on:click="resetTimer" variant="warning">
                    <b-icon-arrow-counterclockwise font-scale="4"></b-icon-arrow-counterclockwise>
                </b-button>
            </b-button-group>
        </div>

        <div class="mt-3">
            <b-card-group deck>
                <b-card bg-variant="info" text-variant="white" header="Work" class="text-center">
                    <b-form-input v-model="workTime" @change="timeChangeBeforeStartHandler" type="number" min="0" max="30"></b-form-input>
                </b-card>

                <b-card bg-variant="warning" text-variant="white" header="Short Break" class="text-center">
                    <b-form-input v-model="shortBreakTime" @change="timeChangeBeforeStartHandler" type="number" min="0" max="30"></b-form-input>
                </b-card>

                <b-card bg-variant="danger" text-variant="white" header="Long Break" class="text-center">
                    <b-form-input  v-model="longBreakTime" @change="timeChangeBeforeStartHandler" type="number" min="0" max="30"></b-form-input>
                </b-card>
            </b-card-group>
        </div>

    </div>
</template>

<script>
    export default {
        name: "Timer",
        data() {
            return {
                periods: [
                    {
                        type: "work",
                    },
                    {
                        type: "short_break",
                    },
                    {
                        type: "work",
                    },
                    {
                        type: "short_break",
                    },
                    {
                        type: "work",
                    },
                    {
                        type: "short_break",
                    },
                    {
                        type: "work",
                    },
                    {
                        type: "long_break",
                    },
                ],
                currentPeriod: 0,
                time: "",
                currentTime: 0,
                timer: null,
                autoStart: false,
                isDone: true,
                isPaused: false,
                isNotStarted: true,
                workTime: 25,
                shortBreakTime: 5,
                longBreakTime: 30
            }
        },
        created: function () {
            this.resetTimer();
            this.updateTime();
        },
        methods: {
            start: function () {

                this.currentPeriod = 0;
                this.isNotStarted = false;
                this.isDone = false;

                this.resetTimer();
                this.startTimer();

            },
            next: function () {
                this.isDone = false;
                if (this.currentPeriod < this.periods.length) {
                    this.updateTimer();
                    this.resetTimer();
                    this.startTimer();
                } else {
                    this.isNotStarted = true;
                }
            },
            updateTime: function () {

                const minutes = Math.trunc(this.currentTime / 60);
                const seconds = this.currentTime % 60;

                this.time = ((minutes < 10) ? "0" + minutes : minutes) + ":" + ((seconds < 10) ? "0" + seconds : seconds);
            },
            startTimer: function () {
                this.isPaused = false;
                this.timer = setInterval(
                    () => {
                        if (this.currentTime === 0) {
                            this.stopTimer();
                            this.isDone = true;
                            if (this.autoStart) {
                                this.next();
                            }
                        } else {
                            this.isDone = false;
                            this.currentTime -= 1;
                        }
                        this.updateTime();
                    }
                    , 1000);
            },
            updateTimer: function () {
                this.currentPeriod += 1;
            },
            stopTimer: function () {
                this.isPaused = true;
                clearInterval(this.timer);
            },
            resetTimer: function () {
                this.currentTime = this.getPeriodLimit(this.periods[this.currentPeriod].type) * 60;
            },
            deleteTimer: function () {
                this.stopTimer();
                this.timer = null;
            },
            getPeriodLimit: function(type) {
                if(type === "short_break") {
                    return this.shortBreakTime;
                } else if(type === "long_break") {
                    return this.longBreakTime;
                } else if(type === "work") {
                    return this.workTime;
                }
                
                return -1;
            },
            timeChangeBeforeStartHandler: function() {
                if(this.isNotStarted) {
                    this.resetTimer();
                    this.updateTime();
                }
            }

        }
    }
</script>

<style scoped>

    .options {
        text-align: left;
    }
    
</style>