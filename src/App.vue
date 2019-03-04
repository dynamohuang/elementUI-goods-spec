<template>
    <div id="app">
        <div>
            <h2>商品规格：</h2>
            <div v-for="Spec in SpecList">
                规格名
                <el-select v-model="Spec.SpecName" autocomplete filterable allow-create default-first-option
                           placeholder="请选择">
                    <el-option
                            v-for="item in SpecOptions"
                            :key="item.value"
                            :label="item.value"
                            :value="item.value">
                    </el-option>
                </el-select>

                <span v-for="tag in Spec.SpecTagList">
<el-tag closable :disable-transitions="false" v-show="!tag.editVisible" @click="handleEdit(tag)"
        @close="handleClose(tag,Spec.SpecTagList)">
  {{tag.label}}
</el-tag>
        <el-input class="input-edit-tag" v-show="tag.editVisible" v-model="tag.label" ref="saveTagInput" size="small"
                  @keyup.enter.native="handleTagEditConfirm(tag)" @blur="handleTagEditConfirm(tag)"></el-input>
</span>

                <el-input class="input-new-tag" v-if="Spec.SpecTagAddVisible" v-model="Spec.SpecTagAddValue"
                          ref="saveTagInput" size="small"
                          @keyup.enter.native="handleInputConfirm(Spec.SpecTagAddValue,Spec)"
                          @blur="handleInputConfirm(Spec.SpecTagAddValue,Spec)">
                </el-input>
                <el-button v-else class="button-new-tag" size="small" @click="showInput(Spec)">+ 规格选项</el-button>


                <el-button type="danger" icon="el-icon-delete" circle @click="delSpec($index)"></el-button>
            </div>
            <el-button @click="addSpec()" type="primary"><i class="el-icon-plus"></i>添加规格项</el-button>
        </div>
        <div>
            <h2>规格明细：</h2>

            <el-table :data="specDetailList"
                      style="width: 100%">
                <el-table-column
                        :prop="column.prop"
                        :label="column.label"
                        v-for="column in specDetailColumn"
                >
                </el-table-column>
            </el-table>
        </div>

        <pre>
        {{ specDetailList }}
    </pre>
    </div>


</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                label: '商品规格',
                SpecOptions: [{
                    value: '规格',
                }, {
                    value: '颜色',
                }, {
                    value: '款式',
                }, {
                    value: '网络',
                }, {
                    value: '型号',
                }],
                //用户输入数据
                SpecList: [],
                specDetailDefaultColumn: [{'prop': 'sku', 'label': 'sku'}, {
                    'prop': 'price',
                    'label': 'price'
                }, {'prop': 'left', 'label': '库存'}],


            }
        },

        methods: {
            addSpec: function () {
                let newSpec = {
                    SpecName: '',
                    SpecTagList: [],
                    SpecTagAddVisible: false,
                    SpecTagAddValue: ''
                };
                this.SpecList.push(newSpec);
            },
            delSpec: function (index) {
                this.SpecList.splice(index, 1);
                //:todo update table
            },
            handleClose(tag, SpecTagList) {

                SpecTagList.splice(SpecTagList.indexOf(tag), 1);
            },
            handleEdit(tag) {
                tag.editVisible = true;
//                this.$nextTick(function()  {
////                    this.$refs.saveTagInput.first().focus();
//                });
            },

            showInput(Spec) {
                Spec.SpecTagAddVisible = true;
//                this.$nextTick(function ()  {
//                    let input = this.$refs.saveTagInput;
//
//                    input[0].focus();
//
//                });
            },

            handleTagEditConfirm(tag) {
                tag.editVisible = false;
            },

            handleInputConfirm(inputValue, Spec) {

                if (inputValue) {
                    Spec.SpecTagList.push({'editVisible': false, 'label': inputValue});
                }
                Spec.SpecTagAddVisible = false;
                Spec.SpecTagAddValue = '';
            }

        },
        computed: {
            specDetailColumn: function () {

                let specDetailColumn = JSON.parse(JSON.stringify(this.specDetailDefaultColumn));
                this.SpecList.forEach(
                    function (element) {
                        if (element.SpecName && element.SpecTagList.length > 0) {
                            specDetailColumn = [{
                                'prop': element.SpecName,
                                'label': element.SpecName,
                                'diy': true
                            }].concat(specDetailColumn);
                        }
                    }
                );
                return specDetailColumn;

            },
            specDetailList: function () {

                let specDetailListOld = [{sku: '', price: '', left: ''}];
                let specDetailList = JSON.parse(JSON.stringify(specDetailListOld));

                this.SpecList.forEach(function (spec) {

                    if (spec.SpecName && spec.SpecTagList.length > 0) {

                         let specDetailListTmp = [];

                        specDetailList.forEach(function (row) {
                            spec.SpecTagList.forEach(function (SpecTag) {
                                let newRow = JSON.parse(JSON.stringify(row));
                                newRow[spec.SpecName] = SpecTag.label;
                                specDetailListTmp.push(newRow);

                            });
                        });
                        specDetailList = JSON.parse(JSON.stringify(specDetailListTmp));
                    }
                });
                return specDetailList;

            }
        },
        watch: {}


    }
</script>

<style lang="scss">
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    .el-tag + .el-tag {
        margin-left: 10px;
    }

    .button-new-tag {
        margin-left: 10px;
        height: 32px;
        line-height: 30px;
        padding-top: 0;
        padding-bottom: 0;
    }

    .input-new-tag, .input-edit-tag {
        width: 90px;
        margin-left: 10px;
        vertical-align: bottom;
    }
</style>
