<template>
<div class="">
    <el-form label-position="top" label-width="150px">
        <el-form-item>
            <el-input placeholder="What did you buy? (e.g. Groceries)" v-model="new_item.title">
                <el-select slot="prepend" v-model="new_item.user" style="width:100px">
                    <el-option v-for="(_, user) in users"
                        :label="user"
                        :value="user">
                    </el-option>
                </el-select>
            </el-input>
        </el-form-item>
        <el-form-item>
            <el-input type="number" v-model="new_item.value">
                <template slot="prepend">$</template>
            </el-input>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit">Send it m8</el-button>
        </el-form-item>
    </el-form>

    <!-- Card -->
    <div class="m1">
        <el-row>
            <el-col class="grid-content" :xs="24" :sm="8"  v-for="(user, name) in users">
                <el-card class="mp25">
                    <!-- Header -->
                    <div slot="header" class="mp25">
                        <el-row>
                            <el-col :span="18">
                                <span>
                                    {{name}}
                                </span>
                            </el-col>
                            <el-col :span="6">
                                <div :class="diffColor(user.owes)" class="bold ib">
                                    {{user.owes | money }}
                                </div>
                            </el-col>
                        </el-row>
                    </div>
                    <div v-for="item in user.items">
                        <div style="width:50px" class="ib mp25">
                            {{item.value | money }}
                        </div>
                        {{item.title}}
                    </div>
                </el-card>
            </el-col>
        </el-row>
    </div>
</div>
</template>

<script>
export default {
    name: 'hello',
    methods: {
        diffColor (input) {
            input = Number(input)
            return {
                active: true,
                'green': (input > 0),
                'red': (input < 0)
            }
        },
        onSubmit () {
            // push the new item into the users items array
            this.users[this.new_item.user].items.push({
                'title': this.new_item.title,
                'value': Number(this.new_item.value)
            })
            // recalculate
            this.calculate()
        },
        calculate () {
            var total = 0
            var num_users = Object.keys(this.users).length
            for (var name in this.users) {
                var user = this.users[name]
                user.payed = 0
                user.items.forEach((item) => {
                    user.payed += item.value
                    item.value = Number(item.value)
                    total += item.value
                })
            }
            for (name in this.users) {
                user = this.users[name]
                user.owes = user.payed - (total / num_users)
            }
        }
    },
    created () {
        this.calculate()
    },
    filters: {
        money (input) {
            // split on decimal if present
            // ex. 25.66666666 -> "25.66666666" -> ["25", "6666666666666"]
            input = input.toString().split('.')
            // if decimal was present
            if (input.length > 1) {
                // if decimal value was one digit add a 0
                input[1] = input[1].length === 1 ? input[1] + '0' : input[1]
                // make sure it is only the first 2 digits
                input[1] = input[1].substr(0, 2)
            // if decimal not present
            } else {
                // add two 0s
                input.push('00')
            }
            // put em back together
            // `${any_variable} <- This is a string of this variable`
            return `${input[0]}.${input[1]}`
        }
    },
    data () {
        return {
            'new_item': {
                'user': 'Neil',
                'title': '',
                'value': 50
            },
            'users': {
                'Neil': {
                    'payed': 0,
                    'owes': 0,
                    'items': [
                        {
                            'title': 'Power Rack',
                            'value': 763.60
                        }
                    ]
                },
                'Austen': {
                    'payed': 0,
                    'owes': 0,
                    'items': [
                        {
                            'title': 'Groceries',
                            'value': 36.42
                        }, {
                            'title': 'Protein Powder',
                            'value': 180.00
                        }
                    ]
                },
                'Nico': {
                    'payed': 0,
                    'owes': 0,
                    'items': [
                        {
                            'title': 'Insurance',
                            'value': 360.00
                        }
                    ]
                }
            }
        }
    }
}
</script>
