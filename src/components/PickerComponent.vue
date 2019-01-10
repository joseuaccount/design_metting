<template>
    <div>
        <div class="column is-offset-2 is-8">
            <div class="columns">
                <div class="column is-4">
                    <div class="title">
                        Add to List
                    </div>
                    <b-field>
                        <form @submit.prevent="addEvent">
                            <b-input placeholder="new event" v-model="new_event"></b-input>
                        </form>
                    </b-field>
                </div>
                <div class="column">
                    <template v-if="myEvents.length">
                        <b-table :striped="isStriped" :data="isEmpty ? [] : myEvents" :checked-rows.sync="checkedRows"
                            checkable>
                            <template slot-scope="props">
                                <b-table-column field="id" label="ID" with="10">
                                    {{ props.row.id }}
                                </b-table-column>
                                <b-table-column field="place_name" label="Place Name">
                                    {{ props.row.place_name }}
                                </b-table-column>
                                <b-table-column>
                                    <button class="button is-primary" @click="removeEvent(props.index)">
                                        <b-icon icon="delete-empty" size="is-small">
                                        </b-icon>
                                    </button>
                                </b-table-column>
                            </template>
                        </b-table>
                        <br>
                        <button class="button is-primary" @click="openModal" :disabled="this.checkedRows.length === 0">Start</button>
                        <button class="button is-danger is-outlined" style="float: right" @click="cleanAll" :disabled="this.myEvents.length === 0">
                            <span>Remove All</span>
                        </button>
                        &nbsp;
                        <br>
                        <br>
                        <pre>{{ checkedRows }}</pre>
                    </template>
                    <template v-else>
                        <div class="columns">
                            <div class="column is-4 is-offset-4">
                                <h3 class="is-aligned-center">No content yet!</h3>
                            </div>
                        </div>
                    </template>
                </div>
            </div>
        </div>
        <b-modal :active.sync="isModalActive">
            <div class="card">
                <div class="card-content">
                    <div v-if="!isLoading" class="card-title">
                        {{ randomPlace.place_name }} !
                        <br>
                        <br>
                        <button class="button is-primary" @click="randomChoose">Again</button>
                    </div>
                </div>
            </div>
            <b-loading :is-full-page="isFullPage" :active.sync="isLoading" :can-cancel="true"></b-loading>
        </b-modal>
    </div>
</template>

<script>
    export default {
        data() {
            const myEvents = [];
            return {
                myEvents,
                checkedRows: [], // rows checked by me
                isModalActive: false, // modal
                isLoading: false, // loading
                isFullPage: false, // loading on full page
                isEmpty: false,
                isStriped: true,
                new_event: '',
                columns: [{
                        field: 'id',
                        label: 'ID',
                        width: '40',
                        numeric: true
                    },
                    {
                        field: 'place_name',
                        label: 'Place Name',
                    }
                ], // columns to display on table
                randomPlace: [],
            }
        },
        methods: {
            openModal() {
                this.isModalActive = true;
                this.randomChoose();
            },
            openLoading() {
                this.isLoading = true
                setTimeout(() => {
                    this.isLoading = false
                }, 10 * 100)
            },
            randomChoose() {
                this.openLoading();
                this.randomPlace = this.checkedRows[Math.floor(Math.random() * this.checkedRows.length)];
            },
            plusOne() {
                return this.myEvents.length + 1;
            },
            addEvent() {
                if (this.new_event !== '' && this.new_event !== null) {
                    this.myEvents.push({
                        id: this.plusOne(),
                        place_name: this.new_event
                    });
                } else {
                    this.snackbar()
                }
                this.new_event = null;
            },
            snackbar() {
                this.$snackbar.open({
                    duration: 2000,
                    message: `To add one event, it cannot be empty!`,
                    type: 'is-danger',
                    position: 'is-bottom-left',
                    actionText: 'OK'
                })
            },
            removeEvent($event) {
                this.myEvents.splice($event, 1) // removing from the events array
                this.checkedRows.splice($event, 1) // removing from the checked events to choose randomly.
            },
            cleanAll() {
                this.myEvents = []
                this.checkedRows = []
            }
        }
    }
</script>