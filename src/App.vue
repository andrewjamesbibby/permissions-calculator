<template>
    <div id="app">

        <header>
            <img alt="Vue logo" src="./assets/matrix.png">
            <h1>Permissions Calculator</h1>
        </header>

        <div id="permissions">
            <div class="permission-group">
                <h2>USER</h2>

                <div class="option-group">
                    <div class="title">READ</div>
                    <input id="user-read" v-model="permissions.user.read.state" type="checkbox">
                    <label for="user-read"></label>
                </div>

                <div class="option-group">
                    <div class="title">WRITE</div>
                    <input id="user-write" v-model="permissions.user.write.state" type="checkbox">
                    <label for="user-write"></label>
                </div>

                <div class="option-group">
                    <div class="title">EXECUTE</div>
                    <input id="user-execute" v-model="permissions.user.execute.state" type="checkbox">
                    <label for="user-execute"></label>
                </div>
            </div>
            <div class="permission-group">
                <h2>GROUP</h2>

                <div class="option-group">
                    <div class="title">READ</div>
                    <input id="group-read" v-model="permissions.group.read.state" type="checkbox">
                    <label for="group-read"></label>
                </div>

                <div class="option-group">
                    <div class="title">WRITE</div>
                    <input id="group-write" v-model="permissions.group.write.state" type="checkbox">
                    <label for="group-write"></label>
                </div>

                <div class="option-group">
                    <div class="title">EXECUTE</div>
                    <input id="group-execute" v-model="permissions.group.execute.state" type="checkbox">
                    <label for="group-execute"></label>
                </div>
            </div>
            <div class="permission-group">
                <h2>EXECUTE</h2>

                <div class="option-group">
                    <div class="title">READ</div>
                    <input id="any-read" v-model="permissions.any.read.state" type="checkbox">
                    <label for="any-read"></label>
                </div>

                <div class="option-group">
                    <div class="title">WRITE</div>
                    <input id="any-write" v-model="permissions.any.write.state" type="checkbox">
                    <label for="any-write"></label>
                </div>

                <div class="option-group">
                    <div class="title">EXECUTE</div>
                    <input id="any-execute" v-model="permissions.any.execute.state" type="checkbox">
                    <label for="any-execute"></label>
                </div>
            </div>
        </div>

        <div id="numeric-permissions">
            {{ binary_permissions.user }} {{ binary_permissions.group }} {{ binary_permissions.any }}
        </div>

        <footer>
            A project by <a href="https://www.andrewjamesbibby.com" target="_blank">Andrew James Bibby</a>
        </footer>

    </div>
</template>

<script>
    export default {
        name: 'App',
        data() {
            return {
                permissions: {
                    user: {
                        read: {state: false, binary: 4, value: 'r'},
                        write: {state: false, binary: 2, value: 'w'},
                        execute: {state: false, binary: 1, value: 'x'}
                    },
                    group: {
                        read: {state: false, binary: 4, value: 'r'},
                        write: {state: false, binary: 2, value: 'w'},
                        execute: {state: false, binary: 1, value: 'x'}
                    },
                    any: {
                        read: {state: false, binary: 4, value: 'r'},
                        write: {state: false, binary: 2, value: 'w'},
                        execute: {state: false, binary: 1, value: 'x'}
                    }
                },
                binary_permissions: {
                    user: 0,
                    group: 0,
                    any: 0,
                },
            }
        },
        methods: {
            calculatePermissions() {
                this.binary_permissions = {
                    user: this.permissionToBinary(this.permissions.user),
                    group: this.permissionToBinary(this.permissions.group),
                    any: this.permissionToBinary(this.permissions.any),
                }
            },
            permissionToBinary(permission) {

                let result = 0

                for(const item in permission) {
                    if(permission[item].state) {
                        result += permission[item].binary
                    }
                }

                return result
            }
        },
        watch: {
            permissions: {
                handler: function() {
                    this.calculatePermissions();
                },
                deep: true
            }
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: #2c3e50;
        margin-top: 60px;
        box-sizing: border-box;
        padding: 1em;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    header {
        width: 100%;
        display: flex;
        align-items: center;
        flex-direction: column;
        text-align: center;
        user-select: none;
    }

    header img {
        width: 100px;
    }

    #permissions {
        width: 100%;
        height: 300px;
        padding: 1em 0em 1em 0em;
        border-bottom: solid 1px silver;
        display: inline-flex;
        justify-content: center;
    }

    #permissions .permission-group {
        border: solid 1px #d4d4d4;
        width: 200px;
        margin: 0.5em;
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        user-select: none;
    }

    #permissions .permission-group h2 {
        font-size: 1em;
        margin: 0;
        padding: 0 0 0.5em 0;
        text-align: center;
        border-bottom: solid 1px #d4d4d4;
    }

    .option-group {
        text-align: center;
    }

    .option-group .title {
        font-weight: 500;
        margin-bottom: 5px;
        font-size: 0.8em;
    }

    .option-group input[type=checkbox] {
        display: none;
    }

    .option-group input[type=checkbox] + label {
        display: inline-block;
        border: solid 1px #d4d4d4;
        width: 20px;
        height: 20px;
        cursor: pointer;
    }

    .option-group input[type=checkbox]:checked + label {
        background-color: #d4d4d4;
        background-image: url('./assets/check.png');
        background-size: 75%;
        background-position: center;
        background-repeat: no-repeat;
    }

    #permissions .permission-group h1 {
        border-bottom: solid 1px silver;
        margin: 0px;
        font-size: 1.3em;
    }

    #numeric-permissions {
        width: 100%;
        height: 20px;
        padding: 1em 0em 1em 0em;
        border-bottom: solid 1px silver;
        display: inline-flex;
        justify-content: center;
        font-weight: bold;
        font-size: 1.5em;
        line-height: 1em;
    }

    footer {
        padding-top: 2em;
    }

    footer a {
        text-decoration: none;
        color: #42b983;
    }
</style>
