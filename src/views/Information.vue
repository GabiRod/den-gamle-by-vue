<template>
    <div class="information">
        <h1>Information</h1>
        <v-row class="icons" justify="space-around">
            <v-icon x-large>mdi-human-male-female</v-icon>

            <v-icon x-large>mdi-silverware-fork-knife</v-icon>

            <v-icon x-large>mdi-home</v-icon>
        </v-row>

        <div class="expansion-panel">
            <v-expansion-panels class="mb-6">
                <v-expansion-panel
                    v-for="zone of zones"
                    v-bind:key="zone.title"
                >
                    <v-expansion-panel-header>
                        <h2>{{ zone.title }}</h2>
                    </v-expansion-panel-header>
                    <v-expansion-panel-content>
                        <p>
                            <v-icon>mdi-silverware-fork-knife</v-icon>
                            {{ zone.content }}
                        </p>
                    </v-expansion-panel-content>
                </v-expansion-panel>
            </v-expansion-panels>
        </div>
    </div>
</template>

<style scoped lang="scss">
.information {
    overflow-x: hidden;
    padding: 0 16px;

    .icons {
        padding-top: 20px;

        .mdi-silverware-fork-knife::before {
            color: $orange;
        }
    }

    .expansion-panel {
        padding-top: 30px;
    }
}

.theme--light.v-expansion-panels .v-expansion-panel {
    margin-bottom: 35px;
    margin-top: 0;
    border-radius: 10px;
}

.v-application--is-ltr .v-expansion-panel-header {
    border: 3px solid $blue;
    border-radius: 10px;
}

.v-expansion-panel--active > .v-expansion-panel-header {
    min-height: 64px;
    border-radius: 10px !important;
}

// .v-expansion-panels:not(.v-expansion-panels--accordion)
//     > .v-expansion-panel--active {
//     border-bottom-left-radius: 0 !important;
//     border-bottom-right-radius: 0 !important;
// }

.v-expansion-panel-content {
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;

    .v-expansion-panel-content__wrap {
        border: 3px solid $blue;

        p {
            margin: 0;
            padding-top: 16px;
        }
    }
}
</style>

<script>
export default {
    data() {
        return {
            zones: [
                {
                    title: '1864 zone',
                    content: 'Backery'
                },
                {
                    title: '1927 zone',
                    content: 'Restaurant'
                },
                {
                    title: '1974 zone',
                    content: 'Bar'
                }
            ]
        };
    }
};
</script>
